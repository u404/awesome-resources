### 方案1：请求无条件接收，逐步执行，异步通知，4个服务3个队列，纯队列存储
1. 当api-service收到合约交易的请求后，将其直接放入MQ - requests队列，告知请求方：已收到请求。
1. 交易执行者服务executor-service，不停的检查MQ - requests队列，如果有项目则取出一批（最多同时取10个），批量进行合约调用。调用成功的，将交易hash放入MQ - sended队列，并返回ack，调用失败的，会重新回到队列。
1. 交易检查者服务checker-service，不停的检查MQ - sended队列，如果有项目则取出对其进行交易状态查询，先使用getTransaction，查询交易是否存在，是否被挖掘，对于已被挖掘的进一步使用getTransactionReceipt查询交易状态。若交易不存在，表明被链上丢弃，则将其重新放入MQ - requests队列中。若交易状态为成功或失败，那么将IQ放入MQ - webhooks队列，返回ack。否则重新回到队列。
1. 交易通知者服务webhooks-service，不停的检查MQ - webhooks队列，如果有项目，则向相应的webhook通知地址发送通知，接收通知的接口收到通知，向MQ返回ack。
### 方案2：请求即执行，异步通知，3个服务2个队列，纯队列存储
1. 当api-service收到合约交易的请求后，经过校验（幂等性、重复），对于非重复的请求，计算nonce和gasPrice后，发送交易并获得hash。将该交易放入MQ - sended队列。告知请求方：请求发送成功。
1. 交易检查者服务checker-service，不停的检查MQ - sended队列，如果有项目则取出对其进行交易状态查询，先使用getTransaction，查询交易是否存在，是否被挖掘，对于已被挖掘的进一步使用getTransactionReceipt查询交易状态。若交易不存在，表明被链上丢弃，则调用api-service的重发接口重新发送交易（重发接口需充分考虑服务器异常引起的可能存在的重复交易问题），并返回ack。若交易状态为成功或失败，那么将交易和结果放入MQ - webhooks队列，返回ack。否则重新回到队列。
1. 交易通知者服务webhooks-service，不停的检查MQ - webhooks队列，如果有项目，则向相应的webhook通知地址发送通知，接收通知的接口收到通知，向MQ返回ack。


参考：
- [RabbitMQ消息确认机制](https://www.cnblogs.com/cjzzz/p/16087602.html)
