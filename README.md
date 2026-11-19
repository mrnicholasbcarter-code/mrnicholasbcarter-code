# Nick Carter

I'm a software engineer with 20+ years of web, mobile and backend work. At AgileThought, I led a Deloitte Symphony team and built Bankers Surety's platform end to end. At Mad Mobile, I owned VF Corp's MPOS and Sysco's inventory apps. Since 2023 I've worked independently on AI infrastructure.

Right now I build tooling that lets coding agents do real work without losing track of what they did: which model ran, why it was allowed to, who reviewed it, and a record you can check afterwards.

I'm available for contract work. I help teams:

- run coding agents under review: a plan, parallel workers, an independent reviewer and an audit log
- route LLM calls across providers, with fallback when one hits a rate limit, quota or timeout
- add guardrails and audit trails to Python and Node AI APIs

**Hire me:** [Upwork](https://www.upwork.com/freelancers/~01aa0036f6c4dd8a21) · [nick@nickcarter.dev](mailto:nick@nickcarter.dev) · [nickcarter.dev](https://nickcarter.dev)

## Verdict

[Verdict](https://github.com/mrnicholasbcarter-code/verdict-core) is a control plane for coding agents. It decides which model may run each task, and keeps a record of the run that you can verify afterwards.

Agents fail in boring ways: a provider rate-limits, a quota runs out, a reviewer quietly passes its own work. Verdict treats each of those as a named, bounded outcome instead of a hang or a guess.

<a href="https://github.com/mrnicholasbcarter-code/verdict-core"><img src="https://raw.githubusercontent.com/mrnicholasbcarter-code/verdict-core/main/docs/assets/live-cockpit.svg" alt="Replay of one live verdict orchestrate run on free models: a real HTTP 429 rate limit and a failed test, both nodes rerouted to another provider, an independent review PASS, and a receipt that verifies" width="860"></a>

What it does, and where to check it:

- **Admits models only on live evidence.** A model must pass account, health and quota checks before it is ranked; each rejected candidate gets a named reason. [eligibility.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/eligibility.py), [tests/test_admission.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/tests/test_admission.py)
- **Runs a goal as a plan of parallel nodes**, each in its own git worktree. [runtime.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/runtime.py), [ADR-036](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/docs/adr/ADR-036-goal-to-receipt-orchestration.md)
- **Recovers within a bounded number of attempts.** On a rate limit, quota or timeout the node moves to another admitted provider; when attempts or the pool run out, it stops with `FAIL_CLOSED`. [recovery.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/recovery.py), [tests/test_orch_recovery.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/tests/test_orch_recovery.py)
- **Sends the result to an independent reviewer** on a different route, preferably a different model family. A skipped or empty review is an error, not a pass. [review.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/review.py), [tests/test_orch_review.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/tests/test_orch_review.py)
- **Writes a receipt bound to the SHA-256 of the event log**; `verdict run-receipt` recomputes it and flags any edit. [receipt.py](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/verdict/orchestration/receipt.py), and a [live run record](https://github.com/mrnicholasbcarter-code/verdict-core/tree/main/docs/proof/live-run-20261119T000727Z) on free models: review PASS, and the run's receipt check is quoted (the raw event log is kept private because it contains host paths).

8,800+ [tests](https://github.com/mrnicholasbcarter-code/verdict-core/tree/main/tests), CI on Python 3.10 to 3.13, MIT licence.

Try it without API keys:

```bash
pipx install verdict-core
verdict demo
```

This installs 0.4.2, the latest release on PyPI, and runs an offline demo with scripted workers ([what it shows](https://github.com/mrnicholasbcarter-code/verdict-core/blob/main/docs/guides/offline-demo.md)). Version 1.0.0 is on the main branch and not released yet.

## Projects

- [verdict-core](https://github.com/mrnicholasbcarter-code/verdict-core): the Python CLI and MCP server described above. Release on [PyPI](https://pypi.org/project/verdict-core/).
- [verdict-node](https://github.com/mrnicholasbcarter-code/verdict-node): Express and Next.js middleware that blocks unapproved LLM calls and forwards approved ones to an OpenAI-compatible API. Alpha, 0.2.0 on [npm](https://www.npmjs.com/package/@bodanglin/verdict-node).
- [verdict-cockpit](https://github.com/mrnicholasbcarter-code/verdict-cockpit): a read-only Next.js viewer for agent runs: which model ran, what was rejected and why. It shows fixture data only for now.

Docs: [nickcarter.dev/docs/verdict](https://nickcarter.dev/docs/verdict)

**Stack:** Python, TypeScript, FastAPI, Express/Next.js, OpenAI/Anthropic, MCP, OpenTelemetry
