# 读类型方法

读方法不改变链数据. 它帮助你为你的用户查询信息, 也为你的Dapp提供必要的信息:

## getProvider_获得提供者信息

```typescript
Teemmo.NEO.getProvider()
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
  "name": "TeemmoWallet",
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
Teemmo.NEO.getAccount()
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
Teemmo.NEO.getBalance({
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
| fetchUTXO? | boolean  | 这是一个可选的参数，表示是否需要返回UTXO（Teemmo暂时不支持）          |

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
| unspent   | UTXO[]? | 未使用的UTXO（如果入参fetchUTXO为true）（Teemmo暂时不支持）                                             |

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
Teemmo.NEO.getStorage({
  scriptHash: '505663a29d83663a838eee091249abd167e928f5',
  key: 'game.status',
  network: 'TestNet'
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
  result: 'hello world'
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
Teemmo.NEO.invokeRead({
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
Teemmo.NEO.invokeReadGroup({
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