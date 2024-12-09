---
title: "Making Your First Batch ERC-20 Token Payment on Request Network: A Developer's Guide"
datePublished: Mon Dec 09 2024 20:59:04 GMT+0000 (Coordinated Universal Time)
cuid: cm4himfos000209mpea2oddtq
slug: making-your-first-batch-erc-20-token-payment-on-request-network-a-developers-guide
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1733777562747/bcd50e95-d6d7-4ffe-8053-4d2d33193abe.png
tags: blogging, development, developer, guide, integration, ethereum, web3, requests, decentralization, request-network

---

# Introduction

In this guide, we’ll explore how to integrate the Request Network into your dApp. We'll cover creating ERC-20 token requests using the Request Network and demonstrate how to batch-pay these requests efficiently in a single transaction.

# What is Request Network?

[Request Network](https://request.network/) is a blockchain-based financial infrastructure platform designed to empower web3 builders with innovative financial tools and services. The platform provides a comprehensive suite of solutions that enable developers to create advanced financial applications with improved data ownership, compliance, and traceability.

![Request price today, REQ to USD live price, marketcap and chart |  CoinMarketCap](https://s3.coinmarketcap.com/static-gravity/image/b626cfff787c4680a20ef8385b050866.jpg align="left")

# In this Guide

* Creating an ERC20 request
    
* Fetching and Tracking requests
    
* Paying the requests
    
* Batch paying the requests
    

## Prerequisites

Before we dive into the code, make sure you have:

* A Web3 wallet (like MetaMask)
    
* Basic understanding of React and blockchain development
    
* Familiarity with Ethereum and smart contract interactions
    
* The Request Network SDKs installed
    

To be specific you need the following SDKs to use this guide completely.

```plaintext
@requestnetwork/payment-processor
@requestnetwork/request-client.js
@requestnetwork/web3-signature
```

# Code and Resources

The guide is based on the project, [Request Tasks](https://dorahacks.io/buidl/20603) submitted for the [end-of-year hackathon](https://dorahacks.io/hackathon/request-network-end-of-year/).

Github: [https://github.com/AnoyRC/request\_tasks](https://github.com/AnoyRC/request_tasks)

Request Tasks is live at: [https://www.requesttasks.xyz/](https://www.requesttasks.xyz/)

# Getting Started

In this guide, we’ll skip the steps for setting up a wallet connection and installing essential packages like **Wagmi** and **Ethers**.

*<mark>Important Note: Request Network currently does not support Ethers v6.</mark>*

Before diving into request creation, ensure you have the following details ready:

* **Blockchain Details**
    
    Chain Name: Sepolia
    
    Chain Id: 11155111
    
    Rpc Url: [https://sepolia.drpc.org](https://sepolia.drpc.org)  
    Request Network Node Url: [https://sepolia.gateway.request.network/](https://sepolia.gateway.request.network/)
    
* **ERC-20 Token Details**
    
    Token Name: USDC
    
    Token Contract: [0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238](https://sepolia.etherscan.io/token/0x1c7d4b196cb0c7b01d743fbc6116a902379c7238)
    
    Decimals: 6
    
    Faucet Link: [https://faucet.circle.com/](https://faucet.circle.com/)
    
* **Payee/Payer Details**
    
    Assign these variables
    
    ```javascript
    const payeeIdentity = "";
    const payerIdentity = "";
    ```
    
    With these details in place, you’re ready to start creating requests.
    

# Creating an ERC-20 Request

Let’s create an ERC-20 payment request which the payee himself signs.

Create a `Web3SignatureProvider` who will sign the request.

```javascript
import { Web3SignatureProvider } from "@requestnetwork/web3-signature";

// for EthersV5
const web3SignatureProvider = new Web3SignatureProvider(provider);

// for Wagmi/Viem
const { data: walletClient } = useWalletClient();
const web3SignatureProvider = new Web3SignatureProvider(walletClient);
```

Initialize `RequestNetwork` to set up a connection with the request network node.

```javascript
import { RequestNetwork } from "@requestnetwork/request-client.js";

const requestClient = new RequestNetwork({
   nodeConnectionConfig: {
      baseURL: "https://sepolia.gateway.request.network/",
   },
   signatureProvider: web3SignatureProvider,
});
```

Create a request parameter that is required to sign.

```javascript
    import {Types, Utils} from "@requestnetwork/request-client.js";    

    // Address of payee
    const payeeIdentity = "";
    // Address of payer
    const payerIdentity = "";
    // If you want to impose extra fees
    const feeAmount = "0";
    const feeRecipient = payerIdentity;

    const requestCreateParameters = {
      requestInfo: {
        // The currency in which the request is denominated
        currency: {
          type: Types.RequestLogic.CURRENCY.ERC20,
          // USDC Token Contract
          value: "0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238",
          network: "sepolia",
        },

        // Sending 2 USDC, Amount * 10 ^ decimals
        expectedAmount: (2 * 10 ** 6).toFixed(0),

        // The payee identity. Not necessarily the same as the payment recipient.
        payee: {
          type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
          value: payeeIdentity,
        },

        // The payer identity. If omitted, any identity can pay the request.
        payer: {
          type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
          value: payerIdentity,
        },

        // The request creation timestamp.
        timestamp: Utils.getCurrentTimestampInSecond(),
      },

      // The paymentNetwork is the method of payment and related details.
      paymentNetwork: {
        id: Types.Extension.PAYMENT_NETWORK_ID.ERC20_FEE_PROXY_CONTRACT,
        parameters: {
          paymentNetworkName: "sepolia",
          paymentAddress: payeeIdentity,
          feeAddress: feeRecipient,
          feeAmount: feeAmount,
        },
      },

      // The contentData can contain anything.
      contentData: {
        topic: "guide",
        platform: "Hashnode",
      },

      // The identity that signs the request, either payee or payer identity.
      signer: {
        type: Types.Identity.TYPE.ETHEREUM_ADDRESS,
        value: payeeIdentity,
      },
    };
```

After setting the request parameter, we are ready to create a request.

```javascript
const request = await requestClient.createRequest(requestCreateParameters);

const confirmedRequestData = await request.waitForConfirmation();

const requestId = confirmedRequestData.requestId
```

Voila! You have created your first request on Request Network :)

![Celebrity gif. Rodney Dangerfield dressed as a car mechanic with his hand making an OK sign, bobbing back and forth while laughing.](https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExcDk4N3dnanlrZDNlZDZ2OTk0b29wYzlzMHQ3dzFyMWh3aWl2cjc1ZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/l3fQf1OEAq0iri9RC/giphy.gif align="center")

# Fetching and Tracking Request

Let’s fetch and check the status of the request.

```javascript
import { RequestNetwork } from "@requestnetwork/request-client.js";

const requestId = "";    

const requestClient = new RequestNetwork({
    nodeConnectionConfig: {
      baseURL: "https://sepolia.gateway.request.network/",
    },
});

const request = await requestClient.fromRequestId(requestId);
let requestData = request.getData();
```

You can track if the request is paid or not too.

```javascript
if (requestData.balance?.balance >= requestData.expectedAmount) {
    console.log("Request is Paid!!");
} else {
    console.log("Request is not paid!");
}
```

# Paying the requests

Before paying the request, we need the `requestData`.

```javascript
import { RequestNetwork } from "@requestnetwork/request-client.js";

const requestId = "";    

const requestClient = new RequestNetwork({
    nodeConnectionConfig: {
      baseURL: "https://sepolia.gateway.request.network/",
    },
});

const request = await requestClient.fromRequestId(requestId);
let requestData = request.getData();
```

To pay the requests, we need the signer. For this, only EtherV5 Provider & Signer is supported. Check out: [https://wagmi.sh/react/guides/ethers](https://wagmi.sh/react/guides/ethers) to convert walletClient to provider and signer if you are using Wagmi/Viem.

Request Network provides in-built SDK functions that check for ERC-20 approvals and execute if you haven’t approved your ERC-20 tokens.

```javascript
import { approveErc20,
      hasErc20Approval,
      hasSufficientFunds } from "@requestnetwork/payment-processor";

const _hasSufficientFunds = await hasSufficientFunds({
    request: requestData,
    address: requestData.payer.value,
    providerOptions: {
       provider: provider,
    },
});

if (!_hasSufficientFunds) {
    throw new Error("Insufficient funds");
    return;
}

const _hasErc20Approval = await hasErc20Approval(
    requestData,
    requestData.payer.value,
    signer
);

if (!_hasErc20Approval) {
    const approvalTx = await approveErc20(requestData, signer);
    await approvalTx.wait(2);
}
```

Finally, let’s pay the request.

```javascript
import { payRequest } from "@requestnetwork/payment-processor";

const paymentTx = await payRequest(requestData, signer);
await paymentTx.wait(2);
```

Now, we can check if the request is confirmed by the Request Network node.

```javascript
while (requestData.balance?.balance < requestData.expectedAmount) {
    requestData = await request.refresh();
    await new Promise((resolve) => setTimeout(resolve, 1000));
}

console.log("Request is now comfirmed and paid");
```

Congratulations! You have finally paid your first request :)

![Movie gif. Leonardo DiCaprio as Jay Gatsby in The Great Gatsby raises a champagne glass in a celebratory toast as fireworks explode behind him.](https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExOGZ5N2U2NXJhbXN1aGZoaWF3MzZ3ZHhiZzNzYXR6bzF3cnM4ZHBsNCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/g9582DNuQppxC/giphy.gif align="center")

# Batch paying the requests

Handling multiple ERC-20 token payment requests individually can be inefficient and costly due to higher gas fees and time-consuming execution. By batching these payments, you can streamline the process, reduce gas costs, and execute all requests in a single transaction.

For batching requests, we need to modify the user's payload. Let’s convert `requestData` to `EnrichedRequests`.

```javascript
import { RequestNetwork, Types } from "@requestnetwork/request-client.js";

const requestClient = new RequestNetwork({
    nodeConnectionConfig: {
      baseURL: "https://sepolia.gateway.request.network/",
    },
});

// Multiple RequestIds for multiple requests
const requestIds = []

const enrichedRequests = await Promise.all(
     requestIds.map(async (requestId) => {
       const request = await requestClient.fromRequestId(requestId);
       const requestData = request.getData();

       return {
         request: requestData,
         paymentSettings: {
           // In this case, we are paying the requests in USDC
           currency: {
             type: Types.RequestLogic.CURRENCY.ERC20,
             value: "0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238",
             network: "sepolia",
           },
         },
         paymentNetworkId:
           Types.Extension.PAYMENT_NETWORK_ID.ERC20_FEE_PROXY_CONTRACT,
       };
     })
);
```

In Batch Requests too, Request Network provides approval checking functions for ERC-20 tokens. Simply, import the function and use it.

```javascript
import { approveErc20BatchConversionIfNeeded,
} from "@requestnetwork/payment-processor";

await approveErc20BatchConversionIfNeeded(
        enrichedRequests[0].request,
        signer._address,
        signer,
        undefined,
        {
          currency: {
            type: Types.RequestLogic.CURRENCY.ERC20,
            value: "0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238",
            network: "sepolia",
          },
          maxToSpend: "0",
        },
        "0.1.0"
);
```

If you have more than one ERC-20 token, you are paying for, use the following code.

```javascript
import { approveErc20BatchConversionIfNeeded
} from "@requestnetwork/payment-processor";

for (const enrichedRequest of enrichedRequests) {
        await approveErc20BatchConversionIfNeeded(
          enrichedRequest.request,
          signer._address,
          signer,
          undefined,
          {
            currency: {
              type: Types.RequestLogic.CURRENCY.ERC20,
              value: "0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238",
              network: "sepolia",
            },
            maxToSpend: "0",
          },
          "0.1.0"
        );
      }
```

Finally, you can pay your batch request at once through `payBatchConversionProxyRequest`.

```javascript
import { payBatchConversionProxyRequest
} from "@requestnetwork/payment-processor";

const tx = await payBatchConversionProxyRequest(
        enrichedRequests,
        signer,
        {
          skipFeeUSDLimit: true,
          conversion: {
            currencyManager: currencyManager,
          },
          version: "0.1.0",
        }
      );

await tx.wait(2);
```

Congratulations, you have now fulfilled the Batch ERC-20 Payment request. Additionally, you can wait to see if all the requests have been fulfilled.

```javascript
import { RequestNetwork } from "@requestnetwork/request-client.js";

const requestClient = new RequestNetwork({
    nodeConnectionConfig: {
      baseURL: "https://sepolia.gateway.request.network/",
    },
});

// Multiple RequestIds for multiple requests
const requestIds = []

await Promise.all(
        requestIds.map(async (requestId) => {
          while (true) {
            const request = await requestClient.fromRequestId(requestId);
            const requestData = await request.refresh();

            if (requestData.balance?.balance >= requestData.expectedAmount) {
              await checkForCompletedTask(task);
              break;
            }

            await new Promise((resolve) => setTimeout(resolve, 1000));
          }
        })
      );

console.log("Batch Payment Successful!!");
```

![Video gif. A little boy in a Chuck E. Cheese birthday crown dances in celebration. Text, “Celebrate!!!”](https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExcjlpcnJlOWdnaXBoenFiaXBkeDFyYndxOWtpb3M1NHI2aXl5Y3hyMSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/YTbZzCkRQCEJa/giphy.gif align="center")

# Conclusion

Remember, the key to successful implementation lies in thorough testing, robust error handling, and a deep understanding of both the technical mechanics and the broader blockchain ecosystem.  
  
The journey into blockchain-powered payments is just beginning, and tools like Request Network are paving the way for more efficient, transparent, and accessible financial technologies.  
  
Happy coding!