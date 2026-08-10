---
name: follow-builders
description: "AI 行业动态速递 — 追踪顶级 AI builder 的 X/Twitter、播客和博客，生成中文摘要+英文原文的双语日报。用户说 /ai 或定时触发。"
---

# Follow Builders — AI 行业动态速递

追踪 AI 领域的顶级 builder（真正在做产品、做研究的人），每天生成中文摘要 + 英文原文的双语日报。

## 触发条件

- 用户输入 `/ai` 或要求查看 AI 行业动态
- cron 定时任务触发

## 数据来源

所有内容通过 GitHub Actions 每天自动抓取，存储为 feed JSON：
- `feed-x.json` — X/Twitter builder 推文
- `feed-podcasts.json` — 播客转录
- `feed-blogs.json` — AI 公司博客

Feed 仓库：https://github.com/Golda68/follow-builders

## 工作流程

### Step 1: 拉取数据

```bash
cd ~/.hermes/skills/follow-builders/scripts && node prepare-digest.js 2>/dev/null
```

脚本输出一个 JSON blob，包含：
- `config` — 用户语言和推送偏好
- `podcasts` — 播客转录
- `x` — builder 推文
- `blogs` — 博客文章
- `prompts` — 摘要规则
- `stats` — 数量统计

### Step 2: 检查内容

如果 `stats` 中所有计数为 0，回复"今天没有新动态，明天再看"。然后停止。

### Step 3: 生成双语摘要

**核心规则：每条内容先中文摘要，再英文原文。**

读取 `prompts` 字段中的规则：
- `prompts.digest_intro` — 整体框架
- `prompts.summarize_podcast` — 播客摘要规则
- `prompts.summarize_tweets` — 推文摘要规则
- `prompts.summarize_blogs` — 博客摘要规则
- `prompts.translate` — 翻译规则

处理顺序：推文 → 博客 → 播客

**绝对规则：**
- 不编造内容，只用 JSON 中的真实数据
- 每条内容必须有原始链接
- 不猜测职位，用 `bio` 字段

### Step 4: 交付

直接输出摘要到聊天窗口。如果用户配置了飞书推送，也创建飞书文档。

## 语言规则

**双语格式（固定）：**

每条内容：
1. **中文摘要** — 1-2 句话核心观点
2. **English original** — 英文原文关键句
3. 原始链接

技术术语保留英文：AI, LLM, GPU, API, agent, prompt, RAG 等。

## 配置

用户配置文件：`~/.follow-builders/config.json`

```json
{
  "language": "bilingual",
  "timezone": "Asia/Shanghai",
  "frequency": "daily",
  "deliveryTime": "09:00",
  "delivery": {
    "method": "stdout"
  },
  "onboardingComplete": true
}
```

配置变更通过自然语言对话处理：
- "改成每周一次" → 更新 frequency
- "换到早上8点" → 更新 deliveryTime
- "中文摘要再短一点" → 编辑 prompts 文件

## Cron 设置

Hermes cron job，每天定时触发：
- 从 GitHub feed JSON 拉取数据
- LLM 生成双语摘要
- 推送到飞书

## 信源管理

信源清单在 `config/default-sources.json`，修改后 push 到 GitHub fork。
X/Twitter 抓取需要 GitHub Secrets 中配置 `X_BEARER_TOKEN`。
播客和博客不需要额外配置。

## 自定义信源

用户可以随时要求添加/删除信源，修改 `default-sources.json` 后 push。
