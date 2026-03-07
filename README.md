# TraceWeave

TraceWeave is a unified platform for **API development, distributed tracing, automated testing, traffic replay, failure simulation, and performance analysis** for local and cloud-based microservices.

---

## Getting Started

### Prerequisites

Ensure the following are installed:

* **Node.js**: v18+
* **Docker & Docker Compose**: For running the database and infrastructure
* **Git**: For version control and submodule management

---

### Cloning the Repository

This project uses Git submodules. Clone recursively:

```bash
git clone --recursive https://github.com/your-username/traceweave.git
cd traceweave
```

If you already cloned without submodules:

```bash
git submodule update --init --recursive
```

---

## Running the Application

### Option A: Using Docker (Recommended)

The project provides a multi-container setup for the API, database, and other services:

```bash
docker compose -f ./docker-compose.dev.yml -f ./docker-compose.yml up --build
```

### Option B: Local Manual Start

#### Backend (Core API)

```bash
cd backend/core-api
npm install
npx prisma generate   # Ensure Prisma client is generated
npm run dev           # Start in development mode
```

#### Frontend

```bash
cd frontend
npm install
npm run dev
```

---

## Submodule Branching Strategy

The repository is a **parent shell** for the `backend` and `frontend` submodules.

1. **Isolation**: Work on features within the respective submodule directories.

For the dev branch to develop on the backend submodule:
```bash
cd backend
git checkout dev
```

For the dev branch to develop on the frontend submodule:
```bash
cd frontend
git checkout dev
```

---

## Troubleshooting

| Issue                            | Solution                                                                      |
| -------------------------------- | ----------------------------------------------------------------------------- |
| **Submodule directory is empty** | `git submodule update --init --recursive`                                     |
| **Prisma "Client not found"**    | Run `npx prisma generate` inside `backend/core-api`                           |
| **Docker port conflicts**        | Stop local Postgres or Node instances on ports `5432`, `5000`, or `3000`      |
| **Frontend/Backend Sync**        | Pull the latest pointers in the root repo: `git pull && git submodule update` |

---

## Project Structure

```
/frontend      # Next.js application (submodule)
/backend       # Express/Prisma microservices (submodule)
/database      # Migrations and database configuration
/gateway       # API Gateway configuration
```
