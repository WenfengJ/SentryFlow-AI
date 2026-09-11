# 调研计划 / Research Plan

## 目标 / Goal

验证 SentryFlow AI 是否是一个强黑客松方向，并找到海外能源、工业和远程场站摄像头巡检的最佳 Pitch 角度。  
Validate whether SentryFlow AI is a strong hackathon direction and identify the best pitch angle for overseas energy, industrial, and remote-site camera inspection.

## Track 1: 马来西亚工业园区和工厂 AI CCTV 需求 / Malaysia Industrial Park and Factory AI CCTV Demand

问题 / Questions:

- 马来西亚市场常见的 AI CCTV 痛点是什么？ / What AI CCTV problems are commonly marketed in Malaysia?
- 工厂和工业园区是否购买 PPE、入侵、火灾、烟雾、远程监控方案？ / Are factories and industrial parks buying PPE, intrusion, fire, smoke, or remote monitoring solutions?
- 哪些本地厂商已经提供 AI CCTV？ / Which local vendors already offer AI CCTV?
- 厂商如何描述客户痛点？ / What language do they use to describe customer pain?

证据来源 / Useful evidence:

- 厂商网站 / Vendor websites
- 案例研究 / Case studies
- 产品页 / Product pages
- 安防集成商方案 / Security integrator offerings
- 政府或工业安全资料 / Government or industrial safety references

## Track 2: 太阳能和储能场站远程巡检 / Solar and Energy Storage Site Remote Inspection

问题 / Questions:

- 太阳能场站和 BESS 场站常见的安防与巡检问题是什么？ / What are common security and inspection problems for solar farms and BESS sites?
- 远程场站多久需要一次视觉巡检？ / How often do remote sites need visual inspection?
- 哪些事件最重要：盗窃、入侵、火灾、植被、天气损坏、设备异常？ / What incidents matter most: theft, intrusion, fire, vegetation, weather damage, equipment anomalies?
- 买方是谁：业主、EPC、运营方、保险方、安防服务商？ / Who is the buyer: owner, EPC, operator, insurer, security vendor?

## Track 3: PPE 和安全合规 / PPE and Safety Compliance

问题 / Questions:

- 哪些 PPE 场景最容易用 AI CCTV 演示？ / Which PPE scenarios are easiest to demonstrate with AI CCTV?
- OSHA 风格的安全监管需求与东南亚及海外工业客户有多相关？ / How relevant are OSHA-style safety needs to Southeast Asia and overseas industrial clients?
- PPE 应该进入 MVP，还是保留为扩展场景？ / Does PPE belong in the MVP or should it remain an expansion scenario?

当前判断 / Current assumption:

PPE 对工厂和工业场景扩展有价值，但 MVP 应优先聚焦能源场站的入侵和设备区风险。  
PPE is useful for factory and industrial expansion, but the MVP should prioritize intrusion and equipment-zone risks for energy sites.

## Track 4: 矿区和远程工地 / Mines and Remote Construction Sites

问题 / Questions:

- 矿区和工地是否已经使用 AI 摄像头做边界入侵和远程监控？ / Are mines and construction sites already using AI cameras for boundary intrusion and remote monitoring?
- 哪些风险与太阳能和储能场站重叠？ / What risks overlap with solar and energy storage sites?
- 这个方向能否强化扩展叙事，同时不削弱主 Pitch？ / Can this strengthen the expansion story without weakening the main pitch?

## Track 5: eufy SDK 和设备能力 / eufy SDK and Device Capabilities

问题 / Questions:

- 是否可以访问运动事件？ / Can we access motion events?
- 是否可以访问截图或视频片段？ / Can we access snapshots or video clips?
- 是否支持隐私区？ / Are privacy zones available?
- 相关设备是否支持 PTZ？ / Is PTZ available for relevant devices?
- 是否可以控制夜视、灯光、警报或补光灯状态？ / Can night vision, light, alarm, or spotlight states be controlled?
- 哪些能力是官方支持，哪些是社区逆向能力？ / What is officially supported versus community reverse-engineered?

需要决策 / Decision needed:

如果官方 SDK 接入有限，MVP 先用样例视频和模拟 eufy 事件流跑通，再把真实设备接入描述为下一阶段。  
If official SDK access is limited, build the MVP with sample video and a simulated eufy event stream, then describe real-device integration as the next phase.

## Track 6: 竞品分析 / Competitor Mapping

待调研竞品 / Competitors to check:

- Hikvision
- Milesight
- 马来西亚本地 AI CCTV 厂商 / Local Malaysia AI CCTV vendors
- 工业 AI 视频分析平台 / Industrial AI video analytics platforms
- 太阳能场站安防监控厂商 / Solar farm security monitoring vendors

对比维度 / Comparison dimensions:

- 目标场景 / Target scene
- AI 功能 / AI features
- 硬件依赖 / Hardware dependency
- 报告能力 / Reporting capability
- Agent 解释能力 / Agent explanation capability
- 是否适合较小规模海外运营方 / Suitability for smaller overseas operators

## 调研输出格式 / Research Output Format

每个来源记录：  
For each source, record:

```text
Source / 来源:
Region / 地区:
Scene / 场景:
Pain point / 痛点:
Feature / 功能:
Evidence / 证据:
How it supports SentryFlow AI / 如何支持 SentryFlow AI:
Link / 链接:
```

## Pitch 假设 / Pitch Hypothesis

最强 Pitch 是：  
The strongest pitch is:

> 远程能源场站需要低成本、AI 辅助的视觉巡检。eufy 摄像头提供易部署硬件，SentryFlow AI 补上缺失的巡检智能：风险解释、处置建议和每日报告。  
> Remote energy sites need low-cost, AI-assisted visual inspection. eufy cameras already provide accessible hardware, while SentryFlow AI adds the missing inspection intelligence: risk explanation, suggested action, and daily reports.

