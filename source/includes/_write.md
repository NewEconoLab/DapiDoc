# Write Methods

Write methods will alter the state on the blockchain, and require a user signature.

## send_EN

```typescript
Teemmo.NEO.send({
  fromAddress: 'ATaWxfUAiBcQixNPq6TvKHEHPQum9bx79d',
  toAddress: 'ATaWxfUAiBcQixNPq6TvKHEHPQum9bx79d',
  asset: 'GAS',
  amount: '0.0001',
  remark: 'Hash puppy clothing purchase. Invoice#abc123',
  fee: '0.0001',
  network: 'MainNet'
})
.then(({txid, nodeUrl}: SendOutput) => {
  console.log('Send transaction success!');
  console.log('Transaction ID: ' + txid);
  console.log('RPC node URL: ' + nodeUrl);
})
.catch(({type: string, description: string, data: any}) => {
  switch(type) {
    case NO_PROVIDER:
      console.log('No provider available.');
      break;
    case SEND_ERROR:
      console.log('There was an error when broadcasting this transaction to the network.');
      break;
    case MALFORMED_INPUT:
      console.log('The receiver address provided is not valid.');
      break;
    case CANCELED:
      console.log('The user has canceled this transaction.');
      break;
    case INSUFFICIENT_FUNDS:
      console.log('The user has insufficient funds to execute this transaction.');
      break;
  }
});
```

> Example Response

```typescript
{
  txid: 'ed54fb38dff371be6e3f96e4880405758c07fe6dd1295eb136fe15f311e9ff77',
  nodeUrl: 'http://seed7.ngd.network:10332',
}
```

The send API can be used for accepting payments from the user in a cryptocurrency that is located on the NEO blockchain. It requires user authentication in order for the transaction to be relayed. The transaction will be relayed by the wallet.

### Input Arguments
| Parameter   | Type    | Description                                                                                                                                        |
|:----------- |:------- |:-------------------------------------------------------------------------------------------------------------------------------------------------- |
| fromAddress | String  | The address from where the transaction is being sent. This will be the same value as the one received from the getAccount API                      |
| toAddress   | String  | The address to where the user should send their funds                                                                                              |
| asset       | String  | The asset which is being requested for payment...e.g NEP5 scripHash, GAS or CGAS                                                                   |
| amount      | String  | The amount which is being requested for payment                                                                                                    |
| remark      | String? | A transaction attribute remark which may be placed in the transaction, this data will appear in the transaction record on the blockchain           |
| fee         | String? | If a fee is specified then the wallet SHOULD NOT override it, if a fee is not specified the wallet SHOULD allow the user to attach an optional fee |
| network     | String  | Network alias to submit this request to.                                                                                                           |

### Success Response
| Parameter | Type   | Description                                                                   |
|:--------- |:------ |:----------------------------------------------------------------------------- |
| txid      | String | The transaction id of the send request which can be queried on the blockchain |
| nodeURL   | String | The node to which the transaction was submitted to                            |

<aside class="warning">
It is reccommended that the DAPP take appropriate levels of risk prevention when accepting transactions. The dapp can query the mempool of a known node to ensure that the transaction will indeed be broadcast on the network.
</aside>

### Error Response
| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## Invoke_EN
```typescript
Teemmo.NEO.invoke({
  "scriptHash":"74f2dc36a68fdc4682034178eb2220729231db76",
  "operation":"transfer",
  "arguments":[
      {
        "type":"Address",
        "value":"AHDV7M54NHukq8f76QQtBTbrCqKJrBH9UF"
      },
      {
        "type":"Address",
        "value":"AbU7BUQHW9sa69pTac7pPR3cq4gQHYC1DH"
      },
      {
        "type":"Integer",
        "value":"100000"
      }
  ],
  "fee":"0.001",
  "description":"NNC transfer",
  "network":"TestNet"
})
.then(({txid, nodeUrl}: InvokeOutput) => {
  console.log('Invoke transaction success!');
  console.log('Transaction ID: ' + txid);
  console.log('RPC node URL: ' + nodeUrl);
})
.catch(({type: string, description: string, data: any}) => {
  switch(type) {
    case NO_PROVIDER:
      console.log('No provider available.');
      break;
    case RPC_ERROR:
      console.log('There was an error when broadcasting this transaction to the network.');
      break;
    case CANCELED:
      console.log('The user has canceled this transaction.');
      break;
  }
});
```

> Example Response

```typescript
{
  txid: 'ed54fb38dff371be6e3f96e4880405758c07fe6dd1295eb136fe15f311e9ff77',
  nodeUrl: 'http://seed7.ngd.network:10332',
}:
```

Invoke allows for the generic execution of smart contracts on behalf of the user. It is reccommended to have a general understanding of the NEO blockchain, and to be able successfully use all other commands listed previously in this document before attempting a generic contract execution.

### Input arguments
| Parameter                   | Type                 | Description                                                                                                                                        |
|:--------------------------- |:-------------------- |:-------------------------------------------------------------------------------------------------------------------------------------------------- |
| scriptHash                  | String               | The script hash of the contract that you wish to invoke                                                                                            |
| operation                   | String               | The operation on the smart contract that you wish to call. This can be fetched from the contract ABI                                               |
| args                        | Argument[]           | A list of arguments necessary to perform on the operation you wish to call                                                                         |
| description                 | String               | For a brief description of the operation, the description will be displayed on the confirmation page of the wallet to help the user understand (the description will not be stored on the Blockchain) |
| fee                         | String?              | If a fee is specified then the wallet SHOULD NOT override it, if a fee is not specified the wallet SHOULD allow the user to attach an optional fee |
| network                     | String               | Network alias to submit this request to.                                                                                                           |
| attachedAssets              | AttachedAssets?      | Describes the assets to attach with the smart contract, e.g. attaching assets to mint tokens during a token sale(Teemmo Not yet supported)                                   |
| assetIntentOverrides        | AssetIntentOverrides | Used to specify the exact UTXO's to use for attached assets. If this is provided fee and attachedAssets will be ignored(Teemmo Not yet supported)                            |
| triggerContractVerification | Boolean?             | Adds the instruction to invoke the contract verifican trigger(Teemmo Not yet supported)                                                                                      |

#### Argument
| Parameter | Type   | Description                                               |
|:--------- |:------ |:--------------------------------------------------------- |
| type      | String | The type of the argument with you are using               |
| value     | String | String representation of the argument which you are using |

<aside class =notice>
Available types are "String"|"Boolean"|"Hash160"|"Hash256"|"Integer"|"ByteArray"|"Array"|"Address"
</aside>

#### AttachedAssets(Teemmo Not yet supported)
| Parameter | Type    | Description                                            |
|:--------- |:------- |:------------------------------------------------------ |
| NEO       | String? | The amount of NEO to attach to the contract invocation |
| GAS       | String? | The amount of GAS to attach to the contract invocation |

#### AssetIntentOverrides(Teemmo Not yet supported)
| Parameter | Type          | Description                                        |
|:--------- |:------------- |:-------------------------------------------------- |
| inputs    | AssetInput[]  | A list of UTXO inputs to use for this transaction  |
| outputs   | AssetOutput[] | A list of UTXO outputs to use for this transaction |

#### AssetInput(Teemmo Not yet supported)
| Parameter | Type   | Description                                              |
|:--------- |:------ |:-------------------------------------------------------- |
| txid      | String | Transaction id to be used as input                       |
| index     | String | Index of the UTXO, can be found from transaction details |

#### AssetOutput(Teemmo Not yet supported)
| Parameter | Type   | Description                                                           |
|:--------- |:------ |:--------------------------------------------------------------------- |
| asset     | String | asset ID                                                              |
| value     | String | asset amount                                                          |
| address   | String | The address to where the user should send their funds                 |

### Success Response
| Parameter | Type   | Description                                                                   |
|:--------- |:------ |:----------------------------------------------------------------------------- |
| txid      | String | The transaction id of the send request which can be queried on the blockchain |
| nodeURL   | String | The node to which the transaction was submitted to                            |

### Error Response
| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |

## InvokeGroup_EN
```typescript
Teemmo.NEO.invokeGroup({
  "merge": true,
  "group": [
    {
      "scriptHash": "74f2dc36a68fdc4682034178eb2220729231db76",
      "operation": "transfer",
      "arguments": [
        {
          "type": "Address",
          "value": "AHDV7M54NHukq8f76QQtBTbrCqKJrBH9UF"
        },
        {
          "type": "Address",
          "value": "AbU7BUQHW9sa69pTac7pPR3cq4gQHYC1DH"
        },
        {
          "type": "Integer",
          "value": "100000"
        }
      ],
      "description": "NNC transfer",
      "fee": "0",
      "network": "TestNet"
    },
    {
      "scriptHash": "00d00d0ac467a5b7b2ad04052de154bb9fe8c2ff",
      "operation": "setmoneyin",
      "arguments": [
        {
          "type": "Hook_Txid",
          "value": 0
        },
        {
          "type": "Hash160",
          "value": "74f2dc36a68fdc4682034178eb2220729231db76"
        }
      ],
      "description": "Recharge confirmation",
      "fee": "0.001",
      "network": "TestNet"
    }
  ]
})
.then(({txid, nodeUrl}: InvokeOutput) => {
  console.log('Invoke transaction success!');
  console.log('Transaction ID: ' + txid);
  console.log('RPC node URL: ' + nodeUrl);
})
.catch(({type: string, description: string, data: any}) => {
  switch(type) {
    case NO_PROVIDER:
      console.log('No provider available.');
      break;
    case RPC_ERROR:
      console.log('There was an error when broadcasting this transaction to the network.');
      break;
    case CANCELED:
      console.log('The user has canceled this transaction.');
      break;
  }
});
```

> Example Response

```typescript
//merge=true
[
  {
    "txid": "0cba61b8779f3343ff39fbecee596395b06490bce7245412ee788c8620813b63",
    "nodeUrl": "https://api.nel.group/api"
  }
]

//merge=false
[
  {
    "txid": "045e1612d2dd2edf29d6bb45657329bacc299072655691f9cc0f486fb649a30d",
    "nodeUrl": "https://api.nel.group/api"
  },
  {
    "txid": "4747e2b50cbd853c13b24e67d59225816f152a13ee281345751eb51bf811355c",
    "nodeUrl": "https://api.nel.group/api"
  }
]
```

This method constructs a contract to call a transaction group and sends it, and has two modes: merge mode and non-merge mode:

Merge mode: The wallet will construct each Invoke contract call operation to be entered in order into **a single** contract invoke transaction body, enabling multiple contract methods to be executed in one transaction. This mode is suitable for small transactions (less costly GAS) for multiple different contracts to be merged into one transaction. Regarding the merge mode, it needs to be paid attention to whether the GAS consumption of the script execution will surpass 10GAS after the merge. In the merge mode, the total fee of the transaction is the sum of the fees in each Invoke element.

Non-merge mode: The wallet will construct each Inovke contract call operation to be entered in order as **multiple independent** contract invoke transaction bodies. Only after the previous transaction is confirmed as consensus (confirmed on the blockchain), the next transaction will be sent. This mode is suitable for a series of associated contract operations, and is automatically lined up and executed in a logical relationship.

Hook_Txid: This type is specific to the InvokeGroup method, which means that the txid of the previous transaction is used as the input parameter of the next transaction, and both the merge mode and the non-merge mode are supported. However, there are the following restrictions:
- Hook_Txid cannot appear in the first Invoke element
- The value of Hook_Txid must be 0, meaning that only the txid of the first transaction can be taken.
- When using Hook_Txid, you can only define up to two Invoke elements in the merge mode.

### Input arguments
| Parameter | Type     | Description                                               |
|:--------- |:------   |:--------------------------------------------------------- |
| merge     | Boolean  | Choose to use merge mode                                  |
| group     | Invoke[] | A list of Invoke                                          |

#### Invoke
| Parameter                   | Type                 | Description                                                                                                                                        |
|:--------------------------- |:-------------------- |:-------------------------------------------------------------------------------------------------------------------------------------------------- |
| scriptHash                  | String               | The script hash of the contract that you wish to invoke                                                                                            |
| operation                   | String               | The operation on the smart contract that you wish to call. This can be fetched from the contract ABI                                               |
| args                        | Argument[]           | A list of arguments necessary to perform on the operation you wish to call                                                                         |
| description                 | String               | For a brief description of the operation, the description will be displayed on the confirmation page of the wallet to help the user understand (the description will not be stored on the Blockchain) |
| fee                         | String?              | If a fee is specified then the wallet SHOULD NOT override it, if a fee is not specified the wallet SHOULD allow the user to attach an optional fee |
| network                     | String               | Network alias to submit this request to.                                                                                                           |
| attachedAssets              | AttachedAssets?      | Describes the assets to attach with the smart contract, e.g. attaching assets to mint tokens during a token sale(Teemmo Not yet supported)                                   |
| assetIntentOverrides        | AssetIntentOverrides | Used to specify the exact UTXO's to use for attached assets. If this is provided fee and attachedAssets will be ignored(Teemmo Not yet supported)                            |
| triggerContractVerification | Boolean?             | Adds the instruction to invoke the contract verifican trigger(Teemmo Not yet supported)                                                                                      |

##### Argument
| Parameter | Type   | Description                                               |
|:--------- |:------ |:--------------------------------------------------------- |
| type      | String | The type of the argument with you are using               |
| value     | String | String representation of the argument which you are using |

<aside class =notice>
Available types are "String"|"Boolean"|"Hash160"|"Hash256"|"Integer"|"ByteArray"|"Array"|"Address"|"Hook_Txid"
</aside>

### Success Response
| Parameter | Type   | Description                                                                   |
|:--------- |:------ |:----------------------------------------------------------------------------- |
| []        | Array  | A List of Invoke Response|

#### Invoke Response
| Parameter | Type   | Description                                                                   |
|:--------- |:------ |:----------------------------------------------------------------------------- |
| txid      | String | The transaction id of the send request which can be queried on the blockchain |
| nodeURL   | String | The node to which the transaction was submitted to                            |

### Error Response
| Parameter   | Type    | Description                                  |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | The type of error which has occured          |
| description | String? | A description of the error which has occured |
| data        | String? | Any raw data associated with the error       |