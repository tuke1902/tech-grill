# Tech Grill

Tech Grill is an open Agent Skill for pressure-testing engineering plans before implementation. It turns a vague or fragile technical proposal into a compact decision brief through short, evidence-based question rounds.

It is inspired by the conversational pressure-testing idea behind Matt Pocock's [`grill-me`](https://github.com/mattpocock/skills/tree/main/skills/productivity/grill-me), but uses an original engineering-specific workflow. Tech Grill ranks questions by consequence, reversibility, and uncertainty instead of trying to exhaust every branch of a general design tree.

## What it does

- Inspects available evidence before asking the user for facts.
- Challenges the one to three highest-risk decisions per round.
- Gives a concrete recommendation and explains the cost of being wrong.
- Scales from a single challenge for a reversible change to multiple rounds for migrations and architectural commitments.
- Ends with a decision brief containing confirmed choices, assumptions, invariants, verification, change strategy, and deferred risks.

Tech Grill does not implement the proposal during the interview unless the user explicitly asks to continue after confirming the brief.

## Install

The portable skill directory is `skills/tech-grill/`, containing `SKILL.md` and its optional supporting resources. Copy or link that directory into the skill directory used by any Agent Skills-compatible client.

Common personal locations include:

```text
Codex:       ~/.codex/skills/tech-grill/
Claude Code: ~/.claude/skills/tech-grill/
```

For a project-only installation, place the same directory under the client's project skill location. The `agents/openai.yaml` file adds optional Codex interface metadata; clients that do not use it can ignore it.

## Use

Invoke the skill explicitly:

```text
$tech-grill pressure-test this plan to move our event pipeline from polling to webhooks.
```

It may also activate when you explicitly ask an agent to grill, challenge, cross-check, de-risk, or stress-test a proposed technical decision. Here, `cross-check` means examining the decision and its tradeoffs. Checking the correctness of code, a schema, a migration file, a configuration, or another existing artifact belongs to a review or testing workflow instead.

Useful session controls:

- `Use the recommendations` accepts every design recommendation in the current round. It does not authorize execution.
- `Defer Q2` removes that question from later rounds while keeping its consequence visible in the brief.
- `Stop` closes the session with the best decision brief possible, without another question.

## Scope

Tech Grill is intentionally independent of languages, frameworks, cloud providers, shells, issue trackers, and multi-agent features. It can be used for:

- architecture and module boundaries;
- feature and API design;
- data models and consistency rules;
- refactors and dependency changes;
- infrastructure and operational changes;
- migrations, rollouts, and rollback plans.

Its trigger excludes ordinary implementation, diagnosis of a known bug, and review of an existing diff unless the user specifically requests an interview about the underlying decisions.

## Security and permissions

Tech Grill is instruction-only: it ships no executable scripts, makes no network calls, and requires no credentials. During a grill, evidence gathering stays read-only. Accepting recommendations or confirming the decision brief does not by itself authorize edits, deployments, data migrations, purchases, messages, or destructive actions.

## Evaluation

[`skills/tech-grill/evals/cases.yaml`](skills/tech-grill/evals/cases.yaml) contains positive, negative, and edge-case prompts with behavior-based assertions. It is a declarative, vendor-neutral corpus rather than a standalone test runner. See [`skills/tech-grill/evals/README.md`](skills/tech-grill/evals/README.md) for the expected discovery and behavior test passes and the minimal harness contract.

When changing the skill, evaluate decisions and observable behavior rather than exact wording:

- Did it investigate available facts first?
- Did it ask no more than three material questions?
- Did every recommendation use the actual evidence?
- Did it avoid implementation before confirmation?
- Did the final brief preserve assumptions and deferred risks?

The readiness labels have fixed meanings: `Decision-ready` has no material condition left; `Ready with conditions` has a selected direction plus named validation or mitigation gates; `Needs decisions` still has a material design choice open.

Add or update an evaluation case whenever a real usage example reveals a repeatable failure mode.

## Contributing

Keep the core skill portable and concise. Changes should address an observed behavior or a clear compatibility need. Avoid dependencies on a single client unless they live in an optional adapter, and accompany meaningful behavior changes with an evaluation case.

## 中文快速开始

Tech Grill 是一个通用工程决策质询 skill。它会先检查已有代码、文档和配置，再按“选错的后果、是否容易回退、当前不确定性”筛选最值得追问的问题。每轮最多提出三个问题，并给出推荐方案和选错代价。结束时，它会生成一份可以直接交给工程师实施的决策摘要。

示例：

```text
$tech-grill 帮我质询一下把单体应用拆成微服务的方案。
```

你可以回复“采用推荐”“延后 Q2”或“停止”。接受设计建议或确认最终摘要都不会自动授权修改文件、部署、迁移数据或执行其他外部操作。

## License

[MIT](LICENSE)
