# chinb.js 参考

## Install

This is a Node.js module available through the npm registry.

Before installing, download and install Node.js. 

Installation is done using the npm install command:

```$ npm install chinb```

## Introduction

This is a chinb client for interacting with chinb blockchain.

Here is an example on how to use it:

```javascript
const Chinb = require('chinb');
const chinb = new Chinb;
const account = chinb.account;
account.genKey()
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

The chinb.js object is an umbrella package to house all chinb related modules.

```
const Chinb = require('chinb');

>Chinb.
>
>
```
## Account.new

初始化用户模块


Generate private/public key.

> This is a code annotation. It will appear in the area to the right, next to the code samples.

```javascript
const Account = require('Account');

let key     = Account.genKey(password);
let pubKey  = key.getPubKey();
let privKey = key.getPrivKey(password);
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

### Parameters

Parameter | Default | Description
--------- | ------- | -----------
password | null | Password used to encrypt generated private key before storing on client's device

<aside class="success">
Is `Account` a singleton class design?
</aside>


## Account.getStats

## Account.getPubkey

## Account.getInfo

## Account.getStats

## Account.getBalance

## Account.setVerified

## Account.transfer

## Account.authorize
