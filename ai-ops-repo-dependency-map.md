# AI Ops System — Dependency Map & Execution Order

This document defines:

- file dependency order
- system layer hierarchy
- safe execution sequence for Claude/OpenHands
- circular dependency prevention rules

It ensures the AI does not misinterpret or reorder system logic.

---

# 1. CORE PRINCIPLE

The system MUST always be read and executed top-down:

1. Safety first
2. Execution last
3. Optimization never overrides safety
4. Prediction never triggers direct execution

---

# 2. SYSTEM LAYER HIERARCHY

## L0 — SAFETY LAYER (HIGHEST PRIORITY)

- ai-ops-safe-integration-bridge.md

Purpose:
- controls all execution permissions
- enforces human approval
- prevents unsafe automation

ALL OTHER LAYERS DEPEND ON THIS.

---

## L1 — CORE ORCHESTRATION

- ai-ops-master-orchestration.md

Purpose:
- defines full system architecture
- defines execution pipeline
- defines model responsibilities

Depends on:
- Safety Layer

---

## L2 — OPERATIONAL LAYERS

### Monitoring
- tor-monitoring-system.md

### Predictive
- ai-ops-predictive-layer.md

### Optimization
- ai-ops-autonomous-optimization-layer.md

Depends on:
- Master Orchestration
- Safety Layer

---

## L3 — EXECUTION LAYER

- OpenHands (runtime system)
- Ansible (deployment enforcement)

Depends on:
- Safety Layer
- Master Orchestration
- Approval Gates

---

## L4 — DEPLOYMENT LOGIC

- ai-ops-deployment-plan.md

Purpose:
- defines staged rollout order
- defines installation sequence

Depends on:
- All above layers (read-only dependency)

---

# 3. FILE DEPENDENCY GRAPH

ai-ops-safe-integration-bridge.md
        ↓
ai-ops-master-orchestration.md
        ↓
monitoring layer
        ↓
predictive layer
        ↓
optimization layer
        ↓
OpenHands runtime layer
        ↓
Ansible execution layer
        ↓
ai-ops-deployment-plan.md
        ↓
Git (system state + audit log)


---

# 4. CLAUDE READ ORDER (CRITICAL)

When Claude is given this repo, it MUST read in this order:

1. ai-ops-safe-integration-bridge.md
2. ai-ops-master-orchestration.md
3. ai-ops-deployment-plan.md
4. ai-ops-predictive-layer.md
5. ai-ops-autonomous-optimization-layer.md
6. tor-monitoring-system.md

If order is not followed:
→ system design may be misinterpreted
→ unsafe execution may occur

---

# 5. OPENHANDS EXECUTION ORDER

OpenHands MUST follow this sequence:

## Phase 1
- Read safety bridge only
- Confirm constraints

## Phase 2
- Read orchestration spec
- Build internal task model

## Phase 3
- Read deployment plan
- Prepare execution graph

## Phase 4
- Execute monitoring layer first (read-only)

## Phase 5
- Enable predictive layer (read-only)

## Phase 6
- Enable optimization layer (dry-run only)

## Phase 7
- Enable Ansible execution (canary only)

---

# 6. CIRCULAR DEPENDENCY PREVENTION

The system MUST NOT:

- let predictive layer trigger execution directly
- let optimization bypass safety bridge
- let monitoring trigger Ansible directly
- allow OpenHands to self-authorize deployments

ALL execution MUST pass through:

Safety Bridge → Approval → Ansible

---

# 7. EXECUTION GUARANTEE MODEL

No system component is allowed to:

- override safety layer
- bypass approval gates
- execute without validation chain
- escalate risk without human context

---

# 8. FAILURE ISOLATION RULE

If ANY layer fails:

- isolate affected layer only
- do not propagate failure assumptions
- do not trigger global rollback automatically
- escalate to human review

---

# 9. FINAL SYSTEM UNDERSTANDING

The system is NOT autonomous.

It is:

> A layered AI-assisted infrastructure control system with strict dependency ordering and enforced safety boundaries.

---

# END OF DEPENDENCY SPEC
