---
title: Web3 接入 WalletConnect1.0
tags: [Vite, Vue3, Typescript, web3]
date: 2022-03-23
updated: 2022-03-23
categories: 
  - ['Vite']
  - ['Vue3']
  - ['Typescript']
  - ['web3']
cover: https://github.com/ChainSafe/web3.js/blob/1.x/assets/logo/web3js.jpg?raw=true

---

# Web3 接入 WalletConnect1.0



## 安装依赖

``` sh
yarn add web3 @walletconnect/web3-provider
```

## 创建 *walletConnetct provider*

``` ts
import WalletConnectProvider from "@walletconnect/web3-provider"

// 使用 infura 创建，需注册 infura 账号
const provider = new WalletConnectProvider({
  infuraId: <infuraId>
})
// or 使用自定义 RPC 创建
const provider: any = new WalletConnectProvider({
  rpc: {
    56: "https://bsc-dataseed1.binance.org/",	// BSC 主网
    97: "https://data-seed-prebsc-1-s1.binance.org:8545/"	// BSC 测试网
  },
  qrcodeModalOptions: {
    mobileLinks: [	// 配置 modal 显示列表
      "metamask",
      "tokenpocket"
    ]
  }
})

const subscribeToEvents = (provider: WalletConnectProvider) => {
  // 监听连接
  provider.on("connect", () => {
    console.log("connect")
  })
  // 监听 accounts 变化
  provider.on("accountsChanged", (accounts: string[]) => {
    console.log('accountsChanged', accounts)
  })
  // 监听 accounts 变化
  provider.on("chainChanged", (chainId: number) => {
    console.log('chainChanged', chainId)
  })
  // 监听断开
  provider.on("disconnect", (code: number, reason: string) => {
    console.log('disconnect', code, reason)
  })
}
subscribeToEvents(provider)

//  调用 provider 的 enable 方法调起 QR code modal
await provider.enable()
```

## 创建 web3 实例

``` ts
import Web3 from 'web3'
// or 如第一种引入方式报错请使用第二种方式
import Web3 from 'web3/dist/web3.min.js'

// 创建 web3 实例
const web3: typeof Web3 = new Web3(provider)
// 获取连接的 accounts
const accounts: string[] = await web3?.eth.getAccounts()
```

### 断开连接

``` ts
const killSession = () => {
  provider.disconnect()
}
```



## 可能存在的问题

1. 使用 vite 报 Buffer process EventEmitter global 不存在的错误

   解决方案一（推荐）

   ``` sh
   # 安装以下依赖
   yarn add -D rollup-plugin-polyfill-node
   ```

   修改 vite.config.ts

   ``` ts
   import nodePolyfills from 'rollup-plugin-polyfill-node'
   
   export default defineConfig({
     // ...
     plugins: [
       {
         ...nodePolyfills ({ include: ['node_modules/**/*.js', /node_modules\/.vite\/.*js/] }),
         apply: 'serve'
       },
     ],
     build: {
       rollupOptions: {
         plugins: [nodePolyfills()]
       },
       commonjsOptions: {
         transformMixedEsModules: true
       }
     }
     // ...
   })
   ```

   

   解决方案二

   ``` sh
   # 安装以下依赖
   yarn add buffer buffer process util
   ```

   在 *index.html* 中添加以下代码

   ``` html
   <head>
     <script>
       window.global = window;
     </script>
     <script type="module">
       import process from "process";
       import { Buffer } from "buffer";
       import EventEmitter from "events";
   
       window.Buffer = Buffer;
       window.process = process;
       window.EventEmitter = EventEmitter;
     </script>  
   </head>
   ```

   在 *vite.config.ts* 中添加以下代码

   ``` ts
   // 解决 build 后报错
   export default defineConfig({
     // ...
     build: {
       commonjsOptions: {
         transformMixedEsModules: true
       }
     }
     // ...
   })
   
   ```

2. 无法调起 walletConnect QR code modal

   walletConnect 必须在 https 环境下使用

   ``` ts
   // 配置 vite.config.ts
   export default defineConfig({
     // ...
     server: {
       https: true
     }
     // ...
   })
   ```

   或者使用 *nginx* 配置 https

   ``` sh
   # nginx.conf
   server {
       listen 443  ssl http2;
       server_name example.io;
   
   		# 引入 ssl 证书
       include ssl/example.io/ssl.conf;
   
       location / {
         proxy_pass https://127.0.0.1:3000;
       }
   }
   ```

   
