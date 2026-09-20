# 15 AI Review Rules

这份文档回答一个问题：SentryFlow AI 如何把一夜 47 条 eufy 摄像头告警，复核成 `44 ignored / 2 explained / 1 action_needed`。

AI 在这里不是最终裁决者。它负责阅读事件证据、解释风险、降低误报、生成交接结论；高风险或不确定事件必须交给人复核。

## 1. 判断对象

AI 每次只复核一条摄像头事件。事件来自 eufy 运动告警或 Demo 模拟事件流，进入系统后统一成 Event Schema。

当前 Demo 数据位置：

- `app/data/events.json`：原始事件。
- `app/data/reviewed_events.json`：复核结果。
- `app/data/handoff_summary.json`：交接摘要。
- `app/data/actions.json`：用户处置动作。

## 2. AI 输入

每条事件进入 AI Review 时，输入包含以下信息。

| 输入 | 含义 | 示例 |
| --- | --- | --- |
| 事件截图或短视频 | 用来判断画面里是否有人、车辆、雨水、昆虫、反光、遮挡 | `thumbnail` / `clip` |
| 摄像头位置 | 判断事件发生在前门、后门、停车区还是店内 | `BackGate-01` / `后门` |
| 事件时间 | 判断是否发生在营业外时间 | `02:13` |
| 营业时间 | 判断是否需要按闭店风险处理 | `09:00-21:00` |
| 是否敏感区域 | 后门、门锁、仓库入口等区域风险更高 | `sensitive_area: true` |
| 设备状态 | 判断夜视、网络、电量、遮挡等是否影响可信度 | `night_vision: true` / `signal: good` |

Demo 中 `evt_001` 的关键输入：

```json
{
  "event_id": "evt_001",
  "local_time": "02:13",
  "camera_id": "BackGate-01",
  "camera_area": "后门",
  "event_type": "after_hours_person_lingering",
  "motion_label": "person",
  "duration_seconds": 38,
  "context": {
    "is_after_hours": true,
    "sensitive_area": true,
    "store_status": "closed"
  }
}
```

## 3. AI 输出

AI Review 对每条事件输出结构化结果，供事件列表、详情页、问答和晨间摘要复用。

| 输出 | 含义 | 示例 |
| --- | --- | --- |
| 事件摘要 | 用一句话说明发生了什么 | `02:13 后门出现陌生人停留 38 秒` |
| 风险等级 | `high` / `medium` / `low` / `unknown` | `high` |
| 误报可能 | `high` / `medium` / `low` | `low` |
| 证据点 | 支撑判断的事实列表 | `营业外时间`、`后门敏感区域` |
| 风险原因 | 为什么需要处理或为什么可以忽略 | `闭店后后门区域停留超过普通经过` |
| 建议动作 | 用户接下来可以点击或执行的动作 | `检查后门门锁`、`通知店长复核` |
| 是否需要人工复核 | 高风险或不确定时为 true | `needs_human_review: true` |
| 交接状态 | 进入哪一类队列 | `ignored` / `explained` / `action_needed` / `review` |

`evt_001` 的输出：

```json
{
  "event_id": "evt_001",
  "review_result": "action_needed",
  "risk_level": "high",
  "false_positive_likelihood": "low",
  "scene_summary": "02:13 后门出现陌生人停留 38 秒，未见进入画面。",
  "risk_reason": "事件发生在闭店后，位置是后门敏感区域，人员停留时间超过普通经过。",
  "evidence_points": [
    "营业外时间",
    "后门敏感区域",
    "人员停留 38 秒",
    "不像雨水、昆虫或车灯",
    "未匹配已解释员工事件"
  ],
  "recommended_action": [
    "开门前检查后门门锁",
    "通知店长复核 02:13 的 BackGate-01 画面"
  ],
  "needs_human_review": true,
  "handoff_status": "action_needed"
}
```

## 4. 分类结果

AI Review 的结果进入四类队列。

| 分类 | 页面含义 | 用户动作 |
| --- | --- | --- |
| `ignored` | 低价值噪声或误报 | 不进入今日待办 |
| `explained` | 事件真实发生，但原因清楚 | 归档，不升级 |
| `action_needed` | 需要早上优先处理 | 通知、巡检、复核 |
| `review` | 信息不足或画面不清 | 标记人工复核 |

当前 Demo 的统计结果：

```text
47 total alerts
44 ignored
2 explained
1 action_needed
0 review
```

`review` 是产品规则保留项。当前演示数据中没有进入 `review` 的事件，因为主线需要突出 `47 -> 1`。

## 5. 风险规则

规则层不替代 AI 视觉理解，而是把 AI 看到的内容和业务上下文合并判断。

### 5.1 升级为 high / action_needed

命中以下组合时，事件进入 `action_needed`。

```text
营业外时间 + 敏感区域 + 人员停留超过 30 秒 = high
```

当前 Demo 对应：

```text
02:13 + 后门 + 陌生人停留 38 秒 = action_needed
```

判定依据：

- 时间在闭店后。
- 区域是后门，属于敏感区域。
- 目标是人，不是光线或昆虫。
- 停留时间超过普通经过。
- 没有匹配到员工返回取货等解释。

输出动作：

- 开门前检查后门门锁。
- 通知店长复核 02:13 画面。
- 保留人工确认，不直接说已经入侵。

### 5.2 降级为 ignored

命中以下组合时，事件进入 `ignored`。

```text
雨水 / 反光 / 昆虫 / 车灯 / 树影 / 普通路过 + 无人员进入 = ignored
```

典型判断：

| 类型 | 判断理由 | 输出 |
| --- | --- | --- |
| 雨水反光 | 画面亮度变化，无人员轮廓 | `ignored` |
| 昆虫靠近镜头 | 运动贴近镜头，持续时间短 | `ignored` |
| 车灯扫过 | 光线从画面边缘经过，无人靠近 | `ignored` |
| 普通路过 | 人在公共区域经过，没有停留 | `ignored` |
| 招牌反光 | 固定外立面反光，没有实体目标 | `ignored` |
| 树影移动 | 阴影随风变化，不是人或车辆 | `ignored` |

输出动作：

- 不进入今日待办。
- 可在问答中说明为什么是误报或低价值事件。

### 5.3 归类为 explained

命中以下组合时，事件进入 `explained`。

```text
已知身份 / 正常返回取货 / 配送车辆正常经过 = explained
```

当前 Demo 的 2 条 explained：

| 事件 | 时间 | 判断 |
| --- | --- | --- |
| 员工返回取货 | 23:18 | 已知员工，行为与备注一致 |
| 配送车辆经过 | 00:41 | 车辆在道路侧经过，没有停留或靠近门锁 |

输出动作：

- 归档为已解释事件。
- 不进入 `Action needed`。
- 在晨间摘要里占用一行说明，但不打扰用户处理。

### 5.4 标记为 review

命中以下组合时，事件进入 `review`。

```text
低光 / 遮挡 / 设备状态异常 + 无法确认是否有人进入 = review
```

`review` 的含义不是高风险，也不是忽略，而是证据不足。

可能原因：

- 夜视画面过暗。
- 镜头被局部遮挡。
- 网络或电量导致片段不完整。
- 人或物体只出现局部轮廓，无法判断身份和行为。

输出动作：

- 提醒用户人工复核。
- 不编造画面里没有出现的细节。
- 不直接升级为入侵。

## 6. 当前 Demo 的判断链路

```text
raw eufy / mock motion event
-> Event Schema
-> AI 识别画面内容
-> 结合时间、区域、营业状态、设备状态
-> Risk Engine 分类
-> reviewed_events.json
-> handoff_summary.json
-> Q&A + Actions
```

以 `evt_001` 为例：

```text
BackGate-01 捕捉到人员移动
-> 发生在 02:13，店铺已闭店
-> 区域是后门，属于敏感区域
-> 人员停留 38 秒，不像普通经过
-> 不符合雨水、反光、昆虫、车灯特征
-> 输出 high / action_needed / needs_human_review=true
```

以雨水反光事件为例：

```text
Parking-01 捕捉到画面亮度变化
-> 天气为 light rain
-> 无人员轮廓
-> 无门窗接触
-> 输出 low / ignored / needs_human_review=false
```

## 7. 问答边界

问答只基于当前交接数据回答，不能扩大到数据之外。

可使用的数据：

- `reviewed_events.json`
- `handoff_summary.json`
- 当前用户点击的事件详情
- 已记录的 `actions.json`

禁止回答：

- 编造没有发生的事件。
- 编造摄像头没有提供的画面细节。
- 把 `可能风险` 说成 `一定入侵`。
- 把误报判断说成 100% 绝对正确。
- 替用户做最终安保责任判断。
- 声称已经接入真实 eufy 官方 SDK。

必须回答：

- 高风险事件必须提醒人工复核。
- 不确定事件必须说明证据不足。
- 问到误报时，必须给出误报依据。
- 问到优先处理时，必须指向具体事件、时间、摄像头和动作。

## 8. Demo 问答口径

### 昨晚有什么异常？

回答应包含：

```text
昨晚只有 1 件需要优先处理：02:13，BackGate-01 后门区域出现陌生人停留 38 秒。其余 44 条已判断为雨水、车灯、昆虫、树影、招牌反光或普通低风险运动，2 条已解释为员工返回取货和配送车辆经过。
```

### 今天先处理哪里？

回答应包含：

```text
先处理后门。原因是事件发生在营业外时间，位置是后门敏感区域，人员停留时间超过普通经过。开门前检查后门门锁，并请店长复核 02:13 的 BackGate-01 画面。
```

### 哪些是误报？

回答应包含：

```text
44 条事件已归为 ignored，主要来自雨水反光、昆虫靠近镜头、车灯扫过、普通路过、低光遮挡、招牌反光和树影移动。这些事件没有进入今日待处理清单。
```

## 9. 评委追问回答

如果评委问：“AI 到底怎么判断？”

直接回答：

```text
我们不是只做目标识别，而是把画面内容和业务上下文一起判断。每条事件会输入截图或短视频、摄像头位置、事件时间、营业时间、敏感区域和设备状态。AI 先解释画面发生了什么，再由风险规则判断是 ignored、explained、action_needed 还是 review。

比如后门 02:13 事件，因为它同时满足营业外时间、后门敏感区域、人员停留超过 30 秒，所以进入 high / action_needed，并要求人工复核。雨水、反光、昆虫、车灯这类没有人员进入的事件会进入 ignored。员工返回取货这类有合理解释的事件进入 explained。
```

## 10. 实现边界

当前黑客松 Demo 使用固定数据和可解释规则展示完整闭环。

已经具备：

- 47 条模拟 eufy motion events。
- 每条事件的复核结论。
- 44 / 2 / 1 可统计结果。
- 高风险事件的证据点、风险原因、建议动作。
- 基于交接数据的问答边界。

未承诺：

- 未接入真实 eufy 官方 SDK。
- 未读取真实用户摄像头画面。
- 未做生产级安防判定。
- 未把 AI 输出作为最终安全裁决。
