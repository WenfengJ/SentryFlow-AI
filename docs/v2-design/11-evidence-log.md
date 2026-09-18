# 11 Evidence Log / 证据账本

SentryFlow AI v2 的证据账本，用于统一路演、答辩和 Pitch Deck 中的事实引用与表达边界。

## 使用原则

```text
能确认的，说“已确认”。
只能推断的，说“较可信”。
有风险的，说“待确认”。
不要把市场机会说成市场空白。
不要把社区 SDK 说成官方 SDK。
```

## 1. eufy 产品能力

| 编号 | 事实 | 状态 | 使用方式 | 注意边界 |
| --- | --- | --- | --- | --- |
| E-EUFY-001 | eufy Security 产品矩阵覆盖摄像头、门铃、门锁、传感器和 HomeBase。 | 已确认 | 说明 eufy 有完整安防硬件入口 | 不等于所有能力都开放 API |
| E-EUFY-002 | eufy 公开产品能力包括夜视、4G/太阳能、本地存储、AI 检测、PTZ、每日安全报告等。 | 已确认 | 用于说明 Demo 贴 eufy 设备能力 | 不能直接说 SDK 可调用 |
| E-EUFY-003 | eufy 强调隐私、本地存储、本地 AI 等信任卖点。 | 较可信 | 可用于小商业信任价值表达 | 具体产品能力需按设备型号确认 |

## 2. SDK / API 风险

| 编号 | 事实 | 状态 | 使用方式 | 注意边界 |
| --- | --- | --- | --- | --- |
| E-SDK-001 | 公开资料未确认 eufy Security 有稳定完整的第三方官方开放 SDK/API。 | 较可信 | 解释为什么必须有模拟事件流兜底 | 不要说“完全没有 SDK” |
| E-SDK-002 | 社区 eufy 集成存在，但稳定性、合规性和官方支持边界不适合作为主承诺。 | 已确认 | 说明可以参考，但不作为比赛主路径 | 不要把社区方案讲成官方能力 |
| E-SDK-003 | 技术方案采用 Event Schema 抽象，SDK 可用和不可用都能进入同一流水线。 | 已确认 | 技术答辩重点 | 需要 Demo 中清楚标注 source |

## 3. 市场与竞品

| 编号 | 事实 | 状态 | 使用方式 | 注意边界 |
| --- | --- | --- | --- | --- |
| E-MKT-001 | 马来西亚有家庭、办公室、商铺、工厂、农场等 CCTV 安装需求。 | 较可信 | 说明小商业/远程边缘资产有摄像头购买基础 | 不要说市场完全空白 |
| E-MKT-002 | 本地安装商常以 4/8/16 路摄像头套餐、布线、安装、售后打包销售。 | 较可信 | 支撑“摄像头套餐增值项”商业路径 | 价格区间仅供参考 |
| E-MKT-003 | AI CCTV 已经存在，常见能力包括检测、告警、事件管理和运营报告。 | 已确认 | 防守竞品问题 | 不要说“别人做不到 AI 视频分析” |
| E-MKT-004 | 现有重型方案多偏企业平台、项目制集成和安防运营中心。 | 较可信 | 支撑 eufy 轻量产品化机会 | 需要避免一概而论 |
| E-MKT-005 | Hikvision、Dahua、UNV、Milesight 等在硬件和平台上成熟。 | 已确认 | 说明不正面拼重平台 | 不能贬低竞品 |

## 4. 产品机会

| 编号 | 事实 | 状态 | 使用方式 | 注意边界 |
| --- | --- | --- | --- | --- |
| E-OPP-001 | SentryFlow AI 的定位是 eufy AI 安防交接服务。 | 已确认 | 统一所有文档标题和路演口径 | 不再用 v1 做主线 |
| E-OPP-002 | 差异化不在基础识别，而在误报复核、风险排序、交接摘要、问答和处置闭环。 | 已确认 | 回答“有什么不同” | 不说误报复核是独有能力 |
| E-OPP-003 | 太阳能/储能适合作为高价值扩展样例，不适合作为唯一主叙事。 | 已确认 | 用于 Demo 第二场景 | 避免讲成重工业平台 |
| E-OPP-004 | “47 alerts -> 1 clear action” 是路演主故事。 | 已确认 | Demo、Pitch、视频统一使用 | 数字是 Demo 叙事设定，不代表统计结论 |

## 5. 可引用来源清单

主要来源来自项目内调研文档：

- `docs/v2-design/02-market-research.md`
- `docs/v2-design/03-solution-plan.md`
- `docs/v2-design/05-technical-demo-plan.md`
- `参赛规则.md`

外部来源已在 `02-market-research.md` 的 Sources 部分整理，包括：

- eufy Security Products / Features / eufyCam / 360 Security Cameras
- Home Assistant EufyHome
- eufy community API discussion
- fuatakgun/eufy_security
- bropat/hassio-eufy-security-ws
- Malaysia CCTV market reports and local installer pages
- Camart / Milesight references

## 6. Evidence Statements for Pitch

Approved wording:

```text
AI CCTV 已经存在，机会不是能力空白，而是产品形态空位。
```

```text
eufy 的机会不是做更重的平台，而是把事件复核和交接做成小商业也能使用的轻量体验。
```

```text
公开资料无法确认 eufy Security 有稳定完整的官方第三方 SDK，所以我们设计了 SDK 可用和不可用两条路径。
```

Avoid:

```text
马来西亚没有 AI CCTV。
```

```text
我们已经接入 eufy 官方 SDK。
```

```text
我们比 Hikvision / Dahua 更强。
```

```text
误报复核是我们的独有能力。
```
