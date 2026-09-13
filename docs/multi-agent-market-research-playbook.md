# 多 Agent 市场调研使用手册

## 使用目标

这套流程用于评估 SentryFlow AI 这类 Anker / eufy 智能安防黑客松项目是否跑偏、是否符合主办方期待、是否有拿奖潜力，并把结论沉淀成可用于路演、MVP 设计和 24 小时现场开发的决策材料。

核心原则：

- 少角色，强结构。
- 不一次性把所有资料塞进上下文。
- 每个 Agent 处理一组高度相关的问题。
- 所有市场、竞品、SDK、占有率、销售表现类结论都要带来源和日期。
- 最终输出必须回到比赛拿奖，而不是做一份泛泛市场报告。

## 推荐 4-Agent 架构

```text
Strategy Agent / 策略评审 Agent
  判断比赛想要什么、当前方向是否跑偏、拿奖项目应该长什么样。

Market Agent / 市场调研 Agent
  调研 Anker / eufy 产品与 SDK、中国竞品、马来西亚市场机会。

Solution Agent / 方案架构 Agent
  收敛项目切入点、AI Agent 能力、24 小时技术实现方案。

Pitch Agent / 路演整合 Agent
  汇总最终 6 项输出，校验证据，并包装成比赛路演故事线。
```

这个版本把原来的 8 个角色压缩成 4 个：

| 原角色 | 合并到 |
| --- | --- |
| Hackathon Judge Agent | Strategy Agent |
| Anker eufy Product Agent | Market Agent |
| China Competitor Agent | Market Agent |
| Malaysia Market Agent | Market Agent |
| Opportunity Agent | Solution Agent |
| Technical Architecture Agent | Solution Agent |
| Evidence Checker Agent | Pitch Agent |
| Pitch Story Agent | Pitch Agent |

## 上下文分包方式

### Context Pack A：比赛与战略包

给 `Strategy Agent` 使用。

包含：

- `refer/anker-eufy-smart-security-official-brief.md`
- 黑客松背景
- 用户目标：拿奖，不只是做可用产品
- 当前项目一句话方向，如有

不包含：

- 长篇市场调研
- 技术实现细节
- 原型 UI 细节

### Context Pack B：市场调研包

给 `Market Agent` 使用。

包含：

- `Strategy Agent` 输出的比赛判断
- 用户原始需求中的第 2、3、4 部分
- 必要时包含当前项目方向，但允许 Market Agent 不迁就已有方向

必须分成三个小节输出：

1. `Anker / eufy 产品与 SDK`
2. `中国竞品与 AI 摄像头成熟场景`
3. `马来西亚市场机会`

### Context Pack C：方案收敛包

给 `Solution Agent` 使用。

包含：

- `Strategy Agent` 输出
- `Market Agent` 输出
- 当前项目文档摘要：
  - `README.md`
  - `docs/hackathon-brief.md`
  - `product/mvp-goal-and-prototype.md`
  - `product/user-stories.md`
  - `docs/mvp-roadmap.md`
  - `prompts/inspection-agent-prompts.md`

注意：Solution Agent 可以推翻当前方向，不必迁就已有原型或当前 focus。

### Context Pack D：最终整合包

给 `Pitch Agent` 使用。

包含：

- `Strategy Agent` 最终摘要
- `Market Agent` 关键证据和不确定项
- `Solution Agent` 推荐方案和技术路线
- 用户要求的最终 6 项输出

Pitch Agent 必须做一次证据校验：

- 哪些结论可以放进路演
- 哪些只能作为假设
- 哪些说法需要弱化或删除
- 哪些证据还要补

## 每个 Agent 的统一输出格式

```text
Agent:
Scope:
Key Findings:
Evidence:
Risks / Unknowns:
Implication for Project:
Recommendation:
Confidence: high / medium / low
Next Research Needed:
```

## Agent Prompt 模板

### 1. Strategy Agent

```text
你是 Strategy Agent，负责从黑客松评审和主办方期待角度判断项目方向。

请只基于 refer/ 资料、黑客松背景和当前项目一句话方向进行分析。先不要联网深挖。

背景：
- 这是 Anker / eufy 相关智能安防黑客松。
- 前期主要准备项目思路和文档。
- 正式比赛现场 24 小时内由官方提供硬件、数据和资源，再完成实现与展示。
- 我的目标是拿奖，不只是做一个技术 demo。

你必须回答：
1. 这类活动更看重什么？
2. 主办方/评审真正想看到的作品特征是什么？
3. 评审更喜欢通用技术展示，还是具体场景闭环？
4. 当前项目方向是否符合比赛场景？
5. 当前项目方向是否可能偏离主办方需求？
6. 如果目标是拿奖，应该强化哪些点？
7. 哪些方向或表达方式应该避免？

输出格式：
- 主办方期待
- 评审喜欢的信号
- 当前方向匹配度
- 跑偏风险
- 拿奖潜力判断
- 拿奖建议
- 需要避免的坑
- 下一轮 Market Agent 需要验证的问题
- Confidence
```

### 2. Market Agent

```text
你是 Market Agent，负责做 Anker / eufy 产品调研、中国竞品调研和马来西亚市场分析。

请联网调研，所有市场、竞品、SDK、占有率、销售表现类结论都必须给来源链接、发布日期或访问日期。

请严格分成三个部分输出。

第一部分：Anker / eufy 产品与 SDK
必须回答：
1. Anker / eufy 当前主要产品线是什么？
2. eufy 智能摄像头、智能门铃、家庭安防、智能锁等相关产品有哪些？
3. eufy 智能安防产品的核心卖点和主要功能是什么？
4. eufy 在市场中的占有率、销售表现或公开可验证的市场位置如何？
5. Anker / eufy 官方是否提供开放 SDK、API 或开发者资源？
6. 如果有 SDK/API，它是否支持视频流、图片、运动事件、设备状态、PTZ、灯光、夜视、隐私区等能力？
7. 哪些能力是官方确认，哪些只是社区方案或不确定信息？
8. SDK/API 如何接入 AI Agent 链路，实现简单的视频处理或智能安防分析？

第二部分：中国竞品与 AI 摄像头成熟场景
必须回答：
1. 中国市场中，与 eufy 类似的智能摄像头 / 智能安防产品有哪些？
2. 家庭、商铺、社区、园区、养老、宠物、儿童看护等领域，哪些摄像头产品占有率或成熟度最高？
3. 摄像头 + AI 结合的产品有哪些？
4. 最先进的 AI 摄像头能力已经做到什么程度？
5. Anker / eufy 当前已经做到哪些能力？
6. Anker / eufy 的短板是什么？
7. 哪些细分领域还有机会突破？
8. 哪些技术点还有明显优化空间？

第三部分：马来西亚市场机会
必须回答：
1. 马来西亚摄像头或智能安防产品普及情况如何？
2. 家庭、商用、公共、工业场景的使用情况如何？
3. 当前 AI + 摄像头在马来西亚是否已经普及？
4. 相比中国市场，马来西亚是否还不够饱和？
5. 马来西亚本地或主要销售的竞品有哪些？
6. 它们的技术水平、覆盖范围、价格区间和市场定位如何？
7. 马来西亚市场是否仍存在可以突破的边缘场景或痛点？

每部分都要输出：
- Key Findings
- Evidence Table: claim / source / date / confidence
- Implication for SentryFlow AI
- Risks / Unknowns
- Recommendation

最后输出：
- 市场调研总判断
- 对项目切入点的启发
- 下一轮 Solution Agent 必须决策的问题
```

### 3. Solution Agent

```text
你是 Solution Agent，负责重新审视项目切入点，并设计 24 小时黑客松可落地技术方案。

你可以推翻当前方向，不必迁就已有原型、已有 README 或当前 focus。目标是找到最符合 Anker / eufy 黑客松、最有展示效果、最有拿奖潜力的智能安防 AI Agent 方向。

输入：
- Strategy Agent 结论
- Market Agent 结论
- 当前项目文档摘要

第一部分：项目切入点判断
必须回答：
1. 我应该做一个什么样的 AI Agent，才能更好满足智能安防场景？
2. 这个 Agent 应该解决什么具体问题，而不是只做泛泛的视频分析工具？
3. 当前项目切入点是否合理？
4. 当前项目切入点是否足够具体？
5. 当前项目切入点是否有商业价值和展示效果？
6. 请发散 3-5 个可能更容易打动评审的项目方向。
7. 请对每个候选方向评分：
   - 主办方匹配度
   - eufy 硬件匹配度
   - AI Agent 展示效果
   - 24 小时可落地性
   - 商业价值
   - 差异化
   - 拿奖潜力
8. 最后只推荐 1 个主方向，并说明为什么。

第二部分：技术实现架构
必须回答：
1. 如何接入 Anker / eufy SDK 或设备数据源？
2. 如何获取视频流、图片、事件、告警等数据？
3. 如何把原始数据解析成 AI 可处理的数据？
4. 用什么 AI 框架或 Agent 框架更合适？
5. 视频分析怎么实现？
6. 事件识别怎么实现？
7. 告警总结怎么实现？
8. 用户问答怎么实现？
9. 自动化处理怎么实现？
10. 如果比赛现场只有 24 小时，应该优先实现哪些核心功能？
11. 哪些功能可以作为演示增强项？

技术方案必须给两条路径：

路径 A：官方 SDK/API 可用
- 数据接入
- 事件触发
- AI 分析
- 告警和报告
- Demo 展示

路径 B：SDK/API 不可用或现场不稳定
- 样例视频/截图
- 模拟事件流
- 本地 JSON 事件
- AI 分析
- 前端演示
- 如何解释为未来可接入真实 eufy 设备

输出格式：
- 当前方向判断
- 候选方向评分表
- 最推荐切入点
- AI Agent 产品定义
- 24 小时 MVP 范围
- 技术架构图
- 路径 A：SDK/API 可用方案
- 路径 B：模拟兜底方案
- 必做功能
- 增强功能
- 最大技术风险
- Demo 降级方案
- 下一轮 Pitch Agent 需要包装的重点
```

### 4. Pitch Agent

```text
你是 Pitch Agent，负责把 Strategy、Market、Solution 的结果整合成最终比赛决策报告和路演故事。

你必须先做证据校验：
1. 哪些结论有可靠来源，可以放进路演？
2. 哪些结论只能作为假设？
3. 哪些市场或占有率说法证据不足，需要弱化？
4. 哪些说法可能过时、夸大或不适合写进比赛材料？

然后输出最终报告，必须包含以下 6 项：
1. 我当前方向是否跑偏的判断。
2. 主办方/评审可能真正想看到的东西。
3. 最推荐的项目切入点。
4. 产品调研和市场调研的重点清单。
5. 24 小时黑客松可落地的技术方案。
6. 一个适合比赛路演的项目故事线。

路演故事线必须包含：
- 20 秒开场痛点
- 1 分钟产品方案
- 1 分钟 Demo 流程
- 30 秒商业价值和 Anker / eufy 生态价值
- 5 页 PPT 结构

输出格式：
# SentryFlow AI 黑客松方向评估报告

## 1. 方向是否跑偏

## 2. 主办方和评审真正想看到什么

## 3. 最推荐项目切入点

## 4. 产品调研和市场调研重点清单

## 5. 24 小时 MVP 技术方案

## 6. 路演故事线

## 7. 证据校验

## 8. 待验证问题
```

## 推荐执行顺序

### Round 1：Strategy Agent

目标：先判断比赛方向和拿奖标准，不急着做大调研。

输入：

- `refer/anker-eufy-smart-security-official-brief.md`
- 黑客松背景
- 当前项目一句话方向

输出：

- 主办方期待
- 评审喜欢什么
- 当前方向是否跑偏
- 拿奖潜力初判
- Market Agent 需要验证的问题

### Round 2：Market Agent

目标：一次完成三块外部调研，但必须分小节输出，避免混乱。

输入：

- Strategy Agent 输出
- 用户原始需求第 2、3、4 部分

输出：

- Anker / eufy 产品与 SDK
- 中国竞品与 AI 摄像头成熟场景
- 马来西亚市场机会
- 对项目切入点的启发

### Round 3：Solution Agent

目标：把调研结果收敛成项目方向和 24 小时技术方案。

输入：

- Strategy Agent 输出
- Market Agent 输出
- 当前项目文档摘要

输出：

- 3-5 个候选方向评分
- 最推荐切入点
- AI Agent 产品定义
- SDK/API 可用方案
- SDK/API 不可用兜底方案
- 24 小时必做和增强功能

### Round 4：Pitch Agent

目标：生成最终可用的比赛判断、准备清单和路演材料。

输入：

- Strategy Agent 输出
- Market Agent 输出
- Solution Agent 输出

输出用户要求的 6 项：

1. 当前方向是否跑偏的判断。
2. 主办方/评审可能真正想看到的东西。
3. 最推荐的项目切入点。
4. 产品调研和市场调研的重点清单。
5. 24 小时黑客松可落地的技术方案。
6. 适合比赛路演的项目故事线。

## 怎么在 Codex 里实际使用

### 用法一：按轮次跑

第一轮：

```text
请按 docs/multi-agent-market-research-playbook.md 执行 Round 1：Strategy Agent。
只读取 refer/ 资料和当前项目背景，先不要联网。
目标是判断：我的智能安防 AI 项目方向是否跑偏，是否符合 Anker/eufy 黑客松评审期待，以及拿奖项目应该具备哪些特征。
```

第二轮：

```text
继续 Round 2：Market Agent。
请联网调研 Anker/eufy 产品与 SDK、中国竞品、马来西亚市场。
严格按 playbook 的三个小节输出，每条关键结论都要带来源和日期。
```

第三轮：

```text
继续 Round 3：Solution Agent。
请基于 Strategy 和 Market 的结论，重新判断项目切入点，并给出 24 小时黑客松可落地技术方案。
允许推翻当前方向，不必迁就已有原型。
```

第四轮：

```text
继续 Round 4：Pitch Agent。
请先做证据校验，再输出最终 6 项：是否跑偏、评审想看什么、推荐切入点、调研重点清单、24 小时技术方案、路演故事线。
```

### 用法二：只跑一个 Agent

如果担心上下文太长，可以每次只跑一个：

```text
只运行 Market Agent 的第三部分：马来西亚市场机会。
不要输出最终项目建议，只输出市场成熟度、竞品、价格区间、机会和证据链接。
```

### 用法三：先判断方向，再补证据

如果想先快速判断：

```text
请按 playbook 先做 Round 1 和 Round 3 的轻量版。
先不要联网，只基于 refer/ 和当前项目文档判断方向是否跑偏，并列出必须联网验证的证据清单。
```

然后再逐项补证据：

```text
现在只补 eufy SDK/API 的证据。
```

```text
现在只补马来西亚 AI CCTV 市场证据。
```

## 当前方向的初步判断框架

在正式调研前，可以先用这个判断表：

| 维度 | 判断问题 | 对拿奖的影响 |
| --- | --- | --- |
| 主办方匹配 | 是否体现 eufy 摄像头能力，而不是脱离硬件讲 AI？ | 高 |
| 场景具体性 | 是否服务一个具体人、空间和任务？ | 高 |
| 闭环完整度 | 是否从看见、理解、行动到反馈形成闭环？ | 高 |
| 现场可演示 | 24 小时内能否稳定跑通？ | 高 |
| 差异化 | 是否避开普通家庭看护和通用监控？ | 中高 |
| 商业想象 | 是否能帮 Anker / eufy 扩展新市场？ | 中高 |
| SDK 风险 | 是否依赖现场不确定能力？ | 高 |

