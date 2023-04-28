---
title: "21 - Multiplayer Decentralized Card Game"
datePublished: Tue Feb 14 2023 04:31:50 GMT+0000 (Coordinated Universal Time)
cuid: cle3qz7u0000308l4atn86wpy
slug: 21-multiplayer-decentralized-card-game
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1676348982529/f90eb156-a71b-43b3-a3ab-9d6868255470.png
tags: projects, thirdweb, thirdwebhackathon, breakdown, submission

---

## Context-

Hi, I am Anoy and with my friend Rahul, we created 21, a decentralized multiplayer game resembling blackjack.

I bet that every card game you see nowadays will contain a lo-fi green board, where you play with the same cards day after day. So, we thought about trying out something different and out of the box.

Without any further adieu, let's jump right to the breakdown:)

## Project Breakdown-

We made this project in Unity 2021.3.16f1. We thought about using Three.js but the Gaming kit looked a lot more fun than using regular sdk. As we have been participating in some hackathons recently, we are bored using typescript and javascript.

For making UI, maximum people stick to use Figma. But we used Affinity Designer to make the UI concepts and bring them to life in Unity. For me, Affinity Designer is better than Adobe Illustrator.

Now coming to the 3d renders, we used Blender 3.4. It is a free-to-use open-source 3D modeling software. 3d modeling was my hobby, so I thought why not we put that to use and give a 3D touch to a 2D game :)

To initiate multiplayer in Unity, there is a popular package known as Photon PUN 2, we used the free version, which provides a maximum of 20 players to join the server and the backup is not awesome too. But what's a card game if it is not multiplayer?

And lastly, the third web!! The awesome Gaming-kit provides build it function to access our contract from the dashboard. Even though the code provided in the dashboard is not updated, the docs are. Made in Mumbai, testnet for polygon and it was a blast making it.

## Problems Faced -

First of all, we didn't know much about unity, our knowledge about photon was null. Yet, we spent 3 days of hackathon just reading the respective docs and learning about them. The setting of Photon took 3 days and Unity was easy to use, as we are engineering students, we know how to code in C#.

The open pack function of the pack contract was our major problem. It was solved on the last day of the submission. As our pack was large, it was exceeding the gas limit. But fixing didn't solve our problem at all. We cannot fetch the ERC1155 reward as we were using contract.write. It returns transaction result which doesn't contains our rewards. 9 hrs left for submission, and we decided to take the hard way. We decided to compare the ERC1155 balance of each skin, before and after the transaction. Although It takes time, it gets the work done.

## Gallery -

![UI Concept of our Game](https://cdn.hashnode.com/res/hashnode/image/upload/v1676348530492/30f2b983-54e9-4a40-93db-ac6718dca32d.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676348610163/4877fcd6-34a5-4d84-bbc1-9f55944d98ed.png align="center")

The Difference between concept and in-game experience has been shown, as more ideas bloom while making the project.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676348743501/42a61f29-4741-4ae7-8c7c-5fd7004296ec.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676349018754/1b5587b1-d152-4a30-bfb8-95694765475f.png align="center")

Some renders, made in Blender and are used in in-game UI.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676348857453/33a7c769-0cda-4026-96e0-1332cbfd482d.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1676348889613/b92aef57-2afe-4491-bb3a-8ec6f367655b.png align="center")

Some card skin concepts, the first was taken from royalty-free vector files.