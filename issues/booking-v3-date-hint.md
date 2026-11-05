# Booking-v3 shows a month-first hint to day-first tenants

## Report count

33 unique reports from ticket #4471.

## Problem

Booking-v3 displays the hint `MM/DD/YYYY` for tenants whose locales use day-first dates. People follow the hint and enter month-first values, but the form reads them in the tenant's day-first order. The result is a different bug from the shared parser failure.

This affects booking-form reports from Alder Physio, Copper Studio, and Pale Harbour. The report count is based on unique report IDs; the export contains overlapping rows that must not be counted twice.

## Evidence

- First seen: 2026-04-17
- Last seen: 2026-09-22
- Affected tenants: 3
- Channel: form (33)
- Intake tags were not reliable: 18 were tagged `hint`, while 15 were tagged `parser`.

## Expected behavior

Display a hint that matches the tenant locale, and parse the submitted value using the same order shown by the hint.

## Source

Support ticket #4471 and the exported `reports-4471.json`, classified with the tenant directory and `src/lib/kb/causes.ts`.
