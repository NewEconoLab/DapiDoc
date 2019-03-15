# 写类型方法

写类型方法会修改区块状态，并且需要用户私钥签名（需要用户显式允许）

## send 发送转账交易

```typescript
Teemmo.NEO.send({
  fromAddress: 'ATaWxfUAiBcQixNPq6TvKHEHPQum9bx79d',
  toAddress: 'ATaWxfUAiBcQixNPq6TvKHEHPQum9bx79d',
  asset: '602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee7969282de7',
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

> 示例返回

```typescript
{
  txid: 'ed54fb38dff371be6e3f96e4880405758c07fe6dd1295eb136fe15f311e9ff77',
  nodeUrl: 'http://seed7.ngd.network:10332',
}
```

本方法可以用于在NEO区块链上进行转账操作，同时支持全局资产（UTXO资产）和合约资产（NEP-5资产），该方法需要用户确认（会有一个确认页面弹出）。

### 输入参数
| 参数        | 类型     | 说明                                                                                                                                               |
|:----------- |:------- |:-------------------------------------------------------------------------------------------------------------------------------------------------- |
| fromAddress | String  | 转账的发送方地址。需要用此地址的私钥进行签名（用户拥有此地址）                                                                                           |
| toAddress   | String  | 转账的接收方地址。                                                                                                                                   |
| asset       | String  | 资产的ID，如GAS为602c79718b16e442de58778e148d0b1084e3b2dffd5de6b7b16cee7969282de7，或NNC为fc732edee1efdf968c23c20a9628eaa5a6ccb934                   |
| amount      | String  | 转账的金额。钱包（Dapi提供者）将自动为您处理资产精度，只需输入浮点数。                                                                                   |
| remark      | String? | 交易的备注, 备注将会被记录到链上                                                                                                                      |
| fee         | String? | 网络手续费，目前的定义为网络费低于0.0001GAS的交易视作免费交易，其共识优先级低于付费交易。如果为0，用户还可以在确认页添加网络费                                |
| network     | String  | 网络类别的选择，信息来自getNetworks方法                                                                                                               |

### 成功的返回
| 参数      | 类型    | 说明                                                                          |
|:--------- |:------ |:----------------------------------------------------------------------------- |
| txid      | String | 发送的交易体的hash                                                             |
| nodeURL   | String | 发送交易体的NEO节点的URL地址                                                    |

<aside class="warning">
Dapp应该注意，获取到txid只代表节点发送交易体成功，并不代表交易已被共识（上链）。 Dapp可以查询已知节点的gettransactionheight方法（CLI 2.10.0+）以确保事务确实将在网络上广播。
</aside>

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## Invoke 发送合约调用交易
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
  "description":"NNC转账",
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

> 返回示例

```typescript
{
  txid: 'ed54fb38dff371be6e3f96e4880405758c07fe6dd1295eb136fe15f311e9ff77',
  nodeUrl: 'http://seed7.ngd.network:10332',
}:
```

本方法构造合约调用交易并发送。合约调用交易是使用智能合约写方法（改变私有化存储区的操作）的唯一途径，无论是执行NEP-5转账还是参与一次NNS竞拍，都需要构造合约调用交易。

### 输入参数
| 参数                        | 类型                  | 说明                                                                                                                 |
|:--------------------------- |:-------------------- |:-------------------------------------------------------------------------------------------------------------------- |
| scriptHash                  | String               | 所需调用合约的Hash                                                                                                    |
| operation                   | String               | 所需调用合约的方法名. 可以从合约的ABI文件或者合约开发者的文档获得                                                         |
| args                        | Argument[]           | 一个合约方法输入参数的数组                                                                                             |
| description                 | String               | 对于此次操作的简要说明，该说明将会显示在钱包的确认页面帮助用户理解（说明暂时不会存储在链上）                                 |
| fee                         | String?              | 网络手续费，目前的定义为网络费低于0.0001GAS的交易视作免费交易，其共识优先级低于付费交易。如果为0，用户还可以在确认页添加网络费 |
| network                     | String               | 网络类别的选择，信息来自getNetworks方法                                                                                |
| attachedAssets              | AttachedAssets?      | 合约附加资产，在代币销售方法会需要                                                                                      |
| assetIntentOverrides        | AssetIntentOverrides | 指定附加资产UTXO，如果使用此项，fee和attachedAssets将被忽略（Teemmo暂不支持）                                            |
| triggerContractVerification | Boolean?             | 添加指令调用合约的鉴权触发器（Teemmo暂不支持）                                                                          |

#### Argument参数结构
| 参数名     | 类型   | 说明                                                       |
|:--------- |:------ |:--------------------------------------------------------- |
| type      | String | 合约参数的类型                                             |
| value     | String | 合约参数的值                                               |

<aside class =notice>
type必须是以下之一： "String"|"Boolean"|"Hash160"|"Hash256"|"Integer"|"ByteArray"|"Array"|"Address"
</aside>

#### AttachedAssets 附加资产
| 参数名     | 类型    | 说明                                                   |
|:--------- |:------- |:------------------------------------------------------ |
| NEO       | String? | 附加NEO的数量                                           |
| GAS       | String? | 附加GAS的数量                                           |

#### AssetIntentOverrides 附加资产UTXO覆盖 （Teemmo暂不支持）
| 参数名     | 类型          | 说明                                               |
|:--------- |:------------- |:-------------------------------------------------- |
| inputs    | AssetInput[]  | 一个 UTXO inputs 的数组                             |
| outputs   | AssetOutput[] | 一个 UTXO outputs 的数组                            |

#### AssetInput 附加资产UTXO输入 （Teemmo暂不支持）
| 参数名     | 类型   | 说明                                                      |
|:--------- |:------ |:-------------------------------------------------------- |
| txid      | String | 交易的ID将被作为UTXO的输入使用                             |
| index     | String | Index of the UTXO, can be found from transaction details |

#### AssetOutput 附加资产UTXO输出 （Teemmo暂不支持）
| 参数名     | 类型   | 说明                                                                  |
|:--------- |:------ |:--------------------------------------------------------------------- |
| asset     | String | 资产ID                                                                |
| value     | String | 资产金额                                                               |
| address   | String | 转账的接收方的地址                                                     |

### 成功的返回
| 参数      | 类型    | 说明                                                                          |
|:--------- |:------ |:----------------------------------------------------------------------------- |
| txid      | String | 发送的交易体的hash                                                             |
| nodeURL   | String | 发送交易体的NEO节点的URL地址                                                    |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |

## InvokeGroup 发送合约调用交易组
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
      "description": "NNC转账",
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
      "description": "充值确认",
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

> 返回示例

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

本方法构造合约调用交易组并发送，并且内涵两种模式：聚合模式和非聚合模式：

聚合模式：钱包将输入的每个Invoke合约调用操作按照顺序统一构造进入 **一个单一的** 合约调用交易体，使得多个合约方法能够在一个交易中执行完成。该模式适合多个不同合约的小操作（耗费GAS少的）合并一个交易。聚合模式需要注意，是否聚合后会是的脚本执行超过10GAS。聚合模式下，交易的总的fee为各Invoke元素中fee之和。

非聚合模式：钱包将输入的每个Inovke合约调用操作按照顺序构造为 **多个独立的** 合约调用交易体，只有前一个交易被确认已经共识（上链）才会发送下一个交易。该模式适合一系列关联的合约操作，按先后逻辑关系自动搭接执行。

Hook_Txid：此类型为InvokeGroup方法特有，意为将上一个交易的txid作为下一个交易的输入参数，同时支持聚合模式和非聚合模式。但是有以下限制：

1. Hook_Txid不能出现在第一个Invoke元素
2. Hook_Txid的value必须是0，意为只能取第一个交易的txid
3. 使用Hook_Txid时，在聚合模式下，最多只能定义两个Invoke元素

### 输入参数
| 参数名     | 类型     | 说明                                                      |
|:--------- |:------   |:--------------------------------------------------------- |
| merge     | Boolean  | 选择是否采用聚合模式                                        |
| group     | Invoke[] | Invoke结构的数组                                           |

#### Invoke结构
| 参数                        | 类型                  | 说明                                                                                                                 |
|:--------------------------- |:-------------------- |:-------------------------------------------------------------------------------------------------------------------- |
| scriptHash                  | String               | 所需调用合约的Hash                                                                                                    |
| operation                   | String               | 所需调用合约的方法名. 可以从合约的ABI文件或者合约开发者的文档获得                                                         |
| args                        | Argument[]           | 一个合约方法输入参数的数组                                                                                             |
| description                 | String               | 对于此次操作的简要说明，该说明将会显示在钱包的确认页面帮助用户理解（说明暂时不会存储在链上）                                 |
| fee                         | String?              | 网络手续费，目前的定义为网络费低于0.0001GAS的交易视作免费交易，其共识优先级低于付费交易。如果为0，用户还可以在确认页添加网络费 |
| network                     | String               | 网络类别的选择，信息来自getNetworks方法                                                                                |
| attachedAssets              | AttachedAssets?      | 合约附加资产，在代币销售方法会需要                                                                                      |
| assetIntentOverrides        | AssetIntentOverrides | 指定附加资产UTXO，如果使用此项，fee和attachedAssets将被忽略（Teemmo暂不支持）                                            |
| triggerContractVerification | Boolean?             | 添加指令调用合约的鉴权触发器（Teemmo暂不支持）                                                                          |

##### Argument参数结构
| 参数名     | 类型   | 说明                                                       |
|:--------- |:------ |:--------------------------------------------------------- |
| type      | String | 合约参数的类型                                             |
| value     | String | 合约参数的值                                               |

<aside class =notice>
type必须是以下之一： "String"|"Boolean"|"Hash160"|"Hash256"|"Integer"|"ByteArray"|"Array"|"Address"|"Hook_Txid"
</aside>

#### AttachedAssets 附加资产
| 参数名     | 类型    | 说明                                                   |
|:--------- |:------- |:------------------------------------------------------ |
| NEO       | String? | 附加NEO的数量                                           |
| GAS       | String? | 附加GAS的数量                                           |

### 成功的返回
| 参数      | 类型    | 说明                                                                          |
|:--------- |:------ |:----------------------------------------------------------------------------- |
| []        | Array   | invoke返回结构的数组                                                             |

#### invoke返回结构
| 参数      | 类型    | 说明                                                                          |
|:--------- |:------ |:----------------------------------------------------------------------------- |
| txid      | String | 发送的交易体的hash                                                             |
| nodeURL   | String | 发送交易体的NEO节点的URL地址                                                    |

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |