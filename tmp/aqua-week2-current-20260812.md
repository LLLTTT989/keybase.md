# 0xE01e…5950 Aqua 第二周奖励与当前区间核验

地址：`0xE01ecFF2F6c4F2416E83e6861e8ABf79b1C95950`

## 数据截点

- Merkl 奖励快照：2026-08-12 00:11:45 JST。
- 当前策略生命周期快照：Ethereum block 25732593，约 2026-08-12 00:18 JST。
- 策略字节和价格区间复核：Ethereum block 25732622，约 2026-08-12 00:23 JST。

## 奖励状态

### 已经真正领取

- 1INCH：`77,283.464986563401351269`
- USDC：`7,728.346495`
- 当前 claimable：两者均为 0。

Aqua 的 1INCH 和 USDC 只发现一笔实际领取：

- 时间：2026-08-05 03:02:35 JST
- 交易：`0x67b55eee4a6ea8b99ca5be4042ab2ea43e388ae28080e3a2d3de617161d0c879`
- 到账：`77,283.464986563401351269 1INCH + 7,728.346495 USDC`

截至本次快照，没有发现第二笔 Aqua 1INCH/USDC claim。之后的 claim 交易到账均为 UNI，不属于 Aqua Season 1 的 1INCH/USDC 奖励。

### 第二周（epoch-1）已计算但仍为 pending

- 1INCH pending：`27,441.963715806385552061`
- USDC pending：`2,744.196368`
- credited/amount 增量：0
- claimed 增量：0

这表示第二周结果已出现在 Merkl 用户数据中，但尚未写入可领取的 Merkle amount，也没有被地址领取。

### 累计赚到（已领取 + pending）

- 1INCH：`104,725.428702369786903330`
- USDC：`10,472.542863`

按快照时 Merkl 返回的 `1INCH = 0.08325757567381867 USD` 估值：

- 已领取 Aqua 奖励约：`14,162.780429453718 USD`
- 第二周 pending 约：`5,028.947738706936 USD`
- 累计已赚含 pending 约：`19,191.728168160654 USD`

## 第二周 pending 分类

| 类别 | 1INCH pending | USDC pending |
|---|---:|---:|
| Ethereum — ETH & LSTs | 15,306.987021385524560470 | 1,530.698702 |
| Ethereum — Stablecoins | 5,006.337507659186019753 | 500.633750 |
| Ethereum — BTC wrappers | 1,767.994503791011824439 | 176.799450 |
| Ethereum — RWA | 554.785980019133915903 | 55.478598 |
| Ethereum — DeFi majors | 581.830524334141644754 | 58.183052 |
| Robinhood Chain — RWA | 1,735.693215704786834989 | 173.569321 |
| BNB Chain — RWA | 555.869013462610051338 | 55.586901 |
| Robinhood Chain — DeFi majors | 1,932.465949449990700415 | 193.246594 |
| **合计** | **27,441.963715806385552061** | **2,744.196368** |

## 当前活跃策略

从 2026-08-08 起重新核对所有直接 `ship` / `dock`，在 block 25732593 时只有两条未被后续 `dock` 的活跃策略。

### 1INCH / WETH

- ship 时间：2026-08-11 20:45:11 JST
- ship 交易：`0xd3f78f29098c2ba400898c4305f2664b90121722591dbbc2a3cf25350ce9343c`
- Strategy hash：`0x41decf86ae6fa2a23f8d2e98432584ec0613fd54a4b1ba4cccbe7cca9439fe07`
- 虚拟余额：`1,424.966281799999999999 1INCH + 0.095560199993674968 WETH`
- 当前未发现后续 dock。

配置区间：

- `WETH per 1INCH`：`0.0000441351915618879925924239646225` ～ `0.000044338028683622991478934832592009`
- 倒数方向 `1INCH per WETH`：`22,554.0022795232456402797944767` ～ `22,657.6562740814920073011636612`
- 区间宽度约：`0.4595813784%`

按奖励快照时 1INCH 价格折算，对应隐含 WETH 美元价格约 `$1,877.79 ～ $1,886.42`；此折算仅用于理解区间，不是合约参数的一部分。

### 1INCH / WBTC

- ship 时间：2026-08-11 20:45:47 JST
- ship 交易：`0x5aabf1fdf4223523c6d84efede0e1ddbae5e374c8a94624e56507b65b8121f5e`
- Strategy hash：`0xd847a50ba9a438f181c50cc4537946ab8e33aa4d9d3484b77462a4cbdaedba37`
- 虚拟余额：`299.697470299999916493 1INCH + 0.00088489 WBTC`
- 当前未发现后续 dock。

配置区间：

- `WBTC per 1INCH`：`0.00000128970233820167850289` ～ `0.00000130612030347843209764`
- 倒数方向 `1INCH per WBTC`：`765,626.257655455665588591067` ～ `775,372.712275895705328587711`
- 区间宽度约：`1.2730042267%`

按奖励快照时 1INCH 价格折算，对应隐含 WBTC 美元价格约 `$63,744.19 ～ $64,555.65`；此折算仅用于理解区间，不是合约参数的一部分。

## 字节级核验方法

两条策略都使用 320 字节 ABI 编码 Order；解出 MakerTraits 后，程序均为 132 字节。程序包含：

- tx.origin access-token 检查；
- Aqua input protocol fee；
- `CONCENTRATE_GROW_LIQUIDITY_2D`；
- input flat fee；
- XYC swap；
- salt。

价格区间来自 concentrate 指令的两个 uint256：`sqrtPriceMin` 与 `sqrtPriceMax`。按 SwapVM 规则：

`P = tokenGt / tokenLt`，`rawP = sqrtPrice² / 1e18`，之后再按两种代币 decimals 转成人类价格。

WETH 策略：

- sqrtPriceMin：`6643432212485350`
- sqrtPriceMax：`6658680701432003`

WBTC 策略：

- sqrtPriceMin：`11356506233`
- sqrtPriceMax：`11428562042`
