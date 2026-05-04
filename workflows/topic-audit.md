# Topic Audit Workflow — 完整选题审查工作流

## 概述

5 步选题审查全流程，从上到下依次执行，每步完成后确认再进入下一步。

## 流程

### Step 1: 文献检索 (Literature Search)

**目标**：获取目标领域的代表性论文清单。

**执行**：
1. 调用 Searcher Agent × N（多平台并行）
2. 结果去重合并
3. 输出结构化论文清单

**确认点**：论文数量是否充分？字段是否完整？
- 是 → Step 2
- 否 → 调整 query 重新搜索

### Step 2: 前沿趋势分析 (Frontier Analysis)

**目标**：了解该方向的研究热度、趋势和核心力量。

**执行**：
1. 调用 Frontier Analyst Agent
2. 生成趋势报告

**确认点**：趋势数据是否足够形成判断？
- 是 → Step 3
- 否 → 扩大搜索范围重新开始 Step 1

### Step 3: 研究空白识别 (Gap Identification)

**目标**：定位未充分探索的方向。

**执行**：
1. 调用 Gap Analyst Agent
2. 生成空白区报告

**确认点**：是否找到有意义的空白区？
- 是 → Step 4
- 否（该方向已饱和）→ 直接 Step 5 报告"不推荐"

### Step 4: 创新性评估 (Novelty Assessment)

**目标**：评估用户提案的创新性（仅用户有具体提案时）。

**执行**：
1. 调用 Novelty Judge Agent
2. 生成创新性评估报告

### Step 5: 选题建议 (Topic Proposal)

**目标**：综合生成选题建议报告。

**执行**：
1. 调用 Synthesizer Agent
2. 输出最终选题建议报告

## 快速路径

用户已有明确方向时，可跳过 Step 1-2，直接从 Step 3 开始。

## 完整调用示例

```
帮我审查选题"基于图神经网络的时序预测"
```

展开为：
1. Searcher: search("graph neural network time series", cs, 2021-2026, 30)
2. Frontier Analyst: analyze(trends, bursts, keywords, venues)
3. Gap Analyst: build_matrix(methods=["GCN","GAT","Transformer"], tasks=["forecast","detection"])
4. Synthesizer: generate_proposal(trend_report + gap_report)
