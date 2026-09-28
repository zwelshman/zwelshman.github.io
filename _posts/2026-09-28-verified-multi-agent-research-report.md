---
title: "How I Built a Verified, Multi-Agent Research Report (And What It Found)"
date: 2026-09-28 10:00:00 +0100
---

I ran an AI-agented research review into the real-world impact of AI coding assistants and agents — then packaged the whole thing as a repeatable cron job. This post is the summary; the full 74 KB report lives [here](/report/).

*Every case in the report is anchored to a primary source fetched in full during the run, with the verbatim supporting quote copied from the fetched page. Search snippets were never used as evidence.*

---

## What it found — the key conclusions

1. **The sign of the effect depends on who you are and what you're doing.** Gains concentrate in *less-experienced workers on well-specified tasks in short-lived settings* (Copilot RCT 55.8% faster, multi-site RCT +26%). The strongest negative result (METR 2025) measured *experienced developers on complex, mature codebases*. Not a contradiction — different regimes.

2. **Perception is systematically unreliable.** METR: people believed −20%, measured +19%. NAV IT: felt faster, metrics flat. The UK DBT evaluation: time savings didn't become productivity. Self-reported savings can't be used as evidence either way.

3. **Verification harnesses are the dividing line between wins and disasters.** Every platform-verifiable win (DARPA AIxCC's 86%/68% plus 18 real 0-days) had ground truth. Every disaster (PocketOS's 9-second double deletion, Replit's database wipe, Deloitte's fabricated report) lacked one.

4. **Throughput gains are front-loaded; quality/complexity costs compound.** +55.4% commits in month one, collapsing to +14.5% by month two, while warnings (+30.3%) and complexity (+41.6%) rose.

5. **Human review is the new bottleneck — and it's measurably failing.** 81.1% of leaked secrets in agent PRs escaped review; Anthropic's own telemetry (93% prompt approval, ~17% overeager actions through). The constraint moved from writing code to understanding it.

6. **Security did not improve as models got better.** Veracode's pass rate is flat across generations; Georgia Tech's AI-attributed CVE count went from ~18 in seven months to 56 in three months.

7. **Institutional outcomes lean negative or unproven.** curl killed a working bounty programme; the best enterprise-agent ROI figures are vendor sales material. The practitioners with the best outcomes all gated on tests and reviewed every line.

## The process — how the output was generated

The report was produced by **Hermes Agent** (`openrouter/auto`) running as a **scheduled cron job**, which orchestrated **four parallel research subagents** (via `delegate_task`), one per domain:

| Pass | Focus |
|------|-------|
| A | Academic / peer-reviewed studies (RCTs, field studies, security, benchmark critiques) |
| B | First-person senior-engineer accounts |
| C | Security + non-software domains (bug bounty, healthcare, legal, support) |
| D | Enterprise adoption wins and failures |

**The agents** — one orchestrator + four research subagents (each with its own isolated context/terminal). Each pass wrote its raw findings to its own file; the orchestrator synthesised them into the single report.

**The method rules baked into the prompt** (non-negotiable, enforced each run):
1. Search broadly for *both* net-positive and net-negative evidence.
2. For every case, fetch the primary source and quote it **verbatim** — search snippets are never evidence; unverifiable claims are dropped and listed in the appendix rather than fabricated.
3. Grade every case's independence: High / Medium / Low (vendor self-reports labelled explicitly).
4. Structure output: NET POSITIVE / NET NEGATIVE / MIXED-CONTESTED + a synthesis + a "claims we could not verify" appendix.
5. Run 4 parallel subagent passes via `delegate_task` across the four domains.
6. Write the review to disk, overwriting the previous edition.

**The cron job**: "AI usage outcomes review", scheduled daily at 09:00, currently paused. Re-running it regenerates a fresh, up-to-date edition.

---

**Disclosure of a key editorial decision:** I treat vendor self-reports as evidence of what vendors *claim*, never of measured impact — every such case is labelled. The report's synthesis deliberately leans on platform-verifiable outcomes (CVEs, leaderboards, public repos) over self-reported ones.

*This review was compiled by an AI agent following the above method; the full source files (report + four passes) are kept locally.*
