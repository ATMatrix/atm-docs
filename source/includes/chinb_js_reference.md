# chinb.js 参考

## Install

This is a Node.js module available through the npm registry.

Before installing, download and install Node.js. 

Installation is done using the npm install command:

```$ npm install chinb```

## Introduction

chinb.js is a collection of libraries which allow you to interact with a Chinb node, using a HTTP or IPC connection.

Here is an example on how to use it:

```javascript
const Chinb = require('chinb');
let options = {
  institue_private_key: "XXXXXX",
  validator_private_key: "XXXXX"
}
const chinb = new Chinb(options);
chinb.accounts.create()
.then(function(key){
    ...
})
.catch(function(error){
    ...
});
```

<aside class="success">
Node.js 0.8 or higher is required.
</aside>

## chinb 

The chinb.js object is an umbrella package to house all Chinb related modules.

```
const Chinb = require('chinb');
const chinb = new Chinb(options);

> chinb.accounts
> chinb.institue
> chinb.validator
> chinb.utils
> chinb.version
```
## chinb.accounts

The ```chinb.accounts``` contains functions to generate Chinb accounts and authorize data.

## chinb.accounts.create

```chinb.accounts.create(id, password)```

Generate an account object with private key and public key.

```javascript
chinb.accounts.create("111111111111111", 'test')
.then(function(key){
    console.log(key)
})
.catch(function(error){
    console.log(error)
});
```

> returns JSON structured like this:

```json
{
  "id": "987654321000000000",
  "address": "0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa01",
  "keystore": "{
        version: 3,
        id: '04e9bcbb-96fa-497b-94d1-14df4cd20af6',
        address: '2c7536e3605d9c16a7a3d7b1898e529396a65c23',
        crypto: {
            ciphertext: 'a1c25da3ecde4e6a24f3697251dd15d6208520efc84ad97397e906e6df24d251',
            cipherparams: { iv: '2885df2b63f7ef247d753c82fa20038a' },
            cipher: 'aes-128-ctr',
            kdf: 'scrypt',
            kdfparams: {
                dklen: 32,
                salt: '4531b3c174cc3ff32a6a7a85d6761b410db674807b2d216d022318ceee50be10',
                n: 262144,
                r: 8,
                p: 1
            },
            mac: 'b8b010fff37f9ae5559a352a185e86f9b9c1d7f7a9f1bd4e82a5dd35468fc7f6'
        }
    }"
}
```

<aside class="warning">
Be sure to store the private keys on the client only!
</aside>

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | null | user ID
password | null | Password used to encrypt generated private key before storing on client's device

## chinb.accounts.getBalance

```chinb.accounts.getBalance(address [, defaultBlock])```

Get the balance of an address at a given block.

```javascript
chinb.accounts.getBalance("0x407d73d8a49eeb85d32cf465507dd71d507100c1")
.then(console.log)
```

> The current balance for the given address

```
"1000000000000"
```

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
address | null | user public key

## chinb.accounts.transfer

```chinb.accounts.transfer(from, to, tokens)```

Send `tokens` amount of tokens from address `from` to address `to`.

```javascript
chinb.accounts.transfer("0x1111111111111111111111111111111111111111", "0x2222222222222222222222222222222222222222", 100)
.then(console.log)
```

> returns ```receipt``` like this:

```json
{
  "status": true,
  "transactionHash": "0x9fc76417374aa880d4449a1f7f31ec597f00b1f6f3dd2d66f4c9c6c445836d8b",
  "transactionIndex": 0,
  "blockHash": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",
  "blockNumber": 3,
  "contractAddress": "0x11f4d0A3c12e86B4b5F39B213F7E19D048276DAe",
  "cumulativeGasUsed": 314159,
  "gasUsed": 30234,
  "logs": [{
         // logs as returned by getPastLogs, etc.
     }, ...]
}
```

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
from | null | from address
to | null | to address
tokens | null | amount of tokens

## chinb.accounts.authorize

```chinb.accounts.authorize(address, password, msg)```

Signs arbitrary data. This data is before UTF-8 HEX decoded and enveloped as follows: "\x19Chinb Signed Message:\n" + message.length + message.



```javascript
chinb.accounts.authorize("0x407d73d8a49eeb85d32cf465507dd71d507100c1", "test", "Some data")
.then(console.log)
```

> returns ```sign``` like this:

```
{
    message: 'Some data',
    messageHash: '0x1da44b586eb0729ff70a73c326926f6ed5a25f5b056e7f47fbc6e58d86871655',
    v: '0x1c',
    r: '0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd',
    s: '0x6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a029',
    signature: '0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a0291c'
}
```

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
from | null | from address
password | null | user password
msg | null | the given data


## chinb.institue

The ```chinb.institue``` contains functions to validate sign and recover account.

## chinb.institue.validate

```chinb.institue.validate(address, sign, msg)```

Validate the sign of the address

```javascript
chinb.institue.validate("0x407d73d8a49eeb85d32cf465507dd71d507100c1", "0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a0291c")
.then(console.log)
```

> returns ```result``` like this:

```
true
```

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
address | null | the address of sign
sign | null | the signature of authorizing data
msg | null | the data of sign

## chinb.institue.recover

```chinb.institue.recover(address)```

Recover account

```javascript
chinb.institue.recover("0x407d73d8a49eeb85d32cf465507dd71d507100c1")
.then(console.log)
```

> returns ```result``` like this:

```
true
```
