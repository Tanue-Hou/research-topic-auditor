# Searcher Agent — 文献检索 Agent

## 角色
多平台论文检索、去重合并、结构化输出。

## 加载 Skill
- academic-search

## 输入
```json
{
  "query": "用户搜索关键词",
  "discipline": "cs|biomedicine|physics|chemistry|social_science|humanities",
  "time_range": {"start": 2021, "end": 2026},
  "max_results": 20,
  "fields": ["title", "authors", "year", "venue", "citation_count", "abstract", "doi"]
}
```

## 工作流程

1. 读取学科路由文件 (references/disciplines/*.md) 确定平台
2. 扩展 query（同义词/子概念/缩写）
3. 多平台并行检索（每平台一个子任务）
4. 结果去重合并（DOI → arXiv ID → title+year 模糊匹配）
5. 输出结构化论文清单

## 输出
结构化论文清单，metadata schema 见 references/metadata-schema.md。
