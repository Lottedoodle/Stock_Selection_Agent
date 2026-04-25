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


## 🚀 Getting Started

### Prerequisites

- Python `>=3.10, <3.14`
- [UV](https://docs.astral.sh/uv/) package manager
- API Keys for: OpenAI, Serper, and Pushover

### 1. Install UV

```bash
pip install uv
```

### 2. Clone & Install Dependencies

```bash
git clone https://github.com/your-username/stock-selection-agent.git
cd stock_selection
crewai install
```

### 3. Configure Environment Variables

Create a `.env` file in the project root:

```env
# LLM Provider
OPENAI_API_KEY=your_openai_api_key

# Search Tool (https://serper.dev)
SERPER_API_KEY=your_serper_api_key

# Push Notifications (https://pushover.net)
PUSHOVER_USER=your_pushover_user_key
PUSHOVER_TOKEN=your_pushover_app_token
```

### 4. Run the Agent

```bash
uv run stock_selection
```

Or using the crewAI CLI:

```bash
crewai run
```

---

## ⚙️ Configuration

### Changing the Target Sector

Edit `src/stock_selection/main.py` and update the `sector` input:

```python
inputs = {
    'sector': 'Healthcare',   # e.g., Technology, Energy, Finance
    "current_date": str(datetime.now())
}
```

### Customizing Agents

Edit `src/stock_selection/config/agents.yaml` to change agent roles, goals, backstories, or LLM models.

### Customizing Tasks

Edit `src/stock_selection/config/tasks.yaml` to modify task descriptions, expected outputs, or output file paths.

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
