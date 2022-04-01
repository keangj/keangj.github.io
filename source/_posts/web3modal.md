---
title: Vue3 Vite Web3Modal 搭建 Web3 项目
tags: [Vite, Vue3, Typescript, web3]
date: 2022-04-01 22:30:32
updated: 2022-04-01 22:30:32
categories: 
  - ['Vite']
  - ['Vue3']
  - ['Typescript']
  - ['web3']
cover: https://github.com/ChainSafe/web3.js/blob/1.x/assets/logo/web3js.jpg?raw=true

---

# Vue3 Vite Web3Modal 搭建 Web3 项目

## 创建项目并运行项目

``` sh
pnpm create vite web3Modal -- --template vue-ts
cd web3Modal
pnpm install
pnpm dev
```

## 安装相关依赖

``` sh
pnpm add web3 web3modal @walletconnect/web3-provider rollup-plugin-polyfill-node
```



如有 ts 找不到模块类型声明的问题，在项目中的 xxx.d.ts 中添加以下代码

``` ts
// declare module <package name>
declare module 'web3modal'
declare module 'web3/dist/web3.min.js'
```

## 添加 web3modal 配置

创建配置文件 hooks/web3/config.ts

``` ts
import WalletConnectProvider from '@walletconnect/web3-provider'

const providerOptions = {
  walletconnect: {
    package: WalletConnectProvider,
    options: {
      // 自定义 rpc
      rpc: {
        56: 'https://bsc-dataseed1.binance.org/',
        97: 'https://data-seed-prebsc-1-s1.binance.org:8545/'
      }
    }
  }
}

export { providerOptions }
```

## 创建 web3modal provider

创建文件 hook/web3/useWallet.ts

``` ts
import Web3Modal, { providers } from 'web3modal'
import { providerOptions } from './config'
import WalletConnectProvider from "@walletconnect/web3-provider"
import Web3 from "web3/dist/web3.min.js"
import { reactive, toRefs } from "vue"

interface WalletInfo {
  web3: typeof Web3 | null
  provider: typeof providers | null
  account: string | null
}

const walletInfo: WalletInfo = reactive({
  web3: null,
  provider: null,
  account: null
})
export function useWallet () {
  // 创建 web3Modal 实例
  const web3Modal = new Web3Modal({
    providerOptions
  })

  // 获取当前连接钱包地址
  const getAccounts = async () => {
    const [account] = await walletInfo.web3.eth.getAccounts()
    walletInfo.account = account
  }
	
  // 订阅事件
  const subscribeToEvents = (provider: WalletConnectProvider) => {
    if (!provider) return
    provider.on('connect', () => {
    })
    provider.on('accountsChanged', (accounts: string[]) => {
      getAccounts()
    })
    provider.on('chainChanged', (chainId: number) => {
    })
    provider.on('disconnect', (code: number, reason: string) => {
    })
  }

  // 建立连接
  const onConnect = async () => {
    const provider = await web3Modal.connect()
    subscribeToEvents(provider)
    const web3 = new Web3(provider)
    walletInfo.provider = provider
    walletInfo.web3 = web3
    getAccounts()
  }

  // 重置钱包
  const resetWallet = async () => {
    await walletInfo.web3?.currentProvider?.close?.();
    web3Modal.clearCachedProvider();
    walletInfo.provider = null
    walletInfo.web3 = null
    walletInfo.account = null
  }

  return {
    ...toRefs(walletInfo),
    onConnect,
    resetWallet
  }
}

```



## 添加代币合约

``` ts
// hooks/web3/useERC20.ts, 封装代币合约方法调用
import { useWallet } from './useWallet'
import { computed } from 'vue'
import { utils } from 'web3/dist/web3.min.js'

const { web3 } = useWallet()
// busd abi
const abi = [{ "inputs": [], "stateMutability": "nonpayable", "type": "constructor" }, { "anonymous": false, "inputs": [{ "indexed": true, "internalType": "address", "name": "owner", "type": "address" }, { "indexed": true, "internalType": "address", "name": "spender", "type": "address" }, { "indexed": false, "internalType": "uint256", "name": "value", "type": "uint256" }], "name": "Approval", "type": "event" }, { "anonymous": false, "inputs": [{ "indexed": true, "internalType": "address", "name": "previousOwner", "type": "address" }, { "indexed": true, "internalType": "address", "name": "newOwner", "type": "address" }], "name": "OwnershipTransferred", "type": "event" }, { "anonymous": false, "inputs": [{ "indexed": true, "internalType": "address", "name": "from", "type": "address" }, { "indexed": true, "internalType": "address", "name": "to", "type": "address" }, { "indexed": false, "internalType": "uint256", "name": "value", "type": "uint256" }], "name": "Transfer", "type": "event" }, { "inputs": [{ "internalType": "address", "name": "owner", "type": "address" }, { "internalType": "address", "name": "spender", "type": "address" }], "name": "allowance", "outputs": [{ "internalType": "uint256", "name": "", "type": "uint256" }], "stateMutability": "view", "type": "function" }, { "inputs": [{ "internalType": "address", "name": "spender", "type": "address" }, { "internalType": "uint256", "name": "amount", "type": "uint256" }], "name": "approve", "outputs": [{ "internalType": "bool", "name": "", "type": "bool" }], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [{ "internalType": "address", "name": "account", "type": "address" }], "name": "balanceOf", "outputs": [{ "internalType": "uint256", "name": "", "type": "uint256" }], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "decimals", "outputs": [{ "internalType": "uint8", "name": "", "type": "uint8" }], "stateMutability": "view", "type": "function" }, { "inputs": [{ "internalType": "address", "name": "spender", "type": "address" }, { "internalType": "uint256", "name": "subtractedValue", "type": "uint256" }], "name": "decreaseAllowance", "outputs": [{ "internalType": "bool", "name": "", "type": "bool" }], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [{ "internalType": "address", "name": "spender", "type": "address" }, { "internalType": "uint256", "name": "addedValue", "type": "uint256" }], "name": "increaseAllowance", "outputs": [{ "internalType": "bool", "name": "", "type": "bool" }], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [{ "internalType": "uint256", "name": "amount", "type": "uint256" }], "name": "mint", "outputs": [], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [], "name": "name", "outputs": [{ "internalType": "string", "name": "", "type": "string" }], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "owner", "outputs": [{ "internalType": "address", "name": "", "type": "address" }], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "renounceOwnership", "outputs": [], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [], "name": "symbol", "outputs": [{ "internalType": "string", "name": "", "type": "string" }], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "totalSupply", "outputs": [{ "internalType": "uint256", "name": "", "type": "uint256" }], "stateMutability": "view", "type": "function" }, { "inputs": [{ "internalType": "address", "name": "recipient", "type": "address" }, { "internalType": "uint256", "name": "amount", "type": "uint256" }], "name": "transfer", "outputs": [{ "internalType": "bool", "name": "", "type": "bool" }], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [{ "internalType": "address", "name": "sender", "type": "address" }, { "internalType": "address", "name": "recipient", "type": "address" }, { "internalType": "uint256", "name": "amount", "type": "uint256" }], "name": "transferFrom", "outputs": [{ "internalType": "bool", "name": "", "type": "bool" }], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [{ "internalType": "address", "name": "newOwner", "type": "address" }], "name": "transferOwnership", "outputs": [], "stateMutability": "nonpayable", "type": "function" }]
type ERC20ContractType = 'busd'
const contracts = {
  busd: '0xe9e7cea3dedca5984780bafc599bd69add087d56'	// busd 合约地址
  // other contract
}

export function useERC20(contractType: ERC20ContractType) {
  const contractAddress = contracts[contractType]
  const erc20Instance = computed(() => {
    const { Contract } = web3.value.eth
    return new Contract(abi, contractAddress)
  })
	// 获取余额，address 为要查询的钱包地址
  function getBalance(address: string): Promise<string> {
    return new Promise((resolve, reject) => {
      erc20Instance.value?.methods
        .balanceOf(address).call()
        .then((res: string) => resolve(utils.fromWei(res)))
        .catch((error: Error) => reject(error))
    })
  }
 // 授权给 address，price 数额
  async function approve(address: string, price: number) {
    const [account] = await web3.value.eth.getAccounts()
    const balance = await getBalance(account)
    if (Number.parseFloat(balance) < price) {
      return Promise.reject(new Error('余额不足'))
    }
    return new Promise((resolve, reject) => {
      erc20Instance.value.methods
        .approve(address, (utils.toWei(price.toString())))
        .send({ from: account })	// from 为授权方
        .then((res: any) => {
          resolve(res)
        })
        .catch((error: Error) => reject(error))
    })
  }
  return {
    getBalance,
    approve
  }
}

```

## 在 vue 中调用

``` vue
<script setup lang="ts">
import { useWallet } from './hooks/web3/useWallet'
import { useERC20 } from './hooks/web3/useErc20'
import { ref } from 'vue';
const { onConnect, account } = useWallet()
const { getBalance: getBusdBalance } = useERC20('busd')

const amount = ref<string | null>(null)
const getBalance = async () => {
  amount.value = await getBusdBalance(account.value)
}
</script>

<template>
  <button @click="onConnect">onConnect</button>
  <button v-if="account" @click="getBalance">getBalance</button>
  <p>{{ account }}</p>
  <p>{{ amount }}</p>
</template>
```



## 项目地址

[web3Modal-example](https://github.com/keangj/web3Modal-example)
