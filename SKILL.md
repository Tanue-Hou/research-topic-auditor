---
name: research-topic-auditor
description: |
  科研选题审查专用 Skill。Use when the user asks to: audit/evaluate a research topic, do literature review for topic selection, assess novelty/innovation of a research idea, identify research gaps, analyze frontier trends, generate algorithmic topic proposals, evaluate feasibility of a research direction, or perform systematic literature analysis for topic validation. 触发词：选题审查、选题评估、研究空白、创新性判断、前沿分析、课题可行性、topic audit、novelty assessment、gap analysis、research frontier.
metadata:
  version: "0.1.0"
  built_on: "academic-search v1.2.0"
  architecture: "two-layer"
  layers:
    infrastructure: "academic-search — 多平台论文检索、元数据提取、PDF/OA获取、BibTeX导出、引用关系分析"
    application: "research-topic-auditor — 科研审查、前沿分析、空白识别、创新性判断、多智能体选题建议"
---

# Research Topic Auditor — 科研选题审查 Skill

## 架构概览

本 Skill 采用双层架构：

```
┌──────────────────────────────────────────────────────────────────────┐
│  应用层：research-topic-auditor                                      │
│  科研审查 · 前沿分析 · 空白识别 · 创新性判断 · 多智能体选题建议      │
│                                                                      │
│  · 选题审查工作流 (Topic Audit Workflow)                             │
│  · 前沿趋势分析 (Frontier Trend Analysis)                            │
│  · 研究空白识别 (Research Gap Identification)                        │
│  · 创新性评估 (Novelty Assessment)                                   │
│  · 多智能体协作 (Multi-Agent Collaboration)                          │
│  · 选题建议报告生成 (Topic Proposal Generation)                      │
├──────────────────────────────────────────────────────────────────────┤
│  基础设施层：academic-search v1.2.0                                  │
│  论文检索 · 元数据提取 · PDF/OA · BibTeX · 引用关系 · 多学科路由     │
│                                                                      │
│  · 10+ 学术平台 API / CDP                                            │
│  · 6 大学科路由                                                      │
│  · Query 扩展 · 前沿性排序 · 两遍搜索 · 跨平台去重                   │
│  · OA PDF 级联获取 · BibTeX 导出 · 站点经验                          │
│  · 并行分治 · 失败信号处理                                           │
└──────────────────────────────────────────────────────────────────────┘
```

**前置要求**：本 Skill 依赖 [academic-search](https://github.com/Mingyue-Cheng/academic-search)（作者：Mingyue Cheng，MIT License）提供搜索基础设施。运行 `scripts/check-deps.sh` 确认环境就绪。

---

## 1. 核心工作流：选题审查五步法

### 工作流总览

```
Step 1 ─ 文献检索 (Literature Search)
  ├── 多平台并行检索（arXiv, S2, PubMed, CNKI...）
  ├── 学科路由 + Query 扩展
  ├── 两遍策略：摘要表 → 深拉元数据
  └── 输出：结构化论文清单 + 引用关系

Step 2 ─ 前沿趋势分析 (Frontier Analysis)
  ├── 历年发文量/引用量趋势
  ├── Citation Burst 检测（新兴方向识别）
  ├── Venue 分布与顶级会议/期刊占比
  ├── 关键词热度变迁（上升/下降/稳定）
  └── 输出：趋势报告 + 热点图谱

Step 3 ─ 研究空白识别 (Gap Identification)
  ├── 方法-任务矩阵构建
  ├── 交叉学科空白区检测
  ├── 已有综述/调研论文的 Future Work 提取
  ├── 数据集/基准覆盖分析
  └── 输出：空白区列表 + 机会评分

Step 4 ─ 创新性评估 (Novelty Assessment)
  ├── 与已有工作的差异化对比
  ├── 方法新颖性：是否首次将某方法用于某任务
  ├── 场景新颖性：是否首次在特定场景/领域应用
  ├── 组合创新：已知方法的非平凡组合
  └── 输出：创新性评分 + 差异化报告

Step 5 ─ 选题建议 (Topic Proposal)
  ├── 综合前三步结果生成备选方向
  ├── 每个方向的可行性评估
  ├── 资源需求估计（数据/算力/领域知识）
  ├── 风险与挑战分析
  └── 输出：选题建议报告（含优先级排序）
```

---

## 2. 分步指令

### Step 1: 文献检索

调用 academic-search 基础设施进行多平台检索。

1. **确定学科** → 读取 `references/disciplines/*.md`
2. **Query 扩展** → 自动展开 2-3 个互补 query
3. **意图判断** → 用户明确说数量时直接输出，否则两遍策略
4. **多平台并行** → 按学科路由分发子 Agent
5. **结果合并** → 按 `references/metadata-schema.md` 合并去重
6. **引用关系** → 对核心论文拉取 S2 引用/被引列表

详细操作参见上游 `SKILL.md`（academic-search 基础设施层）。

### Step 2: 前沿趋势分析

在 Step 1 的论文清单基础上，进行趋势分析。

**关键指标**：

| 指标 | 数据来源 | 计算方法 |
|------|---------|---------|
| 年度发文量 | S2/arXiv/PubMed 历年结果 | 按年份统计 paper count |
| 年度引用中位数 | S2 citationCount | 按年份统计 median |
| 关键词频率趋势 | 论文标题+摘要 | TF-IDF + 滑动窗口 |
| Citation Burst | S2 citationCount 序列 | 改进的 Kleinberg burst detection |
| Venue 分布 | S2 venue 字段 | 按 venue 分组计数 |
| 核心作者识别 | S2 authors | 发文量 + 引用量综合排序 |

**输出格式示例**：

```json
{
  "topic": "graph neural network for time series",
  "analysis_period": "2022-2026",
  "total_papers": 342,
  "trends": {
    "yearly_papers": {"2022": 45, "2023": 78, "2024": 112, "2025": 87, "2026": 20},
    "citation_bursts": [
      {"year": 2024, "paper": "...", "burst_strength": 12.5, "keyword": "spatial-temporal GNN"}
    ],
    "rising_keywords": ["spatial-temporal", "dynamic graph", "heterogeneous"],
    "declining_keywords": ["static graph", "node classification"]
  },
  "top_venues": ["NeurIPS", "ICLR", "KDD", "TKDE"],
  "core_authors": ["Author A", "Author B"]
}
```

### Step 3: 研究空白识别

**方法-任务矩阵**：构建 {方法} × {任务} 交叉表，标记已探索和未探索区域。

```
示例（Graph Neural Network × Time Series）：

                 时序预测  异常检测  分类  生成  可解释性
GCN              ██████   ██████   ███   ██   ░░░░
GAT              █████    ████    ███   █    ░░░░
Transformer-GNN  ███████  ███     ██    ░░   ░░░░
GraphGPT         ██       █       ░     ░    ░░░░
Graph Diffusion  ░        ░       ░     █    ░░░░

░ = 研究空白（opportunity）  █ = 已有一定工作量
```

**Future Work 挖掘**：搜索 "future work"、"open challenge"、"limitation" 等关键词。

**空白评分公式**：

```
gap_score = w1 × (1 - coverage_ratio) + w2 × relevance_score + w3 × feasibility_score
```

### Step 4: 创新性评估

| 维度 | 描述 | 评分 |
|------|------|------|
| 方法新颖性 | 该方法是否首次用于该任务/领域 | 1-5 |
| 场景新颖性 | 该场景/领域是否首次被系统研究 | 1-5 |
| 组合创新性 | 已知元素的非平凡组合 | 1-5 |
| 可行性 | 数据可得性、算力需求、领域门槛 | 1-5 |
| 影响力潜力 | 潜在引用/应用价值 | 1-5 |

综合评分 = weighted_sum(novelty, feasibility, impact, ...)

### Step 5: 选题建议

综合 Step 1-4，生成选题建议报告。

**输出模板**：

```markdown
## 推荐方向 1：[方向名称]
- **创新性评分**: ★★★★☆ (4.2/5)
- **可行性评分**: ★★★★☆ (3.8/5)
- **核心依据**: ...
- **关键论文**: [2-3 篇核心参考文献]
- **推荐切入点**: ...
- **潜在风险**: ...
```

---

## 3. 多智能体工作流

复杂选题审查任务应分发子 Agent 并行执行。

### Agent 角色

| Agent | 职责 | 输入 | 输出 |
|-------|------|------|------|
| **Searcher** | 多平台论文检索 | 关键词 + 学科 | 结构化论文清单 |
| **Frontier Analyst** | 前沿趋势分析 | 论文清单 | 趋势报告 |
| **Gap Analyst** | 研究空白识别 | 论文清单 + 矩阵 | 空白区报告 |
| **Novelty Judge** | 创新性评估 | 论文清单 + 提案 | 评分报告 |
| **Synthesizer** | 综合生成建议 | 所有子报告 | 选题建议报告 |

### 分发策略

```
主 Agent（意图解析 → 拆解 → 分发 → 综合）
  ├── Searcher Agent × N（多平台并行）
  │     └── 去重合并 → 论文清单
  ├── Frontier Analyst + Gap Analyst（并行）
  │     └── 趋势 + 空白 → 分析基座
  ├── (可选) Novelty Judge
  │     └── 创新性评分
  └── Synthesizer 综合 → 选题建议报告
```

子 Agent Prompt 必须指定加载 academic-search 和 research-topic-auditor 两个 Skill。

---

## 4. 参考文件索引

### 基础设施层（academic-search）

| 文件 | 用途 |
|------|------|
| `references/api-cookbook.md` | 各平台 API 调用模板 |
| `references/metadata-schema.md` | 统一元数据 Schema 与去重规则 |
| `references/venue-rankings.md` | CS 会议/期刊 CCF 分级 |
| `references/disciplines/*.md` | 6 大学科路由 profile |
| `references/site-patterns/*.md` | 平台出版商操作经验 |

### 应用层（research-topic-auditor）

| 文件 | 用途 |
|------|------|
| `references/auditor/frontier-analysis.md` | 前沿趋势分析方法论 |
| `references/auditor/gap-identification.md` | 研究空白识别方法论 |
| `references/auditor/novelty-assessment.md` | 创新性评估框架 |
| `references/auditor/topic-proposal.md` | 选题建议生成模板 |
| `references/auditor/multi-agent-workflow.md` | 多智能体协作工作流 |
| `workflows/topic-audit.md` | 完整选题审查工作流 |
| `agents/auditor/*.md` | 各 Agent 角色模板 |
| `需求清单与能力规划.md` | 完整需求文档与路线图 |

---

## 5. 设计原则

1. **双层解耦**：基础设施层只负责"找"和"取"，应用层只负责"分析"和"判断"
2. **证据驱动**：所有分析结论必须有论文引用支撑
3. **空白 = 机会**：研究空白的识别结果是对用户最有价值的信息
4. **多智能体协作**：复杂任务拆解为子任务，专用 Agent 并行处理
5. **渐进式深入**：先宽后深，先全景扫描再深入分析
6. **排序透明**：评分逻辑必须可解释，用户能理解每一项得分原因

---

## 6. 使用示例

```
帮我审查一下这个选题：基于图神经网络的时序预测方法研究
```
```
帮我分析一下大语言模型在医疗领域的研究前沿和空白
```
```
评估这个 idea 的创新性：用扩散模型做分子构象生成
```
```
给我推荐 3 个 NLP 方向有潜力的研究课题
```
```
systematic audit: 对比分析知识图谱与大语言模型结合的三个研究方向
```
