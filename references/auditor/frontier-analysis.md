# Frontier Trend Analysis — 前沿趋势分析方法论

## 概述

对给定研究方向的历年论文数据进行趋势分析，识别热点变迁、新兴方向和核心研究力量。

## 关键指标

### 1. 年度发文量与引用量

| 指标 | 说明 | 数据来源 |
|------|------|---------|
| Annual Paper Count | 每年发表论文数 | S2 / arXiv / PubMed |
| Annual Median Citation | 每年论文引用中位数 | S2 citationCount |
| Annual Total Citation | 每年论文引用总数 | S2 citationCount |
| Growth Rate | 年增长率 = (Y_n - Y_{n-1}) / Y_{n-1} | 计算得出 |

### 2. Citation Burst Detection

检测短时间内引用数激增的论文/方向，参考 Kleinberg (2002) 的 burst detection 算法。

**简化实现**：
- 对每篇论文，计算其引用数在时间窗口内的增长率
- 增长率超过阈值（如 3 倍于同领域平均增长率）标记为 burst
- 同一方向多篇论文同时出现 burst 标记为该方向新兴

```python
def detect_burst(citation_year_series, threshold=3.0):
    """简化的 burst detection"""
    # citation_year_series: {year: citation_count}
    # 计算滑动窗口增长率
    rates = []
    for i in range(1, len(citation_year_series)):
        prev = citation_year_series[i-1]
        curr = citation_year_series[i]
        rate = curr / prev if prev > 0 else float('inf')
        rates.append(rate)
    # 标记 burst
    bursts = [i for i, r in enumerate(rates) if r > threshold]
    return bursts
```

### 3. 关键词热度变迁

| 类型 | 定义 | 识别方法 |
|------|------|---------|
| Rising | 频次显著增长 | 近 2 年 vs 前 3 年的频次变化率 > 50% |
| Stable | 频次基本不变 | 变化率在 -20% 到 50% 之间 |
| Declining | 频次显著下降 | 近 2 年 vs 前 3 年的频次变化率 < -20% |
| Emerging | 新出现的关键词 | 前 3 年未出现，近 2 年出现 |

### 4. Venue 分布

统计论文在顶级会议/期刊的分布比例，评估该方向的学术认可度。

### 5. 核心研究力量

| 维度 | 识别方法 |
|------|---------|
| 核心作者 | 发文量 × 引用量综合排序 |
| 核心机构 | 作者 affiliation 聚合 |
| 国家分布 | 机构所属国家聚合 |

## 输出格式

```json
{
  "topic": "string",
  "analysis_period": "YYYY-YYYY",
  "total_papers": 0,
  "trends": {
    "yearly_papers": {"YYYY": 0},
    "yearly_median_citations": {"YYYY": 0.0},
    "growth_rate": 0.0,
    "citation_bursts": [
      {"year": YYYY, "paper": "title", "burst_strength": 0.0}
    ],
    "keywords": {
      "rising": ["term"],
      "emerging": ["term"],
      "stable": ["term"],
      "declining": ["term"]
    }
  },
  "venues": {
    "top_venues": ["name"],
    "top_venue_ratio": 0.0
  },
  "core_researchers": [
    {"name": "", "paper_count": 0, "total_citations": 0}
  ],
  "core_institutions": [
    {"name": "", "paper_count": 0}
  ]
}
```
