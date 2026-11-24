# Map/reduce yielding
<!-- id: topic:map_reduce.yielding -->

Part of: [Map/reduce processing model](map_reduce.md)

See also: [Map/reduce governance](map_reduce.governance.md), [Map/Reduce deployment record](map_reduce.deployment.md),
[SuiteCloud Processors](suitecloud_processors.md).

Yielding is how a long-running map or reduce job steps aside so other jobs get a turn on the
processor. It applies only to the map and reduce stages.

## How it works

- After every map or reduce invocation, the job checks whether it has gone past a yield threshold.
- If it has, the job ends cleanly and a new job takes over the remaining work. The new job has the
  same priority but a later timestamp, so it lines up behind work submitted in the meantime.
- Yielding is automatic. There is no API to force a yield, and the check happens only after a limit
  has already been exceeded.
- A yield never interrupts an invocation: the current call always runs to the end first.

## What triggers a yield

- The job has run longer than the deployment's **Yield After Minutes** value. That field accepts 3
  to 60 minutes and defaults to 60.
- The job has used more than 10,000 usage units.

## Yielding is not a safety net for hard limits

Yielding is separate from the hard limits that apply to each single invocation. If one invocation
exceeds its own limit, `SSS_USAGE_LIMIT_EXCEEDED` is raised and that function stops; no yield can
prevent it. See [Map/reduce governance](map_reduce.governance.md) for the per-stage figures.

## Compared with SuiteScript 1.0

SuiteScript 1.0 scheduled scripts could yield manually. Map/reduce scripts cannot; the platform
decides when to yield.
