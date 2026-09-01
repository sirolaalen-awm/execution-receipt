# Execution Receipt — submission draft

## Tagline

Turn “done” from an agent claim into a verifiable artifact.

## Inspiration

AI-assisted workflows often fail one step before the visible answer. A model says that a file was loaded, a package was built, or a submission was completed, and the claim becomes input for later work even when the action never happened. The result can look coherent while its trace is already contaminated.

Execution Receipt turns that failure into a small, inspectable product. The application does not ask whether an agent sounds confident. It asks what bytes were actually created, from which source revision, and whether later source changes made the earlier result stale.

The problem is derived from the prior research work “False Execution Status in LLM-Guided Processes: From Functional Substitution to Pseudo-Trace Contamination.” The competition entry is a new standalone WebMCP implementation, not the research paper or the full AWM development method.

## What it does

The user and an agent prepare a document package in one shared application state.

- Required document slots expose what is present and missing.
- An incomplete build is rejected with a concrete reason.
- A successful build creates real ZIP bytes, a manifest, per-document SHA-256 hashes, and a package SHA-256 receipt.
- Repeating an unchanged build returns the same receipt instead of inventing another execution.
- Changing a source document preserves the old package but marks its receipt stale.
- A download request is recorded only as a request; it is never reported as a confirmed save, submission, receipt, or acceptance.

The manual interface and WebMCP tools operate on the same state transition logic.

## WebMCP tools

- `get_case`: reads requirements, current revision, receipts, and the execution trail.
- `attach_document`: attaches or replaces UTF-8 text in a fixed document slot using the current revision.
- `build_package`: creates the actual ZIP and receipt, or returns the missing evidence/revision conflict.
- `get_receipt`: reads whether a receipt is current or stale.

## How it was built

The prototype uses dependency-free HTML, CSS, and JavaScript. ZIP creation, CRC32, SHA-256 hashing, revision control, receipt state, and the WebMCP adapter are implemented directly in the project. The application stores documents only in the active browser tab for the demo.

Development was regulated with AWM as a production method: define the target, separate claims from traces, test negative paths, preserve provenance, and stop at the smallest demonstrable product. AWM is not a hidden runtime dependency of the submitted application.

## Challenges

The core challenge was defining a narrow proof boundary. A hash proves that specific bytes were hashed; it does not prove that a document is true or legally valid. Creating ZIP bytes does not prove that the user saved them. A WebMCP handler call identifies the application path used, not an authenticated agent identity. The interface and receipt wording preserve these distinctions.

Concurrency was another important failure mode. An older build can finish after a source change. The application performs a final revision check before committing a receipt, so obsolete work cannot become the current result.

## Accomplishments

- Real package bytes and inspectable manifest, not a simulated success state.
- Idempotent package builds and preserved stale receipts.
- Optimistic revision checks for manual and agent-driven changes.
- Cancellation and concurrent-build protection.
- Binary-safe manual file attachments up to 2 MiB.
- 21 automated tests, including 50 deterministic chaotic sessions with 6,000 mixed operations and an independent archive verifier.

## What we learned

The strongest reliability boundary is not a longer explanation from the model. It is a product state that makes unsupported transitions difficult, exposes missing evidence, and carries the exact source revision into the result.

WebMCP is useful here because the agent and the human can act on the same visible page state. The application remains usable manually when WebMCP is unavailable.

## What is next

- Complete a real-agent WebMCP invocation test in the supported competition browser.
- Add the public HTTPS demo and final repository URL.
- Record the under-three-minute demo video.
- Consider durable signed receipts only after the local proof boundary is validated.

## Current validation status

Automated core and contract tests pass. A real WebMCP browser invocation, public hosting, and final submission are still pending and must not be presented as completed until their external traces exist.
