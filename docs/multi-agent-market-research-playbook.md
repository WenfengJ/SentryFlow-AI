# 多 Agent 市场调研使用手册

## 使用目标

这套流程用于评估 SentryFlow AI 这类 Anker / eufy 智能安防黑客松项目是否跑偏、是否符合主办方期待、是否有拿奖潜力，并把结论沉淀成可用于路演、MVP 设计和 24 小时现场开发的决策材料。

核心原则：

- 少角色，强结构。
- 不一次性把所有资料塞进上下文。
- 每个 Agent 处理一组高度相关的问题。
- 每个 Agent 必须参考前序 Agent 的确定性结果，不能把自己当成孤立任务。
- 后续 Agent 如果发现前序 Agent 的事实、判断或假设有误，必须显式提出更正。
- 所有市场、竞品、SDK、占有率、销售表现类结论都要带来源和日期。
- 所有跨 Agent 复用的关键事实，都进入共享事实账本。
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

## 跨 Agent 协作机制

这套 playbook 不是简单的“4 个 Agent 各写一段”。它必须按下面方式协作：

```text
前序 Agent 输出确定性结果
  -> 后续 Agent 引用这些结果
  -> 后续 Agent 用新证据验证或修正
  -> 如果发现冲突，生成修订建议
  -> 最终 Pitch Agent 只采用已校验后的事实
```

### 共享事实账本

每一轮结束后，都要维护一个 `Shared Facts / 共享事实账本`。它可以放在当轮文档末尾，也可以单独沉淀到 `docs/v2-design/shared-facts.md`。

格式：

```text
Fact ID:
Statement:
Status: confirmed / likely / hypothesis / rejected / superseded
Source:
Owner Agent:
Last Updated:
Used By:
Notes:
```

示例：

```text
Fact ID: F-SDK-001
Statement: 公开资料未确认 eufy Security 存在稳定完整的第三方官方开放 SDK/API。
Status: likely
Source: eufy official site scan + Home Assistant integration + community API discussions
Owner Agent: Market Agent
Last Updated: 2026-09-13
Used By: Solution Agent, Pitch Agent
Notes: 不能在 Pitch 中宣称官方 SDK 已开放，除非赛事方现场确认。
```

### 结论状态定义

| 状态 | 含义 | 最终报告如何使用 |
| --- | --- | --- |
| confirmed | 有可靠来源或本地资料直接支持 | 可以写进主结论和路演 |
| likely | 多个迹象支持，但仍缺少官方或一手证据 | 可以谨慎使用，要弱化语气 |
| hypothesis | 当前只是策略假设或推断 | 只能作为待验证机会 |
| rejected | 已被后续证据否定 | 不能再使用 |
| superseded | 被更准确的新结论替代 | 使用新结论，并说明旧结论已更新 |

### 冲突处理规则

后续 Agent 发现冲突时，不要静默覆盖。必须输出：

```text
Conflict:
Previous Claim:
New Evidence:
Correction:
Impact:
Files / Sections To Update:
```

例如：

```text
Conflict:
Strategy Agent 假设 eufy 可能有开放 SDK。

Previous Claim:
SDK/API 可用性待确认。

New Evidence:
Market Agent 未找到官方公开 SDK，社区方案也提示旧 API 可能不稳定。

Correction:
把技术主路径改成“官方现场 SDK 可用则接入，否则模拟事件流兜底”，不要在 Pitch 中承诺公开 SDK。

Impact:
Solution Agent 必须设计 A/B 两条路径；Pitch Agent 必须弱化 SDK 相关说法。

Files / Sections To Update:
docs/v2-design/round1-strategy-agent.md 的 SDK 风险说明
docs/v2-design/round3-solution-agent.md 的技术架构
最终 Pitch 的技术可行性表述
```

### 反向修订规则

如果后续 Agent 的新证据会改变前序 Agent 的关键判断，必须做两件事：

1. 在当前 Agent 文档中写出 `对后续方案的影响`。
2. 必要时更新前序准备文档，或追加一个 `Correction Note`，避免最终报告继续引用旧说法。

推荐格式：

```text
## 对后续方案的影响

| Previous Round | Original Claim | Correction | Reason | Action |
| --- | --- | --- | --- | --- |
| Round 1 Strategy | eufy SDK 可能可用 | 公开资料无法确认官方开放 SDK，必须保留模拟兜底 | Market Agent 外部调研 | 更新 Round 3 技术路径 |
```

### 最终采用规则

最终 Pitch Agent 只能采用：

- `confirmed` 结论
- 明确标注为谨慎推断的 `likely` 结论
- 对项目方向有用但不作为事实宣称的 `hypothesis`

Pitch Agent 必须删除或降级：

- 无来源的市场规模
- 无来源的占有率
- 未确认的 SDK/API 能力
- 与后续证据冲突但未修订的早期判断
- 听起来很强但无法在 24 小时 Demo 中体现的能力

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
- `Strategy Agent` 的共享事实账本和待验证问题
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
- 当前共享事实账本
- 前两轮的关键变化和对后续方案的影响
- 当前项目文档摘要：
  - `README.md`
  - `docs/v1-design/hackathon-brief.md`
  - `docs/v1-design/product/mvp-goal-and-prototype.md`
  - `docs/v1-design/product/user-stories.md`
  - `docs/v1-design/mvp-roadmap.md`
  - `docs/v1-design/prompts/inspection-agent-prompts.md`

注意：Solution Agent 可以推翻当前方向，不必迁就已有原型或当前 focus。

### Context Pack D：最终整合包

给 `Pitch Agent` 使用。

包含：

- `Strategy Agent` 最终摘要
- `Market Agent` 关键证据和不确定项
- `Solution Agent` 推荐方案和技术路线
- 当前共享事实账本
- 所有关键变化、更正和仍待验证的问题
- 用户要求的最终 6 项输出

Pitch Agent 必须做一次证据校验：

- 哪些结论可以放进路演
- 哪些只能作为假设
- 哪些说法需要弱化或删除
- 哪些证据还要补

## 每个 Agent 的统一输出格式

```text
Agent:
本轮目标:
本轮使用材料:
承接的前序判断:
本轮关键发现:
证据:
本轮关键变化:
对后续方案的影响:
风险和未知项:
对项目的含义:
建议:
后续可复用事实:
整体可信度: high / medium / low
下一步需要补充:
```

## Agent Prompt 模板

### 1. Strategy Agent

```text
你是 Strategy Agent，负责从黑客松评审和主办方期待角度判断项目方向。

请只基于 refer/ 资料、黑客松背景和当前项目一句话方向进行分析。先不要联网深挖。

你是第一轮 Agent，因此不需要引用前序 Agent 结论。但你必须把自己的判断拆成：
- confirmed：由 refer/ 或官方赛事资料直接支持
- likely：基于比赛特点的合理推断
- hypothesis：需要后续 Market Agent 验证的假设

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
- Shared Facts To Add
- Hypotheses To Verify
- 下一轮 Market Agent 需要验证的问题
- Confidence
```

### 2. Market Agent

```text
你是 Market Agent，负责做 Anker / eufy 产品调研、中国竞品调研和马来西亚市场分析。

请联网调研，所有市场、竞品、SDK、占有率、销售表现类结论都必须给来源链接、发布日期或访问日期。

你必须先读取并参考 Strategy Agent 的确定性结果和共享事实账本。

执行规则：
1. Strategy Agent 的 confirmed 结论默认沿用，除非你找到反证。
2. Strategy Agent 的 likely / hypothesis 结论必须用外部资料验证。
3. 如果外部资料推翻或修正 Strategy Agent 的判断，必须输出 `本轮关键变化` 和 `对后续方案的影响`。
4. 不允许把已被修正的旧判断继续传给 Solution Agent。
5. 所有会影响项目方向的市场事实，都要加入 `后续可复用事实`。

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
- 关键发现
- 证据表：结论 / 来源 / 日期 / 可信度
- 对 SentryFlow AI 的含义
- 风险和未知项
- 建议

最后输出：
- 承接的前序判断
- 本轮关键变化
- 对后续方案的影响
- 后续可复用事实
- 市场调研总判断
- 对项目切入点的启发
- 下一轮 Solution Agent 必须决策的问题
```

### 3. Solution Agent

```text
你是 Solution Agent，负责重新审视项目切入点，并设计 24 小时黑客松可落地技术方案。

你可以推翻当前方向，不必迁就已有原型、已有 README 或当前 focus。目标是找到最符合 Anker / eufy 黑客松、最有展示效果、最有拿奖潜力的智能安防 AI Agent 方向。

你必须先读取并参考：
- Strategy Agent 的比赛判断
- Market Agent 的产品、竞品、市场事实
- 当前共享事实账本
- 前两轮的关键变化和对后续方案的影响

执行规则：
1. 技术方案必须服从 Market Agent 已验证的产品和 SDK/API 事实。
2. 如果 Market Agent 发现 SDK/API 不确定，你不能把“接入官方 SDK”当成唯一主路径。
3. 如果你认为 Market Agent 的市场结论会导致方向调整，必须明确说明调整原因。
4. 如果你发现 Strategy 或 Market 的结论之间有冲突，必须先解决冲突，再推荐项目方向。
5. 你输出的推荐方向要更新共享事实账本，供 Pitch Agent 使用。

输入：
- Strategy Agent 结论
- Market Agent 结论
- Shared Facts / 共享事实账本
- 关键变化和对后续方案的影响
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
- 承接的前序判断
- 本轮关键变化
- 对后续方案的影响
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
- 后续可复用事实
- 下一轮 Pitch Agent 需要包装的重点
```

### 4. Pitch Agent

```text
你是 Pitch Agent，负责把 Strategy、Market、Solution 的结果整合成最终比赛决策报告和路演故事。

你必须先读取：
- Strategy Agent 输出
- Market Agent 输出
- Solution Agent 输出
- Shared Facts / 共享事实账本
- 所有关键变化和对后续方案的影响

执行规则：
1. 最终报告只能采用最新版本的共享事实。
2. 如果前序 Agent 的旧说法已经被更正，最终报告不能继续使用旧说法。
3. 如果 Strategy、Market、Solution 之间仍有冲突，必须先列出并给出取舍。
4. 对 `confirmed` 结论可以直接写入报告。
5. 对 `likely` 结论必须使用谨慎表达。
6. 对 `hypothesis` 结论只能写入待验证问题或机会假设，不能当事实宣传。
7. 对 `rejected` 或 `superseded` 结论必须删除或替换。

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

## 0. 已确认共识与关键变化

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
- Shared Facts / 共享事实账本初版
- Hypotheses To Verify / 待验证假设
- Market Agent 需要验证的问题

### Round 2：Market Agent

目标：一次完成三块外部调研，但必须分小节输出，并验证或修正 Round 1 的假设。

输入：

- Strategy Agent 输出
- Shared Facts / 共享事实账本初版
- 用户原始需求第 2、3、4 部分

输出：

- Anker / eufy 产品与 SDK
- 中国竞品与 AI 摄像头成熟场景
- 马来西亚市场机会
- 对 Strategy Agent 假设的验证结果
- 关键变化和对后续方案的影响
- 更新后的后续可复用事实
- 对项目切入点的启发

### Round 3：Solution Agent

目标：把调研结果收敛成项目方向和 24 小时技术方案，并确保技术方案服从已验证事实。

输入：

- Strategy Agent 输出
- Market Agent 输出
- 更新后的后续可复用事实
- 关键变化和对后续方案的影响
- 当前项目文档摘要

输出：

- 3-5 个候选方向评分
- 最推荐切入点
- AI Agent 产品定义
- SDK/API 可用方案
- SDK/API 不可用兜底方案
- 24 小时必做和增强功能
- 对前序冲突的处理结果
- 更新后的后续可复用事实

### Round 4：Pitch Agent

目标：生成最终可用的比赛判断、准备清单和路演材料，并只采用最新、已校验的事实。

输入：

- Strategy Agent 输出
- Market Agent 输出
- Solution Agent 输出
- 最终后续可复用事实
- 所有关键变化和对后续方案的影响

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
同时请验证 Round 1 的待验证假设。如果发现 Round 1 的判断需要修正，请用“本轮关键变化”和“对后续方案的影响”说明，并更新“后续可复用事实”。
```

第三轮：

```text
继续 Round 3：Solution Agent。
请基于 Strategy 和 Market 的结论，重新判断项目切入点，并给出 24 小时黑客松可落地技术方案。
允许推翻当前方向，不必迁就已有原型。
请先读取“后续可复用事实”和“对后续方案的影响”，不要沿用已被 Market Agent 修正的旧判断。
```

第四轮：

```text
继续 Round 4：Pitch Agent。
请先做证据校验，再输出最终 6 项：是否跑偏、评审想看什么、推荐切入点、调研重点清单、24 小时技术方案、路演故事线。
最终报告只能采用最新“后续可复用事实”。如果前序 Agent 的结论互相冲突，请先说明取舍，再写最终判断。
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
