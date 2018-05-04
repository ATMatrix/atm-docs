# chinb.js 参考

## 安装

该 Node.js 模块在 npm 上注册.

在安装之前，请确保下载并安装 Node.js。

安装使用以下 npm install 命令完成：

```$ npm install chinb```

## 介绍

chinb.js 是一个库的集合，它使用 HTTP 或者 IPC 连接与Chinb节点交互，

使用示例:

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
Node.js的版本确保在8.x以上
</aside>

## chinb 

chinb.js 对象是一个容纳所有 Chinb 相关模块的伞包。

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

```chinb.accounts``` 包含生成 Chinb 帐号和授权数据的功能。

## chinb.accounts.create

```chinb.accounts.create(id, password)```

生成带有私钥和公钥的账户对象。

```javascript
chinb.accounts.create("111111111111111", 'test')
.then(function(key){
    console.log(key)
})
.catch(function(error){
    console.log(error)
});
```

> 返回的JSON数据结构如下:

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
请确保私钥仅保存在客户端！
</aside>

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
id | null | 用户ID信息
password | null | 在客户端保存私钥之前，用作加密生成的私钥的密码

## chinb.accounts.getBalance

```chinb.accounts.getBalance(address [, 默认值Block])```

在指定块上查询给定账户地址的余额。

```javascript
chinb.accounts.getBalance("0x407d73d8a49eeb85d32cf465507dd71d507100c1")
.then(console.log)
```

> 返回 ```balance``` 如下

```
"1000000000000"
```

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
address | null | 用户地址信息
默认值Block | "latest" | 如果设置该参数会覆盖掉默认指定块

## chinb.accounts.transfer

```chinb.accounts.transfer(from, to, tokens)```

从 `from` 账户地址发送 `tokens` 数量的代币到 `to` 账户地址。

```javascript
chinb.accounts.transfer("0x1111111111111111111111111111111111111111", "0x2222222222222222222222222222222222222222", 100)
.then(console.log)
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
from | null | from address
to | null | to address
tokens | null | amount of tokens

## chinb.accounts.authorize

```chinb.accounts.authorize(address, password, msg)```

授权给定的数据信息。

```javascript
chinb.accounts.authorize("0x407d73d8a49eeb85d32cf465507dd71d507100c1", "test", "Some data")
.then(console.log)
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
from | null | 授权的账户地址
password | null | 账户的密码
msg | null | 指定授权的信息


## chinb.institue

```chinb.institue``` 包含了验证授权信息和恢复用户账户的功能。

## chinb.institue.validate

```chinb.institue.validate(address, sign, msg)```

验证授权的信息

```javascript
chinb.institue.validate("0x407d73d8a49eeb85d32cf465507dd71d507100c1", "0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a0291c")
.then(console.log)
```

> 返回 ```result``` 如下:

```
true
```

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
address | null | 授权的账户地址
sign | null | 授权的签名
msg | null | 授权的信息

## chinb.institue.recover

    ```chinb.institue.recover(address)```

恢复账户

```javascript
chinb.institue.recover("0x407d73d8a49eeb85d32cf465507dd71d507100c1")
.then(console.log)
```

> 返回 ```result``` 如下:

```
true
```

### 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
address | null | 恢复账户的地址