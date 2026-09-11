# SentryFlow AI

面向海外远程能源场站的智能摄像头巡检 Agent  
AI camera inspection agent for overseas remote energy sites

> 让 eufy 摄像头成为太阳能与储能场站的 AI 巡检员。  
> Turn eufy cameras into AI inspection agents for solar and energy storage sites.

SentryFlow AI 面向远程太阳能、储能和工业场站，把摄像头事件转化为风险解释、处置建议和每日巡检摘要。  
SentryFlow AI is built for remote solar, energy storage, and industrial sites. It turns camera events into risk explanations, recommended actions, and daily inspection summaries.

## 黑客松方向 / Hackathon Direction

本项目面向 Anker / eufy 生态的黑客松挑战赛事，优先选择「远程能源 / 储能 / 太阳能场站」作为主场景，工业园区、工厂、矿区、仓库作为可扩展场景。  
This project is designed for an Anker / eufy ecosystem hackathon. The primary scenario is remote energy, storage, and solar sites, with industrial parks, factories, mines, and warehouses as expansion scenarios.

目标不是做一个通用监控大屏，而是做一个能帮助海外业主、运维团队、安防服务商降低远程巡检成本的 AI Agent。  
The goal is not a generic CCTV dashboard. The goal is an AI Agent that helps overseas site owners, operations teams, and security service providers reduce remote inspection costs.

## MVP 范围 / MVP Scope

### 1. 夜间入侵 / Night Intrusion

有人进入围栏、设备区或禁入区域时，Agent 识别事件，并生成风险解释、可能原因、处置建议和人工复核判断。  
When a person enters a fenced area, equipment zone, or restricted zone, the Agent detects the event and generates a risk explanation, possible cause, recommended action, and human-review decision.

### 2. 设备区异常 / Equipment-zone Anomaly

摄像头画面出现遮挡、低光、烟雾 / 火光、异常停留、设备区可疑活动时，系统标记风险。  
When the camera view shows obstruction, low light, smoke / fire, abnormal stay, or suspicious equipment-zone activity, the system flags the risk.

### 3. 自动巡检报告 / Automated Inspection Report

每天生成场站巡检摘要：事件次数、风险等级、摄像头 / 设备状态、待处理事项和建议优先级。  
The system generates a daily site inspection summary covering event count, risk level, camera / device status, pending issues, and recommended priority.

## 为什么重要 / Why This Matters

海外远程能源场站通常具备这些特点：场地分散、人工巡检成本高、夜间和边界安全风险明显、传统 CCTV 只记录画面不解释风险、小型业主需要低成本且易部署的远程方案。  
Remote overseas energy sites are often distributed, costly to inspect manually, exposed to night and perimeter risks, and underserved by traditional CCTV systems that record footage without explaining risks. Smaller operators need low-cost, easy-to-deploy remote solutions.

SentryFlow AI 尝试把摄像头从「被动录像设备」升级为「主动巡检 Agent」。  
SentryFlow AI upgrades cameras from passive recording devices into active inspection agents.

## 调研方向 / Research Tracks

当前需要调研 6 个方向：  
The current research work is split into six tracks:

1. 马来西亚工业园区和工厂 AI CCTV 需求 / Malaysia industrial park and factory AI CCTV demand
2. 太阳能 / 储能场站远程巡检需求 / Remote inspection needs for solar and energy storage sites
3. PPE 合规和 OSHA / 安全监管场景 / PPE compliance and OSHA-style safety scenarios
4. 矿区 / 远程工地摄像头巡检 / Camera inspection for mines and remote construction sites
5. eufy SDK 能力：隐私区、运动事件、PTZ、夜视、灯光 / eufy SDK capabilities: privacy zones, motion events, PTZ, night vision, lights
6. 竞品：Hikvision、Milesight、本地 AI CCTV 厂商、工业 AI 视频分析平台 / Competitors: Hikvision, Milesight, local AI CCTV vendors, industrial AI video analytics platforms

详情见 [research/research-plan.md](research/research-plan.md).  
See [research/research-plan.md](research/research-plan.md) for details.

## 目标用户 / Target Users

- 海外太阳能 / 储能场站业主 / Overseas solar and energy storage site owners
- 工业园区和工厂安全负责人 / Safety managers in industrial parks and factories
- 远程工地和矿区管理方 / Managers of remote construction sites and mines
- 安防服务商 / Security service providers
- eufy / Anker 海外硬件渠道和方案团队 / eufy / Anker overseas hardware and solution teams

## 招募角色 / Team Roles Wanted

计划招募 2-3 位队友：  
Looking for 2-3 teammates:

- AI / CV Engineer: 视频事件识别、风险解释、多模态模型调用 / video event recognition, risk explanation, multimodal model integration
- Frontend / Product Engineer: Demo 页面、巡检报告、事件工作台 / demo UI, inspection report, event workbench
- IoT / Backend Engineer: 摄像头事件接入、数据流、定时报告、部署 / camera event ingestion, data flow, scheduled reports, deployment
- Product / Research Partner: 海外场景调研、竞品分析、Pitch 材料 / overseas market research, competitor analysis, pitch materials

## Demo 流程 / Suggested Demo Flow

1. 上传或接入一段夜间能源场站摄像头画面 / Upload or connect a night-time remote energy site camera clip
2. 系统识别入侵、低光、遮挡、烟雾等风险 / Detect intrusion, low light, obstruction, smoke, and other risks
3. Agent 输出风险解释和处置建议 / Generate risk explanation and recommended action
4. Dashboard 展示事件列表和风险等级 / Show events and risk levels in a dashboard
5. 自动生成一份每日场站巡检报告 / Generate a daily site inspection report

## 项目结构 / Repository Structure

```text
SentryFlow-AI/
  README.md
  docs/
    hackathon-brief.md
    mvp-roadmap.md
    on-site-recruiting-script.md
    team-recruitment.md
  product/
    user-stories.md
  prompts/
    inspection-agent-prompts.md
  refer/
    anker-eufy-smart-security-official-brief.md
  research/
    research-plan.md
```

## 当前状态 / Status

当前阶段：方向验证、队友招募、MVP 规划。  
Current phase: idea validation, team recruiting, and MVP planning.
# SentryFlow-AI
