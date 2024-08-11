# Historeum

Historeum, previously known as Caerus, is a Layer-2 solution built on Ethereum. It introduces a time-aware Ethereum Virtual Machine (EVM) that allows smart contracts to efficiently access historical on-chain data.

## Overview

Historeum enhances the Ethereum blockchain by enabling direct access to past data, eliminating the need for centralized oracle services. Built on the Optimism OP Stack, Historeum is optimized for scalability and efficiency, offering a powerful solution for developers needing historical data in their smart contracts.

## Key Features

- **Archive-powered EVM**: Historeum integrates Archive Storage directly with the EVM, allowing seamless access to historical data on-chain.
- **Solidity Extension**: The modified Solidity compiler introduces the `caerus` keyword, enabling developers to query historical data within smart contracts.
- **Versatile Applicability**: Historeum is ideal for various blockchain ecosystems, particularly in reputation systems and financial services where historical data is essential.

---

## Building Historeum

Historeum is based on the OP Stack, and building it requires Go (version 1.19 or later) and a C compiler. For detailed build instructions, please refer to the [OP Stack Installation Guide](https://geth.ethereum.org/docs/getting-started/installing-geth).

To build the Historeum client (`historeum`), use the following command:

```shell
make geth
```

Alternatively, to build the full suite of utilities:

```shell
make all
```

## Running Historeum

### Full Node on the Main Ethereum Network

To start a full node on the main Ethereum network:

```shell
geth console
```

This command will start Historeum in snap sync mode, downloading more data to avoid processing the entire Ethereum history.

### Accessing Historical Data

Historeum extends the Ethereum Virtual Machine by allowing direct access to historical data. Developers can retrieve past blockchain data without relying on centralized oracles.

To access historical data, use the precompiled contract 19 (0x13) with the account address, slot number, and past block number:

```solidity
bytes memory args = abi.encodePacked(
  address(this),        // account
  slotNumber,           // slot number
  blockNumber           // block number
);

(
  bool success,         // status
  bytes memory result   // result (bytes)
) = address(0x13).staticcall(args);
```

Alternatively, use the `caerus` keyword with our customized Solidity compiler:

```solidity
bytes32 result = caerus(
  address(this),        // account
  slotNumber,           // slot number
  blockNumber           // block number
);
```

## Docker Quick Start

To get Historeum up and running quickly using Docker:

```shell
docker run -d --name ethereum-node -v /Users/alice/ethereum:/root \
           -p 8545:8545 -p 30303:30303 \
           ethereum/client-go
```

This starts Historeum in snap-sync mode with a DB memory allowance of 1GB. For RPC access from other containers or hosts, include `--http.addr 0.0.0.0`.
