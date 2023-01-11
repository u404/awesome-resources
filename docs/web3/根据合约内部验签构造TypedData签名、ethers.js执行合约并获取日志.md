## 根据合约内部验签构造TypedData签名、ethers.js执行合约并获取日志
本次合约交互的开发中，合约中使用了typedData的验签方式，其特征是：合约中解析原文时，会以"\x19\x01"开头，区别于普通的消息以"\x19Ethereum Signed Message:\n"开头。  
具体以太坊提案参考：[EIP-712](https://github.com/ethereum/EIPs/blob/f589107dbf87ddc37e5c891f6fbb9b12a9b913c2/EIPS/eip-712.md)

## 合约中验签的代码示例：
```ts
// 验签方法外预定义的一些属性-------------------
// 合约属性
bytes32 public constant MINTER_TYPEHASH = keccak256('Deployer(address wallet)');
bytes32 public DOMAIN_SEPARATOR;
// 由可升级合约的 initialize() 函数赋值
DOMAIN_SEPARATOR = keccak256(abi.encode(keccak256('EIP712Domain(uint256 chainId)'), block.chainid));

// 验签方法定义---------------------------------
// 其中的recover方法是由
function getSigner(bytes calldata signature) public view returns (address) {
    require(admin != address(0), 'whitelist not enabled');
    // Verify EIP-712 signature by recreating the data structure
    // that we signed on the client side, and then using that to recover
    // the address that signed the signature for this data.
    bytes32 digest = keccak256(
        abi.encodePacked('\x19\x01', DOMAIN_SEPARATOR, keccak256(abi.encode(MINTER_TYPEHASH, msg.sender)))
    );
    // Use the recover method to see what address was used to create
    // the signature on this data.
    // Note that if the digest doesn't exactly match what was signed we'll
    // get a random recovered address.
    address recoveredAddress = digest.recover(signature);
    return recoveredAddress;
}
```

## js中如何构建typedData签名（基于ethers）：
```ts
// domain中属性支持：name、version、chainId、verifyingContract
// 根据合约中的 DOMAIN_SEPARATOR = keccak256(abi.encode(keccak256('EIP712Domain(uint256 chainId)'), block.chainid));
// 可对应构造出domain结构
const domain = {
  chainId: this.chain.chainId,
};
// types中定义value的类型及属性类型
// 根据合约中的 MINTER_TYPEHASH = keccak256('Deployer(address wallet)');
// 可以看到：主类型是Deployer，只有一个wallet属性是address内置类型
const types = {
  Deployer: [
    { name: 'wallet', type: 'address' },
  ],
};
// value中定义
// 根据合约中的打包的消息字符串 abi.encodePacked('\x19\x01', DOMAIN_SEPARATOR, keccak256(abi.encode(MINTER_TYPEHASH, msg.sender)))
// 可以构造出value
const value = {
  wallet: this.chain.account,
};
// 
signer._signTypedData(domain, types, value);
```

## ethers.js执行合约并获取交易事件（日志）
```ts
try {
  const tx = await contract.method(...);
  const receipt = await tx.wait();
  if (receipt.status === 1) {
    return receipt;
  }
  throw new Error('transaction failed');
} catch (e) {
  if ((e as any).code === 'TRANSACTION_REPLACED') {
    return (e as any).receipt;
  }
  throw e;
}
// 获取交易的事件
const event = receipt.events.find(e => e.event === 'CreatedArtist');
console.log(event.args);

```