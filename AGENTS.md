# gpu-cluster-bringup — agent guardrails

- `from __future__ import annotations` on line 1 of every module. Module-level constants (`ENCODING`, `MAX_ATTEMPTS`, never `MAX_RETRIES`). JSON/text via `Path.open("r", encoding=...)`. Bare `raise`. No pandas/numpy/statistics.
- Gates read only `bringup.model` records, never raw tool output, so they stay vendor-neutral.
- Missing or unparseable evidence fails closed (`evidence_complete`). Never default a missing value to "pass".
- A new defect gets an entry in `fleetgen.DEFECTS` (stage, check) and is covered by the parametrized e2e test. Never weaken that test to get green.
- Thresholds live in `manifests/*.json`, with the reasoning in `docs/THRESHOLDS.md`. Change both together.
- Provisioning pins (`provision/onprem/group_vars/all.yml`) and `manifests/golden.json` change in the same PR.
- Never claim real-hardware validation. The fixtures are hand-built in real formats; they were not captured from hardware.

Done means: `python -m pytest` green, plus `python -m bringup run` on the `defects` fleet exiting 3 with the cordon list equal to the injected set.
