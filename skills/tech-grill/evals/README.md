# Running the Tech Grill evaluations

`cases.yaml` is a declarative evaluation corpus. It does not call a model or depend on a particular vendor. Run it with any harness that can:

1. expose a skill's name and description for automatic discovery;
2. optionally force-load the skill for behavior testing;
3. replay a single prompt or a multi-turn conversation;
4. capture whether the skill activated and the next assistant response;
5. grade semantic assertions against the response.

## Two test passes

Run discovery and behavior separately so a strong response cannot hide a bad trigger.

### Discovery pass

Expose Tech Grill alongside the client's normal skill catalog without forcing it to load. Run every case and compare observed activation with `trigger_expected`.

This pass is especially important for the adjacent negative cases. Words such as `challenge`, `cross-check`, and `migration` should activate Tech Grill only when they describe an interview about proposed engineering choices, not testing or correctness review of an existing artifact.

### Behavior pass

Force-load or explicitly invoke Tech Grill for cases where `trigger_expected` is `true`:

- For a case with `prompt`, send that text as the user request.
- For a case with `turns`, replay the messages in order and capture the response after the final user message.
- Apply `global_invariants` plus the case's `expected` assertions.
- Fail the case when any `avoid` behavior appears.

Grade meaning rather than exact wording or Markdown decoration. A human reviewer or semantic model grader can perform this step. Repeat representative cases when measuring trigger reliability or model variance.

## Suggested result record

Store at least:

```yaml
case_id: vague-greenfield-api
client: client-and-version
model: model-and-version
discovery: pass
behavior: pass
notes: concise evidence for any failure
```

Record the client and model because discovery and instruction-following behavior can vary independently of the skill files.
