# Execution Receipt — local prototype 0.1

Working product name. Source concept: **False Execution Status in LLM-Guided Processes: From Functional Substitution to Pseudo-Trace Contamination**. AWM is the development method, not a runtime dependency.

## Run and test in Codex

Requires Node.js 22+; no npm install. The independent archive test additionally uses Python 3's standard-library zipfile module (no pip install).

```sh
node --test tests/core.test.mjs
node server.mjs
```

Open http://127.0.0.1:8080 in a browser. Serve over localhost or HTTPS rather than opening index.html as a file. Stop the server with Ctrl+C. Only loopback is bound; this is not public hosting.

## What is implemented

- Three document slots with user-selectable requirements (at least one required).
- Synthetic brief and budget initially present; approval document missing.
- Real file attachment/replacement, binary-safe, 1 byte–2 MiB per document.
- Shared state logic for manual UI and WebMCP handlers; optimistic revision checks.
- Real ZIP STORE archive with selected documents and manifest.json.
- SHA-256 per document and ZIP; receipt JSON downloads separately.
- Source revision changes mark existing receipts stale without destroying their bytes.
- Idempotent build, concurrency/revision protection, cancellation before commit.
- Document text is rendered with textContent; no execution of supplied HTML.
- Session-only in-memory state; no uploads to servers, telemetry, model API or credentials.

## Manual acceptance test

1. Open page: 2/3 documents, missing approval; no receipt.
2. Click Build package: rejected with missing slot; no ZIP and a blocked event.
3. Add sample approval (synthetic, not legal approval): 3/3.
4. Build: actual ZIP bytes plus current receipt; download ZIP and Receipt JSON.
5. Extract ZIP with an independent archive program; compare files and manifest hashes. ZIP SHA-256 must match Receipt JSON.
6. Replace the brief: prior receipt becomes stale, old ZIP remains downloadable.
7. Build again: new receipt for current revision; old one remains stale.
8. Repeat build unchanged: no duplicate receipt.
9. Change required selection: prior receipts become stale; next ZIP contains selected slots only.
10. Check keyboard use, phone/tablet layout, file attachment, errors and downloads. Refresh: workspace intentionally resets.

## WebMCP integration

Uses `document.modelContext.registerTool` with feature detection and AbortSignal registration cleanup, following the official Chrome imperative API page checked 2026-08-31:
https://developer.chrome.com/docs/ai/webmcp/imperative-api

Tools: get_case, attach_document, build_package, get_receipt. attach_document accepts UTF-8 text; the manual UI supports binary files. Agent writes require the current expectedRevision. Build creates in-memory ZIP bytes; a human downloads via UI.

Real-agent test: in a WebMCP-enabled browser, ask the agent to read the case, attempt the incomplete build, attach a clearly synthetic approval document, and build. Then replace a source manually and ask the agent to read the receipt again. Observe actual tool discovery/calls and UI changes. **Tool-definition tests with a mock API do not prove this real-browser integration.** No simulated agent chat is provided.

## Evidence boundaries and security

Receipt means ZIP bytes were created in the current tab. It does not mean saved-to-disk, submitted, received or accepted. Download requests always remain not_confirmed. Presence/hashes do not prove truth, semantic completeness, legal approval or safety of documents. Original filenames and files may be malicious; treat them as untrusted, and do not execute extracted files. SHA-256 hashes are not digital signatures.

The event actor labels indicate the invoked application handler, not authenticated identity. In-memory state and receipt JSON are not a tamper-proof ledger; local scripts or a compromised browser can falsify them. Requirements are editable in the manual UI. The tool interface deliberately does not expose a requirement-removal action, but this is not a security boundary against code with page access.

## License

Copyright 2026 Alen Širola. Licensed under the Apache License, Version 2.0. See `LICENSE` and `NOTICE`.

The license covers the files distributed in this repository. It does not grant rights to the broader AWM method, unpublished development memory, or separate research works merely referenced by this project.

## Delivery status

Local source and automated contract/unit tests only. Browser UI, real WebMCP agent invocation, public hosting, submission video, registration and competition submission remain separate validation/release steps. No contest acceptance or recognition is claimed.

## Suggested next Codex task

Run the tests, then test the manual acceptance flow in a browser. Inspect the ZIP with an independent extractor. Report each check as passed, failed or not tested. Separately test actual WebMCP discovery and invocation in a supported browser. Do not equate mock handler execution or a registration badge with a real agent test. Preserve the distinction between package creation and disk download. Do not publish or submit without approval.
