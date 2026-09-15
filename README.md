# Tech Grill

[中文](#中文) · [English](#english)

> **中文：** 一个开源、通用的工程决策质询 skill，帮助你在实施前压力测试技术方案。
>
> **English:** An open, general-purpose Agent Skill for pressure-testing engineering decisions before implementation.

---

## 中文

Tech Grill 是一个通用的工程决策质询 skill。它会先检查已有代码、文档和配置，再通过短轮次、基于证据的问题，把模糊或脆弱的技术方案整理成一份紧凑的决策摘要。

它受 Matt Pocock 的 [`grill-me`](https://github.com/mattpocock/skills/tree/main/skills/productivity/grill-me) 启发，但采用了原创的工程专用流程：按照选错的后果、可回退性和当前不确定性来排序问题，而不是穷举通用设计树中的每个分支。

### 功能

- 在向用户提问前，先检查可用的代码、文档和配置证据。
- 每轮挑战一到三个后果最高的决策。
- 给出具体建议，并解释选错的代价。
- 可用于可回退的小改动，也可用于迁移和架构承诺等多轮讨论。
- 最终生成包含已确认选择、假设、不变量、验证方式、变更策略和延期风险的决策摘要。

Tech Grill 不会在质询过程中直接实施方案；只有用户确认摘要并明确要求继续时，才进入后续执行工作。

### 安装

仓库中的可移植 skill 目录是 `skills/tech-grill/`，其中包含 `SKILL.md` 以及可选的支持资源。将这个目录复制或链接到任何兼容 Agent Skills 的客户端即可。

常见的个人安装位置：

```text
Codex:       ~/.codex/skills/tech-grill/
Claude Code: ~/.claude/skills/tech-grill/
```

也可以使用 `skills` CLI 安装：

```bash
npx skills add tuke1902/tech-grill --skill tech-grill -g -a codex -y
```

如果使用 fork，请将 `tuke1902` 替换为你的 GitHub 用户名。`agents/openai.yaml` 提供可选的 Codex 界面元数据；不使用该元数据的客户端可以忽略它。

### 使用

显式调用：

```text
$tech-grill 帮我压力测试一下把事件管道从轮询迁移到 Webhook 的方案。
```

当你明确要求 agent 质询、挑战、交叉检查、降低风险或压力测试一个技术决策时，也可以自动触发。这里的“交叉检查”指检查决策及其权衡；检查代码、数据库 schema、迁移文件、配置或其他现有工件的正确性，则属于审查或测试流程，不属于本 skill 的默认触发范围。

会话控制：

- `采用推荐`：接受当前轮次的所有设计建议，但不授权执行。
- `延后 Q2`：将问题从后续轮次中移除，同时保留它在摘要中的影响。
- `停止`：不再提问，直接用当前信息生成最佳决策摘要。

### 范围与边界

Tech Grill 与具体语言、框架、云厂商、shell、issue tracker 和多 agent 功能无关，可用于：

- 架构与模块边界；
- 功能与 API 设计；
- 数据模型与一致性规则；
- 重构与依赖变更；
- 基础设施与运维变更；
- 迁移、发布和回滚计划。

它不会默认触发普通实现、已知 bug 的诊断，或对现有 diff 的正确性审查；除非用户明确要求围绕其中的决策进行质询。

### 安全与权限

Tech Grill 只有指令文件，不包含可执行脚本、不发起网络请求，也不需要凭据。质询过程中的证据收集保持只读。接受建议或确认最终摘要，不会自动授权修改文件、部署、数据迁移、购买、发消息或其他破坏性操作。

### 评估

[`skills/tech-grill/evals/cases.yaml`](skills/tech-grill/evals/cases.yaml) 包含正向、负向和边界用例，以及基于行为的断言。它是声明式、与厂商无关的用例集，不是独立的测试运行器。预期的发现流程、行为测试和最小 harness 契约见 [`skills/tech-grill/evals/README.md`](skills/tech-grill/evals/README.md)。

修改 skill 时，应评估决策质量和可观察行为，而不是匹配固定措辞：

- 是否先调查了可用事实？
- 每轮是否不超过三个重要问题？
- 每条建议是否使用了实际证据？
- 是否在确认前避免了实施？
- 最终摘要是否保留了假设和延期风险？

准备度标签含义固定：`Decision-ready` 表示没有剩余的重要条件；`Ready with conditions` 表示方向已选定，但仍有明确的验证或缓解门槛；`Needs decisions` 表示仍有重要设计选择未决。

当真实使用暴露出可重复的失败模式时，请新增或更新评估用例。

### 贡献

保持核心 skill 可移植且简洁。提交应解决已观察到的行为问题或明确的兼容性需求。除非放在可选适配器中，否则避免依赖单一客户端；有意义的行为变化应同时补充评估用例。

### 许可证

[MIT](LICENSE)

---

## English

Tech Grill is an open Agent Skill for pressure-testing engineering plans before implementation. It turns a vague or fragile technical proposal into a compact decision brief through short, evidence-based question rounds.

It is inspired by the conversational pressure-testing idea behind Matt Pocock's [`grill-me`](https://github.com/mattpocock/skills/tree/main/skills/productivity/grill-me), but uses an original engineering-specific workflow. Tech Grill ranks questions by consequence, reversibility, and uncertainty instead of trying to exhaust every branch of a general design tree.

### What it does

- Inspects available evidence before asking the user for facts.
- Challenges the one to three highest-risk decisions per round.
- Gives a concrete recommendation and explains the cost of being wrong.
- Scales from a single challenge for a reversible change to multiple rounds for migrations and architectural commitments.
- Ends with a decision brief containing confirmed choices, assumptions, invariants, verification, change strategy, and deferred risks.

Tech Grill does not implement the proposal during the interview unless the user explicitly asks to continue after confirming the brief.

### Install

The portable skill directory is `skills/tech-grill/`, containing `SKILL.md` and its optional supporting resources. Copy or link that directory into the skill directory used by any Agent Skills-compatible client.

Common personal locations include:

```text
Codex:       ~/.codex/skills/tech-grill/
Claude Code: ~/.claude/skills/tech-grill/
```

For a project-only installation, place the same directory under the client's project skill location. The `agents/openai.yaml` file adds optional Codex interface metadata; clients that do not use it can ignore it.

You can also install it with the `skills` CLI:

```bash
npx skills add tuke1902/tech-grill --skill tech-grill -g -a codex -y
```

Replace `tuke1902` with your GitHub username when installing a fork.

### Use

Invoke the skill explicitly:

```text
$tech-grill pressure-test this plan to move our event pipeline from polling to webhooks.
```

It may also activate when you explicitly ask an agent to grill, challenge, cross-check, de-risk, or stress-test a proposed technical decision. Here, `cross-check` means examining the decision and its tradeoffs. Checking the correctness of code, a schema, a migration file, a configuration, or another existing artifact belongs to a review or testing workflow instead.

Useful session controls:

- `Use the recommendations` accepts every design recommendation in the current round. It does not authorize execution.
- `Defer Q2` removes that question from later rounds while keeping its consequence visible in the brief.
- `Stop` closes the session with the best decision brief possible, without another question.

### Scope

Tech Grill is intentionally independent of languages, frameworks, cloud providers, shells, issue trackers, and multi-agent features. It can be used for:

- architecture and module boundaries;
- feature and API design;
- data models and consistency rules;
- refactors and dependency changes;
- infrastructure and operational changes;
- migrations, rollouts, and rollback plans.

Its trigger excludes ordinary implementation, diagnosis of a known bug, and review of an existing diff unless the user specifically requests an interview about the underlying decisions.

### Security and permissions

Tech Grill is instruction-only: it ships no executable scripts, makes no network calls, and requires no credentials. During a grill, evidence gathering stays read-only. Accepting recommendations or confirming the decision brief does not by itself authorize edits, deployments, data migrations, purchases, messages, or destructive actions.

### Evaluation

[`skills/tech-grill/evals/cases.yaml`](skills/tech-grill/evals/cases.yaml) contains positive, negative, and edge-case prompts with behavior-based assertions. It is a declarative, vendor-neutral corpus rather than a standalone test runner. See [`skills/tech-grill/evals/README.md`](skills/tech-grill/evals/README.md) for the expected discovery and behavior test passes and the minimal harness contract.

When changing the skill, evaluate decisions and observable behavior rather than exact wording:

- Did it investigate available facts first?
- Did it ask no more than three material questions?
- Did every recommendation use the actual evidence?
- Did it avoid implementation before confirmation?
- Did the final brief preserve assumptions and deferred risks?

The readiness labels have fixed meanings: `Decision-ready` has no material condition left; `Ready with conditions` has a selected direction plus named validation or mitigation gates; `Needs decisions` still has a material design choice open.

Add or update an evaluation case whenever a real usage example reveals a repeatable failure mode.

### Contributing

Keep the core skill portable and concise. Changes should address an observed behavior or a clear compatibility need. Avoid dependencies on a single client unless they live in an optional adapter, and accompany meaningful behavior changes with an evaluation case.

### License

[MIT](LICENSE)
