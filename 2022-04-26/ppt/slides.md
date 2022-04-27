---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: /img/bg.jpg
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: true
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: true

---

## 使用 Gear 构建 DAO 投票平台

---

## Gear

### 技术 白皮书

https://github.com/GearFans/gear-technical/blob/cn/cn.md

---

### Gear 的关键技术

## 开发资料

### Wiki

https://wiki.gear-tech.io/zh-cn/

<img src="/img/wiki.png" class="w-150" />

---

### gear-js SDK

gear-js 是 Gear 的 js SDK，通过这个工具我们可以连接节点，上传合约，发送交易，还有解析 Gear 合约等。

相关介绍：[如何使用 gear-js SDK](https://mp.weixin.qq.com/s/xwFhLISx2Pdi7p3u4Tn0IA)

---

### 在线 IDE

https://idea.gear-tech.io/

<img src="/img/ide.png" class="w-150" />

---

## 合约

### 合约资料

[Gear 合约大揭秘](https://mp.weixin.qq.com/s/URoDFMWeWZYUEdIKNTZbyg)

[Rust 初学者如何编写 Gear 智能合约（1）](https://mp.weixin.qq.com/s/Yal1kLNcbDijO8iuPmtlaQ)

---

### 合约基础结构

```rust
#![no_std]
gstd::metadata! {
  title: "dao",
  init:
    input: InitConfig,
  handle:
    input: Action,
    output: Event,
  state:
    input: State,
    output: StateReply,
}

#[no_mangle]
pub unsafe extern "C" fn handle() {}

#[no_mangle]
pub unsafe extern "C" fn init() {}

#[no_mangle]
pub unsafe extern "C" fn meta_state() -> *mut [i32; 2] {}

```

---

### 常用方法

#### exec

```rust
gstd::exec::block_timestamp() // 获取当前时间
gstd::exec::block_height() // 获取当前区块高度
gstd::exec::program_id() // 获取当前程序的 id
gstd::exec::value_available // 获取当前余额

gstd::exec::exit // 终止程序的执行

```

详细资料：https://docs.gear.rs/gstd/exec/index.html

---

#### msg

```rust
gstd::msg::id // 获取消息 Id
gstd::msg::source //获取消息发送者的钱包地址，类似 solidity 中的 msg.sender

gstd::msg::load // 获取发送给合约的消息
gstd::msg::load_bytes // 获取发送给合约的消息

gstd::msg::reply // 发送一条新信息作为对当前正在处理的信息的回复
gstd::msg::reply_bytes // 发送一条新信息作为对当前正在处理的信息的回复
gstd::msg::reply_to // 获取当前 handle_reply 函数被调用的初始信息的标识符

gstd::msg::send_bytes // 向合约或者用户发现新消息
gstd::msg::send // 向合约或者用户发现新消息

```

详细资料：https://docs.gear.rs/gstd/msg/index.html

---

## DAO

### 安装 Rust

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 添加 wasm32-unknown-unknown toolchains

```shell
rustup toolchain add nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```

### 安装 Polkadot.js 插件

下载插件 https://polkadot.js.org/extension/

---

### 编译合约

- clone 代码：`git clone https://github.com/gear-tech/apps`
- 修改 `Cargo.toml`
- 编译合约：`make`

- 发现 wasm 合约文件，**请注意文件路径** ：`ls -l target/wasm32-unknown-unknown/release/*.wasm`

```shell
dao_light.meta.wasm # meta 文件，类似 abi 文件，后缀 meta.wasm 结尾
dao_light.opt.wasm # 主合约文件，后缀 opt.wasm 结尾
dao_light.wasm # meta + opt 的 “合体”文件

fungible_token.meta.wasm
fungible_token.opt.wasm
fungible_token.wasm
```

---

### 上传 DAO 合约

- https://idea.gear-tech.io/

- 初始化参数

```js
{
  "approvedTokenProgramId": "0x6b7f0a5c5253769b44648c7d506ff37a59bb817f912adbe2cf9c97b8963e7705",
  "votingPeriodLength": "30000",
  "periodDuration": "300000", //关键
  "gracePeriodLength": "30000"
}

```

- 上传 `.opt.wasm` 合约，和 `.meta.wasm` 合约，设置好 gas limit

---

## dao-dapp

- https://github.com/GearFans/dao-app

- 切换分支 ef-dao-light

- gear-meta

https://github.com/gear-tech/gear-js/tree/master/utils/meta-cli

- 修改 `.env`

---

### 使用

<img src="/img/dao.png" class="w-200" />


---

## 谢谢关注 Gear

<img src="/img/end.png" class="w-200" />
