---
title: Packages
category: dApp
position: 3
---

Since we have everything ready, we can start creating dApp.

For this purpose, we will use the Nuxt.js framework and NPM packages from Nervos developers.

## Nuxt.js

Nuxt is a very convenient Vue based framework. So let's create the skeleton of our application using the code below.

<code-group>
  <code-block label="Yarn" active>

  ```bash
  yarn create nuxt-app <project-name>
  ```

  </code-block>
  <code-block label="NPM">

  ```bash
  npm init nuxt-app <project-name>
  ```

  </code-block>
  <code-block label="NPX">

  ```bash
  npx create-nuxt-app <project-name>
  ```

  </code-block>
</code-group>

The generator will ask a few questions, the choices are free and after the process is finished a folder with the skeleton will be created.

### Test

After successful completion, we can test if everything is alright with the following command:

<code-group>
  <code-block label="Yarn" active>

  ```bash
  cd <project-name>
  yarn dev
  ```

  </code-block>
  <code-block label="NPM">

  ```bash
  cd <project-name>
  npm run dev
  ```

  </code-block>
</code-group>

## Truffle

Truffle is a framework that allows you to work with contracts. It will be required in our project to compile the contracts before deploying them to the network.

<code-group>
  <code-block label="Yarn" active>

  ```bash
  yarn add truffle
  ```

  </code-block>
  <code-block label="NPM">

  ```bash
  npm install truffle
  ```

  </code-block>
</code-group>

### Initialize

After installing the package, initialization and creation of the appropriate configuration are required.

In the main application directory, create a `truffle-config.js` file with the configuration of your choice. The following is an example file used in the `Fancy Names` example.

**truffle-config.js**

```js
/**
 * Use this file to configure your truffle project. It's seeded with some
 * common settings for different networks and features like migrations,
 * compilation and testing. Uncomment the ones you need or modify
 * them to suit your project as necessary.
 *
 * More information about configuration can be found at:
 *
 * trufflesuite.com/docs/advanced/configuration
 *
 * To deploy via Infura you'll need a wallet provider (like @truffle/hdwallet-provider)
 * to sign your transactions before they're sent to a remote public node. Infura accounts
 * are available for free at: infura.io/register.
 *
 * You'll also need a mnemonic - the twelve word phrase the wallet uses to generate
 * public/private key pairs. If you're publishing your code to GitHub make sure you load this
 * phrase from a file you've .gitignored so it doesn't accidentally become public.
 *
 */

// const HDWalletProvider = require('@truffle/hdwallet-provider');
// const infuraKey = "fj4jll3k.....";
//
// const fs = require('fs');
// const mnemonic = fs.readFileSync(".secret").toString().trim();

module.exports = {
  /**
   * Networks define how you connect to your ethereum client and let you set the
   * defaults web3 uses to send transactions. If you don't specify one truffle
   * will spin up a development blockchain for you on port 9545 when you
   * run `develop` or `test`. You can ask a truffle command to use a specific
   * network from the command line, e.g
   *
   * $ truffle test --network <network-name>
   */

  networks: {
      // Useful for testing. The `development` name is special - truffle uses it by default
      // if it's defined here and no other network is specified at the command line.
      // You should run a client (like ganache-cli, geth or parity) in a separate terminal
      // tab if you use this network and you must also set the `host`, `port` and `network_id`
      // options below to some value.
      //
      // development: {
      //  host: "127.0.0.1",     // Localhost (default: none)
      //  port: 8545,            // Standard Ethereum port (default: none)
      //  network_id: "*",       // Any network (default: none)
      // },
      // Another network with more advanced options...
      // advanced: {
      // port: 8777,             // Custom port
      // network_id: 1342,       // Custom network
      // gas: 8500000,           // Gas sent with each transaction (default: ~6700000)
      // gasPrice: 20000000000,  // 20 gwei (in wei) (default: 100 gwei)
      // from: <address>,        // Account to send txs from (default: accounts[0])
      // websocket: true        // Enable EventEmitter interface for web3 (default: false)
      // },
      // Useful for deploying to a public network.
      // NB: It's important to wrap the provider as a function.
      // ropsten: {
      // provider: () => new HDWalletProvider(mnemonic, `https://ropsten.infura.io/v3/YOUR-PROJECT-ID`),
      // network_id: 3,       // Ropsten's id
      // gas: 5500000,        // Ropsten has a lower block limit than mainnet
      // confirmations: 2,    // # of confs to wait between deployments. (default: 0)
      // timeoutBlocks: 200,  // # of blocks before a deployment times out  (minimum/default: 50)
      // skipDryRun: true     // Skip dry run before migrations? (default: false for public nets )
      // },
      // Useful for private networks
      // private: {
      // provider: () => new HDWalletProvider(mnemonic, `https://network.io`),
      // network_id: 2111,   // This network is yours, in the cloud.
      // production: true    // Treats this network as if it was a public net. (default: false)
      // }
  },

  // Set default mocha options here, use special reporters etc.
  mocha: {
      // timeout: 100000
  },

  // Configure your compilers
  compilers: {
      solc: {
          version: '0.8.3', // Fetch exact version from solc-bin (default: truffle's version)
          docker: true, // Use "0.5.1" you've installed locally with docker (default: false)
          settings: {
              // See the solidity docs for advice about optimization and evmVersion
              optimizer: {
                  enabled: false,
                  runs: 200
              },
              evmVersion: 'istanbul'
          }
      }
  },

  // Truffle DB is currently disabled by default; to enable it, change enabled: false to enabled: true
  //
  // Note: if you migrated your contracts prior to enabling this field in your Truffle project and want
  // those previously migrated contracts available in the .db directory, you will need to run the following:
  // $ truffle migrate --reset --compile-all

  db: {
      enabled: false
  }
};
```

## Web3

Web3 is Ethereum JavaScript API. We will use it to operate on the contract, its reading and writing.

<code-group>
  <code-block label="Yarn" active>

  ```bash
  yarn add web3
  ```

  </code-block>
  <code-block label="NPM">

  ```bash
  npm install web3
  ```

  </code-block>
</code-group>

### Polyjuice Provider

It is a provider from the creators of Nervos that allows you to use Web3 in a 2-layer network.

<code-group>
  <code-block label="Yarn" active>

  ```bash
  yarn add @polyjuice-provider/web3
  ```

  </code-block>
  <code-block label="NPM">

  ```bash
  npm install @polyjuice-provider/web3
  ```

  </code-block>
</code-group>

### Godwoken Integration

We will need this package to convert the Ethereum address to the Polyjuice address used among others for Force Bridge.

<code-group>
  <code-block label="Yarn" active>

  ```bash
  yarn add nervos-godwoken-integration
  ```

  </code-block>
  <code-block label="NPM">

  ```bash
  npm install nervos-godwoken-integration
  ```

  </code-block>
</code-group>