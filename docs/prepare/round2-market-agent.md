# Round 2: Market Agent - 产品、竞品与马来西亚市场调研

生成日期：2026-09-13  
阶段：Round 2 / Market Agent  
目标：调研 Anker / eufy 产品与 SDK、中国竞品、马来西亚市场机会，并为 Round 3 / Solution Agent 提供方向决策依据。

## 1. 承接的前序判断

本轮市场调研不是从零开始，而是承接 Round 1 / Strategy Agent 的判断，再用外部资料做验证、强化或修正。下面这张表用于告诉项目经理：上一轮哪些判断可以继续沿用，哪些需要调整。

| 编号 | Round 1 结论 | 原状态 | 本轮调研后的处理 |
| --- | --- | --- | --- |
| F-STR-001 | Anker / eufy 智能安防赛道更看重真实场景闭环，而不是纯技术展示。 | confirmed | 沿用。eufy 产品页和市场竞品调研进一步说明，基础识别能力已普遍存在，项目必须强调“理解、处置、反馈”。 |
| F-STR-002 | 评审喜欢具体场景：一个用户、一个空间、一个任务。 | likely | 强化。中国竞品已经覆盖大量泛家庭 AI 能力，项目更需要选择一个清晰场景，不宜做万能平台。 |
| F-STR-003 | 当前方向不算跑偏，但“远程能源/工业场站”表达可能太 B2B、太远、太大。 | likely | 修正并强化。马来西亚资料支持商业/工业/远程资产有机会，但 eufy 产品心智更偏家庭和小商业，因此建议从“远程能源场站平台”调整为“小商业/轻工业/远程边缘资产的 eufy AI 安防交接 Agent”。 |
| F-STR-004 | 需要确认 eufy SDK/API 或现场设备能力。 | hypothesis | 降级为高风险待确认。公开资料未确认 eufy Security 有稳定完整的第三方官方开放 SDK/API；Round 3 必须设计 SDK/API 可用与不可用两条路径。 |
| F-STR-005 | Demo 必须体现 eufy 设备能力，例如运动事件、夜视、设备状态、PTZ、灯光、隐私区。 | likely | 部分验证。eufy 官方产品资料确认夜视、4G/太阳能、360 度 PTZ、AI 检测、本地存储、设备联动等能力；但具体 SDK 能否调用这些能力仍待确认。 |
| F-STR-006 | “误报处理”和“处置建议”比单纯识别更适合安防场景。 | likely | 强化。中国竞品已经普遍具备人形、宠物、哭声、区域入侵等识别能力，差异化应放在误报复核、交接报告、自然语言问答和处置闭环。 |

## 2. 本轮带来的关键变化

本轮没有推翻 Round 1 的主判断，但带来了四处会影响后续方案设计的关键变化。

### 2.1 SDK/API 可用性不能作为默认前提

```text
原先判断：
Round 1 将 eufy SDK/API 作为待确认能力，并建议 Demo 可设计官方 SDK 可用和不可用两条路径。

本轮证据：
公开资料未确认 eufy Security 有稳定完整的第三方官方开放 SDK/API；Home Assistant 官方 EufyHome 集成不等同 eufy Security 官方开放 SDK；社区 eufy_security / eufy-security-ws 存在但不稳定或非官方。

更新后的判断：
Round 3 不能把“官方 SDK/API 可用”作为唯一主路径。必须把模拟 eufy 事件流 + 样例视频/截图作为同等重要的兜底路径。

对后续方案的影响：
技术方案要以事件抽象层开头，而不是直接绑定 SDK。Pitch 中只能说“如果现场官方 SDK/API 可用，可接入真实事件和设备状态”，不能宣称公开 SDK 已确认可用。
```

### 2.2 远程能源场站不适合作为唯一主叙事

```text
原先判断：
Round 1 认为远程能源/储能场站有差异化，但存在离 eufy 消费级安防心智较远的风险。

本轮证据：
eufy 当前公开产品更偏家庭、小商业、户外和轻量安防；马来西亚本地 CCTV 安装商覆盖 home / office / factory / store / farm 等场景；Milesight 等竞品已经覆盖 4G/solar-powered perimeter sensing 这类远程边缘场景。

更新后的判断：
项目主叙事建议从“远程能源场站 AI 巡检”修正为“面向小商业、仓库、轻工业和远程边缘资产的 eufy AI 安防交接 Agent”。太阳能/储能场站可以作为高价值 Demo 样例或扩展场景。

对后续方案的影响：
Round 3 需要重新比较“小商业/轻工业夜间守护”与“远程太阳能/储能边缘资产守护”，不要默认沿用旧 README 标题。
```

### 2.3 AI 识别能力不是差异化核心

```text
原先判断：
Round 1 已提示“处置 Agent”比“识别 Agent”更好。

本轮证据：
中国竞品已经在家庭、看店、老人、儿童、宠物、庭院等场景提供人形、人脸、宠物、哭声、区域入侵、越界、跌倒、声光报警等能力。

更新后的判断：
Round 3 的核心功能不应写成“识别人/车/宠物/烟雾”，而应写成“事件复核、风险解释、处置建议、交接报告、用户问答”。

对后续方案的影响：
MVP 必须把评审看到的第一屏从检测标签改成事件处置闭环。
```

### 2.4 马来西亚能源场景不一定只讲安防

```text
补充判断：
马来西亚靠近赤道，太阳能资源本身不是主要瓶颈。真正值得讨论的是：在高日照、热带雨季、湿热、云雨变化、远程分散部署的环境下，摄像头和 AI Agent 能不能帮助能源资产完成日常状态确认和风险交接。

本轮证据：
World Bank / Global Solar Atlas 提供马来西亚太阳辐照和 PV power potential 数据；World Bank 的全球 PV 潜力研究说明高潜力国家通常具有较低季节性，太阳能输出在月份之间相对稳定。公开能源资料也显示，马来西亚正在扩大可再生能源装机。

更新后的判断：
如果继续保留太阳能/储能场景，不一定只强调“安防”。可以把方向扩展成“能源资产守护 Agent”或“太阳能/储能场站交接 Agent”，覆盖入侵、遮挡、低光、积水/暴雨后可见性、设备区异常、巡检摘要等。

对后续方案的影响：
Round 3 应比较两种叙事：
1. AI 安防交接 Agent：更贴近 eufy 智能安防赛道，容易讲清楚。
2. AI 能源资产守护 Agent：更有差异化，可结合马来西亚太阳能资源和远程运维，但需要避免偏离 eufy 智能安防赛道。
```

## 3. 对后续方案的影响

下面这张表把本轮调研带来的变化翻译成 Round 3 / Solution Agent 必须执行的动作。

| 来源 | 原判断 | 更新后的判断 | 原因 | Round 3 必须怎么做 |
| --- | --- | --- | --- | --- |
| Round 1 Strategy | SDK/API 可用性待确认，可设计两条路径。 | 公开资料未确认稳定完整的 eufy Security 官方开放 SDK/API，模拟事件流兜底必须成为正式技术路径。 | Round 2 查到官方公开 SDK 证据不足，社区方案非官方且稳定性有风险。 | 技术架构必须先设计统一事件抽象，再分别适配真实 SDK 和模拟事件。 |
| Round 1 Strategy | 当前方向不算跑偏，但远程能源/工业场站可能太 B2B。 | 方向应从“远程能源场站平台”调整为“小商业/轻工业/仓库/远程边缘资产的 AI 安防交接 Agent”。 | eufy 产品心智更偏家庭、小商业、户外轻量安防；马来西亚本地市场也覆盖 home/office/factory/store/farm。 | Solution Agent 必须重新评分候选方向，不要默认保留旧主场景。 |
| Round 1 Strategy | 强化 eufy 原生能力。 | 已验证 eufy 有夜视、4G/太阳能、360 度 PTZ、本地 AI、本地存储、跨摄像头追踪、每日安全报告等公开能力；但 SDK 可调用性仍不确定。 | eufy 官方产品页和功能页支持产品能力，SDK/API 公开性证据不足。 | Demo 可展示这些能力概念，但实现上要准备模拟状态和模拟事件。 |
| Round 1 Strategy | 马来西亚/海外小型场站痛点需要市场证据。 | 马来西亚 CCTV 市场和本地安装商资料支持商业、工业、商铺、农场等场景存在需求，但具体 AI CCTV 渗透率和太阳能/储能痛点仍需补证。 | 市场报告和安装商资料能证明 CCTV 需求，不能完全证明 AI Agent 需求。 | Pitch 可以说“机会假设”，不要夸大马来西亚 AI CCTV 空白。 |
| Round 1 Strategy | 误报处理和处置建议是重要方向。 | 该判断被强化：基础 AI 识别已成熟，差异化应转向误报复核、交接摘要、自然语言问答和处置闭环。 | 中国竞品已有大量识别和场景技能。 | MVP 功能优先级应从“识别多类别”改为“把事件转成可执行交接”。 |
| 用户补充判断 | 马来西亚天气下太阳能资源一定不愁，是否结合能源方向的发挥点不一定是安防。 | 该判断成立，建议把太阳能/储能从“安防场景”扩展为“能源资产守护/运维交接场景”。 | 马来西亚具备太阳能资源基础；能源场景的问题更可能是远程运维、天气后状态确认、遮挡、设备区异常和交接，而不只是入侵。 | Round 3 要单独比较“AI 安防交接 Agent”和“AI 能源资产守护 Agent”，判断哪个更贴比赛、可演示、能拿奖。 |

## 4. Market Agent 总判断

SentryFlow AI 不应被包装成“从零做一个工业 CCTV AI 平台”。更适合 Anker / eufy 黑客松的市场定位是：

```text
面向小商业、仓库、轻工业和远程边缘资产的 eufy AI 安防交接 Agent。
太阳能/储能场站可以作为高价值 Demo 样例或扩展场景。
```

更精炼地说：

```text
不是再做一个“会识别”的摄像头，
而是让 eufy 摄像头在无人值守场景里完成一次安全交接：
昨晚发生了什么、哪些是真风险、今天先处理什么。
```

### 4.1 为什么这个方向更抓评委

评委很可能关注四件事：

1. 这个项目是否贴 Anker / eufy 的真实硬件和产品生态。
2. 这个项目是否解决真实用户问题，而不是炫模型能力。
3. 这个项目是否有清楚的 Demo 闭环，24 小时内能展示。
4. 这个项目是否有市场切入点，未来可能变成 eufy 的新功能、新场景或新渠道方案。

`eufy AI 安防交接 Agent` 能同时命中这四点：

- 贴硬件：它天然从 eufy 摄像头的运动事件、夜视、PTZ、4G/太阳能、本地 AI 和 HomeBase 能力出发。
- 贴场景：小商业、仓库、轻工业、农场和远程资产都有“闭店后/夜间没人盯屏”的问题。
- 贴体验：评委能在 30 秒内看懂从事件到交接报告的完整闭环。
- 贴产品：它可以变成 eufy App 里的“昨晚安全交接”“今日待处理风险”“问问昨晚发生了什么”等功能。

### 4.2 为什么马来西亚是合适切入点

马来西亚不是一个“没有摄像头”的市场，而是一个适合做轻量升级的市场：

- 本地 CCTV 安装和使用基础已经存在，用户理解摄像头价值。
- 主流销售仍以 Hikvision、Dahua、UNV、TP-Link、Ezviz 等硬件和安装包为主。
- 很多方案强调远程查看、运动检测、夜视、移动通知和基础 AI human detection。
- 小商业、仓库、办公室、工厂、农场等场景分散，适合低成本、易部署、少人值守的方案。
- 马来西亚太阳能资源基础好，太阳能/储能场站可以作为远程边缘资产的高价值样例，但项目不必只讲安防，也可以讲能源资产的状态交接和运维摘要。

这给 SentryFlow AI 的切入点是：

```text
不替代本地 CCTV 安装商，
而是在已有摄像头/轻量硬件包之上，补一个 AI 交接层。
```

### 4.3 相比竞品的优势表达

不要宣称我们比海康、大华、Milesight 更懂摄像头硬件。更好的竞品表达是：

| 竞品类型 | 它们强在哪里 | 我们避开的正面竞争 | 我们可以强调的优势 |
| --- | --- | --- | --- |
| Hikvision / Dahua / UNV | 硬件、NVR、工程项目、专业安防体系成熟 | 不做重平台，不做大型安防项目 | 更轻量、更适合小商业/远程资产的事件交接体验 |
| TP-Link / Ezviz / 小米 / 萤石 | 家庭和小商业摄像头普及，基础 AI 识别成熟 | 不比拼人形/宠物/区域入侵识别 | 把多个事件变成“今天要处理什么”的行动摘要 |
| Milesight 等工业 AI CCTV | 4G/太阳能/周界感知等远程场景成熟 | 不做完整工业 AI CCTV 系统 | 用 eufy 的易部署硬件和 App 体验切入轻量边缘场景 |
| 本地安装商 | 安装、布线、套餐、售后 | 不替代安装商 | 可作为安装包的 AI 增值层，提高客单价和差异化 |

真正的差异化不是：

```text
我们也能识别人、车、宠物、入侵。
```

而是：

```text
我们把摄像头事件整理成老板、店长、仓库管理员、远程资产负责人第二天能直接执行的交接清单。
```

### 4.4 为什么这个方向可实践

这个方向可实践，不是因为技术最炫，而是因为它贴合现有能力边界：

- eufy 现有产品能力已经覆盖 4K、太阳能、4G、360 度 PTZ、夜视、本地存储、AI 检测、跨摄像头追踪、每日报告等基础。
- 中国市场的 AI 摄像头已经在家庭、看店、母婴、宠物、老人、庭院、区域入侵、声光报警等场景高度成熟，单纯做“识别”不新。
- 马来西亚 CCTV 市场仍以 Hikvision、Dahua、UNV、TP-Link、Ezviz 等硬件和安装包为主，AI 能力在增长，但大量本地方案仍停留在安装、远程查看、运动检测、夜视、基础告警层面。
- 马来西亚太阳能资源具备基础优势，因此能源方向的发挥点不应只讲“有没有太阳”，而应讲“远程能源资产如何低成本完成状态确认、风险交接和运维摘要”。
- 最值得切入的是“低成本、易部署、少人值守的边缘场景”，而不是大型政府/城市安防，也不是中国式高度内卷的家庭摄像头场景。

### 4.5 本轮给 Round 3 的方向约束

Round 3 / Solution Agent 暂时不要展开技术实现，先围绕以下问题做方向收敛：

1. 主叙事选 `AI 安防交接 Agent`，还是 `AI 能源资产守护 Agent`？
2. 如果选安防交接，太阳能/储能是否作为高价值 Demo 样例？
3. 如果选能源资产守护，如何避免偏离 eufy 智能安防赛道？
4. 评委第一眼看到的价值，到底是“更安心”，还是“少人值守资产的状态交接”？
5. 面对马来西亚本地 CCTV 竞品，我们的优势是否能说成“轻量 AI 增值层”，而不是硬件替代？

## 5. 第一部分：Anker / eufy 产品与 SDK

### 5.1 关键发现

1. Anker 业务主线包括充电储能、智能创新、智能影音；eufy 属于智能创新类核心品牌，覆盖 Security、Clean、Mom & Baby、eufyMake 等产品系列。
2. eufy Security 的核心安防产品矩阵包括智能无线安防摄像头、PoE/NVR 系统、智能门铃、智能锁、传感器、HomeBase 本地中枢等。
3. eufy 安防卖点集中在：本地存储、无强制月费、隐私安全、4K/2K 清晰度、太阳能长续航、4G 弱网/无 Wi-Fi 场景、360 度 PTZ、AI 人/车/宠物/包裹/人脸检测、跨摄像头追踪、每日报告、设备联动。
4. 安克 2025 年年报显示，智能创新类业务实现营业收入 82.71 亿元，同比增长 30.53%，占总营收 27.11%；其中 eufy Security 围绕安防摄像头、智能门铃、智能门锁、安防传感设备形成产品矩阵。
5. 公开资料未看到稳定、完整、面向第三方开发者的 eufy Security 官方开放 SDK/API。现有 Home Assistant 相关集成更多是社区维护、非官方或依赖旧接口。
6. eufy 相关社区集成可以作为“可行性启发”，但不适合在比赛 Pitch 中宣称官方 SDK 已开放，除非现场官方明确提供。
7. 对黑客松来说，最稳妥的技术表达是“两条路径”：现场官方 SDK/API 可用则接真实设备；不可用则用模拟 eufy 事件流和样例视频完成闭环。

### 5.2 Evidence Table

| Claim | Source | Date | Confidence |
| --- | --- | --- | --- |
| eufy Security 产品页列出 PoE/NVR 系统、eufyCam、SoloCam、Floodlight、Wall Light Cam、Video Doorbell、Smart Lock 等安防产品，价格从几十美元到数千美元套装不等。 | eufy US Security Products，访问日期 2026-09-13，https://us.eufy.com/collections/security | 2026-09-13 | high |
| HomeBase S380 支持 BionicMind、本地 AI、跨摄像头追踪、每日报告、人脸/人/车/包裹/宠物检测、自动化和隐私模式。 | eufy Security Features，访问日期 2026-09-13，https://www.eufy.com/security-features | 2026-09-13 | high |
| eufyCam 主打 4K、夜视、太阳能长续航、BionicMind AI、本地存储和隐私。 | eufyCam 官方页，访问日期 2026-09-13，https://www.eufy.com/eufycam | 2026-09-13 | high |
| eufy 360 摄像头覆盖 4K、360 度 PTZ、IP67、太阳能、人/车/宠物检测、自动追踪、本地存储、无强制月费。 | eufy 360 Security Cameras，访问日期 2026-09-13，https://www.eufy.com/collections/360-security-camera | 2026-09-13 | high |
| Indoor Cam E220 支持 2K、红外夜视、360 度云台、AI 人/宠物/哭声检测、本地 microSD、可选云、NAS via RTSP、HomeKit/Alexa/Google。 | eufy Indoor Cam E220，访问日期 2026-09-13，https://www.eufy.com/products/t8410121 | 2026-09-13 | high |
| 安克 2025 年智能创新类收入 82.71 亿元，同比增长 30.53%，占总营收 27.11%；eufy Security 覆盖安防摄像头、门铃、门锁和安防传感设备。 | 安克创新 2025 年年度报告 / FinancialFilings，发布于 2026，访问日期 2026-09-13，https://financialfilings.com/filings/anker-innovations-technology-co-ltd/annual-report/2026/40078408/ | 2026-09-13 | high |
| 安克 2026 半年度智能创新类收入 41.86 亿元，高于 2025 半年度 32.51 亿元。 | 安克创新 2026 年半年度报告，访问日期 2026-09-13，https://vip.stock.finance.sina.com.cn/corp/view/vCB_AllBulletinDetail.php?id=12575150&stockid=300866 | 2026-09-13 | high |
| Home Assistant 官方的 EufyHome 集成只支持 EufyHome 产品线的 light/switch，且为 legacy community maintained，不等同 eufy Security 官方开放 SDK。 | Home Assistant EufyHome，访问日期 2026-09-13，https://www.home-assistant.io/integrations/eufy | 2026-09-13 | high |
| 社区 eufy_security 集成可管理 Eufy Security 摄像头、HomeBase、门铃、motion/contact sensors，但属于 GitHub 社区项目。 | fuatakgun/eufy_security GitHub，访问日期 2026-09-13，https://github.com/fuatakgun/eufy_security | 2026-09-13 | medium |
| bropat eufy-security-ws 明确提示旧 API 正被 Eufy 迁移/关闭，未来稳定性不保证。 | bropat/hassio-eufy-security-ws，访问日期 2026-09-13，https://github.com/bropat/hassio-eufy-security-ws/blob/master/README.md | 2026-09-13 | high |
| eufy 社区仍有用户询问 eufy Security API/HomeBase 3 API，说明公开 API 诉求存在但未被官方清晰满足。 | eufy Community API 讨论，发布于 2025-10-30，访问日期 2026-09-13，https://community.eufy.com/t/api-for-eufy-security/5759489 | 2026-09-13 | medium |

### 5.3 Implication for SentryFlow AI

eufy 已经有较强“看见”和“基础智能检测”能力，所以项目不能只做：

```text
检测人/车/宠物/包裹。
```

更应该做：

```text
把 eufy 的事件、截图、视频片段、设备状态转化成可执行处置：
风险解释、误报判断、下一步动作、巡检摘要、自然语言问答。
```

对 Demo 的启发：

- 用 HomeBase/本地 AI/隐私作为信任卖点。
- 用 4G + solar + 360 度 PTZ 支撑“远程边缘场景”。
- 用 daily report 和 cross-camera tracking 做差异化连接，但不要照搬，要升级成“运维/安防 Agent 报告”。
- SDK/API 不要赌死，现场要准备模拟事件流兜底。

### 5.4 风险和未知项

- 官方比赛现场可能提供内部 SDK，但公开网页无法确认能力边界。
- 公开社区 API 不稳定，不适合做主路径。
- eufy 已有 Daily Security Reports，项目需要强调“面向具体场景的处置闭环”，否则容易和官方已有功能撞车。

### 5.5 Recommendation

Round 3 应优先设计“事件处置 Agent”，不要做底层摄像头平台。推荐技术入口：

```text
eufy event/snapshot/video clip/device state
  -> visual analysis
  -> risk reasoning
  -> action recommendation
  -> daily briefing / user Q&A
```

## 6. 第二部分：中国竞品与 AI 摄像头成熟场景

### 6.1 关键发现

1. 中国消费级智能摄像头市场竞争高度成熟，头部品牌包括萤石、小米、乔安、普联、海雀、乐橙等；专业安防侧包括海康威视、大华、宇视、华为好望、天地伟业等。
2. 2025 年上半年，中国智能摄像头市场头部集中度明显提升；公开报告口径中，萤石、小米等处于领先位置，但不同机构和统计口径会导致排名差异。
3. 家庭、看店、宠物、母婴、老人、庭院、商铺是消费级 AI 摄像头最成熟的场景。
4. 主流 AI 能力已经包括人形检测、人脸识别、宠物检测、哭声检测、区域入侵/越界/离开区域、声光报警、智能追踪、录像筛选、跌倒检测、店铺迎宾、火焰检测等。
5. 中国竞品已经把“摄像头 + AI 技能”做到很细，尤其家庭和看店场景。黑客松如果只做家庭看护或宠物日记，容易撞题。
6. 机会不在“识别更多类别”，而在“跨事件理解 + 误报复核 + 处置建议 + 报告 + 多设备联动 + 行业化场景”。

### 6.2 Evidence Table

| Claim | Source | Date | Confidence |
| --- | --- | --- | --- |
| 2025 年上半年中国智能摄像头市场中，萤石、小米、乔安、普联、海雀等品牌位居前列，CR3/CR5 集中度提升。 | 观研报告网，访问日期 2026-09-13，https://www.chinabaogao.com/detail/768234.html | 2026-09-13 | medium |
| 洛图科技口径显示，2025 上半年中国消费级监控摄像头销量 2796 万台，销额 55.6 亿元；小米、乔安、萤石、海康威视在主流电商平台销量维度居前。 | IT之家转洛图科技，发布于 2025-08-05，访问日期 2026-09-13，https://www.ithome.com/0/873/197.htm | 2026-09-13 | high |
| 2025 Q1 全球前五消费级摄像头品牌中中国品牌占四家，萤石、小米、大华乐橙、普联位列其中。 | 智研咨询，访问日期 2026-09-13，https://www.chyxx.com/industry/1230869.html | 2026-09-13 | medium |
| 中国专业/消费级安防品牌包括海康威视、大华、华为、宇视、小米、天地伟业、360、萤石等。 | 中商情报网，发布于 2025-07-21，访问日期 2026-09-13，https://www.askci.com/news/20250721/172234275308974662362375.shtml | 2026-09-13 | medium |
| 360 摄像头 8Max AI 版宣传 AI 技能商店、人脸识别、无人出现提醒、母婴哭声侦测、宝宝虚拟围栏、宠物监测、看家看店、误报减少、声光警报等能力。 | 知乎产品体验文章，发布于 2026-08，访问日期 2026-09-13，https://www.zhihu.com/tardis/zm/art/533272746 | 2026-09-13 | medium |
| TP-Link 摄像头产品页宣传 AI 芯片、人形侦测、婴儿哭声侦测、人形/宠物录像筛选、区域入侵/越界/离开区域、智能跟踪、声光报警等能力。 | TP-Link 中国产品页，访问日期 2026-09-13，https://www.tp-link.com.cn/m/intro_1_4474.html | 2026-09-13 | high |
| 小豚当家覆盖孩子看护、宠物看护、老人看护、庭院看护、店铺看护、火焰检测等场景，宣传哭声侦测、宝宝围栏、跌倒侦测、区域布防、人脸识别、声光报警等。 | 小豚当家官网，访问日期 2026-09-13，https://www.xiaotun.cn/?about_2= | 2026-09-13 | medium |
| 中兴家庭 DICT 方案将家庭摄像头定位为家庭、庭院、宠物、商铺、仓库等场景，并强调 AI 人形/宠物识别、降低误报、远程查看、双向对讲、全彩夜视。 | 中兴通讯，访问日期 2026-09-13，https://www.zte.com.cn/china/solutions_latest/gigabit_home_broadband/home_dict.html | 2026-09-13 | high |

### 6.3 竞品矩阵

| 厂商/品牌 | 主要场景 | AI 能力 | 定位 | 对 SentryFlow AI 的启发 |
| --- | --- | --- | --- | --- |
| 萤石 | 家庭、商铺、庭院、小商业 | 人形、人脸、移动检测、云服务、智能家居联动 | 消费级头部品牌 | 家庭/商铺基础能力很成熟，SentryFlow 要避开普通家用同质化 |
| 小米 | 家庭、智能家居生态 | 人形检测、哭声检测、宠物/家人看护、米家联动 | 高性价比生态 | 单点功能不够，要强调跨设备和处置闭环 |
| 乔安 | 家庭、小店、海外消费级 | 低价摄像头、基础 AI 检测 | 性价比/海外渠道 | eufy 若打海外小场景，需强调隐私、本地、可靠和 Agent 价值 |
| TP-Link | 家庭、商铺、宠物、母婴 | AI 芯片、人形、宠物、哭声、区域入侵、越界、声光报警 | 高性价比网络设备生态 | 区域入侵和声光联动已普及，项目要升级为解释和流程推进 |
| 360 | 家庭、看店、老人、儿童、宠物 | AI 技能商店、人脸、哭声、虚拟围栏、宠物监测、误报过滤 | AI 家庭看护 | “AI 技能”模式适合借鉴，但比赛项目应聚焦一个垂直任务 |
| 小豚当家 | 华为智选生态、家庭看护 | 老人跌倒、宠物、儿童、店铺、火焰检测 | 家庭/陪伴细分 | 说明中国市场已把生活场景做细，普通家庭看护难出彩 |
| 海康威视/大华/宇视/华为好望 | 城市、园区、工厂、交通、政企 | 行业级 AI 视频结构化、周界、行为分析、平台化管理 | 专业安防 | 大型平台不适合 24 小时黑客松，SentryFlow 应做轻量边缘 Agent |

### 6.4 Implication for SentryFlow AI

中国竞品说明：

- 家庭安防、宠物、儿童、老人、看店已经很卷。
- “AI 识别 + App 告警”已经不是强创新。
- 真正有空间的是“把事件变成处置流程”，尤其在小商业、轻工业、远程场站这类没人全天盯屏的场景。

对当前方向的修正：

```text
从“远程能源场站巡检”稍微收敛成“低成本边缘资产守护”。
```

它可以覆盖：

- 小型太阳能/储能站
- 农场/仓库
- 小商业后院/停车区
- 轻工业设备区

其中 Demo 选择最直观的一个场景即可。

### 6.5 风险和未知项

- 中国市场份额来源口径差异明显，Pitch 中不要同时混用多个排名。
- 部分产品功能来自品牌宣传或体验文章，不能等同第三方评测结论。
- 中国竞品能力强，不宜把“人形识别、区域入侵、声光报警”说成独创。

### 6.6 Recommendation

Round 3 不建议选择“家庭老人/儿童/宠物看护”作为主方向，除非能做出很强情感体验。更推荐：

```text
小商业/轻工业/远程边缘资产的 AI 守护 Agent
```

核心差异化：

- 低成本部署
- 误报复核
- 非工作时段风险解释
- 多事件汇总
- 每日交接报告
- 可接 eufy 太阳能/4G/360 度摄像头

## 7. 第三部分：马来西亚市场机会

### 7.1 关键发现

1. 马来西亚 CCTV 市场仍有增长空间，公开市场报告称 2025 年 Malaysia CCTV camera market 规模约 6.48 亿美元，并预计 2026-2034 年 CAGR 18.32%；该报告为商业研究报告，数值可作为方向性参考，Pitch 中应谨慎使用。
2. 工业、商业、政府/公共安全、零售、住宅都是马来西亚 CCTV 需求来源；公开报告称工业在 2025 年终端垂直市场占比 33%，主要需求来自周界安全、运营监控、工作场所安全合规和资产保护。
3. 本地安装商页面显示，马来西亚市场主流品牌是 Hikvision、Dahua、UNV、TP-Link、Ezviz 等，常以 4/8/16 路摄像头包、NVR/DVR、硬盘、布线、安装售后打包销售。
4. 本地价格常见区间：4 摄像头系统约 RM900-RM3,500，8 摄像头约 RM1,800-RM7,000，16 摄像头约 RM5,799-RM7,899 以上，取决于地区、品牌、分辨率、布线和安装复杂度。
5. AI 功能正在进入马来西亚市场，但很多本地销售表达仍集中在 motion detection、night vision、mobile notification、remote monitoring、AI human detection、AI motion detection，Agent 式总结和处置闭环不常见。
6. 马来西亚更适合作为“海外边缘场景”验证市场，而不是与中国家庭摄像头正面对抗。

### 7.2 Evidence Table

| Claim | Source | Date | Confidence |
| --- | --- | --- | --- |
| Malaysia CCTV camera market 2025 年规模为 USD 648.08M，2034 年预计 USD 2,944.73M，2026-2034 CAGR 18.32%；工业终端占比 33%。 | IMARC Malaysia CCTV Camera Market，访问日期 2026-09-13，https://www.imarcgroup.com/malaysia-cctv-camera-market | 2026-09-13 | medium |
| IMARC 称马来西亚 CCTV 市场受公共安全、智慧城市、城市化、AI-enabled monitoring、IP 摄像头转型推动；Johor Baru 2024 年有 AI CCTV 计划用于车牌、人脸、事故、犯罪、非法倾倒监控。 | IMARC Malaysia CCTV Camera Market，访问日期 2026-09-13，https://www.imarcgroup.com/malaysia-cctv-camera-market | 2026-09-13 | medium |
| Klang Valley 2026 年 4 路 Hikvision/Dahua 系统含安装约 RM2,599-RM2,899，8 路 RM3,099-RM3,899，16 路 RM5,899-RM7,899。 | ClickBina CCTV Installation Cost Malaysia 2026，发布于 2026-08，访问日期 2026-09-13，https://clickbina.com/guides/cctv-installation-cost-malaysia/ | 2026-09-13 | medium |
| 马来西亚安装商 Bang Guard 宣传家庭和商业 CCTV，同日安装，Hikvision/Dahua，4 路约 RM2,599-RM2,899，8 路约 RM3,099-RM3,899，16 路约 RM5,899-RM7,899。 | CCTV Installation Malaysia，访问日期 2026-09-13，https://www.cctvinstallation.my/ | 2026-09-13 | medium |
| Zashtech 马来西亚 CCTV 服务面向 landed homes、bungalows、small offices、shoplots，品牌包括 Dahua、Uniview、TP-Link，8-camera IP package 起价 RM6,500。 | Zashtech CCTV System Malaysia，访问日期 2026-09-13，https://www.zashtech.com/services/cctv-installation/ | 2026-09-13 | medium |
| Camart 是 Milesight 在马来西亚的授权经销商，提供 AI CCTV、商业和工业系统集成；Milesight 有 4G solar-powered perimeter sensing camera、AI visual verification、PIR + AI tri-sensing、wireless deployment。 | Camart Milesight，访问日期 2026-09-13，https://camartcctv.com/milesight/ | 2026-09-13 | high |
| Penang 本地价格指南称 4 摄像头基础系统 RM900-RM1,800，8 摄像头 RM1,800-RM3,500，商业系统 RM3,500-RM8,000。 | Penang Renovations，发布/更新于 2026-06/07，访问日期 2026-09-13，https://penangrenovations.com/cost-guide/cctv-installation-cost-penang | 2026-09-13 | medium |
| LT Computer Solution 马来西亚 CCTV 包覆盖 Home / Office / Factory / Store / Farm，4 路 Hikvision RM1,699，8 路 Dahua RM2,699，16 路 Dahua RM5,799。 | LT Computer Solution，访问日期 2026-09-13，https://ltcomputersolution.com/advanced-cctv-installation/ | 2026-09-13 | medium |
| Kuching HJ Security 价格指南称 2-4 摄像头基础家庭包 RM1,500-RM3,500，4-8 摄像头标准家庭包 RM3,500-RM7,000，部分含 AI human detection。 | HJ Security Malaysia，发布于 2026-06-10，访问日期 2026-09-13，https://www.hikvisionhj.com/blogs/cctv-surveillance-systems-kuching/how-much-does-cctv-installation-cost-in-kuching-2024-2025-price-guide-hj-security | 2026-09-13 | medium |
| NKTSolution Kuching 套餐宣传 AI Motion Detection、Night Vision，授权品牌包括 Hikvision、Dahua、Ezviz、Supa，4-8 路套餐 RM1,950-RM3,999。 | NKTSolution，访问日期 2026-09-13，https://www.nktsolution.com/services/ | 2026-09-13 | medium |
| 6Wresearch Malaysia Smart Home Security Camera 报告覆盖 indoor/outdoor cameras、AI-powered doorbell、baby/pet monitoring、AI tracking、face recognition、motion detection、object tracking、night vision，品牌包括 Arlo、Wyze、Blink、Ring、Google Nest、Eufy 等。 | 6Wresearch，发布于 2025-04，更新于 2025-08，访问日期 2026-09-13，https://www.6wresearch.com/industry-report/malaysia-smart-home-security-camera-market | 2026-09-13 | medium |

### 7.3 本地竞品与定位

| 品牌/厂商 | 类型 | 场景 | 技术水平 | 价格/定位 | 对项目启发 |
| --- | --- | --- | --- | --- | --- |
| Hikvision | 专业安防硬件/方案 | 家庭、办公室、商铺、工厂、公共场景 | 成熟，部分支持 AcuSense/ColorVu/AI human detection | 中端到专业 | 直接竞争强，不适合正面做平台，要做轻量 Agent 增值层 |
| Dahua | 专业安防硬件/方案 | 家庭、办公室、商铺、工厂 | 成熟，WizSense、AI、人形、夜视等 | 中端到专业 | 本地接受度高，说明用户愿意买品牌硬件套装 |
| Uniview / UNV | 企业级/中高端 | 多站点、商业、工业 | AI feature set、集中管理 | 中高端 | eufy 可用“更易部署、更轻量”避开重平台 |
| TP-Link | 网络设备+摄像头 | 家庭、小办公室、商铺 | 远程管理、基础 AI | 亲民、易部署 | eufy 需强调隐私、本地 AI 和更强体验 |
| Ezviz | 消费级智能安防 | 家庭、小商业 | App、云、基础 AI | 亲民 | eufy 与其接近，但可通过 Agent 报告差异化 |
| Milesight via Camart | AI CCTV/工业方案 | 商业、工业、周界、4G/太阳能 | AI visual verification、无线、太阳能、周界感知 | 工业/项目型 | 与 SentryFlow 远程边缘资产场景高度相似，是重要竞品和参考 |
| 本地安装商/集成商 | 方案集成 | home / office / factory / store / farm | 以安装、布线、远程查看、基础 AI 为主 | 套餐化 | SentryFlow 可以作为这些硬件包的 AI 增值服务 |

### 7.4 Implication for SentryFlow AI

马来西亚市场给出的启发：

1. 市场不是没有摄像头，而是大量摄像头仍停留在“安装好、能看、能报警”。
2. AI 不是空白，但常见宣传更偏检测和告警，少见“每日交接、误报复核、自然语言询问、处置建议”。
3. 工业、小办公室、商铺、仓库、农场等场景比纯家庭更适合体现商业价值。
4. 太阳能/4G/无线周界方案已有竞品，说明远程边缘场景真实存在，但也说明必须做出 Agent 层差异化。

### 7.5 风险和未知项

- IMARC、6Wresearch 等商业报告公开页信息有限，具体份额和品牌排名需要购买报告才能确认。
- 本地安装商价格是网站报价，地区和实际工况差异较大。
- 马来西亚 AI CCTV 是否“不饱和”只能做中等可信度判断，需要更多本地案例和客户访谈。
- 太阳能/储能场站数据还不够，需要 Round 3 或后续补一轮专门的 energy-site security 证据。

### 7.6 Recommendation

Round 3 推荐优先比较以下项目方向：

1. `小商业/轻工业夜间守护 Agent`
2. `远程太阳能/储能边缘资产守护 Agent`
3. `商铺/仓库闭店后异常事件交接 Agent`
4. `家庭/小商业误报复核与每日安全摘要 Agent`

其中最有差异化但仍要贴近 eufy 的方向是：

```text
面向马来西亚小商业和远程边缘资产的 eufy AI 守护 Agent
```

可用太阳能/储能场站作为 Demo 场景之一，但不建议把唯一主标题写得过重工业化。

## 8. 对项目切入点的启发

### 8.1 不建议主打

- 泛用 AI 视频分析平台
- 单纯人形/车辆/宠物/包裹识别
- 大型智慧城市安防
- 中国式家庭摄像头全场景看护
- 复杂工业安全合规平台

### 8.2 建议主打

```text
AI Security Handoff Agent
```

中文可叫：

```text
eufy AI 安防交接 Agent
```

它解决的不是“有没有运动”，而是：

```text
昨晚哪些事件真的值得处理？
哪些是误报？
哪个区域风险最高？
应该通知谁？
今天开店/巡检前需要先看什么？
```

### 8.3 推荐场景表达

比“远程能源场站 AI 巡检”更稳的表达：

```text
为马来西亚小商业、仓库和远程资产提供闭店后/夜间 AI 安防交接。
```

太阳能/储能场站可以作为一个高价值边缘资产样例：

```text
从小商铺后门，到仓库围栏，再到小型太阳能场站，eufy 摄像头都可以从被动录像机升级为会交接风险的守护 Agent。
```

## 9. 下一轮 Solution Agent 必须决策的问题

1. 主方向到底选“小商业/轻工业夜间守护”，还是“远程太阳能/储能场站巡检”？
2. Demo 里是否保留太阳能场站？如果保留，是主场景还是扩展场景？
3. Agent 的名字和核心任务是否改成“安防交接 / Security Handoff”？
4. MVP 的三个核心事件应选哪些？
   - 夜间入侵
   - 禁区/区域进入
   - 摄像头遮挡
   - 低光/夜视不足
   - 可疑停留
   - 声光联动建议
5. SDK/API 可用时接什么能力？
   - 运动事件
   - 截图/视频片段
   - 设备状态
   - PTZ/视角切换
   - 灯光/警报
6. SDK/API 不可用时如何模拟？
   - 本地 JSON 事件流
   - 样例截图/视频
   - 模拟 eufy camera state
   - 前端一键触发事件
7. 最终 Pitch 需要避免哪些未经证实的市场说法？

## 10. 后续可复用事实清单

下面这些是后续方案设计和路演材料可以复用的事实。状态为 `confirmed` 的可以直接使用；状态为 `likely` 的可以谨慎使用，但不要写成绝对结论。

| 编号 | 结论 | 状态 | 来源 | 适用位置 | 使用提醒 |
| --- | --- | --- | --- | --- | --- |
| F-EUFY-001 | eufy Security 的公开产品矩阵覆盖摄像头、PoE/NVR、智能门铃、智能锁、传感器和 HomeBase。 | confirmed | eufy Security Products | 方案设计、路演 | 可用于说明 eufy 已有完整安防硬件入口。 |
| F-EUFY-002 | eufy 公开产品能力覆盖本地存储、隐私、4K/2K、夜视、太阳能、4G、360 度 PTZ、AI 人/车/宠物/包裹/人脸检测、跨摄像头追踪、每日报告和自动化。 | confirmed | eufy 官方产品页和功能页 | Demo 设计、路演 | 可以用于设计 Demo 的设备能力映射，但不能直接等同 SDK 可调用。 |
| F-SDK-001 | 公开资料未确认 eufy Security 有稳定完整的第三方官方开放 SDK/API。 | likely | eufy official site scan, Home Assistant EufyHome, eufy community API discussions, bropat/eufy-security-ws | 技术架构、风险兜底 | Pitch 中不能宣称公开 SDK 已确认可用；现场官方若提供 SDK，可作为路径 A。 |
| F-SDK-002 | eufy Security 社区集成存在，但属于非官方或社区维护方案，稳定性和合规性不适合作为黑客松主路径承诺。 | confirmed | fuatakgun/eufy_security, bropat/hassio-eufy-security-ws | 技术架构 | 可作为可行性启发，不建议写成官方能力。 |
| F-CN-001 | 中国智能摄像头市场在家庭、看店、宠物、母婴、老人、庭院、商铺等场景已经高度成熟。 | likely | 观研报告网、IT之家转洛图科技、TP-Link、小豚当家、中兴通讯等 | 方向取舍、竞品判断 | 不要主打普通家庭全场景看护，除非情感体验特别强。 |
| F-CN-002 | 主流中国竞品已覆盖人形、人脸、宠物、哭声、区域入侵、越界、声光报警、智能追踪、录像筛选、跌倒和火焰检测等能力。 | confirmed | TP-Link、小豚当家、中兴通讯、360 体验文章 | 差异化判断 | 项目差异化不能写成“我们能识别人/宠物/区域入侵”。 |
| F-MY-001 | 马来西亚 CCTV 市场存在家庭、商业、公共、工业、零售、农场等需求，本地安装商常以硬件包、布线、安装和售后打包销售。 | likely | IMARC, ClickBina, CCTV Installation Malaysia, Zashtech, LT Computer Solution | 市场机会、路演 | 可用于说明目标市场有摄像头购买基础。 |
| F-MY-002 | 马来西亚 CCTV 常见价格区间因地区和配置差异较大，公开安装商报价显示 4 路系统大约 RM900-RM3,500，8 路系统大约 RM1,800-RM7,000。 | likely | ClickBina, Penang Renovations, LT Computer Solution, HJ Security, NKTSolution | 路演、商业价值 | 只能作为区间参考，不要当作精确市场价格。 |
| F-MY-003 | 马来西亚 AI CCTV 正在增长，但公开销售表达仍多集中在远程查看、运动检测、夜视、移动通知、AI human detection、AI motion detection。 | likely | IMARC, local installer pages, 6Wresearch, Camart Milesight | 机会判断、路演 | Agent 式处置、交接报告和问答有表达空间，但“未普及”不能绝对化。 |
| F-MY-SOLAR-001 | 马来西亚具备可用于太阳能项目初步评估的公开太阳辐照和 PV power potential 数据，太阳能资源基础不是主要短板。 | confirmed | World Bank Data Catalog: Malaysia solar irradiation and PV power potential maps; Global Solar Atlas | 方向取舍、能源场景判断 | 可以支撑能源方向，但不要把项目讲成发电效率优化。 |
| F-MY-SOLAR-002 | 结合能源方向时，发挥点不一定是安防；更适合讲“能源资产守护/运维交接”，包括天气后状态确认、遮挡、可见性、设备区异常和风险摘要。 | hypothesis | 用户补充判断 + Market synthesis + World Bank solar data | Round 3 方案比较 | 需要 Solution Agent 判断是否比纯安防更贴合 eufy 智能安防赛道。 |
| F-OPP-001 | 项目推荐方向应从“远程能源场站 AI 视频平台”调整为“eufy AI 安防交接 Agent”，服务小商业、轻工业、仓库、农场和远程边缘资产。 | likely | Market synthesis based on eufy products, China competitors, Malaysia market | 方案收敛 | Round 3 必须重新评分并决定是否保留太阳能/储能为主场景。 |
| F-OPP-002 | 核心差异化应放在误报复核、风险解释、处置建议、每日交接报告和自然语言问答，而不是基础识别。 | confirmed | Round 1 strategy + China competitor evidence + eufy existing features | MVP、路演 | MVP 第一屏应展示处置闭环。 |

## 11. Market Agent 结论

### 11.1 关键证据素材

马来西亚市场并不是没有 AI 视频分析能力，已有竞品已经覆盖事件检测、视频检索、误报减少、跨摄像头回看、事件记录、响应流程和运营报告等能力。

| 竞品/方案 | 已有能力 | 对我们的含义 |
| --- | --- | --- |
| WyseTime / Wyse Envision | 从现有摄像头生成 operational intelligence，强调试点验收、误报数量、响应时间、报告交付和业务节省。 | “事件到运营结果”已有玩家在做，不能把交接能力说成市场空白。 |
| Spot Me Tech / VisionInsight | Detect / Search / Investigate / Respond，支持事件队列、视觉搜索、跨摄像头活动回看和团队响应。 | 马来西亚竞品已经在做从检测到响应的工作流。 |
| Methods Alliance / PreCog | 支持事件记录、快照、视频证据、优先级、工作流状态、通知、API/webhooks 和组织级报告。 | 企业级 AI 视频平台已经具备较完整的事件管理能力。 |
| SECOM Smart + Deepguard AI | 面向高风险周界，强调实时威胁检测和减少误报。 | 误报复核不是独有卖点，只能作为轻量场景体验的一部分。 |
| Hikvision / Dahua / UNV / Milesight 等 | 硬件、NVR、工程集成、周界感知、AI 检测和集中管理能力成熟。 | 不适合正面拼硬件平台，应避开重型项目制竞争。 |

### 11.2 可引用结论

“AI 视频分析”在马来西亚已经存在；真正的机会不是能力空白，而是产品形态空位。

现有方案多偏企业级平台、项目制集成和安防运营中心；eufy 更适合切入小商业、仓库、轻工业和远程边缘资产，用轻量设备 + AI 交接体验降低部署门槛。

### 11.3 推荐定位

推荐方向：

```text
面向小商业、仓库、轻工业和远程边缘资产的 eufy AI 安防交接 Agent。
太阳能/储能场站作为高价值 Demo 样例或扩展场景。
```

这个定位的重点不是证明“别人做不到”，而是证明 eufy 可以把原本偏企业级、项目制的 AI 视频能力，产品化成更轻、更易部署、更适合小场景的体验。

### 11.4 评委价值

这个方向能同时回答评委最关心的三个问题：

- eufy 设备为什么有用：它从摄像头事件、夜视、4G/太阳能、PTZ、本地 AI 等能力出发。
- AI 增量在哪里：不是再做一次识别，而是把事件组织成可理解、可复核、可交接的结果。
- 用户为什么愿意用：小商业和远程资产负责人不需要多看一个监控平台，而是获得一份能直接处理的交接摘要。

### 11.5 市场切入

马来西亚适合做切入点，因为它有摄像头安装基础，也有小商业、仓库、工厂、农场等分散场景；这些用户不一定需要重型 AI 视频平台，但需要更低门槛的值守升级。

因此，我们不是替代本地 CCTV 安装商，而是提供一个可以叠加在硬件包和安装服务之上的 AI 增值层。

本地市场已经习惯按 4/8/16 路摄像头套餐购买硬件、布线、安装和售后；这意味着更自然的商业切口不是单卖软件，而是把 AI 交接能力包装成摄像头方案的升级项。

换句话说，客户买的不是一个独立 AI 软件，而是一套安防系统。eufy 原本可以卖“摄像头 + 安装 + 远程查看”，升级后可以卖“摄像头 + 安装 + 远程查看 + AI 事件交接报告”。

### 11.6 竞品差异

面对 Hikvision / Dahua / Milesight 这类重型方案，eufy 的优势不应被包装成“更强平台”，而应是“更轻部署、更好体验、更适合小场景”。

面对小米 / 萤石 / TP-Link 这类消费级竞品，差异化也不应停在家庭识别，而是向小商业和远程资产的责任交接延伸。

eufy 还可以把本地存储、隐私、无强制月费、HomeBase 和本地 AI 作为信任卖点；这在小商业和远程资产场景里，比单纯强调“云端 AI 更强”更容易形成品牌辨识度。

### 11.7 Anker / eufy 战略适配

安克智能创新类业务仍在增长，eufy Security 已经形成摄像头、门铃、门锁、传感器和 HomeBase 的产品矩阵；黑客松项目如果能证明新场景可进入产品线，比单纯做一个 Demo 更符合主办方利益。

因此，这个方向要讲成“eufy 安防能力的场景扩展”，而不是一个独立于 eufy 生态之外的第三方平台。

### 11.8 能源场景定位

太阳能/储能适合作为加分样例，因为马来西亚太阳能资源基础好，且热带天气、遮挡、暴雨后可见性、设备区异常和远程巡检都能放大“无人值守交接”的价值。

但能源场景不宜讲成重工业平台。更稳的表达是：从小商业后门、仓库围栏，到小型太阳能/储能站，eufy 摄像头都可以成为轻量的边缘资产值守入口。

### 11.9 SDK 表达边界

公开资料无法确认 eufy Security 有稳定完整的第三方官方开放 SDK/API，因此路演中不要把“已接入官方 SDK”作为前提。

更稳的表达是：项目围绕 eufy 事件、截图、设备状态和视频片段设计能力；现场若有官方 SDK 就接真实设备，否则用模拟事件流证明产品闭环。

### 11.10 Round 3 收敛重点

Round 3 先不展开技术方案，先收敛主叙事：

- 稳妥路径：`AI 安防交接 Agent`，更贴 eufy 智能安防赛道，太阳能/储能作为高价值样例。
- 差异化路径：`AI 能源资产守护 Agent`，更有市场故事，但必须守住“摄像头理解和处置闭环”的赛题边界。

## 12. Sources

- eufy Security Products，访问日期 2026-09-13：https://us.eufy.com/collections/security
- eufy Security Features，访问日期 2026-09-13：https://www.eufy.com/security-features
- eufyCam，访问日期 2026-09-13：https://www.eufy.com/eufycam
- eufy 360 Security Cameras，访问日期 2026-09-13：https://www.eufy.com/collections/360-security-camera
- eufy Indoor Cam E220，访问日期 2026-09-13：https://www.eufy.com/products/t8410121
- 安克创新 2025 年年度报告 / FinancialFilings，访问日期 2026-09-13：https://financialfilings.com/filings/anker-innovations-technology-co-ltd/annual-report/2026/40078408/
- 安克创新 2026 年半年度报告，访问日期 2026-09-13：https://vip.stock.finance.sina.com.cn/corp/view/vCB_AllBulletinDetail.php?id=12575150&stockid=300866
- Home Assistant EufyHome，访问日期 2026-09-13：https://www.home-assistant.io/integrations/eufy
- fuatakgun/eufy_security，访问日期 2026-09-13：https://github.com/fuatakgun/eufy_security
- bropat/hassio-eufy-security-ws，访问日期 2026-09-13：https://github.com/bropat/hassio-eufy-security-ws/blob/master/README.md
- eufy Community API discussion，访问日期 2026-09-13：https://community.eufy.com/t/api-for-eufy-security/5759489
- 观研报告网：中国智能摄像头行业，访问日期 2026-09-13：https://www.chinabaogao.com/detail/768234.html
- IT之家转洛图科技，访问日期 2026-09-13：https://www.ithome.com/0/873/197.htm
- 智研咨询，访问日期 2026-09-13：https://www.chyxx.com/industry/1230869.html
- 中商情报网，访问日期 2026-09-13：https://www.askci.com/news/20250721/172234275308974662362375.shtml
- 360 摄像头 8Max AI 版体验，访问日期 2026-09-13：https://www.zhihu.com/tardis/zm/art/533272746
- TP-Link 中国产品页，访问日期 2026-09-13：https://www.tp-link.com.cn/m/intro_1_4474.html
- 小豚当家官网，访问日期 2026-09-13：https://www.xiaotun.cn/?about_2=
- 中兴通讯家庭 DICT，访问日期 2026-09-13：https://www.zte.com.cn/china/solutions_latest/gigabit_home_broadband/home_dict.html
- IMARC Malaysia CCTV Camera Market，访问日期 2026-09-13：https://www.imarcgroup.com/malaysia-cctv-camera-market
- World Bank: Solar Photovoltaic Power Potential by Country，访问日期 2026-09-13：https://www.worldbank.org/en/topic/energy/publication/solar-photovoltaic-power-potential-by-country
- World Bank Data Catalog: Malaysia - Solar irradiation and PV power potential maps，访问日期 2026-09-13：https://datacatalog.worldbank.org/search/dataset/0041753/malaysia-solar-irradiation-and-pv-power-potential-maps
- Global Solar Atlas: Global PV Potential Study，访问日期 2026-09-13：https://globalsolaratlas.info/global-pv-potential-study
- WyseTime，访问日期 2026-09-13：https://www.wysetime.com/
- Spot Me Tech VisionInsight，访问日期 2026-09-13：https://www.spotme-tech.my/visioninsight.html
- Methods Alliance PreCog Video Analytics Platform，访问日期 2026-09-13：https://www.methods-elv.com.my/video-analytics-platform.html
- SECOM Smart AI Solutions powered by Deepguard AI，访问日期 2026-09-13：https://www.secomsmart.com.my/secom-smart-ai-solutions-powered-by-deepguard-ai-dark/
- ClickBina CCTV Installation Cost Malaysia 2026，访问日期 2026-09-13：https://clickbina.com/guides/cctv-installation-cost-malaysia/
- CCTV Installation Malaysia，访问日期 2026-09-13：https://www.cctvinstallation.my/
- Zashtech CCTV System Malaysia，访问日期 2026-09-13：https://www.zashtech.com/services/cctv-installation/
- Camart Milesight，访问日期 2026-09-13：https://camartcctv.com/milesight/
- Penang Renovations CCTV cost，访问日期 2026-09-13：https://penangrenovations.com/cost-guide/cctv-installation-cost-penang
- LT Computer Solution，访问日期 2026-09-13：https://ltcomputersolution.com/advanced-cctv-installation/
- HJ Security Malaysia，访问日期 2026-09-13：https://www.hikvisionhj.com/blogs/cctv-surveillance-systems-kuching/how-much-does-cctv-installation-cost-in-kuching-2024-2025-price-guide-hj-security
- NKTSolution，访问日期 2026-09-13：https://www.nktsolution.com/services/
- 6Wresearch Malaysia Smart Home Security Camera，访问日期 2026-09-13：https://www.6wresearch.com/industry-report/malaysia-smart-home-security-camera-market
