---
title: NEL Dapi Docs

language_tabs: # must be one of https://git.io/vQNgJ
#  - typescript

toc_footers:
  - <a href='https://dapi.nel.group/en/'>Browse the English version</a>
  - <a href='https://nel.group/'>NewEconoLabs NEL</a>

includes:
  - _read.md
  - _write.md
  - _events.md
  - _errors.md

search: true
---

# NEO Dapi简介

NEO Dapi 是一种NEO环境中通用的Dapi标准，其由NEL与O3和其他NEO社区成员一同维护。

提供Dapi的程序有各种形态，有NEL的Teemo Chrome 插件钱包，也有O3钱包以及其他形态。

我们力求Dapi在总体上是一致且互相兼容的，但是每个Dapi提供者（程序）会根据其自身特性稍有不同。

这里我们将提供开发者Dapi调用方法（部分会比较侧重NEL的Teemo Chrome 插件钱包）

# Teemo 插件钱包

Teemo是NEL开发的一款Chrome插件钱包，该钱包力求Dapp开发者在前端开发Dapp时用时最少，将全部精力用在Dapp开发本身上，而不是用在NEO的交互对接上。

插件钱包的另一个特点是安全性，它将用户钱包私钥保护在插件内部，Dapp无法直接访问到。而且每次使用私钥签名（发送交易），都需要用户授权。事实上，未授权的Dapp连用户地址都看不到。如此一来，用户只需认准一个钱包，就可以放心地尝试任何新奇的Dapp，而不用担心安全问题。

## Teemo 资源
[Teemo Github](https://github.com/NewEconoLab/TeemoWallet)


## Teemo 安装与开发

### 安装与体验

#### 开发版安装
克隆项目

```
git clone https://github.com/NewEconoLab/TeemoWallet.git
```
打开Chrome（最新版）
Chrome菜单-更多工具-扩展程序

切换到开发者模式

点击“加载已解压的扩展程序”-选择**TeemoWallet\dist文件夹

可以将**TeemoWallet\test 文件夹配置为一个网站（比如http://localhost/）。你也可以直接浏览 [https://teemo.nel.group/test/](https://teemo.nel.group/test/),免去自己部署

打开这个网站，即可测试事件和所有方法。这个网站同时也是一个开发示例

#### 稳定版安装

[Github Release](https://github.com/NewEconoLab/TeemoWallet/releases)

下载release ZIP包并解压，其他步骤和开发版相同

### 动手开发

```typescript
window.addEventListener('Teemo.NEO.READY',(data:CustomEvent)=>{
    console.log("Teemo READY ");
    console.log(JSON.stringify(data.detail))

    const main = new Main();
    main.start();//监听到这个事件后，才能开始插件的相关方法调用
})
```

Teemo的Dapi在调用前无需事先引用任何JS或NPM包，和Teemo交互的JS代码会自动注入到你的页面中，但是这个注入可能并不总是在你的第一句Dapi调用代码前完成。

为了总是能够正常和Teemo交互，我们强烈建议在任何Dapi方法被使用前，首先捕获“Teemo.NEO.READY”事件，此事件代表Teemo交互JS已经注入到你的页面。

如果你采用TS（TypeScript）进行开发，可能还需要在你的目录中添加https://github.com/NewEconoLab/TeemoWallet/blob/master/dist/js/inject.d.ts，它将帮助tsc引擎识别Dapi方法和类型。