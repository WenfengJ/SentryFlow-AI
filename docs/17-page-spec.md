# 17 Page Spec

这份文档定义 SentryFlow AI Demo 的页面结构。设计 HTML 或 React 页面时，以这里的区域、字段和状态为准。

当前主页面：`prototype/handoff-agent.html`

当前数据源：

- `app/data/events.json`
- `app/data/reviewed_events.json`
- `app/data/handoff_summary.json`
- `app/data/actions.json`

## 1. 页面总结构

SentryFlow AI Demo 使用一屏工作台结构。

```text
Top Bar
Status Ribbon
Morning Summary
Main Workspace
  - Event List
  - Event Detail
  - Handoff / Q&A / Actions
Technical Architecture Display
```

页面打开后的默认状态：

- 默认选中 `evt_001`。
- 默认显示 `47 / 44 / 2 / 1`。
- 默认状态为 `Action needed`。
- 用户可以点击 `开始昨晚事件回放` 进入演示节奏。

## 2. 顶部入口区

### 页面目的

让用户第一眼知道这是 `eufy Morning Handoff`，不是普通摄像头后台。

### 用户看到什么

- 产品名：`SentryFlow AI`
- 副标题：`eufy Morning Handoff · 晨间安防交接`
- 主操作按钮：
  - `导出交接摘要`
  - `开始昨晚事件回放`
  - `通知店长复核`

### 用户能做什么

- 点击 `开始昨晚事件回放`，触发演示主线。
- 点击 `通知店长复核`，快速把最高风险事件分派给店长。
- 点击 `导出交接摘要`，生成交接结果日志。

### 使用哪些数据字段

| 数据源 | 字段 |
| --- | --- |
| `handoff_summary.json` | `site_name` |
| `handoff_summary.json` | `demo_claim` |
| `handoff_summary.json` | `status` |
| `actions.json` | `label` / `action_type` |

### 空状态 / 失败状态

| 状态 | 页面表现 |
| --- | --- |
| 数据未加载 | 标题仍显示，按钮禁用，状态显示 `Loading handoff data` |
| 数据加载失败 | 显示 `Demo data unavailable`，提供 `Use fallback demo data` |
| 无处置动作 | 隐藏 `通知店长复核` 快捷按钮，只保留回放按钮 |

### 验收标准

- 用户打开页面 5 秒内能看出产品是 eufy 晨间安防交接。
- `开始昨晚事件回放` 是视觉上最明显的主按钮。
- 即使数据加载失败，页面仍有 fallback 入口。

## 3. 晨间交接总览

### 页面目的

把一夜告警压缩成用户早上能立刻理解的结果：47 条告警，只处理 1 件事。

### 用户看到什么

- 主标题：`47 条告警已复核。开门前只处理 1 件事。`
- 四个指标卡：
  - `Total alerts: 47`
  - `Ignored: 44`
  - `Explained: 2`
  - `Action needed: 1`
- 简短摘要：昨晚整体安全，最高风险来自后门 02:13 事件。

### 用户能做什么

- 快速判断今天是否需要处理安全事件。
- 点击 `Action needed` 指标后定位到后门事件。
- 从总览进入事件详情和处置动作。

### 使用哪些数据字段

| 数据源 | 字段 | 页面用途 |
| --- | --- | --- |
| `handoff_summary.json` | `total_alerts` | 总告警数 |
| `handoff_summary.json` | `ignored_alerts` | 忽略数量 |
| `handoff_summary.json` | `explained_alerts` | 已解释数量 |
| `handoff_summary.json` | `action_needed` | 待处理数量 |
| `handoff_summary.json` | `summary_text` | 总览摘要 |
| `handoff_summary.json` | `top_risk_event_id` | 点击后定位事件 |
| `handoff_summary.json` | `top_risk_time` | 高风险时间 |
| `handoff_summary.json` | `top_risk_area` | 高风险区域 |

### 空状态 / 失败状态

| 状态 | 页面表现 |
| --- | --- |
| 没有告警 | 显示 `昨晚没有需要复核的告警`，四个指标为 0 |
| 没有待处理事件 | `Action needed` 为 0，主标题改为 `昨晚没有需要优先处理的事件` |
| 摘要加载失败 | 指标仍可从 `reviewed_events.json` 统计，摘要显示 `Summary unavailable` |
| 统计不一致 | 显示数据异常提示，不进入正式演示模式 |

### 验收标准

- 总览数字必须来自数据统计或 `handoff_summary.json`，不能只写死在 HTML 文案里。
- `47 / 44 / 2 / 1` 必须在首屏可见。
- 点击最高风险入口能定位到 `evt_001`。

## 4. 事件列表

### 页面目的

让用户看到 AI 已经把一夜告警分成三类：需要处理、已解释、已忽略。

### 用户看到什么

左侧事件列表按分组展示：

```text
Action needed 1
Explained 2
Ignored 44
```

每条事件显示：

- 事件标题
- 时间
- 摄像头或区域
- 风险标签
- 一句话摘要

### 用户能做什么

- 点击事件，查看详情。
- 优先查看 `Action needed`。
- 展开查看 `Explained` 和 `Ignored` 的代表事件。
- 在问答引用事件时，自动跳转到对应事件。

### 使用哪些数据字段

| 数据源 | 字段 | 页面用途 |
| --- | --- | --- |
| `reviewed_events.json` | `event_id` | 列表项唯一标识 |
| `reviewed_events.json` | `review_result` | 分组依据 |
| `reviewed_events.json` | `risk_level` | 风险标签 |
| `reviewed_events.json` | `local_time` | 事件时间 |
| `reviewed_events.json` | `camera_id` | 摄像头编号 |
| `reviewed_events.json` | `camera_area` | 区域名称 |
| `reviewed_events.json` | `scene_summary` | 列表摘要 |
| `reviewed_events.json` | `handoff_status` | 当前状态 |

### 空状态 / 失败状态

| 状态 | 页面表现 |
| --- | --- |
| 无事件 | 显示 `No night events` |
| 无 Action needed | 该分组显示 0，并隐藏红色风险提示 |
| ignored 太多 | 默认只展示代表事件，保留分组总数 |
| reviewed events 加载失败 | 列表显示错误提示，详情区不可点击 |

### 验收标准

- 分组数量必须和 `reviewed_events.json` 统计一致。
- `Action needed` 分组必须显示后门 02:13 事件。
- 点击任意列表项，中间详情区必须联动。
- ignored 分组允许折叠，但不能丢失 `44` 的总数。

## 5. 事件详情

### 页面目的

解释某条事件为什么被 AI 这样判断，让用户相信结论不是随便给的。

### 用户看到什么

中间详情区包含：

- 事件标题
- 时间
- 摄像头
- 区域
- 风险等级
- 截图或短视频占位
- 事件摘要
- 风险原因
- 误报说明
- 证据点
- 建议动作

高风险事件 `evt_001` 必须显示：

```text
BackGate-01
02:13
后门
High
陌生人在后门停留 38 秒
开门前检查后门门锁，并通知店长复核画面
```

### 用户能做什么

- 查看截图或短视频占位。
- 阅读 AI 的判断理由。
- 查看证据点。
- 根据建议动作进入右侧处置区。
- 切换其他事件并对比不同判断原因。

### 使用哪些数据字段

| 数据源 | 字段 | 页面用途 |
| --- | --- | --- |
| `events.json` | `event_id` | 与复核结果关联 |
| `events.json` | `thumbnail` | 截图占位 |
| `events.json` | `clip` | 短视频占位 |
| `events.json` | `camera_placement` | 摄像头位置说明 |
| `events.json` | `device_state` | 夜视、网络、电量、本地存储状态 |
| `events.json` | `context` | 营业状态、天气、敏感区域 |
| `reviewed_events.json` | `scene_summary` | 事件解释 |
| `reviewed_events.json` | `risk_level` | 风险等级 |
| `reviewed_events.json` | `false_positive_likelihood` | 误报可能 |
| `reviewed_events.json` | `risk_reason` | 风险原因 |
| `reviewed_events.json` | `evidence_points` | 证据点 |
| `reviewed_events.json` | `recommended_action` | 建议动作 |
| `reviewed_events.json` | `needs_human_review` | 是否人工复核 |

### 空状态 / 失败状态

| 状态 | 页面表现 |
| --- | --- |
| 未选择事件 | 默认选中最高风险事件 |
| 事件详情缺失 | 显示 `Event detail unavailable` |
| 截图缺失 | 显示摄像头占位画面和事件文字说明 |
| 复核结果缺失 | 显示 `Needs review`，不展示确定结论 |
| 低光或遮挡 | 显示 `Review needed`，提示人工复核 |

### 验收标准

- 事件详情必须由 `events.json` + `reviewed_events.json` 合并渲染。
- 高风险事件必须展示人工复核提醒。
- ignored 事件必须展示误报依据。
- explained 事件必须展示解释依据。
- 不允许把 `review` 或不确定事件写成确定入侵。

## 6. AI 问答

### 页面目的

让用户用自然问题理解交接结果，但回答范围必须受控，避免编造。

### 用户看到什么

右侧问答区包含：

- 标题：`Ask SentryFlow AI`
- 三个预设问题：
  - `昨晚有什么异常？`
  - `今天先处理哪里？`
  - `哪些是误报？`
- 输入框
- 回答区域

### 用户能做什么

- 点击预设问题查看回答。
- 输入同类问题并按回车。
- 从回答中定位到引用事件。

### 使用哪些数据字段

| 数据源 | 字段 | 页面用途 |
| --- | --- | --- |
| `handoff_summary.json` | `summary_text` | 回答昨晚整体情况 |
| `handoff_summary.json` | `recommended_next_action` | 回答今天先做什么 |
| `handoff_summary.json` | `top_risk_event_id` | 引用高风险事件 |
| `handoff_summary.json` | `ignored_breakdown` | 回答误报类型 |
| `reviewed_events.json` | `scene_summary` | 回答事件发生了什么 |
| `reviewed_events.json` | `risk_reason` | 回答为什么有风险 |
| `reviewed_events.json` | `recommended_action` | 回答下一步动作 |
| `reviewed_events.json` | `needs_human_review` | 回答是否人工复核 |

### 空状态 / 失败状态

| 状态 | 页面表现 |
| --- | --- |
| 没有摘要 | 回答 `当前没有可用交接摘要` |
| 问到数据外内容 | 回答 `当前交接数据中没有这类事件` |
| 问到确定责任 | 回答 `需要人工复核，不能直接判断为入侵` |
| 问答服务失败 | 保留三个固定问题的本地回答 |

### 验收标准

- 三个预设问题必须可用。
- 回答只能基于 `handoff_summary.json` 和 `reviewed_events.json`。
- 高风险回答必须包含 `人工复核` 或 `店长复核`。
- 回答中不能出现数据里不存在的时间、摄像头、事件。

## 7. 处置状态

### 页面目的

把 AI 结论转成用户可执行动作，形成从告警到交接关闭的闭环。

### 用户看到什么

右侧处置区包含：

- 当前优先动作
- 当前状态
- 三个按钮：
  - `通知店长复核`
  - `安排巡检`
  - `标记已复核`
- 动作日志
- 交接摘要

### 用户能做什么

- 通知店长复核后门 02:13 画面。
- 安排早班检查后门门锁。
- 标记事件已复核。
- 关闭今日交接。

### 使用哪些数据字段

| 数据源 | 字段 | 页面用途 |
| --- | --- | --- |
| `actions.json` | `action_id` | 动作唯一标识 |
| `actions.json` | `action_type` | 动作类型 |
| `actions.json` | `label` | 按钮文案 |
| `actions.json` | `actor` | 操作者 |
| `actions.json` | `assignee` | 接收人 |
| `actions.json` | `from_status` | 前置状态 |
| `actions.json` | `to_status` | 点击后状态 |
| `actions.json` | `display_text` | 页面日志 |
| `handoff_summary.json` | `status` | 初始交接状态 |
| `reviewed_events.json` | `recommended_action` | 优先动作 |

### 状态流

```text
action_needed
-> assigned
-> reviewed
-> handoff_closed
```

### 空状态 / 失败状态

| 状态 | 页面表现 |
| --- | --- |
| 没有 action_needed | 处置区显示 `No action needed`，按钮置灰 |
| actions 加载失败 | 使用本地按钮状态，不写回文件 |
| 动作顺序不合法 | 按钮禁用或提示先完成上一步 |
| 已关闭交接 | 显示 `Handoff closed`，今日不再重复提醒 |

### 验收标准

- 点击处置按钮后，状态和日志必须变化。
- 最终必须能显示 `Handoff closed`。
- 刷新页面后可以回到初始演示状态。
- 不需要真实发送短信或消息。

## 8. 技术架构展示

### 页面目的

让评委快速理解：真实 eufy 事件和模拟事件会进入同一套交接流水线。

### 用户看到什么

页面底部展示 5 个节点：

```text
eufy / mock events
-> Event Schema
-> AI Review
-> Risk Engine
-> Handoff + Actions
```

旁边展示 eufy 适配点：

- 运动事件
- 夜视证据
- eufy App
- SDK 可替换

### 用户能做什么

- 不需要复杂操作。
- 演示者可以用这一块回答技术落地问题。
- 后续 React 版本可点击节点查看说明。

### 使用哪些数据字段

| 数据源 | 字段 | 页面用途 |
| --- | --- | --- |
| `events.json` | `source` | 事件来源 |
| `events.json` | `source_note` | mock / SDK 边界说明 |
| `events.json` | `device_state.local_storage` | HomeBase / 本地存储表达 |
| `handoff_summary.json` | `generated_from` | 摘要来源 |
| `docs/16-technical-implementation-plan.md` | Task 6-8 | API / adapter 说明 |

### 空状态 / 失败状态

| 状态 | 页面表现 |
| --- | --- |
| 技术展示隐藏 | 不影响主 Demo |
| 数据 source 缺失 | 显示 `mock_event_stream` 作为默认来源 |
| SDK 未接入 | 明确显示 `SDK adapter reserved` |

### 验收标准

- 架构节点不超过 5 个，评委能 10 秒看懂。
- 必须明确 `SDK 可替换`，不暗示已经接入官方 SDK。
- 技术展示不能抢走 Demo 主视觉。

## 9. 响应式要求

### 桌面端

桌面端使用三栏工作台：

```text
Event List | Event Detail | Handoff / Q&A / Actions
```

要求：

- 首屏能看到总览数字。
- 三栏不互相遮挡。
- 右侧按钮不换行到不可读状态。

### 移动端

移动端改为单列顺序：

```text
Summary
Event List
Event Detail
Handoff / Q&A / Actions
Technical Architecture
```

要求：

- 指标卡上下排列。
- 按钮高度不低于 38px。
- 文案不能溢出卡片。
- 事件列表可滚动或折叠。

## 10. 视觉原则

- 主色：绿色、黑、白、浅灰。
- 风险提示使用少量红色。
- 不使用花哨渐变。
- 不做营销页大 Hero。
- 一屏优先呈现真实产品操作感。
- 卡片用于具体信息块，不做层层嵌套。
- 页面文案不写“建议你点击这里”这类说明口吻。

## 11. 页面验收清单

完成页面设计或开发后，逐项检查：

- 打开页面后能看到 `eufy Morning Handoff`。
- 首屏能看到 `47 / 44 / 2 / 1`。
- 事件列表显示 `Action needed / Explained / Ignored`。
- `Action needed` 指向 `BackGate-01 / 02:13 / 后门`。
- 事件详情展示截图占位、时间、摄像头、区域、风险原因、证据点、建议动作。
- AI 问答支持三个固定问题。
- 处置按钮至少包含通知店长、安排巡检、标记已复核。
- 点击处置按钮后状态变化。
- 最终可以进入 `Handoff closed`。
- 技术架构展示清楚说明 `eufy / mock events -> Event Schema -> AI Review -> Risk Engine -> Handoff + Actions`。
- 页面在桌面和移动端没有文字重叠或按钮溢出。
