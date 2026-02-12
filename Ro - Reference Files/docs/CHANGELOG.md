# Changelog

## v1.2 (Current) - 2026-01-17
Commit: 6f6da35 | Tag: v1.2 | Status: Stable

- Cloud sync stable with Supabase (RLS disabled)
- Rolled back delete sync (was causing issues)

## v1.1 - 2026-01-17
Commits: bb471ae to a62a4c9 | Tag: v1.1

Added:
- Supabase cloud sync for cross-device data
- Load from Supabase first, fallback to IndexedDB
- Save to both Supabase and IndexedDB

Fixed:
- a62a4c9: Use legacy Supabase anon key (JWT format)

## v0.41 - 2026-01-16
Commit: 618ca70

- Refactored code structure
- Extracted ActivityCard and ChecklistItem components

## v0.40 - Baseline
Commit: 5567028

- 10 activity categories, wake-up tracking
- Daily/nightly checklists
- Monthly chart and calendar views
- CSV import/export, IndexedDB persistence

## Recent Commits
6f6da35 rollback: restore working version
a62a4c9 fix: use legacy Supabase anon key
bb471ae v1.1 - Add Supabase cloud sync
618ca70 chore: reorganize folder structure
5567028 v0.40 - baseline before refactor

## Known Issues
1. Delete not syncing to Supabase (workaround: delete in dashboard)
