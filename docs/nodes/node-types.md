# TON Node Types

**Blockchain node** is a device, usually a computer, that runs the TON blockchain's software - and therefore, participates in the blockchain operation.
In general, nodes ensure decentralization of the TON network.

Nodes perform different functions within the TON protocol:

- **Full** and **archive nodes** maintain the blockchain block and transaction history, enable users and client applications to look for blocks and transactions, as well as to send new transactions into the blockchain;
- **Validator nodes** verify transactions ensuring the blockchain security.

Below, you will find more detailed information about each of these node types, as well as about interaction of full and archive nodes with client applications.

## Full Node

**Full node** is a basic node type within the TON blockchain.
They serve as a backbone of the TON blockchain by keeping its block history - in other words, its _current state_.

In comparison with **archive nodes**, full nodes keep only the latest part of the blockchain state vital for ensuring the network stability and operation of client applications.
Full nodes _prune_ the state of the TON blockchain kept by them - that means, earlier blocks that become unnecessary for the network, are automatically removed by the full node to effectively manage its data volume.

To allow client applications to look for blocks and transactions, as well as to send new transactions into the TON blockchain, full nodes are equipped with the liteserver functionality: see [Interacting with TON nodes](#interacting-with-ton-nodes) below.

## Archive Node

**Archive node** is a full node that keeps the entire block history of the TON blockchain.
Such nodes act as the decentralized point of truth in terms of ensuring consistency of the whole blockchain history.
They serve as a backend for blockchain explorers and other applications relying on deep transaction history.

Archive nodes do not prune the blockchain state which elevates their system requirements, especially in terms of storage.
According to the latest estimations, while full nodes and validator nodes require about 1 TB disk space, archive nodes would require about 8 TB to store the complete block history.

## Validator Node

**Validator nodes** or **validators** are TON network participants that propose new blocks and verify transactions in them according to the TON's _Proof-of-Stake_ mechanism.
In this way, validators contribute to the overall blockchain security.

For successful participation in the validation process, validators get [rewards in TON](/nodes/validator/staking-incentives.md).

To be entitled to propose and validate blocks, validators are elected by other participants according to amount of TON being held with them - in other words, their _stake_: the more TON is staked with a validator, the more is its chance to be elected, validate blocks for the network and acquire rewards.
As a rule, validator operators motivate other TON holders to stake with them to get passive income from resulting rewards.
In this way, validators ensure network stability, security and contribute to its growth.

## Interacting with TON nodes

TON nodes are equipped with a _Liteserver_ functionality allowing external applications (in other words, _lite clients_) to interact with the TON blockchain via them.
As a rule, the liteserver mode is used with full and archive nodes while validator nodes do not enable it to increase validation performance.

The liteserver mode allows lite clients to send transactions via TON nodes, as well as to retrieve information about blocks and transactions with them - for instance, to fetch and update wallet balances.

You have two options to allow your lite client application to interact with the TON blockchain:

1. To have a stable connection, you can run your own full or archive node with a Liteserver mode enabled in your node configuration file.
2. In case you have no opportunity to set up your own TON node with a Liteserver, you can use the mesh of public Liteservers provided by the TON Core. For this purpose, use following configuraiton files:
    - [Public Liteserver Configurations - mainnet](https://ton.org/global-config.json)
    - [Public Liteserver Configurations - testnet](https://ton.org/testnet-global.config.json)

> :warning: **Usage of public Liteservers in production:**
> 
> Because of a permanent high load on public Liteservers, the majority of them is rate limited, that is why it is not recomended to use them in production.
This may drive to instability of your your lite client application.
