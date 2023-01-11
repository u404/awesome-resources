# 区块链上的event、logs、topics
- event 是智能合约向前端发送通知的。event发出之后，EVM会将事件log写入区块链。
- 事件logs 可以作为一种廉价的存储方式（日志每字节8 gas，合约每32字节 20000 gas），缺点是只能由前端访问，无法由智能合约读取。
- 事件中的参数可以被标记为 indexed，最多可标记3个。这被称为主题（topic），可以用来检索事件。
除了匿名事件外，所有事件都含有一个默认topic——事件签名（如：'Transfer(address, address, uint256)' ）的keccak256 hash

### 参考资料：

- [了解以太坊区块链上的事件日志](https://medium.com/mycrypto/understanding-event-logs-on-the-ethereum-blockchain-f4ae7ba50378)
- [以太坊智能合约中的事件和日志指南](https://consensys.net/blog/developers/guide-to-events-and-logs-in-ethereum-smart-contracts/)

- [什么是事件topics](https://subscription.packtpub.com/book/big-data-&-business-intelligence/9781787122147/8/ch08lvl1sec73/what-are-event-topics)
