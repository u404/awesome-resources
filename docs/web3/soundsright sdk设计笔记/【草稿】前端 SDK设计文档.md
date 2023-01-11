> 目的是实现一个集成了钱包连接、链交互的sdk

## 参考资料：
- 钱包插件注入的window.ethereum 相关api：https://docs.metamask.io/guide/ethereum-provider.html
- 参考项目：pancake-frontend
- 钱包连接功能实现：web3-react
- 移动端app钱包连接：https://docs.walletconnect.com/
- 钱包按钮部分：pancake-toolkit/packages/pacake-uikit

## 方案1：基于@web3-react部分依赖开发
- web3-react中，连接钱包的部分逻辑，分了几个层次封装，从React Context 状态组件 - 钱包Connector抽象 - 多种连接方式的Connector实现类。Connector实现类，不依赖react，且具有较为严谨的逻辑封装，可以直接引用。  
- 钱包连接的状态管理部分，独立实现。
```ts
class GustoChain {
    // constructor(); // 保持之前的连接
    
    // 属性
    chainId: number;
    account: string;
    connectType: ConnectType; // 枚举值
    provider: ethers.providers.Web3Provider;
    signer: ethers.Signer;
    error: Error;
    
    // 方法
    activate(type?: ConnectType): Promise<void>; // 连接钱包，如果网络不合要求，请求切换网络。
    deactivate(): void;
    setupNetwork(): Promise<boolean>;
    on(event: string, handler: e => void): void;
    off(event: string, handler: e => void): void;
    // ... 继承自EventEmitter
    
    signMessage(msg: string): Promise<void>; // 发送签名消息
    getContract(address, abi, readonly: boolean = true): ethers.Contract;  // 获取合约（封装不完善）
    execContractMethodRead(address, abi, method, params: any[]) // 待定，执行合约的只读方法
    execContractMethodWrite(address, abi, method, params: any[]) // 待定，执行合约的写方法
    // 事件 - 暂定
    on("update", ({ chainId, account }) => void); // update事件触发时，如果连接正常，会携带正确的chainId和account
    on("error", (error) => void); // 错误，触发内部不处理的错误，连接会断开。在触发之前，会先触发update，将chainId，account清空
    // 由于web3-react定义的connector，且api可能具有一定的差异，
    // 其所有connector，统一对外暴露的事件只有3个，因此难以做到更细粒度的事件控制
    // export enum ConnectorEvent {
    //   Update = 'Web3ReactUpdate',
    //   Error = 'Web3ReactError',
    //   Deactivate = 'Web3ReactDeactivate'
    //  }
    
}
```

## 方案2：参考其钱包连接代码，彻底独立实现
- web3-react中，封装的connector类型极多，因此需要自行将项目中需要用到的，基于策略模式抽象封装一下。
```ts
// GustoChain 依赖 ChainClient ，包装具体的合约操作，如swap、liquidity、stake 等具体的合约操作
class GustoChain {
    activate;
    deactivate;
    on;
    off;
    swap;
    liquidity;
    stake;
}
```