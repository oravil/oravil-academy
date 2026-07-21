# ADR-0007 — PHP Runtime Version

**Document ID:** OA-ADR-0007
**Version:** 1.0.0
**Status:** Accepted
**Date:** 2026-07-21
**Reference:** OA-REV-003 F-3, OA-HANDOFF-001 PMV-003

---

## Status

Accepted

---

## Context

OA-REV-003 finding F-3 recorded PHP 8.4 as the approved runtime version, with `composer.json` and CI to be aligned to it. That alignment was completed: `composer.json` requires `^8.4` and CI pins 8.4.

During OA-HANDOFF-001 Task 4 (PMV-003), the deployment server's PHP CLI was found running 8.5.4 instead of the approved 8.4 — installed as the distribution default on this server's OS (Ubuntu 26.04 LTS "resolute").

Pinning the server to 8.4 was attempted via `update-alternatives`, which requires a PHP 8.4 package to switch to. None exists in this OS release's default apt repositories. The standard fallback for non-default PHP versions on Ubuntu, the `ondrej/php` PPA, was added and queried — it has no published package index for the "resolute" release at all (`404 Not Found` on the PPA's `Release` file), so no PHP 8.4 package is installable from it either. Building PHP 8.4 from source, or forcing packages built for an older Ubuntu codename onto a newer base OS (with attendant library/ABI mismatch risk), were both considered and rejected as disproportionate to the problem: there is no material technical reason this MVP requires 8.4 specifically over 8.5, only that 8.4 was the version originally approved.

---

## Decision

The approved PHP runtime version is amended from **8.4** to **8.5**.

`composer.json` (`^8.4` → `^8.5`), CI pin, and `CLAUDE.md` §2 are updated to match, so the documented decision, the dependency constraint, and the CI runtime are consistent — the exact drift OA-GUIDE-001 prohibits.

---

## Non-Goals

- This ADR does not change any other part of the approved stack (Laravel 12, PostgreSQL 16, Sanctum stateful SPA auth, etc.).
- This ADR does not mandate 8.5 for environments outside this server unless those environments hit the same OS-availability constraint.

---

## Consequences

### Included

- `composer.json` `require.php` becomes `^8.5`.
- CI workflow's PHP version pin becomes 8.5.
- `CLAUDE.md` §2 stack line updated to read PHP 8.5.
- Server CLI (already 8.5.4) requires no further change — it already satisfies the amended decision.

### Excluded

- No code changes are implied by this version bump; nothing in the codebase was found to require 8.4-specific behavior.

### Future Implications

- If a future PHP version bump is needed, check OS/PPA package availability before approving a specific minor version, to avoid repeating this gap between an approved decision and what the target OS can actually install.
