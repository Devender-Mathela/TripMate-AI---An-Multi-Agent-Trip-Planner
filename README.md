# ✈️ TripMate AI — Multi-Agent Travel Planner

[![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![LangGraph](https://img.shields.io/badge/LangGraph-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)](https://langchain-ai.github.io/langgraph/)
[![Gemini](https://img.shields.io/badge/Gemini_3.5_Flash_Lite-4285F4?style=for-the-badge&logo=google&logoColor=white)](https://aistudio.google.com/)
[![Groq](https://img.shields.io/badge/Groq_Qwen_3.8-F55036?style=for-the-badge&logo=groq&logoColor=white)](https://groq.com/)
[![Model Context Protocol](https://img.shields.io/badge/MCP-Protocol-6366F1?style=for-the-badge)](https://modelcontextprotocol.io/)

**TripMate AI** is an autonomous multi-agent travel planning platform built with **LangGraph**, **FastAPI**, and **Model Context Protocol (MCP)**. It dynamically routes travel queries across specialized AI agents (flights, hotels, weather, budget), enforces guardrails, and incorporates **Human-in-the-Loop (HITL)** approval before generating the final travel itinerary.

---

## 📸 Application Preview

### 🏠 1. Interactive Planning Interface
User enters a natural-language travel request or selects quick presets. The supervisor analyzes constraints and selects required specialists.

![TripMate Home Interface](assets/tripmate-home.png)

---

### 👤 2. Human-in-the-Loop Review & Revision
LangGraph pauses execution when the draft itinerary is ready. The traveler can approve it or submit revision feedback (e.g. adjust budget, add a free day).

![TripMate HITL Approval](assets/tripmate-hitl.png)

---

### 📋 3. Supervisor Execution Plan & Polished Itinerary
Shows the active supervisor routing chips, guardrail status, flight data, hotel recommendations, weather forecast, and a day-by-day itinerary.

![TripMate Execution Plan and Itinerary](assets/tripmate-plan.png)

---

## ⚡ Key Features

- **🛡️ Input Guardrail & Supervisor Agent:** Validates travel relevance and dynamically dispatches tasks to only the required specialists.
- **✈️ Flight Agent (MCP):** Real-time airports, routes, and airline data via AviationStack MCP.
- **🏨 Hotel Agent (MCP):** Accommodation recommendations and neighborhood discovery via Tavily MCP.
- **🌦️ Weather Agent (MCP):** Live conditions, multi-day forecasts, and packing advice via Custom FastMCP Server.
- **💰 Budget Agent:** Cost category breakdown, financial risk detection, and savings suggestions.
- **👤 Human-in-the-Loop (HITL):** Thread-based interrupts allow human approval and revisions before the final plan is finalized.
- **🔄 Dual LLM Engine:** Native support for **Gemini 3.5 Flash Lite** with zero-downtime fallback to **Groq (`qwen/qwen3.8-27b`)**.
- **💾 Resilient State:** PostgreSQL checkpointing with automatic fallback to in-memory state.

---

## 📁 Project Structure

```text
TripMate-AI/
├── assets/                       # UI screenshots & preview images
│   ├── tripmate-home.png         # Main landing & prompt interface
│   ├── tripmate-hitl.png         # Human-in-the-loop review modal
│   └── tripmate-plan.png         # Final itinerary & supervisor view
├── static/                       # Frontend assets (script.js, style.css)
├── templates/                    # Jinja2 template (index.html)
├── tools/                        # Helper search & flight modules
├── app.py                        # FastAPI web server & REST endpoints
├── backend.py                    # LangGraph multi-agent graph & supervisor
├── custom_weather_mcp_server.py  # FastMCP weather server (stdio)
├── mcp_client.py                 # MultiServer MCP client (Tavily, Aviation, Weather)
└── requirements.txt              # Python dependencies
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

# PostgreSQL Checkpointer (Optional, falls back to MemorySaver)
DATABASE_URL=postgresql://user:password@localhost:5432/travel_db
```

### 3. Run the App

```bash
python app.py
```

Open your browser at **`http://127.0.0.1:8000/`**.

---

## 🌐 API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Serves the web UI |
| `POST` | `/api/travel` | Starts a multi-agent trip plan and pauses for review |
| `POST` | `/api/travel/approve` | Resumes the LangGraph thread with approval or revision |
| `GET` | `/health` | Health check & active system features |

---

## 📄 License

Distributed under the [MIT License](LICENSE).
