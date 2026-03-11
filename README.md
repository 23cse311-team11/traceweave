# TraceWeave

TraceWeave is a unified platform for API development, distributed tracing, automated testing, traffic replay, failure simulation, and performance analysis for local and cloud-based microservices.

---

## Table of Contents

1. [Using TraceWeave (Hosted Platform)](#1-using-traceweave-hosted-platform)
2. [Developer Setup (Local Development)](#2-developer-setup-local-development)
3. [System Design](#3-system-design)
4. [Contribution Guide](#4-contribution-guide)

---

## 1. Using TraceWeave (Hosted Platform)

If you are a general user, you do not need to run the project locally.

### Access the Platform

Open the hosted application in your browser:

```
https://app.traceweavesupply.tech/
```

### What You Can Do

TraceWeave allows you to:

- Upload and test APIs
- Run automated tests on API endpoints
- Analyze service performance

### Account Access

1. Create an account or log in.
2. Create a project.
3. Upload API definitions.
4. Start testing and analyzing your services.

> All processing is handled by the hosted infrastructure, so no local installation is required.

If you want to have a desktop installtion you have two options:
1. Download the release from traceweave-desktop.
2. Make an account on the hosted webiste and click on the download option.

---

## 2. Developer Setup (Local Development)

This section is intended for developers contributing to the TraceWeave platform.

### Prerequisites

Ensure the following tools are installed:

- **Node.js:** v18 or newer
- **Docker**
- **Docker Compose**
- **Git**

### Cloning the Repository

This project uses Git submodules for the frontend and backend.

Clone the repository with submodules:

```bash
git clone --recursive https://github.com/23cse311-team11/traceweave.git
cd traceweave
```

If the repository was cloned without submodules:

```bash
git submodule update --init --recursive
```

### Environment Configuration

Developers must configure environment variables for backend services. Create the required environment files.

Example file path:

```
backend/core-api/.env.docker
```

Example configuration:

```env
NODE_ENV=development
PORT=4000
MONGO_URI=mongodb://mongo:27017/traceweave
JWT_SECRET=your_jwt_secret
EMAIL_SERVICE_API_KEY=your_email_api_key
```

> Environment variables may vary depending on the services used (e.g., email providers, authentication providers).

### Running the Platform (Docker Development Environment)

The project provides a multi-container development environment.

Start all services:

```bash
docker compose -f docker-compose.dev.yml up --build
```

This starts:

- Core API
- AI service
- Proxy service
- Frontend
- MongoDB
- Gateway

After startup, the following services will be available:

```
Frontend:  http://localhost:3000
API:       http://localhost:4000
Gateway:   http://localhost
```

### Running Services Manually (Optional)

If you prefer not to use Docker, services can be started manually.

**Backend (Core API)**

```bash
cd backend/core-api
npm install
npx prisma generate
npm run dev
```

**Frontend**

```bash
cd frontend
npm install
npm run dev
```

### Submodule Development Workflow

The root repository contains two submodules:

```
/frontend
/backend
```

Developers should work directly inside the appropriate submodule.

**Backend Development**

```bash
cd backend
git checkout dev
```

**Frontend Development**

```bash
cd frontend
git checkout dev
```

After making changes:

1. Commit inside the submodule.
2. Push the submodule.
3. Update the submodule reference in the root repository.

Example:

```bash
git add backend
git commit -m "Update backend submodule reference"
git push
```

### Troubleshooting

| Issue | Solution |
|-------|----------|
| Submodule directory is empty | `git submodule update --init --recursive` |
| Prisma client missing | Run `npx prisma generate` in `backend/core-api` |
| Docker port conflicts | Stop local services using ports `3000`, `4000`, or `27017` |
| Submodules not updating | Run `git pull && git submodule update --recursive` |

### Project Structure

```
/frontend     Next.js application (submodule)
/backend      Express and microservices (submodule)
/database     Database configuration and migrations
/gateway      API Gateway configuration
```

---

## 3. System Design

| Component | Details |
|-----------|---------|
| **Web Browsers** | Chrome, Firefox, Edge |
| **Desktop OS** | Windows, macOS, Linux |
| **Backend Runtime** | Node.js, Python |
| **Databases** | PostgreSQL, MongoDB |
| **Object Storage** | Cloud-based or self-hosted object storage for binary artifacts |
| **Deployment Model** | Cloud-hosted services with local desktop integration |
| **Desktop Application** | Electron |

---

## 4. Contribution Guide

To get started, contact the organization owner for repository access.

### 4.1 Repository & Branching Strategy

> `main` and `dev` are protected branches. No direct pushes allowed.

#### Branch Hierarchy

| Branch | Role | Description |
|--------|------|-------------|
| `main` | Production | The main production branch. Contains only stable, demo-ready code. Merges occur only at the end of a sprint. |
| `dev` | Integration | The sandbox. This is where daily work merges. It may be unstable, but it must compile. |
| `feat/*` | Working | Temporary branches for specific tasks. |
| `fix/*` | Bug Fix | Temporary branches for bug fixes. |

**Naming conventions:**

- Features: `feat/<task-id>-<short-description>` — e.g., `feat/OP-12-login-ui`
- Bug fixes: `fix/<bug-id>-<description>` — e.g., `fix/OP-45-proxy-crash`

Create and sync a new working branch:

```bash
git checkout -b feat/<task-id>-<short-desc>
git pull origin dev
```

#### Protection Rules

- **Pull Requests (PRs) are mandatory** for any code entering `dev` or `main`.
- **One approval required:** Code must be reviewed by at least one other squad member before merging.

### 4.2 Testing

Write unit tests for every feature you implement using **Jest**.

### 4.3 CI/CD Pipeline

GitHub Actions is configured for automated testing and deployment across the following repositories:

- **`traceweave-frontend`** — GitHub Actions runs unit tests automatically on pushes and pull requests to the `main` and `dev` branches.
- **`traceweave-backend`** — GitHub Actions runs unit tests automatically on pushes and pull requests to the `main` and `dev` branches.
- **`traceweave` (root)** — GitHub Actions workflow handles deployment, triggered from the `main` branch.