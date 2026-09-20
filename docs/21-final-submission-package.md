# 21 Final Submission Package

这份文档是比赛提交前的最终索引。提交前只看这一份，确认材料是否齐、路径是否对、哪些还缺。

## 1. 提交主线

项目名称：

```text
SentryFlow AI
```

一句话：

```text
eufy 摄像头的晨间安防交接。
```

核心记忆点：

```text
47 条告警 -> 1 个清楚动作
```

当前提交主线：

```text
eufy Morning Handoff
```

## 2. 最终提交材料

| 材料 | 路径 / 链接 | 状态 | 用途 |
| --- | --- | --- | --- |
| GitHub / repo 链接 | 待填写 | Missing | 主办方查看项目代码和材料 |
| Pitch Deck PDF | `pitch/SentryFlow-AI-pitch-deck.pdf` | Ready | 预选提交和现场路演 |
| Pitch Deck PPTX | `pitch/SentryFlow-AI-pitch-deck.pptx` | Ready | 可编辑版 |
| Demo Video | 待录制 | Missing | 预选提交和异步评审 |
| Demo Guide | `DEMO-GUIDE-Demo指南.md` | Ready | 现场演示说明 |
| Demo Runbook | `prototype/DEMO-RUNBOOK.md` | Ready | 队友照着跑 Demo |
| 原型入口 | `prototype/handoff-agent.html` | Ready | 可执行 Demo |
| 数据说明 | `app/data/README.md` | Ready | 说明 47/44/2/1 数据来源 |
| Demo 数据包 | `app/data/*.json` | Ready | events、reviewed_events、summary、actions |
| 技术架构 | `docs/06-工程架构-technical-architecture.md` | Ready | 模块、数据、API、SDK/Mock 双路径 |
| AI Review 规则 | `docs/15-ai-review-rules.md` | Ready | AI 判断逻辑和问答边界 |
| 技术实现计划 | `docs/16-technical-implementation-plan.md` | Ready | Task 1-8 工程拆分 |
| 页面规格 | `docs/17-page-spec.md` | Ready | 页面设计和开发规格 |
| 截图清单 | `docs/18-screenshot-shot-list.md` | Ready | Pitch、提交页、视频素材 |
| 边界披露 | `docs/19-limitations-disclosure.md` | Ready | 答辩备用材料 |
| Demo Video 脚本 | `docs/20-demo-video-storyboard.md` | Ready | 90 秒和 3 分钟录屏脚本 |
| 答辩 Q&A | `docs/08-答辩QA-judge-qa.md` | Ready | 评委追问口径 |
| 提交清单 | `docs/09-提交清单-submission-checklist.md` | Ready | 原有提交检查表 |

## 3. GitHub / Repo 链接

当前状态：待填写。

提交前填写：

```text
GitHub repo: <repo_url>
```

检查要求：

- 仓库能打开。
- README 首屏能说明项目是什么。
- `prototype/handoff-agent.html` 能找到。
- `pitch/SentryFlow-AI-pitch-deck.pdf` 能找到。
- `app/data/*.json` 能找到。
- 不提交隐私文件、账号、密钥或真实摄像头素材。

## 4. Pitch Deck PDF

文件：

```text
pitch/SentryFlow-AI-pitch-deck.pdf
```

检查要求：

- PDF 能打开。
- 页数控制在 5-7 页。
- 第一页能看到 `SentryFlow AI` 和 `47 -> 1`。
- 有 Demo 页。
- 有 AI 技术页。
- 有架构页。
- 没有承诺已接入官方 eufy SDK。

## 5. Demo Video

当前状态：待录制。

脚本：

```text
docs/20-demo-video-storyboard.md
```

推荐输出文件：

```text
pitch/SentryFlow-AI-demo-video-90s.mp4
pitch/SentryFlow-AI-demo-video-3min.mp4
```

至少提交一个版本：

- 90 秒版：适合预选提交。
- 3 分钟版：适合完整演示备份。

检查要求：

- 能看到 `47 / 44 / 2 / 1`。
- 能看到后门 `02:13` 事件。
- 能看到 AI 问答。
- 能看到处置状态变化。
- 能看到或讲清技术架构。
- 明确说明 Demo 使用模拟事件流。

## 6. Demo Guide

文件：

```text
DEMO-GUIDE-Demo指南.md
prototype/DEMO-RUNBOOK.md
```

检查要求：

- 队友能按文档打开原型。
- 队友能完成 3 分钟演示。
- 弱网、无 SDK、无后端时仍能演示。

## 7. 原型入口

文件：

```text
prototype/handoff-agent.html
```

检查要求：

- 浏览器能直接打开。
- 能点击 `开始昨晚事件回放`。
- 能显示 `47 / 44 / 2 / 1`。
- 能显示 `BackGate-01 · 02:13`。
- 能点击三个问答问题。
- 能点击处置按钮。
- 最终能进入 `Handoff closed`。

## 8. 数据说明

文件：

```text
app/data/README.md
app/data/events.json
app/data/reviewed_events.json
app/data/handoff_summary.json
app/data/actions.json
```

检查要求：

- `events.json` 有 47 条。
- `reviewed_events.json` 有 47 条。
- 统计结果是 44 ignored、2 explained、1 action_needed。
- `handoff_summary.json` 与统计结果一致。
- `actions.json` 覆盖通知、巡检、复核、关闭交接。

## 9. 技术架构

文件：

```text
docs/06-工程架构-technical-architecture.md
docs/16-technical-implementation-plan.md
```

检查要求：

- 说明 SDK / Mock 双路径。
- 说明 Event Schema。
- 说明 AI Review、Risk Engine、Handoff Summary。
- 说明 Task 1-5 可以支撑现场 Demo。
- 说明 Task 7-8 是后续 adapter 预留。

## 10. 边界披露

文件：

```text
docs/19-limitations-disclosure.md
```

检查要求：

- 不承诺已接入官方 eufy SDK。
- 说明当前 Demo 可使用模拟事件流。
- 说明不做生产级实时视频流。
- 说明不替代安保责任人。
- 说明 AI 只做复核、解释和交接。
- 说明高风险或不确定事件必须人工复核。

## 11. 答辩 Q&A

文件：

```text
docs/08-答辩QA-judge-qa.md
docs/15-ai-review-rules.md
docs/19-limitations-disclosure.md
```

检查要求：

- 能回答“AI 到底怎么判断”。
- 能回答“SDK 没接入怎么办”。
- 能回答“这是不是只是模拟数据”。
- 能回答“AI 判断错了怎么办”。
- 能回答“为什么适合 eufy”。

## 12. 提交前缺口

当前还缺：

```text
1. GitHub / repo 链接
2. Demo Video 文件
3. 最终截图素材 assets/screenshots/*.png
```

可选补充：

```text
1. 原型读取 app/data/*.json，而不是页面内写死数据
2. 本地 API：GET /api/handoff/today、GET /api/events、POST /api/qa、POST /api/actions
3. 更精细的 React 版本 UI
```

## 13. 最终检查

提交前逐项确认：

- 根目录 `README.md` 是项目入口。
- `docs/README.md` 不再存在，避免入口混乱。
- `prototype/DEMO-RUNBOOK.md` 是演示操作手册。
- Pitch Deck PDF 可打开。
- 原型 HTML 可打开。
- 数据统计能得到 `47 / 44 / 2 / 1`。
- 边界披露没有夸大。
- 视频脚本已录制或准备录制。
- 提交页材料路径全部可访问。
