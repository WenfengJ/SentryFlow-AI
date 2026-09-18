# SentryFlow AI 待补充与整改任务单

## P0：先把产品设计补完整

### 1. 丰富 `docs/v2-design/00-产品定位-final-product-design.md`

补成一份完整产品设计稿，而不是只讲定位。

必须补充：

- `产品一句话`：控制在 30 字以内。
- `核心用户`：只保留小商铺老板、仓库管理员、eufy App 用户 3 类。
- `核心场景`：闭店后一夜 47 条告警，早上只处理 1 件事。
- `用户完整路径`：收到告警、早上打开交接、查看证据、询问 AI、通知店长、关闭交接。
- `产品输出`：晨间摘要、事件解释、误报说明、风险原因、建议动作、处置状态。
- `产品边界`：不做重型 CCTV，不承诺官方 SDK，不替代安保责任人。
- `为什么适合 eufy`：运动事件、夜视、本地存储、HomeBase、eufy App、隐私能力。

验收标准：

- 打开这一份文档，就能理解产品是什么、给谁用、怎么用、当前做什么、不做什么。
- 不需要再翻 05、06、07 才能理解主线。

---

### 2. 新增 `docs/v2-design/12-product-spec.md`

这是一份最终产品规格稿，用于统一后续原型、代码、Pitch 和 Demo。

文档结构固定为：

```text
1. 产品名称
2. 一句话介绍
3. 产品定位
4. 核心用户
5. 核心场景
6. 用户流程
7. 功能模块
8. 页面结构
9. 数据输入
10. AI 输出
11. 处置闭环
12. 技术边界
13. 隐私边界
14. 可演示范围
15. 赛后产品化方向
```

必须写清楚：

- 用户每天什么时候打开它。
- 用户第一眼看到什么。
- AI 给出的结论长什么样。
- 用户能点击哪些动作。
- 哪些能力是 Demo 已有，哪些是赛后接入。

验收标准：

- 后续做 HTML 原型和后端接口时，可以直接按这份文档拆任务。

---

### 3. 新增 `docs/v2-design/13-user-flow.md`

把产品流程写成可实现的用户路径。

必须包含 3 条流程：

#### 流程 A：晨间交接

```text
用户打开 eufy App / 网页交接页
-> 看见 47 条告警已复核
-> 看见 1 条需要处理
-> 点开后门事件
-> 查看证据、原因、建议动作
-> 点击通知店长复核
-> 状态变成 Assigned
```

#### 流程 B：AI 问答

```text
用户问：昨晚有什么异常？
-> AI 只基于 reviewed_events 和 handoff_summary 回答
-> AI 指向 02:13 后门事件
-> AI 给出下一步动作
```

#### 流程 C：关闭交接

```text
用户完成复核
-> 点击标记已复核
-> 系统记录 action
-> 交接状态变成 Handoff closed
-> 今日不再重复提醒
```

验收标准：

- 每条流程都能映射到页面按钮和接口。

---

## P1：把原型从展示页升级成可执行 Demo

### 4. 丰富 `prototype/handoff-agent.html`

当前页面已经能表达概念，但需要更像真实产品 Demo。

必须补充：

- 顶部增加 `eufy Morning Handoff` 产品入口感。
- 增加 `开始昨晚事件回放` 按钮。
- 事件列表按 `Action needed / Explained / Ignored` 分组。
- 高风险事件详情展示：截图占位、时间、摄像头、区域、风险原因、证据点、建议动作。
- 问答区至少支持 3 个问题：
  - 昨晚有什么异常？
  - 今天先处理哪里？
  - 哪些是误报？
- 处置按钮至少 3 个：
  - 通知店长复核
  - 安排巡检
  - 标记已复核
- 点击处置后，状态必须变化。

验收标准：

- 不看 PPT，只打开 HTML，也能完成 3 分钟演示。
- 页面必须展示 `47 / 44 / 2 / 1`。
- 页面必须展示后门 02:13 事件。

---

### 5. 新增 `prototype/README.md`

写清楚原型怎么打开、怎么演示。

必须包含：

```text
1. 原型入口
2. 本地运行方式
3. 演示步骤
4. 演示用问题
5. 演示用按钮
6. 弱网兜底方式
7. 当前原型不包含什么
```

验收标准：

- 任何队友打开这个 README，都能自己完成一遍 Demo。

---

### 6. 新增 `app/data/` Demo 数据包

先不用做完整后端，先把数据固定下来。

需要新增：

```text
app/data/events.json
app/data/reviewed_events.json
app/data/handoff_summary.json
app/data/actions.json
```

数据要求：

- `events.json` 包含 47 条事件。
- `reviewed_events.json` 包含每条事件的复核结果。
- `handoff_summary.json` 固定输出：47 total、44 ignored、2 explained、1 action_needed。
- `actions.json` 保存用户处置动作样例。

事件类型至少覆盖：

- 后门 02:13 陌生人停留。
- 员工返回取货。
- 雨水反光。
- 昆虫靠近镜头。
- 普通路过。
- 低光或遮挡。

验收标准：

- `47 -> 1` 不是写死文案，而是能从数据统计出来。

---

## P2：补工程和 AI 规则

### 7. 新增 `docs/v2-design/14-api-data-contract.md`

定义前端和后端的数据契约。

必须定义这些接口：

```text
GET /api/events
GET /api/events/{event_id}
GET /api/handoff/today
POST /api/qa
POST /api/actions
```

每个接口必须写：

- 请求参数。
- 返回 JSON。
- 读取哪个数据文件。
- 页面哪个模块会调用。
- 失败时如何 fallback。

验收标准：

- 后续写 FastAPI 或 Node 服务时，不需要重新设计接口。

---

### 8. 新增 `docs/v2-design/15-ai-review-rules.md`

把 AI 判断逻辑写清楚。

必须包含：

#### AI 输入

- 事件截图或短视频。
- 摄像头位置。
- 事件时间。
- 营业时间。
- 是否敏感区域。
- 设备状态。

#### AI 输出

- 事件摘要。
- 风险等级。
- 误报可能。
- 证据点。
- 风险原因。
- 建议动作。
- 是否需要人工复核。

#### 风险规则

示例：

```text
营业外时间 + 后门区域 + 人员停留超过 30 秒 = high
雨水/反光/昆虫 + 无人员进入 = ignored
员工身份或正常返回取货 = explained
低光/遮挡导致无法判断 = review
```

#### 问答边界

- 只能基于当前事件回答。
- 不能编造没有发生的事件。
- 不能说“一定入侵”。
- 高风险事件必须建议人工复核。

验收标准：

- 评委问“AI 到底怎么判断”，这份文档可以直接回答。

---

### 9. 新增 `docs/v2-design/16-technical-implementation-plan.md`

把工程实现拆成可执行任务。

建议任务拆分：

```text
Task 1：整理 JSON 数据
Task 2：前端读取数据并渲染总览
Task 3：事件详情联动
Task 4：问答区基于固定数据回答
Task 5：处置按钮更新 actions 状态
Task 6：封装本地 API
Task 7：预留 eufy SDK adapter
Task 8：预留模型调用 adapter
```

验收标准：

- 每个 task 都能独立完成。
- 完成 Task 1-5 就能支撑现场 Demo。

---

## P3：补页面设计和素材

### 10. 新增 `docs/v2-design/17-page-spec.md`

把页面拆成可设计、可开发的规格。

必须包含这些页面或区域：

```text
1. 晨间交接总览
2. 事件列表
3. 事件详情
4. AI 问答
5. 处置状态
6. 技术架构展示
```

每个页面写：

- 页面目的。
- 用户看到什么。
- 用户能做什么。
- 使用哪些数据字段。
- 空状态 / 失败状态。

验收标准：

- 设计 HTML 或 React 页面时，不需要再猜页面结构。

---

### 11. 新增 `docs/v2-design/18-screenshot-shot-list.md`

截图清单用于 Pitch、提交页和视频。

必须准备这些截图：

- 总览页：47/44/2/1。
- 高风险事件详情：后门 02:13。
- AI 问答：今天先处理哪里。
- 处置状态：Assigned 或 Handoff closed。
- 架构图：SDK / Mock -> Event Schema -> AI Review -> Handoff。

验收标准：

- Pitch Deck、提交页、Demo Video 都能复用这些截图。

---

## P4：补边界、答辩和提交包

### 12. 新增 `docs/v2-design/19-limitations-disclosure.md`

单独写清楚不能夸大的地方。

必须包含：

- 当前不承诺已接入官方 eufy SDK。
- 当前 Demo 可使用模拟事件流。
- 当前不做生产级实时视频流。
- 当前不替代安保责任人。
- AI 只做复核、解释和交接，不做最终安全裁决。
- 高风险或不确定事件必须人工复核。

验收标准：

- 这份文档可以直接放进答辩备用材料。

---

### 13. 新增 `docs/v2-design/20-demo-video-storyboard.md`

准备视频脚本。

必须包含两个版本：

#### 90 秒版

结构：

```text
0-15 秒：痛点
15-35 秒：47 -> 1 总览
35-60 秒：后门事件详情
60-75 秒：AI 问答
75-90 秒：通知店长并关闭交接
```

#### 3 分钟版

结构：

```text
0:00-0:30 痛点
0:30-1:00 产品一句话
1:00-2:10 Demo 操作
2:10-2:40 技术架构
2:40-3:00 eufy 适配和收尾
```

验收标准：

- 可以直接照着录屏。

---

### 14. 新增 `docs/v2-design/21-final-submission-package.md`

最终提交索引。

必须列出：

- GitHub / repo 链接。
- Pitch Deck PDF。
- Demo Video。
- Demo Guide。
- 原型入口。
- 数据说明。
- 技术架构。
- 边界披露。
- 答辩 Q&A。

验收标准：

- 比赛提交前只看这一份，就知道还缺什么。

---

## 推荐执行顺序

### 第一阶段：补产品规格

```text
1. 丰富 00-产品定位-final-product-design.md
2. 新增 12-product-spec.md
3. 新增 13-user-flow.md
```

### 第二阶段：补 Demo 和数据

```text
4. 丰富 prototype/handoff-agent.html
5. 新增 prototype/README.md
6. 新增 app/data/events.json
7. 新增 app/data/reviewed_events.json
8. 新增 app/data/handoff_summary.json
9. 新增 app/data/actions.json
```

### 第三阶段：补工程可信度

```text
10. 新增 14-api-data-contract.md
11. 新增 15-ai-review-rules.md
12. 新增 16-technical-implementation-plan.md
```

### 第四阶段：补提交材料

```text
13. 新增 17-page-spec.md
14. 新增 18-screenshot-shot-list.md
15. 新增 19-limitations-disclosure.md
16. 新增 20-demo-video-storyboard.md
17. 新增 21-final-submission-package.md
```

## 完成标准

所有补充完成后，项目应该达到这个状态：

```text
打开 README：知道项目是什么。
打开 product spec：知道产品怎么用。
打开 prototype：能完成现场演示。
打开 app/data：能看到 47 条事件数据。
打开 api contract：能开始写后端。
打开 ai rules：知道 AI 怎么判断。
打开 final package：知道怎么提交比赛。
```
