# 10 Pitch Deck 大纲

SentryFlow AI 的 Pitch Deck 控制在 7 页以内，围绕「问题、方案、演示、技术、商业、eufy 生态适配」展开。

## Deck Structure

```text
1. Title / 一句话
2. Problem / 用户痛点
3. Solution / 产品方案
4. Demo Flow / 演示闭环
5. Technology / 技术架构
6. Market & Business / 市场和商业
7. Why eufy + Next Step / 生态适配和下一步
```

## Slide 1：Title

```text
SentryFlow AI
From 47 alerts to 1 clear action
```

副标题：

```text
把 eufy 摄像头的一夜告警，变成早上 30 秒能读完的安全交接。
```

页面内容：

- 项目名
- 一句话价值
- 团队名/成员
- Anker / eufy 智能安防黑客松

## Slide 2：Problem

```text
Small businesses do not lack cameras. They lack a clear handoff.
```

核心信息：

- 小商铺、仓库和远程资产已经有摄像头。
- 闭店后一夜会有大量 motion alerts。
- 雨水、车灯、昆虫、员工活动和真实风险混在一起。
- 用户真正要知道的是：今天先处理什么。

视觉表达：

```text
47 motion alerts -> user ignores most of them -> real risk may be missed
```

## Slide 3：Solution

```text
eufy Morning Handoff
```

核心信息：

```text
SentryFlow AI reviews nightly camera events and turns them into a morning security handoff.
```

产品输出分为三类：

- Ignore：雨水、车灯、昆虫等低风险事件。
- Explain：员工活动、正常经过等可解释事件。
- Act：真正需要处理的高优先级事件。

主张：

```text
不是多一个告警，而是一次清楚的交接。
```

## Slide 4：Demo Flow

```text
47 alerts -> 1 action needed
```

演示流程：

1. 播放昨晚事件流。
2. AI 复核 47 条告警。
3. 44 条忽略，2 条解释，1 条待处理。
4. 点开后门事件。
5. 查看证据、风险原因和建议动作。
6. 用户点击通知店长或安排巡检。

配图：

- 总览页
- 高风险事件详情
- 问答/处置状态

## Slide 5：Technology

```text
Built as an event handoff pipeline
```

架构：

```text
eufy / mock events
-> Event Schema
-> AI Review
-> Risk Engine
-> Handoff Summary
-> Q&A + Actions
```

技术边界：

- SDK 可用：接真实 eufy 事件。
- SDK 不可用：模拟事件流兜底。
- 同一套 Event Schema，不依赖单一路径。
- 24 小时内交付稳定闭环。

## Slide 6：Market & Business

```text
A lightweight AI upgrade for camera packages
```

核心信息：

- AI CCTV 已经存在，但很多方案偏企业级和项目制。
- eufy 可以切入小商业、仓库、轻工业和远程边缘资产。
- 商业路径不是单卖复杂软件，而是摄像头套餐或 App 增值功能。

价值主张：

```text
少看：不用翻几十条告警。
少跑：不用每次异常都派人去现场。
少漏：真正风险不会被误报淹没。
```

## Slide 7：Why eufy + Next Step

```text
Turn eufy cameras into handoff assistants
```

eufy 适配点：

- eufy 已有摄像头、门铃、HomeBase、夜视、本地存储、隐私能力。
- SentryFlow AI 把硬件事件转成服务体验。
- 项目从家庭安防自然扩展到小商业和远程边缘资产。

下一步：

1. 完成稳定 Demo。
2. 确认现场 SDK/API 能力边界。
3. 接入真实事件或保持模拟事件流。
4. 扩展太阳能小站/远程资产样例。
5. 打磨 eufy App 内 Morning Handoff 概念。

## Backup：Risk & Boundary

- 不承诺已接入官方 SDK。
- 不替代安保责任人。
- 高风险或不确定事件仍需人工复核。
- 不做重型 CCTV 平台。

## Supporting Materials

- `07-demo-script.md`：现场讲稿。
- `08-judge-qa.md`：答辩口径。
- `06-technical-architecture.md`：技术细节。
