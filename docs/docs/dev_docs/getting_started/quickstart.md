---
title: Quickstart
---

In this guide, you will

1. Set up the Aztec sandbox locally
2. Install the Aztec CLI
3. Use the CLI to deploy an example contract that comes with the sandbox
4. Use the CLI to interact with the contract you just deployed

... in less than 10 minutes.

## Prerequisites

- Node.js >= v18 (recommend installing with [nvm](https://github.com/nvm-sh/nvm))
- Docker and Docker Compose (Docker Desktop under WSL2 on windows)

## Install the Sandbox

You can run the Sandbox using either Docker or npm. In this guide we will use Docker, but you can learn more about alternative installation methods [here](../cli/sandbox-reference.md).

To install the latest Sandbox version, run:

```bash
/bin/bash -c "$(curl -fsSL 'https://sandbox.aztec.network')"
```

This will attempt to run the Sandbox on ` localhost:8080`, so you will have to make sure nothing else is running on that port or change the port defined in `./.aztec/docker-compose.yml`. Running the command again will overwrite any changes made to the `docker-compose.yml`.

Alternatively, you can [run the sandbox as an npm package](../cli/sandbox-reference.md#with-npm).

## Install the CLI

To interact with the Sandbox now that it's running locally, install the [Aztec CLI](https://www.npmjs.com/package/@aztec/cli):

```bash
npm install -g @aztec/cli
```

## Deploy a contract using the CLI

The sandbox is preloaded with multiple accounts. Let's assign them to shell variables. Run the following in your terminal, so we can refer to the accounts as $ALICE and $BOB from now on:

:::note
The default accounts that come with sandbox will likely change over time. Save two of the "Initial accounts" that are printed in the terminal when you started the sandbox.
:::

#include_code declare-accounts yarn-project/end-to-end/src/guides/up_quick_start.sh bash

Start by deploying a token contract. After it is deployed, we check that the deployment succeeded, and export the deployment address to use in future commands. For more detail on how the token contract works, see the [token contract tutorial](../tutorials/writing_token_contract.md).

#include_code deploy yarn-project/end-to-end/src/guides/up_quick_start.sh bash

Note that the deployed contract address is exported, so we can use it as `$CONTRACT` later on.

## Call a contract with the CLI

Alice is set up as the contract admin and token minter in the `_initialize` function. Let's get Alice some private tokens.

We need to export the `SECRET` and `SECRET_HASH` values in order to privately mint tokens. Private tokens are claimable by anyone with the pre-image to a provided hash, see more about how the token contract works in the [token contract tutorial](../tutorials/writing_token_contract.md). After the tokens have been minted, the notes will have to added to the [Private Execution Environment](../../apis/pxe/interfaces/PXE) (PXE) to be consumed by private functions. Once added, Alice can claim them with the `redeem_shield` function. After this, Alice should have 1000 tokens in their private balance.

#include_code mint-private yarn-project/end-to-end/src/guides/up_quick_start.sh bash

We can have Alice privately transfer tokens to Bob. Only Alice and Bob will know what's happened. Here, we use Alice's private key to send a transaction to transfer tokens to Bob. Once they are transferred, we can verify that it worked as expected by checking Alice's and Bob's balances:

#include_code transfer yarn-project/end-to-end/src/guides/up_quick_start.sh bash

Alice and Bob should have 500 tokens.

Congratulations! You are all set up with the Aztec sandbox!

## What's next?

To start writing your first Aztec.nr smart contract, go to the [next page](aztecnr-getting-started.md).

You can also dig more into the sandbox and CLI [here](../cli/main.md).
