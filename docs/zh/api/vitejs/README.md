---
sidebarDepth: 4
title: Version 2.0.0
---

:::tip 作者
[cs](https://github.com/lovelycs)
[hurrytospring](https://github.com/hurrytospring)
:::

:::tip 简介

2.0.0版本及以上，对于vitejs包进行了细化拆分。

1. 如果你需要vitejs中的全部功能，可直接引用@vite/vitejs。
2. 若使用某个功能，可以单独引用某个包
@vite/vitejs-abi、@vite/vitejs-addraccount、@vite/vitejs-account、@vite/vitejs-accountblock、
@vite/vitejs-client、@vite/vitejs-communication、@vite/vitejs-constant、@vite/vitejs-error、
@vite/vitejs-hdaccount、@vite/vitejs-hdaddr、@vite/vitejs-keystore、@vite/vitejs-netprocessor、
@vite/vitejs-privtoaddr、@vite/vitejs-utils、@vite/vitejs-ws、@vite/vitejs-http、@vite/vitejs-ipc
3. 若使用部分功能，需要处理项目依赖以及避免不必要的代码重复，可安装@vite/vitejs，引用其中的es5模块，使用你喜欢的任意打包工具自行打包
4. vitejs的任何包都支持es5语法，无需做特殊兼容

【注意】
1. 引用npm包时最好版本一致，以避免不必要的错误与冲突。
2. 阅读文档前, 可先行了解RPC接口, 直接调用RPC接口，数据返回不做处理。

:::

## 安装

:::demo

```bash tab:npm
npm install @vite/vitejs --save
npm install @vite/vitejs-ws --save
```

```bash tab:yarn
yarn add @vite/vitejs
yarn add @vite/vitejs-ws
```

:::

## 引入

:::demo

```javascript tab:import
import {
    constant, error, utils, accountBlock, keystore, 
    privToAddr, hdAddr, netProcessor, client, 
    addrAccount, account, hdAccount, abi
} from '@vite/vitejs';

// 需要使用网络服务时，需单独安装http/ipc/ws包
import ws from '@vite/vitejs-ws';
import http from '@vite/vitejs-http';
import ipc from '@vite/vitejs-ipc';
```

```javascript tab:require
const {
    constant, error, utils, accountBlock, keystore, 
    privToAddr, hdAddr, netProcessor, client, 
    addrAccount, account, hdAccount, abi
} = require('@vite/vitejs');

// 需要使用网络服务时，需单独安装http/ipc/ws包
const { WS_RPC } = require('@vite/vitejs-ws');
const { HTTP_RPC } = require('@vite/vitejs-http');
const { IPC_RPC } = require('@vite/vitejs-ipc');
```

:::

## 快速开始  

```javascript

import WS_RPC from '@vite/vitejs-ws';
import { client, constant } from '@vite/vitejs';

const { methods } = constant;
let provider = new WS_RPC("wss://example.com");

let myClient = new client(provider, (_myClient) => {
    console.log("Connected");
});

myClient.ledger.getSnapshotChainHeight().then((result) => {
    console.log(result);
}).catch((err) => {
    console.warn(err);
});

```

## 常用类型及说明
[可同时参考 constant 模块](/api/vitejs/constant/constant.html)

- RPCrequest
    - type 请求类型（request | notification | batch）
    - methodName [方法名称](/api/vitejs/constant/constant.html)
    - params 传参

- RPCrequest
    - jsonrpc 2.0
    - id
    - result
    - error RPCerror

- RPCerror
    - code
    - message

```typescript example
export declare type Hex = string;
export declare type HexAddr = string;
export declare type Addr = string;
export declare type Base64 = string;
export declare type TokenId = string;
export declare type Int64 = number;
export declare type Uint64 = string;
export declare type BigInt = string;

export declare type AddrObj = {
    addr: Addr;         // 真实地址
    pubKey: Hex;        // 公钥
    privKey: Hex;       // 私钥
    hexAddr: HexAddr;   // hex编码地址
}

export declare type AccountBlock = {
    accountAddress: HexAddr;
    blockType: BlockType;
    prevHash: Hex;
    snapshotHash: Hex;
    timestamp: Int64;
    height: Uint64;
    hash: Hex;
    signature: Base64;
    publicKey: Base64;
    fee?: BigInt;
    fromBlockHash?: Hex;
    toAddress?: HexAddr;
    tokenId?: TokenId;
    amount?: BigInt;
    data?: Base64;
    nonce?: Base64;
    logHash?: Hex;
}

// For example

// Type HexAddr
const hexAddr = "vite_c5f6afcbf1e1827929d83cae9ccb054f5b06fef197191d1944";

// Type Addr
const addr = "69f3bdb5cdcfa145ae6cc42593a89088ff3dac58";

// Type TokenId
const tokenId = "tti_5649544520544f4b454e6e40";

// Type RawTokenId
const rawTokenId = "5649544520544f4b454e";
```