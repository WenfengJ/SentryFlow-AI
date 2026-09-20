# Demo Data Package

这个目录固定 SentryFlow AI 原型和后续接口使用的演示数据。数据主线是：闭店后一夜产生 47 条 eufy 摄像头告警，AI 复核后只留下 1 条需要早上处理的后门事件。

## Files

- `events.json`：47 条原始摄像头告警，模拟 eufy motion alerts。
- `reviewed_events.json`：47 条 AI 复核结果，每条都有结论、理由、证据点和动作。
- `handoff_summary.json`：从复核结果汇总出的晨间交接摘要。
- `actions.json`：用户处置动作样例，覆盖通知、巡检、复核、关闭交接。

## Story

```text
47 total alerts
-> 44 ignored
-> 2 explained
-> 1 action_needed
-> BackGate-01 / 02:13 / 后门陌生人停留 38 秒
```

## Event Coverage

当前数据覆盖：

- 后门 02:13 陌生人停留：`after_hours_person_lingering`
- 员工返回取货：`staff_return_pickup`
- 配送车辆经过：`delivery_vehicle_passed`
- 雨水反光：`rain_reflection`
- 昆虫靠近镜头：`insect_near_lens`
- 普通路过：`normal_passerby`
- 低光或遮挡：`low_light_or_occlusion`
- 车灯扫过：`vehicle_headlight`
- 招牌反光：`signboard_light_reflection`
- 树影移动：`tree_shadow`

## Validation

可用下面的命令验证 47 -> 1 来自数据统计：

```bash
python3 - <<'CHECK'
import json
from collections import Counter
from pathlib import Path
base = Path('app/data')
events = json.loads((base / 'events.json').read_text())
reviewed = json.loads((base / 'reviewed_events.json').read_text())
print(len(events))
print(Counter(x['review_result'] for x in reviewed))
CHECK
```

期望结果：

```text
47
Counter({'ignored': 44, 'explained': 2, 'action_needed': 1})
```

## Boundary

这些数据是 Demo-safe mock events，不包含真实用户画面、真实设备凭证或真实 eufy SDK 接入结果。
