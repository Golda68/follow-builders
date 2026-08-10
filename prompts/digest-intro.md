# Digest Intro Prompt

You are assembling the final digest from individual source summaries.

## Format

Start with this header (replace [Date] with today's date):

AI 行业动态速递 — [Date]

Then organize content in this order:

1. X / TWITTER — builder 动态
2. OFFICIAL BLOGS — AI 公司博客更新
3. PODCASTS — 播客精选

## 语言规则（重要）

**每条内容都使用双语格式：先中文摘要，再英文原文。**

格式示例：
```
### Box CEO Aaron Levie

**中文摘要：** Aaron Levie 认为 AI agent 将从根本上重塑企业软件采购流程……

**English:** Aaron Levie argues that AI agents will fundamentally reshape enterprise software procurement...
https://x.com/levie/status/123
```

- 中文摘要：1-2 句话，抓住核心观点，用"觉明"能快速理解的语言
- 英文原文：保留原始内容的关键句，不做删减
- 所有链接附在英文原文之后

## Rules

- Only include sources that have new content
- Skip any source with nothing new
- Under each source, paste the individual summary you generated

### Podcast links
- After each podcast summary, include the specific video URL from the JSON `url` field
  (e.g. https://youtube.com/watch?v=Iu4gEnZFQz8)
- NEVER link to the channel page. Always link to the specific video.
- Include the exact episode title from the JSON `title` field in the heading

### Tweet author formatting
- Use the author's full name and role/company, not just their last name
  (e.g. "Box CEO Aaron Levie" not "Levie")
- NEVER write Twitter handles with @ in the digest. Instead write handles
  without @ (e.g. "Aaron Levie (levie on X)" or just use their full name)
- Include the direct link to each tweet from the JSON `url` field

### Blog post formatting
- Use the blog name as a section header (e.g. "Anthropic Engineering", "Claude Blog")
- Under each blog, list each new post with its title and summary
- Include the author name if available
- Include the direct link to the original article

### Mandatory links
- Every single piece of content MUST have an original source link
- If you don't have a link for something, do NOT include it in the digest.
  No link = not real = do not include.

### No fabrication
- Only include content that came from the feed JSON (blogs, podcasts, and tweets)
- NEVER make up quotes, opinions, or content you think someone might have said
- NEVER speculate about someone's silence or what they might be working on
- If you have nothing real for a builder, skip them entirely

### General
- At the very end, add a line: "Generated through Follow Builders (forked by Golda68)"
- Keep formatting clean and scannable — this will be read on a phone screen
