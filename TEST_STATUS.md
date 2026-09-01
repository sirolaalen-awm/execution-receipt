# Validation status — 2026-08-31

Executed against the local 0.1 source in Codex:

| Check | Result |
|---|---|
| `node --test tests/*.test.mjs` | PASS — 21 tests, 0 failures |
| Deterministic chaos test | PASS — 50 sessions × 120 mixed operations = 6,000 steps |
| Competing obsolete/current builds | PASS — obsolete build cannot commit a current receipt |
| Independent Python zipfile extraction / CRC / document SHA-256 | PASS — included in test suite |
| `node --check app.mjs` | PASS |
| Local HTTP GET /, app.mjs, core.mjs, webmcp.mjs, style.css | PASS — correct status and MIME types |
| Unknown HTTP path / disallowed POST | PASS — 404 / 405 |
| Visual layout, mobile layout, keyboard flow | NOT TESTED — cloud browser security policy blocked local/data URL; no bypass attempted |
| Browser file picker and download completion | NOT TESTED |
| Real WebMCP browser registration and agent invocation | NOT TESTED — mock contract tests only |
| Public hosting / competition submission | NOT PERFORMED |

The passing tests are evidence for core state transitions and archive creation, not for untested browser or external workflows. Download remains unconfirmed by design.
