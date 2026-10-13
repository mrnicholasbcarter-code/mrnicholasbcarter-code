# Nick Carter: LLM and AI-agent infrastructure (available for contract work)

Senior engineer, 20 years in. Enterprise delivery at AgileThought (for Deloitte and Bankers Surety), Mad Mobile and Blue Cross Blue Shield of Michigan. Since 2023 I've worked independently on AI infrastructure.

I help teams:

- cut LLM cost and outages with multi-provider routing, quotas and automatic fallback
- run AI agents safely: a plan, parallel workers, an independent AI reviewer and an audit log
- add guardrails and audit trails to Python and Node AI APIs

**Hire me:** [Upwork](https://www.upwork.com/freelancers/~01aa0036f6c4dd8a21) · [nick@nickcarter.dev](mailto:nick@nickcarter.dev) · [nickcarter.dev](https://nickcarter.dev)

<a href="https://github.com/mrnicholasbcarter-code/verdict-core#try-it-without-api-keys"><img src="https://raw.githubusercontent.com/mrnicholasbcarter-code/verdict-core/main/docs/assets/demo-tui.svg" alt="Verdict terminal UI: a task graph, a rate-limit reroute, a review step and a receipt check" width="860"></a>

<sub>Offline demo of Verdict's terminal UI. Scripted workers and injected failures; it makes zero model calls.</sub>

## Proof

| Project | What it does |
|---|---|
| [verdict-core](https://github.com/mrnicholasbcarter-code/verdict-core) ([PyPI](https://pypi.org/project/verdict-core/)) | Python CLI and MCP server. Picks the cheapest healthy model per task, reroutes on rate limits and timeouts, and keeps a tamper-evident log of each run. 6,000+ tests. |
| [verdict-node](https://github.com/mrnicholasbcarter-code/verdict-node) ([npm](https://www.npmjs.com/package/@bodanglin/verdict-node)) | Express and Next.js middleware that blocks unapproved LLM calls and forwards approved ones to any OpenAI-compatible API. Alpha. |
| [verdict-cockpit](https://github.com/mrnicholasbcarter-code/verdict-cockpit) | Next.js dashboard for reviewing agent runs: which model ran, what got rejected and why. Read-only, fixture data for now. |

Docs: [nickcarter.dev/docs/verdict](https://nickcarter.dev/docs/verdict)

**Stack:** Python, TypeScript, FastAPI, Express/Next.js, OpenAI/Anthropic, MCP, OpenTelemetry

## Try Verdict without API keys

The install pulls from PyPI. The demo itself needs no key, gateway or network.

```bash
pipx install verdict-core
verdict demo
```

More options are in the [verdict-core README](https://github.com/mrnicholasbcarter-code/verdict-core#try-it-without-api-keys).

## How Verdict works (for engineers)

- Plan, then fan out. A planner splits a goal into a dependency graph, and each node gets its own model and git worktree ([demo run proof](https://github.com/mrnicholasbcarter-code/verdict-core/tree/main/docs/proof/demo-run)).
- Admission before ranking. A model has to pass live account, health and quota checks first, and every dropped candidate gets a named reason ([eligibility.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/eligibility.py)).
- Bounded recovery. A quota, rate-limit or timeout failure cools down the route or provider, and the node moves to another admitted model. After 4 attempts or an empty pool, it stops with a named `FAIL_CLOSED` ([recovery.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/recovery.py)).
- A separate reviewer. Review runs on a route that didn't implement the work, and another model family is preferred ([review.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/review.py)).
- Receipts you can check. Each run stores a SHA-256 digest of its event log, and `verdict run-receipt` recomputes it and flags any edit ([receipt.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/receipt.py)).

Further reading: [orchestration design (ADR-036)](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/docs/adr/ADR-036-goal-to-receipt-orchestration.md) · [claims audit](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/docs/proof/CLAIMS_AUDIT_2026-09-06.md)
