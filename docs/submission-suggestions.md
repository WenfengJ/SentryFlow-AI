# SentryFlow AI 剩余提交任务单

这份任务单只保留比赛提交前真正需要做的事。已经完成的产品设计、Demo 规格、AI 规则、页面规格、边界披露和最终提交索引，不再重复拆成新任务。

当前判断：材料已经足够多，下一阶段不要继续堆文档。重点转向能被评委直接看到的提交证据：原型、截图、视频、仓库入口、边界口径。

## 当前已完成

| 模块 | 文件 | 状态 |
| --- | --- | --- |
| 项目入口 | `README.md` | 已完成 |
| 产品定位 | `docs/00-产品定位-final-product-design.md` | 已完成 |
| 产品规格 | `docs/12-product-spec.md` | 已完成 |
| 用户流程 | `docs/13-user-flow.md` | 已完成 |
| Demo 原型 | `prototype/handoff-agent.html` | 已完成基础版 |
| Demo 操作手册 | `prototype/DEMO-RUNBOOK.md` | 已完成 |
| Demo 数据包 | `app/data/*.json` | 已完成 |
| AI 判断规则 | `docs/15-ai-review-rules.md` | 已完成 |
| 技术实现计划 | `docs/16-technical-implementation-plan.md` | 已完成 |
| 页面规格 | `docs/17-page-spec.md` | 已完成 |
| 截图清单 | `docs/18-screenshot-shot-list.md` | 已完成 |
| 边界披露 | `docs/19-limitations-disclosure.md` | 已完成 |
| Demo Video 脚本 | `docs/20-demo-video-storyboard.md` | 已完成 |
| 最终提交索引 | `docs/21-final-submission-package.md` | 已完成 |
| Pitch Deck | `pitch/SentryFlow-AI-pitch-deck.pdf` / `.pptx` | 已完成 |
| 答辩 Q&A | `docs/08-答辩QA-judge-qa.md` | 已完成 |

## 不再新增的任务

以下任务不再单独新增文档，避免材料膨胀。

| 原任务 | 处理方式 |
| --- | --- |
| `22-preliminary-submission-one-pager.md` | 不单独做。提交页简介直接写在 `21-final-submission-package.md` 或提交表单里。 |
| `23-submission-form-copy.md` | 不单独做。需要时在最终提交前补 120 字和 500 字文案即可。 |
| `24-repo-readme-submission-version.md` | 不单独做。根目录 `README.md` 已经承担仓库首页职责。 |
| `25-demo-video-script.md` | 不单独做。已由 `20-demo-video-storyboard.md` 覆盖。 |
| `26-judging-evidence-map.md` | 不单独做。已由 `21-final-submission-package.md` 覆盖。 |
| `27-team-and-build-plan.md` | 暂不做。除非提交表单明确要求团队分工或 24 小时计划。 |
| `28-originality-and-credits.md` | 暂不做。只在提交平台要求原创性/引用声明时补短版。 |
| `29-preliminary-submission-checklist.md` | 不单独做。已由 `21-final-submission-package.md` 覆盖。 |

## P0：提交前必须完成

### 1. 生成最终截图素材

输出目录：

```text
assets/screenshots/
```

必须至少生成：

```text
01-overview-47-44-2-1.png
02-high-risk-backgate-0213.png
03-ai-qa-next-action.png
04-action-status-assigned.png
06-architecture-pipeline.png
```

可选补充：

```text
05-handoff-closed.png
```

依据文档：

```text
docs/18-screenshot-shot-list.md
```

验收标准：

- Pitch Deck、提交页、Demo Video 都能复用。
- 每张图都能看清关键文字。
- 不出现浏览器隐私路径、控制台、无关桌面信息。

### 2. 录制 Demo Video

依据文档：

```text
docs/20-demo-video-storyboard.md
```

至少输出一个视频：

```text
pitch/SentryFlow-AI-demo-video-90s.mp4
```

推荐再补一个完整版本：

```text
pitch/SentryFlow-AI-demo-video-3min.mp4
```

验收标准：

- 能看到 `47 / 44 / 2 / 1`。
- 能看到后门 `02:13` 事件。
- 能看到 AI 问答：`今天先处理哪里？`
- 能看到处置状态变化：`Assigned` 或 `Handoff closed`。
- 明确说明当前 Demo 使用模拟事件流，不承诺已接入官方 eufy SDK。

### 3. 填写 GitHub / repo 链接

需要在最终提交前填写到：

```text
docs/21-final-submission-package.md
```

当前占位：

```text
GitHub repo: <repo_url>
```

验收标准：

- 链接可打开。
- README 首屏能说明项目是什么。
- Pitch、原型、数据包、Demo Guide 都能从仓库找到。
- 不包含密钥、账号、真实用户视频或隐私素材。

### 4. 最终检查原型是否可演示

检查文件：

```text
prototype/handoff-agent.html
prototype/DEMO-RUNBOOK.md
```

验收标准：

- 双击 HTML 可以打开。
- 点击 `开始昨晚事件回放` 有反馈。
- 页面显示 `47 / 44 / 2 / 1`。
- 页面显示 `BackGate-01 · 02:13`。
- 三个问答按钮可用。
- 三个处置按钮可用。
- 最终能进入 `Handoff closed`。

## P1：有时间再做，但不影响提交主线

### 5. 让原型读取 `app/data/*.json`

当前原型已经能演示，但部分数据仍在页面内。更好的版本是让页面读取数据包：

```text
app/data/events.json
app/data/reviewed_events.json
app/data/handoff_summary.json
app/data/actions.json
```

验收标准：

- 总览数字来自 `handoff_summary.json` 或 `reviewed_events.json` 统计。
- 事件列表来自 `reviewed_events.json`。
- 事件详情由 `events.json` + `reviewed_events.json` 合并展示。
- 读取失败时仍有 fallback，不影响现场演示。

这项是加分项，不是提交必需项。当前静态原型已经可以支撑 3 分钟演示。

### 6. 补短版提交表单文案

只有提交平台需要填写时再补。

需要准备：

```text
项目一句话：120 字以内
项目简介：500 字以内
技术亮点：5 条以内
当前完成度：事实描述
SDK/API 边界：1 段话
```

可以直接从以下文档摘取：

- `README.md`
- `docs/21-final-submission-package.md`
- `docs/19-limitations-disclosure.md`
- `docs/15-ai-review-rules.md`

验收标准：

- 不写长篇市场背景。
- 不写融资 BP 口吻。
- 不承诺已接入官方 eufy SDK。
- 第一段出现 `47 条告警 -> 1 个清楚动作`。

### 7. 补团队分工和 24 小时 Build Plan

只有主办方要求团队信息或现场开发计划时再补。

最小内容：

```text
产品 / Demo 负责人
前端负责人
数据 / AI 规则负责人
Pitch / 答辩负责人
24 小时现场开发排期
SDK 可用和不可用两套路径
```

验收标准：

- 能说明团队知道现场怎么推进。
- 不需要单独扩写成长文档。

## P2：暂时不做

以下任务当前不做，除非比赛规则明确要求：

- 完整后端 API。
- FastAPI / Node 服务。
- 真实 eufy SDK 接入。
- 真实模型调用。
- 生产级实时视频流。
- 多用户账号和权限。
- 多门店后台。
- 单独原创性与 credits 长文档。
- 单独 judging evidence map。

原因：

- 当前目标是参赛初始材料和可演示闭环。
- 评委更需要看到清晰 Demo、技术边界和 eufy 适配，而不是一堆未实现的工程文件。
- 继续新增过多文档会稀释主线。

## 最终提交前只看这三份

提交前优先看：

```text
README.md
prototype/DEMO-RUNBOOK.md
docs/21-final-submission-package.md
```

如果要录视频，看：

```text
docs/20-demo-video-storyboard.md
```

如果要答辩，看：

```text
docs/08-答辩QA-judge-qa.md
docs/19-limitations-disclosure.md
docs/15-ai-review-rules.md
```

## 当前真正缺口

截至当前，真正还缺的是：

```text
1. 最终截图素材
2. Demo Video
3. GitHub / repo 链接
4. 提交平台要求的短文案
```

其他内容先不要继续扩散。
