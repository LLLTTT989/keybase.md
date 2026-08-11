# Aqua 最新策略链上核验

核验地址：`0xE01ecFF2F6c4F2416E83e6861e8ABf79b1C95950`

查询截点：Ethereum block `25729680`，约 2026-08-11 14:30 JST。

## 最新一笔 ship

- 时间：2026-08-10 02:58:47 JST
- 区块：25719070
- 交易：`0x6238db6ca6eb9092c5494dd713e072cb709d17008660be9040219a4185268cab`
- App：`0x111111338c5091e8440b67b168bae16a668ac0de`
- Strategy hash：`0x89a88977c8083e859c7e680286f7a3b6f7a0e6fd27a038dcaeeaada4987494e4`
- 币对：1INCH / SPYx
- ship 初始虚拟余额：0 1INCH + 0.160382 SPYx

## 当前状态

- 该 strategy hash 之后没有对应 dock，仍为 active。
- 从 ship 到查询截点，没有发现该 strategy hash 的 SwapVM `Swapped` 成交事件。
- Aqua `safeBalances` 直接读取：0 1INCH + 0.160382 SPYx。
- Aqua `rawBalances`：1INCH = 0，SPYx = 0.160382，两个 token 的 `tokensCount` 均为 2，符合活跃双 token 策略。
- 查询截点的钱包 ERC-20 余额：2.599364411846314714 1INCH + 0.005435514999871609 SPYx。

因此，策略配置/账面虚拟余额是 0.160382 SPYx，但当时钱包实际 SPYx 库存只有 0.005435514999871609 SPYx。Aqua 虚拟余额可在多个策略间共享，不能将虚拟余额直接等同于独立锁仓或保证全部可一次成交的真实库存。

## 前一笔最新策略

- 时间：2026-08-10 02:57:23 JST
- 交易：`0x1bc839184c9da10925c0e17b39dac4d84a468a6bc83d5662cf828034b4f94569`
- Strategy hash：`0x0490ace19cb37081e874f6b162c731a59419b3bf8743818dfa949045a156fa22`
- 币对：1INCH / WBTC
- ship 初始虚拟余额：1091.320782799999998658 1INCH + 0.00088489 WBTC
- 在查询范围内未发现对应 dock，仍显示 active。
