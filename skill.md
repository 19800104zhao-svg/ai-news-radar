---
name: news-radar-visitor
description: 读 news.tonyzhao.com（24h 中文 AI 新闻雷达）的结构化数据，做浏览、日报、选题挖掘、问答。当用户问"今天 AI 圈有什么""帮我看看 AI 日报""有什么值得写的 AI 选题"时使用。
---

# news.tonyzhao.com 访客技能

## 这是什么
一个纯静态 AI 新闻雷达站。它只提供**结构化素材**（JSON），不做服务端推理。
你（访客 agent）负责拉数据、筛选、推理、呈现。

## 怎么取数（无鉴权，直接 GET）
1. 先读索引：GET https://news.tonyzhao.com/data/manifest.json（拿到所有端点+字段 schema）
2. 日常/日报/选题：GET https://news.tonyzhao.com/data/daily-brief.json
3. 需要全景/长尾：GET https://news.tonyzhao.com/data/stories-merged.json

## 怎么用（推理在你这边做）
- 优先级：按 items[].importance_score 降序。
- 判热度/可信度：source_count >= 2 = 多源认证，更值得信。
- 过滤类型：category（official 官方发布 / industry 行业 / watch 观察 / multi_source 多源热议）。
- 每条的多源证据在 sources[]，可交叉引用原文链接。
- 时间：earliest_at / latest_at 为 ISO8601 UTC。

## 边界
- 本站没有搜索/问答/写入接口，别去找。需要更多信息就顺着 sources[].url 去读原文。
- 数据每天更新一次，以 generated_at 为准，别假设实时。

## 典型任务：出今日 AI 选题
读 daily-brief.json → 取 importance_score Top 10 → 对每条给「一句话是什么 + 为什么值得关注（多源/官方/反常识）+ 建议切入角度」。
