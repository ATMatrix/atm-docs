# 快速起步

## 基础

* go(>=1.11)
* [chain-core](https://github.com/ATNIO/chain-core)
* [atn-js](https://github.com/ATNIO/atn-js)
* [atn-console](https://github.com/ATNIO/atn-console)

## 节点部署

你可以用`--rpc`启动HTTP JSON-RPC服务,改变默认端口,监听本地地址为：

```bash
atnchain --rpc --rpcaddr <ip> --rpcport <portnumber>
```

## 节点准入

部署`NodeFilter`合约，添加信任节点地址到合约

```
contract NodeFilter {
    
    struct NodeInfo {
        string nodeInfo;
        uint256 blockNo;
        uint256 expiredNo;
    }
    
    address public controller;
    mapping(address=>NodeInfo) public addresses;

    modifier owner() {
        require(msg.sender == controller || controller == address(0x0));
        _;
    }
    
    function changeOwner(address newowner) owner() public {
        controller = newowner;
    }
    
    function addNode(address node, uint256 expired, string memory info) owner() public {
        addresses[node] = NodeInfo({
            nodeInfo: info,
            blockNo: block.number,
            expiredNo: expired
        });
    }
    
    function rmvNode(address node) owner() public {
        delete addresses[node];
    }
}
```

## 示例

