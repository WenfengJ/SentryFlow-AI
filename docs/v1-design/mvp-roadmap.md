# MVP 路线图 / MVP Roadmap

## Phase 0: 项目启动 / Project Setup

- 定义项目定位 / Define project positioning
- 创建 README 和 Pitch Brief / Create README and pitch brief
- 准备队友招募文案 / Prepare team recruitment message
- 拆分调研方向 / Split research tracks

## Phase 1: Demo 数据 / Demo Data

收集 3-5 个样例场景：  
Collect 3-5 sample scenarios:

- 夜间入侵 / Night intrusion
- 低光设备区 / Low-light equipment area
- 摄像头遮挡 / Camera obstruction
- 烟雾 / 类火光画面 / Smoke or fire-like scene
- 人员在禁区设备附近异常停留 / Abnormal human stay near restricted equipment

记录样例元数据：  
Store sample metadata:

- 场景 / Scenario
- 风险等级 / Risk level
- 预期解释 / Expected explanation
- 建议动作 / Suggested action

## Phase 2: AI 巡检 Agent / AI Inspection Agent

核心输出格式：  
Core output format:

```json
{
  "event_type": "night_intrusion",
  "risk_level": "high",
  "scene_summary": "A person appears inside the restricted equipment area at night.",
  "risk_reason": "The person is close to critical energy equipment outside normal working hours.",
  "recommended_action": "Notify the site operator and request human verification.",
  "needs_human_review": true
}
```

## Phase 3: 事件工作台 / Event Workbench

最小 UI：  
Minimum UI:

- 事件列表 / Event list
- 风险等级 / Risk level
- 摄像头 / 位置 / Camera / location
- 场景截图或视频占位 / Scene snapshot or video placeholder
- Agent 解释 / Agent explanation
- 建议动作 / Recommended action
- 标记为已复核 / Mark as reviewed

## Phase 4: 每日报告 / Daily Report

生成每日摘要：  
Generate a daily summary:

- 总事件数 / Total events
- 高风险事件数 / High-risk events
- 摄像头健康状态说明 / Camera health notes
- 未解决事项 / Unresolved issues
- 下一步动作 / Next actions

## Phase 5: 黑客松 Pitch / Hackathon Pitch

准备材料：  
Prepare:

- 3 分钟 Demo 脚本 / 3-minute demo script
- 5 页 Pitch Deck / 5-slide pitch deck
- 市场和竞品摘要 / Market and competitor summary
- eufy 能力的未来集成计划 / Future integration plan with eufy capabilities

## MVP 暂不做 / Out of Scope for MVP

- 完整生产级视频流 / Full production-grade video streaming
- 实时多摄像头扩展 / Real-time multi-camera scaling
- 认证级安全合规 / Certified safety compliance
- 复杂门禁联动流程 / Complex access-control workflow
- 在 SDK 可行性确认前完成完整 eufy SDK 集成 / Full eufy SDK integration before SDK feasibility is confirmed

