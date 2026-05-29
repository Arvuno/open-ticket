# Selected 10 PR Plan — open-discord-bots/open-ticket

## Selection Criteria

- Low risk only (no permission/security changes, no public API changes, no database migrations)
- Max 350 lines per PR
- Each independently mergeable
- No new dependencies

---

## Selected PRs

### PR-01: Implement `/topic list` subcommand
| Field | Value |
|-------|-------|
| candidate_id | PR-02 |
| Title | Implement `/topic list` subcommand |
| Linked Issue | #200 |
| Risk Level | Low |
| Expected Diff | ~80 lines |
| Maintainer Discussion | No |
| Merge Likelihood | High |
| File | `src/data/framework/commandLoader.ts` |
| Test Plan | Build succeeds, command registers, list displays correctly with status/priority filters |
| Notes | Addresses explicit TODO in commandLoader.ts:576 `//TODO: list (v4.2)` |

### PR-02: Implement `/priority list` subcommand
| Field | Value |
|-------|-------|
| candidate_id | PR-03 |
| Title | Implement `/priority list` subcommand |
| Linked Issue | #200 |
| Risk Level | Low |
| Expected Diff | ~80 lines |
| Maintainer Discussion | No |
| Merge Likelihood | High |
| File | `src/data/framework/commandLoader.ts` |
| Test Plan | Build succeeds, command registers, list displays correctly with status/priority filters |
| Notes | Addresses explicit TODO in commandLoader.ts:613 and :1178 |

### PR-03: Improve "Server Id Missing" error message
| Field | Value |
|-------|-------|
| candidate_id | PR-04 |
| Title | Improve "Server Id Missing" error message |
| Linked Issue | None |
| Risk Level | Low |
| Expected Diff | ~5 lines |
| Maintainer Discussion | No |
| Merge Likelihood | High |
| File | `src/index.ts` |
| Test Plan | Build succeeds, error message includes config file and setting name |
| Notes | Cryptic error from index.ts:370 — should tell users which config to fix |

### PR-04: Improve "Unknown backup language" error message
| Field | Value |
|-------|-------|
| candidate_id | PR-05 |
| Title | Improve "Unknown backup language" error message |
| Linked Issue | None |
| Risk Level | Low |
| Expected Diff | ~5 lines |
| Maintainer Discussion | No |
| Merge Likelihood | High |
| Files | `src/index.ts` |
| Test Plan | Build succeeds, error message explains the setting and config file |
| Notes | Error from index.ts:223 — backup language = fallbackLanguage in general.jsonc |

### PR-05: Complete Japanese urlInvalid translation keys
| Field | Value |
|-------|-------|
| candidate_id | PR-06 |
| Title | Complete Japanese urlInvalid translation keys |
| Linked Issue | None |
| Risk Level | Low |
| Expected Diff | ~10 lines |
| Maintainer Discussion | No |
| Merge Likelihood | High |
| File | `languages/japanese.json` |
| Test Plan | Build succeeds, Japanese bot responds with translated error messages |
| Notes | Found during quality audit — keys `urlInvalidHttp` and `urlInvalidProtocol` have English text in Japanese file |

### PR-06: Complete Korean urlInvalid translation keys
| Field | Value |
|-------|-------|
| candidate_id | PR-07 |
| Title | Complete Korean urlInvalid translation keys |
| Linked Issue | None |
| Risk Level | Low |
| Expected Diff | ~10 lines |
| Maintainer Discussion | No |
| Merge Likelihood | High |
| File | `languages/korean.json` |
| Test Plan | Build succeeds, Korean bot responds with translated error messages |
| Notes | Same audit finding as PR-05 for Korean language |

### PR-07: Complete Simplified Chinese urlInvalid translation keys
| Field | Value |
|-------|-------|
| candidate_id | PR-08 |
| Title | Complete Simplified Chinese urlInvalid translation keys |
| Linked Issue | None |
| Risk Level | Low |
| Expected Diff | ~10 lines |
| Maintainer Discussion | No |
| Merge Likelihood | High |
| File | `languages/simplified-chinese.json` |
| Test Plan | Build succeeds, Chinese bot responds with translated error messages |
| Notes | Same audit finding for Simplified Chinese |

### PR-08: Add panelButtonRowLength minimum validation
| Field | Value |
|-------|-------|
| candidate_id | PR-15 |
| Title | Add panelButtonRowLength minimum validation |
| Linked Issue | #200 |
| Risk Level | Low |
| Expected Diff | ~15 lines |
| Maintainer Discussion | No |
| Merge Likelihood | High |
| Files | `src/data/framework/checkerLoader.ts` |
| Test Plan | Build succeeds, config rejects panelButtonRowLength < 1 with clear error |
| Notes | Max validation exists for 5-button Discord limit; minimum (1) not yet validated |

### PR-09: Clean up TODO placeholders in component loading sequence
| Field | Value |
|-------|-------|
| candidate_id | PR-12 |
| Title | Clean up TODO placeholders in component loading sequence |
| Linked Issue | None |
| Risk Risk | Low |
| Expected Diff | ~15 lines |
| Maintainer Discussion | No |
| Merge Likelihood | High |
| Files | `src/index.ts` |
| Test Plan | Build succeeds, startup sequence unchanged |
| Notes | Three `//TODO!!` placeholders at lines 660, 668, 771 — either implement or document why skipped |

### PR-10: Improve panel dropdown error message specificity
| Field | Value |
|-------|-------|
| candidate_id | PR-14 |
| Title | Improve panel dropdown error message specificity |
| Linked Issue | None |
| Risk Level | Low |
| Expected Diff | ~5 lines |
| Maintainer Discussion | No |
| Merge Likelihood | High |
| Files | `src/builders/dropdowns.ts` |
| Test Plan | Build succeeds, error message identifies the failing option |
| Notes | Error from dropdowns.ts:58 — currently doesn't say which option caused the mismatch |

---

## Merge Order

| Order | PR | Reason |
|-------|----|--------|
| 1 | PR-05, PR-06, PR-07 (3 translation PRs) | Trivial, independently mergeable, no conflicts |
| 2 | PR-03, PR-04 (2 error message PRs) | Trivial, no file overlap, independently mergeable |
| 3 | PR-10 (panel dropdown error) | Small, different file |
| 4 | PR-08 (panelButtonRowLength validation) | Small, checkerLoader.ts — no overlap |
| 5 | PR-09 (TODO cleanup) | Small, index.ts — no overlap |
| 6 | PR-01, PR-02 (topic list / priority list) | Same file (commandLoader.ts) but different sections — safe to merge in either order |

---

## Deferred Candidates (Not Selected)

| Candidate | Reason |
|-----------|--------|
| PR-01 (response-time/resolution-time stats) | Too large combined (~550 lines). Could be split into statLoader implementation only (~150 lines) as separate PR. |
| PR-10 (autoclose/autodelete stat language keys) | ~200 lines across 38 language files — valid candidate but defer to after core stats implementation |
| PR-11 (verify bar permission check docs) | Would require understanding current behavior + potential code change — needs issue |
| PR-16, PR-17, PR-18 (docs + CI) | Nice to have but lower priority than code changes |

---

## Summary

| Metric | Value |
|--------|-------|
| Total PRs selected | 10 |
| Total estimated lines | ~155 lines |
| Files touched | 7 unique files |
| Languages touched | 3 |
| New dependencies | 0 |
| Risk level | All Low |
| Independent merges | All independently mergeable |