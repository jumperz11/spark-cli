# Active Review Tracker

## Snapshot
- Updated (UTC): 2026-06-01T07:46:28Z
- Last automation run (UTC): 2026-05-31T22:58:53.021Z
- Scope: Spark Compete active PR/review gates for `vibeforge1111/spark-cli` plus Telegram-first hunt lane.

## Active PRs (author: `jumperz11`)

### PR #510 — `[spark-compete] surface latest browser-use action in status`
- URL: https://github.com/vibeforge1111/spark-cli/pull/510
- State: open (`mergeable=true`, `draft=false`)
- Maintainer/reviewer signal: no timeline comments; no review threads.
- Gate: waiting for first maintainer/security/jury/lab signal.

### PR #462 — `[spark-compete] Redact browser-use JSON artifact paths`
- URL: https://github.com/vibeforge1111/spark-cli/pull/462
- State: open (`mergeable=true`, `draft=false`)
- Last maintainer signal: focused rebase/clean replacement required; contributor replied with one-commit refreshed proof (`93f18dd`).
- Review threads: none (`review_threads=[]`).
- Gate: waiting for maintainer/security/lab re-check.

### PR #461 — `[spark-compete] Fix browser-use task option parsing`
- URL: https://github.com/vibeforge1111/spark-cli/pull/461
- State: open (`mergeable=true`, `draft=false`)
- Last maintainer signal: merge/adopt rebase-required gate (`BLOCKED`); contributor replied branch current + focused proof.
- Review threads: none (`review_threads=[]`).
- Gate: waiting for maintainer/jury/security/lab follow-up.

### PR #451 — `[spark-compete] Use configured bundle in verify repair commands`
- URL: https://github.com/vibeforge1111/spark-cli/pull/451
- State: open (`mergeable=true`, `draft=false`)
- Last maintainer signal: merge/adopt rebase-required gate (`BLOCKED`); contributor replied branch current (`0 1`) with focused proof.
- Review threads: none (`review_threads=[]`).
- Gate: waiting for maintainer/jury/security/lab refresh.

### PR #441 — `[spark-compete] Report setup-needed state for update repair`
- URL: https://github.com/vibeforge1111/spark-cli/pull/441
- State: open (`mergeable=true`, `draft=false`)
- Last maintainer signal: merge/adopt rebase-required gate (`BLOCKED`); contributor replied branch current (`0 1`) with focused proof.
- Review threads: none (`review_threads=[]`).
- Gate: waiting for maintainer/jury/security/lab refresh.

### PR #440 — `[spark-compete] Keep healthcheck repair details from collapsing to braces`
- URL: https://github.com/vibeforge1111/spark-cli/pull/440
- State: open (`mergeable=true`, `draft=false`)
- Last maintainer signal: security-safe redesign requested; contributor posted one-commit refreshed proof (`99f2a8e`).
- Review threads: none (`review_threads=[]`).
- Gate: waiting for maintainer/security/lab follow-up.

### PR #439 — `[spark-compete] Fix Codex provider readiness when CLI auth is missing`
- URL: https://github.com/vibeforge1111/spark-cli/pull/439
- State: open (`mergeable=true`, `draft=false`)
- Last maintainer signal: merge/adopt rebase-required gate (`BLOCKED`); contributor replied branch current (`0 1`) with focused proof.
- Review threads: none (`review_threads=[]`).
- Gate: waiting for maintainer/jury/security/lab refresh.

### PR #438 — `[spark-compete] Isolate live command tests from operator config`
- URL: https://github.com/vibeforge1111/spark-cli/pull/438
- State: open (`mergeable=true`, `draft=false`)
- Last maintainer signal: merge/adopt rebase-required gate (`BLOCKED`); contributor replied branch current (`0 1`) with focused proof.
- Review threads: none (`review_threads=[]`).
- Gate: waiting for maintainer/jury/security/lab refresh.

## Delta Since Last Run (2026-05-31T22:58:53.021Z)
- GitHub connector polling succeeded for PR metadata, timeline comments, and review-thread checks across PRs #510/#462/#461/#451/#441/#440/#439/#438.
- No maintainer/reviewer comments newer than the previous run window were found. Latest maintainer exchange remains the 2026-05-31 19:18-19:39 UTC `merge/adopt rebase required (BLOCKED)` series with contributor proof replies.
- All polled review-thread lists remain empty (`review_threads=[]`).
- Local `gh` CLI remains unusable (`default` token invalid; direct API call fails).

## Review Queue Decision
- No new maintainer feedback requires a contributor reply in this run.
- Queue remains in wait-for-maintainer/security/jury/lab refresh state.

## Hunt Lane (Telegram-first / direct Spark behavior)
- Runtime onboarding in this environment is still not fully green:
  - `/Users/jumperz/.spark/bin/spark verify --onboarding --json` -> `ok=false` (`module_health`, `builder_memory_direct_smoke`, `spawner_mission_relay`, `runtime_processes`)
- Distinct high-signal bug validated in local source tree:
  - Both `spark fix telegram --json` and `spark verify --onboarding --json` previously emitted hard-coded base-bundle repair commands (`telegram-starter`) even when setup bundle is `telegram-voice-starter`.
- Local patch status (not published in this run):
  - `src/spark_cli/cli.py`: both Telegram fix and onboarding verify payloads now derive setup/start/restart repairs from the configured bundle.
  - `tests/test_cli.py`: regressions cover both fix payload and verify payload voice-bundle repair commands.
  - Packet draft: `docs/spark-compete-hotfix-fix-telegram-bundle-aware-commands.json`.

## Verification (this run)
- Review polling
  - GitHub connector: `_fetch_pr`, `_fetch_pr_comments`, `_list_pull_request_review_threads` for PRs #510/#462/#461/#451/#441/#440/#439/#438.
- Hunt proof
  - `PYTHONPATH=src python -m pytest tests/test_cli.py -q -k "collect_telegram_fix_payload_uses_configured_voice_bundle_commands or collect_verify_payload_uses_configured_voice_bundle_repairs or collect_verify_payload_reports_launch_ready_stack or collect_telegram_fix_payload_reports_polling_conflict_from_logs"` -> 4 passed
  - `python -m py_compile src/spark_cli/cli.py tests/test_cli.py` -> passed
  - `PYTHONPATH=src python -m spark_cli.cli fix telegram --json` -> payload now emits `spark restart telegram-voice-starter` and `spark setup telegram-voice-starter`
  - `PYTHONPATH=src python -m spark_cli.cli verify --onboarding --json` -> payload now emits `spark setup telegram-voice-starter` / `spark start telegram-voice-starter` in repair checks and `next_commands`
  - `git diff --check` -> passed

## Remaining Blockers
- Maintainer/security/jury/lab gate refreshes are still pending on all active PRs.
- Local `gh` CLI auth/connectivity remains broken.
- Local runtime onboarding remains degraded in this environment (spawner supervision + builder-memory direct-smoke + mission relay).

## Next Best Action
1. Keep review queue in watch mode; only reply when a fresh maintainer/security/jury comment lands.
2. If author chooses to publish the hunt fix, open one focused PR for `spark fix telegram` bundle-aware restart/setup guidance using the prepared patch + packet.
3. Prefer publishing the broader bundle-guidance fix as one focused PR that covers both `spark fix telegram` and `spark verify --onboarding`, since the same user-visible bug is present in both surfaces.
4. Continue local runtime recovery toward a fully green onboarding baseline before additional Telegram-first hunts.
