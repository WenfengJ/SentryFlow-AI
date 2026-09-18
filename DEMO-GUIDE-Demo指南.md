# DEMO GUIDE

SentryFlow AI v2 Demo 展示一条完整的安防交接闭环：

```text
闭店后一夜 47 条告警，最后只告诉用户 1 件该处理的事。
```

```text
事件输入 -> AI 复核 -> 风险排序 -> 晨间交接 -> 用户问答 -> 处置状态
```

## 1. Prototype Entry

当前原型入口：

```text
prototype/handoff-agent.html
```

备用入口：

```text
prototype/index.html
```

浏览器可直接打开 `prototype/handoff-agent.html`。如果需要通过本地静态服务访问：

```bash
python -m http.server 8080
```

访问地址：

```text
http://localhost:8080/prototype/handoff-agent.html
```

## 2. Live Demo Flow

### Step 1：开场

```text
很多小商业不是没有摄像头，而是没人有精力看摄像头。
闭店后一夜 47 条 motion alerts，老板真正需要知道的可能只有一件事。
```

### Step 2：展示总览

```text
47 alerts reviewed
44 ignored
2 explained
1 action needed
```

### Step 3：点开高风险事件

画面呈现：

- 后门事件。
- 02:13 陌生人停留。
- 风险原因。
- 处置动作。

讲解口径：

```text
系统不是只识别“有人”，而是判断这个事件是否值得进入交接。
```

### Step 4：问答

问题：

```text
今天先处理哪里？
```

回答：

```text
先检查后门门锁，并请店长复核 02:13 的后门画面。
```

### Step 5：处置闭环

点击：

```text
通知店长复核 / 安排巡检 / 标记已复核
```

状态变化：

```text
Action needed -> Assigned / Handoff closed
```

## 3. Technical Explanation

评委问技术时使用以下口径：

```text
系统把事件统一成 Event Schema。
SDK 可用时，事件来自真实 eufy 设备；SDK 不稳定时，使用模拟事件流。
后续 AI 复核、风险排序、交接摘要和问答都走同一套流水线。
```

评委问是否已接入官方 SDK 时使用以下口径：

```text
当前不把官方 SDK 可用作为前提。系统设计了真实 SDK 和模拟事件流双路径，保证现场 Demo 稳定。
```

## 4. Fallback Plan

| 问题 | 兜底 |
| --- | --- |
| SDK 不可用 | 使用模拟事件流 |
| 网络不稳定 | 使用静态原型和预置结果 |
| AI 调用失败 | 展示固定 reviewed events 和 handoff summary |
| 原型交互异常 | 用截图 + 讲稿完成演示 |
| 视频无法播放 | 使用静态截图和文本证据 |

## 5. Pre-Demo Check

- [ ] 原型页面能打开。
- [ ] 主数字 `47 -> 1` 能看见。
- [ ] 高风险事件能展示。
- [ ] 处置动作能展示。
- [ ] 问答问题准备好。
- [ ] 处置状态能讲清楚。
- [ ] SDK 可用/不可用两套说法准备好。
- [ ] 截图兜底已准备。

## 6. Related Materials

- `docs/v2-design/00-产品定位-final-product-design.md`
- `docs/v2-design/06-工程架构-technical-architecture.md`
- `docs/v2-design/07-路演脚本-demo-script.md`
- `docs/v2-design/08-答辩QA-judge-qa.md`
