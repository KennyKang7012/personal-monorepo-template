---
name: esun-datapack-guarded-download
description: Download and validate an official eSun challenge datapack when the live `/docs` page publishes a transaction date; stop safely for unavailable dates or ambiguous browser outcomes.
last_edited: 2026-10-31
---

## When to use

Use for the eSun AI challenge’s manual or scheduled datapack-download workflow. Do not use it for uploads, submissions, CAPTCHA bypass, or any action that treats form/page access as a successful download.

## Inputs and first checks

1. Use `$0` as the project root, or `/Users/kennykang/Projects/2026-esun-ai-challenge-datapack` if it is omitted.
2. Load authenticated `/docs` and read the live `本次繳交對應交易日` or an official filename.
3. If the date is `—`, the window is unavailable, login/CAPTCHA blocks the download, or the date is ambiguous, stop and report the observed state. Do not infer a date from the calendar or a trial-validation window.
4. Check whether the exact canonical root file `DataPack_<team>_<date>.json` already exists. Preserve existing or corrupt files; do not overwrite.

## Procedure

1. Register the browser download event before clicking `下載資料包`.
2. Require the event to complete and record its actual local path. A browser-generated suffix such as `(1)` or `(3)` is source-only, not canonical naming.
3. Copy only to the canonical root filename after the non-overwrite check.
4. Validate the destination is non-empty, JSON parses, source and destination are byte-for-byte identical, and record SHA-256.
5. Report the official date, canonical path, source path, byte count, parse/equality/hash evidence, or the exact guarded-stop reason.

## Efficiency plan

- Read `/docs` and check the exact canonical destination before any click.
- Reuse one validation command for non-empty, JSON parsing, byte equality, and SHA-256.
- Stop immediately on unavailable/ambiguous date or missing download event; do not retry a timeout without explicit reauthorization and fresh date/destination checks.

## Pitfalls and fixes

- `waitForEvent('download')` times out or the tab changes page: do not claim a path or success; wait for explicit reauthorization before retrying.
- Browser download has a suffix: retain its actual path as source and copy to the non-suffixed official filename only.
- Existing canonical file: do not redownload or overwrite; inspect/report its state according to the requested scope.

## Verification checklist

- Live official transaction date/filename was observed.
- Exact canonical destination was absent before copy, or a guarded skip was reported.
- Download event completed with a concrete source path.
- Destination is non-empty, parseable JSON, byte-identical to source, and has a recorded SHA-256.
