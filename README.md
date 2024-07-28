# Foundry Smart Contract Lottery

This README provides clear instructions and includes all the necessary details to get started, deploy, and test this `Smart Contract Lottery` project.

## Table of Contents

  - [Table of Contents](#table-of-contents)
  - [Getting Started](#getting-started)
    - [Requirements](#requirements)
    - [Quickstart](#quickstart)
  - [Usage](#usage)
    - [Start a Local Node](#start-a-local-node)
    - [Library](#library)
  - [Deploy](#deploy)
    - [Deploy - Other Network](#deploy---other-network)
  - [Testing](#testing)
    - [Test Coverage](#test-coverage)
  - [Deployment to a Testnet or Mainnet](#deployment-to-a-testnet-or-mainnet)
    - [Setup Environment Variables](#setup-environment-variables)
    - [Get Testnet ETH](#get-testnet-eth)
    - [Deploy](#deploy-1)
    - [Register a Chainlink Automation Upkeep](#register-a-chainlink-automation-upkeep)
  - [Scripts](#scripts)
    - [Estimate Gas](#estimate-gas)
    - [Formatting](#formatting)
  - [Additional Info](#additional-info)
    - [Let's Talk About What "Official" Means](#lets-talk-about-what-official-means)
  - [Summary](#summary)
  - [Thank You!](#thank-you)

## Getting Started

### Requirements

- **Git**
  - You'll know you did it right if you can run `git --version` and see a response like `git version x.x.x`
- **Foundry**
  - You'll know you did it right if you can run `forge --version` and see a response like `forge 0.2.0 (816e00b 2023-03-16T00:05:26.396218Z)`

### Quickstart

```sh
git clone https://github.com/Cyfrin/foundry-smart-contract-lottery-cu
cd foundry-smart-contract-lottery-cu
forge build
```

## Usage

### Start a Local Node

```sh
make anvil
```

### Library

If you're having a hard time installing the Chainlink library, you can optionally run this command:

```sh
forge install smartcontractkit/chainlink-brownie-contracts@0.6.1 --no-commit
```

## Deploy

This will default to your local node. You need to have it running in another terminal in order for it to deploy.

```sh
make deploy
```

### Deploy - Other Network

See below for details on deploying to other networks.

## Testing

We talk about 4 test tiers in the video:

1. Unit
2. Integration
3. Forked
4. Staging

This repo covers #1 and #3.

```sh
forge test
```

or

```sh
forge test --fork-url $SEPOLIA_RPC_URL
```

### Test Coverage

```sh
forge coverage
```

## Deployment to a Testnet or Mainnet

### Setup Environment Variables

You'll want to set your `SEPOLIA_RPC_URL` and `PRIVATE_KEY` as environment variables. You can add them to a `.env` file, similar to what you see in `.env.example`.

- **PRIVATE_KEY**: The private key of your account (like from MetaMask). NOTE: FOR DEVELOPMENT, PLEASE USE A KEY THAT DOESN'T HAVE ANY REAL FUNDS ASSOCIATED WITH IT.
- **SEPOLIA_RPC_URL**: This is the URL of the Sepolia testnet node you're working with. You can get set up with one for free from Alchemy.
- **ETHERSCAN_API_KEY**: Optionally, add your Etherscan API key if you want to verify your contract on Etherscan.

### Get Testnet ETH

Head over to [faucets.chain.link](https://faucets.chain.link/) and get some testnet ETH. You should see the ETH show up in your MetaMask.

### Deploy

```sh
make deploy ARGS="--network sepolia"
```

This will set up a Chainlink VRF Subscription for you. If you already have one, update it in the `scripts/HelperConfig.s.sol` file. It will also automatically add your contract as a consumer.

### Register a Chainlink Automation Upkeep

You can follow the [documentation](https://docs.chain.link/docs/chainlink-automation/) if you get lost.

Go to [automation.chain.link](https://automation.chain.link/) and register a new upkeep. Choose "Custom logic" as your trigger mechanism for automation. Your UI will look something like this once completed:

![Automation](automation_ui.png)

## Scripts

After deploying to a testnet or local net, you can run the scripts.

Using cast deployed locally example:

```sh
cast send <RAFFLE_CONTRACT_ADDRESS> "enterRaffle()" --value 0.1ether --private-key <PRIVATE_KEY> --rpc-url $SEPOLIA_RPC_URL
```

or, to create a Chainlink VRF Subscription:

```sh
make createSubscription ARGS="--network sepolia"
```

### Estimate Gas

You can estimate how much gas things cost by running:

```sh
forge snapshot
```

And you'll see an output file called `.gas-snapshot`.

### Formatting

To run code formatting:

```sh
forge fmt
```

## Additional Info

 `chainlink-brownie-contracts` is an official Chainlink repository. Here is the info: `chainlink-brownie-contracts` is an official repo. The repository is owned and maintained by the Chainlink team for this very purpose, and gets releases from the proper Chainlink release process. You can see it's still the `smartcontractkit` org as well.

[chainlink-brownie-contracts](https://github.com/smartcontractkit/chainlink-brownie-contracts)

### Let's Talk About What "Official" Means

The "official" release process is that Chainlink deploys its packages to npm. So technically, even downloading directly from `smartcontractkit/chainlink` is wrong because it could be using unreleased code.

So, you have two options:

1. Download from NPM and have your codebase have dependencies foreign to Foundry.
2. Download from the `chainlink-brownie-contracts` repo, which already downloads from npm and then packages it nicely for you to use in Foundry.

## Summary

- That is an official repo maintained by the same org.
- It downloads from the official release cycle Chainlink/contracts use (npm) and packages it nicely for digestion from Foundry.

## Thank You!
