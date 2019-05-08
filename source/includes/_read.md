# Read Methods

Read methods do not alter the state of the blockchain. It can help you query information about your user, and provide you with relevant information:

## getProvider

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

> Example Response

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

Returns information about the dAPI provider, including who this provider is, the version of their dAPI, and the NEP that the interface is compatible with.

### Input Arguments

None

### Success Response
| Parameter     | Type     | Description                                                      |
|:------------- |:-------- |:---------------------------------------------------------------- |
| name          | String   | The name of the wallet provider                                  |
| website       | String   | The website of the wallet provider                               |
| version       | String   | The version of the dAPI that the the wallet supports             |
| compatibility | String[] | A list of all applicable NEPs which the wallet provider supports |
| extra         | Object   | Provider specific attributes                                     |

##### extra
| Parameter | Type   | Description               |
| --------- | ------ | ------------------------- |
| theme     | string | UI theme of the provider  |
| currency  | string | Base currency set by user |

### Error Response
| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## getNetworks

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

> Example Response

```typescript
{
  networks: ["TestNet", "TestNet", "PrivateNet"],
  defaultNetwork: "TestNet",
}
```

Returns the networks the wallet provider has available to connect to, along with the default network the wallet is currently set to.

### Input Arguments

None

### Success Response

| Parameter      | Type     | Description                                                        |
|:-------------- |:-------- |:------------------------------------------------------------------ |
| networks       | String[] | A list of all networks which this wallet provider allows access to |
| defaultNetwork | String   | Network the wallet is currently set to                             |

### Error Response
| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |


## getAccount

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

> Example Response

```typescript
{
  address: 'AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru',
  label: 'My Spending Wallet'
}
```

Return the Account that is currently connected to the dApp.

### Success Response
| Parameter | Type   | Description                                                        |
|:--------- |:------ |:------------------------------------------------------------------ |
| address   | String | The address of the account that is currently connected to the dapp |
| label     | String | A label the users has set to identify their wallet                 |

### Error Response
| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## getBalance

```typescript
Teemo.NEO.getBalance({
  params:{  
    "address": "ASBhJFN3XiDu38EdEQyMY3N2XwGh1gd5WW",  
    "assets": ["74f2dc36a68fdc4682034178eb2220729231db76"]
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

> Single Address with specific balances requested

```typescript
// input
{
  params: {
    address: 'AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru',
    assets: ["602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee7969282de7"]
  },
  network: 'TestNet',
}

// output
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

> Single Address with all balances requested

```typescript
// input
{
  params: {
    address: 'AeysVbKWiLSuSDhg7DTzUdDyYYKfgjojru',
  },
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


Allows the DAPP to query the balance of a user, this includes both native assets (NEO/GAS) and NEP-5 tokens

### Input Arguments
| Parameter | Type                               | Description                                                                              |
|:--------- |:---------------------------------- |:---------------------------------------------------------------------------------------- |
| params    | BalanceRequest or BalanceRequest[] | A list of Balance Request Objects, specifying which addresses, and which assets to query |
| network   | String                             | The call will only work for the networks available in the GetNetworks command            |

#### Balance Request
| Parameter  | Type     | Description                                                                                   |
|:---------- |:-------- |:--------------------------------------------------------------------------------------------  |
| address    | String   | The address whose balance you want to query                                                   |
| assets     | String[] | A list of contract hash (or symbold on TestNet only) to query the balance for                 |
| fetchUTXO? | boolean  | The response will fetch NEO and GAS UTXO's if this attribute is true(Teemo Not yet supported)|

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
| Parameter | Type    | Description                                                                                                                   |
|:--------- |:------- |:----------------------------------------------------------------------------------------------------------------------------- |
| assetID   | String  | ID of the given asset                                                                                                         |
| symbol    | String  | Symbol of the given asset                                                                                                     |
| amount    | String  | Double Value of the balance represented as a String                                                                           |
| unspent   | UTXO[]? | If fetch utxo's was turned on then the utxo array will be returned for the native assets NEO and GAS(Teemo Not yet supported)|

#### UTXO (Teemo Not yet supported)
| Parameter      | Type   | Description                                                           |
|:-------------- |:------ |:--------------------------------------------------------------------- |
| asset          | String | Script hash of the native asset                                       |
| createdAtBlock | String | Block number where this utxo was created                              |
| index          | Int    | Output index of the UTXO relative to the txid in which it was created |
| txid           | String | The transaction id of this UTXO                                       |
| value          | String | The double value of this UTXO represented as a String                 |

## getStorage

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

> Example Response

```typescript
{
  result: '4c696e'
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
| network    | String     | Network alias to submit this request to. If omitted, will default to "TestNet" |

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

## invokeReadGroup

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

> Example Response

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

Execute a contract invocation group in read-only mode.You can get multiple information in the contract at one time, such as getting all the basic information of NEP5 at one time.

### Input Parameters
| Parameter  | Type             | Description                                                                    |
|:---------- |:-----------------|:------------------------------------------------------------------------------ |
| group      | invokeRead[]     | An array of invokeRead input arguments                                         |

#### invokeRead
| Parameter  | Type       | Description                                                                    |
|:---------- |:---------- |:------------------------------------------------------------------------------ |
| scriptHash | String     | The script hash of the contract you want to invoke a read on                   |
| operation  | String     | The operation on the smart contract that you want to invoke a read on          |
| args       | Argument[] | The input arguments necessary to perform this operation                        |
| network    | String     | Network alias to submit this request to. If omitted, will default to "TestNet" |

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

## getBlock

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

> Example Response

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

Get block information based on block height

### Input Parameters
| Parameter   | Type             | Description                       |
|:----------  |:-----------------|:----------------------------------|
| blockHeight | Number           | Block height                      |
| network     | String           | Network category selection        |

### Success Response
The result will be returned directly from the CLI RPC interface

| Parameter          | Type       | Description                                             |
|:------------------ |:---------- |:------------------------------------------------------- |
| hash               | String     | Block hash                                              |
| size               | Number     | Block size (bytes)                                      |
| version            | Number     | The version number of the block execution               |
| previousblockhash  | String     | Previous block Hash                                     |
| merkleroot         | String     | Merkel root                                             |
| time               | Number     | Block generation timestamp                              |
| index              | Number     | Block index (height)                                    |
| nonce              | String     | Block pseudo-random number                              |
| nextconsensus      | String     | Next master biller                                      |
| script             | String     | Block call signature authentication information         |
| tx                 | TX[]       | Block containing trading group                          |
| confirmations      | Number     | Confirmation number (number of blocks after this block) |
| nextblockhash      | String     | Next block hash                                         |

### Error Response
| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## getTransaction

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

> Example Response

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

Get transaction information based on TXID

### Input Parameters
| Parameter   | Type             | Description                      |
|:----------  |:-----------------|:---------------------------------|
| txid        | String           | Transaction Hash                 |
| network     | String           | Network category selection       |

### Success Response
The result will be returned directly from the CLI RPC interface

| Parameter          | Type           | Description                                               |
|:------------------ |:----------     |:--------------------------------------------------------- |
| txid               | String         | Transaction Hash                                          |
| size               | Number         | Transaction Size (bytes)                                  |
| type               | String         | Transaction Type                                          |
| version            | Number         | Transaction Version                                       |
| attributes         | attribute[]    | Transaction Additional attribute group                    |
| vin                | UTXOinput[]    | UTXO Inputs                                               |
| vout               | UTXOouttput[]  | UTXO Outputs                                              |
| sys_fee            | String         | Transaction System fee                                    |
| net_fee            | String         | Transaction Net fee                                       |
| scripts            | script[]       | Transaction Signature authentication information group    |
| script             | String         | Transaction Invoke script                                 |
| gas                | String         | Transaction Consume GAS                                   |
| blockhash          | String         | Transaction Block Hash                                    |
| confirmations      | Number         | Transaction Block Confirmation number                     |
| blocktime          | Number         | Transaction Block timestamp                               |

<aside class =notice>
If the last three attributes of TX are not returned, it means that the transaction has no consensus, so you can judge whether the transaction has been agreed.
</aside>

### Error Response
| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## getApplicationLog

```typescript
Teemo.NEO.getApplicationLog({
            txid:'0c13d46dd72a47b61baf9b14cf787a8325f14cb1bfd5eafb7d407852e53299c6',
            network:"TestNet"
        }) 
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

> Example Response

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

Get the transaction execution log according to the TXID

### Input Parameters
| Parameter   | Type             | Description                      |
|:----------  |:-----------------|:---------------------------------|
| txid        | String           | Transaction Hash                 |
| network     | String           | Network category selection       |

### Success Response
The result will be returned directly from the CLI RPC interface

| Parameter                    | Type               | Description                                                   |
|:---------------------------- |:--------------     |:------------------------------------------------------------- |
| txid                         | String             | Transaction Hash                                              |
| executions                   | execution[]        | Transaction execution Array                                   |
| executions.trigger           | String             | Transaction execution trigger type                            |
| executions.contract          | String             | Transaction execution Script Hash                             |
| executions.vmstate           | String             | Transaction execution VM state(Is the VM crashing halfway)    |
| executions.gas_consumed      | String             | Transaction execution Consume GAS                             |
| executions.stack             | TypeValue[]        | Transaction execution Output stack (contract return value)    |
| executions.notifications     | notification[]     | Transaction execution notification Array                      |

#### notification structure
| Parameter          | Type              | Description                                                     |
|:------------------ |:--------------    |:--------------------------------------------------------------- |
| contract           | String            | Contract Hash where the transaction execution notice is located |
| state              | TypeValue         | Transaction execution notification data                         |

### Error Response
| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |