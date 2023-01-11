## 目标：
- 分析window.ethereum 对象的实现源码 - 最终在node端实现 JSON-RPC request 的请求
- 分析钱包签名的实现逻辑 - 最终在node端实现 签名数据的功能

## 原理：
1. 通过content script 注入脚本到页面（inject script）—— 即注入 window.ethereum 对象
   - window.ethereum 即 遵循 [EIP-1193规范](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1193.md) 的provider，最主要功能是实现了 JSON RPC request 功能
   - 无论是钱包连接、合约交互、消息签发，都是 [EIP-1474规范](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1474.md) 定义的，也可以参考 [MetaMask 更友好的文档](https://docs.metamask.io/guide/rpc-api.html)
   - JSON RPC request 的底层实现是通过将请求转发到 background 实现的
   - 另外，还有事件通知的触发 - 需要另行分析
1. 建立inject script 到 content script 间的通信
   - JSON RPC request 的实现是基于 JsonRpcEngine，实现了一个洋葱圈的模型，便于集成中间件。
   - 其中请求实际的发送，是由json-rpc-middleware-stream中的createStreamMiddleware实现的，将其转发到了流
     - 流的转发逻辑比较绕，先是由 [@metamask/object-multiplex](https://www.npmjs.com/package/@metamask/object-multiplex)，创建了多路复用的流mux
     - mux 连接了 由参数传入的 WindowPostMessageStream 流 和 createStreamMiddleware 创建的流
     - 最终由 postMessage 发送出去
   - 底层基于postMessage 和 addEventListener('message', handler)
   - 将其包装成了Duplex（双工流），易于传递
1. 建立content script 到 background 间的通信
   - 基于 [extensionizer](https://www.npmjs.com/package/extensionizer) 库，这个库进一步包装了 用于支持浏览器插件开发的 [WebExtensions API](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions)，用于兼容多浏览器
   - 基于 [Javascript APIs](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API)  [browser.runtime](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/runtime) 实现的 content script 到 background 之间的通信
1. background 中 [extension.runtime.onConnect](https://developer.mozilla.org/en-US/docs/Mozilla/Add-ons/WebExtensions/API/runtime/onConnect) 监听请求
   - 回调参数Port的格式：{ name, onMessage, postMessage, sender: { tab,  url } }
   - 重新把Port包装为PortStream 对象
   - 将PortStream传递到MetamaskController.setupUntrustedCommunication() 方法，其中会创建一个多路流连接PortStream 和 内部处理
   - 多路流的一端设置通过setupProviderConnection()，其中：
     - engine = this.setupProviderEngine()，providerStream = createEngineStream({ engine })，providerStream连接到多路流的一端
     - setupProviderEngine()中，engine = new JsonRpcEngine(); 洋葱圈模型，注册了大量辅助处理请求的中间件。其中由metamask插件发起的，那么origin被赋值为metamask，如果是从网页发起的，则是网页域名。
     - 核心中间件1：createMethodMiddleware(hooks) // 其中的hooks 定义了用于处理 一部分账户、钱包相关的rpc请求。
     - 核心中间件2：providerAsMiddleware(provider) // provider = this.networkController.getProviderAndBlockTracker().provider。其中 networkController = new NetworkController(initState.NetworkController) ，然后立即initializeProvider()，即调用networkController.initializeProvider(opts) 初始化了provider
       - opts 保存到了 _baseProviderParams 私有变量里，用于后边在创建metamaskMiddleware
       - 然后 { type, rpcUrl, chainId } 从内部的变量里取出，使用这些参数 _configureProvider()
         - type分为两类，一类是infura支持的，会创建networkClient = InfuraClient
         - 另一类是“rpc”，需要配置rpcUrl（基于url的rpc endpoints），会创建networkClient = JsonRpcClient
         - 然后执行_setNetworkClient({ networkMiddleware, blockTracker })，也就是说，networkClient实际是抽象了两个功能，一个用于JsonRpcEngine的middleware，另一个是blockTracker
         - provider 最终是 providerFromEngine(engine)  // 最终实际提供sendAsync、send方法
           - engine是个JsonRpcEngine，集成了metamaskMiddleware和networkMiddleware，用于处理不同的请求。
           - 其中metamaskMiddleware，拦截了account、需要签名的、需要钱包提示的请求方法，请求由钱包插件本身处理。
             - ScaffoldMiddleware：读取eth_syncing, web3_clientVersion
             - WalletMiddleware：处理钱包账户、交易、请求
             - PendingNonceMiddleware：请求
             - PendingTxMiddleware：
           - networkMiddleware即由InfuraClient或JsonRpcClient创建，用于处理json rpc请求以及一些可以通过本地/内存缓存或变量/配置读取的请求：eth_chainId、net_version、eth_getTransactionByHash、eth_getTransactionReceipt等等。
             - NetworkAndChainIdMiddleware：从当前配置读取chainId和networkId
             - BlockCacheMiddleware：缓存与区块关联的请求
             - InflightCacheMiddleware：缓存非区块关联的请求，基于method和params
             - BlockRefMiddleware：为携带blockTag的请求，创建子请求，子请求附加blockRefIndex = 最后一个区块编号，子请求res最终合并到主请求res
             - RetryOnEmptyMiddleware：为携带blockTag的请求，创建子请求，子请求如果无数据，会不断重试，直到达到最大重试次数10。
             - BlockTrackeInspectorMiddleware：根据某些请求的响应，检查最新块，同步更新本地块跟踪器缓存。
1. 账户地址和私钥管理机制
   - 账户加入metamask的管理，有3种方式：创建账户、导入账户、连接硬件钱包
     - keyring——钥匙圈，定义签名策略，keyring的type有多种类型：[eth-simple-keyring](https://www.npmjs.com/package/eth-simple-keyring)、[eth-hd-keyring](https://github.com/MetaMask/eth-hd-keyring/blob/main/index.js)、eth-trezor-keyring、@metamask/eth-ledger-bridge-keyring、eth-lattice-keyring、@keystonehq/metamask-airgapped-keyring。每个keyring都实现了创建私钥、根据私钥创建公钥、将公钥转化为地址等功能。可参考：[eth-simple-keyring源码](https://github.com/MetaMask/eth-simple-keyring/blob/main/index.js)
     - metamask默认创建账户使用的keyring 是 eth-hd-keyring，调用keyringController.addNewAccount(primaryKeyring) 创建账户
       - 先使用 [bip39](https://www.npmjs.com/package/bip39) 生成助记词，然后生成种子seed
       - 然后根据种子，基于 [ethereumjs-wallet](https://github.com/ethereumjs/ethereumjs-wallet/blob/83c1b1aa601977e5b0bbcf3e09353c91f993ff54/docs/classes/ethereumhdkey.md) 库生成 EthereumHDKey的实例，然后再根据path 派生 一个 EthereumHDKey 实例作为root。这个root就是对应于用户。
       - 用户下有多个账户，每个账户都是 root.deriveChild(i)，派生的孩子child，child.getWallet()，即生成了钱包 { privateKey, publicKey, getAccounts() }
       - 钱包的生成，是不需要与链上交互的。
       - 一个现成可用的网页版钱包生成器：https://vanity-eth.tk/，可以用来生成靓号地址。
     - 导入账户功能就是导入一个已知的私钥 或 json文件（{ locked?, private, encrypted? }，实际也是导入一个私钥）
       - 最终私钥作为eth-simple-keyring类型，keyringController.addNewKeyring()管理
   - 签名——签名最终调用的是keyring.signTransaction 或 signMessage 等相关方法，所有keyring的相关方法都是继承自eth-simple-keyring，原理就是私钥加密。


## 通过contentscript注入的 window.ethereum（eip-1193 provider）对象：
```ts
// metamask-extension 中
// app/contentscript.js

// 
const inpageContent = fs.readFileSync('dist/chrome/inpage.js');

// 
injectScript(inpageContent);

// inpage.js - 即注入到页面的代码
import { initializeProvider } from '@metamask/providers/dist/initializeInpageProvider';
initializeProvider({
  // metamaskStream 用postMessage方式模拟的stream，
  // 用于contentScript 和 页面script通信  
  connectionStream: metamaskStream, 
  logger: log,
  shouldShimWeb3: true,
})

// @metamask/providers
// /src/initializeInpageProvider.ts
// 注入 window.ethereum
// 该方法的全部签名 - 显然，可以直接引入到node端
export function initializeProvider({
  connectionStream,
  jsonRpcStreamName,
  logger = console,
  maxEventListeners = 100,
  shouldSendMetadata = true,
  shouldSetOnWindow = true,
  shouldShimWeb3 = false,
}: InitializeProviderOptions): MetaMaskInpageProvider {
    let provider = new MetaMaskInpageProvider(connectionStream, {
        jsonRpcStreamName: undefined,
        logger,
        maxEventListeners: 100,
        shouldSendMetadata: true,
    })
    if(shouldSetOnWindow) {
        setGlobalProvider(provider); // 设置到window全局对象
    }
    if(shouldShimWeb3) { // 注入一个window.web3兼容web3.js
        ...
    }
    return provider;    
}


// @metamask/providers
// /src/MetaMaskInpageProvider.ts
// MetaMaskInpageProvider
// 如果有不适合Node的地方，可以继承重写该类
export default class MetaMaskInpageProvider extends BaseProvider {
    // 继承自BaseProvider的属性和方法
    chainId: string | null;
    selectedAddress: string | null;
    isConnected(): boolean;
    request<T>(args: RequestArguments): Promise<Maybe<T>>;
    // 调用this._rpcRequest()，下面定义的send()、sendAsync()、enable()一致
    
    
    // 自身声明的属性和方法
    _metamask: ...; // ReturnType<MetaMaskInpageProvider['_getExperimentalApi']>
    networkVersion: string;
    isMetaMask: true;
    constructor(connectionStream: Duplex, { // 双工流，用于消息通信
      jsonRpcStreamName = 'metamask-provider',
      logger = console,
      maxEventListeners = 100,
      shouldSendMetadata = true,    // 该选项引用了window/document，在Node端不适用        
    });
    sendAsync(payload, callback);
    addListener(eventName, listener);
    on(eventName, listener);
    once(eventName, listener);
    prependListener(event, listener);
    perpendOnceListener(event, listener);
    enable(): Promise<string[]>;
    // send方法的3个重载
    send<T>(method: string, params): Promise<JsonRpcResponse<T>>;
    send<T>(payload: JsonRpcRequest<unknown>, callback): void;
    send<T>(payload: SendSyncJsonRpcRequest): JsonRpcResponse<T>;
    
    // 底层请求封装
    protected _rpcRequest(payload, callback);
    // 调用this._rpcEngine.handle(payload, cb);
    // const rpcEngine = new JsonRpcEngine();
    // rpcEngine.push(createIdRemapMiddleware());
    // rpcEngine.push(createErrorMiddleware(this._log));
    // rpcEngine.push(this._jsonRpcConnection.middleware);
    // this._rpcEngine = rpcEngine;
    
    
}

// create
```





