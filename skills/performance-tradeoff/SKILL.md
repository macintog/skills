---
name: performance-tradeoff
description: Judge performance or resource claims on real hardware, and design the test, replay, evaluation, benchmark, soak or watch before it runs: latency, throughput, memory or headroom numbers, and accept-or-reject decisions on optimizations. Not for refactors with no resource claim.
---

# Performance tradeoff

Before a test, replay, evaluation, benchmark, soak or watch runs, write down in one place: the question it settles and the decision that changes; what in the project's current plan or next host makes the number moot; the smallest input that answers it, located from indexes, logs or labels rather than scanned for; the mode (offline, seeking and batched, unless the property is wall-clock); the hardware shape (batched, pipelined, the fastest engine the target has); the estimate as input ÷ measured throughput × runs, from a smoke run; and the stop rule. A run with no written plan is the failure this skill exists to prevent. Thresholds: over about 15 device-minutes, or a 1× replay longer than the event it tests, get the coordinator's or operator's OK first; a serial batch-1 loop on an idle accelerator is a defect to fix before scaling, not a budget.

Before trusting or reporting a number, check:

- **Equivalent arms.** Only the intended change differs: build, inputs, load, power/thermal state, output quality. Alternate arm order; a cache warmed by one arm contaminates the next.
- **Separate regimes.** Cold, warm and sustained are different results. Claim a cold-start win only from fresh-cache runs of both arms.
- **Noise before change.** Compare the delta with each arm's run-to-run spread; overlapping ranges or few samples mean inconclusive. Give latency percentiles with sample counts.
- **Completion, not enqueue.** Time async/GPU work to completion, with identical sync and transfer boundaries in both arms.
- **Raw-value arithmetic.** Relative change = (candidate − baseline) / baseline, direction stated; recheck supplied percentages. No speedup against a timeout or OOM.
- **One memory scope.** Don't sum overlapping counters (a unified-memory footprint may already include GPU allocations). Headroom is peak versus what the target actually has free; note the sampling interval.
- **Phase is not end to end.** A faster component isn't a faster request until measured; overlapping work doesn't add serially.

Decide in order: correctness and quality; whether the change enables a supported workload the baseline fails or nearly fails; absolute user-visible cost; maintenance cost. A resource reduction that enables nothing measured doesn't justify a latency, quality or complexity cost. Set thresholds and margins before seeing candidate results.

Answer accept, reject or unproven per hardware and workload case, naming the smallest missing measurement. Report only the metrics that decide it.
