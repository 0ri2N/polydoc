---
title: Configuration
category: dApp
position: 6
---

In this part of the tutorial, we will configure the Nuxt.js framework to meet our needs.

## Environment Variables

We have previously created the appropriate variables and want to use them easily in our application. So we will configure to get them from a file.

Modify the `nuxt.config.js` file by adding a new object called `publicRuntimeConfig` to it and add environment variables to it that will be used in the application.

An example implementation is provided below.


<alert type="info">

You can read more about this feature here: https://nuxtjs.org/docs/2.x/configuration-glossary/configuration-runtime-config

</alert>

`nuxt.config.js`

```js
export default {
  ...

  publicRuntimeConfig: {
    rollupTypeHash: process.env.ROLLUP_TYPE_HASH,
    web3ProviderUrl: process.env.GODWOKEN_RPC_URL,
    ethAccountLockCodeHash: process.env.ETH_ACCOUNT_LOCK_CODE_HASH,
    sudtProxyContractAddress: process.env.SUDT_PROXY_CONTRACT_ADDRESS,
    fancyContractAddress: process.env.FANCY_CONTRACT_ADDRESS,
  },

  ...
}
```

**After configuring these variables, we will be able to get to them from the context level using: `$config.variable`, ex: `$config.fancyContractAddress`**

## Polyjuice Provider

In the dApp, we want to use Web3 with support for Polyjuice. So you need to create an appropriate plugin that will make it possible.

- create a `plugins` directory if it does not exist.
- create a file in the `plugins` directory called `web3.js`

`plugins/web3.js`

```js
import Web3 from 'web3';
import { PolyjuiceHttpProvider } from '@polyjuice-provider/web3';

export default function ({ $config }, inject) {

    const godwokenRpcUrl = $config.web3ProviderUrl;

    const providerConfig = {
        web3Url: godwokenRpcUrl,
        rollupTypeHash: $config.rollupTypeHash,
        ethAccountLockCodeHash: $config.ethAccountLockCodeHash,
    };

    const provider = new PolyjuiceHttpProvider(godwokenRpcUrl, providerConfig);
    
    const web3 = new Web3(provider);

    inject("web3", web3)
}
```
### Code analysis

- at the very beginning, we import the `Web3` and `Polyjuice` libraries. They were previously installed in step: `dApp/Packages`.
- then we export the function that creates the plugin in the `Nuxt` framework.
- in the middle of the function, we get the environment variables that we previously set with `$config`.
- then we modify the default `Web3` library to support `Polyjuice`.
- we do an injection so that `Web3` can be used from all code in the framework in an easy way.

**Now we can use the `Web3` code in the application with a variable: `$web3`**

## Detect Provider

To use dApp, Web3 support in the browser is required. This support is provided by plugins such as `metamask`.

Our goal is to display relevant information or create a specific action if the Web3 plugin in the browser is not detected.

- create a file in the `plugins` directory called `detect.client.js`

<alert type="info">

The end of the file `.client.js` means that the plugin code will be executed only in the browser and not on the server side [[info]](https://nuxtjs.org/docs/2.x/directory-structure/plugins).

</alert>

`detect.client.js`

```js
import detectEthereumProvider from '@metamask/detect-provider'
 
export default function () {
    const provider = await detectEthereumProvider()
    
    if (provider) {
        console.log('Web3 successfully detected!')
    } else {
        alert('Please install MetaMask!')
    }
}
```

## Integration

Okey. We already have the appropriate Nuxt plugins. It's time to implement them and start using them.

Modify the `nuxt.config.js` file by adding/modify a object called `plugins`.

An example implementation is provided below.

`nuxt.config.js`

```js
export default {
    ...

    plugins: [
        '~/plugins/web3.js'
        '~/plugins/detect.client.js'
    ],

    ...
}
```