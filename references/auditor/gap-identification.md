# Research Gap Identification — 研究空白识别方法论

## 概述

系统性地识别给定研究领域中尚未被充分探索的方向，为选题提供依据。

## 方法

### 1. 方法-任务矩阵

构建 {方法} × {任务} 交叉表，从论文摘要/关键词中自动提取方法-任务对。

**构建步骤**：
1. 收集目标领域论文清单（Step 1 输出）
2. 从标题/摘要中提取 "method + for + task" 模式（LLM 辅助提取）
3. 汇总形成方法集 M 和任务集 T
4. 构建 |M| × |T| 矩阵，单元格值为该 {方法, 任务} 出现的论文数
5. 稀疏/零值单元格 = 潜在研究空白

**示例矩阵**：

```
                 时序预测  异常检测  分类  生成  可解释性
GCN              12        8        5     2     0
GAT              8         4        3     1     0
Transformer-GNN  15        3        2     0     0
GraphGPT         2         1        0     0     0
Graph Diffusion  0         0        0     3     0
```

### 2. 交叉学科空白区检测

检测方法 A 学科的方法在 B 学科任务中的应用空白。

**实现**：
1. 确定源学科（方法丰富）和目标学科（任务丰富）
2. 在源学科中提取代表性方法
3. 在目标学科中搜索这些方法的应用
4. 未应用或少应用的 {方法, 任务} 对 = 交叉学科空白

### 3. Future Work 挖掘

从高引用综述/论文中提取 "future work"、"open challenge"、"limitation"、"future direction" 等段落。

**搜索策略**：
```
"future work" "{topic}" survey
"open challenge" "{topic}"
"{topic}" limitation
"{topic}" future direction
```

### 4. 数据集/基准覆盖分析

| 维度 | 说明 |
|------|------|
| 任务覆盖率 | 标准数据集覆盖了多少子任务 |
| 规模分布 | 数据集规模（小/中/大）分布 |
| 语言/领域偏置 | 数据集是否存在严重偏置 |

## 空白评分

```
gap_score = w1 × (1 - coverage_ratio) + w2 × relevance + w3 × feasibility + w4 × impact
```

| 参数 | 默认值 | 说明 |
|------|--------|------|
| coverage_ratio | — | 该空白区已有论文数 / 该方向总论文数 |
| relevance | 0-1 | 与用户研究兴趣的相关性 |
| feasibility | 0-1 | 数据/算力/领域知识可得性 |
| impact | 0-1 | 潜在学术/应用价值 |
| w1, w2, w3, w4 | 0.4, 0.3, 0.2, 0.1 | 权重可调 |

## 输出格式

```json
{
  "gaps": [
    {
      "method": "",
      "task": "",
      "gap_score": 0.0,
      "coverage_ratio": 0.0,
      "opportunity": "high|medium|low",
      "evidence": {
        "existing_papers": 0,
        "future_work_mentions": 0,
        "cross_disciplinary": true
      },
      "suggested_entry_point": ""
    }
  ],
  "method_task_matrix": {},
  "future_work_insights": [
    {"source": "paper_title", "quote": "...", "gap": ""}
  ]
}
```
