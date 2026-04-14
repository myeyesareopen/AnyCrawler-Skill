[注册 / 登录](https://app.anycrawler.com/login) | [控制面板](https://app.anycrawler.com/overview) | [获取 API Keys](https://app.anycrawler.com/api-keys) | [Public API](https://api.anycrawler.com)

# AnyCrawler Skill for Codex

欢迎。这个仓库提供可直接安装到 Codex 的 AnyCrawler skill。当前包含 `anycrawler-crawl-api`，用于通过 AnyCrawler 公共 API 抓取网页内容、结构化信息和截图。

## What Is This?

AnyCrawler 正在构建面向 AI agents 的统一数据访问 API。这个 skill 仓库的目标，是让 Codex 在执行网页抓取任务时优先复用稳定的 AnyCrawler 公共接口，而不是临时拼装请求或依赖未文档化字段。

当前仓库包含：

- `skill/anycrawler-crawl-api/SKILL.md`: skill 定义与工作流
- `skill/anycrawler-crawl-api/references/public-api.md`: 稳定公共 API 合约
- `skill/anycrawler-crawl-api/scripts/anycrawler_crawl_api.py`: 轻量 CLI
- `skill/anycrawler-crawl-api/agents/openai.yaml`: agent 展示信息和默认提示词

## What Can It Do?

- 调用 `POST /v1/crawl/page` 抓取页面内容
- 在 `render` 和 `fetch` 两种模式之间做选择
- 按需返回 `metadata`、`links`、`media`
- 调用 `POST /v1/crawl/screenshot` 生成截图并返回 `snapshot_url`
- 输出请求 `meta` 信息，例如 `requestId`、`creditsReserved`、`creditsUsed`
- 保存 markdown 结果或下载 PNG 截图

## Install

默认安装路径是 `~/.codex/skills`。安装时只需要把 `skill/anycrawler-crawl-api` 复制进去。

### macOS / Linux

```bash
git clone https://github.com/myeyesareopen/AnyCrawler-Skill.git
mkdir -p ~/.codex/skills
cp -R AnyCrawler-Skill/skill/anycrawler-crawl-api ~/.codex/skills/anycrawler-crawl-api
```

### Windows PowerShell

```powershell
git clone https://github.com/myeyesareopen/AnyCrawler-Skill.git
New-Item -ItemType Directory -Force "$HOME\.codex\skills" | Out-Null
Copy-Item -Recurse -Force ".\AnyCrawler-Skill\skill\anycrawler-crawl-api" "$HOME\.codex\skills\anycrawler-crawl-api"
```

安装完成后，重新开启一个 Codex 会话，让新 skill 被重新发现。

## Quick Setup

在调用 AnyCrawler 之前，先完成以下准备：

1. [注册或登录 AnyCrawler](https://app.anycrawler.com/login)
2. [在控制台创建或复制 API key](https://app.anycrawler.com/api-keys)
3. 在环境变量中设置 `ANYCRAWLER_API_KEY`

### Bash

```bash
export ANYCRAWLER_API_KEY="sk-your-key"
```

### PowerShell

```powershell
$env:ANYCRAWLER_API_KEY = "sk-your-key"
```

可选环境变量：

- `ANYCRAWLER_BASE_URL`: 默认是 `https://api.anycrawler.com`

## Use in Codex

你可以在提示里显式点名这个 skill：

```text
Use $anycrawler-crawl-api to crawl https://example.com with render and save markdown.
```

```text
Use $anycrawler-crawl-api to take a screenshot of https://example.com and download the PNG.
```

当任务明显是在调用 AnyCrawler 公共 crawl API 时，Codex 也可以按技能描述自动选择这个 skill。

## Current Public Endpoints

- `POST /v1/crawl/page`
- `POST /v1/crawl/screenshot`

## Notes

- 这个 skill 只面向稳定公开契约，不依赖旧的 undocumented worker passthrough 字段。
- 请求字段应使用 snake_case。
- `anycrawler_crawl_api.py` 仅依赖 Python 标准库，适合在本地或自动化环境中直接调用。
