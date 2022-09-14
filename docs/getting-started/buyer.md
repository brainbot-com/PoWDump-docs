---
sidebar_position: 3
---

# Buyer

If you want to participate as a buyer in a swap, you would need some technical knowledge. There is no user interface for
a buyer.

Currently, people interested in buying would need to do this through the scripts provided in
the [powdump repository](https://github.com/brainbot-com/PoWDump).

The scripts are thought as a starting ground for people who want to build their own more sophisticated trading scripts.
For example - you could build a script that compares the price of PoW ETH on PoWDump with the price on other exchanges
and uses this information to make a decision whether to buy or not.

You can of course use the scripts as they are to match a particular swap offer or to match any offer that is
submitted to the smart contract.

## How to run the scripts

:::caution Prerequisites
To run the scripts make sure that you have [nodejs](https://nodejs.org/en/)
, [yarn classic](https://classic.yarnpkg.com/lang/en/docs/install/) installed &
[npx](https://github.com/npm/npx#readme) installed.

Some knowledge of the command line & git (or at lest knowing how to clone a repository) is required.
:::

:::note
All commands below were executed on a Mac. There might be some differences if you execute the commands on a different
operating systems.
:::

To start clone the PoWDump repository:

```bash
git clone git@github.com:brainbot-com/PoWDump.git
```

Then install the dependencies navigate inside the PoWDump directory and run:

```bash
yarn
```

Now you need to compile the contracts. To do so run, navigate inside the `packages/contracts` directory and run:

```bash
npx hardhat compile
```

Once the contracts are compiled navigate inside the `packages/bots` directory and run:

```bash
npx ts-node src/cmd/match.ts --help
```

The above command should output all the possible `commands` that you can use:

```
match.ts <command>

Commands:
  match.ts match                            Match a commitment from the source
  <transactionWithSwapCommitment>           chain to the target chain by finding
  [swapContractAddressOnTargetChain]        a swap commitment in the given
  [commitmentSourceChainRPC]                transaction hash.
  [commitmentTargetChainRPC] [privateKey]
  match.ts matchBySwapId <swapId>           Match a commitment from the source
  [swapContractAddressOnTargetChain]        chain to the target chain by finding
  [commitmentSourceChainRPC]                a swap commitment by the given swap
  [commitmentTargetChainRPC] [privateKey]   id
  match.ts watch                            Watches the source contract for new
  <swapContractAddressOnSourceChain>        commitments and matches them on the
  [swapContractAddressOnTargetChain]        target chain
  [commitmentSourceChainRPC]
  [commitmentTargetChainRPC] [privateKey]

Options:
  --help     Show help                                                 [boolean]
  --version  Show version number                                       [boolean]
```

You can get more information about a specific command by running:

```bash
npx ts-node src/cmd/match.ts <command> --help
```

for example

```bash
npx ts-node src/cmd/match.ts match --help
```

will produce the following output:

```
match.ts match <transactionWithSwapCommitment>
[swapContractAddressOnTargetChain] [commitmentSourceChainRPC]
[commitmentTargetChainRPC] [privateKey]

Match a commitment from the source chain to the target chain by finding a swap
commitment in the given transaction hash.

Options:
  --help                              Show help                        [boolean]
  --version                           Show version number              [boolean]
  --transactionWithSwapCommitment     The commitment hash to match      [string]
  --swapContractAddressOnSourceChain  The swapContractAddressOnSourceChain
                                      address on the source chain
                                                             [string] [required]
  --swapContractAddressOnTargetChain  The swapContractAddressOnTargetChain
                                      address on the target chain
                                                             [string] [required]
  --commitmentSourceChainRPC          The source chain the commitment was made
                                      on       [default: "http://127.0.01:8545"]
  --commitmentTargetChainRPC          The target chain the commitment has to be
                                      deployed on
                        [default: "https://ethereum-goerli-rpc.allthatnode.com"]
  --privateKey                        The private key to use to sign the
                                      transaction. You can use .env variable
                                      POW_DUMP_PRIVATE_KEY to pass this
                                                             [string] [required]
```

Before going ahead and running the commands, you'll notice that every command
requires a private key. You can provide the private key as a command line argument, but that is not recommended
as it will be visible in your shell history. Instead, you can use the `.env` file to pass the private key.

What we like to do is to create a `private-notes` file in the bots directory with the command arguments we want to use:
```bash
export POW_DUMP_privateKey=0x<your-private-key>
export POW_DUMP_swapContractAddressOnSourceChain=<contractAddressOnSourceChain>
export POW_DUMP_swapContractAddressOnTargetChain=<contractAddressOnTargetChain>
export POW_DUMP_commitmentSourceChainRPC="<sourceChainRPC>"
export POW_DUMP_commitmentTargetChainRPC="<targetChainRPC>"
```

Then we can source the file before running the commands:

```bash
source private-notes
```

now our environment variables are set and running any of the script commands will use them.

## Available commands

As already stated above executing `npx ts-node src/cmd/match.ts --help` will output all the currently available commands.

:::caution
Since the interaction with the PoWDump Contracts are asynchronous in nature (you have a counterparty on the other hand that
have to reveal the secret) the scripts don't exit after they match a commitment. Instead, they will wait for the counterparty
to reveal the secret. Once this happens they will try to submit the secret to the smart contract on the source chain and exit.
:::
As of the time of writing this documentation the following commands are available:

### match
```
npx ts-node src/cmd/match.ts match <transactionWithSwapCommitment>
```

This command will match a commitment from the source chain to the target chain by finding a swap commitment in the given
transaction hash. 

When to use this command? 
If you know the transaction hash of a Commit transaction that was made on the source chain. The transaction hash is
output in the user interface when you submit a swap offer.

### matchBySwapId
```
npx ts-node src/cmd/match.ts matchBySwapId <swapId>
```

This command will match a commitment from the source chain to the target chain by finding a swap commitment by the given swap id.

When to use this command?
This command is analogous to the `match` command, but instead of providing the transaction hash, you provide the swap id.
Since swap ids are just numbers, it is way easier to use them. The swap id is output in the user interface when you submit a swap offer.

### watch

:::danger
This command watches the source contract and would try to match any commitment that is made on the source chain without
taking into account the current market price and whether the commitment is profitable or not. This command is only
useful for testing purposes, but if you want to be profitable you should implement your own checks about the market price.
:::
```
npx ts-node src/cmd/match.ts watch
```

This command will watch the source contract for new commitments and will match them on the target chain.

When to use this command?
If you want to match any commitment that is made on the source chain, you can use this command.

