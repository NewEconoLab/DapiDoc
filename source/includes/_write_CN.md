# Write Methods

Write methods will alter the state on the blockchain, and require a user signature.

## send

```typescript
o3dapi.NEO.send({
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

## Invoke
```typescript
o3dapi.NEO.invoke({
  scriptHash: '505663a29d83663a838eee091249abd167e928f5',
  operation: 'storeData',
  arguments: [
    {
      type: 'string',
      value: 'hello'
    }
  ],
  attachedAssets: {
    NEO: '100',
    GAS: '0.0001',
  },
  fee: '0.001',
  network: 'TestNet',
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
| fee                         | String?              | If a fee is specified then the wallet SHOULD NOT override it, if a fee is not specified the wallet SHOULD allow the user to attach an optional fee |
| network                     | String               | Network alias to submit this request to.                                                                                                           |
| attachedAssets              | AttachedAssets?      | Describes the assets to attach with the smart contract, e.g. attaching assets to mint tokens during a token sale                                   |
| assetIntentOverrides        | AssetIntentOverrides | Used to specify the exact UTXO's to use for attached assets. If this is provided fee and attachedAssets will be ignored                            |
| triggerContractVerification | Boolean?             | Adds the instruction to invoke the contract verifican trigger                                                                                      |

#### Argument
| Parameter | Type   | Description                                               |
|:--------- |:------ |:--------------------------------------------------------- |
| type      | String | The type of the argument with you are using               |
| value     | String | String representation of the argument which you are using |

<aside class =notice>
Available types are "String"|"Boolean"|"Hash160"|"Hash256"|"Integer"|"ByteArray"|"Array"|"Address"
</aside>

#### AttachedAssets
| Parameter | Type    | Description                                            |
|:--------- |:------- |:------------------------------------------------------ |
| NEO       | String? | The amount of NEO to attach to the contract invocation |
| GAS       | String? | The amount of GAS to attach to the contract invocation |

#### AssetIntentOverrides
| Parameter | Type          | Description                                        |
|:--------- |:------------- |:-------------------------------------------------- |
| inputs    | AssetInput[]  | A list of UTXO inputs to use for this transaction  |
| outputs   | AssetOutput[] | A list of UTXO outputs to use for this transaction |

#### AssetInput
| Parameter | Type   | Description                                              |
|:--------- |:------ |:-------------------------------------------------------- |
| txid      | String | Transaction id to be used as input                       |
| index     | String | Index of the UTXO, can be found from transaction details |

#### AssetOutput
| Parameter | Type   | Description                                                           |
|:--------- |:------ |:--------------------------------------------------------------------- |
| asset     | String | A list of UTXO inputs to use for this transaction                     |
| address   | String | A list of UTXO outputs to use for this transaction                    |
| value     | String | String representation of double or integer value to be used as output |


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


## signMessage

```typescript
o3dapi.NEO.signMessage({
  message: 'Hello World!',
})
.then((signedMessage: SignedMessage) => {
  const {
    publicKey,
    message,
    salt,
    data,
  } = signedMessage;

  console.log('Public key used to sign:', publicKey);
  console.log('Original message:', message);
  console.log('Salt added to message:', salt);
  console.log('Signed data:', data);
})
.catch(({type: string, description: string, data: any}) => {
  switch(type) {
    case UNKNOWN_ERROR:
      console.log(description);
      break;
  }
});
```

> Example Response

```typescript
{
  publicKey: '0241392007396d6ef96159f047967c5a61f1af0871ecf9dc82afbedb68afbb949a',
  data: '0147fb89d0999e9d8a90edacfa26152fe695ec8b3770dcad522048297ab903822e12472364e254ff2e088fc3ebb641cc24722c563ff679bb1d1623d08bd5863d0d',
  salt: '058b9e03e7154e4db1e489c99256b7fa',
  message: 'Hello World!',
}
```

Signs a provided messaged with an account selected by user. A salt prefix is added to the input string, and provided as a part of the data while signing. In the example, the signed value would be `058b9e03e7154e4db1e489c99256b7faHello World!`.

### Input Arguments

| Parameter | Type   | Description         |
|:--------- |:------ |:------------------- |
| message   | String | The message to sign |

### Success Response

| Parameter | Type   | Description                                                  |
|:--------- |:------ |:------------------------------------------------------------ |
| publicKey | String | The public key used to sign message                          |
| data      | String | The signed data                                              |
| salt      | String | The salt prefix added to the original message before signing |
| message   | String | The original message                                         |


### Error Response

| Parameter   | Type    | Description                                   |
|:----------- |:------- |:--------------------------------------------- |
| type        | String  | The type of error which has occurred          |
| description | String  | A description of the error which has occurred |
| data        | String? | Any raw data associated with the error        |
