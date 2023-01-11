## 基础使用方法：
```ts
// Node中
const client = new BaseClient(node-fetch, provider, signer);
const res = await client.nft.mint(params); // 铸造
const res = await client.order.sell(params); // 售卖

// 前端中
const client = new Web3Client(fetch, ethereum); // fetch 或 ethereum 可以换为其他的
client.wallet.on("accountChanged", (account) => {
    console.log(account);
})
const account = await client.wallet.connect(); // 连接钱包

const res = await client.nft.mint();

const res = client.contracts.ERC721ContractAddress;



```

## contracts abi、address 怎么存储，单独打成一个repo
```ts
// contracts 
    // src
        ERC721.abi.json
        ERC721.address.ts
        index.ts
```

## 最终基于ethers + contracts 的代码实现
```ts
class BaseClient {
    requestHandler;
    provider;
    signer;
    constructor() {
        define
    }
}

```