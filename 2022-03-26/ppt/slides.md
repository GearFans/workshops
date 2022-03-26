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

## Gear

### Gear 的关键技术

- Gear 使用 Actor 通信模型

简而言之，这意味着程序（智能合约）从不共享任何 "状态"，只在彼此之间交换消息。
这些消息的结果可能导致发送其他消息、创建新的 actor 或为 actor 收到的下一个消息设置指定的行为。

- 支持并行处理

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

---
src: ./example.md
---