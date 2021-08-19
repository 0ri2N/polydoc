---
title: Preparations
category: 'Getting started'
position: 2
---

## Contract

You must have a contract before you can port/create dApp. You can also create it from scratch and implement it in any form, even for a test!

For example, in this documentation we will use the previously prepared contract to create fancy names, it looks like below.


<alert type="info">

As mentioned in Intruduction, this contract was used in the app for the Nervos hackathon. (see [live app](https://fancy-names.vercel.app)).

</alert>

**FancyNames.sol**

```sol
pragma solidity >=0.8.0;

contract FancyNames {

    struct Claimer {
        bool claimed;
        string name;
    }

    mapping(address => Claimer) public claimers;

    function claim(string memory name) public {
        Claimer storage sender = claimers[msg.sender];

        require(!sender.claimed, "Already claimed.");
        
        sender.name = name;
        sender.claimed = true;
    }
    
    function read() public view returns (string memory) {
        Claimer storage sender = claimers[msg.sender];
        
        return sender.name;
     }
}
```

## Godwoken Account
To use the 2nd layer called Polyjuice, you must have a previously prepared account and funds on it. 

For this purpose, it is worth using the instructions provided by the Nervos developers. 

<alert type="info">

The guides were created for the needs of Hackaton: [Nervos - Broaden the Spectrum](https://gitcoin.co/hackathon/nervos?) and are a great help for novice application developers. 

</alert>


### Create and Fund an Account with CKBytes on Layer 1

In this step, you create a test account on layer 1 and fund it with CKB funds for subsequent activities.

<badge>[The guide is here!](https://github.com/Kuzirashi/gw-gitcoin-instruction/blob/master/src/component-tutorials/1.setup.account.in.ckb.cli.md)</badge>

### Deposit some CKBytes on Layer 2

Then you need to prepare the private key from the 1st layer, create a deposit from the 1st layer to the 2nd layer and prepare the accounts.

<badge>[The guide is here!](https://github.com/Kuzirashi/gw-gitcoin-instruction/blob/master/src/component-tutorials/4.layer2.deposit.md)</badge>
