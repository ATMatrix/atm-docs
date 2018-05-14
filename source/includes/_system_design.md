# 系统设计

## 代币设计

## 流程图

## KYC

## 智能合约

## token合约

机构和用户使用的代币合约

```
contract ChinbToken is DSToken, ERC223 {
  function balanceOf(address) {}

  function transfer(address des, uint value) {}

  function transferFrom(address src, address des, uint value) auth {}

  function mint() auth {}

  function burn() auth {}

}
```

## 机构合约

维护所有机构信息的合约

```
contract Institution is DSAuth{

  mapping(bytes32 => address) public institution;
  mapping(address => mapping(bytes32 => bytes32)) public user;
  //get all users' address by insId
  mapping(bytes32 => address[]) public insUsers;

  function addInstitution(
        address _insAddress, 
        bytes32 _insId
    ){}
  
  function updateInstitution(
        address _newInsAddress, 
        bytes32 _insId
    )

  function removeInstitution(
        address _insAddress, 
        bytes32 _insId
    )

  function addUser(
        address _user, 
        bytes32 _idno, 
        bytes32 _insId){}

  function deleteUser(
        address _user,
        bytes32 _idno,
        bytes32 _insId){}

  function getInsUsersLength(bytes32 _insId)
        public 
        view
        returns(uint){}

}
```

## 多签合约

多重签名合约

```
contract MultiSigConfirm is DSAuth{
  struct Transaction {
        address src;
        address des;
        bool executed;
    }

  mapping (uint => Transaction) public transactions;
  mapping (uint => mapping (address => bool)) public confirmations;
  mapping (uint => bool) public executed;
  address[] public institutions;

  function setTokenAddress(address _chinbAddress) 
        public 
        auth 
        returns (bool){}

  function getTokenAddress() 
        public 
        returns (address) {}

  function setInstitution(address[] _institutions) 
        public 
        auth {}
  
  function addInstitution(
        address _insAddress
    )
        public
        auth {}


  function removeInstitution(
        address _insAddress
    )
        public
        auth{}

  function submitTransaction(
        address src, 
        address des)
        public
        auth
        returns (uint transactionId){}

  function confirmTransaction(uint transactionId){}


  function recover isConfirmed(uint transactionId){
  }

  function executeTransaction(uint transactionId)
        public{}

  function recoverAccount(
        address _src,
        address _des,
        uint _value
    )
        public
        auth
        notNull(_des)
        notNull(chinbToken)
        returns (bool){}
}
```

## Authority合约
```
contract Authority is DSAuth, DSAuthority {
    mapping (address => mapping (address => mapping (bytes4 => bool))) public whiteList;

    function permit(address src, address dst, bytes4 sig)
        public
        auth
        returns (bool){}

    
    function forbid(address src, address dst, bytes4 sig)
        public
        auth
        returns (bool){}

    function canCall(address src, address dst, bytes4 sig)
        public
        view
        returns (bool){}
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