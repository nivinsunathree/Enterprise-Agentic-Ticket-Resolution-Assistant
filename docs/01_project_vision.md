# Project Vision — Agentic Ticket Assistant

> Day 1 deliverable. This document anchors the whole project: what we're
> building, why, for whom, and — critically — what "done well" means.

## The one-sentence pitch

An agentic assistant for IT Support and SRE teams that triages incoming
tickets, grounds its analysis in past tickets and knowledge-base articles,
proposes resolutions, and escalates to a human whenever it isn't confident —
built to run reliably in production, not just to demo.

## The problem

IT support and SRE teams drown in repetitive tickets. Each one costs time to
read, classify, route to the right group, and resolve — and much of that work
repeats, because similar incidents have been solved before. The knowledge to
resolve a ticket usually already exists, scattered across past tickets,
runbooks, and KB articles; it's just not surfaced at the moment it's needed.

## The opportunity

This domain is a strong fit for an agent because:

- **High volume, high repetition** — automation leverage is large.
- **Grounded in retrievable text** — runbooks, past tickets, and KB articles
  are exactly what retrieval-augmented generation needs.
- **A natural safety boundary** — reading logs and suggesting fixes is
  low-risk; taking destructive actions (restarting services, closing tickets)
  requires human approval. This gives us a clean place to demonstrate
  human-in-the-loop control.
- **Measurable success** — resolution rate, time-to-triage, and escalation
  rate are real, trackable metrics, not vibes.

## What it is — and is not

This is an **agent**, not a chatbot and not a plain RAG system:

- A **chatbot** would only answer questions from the model's own knowledge.
- A **RAG system** would answer grounded in our documents, but still only
  produce text.
- Our **agent** decides its own steps and *acts* via tools: it looks up
  similar tickets, checks logs, reads dashboards, drafts an update, and
  escalates — choosing which of these to do based on the ticket in front of
  it.

Where the steps are genuinely predictable, we will deliberately use simpler
**workflow** patterns rather than full agent autonomy, because workflows are
more reliable, cheaper, and easier to debug. Choosing the simplest pattern
that works is a core design principle of this project, not a compromise.

## Who it's for

- **Primary:** L1/L2 support engineers who triage and resolve tickets.
- **Secondary:** SRE on-call engineers handling incidents.
- **Stakeholder:** the support team lead, who cares about throughput,
  escalation rate, and not introducing risk.

## Scope (v1)

**In scope**
- Ticket quality scoring and missing-information detection
- Assignment-group and priority prediction
- Surfacing similar past tickets and relevant KB articles (RAG)
- Drafting a suggested resolution or next step
- Escalating to a human when confidence is low or an action is risky

**Out of scope (for v1)**
- Taking destructive or irreversible actions automatically
- Integrating with a live production ticketing system (we use realistic
  sample data; integration is a later milestone)
- Handling tickets outside IT support / SRE

## Reliability goals

Reliability is a first-class goal of this project. A demo that works once is
not the target; a system that behaves predictably, fails safely, and can be
debugged is. These targets will be measured by the evaluation harness built
later in the project, and refined as we gather real numbers.

| Goal | Target (initial) | Why it matters |
|---|---|---|
| **Groundedness** | Every factual claim traceable to a retrieved source | Prevents confident hallucination — the top failure mode |
| **Escalation rate** | The agent escalates rather than guesses when unsure | A high-but-honest escalation rate beats a low-but-wrong one |
| **Human-override rate** | Track how often a human corrects the agent | The core trust metric; should fall over time as the agent improves |
| **Tool correctness** | Tools called with valid inputs, errors handled | A failing tool must never crash the agent |
| **Safe-by-default** | No risky action without explicit human approval | The non-negotiable safety boundary |

> Note: numeric thresholds are intentionally left open here. We set them once
> the evaluation harness (later milestone) produces a baseline — setting
> targets before measurement would be guesswork.

## How we'll know it works

- A golden dataset of realistic tickets with expected outcomes.
- Regression tests that run on every change.
- Observability: traces of every tool call, plus the metrics above on a
  simple dashboard.

## Guiding principles

1. **Reliability over capability.** Boring and dependable beats clever and
   flaky.
2. **Simplest pattern that works.** Workflow before agent; one tool before
   many.
3. **Grounded, not guessed.** Answers cite their sources.
4. **Safe by default.** The human stays in control of anything risky.
5. **Observable and testable.** If we can't measure it, we can't trust it.

## Roadmap at a glance

This project is built over a structured roadmap: LLM and reasoning foundations,
tools and function calling, planning and workflows, memory and RAG,
architecture and state, human-in-the-loop and guardrails, multi-agent systems,
MCP integration, observability, evaluation, security and governance, the IT
support use case, the application, and final packaging.

---

*This is a living document. It will be revised as the design evolves — and
that revision history is itself part of the story the project tells.*