# 香港原生 IP VPS：DMIT 香港机房三条线路怎么挑，CN2 GIA、Eyeball、Tier 1 全套餐价格和原生 IP 的真实成色

搜"香港原生 IP VPS"的人，多半已经踩过一次坑了。买回来的机器配置不差，价格也不贵，结果打开 Netflix 提示"检测到代理"，登录 PayPal 触发验证，TikTok 发出去的视频播放量卡在两位数。折腾一圈才发现：问题不在配置，在 IP。

下面把这件事从头讲清楚，再顺手把 DMIT 香港机房目前在售的线路和套餐摊开对比——它是少数几家把"原生 IP"当默认配置而不是卖点的机房商。

## 先把"原生 IP"四个字拆开

原生 IP 的判断标准很朴素：**IP 的注册归属地和机房实际所在地一致**。香港机房给你的 IP，WHOIS 查出来、GeoIP 库查出来、ASN 归属都是香港，这就是香港原生 IP。反过来，机器在香港，IP 却挂在荷兰或印度名下，那是通过 BGP 把别处的 IP 段"搬"过来广播的，叫广播 IP。

流媒体和支付平台判断你在哪，靠的不是你的服务器物理位置，而是 IP 的注册信息。IP 归属和机房位置对不上的时候，风控系统一眼就能看出这是"借来的地址"。

这里有个几乎所有教程都含糊过去的点：**原生不等于住宅。** 一个 IP 可以是香港原生（归属地对得上），同时仍然带着 Data Center 标签，因为它是机房向本地运营商申请的 IDC 段，不是 PCCW、HGC 那种从家宽线路里分出来的住宅 IP。原生只解决了"地址对不上"这一层问题，解决不了"这压根是台服务器"这一层。

自己验证也不难，登录机器跑一下 `curl ifconfig.me`，拿结果去 IP 归属查询站看 ASN 和属性标签，是不是原生、是不是机房段，一目了然。

## 香港机房的第二个变量：回程线路

同样标注"1 核 1G、20GB SSD"的香港 VPS，价格能差十倍。DMIT 香港的入门款 T1.TINY 是 $6.90/月，Pro.TINY 是 $39.90/月，配置几乎一样。差价全在流量怎么走。

从香港回大陆有三条路可走：走电信 CN2 GIA 精品线路、走移动 CMI 直连、或者走国际 Tier 1 骨干网。前两者延迟低、晚高峰不易堵；Tier 1 便宜、带宽大，但大陆访问没有专项优化，电信联通用户可能绕日本甚至绕美国再回来。第三方测评里，DMIT 香港 Pro 系列到大陆三网平均延迟约 43–49ms，这是 CN2 GIA 撑起来的结果，不是"香港离得近"自动获得的。

所以选香港 VPS 的顺序应该是：**先确认 IP 属性对不对，再确认线路给谁用。** 两台机器都是香港原生 IP，一台给国内用户用，一台给海外用户用，答案完全不同。

## DMIT 香港：三条线路，定位差在哪

DMIT 从 2017 年前后开始做 VPS，香港节点走的是 Equinix HK2，香港产品按路由分成三条线，硬件平台则有 AS3（AMD EPYC 7003）和 AN5（AMD EPYC 9005）两代。香港 Pro 常规款目前基本已经切到 AN5 平台，配 DDR5 内存。

| 线路 | 全称 | 大陆优化 | 适合谁 | 流量跑完会怎样 | 测试 IP |
| --- | --- | --- | --- | --- | --- |
| Premium | HKG.Pro | 三网回程 CN2 GIA，电信走香港 CTG 接国内 CN2 GIA，联通 AS4837+AS10099，移动 CMI 直连 | 访客、客户主要在大陆 | 暂停，等下个计费周期 | 103.117.100.199 |
| Eyeball | HKG.EB | 电信去程 CN2，联通移动去程走 NTT/Lumen 等国际线路；回程联通 CUG+CMI 动态负载、移动 CMI、电信 163 | 主要用户走移动、预算中等 | 暂停，等下个计费周期 | 154.12.188.3 |
| Tier 1 | HKG.T1 | 无。接入 RETN、NTT 等国际骨干，香港到欧洲延迟低 | 面向海外用户、大流量、自建中转 | 降速不限量，不掐线 | 154.12.176.28 |

三个测试 IP 可以先 ping 一轮再决定，比看任何测评都直接。

## 香港全套餐价格表

### Premium（HKG.AN5.Pro，AMD EPYC 9005）

三网回程走 CN2 GIA，全系 1Gbps 端口、1 个 IPv4 + 1 个 IPv6 /64、标准 DDoS 防护、KVM 虚拟化。这是 DMIT 香港最贵也最稳的一档。

| 套餐 | vCPU | 内存 | 存储 | 月流量 | 月付价格 | 购买 |
| --- | --- | --- | --- | --- | --- | --- |
| HKG.AN5.Pro.TINY | 1 核 | 1.0GB | 20GB SSD | 500GB | $39.90 | [ 选 TINY 入门款](https://www.dmit.io/cart.php?region=hong-kong&generation=an5&network=premium&aff=12046) |
| HKG.AN5.Pro.STARTER | 1 核 | 2.0GB | 40GB SSD | 1000GB | $79.90 | [ 选 STARTER](https://www.dmit.io/cart.php?region=hong-kong&generation=an5&network=premium&aff=12046) |
| HKG.AN5.Pro.MINI | 4 核 | 4.0GB | 80GB SSD | 1500GB | $149.90 | [ 选 MINI](https://www.dmit.io/cart.php?region=hong-kong&generation=an5&network=premium&aff=12046) |
| HKG.AN5.Pro.MICRO | 4 核 | 4.0GB | 160GB SSD | 2000GB | $199.90 | [ 选 MICRO](https://www.dmit.io/cart.php?region=hong-kong&generation=an5&network=premium&aff=12046) |
| HKG.AN5.Pro.MEDIUM | 6 核 | 8.0GB | 160GB SSD | 2500GB | $279.90 | [ 选 MEDIUM](https://www.dmit.io/cart.php?region=hong-kong&generation=an5&network=premium&aff=12046) |
| HKG.AN5.Pro.LARGE | 8 核 | 16.0GB | 320GB SSD | 3000GB | $359.90 | [ 选 LARGE](https://www.dmit.io/cart.php?region=hong-kong&generation=an5&network=premium&aff=12046) |
| HKG.AN5.Pro.GIANT | 12 核 | 24.0GB | 640GB SSD | 6000GB | $759.90 | [ 选 GIANT](https://www.dmit.io/cart.php?region=hong-kong&generation=an5&network=premium&aff=12046) |

MINI 和 MICRO 都是 4 核 4G，区别只在硬盘（80GB 对 160GB）和月流量（1500GB 对 2000GB），差价 $50。如果你只是跑个站点或几个容器，多出来的 500GB 流量未必值这 $50；真要跑数据库或者存日志，那 80GB 的硬盘空间才是分水岭。

### Premium（HKG.AS3.Pro，AMD EPYC 7003）

AS3 平台的常规款正在陆续切换成 AN5，目前购物车里还能看到的两档：

| 套餐 | vCPU | 内存 | 存储 | 月流量 | 月付价格 | 购买 |
| --- | --- | --- | --- | --- | --- | --- |
| HKG.AS3.Pro.TINY | 1 核 | 1.0GB | 20GB SSD | 500GB | $39.90 | [ 查看 AS3 平台 Pro 套餐](https://www.dmit.io/cart.php?region=hong-kong&generation=as3&network=premium&aff=12046) |
| HKG.AS3.Pro.STARTER | 1 核 | 2.0GB | 40GB SSD | 1000GB | $79.90 | [ 查看 AS3 平台 Pro 套餐](https://www.dmit.io/cart.php?region=hong-kong&generation=as3&network=premium&aff=12046) |

价格和同名的 AN5 款一模一样。既然一样贵，能选 AN5 就没理由选 AS3。

### Tier 1（HKG.AS3.T1，国际线路）

这一档的存在意义就是便宜和大流量。跟 Pro 系列最大的区别不是配置，是超量后的处理方式：T1 流量跑完不掐线，降到 50Mbps–1Gbps 继续不限量跑。对流量不好预估的业务来说，这条规则比省下来的几美元值钱。

| 套餐 | vCPU | 内存 | 存储 | 月流量 | 价格 | 购买 |
| --- | --- | --- | --- | --- | --- | --- |
| HKG.T1.WEE | 1 核 | 1GB | 20GB SSD | 1TB | $36.90/年 | [ 拿年付最便宜的那台](https://www.dmit.io/aff.php?aff=12046) |
| HKG.T1.TINY | 1 核 | 1GB | 20GB SSD | 2TB | $6.90/月，$76.90/年 | [ 选 T1.TINY](https://www.dmit.io/aff.php?aff=12046) |
| HKG.T1.STARTER | 1 核 | 2GB | 40GB SSD | 4TB | $12.90/月，$154.80/年 | [ 选 T1.STARTER](https://www.dmit.io/aff.php?aff=12046) |
| HKG.T1.MINI | 2 核 | 2GB | 60GB SSD | 8TB | $21.90/月，$262.80/年 | [ 选 T1.MINI](https://www.dmit.io/aff.php?aff=12046) |
| HKG.T1.MICRO | 4 核 | 4GB | 80GB SSD | 16TB | $32.90/月 | [ 选 T1.MICRO](https://www.dmit.io/aff.php?aff=12046) |
| HKG.T1.MEDIUM | 4 核 | 8GB | 160GB SSD | 32TB | $49.90/月 | [ 选 T1.MEDIUM](https://www.dmit.io/aff.php?aff=12046) |
| HKG.T1.LARGE | 8 核 | 16GB | 320GB SSD | 64TB | $99.90/月 | [ 选 T1.LARGE](https://www.dmit.io/aff.php?aff=12046) |
| HKG.T1.GIANT | 8 核 | 24GB | 640GB SSD | 128TB | $199.90/月 | [ 选 T1.GIANT](https://www.dmit.io/aff.php?aff=12046) |

端口带宽从 4Gbps 起步，中高配到 10Gbps。$12.90 一个月换 4TB 流量和 10Gbps 端口，这个价位放在香港节点上确实不多见——代价是大陆访问没有优化，别指望它。

### Eyeball（HKG.EB，中间档）

定位在 Pro 和 T1 之间：移动用户双向 CMI 直连体验不错，电信联通去程绕日本，回程才直连。

需要提醒一句，DMIT 官方把 HKG.EB 标注为 Beta，明确说明线路和性能仍在调整，不建议用在稳定性要求高的生产环境上。目前这个系列多数时候处于缺货状态，下面价格是补货时的公开报价，以官网实际显示为准。

| 套餐 | vCPU | 内存 | 存储 | 月流量 | 端口 | 月付价格 | 购买 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HKG.AS3.EB.WEE | 1 核 | 1GB | 20GB SSD | 500GB | 500Mbps | $24.90 | [ 看 Eyeball 是否补货](https://www.dmit.io/aff.php?aff=12046) |
| HKG.AS3.EB.TINY | 1 核 | 1GB | 20GB SSD | 800GB | 1Gbps | $39.90 | [ 看 Eyeball 是否补货](https://www.dmit.io/aff.php?aff=12046) |
| HKG.AS3.EB.STARTER | 1 核 | 2GB | 40GB SSD | 1.5TB | 1Gbps | $79.90 | [ 看 Eyeball 是否补货](https://www.dmit.io/aff.php?aff=12046) |
| HKG.AS3.EB.MINI | 2 核 | 4GB | 60GB SSD | 2.2TB | 1Gbps | $126.90 | [ 看 Eyeball 是否补货](https://www.dmit.io/aff.php?aff=12046) |
| HKG.AS3.EB.MICRO | 4 核 | 4GB | 80GB SSD | 3TB | 1Gbps | $179.90 | [ 看 Eyeball 是否补货](https://www.dmit.io/aff.php?aff=12046) |
| HKG.AS3.EB.MEDIUM | 4 核 | 8GB | 160GB SSD | 4TB | 1Gbps | $239.90 | [ 看 Eyeball 是否补货](https://www.dmit.io/aff.php?aff=12046) |
| HKG.AN5.EB.MINI | 4 核 | 4GB | 80GB SSD | 2.2TB | 1Gbps | $149.90 | [ 看 Eyeball 是否补货](https://www.dmit.io/aff.php?aff=12046) |
| HKG.AN5.EB.MICRO | 4 核 | 4GB | 160GB SSD | 3TB | 1Gbps | $199.90 | [ 看 Eyeball 是否补货](https://www.dmit.io/aff.php?aff=12046) |
| HKG.AN5.EB.MEDIUM | 6 核 | 8GB | 160GB SSD | 4TB | 1Gbps | $279.90 | [ 看 Eyeball 是否补货](https://www.dmit.io/aff.php?aff=12046) |
| HKG.AN5.EB.LARGE | 8 核 | 16GB | 320GB SSD | 4.5TB | 1Gbps | $359.90 | [ 看 Eyeball 是否补货](https://www.dmit.io/aff.php?aff=12046) |
| HKG.AN5.EB.GIANT | 12 核 | 24GB | 640GB SSD | 9TB | 1Gbps | $759.90 | [ 看 Eyeball 是否补货](https://www.dmit.io/aff.php?aff=12046) |

## 原生 IP 的成色：能做什么，不能做什么

第三方测评对 DMIT 香港 IP 做过归属查询，结论是：香港原生、归属 DMIT 自家 ASN、IP 库标签为 IDC 数据中心、非住宅、风险评级偏低。也就是说，你在"原生"这件事上拿到的是真货，但它依然是一台机房机器的 IP。

具体到使用：

- **港区流媒体**：有测评实测能解锁香港本地 TikTok、Disney+ 等内容，YouTube 跑 4K 没问题。
- **TVB、myTV SUPER 这类本地平台**：这类服务对住宅属性更敏感，机房 IP 能不能过要看平台当期策略，不是一个稳定承诺。
- **跨境电商、广告投放**：IP 干净、被滥用历史少，比随便找的便宜机器稳。
- **AI 类服务**：机房的香港 IP 偶尔会触发人机验证，这一点所有 IDC 商都躲不掉。

DMIT 官方对解锁能力不做保证，这一点它写在明面上。另外有第三方资料提到 IP 被墙后的更换规则（大致是 15 天可免费换一次、其他情况 $5 一次），但这类细则商家会随时调整，下单前用测试 IP 试一遍、必要时开个工单问清楚，比看任何汇总表都靠谱。

## 四种人，四种选法

**国内访客为主的网站或业务。** 只有一个答案：HKG.Pro。CN2 GIA 的回程是花在延迟和丢包上的钱，晚高峰体验差一档就是差一档。预算紧张的话从 TINY 起步，$39.90/月。

**面向欧美、东南亚用户，或自建中转。** HKG.T1。国际线路接 RETN、NTT，香港到欧洲的延迟比同类便宜机房好；超量降速不断线，跑大流量业务心里有底。$6.90/月或 $36.90/年入场，试错成本极低。

**主要用户在移动网络。** HKG.EB 理论上最对路，但得接受两件事：官方 Beta 标记，以及长期缺货。抢不到就老实上 Pro。

**预算极低、只想拿个香港 IP 做测试。** T1.WEE，$36.90 一年。大陆访问别抱期待，但作为一台干净的原生香港 IP 落地机，这个价格没什么好挑的。

## 下单前值得确认的几件事

- **付款方式**：支付宝、微信、PayPal、信用卡都能用，国内用户不需要折腾外币卡。
- **库存**：香港 Pro 和 T1 长期处于"补一批、卖一阵、再缺货"的节奏，看中某档别拖太久。
- **支持语言**：有测评提到 DMIT 已取消独立中文支持团队，统一改走英文工单。语言不通的话，问题描述尽量写清楚。
- **优惠码**：网上流传过若干香港 T1 年付折扣码，但多个渠道的信息都指向香港 T1 本身就是特价、没有通用可用码。与其信汇总帖，不如在结算页自己贴一次码验证——不生效就是不生效。

香港原生 IP 这件事，本质上没有黑科技：IP 归属对得上、线路给你真正的用户群、超量规则别坑你。三条都满足了，剩下的就是配置和预算的匹配问题。DMIT 香港这套产品线，价格不算便宜，但把这三件事都摆在明面上讲清楚了——[👉 打开官网购物车看香港三条线路的实时库存和价格](https://www.dmit.io/aff.php?aff=12046)，比看十篇测评快。
