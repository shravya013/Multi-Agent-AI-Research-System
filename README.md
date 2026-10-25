# 🔬 ResearchMind — Multi-Agent AI Research System

ResearchMind is a multi-agent AI pipeline that takes a research topic and produces a structured, sourced research report, then reviews and scores that report. Four specialized components work in sequence: a **Search Agent**, a **Reader Agent**, a **Writer Chain** and a **Critic Chain**. It is built with [LangChain](https://www.langchain.com/), powered by [Groq](https://groq.com/), uses [Tavily](https://tavily.com/) for web search, and includes a [Streamlit](https://streamlit.io/) web interface.

---

## ✨ Features

- **Four-stage pipeline**: search → scrape → write → critique
- **Live web search** via Tavily (top 5 results with titles, URLs and snippets)
- **Deep reading**: scrapes the most relevant page for fuller content
- **Structured reports**: Introduction, Key Findings (3+ points), Conclusion and Sources
- **Built-in critic**: scores the report out of 10 and lists strengths, areas to improve and a one-line verdict
- **Two ways to run it**: a polished Streamlit UI or a plain command-line script
- **One-click download** of the final report as Markdown (`.md`)

---

## 🧠 How It Works

```
Topic
  │
  ▼
┌──────────────┐   web_search (Tavily)
│ Search Agent │ ─────────────────────► titles, URLs, snippets
└──────┬───────┘
       ▼
┌──────────────┐   scrape_url (requests + BeautifulSoup)
│ Reader Agent │ ─────────────────────► clean text from the best URL
└──────┬───────┘
       ▼
┌──────────────┐
│ Writer Chain │ ─────────────────────► full research report
└──────┬───────┘
       ▼
┌──────────────┐
│ Critic Chain │ ─────────────────────► score, strengths, improvements, verdict
└──────────────┘
```

| Step | Component | Type | Role |
|------|-----------|------|------|
| 1 | Search Agent | LangChain agent + `web_search` tool | Finds recent, reliable information on the topic |
| 2 | Reader Agent | LangChain agent + `scrape_url` tool | Picks the most relevant URL and extracts deeper content |
| 3 | Writer Chain | Prompt → LLM → string parser | Drafts the final report from the gathered research |
| 4 | Critic Chain | Prompt → LLM → string parser | Reviews the report and returns a structured evaluation |

All components use the `openai/gpt-oss-20b` model served through Groq.

---

## 📁 Project Structure

```
Multi-Agent-AI-Research-System/
├── app.py            # Streamlit web interface
├── pipeline.py       # Command-line version of the pipeline
├── agents.py         # Agent builders, writer chain and critic chain
├── tools.py          # web_search (Tavily) and scrape_url tools
├── requirements.txt  # Python dependencies
└── .gitignore
```

---

## 🛠️ Tech Stack

- **Python 3.10+**
- **LangChain** (`langchain`, `langchain-core`, `langgraph`, `langchain-classic`) for agents, tools and chains
- **Groq** (`langchain-groq`) for LLM inference
- **Tavily** (`tavily-python`) for web search
- **BeautifulSoup4 + Requests** for web scraping
- **Streamlit** for the UI
- **python-dotenv** for environment variables

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/shravya013/Multi-Agent-AI-Research-System.git
cd Multi-Agent-AI-Research-System
```

### 2. Create a virtual environment (recommended)

```bash
python -m venv venv
source venv/bin/activate        # macOS / Linux
venv\Scripts\activate           # Windows
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
pip install streamlit           # needed for the web UI
```

> **Note:** `streamlit` is used by `app.py` but is not listed in `requirements.txt`, so install it separately (or add it to the file).

### 4. Set up API keys

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
TAVILY_API_KEY=your_tavily_api_key_here
```

- Get a Groq API key at [console.groq.com](https://console.groq.com/)
- Get a Tavily API key at [tavily.com](https://tavily.com/)

> The `.env` file holds secrets. Make sure it is listed in `.gitignore` and never commit it.

---

## ▶️ Usage

### Option A: Streamlit web app

```bash
streamlit run app.py
```

Then open the local URL shown in your terminal (usually `http://localhost:8501`), type a research topic, and click **⚡ Run Research Pipeline**. The pipeline panel shows live progress for each step. When it finishes you can:

- Expand the raw **Search Results** and **Scraped Content**
- Read the **Final Research Report**
- Download the report as a `.md` file
- Read the **Critic Feedback**

### Option B: Command line

```bash
python pipeline.py
```

Enter a topic when prompted. Each stage's output is printed to the terminal, and the final report and critic feedback appear at the end.

---

## 💡 Example Topics

- `LLM agents 2025`
- `CRISPR gene editing`
- `Fusion energy progress`

---

## 📝 Sample Critic Output Format

```
Score: X/10

Strengths:
- ...
- ...

Areas to Improve:
- ...
- ...

One line verdict:
...
```

---

## ⚠️ Known Limitations

- **Scraping is shallow**: only the first 3,000 characters of one page are used, and only the first 800 characters of the search results are passed to the Reader Agent.
- **One source scraped**: the Reader Agent picks a single URL, so reports lean on search snippets plus one page.
- **No JavaScript rendering**: `scrape_url` uses plain HTTP requests, so dynamic or paywalled pages may fail or return little text.
- **Critic is advisory only**: the critic scores the report but does not trigger an automatic rewrite.
- **Possible bug in `pipeline.py`**: the Search and Reader steps appear to use each other's agent builders (`build_reader_agent()` for search, `build_search_agent()` for reading). `app.py` uses them correctly.

---

## 🗺️ Roadmap Ideas

- Scrape and summarize multiple URLs per run
- Feed critic feedback back to the writer for an automatic revision loop
- Export reports as PDF or DOCX
- Add configurable model and search-depth settings in the UI
- Add `streamlit` to `requirements.txt`

---

## 🤝 Contributing

Contributions, issues and feature requests are welcome. Fork the repo, create a feature branch, and open a pull request.

---

## 🙏 Acknowledgements

- [LangChain](https://www.langchain.com/) for the agent framework
- [Groq](https://groq.com/) for fast inference
- [Tavily](https://tavily.com/) for the search API
- [Streamlit](https://streamlit.io/) for the UI framework
