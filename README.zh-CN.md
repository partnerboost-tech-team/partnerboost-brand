[English](README.md) · **简体中文**

# PartnerBoost 商家端 API Skill

本仓库提供面向 **PartnerBoost 商家端（Merchant）** 的 AI Agent Skill：通过自然语言驱动，让 Agent 使用 `curl` 调用 PartnerBoost WebUI 同源 API，完成订单查询、业绩分析、结算与账户等操作。

核心说明与完整接口文档见仓库根目录的 [`SKILL.md`](./SKILL.md)。

## 适用场景

- 在支持本 Skill 的 Agent 环境中（如 OpenClaw、QClaw、已配置的悟空等），为商家侧自动化查询与汇总数据
- 需要统一认证方式（`X-Api-Key`）与 Base URL 时，作为单一事实来源

## Skill 元信息

| 项 | 值 |
|----|-----|
| 名称 | `partnerboost-api` |
| 版本 | 0.1.0 |
| 标签 | partnerboost, api, merchant |

## 前置条件

1. **API Key**：向 CSM 申请商家端 API Key。
2. **环境变量**：Agent 运行环境需能读取 `PARTNERBOOST_API_KEY`（见下节配置示例）。

## 安装方式

### Skills CLI（`npx skills add`）

使用 [Skills CLI](https://skills.sh/docs/cli)（无需全局安装）：

```bash
npx skills add <owner>/<repo>
```

他人直接用这个 Skill **所在仓库**在代码托管平台上的地址即可安装（GitHub 的 `owner/repo`、完整仓库 URL 等，以 [Skills CLI](https://skills.sh/docs/cli) 支持的格式为准）。把 `<owner>/<repo>` 换成仓库主页上显示的「所有者/仓库名」，例如：

```bash
npx skills add OWNER/partnerboost-brand
```

更多参数见 `npx skills add --help`（如 `-g` / `--global`、`-a` / `--agent` 指定 Cursor、Claude Code、OpenCode 等目标）。

安装完成后仍需配置 **`PARTNERBOOST_API_KEY`**（见下文 OpenClaw / QClaw，或你所用工具的密钥 / 环境变量界面）。

### OpenClaw / QClaw 共用：配置 API Key

**OpenClaw** 与 **QClaw**（腾讯基于 OpenClaw 的桌面客户端）使用同一套配置模型。在 `~/.openclaw/openclaw.json` 中为 Skill 注入环境变量，例如：

```json5
{
  skills: {
    entries: {
      "partnerboost-api": {
        env: {
          PARTNERBOOST_API_KEY: "your-api-key-here"
        }
      }
    }
  }
}
```

若文件中已有 `skills` 段，请将 `entries` 合并进现有结构，避免重复顶层键。

### OpenClaw

将本仓库中的 [`SKILL.md`](./SKILL.md) 按你的安装方式注册为名为 **`partnerboost-api`** 的 Skill（例如放到附加 Skills 目录、配置 `skills.load.extraDirs`、或使用打包/同步工具）。细则见 [OpenClaw Skills 配置](https://docs.openclaw.ai/zh-CN/tools/skills-config)。

### QClaw

**QClaw** 为腾讯出品的 OpenClaw 一键客户端（微信远程、ClawHub、可关联已有 OpenClaw 环境等）。

1. 从 [QClaw 官网](https://qclaw.qq.com) 安装并完成引导（渠道、模型等）。
2. **Skills**：在客户端左侧打开 **「Skills」**——可从 **ClawHub** 安装；若支持 **GitHub / 自定义技能**，按客户端说明添加本仓库或其中的 `SKILL.md`。
3. **本地目录**：也可将 `SKILL.md` 放入 OpenClaw 会扫描的 skills 目录（常见为 `~/.openclaw/workspace/skills/partnerboost-api/SKILL.md`），并确保该路径在 `load.extraDirs` 或默认工作区规则下生效。
4. **`PARTNERBOOST_API_KEY`**：与上一节 JSON 示例相同。

非官方中文教程与安装步骤（含「安装第一个 Skill」）：[qclaw.run](https://www.qclaw.run/zh/) · [QClaw 安装教程](https://www.qclaw.run/zh/qclaw-anzhuang-jiaocheng/)。

### 悟空（钉钉 / 阿里企业级 Agent）

**悟空**为阿里巴巴发布的企业级 AI Agent 平台（独立应用与钉钉内置能力），配套 **AI 能力市场**，并强调兼容开源 Skill 体系。

1. 在 **悟空客户端** 或 **钉钉** 侧（以你所在组织开通方式为准）进入技能 / 能力市场或组织管理员配置的 **自定义 Skill** 入口。
2. 将本仓库的 [`SKILL.md`](./SKILL.md) 按平台要求导入或上架，Skill 标识建议使用 **`partnerboost-api`**（若平台强制其他命名，以平台为准）。
3. 在平台提供的 **环境变量、密钥或 Skill 专属配置** 中填写 **`PARTNERBOOST_API_KEY`**（各版本菜单名称可能不同）。

邀测/公测范围与界面以 **钉钉 AI**、**悟空** 官方文档与公告为准。

## API 概览

- **Base URL**：`https://app.partnerboost.com`
- **认证**：所有请求需带头 `X-Api-Key: $PARTNERBOOST_API_KEY`
- **约定**：GET/POST 路径形态、JSON 响应结构（`code` / `message` / `data`）及分页字段说明均写在 [`SKILL.md`](./SKILL.md)

当前文档覆盖的主要模块：

| 模块 | 说明 |
|------|------|
| Transaction | 订单列表、统计、最近订单 |
| Performance | 图表、明细列表、汇总、合作方搜索 |
| Account / Billing | 账单列表、账户信息、当前套餐、预付款记录 |

## 仓库结构

```
.
├── README.md        # English
├── README.zh-CN.md  # 本说明（简体中文）
└── SKILL.md         # Agent Skill 定义与完整 API 参考
```

## 许可与责任

API 的使用范围、速率与合规要求以 PartnerBoost 官方及合同约定为准；请勿在日志或对话中泄露 API Key。
