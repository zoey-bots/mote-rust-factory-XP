# Capability map: full executable-only MOTE XP reimplementation

Status: **approved by the user on 2026-10-18**. The reviewed content is pinned in `CAPABILITY-MAP-APPROVAL.json`. This approval establishes module boundaries/build direction, not a claim that all reference behavior is already known or implemented.

## Authorized goal

- Independent usable Rust and Go implementations of the pinned Windows MOTE executable's behavior.
- Original MOTE and SDK source stays forbidden; original source lookup, copying and wrapping are not substitutes for reconstruction.
- Ghost Factory owns real OpenCode worker dispatch; Jack supplies controlled probes and independent QA.
- Automatically push a milestone to `zoey-bots/mote-cli-factory-XP` only after its tests and independent QA pass. Keep XP/limitations honest until the completion gate passes.
- Target: observed `mote 0.13.1`, executable SHA-256 `fcbaf70916a2ab065366b14cd6b6b7d39afc50a59626a41adc8b157d5a3bc92f`.

## Modules and dependency direction

- **observation-contract** — safe black-box fixtures, exact input/output/protocol capture, reference identity and public/protected partitions. No dependencies.
- **manifest-admission** — discover and reproduce required/default fields, nested model/decision/shell shapes, validation order, capability combinations and numeric bounds. Depends on observation-contract.
- **provider-wire** — observe then implement actual model request/response, auth/config, error and streaming behavior through a loopback provider fixture. Depends on manifest-admission and observation-contract. Provider names in binary strings are hints, not proven protocols.
- **agent-loop** — actual model/action/observation iteration and termination, including any observed decision-routing contract. Depends on provider-wire. Decision variants suggested by strings must be observed; split out an additional module if independently warranted rather than silently omitting them.
- **capability-tools** — real permitted file/directory, shell/git, decision/custom and completion actions, only as established by observations; enforce capability and path/program restrictions. Depends on agent-loop.
- **runtime-budgets** — iteration/per-tool/output/runtime/command limits, repeated-action behavior, timeouts and cancellation. Depends on the relevant agent-loop and tool contracts.
- **event-artifacts** — observed JSONL/event order, error/success status and persisted output semantics. Depends on agent-loop; artifact/file restrictions depend on capability-tools.
- **daily-use-qualification** — clean-checkout builds, fresh protected comparisons, deterministic end-to-end tool/file workflows and a real free-provider-backed coding task, including negative/failure paths. Depends on the complete approved contract matrix above.

Build direction: observation-contract → manifest-admission → provider-wire → agent-loop → capability-tools → runtime-budgets / event-artifacts → daily-use-qualification. Rust and Go implementation nodes may run in parallel against the same frozen observed contract; heavy compilation remains serialized.

## Evidence already produced by Factory

Actual parallel OpenCode assessments were accepted at local commit `86d37971e0b53c33f9d2c4ba39f2a21a3cb053a4`. Both source/evidence boundaries and report writes were independently audited; all 14 public baseline files stayed unchanged. This is assessment acceptance, **not new runtime functionality**.

Both assessments identify the same blocker: existing replicas implement offline CLI/admission only. Validated model configuration and request/action/event contracts are not yet executable-observed. Binary strings suggest possible action names, budgets and decision variants, but do not establish their schemas or behavior.

The first module specification should establish controlled observation and valid-manifest/provider configuration. Implementing a guessed OpenAI/action API first would repeat the earlier self-test/false-parity problem.

## Boundaries for later module specifications

- Only disposable fixture workspaces; negative containment tests may target separately owned disposable sibling fixtures, never user files, original source, secrets or real repositories.
- Loopback mock providers first, no paid endpoints or credentials in coding-model inputs. Explicitly document trusted-local limitations; do not pretend process/environment policy is OS-enforced network/filesystem isolation.
- Preserve existing exact offline comparisons. For genuinely nondeterministic runtime fields, any semantic comparison allowance must be justified and approved in the observed contract; do not normalize deterministic failures away.
- Retain all failed generations and exact effects. No destructive cleanup, force pushes, silent repository overwrite, installs/dependency changes, live-service modification or unrestricted command execution is authorized by this map.
- Keep unknown and unsupported reference behavior visible. No full-parity claim based solely on generated unit tests, mocks, static strings or a single successful coding demo.

## Human review gate

The user approved these module boundaries/build direction. The next module's concrete safety/success contract is separately reviewable before execution. No runtime implementation or unattended continuation has been authorized by the map approval alone.
