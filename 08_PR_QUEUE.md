# PR Queue — open-discord-bots/open-ticket

## Active PRs to Open

| Order | Branch | Title | Description | Target | Labels |
|-------|--------|-------|-------------|--------|--------|
| 1 | `pr/japanese-url-invalid-translation` | Fix incomplete Japanese translations for urlInvalidHttp/urlInvalidProtocol | Translate two missing keys in japanese.json | dev | translation |
| 2 | `pr/korean-url-invalid-translation` | Fix incomplete Korean translations for urlInvalidHttp/urlInvalidProtocol | Translate two missing keys in korean.json | dev | translation |
| 3 | `pr/simplified-chinese-url-invalid-translation` | Fix incomplete Simplified Chinese translations for urlInvalidHttp/urlInvalidProtocol | Translate two missing keys in simplified-chinese.json | dev | translation |
| 4 | `pr/server-id-error-improvement` | Improve "Server Id Missing" error message with config path | Add config file and setting name to error message | dev | bug,dx |
| 5 | `pr/backup-language-error-improvement` | Improve "Unknown backup language" error message | Explain what fallbackLanguage is and which config file | dev | bug,dx |
| 6 | `pr/panel-dropdown-error-improvement` | Improve panel dropdown mismatch error message | Include the invalid option ID in the error | dev | bug,dx |
| 7 | `pr/panel-button-row-length-validation` | Add minimum validation for panelButtonRowLength | Validate panelButtonRowLength >= 1 in checkerLoader | dev | bug,configuration |
| 8 | `pr/todo-cleanup-component-loading` | Clean up TODO placeholders in component loading sequence | Remove or document the three //TODO!! blocks in index.ts | dev | cleanup |
| 9 | `pr/topic-list-subcommand` | Implement /topic list subcommand | Add topic list command with filter options | dev | feature |
| 10 | `pr/priority-list-subcommand` | Implement /priority list subcommand | Add priority list command with filter options | dev | feature |

---

## PR Template

```markdown
## About
[Brief description of what this PR does]

## Changes
- [List of specific changes made]

## Testing
- [ ] Build succeeds (`npm run build`)
- [ ] [Specific test for this change]

## Additional Notes
[Any relevant context — linked issues, rationale, etc.]
```

---

## Merge Strategy

1. Open PRs in order 1-3 (translation fixes) — small, low risk, easy review
2. Open PRs in order 4-8 (error messages + validation) — all independent, can review in parallel
3. Open PRs 9-10 (topic/priority list) — same file, different sections, can merge in either order

All PRs target `dev` branch per contribution guidelines.

---

## Completed PRs (logged for reference)

| PR | Status | Notes |
|----|--------|-------|
| — | — | No PRs completed yet in this session |

---

## Summary

| Metric | Value |
|--------|-------|
| PRs queued | 10 |
| Target branch | dev |
| Estimated total lines | ~155 |
| Dependencies between PRs | None — all independent |