# Demo video script — target 2:20–2:40

## 0:00–0:15 — Problem

Screen: title and initial 2/3 readiness.

Narration: “An agent can say a task is complete even when the required action never happened. Once that claim becomes input for the next step, the workflow has a false trace.”

## 0:15–0:35 — Shared state

Screen: required documents and missing approval document; WebMCP connection status.

Narration: “Execution Receipt gives the user and the agent one shared state. The project brief and budget exist. The approval document does not.”

## 0:35–0:55 — Rejected false completion

Action: ask the agent to read the case and build the package. Show the tool call and blocked result.

Narration: “The agent attempts completion. The application rejects it and names the missing evidence. A confident claim cannot change readiness.”

## 0:55–1:20 — Actual execution

Action: agent attaches a clearly synthetic approval document, reads the new revision, then calls build_package.

Narration: “The agent adds the missing demo document using the current revision. The application now creates actual ZIP bytes, an inspectable manifest, and SHA-256 receipts for the files and package.”

## 1:20–1:45 — Human inspection

Action: expand manifest; download ZIP and receipt JSON.

Narration: “The human can inspect the same result. The boundary stays explicit: the package exists in this tab, while saving to disk remains unconfirmed until it happens outside the application.”

## 1:45–2:10 — Staleness

Action: manually replace the project brief; show old receipt become stale; ask agent to get_receipt.

Narration: “When a source changes, the old package is preserved but it stops representing the current revision. The next agent call sees the corrected state.”

## 2:10–2:30 — Close

Screen: execution trail, current/stale receipt, title.

Narration: “Execution Receipt turns ‘done’ from a sentence into an artifact with a bounded, inspectable trace. It does not promise universal truth. It proves the execution that the application actually performed.”

## Recording locks

- Use only synthetic documents.
- Show a real WebMCP tool call; do not simulate chat or tool output.
- Keep the browser address and connection state visible long enough to verify the live demo.
- Do not say downloaded, submitted, accepted, secure, authenticated, or tamper-proof unless separately proven.
- Final public URL, repository, license, and WebMCP test must exist before recording.
