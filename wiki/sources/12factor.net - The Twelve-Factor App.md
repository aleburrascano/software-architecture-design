---
type: source
created: 2026-05-03
updated: 2026-05-03
source_path: https://12factor.net/
source_date: 2011
source_author: Adam Wiggins (Heroku)
tags:
  - cloud-native
  - twelve-factor
  - devops
  - heroku
---

# 12factor.net — The Twelve-Factor App

A methodology for building software-as-a-service apps published by Adam Wiggins and the Heroku team at [https://12factor.net/](https://12factor.net/).

## Summary

The twelve-factor methodology was distilled from experience operating and deploying millions of apps on the Heroku platform. It describes twelve principles for building cloud-native, horizontally scalable, deployable applications.

## Key Takeaways

1. **Codebase** — One codebase, many deploys; shared code via dependency managers, not shared repos.
2. **Dependencies** — Explicitly declare all dependencies; no implicit system-level packages.
3. **Config** — Store config in environment variables; never in code or named environment groups.
4. **Backing Services** — Local and remote services are interchangeable attached resources.
5. **Build, Release, Run** — Three separate, immutable stages; releases are versioned.
6. **Processes** — Stateless, share-nothing processes; all persistence via backing services.
7. **Port Binding** — App is self-contained, binds its own port.
8. **Concurrency** — Scale by adding stateless processes, not by threading.
9. **Disposability** — Fast startup, graceful shutdown; crash-resilient.
10. **Dev/Prod Parity** — Minimize time, personnel, and tools gaps between environments.
11. **Logs** — Write to stdout; let the platform route and store.
12. **Admin Processes** — Run as one-off processes in the same environment as the app.

## Wiki Pages That Cite This Source

- [[Twelve-Factor App]] — full treatment of all 12 factors
- [[Continuous Integration and Delivery]] — Factor V (Build/Release/Run)
- [[Infrastructure as Code]] — Factor IV and Factor X
- [[Blue-Green Deployment]] — Factor IX (Disposability)
- [[Canary Release]] — Factor V (immutable releases)
