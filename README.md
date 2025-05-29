# indproject

# HANnah MVP 🚀  
_On-premise RAG assistant for HAN University_

![architecture](docs/architecture.png) &nbsp;<!-- optional screenshot/diagram -->

---

## 1  What is this?

**HANnah** is a self-hosted chatbot that answers study-related questions 24 / 7 while keeping all data inside HAN’s network.  
The stack combines:

| Layer | Technology | Purpose |
|-------|------------|---------|
| LLMs | **Llama 3.1 70 B** (heavy) · **Llama 3.1 13 B** (light) | Generates answers |
| Embedding | **Nomic `embed-text`** · Llama 3.1 8 B *(fallback)* | Turns text & queries into vectors |
| Vector DB | **Qdrant** | Stores and retrieves context |
| Orchestration | **n8n** | No-code workflow: scrape → split → embed → chat |
| Interface | **Telegram / API** | Students & staff talk to HANnah |

---

## 2  Quick start 🧑‍💻

> **Prerequisites:**  
> * Docker ≥ 24 & docker-compose plugin  
> * ~48 GB GPU VRAM if you plan to run the 70 B model

```bash
git clone https://github.com/ums11/indproject.git
cd indproject

# spin up database + n8n + Ollama (models auto-pull on first run)
docker compose up -d

# open n8n, import workflow JSONs
open http://localhost:5678   # default credentials: user / password
# import n8n/hannah_workflow.json  (Settings → Import)

# talk to the bot (Telegram token set? see .env.example)


Default ports Service
11434 Ollama (LLM API)
6333 Qdrant
5678 n8n UI

indproject/
├─ docker-compose.yml      # spins up entire MVP
├─ .env.example            # configure Telegram token, model names, etc.
├─ n8n/
│   ├─ hannah_workflow.json       # main RAG workflow
│   └─ scraper_workflow.json      # optional Playwright crawler
├─ examples/
│   └─ demo-policy.pdf     # sample doc for quick test
└─ docs/
    └─ architecture.png    # diagram used in the report

Symptom
Fix
context length exceeded
Reduce chunk size in splitter node (n8n).
GPU OOM on startup
Comment out LLAMA_70B service in compose; run only 13 B.
Telegram doesn’t respond
Check TELEGRAM_TOKEN + webhook URL in .env.
Slow first answer
First run downloads & warms models (~15 GB for 13 B, ~40 GB for 70 B).

Made with ♥ by UMS

