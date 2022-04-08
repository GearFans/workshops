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

## Gear 合约入门开发 - 去中心化 Twitter

---

## Gear

### 技术 白皮书

https://github.com/GearFans/gear-technical/blob/cn/cn.md

---

### Gear 的关键技术

- Gear 使用 Actor 通信模型

简而言之，这意味着程序（智能合约）从不共享任何 "状态"，只在彼此之间交换消息。
这些消息的结果可能导致发送其他消息、创建新的 actor 或为 actor 收到的下一个消息设置指定的行为。

- 支持并行处理

---

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
  title: "ERC20",
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

## Gear 去中心化 Twitter

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

### 获取测试 coin

用 `Polkadot.js 插件` 创建地址，点击 **Get test account** 按钮，获取测试 coin

---

### 编译合约

- clone 代码：`git clone https://github.com/gear-tech/gear-feeds-channel`
- 编辑本地代码，将 `./gear-feeds-channel/contract/src/lib.rs` 文件的 TODO 内容改成自己的内容
- 编译合约：`make`

- 发现 wasm 合约文件，**请注意文件路径** ：`ls -l target/wasm32-unknown-unknown/release/*.wasm`

```shell
-rw-r--r--  gear_feeds_channel.meta.wasm # meta 文件，类似 abi 文件，后缀 meta.wasm 结尾
-rw-r--r--  gear_feeds_channel.opt.wasm # 主合约文件，后缀 opt.wasm 结尾
-rwxr-xr-x  gear_feeds_channel.wasm # meta + opt 的“合体”文件
```

---

### 上传合约

- https://idea.gear-tech.io/ ，请**确保 rpc** 为 `wss://node-workshop.gear.rs:443`

- 上传 `.opt.wasm` 合约文件，和 `.meta.wasm` 文件，设置好 gas limit，推荐 1000000000

- 上传成功后，记录合约地址

---

### 将 Twitter 用户合约注册到 Twiter Router

- 在 https://idea.gear-tech.io/ 查看 **All programs**，找到 [Router_8_04](https://idea.gear-tech.io/program/0xb82cb2ee32e406fab38c19ad77b145116faffbff62e5f91cd161091f911b83a3?node=wss%3A%2F%2Fnode-workshop.gear.rs%3A443)

- 将在上一步得到的合约地址，注册到 `Router_8_04`

- 设置 **Gas limit** 为 `500000000`（如果失败，尝试扩大 100 倍），然后点击 **Send request**

- 注册成功后，可以在 https://workshop.gear-tech.io/all 发现新的 Twitter 用户

<img src="/img/contract.png" class="w-150" />

---

### 使用

- 访问 https://workshop.gear-tech.io/all 查看自己的账户

- 尝试发送消息，订阅，取消订阅

<img src="/img/channel.png" class="w-100" />


---

## 谢谢关注 Gear

<img src="/img/end.png" class="w-200" />
