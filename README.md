# Nicholas Carter

I build AI infrastructure and agent orchestration systems.

## Flagship: Verdict

[**Verdict**](https://github.com/mrnicholasbcarter-code/verdict-core) is an autonomous development control plane. A goal goes in; a verified, independently reviewed, receipted change comes out.

Key capabilities:

- **Goal-to-DAG orchestration**: frontier planner decomposes work into a dependency graph; each node gets its own model and worktree ([demo run proof](https://github.com/mrnicholasbcarter-code/verdict-core/tree/main/docs/proof/demo-run))
- **Admission control**: live eligibility ladder (`DISCOVERED → ENTITLED → HEALTHY → AVAILABLE → TASK_ELIGIBLE → SELECTED`) keeps cheaper capacity first; every dropped candidate carries a named reason ([source](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/eligibility.py))
- **Same-node reroute**: quota, rate-limit, timeout failures cool down the route or provider; the same node reassigns to another admitted model until pool exhaustion ([recovery intelligence](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/recovery.py))
- **Independent review**: reviewer excluded from all implementers; route-level or family-level independence enforced ([review policy](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/review.py))
- **Tamper-evident receipts**: SHA-256 digest of the event log; any later edit is detected on verification ([receipt module](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/receipt.py))

**Credential-free demo** (no API key, no gateway, no network):

```bash
pip install verdict-core
verdict quickstart --non-interactive --dry-run
```

The demo makes one deterministic routing decision and names every excluded candidate. Watch the [terminal recording](https://github.com/mrnicholasbcarter-code/verdict-core#demo-goal-to-receipt) or inspect the [committed run proof](https://github.com/mrnicholasbcarter-code/verdict-core/tree/main/docs/proof/demo-run).

Read more: [main README](https://github.com/mrnicholasbcarter-code/verdict-core#verdict), [orchestration architecture](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/docs/adr/ADR-036-goal-to-receipt-orchestration.md), [claims audit](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/docs/proof/CLAIMS_AUDIT_2026-09-06.md).

## Other projects

| Repo | Purpose |
|---|---|
| [verdict-node](https://github.com/mrnicholasbcarter-code/verdict-node) | TypeScript envelope enforcement and task classification for Node.js; published as [@bodanglin/verdict-node 0.2.0](https://www.npmjs.com/package/@bodanglin/verdict-node) |
| [verdict-cockpit](https://github.com/mrnicholasbcarter-code/verdict-cockpit) | Source-available fixture-mode viewer (not published; active development) |

## Engineering focus

Python, TypeScript, FastAPI, async/event-driven systems, policy enforcement, admission control, deterministic recovery, reproducible testing, and evidence-backed technical communication.
