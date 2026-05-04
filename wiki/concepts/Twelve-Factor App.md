---
type: concept
created: 2026-05-03
updated: 2026-05-03
sources:
  - https://12factor.net/
  - https://12factor.net/codebase
  - https://12factor.net/config
  - https://12factor.net/backing-services
  - https://12factor.net/processes
tags:
  - cloud-native
  - devops
  - twelve-factor
  - app-design
  - heroku
---

# Twelve-Factor App

A methodology for building modern, cloud-native web applications — formulated by Adam Wiggins and the Heroku team around 2011 — defining twelve principles that maximise portability, deployability, and scalability in cloud environments.

## Problem

Applications designed for a single server break when deployed to cloud platforms: they store state locally, hardcode configuration, rely on server-specific file systems, and cannot scale horizontally. Cloud platforms (Heroku, AWS, GCP, Azure) need applications designed for ephemerality, horizontal scaling, and continuous deployment.

## Solution / Explanation

The twelve factors address the full application lifecycle from codebase to process management. Each factor targets a specific class of problems in cloud deployment.

---

### I. Codebase

**"One codebase tracked in revision control, many deploys."**

- One codebase per application in a version control system (Git, SVN).
- Multiple deploys (production, staging, local dev) run from the same codebase.
- Multiple apps sharing code should extract shared code into libraries managed via a dependency manager — not by sharing codebases.

---

### II. Dependencies

**"Explicitly declare and isolate dependencies."**

- All dependencies explicitly declared in a manifest (e.g., `package.json`, `requirements.txt`, `go.mod`).
- No reliance on system-wide packages or implicit dependencies.
- Dependency isolation tools (virtual environments, containers) ensure declared dependencies are the only ones present.

---

### III. Config

**"Store config in the environment."**

Config includes anything that varies between deploys (staging, production, developer local): database URLs, credentials, API keys, feature flags.

- Never store credentials or environment-specific config in the codebase (no hardcoding, no config files committed to source control).
- Store in **environment variables** — language-agnostic, OS-standard, impossible to accidentally commit.
- Avoids the proliferation of named environments (`development`, `staging`, `production`) that become unmanageable at scale.

---

### IV. Backing Services

**"Treat backing services as attached resources."**

A backing service is any service consumed over the network: databases (MySQL, PostgreSQL), caches (Memcached, Redis), queues (RabbitMQ), email (Postmark), and third-party APIs.

- No distinction between local and third-party services — both are accessed via a URL/credentials stored in config.
- Swapping a local PostgreSQL for Amazon RDS requires only a config change, no code change.
- Enables [[Infrastructure as Code]] and environment parity.

---

### V. Build, Release, Run

**"Strictly separate build and run stages."**

Three distinct stages:
1. **Build** — compile code, fetch dependencies, create an executable artefact.
2. **Release** — combine the build artefact with deployment config; produces a release (tagged and immutable).
3. **Run** — execute the release's processes in the execution environment.

Releases are immutable: cannot be changed after the fact; roll back by deploying a prior release. This is the foundation of [[Continuous Integration and Delivery]].

---

### VI. Processes

**"Execute the app as one or more stateless processes."**

- Processes are **stateless** and **share-nothing**.
- Any data that must persist is stored in a stateful backing service (database, Redis).
- No local filesystem state that persists between requests.
- "Sticky sessions" (routing a user to the same server) violate this factor; use a session store like Redis instead.
- Enables horizontal scaling: any process instance can serve any request.

---

### VII. Port Binding

**"Export services via port binding."**

- The application is self-contained: it binds to a port and serves HTTP (or other protocol) directly — no external web server (Apache, nginx) required.
- Web servers (Puma, Uvicorn, Jetty) are embedded as dependencies.
- Makes the app a first-class service: another app can treat it as a backing service by pointing to its URL.

---

### VIII. Concurrency

**"Scale out via the process model."**

- Scale by running more processes, not by adding threads.
- Different work types run as different **process types** (web, worker, scheduler).
- Horizontal scaling — spin up more web or worker processes — is the primary scaling mechanism.
- Statelessness (Factor VI) is a prerequisite for this to work.

---

### IX. Disposability

**"Maximize robustness with fast startup and graceful shutdown."**

- Processes start fast (seconds, not minutes) to enable rapid scaling and deployment.
- Processes shut down gracefully: finish current work, stop accepting new work, exit cleanly.
- Robust against sudden death: crash-only design; restarts are cheap.
- Supports deployment to ephemeral cloud infrastructure.

---

### X. Dev/Prod Parity

**"Keep development, staging, and production as similar as possible."**

Three traditional gaps to close:
- **Time gap**: deploy code hours or days after it's written (target: continuous deployment).
- **Personnel gap**: developers write code, operations deploy it (target: same person, DevOps culture).
- **Tools gap**: use the same backing services in development as in production (use PostgreSQL locally, not SQLite).

The closer dev and prod are, the fewer "works on my machine" bugs.

---

### XI. Logs

**"Treat logs as event streams."**

- The app writes logs to `stdout` and `stderr` as unbuffered event streams.
- The execution environment is responsible for capturing, routing, and storing logs.
- Locally: view in the terminal. In production: aggregate to Splunk, ELK, Datadog.
- The app never manages log files, rotations, or retention — that is the platform's responsibility.

---

### XII. Admin Processes

**"Run admin/management tasks as one-off processes."**

- Database migrations, one-time scripts, console sessions run as isolated processes.
- They run in the same environment and against the same release as the production app.
- Prevents the "it works on my machine" problem for admin code.
- Admin processes ship with the application codebase, not as separate tools.

---

## Origins

Developed by Adam Wiggins and the Heroku platform team based on experience with millions of app deployments. Published at [12factor.net](https://12factor.net/). Originally applied to web apps but the principles generalise to any long-running server process.

## When to Use

- Designing a new application targeting cloud deployment.
- Evaluating an existing application's cloud-readiness.
- Establishing engineering standards for a platform team.
- Migrating a monolith to microservices — the twelve factors are a checklist for each service.

## Trade-offs

| Benefit | Cost |
|---------|------|
| Horizontal scalability | Requires stateless design upfront |
| Environment portability | More infrastructure (backing services for session, cache) |
| Continuous deployment readiness | Strict build/release/run separation adds pipeline complexity |
| Dev/prod parity reduces bugs | Local setup with production-equivalent services takes effort |

## Related

- [[Infrastructure as Code]] — automates the infrastructure backing Factor IV (backing services)
- [[Continuous Integration and Delivery]] — implements Factor V (Build, Release, Run)
- [[Microservices Architecture]] — twelve-factor principles apply to each service
- [[Blue-Green Deployment]] — deployment strategy compatible with Factor IX (Disposability)
- [[Canary Release]] — compatible with Factor V (immutable releases)
