# 事件

事件是一种钱包（Dapi提供者）主动通知方式，被授权的Dapp能够在事件发生时被动的捕获事件和相关数据，并据此调整Dapp的行为模式。

## READY 准备完成事件
当钱包的通信js注入Dapp页面完成，页面就会收到“准备完成事件”。钱包的所有方法，都应该在这个事件发生后开始调用。收到此事件同时，会获得一个getProvider方法的返回数据。

### 完整事件名
Teemo.NEO.READY

### 事件数据
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

## ACCOUNT_CHANGED 地址变更事件
当钱包的使用者，在钱包管理界面中更换地址时，会触发此事件。收到此事件同时，会获得一个getAccount方法的返回数据。

### 完整事件名
Teemo.NEO.ACCOUNT_CHANGED

### 事件数据
| 参数名     | 类型   | 说明                                                                |
|:--------- |:------ |:------------------------------------------------------------------  |
| address   | String | 当前钱包（Dapi提供者）被选中（使用中）的NEO地址                         |
| label     | String | 当前钱包（Dapi提供者）被选中（使用中）的用户设定名称                    |

## CONNECTED 已连接事件

Dapp在首次请求钱包的任何方法前，都需要用户确认是否同意授权Dapp访问钱包。如果用户同意，Dapp将收到此事件。收到此事件同时，会获得一个getAccount方法的返回数据。

另一种情况，如果钱包从非登录（锁定）状态变为登录（解锁）状态，Dapp也将收到此事件。

### 完整事件名
Teemo.NEO.CONNECTED

### 事件数据
| 参数名    | 类型    | 说明                                               |
|:--------- |:------ |:-------------------------------------------------- |
| address   | String | 连接时钱包选择的地址                                 |
| label     | String | 连接时钱包选择的账户标签                             |

## DISCONNECTED 连接断开事件

当钱包和Dapp断开连接时会收到此事件，例如退出登录操作会触发这一事件。这个事件没有附带数据。

### 完整事件名
Teemo.NEO.DISCONNECTED

## NETWORK_CHANGED 网络变更事件

当钱包的使用者，在钱包管理界面中更换网络时，会触发此事件。收到此事件同时，会获得一个getNetworks方法的返回数据。

### 完整事件名
Teemo.NEO.NETWORK_CHANGED

### 事件数据
| 参数名         | 类型      | 说明                                                                |
|:-------------- |:-------- |:------------------------------------------------------------------ |
| networks       | String[] | 一个允许的网络类型的列表                                             |
| defaultNetwork | String   | 目前钱包设置的网络类型                                               |

## BLOCK_HEIGHT_CHANGED 块高度变更事件

当所在网络区块高度变更时（出块时），将触发此事件，Dapp可根据此事件执行相应动作。本事件数据包含，新块高度、块Hash、块时间戳和块包含TXID表

### 完整事件名
Teemo.NEO.BLOCK_HEIGHT_CHANGED

### 事件数据
| 参数名          | 类型   | 说明                     |
|:-------------- |:------ |:----------------------- |
| network        | String | 网络类型                 |
| blockHeight    | Number | 新块索引值（高度）        |
| blockTime      | Number | 新块时间戳               |
| blockHash      | String | 新块Hash                 |
| tx             | TXID[] | 新块包含的TXID组          |

## TRANSACTION_CONFIRMED 交易共识（确认）事件

当通过钱包（Dapi提供者）发出的交易被共识（确认）时，将触发此事件。Dapp可根据此事件执行相应动作。

### 完整事件名
Teemo.NEO.TRANSACTION_CONFIRMED

### 事件数据
| 参数名          | 类型   | 说明                     |
|:-------------- |:------ |:------------------------ |
| TXID           | String | 交易Hash                 |
| blockHeight    | Number | 交易所在块索引值（高度）   |
| blockTime      | Number | 交易所在块时间戳          |

## 事件方法

### 捕获事件

```typescript
window.addEventListener('Teemo.NEO.READY',(data:CustomEvent)=>{
    console.log("Teemo READY ");
    console.log(JSON.stringify(data.detail))

    const main = new Main();
    main.start();//监听到这个事件后，才能开始插件的相关方法调用
})
```

在Dapp页面采用标准JS 事件获取方法window.addEventListener获取钱包的自定义事件和事件数据，并在收到事件和数据后进行相应逻辑处理。

注意，事件都是异步的。
