# fuze-api-gateway

> **TypeScript / Express REST & WebSocket gateway for FUZE.ac**  
> One surface to control bots, query KPI data, manage unlock calendars, and orchestrate OTC bonds.

[![Node 20+](https://img.shields.io/badge/Node.js-20%2B-brightgreen)](https://nodejs.org)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.4-blue)](https://www.typescriptlang.org/)
[![Docker](https://img.shields.io/badge/Docker-ready-blue)](https://www.docker.com/)
[![CI](https://github.com/fuze-ac/fuze-api-gateway/actions/workflows/ci.yml/badge.svg)](./actions)

---

## ✨  Capabilities

| Domain          | Example Route                         | What It Does                                                   |
|-----------------|---------------------------------------|----------------------------------------------------------------|
| **Bots**        | `POST /v1/bots/start`                 | Start a PM2 pair‑worker with chosen strategy mode.             |
|                 | `GET  /v1/bots/:id/status`            | Live PnL, depth, spread, last‑heartbeat.                       |
| **KPI**         | `GET  /v1/kpi/realtime`               | Volume, spread, depth vs exchange thresholds.                  |
|                 | `GET  /v1/kpi/history`                | Time‑series data streamed from TimescaleDB.                    |
| **Unlocks**     | `POST /v1/unlocks/schedule`           | Add/modify cliff & vesting events; syncs AI Pre‑Unlock mode.   |
|                 | `GET  /v1/unlocks`                    | Upcoming + historical unlocks with status.                     |
| **OTC**         | `POST /v1/otc/deal`                   | Create KPI‑bond escrow; returns escrow address & deal code.    |
|                 | `GET  /v1/otc/:dealId/status`         | Track milestone %, next tranche, slash risk.                   |
| **Auth / IAM**  | `POST /v1/auth/login`                 | JWT issuance (RBAC: admin / project / viewer).                 |
| **WebSocket**   | `/ws/kpi`                             | Push realtime KPI delist‑risk alerts & bot lifecycle events.   |

All endpoints documented in **OpenAPI 3.1** (`/docs/openapi.yaml`) and rendered with Swagger‑UI at `/api-docs`.

---

## 🏗  Quick Start

```bash
git clone https://github.com/fuze-ac/fuze-api-gateway.git
cd fuze-api-gateway
pnpm i
cp .env.sample .env       # add DB & JWT secrets
pnpm dev                  # nodemon + ts-node
# or
docker compose up --build
````

Visit **[http://localhost:4000/api-docs](http://localhost:4000/api-docs)** for playground.

---

## ⚙️  Configuration

`.env`

```
PORT=4000
JWT_SECRET=change_me
DATABASE_URL=postgres://...
TIMESCALE_URL=postgres://...
PM2_RPC=http://pm2-mm:9615
KPI_RULES_FILE=./config/kpi-rules.yaml
```

---

## 🔒  Security

* JWT + role‑based guards on each route.
* Rate‑limit middleware (`express-rate-limit`).
* Input validation with **Zod** schemas (auto‑generated from OpenAPI).
* Audit logs stream to Loki (`/middleware/audit.ts`).

---

## 📚  Directory Layout

```
src/
├─ server.ts          # bootstrap
├─ routes/
│   ├─ bots.ts
│   ├─ kpi.ts
│   ├─ unlocks.ts
│   └─ otc.ts
├─ services/
│   ├─ PM2Service.ts
│   ├─ KPIService.ts
│   └─ OTCService.ts
├─ models/            # Prisma schemas
└─ middleware/        # auth, rate‑limit, audit
```

---

## 🧪  Tests

```bash
pnpm test           # Vitest
pnpm run jest       # optional jest e2e
```

Coverage badge generated on CI.

---

## 🚀  Deploy

* **Docker Compose** – includes Postgres + Timescale + Loki stack.
* **Kubernetes Helm chart** (`deploy/helm/`) – HPA on CPU + RPS; secrets via Vault.
* **Serverless** – adapter script for AWS Lambda + API Gateway (`deploy/serverless.ts`).

---

## 🗺  Roadmap

* [ ] GRPC streaming layer for high‑freq bot metrics
* [ ] OAuth 2.0 client‑credential flow
* [ ] Multi‑tenant API key quota dashboards
* [ ] GraphQL gateway (Apollo v4)

---

## 🤝  Contributing

PRs welcome!

1. Fork, create feature branch.
2. `pnpm lint && pnpm test`.
3. Submit PR to `main`, reference an issue.

All contributors must sign the CLA.

---

## 📝  License

MIT © 2025 FUZE Foundation

```
::contentReference[oaicite:0]{index=0}
```
