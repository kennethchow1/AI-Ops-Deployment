# AI Ops System Deployment Plan (Staged)

This guide installs the system in safe phases to avoid breaking production or losing control of the server.

System components:
- OpenHands (execution layer)
- Claude (reasoning layer)
- GPT API (code + automation layer)
- Ansible (deployment enforcement)
- Tailscale (secure network)
- Git (system memory)

---

# PHASE 0 — PREPARATION (DO NOT SKIP)

Goal: Ensure server is safe to modify

## Steps:

- Confirm Debian system is stable
- Confirm SSH access via Tailscale works
- Install:
  - git
  - curl
  - vim/nano
- Create backup snapshot if possible

## Rule:
Do NOT install OpenHands yet.

---

# PHASE 1 — TAILSCALE NETWORK FOUNDATION

Goal: Secure all access paths

## Install Tailscale on:
- control node (Debian server)
- at least 1 test relay node

## Verify:
- nodes can ping each other via tailnet IP
- no public SSH needed

## Hard rule:
All future access MUST use Tailscale only.

---

# PHASE 2 — BASE TOOLING INSTALLATION

Goal: Prepare system environment

Install:

- Docker
- Docker Compose plugin
- Python3 + pip
- Ansible-core
- Git repo structure

Create directory:

/opt/ai-ops/

Initialize:

- ai-ops-system Git repo
- basic folder structure (no AI logic yet)

Commit baseline state.

---

# PHASE 3 — OPENHANDS DEPLOYMENT (EXECUTION LAYER)

Goal: Bring AI execution engine online

Install OpenHands via Docker:

- persistent volume enabled
- browser UI exposed ONLY via Tailscale IP
- restart: always

Verify:

- UI loads from iPhone via Tailscale
- test task executes:
  "run ls on server"

Rule:
Do NOT connect AI models yet.

---

# PHASE 4 — ANSIBLE INTEGRATION

Goal: Enable safe infrastructure control

Install and configure:

- ansible-core
- inventory for:
  - localhost
  - test relay node

Test:

- ansible ping module
- ansible --check mode

Rule:
No production deployment yet.

---

# PHASE 5 — MODEL LAYER INTEGRATION

Goal: Add AI intelligence layer

Configure API access:

## Claude:
- used for reasoning only
- receives only summarized logs

## GPT:
- used for code generation only
- handles Ansible + Python

## Add model routing config:
- Haiku → log compression
- Sonnet → reasoning
- GPT-4.1 → coding
- GPT mini → utilities

Test:
- run fake incident → ensure routing works

---

# PHASE 6 — EVENT PROCESSOR (MONITORING CORE)

Goal: Convert system events into AI tasks

Deploy Python service that:

- reads system logs
- reads Tailscale connectivity
- detects anomalies
- converts them into OpenHands tasks

Test:
- simulate relay failure event
- confirm OpenHands task is created

---

# PHASE 7 — SAFETY SYSTEM ENABLEMENT

Goal: Prevent unsafe deployments

Enable:

- canary deployment requirement
- ansible --check enforcement
- rollout limits (10% → 50% → 100%)
- rollback automation

Test:
- simulate bad deployment
- ensure rollback triggers

---

# PHASE 8 — FULL AI LOOP INTEGRATION

Goal: Connect full system

Enable workflow:

Event → OpenHands → Claude → GPT → Ansible → Git

Test scenario:
- fake Tor relay failure
- system must:
  1. detect
  2. create task
  3. analyze
  4. generate fix
  5. apply safely to canary
  6. rollback if needed

---

# PHASE 9 — IPHONE OPERATION MODE

Goal: Enable remote control

From iPhone (via Tailscale):

- access OpenHands UI
- view tasks
- approve deployments
- trigger rollback

Verify:
- no SSH needed
- no raw system access exposed

---

# PHASE 10 — PRODUCTION ENABLEMENT

Only enable after all tests pass.

Checklist:

[ ] Tailscale stable
[ ] OpenHands stable
[ ] Ansible validated
[ ] Claude routing correct
[ ] GPT routing correct
[ ] rollback tested
[ ] canary deployment works
[ ] event processor stable

If ANY fail → do not proceed.

---

# FINAL STATE

System is now:

- self-monitoring
- safely automated
- rollback-protected
- mobile controllable
- AI-assisted but not AI-dependent

END
