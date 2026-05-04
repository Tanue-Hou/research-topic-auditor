<p align="center">
  <strong style="font-size:2.2em; vertical-align:middle;">Research Topic Auditor</strong>
</p>

<p align="center">科研选题审查 Skill —— 不只搜论文，更帮你找方向</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-v0.1.0-0f766e" />
  <img src="https://img.shields.io/badge/license-MIT-1f2937" />
  <img src="https://img.shields.io/badge/status-building-yellow" />
  <img src="https://img.shields.io/badge/built_on-academic_search_v1.2.0-blue" />
</p>

<p align="center">🌐 <a href="README.en.md">English</a> | 简体中文</p>

---

> 🚧 **本项目基于 [Mingyue-Cheng/academic-search](https://github.com/Mingyue-Cheng/academic-search) v1.2.0（作者：Mingyue Cheng，MIT License）进行扩展开发。**
> 原项目 `academic-search` 提供了多平台学术搜索基础设施。**research-topic-auditor** 在此基础上构建 **选题审查** 能力，形成双层架构：底层搜索 + 上层审查。

## News

- `2026-05-04` Fork 并初始化项目，发布详细需求清单与能力规划
- `2026-05-01` 上游 v1.2.0：多学科使用指引、CNKI 支持、前沿性排序、Query 扩展、PDF 直取

---

🚀 **底层引擎**：arXiv、Semantic Scholar、OpenAlex、Crossref、Unpaywall、Google Scholar、CNKI... 10+ 学术平台协同检索。
📊 **审查核心**：前沿趋势分析、研究空白识别、创新性评估、选题建议，五步工作流驱动。
📑 **获取稳**：开放获取 PDF 级联获取，明确标注机构权限和反爬限制。  
🎯 **策略精**：时效性优先排序，自带 CCF 等级标注，只看最值得看的顶会干货。  
💡 **选题新**：研究热点检测、Citation Burst 分析、文献聚类、多智能体选题建议。

## 项目定位

```
academic-search (上游)         → 多平台学术搜索基础设施
         +
research-topic-auditor (新增)  → 科研选题审查五步法
         =
从搜索到选题的一站式科研审查助手
```

## Quick Start

```bash
# 克隆本仓库到 Claude Code skills 目录
git clone git@github.com:Tanue-Hou/research-topic-auditor.git ~/.claude/skills/research-topic-auditor
# 检查环境依赖
bash ~/.claude/skills/research-topic-auditor/scripts/check-deps.sh
```

然后直接对 Claude Code 说：

```
审查选题：基于图神经网络的时序预测方法研究，帮我分析前沿趋势和空白
```

或者：

```
帮我推荐 3 个 NLP 方向有潜力的研究课题
```

---

## 核心能力

### 搜索与获取（由上游提供 ✅）

**检索与筛选**
- 学科路由：按 CS/AI、医学/生命科学、物理/数学、化学/材料、社科/经济、人文/法律选择检索源和评价标准
- 两遍策略：先输出轻量摘要表，用户确认核心论文后再深拉完整元数据
- Query 扩展：自动展开 2-3 个互补 query，覆盖率比单 query 提升 30-50%
- 前沿性排序：**时效性优先**（近 6 月 `[新]` 置顶）→ 引用数 → CCF 等级
- 多平台结果以 DOI/arXiv ID 为主键自动去重合并

**数据获取**
- OA PDF 级联获取（arXiv 直链 → S2 → OpenAlex → Unpaywall → 领域预印本）
- 全文状态标注：`open_pdf` / `needs_institution` / `no_open_pdf` / `anti_bot_blocked` / `html_not_pdf`
- BibTeX 导出：平台原生导出 + 字段拼装双路径
- 跨学科元数据：Crossref / OpenAlex / Unpaywall 补全
- 代码链接：Papers with Code API 自动补全

**可靠性与扩展**
- 失败信号处理：429 / 超时 / 空结果各有对应策略
- CDP 浏览器模式：Google Scholar、CNKI 等强反爬平台
- 并行分治：多目标分发子 Agent 并行执行
- 站点经验预置：14+ 平台出版商操作经验文件

### 选题发现（新增 🚧）

| 能力 | 状态 | 说明 |
|------|:----:|------|
| 热点检测 | 🚧 规划中 | 关键词频率趋势、Citation Burst 检测 |
| 文献聚类 | 🚧 规划中 | 语义聚类、研究方向树 |
| 趋势分析 | 🚧 规划中 | 年度热点变迁、新兴方向预警 |
| 研究空白识别 | 🚧 规划中 | 方法-任务矩阵、交叉方向空白区 |
| 论文推荐 | 🚧 规划中 | 基于种子论文的推荐 |
| 综述生成 | 🚧 规划中 | 搜索→聚类→结构化综述初稿 |
| 学术网络分析 | 🚧 规划中 | 作者/机构合作网络、引用网络 |

详见 [需求清单与能力规划](需求清单与能力规划.md) 了解完整路线图。

## 多学科使用方式

按学科选择检索源、query expansion、排序规则和输出字段：

| 学科 | 重点能力 |
|------|----------|
| CS / AI | arXiv、Semantic Scholar、ACM/IEEE、Papers with Code、CCF/顶会标注 |
| 医学 / 生命科学 | PubMed、Europe PMC、MeSH、系统综述/RCT 等证据等级 |
| 物理 / 数学 | arXiv 分类、MSC、NASA ADS / INSPIRE HEP 方向预留 |
| 化学 / 材料 | Crossref、OpenAlex、ChemRxiv、ACS/RSC/Springer/Wiley 访问状态 |
| 社科 / 经济 | JEL、RePEc/NBER/SSRN、方法类型和工作论文状态 |
| 人文 / 法律 | 图书/章节/档案/法律来源优先，引用数仅作辅助 |

---

## 安装

本 Skill 需要先安装底层搜索基础设施（academic-search），再安装上层审查 Skill。

```bash
# 步骤一：安装底层搜索 Skill（academic-search）
git clone https://github.com/Mingyue-Cheng/academic-search ~/.claude/skills/academic-search

# 步骤二：安装本 Skill（research-topic-auditor）
git clone git@github.com:Tanue-Hou/research-topic-auditor.git ~/.claude/skills/research-topic-auditor

# 步骤三：检查环境依赖
bash ~/.claude/skills/research-topic-auditor/scripts/check-deps.sh
```

**前置要求（仅 CDP 模式需要）**：arXiv / S2 / PubMed 等 API 平台直接可用，无需配置。如需访问 Google Scholar，需开启 Chrome 远程调试：

1. 打开 `chrome://inspect/#remote-debugging`
2. 勾选 **Allow remote debugging for this browser instance**

---

## 平台访问策略

Open API 优先，Google Scholar 与 CNKI 等无公开 API 或强反爬平台需要 Chrome 远程调试：

| 平台 | 访问方式 |
|------|---------|
| arXiv | REST API |
| Semantic Scholar | REST API |
| Crossref | REST API |
| OpenAlex | REST API |
| Unpaywall | REST API |
| PubMed | NCBI E-utilities |
| Papers with Code | REST API |
| ACM DL | WebFetch + Jina |
| IEEE Xplore | WebFetch / Jina / 官方 API |
| ScienceDirect / Wiley / Springer / ACS | 开放获取判定 + 机构访问提示 |
| **Google Scholar** | **CDP 浏览器（需 Chrome 调试）** |
| **CNKI（知网）** | **CDP 浏览器（需 Chrome 调试）** |

全文获取只针对合法开放访问来源。商业出版商页面可访问不代表 PDF 可下载；遇到需要机构权限、Cloudflare、验证码或 PDF 路由返回 HTML 时，Skill 会报告状态而不是继续尝试绕过限制。

---

## 项目结构

```
research-topic-auditor/
├── SKILL.md                    # 主指令文件（双层架构：搜索基础设施 + 选题审查）
├── 需求清单与能力规划.md        # 完整需求文档与路线图
├── .claude/settings.json       # Claude Code 项目配置
├── scripts/
│   ├── cdp-proxy.mjs           # CDP Proxy（直连用户 Chrome）
│   ├── check-deps.sh           # 环境检查 + 自动启动 Proxy
│   ├── self-test.sh            # 本地回归测试
│   └── release-test.sh         # 发布前测试
├── agents/
│   ├── openai.yaml             # OpenAI 兼容 API 配置
│   └── auditor/                # 选题审查多智能体模板
│       ├── searcher.md         #   文献检索 Agent
│       ├── frontier-analyst.md #   前沿分析 Agent
│       ├── gap-analyst.md      #   空白识别 Agent
│       ├── novelty-judge.md    #   创新性评估 Agent
│       └── synthesizer.md      #   综合建议 Agent
├── references/
│   ├── api-cookbook.md         # 多平台 API 调用速查
│   ├── metadata-schema.md      # 跨平台统一元数据 schema
│   ├── venue-rankings.md       # CS 会议/期刊 CCF 分级速查
│   ├── cdp-api.md              # CDP Proxy HTTP API 完整参考
│   ├── disciplines/            # 多学科学科路由与 query expansion
│   ├── rankings/               # 非 CS 学科评价/证据等级
│   ├── site-patterns/          # 平台与出版商操作经验文件
│   ├── workflows/              # 系统综述等工作流
│   └── auditor/                # 选题审查方法论
│       ├── frontier-analysis.md
│       ├── gap-identification.md
│       ├── novelty-assessment.md
│       ├── topic-proposal.md
│       └── multi-agent-workflow.md
├── workflows/
│   └── topic-audit.md          # 完整选题审查工作流
└── docs/
    ├── skill-usage-comparison.md
    └── multidisciplinary-improvement-analysis.md
```

## 功能路线图

```
Phase 1 (1-4周)            Phase 2 (4-8周)           Phase 3 (8-12周)
┌────────────────┐        ┌────────────────┐        ┌────────────────┐
│ 平台完善       │        │ 选题发现核心   │        │ 扩展与体验     │
│                │        │                │        │                │
│ · 万方/维普    │        │ · Citation     │        │ · 综述初稿生成 │
│ · 中英互译     │        │   Burst 检测   │        │ · 网络分析     │
│ · RIS/Zotero   │ ─────→ │ · 研究空白识别  │ ─────→ │ · MCP Server   │
│ · 意图拆解     │        │ · 论文推荐     │        │ · 会话持久化   │
│ · 论文主题聚类 │        │ · 基金关联     │        │ · 定时检索     │
│ · 热点变迁分析 │        │ · 插件接口     │        │ · 交互筛选     │
└────────────────┘        └────────────────┘        └────────────────┘
```

## 使用方法

本 Skill 的核心场景是选题审查，以下是一些典型用法：

```
审查选题：基于图神经网络的时序预测方法研究
```
```
帮我分析一下大语言模型在医疗领域的研究前沿和空白
```
```
评估这个 idea 的创新性：用扩散模型做分子构象生成
```
```
帮我推荐 3 个 NLP 方向有潜力的研究课题
```
```
systematic audit: 对比分析知识图谱与大语言模型结合的三个研究方向
```

底层搜索功能（论文检索、引用查询、BibTeX 导出等）同样可用：

```
帮我找 Yann LeCun 在 Semantic Scholar 上的所有论文，按引用数排序
```

---

## CDP Proxy API

Proxy 通过 WebSocket 直连 Chrome，提供 HTTP API（Agent 自动管理生命周期）：

```bash
curl -s "http://127.0.0.1:${CDP_PROXY_PORT:-3456}/new?url=URL"                              # 新建 tab
curl -s -X POST "http://127.0.0.1:${CDP_PROXY_PORT:-3456}/eval?target=ID" -d 'JS 表达式'    # 执行 JS
curl -s -X POST "http://127.0.0.1:${CDP_PROXY_PORT:-3456}/click?target=ID" -d 'CSS 选择器'  # 点击元素
curl -s "http://127.0.0.1:${CDP_PROXY_PORT:-3456}/screenshot?target=ID&file=/tmp/shot.png"  # 截图
curl -s "http://127.0.0.1:${CDP_PROXY_PORT:-3456}/close?target=ID"                          # 关闭 tab
```

完整参考见 [`references/cdp-api.md`](references/cdp-api.md)。

---

## 设计理念

> Skill = 哲学 + 技术事实，不是操作手册。讲清 tradeoff 让 AI 自己选，不替它推理。

搜索的瓶颈不在"搜"，在"筛"。核心策略是先输出轻量摘要表，让用户确认核心论文后再深拉，避免无效的完整元数据抓取。

排序优先级：**时效性（近 6 月 `[新]` 置顶）→ 引用数 → CCF 等级（参考项）**。前沿方向的新论文引用数天然偏低，以时效性为首要维度确保最新进展不被埋没。

**选题发现的瓶颈不在"找论文"，在"找方向"。** 我们正在构建的热点检测、空白识别、文献聚类和论文推荐能力，旨在帮助研究者从文献海洋中快速定位有潜力的研究方向。

---

## 相关资源

- [需求清单与能力规划](需求清单与能力规划.md) — 完整需求文档，60+ 需求项的优先级、工作量评估与能力矩阵
- [上游项目](https://github.com/Mingyue-Cheng/academic-search) — 本 fork 的原始项目，感谢 [Mingyue-Cheng](https://github.com/Mingyue-Cheng) 的出色工作

## License

MIT · Forked from [Mingyue-Cheng/academic-search](https://github.com/Mingyue-Cheng/academic-search)
