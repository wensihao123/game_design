# Settings and Preferences（设置与偏好系统）

> Status: V1  
> Category: Player  
> Path: `design/systems/player/settings-and-preferences.md`  
> Owner: TBD  
> Reviewers: Product / UX / Design / Engineering / Accessibility / Privacy / Security / QA / Support / Data  
> Last Updated: 2026-07-11  
> Version: 1.0  
> Risk Level: High  
> Dependencies: Input and Interaction, Game State and Flow, Save and Persistence, Tutorial and Onboarding, Difficulty and Challenge, Notification and Reminders, Social and Multiplayer  
> Affected Systems: Characters and Loadouts, Audio, Rendering, UI, Matchmaking and Competition, Analytics and Telemetry, Live Operations, Experiment Management

---

## 1. System Summary

Settings and Preferences 系统负责定义：

```text
玩家可以调整什么；
哪些设置属于设备；
哪些属于账户；
哪些属于存档；
哪些设置立即生效；
哪些需要重启、重载或重新进入内容；
设置如何被验证、保存、同步、迁移和恢复；
设置如何支持不同设备、能力、环境和个人偏好；
高风险权限、隐私、通知和社交设置如何获得明确同意。
```

该系统通常包含：

- 显示；
- 图形；
- 音频；
- 语言；
- 输入；
- 控制器；
- 触摸；
- 可访问性；
- 难度；
- 游戏体验；
- 社交；
- 隐私；
- 通知；
- 网络；
- 下载；
- 数据；
- 账户；
- 设备；
- 内容过滤；
- 家长控制；
- 开发和调试设置。

健康的设置系统应让玩家感受到：

```text
我可以让体验适合自己；
改变设置的结果是可预测的；
错误设置可以恢复；
设备切换不会破坏其他设备；
隐私和权限由我决定；
辅助设置不会被羞辱或隐藏。
```

---

## 2. Purpose

### 2.1 Player Value

该系统帮助玩家：

- 调整视觉、声音、输入和节奏；
- 适配设备与环境；
- 降低非核心障碍；
- 保存长期偏好；
- 跨设备保持适合共享的设置；
- 在误配置后恢复；
- 控制隐私、通知和社交曝光；
- 理解某项设置会影响什么；
- 在性能和画质之间做选择；
- 在不同角色、模式和内容中保持一致体验。

### 2.2 Experience Contribution

设置系统直接影响：

- 可访问性；
- 舒适度；
- 控制感；
- 信任；
- 性能；
- 阅读；
- 听觉；
- 输入；
- 隐私；
- 社交安全；
- 长期可用性。

不健康的设置系统会造成：

- 关键辅助隐藏；
- 不同设备相互覆盖；
- 改动后无法恢复；
- 控件说明与实际不一致；
- 默认值不安全；
- 需要频繁重复设置；
- 权限默认开启；
- 复杂选项缺少解释；
- 性能设置导致无法进入；
- 线上模式和单人模式规则混乱。

### 2.3 Product Value

统一设置系统可以：

- 降低各系统重复实现；
- 统一持久化和同步；
- 支持设备能力检测；
- 支持安全默认值；
- 支持可访问性合规；
- 支持隐私与权限管理；
- 支持实验和灰度；
- 支持 Support 恢复；
- 支持版本迁移；
- 降低配置冲突；
- 提高跨平台一致性。

### 2.4 Why This System Exists

缺少统一设置框架时，常见问题包括：

```text
每个页面自行保存偏好；
同一设置存在多个来源；
云端把 PC 画质同步到移动端；
输入重绑定和按键提示不同步；
无障碍设置只在首登可见；
通知关闭后仍发送；
社交隐私只影响 UI，不影响服务端；
设置损坏后应用无法启动；
实验参数覆盖玩家明确选择；
重置按钮清除了购买或存档状态。
```

---

## 3. Non-Goals

该系统不负责：

- 直接修改所有领域业务状态；
- 替代 Input and Interaction；
- 替代 Difficulty；
- 替代 Notification；
- 替代隐私政策和法律文本；
- 自动推断玩家的健康、身份或能力；
- 将关键安全功能藏在高级菜单；
- 让实验永久覆盖玩家明确选择；
- 将所有设备设置强制云同步；
- 使用复杂设置掩盖默认体验不佳；
- 让设置成为付费功能；
- 自动保证所有设备都支持全部功能。

---

## 4. Governing Principles

### 4.1 Player First Design

参考：

- `../../philosophy/foundation/player-first-design.md`

应用原则：

- 设置应容易找到；
- 关键选项有安全默认；
- 高风险改动可预览和撤销；
- 错误配置可恢复；
- 玩家明确选择优先于系统建议。

### 4.2 Clarity and Feedback

参考：

- `../../philosophy/experience/clarity-and-feedback.md`

应用原则：

- 设置名称、范围和结果清楚；
- 立即生效、下次启动生效或不支持要明确；
- 改动后提供反馈；
- 重置和恢复的影响可预览。

### 4.3 Accessibility and Inclusivity

参考：

- `../../philosophy/responsibility/accessibility-and-inclusivity.md`

应用原则：

- 辅助设置早期可达；
- 不要求用户声明身份；
- 支持多种输入、感官和认知需求；
- 辅助选项与正式体验兼容；
- 关键设置不只通过视觉表达。

### 4.4 Consistency and Coherence

参考：

- `../../philosophy/long-term/consistency-and-coherence.md`

应用原则：

- 相同设置在所有入口语义一致；
- 同类设置使用统一范围和保存规则；
- 默认值、重置值和推荐值明确区分；
- 设备和账户作用域稳定。

### 4.5 Ethical Design

参考：

- `../../philosophy/responsibility/ethical-design.md`

应用原则：

- 权限和隐私采用明确同意；
- 不使用暗黑模式隐藏拒绝；
- 通知、追踪和公开分享默认谨慎；
- 不因玩家关闭营销设置降低核心服务质量；
- 儿童账户提供更严格保护。

---

## 5. Player Experience

### 5.1 Player Goal

玩家使用设置系统通常为了：

- 调整画质；
- 改变音量；
- 修改控制；
- 调整字幕；
- 选择语言；
- 开启辅助；
- 改变难度；
- 控制通知；
- 管理隐私；
- 调整社交可见性；
- 管理存储和下载；
- 恢复默认；
- 排查问题。

### 5.2 Entry

设置入口包括：

- 首次启动；
- 主菜单；
- 暂停菜单；
- 角色或模式页面；
- 系统通知；
- 设备切换；
- 错误恢复；
- 帮助中心；
- 账户页面；
- 家长控制；
- 回归页面。

### 5.3 Main Actions

玩家可以：

- 查看；
- 修改；
- 预览；
- 应用；
- 撤销；
- 重置；
- 搜索；
- 筛选；
- 导入；
- 导出；
- 同步；
- 保存配置；
- 切换 Profile；
- 恢复 Last Known Good。

### 5.4 Core Decisions

关键决策包括：

- 是否接受推荐值；
- 是否同步到其他设备；
- 是否允许通知；
- 是否允许公开社交；
- 是否降低画质换取性能；
- 是否启用辅助；
- 是否重置全部或局部设置；
- 是否将当前配置保存为 Profile。

### 5.5 Success

健康体验意味着：

- 玩家能快速找到相关设置；
- 改动效果可理解；
- 设备特定设置不影响其他设备；
- 关键设置立即或按说明生效；
- 错误配置可恢复；
- 辅助、隐私和通知设置真实有效；
- 重绑定后提示同步更新；
- 设置迁移不丢失明确偏好。

### 5.6 Failure

失败包括：

- 设置无法保存；
- 修改后无效；
- 云同步覆盖本地；
- 设备不支持；
- 重启后恢复旧值；
- 画面设置导致黑屏；
- 输入设置导致无法操作；
- 隐私设置与服务端不同步；
- 通知关闭仍发送；
- 设置版本迁移失败。

---

## 6. System Boundary

### 6.1 Inputs

系统接收：

- Player Selection；
- Device Capability；
- Platform Policy；
- Account State；
- Save State；
- Input Device；
- Accessibility Profile；
- Privacy Consent；
- Notification Permission；
- Region and Age Context；
- Experiment Assignment；
- Live Configuration；
- Version and Schema；
- Support Recovery Request。

### 6.2 Outputs

系统产生：

- Setting Value；
- Effective Value；
- Setting Scope；
- Validation Result；
- Apply Result；
- Restart Requirement；
- Sync Result；
- Conflict State；
- Reset Result；
- Profile State；
- Permission State；
- Setting Changed Event；
- Player-Facing Explanation。

### 6.3 Owned State

系统拥有：

- Setting Definition；
- Setting Value；
- Default Value；
- Recommended Value；
- Effective Value；
- Scope；
- Setting Profile；
- Override Stack；
- Validation State；
- Apply State；
- Sync State；
- Conflict State；
- Reset State；
- Consent State Reference；
- Settings Version；
- Settings History。

### 6.4 Read-Only Dependencies

系统读取：

- Device and Platform；
- Input；
- Difficulty；
- Notification；
- Privacy；
- Save；
- Account；
- Social；
- Rendering；
- Audio；
- Network；
- Live Operations。

### 6.5 Write Dependencies

系统通过正式契约请求：

- Input 应用映射；
- Rendering 应用显示和图形；
- Audio 应用音量和输出；
- Difficulty 应用辅助和挑战设置；
- Notification 应用订阅偏好；
- Social 应用可见性和通信偏好；
- Save 持久化；
- Account 保存账户级设置；
- Analytics 记录匿名设置操作结果。

### 6.6 Out of Scope

设置系统不直接：

- 修改资源；
- 发放奖励；
- 修改角色成长；
- 改变权益；
- 处理支付；
- 绕过平台权限；
- 在未经确认时执行高影响重置。

---

## 7. Core Entities and Concepts

| Entity / Concept | Definition | Owner | Lifetime | Notes |
|---|---|---|---|---|
| Setting Definition | 设置的稳定定义 | Settings | 版本级 | 唯一 ID |
| Setting Value | 玩家或系统保存的值 | Settings | 长期或会话 | 原始值 |
| Effective Value | 合并所有层后的实际值 | Settings | 动态 | 权威应用值 |
| Default Value | 产品默认值 | Settings | 版本级 | 可迁移 |
| Recommended Value | 根据设备和情境建议 | Settings | 动态 | 不覆盖明确选择 |
| Scope | 设置作用范围 | Settings | 定义级 | Device / Account 等 |
| Settings Profile | 一组可命名设置 | Settings | 长期 | 可导入导出 |
| Override | 临时或模式级覆盖 | Settings / Domain | 短期 | 有优先级 |
| Validation Rule | 设置合法性检查 | Settings | 版本级 | 能力、范围、组合 |
| Apply Policy | 何时生效 | Settings | 定义级 | Immediate / Restart |
| Consent State | 权限或隐私同意 | Privacy / Platform | 长期 | Settings 只引用 |
| Capability | 设备是否支持 | Device / Platform | 动态 | 不等同可见性 |
| Last Known Good | 最近成功应用的配置 | Settings | 长期 | 用于恢复 |
| Settings History | 关键变更记录 | Settings | 审计期 | 不保存敏感内容 |

---

## 8. Settings Taxonomy

### 8.1 Display

- 分辨率；
- 窗口模式；
- 显示器；
- 安全区域；
- UI 缩放；
- 亮度；
- HDR；
- 刷新率；
- 垂直同步；
- 帧率限制。

### 8.2 Graphics

- 质量预设；
- 纹理；
- 阴影；
- 特效；
- 后处理；
- 抗锯齿；
- 动态分辨率；
- 运动模糊；
- 景深；
- 粒子；
- 视距。

### 8.3 Audio

- 主音量；
- 音乐；
- 音效；
- 对话；
- UI；
- 语音聊天；
- 输出设备；
- 动态范围；
- 单声道；
- 字幕关联。

### 8.4 Language and Localization

- 界面语言；
- 字幕语言；
- 语音语言；
- 数字格式；
- 日期格式；
- 输入法；
- 字体；
- 文本方向。

### 8.5 Input

- 键位；
- 手柄；
- 灵敏度；
- 死区；
- 反转；
- Hold / Toggle；
- 连发；
- 手势；
- 振动；
- 陀螺仪。

### 8.6 Accessibility

- 字幕；
- 文字大小；
- 高对比；
- 色觉；
- 减少运动；
- 屏幕阅读；
- 音频替代；
- 单手；
- 慢速；
- 辅助瞄准；
- 自动化；
- 认知提示。

### 8.7 Gameplay

- 难度；
- 镜头；
- 自动拾取；
- 自动保存；
- 提示；
- 教学；
- 伤害数字；
- 目标高亮；
- 跳过演出；
- 快速确认。

### 8.8 Social

- 好友请求；
- 邀请；
- 语音；
- 文本；
- 公开状态；
- 跨平台；
- 屏蔽；
- 举报反馈；
- 在线状态。

### 8.9 Privacy

- 数据分析；
- 个性化；
- 追踪；
- 位置；
- 联系人；
- 公开 Profile；
- 搜索可见性；
- 数据导出；
- 数据删除。

### 8.10 Notifications

- 系统；
- 活动；
- 社交；
- 奖励；
- 商业；
- 安全；
- 邮件；
- Push；
- Quiet Hours。

### 8.11 Network and Download

- 下载质量；
- 后台下载；
- 数据节省；
- 仅 Wi-Fi；
- 区域；
- 服务器；
- 语音带宽；
- 云同步。

### 8.12 Account and Family

- 登录；
- 账户切换；
- 家长控制；
- 年龄；
- PIN；
- 支出限制；
- 社交限制；
- 时间限制。

---

## 9. Setting Definition Template

```markdown
## Setting Definition

- Setting ID:
- Display Name:
- Category:
- Description:
- Value Type:
- Allowed Values:
- Default Value:
- Recommended Value:
- Scope:
- Apply Policy:
- Persistence:
- Cloud Sync:
- Device Capability:
- Dependencies:
- Conflicts:
- Accessibility Impact:
- Privacy Impact:
- Reset Group:
- Version:
- Owner:
- Risk Level:
```

### 9.1 必须回答

- 设置解决什么问题；
- 玩家为什么需要；
- 取值范围；
- 默认值为何安全；
- 推荐值如何产生；
- 属于哪个作用域；
- 是否云同步；
- 是否立即生效；
- 是否需要预览或确认；
- 失败时如何恢复。

---

## 10. Setting Value Types

### 10.1 Boolean

开 / 关。

### 10.2 Enum

有限离散选项。

### 10.3 Numeric

整数或浮点范围。

### 10.4 Range

最小、最大和步长。

### 10.5 Curve

灵敏度、音频等可使用曲线。

### 10.6 Binding

输入动作与控件映射。

### 10.7 Collection

例如：

- 屏蔽用户列表；
- 允许通知分类；
- 语言优先级。

### 10.8 Structured Profile

多项组合。

### 10.9 Secret or Credential Reference

不应直接以普通 Setting Value 存储密钥或凭据。

---

## 11. Scope Model

### 11.1 Device Scope

只适用于当前设备。

例如：

- 分辨率；
- 图形质量；
- 输出设备；
- 窗口位置。

### 11.2 Platform Scope

适用于同一平台生态。

### 11.3 Account Scope

跨设备共享。

例如：

- 字幕；
- 通用语言；
- 隐私；
- 通知偏好；
- 部分可访问性。

### 11.4 Save Slot Scope

随特定存档。

例如：

- 难度；
- 教学提示；
- 游戏性偏好；
- 当前相机模式。

### 11.5 Character Scope

特定角色。

例如：

- 角色自动化；
- 技能布局；
- 镜头偏好。

### 11.6 Mode Scope

特定模式。

### 11.7 Session Scope

仅本次会话。

### 11.8 Temporary Override

由：

- 教学；
- 竞技；
- 安全模式；
- 平台；
- 内容；

临时覆盖。

---

## 12. Scope Invariants

1. Device Scope 不应云端覆盖其他设备。
2. Account Scope 不应由单一 Save Slot 删除。
3. Session Scope 不应持久化为长期偏好，除非玩家明确保存。
4. Mode Override 结束后恢复之前 Effective Value。
5. Temporary Override 必须可追踪和自动清理。
6. Privacy Consent 不应被普通 Reset 清除。
7. 儿童或家长策略优先于低优先级玩家设置。
8. 平台硬限制优先于产品推荐，但要明确说明。
9. 玩家明确选择优先于实验和推荐。
10. 高风险安全设置不能被内容脚本静默修改。

---

## 13. Value Resolution

实际值通常来自多层：

```text
Platform Hard Limit
→ Legal / Age Restriction
→ Safety Policy
→ Mode Restriction
→ Account Preference
→ Save Preference
→ Character / Profile Preference
→ Session Override
→ Recommended Fallback
→ Product Default
```

### 13.1 Effective Value

系统最终应用 Effective Value，而不是直接使用单层原始值。

### 13.2 Resolution Trace

高复杂设置应可解释：

- 当前值来自哪里；
- 为什么不能修改；
- 哪一层覆盖；
- 何时恢复。

### 13.3 Avoid “Last Callback Wins”

覆盖顺序必须显式定义。

---

## 14. Default, Recommended, Current, Effective

### 14.1 Default Value

产品的安全基础值。

### 14.2 Recommended Value

根据：

- 设备；
- 性能；
- 平台；
- 内容；
- 自我选择；

产生的建议。

### 14.3 Current Value

玩家当前保存值。

### 14.4 Effective Value

所有限制和覆盖后的实际值。

### 14.5 Reset Value

重置时恢复的目标值。

可能是：

- Product Default；
- Device Recommended；
- Accessibility Profile Default；
- Last Known Good。

必须明确。

---

## 15. Safe Defaults

默认值应优先：

- 可阅读；
- 可听见；
- 可控制；
- 可退出；
- 性能稳定；
- 隐私谨慎；
- 通知克制；
- 社交安全；
- 不产生高额数据消耗；
- 不自动开启商业追踪。

### 15.1 Platform Defaults

可以参考平台惯例，但不能无视产品风险。

### 15.2 Region and Age

默认值可能因法律、年龄和地区不同。

### 15.3 No Hidden Aggressive Defaults

不应默认：

- 公开在线状态；
- 自动加入语音；
- 营销通知；
- 个性化追踪；
- 高画质导致不稳定；
- 自动续费；
- 自动公开分享。

---

## 16. Apply Policies

### 16.1 Immediate

修改后立即生效。

适合：

- 音量；
- 字幕；
- UI 缩放；
- 部分输入；
- 高对比。

### 16.2 Preview and Confirm

先预览，玩家确认。

适合：

- 分辨率；
- 显示器；
- HDR；
- 高风险图形模式。

### 16.3 Next Context

下次进入内容或模式生效。

### 16.4 Restart Required

重新启动应用后生效。

### 16.5 Re-login Required

账户或隐私变更需要重新登录。

### 16.6 Server Confirmation

隐私、社交、通知或账户设置需要服务端确认。

### 16.7 Apply Failure

失败时恢复 Last Known Good，并解释原因。

---

## 17. Setting Lifecycle

```text
Default
→ Proposed
→ Validating
→ Previewing
→ Applying
→ Applied
→ Persisting
→ Synced
```

异常：

```text
Validating
→ Rejected
Applying
→ Failed
Persisting
→ Save Failed
Synced
→ Conflict
```

```mermaid
stateDiagram-v2
    [*] --> Default
    Default --> Proposed
    Proposed --> Validating
    Validating --> Previewing
    Previewing --> Applying
    Validating --> Applying
    Applying --> Applied
    Applied --> Persisting
    Persisting --> Synced
    Validating --> Rejected
    Applying --> Failed
    Persisting --> SaveFailed
    Synced --> Conflict
```

---

## 18. Setting State Invariants

1. 未通过 Validation 的值不能成为 Effective Value。
2. Preview 超时必须恢复 Last Known Good。
3. Apply 成功不等于持久化成功，状态需区分。
4. Player Choice 不应被实验静默覆盖。
5. 服务端设置必须在确认后显示为最终成功。
6. Device Capability 变化后应重新验证。
7. 重启要求必须在离开页面前提示。
8. 设置保存失败不能伪装为成功。
9. Cloud Conflict 不应静默覆盖两端。
10. Reset 不能影响超出选择范围的状态。
11. 辅助设置不能因进入竞技而被删除，只能被明确限制或替代。
12. 权限拒绝必须真实生效。

---

## 19. Validation

### 19.1 Structural Validation

类型、范围、枚举、必填。

### 19.2 Capability Validation

设备是否支持。

### 19.3 Dependency Validation

相关功能是否开启。

### 19.4 Conflict Validation

与其他设置是否冲突。

### 19.5 Policy Validation

法律、年龄、平台和安全限制。

### 19.6 Context Validation

当前模式是否允许修改。

### 19.7 Business Validation

订阅、权益或服务是否支持，但基础辅助不应被付费限制。

---

## 20. Dependency and Conflict Rules

### 20.1 Dependency

例如：

```text
HDR
依赖 HDR 显示器和兼容输出模式。
```

### 20.2 Mutual Exclusion

例如：

```text
Exclusive Fullscreen
与某些窗口模式互斥。
```

### 20.3 Conditional Visibility

只在相关能力存在时显示，但应说明不可用原因。

### 20.4 Conflict Resolution

可以：

- 自动调整；
- 提示选择；
- 阻止；
- 使用安全值；
- 保存但暂不生效。

### 20.5 No Silent Cascades

一项设置自动改变多项时，应展示变化摘要。

---

## 21. Profiles and Presets

### 21.1 Profile Types

- Default；
- Performance；
- Quality；
- Accessibility；
- Streaming；
- Quiet；
- Competitive；
- Custom。

### 21.2 Profile Behavior

应用 Profile 前展示：

- 会改变哪些项；
- 不改变哪些项；
- 设备限制；
- 是否可撤销；
- 是否保存为当前偏好。

### 21.3 Partial Profile

可以只覆盖：

- 图形；
- 音频；
- 输入；
- 辅助。

### 21.4 Custom Profile

支持保存、命名、复制、导入和删除。

### 21.5 Profile Ownership

Profile 保存引用或值集合，不应复制权限和账户安全状态。

---

## 22. Search and Discoverability

设置页面应支持：

- 搜索；
- 分类；
- 最近修改；
- 推荐；
- 问题导向入口；
- 帮助链接；
- 相关设置；
- 重置分组。

### 22.1 Search Terms

支持：

- 正式名称；
- 常用词；
- 平台术语；
- 同义词；
- 辅助需求描述。

### 22.2 Problem-Based Entry

例如：

- “文字太小”
- “画面晕”
- “听不清对话”
- “按住太累”
- “掉帧”
- “不想收到活动通知”

比只显示技术术语更易发现。

---

## 23. Settings Navigation

推荐结构：

```text
General
Display
Graphics
Audio
Language
Input
Accessibility
Gameplay
Social
Privacy
Notifications
Network
Account
```

### 23.1 Avoid Deep Nesting

关键设置不应藏在多层菜单。

### 23.2 Contextual Entry

允许从：

- 错误提示；
- 教学；
- 失败支持；
- 设备切换；
- 系统页面；

直接打开相关设置，并返回原上下文。

### 23.3 Search Result Context

搜索结果应显示所属分类和影响范围。

---

## 24. Input Settings

### 24.1 Action-Based Binding

设置操作语义 Action，而不是只设置硬件按键。

### 24.2 Device Profiles

分别保存：

- Keyboard / Mouse；
- Controller；
- Touch；
- Assistive Device。

### 24.3 Rebinding

支持：

- 冲突检测；
- Essential Action 保护；
- 备用绑定；
- Reset；
- Preset；
- 当前设备提示同步。

### 24.4 Sensitivity and Deadzone

应支持：

- 独立轴；
- 曲线；
- 预览；
- 测试区域；
- 恢复默认。

### 24.5 Hold / Toggle

提供：

- Hold；
- Toggle；
- Sticky；
- Auto Repeat；
- Long Press Alternative。

---

## 25. Display and Graphics Settings

### 25.1 Capability Detection

检测：

- GPU；
- 显存；
- 显示器；
- HDR；
- 刷新率；
- 分辨率；
- 平台限制。

### 25.2 Auto Detect

推荐值不等于最终值。

玩家修改后不应每次启动重新覆盖。

### 25.3 Safe Preview

高风险显示设置应：

- 倒计时；
- 确认；
- 自动恢复；
- 保留键盘和手柄确认；
- 支持无画面恢复快捷方式。

### 25.4 Performance Preset

应说明：

- 目标；
- 主要变化；
- 预计影响；
- 是否动态调整。

### 25.5 Dynamic Quality

动态调整需透明，并允许：

- 开关；
- 目标帧率；
- 最低画质；
- 变化范围。

---

## 26. Audio Settings

### 26.1 Volume Groups

- Master；
- Music；
- Effects；
- Dialogue；
- UI；
- Voice Chat；
- Accessibility Cues。

### 26.2 Output Device

设备变化时：

- 检测；
- 提示；
- 自动切换策略；
- 保留玩家选择；
- 提供测试。

### 26.3 Dynamic Range

可提供：

- Night；
- Standard；
- Wide。

### 26.4 Mono Audio

需要明确应用到：

- 游戏音频；
- 语音；
- 提示。

### 26.5 Audio Descriptions

与字幕和辅助提示独立控制。

---

## 27. Language and Localization Settings

### 27.1 Language Types

- UI；
- Subtitle；
- Voice；
- Input；
- Region Format。

### 27.2 Apply Policy

语言可能：

- 立即应用；
- 重新加载页面；
- 下载语言包；
- 重启。

### 27.3 Fallback

缺少文本时使用：

- 安全回退语言；
- 不显示内部 Key；
- 明确下载状态。

### 27.4 Font and Text Direction

语言变化可能影响：

- 字体；
- 行高；
- 布局；
- 焦点顺序；
- 文本方向；
- 输入法。

---

## 28. Accessibility Settings

### 28.1 Visual

- 字体大小；
- UI Scale；
- 高对比；
- 色觉模式；
- Outline；
- 目标高亮；
- 减少闪烁；
- 减少特效；
- 屏幕阅读。

### 28.2 Hearing

- 字幕；
- Speaker Labels；
- Sound Visualization；
- Mono；
- Dialogue Boost；
- Voice-to-Text。

### 28.3 Motor

- 重绑定；
- Hold / Toggle；
- 单手；
- Sticky；
- Auto Repeat；
- 输入窗口；
- 灵敏度；
- Aim Assist。

### 28.4 Cognitive

- 简化 HUD；
- 目标摘要；
- 提示；
- 导航辅助；
- 日志；
- 减少同时信息；
- 文字速度；
- 暂停。

### 28.5 Motion and Comfort

- 减少镜头晃动；
- 减少运动模糊；
- FOV；
- 中心点；
- 固定地平线；
- 慢速；
- 镜头距离。

### 28.6 Assist Presets

预设是起点，不是身份标签。

### 28.7 Availability

辅助设置应在：

- 首次启动；
- 主菜单；
- 暂停；
- 失败支持；

中可达。

---

## 29. Difficulty and Gameplay Preferences

### 29.1 Difficulty Mode

保存：

- Story；
- Standard；
- Hard；
- Custom；
- Assist。

### 29.2 Per-Dimension Difficulty

可独立设置：

- 时间；
- 伤害；
- 提示；
- Checkpoint；
- 资源；
- 输入窗口；
- 失败惩罚。

### 29.3 Mode Restrictions

竞技模式可能限制部分设置，但必须：

- 透明；
- 不删除偏好；
- 离开后恢复；
- 保留基础可访问性。

### 29.4 Gameplay Convenience

例如：

- 自动拾取；
- 跳过重复演出；
- 自动装备；
- 快速确认；
- 伤害数字；
- 教学提示。

---

## 30. Notification Settings

### 30.1 Channel

- In-App；
- Push；
- Email；
- SMS；
- Platform；
- Calendar。

### 30.2 Category

- Security；
- Service；
- Social；
- Reward；
- Event；
- Content；
- Commercial；
- Reminder。

### 30.3 Critical vs Optional

安全和服务必要通知与营销通知分离。

### 30.4 Quiet Hours

支持：

- 时区；
- 开始和结束；
- 工作日；
- 例外；
- 紧急通知。

### 30.5 Permission vs Preference

平台权限与产品偏好是两层：

```text
Platform Permission
+
Product Category Preference
=
Effective Delivery
```

---

## 31. Social Settings

### 31.1 Visibility

- Online Status；
- Activity；
- Profile；
- Achievements；
- Current Content；
- Friend List。

### 31.2 Communication

- Text；
- Voice；
- Invite；
- Friend Request；
- Cross-Platform；
- Direct Message。

### 31.3 Safety

- Block；
- Mute；
- Report；
- Filter；
- Parental Restriction。

### 31.4 Scope

可按：

- Everyone；
- Friends；
- Party；
- Nobody；
- Custom。

### 31.5 Server Enforcement

社交设置必须由服务端实际执行，而不是只隐藏 UI。

---

## 32. Privacy Settings

### 32.1 Consent Categories

- Necessary；
- Analytics；
- Personalization；
- Marketing；
- Location；
- Contacts；
- Voice；
- Tracking。

### 32.2 Granularity

避免：

```text
全部接受
或
完全无法使用。
```

除非某项确属必要。

### 32.3 Consent Record

由 Privacy / Account 系统权威保存：

- Scope；
- Version；
- Timestamp；
- Region；
- Age Context；
- Source；
- Withdrawal。

Settings 显示和请求变更。

### 32.4 Withdrawal

撤回应：

- 真实生效；
- 说明影响；
- 停止后续处理；
- 根据政策删除或停止使用相关数据。

### 32.5 No Deceptive UI

接受和拒绝应：

- 同等清楚；
- 不使用羞辱文案；
- 不隐藏拒绝；
- 不反复骚扰。

---

## 33. Account and Family Settings

### 33.1 Account

- 绑定；
- 切换；
- 安全；
- 二次验证；
- 设备管理；
- 登录历史。

### 33.2 Family and Parental Controls

- 购买限制；
- 社交限制；
- 时间限制；
- 内容过滤；
- 通知；
- 语音；
- 公开分享；
- 数据权限。

### 33.3 PIN and Authorization

高风险变更需要：

- 身份验证；
- PIN；
- 监护确认；
- 审计。

### 33.4 Child Account

默认更谨慎，并防止通过设备 Profile 绕过。

---

## 34. Network and Download Settings

### 34.1 Download

- 自动更新；
- 后台下载；
- 仅 Wi-Fi；
- 高清资源；
- 语言包；
- 可选内容。

### 34.2 Data Usage

- Low；
- Standard；
- High；
- Custom。

### 34.3 Server / Region

手动选择需要说明：

- 延迟；
- 匹配；
- 数据；
- 账户；
- 权益；
- 法律限制。

### 34.4 Cloud Sync

支持：

- 开启；
- 暂停；
- 仅 Wi-Fi；
- 手动同步；
- 冲突状态；
- 设备管理。

---

## 35. Device Capability Changes

设备能力可能因：

- 显示器切换；
- 控制器连接；
- 音频设备切换；
- 权限变化；
- 驱动更新；
- 电池模式；
- 性能模式；

变化。

### 35.1 Revalidation

能力变化后重新验证相关设置。

### 35.2 Preserve Intent

如果暂时不支持，应：

- 保留玩家原值；
- 使用安全 Effective Value；
- 条件恢复时重新可用。

### 35.3 Do Not Destroy Preferences

例如 HDR 显示器断开，不应删除 HDR 偏好。

---

## 36. Temporary Overrides

### 36.1 Sources

- Tutorial；
- Competitive Mode；
- Photo Mode；
- Streaming；
- Safe Mode；
- Performance Emergency；
- Parental Policy；
- Platform Policy。

### 36.2 Override Contract

必须定义：

- Owner；
- Priority；
- Scope；
- Start；
- End；
- Player Feedback；
- Restore Behavior。

### 36.3 Restore

结束后恢复之前值，而不是 Default。

### 36.4 Player Awareness

高影响 Override 必须明确显示。

---

## 37. Experiment Overrides

### 37.1 Allowed Uses

- 新默认测试；
- 设置布局；
- 推荐算法；
- 解释文案；
- 非关键参数。

### 37.2 Disallowed Uses

实验不得静默覆盖：

- 玩家明确选择；
- 隐私；
- 权限；
- 儿童保护；
- 关键辅助；
- 安全；
- 付费确认。

### 37.3 Experiment End

迁移：

- 玩家选择；
- 实验值；
- 新默认；
- 旧默认；
- 历史。

### 37.4 Default Experiment

测试默认值时，应区分：

```text
尚未选择的玩家
```

与：

```text
已经明确修改的玩家。
```

---

## 38. Save and Cloud Sync

### 38.1 Persistence

每个 Setting Definition 指定：

- Local；
- Account；
- Save Slot；
- Session；
- Server；
- No Persistence。

### 38.2 Sync Strategy

- None；
- Account-Wide；
- Platform；
- Device Family；
- Per-Field；
- Profile-Based。

### 38.3 Merge

设置通常按字段合并，而不是整份覆盖。

### 38.4 Revision

记录：

- Setting ID；
- Value；
- Modified At；
- Device Class；
- Revision；
- Source；
- Version。

### 38.5 Device-Specific Exclusion

画质和硬件输入不应错误跨设备同步。

---

## 39. Settings Conflict

冲突可能发生于：

- 多设备离线修改；
- Profile 与单项修改；
- 家长策略与玩家偏好；
- 平台限制与账户值；
- 实验与明确选择；
- 模式 Override 与全局设置；
- 版本迁移。

### 39.1 Automatic Resolution

可根据：

- Priority；
- Scope；
- Revision；
- Capability；
- Player Choice；

解决。

### 39.2 Player Choice

适用于：

- 两个自定义 Profile；
- 输入映射；
- 复杂辅助组合；
- 隐私偏好冲突。

### 39.3 Preserve Both

无法安全合并时保留：

- 本地 Profile；
- 云端 Profile；
- 重命名副本。

---

## 40. Settings Migration

### 40.1 Migration Types

- Rename；
- Value Range Change；
- Enum Change；
- Split；
- Merge；
- Unit Change；
- Scope Change；
- Default Change；
- Capability Change；
- Deprecated Setting。

### 40.2 Migration Principles

- 保留玩家意图；
- 不把旧缺省误当明确选择；
- 不把明确关闭改成开启；
- 辅助和隐私优先保护；
- 迁移失败使用 Last Known Good；
- 旧值可审计。

### 40.3 Default Change

新版本改变 Default 时：

- 未修改用户使用新 Default；
- 已明确选择用户保留选择；
- 无法判断时采用谨慎策略。

### 40.4 Deprecated Setting

应：

- 映射；
- 解释；
- 保留历史；
- 删除旧入口；
- 不静默改变体验。

---

## 41. Reset

### 41.1 Reset Types

- Single Setting；
- Category；
- Profile；
- Device；
- Input；
- Accessibility；
- Privacy；
- All Preferences。

### 41.2 Reset Scope

必须显示：

- 会重置什么；
- 不会影响什么；
- 是否云同步；
- 是否需要重启；
- 是否可以撤销。

### 41.3 Protected Settings

以下不应被普通“恢复默认”清除：

- 账户安全；
- 法律同意记录；
- 家长控制；
- 购买权益；
- 存档；
- 屏蔽和举报历史；
- 删除请求。

### 41.4 Accessibility Reset

重置辅助设置需要额外确认和安全替代。

---

## 42. Last Known Good and Safe Mode

### 42.1 Last Known Good

保存最近成功应用的：

- 显示；
- 图形；
- 输入；
- 音频；
- 语言；
- 基础可访问性。

### 42.2 Safe Mode Trigger

例如：

- 启动崩溃；
- 黑屏；
- 连续加载失败；
- 输入不可用；
- 配置损坏。

### 42.3 Safe Mode Behavior

- 安全分辨率；
- 窗口模式；
- 低图形；
- 默认输入；
- 默认音频；
- 保留语言和辅助；
- 禁用实验 Override；
- 提供恢复入口。

### 42.4 Do Not Erase

Safe Mode 不应立即删除玩家自定义值。

---

## 43. Import and Export

### 43.1 Exportable Profiles

可导出：

- 输入映射；
- 图形 Profile；
- 音频；
- 辅助；
- HUD；
- 通用偏好。

### 43.2 Non-Exportable

不应导出：

- 凭据；
- 权限 Token；
- 家长 PIN；
- 敏感隐私状态；
- 安全密钥。

### 43.3 Import Validation

检查：

- Schema；
- Version；
- Capability；
- Security；
- Conflicts；
- Scope。

### 43.4 Preview

导入前展示将改变的设置。

---

## 44. Player-Facing Feedback

### 44.1 Saved

偏好已持久化。

### 44.2 Applied

实际系统已应用。

### 44.3 Restart Required

需要重启。

### 44.4 Unsupported

设备或平台不支持。

### 44.5 Temporarily Overridden

当前模式暂时覆盖。

### 44.6 Sync Pending

本地已保存，云端未同步。

### 44.7 Conflict

需要解决。

### 44.8 Reverted

预览超时或应用失败，已恢复。

### 44.9 Feedback Rules

- 区分 Saved 与 Applied；
- 不只显示图标；
- 失败提供下一步；
- 高风险改变有撤销；
- 覆盖来源可解释。

---

## 45. Accessibility of Settings UI

### 45.1 Visual

- 字体可调整；
- 高对比；
- 焦点清楚；
- 状态不只靠颜色；
- 示例预览可关闭。

### 45.2 Screen Reader

每项设置应提供：

- 名称；
- 当前值；
- 可选值；
- 描述；
- 是否禁用；
- 禁用原因；
- 是否需要重启。

### 45.3 Input

- 键鼠；
- 手柄；
- 触摸；
- 辅助设备；
- 不强制拖拽；
- 滑块可用按钮精调。

### 45.4 Cognitive

- 分类清楚；
- 使用普通语言；
- 技术细节按需展开；
- 搜索和推荐；
- 变化摘要；
- 重置分组。

### 45.5 Timing

- 显示预览倒计时足够；
- 可暂停阅读；
- 冲突解决不要求快速决定；
- 长操作有状态说明。

---

## 46. Privacy and Security

### 46.1 Sensitive Settings

- 账户；
- 家长控制；
- 隐私；
- 社交；
- 支付限制；
- 数据导出；
- 删除。

### 46.2 Authorization

高风险设置需要：

- 重新认证；
- PIN；
- 二次验证；
- 平台权限；
- 监护批准。

### 46.3 Audit

记录：

- Setting ID；
- Old / New；
- Source；
- Timestamp；
- Authorization；
- Result。

不记录：

- 密码；
- Token；
- PIN 明文；
- 敏感内容。

### 46.4 Client Trust

服务端隐私和社交设置不能只依赖客户端值。

---

## 47. Support and Diagnostics

### 47.1 Support View

可查看：

- Settings Version；
- Effective Value；
- Override Source；
- Capability；
- Apply Failure；
- Sync State；
- Last Known Good；
- Migration History。

### 47.2 Redaction

Support 不应看到：

- 密码；
- 完整个人数据；
- 私密通信；
- 家长 PIN；
- 安全 Token。

### 47.3 Diagnostic Export

可以生成脱敏诊断包。

### 47.4 Recovery Actions

Support 可以：

- 恢复 LKG；
- 清理损坏 Profile；
- 重新同步；
- 移除失效 Override；
- 触发迁移。

需要最小权限和审计。

---

## 48. Performance

### 48.1 Apply Cost

设置应用可能影响：

- 帧率；
- 资源加载；
- Shader；
- 音频设备；
- 输入重建；
- UI 重布局。

### 48.2 Debounce

滑块等高频变化需要防抖。

### 48.3 Preview Isolation

预览不应频繁写入长期存档。

### 48.4 Background Work

资源重载可以异步，但 Effective Value 切换边界要明确。

### 48.5 Large Profiles

Profile 合并和同步需控制大小。

---

## 49. Failure and Recovery

| Failure | Cause | Player Impact | Recovery | Data Guarantee |
|---|---|---|---|---|
| Apply Failed | 设备或运行时错误 | 设置无效 | 恢复 LKG | 原值保留 |
| Save Failed | 存储问题 | 重启后丢失 | 重试、提示 | 不显示 Saved |
| Sync Failed | 网络或服务 | 其他设备不同步 | 保留本地、后台重试 | 本地值不丢 |
| Conflict | 多设备修改 | 覆盖风险 | 字段合并或 Profile 副本 | 双方保留 |
| Unsupported Value | 能力变化 | 功能不可用 | 使用安全 Effective Value | 原意图保留 |
| Input Lockout | 绑定错误 | 无法操作 | 恢复 Essential Actions | 备份映射 |
| Display Black Screen | 分辨率或 HDR 错误 | 无画面 | 倒计时恢复 / Safe Mode | LKG 保留 |
| Privacy Desync | 服务端未确认 | 实际权限不一致 | 查询和重试 | 不显示最终成功 |
| Migration Failed | 版本变化 | 设置丢失或异常 | LKG、旧 Profile | 原始值保留 |
| Override Leak | 模式结束未恢复 | 设置持续错误 | 清理 Override Stack | 历史可审计 |
| Reset Overreach | 范围错误 | 清除过多偏好 | 恢复备份 | Protected State 不变 |
| Corrupted Profile | 文件损坏 | 无法加载 | 隔离、LKG、默认 | 不覆盖证据 |

---

## 50. Edge Cases

### Device

- 显示器拔出；
- 控制器断开；
- 音频设备切换；
- HDR 能力变化；
- 平台休眠；
- 外接键盘；
- 多控制器；
- 云游戏平台。

### Apply

- 修改时进入后台；
- 预览倒计时中崩溃；
- 重启要求未完成；
- 应用与保存顺序不同；
- 多项设置部分成功；
- 模式切换中修改。

### Sync

- 两设备离线；
- Device Scope 被错误同步；
- Profile 同名；
- 云端 Schema 更新；
- 玩家关闭同步；
- 旧端上传已废弃值。

### Privacy and Social

- 撤回同意时离线；
- 儿童账户转成年账户；
- 地区变化；
- 平台权限关闭；
- 社交服务不可用；
- 家长 PIN 丢失。

### Accessibility

- 辅助 Profile 与竞技限制；
- 屏幕阅读器改变焦点；
- 大字体导致布局变化；
- 语言变化影响读屏；
- 设备切换导致辅助映射失效。

---

## 51. Cross-System Dependencies

| System | Dependency Type | Direction | Data or Event | Failure Impact |
|---|---|---|---|---|
| Input and Interaction | Hard | 双向 | Bindings / Sensitivity | 无法操作 |
| Game State and Flow | Hard / Soft | 双向 | Apply Timing / Override | 流程异常 |
| Save and Persistence | Hard | 双向 | Values / Profiles / Sync | 偏好丢失 |
| Tutorial and Onboarding | Soft / Hard | 双向 | Settings Discovery | 新手障碍 |
| Difficulty and Challenge | Hard / Soft | 双向 | Difficulty / Assists | 挑战不适配 |
| Notification and Reminders | Hard / Soft | 双向 | Channel Preferences | 误发通知 |
| Social and Multiplayer | Hard / Soft | 双向 | Visibility / Communication | 隐私风险 |
| Rendering | Hard | Settings → Rendering | Graphics / Display | 黑屏或性能问题 |
| Audio | Hard | Settings → Audio | Volume / Output | 无声或设备错误 |
| Privacy / Account | Critical | 双向 | Consent / Restrictions | 合规风险 |
| Analytics | Soft | Settings → Analytics | Operation Events | 不阻断 |
| Experiment Management | Soft / Hard | 双向 | Defaults / Layout | 玩家选择被覆盖 |
| Live Operations | Soft | Live → Settings | Recommendations | 使用 LKG |

---

## 52. Data and Persistence

| State | Persistent | Authority | Save Trigger | Retention | Recovery |
|---|---|---|---|---|---|
| Setting Definition | 是 | Settings | 配置发布 | 版本期 | LKG |
| Player Value | 是 | Settings | 修改确认 | 长期 | Profile / Backup |
| Effective Value | 可重算 | Settings | Override 变化 | 当前会话 | Resolution |
| Device Capability | 可重算 | Device | 能力变化 | 当前会话 | 重新检测 |
| Settings Profile | 是 | Settings | 保存 | 长期 | Profile Backup |
| Override Stack | 会话或模式级 | Settings | Override 变化 | 短期 | 自动清理 |
| Sync Metadata | 是 | Settings / Save | 同步变化 | 长期 | Cloud Manifest |
| Consent Reference | 是 | Privacy / Account | 同意变化 | 政策期 | 权威查询 |
| Last Known Good | 是 | Settings | 成功应用 | 轮转 | Safe Mode |
| Migration History | 是 | Settings | 版本变化 | 审计期 | 旧值 |
| Settings History | 是 | Settings | 高风险变化 | 审计期 | Support |

---

## 53. Analytics and Validation

### 53.1 Key Assumptions

1. 玩家能快速找到相关设置。
2. 设置名称和影响可理解。
3. Device、Account、Save 和 Session Scope 不会错误互相覆盖。
4. 推荐值不会覆盖明确选择。
5. 高风险设置可以安全预览和恢复。
6. 辅助、隐私、通知和社交设置真实生效。
7. 跨设备同步只同步适合共享的字段。
8. 迁移能保留玩家意图。
9. 实验和 Live Config 不会越过玩家选择。
10. 设置 UI 本身可访问。

### 53.2 Validation Plan

| Hypothesis | Evidence | Success | Failure | Method |
|---|---|---|---|---|
| 可发现 | 问题导向任务 | 能找到相关选项 | 依赖外部搜索 | Usability Test |
| 可理解 | 复述 | 能说明影响和范围 | 误解或不敢修改 | Research |
| Scope 正确 | 跨设备测试 | 设备设置不串扰 | PC 值覆盖移动端 | QA |
| 推荐尊重选择 | 重启和更新 | 明确选择保留 | 每次被自动覆盖 | Integration Test |
| 预览安全 | 黑屏测试 | 自动恢复 | 无法进入 | Fault Injection |
| 隐私真实 | 服务端验证 | 设置与实际一致 | UI 关闭但仍处理 | Security / Privacy Test |
| 辅助有效 | 目标用户测试 | 能找到并完成体验 | 入口隐藏或失效 | Accessibility Test |
| 迁移保留意图 | 历史版本 | 明确开关不翻转 | 默认误判 | Migration Test |
| UI 可访问 | 多输入和读屏 | 全流程可操作 | 焦点或标签错误 | Accessibility QA |

### 53.3 Behavioral Metrics

- Settings Opened；
- Setting Searched；
- Setting Changed；
- Setting Applied；
- Setting Reverted；
- Reset Used；
- Profile Saved；
- Profile Applied；
- Accessibility Opened；
- Permission Changed；
- Notification Preference Changed；
- Conflict Detected；
- Safe Mode Entered；
- Migration Completed。

### 53.4 Outcome Metrics

- Time to Find Setting；
- Apply Success；
- Revert Success；
- Input Lockout Rate；
- Display Recovery Rate；
- Sync Success；
- Conflict Resolution；
- Accessibility Adoption；
- Notification Opt-Out Effectiveness；
- Privacy Enforcement；
- Migration Success；
- Support Volume；
- Safe Mode Recovery。

### 53.5 Negative Metrics

- 设置无法保存；
- 改动无效；
- 云端错误覆盖；
- 输入锁死；
- 黑屏；
- 辅助设置隐藏；
- 通知关闭仍发送；
- 隐私撤回未生效；
- 儿童限制被绕过；
- 实验覆盖玩家选择；
- Override 未清理；
- 重置清除过多状态；
- Device Scope 错误同步；
- 迁移翻转明确选择。

### 53.6 Event Intents

| Event Intent | Trigger | Key Properties | Privacy Notes |
|---|---|---|---|
| Setting Applied | 应用成功 | Category, Scope, Result | 不记录敏感具体值 |
| Setting Reverted | 自动恢复 | Reason, Category | 数据最小化 |
| Settings Conflict Detected | 同步冲突 | Scope, Device Class | 不记录设备唯一标识 |
| Accessibility Entry Used | 打开辅助 | Category | 不推断健康身份 |
| Consent Change Requested | 隐私变更 | Category, Result | 高权限控制 |
| Safe Mode Entered | 安全模式 | Trigger Category | 不记录个人内容 |
| Profile Applied | Profile 应用 | Profile Type, Result | 不记录自定义名称全文 |
| Settings Migration Completed | 迁移 | From, To, Result | 审计 |

---

## 54. Test Strategy

### 54.1 Definition Tests

- 默认值；
- 允许值；
- 范围；
- Scope；
- Apply Policy；
- Reset Group；
- Capability。

### 54.2 Apply Tests

- Immediate；
- Preview；
- Restart；
- Partial Failure；
- Rollback；
- LKG。

### 54.3 Device Tests

- 多分辨率；
- HDR；
- 多音频设备；
- 键鼠；
- 手柄；
- 触摸；
- 辅助设备；
- 热插拔。

### 54.4 Sync Tests

- 多设备；
- 离线；
- 冲突；
- Profile；
- Device Scope；
- Account Scope；
- 旧客户端。

### 54.5 Migration Tests

- Default Change；
- Rename；
- Split；
- Merge；
- Scope Change；
- Deprecated；
- Explicit Choice Preservation。

### 54.6 Privacy Tests

- Consent；
- Withdrawal；
- Platform Permission；
- Child Account；
- Family Policy；
- Server Enforcement。

### 54.7 Accessibility Tests

- 读屏；
- 大字体；
- 高对比；
- 无鼠标；
- 无声音；
- 单手；
- 减少运动；
- 慢速。

### 54.8 Recovery Tests

- 黑屏；
- 输入锁死；
- Profile 损坏；
- Save Failed；
- Override Leak；
- Safe Mode；
- Support Recovery。

---

## 55. Setting Contract Template

```markdown
# Setting Contract

## Definition

- ID:
- Category:
- Owner:
- Value Type:
- Allowed Values:

## Values

- Default:
- Recommended:
- Current:
- Effective:
- Reset:

## Scope

- Device:
- Platform:
- Account:
- Save:
- Character:
- Mode:
- Session:

## Apply

- Policy:
- Preview:
- Restart:
- Failure:
- Last Known Good:

## Persistence

- Local:
- Cloud:
- Merge:
- Conflict:
- Version:

## Constraints

- Capability:
- Dependency:
- Conflict:
- Policy:
- Override:

## Safety and Ethics

- Accessibility:
- Privacy:
- Child Safety:
- Authorization:

## Validation

- Success:
- Failure:
- Metrics:
```

---

## 56. Settings Profile Template

```markdown
# Settings Profile

## Metadata

- Profile ID:
- Name:
- Scope:
- Device Class:
- Version:

## Values

| Setting ID | Value | Source | Modified At |
|---|---|---|---|

## Compatibility

- Required Capabilities:
- Unsupported Values:
- Fallbacks:

## Sync

- Cloud:
- Revision:
- Conflict:

## Import / Export

- Exportable:
- Sensitive Fields:
- Signature:

## Recovery

- Last Known Good:
- Reset:
- Migration:
```

---

## 57. Settings Debt

Settings Debt 包括：

- 重复设置；
- 不明确 Scope；
- 旧设置未删除；
- 无 Owner；
- 默认值失控；
- 临时 Override 长期残留；
- Profile 格式分裂；
- 辅助入口重复；
- 隐私 UI 与服务端不一致；
- 设备设置错误云同步；
- 硬编码设置读取；
- 无迁移逻辑。

### 57.1 Signals

- 玩家同一选项要改多次；
- Support 经常要求删除配置文件；
- 新设备登录后体验异常；
- 设置页面层级不断加深；
- 一项设置改变多个隐藏行为；
- 每次版本更新重置偏好；
- 竞技模式离开后设置未恢复。

### 57.2 Reduction

- 统一 Setting Definition；
- Scope 审计；
- Override Stack；
- Profile 合并；
- 旧设置退役；
- 默认值治理；
- 迁移测试；
- 设备能力抽象；
- 服务端执行隐私和社交设置；
- 定期 Settings Health Review。

---

## 58. Rollout and Migration

### 58.1 Rollout

设置变更应按：

```text
Unit Test
→ Device Matrix
→ Accessibility Test
→ Internal
→ Small Cohort
→ Platform Cohort
→ Broad Release
→ Full Release
```

### 58.2 High-Risk Changes

包括：

- 输入；
- 分辨率；
- HDR；
- 隐私；
- 儿童保护；
- 通知；
- 社交可见性；
- 默认值；
- Scope；
- Profile 格式；
- 设置迁移。

### 58.3 Default Rollout

新默认只应应用于：

- 新用户；
- 未明确选择的字段；
- 明确同意重置的用户。

### 58.4 Migration

必须定义：

- Old Setting ID；
- New Setting ID；
- Value Mapping；
- Scope Mapping；
- Default Interpretation；
- Explicit Choice；
- Unsupported Fallback；
- LKG；
- Rollback。

### 58.5 Rollback

回滚时：

- 保留玩家明确选择；
- 恢复旧 Setting Definition；
- 清理新 Override；
- 不翻转隐私和辅助；
- 保留 Profile；
- 必要时进入 Safe Mode。

### 58.6 Stop Conditions

出现以下情况应停止发布：

- 输入锁死；
- 黑屏或启动失败；
- 隐私设置未生效；
- 儿童限制失效；
- 通知误发显著增加；
- 云同步大规模覆盖；
- 迁移翻转明确偏好；
- Override 无法恢复；
- 设置 Apply 失败率升高；
- 辅助设置不可达。

---

## 59. Risks and Open Questions

| Item | Type | Impact | Probability | Mitigation | Owner |
|---|---|---:|---:|---|---|
| Scope 定义错误 | Architecture Risk | 高 | 高 | Scope Contract | Engineering |
| 输入设置导致锁死 | Usability Risk | 严重 | 中 | Essential Action Recovery | UX / Engineering |
| 显示设置导致黑屏 | Technical Risk | 严重 | 中 | Preview + LKG | Rendering |
| 云同步覆盖设备设置 | Data Risk | 高 | 中 | Device Scope 排除 | Engineering |
| 隐私 UI 与服务端不一致 | Compliance Risk | 严重 | 低 | Server Confirmation | Privacy |
| 实验覆盖明确选择 | Ethical Risk | 高 | 中 | Player Choice Priority | Product |
| 儿童限制被绕过 | Safety Risk | 严重 | 低 | Server Enforcement | Security |
| 辅助设置迁移失败 | Accessibility Risk | 严重 | 中 | Explicit Choice Preservation | Accessibility |
| Override Stack 泄漏 | State Risk | 高 | 中 | 自动清理与审计 | Engineering |
| 设置数量持续膨胀 | Maintenance Risk | 高 | 高 | Settings Health Review | Product |

---

## 60. Review Checklist

### Definition and Scope

- [ ] 每项设置有唯一 Setting ID；
- [ ] Category、Value Type 和 Allowed Values 明确；
- [ ] Device、Account、Save、Mode、Session Scope 区分；
- [ ] Default、Recommended、Current、Effective 和 Reset Value 区分；
- [ ] Non-Goals 已定义。

### Defaults and Safety

- [ ] 默认值安全；
- [ ] 隐私、通知和社交默认谨慎；
- [ ] 辅助设置早期可达；
- [ ] 玩家明确选择优先；
- [ ] 平台和年龄限制可解释。

### Apply and Validation

- [ ] Apply Policy 明确；
- [ ] 高风险设置支持 Preview 和自动恢复；
- [ ] Capability、Dependency、Conflict 和 Policy Validation 完整；
- [ ] Apply、Save、Sync 状态区分；
- [ ] Last Known Good 可恢复。

### Input, Display and Audio

- [ ] Action-Based Rebinding；
- [ ] Essential Action 防锁死；
- [ ] 显示设置有 Safe Preview；
- [ ] 音频输出切换可测试；
- [ ] 设备热插拔有恢复策略。

### Accessibility

- [ ] Visual、Hearing、Motor、Cognitive 和 Motion 选项完整；
- [ ] Assist Preset 不作为身份标签；
- [ ] 所有教学和正式内容兼容辅助；
- [ ] 设置 UI 自身可访问；
- [ ] 重置辅助有额外保护。

### Privacy, Social and Notification

- [ ] Permission 与 Preference 分离；
- [ ] 服务端真实执行隐私和社交设置；
- [ ] Marketing 与 Service Notification 分离；
- [ ] Quiet Hours 和时区明确；
- [ ] 儿童和家长控制不可绕过。

### Profiles, Sync and Conflict

- [ ] Profile 应用前可预览；
- [ ] Device Scope 不跨设备覆盖；
- [ ] 字段级 Merge 规则明确；
- [ ] 冲突保留双方或可解释解决；
- [ ] Import / Export 安全。

### Migration and Recovery

- [ ] Default Change 不覆盖明确选择；
- [ ] Rename、Split、Merge、Scope Change 有迁移；
- [ ] Deprecated 设置有替代；
- [ ] Safe Mode 可进入；
- [ ] Support Recovery 最小权限且可审计。

### Validation and Operations

- [ ] Findability、Apply、Sync、Migration、Privacy 和 Accessibility 有指标；
- [ ] Device Matrix 完整；
- [ ] 黑屏、输入锁死和 Profile 损坏测试完成；
- [ ] Settings Debt 可监控；
- [ ] Rollback 和 Stop Conditions 明确。

---

## 61. V1 Completion Criteria

Settings and Preferences 可以被视为 V1，当：

- Display、Graphics、Audio、Language、Input、Accessibility、Gameplay、Social、Privacy、Notification、Network 和 Account 设置分类完整；
- 每项设置有统一 Setting Definition；
- Device、Platform、Account、Save Slot、Character、Mode、Session 和 Temporary Override Scope 已定义；
- Scope Invariants 和 Effective Value Resolution 顺序明确；
- Default、Recommended、Current、Effective 和 Reset Value 得到区分；
- 安全默认、平台默认、地区和年龄默认有治理方式；
- Immediate、Preview、Next Context、Restart、Re-login 和 Server Confirmation Apply Policy 完整；
- Setting Lifecycle、State Invariants 和 Validation 规则明确；
- Dependency、Conflict、Conditional Visibility 和 Cascade Change 可解释；
- Settings Profile、Preset、Search、Problem-Based Entry 和 Navigation 已定义；
- Input、Display、Graphics、Audio、Language、Accessibility、Difficulty、Notification、Social、Privacy、Account 和 Network 设置有专项规则；
- Device Capability 变化保留玩家意图；
- Temporary Override 和 Experiment Override 有优先级、生命周期和恢复规则；
- Local、Cloud、Per-Field Sync 和 Conflict Resolution 已定义；
- Settings Migration 能区分旧默认与明确选择；
- Reset、Last Known Good、Safe Mode、Import 和 Export 规则完整；
- 设置 UI 本身支持读屏、大字体、多输入和认知可访问性；
- 隐私、社交、儿童、家长控制和高风险设置由服务端或权威系统真实执行；
- Support 和 Diagnostics 在脱敏、最小权限和审计下可用；
- Settings Debt 有识别和治理方式；
- Findability、Apply Success、Sync、Migration、Privacy Enforcement 和 Accessibility 有验证计划；
- 高风险设置变更具有设备矩阵、灰度、迁移、回滚和停止条件；
- 下游 Notification、Social、Input、Difficulty、Save、Experiment 和 Support 可以直接引用本文件。

---

## 62. Related Documents

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
- [Input and Interaction](../core/input-and-interaction.md)
- [Game State and Flow](../core/game-state-and-flow.md)
- [Difficulty and Challenge](../progression/difficulty-and-challenge.md)
- [Tutorial and Onboarding](./tutorial-and-onboarding.md)
- [Save and Persistence](./save-and-persistence.md)
- `notification-and-reminders.md`
- `../social/social-and-multiplayer.md`
- `../social/moderation-and-safety.md`
- `../operations/experiment-management.md`
