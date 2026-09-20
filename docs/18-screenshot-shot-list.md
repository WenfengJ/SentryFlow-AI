# 18 Screenshot Shot List

这份清单用于统一 SentryFlow AI 的截图素材。截图会被复用到 Pitch Deck、黑客松提交页和 Demo Video。

原型入口：`prototype/handoff-agent.html`

推荐截图目录：

```text
assets/screenshots/
```

推荐尺寸：

- Desktop：`1440 x 900`
- Pitch 裁切：`16:9`
- 提交页裁切：保留页面顶部标题和核心操作区
- 视频素材：优先录制完整页面，再按镜头裁切

## 1. 截图总览

| 编号 | 文件名 | 内容 | 主要用途 |
| --- | --- | --- | --- |
| S01 | `01-overview-47-44-2-1.png` | 晨间交接总览，显示 47/44/2/1 | Pitch、提交页、视频开头 |
| S02 | `02-high-risk-backgate-0213.png` | 后门 02:13 高风险事件详情 | Pitch、提交页、答辩 |
| S03 | `03-ai-qa-next-action.png` | AI 问答：今天先处理哪里 | Pitch、视频中段 |
| S04 | `04-action-status-assigned.png` | 处置状态 Assigned | 提交页、视频闭环 |
| S05 | `05-handoff-closed.png` | 处置状态 Handoff closed | 视频结尾、提交页 |
| S06 | `06-architecture-pipeline.png` | SDK / Mock -> Event Schema -> AI Review -> Handoff | Pitch 技术页、答辩 |

最少必须准备 5 张：S01、S02、S03、S04 或 S05、S06。

## 2. S01 总览页：47/44/2/1

### 截图目的

让评委第一眼看到核心结果：47 条告警压缩成 1 个清楚动作。

### 操作步骤

1. 打开 `prototype/handoff-agent.html`。
2. 保持页面初始状态，或点击一次 `开始昨晚事件回放`。
3. 确认首屏显示四个指标：
   - `47 Total alerts`
   - `44 Ignored`
   - `2 Explained`
   - `1 Action needed`
4. 截取顶部入口区、状态条和总览指标区。

### 截图必须包含

- `SentryFlow AI`
- `eufy Morning Handoff`
- `47 条告警已复核。开门前只处理 1 件事。`
- `47 / 44 / 2 / 1`
- `后门 02:13 事件需复核`

### 避免包含

- 页面底部架构区。
- 浏览器多余标签栏。
- 与 Demo 无关的本地文件路径。

### 用途

- Pitch Deck 第 1 页或第 3 页。
- 提交页主图。
- Demo Video 前 10 秒。

### 验收标准

不看说明文字，只看这张图，也能理解：昨晚 47 条告警，早上只需要处理 1 件事。

## 3. S02 高风险事件详情：后门 02:13

### 截图目的

展示 AI 不是只做汇总，而是能解释具体事件为什么有风险。

### 操作步骤

1. 点击 `开始昨晚事件回放`。
2. 左侧选择 `Action needed` 下的 `Back Gate lingering`。
3. 确认中间详情区显示 `BackGate-01 · 02:13`。
4. 截取左侧事件列表和中间事件详情。

### 截图必须包含

- `Action needed 1`
- `Back Gate lingering`
- `BackGate-01 · 02:13`
- 时间：`02:13`
- 摄像头：`BackGate-01`
- 区域：`后门`
- 风险：`High`
- 事件解释：陌生人在后门停留
- 风险原因：闭店后、后门区域、停留时间异常
- 证据点列表

### 避免包含

- 只截摄像头占位，不截风险原因。
- 只截右侧动作区，看不到事件证据。

### 用途

- Pitch Deck 的 Demo 页。
- 提交页的功能说明图。
- 答辩时解释 AI Review。

### 验收标准

评委看到这张图后，能说出：高风险事件是后门 02:13，原因是闭店后人员停留。

## 4. S03 AI 问答：今天先处理哪里

### 截图目的

展示用户不是读长报告，而是可以直接问 AI 下一步处理什么。

### 操作步骤

1. 点击 `开始昨晚事件回放`。
2. 在右侧 `Ask SentryFlow AI` 区域点击：`今天先处理哪里？`
3. 确认回答指向后门、02:13、BackGate-01。
4. 截取右侧问答区，最好同时保留中间事件详情的一部分。

### 截图必须包含

- 问题：`今天先处理哪里？`
- 回答：先处理后门
- 原因：营业外时间、后门敏感区域、停留时间超过普通经过
- 下一步：检查后门门锁，复核 02:13 的 BackGate-01 画面

### 避免包含

- 问答区为空。
- 回答中没有具体时间和摄像头。
- 回答看起来像泛泛安全建议。

### 用途

- Pitch Deck 的 AI 能力页。
- Demo Video 中段。
- 提交页展示 AI 交互能力。

### 验收标准

截图必须证明：AI 回答不是泛聊，而是基于当前 reviewed events 和 handoff summary。

## 5. S04 处置状态：Assigned

### 截图目的

展示 AI 交接可以落到具体负责人，不只是摘要。

### 操作步骤

1. 点击 `开始昨晚事件回放`。
2. 点击 `通知店长复核`。
3. 确认状态从 `Action needed` 变为 `Assigned`。
4. 截取右侧处置区和状态条。

### 截图必须包含

- `Status: Assigned`
- `Assigned: 已通知店长复核 02:13 后门画面`
- `通知店长复核`
- `安排巡检`
- `标记已复核`
- 后门 02:13 相关动作文案

### 避免包含

- 状态仍停留在 `Action needed`。
- 只截按钮，没有状态变化结果。

### 用途

- 提交页展示闭环动作。
- Demo Video 展示操作步骤。
- Pitch Deck 的 Demo Flow 页。

### 验收标准

截图必须证明用户点击后状态发生变化，并且动作指向具体事件。

## 6. S05 处置状态：Handoff closed

### 截图目的

展示完整闭环结束：今日交接已处理完，不再重复提醒。

### 操作步骤

1. 点击 `开始昨晚事件回放`。
2. 点击 `通知店长复核`。
3. 点击 `安排巡检`。
4. 点击 `标记已复核`。
5. 再点击一次 `标记已复核`，进入 `Handoff closed`。
6. 截取右侧处置区和状态条。

### 截图必须包含

- `Status: Handoff closed`
- `Handoff closed: 后门事件已复核，今日不再重复提醒。`
- `今日交接已完成`
- 右侧交接摘要

### 避免包含

- 没有显示最终状态。
- 只显示 Reviewed，没有关闭交接。

### 用途

- Demo Video 结尾。
- 提交页展示完成闭环。
- 评委问“用户最后怎么完成处理”时使用。

### 验收标准

截图必须证明从告警到处理状态已经闭环。

## 7. S06 架构图：SDK / Mock -> Event Schema -> AI Review -> Handoff

### 截图目的

展示技术落地路径和 SDK 边界：真实 eufy 事件与 mock 事件共用同一套流水线。

### 操作步骤

1. 打开 `prototype/handoff-agent.html`。
2. 滚动到底部 `Demo pipeline` 区域。
3. 截取 `Demo pipeline` 和 `Why eufy` 两个模块。

### 截图必须包含

- `eufy / mock events`
- `Event Schema`
- `AI Review`
- `Risk Engine`
- `Handoff + Actions`
- `SDK 可替换`
- `真实接入和模拟事件共用同一套 Schema`

### 避免包含

- 只截产品 UI，不截架构节点。
- 暗示已经接入官方 eufy SDK。
- 架构节点太多导致看不清。

### 用途

- Pitch Deck 技术页。
- 答辩时解释 SDK 风险边界。
- Demo Video 技术收尾。

### 验收标准

评委看到这张图后，能理解：当前 Demo 用 mock event stream 保证稳定，赛后可通过 adapter 替换成真实 eufy 事件。

## 8. 截图命名规范

所有截图使用小写英文和编号，方便放进 Pitch 和视频工程。

```text
assets/screenshots/01-overview-47-44-2-1.png
assets/screenshots/02-high-risk-backgate-0213.png
assets/screenshots/03-ai-qa-next-action.png
assets/screenshots/04-action-status-assigned.png
assets/screenshots/05-handoff-closed.png
assets/screenshots/06-architecture-pipeline.png
```

文件不要使用：

- 中文文件名
- 空格
- `final-final` 这类临时命名
- 截图工具自动生成的长文件名

## 9. 拍摄检查

截图前检查：

- 浏览器缩放为 90% 或 100%。
- 页面宽度接近桌面演示比例。
- 不显示浏览器下载栏、控制台、书签栏。
- 不显示本地隐私路径。
- 文案没有被遮挡或截断。
- 红色风险提示只出现在高风险动作上。

截图后检查：

- S01 能看清 `47 / 44 / 2 / 1`。
- S02 能看清 `BackGate-01 · 02:13`。
- S03 能看清 `今天先处理哪里？` 和回答。
- S04 或 S05 能看清状态变化。
- S06 能看清完整架构链路。

## 10. 素材复用方式

| 素材 | Pitch Deck | 提交页 | Demo Video |
| --- | --- | --- | --- |
| S01 总览 | 封面或 Demo 页 | 主图 | 开头 |
| S02 高风险详情 | Demo 页 | 功能说明 | 事件解释段 |
| S03 AI 问答 | AI 能力页 | 交互说明 | 问答段 |
| S04 Assigned | Demo Flow 页 | 闭环说明 | 操作段 |
| S05 Handoff closed | 结尾页 | 完成状态 | 结尾 |
| S06 架构图 | 技术页 | 技术说明 | 技术收尾 |

最终提交至少使用：

```text
S01 + S02 + S03 + S04/S05 + S06
```
