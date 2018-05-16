# 机构侧 API

## info

获取机构信息。

```shell
curl "http://localhost:3000/api/info"
```

> 返回的JSON数据结构如下:

```json
{
    "id": "CHINB1",     //机构编号
    "addr": "0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa01"     //机构帐号地址
}
```

### HTTP 请求

```GET http://localhost:3000/api/info```

## tokens

查询账户余额

```shell
curl "http://localhost:3000/api/tokens/0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa02"
```

> 返回的JSON数据结构如下:

```json
{
    "tokens": "10000000000000000000"  //机构账户余额
}
```

### HTTP 请求

```GET http://localhost:3000/api/tokens/<addr>```

### URL 参数表

参数 | 默认值 | 描述
--------- | ------- | -----------
addr | null | 需要查询的帐号地址

## transfer

机构账户转账

```shell
curl "http://localhost:3000/api/transfer"
  -X POST
  -H "Content-type: application/json"
  -d "{\"to\": \"0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa02\", \"amount\": \"50\"}"
```

> 返回的```receipt```数据结构如下:

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

### HTTP 请求

```POST http://localhost:3000/api/transfer {to: 0x01, amount: 50}```

### 请求体参数

参数 | 默认值 | 描述
--------- | ------- | -----------
to | null | token接收者帐号地址
amount | null | 需要转账token的数量

## user_register

为KYC之后的用户在链上注册

```shell
curl "http://localhost:3000/api/user"
  -X POST
  -H "Content-type: application/json"
  -d "{\"addr\": \"0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa03\", \"id\": \"0000000000000000000001\"}"
```

> 返回的```receipt```数据结构如下:

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

### HTTP 请求

```POST http://localhost:3000/api/user {addr: 0x02, id: 02}```

### 请求体参数

参数 | 默认值 | 描述
--------- | ------- | -----------
addr | null | 需要注册的用户帐号地址
id | null | 经过KYC的用户身份信息

## user_delete

在链上删除已经注册的用户信息

```shell
curl "http://localhost:3000/api/user"
  -X DELETE
  -H "Content-type: application/json"
  -d "{\"addr\": \"0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa03\"}"
```

> 返回的```receipt```数据结构如下:

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

### HTTP 请求

```DELETE http://localhost:3000/api/user {addr: 0x02}```

### 请求体参数

参数 | 默认值 | 描述
--------- | ------- | -----------
addr | null | 需要删除的用户帐号地址

## recover

为经过KYC的用户恢复账户

```shell
curl "http://localhost:3000/api/recover"
  -X POST
  -H "Content-type: application/json"
  -d "{\"addr_old\": \"0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa03\", \"addr_new\": \"0xb8CE9ab6943e0eCED004cDe8e3bBed6568B2Fa04\"}"
```

> 返回的```receipt```数据结构如下:

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

### HTTP 请求

```POST http://localhost:3000/api/user {addr_old: 0x02, addr_new: 0x03}```

### 请求体参数

参数 | 默认值 | 描述
--------- | ------- | -----------
addr_old | null | 用户恢复前的账户地址
addr_new | null | 用户恢复后的账户地址

## verify

用户授权的信息在链上校验并永久保存

```shell
curl "http://localhost:3000/api/verify"
  -X POST
  -H "Content-type: application/json"
  -d "{\"addr\":\"0x2222222222222222222222222222222222222222\",\"id\":\"11111\", \"message\": \"Some data\", \"messageHash\": \"0x1da44b586eb0729ff70a73c326926f6ed5a25f5b056e7f47fbc6e58d86871655\", \"v\": \"0x1c\", \"r\": \"0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd\", \"s\": \"0x6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a029\", \"signature\": \"Some 0xb91467e570a6466aa9e9876cbcd013baba02900b8979d43fe208a4a4f339f5fd6007e74cd82e037b800186422fc2da167c747ef045e5d18a5f5d4300f8e1a0291c\"}"
```

> 返回的```receipt```数据结构如下:

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

### HTTP 请求

```POST http://localhost:3000/api/verify {signObject}```

### 请求体参数

参数 | 默认值 | 描述
--------- | ------- | -----------
signObject | null | 用户授权签名的签名对象

## 错误

机构侧 API 应用以下错误编码:

Error Code | Meaning
---------- | -------
400 | Bad Request 
401 | Unauthorized 
403 | Forbidden 
404 | Not Found
405 | Method Not Allowed 
406 | Not Acceptable
410 | Gone 
418 | I'm a teapot.
429 | Too Many Requests
500 | Internal Server Error 
503 | Service Unavailable 
