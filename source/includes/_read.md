# 读类型方法

读方法不改变链数据. 它帮助你为你的用户查询信息, 也为你的Dapp提供必要的信息:

## getProvider_获得提供者信息

```typescript
Teemo.NEO.getProvider()
.then((provider: Provider) => {
  const {
    name,
    website,
    version,
    compatibility,
    extra,
  } = provider;

  const {
    theme,
    currency,
  } = extra;

  console.log('Provider name: ' + name);
  console.log('Provider website: ' + website);
  console.log('Provider dAPI version: ' + version);
  console.log('Provider dAPI compatibility: ' + JSON.stringify(compatibility));
  console.log('Provider UI theme: ' + theme);
  console.log('Provider Base currency: ' + currency);
})
.catch(({type: string, description: string, data: any}) => {
  switch(type) {
    case NO_PROVIDER:
      console.log('No provider available.');
      break;
    case CONNECTION_DENIED:
      console.log('The user rejected the request to connect with your dApp.');
      break;
  }
});
```

> 返回示例

```typescript
{
  "name": "TeemoWallet",
  "version": "0.1",
  "website": "nel.group",
  "compatibility": [
    "typescript",
    "javascript"
  ],
  "extra": {
    "theme": "",
    "currency": ""
  }
}
```

返回Dapi提供者（插件钱包）的信息，包括名字、版本、NEP信息以及扩展信息。

### 输入参数

无需输入参数

### 成功的返回
| 参数名         | 类型     | 说明                                                             |
|:------------- |:-------- |:---------------------------------------------------------------- |
| name          | String   | 钱包（Dapi提供者）的名称                                           |
| website       | String   | 钱包（Dapi提供者）的官方网址                                       |
| version       | String   | 钱包（Dapi提供者）提供Dapi的版本                                   |
| compatibility | String[] | 钱包（Dapi提供者）支持的NEP协议列表                                |
| extra         | Object   | 钱包（Dapi提供者）的附加属性                                       |

##### 扩展
| 参数名     | 类型   | 说明                      |
| --------- | ------ | ------------------------- |
| theme     | string | 钱包（Dapi提供者）的UI风格  |
| currency  | string | 用户设置的基础货币          |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getNetworks_获取网络参数

```typescript
dapi.NEO.getNetworks()
.then(response => {
  const {
    networks,
    defaultNetwork,
  } = response.networks;

  console.log('Networks: ' + networks);
  // eg. ["TestNet", "TestNet", "PrivateNet"]

  console.log('Default network: ' + defaultNetwork);
  // eg. "TestNet"
})
.catch(({type: string, description: string, data: any}) => {
  switch(type) {
    case NO_PROVIDER:
      console.log('No provider available.');
      break;
    case CONNECTION_DENIED:
      console.log('The user rejected the request to connect with your dApp');
      break;
  }
});
```

> 返回示例

```typescript
{
  networks: ["TestNet", "TestNet", "PrivateNet"],
  defaultNetwork: "TestNet",
}
```

返回钱包（Dapi提供者）允许的网络类型, 并返回目前用户选择的网络.

### 输入参数

无需输入参数

### 成功的返回

| 参数名         | 类型      | 说明                                                                |
|:-------------- |:-------- |:------------------------------------------------------------------ |
| networks       | String[] | 一个允许的网络类型的列表                                             |
| defaultNetwork | String   | 目前钱包设置的网络类型                                               |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |


## getAccount_获取账户信息

```typescript
Teemo.NEO.getAccount()
.then((account: Account) => {
  const {
    address,
    label,
  } = account;

  console.log('Account address: ' + address);
  console.log('Account label: ' + label);
})
.catch(({type: string, description: string, data: any}) => {
  switch(type) {
    case NO_PROVIDER:
      console.log('No provider available.');
      break;
    case CONNECTION_DENIED:
      console.log('The user rejected the request to connect with your dApp');
      break;
  }
});
```

> 返回示例

```typescript
{
  address: 'AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru',
  label: 'My Spending Wallet'
}
```

返回当前钱包（Dapi提供者）被选中（使用中）的账户信息.

### 成功的返回
| 参数名     | 类型   | 说明                                                                |
|:--------- |:------ |:------------------------------------------------------------------  |
| address   | String | 当前钱包（Dapi提供者）被选中（使用中）的NEO地址                         |
| label     | String | 当前钱包（Dapi提供者）被选中（使用中）的用户设定名称                    |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getBalance_获取资产余额

```typescript
Teemo.NEO.getBalance({
  params: {  
    "address": "ASBhJFN3XiDu38EdEQyMY3N2XwGh1gd5WW",  
    "assets": ["602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee7969282de7"]
  },
  network: 'TestNet',
})
.then((results: BalanceResults) => {
  Object.keys(results).forEach(address => {
    const balances = results[address];
    balances.forEach(balance => {
      const { assetID, symbol, amount } = balance

      console.log('Address: ' + address);
      console.log('Asset ID: ' + assetID);
      console.log('Asset symbol: ' + symbol);
      console.log('Amount: ' + amount);
    });
  });
})
.catch(({type: string, description: string, data: any}) => {
  switch(type) {
    case NO_PROVIDER:
      console.log('No provider available.');
      break;
    case CONNECTION_DENIED:
      console.log('The user rejected the request to connect with your dApp');
      break;
  }
});
```

> 单地址特定资产请求

```typescript
// 输入
{
  params: {
    address: 'AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru',
    assets: ["602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee7969282de7"]
  },
  network: 'TestNet',
}

// 输出
{
  AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru: [
    {
      assetID: '602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee7969282de7',
      symbol: 'GAS',
      amount: '0.00000233',
    }
  ],
}
```

> 单地址全部资产（设置显示）请求

```typescript
// 输入
{
  params: {
    address: 'AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru',
  },
  network: 'TestNet',
}

// 输出
{
  AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru: [
    {
      assetID: 'c56f33fc6ecfcd0c225c4ab356fee59390af8560be0e930faebe74a6daff7c9b',
      symbol: 'NEO',
      amount: '10',
    },
    {
      assetID: '602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee7969282de7',
      symbol: 'GAS',
      amount: '777.0001',
    },
    {
      assetID: '74f2dc36a68fdc4682034178eb2220729231db76',
      symbol: 'CGAS',
      amount: '0.00000233',
    },
    {
      assetID: 'fc732edee1efdf968c23c20a9628eaa5a6ccb934',
      symbol: 'NNC',
      amount: '2000',
    }
  ]
}
```

> 多地址请求

```typescript
// 输入
{
  params: [
    {
      address: 'AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru',
    },
    {
      address: 'AbKNY45nRDy6B65YPVz1B6YXiTnzRqU2uQ',
      asset: ["602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee7969282de7"],
    },
  ],
  network: 'TestNet',
}

// output
{
  AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru: [
    {
      assetID: 'c56f33fc6ecfcd0c225c4ab356fee59390af8560be0e930faebe74a6daff7c9b',
      symbol: 'NEO',
      amount: '10',
    },
    {
      assetID: '602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee7969282de7',
      symbol: 'GAS',
      amount: '777.0001',
    },
    {
      assetID: '74f2dc36a68fdc4682034178eb2220729231db76',
      symbol: 'CGAS',
      amount: '0.00000233',
    },
    {
      assetID: 'fc732edee1efdf968c23c20a9628eaa5a6ccb934',
      symbol: 'NNC',
      amount: '2000',
    }
  ],
  AbKNY45nRDy6B65YPVz1B6YXiTnzRqU2uQ: [
    {
      assetID: '602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee7969282de7',
      symbol: 'GAS',
      amount: '11000',
    }
  ]
}
```


允许Dapp为用户查询资产余额, 此方法同时支持全局（UTXO）资产和合约（NEP-5）资产

### 输入参数
| 参数       | 类型                               | 说明                                                                                     |
|:--------- |:---------------------------------- |:---------------------------------------------------------------------------------------- |
| params    | BalanceRequest or BalanceRequest[] | 一个BalanceRequest结构或者一个BalanceRequest结构组                                         |
| network   | String                             | 从GetNetworks获取的网络名称            |

#### BalanceRequest 余额请求结构
| 参数        | 类型     | 说明                                                                          |
|:---------- |:-------- |:----------------------------------------------------------------------------- |
| address    | String   | 希望获取余额的地址                                                              |
| assets     | String[] | 一个资产ID或一个资产ID组                                                        |
| fetchUTXO? | boolean  | 这是一个可选的参数，表示是否需要返回UTXO（Teemo暂时不支持）          |

### 成功的返回
| 参数       | 类型              | 说明                                                                                 |
|:--------- |:----------------- |:------------------------------------------------------------------------------------ |
| address_1 | BalanceResponse[] | 键是地址，比如"AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru"                                    |
| address_2 | BalanceResponse[] | 键是地址，比如"AbKNY45nRDy6B65YPVz1B6YXiTnzRqU2uQ"                                    |
| address_n | BalanceResponse[] | 键是地址，比如"AUdawyrLMskxXMUE8osX9mSLKz8R7777kE"                                    |

<aside class="notice">
address_1等表示真实地址的指代
</aside>


#### 余额返回结构
| 参数       | 类型    | 说明                                                                                                 |
|:--------- |:------- |:---------------------------------------------------------------------------------------------------- |
| assetID   | String  | 资产ID                                                                                                |
| symbol    | String  | 资产的单位名称                                                                                         |
| amount    | String  | 资产的余额                                                                                            |
| unspent   | UTXO[]? | 未使用的UTXO（如果入参fetchUTXO为true）（Teemo暂时不支持）                                             |

#### UTXO结构
| 参数           | 类型    | 说明                                                                  |
|:-------------- |:------ |:--------------------------------------------------------------------- |
| asset          | String | 资产ID                                                                |
| createdAtBlock | String | UTXO创建的块高度                                                       |
| index          | Int    | UTXO创建交易中的output中的排序号                                        |
| txid           | String | UTXO所在交易的txid                                                     |
| value          | String | UTXO的金额                                                             |


## getStorage_获取存储区值

```typescript
Teemo.NEO.getStorage({
    "scriptHash":"03febccf81ac85e3d795bc5cbd4e84e907812aa3",
    "key":"5065746572",
    "network":"TestNet"
})
.then(res => {
  const value = res.result;
  console.log('Storage value: ' + value);
})
.catch(({type: string, description: string, data: any}) => {
  switch(type) {
    case NO_PROVIDER:
      console.log('No provider available.');
      break;
    case CONNECTION_REFUSED:
      console.log('Connection dApp not connected. Please call the "connect" function.');
      break;
    case RPC_ERROR:
      console.log('There was an error when broadcasting this transaction to the network.');
      break;
  }
});
```

> 返回示例

```typescript
{
  result: '4c696e'
}
```


返回合约私有化存储区的值

### 输入参数
| 参数名     | 类型    | 说明                                                          |
|:---------- |:------ |:------------------------------------------------------------ |
| scriptHash | String | 私有化存储区数据所属的合约Hash                                  |
| key        | String | 需要查询的数据所对应的唯一键（需要转化为hex string）              |
| network    | String | 网络类别选择                                                   |

### 成功的返回
| 参数名     | 类型   | 说明                                       |
|:--------- |:------ |:----------------------------------------- |
| result    | String | 私有化存储区数据的hex string表示            |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## invokeRead_只读模拟执行合约调用

```typescript
Teemo.NEO.invokeRead({
  scriptHash: '505663a29d83663a838eee091249abd167e928f5',
  operation: 'calculatorAdd',
  arguments: [
    {
      type: 'integer',
      value: 2
    },
    {
      type: 'integer',
      value: 10
    }
  ],
  network: 'TestNet'
})
.then((result: Object) => {
  console.log('Read invocation result: ' + JSON.stringigy(result));
})
.catch(({type: string, description: string, data: any}) => {
  switch(type) {
    case NO_PROVIDER:
      console.log('No provider available.');
      break;
    case CONNECTION_REFUSED:
      console.log('Connection dApp not connected. Please call the "connect" function.');
      break;
   case RPC_ERROR:
    console.log('There was an error when broadcasting this transaction to the network.');
    break;
  }
});
```

> 返回示例

```typescript
{
  script: '8h89fh398f42f.....89hf2894hf9834',
  state: 'HALT, BREAK',
  gas_consumed: '0.13',
  stack: [
    {
      type: 'Integer',
      value: '1337'
    }
  ]
}
```

以只读模式模拟执行合约方法

### 输入参数   
| 参数名      | 类型       | 说明                                                                           |
|:---------- |:---------- |:------------------------------------------------------------------------------ |
| scriptHash | String     | 需要执行的合约的Hash                                                             |
| operation  | String     | 需要执行的合约的方法名                                                           |
| args       | Argument[] | Argument合约参数组                                                                  |
| network    | String     | 网络类别选择                                                                    |

#### Argument参数结构
| 参数名     | 类型   | 说明                                                       |
|:--------- |:------ |:--------------------------------------------------------- |
| type      | String | 合约参数的类型                                             |
| value     | String | 合约参数的值                                               |

<aside class =notice>
type必须是以下之一： "String"|"Boolean"|"Hash160"|"Hash256"|"Integer"|"ByteArray"|"Array"|"Address"
</aside>

### 成功的返回
结果将从CLI RPC接口直接返回

| 参数名        | 类型       | 说明                                                                                          |
|:------------ |:---------- |:--------------------------------------------------------------------------------------------- |
| script       | String     | 调用合约的脚本                                                                                 |
| state        | String     | 执行状态结果                                                                                   |
| gas_consumed | String     | 预估的系统费需求（不包括写操作的费用）                                                            |
| stack        | Argument[] | 具体的返回数据组                                                                                |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## invokeReadGroup_只读模拟执行合约调用组

```typescript
Teemo.NEO.invokeReadGroup({
  "group":[
      {
          "scriptHash": "fc732edee1efdf968c23c20a9628eaa5a6ccb934",
          "operation": "totalSupply",
          "arguments": [],
          "network": "TestNet"
      },
      {
          "scriptHash": "fc732edee1efdf968c23c20a9628eaa5a6ccb934",
          "operation": "name",
          "arguments": [],
          "network": "TestNet"
      },
      {
          "scriptHash": "fc732edee1efdf968c23c20a9628eaa5a6ccb934",
          "operation": "symbol",
          "arguments":[],
          "network": "TestNet"
      },
      {
          "scriptHash": "fc732edee1efdf968c23c20a9628eaa5a6ccb934",
          "operation": "decimals",
          "arguments": [],
          "network": "TestNet"
      }
  ]
})
.then((result: Object) => {
  console.log('Read invocation result: ' + JSON.stringigy(result));
})
.catch(({type: string, description: string, data: any}) => {
  switch(type) {
    case NO_PROVIDER:
      console.log('No provider available.');
      break;
    case CONNECTION_REFUSED:
      console.log('Connection dApp not connected. Please call the "connect" function.');
      break;
   case RPC_ERROR:
    console.log('There was an error when broadcasting this transaction to the network.');
    break;
  }
});
```

> 返回示例

```typescript
{
  "script": '8h89fh398f42f.....89hf2894hf9834',
  "state": "HALT, BREAK",
  "gas_consumed": "0.648",
  "stack": [
    {
      "type": "ByteArray",
      "value": "00e8764817"
    },
    {
      "type": "ByteArray",
      "value": "4e454f204e616d6520437265646974"
    },
    {
      "type": "ByteArray",
      "value": "4e4e43"
    },
    {
      "type": "Integer",
      "value": "2"
    }
  ]
}
```

以只读模式模拟执行合约方法组，可以一次性获取合约中多个信息，比如一次性获取NEP5所有基本信息

### 输入参数
| 参数名      | 类型             | 说明                                                                           |
|:---------- |:-----------------|:------------------------------------------------------------------------------ |
| group      | invokeRead[]     | invokeRead入参数组                                                              |

#### invokeRead入参结构
| 参数名      | 类型       | 说明                                                                           |
|:---------- |:---------- |:------------------------------------------------------------------------------ |
| scriptHash | String     | 调用的合约Hash                                                                   |
| operation  | String     | 需要执行的合约的方法名                                                           |
| args       | Argument[] | Argument合约参数组                                                                  |
| network    | String     | 网络类别选择                                                                    |

#### Argument参数结构
| 参数名     | 类型   | 说明                                                       |
|:--------- |:------ |:--------------------------------------------------------- |
| type      | String | 合约参数的类型                                             |
| value     | String | 合约参数的值                                               |

<aside class =notice>
type必须是以下之一： "String"|"Boolean"|"Hash160"|"Hash256"|"Integer"|"ByteArray"|"Array"|"Address"
</aside>

### 成功的返回
结果将从CLI RPC接口直接返回

| 参数名        | 类型       | 说明                                                                                          |
|:------------ |:---------- |:--------------------------------------------------------------------------------------------- |
| script       | String     | 调用合约的脚本                                                                              |
| state        | String     | 执行状态结果                                                                                   |
| gas_consumed | String     | 预估的系统费需求（不包括写操作的费用）                                                            |
| stack        | Argument[] | 具体的返回数据组                                                                                |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getBlock_获取块数据

```typescript
Teemo.NEO.getBlock({
            blockHeight:1112,
            network:"TestNet",
        }) 
.then(result=>{
    console.log(result);
    document.getElementById("getBlock_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log(error);
    reject();
})
```

> 返回示例

```typescript
{
  "hash": "0xb4864e0cadb79cedc11f58b2a73e9669b784e6f3eef12dcc834bad6540043c5b",
  "size": 686,
  "version": 0,
  "previousblockhash": "0xe44d7b49ca1849633713eb3266e70d63dd4ce7c17d5ea69a33101cdeb94e2bb3",
  "merkleroot": "0x6e16a374409b9e47fbf6bba2966312b350e8c85137badb99d0991baa3a380513",
  "time": 1494421978,
  "index": 1112,
  "nonce": "ffd3824d7059d835",
  "nextconsensus": "AdyQbbn6ENjqWDa5JNYMwN3ikNcA4JeZdk",
  "script": {
    "invocation": "40bcc2d6213bb9e6b0b2f4ad026fb0e93b26699637d047cdb2bd2cd9625058689b8a9b1db5e9ee4806872465f782b30344ed73d3f106384cfe8001ccd202f33ac640710ed0a89eed529e8ea8193853e3dc7169f1c287f3cd66d5667e720b3b6ba1d60c071a9a2d0d69217f6154953cc4a242d579612eeedd8a1172f73d16f9b66acd40fadd8a94368d184e91fd2949e1cc01d27bbb3590d49330cf88a584163c3c83f9dc7c8d530cc14d7cb83f592cc6eaacfa34d40d777c536ae1bde646413f429925405ef0ba5cf018d84ea857073d19062fb18528506c0ab14540612b939ef09647b227f107002032f38b5f34f085602648927e7080c55bb777559e530b6c2042798640562bcd08de9cb9ac9c72fee14b99b60afa7549fd319e76d76dee4d1ecdd5393454ba4a2009c7454396836b200f3a6ef88b0fc720d741f915185f9adb36d356f8",
    "verification": "55210209e7fd41dfb5c2f8dc72eb30358ac100ea8c72da18847befe06eade68cebfcb9210327da12b5c40200e9f65569476bbff2218da4f32548ff43b6387ec1416a231ee821034ff5ceeac41acf22cd5ed2da17a6df4dd8358fcb2bfb1a43208ad0feaab2746b21026ce35b29147ad09e4afe4ec4a7319095f08198fa8babbe3c56e970b143528d2221038dddc06ce687677a53d54f096d2591ba2302068cf123c1f2d75c2dddc542557921039dafd8571a641058ccc832c5e2111ea39b09c0bde36050914384f7a48bce9bf92102d02b1873a0863cd042cc717da31cea0d7cf9db32b74d4c72c01b0011503e2e2257ae"
  },
  "tx": [
    {
      "txid": "0x6e16a374409b9e47fbf6bba2966312b350e8c85137badb99d0991baa3a380513",
      "size": 10,
      "type": "MinerTransaction",
      "version": 0,
      "attributes": [],
      "vin": [],
      "vout": [],
      "sys_fee": "0",
      "net_fee": "0",
      "scripts": [],
      "nonce": 1884936245
    }
  ],
  "confirmations": 2607282,
  "nextblockhash": "0x2bc8072546971a6b87d154544cd823327efa880e608267de8029b4ce2926593a"
}

```

根据块高度获取区块信息

### 输入参数
| 参数名       | 类型             | 说明                                                                           |
|:----------  |:-----------------|:------------------------------------------------------------------------------ |
| blockHeight | Number           | 块高度                                                                         |
| network     | String           | 网络类别选择                                                                    |

### 成功的返回
结果将从CLI RPC接口直接返回

| 参数名              | 类型       | 说明                             |
|:------------------ |:---------- |:-------------------------------- |
| hash               | String     | 块Hash                           |
| size               | Number     | 块大小（字节）                    |
| version            | Number     | 块执行的版本号                    |
| previousblockhash  | String     | 上一块Hash                       |
| merkleroot         | String     | 默克尔根                         |
| time               | Number     | 块生成时间戳                      |
| index              | Number     | 块索引（高度）                    |
| nonce              | String     | 块伪随机数                        |
| nextconsensus      | String     | 下一个主记账人                    |
| script             | String     | 块调用签名认证信息                |
| tx                 | TX[]       | 块包含的交易组                    |
| confirmations      | Number     | 确认数（此块之后的块数）           |
| nextblockhash      | String     | 下一个块Hash                      |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getTransaction_获取交易信息

```typescript
Teemo.NEO.getTransaction({
            txid:'0c13d46dd72a47b61baf9b14cf787a8325f14cb1bfd5eafb7d407852e53299c6',
            network:"TestNet"
        }) 
.then(result=>{
    console.log(result);
    document.getElementById("getTransaction_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log(error);
    reject();
})
```

> 返回示例

```typescript
{
  "txid": "0x0c13d46dd72a47b61baf9b14cf787a8325f14cb1bfd5eafb7d407852e53299c6",
  "size": 203,
  "type": "InvocationTransaction",
  "version": 0,
  "attributes": [
    {
      "usage": "Script",
      "data": "55d6d86bdec15db437aca45b4e8705333f1fdb07"
    }
  ],
  "vin": [],
  "vout": [],
  "sys_fee": "0",
  "net_fee": "0",
  "scripts": [
    {
      "invocation": "40091fac773afb2a988e62f0652066e0a730b30a943c953b20b2396a0482501a626e34522d2def050c6651c1000cd7b638397f0ab068d878935986e3e027d096fd",
      "verification": "2103ea379c3fa408d71a4021ec77112478f790c698177352d81575328901ae0ee0e4ac"
    }
  ],
  "script": "0233117504201104431455d6d86bdec15db437aca45b4e8705333f1fdb070a736e656f5f707269636553c10873657454797065426797210e7c98582151ceb37f9748c9a1d27d9ae6fd",
  "gas": "0",
  "blockhash": "0x03e55a9ca7e205bf94b0dbf3c22a47af5b0a7d7233cb296067d0121e6cd08229",
  "confirmations": 64041,
  "blocktime": 1555303134
}

```

根据TXID获取交易信息

### 输入参数
| 参数名       | 类型             | 说明                                                                           |
|:----------  |:-----------------|:------------------------------------------------------------------------------ |
| txid        | String           | 交易Hash                                                                       |
| network     | String           | 网络类别选择                                                                    |

### 成功的返回
结果将从CLI RPC接口直接返回

| 参数名              | 类型           | 说明                             |
|:------------------ |:----------     |:-------------------------------- |
| txid               | String         | 交易Hash                         |
| size               | Number         | 交易大小（字节）                  |
| type               | String         | 交易类型                         |
| version            | Number         | 交易执行的版本                    |
| attributes         | attribute[]    | 交易附加属性组                    |
| vin                | UTXOinput[]    | UTXO输入组                       |
| vout               | UTXOouttput[]  | UTXO输出组                       |
| sys_fee            | String         | 交易系统费                        |
| net_fee            | String         | 交易网络费                        |
| scripts            | script[]       | 交易签名认证信息组                |
| script             | String         | 交易调用脚本                     |
| gas                | String         | 交易消耗GAS                       |
| blockhash          | String         | 交易所在块Hash                    |
| confirmations      | Number         | 交易确认数                        |
| blocktime          | Number         | 交易所在块时间戳                   |

<aside class =notice>
TX最后三个属性如果没有返回，代表交易未共识，可以以此判断交易是否已被共识
</aside>

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## getApplicationLog_获取调用交易执行日志

```typescript
Teemo.NEO.getApplicationLog({
            txid:'0c13d46dd72a47b61baf9b14cf787a8325f14cb1bfd5eafb7d407852e53299c6',
            network:"TestNet"
        }) // 获得余额的方法
.then(result=>{
    console.log(result);
    document.getElementById("getApplicationLog_R").innerText = JSON.stringify(result, null, 2);
    resolve();
})
.catch(error=>{
    console.log(error);
    reject();
})
```

> 返回示例

```typescript
{
  "txid": "0x0c13d46dd72a47b61baf9b14cf787a8325f14cb1bfd5eafb7d407852e53299c6",
  "executions": [
    {
      "trigger": "Application",
      "contract": "0xad23d4ccc2e8d4f2747ab15c35f2852761b8df51",
      "vmstate": "HALT, BREAK",
      "gas_consumed": "9.509",
      "stack": [
        {
          "type": "Integer",
          "value": "1"
        }
      ],
      "notifications": [
        {
          "contract": "0xfde69a7dd2a1c948977fb3ce512158987c0e2197",
          "state": {
            "type": "Array",
            "value": [
              {
                "type": "ByteArray",
                "value": "6f7261636c654f70657261746f72"
              },
              {
                "type": "ByteArray",
                "value": "55d6d86bdec15db437aca45b4e8705333f1fdb07"
              },
              {
                "type": "ByteArray",
                "value": "736e656f5f7072696365"
              },
              {
                "type": "ByteArray",
                "value": ""
              },
              {
                "type": "ByteArray",
                "value": "20110443"
              },
              {
                "type": "Integer",
                "value": "2"
              }
            ]
          }
        },
        {
          "contract": "0xfde69a7dd2a1c948977fb3ce512158987c0e2197",
          "state": {
            "type": "Array",
            "value": [
              {
                "type": "ByteArray",
                "value": "6f7261636c654f70657261746f72"
              },
              {
                "type": "ByteArray",
                "value": "55d6d86bdec15db437aca45b4e8705333f1fdb07"
              },
              {
                "type": "ByteArray",
                "value": "736e656f5f7072696365"
              },
              {
                "type": "ByteArray",
                "value": ""
              },
              {
                "type": "Integer",
                "value": "1127710905"
              },
              {
                "type": "Integer",
                "value": "5"
              }
            ]
          }
        }
      ]
    }
  ]
}
```

根据TXID获取调用交易执行日志

### 输入参数
| 参数名       | 类型             | 说明                                                                           |
|:----------  |:-----------------|:------------------------------------------------------------------------------ |
| txid        | String           | 交易Hash                                                                       |
| network     | String           | 网络类别选择                                                                    |

### 成功的返回
结果将从CLI RPC接口直接返回

| 参数名                        | 类型               | 说明                              |
|:---------------------------- |:--------------     |:--------------------------------- |
| txid                         | String             | 交易Hash                          |
| executions                   | execution[]        | 交易执行情组                       |
| executions.trigger           | String             | 交易执行触发器种类                 |
| executions.contract          | String             | 交易执行合约Hash                   |
| executions.vmstate           | String             | 交易执行状态（VM是否中途奔溃）      |
| executions.gas_consumed      | String             | 交易执行耗费GAS                   |
| executions.stack             | TypeValue[]        | 交易执行输出堆栈（合约返回值）      |
| executions.notifications     | notification[]     | 交易执行通知组                    |

#### notification结构
| 参数名                        | 类型               | 说明                              |
|:----------------------------- |:--------------    |:--------------------------------- |
| contract                      | String            | 交易执行通知所在的合约Hash          |
| state                         | TypeValue         | 交易执行通知数据体                  |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |