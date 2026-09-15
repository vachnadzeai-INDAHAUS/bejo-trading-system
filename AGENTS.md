# Project instructions for Codex

Read `00_START_HERE.md` and `01_ENGINEERING_MANUAL_KA.md` before changing code.
These files and the accompanying contracts/configuration define RC2. Do not import contradictory older “final” manifests.

## Boundaries
- Implement REPLAY/ENGINEERING_PAPER first. Never enable LIVE, request withdrawal access, or spend money without explicit owner authorization.
- Only Chief policy authorizes trading intent. Risk issues an atomic bounded permit; Execution alone talks to a trading API.
- Guardian is read/report-only. It cannot alter weights, cancel trades, or call execution.
- Current features drive decisions. Exact historical similarity is never a mandatory gate. Unfitted models must not invent probabilities or EV.
- Define and validate every event. Use Decimal for money. Preserve event/received/available times and policy/model hashes.
- Treat UNKNOWN write outcomes as unknown: query/reconcile, never blindly resubmit under a new ID.
- Keep one order writer per account. No automatic cross-host failover in V1.
- Keep all fifteen logical responsibilities but use a modular monolith and an offline worker, not fifteen LLM calls or microservices.
- New strategy/risk parameters in this package are engineering proposals; do not misstate them as live approval.

## Work sequence
Follow P0–P7 in chapter22. Implement tests alongside code. Reference tests are examples, not a substitute for exchange/PostgreSQL/replay integration.
Propose policy-changing differences with impact on correctness, uncertainty, latency, cost, and rollback before implementing them.
Preserve user files. Do not reset or overwrite an existing repository without authorization.

## Reporting
Report exact commands, exit codes, test counts, artifacts, commit, and untested paths. Distinguish PASSED/FAILED/NOT_RUN/BLOCKED.
No fabricated backtest, fitted model, performance benchmark, or production-readiness claim.
Use Georgian explanations for the owner, with Georgian definitions of English technical terms.

## RC2 exit-policy binding
- Read chapter 29 and config/exit_policies.json. Baseline mappings in paper.json remain active; 50/50 variants are disabled research candidates, not universal trading rules.
- Bind ExecutionPlan, QuantEvaluation and TradeThesis to the same immutable exit_policy_id and exit_policy_hash; reject reused EV/model results from another exit policy.
- Confirm actual TP1 fills and reconcile the current position quantity before breakeven activation. ACK, chart touch and partial TP1 fill are insufficient.
- Use Decimal and the reference net-floor equations. Never equate entry price with guaranteed net-zero P/L. Never widen an already-confirmed protective stop.
- All closes/reductions require a Chief decision, bounded Risk permit, matching position identity/version and opposite closing side. Reduce-only execution must not create a reversed position.
- Compare baseline, fixed-target 50/50+cost-BE, and trailing 50/50+cost-BE on identical data and costs; retain immutable experiment IDs. No risk increase or live enablement is authorized.
- Apply database/003_exit_policies.sql only after 001 and 002 in an isolated fresh build. Existing RC1 positions need separately reviewed migration; do not silently assign a new exit policy.
- Generate schemas/OpenAPI with scripts/generate_contracts.py. Existing events version 1.0 are not silently accepted as 1.1.
