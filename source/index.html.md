---
title: NEL Dapi Docs

language_tabs: # must be one of https://git.io/vQNgJ
#  - typescript

toc_footers:
  - <a href='https://dapi.nel.group/cn/'>浏览中文版</a>
  - <a href='https://nel.group/'>NewEconoLabs NEL</a>

includes:
  - _read.md
  - _write.md
  - _events.md
  - _errors.md


search: true
---

# NEO Dapi introduction

NEO Dapi is a generic Dapi standard in the NEO environment that is maintained by NEL in conjunction with O3 and other NEO community members.

The programs that provide Dapi come in a variety of forms, including NEL's Teemo Chrome extension wallet, O3 wallet and other forms.

We do our best to implement the overall consistencey and compatibility of the Dapis, but each Dapi provider (program) varies slightly depending on its own characteristics.

Here we will provide the developer Dapi calling methods (part of which are only related to the NEL's Teemo Chrome extension wallet).

# Teemo extension wallet

Teemo is a Chrome extension wallet developed by NEL. The wallet aims to minimize Dapp developers'time spent  on the front-end, so that Dapp developers could focus on Dapp development itself, rather than on interactions with NEO.

Another feature of the extension wallet is security.The Teemo wallet protects the user's wallet private key inside the extension, which Dapp cannot access directly. And user authorization is required when signing a signature(sending a transaction) using a private key each time. In fact,the user's address is invisible to the unauthorized Dapp. In this way, users only need to trust and use one and the same wallet and can safely try any novel Dapps without worrying about security issues 

## Teemo Resources
[Teemo Github](https://github.com/NewEconoLab/TeemoWallet)


## Teemo installation and development

### Installation and experience

Clone the project

```
Git clone https://github.com/NewEconoLab/TeemoWallet.git
```
Open Chrome (latest version)
Chrome menu - more tools - extensions

Switch to developer mode

Click on "Load unzipped extensions" - select the **TeemoWallet\dist folder

You can configure the **TeemoWallet\test folder as a website (such as http://localhost/).You Can also browse [https://teemo.nel.group/test/](https://teemo.nel.group/test/) ,Free from deploying

Open this website and test the events and all the methods. This website page is also a development example.

### Start development

```typescript
window.addEventListener('Teemo.NEO.READY',(data:CustomEvent)=>{
    console.log("Teemo READY ");
    console.log(JSON.stringify(data.detail))

    const main = new Main();
    main.start();//监听到这个事件后，才能开始插件的相关方法调用
})
```

Teemo's Dapi does not need to reference any JS or NPM packages before calling. The JS code that interacts with Teemo is automatically injected into your page, but this injection may not always be done before your first Dapi call code.

In order to always interact with Teemo, we strongly recommend that the "Teemo.NEO.READY" event be captured first before any Dapi method is used. This event represents that the Teemo interaction JS has been injected into your page.

If you are developing with TS (TypeScript), you may also need to add https://github.com/NewEconoLab/TeemoWallet/blob/master/dist/js/inject.d.ts to your directory, which will help the tsc engine. Identify Dapi methods and types.