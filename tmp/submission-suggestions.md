# SentryFlow AI 提交材料补充建议

这份文档记录当前基础材料之外，后续可以逐步补齐的参赛提交材料。

## 当前判断

`docs/v2-design/00-11` 已经覆盖产品定位、市场调研、方案收敛、技术架构、路演脚本、答辩 Q&A、提交清单、Pitch Deck 大纲和证据账本。

当前缺口不在“想法是否清楚”，而在“评委能否看见、跑起来、回放、验证”。

下一步重点：

```text
从文档材料 -> 可展示提交包
```

## 1. Pitch Deck 成品

当前已有：

```text
docs/v2-design/10-PitchDeck大纲-pitch-deck-outline.md
```

还需要：

- 5-7 页 PPT 或 PDF。
- 每页只保留核心信息，不堆长文档。
- 加入 Demo 截图和架构图。

建议文件：

```text
pitch/SentryFlow-AI-pitch-deck.pdf
```

## 2. Demo Video / Screen Recording

用途：

- 评委无法现场完整操作时，可以直接看视频。
- 提交平台常见需要 Demo video 或 screen recording。

需要准备：

- 90 秒短版视频。
- 3 分钟正式版视频。
- 视频分镜脚本。

建议文件：

```text
docs/v2-design/12-demo-video-storyboard.md
```

## 3. 截图素材包

至少准备这些截图：

- 总览页：`47 alerts -> 1 action`
- 高风险事件详情页
- 问答页
- 处置状态变化
- 技术架构图

建议文件：

```text
docs/v2-design/13-screenshot-shot-list.md
assets/submission/
```

## 4. Demo 数据包

需要把 Demo 故事固定成可验证数据，而不是只停留在文案里。

建议数据：

```text
app/data/events.json
app/data/reviewed_events.json
app/data/handoff_summary.json
assets/samples/back_gate_intrusion.jpg
assets/samples/warehouse_low_light.jpg
assets/samples/solar_after_rain.jpg
```

建议文档：

```text
docs/v2-design/14-demo-data-spec.md
```

## 5. 可验证指标

当前主故事是：

```text
47 alerts -> 1 clear action
```

需要明确指标：

- 47 条告警总数。
- 44 条 ignored。
- 2 条 explained。
- 1 条 action needed。
- 用户阅读时间：约 30 秒。
- 用户动作：通知店长 / 安排巡检 / 标记已复核。

这些指标可以放入：

```text
docs/v2-design/14-demo-data-spec.md
```

## 6. Known Limitations / Disclosure

需要单独说明：

- eufy 官方 SDK 未确认。
- 当前 Demo 可使用模拟事件流。
- 哪些是模型输出，哪些是预置样例。
- 不替代安保责任人。
- 高风险或不确定事件仍需人工复核。

建议文件：

```text
docs/v2-design/15-known-limitations-and-disclosure.md
```

## 7. Team / Role / Build Plan

需要说明 24 小时内怎么分工完成：

- 前端 / 产品原型
- 后端 / 数据流
- AI / 多模态复核
- SDK / 硬件接入
- Pitch / Demo / 市场材料

建议文件：

```text
docs/v2-design/16-team-build-plan.md
```

## 8. Code Quality / Repo Hygiene

需要补齐工程提交常见材料：

- `.env.example`
- 运行命令
- 依赖说明
- 项目结构
- 没网怎么演示
- Demo fallback

已有部分：

```text
DEMO-GUIDE-Demo指南.md
README.md
```

后续可以检查是否需要补：

```text
requirements.txt
package.json
app/README.md
```

## 9. Final Submission Package

最后需要一份总清单，告诉自己和队友最终提交什么。

建议文件：

```text
docs/v2-design/17-final-submission-package.md
```

内容包括：

- GitHub / repo 链接
- Pitch Deck
- Demo Video
- Demo Guide
- 原型入口
- 技术架构
- Evidence Log
- Known Limitations
- Team Plan

## 推荐下一步顺序

```text
1. 12-demo-video-storyboard.md
2. 13-screenshot-shot-list.md
3. 14-demo-data-spec.md
4. 15-known-limitations-and-disclosure.md
5. 16-team-build-plan.md
6. 17-final-submission-package.md
```

## 最重要的提醒

现在不是继续扩展想法，而是把已有想法变成评委能看见的材料：

```text
视频、截图、数据、运行说明、边界披露、最终提交包。
```
