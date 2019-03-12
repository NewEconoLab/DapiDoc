# 读类型方法

读方法不改变链数据. 它帮助你为你的用户查询信息, 也为你的Dapp提供必要的信息:

## getProvider 获得提供者信息

```typescript
o3dapi.NEO.getProvider()
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
  name: 'Awesome Wallet',
  website: 'https://www.awesome.com',
  version: 'v0.0.1',
  compatibility: [
    'NEP-14',
    'NEP-23',
    'NEP-29'
  ],
  extra: {
    theme: 'Dark Mode',
    currency: 'USD',
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

## getNetworks 获取网络参数

```typescript
dapi.NEO.getNetworks()
.then(response => {
  const {
    networks,
    defaultNetwork,
  } = response.networks;

  console.log('Networks: ' + networks);
  // eg. ["MainNet", "TestNet", "PrivateNet"]

  console.log('Default network: ' + defaultNetwork);
  // eg. "MainNet"
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
  networks: ["MainNet", "TestNet", "PrivateNet"],
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


## getAccount 获取账户信息

```typescript
o3dapi.NEO.getAccount()
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

## getBalance

```typescript
o3dapi.NEO.getBalance({
  params: {
    address: 'AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru',
    assets: ['NKN']
  },
  network: 'MainNet',
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

> Single Address with specific balances requested

```typescript
// input
{
  params: {
    address: 'AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru',
    assets: ['NKN']
  },
  network: 'MainNet',
}

// output
{
  AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru: [
    {
      assetID: 'c36aee199dbba6c3f439983657558cfb67629599',
      symbol: 'NKN',
      amount: '0.00000233',
    }
  ],
}
```

> Single Address with all balances requested

```typescript
// input
{
  params: {
    address: 'AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru',
  },
  network: 'MainNet',
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
      assetID: 'c36aee199dbba6c3f439983657558cfb67629599',
      symbol: 'NKN',
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

> Multiple address balance queries

```typescript
// input
{
  params: [
    {
      address: 'AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru',
    },
    {
      address: 'AbKNY45nRDy6B65YPVz1B6YXiTnzRqU2uQ',
      asset: 'PHX',
    },
  ],
  network: 'MainNet',
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
      assetID: 'c36aee199dbba6c3f439983657558cfb67629599',
      symbol: 'NKN',
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
      assetID: '1578103c13e39df15d0d29826d957e85d770d8c9',
      symbol: 'PHX',
      amount: '11000',
    }
  ]
}
```


Allows the DAPP to query the balance of a user, this includes both native assets (NEO/GAS) and NEP-5 tokens

### Input Arguments
| Parameter | Type                               | Description                                                                              |
|:--------- |:---------------------------------- |:---------------------------------------------------------------------------------------- |
| params    | BalanceRequest or BalanceRequest[] | A list of Balance Request Objects, specifying which addresses, and which assets to query |
| network   | String                             | The call will only work for the networks available in the GetNetworks command            |

#### Balance Request
| Parameter  | Type     | Description                                                                   |
|:---------- |:-------- |:----------------------------------------------------------------------------- |
| address    | String   | The address whose balance you want to query                                   |
| assets     | String[] | A list of contract hash (or symbold on MainNet only) to query the balance for |
| fetchUTXO? | boolean  | The response will fetch NEO and GAS UTXO's if this attribute is true          |

### Success Response
| Parameter | Type              | Description                                                                          |
|:--------- |:----------------- |:------------------------------------------------------------------------------------ |
| address_1 | BalanceResponse[] | This key is the actual address of the query eg. "AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru" |
| address_2 | BalanceResponse[] | This key is the actual address of the query eg. "AbKNY45nRDy6B65YPVz1B6YXiTnzRqU2uQ" |
| address_n | BalanceResponse[] | This key is the actual address of the query eg. "AUdawyrLMskxXMUE8osX9mSLKz8R7777kE" |

<aside class="notice">
The amount of addresses is n where n is the number of addresses specified in your query
</aside>


#### BalanceResponse
| Parameter | Type    | Description                                                                                          |
|:--------- |:------- |:---------------------------------------------------------------------------------------------------- |
| assetID   | String  | ID of the given asset                                                                                |
| symbol    | String  | Symbol of the given asset                                                                            |
| amount    | String  | Double Value of the balance represented as a String                                                  |
| unspent   | UTXO[]? | If fetch utxo's was turned on then the utxo array will be returned for the native assets NEO and GAS |

#### UTXO
| Parameter      | Type   | Description                                                           |
|:-------------- |:------ |:--------------------------------------------------------------------- |
| asset          | String | Script hash of the native asset                                       |
| createdAtBlock | String | Block number where this utxo was created                              |
| index          | Int    | Output index of the UTXO relative to the txid in which it was created |
| txid           | String | The transaction id of this UTXO                                       |
| value          | String | The double value of this UTXO represented as a String                 |


## getStorage

```typescript
o3dapi.NEO.getStorage({
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

> Example Response

```typescript
{
  result: 'hello world'
}
```


Returns the raw value located in contract storage

### Input Arguments
| Parameter  | Type   | Description                                                  |
|:---------- |:------ |:------------------------------------------------------------ |
| scriptHash | String | Scripthash of the contract whose storage you are querying on |
| key        | String | Key of the storage value to retrieve from the contract       |
| network    | String | Network alias to submit this request to.                     |

### Success Response
| Parameter | Type   | Description                               |
|:--------- |:------ |:----------------------------------------- |
| result    | String | The raw value located in contract storage |

### Error Response
| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## invokeRead

```typescript
o3dapi.NEO.invokeRead({
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
  network: 'PrivNet'
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

> Example Response

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

Execute a contract invocation in read-only mode.

### Input Arguments
| Parameter  | Type       | Description                                                                    |
|:---------- |:---------- |:------------------------------------------------------------------------------ |
| scriptHash | String     | The script hash of the contract you want to invoke a read on                   |
| operation  | String     | The operation on the smart contract that you want to invoke a read on          |
| args       | Argument[] | The input arguments necessary to perform this operation                        |
| network    | String     | Network alias to submit this request to. If omitted, will default to "MainNet" |

#### Argument
| Parameter | Type   | Description                                               |
|:--------- |:------ |:--------------------------------------------------------- |
| type      | String | The type of the argument with you are using               |
| value     | String | String representation of the argument which you are using |

<aside class =notice>
Available types are "String"|"Boolean"|"Hash160"|"Hash256"|"Integer"|"ByteArray"|"Array"|"Address"
</aside>

### Success Response
The wallet will return the direct response from the RPC node.

| Parameter    | Type       | Description                                                                                   |
|:------------ |:---------- |:--------------------------------------------------------------------------------------------- |
| script       | String     | The script which was run                                                                      |
| state        | String     | Status of the executeion                                                                      |
| gas_consumed | String     | Estimated amount of GAS to be used to execute the invocation. (Up to 10 free per transaction) |
| stack        | Argument[] | An array of response arguments                                                                |

### Error Response
| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |


## verifyMessage

```typescript
o3dapi.NEO.verifyMessage({
  message: '058b9e03e7154e4db1e489c99256b7faHello World!',
  data: '0147fb89d0999e9d8a90edacfa26152fe695ec8b3770dcad522048297ab903822e12472364e254ff2e088fc3ebb641cc24722c563ff679bb1d1623d08bd5863d0d',
  publicKey: '0241392007396d6ef96159f047967c5a61f1af0871ecf9dc82afbedb68afbb949a',
})
.then(({result: bool}) => {
  console.log('Signature data matches provided message and public key: ' + result);
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

> Example Response

```typescript
{
  result: true,
}
```

Returns whether the provided signature data matches the provided message and was signed by the account of the provided public key.

### Input Arguments

| Parameter | Type   | Description                                            |
|:--------- |:------ |:------------------------------------------------------ |
| message   | String | The original signed message                            |
| data      | String | The signature data                                     |
| publicKey | String | The public key of the account used to sign the message |

### Success Response

| Parameter | Type    | Description                                                                |
|:--------- |:------- |:-------------------------------------------------------------------------- |
| result    | Boolean | Whether the provided signature matches the provided message and public key |

### Error Response
| Parameter   | Type    | Description                                   |
|:----------- |:------- |:--------------------------------------------- |
| type        | String  | The type of error which has occurred          |
| description | String  | A description of the error which has occurred |
| data        | String? | Any raw data associated with the error        |
