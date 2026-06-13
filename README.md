# Tavily Claude Code 配置指南

> 用 Tavily MCP 搜索引擎替代 DeepSeek API 内置搜索，通过 CC Switch 一键配置。

## 为什么用 Tavily？

| | DeepSeek 内置搜索 | Tavily MCP |
|---|---|---|
| 搜索深度 | 基础 | 深度研究 / 爬取 / 网站地图 |
| 网页抓取 | 基础 | Markdown 提取 / 表格解析 |
| 搜索结果数 | 有限 | 可配置（5-20） |
| 免费额度 | 随 API | 每月 1000 credits |

## 前置条件

- **Node.js 20+**（`node --version` 确认）
- **Tavily API Key**（[app.tavily.com](https://app.tavily.com) 注册，免费套餐无需绑卡）
- **CC Switch**（[github.com/farion1231/cc-switch](https://github.com/farion1231/cc-switch) 下载安装）

---

## 配置步骤

### 第一步：注册 Tavily 获取 API Key

1. 访问 [app.tavily.com](https://app.tavily.com) → Sign Up
2. 选择 **Free 套餐**（每月 1000 credits）
3. Dashboard → API Keys → 复制 Key（格式 `tvly-dev-xxxxxxxx`）

### 第二步：CC Switch 添加 Tavily MCP

1. 打开 **CC Switch** → 左侧导航 **Extensions** → **MCP** 标签页
2. 点击 **「添加 MCP」**
3. 填写：

| 字段 | 值 |
|------|-----|
| MCP 类型 | **自定义** |
| MCP 标题 | `tavily` |
| 显示名称 | `Tavily Search` |
| 启用到应用 | ✅ Claude Code |

4. JSON 配置（替换你的 API Key）：

```json
{
  "transport": "stdio",
  "command": "npx",
  "args": ["-y", "tavily-mcp"],
  "env": {
    "TAVILY_API_KEY": "tvly-你的API-Key"
  }
}
```

> 📄 模板文件：[config/mcp-tavily.json](config/mcp-tavily.json)

5. 点击 **保存**

### 第三步：禁用 DeepSeek 内置搜索

编辑 `C:\Users\<用户名>\.claude\settings.json`，添加：

```json
{
  "permissions": {
    "deny": ["WebSearch", "WebFetch"]
  }
}
```

> 📄 模板文件：[config/permissions.json](config/permissions.json)

### 第四步：重启生效

关闭并重新打开 Claude Code 终端。

---

## 验证

在 Claude Code 中输入：

```
列出你当前可用的所有工具
```

**预期结果：**

- ✅ `tavily_search`、`tavily_extract`、`tavily_crawl`、`tavily_map`、`tavily_research`
- ❌ 不应出现 `WebSearch`、`WebFetch`

也可用命令行检查 MCP 连接：

```bash
claude mcp list
# tavily: cmd /c npx -y tavily-mcp - ✔ Connected
```

---

## Tavily 工具说明

| 工具 | 功能 |
|------|------|
| `tavily_search` | 实时网页搜索，支持时间/域名过滤 |
| `tavily_extract` | 提取网页内容（Markdown/纯文本） |
| `tavily_crawl` | 递归爬取网站页面 |
| `tavily_map` | 生成网站 URL 结构地图 |
| `tavily_research` | 多源深度研究，自动总结 |

---

## 故障排查

| 问题 | 解决 |
|------|------|
| MCP 工具未出现 | 执行 `claude mcp list` 检查连接状态；确认已重启终端 |
| `npx` 报错 | 确认 Node.js 20+ 已安装：`node --version` |
| API Key 无效 | 去 [app.tavily.com](https://app.tavily.com) 确认 Key 状态 |
| 额度耗尽 | 每月 1 日自动重置，或升级套餐 |

---

## 换电脑快速配置

1. 安装 CC Switch + Node.js 20+
2. 注册 Tavily 获取新 API Key
3. 将本项目 `config/` 目录下的 JSON 模板填入新 Key
4. 按第二步、第三步操作即可

---

## 免责声明

本项目仅为个人 Claude Code + Tavily MCP 配置的记录和分享。使用前请确保理解配置内容：

- 本项目**不包含**任何 API Key、Token 或私密凭证
- 配置模板中的 `tvly-你的API-Key` 需替换为你自己的 Tavily API Key
- MCP 服务器 `tavily-mcp` 从 npm 公共仓库拉取，建议使用前确认其安全性
- Tavily 搜索产生的费用由你的 Tavily 账户承担
- 本项目与 Tavily、DeepSeek、Anthropic、CC Switch 无任何关联

因使用本配置造成的任何问题，作者不承担责任。
