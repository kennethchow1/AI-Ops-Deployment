# AI Ops Safe Integration Bridge Layer

This layer sits between AI systems (Claude, GPT, OpenHands) and infrastructure execution (Ansible + Tor relays).

Its purpose is to ensure:

- No automatic production changes
- No uncontrolled Ansible execution
- No AI-triggered fleet-wide modifications
- Strict human approval gates remain in place

---

# 1. CORE PRINCIPLE

AI systems are NEVER allowed to directly modify production systems.

All actions MUST pass through:

AI → Proposal → Review → Approval → Execution

NOT:

AI → Execution

---

# 2. EXECUTION BOUNDARY MODEL

## Allowed AI actions

- generate Ansible playbooks
- propose infrastructure changes
- simulate deployments
- analyze logs (summarized only)
- create OpenHands tasks

## Forbidden AI actions

- running Ansible against production automatically
- SSH into multiple nodes without approval
- modifying relay fleet configuration directly
- bypassing canary validation

---

# 3. OPENHANDS SAFETY MODE

OpenHands MUST operate in one of three modes:

## A. Sandbox Mode (DEFAULT SAFE MODE)

- can only run on localhost
- no access to production inventory
- cannot execute Ansible on real nodes

## B. Read-Only Mode

- can analyze systems
- can generate plans
- cannot execute changes

## C. Controlled Execution Mode (RESTRICTED)

- requires explicit human approval token
- only allows:
  - single-node execution (canary only)
  - ansible --check before apply
- time-limited session (auto-expire)

---

# 4. ANSIBLE EXECUTION GATES

All Ansible runs MUST pass through a gate system:

## Gate 1 — Validation Gate

Required checks:
- ansible-playbook --check passes
- inventory verified (no unexpected nodes)
- diff reviewed

## Gate 2 — Canary Gate

- only 1 node allowed
- must run successfully for defined time window
- health metrics must remain stable

## Gate 3 — Approval Gate

- explicit human approval required (iPhone or CLI)
- approval must be time-bound (expires after N minutes)

## Gate 4 — Rollout Gate

- 10% → 50% → 100% staged rollout
- each stage requires re-validation

---

# 5. HUMAN APPROVAL SYSTEM

## Approval mechanism

All production changes require:

- explicit approval token
- generated per task
- valid for limited time only

Example:

APPROVAL_TOKEN:
- task_id: relay-update-193
- expires: 10 minutes
- scope: 1-node canary only

---

## Approval rules

- tokens cannot be reused
- tokens expire automatically
- tokens are required for ANY Ansible apply

---

# 6. LOOP PREVENTION SYSTEM (CRITICAL)

To prevent AI “fix loops”:

## Hard limits:

- max 2 remediation attempts per incident
- max 1 optimization attempt per 24h per node
- no repeated playbook execution without state change detection

## If loop detected:

SYSTEM ACTION:
- freeze automation
- escalate to human review
- log incident in Git

---

# 7. TAILSCALE SECURITY BOUNDARY

All infrastructure access MUST be:

- routed via Tailscale only
- isolated per node
- no public SSH or API endpoints

---

# 8. OPENHANDS → ANSIBLE CONTROL FLOW

Correct execution pipeline:

1. OpenHands generates plan
2. Plan reviewed (Claude)
3. GPT generates Ansible code
4. OpenHands runs ansible --check ONLY
5. Human approves
6. OpenHands executes on CANARY ONLY
7. Monitoring validates result
8. Gradual rollout if safe

---

# 9. FAILURE HANDLING RULES

If ANY failure occurs:

- STOP all automation immediately
- rollback to last known stable state
- generate summary using GPT (not Claude)
- escalate to Claude for reasoning
- require human approval before retry

---

# 10. SAFETY GUARANTEE MODEL

This system guarantees:

- no autonomous production changes
- no uncontrolled fleet operations
- no AI bypass of Ansible safety gates
- full auditability via Git logs
- full rollback capability

---

# 11. FINAL SYSTEM ROLE DEFINITION

AI systems are:

- advisors (Claude)
- generators (GPT)
- executors (OpenHands)

BUT NEVER:
- autonomous operators of production systems

---

# END OF SAFE INTEGRATION BRIDGE
