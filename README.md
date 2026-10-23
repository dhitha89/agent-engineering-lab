# Agent Engineering Lab

A structured, build-first path from *knowing about* AI agents to *shipping* them: ten core skills, the best free resources for each, and one project that grows module by module.

**[Open the interactive plan →](https://dhitha89.github.io/agent-engineering-lab/)** (progress tracking, adjustable schedule)

## The ten skills

| # | Module | What it's about | Time | Status |
|---|---|---|---|---|
| 01 | [Harness Engineering](01-harness-engineering/) | Give your agent a safe, observable stack with retries, sandboxing and cost limits so it doesn't spiral out of control. | 7 h | ⬜ |
| 02 | [Loop Engineering](02-loop-engineering/) | Design how the agent repeats actions, checks its own progress and knows when to stop instead of looping forever. | 5 h | ⬜ |
| 03 | [Context Engineering](03-context-engineering/) | Feed the model the right information at the right time. | 8 h | ⬜ |
| 04 | [Tool Design](04-tool-design/) | Build tools the agent can actually use correctly, with clear names, strict input schemas and error handling it can recover from. | 8 h | ⬜ |
| 05 | [Memory Architecture](05-memory-architecture/) | Help the agent remember what matters using short-term, long-term and semantic memory instead of dumping everything into one context window. | 8 h | ⬜ |
| 06 | [Orchestration Patterns](06-orchestration-patterns/) | Coordinate multiple agents so they split tasks, hand off work and combine results without stepping on each other. | 8 h | ⬜ |
| 07 | [Guardrails & Permissions](07-guardrails-permissions/) | Keep the agent useful but safe by scoping which tools it can touch and limiting the blast radius if something goes wrong. | 7 h | ⬜ |
| 08 | [Evals for Agents](08-evals-for-agents/) | Test the agent before you trust it. | 9 h | ⬜ |
| 09 | [Human-in-the-Loop Design](09-human-in-the-loop/) | Know exactly when a human should step in for approval, and when the agent should just run. | 5 h | ⬜ |
| 10 | [Observability & Tracing](10-observability-tracing/) | See every step the agent takes so you can debug fast and ship with confidence instead of guessing. | 7 h | ⬜ |
| C | Capstone | Deploy with ArgoCD, demo, write-up | 12 h | ⬜ |

**Total:** ~84 hours · at 6 h/week ≈ 14 weeks

Each module folder has the infographic, key concepts, curated courses/articles/videos with time estimates, a build step, and a "done when" check.

## The running project: Claims-Triage Agent

One small Python agent (Anthropic SDK, Docker) that answers travel-insurance policy questions and triages a synthetic claim. Every topic adds one layer to it, so by the end you have a single portfolio repo that shows all ten skills. Use made-up policies and claims only, never real customer data.

| Module | Layer it adds |
|---|---|
| 01 Harness Engineering | Scaffold the repo: Dockerfile, a run loop that calls Claude with one dummy tool, a hard token/cost ceiling per run, exponential-backoff retries on 429/5xx, and tool code running in a locked-down container (no network, read-only FS). |
| 02 Loop Engineering | Write the agent loop yourself (no framework): explicit state object, max_steps, a 'same tool + same args 3x' runaway detector, and a final 'done / gave up / needs human' status. |
| 03 Context Engineering | Add policy-document retrieval (reuse your existing RAG know-how), a context builder that assembles layers in a fixed order, and history compaction once the transcript passes a token threshold. |
| 04 Tool Design | Expose three tools as an MCP server: lookup_policy(policy_id), check_coverage(policy_id, incident_type, date), create_claim_draft(. |
| 05 Memory Architecture | Add a per-customer memory store (SQLite + embeddings, e. |
| 06 Orchestration Patterns | Split the agent: an orchestrator routes to a Policy-QA worker and a Claim-Intake worker running in parallel where possible; each worker gets only its own context and returns a typed result. |
| 07 Guardrails & Permissions | Add a permission matrix (read-only tools free, create_claim_draft gated), a PII redaction pass on outputs, an injection test set hidden inside a 'policy PDF', and a Kubernetes NetworkPolicy that blocks tool-container egress. |
| 08 Evals for Agents | Write 30 eval cases (covered / not covered / ambiguous / injection), code graders for tool calls and an LLM judge for answer quality, and run them as a GitLab CI job that fails the pipeline below a threshold. |
| 09 Human-in-the-Loop Design | Before create_claim_draft runs above €1,000 or with low confidence, pause, send the summary to Telegram (as in your Gmail agent) and resume on approve / edit / reject. |
| 10 Observability & Tracing | Self-host Langfuse in Docker (later Helm on your cluster), trace every model and tool call with run IDs, build a cost-per-run and success-rate dashboard, and alert when a run costs over 3x the median. |

## Stack

Python · Anthropic SDK · MCP · Pydantic · Docker · Kubernetes · ArgoCD · Langfuse · GitHub Actions

## How I work through it

1. Read/watch the module's resources (ticked in the module README)
2. Build the layer in the project repo
3. Meet the "done when" check
4. Log it in [progress.md](progress.md)

## Repo layout

```
index.html          interactive plan (GitHub Pages)
NN-topic/           README + infographic per module
progress.md         weekly log
```
