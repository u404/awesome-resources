[ipfs基础理解](https://www.sohu.com/a/228754661_100124396)

[infura ipfs文档](https://docs.infura.io/infura/networks/ipfs)
[ipfs.io 规范文档](https://docs.ipfs.io/)
[ipfs js库说明](https://docs.ipfs.io/reference/js/api/#ipfs-and-javascript)
[ipfs-core api文档](https://github.com/ipfs/js-ipfs/blob/master/docs/core-api/README.md)，关注其中的Files

[js-ipfs-examples 示例](https://github.com/ipfs-examples/js-ipfs-examples/tree/master/examples)


其中纯前端客户端使用：ipfs-http-client
Node端可以使用：ipfs-core
可以直接启动一个节点服务：js-ipfs

IPFS网关：  
公共网关列表：https://ipfs.github.io/public-gateway-checker/  

检索查看  
https://ipfs.io/ipfs/QmTHciqrJrY1PBb2y474gtL9T9EyfRk6Sijvwv6viH1Uun  
https://ipfs.io/ipfs/QmTHciqrJrY1PBb2y474gtL9T9EyfRk6Sijvwv6viH1Uun/image.jpg  

ipfs: "张光辉"：https://ipfs.io/ipfs/QmcPXLezhEkvDN9XLNFQZHjBG1s4VNiS41nZj5YNF4hxLU  
ipfs："zhangguanghui"：https://ipfs.io/ipfs/QmX7TYqC31sLysbK3Hden5ZRpFp5vBEYKSMJuSNoVGvGHK  

可用的ipfs节点网址：  
https://ipfs.io/ipfs  
https://ipfs.moralis.io:2053  
https://dweb.link/ipfs/  



ipfs 服务：  
https://www.pinata.cloud/  
https://web3.storage/   免费  
https://nft.storage/   

ipfs 目录形式示例：  
https://ipfs.io/ipfs/QmeSjSinHpPnmXmspMjwiXyN6zS4E9zccariGR3jxcaWtq  
https://www.reddit.com/r/ipfs/comments/t9tenx/how_can_i_make_an_ipfs_directory_behave_like_a/  

MFS目录形式：  
https://www.ked.pub/ipfs/file-systems/  
https://github.com/ipfs/js-ipfs/blob/master/docs/core-api/FILES.md#ipfsadddata-options  


IPFS目录形式上传示例：  
参考：https://github.com/ipfs/js-ipfs/blob/master/docs/core-api/FILES.md  


```ts
// 运行 node  xxx.mjs

import * as IPFS from 'ipfs-core';

const main = async () => {
  const ipfs = await IPFS.create();
  const files = [
    {
      path: '/tmp/1.txt',
      content: '111',
    },
    {
      path: '/tmp/2.txt',
      content: '222',
    },
  ];

  for await (const result of ipfs.addAll(files, { wrapWithDirectory: true })) {
    console.log(result);
  }
};

main();

```