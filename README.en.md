> 🚧 **Built on top of [Mingyue-Cheng/academic-search](https://github.com/Mingyue-Cheng/academic-search) v1.2.0 by Mingyue Cheng (MIT License).**
> The upstream `academic-search` provides multi-platform paper search infrastructure.
> **research-topic-auditor** extends it with **Topic Auditing** capabilities,
> forming a two-layer architecture: search infrastructure + topic auditing.

<p align="center">
  <strong style="font-size:2.2em; vertical-align:middle;">Research Topic Auditor</strong>
</p>

<p align="center">A Claude Skill for evidence-based research topic auditing</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-v0.1.0-0f766e" />
  <img src="https://img.shields.io/badge/license-MIT-1f2937" />
  <img src="https://img.shields.io/badge/status-building-yellow" />
  <img src="https://img.shields.io/badge/built_on-academic_search_v1.2.0-blue" />
</p>

<p align="center"><a href="README.md">简体中文</a> | English</p>

---

## Project Positioning

```
academic-search (upstream)         → multi-platform paper search infrastructure
         +
research-topic-auditor (this repo) → 5-step research topic audit workflow
         =
From search to topic selection — an all-in-one research auditing assistant
```

## Quick Start

```bash
# Clone this repo to Claude Code skills directory
git clone https://github.com/Tanue-Hou/research-topic-auditor.git ~/.claude/skills/research-topic-auditor
# Check dependencies
bash ~/.claude/skills/research-topic-auditor/scripts/check-deps.sh
```

Then directly ask Claude Code:

```text
Audit this topic: research on graph neural network for time series forecasting, analyze frontiers and gaps
```

Or:

```text
Recommend 3 promising research topics in NLP
```

---

## News

- `2026-05-04` Project forked and initialized; detailed requirements document published
- `2026-05-01` Upstream v1.2.0: multidisciplinary guidance, CNKI support, frontier-first ranking, query expansion, direct PDF retrieval

---

## Two-Layer Architecture

```
┌──────────────────────────────────────────────────────────────────────┐
│  Application: research-topic-auditor                                 │
│  Topic Audit · Frontier Analysis · Gap Identification               │
│  Novelty Assessment · Multi-Agent Workflow · Topic Proposal         │
│                                                                      │
│  · 5-Step Topic Audit Workflow                                      │
│  · Frontier Trend Analysis (citation burst, keyword trends)         │
│  · Research Gap Identification (method-task matrix)                 │
│  · 5-Dimension Novelty Assessment                                   │
│  · Multi-Agent Orchestration                                        │
│  · Structured Topic Proposal Report                                 │
├──────────────────────────────────────────────────────────────────────┤
│  Infrastructure: academic-search v1.2.0                             │
│  Paper Search · Metadata Extraction · PDF/OA · BibTeX · Citations   │
│                                                                      │
│  · 10+ academic platforms (API / CDP)                               │
│  · 6 discipline routing profiles                                    │
│  · Query expansion · Frontier-first ranking · Two-pass search       │
│  · OA PDF cascade · BibTeX export · Site patterns                   │
│  · Parallel sub-agents · Failure signal handling                    │
└──────────────────────────────────────────────────────────────────────┘
```

## Core Capabilities

### Topic Auditing (New ✨)

| Capability | Priority | Description |
|-----------|:--------:|-------------|
| Hotspot Detection | 🚧 Planned | Keyword frequency trends, Citation Burst detection |
| Literature Clustering | 🚧 Planned | Semantic clustering, research direction tree |
| Trend Analysis | 🚧 Planned | Yearly hotspot evolution, emerging direction alerts |
| Research Gap Identification | 🚧 Planned | Method-task matrix, cross-disciplinary gaps |
| Paper Recommendation | 🚧 Planned | Seed-paper based recommendation |
| Survey Generation | 🚧 Planned | Search → cluster → structured survey draft |
| Academic Network Analysis | 🚧 Planned | Author/institution collaboration networks |

See [需求清单与能力规划](需求清单与能力规划.md) (Chinese) for the complete roadmap.

### Search & Acquisition (Provided by upstream ✅)

- **Discipline routing**: CS/AI, Medicine, Physics, Chemistry, Social Sciences, Humanities
- **Two-pass strategy**: lightweight summary → deep fetch for confirmed papers
- **Query expansion**: 2-3 complementary queries, 30-50% recall improvement
- **Frontier-first ranking**: recency → citations → venue tier
- **Cross-platform dedup**: DOI/arXiv ID as primary key
- **OA PDF cascade**: arXiv direct → S2 → OpenAlex → Unpaywall → domain repositories
- **Full-text status**: `open_pdf`, `needs_institution`, `no_open_pdf`, `anti_bot_blocked`, `html_not_pdf`
- **BibTeX export**: native + field assembly dual path
- **Code availability**: Papers with Code API auto-fill
- **Failure signal handling**: 429 / timeout / empty results mapped to explicit adjustments
- **CDP browser mode**: Google Scholar, CNKI
- **Pre-seeded site knowledge**: 14+ platform and publisher pattern files

---

## 5-Step Audit Workflow

```
Step 1 ─ Literature Search
  ├── Multi-platform parallel search (arXiv, S2, PubMed, CNKI...)
  ├── Discipline routing + Query expansion
  ├── Two-pass strategy: summary → deep pull
  └── Output: structured paper list + citations

Step 2 ─ Frontier Analysis
  ├── Yearly paper/citation trends
  ├── Citation Burst detection
  ├── Keyword frequency evolution
  ├── Venue distribution analysis
  └── Output: trend report + hotspot map

Step 3 ─ Gap Identification
  ├── Method-task matrix construction
  ├── Cross-disciplinary gap detection
  ├── Future Work mining from surveys
  ├── Dataset/benchmark coverage analysis
  └── Output: gap list + opportunity scores

Step 4 ─ Novelty Assessment
  ├── Method novelty (first application to task/domain)
  ├── Scenario novelty (new problem setting)
  ├── Combinatorial novelty (non-trivial combination)
  ├── Feasibility (data/compute/domain knowledge)
  └── Output: novelty scores + differentiation report

Step 5 ─ Topic Proposal
  ├── Generate candidate directions
  ├── Feasibility assessment per direction
  ├── Resource estimation
  ├── Risk analysis
  └── Output: structured topic proposal report
```

---

## Installation

This Skill requires the upstream search infrastructure (academic-search) plus the auditing layer.

```bash
# Step 1: Install upstream search skill (academic-search)
git clone https://github.com/Mingyue-Cheng/academic-search ~/.claude/skills/academic-search

# Step 2: Install this skill (research-topic-auditor)
git clone https://github.com/Tanue-Hou/research-topic-auditor.git ~/.claude/skills/research-topic-auditor

# Step 3: Check environment dependencies
bash ~/.claude/skills/research-topic-auditor/scripts/check-deps.sh
```

**Requirements (CDP mode only)**: API platforms (arXiv, S2, PubMed, etc.) work out of the box. Google Scholar requires Chrome remote debugging:

1. Open `chrome://inspect/#remote-debugging`
2. Check **Allow remote debugging for this browser instance**

---

## Platform Access Strategy

| Platform | Access Method |
|----------|--------------|
| arXiv | REST API |
| Semantic Scholar | REST API |
| Crossref | REST API |
| OpenAlex | REST API |
| Unpaywall | REST API |
| PubMed | NCBI E-utilities |
| Papers with Code | REST API |
| ACM DL | WebFetch + Jina |
| IEEE Xplore | WebFetch / Jina / Official API |
| ScienceDirect / Wiley / Springer / ACS | OA status + institution access notice |
| **Google Scholar** | **CDP browser** |
| **CNKI** | **CDP browser** |

---

## Usage Examples

Topic auditing:

```text
Audit this topic: research on graph neural network for time series forecasting
Analyze the frontiers and gaps of large language models in healthcare
Evaluate the novelty of this idea: using diffusion models for molecular conformation generation
Recommend 3 promising research topics in NLP
```

Underlying search capabilities are also available:

```text
Find all papers by Yann LeCun on Semantic Scholar, sorted by citation count
```

---

## Project Structure

```
research-topic-auditor/
├── SKILL.md                        # Main instruction file
├── 需求清单与能力规划.md            # Requirements & roadmap (Chinese)
├── .claude/settings.json           # Claude Code project config
├── scripts/
│   ├── cdp-proxy.mjs               # CDP Proxy
│   ├── check-deps.sh               # Dependency check
│   ├── self-test.sh                # Local regression test
│   └── release-test.sh             # Pre-release test
├── agents/
│   ├── openai.yaml
│   └── auditor/                    # Multi-agent templates
│       ├── searcher.md
│       ├── frontier-analyst.md
│       ├── gap-analyst.md
│       ├── novelty-judge.md
│       └── synthesizer.md
├── references/
│   ├── api-cookbook.md
│   ├── metadata-schema.md
│   ├── venue-rankings.md
│   ├── cdp-api.md
│   ├── disciplines/
│   ├── rankings/
│   ├── site-patterns/
│   ├── workflows/
│   └── auditor/
│       ├── frontier-analysis.md
│       ├── gap-identification.md
│       ├── novelty-assessment.md
│       ├── topic-proposal.md
│       └── multi-agent-workflow.md
├── workflows/
│   └── topic-audit.md
└── docs/
    ├── skill-usage-comparison.md
    └── multidisciplinary-improvement-analysis.md
```

## Design Principles

1. **Two-layer decoupling**: infrastructure = "find and fetch", application = "analyze and judge"
2. **Evidence-driven**: all conclusions must be backed by paper citations
3. **Gap = opportunity**: research gap identification is the most valuable output
4. **Multi-agent orchestration**: complex tasks decomposed to parallel specialized agents
5. **Progressive deepening**: broad scan first, deep analysis second
6. **Transparent scoring**: all scores must be explainable and attributable

## Related Resources

- [需求清单与能力规划](需求清单与能力规划.md) — Complete requirements document with 60+ items and capability matrix
- [Upstream project](https://github.com/Mingyue-Cheng/academic-search) — Original project by Mingyue Cheng

## License

MIT · Forked from [Mingyue-Cheng/academic-search](https://github.com/Mingyue-Cheng/academic-search)
