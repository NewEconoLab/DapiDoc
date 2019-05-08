# NNS Methods

This type of method helps Dapp to easily handle things related to NNS.

## getNamehashFromDomain

```typescript
Teemo.NEO.NNS.getNamehashFromDomain("test222.neo")
.then(result=>{
    console.log(result);
    document.getElementById("getNamehashFromDomain_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============Entered the abnormal process");
    
    console.log(error);
    document.getElementById("getNamehashFromDomain_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> Example Response

```typescript
"68bea578a5565e0a4cb37811cda13d092cd6681f0687261f22b085586577b7b1"
```

Convert the entered NNS domain name to NameHash

### Input Arguments

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| domain        | String   | The NNS domain name to be converted                              |

### Success Response

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | Converted NameHash                                               |

### Error Response

| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## getAddressFromDomain

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
    console.log("==============Entered the abnormal process");
    console.log(error);
    document.getElementById("getAddressFromDomain_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> Example Response

```typescript
{
  "address": "AHDV7M54NHukq8f76QQtBTbrCqKJrBH9UF",
  "TTL": "1568428640"
}
```

Parse the entered NNS domain name into an NEO address

### Input Arguments

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| domain        | String   | The NNS domain name to be resolved                               |
| network       | String   | Network type selection                                           |

### Success Response

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| address       | String   | NEO address resolution result of the NNS domain name             |
| TTL           | String   | Expiration time stamp of the NNS domain name                     |

### Error Response

| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## getDomainFromAddress

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
    console.log("==============Entered the abnormal process");
    console.log(error);
    document.getElementById("getDomainFromAddress_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> Example Response

```typescript
{
  "namehash": "68bea578a5565e0a4cb37811cda13d092cd6681f0687261f22b085586577b7b1",
  "fullDomainName": "test222.neo",
  "TTL": "1568428640"
}
```

Reverse the input NEO address to NNS domain name

### Input Arguments

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| address       | String   | NEO address to be resolved in reverse                            |
| network       | String   | Network type selection                                           |

### Success Response

| Parameter      | Type     | Description                                                              |
|:-------------  |:-------- |:------------------------------------------------------------------------ |
| namehash       | String   | Reverse resolution of NNS NameHash                                       |
| fullDomainName | String   | Reverse resolution of the NNS domain name string                         |
| TTL            | String   | NNS domain owner sets the NNS expiration timestamp when reverse parsing  |

### Error Response

| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |