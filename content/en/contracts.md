---
title: Contracts
category: dApp
position: 5
---

Before we start programming the frontend of the application, we need to compile the contracts and deploy them.

## Compile

In the "Preparations" step, we prepared 2 contracts, our main application and a Proxy to work with ETH on the CKB layer. We'll compile them first.

### Creation / Transfer 

- In the app root directory, create (if it doesn't already exist) a folder named `contracts`.
  
  ```bash
  cd <project-name>
  mkdir contracts
  ```

- Move the previously prepared contracts to this folder. The folder with contracts should look like this:
    - `contracts/FancyNames.sol`
    - `contracts/SudtERC20Proxy.sol`

### Compile Script

Modify the `package.json` file so that you can compile contracts with a simple command.

- add the command to the scripts object in this file

```json
"compile": "truffle compile"
```

- after making the change, the beginning of your `package.json` file should look similar to the example below.

```json
{
  "name": "NervosDAPP",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "nuxt",
    "build": "nuxt generate",
    "start": "nuxt start",
    "compile": "truffle compile"
  },

  ...
```

- Now we can compile the contracts. In the root of the project, execute the following code.

<code-group>
  <code-block label="Yarn" active>

  ```bash
  cd <project-name>
  yarn compile
  ```

  </code-block>
  <code-block label="NPM">

  ```bash
  cd <project-name>
  npm compile
  ```

  </code-block>
</code-group>

### Verification

If everything went well in the main directory of the project there should be a directory: `build/contracts` in which there are files:

- `build/contracts/Context.json`
- `build/contracts/ERC20.json`
- `build/contracts/FancyNames.json`
- `build/contracts/IERC20.json`

**If something went wrong, retrace all steps.**

## Deploys

In the "Preparations" step, we prepared 2 contracts, our main application and a Proxy to work with ETH on the CKB layer. We'll compile them first.

### Script

- Create a file named `deploy.js` with the following content:


<alert type="warning">

**Carefully!** You need to make some changes to this file. 

- The first is to replace the string in the code: `<$CONTRACT$>` with the following value:
  - `ERC20.json` – Proxy Contract
  - `FancyNames.json` – dApp Contract

- If you are creating the deploy proxy contract, uncomment the line containing the `arguments` variable.

</alert>

```js
require('dotenv').config();

const Web3 = require('web3');
const { PolyjuiceHttpProvider, PolyjuiceAccounts } = require("@polyjuice-provider/web3");

const CompiledContractArtifact = require(`./build/contracts/<$CONTRACT$>`);

const polyjuiceConfig = {
    rollupTypeHash: process.env.ROLLUP_TYPE_HASH,
    ethAccountLockCodeHash: process.env.ETH_ACCOUNT_LOCK_CODE_HASH,
    web3Url: process.env.GODWOKEN_RPC_URL
};
  
const provider = new PolyjuiceHttpProvider(
    process.env.GODWOKEN_RPC_URL,
    polyjuiceConfig,
);

const web3 = new Web3(provider);

web3.eth.accounts = new PolyjuiceAccounts(polyjuiceConfig);
const account = web3.eth.accounts.wallet.add(process.env.ACCOUNT_PRIVATE_KEY);
web3.eth.Contract.setProvider(provider, web3.eth.accounts);

(async () => {
    console.log(`Using Ethereum address: ${account.address}`);

    const balance = BigInt(await web3.eth.getBalance(account.address));

    if (balance === 0n) {
        console.log(`Insufficient balance. Can't deploy contract. Please deposit funds to your Ethereum address: ${account.address}`);
        return;
    }

    console.log(`Deploying contract...`);

    const deployTx = new web3.eth.Contract(CompiledContractArtifact.abi).deploy({
        data: getBytecodeFromArtifact(CompiledContractArtifact),
        //arguments: [process.env.SUDT_NAME, process.env.SUDT_SYMBOL, process.env.SUDT_TOTAL_SUPPLY, process.env.SUDT_ID]
    }).send({
        from: account.address,
        to: '0x' + new Array(40).fill(0).join(''),
        gas: 6000000,
        gasPrice: '0',
    });

    deployTx.on('transactionHash', hash => console.log(`Transaction hash: ${hash}`));

    const receipt = await deployTx;

    console.log(`Deployed <$CONTRACT$>. Contract address: ${receipt.contractAddress}`);
})();

function getBytecodeFromArtifact(contractArtifact) {
    return contractArtifact.bytecode || contractArtifact.data?.bytecode?.object
}
```

> The code for this file based from the source: https://github.com/Kuzirashi/gw-gitcoin-instruction/blob/master/src/examples/5-erc20-proxy/index.js

### Run deploy.js

We have prepared the code and contracts, so we can deploy them to the network. Run the command below:

```sh
node deploy
```

<alert type="warning">

You must execute this command 2 times. Appropriately modifying the `.js` file from above to the specific contract.

</alert>

If you have funds on your private key and everything went well, you'll see the message like below:

```sh
[ririen@oad NervosDAPP]$ node deploy
Using Ethereum address: 0x5c5D5351aFAf7CdF5c2E7A826846e594B755d05b
Deploying contract...
Transaction hash: 0xf7ae1bb6df9e7abe3fed360df7235c02bed17d43a4041ffdc980d4ee8a4da859
Deployed ERC20.json. Contract address: 0xcda6d0D3C61a1d67CC502ffdbC20ad6Ec4cdE3B1


[ririen@oad NervosDAPP]$ node deploy
Using Ethereum address: 0x5c5D5351aFAf7CdF5c2E7A826846e594B755d05b
Deploying contract...
Transaction hash: 0xa13e4a5a6ec5d6a0cd03416d5c73956f5152cc6ff4d3e63366330c4129eb01de
Deployed FancyNames.json. Contract address: 0xFB5E7347bA0da2d7471E41FAE2308f3eeDE91288
```

### Update .env

The contracts are now deployed, so the `.env` created earlier should be updated.

Complete the variables in the file: `SUDT_PROXY_CONTRACT_ADDRESS` and `FANCY_CONTRACT_ADDRESS`.