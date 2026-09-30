# WORKFORCEX — AI Workforce Digital Twin & Skill Deployment

WORKFORCEX is a full-stack prototype for **AI-assisted workforce management, skill matching, workload monitoring, project team formation, and task allocation**.

The system combines a FastAPI backend, SQLite/SQLAlchemy data layer, deterministic AI scoring, and a browser-based dashboard. It is designed to support the project's hardware workflow as well, where an ESP32 + RC522 RFID reader can identify an employee and the system can retrieve workforce information from the backend.

> **Project status:** Academic prototype / demonstration system.

## Key Features

- Employee and RFID-based workforce records
- Employee skills and skill-level tracking
- Project and task management
- Task assignment and status/progress tracking
- Project-member/team management
- Workload calculation from active tasks
- AI-assisted employee-to-task matching
- AI project-level task allocation
- Skill-gap analysis
- Deadline urgency and task-priority scoring
- Digital-twin style employee view
- Task history / audit trail
- RFID attendance lookup endpoint
- Browser dashboard served directly by FastAPI

## System Architecture

```text
┌──────────────────────┐
│ ESP32 + RC522 RFID   │
│ + ST7735 TFT         │
└──────────┬───────────┘
           │ Employee / RFID UID
           ▼
┌──────────────────────┐
│ FastAPI Backend      │
│ REST API             │
└──────────┬───────────┘
           │
     ┌─────┴───────────────┐
     ▼                     ▼
┌──────────────┐    ┌───────────────┐
│ SQLite +     │    │ AI Scoring &  │
│ SQLAlchemy   │    │ Allocation    │
└──────┬───────┘    └───────┬───────┘
       └────────────┬────────┘
                    ▼
          ┌────────────────────┐
          │ WORKFORCEX         │
          │ Web Dashboard      │
          └────────────────────┘
```

## AI Allocation Logic

The current AI layer is intentionally **local and deterministic** rather than dependent on an external LLM.

The scoring layer considers signals such as:

- Skill match and required skill level
- Current workload / availability
- Project or task priority
- Deadline urgency
- Role-to-task relevance for project-level allocation

This makes the prototype reproducible and keeps the hardware/attendance workflow independent of an external AI service. The architecture leaves room for a future cloud AI provider through environment configuration.

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML, CSS, JavaScript |
| Backend | Python, FastAPI |
| Database | SQLite |
| ORM | SQLAlchemy |
| Validation | Pydantic |
| AI layer | Python rule-based scoring |
| Hardware integration | ESP32, RC522 RFID, ST7735 TFT |
| API style | REST |

## Repository Structure

```text
WORKFORCEX/
├── ai/
│   ├── ai_service.py
│   ├── matcher.py
│   ├── scoring.py
│   └── update_status.py
├── backend/
│   ├── main.py
│   ├── project_progress.py
│   ├── team_formation.py
│   ├── migrate_task_management.py
│   ├── restore_demo_workers.py
│   └── set_test_rfid.py
├── database/
│   └── database.py
├── frontend/
│   └── index.html
├── docs/
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md
```

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/WORKFORCEX.git
cd WORKFORCEX
```

### 2. Create a virtual environment

Windows:

```bash
python -m venv .venv
.venv\Scripts\activate
```

macOS / Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Start the backend

From the repository root:

```bash
uvicorn backend.main:app --reload
```

The backend creates the SQLite schema automatically on first startup.

### 5. Open WORKFORCEX

Open:

```text
http://127.0.0.1:8000/
```

FastAPI serves the dashboard from `frontend/index.html`.

Interactive API documentation is available at:

```text
http://127.0.0.1:8000/docs
```

## Main API Groups

| Endpoint | Purpose |
|---|---|
| `/employees` | Employee records |
| `/employee_skills` | Employee skill records |
| `/skills` | Skill taxonomy |
| `/projects` | Project records |
| `/projects/{id}/members` | Project team membership |
| `/tasks` | Task management |
| `/assignments` | Assignment records |
| `/attendance/scan/{rfid_uid}` | RFID employee lookup |
| `/tasks/{id}/status` | Task progress/status updates |
| `/tasks/{id}/history` | Task audit history |
| `/projects/{id}/progress` | Computed project progress |
| `/ai/workloads` | Live workforce workload |
| `/ai/allocations` | AI allocation data |
| `/ai/projects/{id}/allocate` | Project-level AI allocation |
| `/ai/projects/{id}/allocation-preview` | Allocation preview |
| `/ai/skill-gap/{employee_id}/{task_id}` | Skill-gap analysis |
| `/ai/recommend-task/{employee_id}` | Task recommendations |

## Hardware Integration

The software is designed to work with the WORKFORCEX prototype hardware flow:

```text
RFID Card/Tag
     ↓
RC522 Reader
     ↓
ESP32
     ↓
FastAPI API
     ↓
Employee / Project / Task Data
     ↓
ST7735 TFT + Status Indicators
```

The hardware firmware is maintained separately from this software repository. This repository contains the software/backend side of the digital-twin system.

## Database

The repository deliberately does **not** include the local `workforce.db` file. SQLite is generated locally when the backend starts.

For a demonstration dataset, use the project's data-seeding scripts after reviewing and adapting them to your own demo requirements.

## Configuration

Copy `.env.example` to `.env` when environment-specific settings are required:

```bash
copy .env.example .env
```

The default configuration uses:

```text
AI_PROVIDER=rule_based
```

No API key is required for the current deterministic AI scoring layer.

## Screenshots / Demo Media

Add project screenshots, prototype photographs, architecture diagrams, and demo videos under `docs/` when preparing the repository for a competition, portfolio, or LinkedIn showcase.

Suggested files:

```text
docs/
├── dashboard.png
├── hardware-prototype.jpg
├── architecture.png
└── demo.mp4
```

## Future Scope

- Cloud/LLM-assisted workforce reasoning
- Authentication and role-based access control
- PostgreSQL deployment for multi-user environments
- Real-time WebSocket updates
- Mobile-responsive management views
- Deeper employee performance analytics
- Automated skill-gap learning recommendations
- Production-grade hardware telemetry and device management

## Academic / Project Context

WORKFORCEX was developed as an academic engineering prototype combining **embedded systems, RFID identification, backend APIs, databases, AI-assisted matching, and a digital dashboard** into a single workforce-management workflow.

## License

Add the license that matches your team's intended use before publishing the repository publicly.
