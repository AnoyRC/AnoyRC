---
title: "Setting up Solana on Windows"
seoTitle: "Install Solana for windows"
datePublished: Sat Apr 29 2023 19:15:26 GMT+0000 (Coordinated Universal Time)
cuid: clh2d4kw5000h09l9hizd2n1v
slug: setting-up-solana-on-windows
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1682795660602/01d201d9-d780-44bc-baa0-8af5d602fb5d.png
tags: windows, blockchain, smart-contracts, installation, solana

---

# Introduction

Solana is becoming increasingly popular in the blockchain and cryptocurrency communities. The platform has seen significant growth in terms of adoption, user activity, and investment over the past year, attracting attention from both individual users and institutional investors.

Solana can be installed and run on the Windows operating system. Though, Installing the full development environment for Solana smart contract deployment is a lengthy process. With this guide, you can start writing, testing and deploying Solana smart contracts on localnet, devnet and mainnet-beta.

# What is Solana?

![Solana introduces 'state compression' to lower NFT storage costs](https://forkast.news/wp-content/uploads/2021/08/Solana-1260x787.png align="left")

Solana is a rapidly emerging blockchain platform that has gained significant attention in the blockchain space. Solana is a high-performance blockchain that is designed to handle high-throughput and low-latency transactions, making it well-suited for applications that require fast and efficient processing.

Solana achieves its high performance through its unique consensus mechanism called Proof of History (PoH), which uses a verifiable delay function (VDF) to generate a secure and reliable timestamp for each transaction. This allows Solana to process transactions in parallel and achieve extremely fast confirmation times, with the network currently capable of handling up to 65000 transactions per second.

# Setup

## Setup WSL

We are technically not going to use Windows for this project, but instead Linux! Windows has introduced a feature called Windows Subsystem for Linux which is a compatibility layer in Windows that allows users to run Linux command-line tools and applications directly on Windows.

To get started with WSL, we are going to need to install it. Go ahead and open up cmd.exe in Admin mode to start and then run this command:

```bash
wsl --install
```

This command will enable the required optional components, download the latest Linux kernel, set WSL 2 as your default and install a Linux distribution for you(Ubuntu by default).

## Enable Linux Subsystem Feature

Go to the Search bar located in the taskbar and type Windows feature.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682791821983/c6437ec5-59d9-4d13-b638-1ab34d7fe151.png align="center")

Click on "Turn Windows features on or off" and make sure the following options are checked:

* Windows Subsystem for Linux
    
* Virtual Machine Platform
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682792073472/e4c81d95-2e6b-4655-9131-86edbdf5bf3e.png align="center")

## Turn on Virtualization

Restart your machine and see if you can open the Ubuntu terminal. If you are still running into problems with it, this may mean your CPU does not have Virtualization enabled.

Restart your machine once again and open your BIOS panel. There are different ways to open the BIOS panel depending on your motherboard. Search Google and you will find the way to open BIOS panel for your system.

Finally, Go to the advanced setting option and turn on Virtualization.

Open your Ubuntu terminal and you are ready to go !!

## Install Required Dependencies

Open the Ubuntu terminal and make an admin account to start using it.

### Install Curl

```bash
sudo apt-get install curl
```

### Install NVM

```bash
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash 
```

Check if the NVM is installed, if not, restart the terminal.

```bash
command -v nvm
```

### Install the latest version of Node

```bash
nvm install --lts
```

## Installing Rust

In Solana, the programs are written in Rust. To install Rust, just use the command:

```bash
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

Once you're done, verify by doing:

```bash
rustup --version
```

Then, make sure the rust compiler is installed:

```bash
rustc --version
```

Lastly, let's make sure Cargo is working as well. Cargo is the rust package manager.

```bash
cargo --version
```

## Install Solana CLI

Solana has a super nice CLI that's going to be helpful when we want to test the programs we write.

Hope you have the Ubuntu terminal open :)

```bash
sh -c "$(curl -sSfL https://release.solana.com/v1.15.2/install)"
```

Confirm you have the desired version of Solana installed by running:

```bash
solana --version
```

### Run a local validator(optional)

Run these commands separately:

```bash
solana config set --url localhost
solana config get
```

And lastly, run this command:

```bash
solana-test-validator
```

VOILA !! You're now running a local validator.

This means that Solana is set up to talk to our local network! When developing programs, we can quickly test stuff on our computers.

## Install Mocha

Mocha is a nice testing framework to help us test our Solana programs.

```bash
npm install -g mocha
```

## Install Anchor

If you know about Hardhat from the world of Ethereum, it's sort of like that! Except it's built for Solana.

```bash
npm install --global yarn
```

From here run these commands separately:

```bash
sudo apt-get update && sudo apt-get upgrade && sudo apt-get install -y pkg-config build-essential libudev-dev libssl-dev

cargo install --git https://github.com/project-serum/anchor anchor-cli --locked
```

That's it! At this point, you can run this last command to make sure Anchor is ready to rock:

```bash
anchor --version
```

# Congratulations

Uff!! you can give a pat on your back for making it this far!!

## Create a test project and run it

Make a new project by running the following commands:

```bash
anchor init myproject --javascript
cd myproject
```

### Create a local keypair

Make a new keypair/wallet by using the following commands:

```bash
solana-keygen new
```

Check your current wallet address by running the following command:

```bash
solana address
```

Now, Spin up the `solana-test-validator` and test the boilerplate program in our local Solana network with our wallet

```bash
anchor test
```

As long as you're getting the green words "1 Passing! you're good to go !!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682795374744/2f31e5cf-1d71-45a7-a014-16d6e830d2a6.png align="center")

Congratulations you've successfully set up your Solana environment :)