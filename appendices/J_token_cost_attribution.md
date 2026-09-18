# Appendix J: Token & Cost Attribution — Measuring Budgeted Autonomy in Practice

Chapter 12 and Appendix G establish *why* resource budgeting matters: in a capacity-constrained, cost-surging environment, ungoverned agent workloads are doubly expensive. This appendix addresses the operational question those sections leave open — *how do you actually measure per-agent token and cost consumption, and what can you not measure?*

The distinction matters because a budget you cannot measure is governance theater. Prescribing "every agent operates within a defined token budget" (Chapter 3) is only enforceable if budget-versus-actual is instrumented. This appendix provides the measurement layer, and — equally important — an honest accounting of its blind spots.

## The Measurement Mechanism: Instrument the Inference Boundary

Most agent stacks already emit the data required for token attribution; it is simply not being captured. Every inference call — whether to a hosted API or a locally hosted model server — returns per-request token counts (prompt/input tokens and generated/output tokens). Local inference servers additionally write these counts to their request logs.

The minimal instrumentation is:

1. **Capture at the call site.** Wrap each inference call so it records `{agent_id, task_id, model_id, prompt_tokens, output_tokens, timestamp}` into a cost-governance ledger. This is a few lines at each call site, not a new subsystem.
2. **Harvest existing logs for backfill.** Where instrumentation was not in place, local inference-server logs already contain per-request token counts and timestamps. Parsing them recovers a historical time series without re-running anything — latent telemetry that most teams never look at.
3. **Aggregate into budget-versus-actual.** Roll the ledger up per agent, per task, and per time window. This turns the *prescribed* budget from Chapter 3 into an *enforced* one: the runtime monitor can now compare actual consumption to the allocation and flag or halt on breach.

This is the concrete form of the **Budgeted Autonomy** theme (Chapter 2): boundaries become structural because they are measured, not merely asserted.

## What You Can Measure — and What You Cannot

Honest attribution requires naming the boundary of observability. There are two distinct consumption layers, and only one of them is locally visible.

| Layer | What it is | Observable? |
|---|---|---|
| **Task inference** | Tokens consumed by the agent's own model calls to do the work | **Yes** — per-request counts at the inference boundary |
| **Orchestration context** | Tokens consumed by the controlling/reasoning agent's own context window as it plans, reads, and delegates | **Often not** — hosted orchestration layers rarely expose their own context accounting to the workload |

This is a governance-hollowness trap (the **G** term in the Sovereignty Equation). An organization can build a precise meter on task inference, watch it read low, and conclude its agent estate is cheap — while the orchestration layer, invisible to that meter, is the larger and faster-growing cost. **A token meter that measures only the layer it can see will systematically under-report true consumption.**

The governance implication is not "give up." It is:

- **Measure what you can, precisely** (task inference), and
- **Name what you cannot, explicitly** (orchestration context), so the gap is a *known* unmeasured quantity rather than a silent one. A documented blind spot is governable; an undocumented one is where cost — and hollowness — accumulate.

Where the orchestration layer is a hosted product, the only available levers are behavioral rather than instrumental: shorter working contexts, fewer redundant retrievals, and periodic context compaction. Treat these as governance controls in their own right, even though they cannot be metered from inside.

## The Efficiency Paradox: Why Per-Call Metering Understates Load

There is a second, non-obvious trap. Per-call efficiency and aggregate consumption move in opposite directions as agentic workloads mature.

Individual model calls get cheaper — better models, quantization, prompt optimization. By that logic, total token consumption should fall. It does the opposite, for the same reason efficient engines did not reduce total fuel use: efficiency gains are swamped by growth in *how many calls* a single task now chains. An agentic workload that once made one call now makes dozens — planning, tool selection, sub-task delegation, verification — each individually lean.

This was observed directly in our own instrumentation: within a single multi-step agent session, mean prompt size per call more than doubled over the course of the session as the workload accumulated context and chained deeper, even though each call remained individually modest. The per-call meter looked healthy; the session total did not.

The governance consequence is concrete: **budget at the task and session level, not the call level.** A per-call token meter will report efficiency while aggregate spend climbs. Behavioral baselining (Chapter 2) must therefore track *calls-per-task* and *session-level totals* as first-class signals, not just per-call cost.

## Evidence Status

In the interest of the framework's own honesty standard, the claims in this appendix are tagged by evidence tier:

- **Established:** inference boundaries expose per-request token counts; local inference-server logs contain them. This is a property of current tooling, directly verifiable.
- **Measured (single-environment):** the within-session growth of aggregate consumption despite lean per-call sizes was observed in one instrumented agent environment. The *direction* is consistent with the efficiency-paradox mechanism; the *magnitude* should not be generalized from a single environment.
- **Modeled:** the claim that orchestration-context cost dominates task-inference cost in hosted stacks is an inference from the observability asymmetry, not a measured ratio. It should be treated as a hypothesis to test per deployment, not a settled figure.

## Key Takeaway

Budgeted autonomy is only real when budget-versus-actual is instrumented. Instrument the inference boundary; harvest existing logs for history. Then be explicit about the layer you cannot see — orchestration context — because an unmeasured cost layer is exactly where governance hollowness hides. And budget at the task and session level, because per-call efficiency will otherwise tell a reassuring story while aggregate consumption climbs.
