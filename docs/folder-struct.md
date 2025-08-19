# 🏗 Backend (FastAPI + SQLModel + Alembic)

```
backend/
│── alembic/                 # Alembic migrations
│   ├── versions/            # Migration scripts
│   └── env.py               # Alembic config
│
│── app/
│   │── main.py              # FastAPI entry point
│   │── config.py            # Settings (env, secrets, DB URL)
│   │── dependencies.py      # Reusable dependencies (auth, db session)
│   │── events.py            # Startup/shutdown events
│   │
│   ├── api/                 # API routes (organized by feature)
│   │   ├── v1/              # Versioning for APIs
│   │   │   ├── routes/      
│   │   │   │   ├── auth.py
│   │   │   │   ├── employees.py
│   │   │   │   ├── campaigns.py
│   │   │   │   ├── events.py
│   │   │   │   └── reports.py
│   │   │   └── __init__.py
│   │   └── __init__.py
│   │
│   ├── core/                # Core system configs
│   │   ├── security.py      # JWT, password hashing
│   │   ├── logging.py       # Logging setup
│   │   └── settings.py      # Pydantic BaseSettings
│   │
│   ├── db/
│   │   ├── session.py       # DB session setup
│   │   ├── base.py          # SQLModel base + imports
│   │   └── init_db.py       # Seed data
│   │
│   ├── models/              # SQLModel DB models
│   │   ├── user.py
│   │   ├── employee.py
│   │   ├── campaign.py
│   │   ├── event.py
│   │   └── report.py
│   │
│   ├── schemas/             # Pydantic schemas
│   │   ├── user.py
│   │   ├── employee.py
│   │   ├── campaign.py
│   │   ├── event.py
│   │   └── report.py
│   │
│   ├── services/            # Business logic layer
│   │   ├── email_sender.py  # AWS SES / SendGrid logic
│   │   ├── campaign_service.py
│   │   ├── report_service.py
│   │   ├── tracking_service.py
│   │   └── training_service.py
│   │
│   ├── utils/               # Helpers
│   │   ├── csv_parser.py    # Multi-agent CSV validation
│   │   ├── pdf_generator.py # Reports
│   │   ├── email_utils.py
│   │   └── common.py
│   │
│   └── workers/             # Background tasks (Celery/RQ)
│       ├── tasks.py
│       └── scheduler.py
│
│── tests/                   # Pytest tests
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
│── .env                     # Environment vars
│── requirements.txt
│── pyproject.toml           # (if using poetry)
```

✅ **Why this works**:

* Clear separation: `models`, `schemas`, `services`, `api`.
* Scalable: Adding a new feature = add model + schema + route + service.
* `core/` centralizes security/logging/settings.
* Background workers ready for async jobs (sending mass emails, generating reports).

---

# 🎨 Frontend (React + Vite + Tailwind)

```
frontend/
│── public/                  # Static assets (favicon, logos)
│
│── src/
│   │── main.jsx             # React entry point
│   │── App.jsx              # Root app
│   │── routes.jsx           # App routes
│   │
│   ├── api/                 # API integration layer
│   │   ├── axiosClient.js
│   │   ├── authApi.js
│   │   ├── campaignApi.js
│   │   ├── employeeApi.js
│   │   └── reportApi.js
│   │
│   ├── components/          # Reusable UI components
│   │   ├── layout/          # Navbar, Sidebar, Footer
│   │   ├── forms/           # Input, Select, CSVUploader
│   │   ├── tables/          # DataTables, PaginatedTable
│   │   └── charts/          # Recharts/Chart.js components
│   │
│   ├── features/            # Feature-based folders
│   │   ├── auth/            # Login, Register, ForgotPassword
│   │   ├── employees/       # EmployeeList, EmployeeForm
│   │   ├── campaigns/       # CampaignList, CampaignForm
│   │   ├── events/          # Event tracking views
│   │   └── reports/         # ReportDashboard, PDFViewer
│   │
│   ├── hooks/               # Custom React hooks
│   │   ├── useAuth.js
│   │   ├── useApi.js
│   │   └── useCsvParser.js
│   │
│   ├── context/             # React context (AuthContext, ThemeContext)
│   │
│   ├── lib/                 # Utility functions
│   │   ├── validators.js    # Email, CSV validation
│   │   ├── formatters.js    # Date/number formatting
│   │   └── constants.js
│   │
│   ├── pages/               # Route pages
│   │   ├── Dashboard.jsx
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   ├── Campaigns.jsx
│   │   ├── Employees.jsx
│   │   └── Reports.jsx
│   │
│   └── styles/              # Global & Tailwind configs
│       ├── index.css
│       └── tailwind.css
│
│── vite.config.js
│── package.json
│── .env
```

✅ **Why this works**:

* **Feature-based structure** → campaigns, employees, reports each live in their own folder.
* Centralized API layer (`api/`) keeps requests clean.
* `components/` has reusable UI building blocks.
* `hooks/` encourage reusability and cleaner code.
* Easy to scale: just add a new `feature/` folder.

---

# 🔗 Shared Development Practices

* **Monorepo Option** → Use a single repo with `backend/` and `frontend/` folders.
* **Dockerize** → Add `docker-compose.yml` to run API, frontend, and Postgres easily.
* **CI/CD Ready** → Add GitHub Actions for tests, lint, build, and deploy.
* **Env Handling** → `.env` files per environment (`.env.dev`, `.env.prod`).
* **Code Quality** →

  * Backend: `black`, `isort`, `flake8`.
  * Frontend: `eslint`, `prettier`.
* **Testing Strategy** →

  * Backend: `pytest` (unit + integration).
  * Frontend: `vitest` + `react-testing-library`.

---

⚡ With this structure, you can:

* Add a new phishing type → just create new service, model, schema, and route.
* Add a new dashboard feature → add a `feature/` folder on frontend, API on backend.
* Scale to microservices later if needed (split campaigns, reports, training).