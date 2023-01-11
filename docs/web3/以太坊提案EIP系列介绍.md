# 以太坊提案EIP系列介绍

#### [EIP-1193规范](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1193.md)
定义了客户端provider的接口，如MetaMask 嵌入的window.ethereum

#### [EIP-1474规范](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1474.md)
定义了JSON-RPC请求的API，即客户端provider 和 以太坊节点应该支持的API列表，可以参考[MetaMask 更友好的文档](https://docs.metamask.io/guide/rpc-api.html)

#### [EIP-712规范](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md)
定义了签名规范，其中 [签名的方法的参数格式](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-712.md#parameters)  

结构化签名JSON示例及说明：（4个主字段）
- types —— Record<string, Array<any>>，定义了domain字段和message中使用的数据结构
其中，固定的key是"EIP712Domain"，用于定义domain数据结构
  - name用来定义属性名称，type用来定义属性类型，属性类型包括：原子类型、动态类型、引用类型
  - 原子类型：bytes1 - bytes32、uint8 - uint256、int8 - int256、bool
  - 动态类型：bytes、string。类似于原子类型，但在编码中处理方式不同
  - 引用类型：数组（如：定长数组：uint256[10]、动态数组：bytes32[]）和结构（即types内定义的）
- primaryType —— string，指定message的类型为types中定义的某个类型
- domain —— object，domain中的字段主要用来防止网络重放攻击，服务器验签时，可以结合domain中的某些字段的值，结合服务器请求来源的信息，对比验证请求的合法性。
- message —— object
```json
{
  "types": {
    "EIP712Domain": [
      { "name": "name", "type": "string" },
      { "name": "version", "type": "string" },
      { "name": "chainId", "type": "uint256" },
      { "name": "verifyingContract", "type": "address" }
    ],
    "Person": [
      { "name": "name", "type": "string" },
      { "name": "wallet", "type": "address" }
    ],
    "Mail": [
      { "name": "from", "type": "Person" },
      { "name": "to", "type": "Person" },
      { "name": "contents", "type": "string" }
    ]
  },
  "primaryType": "Mail",
  "domain": {
    "name": "Ether Mail",
    "version": "1",
    "chainId": 1,
    "verifyingContract": "0xCcCCccccCCCCcCCCCCCcCcCccCcCCCcCcccccccC"
  },
  "message": {
    "from": {
      "name": "Cow",
      "wallet": "0xCD2a3d9F938E13CD947Ec05AbC7FE734Df8DD826"
    },
    "to": {
      "name": "Bob",
      "wallet": "0xbBbBBBBbbBBBbbbBbbBbbbbBBbBbbbbBbBbbBBbB"
    },
    "contents": "Hello, Bob!"
  }
}
```

#### [EIP-721规范](https://eips.ethereum.org/EIPS/eip-721)
NFT的规范，其中：
- ERC165——合约检测的接口标准，只定义了一个方法，查询当前合约是否实现了某个接口（如ERC721）
- ERC721——NFT的接口标准，实现该接口的合约即为自定义的NFT合约
- ERC721TokenReceiver——（可选）NFT接收者接口标准，如果合约能接收NFT，则应该实现该接口
- ERC721Metadata——（可选）NFT合约的元数据接口标准，实现该接口的NFT合约支持查询NFT元数据
  - ERC721 元数据 JSON 模式： {titile,type,properties:{name,description,image}}
- ERC721Enumerable——（可选）NFT合约的枚举方法接口标准，NFT合约中发布的NFT可被统计、索引、遍历
- 更多参考：[opensea中关于NFT的开发标准](https://docs.opensea.io/docs/1-structuring-your-smart-contract)

#### [EIP-2981规范](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-2981.md)
定义了NFT的版税的设置和查询，用于交易时使用。  
规范中，定义的版税存储为数组[{ account, value }]的形式，可以支持在铸造nft时，传入多个版税受益人和分成比例，但交易时，实际的版税接收人只有第一个，实际销售时，所有分成比例加在一起，计算出售价的百分比，作为版税金额，发送给（第一个）版税接收人。后续由版税接收人自行分配，因为可能会涉及较多的财务计算，写入合约会大大增加gas消耗。

#### [EIP-1822规范](https://eips.ethereum.org/EIPS/eip-1822)
通用可升级代理标准（UUPS），状态：停滞

#### [EIP-3156规范](https://eips.ethereum.org/EIPS/eip-3156)
闪电贷规范

#### [其他规范参考](https://www.qklw.com/specialcolumn/20210820/212649.html)
