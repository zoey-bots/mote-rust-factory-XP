# Spec: observation-contract

Status: **draft — review required before implementation or reference execution**.
Parent: the user-approved `CAPABILITY-MAP.md`, module id `observation-contract`.

## Objective

Give Ghost Factory a qualified native-Windows-executable observation contract that can supply trustworthy black-box evidence to both Rust and Go implementations. The first increment qualifies capture and guard behavior using the existing public offline cases; it does not implement an agent runtime, guess a provider API, or claim complete manifest knowledge.

The existing REE runtime handoff is a narrow Node/main.cjs profile. It is not already a qualified observer for MOTE's native PE executable. Reuse applicable tested supervision/receipt components only after checking their actual interface; never disguise the PE target as a Node program or loosen that profile's checks.

## Assumptions and scope

- Continue against the pinned Windows executable observed as `mote 0.13.1`, SHA-256 `fcbaf70916a2ab065366b14cd6b6b7d39afc50a59626a41adc8b157d5a3bc92f`.
- Original MOTE and SDK source remains forbidden. Only a copied executable and selected byte-bound observations are inputs.
- First qualification uses the existing **29 public cases**, with their public fixtures, repeated twice. These are previously observed help/version/argument and missing/malformed-manifest paths, not accepted runtime tasks.
- No valid-manifest agent execution, new unknown schema probes, provider transport, shell/git/custom actions, decision routing or live runtime model calls in this increment. Those need their own observed and reviewed module contracts.
- Native-free OpenCode may be used by Factory to implement the observation helper; that coding-provider use is separate from, and does not authorize, model calls by the reference executable.
- Existing Python standard-library QA tooling is reused/extended. The product implementations remain Rust and Go. No new dependency or installation is required or authorized here.

## Execution ownership

Ghost Factory owns the implementation worker, isolated worktree, checks, acceptance and artifact commit. OpenCode implements the helper/tests under that runtime. Jack supplies the pinned request/approval and performs independent source/evidence review; Jack does not implement replica behavior or substitute a directly launched coding session.

Worker inputs contain public examples only. Protected cases, approvals, original locations and evaluators stay outside worker ownership. A trusted operator/evaluator invokes the qualified helper; coding workers do not receive arbitrary reference-execution privileges or personal/provider credentials.

## Required interfaces

The following are **proposed interfaces to implement after approval**, not commands or files claimed to exist now. Run from the replica repository root:

```text
python -m unittest discover -s tests/observation_contract -p "test_*.py" -v
python scripts/observe_mote.py --request control/observation-request.json --approval control/observation-approval.json --approval-sha256 <APPROVAL_SHA256> --output <NEW_OBSERVATION_DIRECTORY>
python scripts/verify_mote_observations.py --request control/observation-request.json --approval control/observation-approval.json --approval-sha256 <APPROVAL_SHA256> --observations <OBSERVATION_DIRECTORY>
```

No broad shell command string is accepted. The production invocation takes a typed argv array and the exact approved native executable, not a caller-selected interpreter or shell. Unit fixtures may exercise a lower-level supervisor with harmless fake children, but must not create a production pin/approval bypass.

## Project structure and style

Expected owned paths are limited to:
- `scripts/observe_mote.py` — fail-closed validation, bounded native capture and terminal receipts.
- `scripts/verify_mote_observations.py` — independent reread/digest/repeat/byte comparison checks; no new reference execution on a verify operation.
- `tests/observation_contract/test_observe_mote.py` — positive capture/repeat/byte preservation tests.
- `tests/observation_contract/test_observation_guards.py` — rejection, timeout/quota and cleanup tests.
- `docs/observation-contract.md` — actual supported commands, schema and limitations.

Use the existing Python standard-library test style, explicit small functions, bytes for process I/O and ordinary `Path` values. Example record shape, not fabricated execution evidence:

```json
{"case_id":"regression-02","repeat":1,"state":"completed","exit_code":0,"stdout_b64":"...","stderr_b64":"...","comparison":"pending"}
```

Unknown states remain unknown. A process exit, capture completion and behavioral match are separate fields, never a single success flag.

## Request and approval boundary

The request binds an explicit schema version, native executable path/hash, public fixture-document hash, unique selected case IDs, exact argv/fixture bytes, repeat count, output parent and capture limits. The approval binds the complete request bytes and explicitly selected case set. Its own bytes are pinned at invocation. Reject duplicates, unknown fields, malformed identifiers/digests and boolean/numeric substitutions instead of repairing them.

Both target and output paths must resolve beneath declared operator-owned roots; neither root nor an original/user repository is a valid output directory. Reject links/junction escapes and pre-existing output generations. A new generation gets a nonce and an ownership marker before any child is spawned. Failed generations stay intact.

These are local operator declarations and trusted-local checks, not authenticated human signatures, hostile-code containment, or OS-enforced network/filesystem isolation.

## Runtime/capture limits

- Per-case deadline: **12 seconds**. Whole observation invocation: **180 seconds**. The earlier deadline always wins.
- Maximum stdout and stderr: **64 KiB each per run**. Read bounded streams; a quota hit kills the owned child tree, retains honest partial evidence and fails qualification. Truncated output cannot pass comparison.
- Run only under fresh disposable fixture working directories. Preserve literal argv and fixture bytes. Never use `shell=True`.
- Use a minimal environment, fresh HOME/USERPROFILE/TEMP paths, no inherited API keys, proxies, tokens, project configuration or credential stores. These environment controls are not an OS egress barrier.
- Admit only the frozen public offline case set in production for this increment. Unexpected reference behavior, any indication of an agent/provider/tool path, a guard failure or a changed target/fixture stops the generation; no automatic retries or extra case discovery.
- Track only spawned process handles/PIDs; terminate and verify only that owned tree. Never kill by executable name or touch unrelated agents. Cleanup uncertainty is a failed qualification, not a successful capture.

## Evidence and disclosure

Each run records request/approval/target/fixture digests, case ID, repeat number, exact argv, observed exit, raw stdout/stderr bytes, quota/timeout state and owned-process cleanup result. Store authoritative base64 bytes and a separately labeled readable rendering; decoding errors must not replace the authoritative bytes.

Write a terminal receipt on normal exit, nonzero exit, spawn failure, deadline, quota and interruption. Do not fabricate missing exit status or full-stream sizes after truncation. Never infer successful behavior from worker prose or a receipt merely existing.

Verification checks identities, output inventory, request binding, selected IDs, both repeats and the existing exact stdout/stderr/exit vectors. Preserve nonzero expected outcomes as nonzero; normalize neither line endings nor JSON whitespace. Dynamic capture metadata is not part of the executable-output comparison.

The export boundary defaults to no external-model disclosure. Existing selected public cases may be exported through an explicit approved public manifest after secret/privacy review. Protected/unselected records, local approvals, credentials and raw private logs are never worker inputs or public uploads.

## Testing and acceptance criteria

Use test-first changes and retain red/green guard evidence. Qualification requires:

1. Missing/stale/mismatched approval, changed executable/fixture/request, duplicate/unknown cases, unsafe paths, links/junctions, reused output and unsupported execution mode are rejected **before process creation**.
2. Harmless controlled children demonstrate exact binary stdout/stderr, nonzero exit retention, repeat mismatch detection, bounded output, timeout, spawn failure and terminal-receipt behavior. Injected false/stale receipts and mutated golden bytes must be rejected.
3. Owned descendants are confirmed exited after cancellation/quota/deadline, with unrelated processes untouched; uncertainty blocks acceptance.
4. The approved public native-MOTE case set runs twice and matches every existing exact vector. A fresh independent verifier reads the saved evidence; Jack independently spot-reexecutes selected cases rather than trusting a writer's assertions.
5. Existing Rust/Go application and public baseline files remain byte-identical. No prototype parser/runtime is changed or falsely relabeled as full MOTE. The helper and documentation form a foundation milestone only.

Factory acceptance plus independent QA must both pass before any automatic publication. Publish only a reviewed allowlisted helper/test/documentation delta to the XP repository; do not upload the original executable, approval files, protected inputs, credentials or private paths. No force push, repository replacement, release or deployment.

## Boundaries and next handoff

Always preserve exact bytes, source pins, failed artifacts, explicit unknowns and honest process state. Ask before any new dependency, reference execution beyond this known case set, accepted runtime manifest, live endpoint, unrestricted command, install or stronger machine-policy change. Never read original MOTE/SDK source or let a coding model choose arbitrary reference commands.

After this foundation qualifies, `manifest-admission` can specify genuinely new schema probes, and `provider-wire` can specify controlled loopback observations. Neither is authorized by passing this module. Complete runtime usability still requires all later approved capabilities and daily-use qualification.

## Review requested

Approve or amend this concrete observation-module contract before Factory implements it or performs new reference captures. No implementation/probe job is started by saving this draft.
