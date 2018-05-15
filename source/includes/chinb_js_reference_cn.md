# chinb.js 参考

## 安装

该 [Node.js](https://nodejs.org/en/) 模块在 [npm registry](https://www.npmjs.com/) 上注册.

在安装之前，请确保[下载并安装 Node.js](https://nodejs.org/en/download/)。

安装使用以下 npm install 命令完成：

```$ npm install chinb```

## 介绍

chinb.js 是一个库的集合，它使用 HTTP 或者 IPC 连接与Chinb节点交互，

使用示例:

```javascript
const chinb = require('chinb');
const account = new chinb.Account();
let id = '123';
let pw = 'test'
let key = account.create(id, pw);
```

<aside class="success">
Node.js的版本确保在8.x以上
</aside>

## chinb 

chinb.js 对象是一个容纳所有 Chinb 相关模块的伞包。

```
const chinb = require('chinb');

> chinb.Account
> chinb.Institue
> chinb.Validator
> chinb.version
```
## chinb.Account

```chinb.Account``` 包含生成 Chinb 帐号和授权数据的功能。


```javascript
const Account = require('chinb').Account;
const account = new Accout();
```

<aside class="warning">
Account 模块仅可在浏览器端使用！
</aside>

## Account.create

```Account.create(id, pw)```

生成带有私钥和公钥的账户对象。

```javascript
let key = account.create("111111111111111", 'test');
console.log(key);
```

> 返回的```keystore```数据结构如下:

```
{
    version: 3,
    id: '04e9bcbb-96fa-497b-94d1-14df4cd20af6',             //keystore id
    address: '2c7536e3605d9c16a7a3d7b1898e529396a65c23',            // 用户公钥地址
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
}
```

<aside class="warning">
请确保私钥仅保存在客户端！
</aside>

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
id | null | 用户身份验证信息
pw | null | 在客户端保存私钥之前，用作加密生成的私钥的密码

## Account.getInfo

```Account.getInfo(addr,id)```

查询用户信息

```javascript
let info = account.getInfo("0x407d73d8a49eeb85d32cf465507dd71d507100c1",'10001', function(err, info){
    if (!err) {
        console.log(info);
    }
});
```

> 返回 ```info``` 如下

```json
{
    "id": "0xef95f2f1ed3ca60b048b4bf67cde2195961e0bba6f70bcbea9a2c4e133e34b46",     //id hash值
    "address": "0x407d73d8a49eeb85d32cf465507dd71d507100c1",            //用户地址
    "insNo": "CHINB1"       //机构编号
}
```


参数 | 默认值 | 描述
--------- | ------- | -----------
addr | null | 用户账户地址
id | null | 用户身份信息

## Account.getTokens

```Account.getTokens(id ,callback)```

在指定块上查询用户的所有账户余额。

```javascript
account.getTokens("111111111111111", function(err, tokens){
    if(!err){
        console.log(tokens)
    }
});
```

> 返回 ```tokens``` 如下

```json
{
    "0x407d73d8a49eeb85d32cf465507dd71d507100c1": "50",
    "0x407d73d8a49eeb85d32cf465507dd71d507100c2": "50"
}
```

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
id | null | 用户省份信息

## Account.transfer

```Account.transfer(id, pw, to, tokens, callback)```

用户发送 `tokens` 数量的代币到 `to` 账户地址。

```javascript
account.transfer("111111111111111", "test", "0x2222222222222222222222222222222222222222", 100, function(err, res){
    if(!err){
        console.log(res);
    }
});
```

> 返回 ```receipt``` 如下:

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

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
id | null | 转账用户的身份信息
pw | null | 转账用户的密码
to | null | token接收者帐号地址
tokens | null | 需要转账token的数量
callback | null | 回调函数

## Account.authorize

```chinb.accounts.authorize(id, pw, data)```

授权给定的数据信息。

```javascript
let sign = account.authorize("111111111111111", "test", "Some data");
console.log(sign);
```

> 返回 ```sign``` 如下:

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

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
id | null | 用户的身份信息
pw | null | 账户的密码
data | null | 指定授权的信息


## chinb.institud

```chinb.Institue``` 包含了验证授权信息和恢复用户账户的功能。


```javascript
const institud = require('chinb').institud;
const priveteKey = "XXXXXXX";   //机构账户密钥
const institud = new institud(privateKey);
```

## institud.getInfo

```institud.getInfo()```

查询机构信息

```javascript
const info = institud.getInfo();
console.log(info);
```

> 返回 ```info``` 如下:

```json
{
    "addr": "0x407d73d8a49eeb85d32cf465507dd71d507100c4", //机构帐号地址
    "insNo": "CHINB1" //机构编号
}
```

## institud.getTokens

```institud.getTokens(addr)```

查询给定地址的token余额

```javascript
const tokens = institud.getTokens("0x407d73d8a49eeb85d32cf465507dd71d507100c4");
console.log(tokens);
```
> 返回 ```info``` 如下:

```
"1000000"
```

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
addr | null | 需要查询余额的地址

## institud.transfer

```institud.transfer(to, tokens, callback)```

机构发送 `tokens` 数量的代币到 `to` 账户地址。

```javascript
institud.transfer("0x2222222222222222222222222222222222222222", 100, function(err, res){
    if(!err){
        console.log(res);
    }
});
```

> 返回 ```receipt``` 如下:

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

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
to | null | token接收者帐号地址
tokens | null | 需要转账token的数量
callback | null | 回调函数

## institud.register

```institud.register(addr, id, callback)```

机构在链上注册经过KYC的用户。

```javascript
institud.register("0x2222222222222222222222222222222222222222", '111111', function(err, res){
    if(!err){
        console.log(res);
    }
});
```

> 返回 ```receipt``` 如下:

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

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
addr | null | 用户账户地址
id | null | 用户身份信息
callback | null | 回调函数

## institud.del

```institud.del(addr, callback)```

机构在链上删除用户信息。

```javascript
institud.del("0x2222222222222222222222222222222222222222", function(err, res){
    if(!err){
        console.log(res);
    }
});
```

> 返回 ```receipt``` 如下:

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

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
addr | null | 用户账户地址
callback | null | 回调函数

## institud.update

```institud.update(addr_old, addr_new, callback)```

机构在链上更新用户信息。

```javascript
institud.register("0x2222222222222222222222222222222222222222", '0x1111111111111111111111111111111111', function(err, res){
    if(!err){
        console.log(res);
    }
});
```

> 返回 ```receipt``` 如下:

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

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
addr_old | null | 用户更新前账户地址
addr_new | null | 用户更新后账户地址
callback | null | 回调函数

## institud.recover

```institud.recover(addr_old, addr_new, callback)```

机构恢复用户信息。

<aside class="warning">
帐号恢复属于敏感操作，请确认KYC后操作！
</aside>

```javascript
institud.recover("0x2222222222222222222222222222222222222222", '0x1111111111111111111111111111111111', function(err, res){
    if(!err){
        console.log(res);
    }
});
```

> 返回 ```receipt``` 如下:

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

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
addr_old | null | 用户恢复前账户地址
addr_new | null | 用户恢复后账户地址
callback | null | 回调函数

## institud.verify

```institue.verify(addr, sign, callback)```

验证授权的信息

```javascript
institue.verify("0x407d73d8a49eeb85d32cf465507dd71d507100c1", {
    message: 'Some data',
    messageHash: '0x1da44b586eb0729ff70a73c326926f6ed5a25f5b056e7f47fbc6e58d86871655',
    v: '0x1c',
    r: '0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd',
    s: '0x6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a029',
    signature: '0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a0291c'
}, function(err, res){
    if(!err){
        console.log(res);
    }
})
```

> 返回 ```receipt``` 如下:

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
### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
address | null | 授权的账户地址
sign | null | 授权的签名对象
