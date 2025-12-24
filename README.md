# SofaScore Football Match Scraper

一个使用 Selenium 无头 Chrome 浏览器爬取 SofaScore 每日足球比赛完赛数据的 Python 脚本。

## 功能特点

- 使用 Selenium 模拟无头 Chrome 浏览器访问 SofaScore
- 支持按日期范围批量爬取比赛数据
- 自动提取已完赛的足球比赛信息
- **输出两个 CSV 文件**：
  - 所有完赛比赛
  - 欧洲五大联赛完赛比赛（英超、西甲、德甲、意甲、法甲）
- 输出格式与 `top5_teams_all_matches.csv` 一致
- 支持提取球队 ID (home_team_id, away_team_id)

## 欧洲五大联赛

脚本自动识别以下联赛：

| 联赛 | 英文名称 |
|------|----------|
| 英超 | Premier League |
| 西甲 | LaLiga / La Liga |
| 德甲 | Bundesliga |
| 意甲 | Serie A |
| 法甲 | Ligue 1 |

## 环境要求

- Python 3.8+
- Google Chrome 浏览器
- ChromeDriver（自动管理）

## 安装依赖

```bash
pip install -r requirements.txt
```

## 使用方法

### 基本用法

使用默认日期范围 (2025-12-05 到 2025-12-24)，输出两个 CSV 文件：

```bash
python sofascore_scraper.py
```

默认输出：
- `sofascore_all_matches.csv` - 所有完赛比赛
- `sofascore_top5_matches.csv` - 欧洲五大联赛完赛比赛

### 自定义日期范围

```bash
python sofascore_scraper.py --start-date 2025-12-01 --end-date 2025-12-31
```

### 指定输出文件名

```bash
python sofascore_scraper.py -o all_matches.csv -t top5_matches.csv
```

### 可视化模式运行（非无头）

```bash
python sofascore_scraper.py --no-headless
```

### 所有参数

| 参数 | 短参数 | 说明 | 默认值 |
|------|--------|------|--------|
| `--start-date` | `-s` | 开始日期 (YYYY-MM-DD) | 2025-12-05 |
| `--end-date` | `-e` | 结束日期 (YYYY-MM-DD) | 2025-12-24 |
| `--output-all` | `-o` | 所有比赛输出 CSV 文件名 | sofascore_all_matches.csv |
| `--output-top5` | `-t` | 五大联赛输出 CSV 文件名 | sofascore_top5_matches.csv |
| `--no-headless` | - | 可视化模式运行 | False |

## 输出格式

两个 CSV 文件格式相同，包含以下字段：

| 字段 | 说明 |
|------|------|
| match_id | 比赛 ID |
| date | 比赛日期 |
| time | 比赛时间 |
| weekday | 星期几 |
| competition | 赛事名称 |
| season | 赛季 |
| round | 轮次 |
| venue | 主客场 |
| opponent | 对手 |
| home_team | 主队名称 |
| away_team | 客队名称 |
| home_team_id | 主队 ID |
| away_team_id | 客队 ID |
| home_goals | 主队进球 |
| away_goals | 客队进球 |
| home_ht | 主队半场进球 |
| away_ht | 客队半场进球 |
| team_goals | 球队进球 |
| opponent_goals | 对手进球 |
| result | 比赛结果 (胜/负/平) |
| status | 比赛状态 |
| match_url | 比赛详情 URL |

## 示例输出

```csv
match_id,date,time,weekday,competition,season,round,venue,opponent,home_team,away_team,home_team_id,away_team_id,home_goals,away_goals,home_ht,away_ht,team_goals,opponent_goals,result,status,match_url
14083135,2025-12-05,20:00,Friday,LaLiga,25/26,15,主场,Barcelona,Real Madrid,Barcelona,2829,2817,2,1,1,0,2,1,胜,finished,https://www.sofascore.com/real-madrid-barcelona/...
```

## 注意事项

1. **请求频率**：脚本在每个日期请求之间设置了2秒延迟，避免请求过于频繁
2. **反爬虫**：SofaScore 可能有反爬虫机制，如遇问题请适当增加延迟时间
3. **网络稳定**：确保网络连接稳定，脚本设置了20秒超时时间
4. **Chrome 版本**：确保 Chrome 浏览器版本与 ChromeDriver 兼容

## 故障排除

### ChromeDriver 版本不匹配

如果遇到 ChromeDriver 版本问题，可以使用 webdriver-manager 自动管理：

```python
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
```

### 页面加载超时

如果页面加载超时，可以尝试：
- 增加等待时间
- 检查网络连接
- 使用可视化模式调试

## License

MIT License
