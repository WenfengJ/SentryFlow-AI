# SentryFlow AI 产品规格稿

## 1. 产品名称

**SentryFlow AI**

产品形态：

```text
eufy Morning Handoff
```

中文描述：

```text
eufy 摄像头的晨间安防交接。
```

## 2. 一句话介绍

SentryFlow AI 把 eufy 摄像头的一夜告警整理成晨间摘要、事件解释、误报说明、风险原因、建议动作和处置状态，让用户早上只处理真正重要的一件事。

短句版本：

```text
47 条告警 -> 1 个清楚动作
```

## 3. 产品定位

SentryFlow AI 不是重型 CCTV 平台，也不是通用视频分析后台。

它是 eufy 摄像头之上的 AI 交接层：

```text
摄像头负责看见事件；
SentryFlow AI 负责解释事件、排序风险、生成交接、推动处理。
```

当前参赛版本验证一条核心闭环：

```text
夜间告警 -> AI 复核 -> 风险排序 -> 晨间交接 -> 用户问答 -> 处置状态
```

产品目标：

- 让用户不用每天翻几十条 motion alerts。
- 让误报不会淹没真正风险。
- 让 eufy 摄像头从“会报警”升级为“会交接”。

## 4. 核心用户

### 小商铺老板

使用时间：

- 早上开店前。
- 收到晨间交接提醒后。

核心诉求：

- 快速知道昨晚店铺是否安全。
- 只看真正需要处理的事件。
- 必要时通知店长或值班人员复核。

交付结果：

```text
47 条告警里，只有 1 件需要开门前处理。
```

### 仓库管理员

使用时间：

- 早上巡检前。
- 夜班交接前。

核心诉求：

- 知道后门、围栏、装卸区是否有异常。
- 得到巡检优先级。
- 对高风险事件完成分派和复核。

交付结果：

```text
昨晚哪些事件已解释，哪一件需要复核，今天先检查哪里。
```

### eufy App 用户

使用时间：

- 起床后打开 eufy App。
- 出门前查看家庭或小型场所安全状态。

核心诉求：

- 不翻历史告警。
- 一句话知道昨晚是否安全。
- 能确认门口、车库、后院等区域是否需要检查。

交付结果：

```text
eufy App 每天早上自动给出 Daily Security Handoff。
```

## 5. 核心场景

主场景：

```text
闭店后一夜 47 条告警，早上只处理 1 件事。
```

事件分布：

| 类型 | 数量 | 说明 |
| --- | ---: | --- |
| Ignored | 44 | 雨水、车灯、昆虫、普通运动 |
| Explained | 2 | 员工返回取货、正常经过 |
| Action needed | 1 | 后门 02:13 陌生人停留 38 秒 |

核心事件：

```text
02:13，BackGate-01，后门区域，陌生人停留 38 秒。
```

用户最终动作：

```text
开门前检查后门门锁，并通知店长复核 02:13 画面。
```

## 6. 用户流程

### 6.1 晨间交接流程

```text
用户早上打开 eufy App / 网页交接页
-> 看见 47 条告警已复核
-> 看见 1 条需要处理
-> 点开后门 02:13 事件
-> 查看证据、风险原因、建议动作
-> 点击通知店长复核
-> 状态变成 Assigned
```

### 6.2 AI 问答流程

```text
用户问：昨晚有什么异常？
-> AI 基于 reviewed_events 和 handoff_summary 回答
-> AI 指向后门 02:13 事件
-> AI 说明为什么这件事优先级最高
-> AI 给出下一步动作
```

### 6.3 关闭交接流程

```text
店长或用户完成复核
-> 用户点击标记已复核
-> 系统记录 action
-> 交接状态变成 Handoff closed
-> 今日不再重复提醒同一事件
```

### 6.4 用户每天什么时候打开它

- 小商铺老板：开门前 5 分钟。
- 仓库管理员：早班巡检前。
- eufy App 用户：早上查看家庭或小型场所安全状态时。

### 6.5 用户第一眼看到什么

第一屏必须看到：

```text
47 条告警已复核
44 条已忽略
2 条已解释
1 条需要处理
今日优先动作：检查后门门锁，复核 02:13 画面
```

第一屏不展示完整事件流，不要求用户先翻视频。

## 7. 功能模块

### 7.1 Event Adapter

职责：接入真实 eufy 事件或模拟事件流。

当前 Demo：

- 从本地 JSON 读取模拟事件。
- 固定 47 条夜间告警。

赛后接入：

- 接 eufy 运动事件。
- 接事件截图或短视频。
- 接摄像头状态、区域、时间和设备信息。

### 7.2 AI Review

职责：复核单条事件。

输入：

- 截图或短视频。
- 摄像头位置。
- 事件时间。
- 营业时间。
- 是否敏感区域。
- 设备状态。

输出：

- 事件摘要。
- 风险等级。
- 误报可能。
- 证据点。
- 风险原因。
- 建议动作。
- 是否需要人工复核。

### 7.3 Risk Engine

职责：把多条事件排序和降噪。

判断规则：

```text
营业外时间 + 后门区域 + 人员停留超过 30 秒 = high
雨水/反光/昆虫 + 无人员进入 = ignored
员工返回取货 = explained
低光/遮挡导致无法判断 = review
```

输出分类：

```text
Ignored / Explained / Action needed
```

### 7.4 Handoff Generator

职责：生成晨间交接摘要。

必须回答：

- 昨晚是否安全。
- 总共多少告警。
- 哪些不用管。
- 哪些已解释。
- 哪一件必须处理。
- 今天先做什么。

### 7.5 Q&A Service

职责：回答用户关于昨晚事件的问题。

支持问题：

- 昨晚有什么异常？
- 今天先处理哪里？
- 哪些是误报？

回答边界：

- 只基于当前 reviewed events 和 handoff summary。
- 不编造不存在的事件。
- 不做绝对安全承诺。
- 高风险事件必须建议人工复核。

### 7.6 Action Service

职责：记录用户处置动作和状态变化。

支持动作：

- 通知店长复核。
- 安排巡检。
- 标记已复核。
- 关闭交接。

状态流转：

```text
Action needed -> Assigned -> Reviewed -> Handoff closed
```

## 8. 页面结构

### 8.1 晨间交接总览

用户看到：

- 47 条告警已复核。
- 44 条已忽略。
- 2 条已解释。
- 1 条需要处理。
- 今日优先动作。

用户能做：

- 点开高风险事件。
- 查看全部事件分类。
- 进入 AI 问答。

使用数据：

- `handoff_summary.total_alerts`
- `handoff_summary.ignored_alerts`
- `handoff_summary.explained_alerts`
- `handoff_summary.action_needed`
- `handoff_summary.recommended_next_action`

### 8.2 事件列表

用户看到：

- Action needed 事件。
- Explained 事件。
- Ignored 事件。
- 每条事件的时间、摄像头、区域、风险等级。

用户能做：

- 切换事件。
- 过滤事件状态。
- 点开事件详情。

使用数据：

- `events.json`
- `reviewed_events.json`

### 8.3 事件详情

用户看到：

- 截图或视频片段。
- AI 看到什么。
- 风险原因。
- 证据点。
- 误报可能。
- 建议动作。

用户能做：

- 通知店长。
- 安排巡检。
- 标记已复核。
- 回到总览。

使用数据：

- `event_id`
- `camera_id`
- `camera_area`
- `timestamp`
- `scene_summary`
- `risk_reason`
- `evidence_points`
- `recommended_action`

### 8.4 AI 问答

用户看到：

- 预设问题。
- AI 回答。
- 回答引用的事件。

用户能做：

- 点击预设问题。
- 输入短问题。
- 从回答跳转到事件详情。

使用数据：

- `handoff_summary.json`
- `reviewed_events.json`

### 8.5 处置状态

用户看到：

- 当前事件状态。
- 已执行动作。
- 负责人或备注。
- 状态更新时间。

用户能做：

- Assigned。
- Reviewed。
- Handoff closed。

使用数据：

- `actions.json`

## 9. 数据输入

### 9.1 RawEvent

```json
{
  "event_id": "evt_001",
  "source": "simulated_eufy_event",
  "site_id": "site_store_001",
  "site_type": "small_store",
  "camera_id": "BackGate-01",
  "camera_area": "back_gate",
  "timestamp": "2026-10-16T02:13:00+08:00",
  "trigger": "motion_detected",
  "media": {
    "snapshot_url": "/assets/samples/back_gate_intrusion.jpg",
    "clip_url": null
  },
  "device_state": {
    "online": true,
    "battery": 78,
    "network": "4G",
    "night_vision": true,
    "privacy_zone_enabled": true
  },
  "context": {
    "business_hours": "09:00-21:00",
    "is_after_hours": true,
    "sensitive_area": true
  }
}
```

### 9.2 ReviewedEvent

```json
{
  "event_id": "evt_001",
  "event_type": "after_hours_person_stay",
  "risk_level": "high",
  "confidence": 0.86,
  "false_positive_likelihood": "low",
  "scene_summary": "A person stayed near the back gate after closing time.",
  "risk_reason": "The event happened after business hours in a sensitive back-gate area, and the person remained for 38 seconds.",
  "evidence_points": [
    "after business hours",
    "person visible near back gate",
    "sensitive area",
    "stayed longer than normal passing motion"
  ],
  "recommended_action": [
    "Check the back gate lock before opening",
    "Ask the store manager to review the 02:13 clip"
  ],
  "needs_human_review": true,
  "handoff_status": "action_needed"
}
```

### 9.3 HandoffSummary

```json
{
  "handoff_id": "handoff_2026_10_17_store_001",
  "site_id": "site_store_001",
  "total_alerts": 47,
  "ignored_alerts": 44,
  "explained_alerts": 2,
  "action_needed": 1,
  "top_risk_event_id": "evt_001",
  "summary_text": "昨晚整体安全。47 条告警已复核，44 条忽略，2 条已解释，1 条需要处理。",
  "recommended_next_action": "开门前检查后门门锁，并请店长复核 02:13 的后门画面。",
  "status": "action_needed"
}
```

### 9.4 Action

```json
{
  "action_id": "act_001",
  "event_id": "evt_001",
  "action_type": "assign_review",
  "assignee": "store_manager",
  "note": "请复核 02:13 后门画面，并在开门前检查门锁。",
  "status": "assigned",
  "created_at": "2026-10-17T08:20:00+08:00"
}
```

## 10. AI 输出

### 10.1 晨间摘要输出

```text
昨晚整体安全。47 条告警已复核，44 条忽略，2 条已解释，1 条需要处理。
优先处理：02:13 后门陌生人停留。
```

### 10.2 事件解释输出

```text
02:13，BackGate-01 捕捉到一名陌生人在后门区域停留约 38 秒。
```

### 10.3 误报说明输出

```text
雨水、车灯反光、昆虫靠近镜头和普通路过没有进入待处理清单。
```

### 10.4 风险原因输出

```text
事件发生在闭店后，位置是后门敏感区域，画面中人员停留时间超过普通经过。
```

### 10.5 建议动作输出

```text
开门前检查后门门锁，并请店长复核 02:13 的后门画面。
```

### 10.6 问答输出

问题：

```text
今天先处理哪里？
```

回答：

```text
先处理后门。02:13 BackGate-01 捕捉到陌生人停留 38 秒，建议开门前检查后门门锁，并请店长复核这段画面。
```

## 11. 处置闭环

### 11.1 用户可点击动作

| 动作 | 作用 | 状态变化 |
| --- | --- | --- |
| 通知店长复核 | 把最高风险事件派给店长 | Action needed -> Assigned |
| 安排巡检 | 生成现场检查任务 | Action needed -> Assigned |
| 标记已复核 | 表示用户已确认事件 | Assigned -> Reviewed |
| 关闭交接 | 今日交接完成 | Reviewed -> Handoff closed |

### 11.2 状态定义

| 状态 | 含义 |
| --- | --- |
| Action needed | 有事件需要用户处理 |
| Assigned | 已通知负责人或安排巡检 |
| Reviewed | 已人工复核 |
| Handoff closed | 今日交接关闭 |

### 11.3 动作记录

每次动作必须记录：

- `action_id`
- `event_id`
- `action_type`
- `assignee`
- `note`
- `status`
- `created_at`

## 12. 技术边界

当前 Demo 不承诺：

- 已接入官方 eufy SDK。
- 已处理真实生产视频流。
- 已实现多门店、多角色权限。
- 已实现真实 App 推送或短信发送。
- 已实现实时多摄像头调度。

当前 Demo 必须做到：

- 模拟事件流可稳定运行。
- 事件数据结构可替换为真实 eufy 事件。
- 47 / 44 / 2 / 1 可从数据统计出来。
- 问答基于固定 reviewed events 和 handoff summary。
- 处置状态可展示变化。

## 13. 隐私边界

原则：

- 只处理用户授权的安防事件。
- 不做无边界实时监控。
- 不把 AI 输出当作最终安全裁决。
- 高风险或不确定事件必须人工复核。
- 原始图片和视频未来应优先本地处理或最小化保存。

产品表达边界：

- 可以说“疑似异常”或“需要复核”。
- 不说“确定入侵”。
- 不说“系统保证安全”。
- 不替代用户、店长或安保责任人。

## 14. 可演示范围

### Demo 已有

- 静态 HTML 原型：`prototype/handoff-agent.html`。
- 晨间总览：47 / 44 / 2 / 1。
- 高风险事件：后门 02:13 陌生人停留。
- 风险原因和建议动作。
- AI 问答展示。
- 处置按钮和状态变化展示。
- Pitch Deck PDF / PPTX。
- Demo Guide、路演脚本、答辩 Q&A。

### Demo 待补

- `app/data/events.json`：47 条事件。
- `app/data/reviewed_events.json`：复核结果。
- `app/data/handoff_summary.json`：晨间摘要。
- `app/data/actions.json`：动作状态。
- API / 数据契约。
- Demo 视频脚本和录屏。

### 赛后接入

- 真实 eufy SDK / API。
- 真实事件截图或短视频。
- 真实 App 通知。
- 多摄像头事件合并。
- 多门店、多用户、多角色权限。

## 15. 赛后产品化方向

### eufy App 内功能

把 Morning Handoff 做成 eufy App 首页卡片：

```text
Daily Security Handoff
昨晚整体安全。1 件事需要处理。
```

用户点击卡片后进入事件详情和处置页面。

### HomeBase / 本地隐私策略

结合 HomeBase 和本地存储，把敏感事件优先在本地复核，只输出必要摘要和授权证据。

### 多摄像头事件合并

把同一时间段、同一区域的多个摄像头事件合并为一个事件，减少重复提醒。

### 安装套餐增值

面向小商铺和小仓库，把 eufy 摄像头套餐升级为：

```text
摄像头 + 本地存储 + 远程查看 + AI 晨间交接
```

### 长期能力

- 每周安全报告。
- 多场所交接。
- 角色分工和责任记录。
- 误报学习和个性化规则。
- 与灯光、警报、门锁等设备联动。
