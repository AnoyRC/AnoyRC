---
title: "How to send your first cross-chain message using Wormhole"
datePublished: Sun Sep 01 2024 15:29:29 GMT+0000 (Coordinated Universal Time)
cuid: cm0jq99fu002009mjami75kuo
slug: how-to-send-your-first-cross-chain-message-using-wormhole
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725204465427/6766666e-1986-43e7-8561-73cc52fbf778.png
tags: ethereum, solidity, smart-contracts, wormhole

---

# Introduction

In this guide, we are going to integrate Wormhole into our existing solidity smart contracts. While numerous solutions exist for sending cross-chain messages, Wormhole stands out as one of the most streamlined options for developers looking to expand their smart contracts across multiple blockchains. In my experience, Wormhole offers a user-friendly approach that simplifies the process of going cross-chain for smart contract developers of all skill levels.  

![Image](https://pbs.twimg.com/profile_banners/1417845872436547587/1715140701/1500x500 align="left")

# What is Wormhole?

Wormhole is a cross-chain message transfer protocol that enables seamless communication between different blockchains. Traditionally, blockchains operate as isolated systems, making it challenging to directly transfer tokens or send messages from one chain to another. This is where Wormhole steps in, providing a trustless solution for transferring messages across various blockchain networks. By bridging these isolated systems, Wormhole facilitates interoperability and opens up new possibilities for decentralized applications and cross-chain functionality.

# Step-by-step guide:

* Environment setup
    
* Writing initial contracts
    
* Integrating wormhole
    
* sending cross-chain messages
    

# Environment Setup

### Pre-requisites:

* Setup Node.js v18+ (recommended via [nvm](https://github.com/nvm-sh/nvm) with `nvm install 18`)
    
* Setup hardhat v2+ (recommended via [npm](https://hardhat.org/hardhat-runner/docs/getting-started) with `npx hardhat init`)
    

### Setup your project

Let's set up a hardhat project to compile and deploy our Solidity Contract. Create a new hardhat project with the following command:

```bash
npx hardhat init
```

After successful compilation, we need to add networks to our `hardhat.config.js` file.

Edit the hardhat.config.js file with the following code:

```javascript
require("@nomicfoundation/hardhat-toolbox");

const PRIVATE_KEY = "YOUR PRIVATE KEY";

/** @type import('hardhat/config').HardhatUserConfig */
module.exports = {
  solidity: "0.8.24",
  networks: {
    opSepolia: {
      url: `YOUR OPTIMISM SEPOLIA RPC URL`,
      accounts: [PRIVATE_KEY],
    },
    baseSepolia: {
      url: `YOUR BASE SEPOLIA RPC URL`,
      accounts: [PRIVATE_KEY],
    },
  },
};
```

Also, `deploy.js` file with the following code to set up deployment.

```javascript
const hre = require("hardhat");

async function main() {
  const [deployer] = await hre.ethers.getSigners();

  console.log("Deploying contracts with the account:", deployer.address);

  const contract = await hre.ethers.deployContract("YourContract");

  console.log("Contract address:", await contract.getAddress());
}

main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

Finally, to deploy the contract, execute the following command in the terminal:

```javascript
npx hardhat run scripts/deploy.js --network opSepolia
```

# Writing initial contracts

Let's examine an example contract that demonstrates how we can send messages on a single blockchain.

```javascript
contract HelloWormhole {
    event MessageSent(uint256 messageId, string message );
    event MessageReceived(address sourceAddress, uint256 messageId, string message);

    function sendMessage(
        address targetAddress,
        uint256 messageId,
        string memory message) public {
            HelloWormhole(targetAddress).receiveMessage(address(this), messageId, message);
            emit MessageSent(messageId, message);
        }

    function receiveMessage(
        address sourceAddress,
        uint256 messageId, 
        string memory message) public {
            emit MessageReceived(sourceAddress, messageId, message);
        }
}
```

This contract demonstrates a simple message-sending mechanism between identical HelloWormhole contracts on a single network. However, it fails to address a crucial aspect of modern blockchain ecosystems: the existence of multiple, diverse blockchains. In reality, users often need to send cross-chain messages from one blockchain to another. While we acknowledge that blockchains are inherently isolated systems, the question arises: how can we make cross-chain communication possible? That's where **<mark>Wormhole</mark>** helps us with.

# Integrating Wormhole

Let's integrate Wormhole into our existing contracts.

## Wormhole ChainIds

In the Wormhole ecosystem, distinct chain identifiers are used to specify the source and destination chains for cross-chain communications. It's important to note that the chainId parameters used in the following contracts are specific to Wormhole's network identification system, rather than the standard Ethereum-based chain IDs. These Wormhole-specific chain IDs uniquely identify each blockchain network within the Wormhole protocol.

Reference - [https://docs.wormhole.com/wormhole/reference/constants](https://docs.wormhole.com/wormhole/reference/constants)

## Required Contracts:

* IWormholeRelayer.sol - [https://github.com/wormhole-foundation/wormhole-solidity-sdk/blob/main/src/interfaces/IWormholeRelayer.sol](https://github.com/wormhole-foundation/wormhole-solidity-sdk/blob/main/src/interfaces/IWormholeRelayer.sol)
    
* IWormholeReceiver.sol - [https://github.com/wormhole-foundation/wormhole-solidity-sdk/blob/main/src/interfaces/IWormholeRelaye.sol](https://github.com/wormhole-foundation/wormhole-solidity-sdk/blob/main/src/interfaces/IWormholeRelaye.sol)
    

Add these contracts inside the project. So that we can inherit those to our `HelloWormhole.sol` contract.

Here is the `HelloWormhole.sol` contract after integrating Wormhole :  

```solidity
import "../IWormholeRelayer.sol";
import "../IWormholeReceiver.sol";

contract HelloWormhole is IWormholeReceiver {
    event MessageSent(uint256 messageId, string message );
    event MessageReceived(address sourceAddress, uint256 messageId, string message);

    // The address of the wormhole relayer
    IWormholeRelayer public immutable wormholeRelayer;

    // Initializing the wormhole relayer
    constructor(address _wormholeRelayer) {
        wormholeRelayer = IWormholeRelayer(_wormholeRelayer);
    }

    // Quotes the cost of sending a message to the targetChain with the given gas_limit using Wormhole
    function quoteCrossChainSendingFee(
        uint16 targetChain,
        uint256 gas_limit
        ) public view returns (uint256 cost) {
            (cost, ) = wormholeRelayer.quoteEVMDeliveryPrice(
                targetChain,
                0,
                gas_limit
            );
        }

    function sendMessage(
        uint16 targetChain,
        uint256 gas_limit,
        address targetAddress,
        uint256 messageId,
        string memory message) public payable{
            // Check the provided fee is valid
            uint256 cost = quoteCrossChainSendingFee(targetChain, gas_limit);
            require(msg.value == cost, "Invalid value sent");

            wormholeRelayer.sendPayloadToEvm{value: cost}(
                targetChain, // Wormhole chainId of the target chain
                targetAddress,
                abi.encode(messageId, message), // payload
                0, // no receiver value needed since we're just passing a message
                gas_limit,
                currentChain, // Wormhole chainId of the refund address
                msg.sender // Address  where the refund will be sent
            );
        }

    function receiveWormholeMessages(
        bytes memory payload,
        bytes[] memory, // additionalVaas
        bytes32 sourceAddress,
        uint16 sourceChain,
        bytes32 // delivery hash
        ) public payable override onlyWormholeRelayer notBase {
            require(payload.length > 0, "Invalid payload");

            (uint256 messageId, string memory message) = abi.decode(
                payload,
                (uint256, string)
            );

            address source = address(uint160(uint256(sourceAddress)));

            emit MessageReceived(source, messageId, message);
        }
}
```

Let's see all the components of the contracts and how it works 😇

## Components

### wormholeRelayer

```solidity

    // The address of the wormhole relayer
    IWormholeRelayer public immutable wormholeRelayer;
```

This is the address of the relayer contract which would relay our message.

Here are the addresses of the relayer contracts used in all EVM chains: [https://docs.wormhole.com/wormhole/reference/blockchain-environments/evm](https://docs.wormhole.com/wormhole/reference/blockchain-environments/evm)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1725201470005/3a9496c3-8127-46a5-b669-a8db57b811ee.png align="center")

### QuoteCrossChainSendingFee

```solidity
// Quotes the cost of sending a message to the targetChain with the given gas_limit using Wormhole
    function quoteCrossChainSendingFee(
        uint16 targetChain,
        uint256 gas_limit
        ) public view returns (uint256 cost) {
            (cost, ) = wormholeRelayer.quoteEVMDeliveryPrice(
                targetChain,
                0,
                gas_limit
            );
        }
```

Wormhole charges some amount of fee to be passed in the `sendMessage` function to deliver it cross-chain. This function returns the fee based on the `targetChain` and the `gas_limit` provided.  

### sendMessage

```solidity
function sendMessage(
        uint16 targetChain,
        uint256 gas_limit,
        address targetAddress,
        uint256 messageId,
        string memory message) public payable{
            // Check the provided fee is valid
            uint256 cost = quoteCrossChainSendingFee(targetChain, gas_limit);
            require(msg.value == cost, "Invalid value sent");

            wormholeRelayer.sendPayloadToEvm{value: cost}(
                targetChain, // Wormhole chainId of the target chain
                targetAddress,
                abi.encode(messageId, message), // payload
                0, // no receiver value needed since we're just passing a message
                gas_limit,
                currentChain, // Wormhole chainId of the refund address
                msg.sender // Address where the refund will be sent
            );
        }
```

Sends a cross-chain message to the target address on the target chain. Internally, calls `sendPayloadToEvm` to interact with wormholeRelayers. You need to pass the fee as `msg.value` to send the message.  
  
Additionally, refunds the excess amount of deducted fees to any of the supported chains and to any address.

### receiveWormholeMessages

```solidity
function receiveWormholeMessages(
        bytes memory payload,
        bytes[] memory, // additionalVaas
        bytes32 sourceAddress,
        uint16 sourceChain,
        bytes32 // delivery hash
        ) public payable override onlyWormholeRelayer notBase {
            require(payload.length > 0, "Invalid payload");

            (uint256 messageId, string memory message) = abi.decode(
                payload,
                (uint256, string)
            );

            address source = address(uint160(uint256(sourceAddress)));

            emit MessageReceived(source, messageId, message);
        }
```

Receives the message emitted from the source address on the source chain where the current contract is provided as the target address.

### Validation

It's crucial to verify both the source address and the chain ID from which the message originates. This verification step is essential because malicious actors could potentially send unauthorized or harmful messages to your contract.

**Check SourceAddress**

```solidity
require(
    address(uint160(uint256(sourceAddress))) == designatedSourceAddress,
    "Invalid source address"
);
```

**Check SourceChain**

```solidity
require(
    sourceChain == designatedChainId,
    "Invalid source chain"
);
```

# Sending cross-chain messages

Let's deploy the contract to Optimism Sepolia and Base Sepolia.  

```bash
npx hardhat run scripts/deploy.js --network opSepolia
npx hardhat run scripts/deploy.js --network baseSepolia
```

After deployment let's send a message from Optimism Sepolia to Base Sepolia.

We are using Ethers V5.7.2 in the following example.

```javascript
const opSepolia = {
    rpcUrl: "YOUR OPTIMISM SEPOLIA RPCURL",
    sourceAddress: "Address of the HelloWormhole contract deployed on Optimism Sepolia",
}

const baseSepolia = {
    rpcUrl: "YOUR BASE SEPOLIA RPCURL",
    targetAddress: "Address of the HelloWormhole contract deployed on Base Sepolia",
    wormholeChainId: 10004, // Reference : https://docs.wormhole.com/wormhole/reference/blockchain-environments/evm
    gasLimit: 300000,
}

const main = async () => {
    const provider = new ethers.providers.JsonRpcProvider(opSepolia.rpcUrl);
    
    const HelloWormholeContract = new ethers.Contract(
        opSepolia.sourceAddress,
        HelloWormholeABI,
        provider
    );

    const sendingCost = await HelloWormholeContract.quoteCrossChainSendingFee(
        baseSepolia.wormholeChainId,
        baseSepolia.gasLimit
    );

     const signer = new ethers.Wallet("Your Private Key", provider);

     const data = HelloWormholeContract.interface.encodeFunctionData(
        "sendMessage",
        [
            baseSepolia.wormholeChainId,
            baseSepolia.gasLimit,
            baseSepolia.targetAddress,
            "1",
            "Hello Wormhole"
        ]
    );

    const unSignedTx = {
      to: opSepolia.sourceAddress,
      value: sendingCost,
      data,
    };

    const signedTx = await signer.sendTransaction(unSignedTx);

    const receipt = await signedTx.wait();

    console.log(receipt);
}

main();
```

# Performance

The performance of sending Cross-chain messages using Wormhole fully depends on the finality of the blocks.  
  
Your message can take more than 15 minutes to be delivered from Optimism Sepolia. Whereas, it takes 1 minute to be delivered from Avalanche Fuji.

# Our Project Fusion on ENCODE X WORMHOLE Hackathon

![Image](https://pbs.twimg.com/profile_banners/1425816757046972422/1724147675/1500x500 align="left")

Check our project submitted for the ENCODE X WORMHOLE Hackathon and see how our ZK-based smart contract wallet goes multi-chain using Wormhole.

[https://www.getfusion.tech](https://www.getfusion.tech/)