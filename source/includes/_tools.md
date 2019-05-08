# Tool Methods

This type of method is designed to help Dapp validate and transform various types of NEO data and simplify NEO-related code in Dapp development. Most of the conversions are designed to convert the contract data information of getApplicationLog into a form that is easy for humans to understand and which Dapp needs to display.

## validateAddress

```typescript
Teemo.NEO.TOOLS.validateAddress("ASBhJFN3XiDu38EdEQyMY3N2XwGh1gd5WW")
.then(result=>{
    console.log(result);
    console.log("Address verification:"+ result);
    document.getElementById("validateAddress_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============Entered the abnormal process");
    console.log(error);
    document.getElementById("validateAddress_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> Example Response

```typescript
true | false
```

Verify that the entered address is a valid NEO address

### Input Arguments

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| address       | String   | Address to be verified                                           |

### Success Response

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | Boolen   | Validated boolean result                                         |

### Error Response

| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## getAddressFromScriptHash

```typescript
Teemo.NEO.TOOLS.getAddressFromScriptHash("72329a15ef9f2ea76cbc8dbc082c72316d87e1ae")
.then(result=>{
    console.log(result);
    console.log("Address:"+ result);
    document.getElementById("getAddressFromScriptHash_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============Entered the abnormal process");
    console.log(error);
    document.getElementById("getAddressFromScriptHash_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> Example Response

```typescript
"ASBhJFN3XiDu38EdEQyMY3N2XwGh1gd5WW"
```

Convert the input scriptHash to an NEO address

### Input Arguments

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| scriptHash    | String   | ScriptHash that needs to be converted                            |

### Success Response

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | Convert the output NEO address                                   |

### Error Response

| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## reverseHexstr

```typescript
Teemo.NEO.TOOLS.reverseHexstr("03febccf81ac85e3d795bc5cbd4e84e907812aa3")
.then(result=>{
    console.log(result);
    document.getElementById("reverseHexstr_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============Entered the abnormal process"); 
    console.log(error);
    document.getElementById("reverseHexstr_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> Example Response

```typescript
"a32a8107e9844ebd5cbc95d7e385ac81cfbcfe03"
```

Reverse the input hexStr

### Input Arguments

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| hexStr        | String   | Hexadecimal byte string                                          |

### Success Response

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | Flip hex string after flipping                                   |

### Error Response

| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## getStringFromHexstr

```typescript
Teemo.NEO.TOOLS.getStringFromHexstr("746573742064656d6f")
.then(result=>{
    console.log(result);
    document.getElementById("getStringFromHexstr_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============Entered the abnormal process");
    console.log(error);
    document.getElementById("getStringFromHexstr_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> Example Response

```typescript
"test demo"
```

Convert the input hexStr to a UTF8 string

### Input Arguments

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| hexStr        | String   | Hexadecimal byte string                                          |

### Success Response

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | UTF8 string after conversion                                     |

### Error Response

| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## getBigIntegerFromHexstr

```typescript
Teemo.NEO.TOOLS.getBigIntegerFromHexstr("3fb83001")
.then(result=>{
    console.log(result);
    console.log("BigInterger"+ result);
    document.getElementById("getBigIntegerFromHexstr_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============Entered the abnormal process");
    console.log(error);
    document.getElementById("getBigIntegerFromHexstr_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> Example Response

```typescript
"19970111"
```

Convert the input hexStr to a BigInteger string

### Input Arguments

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| hexStr        | String   | Hexadecimal byte string                                          |

### Success Response

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | Converted BigInteger string                                      |

### Error Response

| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## getBigIntegerFromAssetAmount

```typescript
Teemo.NEO.TOOLS.getBigIntegerFromAssetAmount({
        "amount":"12345.67890000",
        "assetID":"74f2dc36a68fdc4682034178eb2220729231db76",
        "network":"TestNet"
    })
.then(result=>{
    console.log(result);
    console.log("Decimals"+ result);
    document.getElementById("getDecimalsFromAssetAmount_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log("==============Entered the abnormal process");
    console.log(error);
    document.getElementById("getDecimalsFromAssetAmount_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> Example Response

```typescript
"1234567890000"
```

Convert the entered amount value to a BigInteger string based on the asset precision definition

### Input Arguments

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| amount        | String   | The amount of money that needs to be converted                   |
| assetID       | String   | Asset ID to be converted                                         |
| network       | String   | Network selections that need to be converted                     |

### Success Response

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | Converted BigInteger string                                      |

### Error Response

| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## getDecimalsStrFromAssetAmount

```typescript
Teemo.NEO.TOOLS.getDecimalsStrFromAssetAmount({
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
    console.log("==============Entered the abnormal process");
    console.log(error);
    document.getElementById("getBigIntegerFromAssetAmount_R").innerText = JSON.stringify(error, null, 2);
    reject();
})
```

> Example Response

```typescript
"12345.6789"
```

Convert the entered BigInteger string to a numeric value based on the asset precision definition

### Input Arguments

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| amount        | String   | The amount of BigInteger strings that need to be converted       |
| assetID       | String   | Asset ID to be converted                                         |
| network       | String   | Network selections that need to be converted                     |

### Success Response

| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| result        | String   | Converted amount value                                           |

### Error Response

| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |