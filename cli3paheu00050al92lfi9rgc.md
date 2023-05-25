---
title: "How to make your first blockchain integrated game using Reneverse Unity SDK"
seoTitle: "Blockchain Integrated Unity Game using Reneverse"
datePublished: Thu May 25 2023 22:23:26 GMT+0000 (Coordinated Universal Time)
cuid: cli3paheu00050al92lfi9rgc
slug: how-to-make-your-first-blockchain-integrated-game-using-reneverse-unity-sdk
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685053225976/540338f9-35d0-42ba-a668-12f797643b28.png
tags: game-development, guide, blockchain, web3, reneverse

---

# Introduction

In this guide, we will learn how to make your first Unity game which will be integrated with blockchain using Reneverse Unity SDK. Web3 gaming adoption has been gaining significant traction in recent years. Web3 refers to the third generation of the internet, characterized by the integration of blockchain technology and decentralized protocols. This technology has the potential to revolutionize the gaming industry by offering benefits such as ownership of in-game assets, transparent and secure transactions, interoperability between games, and play-to-earn opportunities.

# What is Reneverse?

[Reneverse](https://reneverse.io/) is a platform that helps you to build a Blockchain Integrated Unity game without having to know Solidity or Rust. It manages everything from onboarding users to web3 gaming to easing the development and coding required to integrate your game with blockchain.

Just Create a new account at [Reneverse](https://app.reneverse.io/) and you have a fully functioning wallet that can interact with games integrated with Reneverse.

# Integrating Reneverse with your Unity game

## Create your account at Reneverse

Head over to [https://app.reneverse.io/register](https://app.reneverse.io/register) and fill up your credentials while selecting User Type as Game Developer.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685028733837/2c141915-5e25-4225-aec3-7f2a5c13c887.png align="center")

After OTP verification, head over to [https://app.reneverse.io/login](https://app.reneverse.io/login) and log in to your account.

## Create a template for your game in Reneverse

After logging in to your account, you will be presented with the Reneverse Dashboard.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685029298134/6e1f2b63-bb18-40c6-ab1f-81c90ebab7ef.png align="center")

Hover in your profile picture at the top right and you will see the following options.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685029376129/137ea053-dd40-4062-b68d-a929271c46f0.png align="center")

Let's create our first organization by clicking on Organizations :)

### Creating Organization

Tap on New Organization, and fill up the credential to create your first organization. Organization in Reneverse helps developers like us to collaborate with other developers, for administration purposes or development of the same game.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685029680916/1f1dd108-e48c-4dc5-be4a-27288a060c66.png align="center")

After creating your Organization,

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685030131699/42d3ab95-0b79-4704-b5ec-f96ee037570b.png align="center")

### Adding your game to Reneverse Dashboard

Let's start creating our game by adding our game to Reneverse Dashboard by clicking on New Game.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685030578815/71689395-b507-43db-875e-41614226fb61.png align="center")

Fill up the details and click on Create. I have added details for my game:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685030975700/925c7c11-0cc8-4abe-a7d6-f5e8a9be52ea.png align="center")

Wait for a few minutes and it will be added to your dashboard.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685031294853/611f74c7-24bb-44af-89f3-142537d5762a.png align="center")

### Creating a collection and making your asset template

Open your game and click on New Collection.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685031585451/0b19bc9d-f328-4c6e-b3e6-6b6af77b62c9.png align="center")

Click on the collection you made.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685031824635/cea869a5-4b7d-4922-a461-83afbae29a61.png align="center")

Let's create our first asset template by clicking on New Asset Template.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685032134033/cce962ec-12e3-4ea9-8440-27d9a27411e5.png align="center")

Add details for your first asset template.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685032405522/d377c105-d74d-42e1-a40e-181b8a415b36.png align="center")

Open your asset template and we are ready to mint our first asset :)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685032572477/b773c16c-67f9-4e67-aa86-e9e8905f9c3f.png align="center")

### Mint our first asset

Open the metadata tab, and let\\'s add some attributes for our NFTs. Click on New Attribute and add a color attribute of the text type.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685044214009/5226ffb6-d4e3-453f-8c2f-cf2239bca2ad.png align="center")

Let's add one more. Add a type attribute of the text type.

Let's add some presets for our attributes. After typing, click on Add to successfully add an attribute preset. And don't forget to save it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685044963336/aba9576e-e35b-41f1-992c-54e15b7958cc.png align="center")

Open the data folder and click on the image folder to add some images to it which would be needed when we have mint our NFTs. You can also upload all the images needed for the NFTs here.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685045766106/514bca2b-4e91-41f1-8657-fab894f6c7eb.png align="center")

For now, we have all the minimum requirements for minting our first NFT. We have images and attributes. LETS MINT :)

Click on Mint Asset to open the mint asset dialog.

We are going to mint the assets ourselves, so let's add our username there. And of course, fill in the necessary details required.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685046011229/2dfe5ae4-cc40-43b0-b803-a02d97e7d6b6.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685046106557/df3200e5-c297-4360-9460-cd7900333c7a.png align="center")

Click on Mint and we have started the process of minting. It will take 4-5 minutes to mint. Meanwhile, let's learn about minting, shall we?

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685046220107/c067ed03-ca5c-4cab-9007-7a393340f5f8.png align="center")

### What is Minting?

In the context of web3 and blockchain technology, "minting" refers to the process of creating a new digital asset or token on a blockchain. It involves the generation and registration of a unique token that represents a specific item, asset, or piece of data.

When minting a token, a smart contract is written to define the characteristics and properties of the token. These properties can include the total supply, the name, the symbol, and any additional metadata associated with the token. The smart contract ensures that the token adheres to predefined rules and standards, such as the ERC-721 or ERC-1155 standards for non-fungible tokens (NFTs).

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685047722398/5381a74e-753a-4292-97e0-5a97e6088924.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685047738645/70ae7c88-766f-49e8-bf40-a5c92fc5b624.png align="center")

### Generate Credentials to integrate with Unity

We need the API key, Private Key and GameID to integrate our assets with Unity. Go back to the Collections tab, and click on settings. Open the API credentials on reveal the keys.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685048174524/7a315a84-bcb2-4b7e-84c6-39d261c32b4d.png align="center")

Well, we are missing an API Key and a private key. Let's generate it, and click on Generate API Credentials. Expiry data cannot be more than 6 months from the current date.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685048237929/3942d790-6410-495d-93cc-78a4b9409e54.png align="center")

Click on Generate.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685048350218/75990451-15d5-4197-9390-8e9a5d55fe26.png align="center")

**<mark>Don't close the tab in your browser, we will copy the credentials from here.</mark>**

## Adding a Login Button and Authorizing Requests to play your game from the user

All we need to do is to add a login button so that users can type their email ID in our game and send authorization requests to our Reneverse dashboard. This will allow the user to connect their Reneverse wallet with our Unity game and access their assets.

I like to keep my workplace tidy. So, make an empty object in Unity and create a new script(maybe name it ReneverseManager.cs) to it.

### Linking Reneverse credentials with Unity

In Unity, open the Window menu and click on Reneverse settings.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685049790806/208c8b91-7bb6-47ae-bbf5-9ef802361c59.png align="center")

Add the credentials from the browser tab you kept open earlier.

### Creating UI

We need 2 panels, one consisting of a login window and a second with a countdown window, to give the users a limited time to authorize the login through the Reneverse dashboard.

You are free to use your design, and feel free to rate my designs :)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685050299194/a8c3962d-12e7-4330-bb16-d2b12efa9f30.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685050345136/1b1c7362-fde7-4676-9ff2-7261085c92e1.png align="center")

Parent both of them in a single panel, keeping the countdown panel inactive.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685050407254/a5d7e8db-7ada-4247-a327-b483d31965fd.png align="center")

Let's get into coding.

### Creating a connect user function

Open the ReneverseManager.cs and let's add a connect user function to it which will pop up a 30 Seconds countdown timer for the user to authorize the game to Reneverse Dashboard.

```csharp
using Rene.Sdk;
using Rene.Sdk.Api.Game.Data;
using ReneVerse;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Threading.Tasks;
using TMPro;
using UnityEngine;

public class ReneverseManager : MonoBehaviour
{
    public static string EmailHandler;
    
    // The TextMeshPro Inputfield
    public GameObject Email;
    // The Timer Text Field which would update every second
    public TextMeshProUGUI Timer;
    
    // The parent panet which contains both sign in and countdown panel
    public GameObject LogInPanel;
    // The Countdown panel which contains the timer
    public GameObject CountdownPanel;

    
    API ReneAPI;

    async Task ConnectUser()
    {
        ReneAPI = ReneAPIManager.API();
        EmailHandler = Email.GetComponent<TMP_InputField>().text;
        bool connected = await ReneAPI.Game().Connect(EmailHandler);
        Debug.Log(connected);
        if (!connected) return;
        StartCoroutine(ConnectReneService(ReneAPI));
    }

    private IEnumerator ConnectReneService(API reneApi)
    {
        CountdownPanel.SetActive(true);
        var counter = 30;
        var userConnected = false;
        var secondsToDecrement = 1;
        while (counter >= 0 && !userConnected)
        {
            Timer.text = counter.ToString();
            if (reneApi.IsAuthorized())
            {

                CountdownPanel.SetActive(false);
                LogInPanel.SetActive(false);
                
                
                // Will Fetch our assets from here
                
              
                userConnected = true;
                LoginStatus = true;
            }

            yield return new WaitForSeconds(secondsToDecrement);
            counter -= secondsToDecrement;
        }
        CountdownPanel.SetActive(false);
    }
}
```

Before fetching our assets, let's learn what our object looks like.

### The Asset Object

We would be fetching the whole Asset object from Reneverse. That's why learning the fields of our Asset Object is Essential. Every Game has its different mechanics and needs, so we would fetch only the data we require.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685051257465/00b686c3-0f71-4a0f-91d0-a4d27aac1e98.png align="center")

### Fetching Assets owned by User

Let's create a function that fetches the assets owned by the user if they have authorized our game.

```csharp
private async Task GetUserAssetsAsync(API reneApi)
    {
        AssetsResponse.AssetsData userAssets = await reneApi.Game().Assets();
        userAssets?.Items.ForEach(asset =>
        {
            // If your game requires the name and NFTId, you can store it in a dictionary or create a class that suits your game
            Debug.Log(asset.Metadata.Name);
            Debug.Log(asset.NftId);
        });
    }
```

Example of fetching complex data:

```csharp
private async Task GetUserAssetsAsync(API reneApi)
{
    AssetsResponse.AssetsData userAssets = await reneApi.Game().Assets();
    userAssets?.Items.ForEach(asset =>
    {
        string assetName = asset.Metadata.Name;
        string assetImageUrl = asset.Metadata.Image;
        string assetColor = "";
        asset.Metadata?.Attributes?.ForEach(attribute =>
        {
            if (attribute.TraitType == "Color")
            {
                assetColor = attribute.Value;
            }
        });
        
        Asset assetObj = new Asset(assetName, assetImageUrl, assetColor);
        // Add to a static list of Asset object maybe
     });
}

public class Asset
{
    public string AssetName { get; set; }
    public string AssetUrl { get; set; }

    public string AssetColor {get; set;}
    public Asset(string assetName, string assetUrl, string assetColor)
    {
        AssetName = assetName;
        AssetUrl = assetUrl;
        AssetStyle = assetColor;
    }
}
```

Lastly, Add this function to the `ConnectReneService` :

```csharp
using Rene.Sdk;
using Rene.Sdk.Api.Game.Data;
using ReneVerse;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Threading.Tasks;
using TMPro;
using UnityEngine;

public class ReneverseManager : MonoBehaviour
{
    public static string EmailHandler;
    
    // The TextMeshPro Inputfield
    public GameObject Email;
    // The Timer Text Field which would update every second
    public TextMeshProUGUI Timer;
    
    // The parent panet which contains both sign in and countdown panel
    public GameObject LogInPanel;
    // The Countdown panel which contains the timer
    public GameObject CountdownPanel;

    
    API ReneAPI;

    async Task ConnectUser()
    {
        ReneAPI = ReneAPIManager.API();
        EmailHandler = Email.GetComponent<TMP_InputField>().text;
        bool connected = await ReneAPI.Game().Connect(EmailHandler);
        Debug.Log(connected);
        if (!connected) return;
        StartCoroutine(ConnectReneService(ReneAPI));
    }

    private IEnumerator ConnectReneService(API reneApi)
    {
        CountdownPanel.SetActive(true);
        var counter = 30;
        var userConnected = false;
        var secondsToDecrement = 1;
        while (counter >= 0 && !userConnected)
        {
            Timer.text = counter.ToString();
            if (reneApi.IsAuthorized())
            {

                CountdownPanel.SetActive(false);
                LogInPanel.SetActive(false);
                
                
                // Will Fetch our assets from here
                yield return GetUserAssetsAsync(reneApi);
                
              
                userConnected = true;
                LoginStatus = true;
            }

            yield return new WaitForSeconds(secondsToDecrement);
            counter -= secondsToDecrement;
        }
        CountdownPanel.SetActive(false);
    }
}
```

Unity buttons don't accept functions returning tasks. Let's fix it!!

### Adding Functions suitable for Unity Buttons to interact

Add this function in the code, to apply in the login button.

```csharp
public async void SignIn()
{
    await ConnectUser();
}
```

## The Final Code

```csharp
using Rene.Sdk;
using Rene.Sdk.Api.Game.Data;
using ReneVerse;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Threading.Tasks;
using TMPro;
using UnityEngine;

public class ReneverseManager : MonoBehaviour
{
    public static string EmailHandler;
    
    // The TextMeshPro Inputfield
    public GameObject Email;
    // The Timer Text Field which would update every second
    public TextMeshProUGUI Timer;
    
    // The parent panet which contains both sign in and countdown panel
    public GameObject LogInPanel;
    // The Countdown panel which contains the timer
    public GameObject CountdownPanel;

    
    API ReneAPI;

    async Task ConnectUser()
    {
        ReneAPI = ReneAPIManager.API();
        EmailHandler = Email.GetComponent<TMP_InputField>().text;
        bool connected = await ReneAPI.Game().Connect(EmailHandler);
        Debug.Log(connected);
        if (!connected) return;
        StartCoroutine(ConnectReneService(ReneAPI));
    }

    private IEnumerator ConnectReneService(API reneApi)
    {
        CountdownPanel.SetActive(true);
        var counter = 30;
        var userConnected = false;
        var secondsToDecrement = 1;
        while (counter >= 0 && !userConnected)
        {
            Timer.text = counter.ToString();
            if (reneApi.IsAuthorized())
            {

                CountdownPanel.SetActive(false);
                LogInPanel.SetActive(false);
                
                
                // Will Fetch our assets from here
                yield return GetUserAssetsAsync(reneApi);
                
              
                userConnected = true;
                LoginStatus = true;
            }

            yield return new WaitForSeconds(secondsToDecrement);
            counter -= secondsToDecrement;
        }
        CountdownPanel.SetActive(false);
    }
    private async Task GetUserAssetsAsync(API reneApi)
    {
        AssetsResponse.AssetsData userAssets = await reneApi.Game().Assets();
        userAssets?.Items.ForEach(asset =>
        {
            string assetName = asset.Metadata.Name;
            string assetImageUrl = asset.Metadata.Image;
            string assetColor = "";
            asset.Metadata?.Attributes?.ForEach(attribute =>
            {
                if (attribute.TraitType == "Color")
                {
                    assetColor = attribute.Value;
                }
            });
        
            Asset assetObj = new Asset(assetName, assetImageUrl, assetColor);
            // Add to a static list of Asset objects maybe
        });
    }
}

public class Asset
{
    public string AssetName { get; set; }
    public string AssetUrl { get; set; }

    public string AssetColor {get; set;}
    public Asset(string assetName, string assetUrl, string assetColor)
    {
        AssetName = assetName;
        AssetUrl = assetUrl;
        AssetStyle = assetColor;
    }
}
```

Check out Both the games developed by me for the Reneverse Borderless Gaming Hackathon and give them a try!!

* [https://anoyrc.itch.io/planet-driver](https://anoyrc.itch.io/planet-driver)
    
* [https://anoyrc.itch.io/beat-rider](https://anoyrc.itch.io/beat-rider)
    

# Useful Links

* [Reneverse](https://reneverse.io/)
    
* [Reneverse Login/SignUp](https://app.reneverse.io/)
    
* [Documentation](https://docs.reneverse.io/reneverse-tour/what-is-reneverse)
    
* [Planet Driver(Hackathon Submission)](https://anoyrc.itch.io/planet-driver)
    
* [Beat Rider(Hackathon Submission)](https://anoyrc.itch.io/beat-rider)
    

Do Drop a like, if you think this article helped you :)