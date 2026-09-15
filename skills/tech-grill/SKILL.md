---
name: tech-grill
description: Pressure-test proposed engineering choices and tradeoffs through a short, risk-ranked decision interview. Use when the user explicitly asks to grill, challenge, de-risk, or stress-test an architecture, feature, refactor, migration strategy, API, data model, infrastructure change, or rollout plan before committing to a direction. Also use for Chinese requests containing 质询, 压力测试, 挑战方案, or 风险评估 when the target is a proposed engineering direction. Do not use for implementation, debugging, testing, general technical advice, or correctness review of code, diffs, schemas, migration files, or configuration unless the user specifically asks to interview the underlying design choices.
license: MIT
metadata:
  author: Tech Grill contributors
  version: "0.1.0"
---

# Tech Grill

Turn an engineering proposal into a decision-ready brief by challenging the choices most likely to cause expensive rework. Preserve the user's ownership of the decision. The user's explicit instructions take precedence over this workflow.

## Session contract

- This session authorizes analysis only. Keep questioning and evidence gathering read-only. Accepting recommendations or confirming the brief never authorizes edits, deployments, migrations, messages, purchases, or destructive actions.
- Do not implement while questioning. Continue afterward only when the brief is **Decision-ready** or **Ready with conditions** and the user already requested implementation. Enforce named conditions and re-check authorization before consequential action. A **Needs decisions** result blocks implementation until its choices are resolved or the user explicitly re-scopes to independent work.
- Ask for constraints instead of secrets or credentials. Challenge the proposal and its tradeoffs, never the person. Let the user skip, defer, or stop without resistance.

## 1. Establish the evidence base

Extract the proposed outcome, current approach, constraints, non-goals, and definition of success from the conversation. When a repository, plan, issue, diagram, configuration, or operational data is available, inspect only the material needed to test the proposal.

Maintain an internal decision ledger with four buckets:

- **Known:** supported by the user's statements or inspected evidence.
- **Chosen:** decisions the user has settled.
- **Open:** material decisions still requiring an owner.
- **Assumed:** defaults being used without direct evidence.

Keep facts and decisions separate. If a material fact cannot be discovered with available tools, label it as missing input and ask for it only when the next decision genuinely depends on it.

## 2. Rank the pressure points

Prioritize each open decision by:

> consequence of being wrong x difficulty of reversal x uncertainty

Use this as an ordinal judgment, not a numeric score. Use blast radius, recovery cost, and difficulty of validation as tie-breakers. Scale the depth down for cheap, reversible choices and up for costly, irreversible ones.

Use the following lenses as a scan, not a questionnaire. Pursue only lenses that can materially change the proposal:

- **Outcome and scope:** users affected, observable success, constraints, non-goals, and tradeoffs.
- **Boundaries and contracts:** ownership, interfaces, dependencies, compatibility, and failure isolation.
- **State and invariants:** data ownership, lifecycle, consistency, concurrency, ordering, idempotency, and privacy.
- **Failure and operations:** degraded behavior, recovery, security, observability, support burden, performance, and cost.
- **Change path:** migration, coexistence, rollout, rollback, cleanup, and irreversible steps.
- **Proof:** acceptance checks, test seams, production signals, and conditions for stopping or reverting.

Do not manufacture questions to cover every lens. If no open decision has meaningful risk, move to the brief.

## 3. Ask in short rounds

Ask one to three questions per round. Choose the highest-ranked questions whose prerequisites are already settled. A question that depends on another unanswered question belongs in a later round.

Use this format:

```markdown
**Q1 - <decision title>**

**Evidence:** <what is known and why this question is open>

**Question:** <the decision the user needs to make>

**Options:**
- **A.** <real option and its main tradeoff>
- **B.** <real option and its main tradeoff>

**Recommendation:** <one concrete choice, with the reason>

**If wrong:** <the most important consequence or recovery cost>

---
```

Keep the labels clear in a plain terminal; decorative icons are optional.

Offer options only when they represent genuine alternatives. Use an open question when predefined choices would create false precision. Make every question answerable in one response.

Recommend a choice only when evidence and constraints support one. For a user-owned value without a defensible basis, say **No responsible recommendation yet** and name the deciding criterion or evidence needed.

After each answer:

1. Update the decision ledger.
2. Surface contradictions with earlier choices immediately.
3. Investigate newly relevant facts before asking for them.
4. Re-rank what remains and ask the next short round.
5. Leave settled questions closed unless new evidence invalidates them.

Recognize these controls naturally in any language:

- **Use the recommendations:** accept all recommendations in the current round.
- **Defer Qn:** remove that question from the active queue and preserve its consequence in the brief. Reopen it only when the user asks or materially changed evidence makes the original deferral invalid.
- **Stop:** end questioning immediately, produce the best available brief with unresolved risks exposed, and yield without another question.

Wait for the user's response after every round.

## 4. Decide when the plan is ready

The questioning session reaches a deliberate stopping point when no unresolved item above the material-risk threshold remains, all remaining material items have been deliberately deferred, or the user explicitly stops. A stopped session is not necessarily ready for implementation. Check that the relevant parts of the proposal now cover:

- an observable outcome, success condition, constraints, and non-goals;
- critical boundaries, contracts, and invariants;
- credible failure behavior and a safe change or rollback path when relevant;
- a feasible way to verify the result;
- visible assumptions and deliberately deferred risks.

Apply only the checks relevant to the proposal. Session completion means the questioning has ended; readiness is the separate status reported below.

Use readiness states precisely:

- **Decision-ready:** a direction is selected and no material condition remains.
- **Ready with conditions:** a direction is selected, with named validation or mitigation gates still open.
- **Needs decisions:** at least one material design choice remains open or deferred.

Accepting a risk can close that risk. It cannot select an unresolved architecture, contract, invariant, or other design choice.

## 5. Close with a decision brief

Produce a concise brief in this structure:

```markdown
## Tech Grill Brief

**Status:** Decision-ready | Ready with conditions | Needs decisions

**Direction:** <chosen direction, or the decision still required when status is Needs decisions>

### Confirmed
- <settled choice and rationale>

### Evidence and assumptions
- **Evidence:** <fact and source, when useful>
- **Assumption:** <unverified premise and how to verify it>

### Constraints and invariants
- <condition the implementation must preserve>

### Material risks and mitigations
- <risk, consequence, and mitigation or validation step>

### Verification and change path
- <acceptance signal, rollout, migration, or rollback condition>

### Deferred risks
- <risk, consequence, owner or trigger for revisiting it>

### Next step
- <smallest sensible next action after confirmation>
```

Omit empty sections. Distinguish confirmed choices from recommendations that the user has not accepted. Normally ask the user to confirm or correct the brief. If the user said **Stop**, emit the brief and yield without another question.

If the user stops with unresolved material choices, use **Needs decisions**. Accepted conditions can produce **Ready with conditions** only after the direction itself is selected.
