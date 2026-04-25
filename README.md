# 📈 Stock Selection Agent

> An autonomous multi-agent AI system that discovers, researches, and selects the best investment opportunities — and notifies you in real-time.

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)](https://www.python.org/)
[![CrewAI](https://img.shields.io/badge/CrewAI-1.9.3-brightgreen)](https://crewai.com)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![UV](https://img.shields.io/badge/Managed%20by-UV-purple)](https://docs.astral.sh/uv/)

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

## 🏗️ Architecture

The system follows a **hierarchical crew process** with four specialized agents:

```
┌─────────────────────────────────────────────┐
│              Manager Agent (GPT-4o)         │
│   Orchestrates, delegates, and supervises   │
└──────────────┬──────────────────────────────┘
               │
       ┌───────┴──────────┐
       ▼                  ▼
┌──────────────┐   ┌───────────────────┐
│  Trending    │   │   Financial       │
│  Company     │──▶│   Researcher      │
│  Finder      │   │   (GPT-4o-mini)   │
│ (GPT-4o-mini)│   └────────┬──────────┘
└──────────────┘            │
                            ▼
                   ┌─────────────────┐
                   │  Stock Picker   │
                   │ (GPT-4o-mini)   │
                   │ + Push Notify   │
                   └─────────────────┘
```

### 🔄 Workflow

1. **Find Trending Companies** — Scans the web for 2–3 companies making news in the target sector.
2. **Research Companies** — Deep-dives into each company's market position, outlook, and investment potential.
3. **Pick Best Company** — Synthesizes research, selects the top candidate, and sends a push notification.

### 🧠 Memory Architecture

| Memory Type | Storage Backend | Purpose |
|---|---|---|
| Long-Term Memory | SQLite (`.db`) | Remembers past picks across sessions |
| Short-Term Memory | RAG (OpenAI Embeddings) | Maintains context within the current session |
| Entity Memory | RAG (OpenAI Embeddings) | Tracks key facts about companies and entities |

---

## 📤 Outputs

After each run, the following files are generated in the `output/` directory:

| File | Description |
|---|---|
| `trending_companies.json` | List of trending companies with tickers and reasons |
| `research_report.json` | Detailed analysis of each company |
| `decision.md` | Final investment recommendation with full rationale |

---

## 🛠️ Tech Stack

| Component | Technology |
|---|---|
| Agent Framework | [CrewAI](https://crewai.com) v1.9.3 |
| LLM Models | OpenAI GPT-4o / GPT-4o-mini |
| Search | [Serper Dev](https://serper.dev) (Google Search API) |
| Push Notifications | [Pushover](https://pushover.net) |
| Memory (RAG) | OpenAI `text-embedding-3-small` |
| Memory (Long-Term) | SQLite via `LTMSQLiteStorage` |
| Data Validation | Pydantic v2 |
| Package Manager | [UV](https://docs.astral.sh/uv/) |

---

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
