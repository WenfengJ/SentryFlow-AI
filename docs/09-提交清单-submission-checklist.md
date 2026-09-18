# 09 提交材料清单

SentryFlow AI v2 的参赛提交包由项目入口、产品设计、技术架构、Demo 材料、答辩材料和证据材料组成。

## 1. Core Submission Package

| 材料 | 文件/位置 | 状态 | 用途 |
| --- | --- | --- | --- |
| 项目入口 README | `README.md` | Ready | 仓库首页，呈现 v2 核心产品思路 |
| v2 最终产品设计 | `docs/v2-design/00-产品定位-final-product-design.md` | Ready | 产品、用户、商业、Demo 和 task list |
| 技术架构说明 | `docs/v2-design/06-工程架构-technical-architecture.md` | Ready | 模块、数据结构、API、Prompt、SDK/模拟双路径 |
| Demo 路演脚本 | `docs/v2-design/07-路演脚本-demo-script.md` | Ready | 3 分钟正式版和 90 秒短版 |
| 评委 Q&A | `docs/v2-design/08-答辩QA-judge-qa.md` | Ready | SDK、竞品、商业、技术、风险边界 |
| Demo 运行说明 | `DEMO-GUIDE-Demo指南.md` | Ready | 原型打开方式、演示路径和兜底方案 |
| Pitch Deck 大纲 | `docs/v2-design/10-PitchDeck大纲-pitch-deck-outline.md` | Ready | PPT 页结构 |
| Evidence Log | `docs/v2-design/11-证据账本-evidence-log.md` | Ready | 可引用事实、来源和表达边界 |

## 2. Repository Package

```text
SentryFlow-AI/
  README.md
  DEMO-GUIDE-Demo指南.md
  prototype/
    handoff-agent.html
    index.html
  docs/
    v2-design/
      00-产品定位-final-product-design.md
      06-工程架构-technical-architecture.md
      07-路演脚本-demo-script.md
      08-答辩QA-judge-qa.md
      09-提交清单-submission-checklist.md
      10-PitchDeck大纲-pitch-deck-outline.md
      11-证据账本-evidence-log.md
```

## 3. Pitch Materials

| 材料 | 状态 | 使用方式 |
| --- | --- | --- |
| 5-7 页 Pitch Deck | To Produce | 按 `10-PitchDeck大纲-pitch-deck-outline.md` 制作 PPT |
| 3 分钟 Demo 脚本 | Ready | 用 `07-路演脚本-demo-script.md` 排练 |
| 90 秒短视频脚本 | Ready | 使用 `07-路演脚本-demo-script.md` 的短版 |
| 产品截图 | To Capture | 从 `prototype/handoff-agent.html` 截 3-5 张关键画面 |
| 架构图 | To Export | 使用 `06-工程架构-technical-architecture.md` 中的 Mermaid 图 |
| 答辩 Q&A | Ready | 用 `08-答辩QA-judge-qa.md` 统一口径 |

## 4. Demo Acceptance Criteria

| 检查项 | 标准 |
| --- | --- |
| 原型可打开 | 本地打开 `prototype/handoff-agent.html` 或按 `DEMO-GUIDE-Demo指南.md` 操作 |
| 主故事清楚 | 47 条告警 -> 1 个待处理动作 |
| 问答能讲通 | 能回答“今天先处理哪里” |
| 技术边界清楚 | SDK 可用/不可用两条路径表达明确 |
| 兜底可演示 | 无网络或 SDK 不可用时，仍能演示模拟事件流 |
| 风险不夸大 | 不宣称已接入官方 SDK，不宣称 AI 自动处理全部风险 |

## 5. Final Pre-Submission Check

- [ ] README 第一屏呈现产品核心。
- [ ] Pitch Deck 不超过 7 页。
- [ ] Demo 路径排练至少 3 次。
- [ ] 90 秒短版视频脚本已确认。
- [ ] SDK 不可用时的解释已确认。
- [ ] Q&A 中的高风险问题已确认。
- [ ] Demo 截图和架构图已导出。
- [ ] 离线兜底素材已准备。
