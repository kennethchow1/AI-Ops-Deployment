# AI Ops Predictive Layer (Tor Relay System)

This layer sits above monitoring and below optimization.

Its purpose is to predict failures before they happen using trend-based analysis.

It does NOT execute changes.

It only:
- detects risk trends
- assigns risk scores
- triggers preventive OpenHands tasks
- informs Claude reasoning layer

---

# 1. PURPOSE

The predictive layer prevents infrastructure failure by identifying:

- degradation trends
- resource exhaustion patterns
- configuration instability
- network deterioration

It is a **warning system, not an execution system**.

---

# 2. INPUT DATA

This layer consumes only aggregated metrics:

## A. System metrics
- CPU usage over time
- memory growth trends
- disk usage growth

## B. Service metrics
- tor restart frequency
- service uptime trends
- crash patterns

## C. Network metrics
- Tailscale reconnect frequency
- latency drift
- packet loss trends

## D. Configuration signals
- repeated Ansible corrections
- ORPort mismatch frequency
- config drift history

---

# 3. RISK SCORING MODEL

Each node receives a risk score:

Risk Score =

+ restart trend score
+ resource growth slope
+ config drift frequency
+ network instability trend

---

## Risk thresholds

- 0–30 → stable
- 31–60 → monitor
- 61–80 → warning (create preventive task)
- 81–100 → critical (immediate preventive action recommended)

---

# 4. OUTPUT FORMAT

The system outputs structured risk objects:

{
  "node": "relay-01",
  "risk_score": 72,
  "status": "warning",
  "likely_issue": "memory leak or process instability",
  "recommendation": "restart tor service and inspect config drift",
  "time_horizon": "24-72h"
}

---

# 5. CLAUDE ROLE

Claude is used ONLY for interpretation.

## Input:

Aggregated trend summary only:

relay-01:
- CPU baseline rising over 5 days
- memory usage +15% weekly
- no current outage

## Output:

{
  "risk": "high",
  "likely_failure": "gradual memory leak in tor process",
  "recommended_action": "restart service + validate config consistency",
  "urgency": "within 24-48h"
}

---

# 6. TASK CREATION RULES

A predictive OpenHands task is created ONLY if:

- risk score ≥ 60
- or accelerating degradation detected
- or repeated anomalies over time window

---

## Example task:

{
  "task": "Preventive maintenance for relay-01",
  "type": "predictive",
  "severity": "medium",
  "reason": "rising memory usage trend + config drift"
}

---

# 7. OPENHANDS ROLE

OpenHands may:

- run diagnostics
- perform dry-run Ansible checks
- simulate restarts
- validate configs

OpenHands MUST NOT:

- deploy changes without approval
- run fleet-wide actions
- bypass canary stage

---

# 8. SAFETY RULES

## Hard constraints:

- NO automatic production changes
- NO full fleet remediation from prediction alone
- NO execution without validation pipeline

---

## Required validation chain:

Prediction → Task → Claude → GPT → OpenHands → Ansible --check → Canary → Approval

---

# 9. LOOP PREVENTION

To avoid false-positive cycles:

- suppress repeated alerts for same node within time window
- require trend confirmation (not single spike)
- minimum 2 data points before escalation

---

# 10. SYSTEM BEHAVIOR

The predictive layer ensures:

- early detection of infrastructure degradation
- reduced downtime risk
- proactive maintenance planning
- safe integration with existing monitoring system

BUT always defers execution to higher safety layers.

---

# END OF SPEC
