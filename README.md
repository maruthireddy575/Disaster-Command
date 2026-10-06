# Disaster Command — Multi-Agent Simulation System

A full-stack disaster management simulation with 4 autonomous agents,
real-time dashboards, and user authentication.

## Project Structure

```
disaster-command/
├── frontend/                   # React + Vite (port 3000)
│   ├── src/
│   │   ├── App.jsx             # Login · Register · Dashboard
│   │   └── main.jsx            # React entry point
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
│
├── backend/                    # FastAPI + Uvicorn (port 8000)
│   ├── main.py                 # All routes + auth middleware
│   ├── auth_db.py              # SQLAlchemy models + auth logic
│   ├── orchestrator.py         # Simulation state machine
│   ├── agents.py               # 4 agent definitions
│   └── requirements.txt
│
├── database/                   # Auto-created at runtime
│   └── disaster_cmd.db         # SQLite (gitignored)
│
├── tests/
│   └── integration_test.py     # Runs all 10 scenarios
│
├── data/
│   └── evaluation_report.json  # Output of integration test
│
└── README.md
```

## Quick Start

### 1 — Backend

```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --reload
# → http://localhost:8000
# → http://localhost:8000/docs  (Swagger UI)
```

### 2 — Frontend

```bash
cd frontend
npm install
npm run dev
# → http://localhost:3000
```

### 3 — Integration tests (optional)

```bash
# from project root
python tests/integration_test.py
```

## Auth Flow

| Step | Endpoint | Notes |
|------|----------|-------|
| Register | `POST /auth/register` | username, email, password |
| Login | `POST /auth/login` | returns Bearer token (24 h TTL) |
| Authenticated call | any `/scenarios` or `/run` | `Authorization: Bearer <token>` |
| Logout | `POST /auth/logout` | invalidates token server-side |

## Agents

| Agent | Role |
|-------|------|
| MonitorAgent | Scans environment, detects disasters |
| ResourceAllocator | Assigns nearest available resources by priority |
| RescueCoordinator | Executes rescue ops, calculates outcomes |
| EvaluatorAgent | Scores results vs baseline, generates final report |

## Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `DB_PATH` | `database/disaster_cmd.db` | Override SQLite path |
