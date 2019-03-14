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
| fee                         | String?              | 网络手续费，目前的定义为网络费低于0.0001GAS的交易视作免费交易，其共识优先级低于付费交易。如果为0，用户还可以在确认页添加网络费 |
| network                     | String               | 网络类别的选择，信息来自getNetworks方法                                                                                |
| attachedAssets              | AttachedAssets?      | 合约附加资产，在代币销售方法会需要（Teemmo暂不支持）                                                                     |
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

#### AttachedAssets 附加资产 （Teemmo暂不支持）
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
| nodeURL   | String | 发送交易体的NEO节点的URL地址 

### 失败的返回
| 参数名       | 类型    | 说明                                         |
|:----------- |:------- |:-------------------------------------------- |
| type        | String  | 错误的类型                                    |
| description | String? | 错误的说明                                    |
| data        | String? | 错误的相关数据                                |