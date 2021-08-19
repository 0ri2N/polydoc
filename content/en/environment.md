---
title: Environment
category: dApp
position: 4
---

For easy variable management and editing, we will create `environment variables`.

## .env

In the main project directory, create a file named `.env` with the following content:

```env
ACCOUNT_PRIVATE_KEY=

SUDT_ID=30
SUDT_NAME=Nervos Ethereum
SUDT_SYMBOL=ckETH
SUDT_TOTAL_SUPPLY=1000000000

ROLLUP_TYPE_HASH=0x4cc2e6526204ae6a2e8fcf12f7ad472f41a1606d5b9624beebd215d780809f6a
GODWOKEN_RPC_URL=https://godwoken-testnet-web3-rpc.ckbapp.dev
ETH_ACCOUNT_LOCK_CODE_HASH=0xdeec13a7b8e100579541384ccaf4b5223733e4a5483c3aec95ddc4c1d5ea5b22

SUDT_PROXY_CONTRACT_ADDRESS=
FANCY_CONTRACT_ADDRESS=
```

### Analysis

Below you will find information on what each variable is responsible for.


<alert type="warning">

**Carefully!** Remember never to share your private key anywhere or add a completed `.env` file with sensitive data to the repositories.

</alert>

#### Contracts

- `ACCOUNT_PRIVATE_KEY`: It is a private key used to deploy previously prepared contracts.
- `SUDT_PROXY_CONTRACT_ADDRESS`: The address of the previously prepared proxy contract. To be completed later after deploying.
- `FANCY_CONTRACT_ADDRESS`: The address of the previously prepared dapp contract. To be completed later after deploying.


#### SUDT

This is information about the token used in dApp. The default is `ckETH`, the `ETH` version of the `CKB` network. There is no need to change this data. Only change them when you want to use a different cryptocurrency.

- `SUDT_ID=30`
- `SUDT_NAME=Nervos Ethereum`
- `SUDT_SYMBOL=ckETH`
- `SUDT_TOTAL_SUPPLY=1000000000`

#### Web 3

This data should not be changed! These are official variables used when working with the Godwoken network and creating test dApps with the participation of CKB.

- `ROLLUP_TYPE_HASH=0x4cc2e6526204ae6a2e8fcf12f7ad472f41a1606d5b9624beebd215d780809f6a`
- `GODWOKEN_RPC_URL=https://godwoken-testnet-web3-rpc.ckbapp.dev`
- `ETH_ACCOUNT_LOCK_CODE_HASH=0xdeec13a7b8e100579541384ccaf4b5223733e4a5483c3aec95ddc4c1d5ea5b22`

