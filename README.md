<div align="center">
  <img alt="lisk-hd-key" src="https://raw.githubusercontent.com/alepop/lisk-hd-key/master/logo.svg" height="109px" />
</div>

Lisk HD Key
============
[![Coverage Status](https://coveralls.io/repos/github/alepop/lisk-hd-key/badge.svg?branch=master)](https://coveralls.io/github/alepop/lisk-hd-key?branch=master)
A Typescript based module that provides addition functionality for the Key Derivation in the Lisk ecosystem.

Installation
------------

    npm i --save @lisk-builders/lisk-hd-key


Examples
-----

**getPublicKey(path, seed)** <br>
In this example we will get the public key for a Lisk address. The function should get the hexadecimal value of the given BIP39 seed.

`seed`   A 256 bits hexadecimal string.
> 06bae687f0250ab9533be2ac9717ae2a802d69c97d5..

`path`  Derived from [BIP0044](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) we use the same pattern. Extra information on the what and why of the parameter can be found on the given url.

> m / purpose' / coin_type' / account' / change' / address_index'

**NOTE**: Every Lisk wallet that provides hardware wallet integration are simply using the following derivation:

> m / purpose' / coin_type' / account'

**Usage**

```js
const { getPublicKey } = require('lisk-hd-key');

const bip39 = require('bip39');
const path = "m/44'/134'/0'";

const doPubKeyTest = async () => {
    // sunny settle rent arrive coast emotion twice outdoor erupt scale once reason
    const seed = bip39.generateMnemonic();
    
    // 18e48e187b700d4596983f2efaf64f63c31ff13b4537abea1157bdf45c1fc9e5c5d8a817048616d24dcd0b7ae638df786cec2dc0749f6847724905988ae56b0e
    const hexSeed = await bip39.mnemonicToSeed(seed);
    
    // <Buffer e7 7d a0 2e 45 ec 11 a3 69 70 58 2e ad 68 11 e2 78 79 7a 14 f3 15 a0 a6 9a 3e fe 9f 6c 76 24 b6>
    const publicKey = getPublicKey(path, hexSeed);
}

doPubKeyTest();
```
<br>

**signTransaction( seed, path, transaction )** <br>
In this example we will sign a transaction. Signing a transaction means that nobody can alter the content of the transaction without invalidating your signature.

`seed`  A 256 bits hexadecimal string.
> 06bae687f0250ab9533be2ac9717ae2a802d69c97d5..

`path`  Derived from [BIP0044](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) we use the same pattern. Extra information on the what and why of the parameter can be found on the given url.

> m / purpose' / coin_type' / account' / change' / address_index'

**NOTE**: Every Lisk wallet that provides hardware wallet integration are simply using the following derivation:

> m / purpose' / coin_type' / account'


`transaction`
This parameter is a [Lisk transaction object](https://lisk.io/documentation/the-lisk-protocol/transactions). It can have the same properties as a transaction described in the given url.

**Usage**

```js
const { signTransaction } = require('lisk-hd-key');

const bip39 = require('bip39');
const path = "m/44'/134'/0'";

const unsignedTransaction = {
   "amount": "25",
   "recipientId": "1L",
    "timestamp": 1525977822,
    "type": 0,
    "fee": "20000000",
    "asset": {
       "data": "Custom message to show i have signed the transaction."
    }
};

const doSignTest = async () => {
    // sunny settle rent arrive coast emotion twice outdoor erupt scale once reason
    const seed = bip39.generateMnemonic();

    // 18e48e187b700d4596983f2efaf64f63c31ff13b4537abea1157bdf45c1fc9e5c5d8a817048616d24dcd0b7ae638df786cec2dc0749f6847724905988ae56b0e
    const hexSeed = await bip39.mnemonicToSeed(seed);

    /*
    {
    "amount": "25",
    "recipientId": "1L",
    "timestamp": 1525977822,
    "type": 0,
    "fee": "20000000",
    "asset": {
        "data": "Custom message to show i have signed the transaction."
    },
    "senderPublicKey": "e77da02e45ec11a36970582ead6811e278797a14f315a0a69a3efe9f6c7624b6",
    "signature": "f6fb203848affe7c88f3b68bfefa012b9eb37581a34eb2b5b930d5045ab3575aab6ce9683bf16f85f63453481576ac08efd39f2b194121639b03c8b4aaf3060e",
    "id": "403666780544575699"
    }
    */
    const signedTransaction = signTransaction(hexSeed, path, unsignedTransaction)
}

doSignTest();
```

Tests
-----
```
npm test
```

References
----------

 - Universal private key derivation - [SLIP-0010](https://github.com/satoshilabs/slips/blob/master/slip-0010.md)
 - Hierarchical Deterministic Wallets- [BIP-0032](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)
 - Multi-Account Hierarchy for Deterministic Wallets - [BIP-0044](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki)
