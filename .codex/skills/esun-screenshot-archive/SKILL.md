---
name: esun-screenshot-archive
description: Archive requested authenticated eSun pages, dropdown/group selections, pagination, and long review content into a dated project folder with auditable verification.
last_edited: 2026-10-31
---

## When to use

Use for requested eSun AI challenge screenshot rescans or evidence archives in `/Users/kennykang/Projects/2026-esun-ai-challenge-datapack`. Do not use it to upload, submit, change code, or infer historical content that the live site no longer exposes.

## Inputs and first checks

1. Use `$0` as the project root, or `/Users/kennykang/Projects/2026-esun-ai-challenge-datapack` if omitted.
2. Inspect existing screenshot archives and create a new dated destination; preserve prior files.
3. Enumerate the requested navigation pages, visible date selectors, group filters, and pagination before capturing.
4. Record the expected selected date/group and page markers or content counts for each final capture.

## Procedure

1. Capture requested base pages and expanded/overflow states where relevant.
2. For every visible selector or group filter, choose each option, wait for selected-option-specific rows, counts, or other content, then save a final selected-state capture. Keep stale intermediate captures only as trace evidence.
3. Traverse every requested pagination branch. After each navigation, verify the updated page marker and visible content before capture.
4. For `pre.rv-pre` or similar large review content, tile both scroll axes, retain source tiles and an index, stitch with integer geometry, then validate a zero-uncovered-pixel coverage mask and Pillow decoding.
5. Produce an index and machine-readable manifest/verification/audit files with exact artifact paths.

## Efficiency plan

- Inventory selectors and pages once, then drive a capture matrix instead of repeatedly rediscovering controls.
- Validate images in batch and use a coverage mask for stitched reviews.
- Stop and report live unavailable/empty states rather than claiming old pages or reviews were recaptured.

## Pitfalls and fixes

- Selector result is stale: wait for the expected date-specific count/content and use the later final capture in the index.
- Pagination screenshot has the old page: verify a page marker and visible row/content after navigation.
- Locator acts on the wrong page: inspect the current page before using a role locator.
- Stitch has a gap or Pillow rejects coordinates: use consistent integer rounding, then assert zero uncovered pixels before reporting success.

## Verification checklist

- Every requested date/group/pagination option has a final indexed artifact.
- Existing archives remain intact and the new archive is dated/project-local.
- Primary images decode successfully.
- Long review outputs have source-tile indexes, valid decoding, and zero uncovered pixels.
- Index, manifest, verification, and audit artifacts provide exact paths and coverage evidence.
