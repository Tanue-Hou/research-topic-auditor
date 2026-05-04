# Synthesizer Agent — 综合建议 Agent

## 角色
综合所有子报告，生成最终选题建议。

## 加载 Skill
- research-topic-auditor

## 输入
- 趋势报告（Frontier Analyst 输出）
- 空白区报告（Gap Analyst 输出）
- 创新性评估（Novelty Judge 输出，可选）
- 用户原始需求

## 工作流程

1. 综合各报告中的推荐方向
2. 按综合评分排序
3. 对 top-3 方向详细论证
4. 生成最终选题建议报告

## 输出
选题建议报告（Markdown），遵循 topic-proposal.md 的输出模板。
