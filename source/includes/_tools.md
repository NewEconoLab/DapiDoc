# 工具类方法

该类方法旨在帮助Dapp验证、转换NEO的各种类型数据，简化Dapp的开发中与NEO相关的代码。转换大多以将getApplicationLog的合约数据信息转换为人类易于理解的、Dapp需要显示的形式为设计初衷。

## validateAddress_验证地址正确性

```typescript
Teemo.NEO.TOOLS.validateAddress("ASBhJFN3XiDu38EdEQyMY3N2XwGh1gd5WW")
.then(result=>{
    console.log(result);
    console.log("地址验证："+ result);
    document.getElementById("validateAddress_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============进入了异常流程");
    console.log(error);
    document.getElementById("validateAddress_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> 返回示例

```typescript
true | false
```

验证输入的地址是否为一个合法的NEO地址

### 输入参数

| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| address       | String   | 需要验证的地址                                                    |

### 成功的返回
| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | Boolen   | 验证的布尔型结果                                                  |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getAddressFromScriptHash_转换ScriptHash为地址

```typescript
Teemo.NEO.TOOLS.getAddressFromScriptHash("72329a15ef9f2ea76cbc8dbc082c72316d87e1ae")
.then(result=>{
    console.log(result);
    console.log("得到的地址"+ result);
    document.getElementById("getAddressFromScriptHash_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============进入了异常流程");
    console.log(error);
    document.getElementById("getAddressFromScriptHash_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> 返回示例

```typescript
"ASBhJFN3XiDu38EdEQyMY3N2XwGh1gd5WW"
```

将输入的scriptHash转换为NEO地址

### 输入参数

| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| scriptHash    | String   | 需要转换的的scriptHash                                            |

### 成功的返回
| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | 转换输出的NEO地址                                                 |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## reverseHexstr_hex字符串翻转

```typescript
Teemo.NEO.TOOLS.reverseHexstr("03febccf81ac85e3d795bc5cbd4e84e907812aa3")
.then(result=>{
    console.log(result);
    document.getElementById("reverseHexstr_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============进入了异常流程"); 
    console.log(error);
    document.getElementById("reverseHexstr_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> 返回示例

```typescript
"a32a8107e9844ebd5cbc95d7e385ac81cfbcfe03"
```

将输入的hexStr进行翻转处理

### 输入参数

| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| hexStr        | String   | 十六进制byte字符串                                                |

### 成功的返回
| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | 翻转后十六进制byte字符串                                           |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getStringFromHexstr_转换hex字符串为字符串

```typescript
Teemo.NEO.TOOLS.getStringFromHexstr("746573742064656d6f")
.then(result=>{
    console.log(result);
    document.getElementById("getStringFromHexstr_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============进入了异常流程");
    console.log(error);
    document.getElementById("getStringFromHexstr_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> 返回示例

```typescript
"test demo"
```

将输入的hexStr转换为UTF8字符串

### 输入参数

| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| hexStr        | String   | 十六进制byte字符串                                                |

### 成功的返回
| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | 转换后UTF8字符串                                                  |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getBigIntegerFromHexstr_转换hex字符串为大整数

```typescript
Teemo.NEO.TOOLS.getBigIntegerFromHexstr("3fb83001")
.then(result=>{
    console.log(result);
    console.log("得到的地址"+ result);
    document.getElementById("getBigIntegerFromHexstr_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============进入了异常流程");
    console.log(error);
    document.getElementById("getBigIntegerFromHexstr_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> 返回示例

```typescript
"19970111"
```

将输入的hexStr转换为大整数字符串

### 输入参数

| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| hexStr        | String   | 十六进制byte字符串                                                |

### 成功的返回
| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | 转换后大整数字符串                                                  |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getDecimalsStrFromAssetAmount_转换大整数金额为数值

```typescript
Teemo.NEO.TOOLS.getDecimalsStrFromAssetAmount({
        "amount":"12345.67890000",
        "assetID":"74f2dc36a68fdc4682034178eb2220729231db76",
        "network":"TestNet"
    })
.then(result=>{
    console.log(result);
    console.log("得到的地址"+ result);
    document.getElementById("getDecimalsFromAssetAmount_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============进入了异常流程");
    console.log(error);
    document.getElementById("getDecimalsFromAssetAmount_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> 返回示例

```typescript
"1234567890000"
```

根据资产精度定义，将输入的金额数值转换为大整数字符串

### 输入参数

| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| amount        | String   | 需要转换的金额数值                                                |
| assetID       | String   | 需要转换的资产ID                                                  |
| network       | String   | 需要转换的网络选择                                                 |

### 成功的返回
| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | 转换后大整数字符串                                                 |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getBigIntegerFromAssetAmount_转换数值金额为大整数

```typescript
Teemo.NEO.TOOLS.getBigIntegerFromAssetAmount({
        "amount":"1234567890000",
        "assetID":"74f2dc36a68fdc4682034178eb2220729231db76",
        "network":"TestNet"
    })
.then(result=>{
    console.log(result);
    console.log("BigInteger",result);
    document.getElementById("getBigIntegerFromAssetAmount_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============进入了异常流程");
    console.log(error);
    document.getElementById("getBigIntegerFromAssetAmount_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> 返回示例

```typescript
"12345.6789"
```

根据资产精度定义，将输入的金额大整数字符串转换为数值

### 输入参数

| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| amount        | String   | 需要转换的金额大整数字符串                                         |
| assetID       | String   | 需要转换的资产ID                                                  |
| network       | String   | 需要转换的网络选择                                                 |

### 成功的返回
| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | 转换后金额数值                                                    |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |