# ✈️ TripMate AI — Multi-Agent Travel Planner

[![Live Demo](https://img.shields.io/badge/Live_Demo-Render-46E3B7?style=for-the-badge&logo=render&logoColor=white)](https://tripmate-ai-an-multi-agent-trip-planner-1.onrender.com/)
[![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![LangGraph](https://img.shields.io/badge/LangGraph-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)](https://langchain-ai.github.io/langgraph/)
[![Gemini](https://img.shields.io/badge/Gemini_3.5_Flash_Lite-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://aistudio.google.com/)
[![Groq](https://img.shields.io/badge/Groq_Qwen_3.8-F55036?style=for-the-badge&logo=groq&logoColor=white)](https://groq.com/)
[![Model Context Protocol](https://img.shields.io/badge/MCP-Protocol-6366F1?style=for-the-badge)](https://modelcontextprotocol.io/)

> 🌐 **Live Application:** [TripMate AI on Render](https://tripmate-ai-an-multi-agent-trip-planner-1.onrender.com/)

**TripMate AI** is a multi-agent travel planning system built with **LangGraph**, **FastAPI**, and **Model Context Protocol (MCP)**. It dynamically routes user requests through specialist agents (flights, hotels, weather, budget), validates input with guardrails, and incorporates **Human-in-the-Loop (HITL)** approval before finalizing the trip itinerary.

---

## 📸 Screenshots

| 🏠 Home & Prompt Interface | 👤 Human-in-the-Loop Review |
|:---:|:---:|
| <img src="./assets/TripMate%20Home.png" width="480" alt="TripMate Home" /> | <img src="./assets/TripMate%20HITL.png" width="480" alt="TripMate HITL" /> |

<div align="center">
  <img src="./assets/TripMate.png" width="800" alt="TripMate Itinerary" />
  <p><i>Supervisor execution plan, real-time agent chips, and full itinerary generation.</i></p>
</div>

---

## ⚡ Core Architecture & Features

- **🛡️ Input Guardrail & Supervisor Agent:** Evaluates and routes travel requests to only the relevant specialist agents (or blocks non-travel queries).
- **✈️ Specialist Agents via MCP:**
  - **Flight Agent:** Queries airports and airline routes via AviationStack MCP.
  - **Hotel Agent:** Discovers live accommodations and neighborhoods via Tavily MCP.
  - **Weather Agent:** Fetches live forecasts and seasonal packing tips via Custom Weather MCP Server.
  - **Budget Agent:** Evaluates feasibility, detects cost risks, and suggests savings.
- **👤 Human-in-the-Loop (HITL):** LangGraph pauses execution at draft completion, letting the user approve or request targeted revisions before generating the final plan.
- **🔄 Resilient Dual-LLM Engine:** Powered by **Gemini 3.5 Flash Lite** with seamless zero-downtime fallback to **Groq (`qwen/qwen3.8-27b`)**.
- **💾 State Persistence:** PostgreSQL checkpointer with automatic fallback to in-memory state if the database is offline.

---

## 📁 Project Structure

```text
TripMate-AI/
├── assets/                       # UI screenshots & visual assets
├── static/                       # Modern UI (script.js, style.css)
├── templates/                    # Jinja2 template (index.html)
├── tools/                        # Auxiliary tool integrations
├── app.py                        # FastAPI server & REST API endpoints
├── backend.py                    # LangGraph multi-agent graph & supervisor
├── custom_weather_mcp_server.py  # FastMCP stdio weather server
├── mcp_client.py                 # MultiServer MCP client (Tavily, Aviation, Weather)
└── requirements.txt              # Project dependencies
```

---

## 🚀 Quick Start

### 1. Clone & Setup Environment

```bash
git clone https://github.com/your-username/TripMate-AI.git
cd TripMate-AI

python -m venv .venv
source .venv/bin/activate    # On Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

### 2. Configure Environment (`.env`)

Create a `.env` file in the project root:

```env
# LLM Provider (Gemini 3.5 Flash Lite or Groq)
GEMINI_API_KEY=your_gemini_api_key
GEMINI_MODEL=gemini-3.5-flash-lite
GROQ_API_KEY=your_groq_api_key

# MCP & Travel APIs
TAVILY_API_KEY=your_tavily_api_key
AVIATIONSTACK_API_KEY=your_aviationstack_api_key
OPENWEATHER_API_KEY=your_openweather_api_key
DEFAULT_ORIGIN_IATA=DAC

# State Checkpointer (Optional, falls back to MemorySaver)
DATABASE_URL=postgresql://user:password@localhost:5432/travel_db
```

### 3. Run the Application

```bash
python app.py
```

Open your browser at **`http://127.0.0.1:8000/`**.

---

## 🌐 API Overview

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Serves the TripMate AI interactive web UI |
| `POST` | `/api/travel` | Starts a multi-agent trip plan and pauses for review |
| `POST` | `/api/travel/approve` | Resumes the LangGraph thread with approval or revision feedback |
| `GET` | `/health` | API health check and active features |

---

## 🚀 Live Deployment

The application is deployed live on Render:
- 🔗 **Web App:** [TripMate AI](https://tripmate-ai-an-multi-agent-trip-planner-1.onrender.com/)
- 🔗 **Health Check:** [TripMate AI Health Endpoint](https://tripmate-ai-an-multi-agent-trip-planner-1.onrender.com/health)

---

## 📄 License

Distributed under the [MIT License](LICENSE).
