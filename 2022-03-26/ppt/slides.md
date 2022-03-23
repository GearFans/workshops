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

# Welcome to Gear Workshop

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

[新手必看！Rust 初学者如何编写 Gear 智能合约（1）](https://mp.weixin.qq.com/s/Yal1kLNcbDijO8iuPmtlaQ)

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

### ERC1155

ERC-1155 是由Enjin项目于2019推出的代币标准，该项目专注于基于区块链的游戏解决方案。

与ERC-721不同，如果要传输多个NFT，每个NFT将需要单笔交易，因为每个NFT由单个智能合约表示。当铸造或交易单个NFT时，就会导致有过高的交易成本。ERC-1155允许批量转移(单个智能合约上的多个资产)，这将使得所有代币可一次性转移，减少网络拥挤，降低gas成本。

例如，当一个用户想要向另一个用户出售游戏中的1000个道具时，ta可以使用ERC-1155的批量代币转移一次性将它们全部进行发送。

---

### Code

```rust

pub trait ERC1155TokenBase {
    fn balance_of(&self, account: &ActorId, id: &u128) -> u128;
    fn balance_of_batch(&self, accounts: &[ActorId], ids: &[u128]) -> Vec<BalanceOfBatchReply>;
    fn set_approval_for_all(&mut self, operator: &ActorId, approved: bool);
    fn is_approved_for_all(&mut self, account: &ActorId, operator: &ActorId) -> &bool;
    fn safe_transfer_from(&mut self, from: &ActorId, to: &ActorId, id: &u128, amount: u128);
    fn safe_batch_transfer_from(
        &mut self,
        from: &ActorId,
        to: &ActorId,
        ids: &[u128],
        amounts: &[u128],
    );
}

pub trait ExtendERC1155TokenBase {
    fn mint(&mut self, account: &ActorId, id: &u128, amount: u128);
    fn mint_batch(&mut self, account: &ActorId, ids: &[u128], amounts: &[u128]);
    fn burn_batch(&mut self, ids: &[u128], amounts: &[u128]);
    fn owner_of(&self, id: &u128) -> bool;
    fn owner_of_batch(&self, ids: &[u128]) -> bool;
}
```
