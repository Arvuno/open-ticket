# Issue Triage Report — open-discord-bots/open-ticket

## Open Issues

| Issue | Type | Clarity | Repro Available | Existing PR? | Estimated Size | Risk | Selected? | Notes |
|-------|------|---------|-----------------|--------------|---------------|------|-----------|-------|
| #198 | Feature (Plugin, Minor Feature, Framework) | High | No | No | Medium (requires framework changes + SQLite integration) | Medium — touches core database layer | No | Built-in SQLite in core framework. Requires `@open-discord-bots/framework` updates. Not a candidate for quick PR — needs design review. |
| #161 | Major Feature / Framework | Medium | No | No | Large (significant architectural refactor to worker splitting) | High — major breaking changes to plugin worker system | No | Improved Plugin Worker Splitting. Mentions breaking changes. Labeled Framework + Major Feature. Out of scope for quick PRs per contribution guidelines. |

---

## Recently Closed Issues (2026)

| Issue | Type | Severity | Resolution | Key Findings |
|-------|------|----------|------------|--------------|
| #207 | Bug (Minor Bug) | Low | Fixed | Backup category doesn't work — category threshold logic (+/- 50 channels) for falling back when main category is full was broken. |
| #205 | Bug (HTML Transcripts) | High | External | HTML Transcript compile failure when fetch() fails (network/transcript service unavailable). Canceled ticket deletion when transcript system malfunctions. |
| #200 | Feature (Minor Feature) | — | Implemented | Minor Improvements list: auto-sync version/API, quick setup CLI additions, events for priority/topic, `/topic list`, `/priority list` subcommands, remote ticket opening (`/ticket <user>`), blacklist button, ephemeral replies for rename/move/transfer, security hiding for transcripts, permission check for verify bar, autoclose/autodelete stats, panelButtonRowLength setting, closed tickets not counting toward limit. |
| #199 | Feature (Minor Feature) | — | Implemented | Closed Ticket channel emojis (e.g. 🔐) — emoji appears before ticket name when closed, works with priority/pin emojis, disappears on reopen. |
| #197 | Feature (Minor Feature) | — | Implemented | .jsonc support (JSON with comments) for all config files via parser + `formatted-json-stringify` updates. |
| #196 | Major Feature | — | Implemented | Transfer Open Ticket API core to `@open-discord-bots/framework` package — moved shared code to npm package for use across multiple bots. |
| #195 | Feature (Minor Feature, Framework) | — | Implemented | Auto-delete "Responder Timeout" error messages after 3-4 seconds for text commands (slash/button/dropdown already use ephemeral). |
| #192 | Feature (Concept) | — | Won't Fix | Redirect option type for channel IDs — rejected in favor of other approaches. |
| #191 | Feature (Concept) | — | Won't Fix | DM message on ticket close/delete separately — marked as concept, not implemented. |
| #188 | Bug (HTML Transcripts) | Low | Fixed | Custom emojis in buttons not showing in HTML transcripts. |
| #186 | Question (HTML Transcripts) | — | Answered | Self-hosting transcript website question — answered, not a bug. |
| #181 | Bug (Minor Bug) | Medium | Fixed | Unable to register permission for global admin when role ID not found. |

---

## Triage Summary

- **Total open:** 2 (both are Framework-level, one Major Feature, one Minor Feature/Plugin)
- **Both open issues require framework changes** — not standalone quick PR candidates
- **Recently closed (2026):** 11 issues — strong signal for documentation, error handling, and minor feature work
- **Key patterns from closed bugs:** backup category logic, transcript failure handling, permission registration edge cases, HTML transcript emoji rendering

## Recommendations

1. **Focus PR candidates on:** documentation improvements, error message clarity, config validation, translation consistency, HTML transcript edge cases, statistic implementation gaps
2. **Avoid:** Framework-level changes (#198, #161) — these need dedicated design and review
3. **High-value quick wins:** Response-time/resolution-time stats (TODO in statisticLoader.ts), topic list / priority list commands (TODO in commandLoader.ts), verify bar permission check mentioned in #200