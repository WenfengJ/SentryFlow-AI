# 16 Technical Implementation Plan

这份文档把 SentryFlow AI 的工程实现拆成可独立完成的任务。当前优先级很清楚：先完成 Task 1-5，支撑稳定现场 Demo；Task 6-8 用于后续工程化和真实接入。

## 0. 实现目标

现场 Demo 需要跑通这条链路：

```text
app/data/*.json
-> 前端读取数据
-> 展示 47 / 44 / 2 / 1 总览
-> 点击后门 02:13 事件
-> 展示证据、风险原因、建议动作
-> 问答区基于固定数据回答
-> 点击处置按钮更新状态
```

完成 Task 1-5 后，即使没有后端、没有网络、没有 eufy SDK，也能完成 3 分钟演示。

## 1. 实现顺序

| 阶段 | Task | 结果 |
| --- | --- | --- |
| Demo 必需 | Task 1-5 | 静态 HTML 可执行 Demo |
| 工程增强 | Task 6 | 本地 API 封装 |
| 真实接入预留 | Task 7 | eufy SDK adapter 边界 |
| AI 增强预留 | Task 8 | 模型调用 adapter 边界 |

## Task 1：整理 JSON 数据

### 目标

把 Demo 所需数据固定下来，让 `47 -> 44 / 2 / 1` 能从数据统计出来。

### 输入

- `docs/12-product-spec.md`
- `docs/13-user-flow.md`
- `docs/15-ai-review-rules.md`
- `prototype/handoff-agent.html` 当前演示内容

### 输出文件

- `app/data/events.json`
- `app/data/reviewed_events.json`
- `app/data/handoff_summary.json`
- `app/data/actions.json`
- `app/data/README.md`

### 数据要求

- `events.json` 包含 47 条原始事件。
- `reviewed_events.json` 包含 47 条复核结果。
- `handoff_summary.json` 中的数字必须来自 reviewed events 的统计结果。
- `actions.json` 包含处置链路样例。
- 主风险事件固定为：`evt_001 / BackGate-01 / 02:13 / 后门 / 人员停留 38 秒`。

### 验收标准

运行统计后得到：

```text
47 total
44 ignored
2 explained
1 action_needed
```

校验命令：

```bash
python3 - <<'PY'
import json
from collections import Counter
from pathlib import Path
base = Path('app/data')
events = json.loads((base / 'events.json').read_text())
reviewed = json.loads((base / 'reviewed_events.json').read_text())
summary = json.loads((base / 'handoff_summary.json').read_text())
counts = Counter(x['review_result'] for x in reviewed)
assert len(events) == 47
assert len(reviewed) == 47
assert counts['ignored'] == summary['ignored_alerts'] == 44
assert counts['explained'] == summary['explained_alerts'] == 2
assert counts['action_needed'] == summary['action_needed'] == 1
print('pass')
PY
```

### 当前状态

已完成。

## Task 2：前端读取数据并渲染总览

### 目标

让 `prototype/handoff-agent.html` 不再依赖页面内写死的总览数字，而是从 `app/data/handoff_summary.json` 和 `reviewed_events.json` 读取并渲染。

### 输入

- `app/data/handoff_summary.json`
- `app/data/reviewed_events.json`

### 改动文件

- `prototype/handoff-agent.html`

### 实现内容

1. 页面加载时读取 JSON 数据。
2. 从 `handoff_summary.json` 渲染：
   - total alerts
   - ignored alerts
   - explained alerts
   - action needed
   - top risk event
3. 从 `reviewed_events.json` 生成事件分组：
   - `Action needed`
   - `Explained`
   - `Ignored`
4. 读取失败时使用页面内 fallback 数据，保证现场稳定。

### 页面要求

页面必须显示：

```text
47 / 44 / 2 / 1
BackGate-01 · 02:13
后门
Action needed
```

### 验收标准

- 修改 `handoff_summary.json` 中的数字后，刷新页面能看到总览变化。
- 删除网络或不开后端时，页面仍能使用 fallback 数据演示。
- 左侧事件分组数量来自数据，不是手写文本。

## Task 3：事件详情联动

### 目标

点击任意事件后，中间详情区显示对应事件的原始信息和复核结果。

### 输入

- `app/data/events.json`
- `app/data/reviewed_events.json`

### 改动文件

- `prototype/handoff-agent.html`

### 实现内容

1. 用 `event_id` 关联 raw event 和 reviewed event。
2. 点击事件列表项后更新：
   - 事件标题
   - 时间
   - 摄像头
   - 区域
   - 风险等级
   - 画面占位
   - 事件摘要
   - 风险原因
   - 误报说明
   - 证据点
   - 建议动作
3. 默认选中 `evt_001`。

### 验收标准

- 点击 `evt_001` 时显示后门 02:13 高风险事件。
- 点击 ignored 事件时，详情区显示对应误报原因。
- 点击 explained 事件时，详情区显示“已解释”的原因。
- 事件详情不出现上一条事件残留内容。

## Task 4：问答区基于固定数据回答

### 目标

问答区不做开放式聊天，只基于当前 Demo 数据回答固定问题，避免编造。

### 输入

- `app/data/handoff_summary.json`
- `app/data/reviewed_events.json`
- `docs/15-ai-review-rules.md`

### 改动文件

- `prototype/handoff-agent.html`

### 支持问题

必须支持：

```text
昨晚有什么异常？
今天先处理哪里？
哪些是误报？
```

### 回答规则

- 只能基于当前 JSON 数据回答。
- 不编造不存在的事件。
- 不说“一定入侵”。
- 高风险事件必须建议人工复核。
- 数据不足时回答“需要人工复核”，不强行判断。

### 实现方式

前端可以先实现固定问答映射：

| 问题 | 数据来源 |
| --- | --- |
| 昨晚有什么异常？ | `handoff_summary.top_risk_event_id` + reviewed event |
| 今天先处理哪里？ | `handoff_summary.recommended_next_action` |
| 哪些是误报？ | `reviewed_events` 中 `review_result=ignored` 的统计 |

### 验收标准

- 点击三个内置问题，都能返回明确答案。
- 回答中能指出 `BackGate-01 / 02:13 / 后门`。
- “哪些是误报”能按 ignored 类型概括。
- 回答不出现 JSON 中不存在的摄像头、时间或事件。

## Task 5：处置按钮更新 actions 状态

### 目标

用户点击处置按钮后，页面状态发生变化，形成完整闭环。

### 输入

- `app/data/actions.json`
- 当前选中事件
- 当前 handoff status

### 改动文件

- `prototype/handoff-agent.html`

### 按钮

必须支持：

```text
通知店长复核
安排巡检
标记已复核
```

### 状态流

```text
action_needed
-> assigned
-> reviewed
-> handoff_closed
```

### 实现内容

1. 点击 `通知店长复核`：
   - 状态变为 `assigned`
   - 日志显示已通知店长复核 02:13 画面
2. 点击 `安排巡检`：
   - 状态保持或变为 `assigned`
   - 日志显示已安排早班检查后门
3. 点击 `标记已复核`：
   - 从 `assigned` 进入 `reviewed`
   - 再次点击或关闭交接后进入 `handoff_closed`
4. 页面内维护 runtime actions，不必写回文件。

### 验收标准

- 点击按钮后，右侧状态和日志必须变化。
- 最终能显示 `Handoff closed`。
- 状态变化不依赖网络。
- 刷新页面后恢复初始演示状态，方便重复 Demo。

## Task 6：封装本地 API

### 目标

把静态 JSON 读取封装成本地 API，为后续前后端分离和演示录屏做准备。

### 输入

- `app/data/*.json`
- Task 2-5 的前端数据需求

### 新增文件

推荐 Python FastAPI：

```text
app/main.py
app/routes/handoff.py
app/routes/events.py
app/routes/qa.py
app/routes/actions.py
app/services/handoff_service.py
app/services/qa_service.py
```

### API

| Method | Path | 用途 |
| --- | --- | --- |
| GET | `/api/handoff/today` | 返回晨间摘要和当前状态 |
| GET | `/api/events` | 返回分组后的事件列表 |
| GET | `/api/events/{event_id}` | 返回事件详情 |
| POST | `/api/qa` | 基于固定数据回答问题 |
| POST | `/api/actions` | 记录处置动作 |

### 验收标准

- 本地启动后可以访问全部 API。
- API 返回结构和前端需要一致。
- 前端可以通过 API 模式运行。
- API 不要求真实数据库，先读写内存或 JSON。

## Task 7：预留 eufy SDK adapter

### 目标

把真实 eufy 接入限制在 adapter 层，不影响后面的 AI Review、Risk Engine 和 Handoff 流水线。

### 新增文件

```text
app/adapters/base_event_adapter.py
app/adapters/mock_event_adapter.py
app/adapters/eufy_event_adapter.py
```

### 接口约定

```python
class EventAdapter:
    def list_events(self, start_time, end_time):
        pass

    def get_event_asset(self, event_id):
        pass
```

### 实现原则

- `MockEventAdapter` 读取 `app/data/events.json`。
- `EufyEventAdapter` 只负责真实 eufy 事件映射。
- 后续服务只依赖统一 Event Schema。
- SDK 不可用时，Demo 不受影响。

### 验收标准

- 切换 mock / eufy source 不改变下游数据结构。
- 没有真实 SDK 时，`EufyEventAdapter` 可以保留 stub。
- 答辩时能清楚说明：真实 SDK 是事件来源，不是系统核心耦合点。

## Task 8：预留模型调用 adapter

### 目标

把模型调用和 Demo 固定数据解耦。黑客松现场使用固定 reviewed events 保证稳定；赛后可以替换成真实多模态模型。

### 新增文件

```text
app/adapters/base_ai_review_adapter.py
app/adapters/static_review_adapter.py
app/adapters/model_review_adapter.py
app/services/risk_engine.py
```

### 接口约定

```python
class AiReviewAdapter:
    def review_event(self, raw_event):
        pass
```

### 实现原则

- `StaticReviewAdapter` 读取 `reviewed_events.json`。
- `ModelReviewAdapter` 调用多模态模型，输入截图/短视频和事件上下文。
- `risk_engine.py` 对模型输出做规则校验。
- 模型不确定时输出 `review`，不强行升级风险。

### 验收标准

- 没有模型 API key 时，Demo 可以用 static review 跑通。
- 有模型 API key 时，可以对单条事件生成 review 结果。
- 模型输出必须符合 `docs/15-ai-review-rules.md`。
- 高风险和不确定事件必须保留人工复核。

## 2. Task 依赖关系

```text
Task 1
-> Task 2
-> Task 3
-> Task 4
-> Task 5
-> Task 6
-> Task 7 / Task 8
```

Task 1-5 是现场 Demo 主路径。Task 6-8 是工程增强路径。

## 3. 最小可交付版本

最小可交付版本只包含：

```text
app/data/*.json
prototype/handoff-agent.html
prototype/DEMO-RUNBOOK.md
```

必须能完成：

1. 打开原型。
2. 点击 `开始昨晚事件回放`。
3. 看到 `47 / 44 / 2 / 1`。
4. 点击后门 `02:13` 事件。
5. 询问三类问题。
6. 点击处置按钮。
7. 状态进入 `Handoff closed`。

## 4. 工程完成标准

| 能力 | Task 1-5 | Task 6 | Task 7-8 |
| --- | --- | --- | --- |
| 现场演示 | 可以 | 可以 | 可以 |
| 离线可跑 | 可以 | 可以 | 可以 |
| 前端数据驱动 | 可以 | 可以 | 可以 |
| 本地 API | 不需要 | 可以 | 可以 |
| 真实 eufy 接入 | 不包含 | 不包含 | 预留 |
| 真实模型调用 | 不包含 | 不包含 | 预留 |

## 5. 不在当前实现范围

当前阶段不做：

- 真实 eufy SDK 接入。
- 真实用户账号登录。
- 真实短信、WhatsApp、邮件发送。
- 真实摄像头视频上传。
- 生产级数据库和权限系统。
- 多门店后台管理。

这些能力不影响黑客松现场 Demo 主线。
