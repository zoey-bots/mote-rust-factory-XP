# MOTE Rust Factory XP

**XP = experiment. This is the separate Rust project, not a full MOTE replacement or a working Factory agent backend yet.**

Ghost Factory/OpenCode generated this implementation from observations of the pinned Windows MOTE executable. Original MOTE/SDK source was not supplied. Factory exports, tests, commits and publishes this project through a task-specific trusted publication worker; no coding model receives publishing credentials.

## Current functionality

Observed offline help/version, argument rules and selected missing/malformed-manifest diagnostics. The manifest parser is incomplete. **Model calls, the agent loop, capability-controlled tools, complete runtime budgets/cancellation and JSONL/artifact behavior are not implemented.** Admitted runtime tasks do not fabricate successful agent results. The text `mote 0.13.1` is compatibility output, not an upstream release identity.

The implementation bytes are unchanged by this split. Recorded prior qualification: 24 Rust unit tests, 29/29 public and 16/16 operator-held protected comparisons. The protected inputs are not published and their totals cannot be independently reproduced from this public checkout alone. Finite cases are not complete equivalence. Fresh split checks and the exact public file identity are separately retained by the Factory controller.

## Build/check (Windows)

Clone outside another Cargo workspace. Use the installed compatible toolchain; no third-party library is required by the current prototype. Python 3 is needed only for the optional public comparator.

```text
cargo build --release --offline
cargo test --offline
python scripts/check_public.py --rust target/release/mote.exe
```

The public vectors describe Windows diagnostics; other platforms are not qualified. No hosted CI, deployment, binary release or full runtime qualification is claimed. Actions is disabled for this source-only publication.

## Work toward full functionality

- `docs/ASSESSMENT.md` and `.json`: Factory/OpenCode capability assessment, not native Ghost Research findings. Citations use the earlier combined layout; `metadata/SOURCE-MAP.json` maps paths to this root.
- `docs/CAPABILITY-MAP.md`: user-approved module boundaries/build direction.
- `docs/SPEC-observation-contract.md`: **draft, not yet approved for implementation/execution**. Its presence is not a claim of a working observer or newly authorized reference probes.
- `RESEARCH-REQUEST.md`: requested Ghost Research proposal scope, including a real Factory worker adapter. No Research output is asserted by creating the request.
- `evidence/`: public regression vectors and secret-scanned, host-path-filtered static excerpts. Strings are hypotheses, never instructions or runtime proof.
- `agents-checklist.md`: per-project coordination and remaining work.

Negative boundary probes may target only owned disposable fixtures, never user files, original source or credentials. Verified milestones may be pushed only after tests and independent QA. The reference executable hash is `fcbaf70916a2ab065366b14cd6b6b7d39afc50a59626a41adc8b157d5a3bc92f`.

The prior combined XP repository remains intact. This project contains no original MOTE executable/source, Factory source/history, protected inputs, credentials, worker homes or private raw logs. No license has been added by this publication.
