---
name: home-assistant
description: "Control and query Home Assistant smart home devices via MCP. Use when: user asks about home temperature/humidity, wants to turn on/off lights, AC, fans, switches, or check device status. Triggers: '智能家居', '空调', '灯', '开关', '温度', '湿度', 'home assistant', 'HA', '家里'. NOT for: Home Assistant config/integration setup (use HA UI directly)."
---

# Home Assistant Skill

通过 Home Assistant 内置 MCP Server 控制智能家居设备。支持灯光、空调、风扇、开关、传感器查询等。

## 前置条件

Home Assistant 2025.2+ 内置 MCP Server，需先在 HA 中启用：
**设置 → 设备与服务 → 添加集成 → 搜索 "Model Context Protocol Server"**

在你的 `mcporter.json` 里加入（合并到 `mcpServers` 下）：

```json
"home-assistant": {
  "baseUrl": "http://<HA_IP>:8123/api/mcp",
  "headers": {
    "Authorization": "Bearer YOUR_HA_LONG_LIVED_TOKEN",
    "Accept": "application/json, text/event-stream"
  }
}
```

> Long-Lived Token：HA 用户头像 → 安全 → 长期访问令牌 → 创建令牌

## 常用工具

### 查询设备状态（REST API 直接查）

MCP 工具不支持直接列出传感器，用 REST API：

```bash
TOKEN="YOUR_TOKEN"
HA="http://HA_IP:8123"

# 查温湿度传感器
curl -sf -H "Authorization: Bearer $TOKEN" "$HA/api/states" | python3 -c "
import json,sys
for s in json.load(sys.stdin):
    u=s.get('attributes',{}).get('unit_of_measurement','')
    if u in ['°C','%']:
        print(s['attributes'].get('friendly_name',''), s['state']+u)
"

# 查指定实体
curl -sf -H "Authorization: Bearer $TOKEN" "$HA/api/states/climate.xxx"
```

### 开关设备

```bash
# 开（推荐用 REST API 指定 entity_id）
curl -sf -X POST -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  -d '{"entity_id": "climate.xxx"}' "$HA/api/services/climate/turn_on"

# 关
curl -sf -X POST -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" \
  -d '{"entity_id": "climate.xxx"}' "$HA/api/services/climate/turn_off"
```

### MCP 工具（通过 mcporter call）

```bash
# 开设备（按名称/区域）
mcporter call home-assistant.HassTurnOn name="卧室空调"
mcporter call home-assistant.HassTurnOn area="客厅" domain='["light"]'

# 关设备
mcporter call home-assistant.HassTurnOff name="卧室空调"

# 调灯光亮度/颜色
mcporter call home-assistant.HassLightSet name="客厅灯" color="warm white"

# 设置空调目标温度
mcporter call home-assistant.HassClimateSetTemperature name="卧室空调" temperature=26

# 设置风扇速度（百分比）
mcporter call home-assistant.HassFanSetSpeed name="风扇" percentage=50
```

## 常见 Domain 对照

| Domain | 设备类型 |
|--------|---------|
| `climate` | 空调、暖气 |
| `light` | 灯光 |
| `switch` | 开关、插座 |
| `fan` | 风扇 |
| `media_player` | 音响、电视 |
| `sensor` | 温湿度、传感器 |
| `humidifier` | 加湿器 |
| `water_heater` | 热水器 |

## 查找 entity_id

MCP 工具按名称匹配时，不一定精准。推荐先用 REST API 查出准确的 `entity_id`，再操作：

```bash
curl -sf -H "Authorization: Bearer $TOKEN" "$HA/api/states" | python3 -c "
import json,sys
for s in json.load(sys.stdin):
    if 'climate' in s['entity_id']:
        print(s['entity_id'], '|', s['attributes'].get('friendly_name',''), '|', s['state'])
"
```

## 使用建议

- **查询类**：优先用 REST API `/api/states`，数据准确
- **控制类**：MCP 工具（`HassTurnOn/Off`）按名称匹配，简单场景够用；复杂场景（多设备、精确 entity_id）用 REST API
- **心跳集成**：可在心跳中查室温/湿度，过冷/过热主动提醒
