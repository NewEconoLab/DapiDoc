# NNS方法

该类方法帮助Dapp便捷处理与NNS相关的事物

## getNamehashFromDomain_转换域名为Namehash

```typescript
Teemo.NEO.NNS.getNamehashFromDomain("test222.neo")
.then(result=>{
    console.log(result);
    document.getElementById("getNamehashFromDomain_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============进入了异常流程");
    
    console.log(error);
    document.getElementById("getNamehashFromDomain_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> 返回示例

```typescript
"68bea578a5565e0a4cb37811cda13d092cd6681f0687261f22b085586577b7b1"
```

将输入的NNS域名转换为NameHash

### 输入参数

| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| domain        | String   | 要转换的NNS域名                                                   |

### 成功的返回
| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | 转换后的NameHash                                                 |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getAddressFromDomain_NNS域名解析

```typescript
Teemo.NEO.NNS.getAddressFromDomain({
            "domain":"test222.neo",
            "network":"TestNet"
        })
.then(result=>{
    console.log(result);
    console.log("BigInteger",result);
    document.getElementById("getAddressFromDomain_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============进入了异常流程");
    console.log(error);
    document.getElementById("getAddressFromDomain_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> 返回示例

```typescript
{
  "address": "AHDV7M54NHukq8f76QQtBTbrCqKJrBH9UF",
  "TTL": "1568428640"
}
```

将输入的NNS域名解析为NEO地址

### 输入参数

| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| domain        | String   | 要解析的NNS域名                                                   |
| network       | String   | 网络类型选择                                                      |

### 成功的返回
| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| address        | String   | NNS域名的NEO地址解析结果                                          |
| TTL            | String   | NNS域名的到期时间戳                                               |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getDomainFromAddress_NNS域名反向解析

```typescript
Teemo.NEO.NNS.getDomainFromAddress({
            "address":"AHDV7M54NHukq8f76QQtBTbrCqKJrBH9UF",
            "network":"TestNet"
        })
.then(result=>{
    console.log(result);
    console.log("BigInteger",result);
    document.getElementById("getDomainFromAddress_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============进入了异常流程");
    console.log(error);
    document.getElementById("getDomainFromAddress_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> 返回示例

```typescript
{
  "namehash": "68bea578a5565e0a4cb37811cda13d092cd6681f0687261f22b085586577b7b1",
  "fullDomainName": "test222.neo",
  "TTL": "1568428640"
}
```

将输入的NNS域名解析为NEO地址

### 输入参数

| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| address       | String   | 要反向解析的NEO地址                                               |
| network       | String   | 网络类型选择                                                      |

### 成功的返回
| 参数名          | 类型     | 说明                                                             |
|:-------------  |:-------- |:---------------------------------------------------------------- |
| namehash       | String   | 反向解析得到的NNS NameHash                                        |
| fullDomainName | String   | 反向解析得到的NNS域名字符串                                        |
| TTL            | String   | NNS域名所有者设置反向解析时的NNS到期时间戳                          |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |