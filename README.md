# ISFP-Data

ISFP 平台的公共数据仓库，存放机场、国家/地区、时区等基础数据，供各端（App、地图、雷达等）统一读取。

## 目录

```
airports/info/
├── airports.json              # 机场主数据，按 ICAO 索引（29303 条）
├── country_multilingual.csv   # 国家/地区名称对照表（249 条，简体中文）
└── timezone_translations.json # 常用时区中文名（36 条）
```

## 字段说明

`airports.json` 以 ICAO 代码为键：

```json
"ZBAA": {
  "icao": "ZBAA",
  "iata": "PEK",
  "name": "Beijing Capital International Airport",
  "city": "Beijing",
  "state": "Beijing",
  "country": "CN",
  "elevation": 116,
  "lat": 40.0801010132,
  "lon": 116.5849990845,
  "tz": "Asia/Shanghai"
}
```

- `iata`：无对应代码时为 `""`
- `country`：ISO 3166-1 alpha-2 代码
- `elevation`：单位英尺（ft）
- `lat` / `lon`：WGS84 十进制度
- `tz`：IANA 时区标识，夏令时由客户端时区库处理

## 使用

```python
import json
from pathlib import Path

data = json.loads(Path("airports/info/airports.json").read_text(encoding="utf-8"))
print(data["ZBAA"]["name"])
```

```js
import airports from "./airports/info/airports.json" with { type: "json" };
console.log(airports["ZBAA"].tz);
```

## 维护

- 统一 UTF-8 无 BOM，JSON 4 空格缩进
- 新增机场时保持键与 `icao` 一致（大写、唯一）
- 提交前校验 JSON 可解析：

```bash
python -c "import json; json.load(open('airports/info/airports.json', encoding='utf-8'))"
```

## 免责声明

数据来自公开机场资料整理，仅用于模拟飞行与平台展示，不可用于真实飞行或导航。
