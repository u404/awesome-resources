## 参考资料：
- 阿里新框架Midway：http://www.midwayjs.org/docs/introduction
- MetaMask 插件分析：https://note.youdao.com/s/CGaPl8s9
- infura API：什么是 Infura？ Infura的使用 Infura官网 Infura 汇智文档   Infura 开发手册汇智
- 以太坊维基json-rpc规范：https://eth.wiki/json-rpc/API
- Ethers.js – 包含 JavaScript 和 TypeScript 的完整以太坊钱包的实现和工具。
客户端实现的两大规范： 
  - EIP-1193规范 ，定义了客户端provider功能。如：MetaMask 嵌入的window.etherum
  - EIP-1474规范 ，定义了客户端请求以太坊节点的JSON-RPC请求。也可以参考 MetaMask 更友好的文档
- 签名

## 主要功能实现：
- 初始化实例（链名称，infura Project ID）
- 创建账户 - 生成私钥、公钥、地址
- 获取链Id - 通过配置
- 调用infura请求
  - eth_call：立即执行一个消息调用，即查询区块信息
  - eth_sendRawTransaction：向节点提交一个已签名的交易
  - eth_gasPrice：返回当前gas价格，单位wei
- 签名信息 - 签名信息功能，主要是为了让钱包账户确认dapp向用户发出的消息，表示已知晓。签名后的消息，由dapp自行保存。
- 执行非签名合约读取方法 - eth_call
- 执行签名的合约写入方法 - eth_sendRawTransaction

## 设计思路：
- 第一版设计——可行，但过于繁琐
  - 对于创建账户、账户签名等功能的实现，可以基于metaMask的keyring库（eth-simple-keyring、eth-hd-keyring），直接引用其 eth-keyring-controller 库
  - 对rpc请求功能做封装，实现一个rpcClient抽象封装，初版中基于infura api，后期可以方便的改为其他api或我们自建节点服务。该部分主要包装了 EIP-1474规范 。
  - 基于rpcClient，实现一个provider，用于兼容ethers.js库。该部分包装了 EIP-1193规范
  - 基于ethers.js 和 provider，实现sdk功能。这部分sdk，可以抽象为跟客户端sdk一致，便于统一维护。
- 第二版设计——不可行，不支持私钥签名的功能
  - 基于ethers.js ，ethers.js 中 已经封装了 infura-provider，可以直接拿来用
- 第三版设计——可行，实际上是对第一版方案的改进
  - 基于ethers.js，ethers.js 中 继承 JsonRpcProvider，实现一个NodeProvider
  - 基于keyringController，和infura api，实现 EIP-1193规范 的provider
- 第四版设计——可行，直接基于ethers.js库，其中

#### provider 抽象封装
```ts
// EIP-1193规范的 provider
class Eip1193Provider extends EventEmitter {
    // 新版api
    request(opts: { method: string, params?: unknow[] | object }): Promise<unknown>;
    
    // event
    on("connect", (info: { chainId: string }) => void);
    on("disconnect", (error) => void);
    on("chainChanged", (chainId: string) => void);
    on("accountsChanged", (accounts: string[]) => void);
    on("message", (message: { type: string, data: unknown }) => void);


    // BaseProvider 应该实现的一些请求method
    eth_blockNumber
    eth_gasPrice
    eth_getBalance
    eth_getTransactionCount
    eth_getCode
    eth_getStorageAt
    eth_sendRawTransaction
    eth_getBlockByNumber
    eth_getBlockByHash
    eth_getTransactionByHash
    eth_getTransactionReceipt
    eth_call
    eth_estimateGas
    eth_getLogs
    
    
    
    
    // request proxies
    
    
    
}

class NodeProvider extends JsonRpcProvider {
    constructor(provider: Eip1193Provider) {};
    
    send(method: string, params: Array<any>): Promise<any>;
}

// 使用方法
const eip1193Provider = new Eip1193Provider();
const provider = new NodeProvider(eip1193Provider);

curl https://mainnet.infura.io/v3/37faf2c74b5c48f6b5855f7e655d6cea \
    -X POST \
    -H "Content-Type: application/json" \
    -d '{"jsonrpc":"2.0","method":"eth_getTransactionReceipt","params": ["0xbb3a336e3f823ec18197f1e13ee875700f08f03e2cab75f0d0b118dabb44cba0"],"id":1}'

```

#### 第一版设计 - 废弃
```ts
import KeyringController from 'eth-keyring-controller';
class RpcClient {
    constructor(keyringController: KeyringController): void;
    
    // 方法
    request(): Promise<unknow>;
    
    // 针对不同的rpc请求method的处理方式
    // 内置代理请求实现——如果注册了代理请求，则直接走代理请求而不发送真实rpc请求
    // 是不是可以设计为中间件模型，便于实现缓存功能？？？？
    proxies: {
        // wallet 代理
        eth_accounts: () => string[],  // 获取账户地址列表
        eth_coinbase: () => string, // accounts[0]
        eth_sendTransaction: () => 
        eth_signTransaction: () =>
        eth_sign: () => 
        eth_signTypedData: () =>
        eth_signTypedData_v3: () =>
        eth_signTypedData_v4: () =>
        personal_sign: () =>
        eth_getEncryptionPublicKey: () =>
        eth_decrypt: () =>
        personal_ecRecover: () =>
        
        // 自定义
        eth_sign: (data: any) => string, // 调用私钥签名数据
        eth_syncing: () => false,
        web3_clientVersion: () => `MetaMask/v${version}`,
        
        eth_getTransactionCount: () =>
        eth_getTransactionByHash: () => 
        
        
        // infura 代理
        eth_chainId: () => chainId, // 0x开头16进制字符串：0x1
        net_version: () => networkId, // 数字：1
            // 块相关请求缓存
            // 参数请求缓存
        
        eth_getTransactionByHash: () =>,
        eth_getTransactionReceipt: () =>,
        
        // 必须手动传入blockParam 参数的请求，有两种类型：
        // blockTag：latest、earliest、pending
        // blockNumber: 0x00000...
        eth_getStorageAt: (address, storagePosition, blockParam) =>,
        eth_getBalance: (address, blockParam) =>,
        eth_getCode: (address, blockParam) =>,
        eth_getTransactionCount: (address, blockParam) =>,
        eth_call: ({ from, to, gas, gasPrice, value, data }, blockParam) =>,
        eth_getBlockByNumber: () =>
        
        

    }
    
    eth_call: ()
    eth_sendRawTransaction: ()
    eth_gasPrice: ()
    
    
}

// provider 
class Provider: EventEmitter {
    
    constructor(rpcClient: RpcClient): void;
    
    // 新版api
    request(opts: { method: string, params?: unknow[] | object }): Promise<unknown>;
    
    // event
    on("connect", (info: { chainId: string }) => void);
    on("disconnect", (error) => void);
    on("chainChanged", (chainId: string) => void);
    on("accountsChanged", (accounts: string[]) => void);
    on("message", (message: { type: string, data: unknown }) => void);
}

// ethers.js
provider = new Ethers....
signer = provider.getSigner() ...


```