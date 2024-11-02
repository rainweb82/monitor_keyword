# URL 监测脚本：关键词检测与 Telegram 通知

该 Bash 脚本用于在青龙面板中监测指定的 URL 是否包含特定关键词。当页面中未检测到关键词时，脚本会通过 Telegram 发送警告通知。此外，脚本还会检查是否出现未列入已知域名列表的新域名，并每日发送统计摘要。

## 功能特点

- **关键词监测**：监测指定 URL 页面中是否存在关键词。
- **通知提醒**：当关键词未被检测到时，通过 Telegram 发送通知。
- **新域名检查**：发现新域名时发送通知提醒，避免未授权的跳转。
- **每日统计**：每天固定时间发送监测统计摘要，提供关键词检测次数的概览。
- **自定义间隔**：支持调整检测时间间隔和每日通知时间。

## 环境变量配置

在 **青龙面板** 中配置以下环境变量，以便脚本正常运行：

1. **`URLS`**：需要监测的 URL 列表，每行一个 URL（换行分隔）。
2. **`KEYWORDS`**：对应的关键词列表，每行一个关键词，数量需与 `URLS` 一一对应（换行分隔）。
3. **`INTERVAL`**：检测时间间隔，单位为秒，默认为 60 秒。
4. **`DAILY_NOTIFICATION_TIME`**：每日通知时间，格式为 `"HH:MM"`，默认为每天上午 10:00。
5. **`BOT_TOKEN`**：Telegram 机器人的 API Token，可以设置多个（换行分隔）。
6. **`CHAT_ID`**：接收通知的 Telegram 用户 ID，可以设置多个（换行分隔）。

**注意**：
- 请确保 `URLS` 和 `KEYWORDS` 数量一致。
- `BOT_TOKEN` 和 `CHAT_ID` 数量一致，且每个 `BOT_TOKEN` 对应一个 `CHAT_ID`。

## 文件准备

1. 在脚本目录下创建 `known_domains.txt` 文件，将已知的最终跳转域名逐行列出，以避免新域名的误报。
2. 确保系统中已安装 `curl` 和 `jq`，用于 API 调用和 JSON 处理。

## 使用说明

1. 在青龙面板的 **脚本管理** 中，将脚本内容保存为 `.sh` 文件，例如 `monitor.sh`。
2. 配置好环境变量后，在青龙面板中启动脚本，即可开始监测。

## 主要功能说明

### 关键词监测

脚本会按照设定的时间间隔检查每个 URL 的页面内容，判断是否包含指定的关键词。如果关键词不存在，则计入“未检测到关键词”次数，若连续三次未检测到关键词，则会通过 Telegram 发送告警通知。

### 新域名检查

脚本会获取每个 URL 的最终跳转域名，检查该域名是否在 `known_domains.txt` 文件中。如果遇到新的域名且未在 30 分钟内发送过通知，脚本将通过 Telegram 发送提醒。

### 每日统计

脚本会在设定的时间（默认为每天上午 10:00）发送每日监测统计，包括每个 URL 的关键词检测次数和未检测到关键词的次数。

### IP 地址信息

脚本会通过公共 API 查询当前服务器的 IP 地址及其地理位置信息，并将其包含在通知中，便于接收方确认请求来源。

## 示例配置

### 青龙面板环境变量配置示例

```plaintext
URLS="https://example.com"
KEYWORDS="keyword1"
INTERVAL="120"
DAILY_NOTIFICATION_TIME="09:30"
BOT_TOKEN="your_bot_token_1"
CHAT_ID="your_chat_id_1"
```

### `known_domains.txt` 文件内容示例

```plaintext
example.com
another.com
```
