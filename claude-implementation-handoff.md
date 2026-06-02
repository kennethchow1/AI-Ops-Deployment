You are an infrastructure implementation agent.

Your task is to take the following system design and turn it into a working, deployable setup on a Debian server.

This system manages:
- Ansible controller
- Python automation scripts
- Tor relay fleet operations
- AI-assisted infrastructure workflows

It uses:
- OpenHands (execution layer)
- Claude (reasoning layer)
- GPT (code generation layer via API)
- Git (source of truth)
- Ansible (deployment enforcement)

---

# 1. OBJECTIVE

Build a working AI operations system that:

1. Runs on a Debian server
2. Uses OpenHands as a persistent execution environment
3. Uses Claude for planning + decision-making
4. Uses GPT API for code generation tasks
5. Enforces safe Ansible deployment workflows
6. Supports async task execution (user can disconnect and return later)
7. Is accessible via browser (mobile-friendly / iPhone compatible)
8. Prevents unsafe mass deployment to Tor relays

---

# 2. CORE ARCHITECTURE TO IMPLEMENT

## Components

### OpenHands
- Runs as Docker service on Debian server
- Provides web UI
- Executes shell commands
- Runs Ansible
- Maintains persistent task state
- Stores workspace in Git

### Claude
- Used for:
  - infrastructure reasoning
  - deployment planning
  - failure analysis
- Must NOT execute commands directly
- Only produces structured decisions

### GPT API
- Used for:
  - Python scripts
  - Ansible YAML generation
  - code refactoring
- Must NOT decide architecture or deployments

### Git repository
- Stores:
  - system state
  - Ansible playbooks
  - logs (summarized)
  - workflows
- Acts as system memory

---

# 3. REQUIRED REPO STRUCTURE

Create:

ai-ops-system/
├── architecture.md
├── model-routing.md
├── safety-rules.md
├── deployment-checklist.md
├── openhands-config.md
├── prompts/
├── workflows/

Populate them with the provided design (system already defined).

---

# 4. MODEL ROUTING RULES (STRICT)

## Claude Sonnet (default reasoning model)
Use for:
- failure analysis
- Ansible rollout planning
- system decisions

Input constraints:
- NO raw logs
- only summarized data

Output:
- structured JSON decision format

---

## Claude Opus (emergency only)
Trigger only when:
- multiple relay failures occur
- Sonnet fails to resolve issue after 2 attempts
- system-wide instability detected

---

## GPT-4.1
Use for:
- Python scripts
- Ansible YAML generation
- automation tools

---

## GPT mini/nano
Use for:
- log summarization
- error extraction
- formatting tasks

---

# 5. SAFETY REQUIREMENTS (CRITICAL)

Implement hard safety constraints:

1. No production deployment without:
   - Ansible --check success
   - Canary node validation
   - Claude approval

2. Rollout limits:
   - Max 10% relays initially
   - Max 50% second phase
   - Full rollout only after validation

3. AI restrictions:
   - No direct mass operations on fleet
   - No skipping canary stage
   - No raw logs to reasoning model

---

# 6. OPENHANDS SETUP REQUIREMENTS

Deploy OpenHands with:

- Docker-based installation
- Web UI accessible remotely
- Persistent storage volume
- Git integration
- Tool access:
  - shell
  - python
  - git
  - ansible

Ensure:
- system restarts preserve state
- tasks can continue after disconnect
- browser access works on mobile (iPhone)

---

# 7. WORKFLOW IMPLEMENTATION

Implement this pipeline:

1. User submits task in OpenHands UI
2. Claude produces plan
3. GPT generates code or scripts
4. OpenHands executes actions
5. Ansible validates changes
6. Canary deployment runs
7. Gradual rollout if safe
8. Results stored in Git

---

# 8. INCIDENT RESPONSE FLOW

If failure occurs:

1. Stop deployment immediately
2. Rollback to last stable state
3. Summarize logs using GPT mini
4. Escalate to Claude Sonnet
5. If unresolved → Claude Opus
6. Require human approval before retry

---

# 9. OUTPUT REQUIREMENTS

You must produce:

1. Full installation steps for Debian server
2. Docker configuration for OpenHands
3. Git repository initialization
4. Model routing implementation (code or config)
5. Ansible integration setup
6. Safety guardrails implementation
7. Example workflow execution

---

# 10. CONSTRAINTS

- System must prioritize safety over autonomy
- Must avoid uncontrolled multi-node changes
- Must be restart-safe
- Must be usable from iPhone browser
- Must minimize token usage via log filtering
