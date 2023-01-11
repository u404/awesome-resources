# 对比ethers.js和solidity的签名方法

### keccak256 —— 一种hash模式
```ts
// solidity 中
bytes32 kHash = keccak256(bytes(字符串));  // 对于字符串，即 solidity中的string
bytes32 kHash = keccak256(bytes字符串);  // 对于solidity中的 bytes
// ethers.js 中
import { utils } from 'ethers';

const kHash = utils.id(字符串);  // 对于字符串
const kHash = utils.solidityKeccak256(["string"], [字符串]); // 对于字符串
const kHash = utils.solidityKeccak256(["bytes"], [bytes字符串]);  // 对于bytes类型的字符串
```

### abi.encodePacked —— 紧打包编码（紧打包连接字符串）
```ts
// solidity 中
bytes dataEncoded = abi.encodePacked(地址, uint256数字, bytes32字符串);

// ethers.js 中
import { utils } from 'ethers';
const dataEncoded = utils.solidityPack(['address', 'uint256', 'bytes32'], [address, tokenId, k1]);
```

### 标准打包模式encode和decode
参考：https://docs.ethers.io/v5/api/utils/abi/coder/#AbiCoder-encode
```ts

```

### solidity中的签名验证 结合 ethers的signMessage
```ts
// ethers.js 中
const signature = await signer.signMessage(text);  // text是string的情况

// signMessage 还可以接收 utils.Bytes 类型
const signature = await signer.signMessage(utils.arrayify(keccakHash));  // 如果是 keccakHash 等16进制内容，则转化为Uint8Array

// solidity 中
const encodedText = abi.encodePacked("\x19Ethereum Signed Message:\n32", text); // text是string的情况
const hash = keccak256(encodedText);
ecrecover(hash, v, r, s);
```

一个签名和验签的典型问题：https://ethereum.stackexchange.com/questions/1777/workflow-on-signing-a-string-with-private-key-followed-by-signature-verificatio  


2022年10月18日一次签名前的hash预处理，这次处理是完全按照合约中验签的流程处理的。最终发现实际应该使用signTypedData方式签名。  

```ts
    const MINTER_TYPEHASH = utils.solidityKeccak256(['string'], ['Deployer(address wallet)']);
    const DOMAIN_SEPARATOR = utils.solidityKeccak256(
      ['bytes'],
      [
        utils.defaultAbiCoder.encode(
          ['bytes32', 'uint256'],
          [utils.id('EIP712Domain(uint256 chainId)'), this.chain.chainId],
        ),
      ],
    );

    const account = '0x3D14A490DdDBaea231CD9D49287cAca18D5Cba01'; // 读取用户钱包地址 this.chain.account;

    const encodeData = utils.defaultAbiCoder.encode(['bytes32', 'address'], [MINTER_TYPEHASH, account]);

    const hashData = utils.solidityKeccak256(['bytes'], [encodeData]);

    const packData = utils.solidityPack(['string', 'bytes32', 'bytes32'], ['\x19\x01', DOMAIN_SEPARATOR, hashData]);

    const digest = utils.solidityKeccak256(['bytes'], [packData]);

    console.log('digest', digest);

    const bytesMessage = utils.arrayify(digest);

    apiData.signature = await this.chain.signer.signMessage(bytesMessage);

    console.log('解密', utils.verifyMessage(bytesMessage, apiData.signature));

    console.log(apiData);
```

### 参考文档：
https://docs.soliditylang.org/en/v0.8.13/abi-spec.html#non-standard-packed-mode  
https://solidity-cn.readthedocs.io/zh/develop/units-and-global-variables.html#id3  
https://solidity-cn.readthedocs.io/zh/develop/abi-spec.html#abi-packed-mode  
https://docs.ethers.io/v5/api/utils/bytes/  