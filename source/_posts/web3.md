---
title: web3.js
tags: [Typescript, web3]
date: 2022-03-28
updated: 2022-03-28
categories: 
  - ['Typescript']
  - ['web3']
cover: https://github.com/ChainSafe/web3.js/blob/1.x/assets/logo/web3js.jpg

---

# Web3

## 前置准备

- 浏览器安装 *[metamask](https://metamask.io/download/)* 插件

- 安装 web3.js

  ``` sh
  npm install web3
  # or
  yarn add web3
  # or
  pnpm add web3
  ```



## web3 示例

``` ts
import Web3, { utils } from 'web3'	// 使用这种方式有可能报错
// or
import Web3, { utils } from 'web3/dist/web3.min.js'

// 在 xx.d.ts 中添加 ethereum 声明
declare global {
  interface Window {
    ethereum: any
  }
}

// 检查是否安装 metamask
if (!window.ethereum) {
  window.$message?.error('Please download the MetaMask plugin')
}

// 创建 web3 实例
const web3: typeof Web3  = new Web3(Web3.givenProvider)

// 获取 account
const [account] = await web3.eth.getAccounts()
// or metamask 方法
const [account] = await window.ethereum?.request({ method: 'eth_requestAccounts' })

// 交易
const params = {
  from: account,
  to: '0x...',	// 交易对方地址
  value: utils.toWei('100'))	// 转账金额 100，utils.toWei 将金额转换为 wei。wei 为以太单位 
}
web3.eth.sendTransaction(params, function(error: Error, transactionHash: string) {
	if (!error) {
    console.log(transactionHash)
  }
})

```



## 监听事件

``` ts
// 监听 accounts 变化
window.ethereum.on('accountsChanged', async (accounts) => {
	console.log('accountsChanged', accounts)
})
// 监听连接
window.ethereum.on('connect',  (connectInfo) => {
  console.log('connect', connectInfo)
})
// 监听 message
window.ethereum.on('message',  (message) => {
  console.log('message', message)
})
// 监听链变化
window.ethereum.on('chainChanged',  (chainId) => {
  console.log('chainChanged', chainId)
})
```



## 网络

### 添加网络

``` ts
function addChain () {
  // 添加 BSC 主网
  window.ethereum
    .request({
    method: 'wallet_addEthereumChain',
    params: [
      {
        chainId: '0x38',
        chainName: 'Binance Smart Chain Mainnet',
        nativeCurrency: {
          name: 'BNB',
          symbol: 'BNB',
          decimals: 18,
        },
        rpcUrls: ['https://bsc-dataseed1.binance.org'],
        blockExplorerUrls: ['https://bscscan.com'],
      },
    ],
  })
  .then(() => {
    console.log('网络切换成功')
  })
  .catch((error: Error) => {
		console.log(error)
  })
}
```

### 切换网络

``` ts
function switchChain (chainId: string) {
  // chainId 需为十六进制
  window.ethereum.request({
    method: 'wallet_switchEthereumChain',
    params: [{ chainId: chainId }],
  })
  .then(() => {
    console.log('success')
  })
  .catch((error: Error) => {
    if (error.code === 4902) {
      addChain()	// 添加网络
    }
  })
}
```

### 获取链 id，并切换网络

``` ts
web3.eth.net.getId((error: Error, id: number) => {
  // 不为 BSC 主网，则切换到 BSC 主网
  id !== 56 && switchChain('0x38')
})
```



## 代币合约（ERC20）

### 创建代币合约实例

``` ts
// 以 BUSD 为例
const busdContractAddress = '0xe9e7cea3dedca5984780bafc599bd69add087d56'	// BUSD 合约地址，测试网为 '0x7F38297507F95F7395D1B8fEC7140fAA3Dbe854c'
const contractAbi = [ { "inputs": [], "stateMutability": "nonpayable", "type": "constructor" }, { "anonymous": false, "inputs": [ { "indexed": true, "internalType": "address", "name": "owner", "type": "address" }, { "indexed": true, "internalType": "address", "name": "spender", "type": "address" }, { "indexed": false, "internalType": "uint256", "name": "value", "type": "uint256" } ], "name": "Approval", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": true, "internalType": "address", "name": "previousOwner", "type": "address" }, { "indexed": true, "internalType": "address", "name": "newOwner", "type": "address" } ], "name": "OwnershipTransferred", "type": "event" }, { "anonymous": false, "inputs": [ { "indexed": true, "internalType": "address", "name": "from", "type": "address" }, { "indexed": true, "internalType": "address", "name": "to", "type": "address" }, { "indexed": false, "internalType": "uint256", "name": "value", "type": "uint256" } ], "name": "Transfer", "type": "event" }, { "inputs": [ { "internalType": "address", "name": "owner", "type": "address" }, { "internalType": "address", "name": "spender", "type": "address" } ], "name": "allowance", "outputs": [ { "internalType": "uint256", "name": "", "type": "uint256" } ], "stateMutability": "view", "type": "function" }, { "inputs": [ { "internalType": "address", "name": "spender", "type": "address" }, { "internalType": "uint256", "name": "amount", "type": "uint256" } ], "name": "approve", "outputs": [ { "internalType": "bool", "name": "", "type": "bool" } ], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [ { "internalType": "address", "name": "account", "type": "address" } ], "name": "balanceOf", "outputs": [ { "internalType": "uint256", "name": "", "type": "uint256" } ], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "decimals", "outputs": [ { "internalType": "uint8", "name": "", "type": "uint8" } ], "stateMutability": "view", "type": "function" }, { "inputs": [ { "internalType": "address", "name": "spender", "type": "address" }, { "internalType": "uint256", "name": "subtractedValue", "type": "uint256" } ], "name": "decreaseAllowance", "outputs": [ { "internalType": "bool", "name": "", "type": "bool" } ], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [ { "internalType": "address", "name": "spender", "type": "address" }, { "internalType": "uint256", "name": "addedValue", "type": "uint256" } ], "name": "increaseAllowance", "outputs": [ { "internalType": "bool", "name": "", "type": "bool" } ], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [ { "internalType": "uint256", "name": "amount", "type": "uint256" } ], "name": "mint", "outputs": [], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [], "name": "name", "outputs": [ { "internalType": "string", "name": "", "type": "string" } ], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "owner", "outputs": [ { "internalType": "address", "name": "", "type": "address" } ], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "renounceOwnership", "outputs": [], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [], "name": "symbol", "outputs": [ { "internalType": "string", "name": "", "type": "string" } ], "stateMutability": "view", "type": "function" }, { "inputs": [], "name": "totalSupply", "outputs": [ { "internalType": "uint256", "name": "", "type": "uint256" } ], "stateMutability": "view", "type": "function" }, { "inputs": [ { "internalType": "address", "name": "recipient", "type": "address" }, { "internalType": "uint256", "name": "amount", "type": "uint256" } ], "name": "transfer", "outputs": [ { "internalType": "bool", "name": "", "type": "bool" } ], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [ { "internalType": "address", "name": "sender", "type": "address" }, { "internalType": "address", "name": "recipient", "type": "address" }, { "internalType": "uint256", "name": "amount", "type": "uint256" } ], "name": "transferFrom", "outputs": [ { "internalType": "bool", "name": "", "type": "bool" } ], "stateMutability": "nonpayable", "type": "function" }, { "inputs": [ { "internalType": "address", "name": "newOwner", "type": "address" } ], "name": "transferOwnership", "outputs": [], "stateMutability": "nonpayable", "type": "function" } ]	// BUSD 合约 ABI

// 创建 BUSD 合约实例
const busdInstance: typeof Web3.eth.Contract = new web3.value.eth.Contract(contractAbi, busdContractAddress)
```

### 查询代币余额

``` ts
// 获取某个钱包的代币余额
// 获取当前连接钱包地址，address 参数可以为当前连接地址或者其他钱包地址
const [address] = await web3.eth.getAccounts()
busdInstance.value?.methods.balanceOf(address).call()
  .then((res: string) => resolve(utils.fromWei(res)))
  .catch((error: Error) => {
		console.log(error)
	})
```

### 代币 *approve*

``` ts
// 使用代币交易（转账）时，需先调用 approve 方法为交易地址授予权限
// 授权给某个地址 100BUSD 的权限
const address = '0x...'
const [account] = await web3.eth.getAccounts()
busdInstance.value.methods.approve(address, utils.toWei('100'))
  .send({ from: account })
	.then((res: any) => {
    console.log(res)
	})
  .catch((error: Error) => {
  	console.log(error)
  })
```

### 交易（转账）

``` ts

const toAddress = '0x...'	// 交易对方地址，地址和金额需与 approve 时的传参一致
const [account] = await web3.eth.getAccounts()
// 当前用户转账
busdInstance.methods.transfer(toAddress, utils.toWei('100'))
  .send({ from: account }, function(error: Error, transactionHash: string) {
    if (!error) {
      console.log(transactionHash)
    }
  })
	.then((res: any) => {
    console.log(res)
	})
  .catch((error: Error) => {
  	console.log(error)
  })
// or 指定用户转账
const [fromAddress] = await web3.eth.getAccounts()
busdInstance.methods.transferFrom(fromAddress, toAddress, utils.toWei('100'))
  .send({ from: account }, function(error: Error, transactionHash: string) {
    if (!error) {
      console.log(transactionHash)
    }
  })
	.then((res: any) => {
    console.log(res)
	})
  .catch((error: Error) => {
  	console.log(error)
  })
```

