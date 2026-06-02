# AI Ops System — Master Orchestration Spec

This document defines a complete, production-safe AI-driven infrastructure control system for managing a Tor relay fleet using:

- OpenHands (execution layer)
- Claude (reasoning + planning layer)
- GPT (code generation + automation layer)
- Ansible (deployment enforcement layer)
- Git (system memory layer)
- Tailscale (secure network layer)

The system includes:
- Monitoring layer
- Predictive failure detection
- Autonomous optimization layer
- Safe execution and rollback system

---

# 1. CORE PRINCIPLE

This system is NOT fully autonomous.

It is:

> AI-assisted infrastructure control with strict safety gates and human approval for production changes.

No AI model is allowed to directly modify production without validation layers.

---

# 2. SYSTEM ARCHITECTURE

### Data Flow

Telemetry (Tor relays)  
    ↓  
Monitoring layer  
    ↓  
Event processor  
    ↓  
Predictive risk engine  
    ↓  
Optimization engine (optional)  
    ↓  
OpenHands task creation  
    ↓  
Claude (reasoning)  
    ↓  
GPT (code generation)  
    ↓  
OpenHands (execution)  
    ↓  
Ansible (validation + enforcement)  
    ↓  
Canary deployment  
    ↓  
Gradual rollout  
    ↓  
Git (state + audit log)


---

# 3. LAYER RESPONSIBILITIES

## A. Monitoring Layer
- detects real-time failures
- tracks service health
- collects system metrics
- outputs structured events only

---

## B. Predictive Layer
- detects failure trends
- calculates risk scores
- triggers preventive tasks
- NO direct execution

---

## C. Optimization Layer
- identifies inefficiencies
- proposes improvements
- runs dry-run simulations
- requires approval before changes

---

## D. OpenHands Layer
- executes safe commands
- runs Ansible
- edits repository files
- maintains persistent task state

---

## E. Claude (Reasoning Layer)
Used ONLY for:
- failure analysis
- risk evaluation
- optimization decisions

INPUT RULE:
- NO raw logs
- ONLY summarized metrics

OUTPUT FORMAT:
{
  "decision": "",
  "risk": "low|medium|high",
  "steps": []
}

---

## F. GPT Layer
Used ONLY for:
- Python scripts
- Ansible playbooks
- automation tooling
- config generation

GPT must NOT:
- decide deployments
- evaluate system risk

---

# 4. SAFETY SYSTEM (HARD GUARANTEES)

## Deployment rules

NO production deployment without:

- ansible --check success
- canary node validation
- explicit approval

---

## Rollout limits

- Stage 1: 1 node (canary)
- Stage 2: 10% fleet
- Stage 3: 50% fleet
- Stage 4: full rollout (only after stability confirmation)

---

## Emergency rollback triggers

Immediate rollback if:

- multiple node failure (>2)
- tor restart loop detected
- network instability across tailnet
- failed canary deployment

---

# 5. TAILSCALE SECURITY MODEL

All access MUST go through Tailscale.

- No public SSH
- No public OpenHands exposure
- All nodes communicate via tailnet IPs

---

# 6. EVENT PROCESSING SYSTEM

## Input:
Raw telemetry signals

## Output:
Structured OpenHands task:

{
  "task": "",
  "node": "",
  "severity": "",
  "context": []
}

Only structured data is passed into AI reasoning layers.

---

# 7. FULL OPERATION FLOW

## Standard operation:

1. Monitoring detects event
2. Event processor structures data
3. Predictive engine assigns risk score
4. (Optional) Optimization engine evaluates improvement
5. OpenHands creates task
6. Claude analyzes issue
7. GPT generates fix
8. OpenHands executes in sandbox
9. Ansible validates changes
10. Canary deployment
11. Gradual rollout
12. Git logs everything

---

# 8. INCIDENT RESPONSE FLOW

If failure occurs:

1. Stop deployment immediately
2. Roll back to last stable state
3. Summarize logs using GPT (NOT Claude)
4. Escalate to Claude for reasoning
5. Escalate to Opus only if unresolved
6. Require human approval for retry

---

# 9. IPHONE CONTROL MODEL (TAILSCALE UI)

From iPhone user can:

- view system status
- approve deployments
- approve optimization changes
- trigger rollback
- view summaries

User cannot:

- bypass safety system
- access raw logs
- directly execute commands

---

# 10. COST CONTROL SYSTEM

To minimize API usage:

- GPT mini handles log filtering
- Claude only sees summaries
- no raw logs sent to reasoning models
- max 2 reasoning loops per task

---

# 11. SYSTEM GUARANTEE MODEL

This system guarantees:

- No uncontrolled production changes
- No direct AI fleet manipulation
- All changes validated by Ansible + canary
- Full audit trail via Git
- Safe remote operation via Tailscale
- Recovery via rollback system

---

# 12. FINAL SYSTEM DEFINITION

This is a:

> Multi-layer AI-assisted infrastructure control system for Tor relay operations with strict safety constraints and human-in-the-loop production safeguards.

Capabilities:
- monitoring
- prediction
- optimization
- safe execution
- rollback
- remote mobile control

Restrictions:
- no autonomous production modification
- no unvalidated AI execution
- no raw-log reasoning loops

---

# END OF SPEC
