# SentryFlow AI 用户流程规格

## 1. 流程目标

这份文档把 SentryFlow AI 的核心产品路径写成可实现的用户流程。每条流程都必须能映射到页面区域、按钮、数据文件和 API。

核心 Demo 场景固定为：

```text
闭店后一夜 47 条告警，早上只处理 1 件事。
```

核心事件固定为：

```text
02:13，BackGate-01，后门区域，陌生人停留 38 秒。
```

## 2. 页面与接口命名

### 2.1 页面区域

| 页面区域 | 说明 | 当前原型位置 |
| --- | --- | --- |
| Morning Summary | 晨间交接总览 | 顶部统计区 |
| Event Queue | 事件列表 | 左侧事件列表 |
| Event Detail | 事件证据与解释 | 中间证据区 |
| Handoff Panel | 建议动作与通知 | 右侧交接区 |
| Q&A Panel | 用户问答 | 右侧问答区 |
| Action Status | 处置状态 | 通知预览和状态文本 |

### 2.2 按钮和动作

| 用户动作 | 当前原型按钮 | 产品动作名 | 目标状态 |
| --- | --- | --- | --- |
| 发送给负责人 | `Send to manager` | `assign_review` | `Assigned` |
| 通知店长复核 | 待补按钮文案 | `notify_manager` | `Assigned` |
| 安排巡检 | 待补按钮文案 | `schedule_check` | `Assigned` |
| 标记已复核 | `Mark handoff closed` | `mark_reviewed` | `Reviewed` |
| 关闭交接 | `Mark handoff closed` | `close_handoff` | `Handoff closed` |

### 2.3 数据文件

| 数据文件 | 用途 |
| --- | --- |
| `app/data/events.json` | 原始 47 条事件 |
| `app/data/reviewed_events.json` | AI 复核结果 |
| `app/data/handoff_summary.json` | 晨间交接摘要 |
| `app/data/actions.json` | 用户处置动作 |

### 2.4 API 映射

| API | 用途 |
| --- | --- |
| `GET /api/handoff/today` | 读取晨间交接总览 |
| `GET /api/events` | 读取事件列表 |
| `GET /api/events/{event_id}` | 读取事件详情 |
| `POST /api/qa` | 提交问题并返回回答 |
| `POST /api/actions` | 记录处置动作和状态变化 |

当前没有后端时，前端可以直接读取本地 JSON 或使用页面内预置数据模拟这些 API。

---

## 3. 流程 A：晨间交接

### 3.1 用户路径

```text
用户打开 eufy App / 网页交接页
-> 看见 47 条告警已复核
-> 看见 1 条需要处理
-> 点开后门事件
-> 查看证据、原因、建议动作
-> 点击通知店长复核
-> 状态变成 Assigned
```

### 3.2 页面步骤

| 步骤 | 用户动作 | 页面反馈 | 数据来源 | API |
| --- | --- | --- | --- | --- |
| A1 | 打开交接页 | 显示 47 / 44 / 2 / 1 总览 | `handoff_summary.json` | `GET /api/handoff/today` |
| A2 | 查看待处理事件 | Action needed 区域显示后门 02:13 | `reviewed_events.json` | `GET /api/events` |
| A3 | 点击后门事件 | 中间展示截图、风险原因、证据点、建议动作 | `events.json` + `reviewed_events.json` | `GET /api/events/{event_id}` |
| A4 | 点击通知店长复核 | 右侧通知预览生成发送内容 | `handoff_summary.json` + event detail | `POST /api/actions` |
| A5 | 系统记录动作 | 状态从 `Action needed` 变成 `Assigned` | `actions.json` | `POST /api/actions` |

### 3.3 第一屏展示要求

第一屏必须展示：

```text
47 条告警已复核
44 条已忽略
2 条已解释
1 条需要处理
今日优先动作：检查后门门锁，复核 02:13 画面
```

第一屏不展示完整 47 条事件，不要求用户先翻视频。

### 3.4 后门事件详情要求

事件详情必须展示：

```text
事件 ID：evt_001
摄像头：BackGate-01
区域：后门
时间：02:13
事件：陌生人停留 38 秒
风险等级：High
风险原因：营业外时间 + 后门敏感区域 + 停留时间较长
建议动作：开门前检查后门门锁，并通知店长复核画面
```

### 3.5 通知店长动作要求

点击 `通知店长复核` 后，系统生成动作记录：

```json
{
  "action_type": "notify_manager",
  "event_id": "evt_001",
  "assignee": "store_manager",
  "note": "请复核 02:13 后门画面，并在开门前检查门锁。",
  "status": "assigned"
}
```

页面状态更新：

```text
Action needed -> Assigned
```

### 3.6 验收标准

- 打开页面后能看到 `47 / 44 / 2 / 1`。
- 用户能点开后门 02:13 事件。
- 用户能看到证据、风险原因和建议动作。
- 用户能点击通知店长复核。
- 点击后状态变成 `Assigned`。

---

## 4. 流程 B：AI 问答

### 4.1 用户路径

```text
用户问：昨晚有什么异常？
-> AI 只基于 reviewed_events 和 handoff_summary 回答
-> AI 指向 02:13 后门事件
-> AI 给出下一步动作
```

### 4.2 页面步骤

| 步骤 | 用户动作 | 页面反馈 | 数据来源 | API |
| --- | --- | --- | --- | --- |
| B1 | 点击或输入“昨晚有什么异常？” | Q&A Panel 显示加载状态 | 用户输入 | `POST /api/qa` |
| B2 | 系统检索交接摘要 | 找到 1 条 action needed | `handoff_summary.json` | `POST /api/qa` |
| B3 | 系统检索事件详情 | 找到 `evt_001` 后门事件 | `reviewed_events.json` | `POST /api/qa` |
| B4 | AI 返回回答 | 回答指出 02:13 后门事件和建议动作 | summary + reviewed events | `POST /api/qa` |
| B5 | 用户点击回答引用 | 页面跳转到后门事件详情 | `event_id` | `GET /api/events/{event_id}` |

### 4.3 问答输入

必须支持 3 个预设问题：

```text
昨晚有什么异常？
今天先处理哪里？
哪些是误报？
```

### 4.4 问答输出样例

问题：

```text
昨晚有什么异常？
```

回答：

```text
昨晚只有 1 件需要优先处理：02:13，BackGate-01 后门区域出现陌生人停留 38 秒。
其余 44 条已判断为雨水、车灯、昆虫或普通低风险运动，2 条已解释为员工活动。
建议开门前先检查后门门锁，并请店长复核这段画面。
```

问题：

```text
今天先处理哪里？
```

回答：

```text
先处理后门。原因是事件发生在营业外时间，位置是后门敏感区域，人员停留时间超过普通经过。
下一步是检查后门门锁，并复核 02:13 的 BackGate-01 画面。
```

问题：

```text
哪些是误报？
```

回答：

```text
44 条事件已归为 ignored，主要是雨水、车灯反光、昆虫靠近镜头和普通路过。
这些事件没有进入待处理清单。
```

### 4.5 AI 回答边界

AI 回答必须遵守：

- 只能基于 `reviewed_events.json` 和 `handoff_summary.json`。
- 不编造不存在的摄像头、事件或人员身份。
- 不说“确定入侵”。
- 不说“系统保证安全”。
- 高风险事件必须提醒人工复核。

### 4.6 POST `/api/qa` 请求样例

```json
{
  "handoff_id": "handoff_2026_10_17_store_001",
  "question": "昨晚有什么异常？"
}
```

### 4.7 POST `/api/qa` 返回样例

```json
{
  "answer": "昨晚只有 1 件需要优先处理：02:13，BackGate-01 后门区域出现陌生人停留 38 秒。建议开门前检查后门门锁，并请店长复核这段画面。",
  "referenced_event_ids": ["evt_001"],
  "recommended_action": "check_back_gate_lock_and_review_clip"
}
```

### 4.8 验收标准

- 用户能输入或点击“昨晚有什么异常？”。
- 回答必须指向后门 02:13 事件。
- 回答必须给出下一步动作。
- 回答里不能出现不存在的事件。
- 回答中的事件引用能跳转到事件详情。

---

## 5. 流程 C：关闭交接

### 5.1 用户路径

```text
用户完成复核
-> 点击标记已复核
-> 系统记录 action
-> 交接状态变成 Handoff closed
-> 今日不再重复提醒
```

### 5.2 页面步骤

| 步骤 | 用户动作 | 页面反馈 | 数据来源 | API |
| --- | --- | --- | --- | --- |
| C1 | 用户确认后门已检查 | 处置面板显示可关闭交接 | `actions.json` | `GET /api/handoff/today` |
| C2 | 点击标记已复核 | 状态从 `Assigned` 变成 `Reviewed` | 用户动作 | `POST /api/actions` |
| C3 | 点击关闭交接 | 状态从 `Reviewed` 变成 `Handoff closed` | 用户动作 | `POST /api/actions` |
| C4 | 系统记录 action | actions 增加关闭记录 | `actions.json` | `POST /api/actions` |
| C5 | 页面刷新交接摘要 | 今日不再显示重复提醒 | `handoff_summary.json` + `actions.json` | `GET /api/handoff/today` |

### 5.3 关闭动作记录样例

```json
{
  "action_type": "close_handoff",
  "event_id": "evt_001",
  "assignee": "owner",
  "note": "后门门锁已检查，店长已复核 02:13 画面。",
  "status": "handoff_closed"
}
```

### 5.4 页面关闭后的展示

关闭后，晨间交接总览显示：

```text
Handoff closed
今日交接已完成。
后门 02:13 事件已复核，不再重复提醒。
```

事件详情显示：

```text
状态：Handoff closed
处置记录：已通知店长复核，后门门锁已检查。
```

### 5.5 今日不再重复提醒规则

同一个 `handoff_id` 下，如果最高风险事件状态为 `handoff_closed`：

- 总览页不再显示红色 Action needed 状态。
- 事件仍保留在历史记录中。
- Q&A 再次询问时，需要说明事件已处理。

示例回答：

```text
今天的后门事件已完成复核。当前交接状态是 Handoff closed，不需要重复处理。
```

### 5.6 验收标准

- 用户能点击标记已复核。
- 系统能记录 action。
- 状态能变成 `Handoff closed`。
- 总览页不再重复提示同一事件。
- Q&A 能识别该事件已处理。

---

## 6. 状态机

```text
Action needed
  -> Assigned
  -> Reviewed
  -> Handoff closed
```

状态含义：

| 状态 | 触发动作 | 页面含义 |
| --- | --- | --- |
| Action needed | AI 复核后发现待处理事件 | 用户必须先看这条事件 |
| Assigned | 通知店长或安排巡检 | 已有人负责处理 |
| Reviewed | 人工完成复核 | 事件已确认 |
| Handoff closed | 关闭今日交接 | 今日不再重复提醒 |

## 7. 前端实现拆分

### 7.1 必做组件

| 组件 | 对应流程 | 数据 |
| --- | --- | --- |
| SummaryMetrics | A | `handoff_summary.json` |
| EventQueue | A / B | `reviewed_events.json` |
| EventDetail | A / B | `events.json` + `reviewed_events.json` |
| HandoffPanel | A / C | `handoff_summary.json` + `actions.json` |
| QaPanel | B | `handoff_summary.json` + `reviewed_events.json` |
| ActionStatus | A / C | `actions.json` |

### 7.2 必做交互

- 点击事件列表，更新事件详情。
- 点击通知店长，写入 assigned 状态。
- 点击标记已复核，写入 reviewed 状态。
- 点击关闭交接，写入 handoff_closed 状态。
- 点击预设问题，展示固定问答结果。
- 点击问答引用事件，跳转事件详情。

## 8. 后端接口拆分

### 8.1 `GET /api/handoff/today`

返回：

- 今日交接摘要。
- 汇总数字。
- 当前最高风险事件。
- 当前交接状态。

### 8.2 `GET /api/events`

返回：

- 事件列表。
- 分类结果。
- 风险等级。
- 状态。

### 8.3 `GET /api/events/{event_id}`

返回：

- 原始事件。
- AI 复核结果。
- 证据点。
- 建议动作。
- 当前处置状态。

### 8.4 `POST /api/qa`

输入：

- `handoff_id`
- `question`

返回：

- `answer`
- `referenced_event_ids`
- `recommended_action`

### 8.5 `POST /api/actions`

输入：

- `event_id`
- `action_type`
- `assignee`
- `note`

返回：

- `action_id`
- `status`
- `created_at`
- 更新后的事件状态。

## 9. Demo 验收清单

- [ ] 打开页面后看到 `47 / 44 / 2 / 1`。
- [ ] 后门 02:13 事件在 Action needed 中置顶。
- [ ] 点击后门事件能看到证据、原因、建议动作。
- [ ] 点击通知店长复核，状态变成 `Assigned`。
- [ ] 输入“昨晚有什么异常？”，AI 回答指向后门事件。
- [ ] 输入“今天先处理哪里？”，AI 回答给出后门门锁和 02:13 复核动作。
- [ ] 点击标记已复核，系统记录 action。
- [ ] 点击关闭交接，状态变成 `Handoff closed`。
- [ ] 关闭后今日不再重复提醒同一事件。
