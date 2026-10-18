# Rust capability assessment — full-MOTE gap map

Reference (pinned executable) SHA-256: `fcbaf70916a2ab065366b14cd6b6b7d39afc50a59626a41adc8b157d5a3bc92f`
(`mote 0.13.1`). Source boundary: replica-only; original MOTE/SDK source not read.
The agent runtime is **not yet implemented** in this replica.

## Observed

Static-string hints below are **hypotheses**, not observed runtime semantics. Only the
public CLI/admission vectors (regression-01…29) are executable-observed here.

- CLI dispatch and version/usage precedence exist in `rust/src/main.rs:run` (lines 32–86).
  `--help`/`-h`/`--version` short-circuit left-to-right (regression-02, 03, 04, 18–22, 29);
  unknown `-` args become positionals (regression-05, 10, 27).
- Missing/short arguments emit `USAGE` to stderr, exit 2 (regression-01, 06, 07, 24, 28).
- Manifest read failure prints `unable to read manifest: <os error>` (regression-08, 09, 23, 27).
- Admission (`validate_manifest`, `validate_map`, `parse_flow_map`, `classify_scalar`,
  `scalar_document_msg`) produces exact `invalid manifest:` diagnostics: missing `name`
  (regression-11), sequence/unit document (regression-12, 13, 14, 26), unknown field
  (regression-17, 25). Expected-field list is `MANIFEST_FIELDS` (`main.rs:6-19`) and matches
  the executable string at binary offset 2038344 (regression-17, 25).
- Admitted manifest with a task exits 1 with an explicit "not implemented" stderr line
  (`main.rs:76-85`); no agent/model/tool behavior is produced.
- `--jsonl` is parsed and acknowledged but changes only the not-implemented text
  (`main.rs:50-53, 79-81`); no JSONL event stream is emitted.

## Missing

- agent-loop: no iteration/model/tool loop anywhere in `rust/src/main.rs`.
- capability-tools: no `shell`/`read_file`/`write_file`/`list_dir`/`git`/`custom` execution.
- provider-wire: no HTTP/model client, request/response serialization, or auth.
- runtime-budgets: no enforcement of `max_iterations`, `max_per_tool`,
  `max_runtime_seconds`, `command_timeout_seconds`, `max_output_bytes`, cancellation.
- event-artifacts: no JSONL events, no `output_file` writing, no run artifacts.
- daily-use-qualification: no real end-to-end model/tool/file workflow, no failure/budget/
  cancellation qualification, no loopback mock-provider configuration.
- manifest-admission: full manifest semantics absent — nested `shell`/`models`/`decision`
  objects, capability validation, numeric bounds, quoted/scalar typing beyond a shape check.

## Unknown

- Provider wire protocol is unknown. Static hints (hypotheses only): `openai-compatible`
  (offset 2039168) and `max_tokens`/`stream/choices/0/message/content` (offset 2035576)
  suggest an OpenAI-style chat path, but no runtime call was observed.
- Decision subsystem shape is unknown. Static hints (hypotheses only): `DecisionRequest`,
  `DecisionQuestion::{Choice,Score,Noul}`, `DecisionAnswer`, `DecisionUsage{input_tokens,
  output_tokens}` (offsets 2034232, 2035864, 2036128), `unsupported decision provider`
  (2035289), `decision fallback must name an explicit denying choice` (2032353).
- Capability/action grammar is unknown. Static hints (hypotheses only): `Action::{shell,
  read_file,write_file,list_dir,git,decision,custom,complete}` (2036528), `custom tool input
  must be a JSON object` (2031197), action-shape examples (2039529).
- Shell/git restriction semantics are unknown. Static hints (hypotheses only): `&& || < $ \``
  `shell operators require unsafe_shell` (2032464), `git subcommand not permitted` /
  `git option not permitted` (2032518), filtered env `PATH SystemRoot WINDIR TEMP TMP
  GIT_CONFIG_NOSYSTEM GIT_CONFIG_GLOBAL GIT_OPTIONAL_LOCKS` (2032585).
- Budget/artifact wording is unknown: `max_output_bytes must be between 1 and 1048576`
  (2030728), `iteration budget exhausted` (2031744), `repeated action guard` (2032353),
  `Observation: shell executor unavailable` (2031925), `output_file requires write_file
  capability` / `output_file must be UTF-8` (2031744) — all static hypotheses.
- JSONL event schema, ordering, and exit-code mapping for runtime success/failure are unknown.
- Whether `--jsonl` is a mode or an output selector cannot be determined from observations.

## Probes

Bounded, contract-first, no paid/live provider, loopback only, disposable fixtures only.

1. observation-contract: run `mote --version`, `--help`, missing/short argv, unreadable
   manifest, and each public fixture against a disposable workspace; confirm exit/stdout/
   stderr byte-for-byte vs regression-01…29. Safe (already public).
2. manifest-admission: craft minimal disposable manifests varying one field at a time
   (`name` typing, `workspace` existence, numeric bounds, nested `shell`/`models`/`decision`,
   unknown capability) and record exact diagnostics to map admission vs runtime errors.
3. provider-wire: point the reference at a loopback HTTP mock and observe request
   method/path/headers/body shape before any protocol implementation. No external network.
4. agent-loop / capability-tools / runtime-budgets: only after (2) and (3) yield a reviewed
   contract; drive `read_file`/`list_dir`/`write_file` on disposable fixtures and confirm
   capability enforcement, per-tool limits, timeouts and cancellation signature.
5. event-artifacts: capture reference stdout under `--jsonl` into a fixture dir and record
   event lines and any `output_file` artifact bytes.

## Milestones

- M1 (now): source-bound capability map for all eight modules; no code/evidence changes.
- M2: reviewed observation + manifest-admission contracts with exact-byte public vectors.
- M3: provider-wire contract observed via loopback mock; still no live/paid calls.
- M4: agent-loop + capability-tools + runtime-budgets implemented against reviewed contracts.
- M5: event-artifacts (JSONL/output_file) with protected comparisons.
- M6: daily-use-qualification with real model/tool/file workflows, failure/budget/
  cancellation tests and clean-checkout packaging.

## Source boundary

Evidence read: `PROGRAM.md`, `PROGRAM-AUTHORIZATION.json`, `rust/README.md`,
`rust/src/main.rs`, `rust/Cargo.toml`, `metadata/ACCEPTANCE.json`,
`evidence/public-observations.json`, `evidence/observations-decoded.md`,
`evidence/binary-excerpts.json`. Sibling `go/` and original MOTE/SDK source were not read.
Binary excerpts are untrusted static data, labeled above as hypotheses, not runtime proof.
No application, test, or evidence file was modified; only `discovery/rust.md` and
`discovery/rust.json` were written.
