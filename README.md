# SentryFlow AI

SentryFlow AI 是一个面向 Anker / eufy 智能安防黑客松的产品方案项目。

当前整理后分为两个版本：

- `v2`：当前参赛主线，推荐用于比赛提交、Demo 和路演。
- `v1`：早期设计方向，保留为长期愿景和扩展参考。

## 当前推荐方向

参赛建议使用 `v2`：

```text
SentryFlow AI：面向小商业、仓库、轻工业和远程边缘资产的 eufy AI 安防交接服务。
```

核心思路：

```text
不是再做一个“会识别”的摄像头，
而是让 eufy 摄像头把一夜的告警整理成可复核、可交接、可执行的安全摘要。
```

最适合黑客松路演的 Demo 故事：

```text
eufy Morning Handoff
47 条告警，最后只告诉你 1 件该处理的事。
From 47 alerts to 1 clear action.
```

## 版本关系

| 版本 | 定位 | 适合用途 | 当前判断 |
| --- | --- | --- | --- |
| v2 | eufy AI 安防交接服务 | 黑客松参赛、Demo、Pitch、最终方案 | 推荐作为主线 |
| v1 | 远程能源 / 工业场站 AI 巡检 Agent | 长期愿景、扩展场景、原始思路追溯 | 不建议作为本次参赛主叙事 |

## 为什么参赛选 v2

v1 的早期设想是让 eufy 摄像头成为远程太阳能、储能和工业场站的 AI 巡检员。这个方向有差异化，但对黑客松来说偏重、偏 B2B，也更容易讲成工业平台。

v2 将方向收敛为小商业、仓库、轻工业和远程边缘资产的“安防交接服务”。它更贴近 eufy 智能安防赛道，也更容易在 24 小时内做出稳定 Demo：

```text
模拟或真实 eufy 事件
-> AI 复核
-> 风险分级
-> 误报判断
-> 处置建议
-> 晨间交接摘要
-> 用户问答
```

一句话判断：

```text
参赛用 v2，v1 作为长期愿景和扩展素材。
```

## 阅读入口

建议从这里开始：

1. [v2 黑客松参赛设计](docs/v2-design/README.md)  
   当前主线。用于理解最终参赛方向、市场判断、方案收敛、Demo 故事和技术演示方案。
2. [v1 原始产品方向](docs/v1-design/README.md)  
   早期方向。用于理解项目最初为什么从远程能源、储能和工业场站切入。
3. [Anker / eufy 官方赛题资料整理](参赛规则.md)  
   用于核对赛题背景和智能安防赛道要求。

## 目录结构

```text
SentryFlow-AI/
  README.md
  docs/
    v2-design/
      README.md
      01-strategy-review.md
      02-market-research.md
      03-solution-plan.md
      04-demo-story-options.md
      05-technical-demo-plan.md
    v1-design/
      README.md
      00-original-root-readme.md
      hackathon-brief.md
      mvp-roadmap.md
      product/
      research/
      prompts/
      team-flag.md
      team-recruitment.md
    multi-agent-market-research-playbook.md
  refer/
    anker-eufy-smart-security-official-brief.md
```

## 当前工作重点

下一步应围绕 v2 推进：

1. 把 Demo 固定为 `eufy Morning Handoff`。
2. 准备 47 条告警到 1 条行动建议的演示数据。
3. 做一个轻量网页工作台和手机通知/交接卡模拟。
4. 明确 eufy SDK/API 可用与不可用两条技术路径。
5. 将 v1 的能源/工业场站内容作为扩展场景，不放在主线开头。
