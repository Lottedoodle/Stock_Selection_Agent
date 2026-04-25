# 📈 Stock Selection Agent

> An autonomous multi-agent AI system that discovers, researches, and selects the best investment opportunities — and notifies you in real-time.

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)](https://www.python.org/)
[![CrewAI](https://img.shields.io/badge/CrewAI-1.9.3-brightgreen)](https://crewai.com)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![UV](https://img.shields.io/badge/Managed%20by-UV-purple)](https://docs.astral.sh/uv/)

---

## 💡 Why Stock Selection Agent?

Traditional stock research is manual, time-consuming, and prone to bias. This project automates the entire workflow — from discovering trending companies to delivering a final investment recommendation — using a team of AI agents that collaborate like a real analyst team.

### Who is this for?

- **Individual investors** looking for data-driven stock picks without spending hours on research
- **AI/ML engineers** exploring practical multi-agent system design
- **Finance professionals** who want an AI-powered first pass before deep-diving into analysis

### What problems does it solve?

| Problem | How This Agent Solves It |
|---|---|
| Keeping up with financial news is overwhelming | Automatically scans real-time news to surface trending companies |
| Research is time-consuming and inconsistent | AI agents produce structured, repeatable analysis every time |
| Decision fatigue from too many options | Narrows down to **one best pick** with a clear rationale |
| Missing opportunities while away from your desk | Sends **instant push notifications** to your phone |
| Repeating the same recommendations | **Persistent memory** ensures the crew never picks the same stock twice |

---

## 🌟 Key Highlights

- 🤖 **Hierarchical Multi-Agent Orchestration** — A dedicated Manager Agent (GPT-4o) coordinates and delegates tasks to specialized sub-agents, mimicking a real analyst team.
- 🧠 **Persistent Memory System** — Three layers of memory (Long-Term, Short-Term, Entity) ensure the crew learns from past decisions and avoids recommending the same stock twice.
- 🔍 **Real-Time News Intelligence** — Integrates with Serper (Google Search API) to scan the latest financial news and surface trending companies in any target sector.
- 📊 **Structured Output with Pydantic** — Every agent output is validated with Pydantic schemas, ensuring clean, reliable, and machine-readable data at every step.
- 🔔 **Instant Push Notifications** — Once the best stock is selected, a push notification is sent immediately to your phone via Pushover — no need to watch the terminal.
- 🗂️ **Fully Configurable via YAML** — Swap agents, roles, goals, and tasks without touching Python code.
- ⚡ **Fast Dependency Management with UV** — Reproducible environments with near-instant installs.

---

## 🏗️ System Architecture

The system follows a **hierarchical crew process** where a Manager Agent orchestrates the entire pipeline, delegating work to three specialized sub-agents:

```
┌──────────────────────────────────────────────────┐
│             🧑‍💼 Manager Agent (GPT-4o)            │
│    Orchestrates, delegates, and supervises all    │
│    tasks — ensures quality and coherence          │
└────────────────────┬─────────────────────────────┘
                     │ delegates
        ┌────────────┼────────────────┐
        ▼            ▼                ▼
┌──────────────┐ ┌────────────────┐ ┌─────────────────┐
│ 🔍 Trending  │ │ 📊 Financial   │ │ 🎯 Stock Picker │
│   Company    │ │   Researcher   │ │  (GPT-4o-mini)  │
│   Finder     │ │  (GPT-4o-mini) │ │                 │
│ (GPT-4o-mini)│ │                │ │ Makes final     │
│              │ │ Deep-dives     │ │ decision &      │
│ Scans news   │ │ into market    │ │ sends push      │
│ for trending │ │ position &     │ │ notification 🔔 │
│ companies    │ │ outlook        │ │                 │
└──────┬───────┘ └───────┬────────┘ └────────┬────────┘
       │                 │                   │
       ▼                 ▼                   ▼
  trending_        research_           decision.md
  companies.json   report.json       + Push Notification
```

### 🤖 Agent Roles

| Agent | Role | Model | Special Tools |
|---|---|---|---|
| **Manager** | Project manager — delegates tasks, ensures quality | GPT-4o | — |
| **Trending Company Finder** | Scans latest news to discover 2–3 trending companies in a given sector | GPT-4o-mini | Serper (Web Search) |
| **Financial Researcher** | Performs deep analysis on each company — market position, outlook, investment potential | GPT-4o-mini | Serper (Web Search) |
| **Stock Picker** | Synthesizes all research and selects the single best investment | GPT-4o-mini | Push Notification (Pushover) |

---

## 🔄 How It Works

The pipeline runs in three sequential stages, each building on the output of the previous:

### Stage 1 — Discover 🔍

> **Agent:** Trending Company Finder

The first agent searches real-time news using Serper (Google Search API) to identify **2–3 companies** that are currently making headlines in the target sector (e.g., Technology, Healthcare, Energy). It returns structured data including company name, ticker symbol, and the reason it's trending.

```
Input:  sector = "Technology", current_date = "2026-04-25"
Output: trending_companies.json
```

### Stage 2 — Research 📊

> **Agent:** Financial Researcher

The researcher takes the list of trending companies and performs a comprehensive analysis of each, covering:

- **Market Position** — Where the company stands relative to competitors
- **Future Outlook** — Growth prospects and upcoming catalysts
- **Investment Potential** — Risk/reward profile and suitability for investment

```
Input:  trending_companies.json (from Stage 1)
Output: research_report.json
```

### Stage 3 — Decide & Notify 🎯

> **Agent:** Stock Picker

The stock picker reviews all research findings and selects **the single best company** for investment. It then:

1. **Sends a push notification** to your phone with the pick and a one-sentence rationale
2. **Generates a detailed report** explaining why this company was chosen and why others were not

```
Input:  research_report.json (from Stage 2)
Output: decision.md + Push Notification 🔔
```

---

## 🧠 Memory System

The crew uses a **three-layer memory architecture** to maintain context and learn from past decisions:

```
┌─────────────────────────────────────────────────────────┐
│                    Memory System                         │
├─────────────────┬─────────────────┬─────────────────────┤
│  Long-Term      │  Short-Term     │  Entity             │
│  Memory         │  Memory         │  Memory             │
│                 │                 │                     │
│  SQLite DB      │  RAG Storage    │  RAG Storage        │
│  Persists       │  Current        │  Tracks company     │
│  across runs    │  session        │  facts & relations  │
│                 │  context        │                     │
│  "Never pick    │  "The manager   │  "NVIDIA: GPU       │
│   the same      │   just asked    │   market leader,    │
│   stock twice"  │   about AI      │   trending due to   │
│                 │   companies"    │   earnings report"  │
└─────────────────┴─────────────────┴─────────────────────┘
         ▲                 ▲                  ▲
         │                 │                  │
    text-embedding    text-embedding    text-embedding
      -3-small          -3-small          -3-small
```

| Memory Type | Storage Backend | Purpose | Persistence |
|---|---|---|---|
| **Long-Term** | SQLite (`long_term_memory_storage.db`) | Remembers past picks to avoid duplicates | ✅ Across sessions |
| **Short-Term** | RAG (OpenAI Embeddings) | Maintains conversational context within a run | ❌ Current session only |
| **Entity** | RAG (OpenAI Embeddings) | Tracks key facts about companies and entities | ❌ Current session only |

---

## 📤 Output Details

After each run, three output files are generated in the `output/` directory:

### `trending_companies.json`
A structured list of companies currently trending in the news:
```json
{
  "companies": [
    {
      "name": "NVIDIA",
      "ticker": "NVDA",
      "reason": "Record Q1 earnings driven by AI chip demand"
    }
  ]
}
```

### `research_report.json`
Comprehensive analysis for each company:
```json
{
  "research_list": [
    {
      "name": "NVIDIA",
      "market_position": "Dominant leader in AI/GPU market...",
      "future_outlook": "Strong growth expected with...",
      "investment_potential": "High potential due to..."
    }
  ]
}
```

### `decision.md`
A human-readable Markdown report containing:
- ✅ The **chosen company** and the detailed reasoning behind the selection
- ❌ The **rejected companies** and why they were not selected
- 📱 A **push notification summary** sent to your phone in real-time

---

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| Agent Framework | [CrewAI](https://crewai.com) v1.9.3 |
| Process Type | Hierarchical (Manager-delegated) |
| LLM Models | OpenAI GPT-4o (Manager) / GPT-4o-mini (Workers) |
| Web Search | [Serper Dev](https://serper.dev) — Google Search API |
| Push Notifications | [Pushover](https://pushover.net) |
| Embeddings | OpenAI `text-embedding-3-small` |
| Long-Term Memory | SQLite via `LTMSQLiteStorage` |
| RAG Memory | CrewAI `RAGStorage` |
| Data Validation | Pydantic v2 |
| Package Manager | [UV](https://docs.astral.sh/uv/) |

---

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
