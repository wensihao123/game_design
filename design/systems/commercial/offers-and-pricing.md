# Offers and Pricing（优惠与定价系统）

> Status: V1  
> Category: Commercial  
> Path: `design/systems/commercial/offers-and-pricing.md`  
> Owner: TBD  
> Reviewers: Product / Economy / Finance / Legal / Tax / Engineering / Data / UX / Accessibility / Privacy / Trust and Safety / Support / Live Operations  
> Last Updated: 2026-07-11  
> Version: 1.0  
> Risk Level: Critical  
> Dependencies: Monetization System, Resources and Economy, Reward System, Content and Unlocks, Entitlement and Ownership, Settings and Preferences, Notification and Reminders, Live Operations, Experiment Management  
> Affected Systems: Storefront, Subscription, Battle Pass, Randomized Purchases, DLC, Gift, Advertising, Analytics and Telemetry, Support, Finance

---

## 1. System Summary

Offers and Pricing 系统负责定义：

```text
商品在什么条件下，以什么价格，向谁，在什么时间和渠道展示；
基础价、地区价、税费、汇率、折扣、礼包和优惠如何计算；
Offer 如何引用真实 Product、Price 和 Entitlement；
价格如何被快照、确认、版本化、审计和回滚；
个性化与实验可以改变什么，不能改变什么；
倒计时、稀缺、原价和折扣如何保持真实；
已拥有内容、重复商品、跨平台价格和退款如何处理；
玩家如何清楚理解总价、期限、所有权和节省金额。
```

该系统通常覆盖：

- Product Catalog；
- Storefront；
- Base Price；
- Regional Price；
- Platform Price；
- Tax；
- Currency；
- Exchange Rate；
- Price Tier；
- Price Book；
- Price Version；
- Discount；
- Promotion；
- Coupon；
- Bundle；
- Dynamic Bundle；
- Upgrade Price；
- Proration；
- Subscription Price；
- Trial Price；
- Introductory Price；
- Renewal Price；
- Personalized Offer；
- Eligibility；
- Targeting；
- Frequency；
- Countdown；
- Scarcity；
- Strikethrough Price；
- Price Snapshot；
- Checkout Price；
- Price Experiment；
- Price Change；
- Price Migration；
- Price Correction；
- Offer Retirement；
- Offer Analytics。

健康的优惠与定价系统应让玩家感受到：

```text
价格是真实且可理解的；
折扣不是虚构的；
优惠条件清楚；
我不会因为不知道规则而多付；
同一结账流程中的价格不会突然变化；
已拥有内容不会让我重复付费而没有价值；
个性化不会根据我的脆弱状态秘密涨价；
倒计时和稀缺都是真实的；
跨平台差异有明确说明。
```

---

## 2. Purpose

### 2.1 Player Value

该系统帮助玩家：

- 清楚理解商品价值；
- 比较单品和礼包；
- 知道折扣是否真实；
- 了解价格是否含税；
- 了解优惠何时结束；
- 知道自己是否符合资格；
- 避免重复购买；
- 知道已拥有内容如何影响礼包；
- 理解订阅升级、降级和按比例计费；
- 在价格变化后仍保留已确认结账价格；
- 看到适合自己的优惠，同时不被秘密操纵；
- 对错误价格和错误折扣获得修正。

### 2.2 Experience Contribution

优惠与定价直接影响：

- 信任；
- 购买理解；
- 价值感；
- 转化；
- 留存；
- 退款；
- Support 压力；
- 法律风险；
- 品牌声誉；
- 长期商业健康。

不健康的系统会造成：

- 虚假原价；
- 虚假倒计时；
- 同一商品多种难以解释价格；
- 个性化价格歧视；
- 结账时加价；
- 税费最后出现；
- 货币换算误导；
- 已拥有商品重复收费；
- 优惠资格不透明；
- 隐藏续费价；
- 平台价差争议；
- Bundle 节省金额错误；
- 促销结束后仍显示折扣；
- 价格实验覆盖已确认订单。

### 2.3 Product Value

统一系统可以：

- 维护单一 Product Catalog；
- 统一 Price Book；
- 支持多地区和多平台；
- 支持税务和汇率；
- 支持折扣和礼包；
- 支持订阅 Proration；
- 支持个性化但受伦理约束；
- 支持 Live Operations；
- 支持实验；
- 支持价格回滚；
- 支持财务审计；
- 降低 Storefront 重复逻辑；
- 提高 Checkout 一致性；
- 降低错误价格事故。

### 2.4 Why This System Exists

如果各页面自行计算价格，常见结果是：

```text
商品页显示 9.99，结账页显示 11.49；
折扣使用一个不存在的历史原价；
礼包“节省 40%”但实际单品总价不同；
玩家已拥有一半礼包内容仍支付全价；
同一账号不同设备看到不同价格；
价格实验影响同一结账中的最终金额；
税费直到最后一步才出现；
订阅升级立即收全价但不退剩余期间；
退款后价格资格未恢复；
倒计时结束后自动重置；
地区切换后付费货币和商品价格错位；
Support 无法判断玩家当时看到的真实价格。
```

---

## 3. Non-Goals

该系统不负责：

- 代替 Monetization System 完成支付；
- 代替 Entitlement 判断最终所有权；
- 代替 Resources and Economy 管理货币余额；
- 代替 Live Operations 管理全部活动；
- 通过复杂定价掩盖真实成本；
- 根据玩家失败、孤独、疲劳或危机状态秘密提高价格；
- 将付费能力作为价格变量；
- 对受保护群体进行歧视性定价；
- 使用虚假库存、虚假倒计时或虚假原价；
- 让客户端成为价格权威；
- 在 Checkout 已确认后静默改变价格；
- 让每个渠道自行定义不同 Product；
- 以实验为由绕过法律、财务和伦理审查；
- 让 Discount 逻辑直接发放 Entitlement；
- 用随机价格让玩家无法比较价值。

---

## 4. Governing Principles

### 4.1 Player First Design

参考：

- `../../philosophy/foundation/player-first-design.md`

应用原则：

- 总价优先；
- 商品内容和期限清楚；
- 已拥有内容可见；
- 关闭和返回容易；
- 错误价格有保护；
- 不通过复杂流程迫使购买。

### 4.2 Clarity and Feedback

参考：

- `../../philosophy/experience/clarity-and-feedback.md`

应用原则：

- Base Price、Discount、Tax、Total 分离；
- 节省金额可验证；
- Offer 结束和资格清楚；
- Price Change 有提前通知；
- Checkout Price Snapshot 清楚。

### 4.3 Consistency and Coherence

参考：

- `../../philosophy/long-term/consistency-and-coherence.md`

应用原则：

- 同一 Product 使用稳定 ID；
- 同一价格版本跨入口一致；
- Storefront 和 Checkout 价格一致；
- Offer、Product、SKU 和 Entitlement 不混淆。

### 4.4 Ethical Design

参考：

- `../../philosophy/responsibility/ethical-design.md`

应用原则：

- 不使用虚假折扣；
- 不做脆弱状态定价；
- 不利用儿童；
- 不用隐藏价格歧视；
- 个性化应基于价值相关条件，而非心理脆弱性；
- 倒计时必须真实；
- 促销频率克制。

### 4.5 Accessibility and Inclusivity

参考：

- `../../philosophy/responsibility/accessibility-and-inclusivity.md`

应用原则：

- 价格和折扣可读；
- 不只靠颜色表达优惠；
- 屏幕阅读器可理解；
- 大字体不隐藏总价；
- 货币和小数本地化正确；
- 不使用仅靠快速点击获取的优惠。

---

## 5. Player Experience

### 5.1 Player Goal

玩家使用优惠与定价系统通常为了：

- 浏览商品；
- 比较价格；
- 了解折扣；
- 查看礼包；
- 判断是否值得购买；
- 使用 Coupon；
- 查看订阅价格；
- 查看升级差价；
- 查看税费；
- 查看优惠期限；
- 查看已拥有内容；
- 确认结账价格；
- 理解价格变化；
- 报告价格错误。

### 5.2 Entry

入口包括：

- Store；
- Product Detail；
- Character Page；
- Cosmetic Page；
- Battle Pass；
- Subscription；
- DLC；
- Event；
- Offer Banner；
- Notification；
- Account；
- Gift；
- Checkout；
- Platform Store。

### 5.3 Main Actions

玩家可以：

- Browse；
- Filter；
- Sort；
- Preview；
- Compare；
- Apply Coupon；
- Remove Coupon；
- Select Bundle；
- Select Tier；
- Upgrade；
- Downgrade；
- View Tax；
- View Price History Summary；
- Hide Offer；
- Dismiss；
- Add to Cart；
- Checkout；
- Report Issue。

### 5.4 Core Decisions

关键决策包括：

- 单品还是礼包；
- 现在购买还是以后；
- 使用哪种币种；
- 是否订阅；
- 是否升级；
- 是否接受限时优惠；
- 是否使用 Coupon；
- 是否购买重复内容；
- 是否购买地区或平台特定商品；
- 是否接受自动续费后的正常价格。

### 5.5 Success

健康体验意味着：

- 价格前后一致；
- 总价清楚；
- 折扣真实；
- Offer 条件清楚；
- 已拥有内容正确处理；
- 订阅和升级价可理解；
- 倒计时真实；
- 价格实验不影响已确认结账；
- 错误价格可追踪和修正；
- 跨平台差异有说明。

### 5.6 Failure

失败包括：

- Price Mismatch；
- Discount Mismatch；
- Tax Mismatch；
- Currency Mismatch；
- Expired Offer；
- Invalid Coupon；
- Owned Item Miscalculation；
- Bundle Savings Error；
- Upgrade Proration Error；
- Price Snapshot Missing；
- Region Mapping Error；
- Experiment Conflict；
- Checkout Price Changed；
- Store Cache Stale；
- False Countdown。

---

## 6. System Boundary

### 6.1 Inputs

系统接收：

- Product Catalog；
- SKU Mapping；
- Base Price；
- Price Book；
- Region；
- Platform；
- Currency；
- Tax Rules；
- Exchange Rate；
- Offer Definition；
- Coupon；
- Eligibility；
- Entitlement；
- Owned Items；
- Account；
- Age / Family；
- Subscription State；
- Purchase History；
- Live Operations Schedule；
- Experiment Assignment；
- Legal Rules；
- Inventory / Availability；
- Time；
- Device；
- Channel。

### 6.2 Outputs

系统产生：

- Price Quote；
- Price Breakdown；
- Offer Instance；
- Eligibility Result；
- Bundle Quote；
- Upgrade Quote；
- Subscription Quote；
- Discount Breakdown；
- Tax Breakdown；
- Price Snapshot；
- Checkout Price；
- Expiry；
- Price Error；
- Price Change Notification；
- Offer Exposure；
- Pricing Audit Event。

### 6.3 Owned State

系统拥有：

- Price Definition；
- Price Book；
- Price Version；
- Offer Definition；
- Offer Instance；
- Discount Rule；
- Coupon Definition；
- Bundle Pricing Rule；
- Proration Rule；
- Eligibility Rule；
- Price Quote；
- Price Snapshot；
- Price Experiment Assignment Reference；
- Offer Frequency State；
- Offer History；
- Pricing Audit；
- Pricing Version。

### 6.4 Read-Only Dependencies

系统读取：

- Product Catalog；
- Entitlement；
- Monetization；
- Economy；
- Account；
- Family；
- Platform；
- Tax；
- Live Operations；
- Experiment；
- Save；
- Time；
- Legal；
- Analytics。

### 6.5 Write Dependencies

系统通过正式契约请求：

- Monetization 使用 Price Snapshot；
- Storefront 展示 Offer；
- Entitlement 提供已拥有状态；
- Account 保存 Coupon / Frequency；
- Notification 发送真实价格变化；
- Live Operations 启停 Offer；
- Analytics 记录曝光和结果；
- Support 查看价格快照；
- Finance 读取 Pricing Audit。

### 6.6 Out of Scope

系统不直接：

- 扣款；
- 发货；
- 创建 Entitlement；
- 修改 Paid Currency；
- 计算 Match Result；
- 修改 Difficulty；
- 绕过 Family Control；
- 通过客户端价格直接生成订单；
- 将 Analytics 转化事件视为价格权威。

---

## 7. Core Entities and Concepts

| Entity / Concept | Definition | Owner | Lifetime | Notes |
|---|---|---|---|---|
| Product | 被定价的商品对象 | Catalog | 长期 | 稳定 Product ID |
| SKU | 渠道或平台具体销售单元 | Commercial | 版本级 | 可映射 Product |
| Price Book | 某地区、平台和币种下的价格集合 | Pricing | 版本级 | 可灰度发布 |
| Price Definition | 一个具体基础价格规则 | Pricing | 版本级 | 有 Price ID |
| Price Version | 价格规则版本 | Pricing | 长期 | 审计 |
| Price Quote | 某时刻计算出的可展示价格 | Pricing | 短期 | 可失效 |
| Price Snapshot | Checkout 固定价格快照 | Pricing | Order 期 | 不可静默改变 |
| Offer Definition | 优惠规则模板 | Offers | 版本级 | 有 Offer ID |
| Offer Instance | 针对某玩家和时刻的具体优惠 | Offers | 短期 | 有 Eligibility |
| Discount Rule | 折扣计算规则 | Pricing | 版本级 | 百分比或固定额 |
| Coupon | 可兑换价格优惠 | Pricing | 有效期 | 有使用限制 |
| Bundle | 多个 Product 组合销售 | Catalog / Offers | 版本级 | 内容可变需版本 |
| Bundle Quote | 考虑已拥有内容后的报价 | Pricing | 短期 | 不等于 Product |
| Proration | 按剩余时间或价值计算差额 | Pricing | Quote 级 | 订阅常用 |
| Eligibility Rule | Offer 适用条件 | Offers | 版本级 | 可解释 |
| Reference Price | 用于对比的合法原价 | Pricing | 版本级 | 必须真实 |
| Savings | 相对 Reference Price 的节省 | Pricing | Quote 级 | 可验证 |
| Scarcity State | 库存或时间限制状态 | Offers | 短期 | 必须真实 |
| Frequency State | 玩家接触优惠的频率状态 | Offers | 周期级 | 防骚扰 |
| Pricing Audit | 价格和优惠的审计记录 | Pricing | 财务期 | 高权限 |

---

## 8. Pricing Taxonomy

### 8.1 Base Price

商品在无优惠情况下的标准价格。

### 8.2 Regional Price

根据地区设定的本地价格。

### 8.3 Platform Price

平台特定价格。

### 8.4 Tax-Inclusive Price

含税显示。

### 8.5 Tax-Exclusive Price

税费另加，但必须在法律和界面要求下清楚。

### 8.6 Introductory Price

首次或早期优惠价格。

### 8.7 Promotional Price

促销期间价格。

### 8.8 Subscription Price

周期价格。

### 8.9 Renewal Price

后续续费价格。

### 8.10 Upgrade Price

从当前版本升级的价格。

### 8.11 Dynamic Bundle Price

根据已拥有内容动态计算。

### 8.12 Gift Price

赠礼购买价格。

### 8.13 Creator Price

创作者市场定价。

### 8.14 Free

价格为零，但不应隐藏后续收费或订阅。

---

## 9. Price Definition Template

```markdown
## Price Definition

- Price ID:
- Product ID:
- SKU:
- Region:
- Platform:
- Currency:
- Amount:
- Tax Mode:
- Price Tier:
- Effective From:
- Effective Until:
- Reference Price:
- Exchange Rate Source:
- Rounding Rule:
- Age / Legal Notes:
- Version:
- Owner:
- Risk Level:
```

### 9.1 必须回答

- 给什么 Product 定价；
- 哪个平台；
- 哪个地区；
- 哪种币种；
- 是否含税；
- 生效时间；
- 参考价；
- 舍入规则；
- 是否续费价；
- 是否与其他价格互斥；
- 是否需要法律审查。

---

## 10. Price Book

Price Book 是：

```text
一组在特定平台、地区、币种和时间范围内有效的价格定义集合。
```

### 10.1 Price Book Fields

- Price Book ID；
- Region；
- Platform；
- Currency；
- Tax Policy；
- Effective Window；
- Product Prices；
- Version；
- Status；
- Approval；
- Rollback Version。

### 10.2 Price Book Lifecycle

```text
Draft
→ Reviewed
→ Approved
→ Scheduled
→ Active
→ Superseded
→ Retired
```

### 10.3 Invariants

1. 同一 Product、Region、Platform、Time 只能有一个有效 Base Price。
2. Price Book 发布必须版本化。
3. Active Price Book 必须有 Last Known Good。
4. Checkout 使用的 Snapshot 不受后续 Price Book 变化影响。
5. 无有效价格时不能猜测价格。
6. 客户端缓存不能成为价格权威。

---

## 11. Offer Taxonomy

### 11.1 Storewide Promotion

覆盖大量商品。

### 11.2 Product Promotion

单一 Product 优惠。

### 11.3 Category Promotion

某类商品优惠。

### 11.4 Bundle Offer

多商品组合优惠。

### 11.5 Upgrade Offer

已有低版本用户升级。

### 11.6 Introductory Offer

首次购买或首次订阅。

### 11.7 Loyalty Offer

基于长期拥有或参与历史。

### 11.8 Return Offer

面向回归玩家。

### 11.9 Event Offer

与真实活动相关。

### 11.10 Coupon Offer

使用 Code 或 Voucher。

### 11.11 Trial Offer

试用或首期优惠。

### 11.12 Gift Offer

赠礼相关。

### 11.13 Creator Offer

创作者分成市场。

### 11.14 Personalized Offer

基于明确、合理和允许的玩家状态生成。

---

## 12. Offer Definition Template

```markdown
## Offer Definition

- Offer ID:
- Display Name:
- Offer Type:
- Products:
- Pricing Rule:
- Reference Price:
- Start:
- End:
- Time Zone:
- Eligibility:
- Exclusions:
- Usage Limit:
- Frequency Limit:
- Channels:
- Owned Item Handling:
- Countdown:
- Scarcity:
- Coupon:
- Experiment:
- Legal Review:
- Version:
- Owner:
- Risk Level:
```

---

## 13. Offer Lifecycle

```text
Draft
→ Reviewed
→ Approved
→ Scheduled
→ Eligible
→ Active
→ Expiring
→ Ended
→ Archived
```

异常：

```text
Paused
Cancelled
Invalidated
Sold Out
Corrected
Rolled Back
```

### 13.1 Offer State Definitions

#### Draft

尚未发布。

#### Reviewed

完成业务、法律、财务和伦理审查。

#### Approved

允许发布。

#### Scheduled

等待开始。

#### Eligible

对特定玩家满足资格。

#### Active

可以展示和购买。

#### Expiring

接近真实结束。

#### Ended

不再创建新 Price Quote。

#### Sold Out

真实库存耗尽。

#### Invalidated

因错误或政策停止。

#### Corrected

价格或内容已修正。

---

## 14. Offer Invariants

1. Offer 必须引用真实 Product 和 Price Rule。
2. Offer Ended 后不能创建新 Checkout Snapshot。
3. 已创建且未过期的 Checkout Snapshot 是否继续有效必须由政策明确。
4. Offer 不能绕过年龄、地区、支出和家长限制。
5. Countdown 必须来自真实结束时间。
6. Reference Price 必须合法且真实。
7. Savings 必须可由当前 Quote 计算。
8. Eligibility 失败必须返回原因。
9. Experiment 不能改变已确认 Snapshot。
10. 已关闭商业通知不等于不能在 Store 中看到正常商品，但不应主动推送。
11. Offer 不能基于玩家脆弱状态秘密加价。
12. 同一 Offer Instance 必须幂等。
13. Offer Exposure 不得自动生成购买。
14. Sold Out 只能用于真实库存限制。
15. Analytics 失败不影响 Offer 资格和价格。

---

## 15. Price Quote

Price Quote 是：

```text
针对某一玩家、商品、平台、地区和时间生成的可展示价格结果。
```

### 15.1 Quote Fields

- Quote ID；
- Product；
- SKU；
- Offer；
- Account；
- Region；
- Platform；
- Currency；
- Base Price；
- Discount；
- Tax；
- Total；
- Reference Price；
- Savings；
- Owned Items；
- Eligibility；
- Created At；
- Expiry；
- Price Version；
- Offer Version；
- Signature。

### 15.2 Quote Lifetime

Quote 是短期的。

适用于：

- 商品页；
- Cart；
- Bundle；
- Upgrade；
- Subscription；
- Gift。

### 15.3 Quote vs Snapshot

Quote 可刷新。

Snapshot 用于 Checkout 和 Order，固定且不可静默改变。

---

## 16. Price Snapshot

### 16.1 Purpose

确保玩家确认结账时：

- 商品；
- 数量；
- Price；
- Discount；
- Tax；
- Total；
- Subscription；
- Duration；
- Offer；

固定。

### 16.2 Snapshot Fields

- Snapshot ID；
- Quote ID；
- Product；
- SKU；
- Account；
- Region；
- Platform；
- Currency；
- Unit Price；
- Quantity；
- Discount Lines；
- Tax Lines；
- Total；
- Reference Price；
- Offer；
- Ownership；
- Expiry；
- Version；
- Signature。

### 16.3 Snapshot Invariants

1. Checkout 只能使用有效 Snapshot。
2. Snapshot 过期后必须重新报价。
3. Snapshot 不能被客户端修改。
4. Order 必须引用 Snapshot。
5. 实验和 Live Config 变化不影响已创建 Snapshot。
6. Snapshot 与实际支付金额必须对账。
7. Snapshot 应保留到财务审计期。

---

## 17. Base Price

### 17.1 Base Price Purpose

建立长期可比较的标准价格。

### 17.2 Base Price Governance

修改 Base Price 需要：

- Finance；
- Product；
- Legal；
- Tax；
- Platform；
- Region；
- Analytics；

审查。

### 17.3 No Artificial Inflation

不能短期抬高 Base Price 只为制造折扣。

### 17.4 Reference Window

若使用历史最低价或常规价作为参考，需按地区法律定义。

---

## 18. Regional Pricing

### 18.1 Inputs

可以考虑：

- Purchasing Power；
- Local Market；
- Tax；
- Platform Tier；
- Currency Stability；
- Distribution Cost；
- Regulation；
- Competitive Landscape。

### 18.2 Fairness

地区价格差异应基于合理市场条件，而不是个人经济画像。

### 18.3 Region Determination

来源可以是：

- Store Region；
- Billing Country；
- Account Region；
- Legal Residence；
- Platform Region。

必须明确优先级。

### 18.4 Region Change

处理：

- 价格更新；
- 支付方式；
- Paid Currency；
- Entitlement；
- Gift；
- Subscription；
- Tax；
- Anti-Arbitrage。

### 18.5 No Precision Geolocation Pricing

不应根据精确地理位置动态变化价格。

---

## 19. Currency

### 19.1 Display Currency

玩家看到的币种。

### 19.2 Settlement Currency

支付结算币种。

### 19.3 Internal Currency

平台或产品内部货币。

### 19.4 Conversion

若存在换算，显示：

- 汇率来源；
- 更新时间；
- 手续费；
- 最终结算币种；
- 可能的银行费用。

### 19.5 No Hidden Conversion

不能只显示低价外币，结账才显示本币高价。

---

## 20. Exchange Rate

### 20.1 Sources

- Platform Fixed Tier；
- Finance Approved Rate；
- Payment Provider；
- Central Bank Reference；
- Market Rate with Buffer。

### 20.2 Update Frequency

根据币种波动决定。

### 20.3 Rounding

各币种定义：

- 小数位；
- 商业定价结尾；
- 税；
- 汇率；
- 最低单位。

### 20.4 Volatility

高波动币种可以：

- 缩短 Quote 有效期；
- 提高更新频率；
- 暂停销售；
- 使用平台 Price Tier。

---

## 21. Tax

### 21.1 Tax Modes

- Included；
- Added at Checkout；
- Platform Managed；
- Seller Managed；
- Tax Exempt；
- Reverse Charge。

### 21.2 Tax Breakdown

在法律允许和要求下展示：

- Subtotal；
- Tax；
- Total；
- Tax Type；
- Region。

### 21.3 Tax Location

依据：

- Billing Address；
- Store Region；
- Platform；
- Legal Rules。

### 21.4 Tax Change

Snapshot 后税规则变化是否影响当前 Checkout，需按法律和 Provider 规则处理。

---

## 22. Discount Taxonomy

### 22.1 Percentage Discount

按百分比。

### 22.2 Fixed Amount Discount

减固定金额。

### 22.3 Buy X Get Y

买 X 得 Y。

### 22.4 Tiered Discount

购买数量越多折扣越大。

### 22.5 Bundle Discount

组合购买优惠。

### 22.6 Loyalty Discount

基于已有合法关系。

### 22.7 Upgrade Discount

已有版本升级。

### 22.8 Introductory Discount

首次购买。

### 22.9 Subscription Discount

首期或年付优惠。

### 22.10 Coupon Discount

使用兑换码。

### 22.11 Platform-Funded Discount

平台承担部分成本。

### 22.12 Creator Discount

创作者或渠道活动。

---

## 23. Discount Rule

Discount Rule 应包含：

- Rule ID；
- Scope；
- Method；
- Value；
- Minimum Price；
- Maximum Discount；
- Stacking；
- Eligibility；
- Start；
- End；
- Usage Limit；
- Product Exclusion；
- Region；
- Platform；
- Currency；
- Tax Treatment；
- Version。

### 23.1 Discount Floor

定义最低成交价。

### 23.2 Stacking

明确：

- 是否叠加；
- 顺序；
- 哪个优先；
- 上限；
- Coupon 和 Storewide 的关系。

### 23.3 Discount Order

例如：

```text
Base Price
→ Product Discount
→ Bundle Discount
→ Coupon
→ Tax
```

顺序必须显式。

---

## 24. Reference Price and Strikethrough Price

### 24.1 Reference Price

用于展示“原价”或“划线价”。

### 24.2 Valid Reference Sources

可能包括：

- 当前正常 Base Price；
- 最近合法常规价；
- 单品总价；
- 订阅标准续费价；
- 平台标准价。

### 24.3 Invalid Reference Sources

不应使用：

- 从未销售过的价格；
- 临时抬高价格；
- 不同地区价格；
- 不同平台价格；
- 不同商品；
- 已过期且不合法的历史价；
- 理论价值。

### 24.4 Savings

```text
Savings = Reference Price - Offer Total
```

百分比按地区法规和舍入规则计算。

### 24.5 Honest Display

若 Reference Price 不适用，不显示虚假折扣。

---

## 25. Coupons

### 25.1 Coupon Types

- Public Code；
- One-Time Code；
- Account-Bound；
- Product-Specific；
- Category；
- Subscription；
- Recovery Credit；
- Partner；
- Gift；
- Support Issued。

### 25.2 Coupon Fields

- Coupon ID；
- Code Hash；
- Discount；
- Products；
- Account；
- Region；
- Platform；
- Start；
- End；
- Usage Limit；
- Per-Account Limit；
- Stack Policy；
- Minimum Spend；
- Age；
- Fraud Policy；
- Version。

### 25.3 Coupon Security

- 不存明文必要时哈希；
- Rate Limit；
- 防枚举；
- 防重放；
- 防转卖；
- 审计；
- 失效；
- 撤销。

### 25.4 Invalid Coupon Feedback

说明：

- 已过期；
- 不适用；
- 已使用；
- 地区错误；
- 商品错误；
- 未达最低金额。

---

## 26. Bundles

### 26.1 Bundle Types

- Static Bundle；
- Dynamic Bundle；
- Build-Your-Own；
- Upgrade Bundle；
- Starter Bundle；
- Event Bundle；
- Subscription Bundle；
- Cross-Product Bundle。

### 26.2 Bundle Definition

- Bundle ID；
- Products；
- Quantities；
- Required Items；
- Optional Items；
- Base Sum；
- Bundle Price；
- Owned Item Policy；
- Duplicate Policy；
- Entitlement；
- Refund；
- Version。

### 26.3 Bundle Savings

必须基于合法单品价格。

### 26.4 Bundle Content Version

内容变化必须创建新版本。

---

## 27. Owned Item Handling

### 27.1 Possible Policies

- Disallow Purchase；
- Remove Owned Item and Reduce Price；
- Keep Full Bundle Price with Explicit Duplicate Value；
- Convert Duplicate；
- Grant Giftable Copy；
- Upgrade Credit；
- No Adjustment。

### 27.2 Preferred Principle

已拥有内容不应让玩家重复付费而没有清楚价值。

### 27.3 Dynamic Bundle Quote

根据：

- Entitlement；
- Platform；
- Region；
- Product Version；

计算。

### 27.4 Refund Impact

若已拥有内容来自退款、订阅或临时权益，需重新报价。

---

## 28. Upgrade Pricing

### 28.1 Upgrade Types

- Standard to Deluxe；
- Tier Upgrade；
- DLC Collection Upgrade；
- Subscription Upgrade；
- Edition Upgrade。

### 28.2 Upgrade Quote

考虑：

- 已拥有 Product；
- 原购买来源；
- 平台；
- 退款；
- 订阅；
- Bundle；
- Promotion；
- Region。

### 28.3 Upgrade Credit

可以按：

- 固定差价；
- 已支付金额；
- 标准差价；
- 剩余价值；
- 政策固定 Credit。

### 28.4 No Double Charge

Upgrade 后不应重复收取已拥有核心内容费用。

---

## 29. Subscription Pricing

### 29.1 Subscription Price Components

- Introductory Price；
- Standard Price；
- Renewal Price；
- Trial；
- Billing Period；
- Tax；
- Proration；
- Upgrade；
- Downgrade；
- Grace；
- Currency。

### 29.2 Display

购买前展示：

- 当前价格；
- 优惠期；
- 优惠结束后的价格；
- 下次扣款；
- 自动续费；
- 取消。

### 29.3 Price Change

提前通知：

- 新价格；
- 生效日；
- 是否需要确认；
- 如何取消；
- 对当前周期影响。

### 29.4 Annual Equivalent

可以展示月均价，但不能隐藏实际一次性扣款总额。

---

## 30. Proration

### 30.1 Use Cases

- Subscription Upgrade；
- Downgrade；
- Plan Change；
- Mid-Cycle Add-On；
- Refund；
- Bundle Upgrade。

### 30.2 Proration Inputs

- 当前周期；
- 已用时间；
- 剩余时间；
- 当前计划价格；
- 新计划价格；
- 税；
- Credit；
- 平台规则；
- 舍入。

### 30.3 Proration Quote

清楚展示：

- 立即收取；
- 立即退还或 Credit；
- 下次扣款；
- 生效时间；
- 权益变化。

### 30.4 Platform Authority

平台订阅使用平台规则时，产品内展示必须一致。

---

## 31. Introductory and Trial Pricing

### 31.1 Introductory Offer

面向首次订阅或首次购买。

### 31.2 Eligibility

定义：

- 是否曾订阅；
- 是否曾使用 Trial；
- 平台；
- 地区；
- Product Family；
- Account；
- Gift；
- Family Sharing。

### 31.3 One-Time Meaning

必须清楚是一生一次、每 Product 一次还是每平台一次。

### 31.4 End of Intro Price

提前说明标准续费价。

---

## 32. Personalized Offers

### 32.1 Acceptable Inputs

可以基于：

- 已拥有 Product；
- 明确兴趣；
- 内容进度；
- 合法参与历史；
- 区域；
- 平台；
- 购物车；
- 订阅层级；
- 已失效但可恢复的权益。

### 32.2 Prohibited Inputs

不得基于：

- 健康；
- 自伤或危机信号；
- 失败后的情绪；
- 孤独或社交排斥；
- 儿童脆弱性；
- 精确财务困境；
- 未授权敏感数据；
- 投诉或举报状态；
- Moderation 状态；
- 付费冲动预测。

### 32.3 Price Personalization vs Offer Personalization

推荐商品和优惠资格可以个性化。

对同一商品进行不可解释的个人价格差异风险极高，应默认禁止或严格受法律和伦理治理。

### 32.4 Explainability

可以说明：

- 因已拥有基础版获得升级价；
- 因首次订阅获得首期优惠；
- 因拥有部分礼包内容获得动态减价。

---

## 33. Offer Eligibility

### 33.1 Eligibility Dimensions

- Account；
- Region；
- Platform；
- Age；
- Family；
- Entitlement；
- Purchase History；
- Subscription；
- Product Ownership；
- Event Participation；
- Coupon；
- Channel；
- Time；
- Usage Limit；
- Spending Controls；
- Legal Restrictions。

### 33.2 Eligibility Result

返回：

- Eligible / Ineligible；
- Reason Code；
- Missing Requirement；
- Recoverable；
- Expiry；
- Alternative；
- Version。

### 33.3 No Hidden Eligibility

若玩家看到 Offer 却不能购买，应解释原因。

---

## 34. Offer Targeting

### 34.1 Targeting Purpose

控制：

- 谁看到；
- 在哪里看到；
- 何时看到；
- 多频繁看到。

### 34.2 Targeting Boundaries

不能将：

- 安全事件；
- 举报；
- 处罚；
- 自伤信号；
- 儿童状态；
- 失败焦虑；

用作商业触达信号。

### 34.3 Channel Targeting

- Store；
- Home；
- Product Page；
- Email；
- Push；
- In-App；
- Partner；
- Platform Store。

### 34.4 Commercial Notification

必须尊重 Notification Preferences 和 Consent。

---

## 35. Offer Frequency

### 35.1 Frequency Dimensions

- 每玩家；
- 每 Offer；
- 每 Surface；
- 每 Session；
- 每日；
- 每周；
- 全局商业预算。

### 35.2 Fatigue Controls

- Impression Cap；
- Cooldown；
- Dismiss Memory；
- Hide Offer；
- Quiet Commercial Hours；
- No Re-Prompt after Decline。

### 35.3 No Escalating Pressure

玩家忽略 Offer 后，不应自动使用更高压文案和更短倒计时。

---

## 36. Countdown

### 36.1 Valid Countdown

必须绑定真实：

- Offer End；
- Event End；
- Inventory Reservation；
- Checkout Expiry；
- Subscription Trial End。

### 36.2 Invalid Countdown

禁止：

- 每次打开重置；
- 针对单一玩家伪造；
- 无真实结束；
- 结束后立即同价重开；
- 倒计时和服务器时间不一致。

### 36.3 Time Zone

展示绝对结束时间，必要时标明时区。

### 36.4 Grace

若存在 Grace，必须说明。

---

## 37. Scarcity

### 37.1 Real Scarcity

- 实体库存；
- 限量数字许可；
- 活动名额；
- 真实创作者批次；
- 法律或许可限制。

### 37.2 Artificial Scarcity

数字商品可无限复制时，不应虚构“只剩 3 件”。

### 37.3 Low Stock Claims

必须有真实库存来源和更新。

### 37.4 Reservation

Checkout 可以短暂保留库存，但需：

- Expiry；
- 释放；
- 防 Bot；
- 不无限锁定。

---

## 38. Dynamic Pricing

### 38.1 Acceptable Uses

- 地区价格；
- 汇率；
- 税；
-平台 Price Tier；
-库存成本；
-订阅 Proration；
-已拥有内容；
-公开时间段促销。

### 38.2 High-Risk Uses

- 实时个人支付意愿；
- 设备价格；
- 重复查看涨价；
- 失败后涨价；
- 高消费用户加价；
- 危机状态价格；
- 隐藏 A/B 个体价格。

### 38.3 Governance

动态价格必须：

- 有合法基础；
- 可解释；
- 有版本；
- 有审计；
- 有公平评估；
- 不影响已创建 Snapshot。

---

## 39. Price Experiments

### 39.1 Suitable Experiments

- Store Layout；
- Price Presentation；
- Bundle Composition；
- Price Point（高风险）；
- Discount Format；
- Free Trial Length；
- Offer Frequency；
- Reference Copy。

### 39.2 High-Risk Controls

价格实验需要：

- Legal；
- Finance；
- Ethics；
- Region；
- Sample；
- Stop Conditions；
- Holdout；
- Audit；
- Long-Term Impact。

### 39.3 Stable Assignment

同一账号实验期间保持稳定。

### 39.4 No Mid-Checkout Change

实验变化不影响现有 Snapshot。

### 39.5 Price Fairness

同一地区和平台的个体价格实验应谨慎，默认优先使用公开 Cohort 或时间窗口，而非秘密个人价格。

---

## 40. Experiment Metrics

不应只看：

- Conversion；
- Revenue per User。

还应看：

- Refund；
- Complaint；
- Confusion；
- Cancel；
- Long-Term Retention；
- Trust；
- Offer Fatigue；
- Child Impact；
- Support；
- Revenue Concentration；
- Spending Health。

### 40.1 Guardrails

- Refund Rate；
- Chargeback；
- Support Contact；
- Misunderstanding；
- Subscription Cancellation；
- High-Spend Concentration；
- Dark Pattern Review；
- Accessibility Failure。

---

## 41. Price Change

### 41.1 Types

- Base Price Increase；
- Base Price Decrease；
- Regional Adjustment；
- Currency Adjustment；
- Tax Change；
- Subscription Renewal Change；
- Platform Tier Change；
- Product Repositioning。

### 41.2 Change Communication

需要根据影响：

- Store 更新；
- Subscription Notice；
- Email；
- In-App；
- Checkout；
- Support；
- Public Notice。

### 41.3 Existing Owners

价格变化不应改变已拥有 Entitlement。

### 41.4 Preorders

已确认 Preorder 的价格保护按平台和政策执行。

### 41.5 Subscription

提前通知并提供取消。

---

## 42. Price Protection

### 42.1 Checkout Protection

Snapshot 有效期内保护价格。

### 42.2 Preorder Protection

若价格下降，可按政策自动应用低价。

### 42.3 Subscription Protection

可提供：

- Legacy Price；
- Grace；
- Transition；
- Grandfathering；
- Limited Period。

### 42.4 Error Price

错误价格如何处理需基于：

- 法律；
- 平台；
- 规模；
- 玩家信赖；
- 是否完成支付；
- 是否明显错误。

---

## 43. Error Pricing

### 43.1 Error Types

- 小数点错误；
- 币种错误；
- 税错误；
- 折扣叠加错误；
- Bundle 错误；
- 已拥有内容错误；
- Platform Mapping 错误；
- Exchange Rate 错误；
- Experiment 错误。

### 43.2 Immediate Actions

- Stop Sale；
- Disable Offer；
- Freeze Checkout；
- Preserve Snapshot；
- Identify Orders；
- Legal / Finance Review；
- Player Notice；
- Refund or Honor；
- Correct；
- Root Cause。

### 43.3 Honor vs Cancel

根据：

- 法律；
- 平台政策；
- 是否已完成；
- 错误明显程度；
- 规模；
- 玩家影响；

决定。

### 43.4 No Silent Charge Correction

不能在购买后未经同意补扣差价。

---

## 44. Offer Retirement

### 44.1 Stop Showing

停止曝光。

### 44.2 Stop Quote

不再生成新 Quote。

### 44.3 Stop Checkout

不再创建 Snapshot。

### 44.4 Honor Existing Snapshot

是否继续履行未过期 Snapshot 需明确。

### 44.5 Archive

保留：

- Definition；
- Version；
- Price；
- Orders；
- Audit；
- Analytics。

---

## 45. Cross-Platform Pricing

### 45.1 Platform Differences

可能来自：

- Fee；
- Tax；
- Price Tier；
- Regional Policy；
- Store Promotion；
- Refund；
- Currency；
- Bundle；
- Subscription。

### 45.2 Communication

不必保证所有平台同价，但应避免误导。

### 45.3 Cross-Platform Purchase

玩家在平台 A 购买后，在平台 B 使用时：

- Entitlement；
- Paid Currency；
- Subscription；
- Refund；
- Upgrade；

规则需清楚。

### 45.4 Platform-Funded Promotion

应明确 Promotion 来源，不影响 Product Base Price 审计。

---

## 46. Creator Marketplace Pricing

### 46.1 Creator Price

创作者可在允许范围内定价。

### 46.2 Platform Constraints

- Minimum；
- Maximum；
- Tier；
- Tax；
- Region；
- Currency；
- Refund；
- Revenue Share；
- Content Rating。

### 46.3 Price Changes

已购买权益不受后续价格影响。

### 46.4 Abuse

防止：

- 洗钱；
- 自买；
- Price Manipulation；
- Scam；
- IP 侵权；
- 儿童不当购买。

---

## 47. Free Offers

### 47.1 Zero Price

免费领取仍需要：

- Product；
- Entitlement；
- Eligibility；
- Limit；
- Region；
- Expiry；
- Audit。

### 47.2 No Fake Free

不能：

- 隐藏自动续费；
- 强制支付信息但不说明；
- 要求广告或数据但称无条件免费；
- 先免费后无提示收费。

### 47.3 Free Trial

与零价永久 Product 区分。

---

## 48. Cart

### 48.1 Cart Purpose

允许多个商品一起结账。

### 48.2 Cart State

- Items；
- Quantity；
- Quote；
- Discount；
- Coupon；
- Tax；
- Total；
- Expiry；
- Owned Items；
- Eligibility；
- Version。

### 48.3 Repricing

Cart 打开期间 Price Book 变化时：

- 标记；
- 重新报价；
- 不静默改变；
- 已创建 Snapshot 不变。

### 48.4 Cart Invariants

1. Cart 不等于 Order。
2. Cart Price 不是永久保证，除非生成 Snapshot。
3. 过期商品必须移除或重新确认。
4. Coupon 重新验证。
5. Owned Item 变化后重新报价。

---

## 49. Storefront Presentation

### 49.1 Required Information

- Product；
- Price；
- Currency；
- Discount；
- Reference Price；
- Savings；
- Duration；
- Ownership；
- Offer End；
- Owned State；
- Subscription；
- Auto-Renew；
- Tax Mode；
- Region / Platform Restriction。

### 49.2 Sorting and Ranking

Store 排序不能秘密基于：

- 玩家脆弱状态；
- 安全 Case；
- 儿童冲动；
- 失败焦虑。

### 49.3 Sponsored Placement

明确标记。

### 49.4 No False Popularity

“热卖”“最受欢迎”需有真实依据或明确是推荐。

---

## 50. Comparison

对于：

- Subscription Tier；
- Edition；
- Bundle；
- DLC Collection；

提供对比表。

### 50.1 Comparison Rules

- 同一维度；
- 不隐藏缺失项；
- 已拥有内容标记；
- 价格和期限清楚；
- 推荐标签有依据；
- 不将最高价默认标为“最佳”。

---

## 51. Savings Display

### 51.1 Absolute Savings

节省具体金额。

### 51.2 Percentage Savings

节省百分比。

### 51.3 Unit Price

每单位价格。

### 51.4 Annual Equivalent

月均或年均。

### 51.5 Risks

单位价和月均价不能隐藏：

- 实际总额；
- 付款周期；
- 最低购买数量；
- 自动续费。

---

## 52. Price Rounding

### 52.1 Rounding Layers

- Base；
- Discount；
- Tax；
- Currency Conversion；
- Total；
- Refund；
- Proration。

### 52.2 Consistency

Store、Checkout、Receipt 和 Refund 使用相同规则。

### 52.3 Penny Difference

若出现小额差异，必须有明确规则。

### 52.4 Display vs Ledger

显示精度和财务精度可以不同，但总额必须一致。

---

## 53. Offer Stacking

### 53.1 Possible Sources

- Storewide；
- Product；
- Bundle；
- Coupon；
- Loyalty；
- Platform；
- Subscription；
- Creator；
- Tax Credit。

### 53.2 Stacking Matrix

每类 Offer 明确：

- Can Stack；
- Cannot Stack；
- Priority；
- Maximum；
- Exclusive Group。

### 53.3 Best Price

可以自动应用最佳合法组合，但应展示组成。

### 53.4 No Surprise Removal

若某 Coupon 会取消更好的折扣，需提示。

---

## 54. Best Price Guarantee

### 54.1 Purpose

自动选择最低合法价格。

### 54.2 Constraints

基于：

- 当前平台；
- 当前地区；
- 当前时间；
- 当前资格；
- 当前 Product；
- 当前 Offer。

### 54.3 Not Cross-Platform Arbitrage

不承诺不同平台全球最低价。

### 54.4 Audit

记录选择了哪些 Rule。

---

## 55. Refund and Price Interaction

### 55.1 Discounted Purchase

退款以实际支付金额为基础。

### 55.2 Bundle

部分退款需重新计算：

- 保留商品价格；
- Bundle 资格；
- 已拥有内容；
- Coupon；
- Tax。

### 55.3 Coupon

退款后 Coupon 是否恢复需明确。

### 55.4 Subscription

Proration 和 Refund 使用一致规则。

### 55.5 Price Drop after Purchase

是否提供 Price Adjustment 需按政策定义。

---

## 56. Gift Pricing

### 56.1 Recipient Context

Gift Price 通常基于购买者地区和平台。

### 56.2 Recipient Eligibility

若接收者已拥有或不可使用：

- 阻止；
- 替代；
- 退款；
- Credit；
- 允许拒绝。

### 56.3 Cross-Region Gift

需要处理：

- Price Difference；
- Tax；
- Region Restriction；
- Fraud；
- Currency；
- Entitlement。

---

## 57. Ads and Sponsored Offers

### 57.1 Labeling

广告和赞助 Offer 明确标记。

### 57.2 Pricing Claims

广告中的价格必须与目标 Checkout 一致或明确条件。

### 57.3 Affiliate / Partner Offer

说明：

- 合作方；
- 跳转；
- 数据共享；
- 价格；
- 退款主体；
- 支持主体。

### 57.4 No Safety Impersonation

不得伪装为安全或系统通知。

---

## 58. Commercial Notifications

### 58.1 Allowed Messages

- 真实优惠开始；
- 真实优惠结束；
- 订阅价格变化；
- Coupon 到期；
- 已关注商品降价；
- 购物车价格变化；
- Gift 状态。

### 58.2 Consent

遵守 Notification Preferences 和 Marketing Consent。

### 58.3 Frequency

受全局商业频率预算约束。

### 58.4 No False Urgency

消息内容与真实 Offer 时间一致。

---

## 59. Child and Family Pricing

### 59.1 Restrictions

- 随机购买隐藏或限制；
- 订阅需家长；
- Gift 限制；
- Price Complexity 降低；
- 总价优先；
- Paid Currency 换算清楚；
- 广告个性化关闭。

### 59.2 Parent View

显示：

- 商品；
- 总价；
- 期限；
- 自动续费；
- 随机性；
- 已有内容；
- 支出历史。

### 59.3 No Child-Targeted Pressure

不使用：

- 倒计时压力；
- 角色责备；
- 同伴比较；
- “只有你没有”；
- 复杂币种；
- 隐藏真实钱。

---

## 60. Accessibility

### 60.1 Visual

- Price、Discount、Tax 和 Total 有文本；
- 不只靠红色和划线；
- 大字体不截断；
- 货币符号和金额顺序本地化；
- 倒计时有绝对时间替代。

### 60.2 Screen Reader

应读取：

- Product；
- Current Price；
- Original Price；
- Discount；
- Total；
- Tax；
- Duration；
- Subscription；
- Owned Items；
- Expiry；
- Primary Action。

### 60.3 Cognitive

- 简单语言；
- 总价优先；
- 折扣组成可展开；
- 不堆叠太多 Offer；
- 订阅续费和试用结束清楚；
- Bundle 内容可逐项查看。

### 60.4 Input

- 不要求精确拖拽；
- Coupon 可输入和粘贴；
- Offer 可关闭；
- Checkout 可返回；
- 价格详情可通过键盘、手柄和触摸访问。

### 60.5 Timing

- 不使用需要几秒内购买的常规优惠；
- 倒计时有真实宽限；
- 价格变化需要重新确认。

---

## 61. Privacy

### 61.1 Pricing Data

包括：

- Offer Exposure；
- Eligibility；
- Quote；
- Coupon；
- Cart；
- Purchase；
- Region；
- Platform；
- Experiment；
- Price History。

### 61.2 Purpose Limitation

不得把安全、健康、危机或举报数据用于定价。

### 61.3 Personalization Consent

若使用个性化推荐，需要符合隐私和同意要求。

### 61.4 Data Minimization

不需要保存完整浏览历史来实现每种优惠。

### 61.5 Retention

Quote、Snapshot、Order 和 Audit 使用不同保留期。

---

## 62. Security

### 62.1 Threats

- Client Price Tampering；
- Coupon Enumeration；
- Coupon Replay；
- Region Spoofing；
- SKU Mapping Attack；
- Bundle Manipulation；
- Tax Tampering；
- Price Snapshot Forgery；
- Experiment Override；
- Checkout Replay；
- Inventory Abuse；
- Creator Price Fraud；
- Admin Abuse。

### 62.2 Controls

- Server Price；
- Signed Quote；
- Signed Snapshot；
- Idempotency；
- Coupon Hash；
- Rate Limit；
- Region Verification；
- RBAC；
- Approval；
- Audit；
- Last Known Good；
- Anomaly Detection。

### 62.3 Client Trust

客户端不能决定：

- Price；
- Discount；
- Tax；
- Eligibility；
- Coupon Validity；
- Savings；
- Offer End；
- Scarcity；
- Proration。

---

## 63. Finance and Audit

### 63.1 Audit Record

保留：

- Product；
- SKU；
- Price Version；
- Offer Version；
- Quote；
- Snapshot；
- Tax；
- Discount；
- Coupon；
- Experiment；
- Order；
- Refund；
- Correction；
- Approval。

### 63.2 Reconciliation

Finance 可以对账：

- Displayed Price；
- Charged Price；
- Provider Amount；
- Tax；
- Revenue；
- Refund；
- Discount Funding。

### 63.3 Approval

高风险价格发布需要多方审批。

### 63.4 Separation of Duties

创建价格、审批价格和发布价格不应由同一人无限制完成。

---

## 64. Support

### 64.1 Support View

可查看：

- 当时 Offer；
- Price Quote；
- Snapshot；
- Tax；
- Coupon；
- Eligibility；
- Owned Items；
- Experiment；
- Checkout Expiry；
- Order；
- Refund。

### 64.2 Redaction

不显示完整支付凭据。

### 64.3 Support Actions

可以：

- 解释价格；
- 重新生成 Quote；
- 恢复 Coupon；
- 提交 Price Error；
- 触发 Refund Review；
- 纠正 Eligibility；
- 转 Finance。

不应直接手工改财务价格。

---

## 65. Pricing Incident Management

### 65.1 Incident Types

- Wrong Price；
- Wrong Currency；
- Wrong Tax；
- False Discount；
- False Countdown；
- Bundle Error；
- Coupon Leak；
- Region Mapping Error；
- Subscription Price Error；
- Price Experiment Leak；
- Owned Item Mispricing；
- Snapshot Mismatch；
- Creator Price Abuse。

### 65.2 Severity

#### SEV-1

大规模错误收费、法律违规、儿童风险、虚假折扣或严重价格歧视。

#### SEV-2

高影响但可快速停止。

#### SEV-3

局部 Product 或地区。

#### SEV-4

低影响展示错误。

### 65.3 Actions

- Stop Offer；
- Stop Checkout；
- Rollback Price Book；
- Freeze Coupon；
- Preserve Snapshots；
- Identify Orders；
- Honor / Refund；
- Notify；
- Support Script；
- Legal Review；
- Root Cause；
- Audit。

---

## 66. Failure and Recovery

| Failure | Cause | Player Impact | Recovery | Data Guarantee |
|---|---|---|---|---|
| No Valid Price | 配置缺失 | 无法购买 | 停售、恢复 LKG | 不猜测价格 |
| Quote Generation Failed | 服务错误 | 商品页无价格 | 重试、缓存 LKG | 不进入 Checkout |
| Snapshot Missing | Checkout 异常 | 无法确认价格 | 重新报价 | 不扣款 |
| Price Mismatch | Store / Checkout 不一致 | 信任受损 | 以有效 Snapshot 为准或重新确认 | 两端版本保留 |
| False Discount | Reference Price 错误 | 误导 | 停止 Offer、纠正、退款审查 | Audit 保留 |
| Tax Error | 地区或规则错误 | 错误总价 | Stop Sale、Recalculate、Refund | Tax Version 保留 |
| Coupon Double Use | 并发 | 超额优惠 | 幂等 Redeem、对账 | Usage History |
| Bundle Owned-State Error | Entitlement 延迟 | 重复付费 | Requote、Refund、Credit | Ownership Snapshot |
| Proration Error | 周期或舍入错误 | 错误收费 | Recalculate、Refund | 原 Quote 保留 |
| Experiment Leak | Assignment 错误 | 价格不一致 | Kill Switch、稳定分组 | Experiment Version |
| Countdown Reset | 客户端逻辑 | 虚假紧迫 | 服务端时间、停用 | Offer End 保留 |
| Region Spoof | 风险或误配置 | 错误价 | 验证、重新报价 | 不处罚无证据玩家 |

---

## 67. Edge Cases

### Price

- Price Book 生效瞬间 Checkout；
- 币种切换；
- 税率变化；
- 平台 Store 缓存；
- 汇率波动；
- 负折扣；
- 零价；
- 极小金额；
- 高额商品。

### Offer

- Offer 开始前缓存；
- Offer 结束但 Snapshot 未过期；
- Event 延期；
- Countdown 跨时区；
- Sold Out 后退款恢复库存；
- Coupon 与 Bundle 叠加；
- 多 Offer 同时适用。

### Ownership

- Entitlement 刚退款；
- Subscription 临时拥有；
- Family Sharing；
- Platform Exclusive；
- Gift Pending；
- DLC 部分拥有；
- Bundle Product 退役。

### Subscription

- Trial 转正；
- Price Change；
- Upgrade Mid-Cycle；
- Downgrade；
- Grace；
- 多平台重复订阅；
- 年付转月付；
- 税区变化。

### Experiments

- 同一账号多设备；
- Assignment 变化；
- Cohort 迁移；
- Price Book 回滚；
- Experiment End；
- Holdout；
- 购物车跨实验版本。

---

## 68. Cross-System Dependencies

| System | Dependency Type | Direction | Data or Event | Failure Impact |
|---|---|---|---|---|
| Monetization System | Critical | 双向 | Quote / Snapshot / Order | 错误收费 |
| Entitlement and Ownership | Critical | Entitlement → Pricing | Owned State / Eligibility | 重复购买 |
| Resources and Economy | Hard | Economy → Pricing | Currency / Exchange | 显示错误 |
| Reward System | Soft / Hard | Reward → Pricing | Bundle Value / Pass | 价值误导 |
| Content and Unlocks | Hard | Content → Pricing | Product Availability | 销售不可用内容 |
| Content Lifecycle | Hard | 双向 | Stop Sale / Retirement | 已购风险 |
| Settings and Preferences | Hard / Soft | Settings → Offers | Consent / Hide / Family | 触达错误 |
| Notification and Reminders | Soft / Hard | Offers → Notification | Price Change / Expiry | 误导消息 |
| Live Operations | Hard | Live → Offers | Schedule / Event | 错误时间 |
| Experiment Management | Critical | 双向 | Assignment / Price Test | 不公平或错价 |
| Save and Persistence | Hard | 双向 | Coupon / Frequency / History | 状态丢失 |
| Moderation and Safety | Hard Boundary | Safety → Offers | No Sensitive Targeting | 伦理风险 |
| Analytics and Telemetry | Soft | Offers → Analytics | Exposure / Quote | 不阻断 |
| Finance / Tax | Critical | 双向 | Tax / Audit / Approval | 财务风险 |

---

## 69. Data and Persistence

| State | Persistent | Authority | Save Trigger | Retention | Recovery |
|---|---|---|---|---|---|
| Price Definition | 是 | Pricing | 发布变化 | 财务长期 | Version History |
| Price Book | 是 | Pricing | 发布变化 | 长期 | LKG |
| Offer Definition | 是 | Offers | 发布变化 | 长期版本 | Archive |
| Offer Instance | 是或短期 | Offers | 创建和状态变化 | Offer 及审计期 | 重新评估 |
| Price Quote | 是或短期 | Pricing | 生成 | Quote 期 | 重新生成 |
| Price Snapshot | 是 | Pricing / Monetization | Checkout | 财务长期 | Order 对账 |
| Coupon Definition | 是 | Pricing | 发布 | 长期 | 版本恢复 |
| Coupon Usage | 是 | Pricing | 兑换 | 审计期 | Usage History |
| Bundle Rule | 是 | Pricing | 发布 | 版本期 | LKG |
| Proration Rule | 是 | Pricing | 发布 | 版本期 | 版本恢复 |
| Frequency State | 是 | Offers | 曝光和关闭 | 周期级 | 重算 |
| Experiment Reference | 是 | Experiment | Assignment | 实验期 | Stable Assignment |
| Pricing Audit | 是 | Pricing / Finance | 高风险变化 | 财务长期 | Audit Store |

---

## 70. Analytics and Validation

### 70.1 Key Assumptions

1. 玩家能理解 Base Price、Discount、Tax 和 Total。
2. Store、Cart、Checkout 和 Receipt 使用一致价格。
3. Reference Price 和 Savings 真实可验证。
4. 已拥有内容正确影响 Bundle 和 Upgrade。
5. Subscription、Trial、Renewal 和 Proration 可理解。
6. Countdown、Scarcity 和 Offer End 真实。
7. 个性化和实验不使用脆弱状态或秘密个人涨价。
8. Coupon、Stacking 和 Best Price 规则正确。
9. Price Change、Error Pricing 和 Rollback 可执行。
10. 多平台、多地区、税和币种差异可解释。

### 70.2 Validation Plan

| Hypothesis | Evidence | Success | Failure | Method |
|---|---|---|---|---|
| 价格可理解 | 用户复述 | 能说明总价和折扣 | 误解税或期限 | Usability Test |
| 全链路一致 | Quote / Snapshot / Receipt | 金额一致 | 前后价差 | Integration Test |
| 折扣真实 | Audit | Reference 合法 | 虚假折扣 | Finance / Legal Review |
| Owned Item 正确 | Bundle 场景 | 不重复无价值付费 | Bundle 错价 | QA |
| Subscription 清楚 | Upgrade 任务 | 能说明立即和下次收费 | Proration 误解 | Research |
| Countdown 真实 | 时间测试 | 结束后不重置 | 假紧迫 | QA |
| 个性化受控 | Data Audit | 不使用敏感或脆弱信号 | 隐藏差价 | Ethics / Privacy Review |
| Coupon 正确 | 并发测试 | 不重复、不越权 | 超额优惠 | Security Test |
| Error Recovery 有效 | 故障演练 | 快速停售和修正 | 持续错误收费 | Incident Drill |
| 地区和税正确 | Region Matrix | 合规且一致 | 税费错误 | Tax QA |

### 70.3 Behavioral Metrics

- Offer Viewed；
- Offer Dismissed；
- Offer Hidden；
- Quote Generated；
- Coupon Applied；
- Coupon Rejected；
- Bundle Viewed；
- Upgrade Quote Viewed；
- Cart Updated；
- Checkout Snapshot Created；
- Price Changed；
- Offer Expired；
- Subscription Price Viewed；
- Comparison Opened；
- Price Issue Reported。

### 70.4 Outcome Metrics

- Quote Success；
- Store / Checkout Match；
- Coupon Success；
- Bundle Conversion；
- Owned Item Adjustment Accuracy；
- Checkout Abandonment；
- Price Confusion；
- Refund due to Pricing；
- Tax Error；
- Countdown Complaint；
- Offer Fatigue；
- Subscription Cancellation after Price Change；
- Price Experiment Long-Term Value；
- Support Contacts；
- Trust Score。

### 70.5 Negative Metrics

- Store / Checkout 价差；
- 虚假原价；
- 虚假倒计时；
- Bundle 节省错误；
- Coupon 重复；
- Tax 错误；
- Currency 错误；
- 已拥有内容重复付费；
- Subscription Proration 错误；
- 个性化价格歧视；
- 儿童高压 Offer；
- Checkout 中途涨价；
- Experiment Assignment 漂移；
- Price Book 无法回滚；
- 错误价格继续售卖。

### 70.6 Event Intents

| Event Intent | Trigger | Key Properties | Privacy Notes |
|---|---|---|---|
| Price Quote Generated | 报价完成 | Product Type, Region, Result | 不记录完整个人画像 |
| Offer Eligibility Evaluated | 资格判断 | Result, Reason | 不记录敏感信号 |
| Checkout Snapshot Created | 进入结账 | Price Version, Offer Version | 财务审计 |
| Coupon Resolved | Coupon 使用 | Result, Reason | 不记录明文 Code |
| Bundle Repriced | Ownership 变化 | Item Count, Adjustment | 不记录私人备注 |
| Price Changed | Price Book 变化 | From, To, Reason | 高权限 |
| Offer Suppressed | 频控或偏好 | Reason, Surface | 隐私用途 |
| Pricing Incident Opened | 事故 | Severity, Scope | 财务和法律权限 |

---

## 71. Test Strategy

### 71.1 Price Tests

- Base；
- Region；
- Platform；
- Currency；
- Tax；
- Exchange Rate；
- Rounding；
- Zero；
- High Value；
- Price Book Switch。

### 71.2 Offer Tests

- Start；
- End；
- Pause；
- Cancel；
- Sold Out；
- Eligibility；
- Frequency；
- Dismiss；
- Countdown；
- Scarcity。

### 71.3 Discount Tests

- Percentage；
- Fixed；
- Bundle；
- Coupon；
- Stack；
- Floor；
- Best Price；
- Reference Price；
- Savings。

### 71.4 Bundle and Upgrade Tests

- None Owned；
- Partially Owned；
- Fully Owned；
- Refund；
- Subscription Ownership；
- Platform Ownership；
- Upgrade；
- Dynamic Bundle；
- Product Retirement。

### 71.5 Subscription Tests

- Trial；
- Intro；
- Renewal；
- Price Change；
- Upgrade；
- Downgrade；
- Proration；
- Grace；
- Tax Change；
- Platform Rule。

### 71.6 Experiment Tests

- Stable Assignment；
- Multi-Device；
- Mid-Checkout；
- Holdout；
- Rollback；
- Price Difference；
- Guardrails；
- Child Exclusion。

### 71.7 Security Tests

- Price Tamper；
- Coupon Enumeration；
- Coupon Replay；
- Region Spoof；
- Snapshot Forgery；
- Bundle Manipulation；
- Tax Tamper；
- Admin Abuse。

### 71.8 Accessibility and Ethics Tests

- 读屏；
- 大字体；
- 总价；
- Countdown；
- Subscription；
- Bundle；
- Child；
- Personalization；
- Dark Pattern Review。

---

## 72. Pricing Contract Template

```markdown
# Pricing Contract

## Product

- Product ID:
- SKU:
- Region:
- Platform:
- Currency:

## Base Price

- Price ID:
- Amount:
- Tax Mode:
- Effective Window:
- Reference Price:
- Rounding:

## Discount

- Rule:
- Value:
- Stack:
- Floor:
- Exclusions:

## Quote

- Eligibility:
- Owned Items:
- Base:
- Discount:
- Tax:
- Total:
- Expiry:

## Snapshot

- Snapshot ID:
- Signature:
- Checkout Expiry:
- Order Reference:

## Safety

- Child:
- Region:
- No Sensitive Targeting:
- Accessibility:

## Validation

- Success:
- Failure:
- Audit:
```

---

## 73. Offer Contract Template

```markdown
# Offer Contract

## Definition

- Offer ID:
- Type:
- Products:
- Price Rule:
- Start:
- End:
- Time Zone:

## Eligibility

- Account:
- Region:
- Platform:
- Ownership:
- Subscription:
- Age:
- Usage Limit:

## Presentation

- Surface:
- Countdown:
- Reference Price:
- Savings:
- Scarcity:
- Sponsored Label:

## Frequency

- Impression Cap:
- Cooldown:
- Dismiss:
- Hide:
- Notification:

## Checkout

- Quote:
- Snapshot:
- Expiry:
- Owned Item Handling:

## Governance

- Legal:
- Finance:
- Ethics:
- Experiment:
- Rollback:
```

---

## 74. Bundle Pricing Contract Template

```markdown
# Bundle Pricing Contract

## Bundle

- Bundle ID:
- Version:
- Products:
- Quantities:
- Required Items:

## Pricing

- Sum of Items:
- Bundle Price:
- Discount:
- Reference Price:
- Savings:

## Ownership

| Product | Owned Handling | Price Adjustment | Duplicate Handling |
|---|---|---:|---|

## Eligibility

- Region:
- Platform:
- Subscription:
- Product Requirements:

## Refund

- Full:
- Partial:
- Owned Item Change:
- Coupon Restore:

## Validation

- Quote:
- Snapshot:
- Order:
- Audit:
```

---

## 75. Pricing and Offer Debt

包括：

- 多套 Price Book；
- Store 与 Checkout 分别算价；
- 无 Price Version；
- 无 Snapshot；
- 虚假 Reference Price；
- Bundle 节省写死；
- Owned Item 不参与报价；
- Coupon 无统一使用记录；
- 订阅 Proration 分散；
- 个性化 Targeting 无边界；
- Countdown 在客户端；
- Offer Frequency 各自实现；
- Tax 逻辑分裂；
- 多平台 SKU 映射失控；
- 实验影响已确认价格；
- Support 看不到历史 Quote。

### 75.1 Signals

- 玩家截图价格和收据不一致；
- Bundle 退款大量人工处理；
- Coupon 被重复利用；
- 订阅升级投诉高；
- 同一商品不同页面价格不同；
- Offer 结束后仍购买；
- 倒计时反复重置；
- 价格实验引发公平争议；
- 价格事故无法定位版本。

### 75.2 Reduction

- 统一 Product / SKU；
- Price Book；
- Price Version；
- Quote Service；
- Snapshot；
- Discount Engine；
- Coupon Ledger；
- Bundle Pricing；
- Proration Service；
- Offer Registry；
- Frequency Budget；
- Server Countdown；
- Pricing Audit；
- 定期 Pricing Health Review。

---

## 76. Rollout and Migration

### 76.1 Rollout

价格和 Offer 变更应按：

```text
Draft
→ Finance Review
→ Legal / Tax Review
→ Ethics Review
→ Sandbox
→ Internal
→ Small Region / Platform
→ Limited Product
→ Broad Release
→ Full Release
```

### 76.2 Shadow Pricing

新规则并行生成 Quote，但不展示和不用于 Checkout。

### 76.3 High-Risk Changes

包括：

- Base Price；
- Tax；
- Currency；
- Exchange Rate；
- Subscription；
- Proration；
- Reference Price；
- Coupon；
- Bundle；
- Dynamic Pricing；
- Personalized Offer；
- Price Experiment；
- Child Pricing；
- Creator Pricing。

### 76.4 Migration

必须定义：

- Product；
- SKU；
- Price Book；
- Price Version；
- Offer；
- Coupon；
- Bundle；
- Proration；
- Reference Price；
- Experiment；
- Frequency；
- Snapshot；
- Audit。

### 76.5 Rollback

回滚时：

- 保留已创建 Snapshot；
- 恢复 LKG Price Book；
- 停止错误 Offer；
- 不重复 Coupon；
- 不重置 Frequency；
- 不改变已确认 Subscription 周期；
- 保留 Orders；
- 修正 Store Cache；
- 记录 Correction。

### 76.6 Stop Conditions

出现以下情况应停止发布：

- Store / Checkout 价格不一致；
- 错误税费；
- 虚假折扣；
- Bundle 错价；
- Subscription Proration 错误；
- Countdown 重置；
- 已拥有内容重复收费；
- Coupon 大规模滥用；
- 个性化价格投诉；
- 儿童 Offer 越界；
- Price Book 无法回滚；
- 错误价格继续生成 Snapshot。

---

## 77. Risks and Open Questions

| Item | Type | Impact | Probability | Mitigation | Owner |
|---|---|---:|---:|---|---|
| Store 与 Checkout 价格分叉 | Transaction Risk | 严重 | 中 | Quote + Snapshot | Engineering |
| Reference Price 不合法 | Legal Risk | 严重 | 中 | Finance / Legal Review | Legal |
| 地区价格被滥用 | Fraud Risk | 高 | 中 | Region Verification | Security |
| Subscription Proration 错误 | Financial Risk | 高 | 中 | Unified Proration | Finance |
| Owned Item Bundle 错价 | Trust Risk | 高 | 高 | Entitlement Snapshot | Product |
| 个性化价格歧视 | Ethical Risk | 严重 | 低 | Default Prohibition | Leadership |
| Countdown 失真 | Dark Pattern Risk | 高 | 中 | Server Authority | Product |
| Coupon 泄露和重放 | Security Risk | 高 | 中 | Hash + Usage Ledger | Engineering |
| Tax / Currency 配置错误 | Compliance Risk | 严重 | 中 | Region Matrix | Finance |
| Pricing Debt 持续增长 | Architecture Risk | 高 | 高 | Contract Governance | Architecture |

---

## 78. Review Checklist

### Product, SKU and Price Book

- [ ] Product、SKU、Price、Offer 和 Entitlement 区分；
- [ ] Price Book 有版本和 LKG；
- [ ] 同一范围只有一个有效 Base Price；
- [ ] Region、Platform、Currency 和 Tax 明确；
- [ ] Non-Goals 已定义。

### Quote and Snapshot

- [ ] Quote 字段完整；
- [ ] Quote 有 Expiry；
- [ ] Snapshot 固定且签名；
- [ ] Order 引用 Snapshot；
- [ ] 实验和配置不改变已确认 Snapshot。

### Discount and Reference Price

- [ ] Discount Rule 有范围、顺序和上限；
- [ ] Stacking Matrix 明确；
- [ ] Reference Price 合法；
- [ ] Savings 可验证；
- [ ] 不存在虚假原价。

### Coupon and Bundle

- [ ] Coupon 防枚举和重放；
- [ ] Usage Limit 幂等；
- [ ] Bundle Content 版本化；
- [ ] Owned Item Handling 明确；
- [ ] Bundle Savings 基于真实单品价格。

### Subscription and Proration

- [ ] Intro、Trial、Renewal 和 Standard Price 区分；
- [ ] Auto-Renew 和下次扣款清楚；
- [ ] Upgrade / Downgrade Quote 完整；
- [ ] Proration 与平台一致；
- [ ] Price Change 有通知和取消。

### Offer and Targeting

- [ ] Offer 生命周期完整；
- [ ] Eligibility 可解释；
- [ ] Countdown 和 Scarcity 真实；
- [ ] Frequency 和 Dismiss 记忆完整；
- [ ] 不使用安全、健康、危机或儿童脆弱信号。

### Experiments

- [ ] Assignment 稳定；
- [ ] Mid-Checkout 不变价；
- [ ] 价格实验有法律、财务和伦理审查；
- [ ] Guardrails 不只看转化；
- [ ] 默认避免秘密个人价格差异。

### Cross-Platform and Region

- [ ] 平台差异可解释；
- [ ] Region Determination 明确；
- [ ] 汇率和舍入统一；
- [ ] 税费展示符合要求；
- [ ] Gift 和跨平台 Entitlement 规则清楚。

### Accessibility and Ethics

- [ ] 总价优先；
- [ ] Price、Discount、Tax、Total 可读；
- [ ] 不只靠颜色；
- [ ] 不使用虚假倒计时和虚假稀缺；
- [ ] 儿童价格界面和家长控制完整。

### Validation and Operations

- [ ] Quote、Snapshot、Coupon、Bundle、Proration、Tax、Experiment 和 Incident 指标完整；
- [ ] Shadow Pricing 可用；
- [ ] Support 能查看历史 Quote；
- [ ] Pricing Debt 可监控；
- [ ] Rollback 和 Stop Conditions 明确。

---

## 79. V1 Completion Criteria

Offers and Pricing 可以被视为 V1，当：

- Base、Regional、Platform、Tax-Inclusive、Tax-Exclusive、Introductory、Promotional、Subscription、Renewal、Upgrade 和 Dynamic Bundle Pricing 类型完整；
- Product、SKU、Price Definition、Price Book、Price Version、Quote、Snapshot、Offer、Discount、Coupon、Bundle、Proration 和 Reference Price 实体明确；
- Price Book Lifecycle、Price Invariants 和 Last Known Good 完整；
- Quote 与 Snapshot 的职责、时效、签名和 Order 引用明确；
- Region、Platform、Currency、Exchange Rate、Tax 和 Rounding 规则完整；
- Percentage、Fixed、Bundle、Loyalty、Upgrade、Introductory、Subscription 和 Coupon Discount 可统一计算；
- Reference Price、Strikethrough Price、Savings 和 Unit Price 的合法展示规则明确；
- Coupon Definition、Security、Usage、Stacking 和 Invalid Feedback 完整；
- Static、Dynamic、Build-Your-Own、Upgrade、Starter 和 Event Bundle 有专项定价；
- Owned Item Handling、Dynamic Repricing、Upgrade Credit 和 Refund Interaction 明确；
- Subscription Intro、Trial、Renewal、Price Change、Upgrade、Downgrade 和 Proration 可执行；
- Personalized Offer 允许和禁止的信号边界明确；
- Offer Eligibility、Targeting、Frequency、Dismiss、Countdown、Scarcity 和 Expiry 有统一治理；
- Dynamic Pricing 和 Price Experiment 有法律、伦理、稳定 Assignment 和 Guardrails；
- Price Change、Price Protection、Error Pricing、Offer Retirement 和 Incident Recovery 可执行；
- Cross-Platform、Creator Marketplace、Free Offer、Cart、Comparison 和 Best Price 有专项规则；
- 商业通知遵守 Consent 和全局 Frequency；
- Child、Family、Accessibility、Privacy 和 Security 通过专项评审；
- Store、Cart、Checkout、Receipt 和 Refund 使用一致价格逻辑；
- Quote、Snapshot、Coupon、Bundle、Proration、Tax、Experiment 和 Incident 有验证计划；
- Pricing and Offer Debt 有识别和治理方式；
- 高风险价格变更支持 Shadow、灰度、迁移、回滚和停止条件；
- 下游 Monetization、Entitlement、Live Operations、Finance、Support 和 Analytics 可以直接引用本文件。

---

## 80. Related Documents

### Philosophy

- [Player First Design](../../philosophy/foundation/player-first-design.md)
- [Clarity and Feedback](../../philosophy/experience/clarity-and-feedback.md)
- [Consistency and Coherence](../../philosophy/long-term/consistency-and-coherence.md)
- [Accessibility and Inclusivity](../../philosophy/responsibility/accessibility-and-inclusivity.md)
- [Ethical Design](../../philosophy/responsibility/ethical-design.md)

### Systems

- [Systems README](../README.md)
- [System Design Framework](../system-design-framework.md)
- [System Map](../system-map.md)
- [Integration Rules](../integration-rules.md)
- [Resources and Economy](../progression/resources-and-economy.md)
- [Reward System](../progression/reward-system.md)
- [Content and Unlocks](../content/content-and-unlocks.md)
- [Content Lifecycle](../content/content-lifecycle.md)
- [Save and Persistence](../player/save-and-persistence.md)
- [Settings and Preferences](../player/settings-and-preferences.md)
- [Notification and Reminders](../player/notification-and-reminders.md)
- [Moderation and Safety](../social/moderation-and-safety.md)
- [Monetization System](./monetization-system.md)
- `entitlement-and-ownership.md`
- `../operations/live-operations.md`
- `../operations/experiment-management.md`
