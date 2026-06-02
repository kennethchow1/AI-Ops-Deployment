# Tor Relay Autonomous Monitoring System

This document defines a production monitoring + auto-remediation system for a Tor relay fleet using:

- Debian control node
- Tailscale private network
- OpenHands execution layer
- Claude (reasoning engine)
- GPT (code generation + log processing)
- Ansible (deployment enforcement)
- Git (system memory)

---

# 1. SYSTEM PURPOSE

This system continuously monitors Tor relay health and automatically:

- Detects failures
- Summarizes issues
- Creates OpenHands tasks
- Proposes fixes via AI
- Executes safe remediation via Ansible
- Rolls back unsafe changes

---

# 2. ARCHITECTURE

## Data Flow

Tor Relay Nodes
   ↓
Local monitoring scripts
   ↓
Tailscale network transport
   ↓
Central Debian AI Ops Node
   ↓
Event Processor (Python service)
   ↓
OpenHands Task Queue
   ↓
Claude (analysis + reasoning)
   ↓
GPT (code + fix generation)
   ↓
OpenHands execution engine
   ↓
Ansible deployment + validation
   ↓
Git (system memory)

---

# 3. MONITORED SIGNALS

Each relay node must collect:

## A. Service Health
- tor service status
- restart frequency
- crash loops

## B. System Health
- CPU usage
- memory usage
- disk usage

## C. Network Health
- Tailscale connectivity
- latency to control node
- packet loss

## D. Configuration Drift
- torrc checksum comparison
- Ansible last applied state

---

# 4. EVENT PROCESSOR (CORE COMPONENT)

A Python service runs on the control node.

It converts raw alerts into structured tasks.

## Example Input

relay-03:
- tor restart loop detected
- ORPort mismatch
- unreachable via Tailscale

## Example Output (OpenHands Task)

{
  "task": "Investigate Tor relay instability",
  "node": "relay-03",
  "severity": "medium",
  "context": [
    "restart loop",
    "ORPort mismatch",
    "unreachable"
  ]
}

---

# 5. TASK CREATION RULES

## Create OpenHands task IF:

- tor restart loop detected
- node unreachable > 2 minutes
- CPU > 90% sustained
- config drift detected
- repeated service failure

---

## DO NOT create task IF:

- short latency spike
- temporary packet loss
- transient CPU burst

---

# 6. CLAUDE ROLE (REASONING LAYER)

Claude is used ONLY for reasoning.

## Input constraints:
- NO raw logs
- ONLY summarized events

## Example input:

relay-03:
- restart loop
- ORPort mismatch
- single node affected

## Output format:

{
  "cause": "",
  "fix": "",
  "risk": "low|medium|high"
}

---

# 7. GPT ROLE (WORKER LAYER)

GPT is used for:

- Python scripts
- Ansible YAML generation
- config patching
- code refactoring

GPT MUST NOT:
- decide deployments
- assess system risk
- make architecture decisions

---

# 8. OPENHANDS ROLE (EXECUTION LAYER)

OpenHands is responsible for:

- executing shell commands
- running Ansible playbooks
- editing repository files
- running diagnostics
- maintaining persistent tasks

OpenHands MUST NOT:
- bypass safety rules
- deploy without Ansible validation
- run uncontrolled fleet-wide commands

---

# 9. AUTOMATION FLOW

## Standard Flow

1. Monitoring detects anomaly
2. Event processor creates structured task
3. OpenHands starts task
4. Claude analyzes cause
5. GPT generates fix
6. OpenHands executes fix
7. Ansible validates change
8. Canary deployment runs
9. Gradual rollout if safe
10. Results stored in Git

---

# 10. SAFETY RULES (CRITICAL)

## Hard Constraints

- No production deployment without:
  - ansible --check success
  - canary node validation
  - Claude approval

---

## Rollout limits:

- 1 node (canary)
- 10% fleet
- 50% fleet
- 100% only after validation

---

## Emergency rollback triggers:

- multiple node failure (>2)
- tor restart loop detected
- Tailscale connectivity loss
- Ansible failure escalation

---

# 11. INCIDENT RESPONSE

If failure occurs:

1. Stop all deployment actions
2. Roll back to last known stable state
3. Summarize logs using GPT (NOT Claude)
4. Escalate to Claude analysis
5. Escalate to Opus only if unresolved
6. Require human approval for retry if critical

---

# 12. TAILSCALE INTEGRATION

All communication MUST use Tailscale.

## Rules:

- No public SSH exposure
- All nodes connected via tailnet
- Control node accessed only via Tailscale IP

---

# 13. IPHONE CONTROL MODEL

From mobile device:

User can:

- view OpenHands tasks
- approve deployments
- trigger rollback
- see summarized status

User cannot:

- access raw logs
- execute direct SSH commands
- bypass safety system

---

# 14. COST CONTROL RULES

To minimize token usage:

- raw logs → GPT mini/nano first
- summaries only → Claude
- Claude limited to 2 reasoning loops per task
- no repeated full log uploads

---

# 15. SYSTEM BEHAVIOR SUMMARY

This system behaves as:

A controlled AI-driven SRE system for Tor relay infrastructure with:

- automated detection
- structured reasoning
- safe execution
- rollback protection
- mobile control via Tailscale

---

# END OF SPEC
