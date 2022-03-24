---
# try also 'default' to start simple
theme: seriph
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: false
# some information about the slides, markdown enabled
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
# persist drawings in exports and build
drawings:
  persist: false

---

# Welcome to Rebase CodingDay
## Gear 入门合约开发

---

## 开发资料

### Wiki

https://wiki.gear-tech.io/zh-cn/

![](/img/wiki.png)

---

### gear-js SDK

gear-js 是 Gear 的 js SDK，通过这个工具我们可以连接节点，上传合约，发送交易，还有解析 Gear 合约等。

相关介绍：[如何使用 gear-js SDK](https://mp.weixin.qq.com/s/xwFhLISx2Pdi7p3u4Tn0IA)

---

### 在线 IDE

https://idea.gear-tech.io/

![](/img/ide.png)

---

## 合约

### 合约资料

[Gear 合约大揭秘](https://mp.weixin.qq.com/s/URoDFMWeWZYUEdIKNTZbyg)

[Rust 初学者如何编写 Gear 智能合约（1）](https://mp.weixin.qq.com/s/Yal1kLNcbDijO8iuPmtlaQ)

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

### 2月相关更新

[2022 年 Gear 月度更新——2月](https://mp.weixin.qq.com/s/TpaBXsychE2PgokQzVKcAw)

重要更新：

- gear-wasm-builder
- msg::reply()

---

## 例子

## Gear 去中心化 Twitter

### 安装 Rust

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 添加 wasm32-unknown-unknown toolchains

```shell
rustup toolchain add nightly
rustup target add wasm32-unknown-unknown --toolchain nightly

cargo install --git https://github.com/gear-tech/gear wasm-proc
```

... 或者 ...

```shell
make prepare
```

---

### 编辑 Twitter 用户合约

1. 把代码clone到本地

```
git clone https://github.com/gear-tech/gear-feeds-channel
```

2. 编辑本地代码

将 `./gear-feeds-channel/src/lib.rs` 文件的TODO内容改成自己的内容

### 编译合约

```shell
cargo +nightly build --target wasm32-unknown-unknown --release
wasm-proc --path ./target/wasm32-unknown-unknown/release/gear_feeds_channel.wasm
```

... 或者 ...

```shell
make
```

---

### 安装 Polkadot.js 插件

从 https://polkadot.js.org/extension/ 下载插件

### 创建 substrate 地址

用 `Polkadot.js 插件` 创建地址

### 上传 Twitter 用户合约

- https://idea.gear-tech.io/ ，请**切换 rpc** 为 `wss://node-workshop.gear.rs/`

- 点击 **Get test account** 按钮，获取测试 coin

- 上传 `.opt.wasm` 合约文件，和 `.meta.wasm` metadata 文件，设置好 gas limit

- 上传成功后，记录合约地址

---

### 将 Twitter 用户合约注册到 Twiter Router

- 在 https://idea.gear-tech.io/ 查看 **All programs**，找到 [CHINESE WORKSHOP ROUTER](https://idea.gear-tech.io/program/0xbc7e98f3f7eb5607a16bbfef3b2228b6dafef788af3518b16769bf74c2dcc1ad)

- 将在上一步得到的合约地址，注册到 `CHINESE WORKSHOP ROUTER`

- 设置 **Gas limit** 为 `500000000`（如果失败，尝试扩大100倍），然后点击 **Send request**

- 注册成功后，可以在 https://workshop.gear-tech.io/all 发现新的 Twitter 用户

---

### 使用

- 去 https://workshop.gear-tech.io/ 查看自己的账户

- 尝试发送短文，订阅，取消订阅

恭喜你，注册成功！
