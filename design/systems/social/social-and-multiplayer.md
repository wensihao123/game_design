# Social and Multiplayer（社交与多人系统）

> Status: V1  
> Category: Social  
> Path: `design/systems/social/social-and-multiplayer.md`  
> Owner: TBD  
> Reviewers: Product / Design / Engineering / UX / QA / Security / Privacy / Trust and Safety / Data / Live Operations  
> Last Updated: 2026-07-11  
> Version: 1.0  
> Risk Level: Critical  
> Dependencies: Game State and Flow, Input and Interaction, Save and Persistence, Settings and Preferences, Notification and Reminders, Characters and Loadouts, Content and Unlocks, Account and Identity  
> Affected Systems: Matchmaking and Competition, Moderation and Safety, Objectives and Quests, Reward System, Difficulty and Challenge, Entitlement and Ownership, Analytics and Telemetry, Live Operations

---

## 1. System Summary

Social and Multiplayer 系统负责定义：

```text
玩家如何被识别；
玩家如何建立和管理关系；
如何创建、加入和离开多人会话；
队伍、房间、Lobby、Party 和 Match 之间如何区分；
多人状态由谁权威维护；
网络异常、断线、重连和主机迁移如何处理；
跨平台、跨地区和跨版本如何协作；
社交互动如何保护隐私、安全和自主性；
多人结果如何在所有参与者之间保持一致。
```

该系统通常覆盖：

- Player Identity；
- Display Name；
- Presence；
- Friends；
- Followers；
- Block；
- Party；
- Lobby；
- Room；
- Session；
- Host；
- Invite；
- Join Request；
- Ready State；
- Team；
- Spectator；
- Voice and Text；
- Cross-Platform；
- Cross-Region；
- Reconnect；
- Host Migration；
- Session Authority；
- Shared Progress；
- Social Privacy；
- Reporting Entry Points。

健康的社交与多人系统应让玩家感受到：

```text
我能决定和谁互动；
加入和退出都清楚；
多人状态不会因单一设备异常而混乱；
断线后有恢复机会；
跨平台规则透明；
社交不会强迫、骚扰或泄露隐私；
团队合作和个人贡献都被合理承认。
```

---

## 2. Purpose

### 2.1 Player Value

该系统帮助玩家：

- 找到并加入熟悉的人；
- 组织队伍；
- 与陌生人协作；
- 在多人会话中保持稳定连接；
- 了解自己和队友的状态；
- 管理好友、邀请、屏蔽和隐私；
- 在断线后恢复；
- 跨设备和跨平台继续社交关系；
- 明确知道谁能看到自己；
- 在不想社交时保持单人体验完整。

### 2.2 Experience Contribution

社交与多人可以支持：

- 协作；
- 竞争；
-归属；
-身份；
-分享；
-长期关系；
-共同目标；
-重玩价值；
-内容发现；
-社区文化。

但设计不健康时会造成：

- 强迫加好友；
- 垃圾邀请；
- 队伍权力滥用；
- 社交压力；
- 隐私泄露；
- 断线惩罚；
- 跨平台不公平；
- 主机优势；
- 队伍状态不同步；
- 贡献误判；
- 陌生人骚扰；
- 社交功能成为核心成长门槛。

### 2.3 Product Value

统一社交与多人系统可以：

- 统一关系模型；
- 统一邀请和加入流程；
- 统一多人会话状态；
- 支持跨平台；
- 支持断线重连；
- 支持 Matchmaking；
- 支持 Moderation；
- 支持共享任务和奖励；
- 支持 Live Operations；
- 降低各玩法重复实现；
- 建立安全与隐私边界；
- 提供可审计的多人结果。

### 2.4 Why This System Exists

如果每个玩法自行实现多人逻辑，常见问题包括：

```text
好友、Party、Lobby、Match 使用不同身份；
一名玩家退出后其他客户端状态不同；
队长权限在不同内容中语义不一致；
邀请已过期但仍可接受；
同一玩家在多设备重复出现；
断线后重复奖励；
主机迁移导致世界状态分叉；
跨平台玩家无法互相屏蔽；
队伍和任务贡献计算规则冲突；
社交隐私只在 UI 隐藏，服务端仍公开。
```

统一系统用于确保：

- 身份唯一；
- 状态权威；
- 生命周期一致；
- 权限可解释；
- 断线可恢复；
- 结果可审计；
- 隐私真实生效。

---

## 3. Non-Goals

该系统不负责：

- 代替 Matchmaking 算法；
- 代替 Moderation and Safety；
- 直接计算所有玩法结果；
- 直接发放奖励；
- 直接修改资源余额；
- 处理支付；
- 强迫所有内容多人化；
- 让社交关系成为核心进度唯一门槛；
- 自动保证所有陌生人互动安全；
- 用社交压力提高留存；
- 将在线状态默认公开给所有人；
- 让客户端拥有高价值多人权威；
- 用 Peer-to-Peer 简化为无安全边界的信任模型。

---

## 4. Governing Principles

### 4.1 Player First Design

参考：

- `../../philosophy/foundation/player-first-design.md`

应用原则：

- 加入和退出都清楚；
- 单人玩家不失去核心价值；
- 邀请和社交可拒绝；
- 断线和系统故障不应被当作恶意行为；
- 多人不应要求不必要公开信息。

### 4.2 Choice and Consequence

参考：

- `../../philosophy/experience/choice-and-consequence.md`

应用原则：

- 队伍角色和协作选择产生真实差异；
- 高影响操作需要预览；
- 队长权限受边界约束；
- 玩家退出、踢出和转让后果清楚。

### 4.3 Challenge and Fairness

参考：

- `../../philosophy/experience/challenge-and-fairness.md`

应用原则：

- 多人规则统一；
- 网络、平台和付费差异不能造成不透明优势；
- 结果由权威状态决定；
- 贡献评价避免只看最后一击或表面输出。

### 4.4 Consistency and Coherence

参考：

- `../../philosophy/long-term/consistency-and-coherence.md`

应用原则：

- Friends、Party、Lobby、Room、Match 术语稳定；
- 同一权限在不同玩法语义一致；
- 邀请、加入、退出和断线规则统一；
- 跨平台关系和屏蔽状态一致。

### 4.5 Accessibility and Inclusivity

参考：

- `../../philosophy/responsibility/accessibility-and-inclusivity.md`

应用原则：

- 不强制语音；
- 支持文本、Ping、预设短语和非语言协作；
- 多人准备时间合理；
- 社交状态不只靠颜色；
- 读屏和替代输入可操作社交流程。

### 4.6 Ethical Design

参考：

- `../../philosophy/responsibility/ethical-design.md`

应用原则：

- 不用队友失望或公开比较强迫参与；
- 不默认公开隐私；
- 不用社交奖励迫使陌生人互动；
- 儿童和脆弱用户有更严格保护；
- 屏蔽、举报和退出真实有效。

---

## 5. Player Experience

### 5.1 Player Goal

玩家使用社交与多人系统通常为了：

- 添加好友；
- 查看在线状态；
- 创建队伍；
- 邀请；
- 接受或拒绝；
- 加入房间；
- 准备；
- 开始内容；
- 与队友沟通；
- 断线重连；
- 离开；
- 屏蔽；
- 举报；
- 继续一起玩。

### 5.2 Entry

入口包括：

- 好友列表；
- 最近玩家；
- Party；
- Matchmaking；
- 角色页面；
- 内容入口；
- 通知；
- 邀请；
- 深链；
- 公会；
- 活动；
- 排行；
- 回归页面；
- Platform Overlay。

### 5.3 Main Actions

玩家可以：

- Search；
- Add Friend；
- Follow；
- Accept；
- Decline；
- Invite；
- Join；
- Request to Join；
- Create Party；
- Leave Party；
- Transfer Leadership；
- Kick；
- Ready；
- Unready；
- Start；
- Spectate；
- Mute；
- Block；
- Report；
- Reconnect。

### 5.4 Core Decisions

关键决策包括：

- 是否公开在线状态；
- 是否接受邀请；
- 是否加入陌生人；
- 是否允许跨平台；
- 是否开启语音；
- 是否成为队长；
- 是否开始匹配；
- 是否踢出成员；
- 是否继续等待断线成员；
- 是否离开当前多人内容。

### 5.5 Success

健康体验意味着：

- 玩家清楚当前社交和会话状态；
- 邀请不会重复骚扰；
- 队伍成员资格一致；
- 准备和开始规则清楚；
- 断线有恢复；
- 退出后状态正确；
- 队友看见的信息受隐私控制；
- 语音不是唯一协作方式；
- 多人结果不会因客户端差异分裂。

### 5.6 Failure

失败包括：

- 邀请失效；
- 重复加入；
- Party 状态分叉；
- 主机离开；
- 断线；
- 版本不一致；
- 内容资格不一致；
- 角色冲突；
- 队伍无法开始；
- 语音失效；
- 跨平台关系不同步；
- 屏蔽后仍匹配；
- 会话结束状态不一致。

---

## 6. System Boundary

### 6.1 Inputs

系统接收：

- Account Identity；
- Player Profile；
- Presence；
- Friend and Block State；
- Platform Identity；
- Party Request；
- Invite；
- Join Request；
- Content Eligibility；
- Character and Loadout State；
- Matchmaking Result；
- Session State；
- Network State；
- Region；
- Version；
- Privacy and Age Policy；
- Moderation State；
- Device State；
- Entitlement State。

### 6.2 Outputs

系统产生：

- Social Graph State；
- Presence State；
- Party；
- Lobby；
- Room；
- Session Membership；
- Invite State；
- Join State；
- Leadership State；
- Ready State；
- Team Assignment；
- Voice / Text Permission；
- Reconnect Token；
- Session Result Reference；
- Social Event；
- Player-Facing Error and Recovery。

### 6.3 Owned State

系统拥有：

- Social Identity Mapping；
- Friend Relationship；
- Follow Relationship；
- Block Relationship Reference；
- Presence；
- Party；
- Lobby；
- Room；
- Session Membership；
- Invite；
- Join Request；
- Leadership；
- Ready State；
- Team Membership；
- Reconnect State；
- Session History；
- Social History；
- Multiplayer Version。

### 6.4 Read-Only Dependencies

系统读取：

- Account；
- Settings；
- Privacy；
- Moderation；
- Content；
- Characters；
- Difficulty；
- Matchmaking；
- Entitlement；
- Save；
- Time；
- Platform；
- Network。

### 6.5 Write Dependencies

系统通过正式契约请求：

- Matchmaking 创建或加入匹配；
- Session Service 创建会话；
- Notification 发送邀请；
- Moderation 应用 Block、Mute、Restriction；
- Save 保存社交和多人状态；
- Reward 处理共享结果；
- Objectives 处理共享进度；
- Analytics 记录非敏感事件。

### 6.6 Out of Scope

系统不直接：

- 计算排名；
- 计算完整战斗结果；
- 发放奖励；
- 判定处罚；
- 处理支付；
- 修改角色成长；
- 绕过 Moderation Restriction；
- 将客户端报告直接视为最终结果。

---

## 7. Core Entities and Concepts

| Entity / Concept | Definition | Owner | Lifetime | Notes |
|---|---|---|---|---|
| Social Identity | 社交层使用的稳定身份 | Account / Social | 长期 | 不等于显示名 |
| Display Name | 玩家可见名称 | Account / Social | 长期可变 | 有审核和唯一性策略 |
| Presence | 在线和活动状态 | Social | 动态 | 受隐私控制 |
| Relationship | Friend、Follow、Block 等关系 | Social / Moderation | 长期 | 有方向性 |
| Party | 跨内容的玩家临时组织 | Social | 会话或跨会话 | 通常有 Leader |
| Lobby | 进入具体内容前的准备空间 | Multiplayer | 短期 | 配置和 Ready |
| Room | 具有成员和规则的多人容器 | Multiplayer | 内容期 | 可公开或私密 |
| Session | 权威多人玩法实例 | Multiplayer / Session Service | 至结束 | 有唯一 Session ID |
| Invite | 主动邀请加入某对象 | Social | 短期 | 有 Expiry |
| Join Request | 玩家请求加入 | Social | 短期 | 需批准或自动 |
| Membership | 玩家在 Party、Lobby、Room、Session 中的成员关系 | Social | 对象期 | 唯一状态 |
| Leadership | 对管理动作的授权 | Social | 对象期 | 不等于无限权限 |
| Ready State | 玩家是否确认准备 | Multiplayer | Lobby 期 | 状态变化可审计 |
| Team Assignment | 玩家所属队伍 | Multiplayer | Session 期 | 与 Party 不同 |
| Reconnect Token | 恢复原 Session Membership 的安全凭据 | Multiplayer | 短期 | 不直接授权业务结果 |
| Session Authority | 会话权威节点或服务 | Multiplayer | Session 期 | 服务器优先 |
| Social History | 邀请、关系和成员变更历史 | Social | 审计期 | 隐私受控 |

---

## 8. Identity Model

### 8.1 Account Identity

用于认证和所有权。

### 8.2 Social Identity

用于好友、Party、邀请和多人。

### 8.3 Platform Identity

来自平台生态。

### 8.4 Display Identity

玩家可见：

- Display Name；
- Avatar；
- Banner；
- Status；
- Platform Badge；
- Pronoun 或自我描述（如支持）。

### 8.5 Session Identity

会话内唯一标识。

### 8.6 Identity Mapping

必须支持：

```text
Account Identity
↔ Social Identity
↔ Platform Identity
↔ Session Identity
```

映射不能由客户端自报。

---

## 9. Display Name

### 9.1 Requirements

定义：

- 长度；
- 字符；
- 重复；
- 大小写；
- 本地化；
- 敏感词；
- 冒充；
- 频率；
- 历史。

### 9.2 Unique vs Non-Unique

可使用：

- 全局唯一名称；
- 名称 + Discriminator；
- 非唯一 Display Name + 唯一账号标识。

### 9.3 Name Changes

应：

- 有频率限制；
- 保留审计；
- 防冒充；
- 更新缓存；
- 不破坏好友关系。

### 9.4 Privacy

不应默认要求真实姓名。

---

## 10. Presence

### 10.1 Presence States

- Offline；
- Online；
- Away；
- Busy；
- In Menu；
- In Party；
- In Matchmaking；
- In Session；
- Spectating；
- Do Not Disturb；
- Invisible。

### 10.2 Activity Detail

可以显示：

- 当前内容；
- 队伍人数；
- 是否可加入；
- 平台；
- 地区；
- 状态。

### 10.3 Privacy Levels

- Everyone；
- Friends；
- Party；
- Nobody；
- Custom。

### 10.4 Invisible

玩家可使用系统但对他人显示离线。

### 10.5 Presence Reliability

Presence 是近实时提示，不是权威业务状态。

---

## 11. Relationship Model

### 11.1 Friend

双向确认关系。

### 11.2 Follow

单向关系。

### 11.3 Favorite

本地或账户级快速访问。

### 11.4 Block

阻止交互和可见性。

### 11.5 Mute

隐藏通信但不一定断绝关系。

### 11.6 Recent Player

近期共同会话记录。

### 11.7 Avoid

用于降低再次匹配概率，但不等同 Block。

### 11.8 Relationship State

```text
None
→ Request Sent
→ Request Received
→ Friends
→ Removed
```

异常或保护状态：

```text
Blocked
Muted
Restricted
Pending Review
```

---

## 12. Friend Request Lifecycle

```text
Created
→ Delivered
→ Pending
→ Accepted
```

分支：

```text
Pending
→ Declined
Pending
→ Expired
Pending
→ Cancelled
Any
→ Blocked
```

### 12.1 Invariants

1. 同一玩家对同一目标不能无限重复请求。
2. Block 优先于 Pending Request。
3. 请求有 Expiry。
4. 接受前重新验证账户和隐私状态。
5. 已成为好友的重复请求应幂等。
6. 删除好友不自动解除 Block 或 Moderation Restriction。
7. Request 状态跨设备一致。

---

## 13. Party

Party 是：

```text
跨多个内容存在的临时玩家组织，
用于共同排队、准备、进入内容和继续一起玩。
```

### 13.1 Party Fields

- Party ID；
- Members；
- Leader；
- Privacy；
- Join Policy；
- Max Size；
- Region；
- Cross-Platform；
- Voice Channel；
- Current Activity；
- Ready Summary；
- Version。

### 13.2 Party Privacy

- Open；
- Friends Only；
- Invite Only；
- Closed。

### 13.3 Join Policy

- Auto Join；
- Request Approval；
- Leader Approval；
- Member Approval；
- Matchmaking Only。

### 13.4 Party Persistence

可在内容间保留，但不应无限存在。

---

## 14. Lobby

Lobby 是进入某一具体内容前的准备空间。

### 14.1 Lobby Responsibilities

- 成员展示；
- Team Assignment；
- Character / Loadout；
- Ready；
- Content；
- Difficulty；
- Rules；
- Map；
- Voice；
- Start；
- Backfill；
- Version Check。

### 14.2 Party vs Lobby

Party 可以跨内容存在。

Lobby 与一个即将开始的具体内容绑定。

### 14.3 Lobby Lifecycle

```text
Created
→ Filling
→ Configuring
→ Ready Check
→ Starting
→ Session Created
```

异常：

```text
Cancelled
Expired
Failed
```

---

## 15. Room

Room 是更通用的多人容器。

可用于：

- 私人房；
- 自定义规则；
- 社交空间；
- 训练；
-观战；
-社区活动。

### 15.1 Room Properties

- Public / Private；
- Password；
- Capacity；
- Owner；
- Moderators；
- Rules；
- Region；
- Version；
- Spectators；
- Discoverability；
- Join Policy。

### 15.2 Room Authority

Room Owner 可以管理成员和规则，但不能绕过：

- 安全政策；
- 平台限制；
- 付费权益；
- Moderation；
- 法律要求。

---

## 16. Session

Session 是权威多人玩法实例。

### 16.1 Session Fields

- Session ID；
- Content ID；
- Rule Version；
- Server / Host；
- Members；
- Teams；
- Start Time；
- State；
- Reconnect Policy；
- Result State；
- Migration State；
- Region；
- Build Version。

### 16.2 Session Lifecycle

```text
Creating
→ Joining
→ Synchronizing
→ Active
→ Ending
→ Finalizing
→ Completed
→ Archived
```

异常：

```text
Join Failed
Disconnected
Migrating
Aborted
Invalidated
```

### 16.3 Session State Authority

会话状态优先由：

- Dedicated Server；
- Authoritative Session Service；
- 经约束的 Host；

维护。

---

## 17. Party, Lobby, Room, Session Differences

| Object | Purpose | Lifetime | Leader / Owner | Main State |
|---|---|---|---|---|
| Party | 组队和共同移动 | 跨内容短期 | Leader | Members |
| Lobby | 战前准备 | 一次内容前 | Leader / Server | Ready / Config |
| Room | 自定义多人容器 | 按房间 | Owner / Moderator | Rules / Membership |
| Session | 权威玩法实例 | 一次多人内容 | Server / Host | Gameplay State |

这些对象不能混用一个模糊的：

```text
Multiplayer Group
```

否则权限和生命周期会失控。

---

## 18. Invite

### 18.1 Invite Types

- Friend Invite；
- Party Invite；
- Lobby Invite；
- Room Invite；
- Session Join Invite；
- Spectate Invite；
- Guild Invite。

### 18.2 Invite Fields

- Invite ID；
- Sender；
- Recipient；
- Target；
- Type；
- Created At；
- Expiry；
- Required State；
- Version；
- Dedupe Key；
- Privacy；
- Status。

### 18.3 Invite Lifecycle

```text
Created
→ Delivered
→ Pending
→ Accepted
```

分支：

```text
Declined
Expired
Cancelled
Invalidated
Blocked
```

### 18.4 Accept Validation

接受时重新检查：

- 目标仍存在；
- 空间；
- 内容资格；
- 版本；
- 平台；
-地区；
- Block；
- Moderation；
- Party 状态；
- Session 状态。

---

## 19. Join Requests

Join Request 用于玩家主动申请加入。

### 19.1 Join Request Policies

- Auto Accept；
- Leader Approval；
- Member Vote；
- Role-Based Approval；
- Friends Only。

### 19.2 Expiry

必须有过期时间。

### 19.3 Spam Protection

限制：

- 频率；
- 重复；
- 同一目标；
- 被拒后冷却；
- 被 Block 后停止。

### 19.4 Privacy

目标玩家可以关闭 Join Request。

---

## 20. Membership

### 20.1 Membership States

- Invited；
- Joining；
- Active；
- Ready；
- Disconnected；
- Reconnecting；
- Left；
- Kicked；
- Removed；
- Banned；
- Spectating。

### 20.2 Membership Invariants

1. 同一玩家在同一对象中只能有一个有效 Membership。
2. Kicked 与 Left 必须区分。
3. Disconnected 不应立即视为 Left，除非策略如此。
4. Block 和 Moderation Restriction 可阻止重新加入。
5. Session Membership 必须有权威时间和版本。
6. 退出一个 Session 不一定退出 Party。
7. 退出 Party 不一定自动中止已开始 Session，需明确规则。

---

## 21. Leadership and Roles

### 21.1 Party Leader

可以：

- 邀请；
- 开始匹配；
- 选择内容；
- 设定 Party Privacy；
- 转让领导；
- 移除成员。

### 21.2 Room Owner

可以管理：

- Room Rules；
- Moderators；
- Discoverability；
- Member Permissions。

### 21.3 Session Host

如果存在 Host，应只拥有技术或特定管理权限。

### 21.4 Leadership Boundaries

Leader 不应：

- 访问私人数据；
- 修改他人构筑；
- 绕过 Block；
- 免除 Moderation；
- 强制开启语音；
- 直接剥夺已确认个人奖励。

### 21.5 Transfer

转让需要：

- 目标在线；
- 目标有资格；
- 服务端确认；
- 所有人状态更新；
- 防重复。

---

## 22. Leader Disconnect

Leader 断线时可以：

- 等待重连；
- 自动转让；
- 由最早成员接任；
- 由最高资格成员接任；
- 解散；
- Session 独立继续。

### 22.1 Selection Rule

应稳定、可预测并由服务端决定。

### 22.2 Leader Return

原 Leader 重连后不应自动夺回权限，除非明确规则。

---

## 23. Ready Check

### 23.1 Purpose

确认：

- 玩家在场；
- 配置合法；
- 内容已加载；
- 网络可用；
- 规则理解；
- 版本一致。

### 23.2 Ready States

- Not Ready；
- Ready；
- Loading；
- Invalid；
- Away；
- Disconnected；
- Auto Ready（谨慎）。

### 23.3 State Reset

以下变化应清除 Ready：

- Loadout 改变；
- 内容改变；
- 难度改变；
- 队伍改变；
- 规则版本改变；
- 断线。

### 23.4 Timeout

Ready Check 超时应：

- 取消开始；
- 移除未响应成员；
- 返回配置；
- 或按模式策略处理。

---

## 24. Team Assignment

### 24.1 Assignment Types

- Manual；
- Automatic；
- Matchmaking；
- Random；
- Role-Based；
- Skill-Based；
- Party-Preserved。

### 24.2 Party Preservation

是否保持 Party 同队必须按模式定义。

### 24.3 Team Privacy

队伍信息只显示协作所需内容。

### 24.4 Team Changes

开始后换队通常受限，需要防止滥用。

---

## 25. Character and Loadout Validation

进入多人内容前检查：

- Character Ownership；
- Availability；
- Loadout Validity；
- Mode Restrictions；
- Duplicate Character；
- Team Composition；
- Role Requirements；
- Entitlement；
- Version；
- Competitive Rules。

### 25.1 Validation Timing

- 加入 Lobby；
- 改变构筑；
- Ready；
- Session Start；
- Reconnect。

### 25.2 Invalid Build

应：

- 说明原因；
- 提供修复；
- 保留 Party；
- 不静默替换高价值配置。

---

## 26. Communication

### 26.1 Channels

- Text Chat；
- Voice Chat；
- Ping；
- Emote；
- Quick Chat；
- Marker；
- Shared Objective；
- System Message。

### 26.2 Voice Is Optional

核心协作不能只依赖语音。

### 26.3 Text Alternatives

提供：

- 预设短语；
- Ping；
- 目标标记；
- 角色状态；
- Ready；
- 危险提示。

### 26.4 Communication Permissions

由：

- Settings；
- Privacy；
- Age；
- Moderation；
- Platform；

共同决定。

### 26.5 Translation

自动翻译如存在，应：

- 标记；
- 可关闭；
- 不作为处罚唯一依据；
- 保护隐私。

---

## 27. Text Chat

### 27.1 Chat Types

- Party；
- Team；
- Room；
- Match；
- Direct；
- Guild；
- System。

### 27.2 Chat History

定义：

- 保留期；
- 本地缓存；
- 服务端存储；
- 举报证据；
- 隐私；
- 删除。

### 27.3 Rate Limits

防止：

- Spam；
- Flood；
- Link Abuse；
- Mention Abuse。

### 27.4 Content Filtering

由 Moderation and Safety 定义和执行。

---

## 28. Voice Chat

### 28.1 Voice Modes

- Push-to-Talk；
- Open Mic；
- Voice Activation；
- Disabled。

### 28.2 Voice Scope

- Party；
- Team；
- Proximity；
- Room；
- Direct。

### 28.3 Safety

支持：

- Mute；
- Block；
- Report；
- Volume per Player；
- Voice Indicator；
- Parental Restriction。

### 28.4 Privacy

明确：

- 是否录音；
- 是否临时缓存；
- 是否用于 Moderation；
- 保留期；
- 同意。

### 28.5 No Forced Voice

不加入语音不应失去核心奖励。

---

## 29. Ping and Non-Verbal Communication

### 29.1 Ping Types

- Location；
- Target；
- Danger；
- Help；
- Defend；
- Attack；
- Ready；
- Retreat；
- Resource；
- Objective。

### 29.2 Rate Limit

防止 Ping Spam。

### 29.3 Accessibility

Ping 需要：

- 视觉；
- 音频；
- 文本；
- 方向；
- 可屏蔽。

### 29.4 Context Sensitivity

同一 Ping 根据目标和环境调整意义。

---

## 30. Cross-Platform

### 30.1 Cross-Play

玩家可以跨平台共同进入 Session。

### 30.2 Cross-Save

存档跨平台。

### 30.3 Cross-Progression

成长和所有权跨平台。

三者必须区分。

### 30.4 Cross-Platform Rules

定义：

- 身份；
- 好友；
- Block；
- Voice；
- Text；
- Matchmaking；
- Input Pool；
- Entitlement；
- 内容；
- 版本；
- 平台限制。

### 30.5 Platform Opt-Out

若平台允许，玩家可关闭 Cross-Play。

---

## 31. Cross-Region

跨地区需要考虑：

- 延迟；
- 数据驻留；
- 法律；
- 语言；
- 商店；
- 内容；
- 时间；
- Moderation；
- 语音；
- 匹配池。

### 31.1 Region Choice

玩家手动选择时应说明影响。

### 31.2 Party Region

Party 可以：

- 由 Leader 决定；
- 自动选择最优；
- 按成员延迟平衡；
- 使用指定地区。

### 31.3 Legal Restrictions

不能因社交邀请绕过地区限制。

---

## 32. Cross-Version

### 32.1 Version Compatibility

定义：

- Same Version Only；
- Compatible Range；
- Content Pack Compatibility；
- Server Translates；
- Read-Only Presence。

### 32.2 Party Version Mismatch

应说明：

- 谁需要更新；
- 是否可以继续 Party；
- 是否可以进入其他内容；
- 是否保留邀请。

### 32.3 Session Rule Version

所有成员必须使用同一权威规则版本。

---

## 33. Network Authority

### 33.1 Dedicated Server

推荐用于：

- 竞技；
- 高价值结果；
- 大型多人；
- 高安全要求。

### 33.2 Listen Server

某玩家兼任主机。

风险：

- Host Advantage；
- 离开；
- 篡改；
- 网络差异；
- 迁移。

### 33.3 Peer-to-Peer

适合低风险、小规模和受控场景。

### 33.4 Authority Policy

必须明确：

- 输入谁验证；
- 状态谁拥有；
- 结果谁确认；
- 随机谁生成；
- 时间谁维护；
- 作弊如何检测。

---

## 34. State Synchronization

### 34.1 Authoritative State

服务端或权威 Host 维护。

### 34.2 Client Prediction

客户端可预测表现，但不能决定最终业务结果。

### 34.3 Reconciliation

权威状态返回后修正客户端。

### 34.4 Snapshot and Delta

可以使用：

- Snapshot；
- State Delta；
- Event Stream；
- Command Ack。

### 34.5 Sequence

需要：

- Tick；
- Revision；
- Sequence；
- Timestamp；
- Correlation。

---

## 35. Latency and Lag

### 35.1 Latency Compensation

可使用：

- Client Prediction；
- Interpolation；
- Rewind；
- Input Buffer；
- Hit Validation；
- Region Selection。

### 35.2 Fairness

不同延迟下的处理要：

- 可解释；
- 不给主机隐藏优势；
- 不因设备平台直接歧视；
- 竞技规则公开。

### 35.3 Lag Indicator

玩家应知道：

- 自己连接状态；
- 是否影响输入；
- 是否可能断线；
- 可采取什么行动。

---

## 36. Disconnect

### 36.1 Disconnect Causes

- 网络中断；
- 应用崩溃；
- 平台挂起；
- 服务器异常；
- 主机离开；
- 账户切换；
- 版本更新；
- Kick；
- Ban。

### 36.2 Disconnect Classification

- Temporary；
- Recoverable；
- Permanent；
- Player-Initiated；
- Server-Initiated；
- Moderation-Initiated。

### 36.3 Grace Window

可在短时间内保留 Membership。

### 36.4 Teammate Behavior

断线玩家可以：

- 暂停角色；
- AI 接管；
- 保持原地；
- 移除；
- 保留槽位。

按模式定义。

---

## 37. Reconnect

### 37.1 Reconnect Flow

```text
Authenticate
→ Validate Session
→ Validate Membership
→ Validate Version
→ Validate Reconnect Token
→ Restore Snapshot
→ Replay Deltas
→ Resume
```

### 37.2 Reconnect Token

应：

- 短期；
- 绑定账户；
- 绑定 Session；
- 防重放；
- 可撤销。

### 37.3 Reconnect State

- Eligible；
- Attempting；
- Synchronizing；
- Resumed；
- Expired；
- Rejected。

### 37.4 Reconnect Result

成功后恢复：

- Membership；
- Team；
- Role；
- Session State；
- Personal State；
- Communication；
- Pending Result。

### 37.5 No Duplicate Membership

重连不能创建第二个玩家实例。

---

## 38. Host Migration

如果使用 Host：

### 38.1 Trigger

- Host 断线；
- Host 主动退出；
- 主机性能不足；
- 迁移命令；
- 平台事件。

### 38.2 Candidate Selection

考虑：

- 网络；
- 性能；
- NAT；
- 平台；
- 稳定性；
- 地区；
- 版本。

### 38.3 Migration State

```text
Detected
→ Freezing
→ Selecting
→ Transferring
→ Reconnecting
→ Resumed
```

### 38.4 Safety

迁移期间：

- 不提交高价值结果；
- 保留事务 ID；
- 防双主；
- 使用唯一 Epoch；
- 超时后安全终止。

### 38.5 Failure

迁移失败时：

- 结束 Session；
- 保留可确认结果；
- 不重复奖励；
- 说明原因；
- 按规则补偿。

---

## 39. Backfill

Backfill 是 Session 开始后补充新玩家。

### 39.1 Suitable Cases

- 非高风险；
- 可公平加入；
- 内容支持动态加入；
- 不影响关键叙事；
- 奖励可以按贡献处理。

### 39.2 Unsuitable Cases

- 竞技决胜阶段；
- 不可逆选择；
- 高难连续挑战；
- 角色唯一性无法满足；
- 结果即将结束。

### 39.3 Backfill Rewards

必须公平处理：

- 参与时长；
- 贡献；
- 完成；
- 首次奖励；
- 退出玩家。

---

## 40. Spectator

### 40.1 Spectator Types

- Friend Spectator；
- Public Spectator；
- Tournament Spectator；
- Coach；
- Replay Viewer。

### 40.2 Spectator Permissions

不能：

- 查看隐藏对手信息；
- 影响玩法；
- 绕过 Block；
- 向选手发送未允许信息。

### 40.3 Delay

竞技观战可能需要延迟。

### 40.4 Privacy

玩家可以限制谁能观战。

---

## 41. Leaving

### 41.1 Leave Types

- Leave Party；
- Leave Lobby；
- Leave Room；
- Leave Session；
- Exit to Menu；
- Disconnect；
- Forfeit。

### 41.2 Consequences

应说明：

- 是否保留 Party；
- 是否失去奖励；
- 是否有冷却；
- 是否影响队友；
- 是否可重连；
- 是否算弃权；
- 是否影响排名。

### 41.3 Safe Exit

普通社交和合作内容不应无理由高压阻止退出。

### 41.4 Competitive Exit

由 Matchmaking and Competition 定义处罚。

---

## 42. Kick and Remove

### 42.1 Reasons

- Party 管理；
- AFK；
- 资格变化；
- 版本；
- 安全；
- Moderation；
- 房间规则。

### 42.2 Kick Authority

只有明确角色可执行。

### 42.3 Kick Protection

防止：

- 开始前最后一刻恶意踢人；
- 奖励前踢人；
- 重复骚扰；
- 队长滥权。

### 42.4 Player Feedback

说明：

- 被谁；
- 原因分类；
- 是否可重连；
- 是否保留进度；
- 是否可举报。

### 42.5 Moderation Kick

与普通 Party Kick 区分。

---

## 43. AFK and Inactivity

### 43.1 Detection

可基于：

- 输入；
- 状态变化；
- 活动参与；
- 响应；
- 网络。

### 43.2 Accessibility

不能只用高频输入判断活跃。

### 43.3 Warning

先：

- 提醒；
- 提供恢复；
- 允许说明；
- 再处理。

### 43.4 Consequence

按模式：

- AI 接管；
- 移除；
- 不计奖励；
- 降低优先级；
- 无处罚。

---

## 44. Shared Progress

### 44.1 Models

- Full Shared；
- Contribution Based；
- Individual within Group；
- Group Completion；
- Community Progress。

### 44.2 Progress Ownership

Objectives and Quests 拥有目标状态。

Social and Multiplayer 提供：

- Membership；
- Presence；
- Contribution Context；
- Session Result。

### 44.3 Contribution

应考虑：

- 支援；
- 防御；
- 控制；
- 治疗；
- 目标；
- 复活；
- 信息；
- 协调。

### 44.4 Avoid Last-Hit Bias

不应只按最后一击。

---

## 45. Shared Rewards

### 45.1 Reward Eligibility

可以基于：

- Membership；
- Participation；
- Completion；
- Contribution；
- Duration；
- Role；
- Backfill；
- Disconnect；
- Reconnect。

### 45.2 Personal Reward Instance

即使团队共同完成，奖励通常为每位玩家独立实例。

### 45.3 Atomicity

同一玩家同一 Session Result 只生成一次奖励资格。

### 45.4 Disconnect

断线玩家是否保留奖励需明确，并防止利用。

---

## 46. Session Results

### 46.1 Result Authority

由权威 Session Service 生成。

### 46.2 Result Fields

- Session ID；
- Rule Version；
- Participants；
- Teams；
- Outcome；
- Contribution；
- Completion；
- Disconnect；
- Integrity；
- Finalized At；
- Result Version。

### 46.3 Finalization

```text
Session Ends
→ Validate State
→ Finalize Result
→ Persist
→ Publish Result
→ Downstream Rewards / Objectives / Ranking
```

### 46.4 Unknown Result

超时后查询，不重复提交。

---

## 47. Persistence

需要保存：

- Friend；
- Follow；
- Block Reference；
- Party Metadata；
- Invite；
- Membership；
- Session History；
- Reconnect；
- Readiness；
- Social Privacy；
- Recent Players；
- Communication Preference；
- Result Reference。

### 47.1 Temporary vs Persistent

Party 和 Lobby 可能短期。

Friends 和 Block 长期。

Session Membership 需在断线窗口内持久化。

### 47.2 Multi-Device

同一账号多设备登录时需定义：

- 是否允许；
- 哪台拥有 Active Session；
- 其他设备是只读还是踢出；
- Presence 如何显示。

---

## 48. Privacy

### 48.1 Data Visible to Others

可能包括：

- Display Name；
- Avatar；
- Presence；
- Platform；
- Current Activity；
- Achievements；
- Profile；
- Friend List；
- Voice Status。

### 48.2 Visibility Controls

- Everyone；
- Friends；
- Party；
- Team；
- Nobody；
- Custom。

### 48.3 Hidden Data

不应公开：

- 真实姓名；
- 精确位置；
- Email；
- 账户标识；
- 支付状态；
- 年龄；
-设备唯一标识；
-未经授权的好友关系。

### 48.4 Social Graph Privacy

好友列表是否公开需明确选择。

### 48.5 Recent Players

保留时间和用途有限。

---

## 49. Block

### 49.1 Block Effects

通常阻止：

- Direct Message；
- Friend Request；
- Invite；
- Join Request；
- Presence Detail；
- Matchmaking Pairing（尽可能）；
- Spectate；
- Profile Interaction。

### 49.2 Cross-Platform Block

应尽量在产品层统一，而不是只依赖平台层。

### 49.3 Block Visibility

被 Block 的玩家通常不应被明确通知具体是谁 Block。

### 49.4 Block Persistence

跨设备保存。

### 49.5 Moderation Integration

Moderation Restriction 优先于普通关系。

---

## 50. Mute

### 50.1 Mute Types

- Text；
- Voice；
- Ping；
- Emote；
- Notifications。

### 50.2 Scope

- Current Session；
- Temporary；
- Permanent；
- Per Channel。

### 50.3 Mute Does Not Remove

Mute 不一定：

- 移除好友；
- 退出 Party；
- 阻止匹配；
- 解除 Block。

---

## 51. Child and Family Safety

### 51.1 Defaults

儿童账户默认：

- 更有限的陌生人互动；
- 更谨慎的公开 Profile；
- 更严格语音和文本；
- 更严格邀请；
- 更克制通知；
- 家长可控。

### 51.2 Parental Controls

可管理：

- Friends；
- Voice；
- Text；
- Cross-Play；
- User-Generated Content；
- Purchases；
- Time；
- Public Rooms；
- Streaming。

### 51.3 No Circumvention

设备切换、平台邀请或深链不能绕过家长限制。

---

## 52. Social Accessibility

### 52.1 Non-Voice Collaboration

必须支持：

- Ping；
- Quick Chat；
- Markers；
- Shared Objectives；
- Ready；
- Role Indicators。

### 52.2 Screen Reader

应读取：

- 玩家名称；
- 状态；
-邀请；
-队伍；
-Ready；
-静音；
-权限。

### 52.3 Color Independence

Team、Ready、Voice 和 Connection 不只靠颜色。

### 52.4 Timing

邀请、Ready 和加入操作有合理时间。

### 52.5 Motor

不要求复杂拖拽管理队伍。

### 52.6 Cognitive

状态和权限使用一致术语。

---

## 53. Ethical Review

### 53.1 Forced Social

核心成长不应要求：

- 添加陌生好友；
- 公开分享；
- 加入语音；
- 邀请通讯录；
- 加入公会。

### 53.2 Social Pressure

不应：

- 公开羞辱低贡献者；
- 用队友失望文案推动付费；
- 用连续组队奖励强迫长时在线；
- 让退出按钮难找。

### 53.3 Privacy

默认最小公开。

### 53.4 Commercial Influence

付费玩家不应拥有：

- 踢人特权；
- 匹配优先；
- 不受 Block；
- 更高社交可见性；
- 竞技权威。

### 53.5 Children and Vulnerable Users

不根据孤独、社交失败或排斥状态推销。

---

## 54. Security

### 54.1 Threats

- Identity Spoofing；
- Invite Forgery；
- Session Hijack；
- Reconnect Replay；
- Host Manipulation；
- Packet Tampering；
- Bot；
- Spam；
- Harassment；
- Cross-Account Access；
- Voice Abuse；
- Deep Link Abuse。

### 54.2 Controls

- Authentication；
- Authorization；
- Signed Session Token；
- Short-Lived Invite；
- Nonce；
- Idempotency；
- Rate Limit；
- Server Authority；
- Encryption；
- Audit；
- Device Revocation；
- Abuse Detection。

### 54.3 Least Privilege

Party Leader、Room Owner 和 Host 都只拥有必要权限。

---

## 55. Failure and Recovery

| Failure | Cause | Player Impact | Recovery | Data Guarantee |
|---|---|---|---|---|
| Invite Lost | 通知或事件丢失 | 无法加入 | Invite 列表查询、重发 | 单一 Invite ID |
| Duplicate Membership | 重试或并发 | 同一玩家出现两次 | 幂等 Join、唯一约束 | 单一有效成员 |
| Party Split-Brain | 多端状态不同 | 成员和 Leader 不一致 | 服务端权威重同步 | Revision 保留 |
| Leader Disconnect | 网络异常 | Party 无法继续 | 自动转让、等待或解散 | Membership 保留 |
| Session Join Failed | 容量、版本或网络 | 无法进入 | 重试、回 Lobby | Party 不丢 |
| Reconnect Failed | Token 或 Session 过期 | 无法恢复 | 安全退出、结果查询 | 不重复奖励 |
| Host Migration Failed | 无新 Host | Session 终止 | 安全 Finalize、补偿 | 已确认状态保留 |
| Voice Service Failure | 外部服务异常 | 无法语音 | Text / Ping Fallback | Session 继续 |
| Cross-Platform Identity Mismatch | 映射错误 | 好友或 Block 不一致 | 重新绑定和对账 | 关系历史保留 |
| Ready State Desync | 客户端不同步 | 无法开始 | 权威刷新 | 版本和原因保留 |
| Result Finalization Timeout | 服务异常 | 奖励 Pending | 查询、幂等恢复 | Session Result 唯一 |
| Block Enforcement Failure | 缓存或服务异常 | 骚扰风险 | 立即刷新、隔离 | Block 权威保留 |

---

## 56. Edge Cases

### Identity

- 同名玩家；
- 名称变更；
- 平台账号解绑；
- 多平台合并；
- Guest 转正式；
- 账户删除；
- 家庭共享。

### Party

- Leader 和成员同时离开；
- Party 满员时接受 Invite；
- 邀请接受与 Party 解散同时发生；
- Party 进入 Matchmaking 时成员加入；
- 成员版本不同；
- Cross-Play 设置不同。

### Session

- Session 开始时一人断线；
- 加载超时；
- 服务器重启；
- 主机迁移；
- 结果生成时断线；
- 规则版本改变；
- Content Retired。

### Communication

- 语音服务中断；
- 玩家被 Block 后仍在当前 Session；
- Chat 历史跨设备；
- 翻译错误；
- 儿童账户加入成人 Party；
- Platform Overlay 邀请。

### Rewards

- Backfill；
- AI 接管；
- 中途离开；
- 重连；
- 同一账号多设备；
- Session 被判无效；
- 结果回滚。

---

## 57. Cross-System Dependencies

| System | Dependency Type | Direction | Data or Event | Failure Impact |
|---|---|---|---|---|
| Account and Identity | Critical | 双向 | Identity / Auth | 越权或串号 |
| Settings and Preferences | Hard | Settings → Social | Privacy / Voice / Cross-Play | 设置失效 |
| Notification and Reminders | Soft / Hard | 双向 | Invite / Social Alerts | 加入延迟 |
| Save and Persistence | Hard | 双向 | Relationships / History | 状态丢失 |
| Characters and Loadouts | Hard / Soft | Characters → Social | Build / Team Eligibility | 无法开始 |
| Content and Unlocks | Hard | Content → Social | Availability / Entry | Session 失效 |
| Difficulty and Challenge | Soft / Hard | Difficulty → Social | Party Scaling / Eligibility | 挑战失配 |
| Matchmaking and Competition | Hard | 双向 | Queue / Match / Rating | 无法匹配 |
| Moderation and Safety | Critical | 双向 | Block / Restriction / Report | 安全风险 |
| Reward System | Hard / Soft | Social → Reward | Session Result | 奖励延迟 |
| Objectives and Quests | Soft / Hard | Social → Objectives | Shared Progress | 任务错误 |
| Entitlement and Ownership | Hard / Soft | Entitlement → Social | Access / Cross-Platform | 付费内容风险 |
| Analytics and Telemetry | Soft | Social → Analytics | Social Events | 不阻断 |
| Live Operations | Soft / Hard | Live → Social | Events / Rooms | 活动异常 |

---

## 58. Data and Persistence

| State | Persistent | Authority | Save Trigger | Retention | Recovery |
|---|---|---|---|---|---|
| Social Identity Mapping | 是 | Account / Social | 绑定变化 | 长期 | Account 对账 |
| Friend Relationship | 是 | Social | 关系变化 | 长期 | 关系历史 |
| Follow Relationship | 是 | Social | 关系变化 | 长期 | 历史恢复 |
| Block Reference | 是 | Moderation / Social | Block 变化 | 长期 | 权威查询 |
| Presence | 否或短期 | Social | 状态变化 | 短期 | 重算 |
| Party | 是或短期 | Social | 成员变化 | Party 期 | 权威重建 |
| Lobby | 短期 | Multiplayer | 状态变化 | Lobby 期 | 回 Party |
| Session Membership | 是 | Multiplayer | 加入、断线、离开 | Session 及审计期 | 重连 |
| Invite | 是 | Social | 创建和状态变化 | 至过期后审计期 | 列表恢复 |
| Ready State | 短期 | Multiplayer | Ready 变化 | Lobby 期 | 重置 |
| Session Result Reference | 是 | Session Service | Finalize | 长期或审计期 | 结果查询 |
| Social History | 是 | Social | 关键变化 | 审计期 | Support |

---

## 59. Analytics and Validation

### 59.1 Key Assumptions

1. 玩家能理解 Friend、Party、Lobby、Room 和 Session 的区别。
2. 邀请、加入、拒绝和退出流程清楚。
3. Party 和 Session 状态在多设备间保持一致。
4. 断线和重连能够恢复 Membership，而不重复实例。
5. 非语音协作足以支持核心多人体验。
6. Cross-Platform、Cross-Region 和 Cross-Version 规则可理解。
7. Block、Mute 和 Privacy 能真实生效。
8. Shared Progress 和 Reward 能公平处理贡献和断线。
9. Leader 和 Host 权限不会被滥用。
10. 多人失败不会导致高价值状态分叉或重复。

### 59.2 Validation Plan

| Hypothesis | Evidence | Success | Failure | Method |
|---|---|---|---|---|
| 对象区别清楚 | 用户复述 | 能说明 Party / Lobby / Session | 状态混淆 | Usability Test |
| 邀请流程清楚 | 加入任务 | 能接受、拒绝和恢复 | 大量失效 | QA / Research |
| 状态一致 | 多设备测试 | 成员和 Leader 一致 | Split-Brain | Integration Test |
| 重连可靠 | 断网测试 | 恢复单一 Membership | 重复角色或失败 | Fault Injection |
| 非语音可协作 | 无语音测试 | 能完成核心目标 | 无法协调 | Accessibility Test |
| 跨平台可理解 | 多平台任务 | 资格和关系正确 | 好友或 Block 不一致 | QA |
| Block 生效 | 安全测试 | 无继续交互 | 仍能骚扰 | Safety Test |
| 奖励公平 | 断线和 Backfill | 结果符合规则 | 重复或误判 | Data / QA |
| 权限受控 | Leader 场景 | 不越权 | 恶意踢人或改状态 | Security Test |
| 结果一致 | 终局和超时 | 单一 Final Result | 多结果 | Integration Test |

### 59.3 Behavioral Metrics

- Friend Request Sent；
- Friend Request Accepted；
- Friend Removed；
- Party Created；
- Party Joined；
- Invite Sent；
- Invite Accepted；
- Invite Declined；
- Lobby Created；
- Ready Changed；
- Session Joined；
- Session Left；
- Disconnect；
- Reconnect Attempt；
- Reconnect Success；
- Leader Transferred；
- Block Applied；
- Mute Applied；
- Voice Joined；
- Ping Used。

### 59.4 Outcome Metrics

- Invite Acceptance；
- Party Formation Time；
- Lobby Start Success；
- Session Join Success；
- Disconnect Rate；
- Reconnect Success；
- Host Migration Success；
- Party State Consistency；
- Cross-Platform Join Success；
- Block Enforcement；
- Shared Progress Accuracy；
- Reward Finalization；
- Social Complaint Rate；
- Voice Dependency；
- Non-Verbal Communication Success。

### 59.5 Negative Metrics

- 邀请 Spam；
- Party Split-Brain；
- 重复 Membership；
- Ready 卡死；
- 主机优势；
- 主机迁移失败；
- 断线被误罚；
- Block 后仍匹配；
- 语音成为强制；
- 跨平台关系失效；
- 儿童限制被绕过；
- 恶意 Kick；
- 重复奖励；
- Shared Progress 不公平；
- Session Result 分叉。

### 59.6 Event Intents

| Event Intent | Trigger | Key Properties | Privacy Notes |
|---|---|---|---|
| Relationship Changed | 好友或 Block 变化 | Type, Result | 不记录私人备注 |
| Party State Changed | Party 变化 | Member Count, Reason | 不记录真实姓名 |
| Invite Resolved | Invite 结束 | Type, Result, Age | 数据最小化 |
| Session Membership Changed | 加入或离开 | State, Reason | 不记录精确 IP |
| Reconnect Completed | 恢复 | Result, Duration | 安全用途 |
| Leader Changed | 权限转移 | Reason, Method | 审计 |
| Communication Preference Applied | Mute / Voice | Type, Scope | 不记录内容 |
| Session Result Finalized | 结果确认 | Outcome, Integrity | 不记录敏感聊天 |

---

## 60. Test Strategy

### 60.1 Identity and Relationship

- 同名；
- 多平台映射；
- Friend；
- Follow；
- Block；
- Remove；
- Account Delete；
- Guest Upgrade。

### 60.2 Party and Invite

- 创建；
- 加入；
- 满员；
- 重复 Invite；
- Expiry；
- Leader Transfer；
- Join Request；
- Privacy；
- Concurrent Accept。

### 60.3 Lobby and Ready

- 配置变化；
- 成员变化；
- 加载超时；
- Ready Reset；
- Version Mismatch；
- Content Ineligible；
- Cross-Play Difference。

### 60.4 Session

- Join；
- Late Join；
- Leave；
- Disconnect；
- Reconnect；
- Server Restart；
- Host Migration；
- Session Abort；
- Result Finalization。

### 60.5 Communication

- Text；
- Voice；
- Ping；
- Mute；
- Block；
- Parental Restriction；
- Provider Failure；
- Translation。

### 60.6 Cross-Platform

- 不同平台；
- 不同版本；
- 平台好友；
- Product Friend；
- Platform Block；
- Product Block；
- Entitlement Difference。

### 60.7 Security

- Invite Forgery；
- Reconnect Replay；
- Session Hijack；
- Identity Spoof；
- Host Tamper；
- Unauthorized Kick；
- Deep Link Abuse。

### 60.8 Accessibility

- 无语音；
- 读屏；
- 大字体；
- 手柄；
- 触摸；
- 辅助输入；
- 色觉；
- 延长 Ready。

---

## 61. Social Contract Template

```markdown
# Social Feature Contract

## Identity

- Identity Type:
- Display Fields:
- Privacy:
- Platform Mapping:

## Relationship

- Type:
- Direction:
- Request:
- Accept:
- Remove:
- Block:
- Retention:

## Interaction

- Invite:
- Join:
- Message:
- Voice:
- Ping:
- Spectate:

## Permissions

- Player:
- Leader:
- Moderator:
- Platform:
- Parent:

## Safety

- Mute:
- Block:
- Report:
- Rate Limit:
- Child Policy:

## Persistence

- Scope:
- Sync:
- History:
- Delete:

## Validation

- Success:
- Failure:
- Metrics:
```

---

## 62. Multiplayer Session Contract Template

```markdown
# Multiplayer Session Contract

## Session

- Session ID:
- Content:
- Rule Version:
- Authority:
- Region:
- Capacity:
- Teams:

## Membership

- Join:
- Leave:
- Kick:
- Disconnect:
- Reconnect:
- Backfill:
- Spectator:

## Lobby

- Ready:
- Loadout Validation:
- Start:
- Timeout:
- Leader:

## Network

- Tick:
- Prediction:
- Reconciliation:
- Migration:
- Failure:

## Result

- Finalization:
- Integrity:
- Rewards:
- Objectives:
- Ranking:
- Unknown Result:

## Security and Privacy

- Auth:
- Token:
- Visibility:
- Block:
- Voice:
- Audit:

## Validation

- Success:
- Failure:
- Metrics:
```

---

## 63. Social and Multiplayer Debt

Social and Multiplayer Debt 包括：

- 多套身份；
- Party / Lobby / Session 概念混乱；
- 不同玩法重复邀请系统；
- 旧 Platform Mapping；
- 无 Expiry Invite；
- Split-Brain 状态；
- 无重连；
- 无 Host Migration；
- 语音依赖；
- Block 不统一；
- 队长权限例外；
- 跨平台行为差异；
- Session Result 无统一权威；
- 多人结果无法审计。

### 63.1 Signals

- 玩家经常重建 Party；
- 同一好友在不同平台显示不同；
- 断线就永久失去内容；
- Support 无法判断玩家是否在 Session；
- 多人玩法各自定义 Ready；
- 恶意 Kick 投诉高；
- 语音关闭后无法完成内容；
- Session 结果争议频繁。

### 63.2 Reduction

- 统一 Identity Mapping；
- 统一 Party、Lobby、Room、Session 模型；
- Invite Registry；
- Membership 状态机；
- Reconnect Contract；
- Session Authority；
- Block 和 Moderation 对接；
- 非语音协作；
- Cross-Platform Contract；
- Result Finalization；
- 定期 Social Health Review。

---

## 64. Rollout and Migration

### 64.1 Rollout

社交和多人变更应按：

```text
Local Simulation
→ Automated Multi-Client
→ Internal
→ Closed Group
→ Small Region
→ Platform Cohort
→ Broad Release
→ Full Release
```

### 64.2 High-Risk Changes

包括：

- Identity Mapping；
- Friends；
- Block；
- Party；
- Session Membership；
- Reconnect；
- Host Migration；
- Voice；
- Cross-Play；
- Child Safety；
- Result Finalization。

### 64.3 Migration

必须定义：

- Social Identity；
- Friend；
- Block；
- Party；
- Invite；
- Session Membership；
- Reconnect；
- Recent Players；
- Voice Preference；
- Cross-Platform Mapping；
- History。

### 64.4 Rollback

回滚时：

- 不重复关系；
- 不丢 Block；
- 不创建重复 Party；
- 不重复 Session Reward；
- 保留当前 Session 或安全结束；
- 恢复旧 Mapping；
- 保留 Invite Expiry；
- 保留 Moderation Restriction。

### 64.5 Stop Conditions

出现以下情况应停止发布：

- 身份串号；
- Block 失效；
- 儿童限制失效；
- Session Result 分叉；
- 重复 Membership；
- Reconnect 大规模失败；
- Party Split-Brain；
- Host Migration 导致资产异常；
- Cross-Platform 好友或 Block 大量丢失；
- 语音隐私异常；
- 未授权玩家进入 Session。

---

## 65. Risks and Open Questions

| Item | Type | Impact | Probability | Mitigation | Owner |
|---|---|---:|---:|---|---|
| 身份映射错误 | Security Risk | 严重 | 低 | 强认证和审计 | Security |
| Party / Session 状态分叉 | Architecture Risk | 严重 | 中 | 服务端权威 Revision | Engineering |
| 断线后重复角色或奖励 | Transaction Risk | 严重 | 中 | 唯一 Membership 和 Result ID | Engineering |
| Block 跨平台不一致 | Safety Risk | 严重 | 中 | 产品层统一 Block | Trust and Safety |
| Host Migration 不可靠 | Network Risk | 高 | 中 | Dedicated Server 或双主保护 | Engineering |
| 语音成为核心门槛 | Accessibility Risk | 高 | 高 | Ping / Quick Chat | Design |
| Leader 权限滥用 | Social Risk | 高 | 中 | 权限边界和保护 | Product |
| Cross-Region 延迟失衡 | Fairness Risk | 高 | 中 | Region 策略 | Engineering |
| 儿童安全绕过 | Safety / Compliance Risk | 严重 | 低 | 服务端 Enforcement | Security |
| 多人系统持续例外化 | Maintenance Risk | 高 | 高 | Session Contract | Architecture |

---

## 66. Review Checklist

### Identity and Relationships

- [ ] Account、Social、Platform 和 Session Identity 区分；
- [ ] Display Name 和唯一标识分离；
- [ ] Presence 受隐私控制；
- [ ] Friend、Follow、Block、Mute 和 Recent Player 区分；
- [ ] Request Lifecycle 完整。

### Party, Lobby, Room and Session

- [ ] 四类对象职责和生命周期清楚；
- [ ] Membership 状态唯一；
- [ ] Leader、Owner 和 Host 权限边界明确；
- [ ] Ready Check 和 Team Assignment 可审计；
- [ ] Session Authority 明确。

### Invite and Join

- [ ] Invite 和 Join Request 类型完整；
- [ ] Expiry、Dedupe 和 Rate Limit 已定义；
- [ ] Accept 时重新验证；
- [ ] Block 和 Moderation 优先；
- [ ] 深链不能绕过资格。

### Network and Recovery

- [ ] Dedicated、Listen 和 P2P 权威边界明确；
- [ ] State Sync、Prediction 和 Reconciliation 已定义；
- [ ] Disconnect 分类和 Grace Window 清楚；
- [ ] Reconnect Token 安全；
- [ ] Host Migration 有双主防护和失败策略。

### Cross-Platform and Version

- [ ] Cross-Play、Cross-Save、Cross-Progression 区分；
- [ ] Cross-Platform Friend 和 Block 一致；
- [ ] Region、Version 和 Platform 限制可解释；
- [ ] Party Version Mismatch 有恢复；
- [ ] Session Rule Version 唯一。

### Communication and Accessibility

- [ ] Voice 非强制；
- [ ] Text、Ping、Quick Chat 和 Marker 可用；
- [ ] Mute、Block 和 Report 易访问；
- [ ] 读屏和替代输入支持完整；
- [ ] Team 和 Ready 不只靠颜色。

### Progress, Rewards and Results

- [ ] Shared Progress Owner 明确；
- [ ] Contribution 不只看最后一击；
- [ ] Session Result 由权威服务 Finalize；
- [ ] Reward Eligibility 幂等；
- [ ] Backfill、Disconnect 和 Reconnect 奖励规则清楚。

### Privacy, Safety and Ethics

- [ ] 默认最小公开；
- [ ] 社交图谱隐私明确；
- [ ] 儿童和家长控制服务端执行；
- [ ] 不强迫社交和公开分享；
- [ ] Leader 和付费玩家无越权特权。

### Validation and Operations

- [ ] Identity、Invite、Party、Session、Reconnect、Block 和 Result 指标完整；
- [ ] Multi-Client、Fault Injection 和 Security Test 完成；
- [ ] Social Debt 可监控；
- [ ] Support 能查看脱敏状态；
- [ ] Rollback 和 Stop Conditions 明确。

---

## 67. V1 Completion Criteria

Social and Multiplayer 可以被视为 V1，当：

- Account、Social、Platform、Display 和 Session Identity 已明确区分；
- Display Name、Presence、Friend、Follow、Block、Mute 和 Recent Player 模型完整；
- Friend Request、Invite 和 Join Request 生命周期、过期、去重和频控明确；
- Party、Lobby、Room 和 Session 的职责、权限和生命周期已经分离；
- Membership、Leadership、Ready、Team Assignment 和 Session Authority 状态明确；
- Party Leader、Room Owner 和 Session Host 的权限边界清楚；
- Character、Loadout、Content、Version、Region 和 Entitlement 进入验证完整；
- Text、Voice、Ping、Quick Chat、Marker 和系统消息有统一通信模型；
- Voice 非核心强制，非语音协作足以完成核心多人内容；
- Cross-Play、Cross-Save 和 Cross-Progression 已区分；
- Cross-Platform、Cross-Region 和 Cross-Version 规则明确；
- Dedicated、Listen 和 P2P 权威边界清楚；
- State Synchronization、Prediction、Reconciliation 和 Revision 规则完整；
- Disconnect、Grace、Reconnect、Reconnect Token 和恢复流程完整；
- Host Migration、Backfill、Spectator、Leave、Kick 和 AFK 有专项规则；
- Shared Progress、Shared Reward 和 Session Result 的 Owner、事务和幂等边界明确；
- Friend、Block、Party、Invite、Membership、Result 和 History 可持久化；
- Privacy、Block、Mute、Child Safety、Parental Control 和最小公开通过评审；
- Identity、Invite、Party、Session、Reconnect、Cross-Platform、Block 和 Result 有验证计划；
- Social and Multiplayer Debt 有识别和治理方式；
- 高风险多人变更支持 Multi-Client 测试、灰度、迁移、回滚和停止条件；
- 下游 Matchmaking、Moderation、Reward、Objectives、Live Operations 和 Analytics 可以直接引用本文件。

---

## 68. Related Documents

### Philosophy

- [Player First Design](../../philosophy/foundation/player-first-design.md)
- [Choice and Consequence](../../philosophy/experience/choice-and-consequence.md)
- [Challenge and Fairness](../../philosophy/experience/challenge-and-fairness.md)
- [Consistency and Coherence](../../philosophy/long-term/consistency-and-coherence.md)
- [Accessibility and Inclusivity](../../philosophy/responsibility/accessibility-and-inclusivity.md)
- [Ethical Design](../../philosophy/responsibility/ethical-design.md)

### Systems

- [Systems README](../README.md)
- [System Design Framework](../system-design-framework.md)
- [System Map](../system-map.md)
- [Integration Rules](../integration-rules.md)
- [Game State and Flow](../core/game-state-and-flow.md)
- [Input and Interaction](../core/input-and-interaction.md)
- [Characters and Loadouts](../content/characters-and-loadouts.md)
- [Content and Unlocks](../content/content-and-unlocks.md)
- [Objectives and Quests](../content/objectives-and-quests.md)
- [Difficulty and Challenge](../progression/difficulty-and-challenge.md)
- [Reward System](../progression/reward-system.md)
- [Save and Persistence](../player/save-and-persistence.md)
- [Settings and Preferences](../player/settings-and-preferences.md)
- [Notification and Reminders](../player/notification-and-reminders.md)
- `matchmaking-and-competition.md`
- `moderation-and-safety.md`
