# Shared parser treats every date as month-first

## Report count

379 unique reports from ticket #4471.

## Problem

The shared parser reads every numeric date as `MM/DD/YYYY`. All 11 tenants in the export use day-first locales, so dates whose day is 12 or less are interpreted with the month and day reversed.

This affects reports submitted through the app, email, and booking forms. The report count is based on unique report IDs; the export contains 15 overlapping duplicate rows that must not be counted twice.

## Evidence

- First seen: 2026-04-01
- Last seen: 2026-10-04
- Affected tenants: 11
- Channels: app (204), email (150), form (25)
- Intake tags were not reliable: 356 were tagged `parser`, while 23 were tagged `hint`.

## Expected behavior

Parse numeric dates in the tenant's locale order, or require an unambiguous machine-readable date format before parsing.

## Source

Support ticket #4471 and the exported `reports-4471.json`, classified with the tenant directory and `src/lib/kb/causes.ts`.
