### 请求格式
```ts
const headers = {
}

const data = {
    
}

axios.put(url, data, { headers })

```

### 服务端对签名等header的计算
```ts
async awsPreUpload() {
    const { hash = "" } = this.ctx.query // 内容hash通过前端计算传入

    const region = "us-east-1"
    const bucket = "fe-resource"
    const service = "s3"
    const AccessKeyId = "XXXXXXXXXXXXXXXXXXXX"
    const SecretAccessKey = "YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY"
    const date = new Date()
    const dateISOString = date.toISOString().replace(/[-:]/g, "").replace(/\.\d+/, "")
    const dateFormatString = `${date.getFullYear()}${date.getMonth() > 8 ? date.getMonth() + 1 : `0${date.getMonth() + 1}`}${date.getDate()}`

    const host = `${bucket}.${service}.amazonaws.com`
    const key = `files/test-upload.mp4` // 变量
    const headers = {
      "x-amz-content-sha256": hash,
      "x-amz-date": dateISOString,
    }
    const _headers = {
      host,
      ...headers,
    }

    const utils = {
      getKeyValueStr: (k, value) => `${k.toLowerCase()}:${value.trim()}`,
    }

    const urlString = `/${key}`
    const queryString = ""
    const headerString = `${Object.keys(_headers).sort().map(k => utils.getKeyValueStr(k, _headers[k])).join("\n")}\n`
    const signedHeaderString = Object.keys(_headers).sort().join(";")
    const payloadString = hash

    const requestString = `PUT
${urlString}
${queryString}
${headerString}
${signedHeaderString}
${payloadString}`

    const scope = `${dateFormatString}/${region}/${service}/aws4_request`

    const hexHashRequestString = crypto.createHash("sha256").update(requestString).digest("hex") // not const hexHmacRequestString = crypto.createHmac("sha256", requestString).digest("base64")

    const StringToSign = `AWS4-HMAC-SHA256
${dateISOString}
${scope}
${hexHashRequestString}`

    const DateKey = crypto.createHmac("sha256", `AWS4${SecretAccessKey}`).update(dateFormatString).digest()
    const DateRegionKey = crypto.createHmac("sha256", DateKey).update(region).digest()
    const DateRegionServiceKey = crypto.createHmac("sha256", DateRegionKey).update(service).digest()
    const SigningKey = crypto.createHmac("sha256", DateRegionServiceKey).update("aws4_request").digest()
    const signature = crypto.createHmac("sha256", SigningKey).update(StringToSign).digest("hex")

    const Authorization = `AWS4-HMAC-SHA256 Credential=${AccessKeyId}/${scope},SignedHeaders=${signedHeaderString},Signature=${signature}`

    headers.Authorization = Authorization

    this.ctx.body = {
      code: 0,
      data: {
        apiUrl: `https://${bucket}.${service}.${region}.amazonaws.com/`,
        url: `https://${host}/${key}`,
        headers,
      },
    }
}
```

### 参考文档

上传对象：https://docs.aws.amazon.com/zh_cn/AmazonS3/latest/userguide/upload-objects.html  
REST API 参考：https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-header-based-auth.html  
PutObject：https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObject.html  
