# 系统设计

## 代币设计

## 流程图

## KYC

## 智能合约

## token合约

机构和用户使用的代币合约

```
Contract ChinbToken is DSToken, ERC223, Controlled {
  function getBalance(address) {}

  function transfer(address des, uint value) {}

  function transferFrom(address src, address des, uint value) auth {}

  function mint() auth {}

  function burn() auth {}
}
```

## 映射合约

维护用户机构的对应关系的合约

```
Contract Register {
  mapping(pubkey => mapping(idno => insid)) user

  function insert() auth {}

  function delete() auth {}

  function update() auth {}
  
}
```
## 机构合约

维护所有机构信息的合约

```
Contract Institution {
  Struct Ins {
    uint insId;
    bytes32 insInfo;
  }
  mapping(insKey => Ins)
  mapping(data => insId)

  function verify(bytes32 hash_data, bytes32 sig, uint insKey ...){}
}
```

## 多签合约

多重签名合约

```
Contract MultiSigConfirm {
  address[] public owners;
  struct Transaction{
    address src,
    address des,
    bytes data,
    bool executed
  }
  function recover isConfirmed(uint transactionId){
  }
  function confirmTransaction ownerExists transactionExists notConfirmed(uint transactionId){}
  function isConfirmed(uint transactionId){}
}
```

## 场景描述：
1、Register an account with KYC 

机构调用 Register insert() 新增用户机构对。

2、Recover account

初始化 MultiSigConfirm 合约，将所有有权限的机构写入owners， 不同机构调用 MultiSigConfirm confirmTransaction()，当满足2/3的机构确认时调用 recover() 进行地址转账。

3、Authorize data

调用 Institution verify()，将验证过的数据记录到链上。

4、Transfer

ChinbToken transfer()

5、Display Balance

ChinbToken getBalance()