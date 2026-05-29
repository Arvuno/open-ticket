# Branch Queue — open-discord-bots/open-ticket

## Branch Naming Convention
`pr/candidate-id-short-description`

---

## Branch Queue (in merge order)

| # | Branch Name | Target | PR | Depends On | Status |
|---|-------------|--------|----|-----------|--------|
| 1 | `pr/japanese-url-invalid-translation` | dev | PR-05 | none | Ready |
| 2 | `pr/korean-url-invalid-translation` | dev | PR-06 | none | Ready |
| 3 | `pr/simplified-chinese-url-invalid-translation` | dev | PR-07 | none | Ready |
| 4 | `pr/server-id-error-improvement` | dev | PR-03 | none | Ready |
| 5 | `pr/backup-language-error-improvement` | dev | PR-04 | none | Ready |
| 6 | `pr/panel-dropdown-error-improvement` | dev | PR-10 | none | Ready |
| 7 | `pr/panel-button-row-length-validation` | dev | PR-08 | none | Ready |
| 8 | `pr/todo-cleanup-component-loading` | dev | PR-09 | none | Ready |
| 9 | `pr/topic-list-subcommand` | dev | PR-01 | none | Ready |
| 10 | `pr/priority-list-subcommand` | dev | PR-02 | none | Ready |

---

## Notes

- All branches target `dev` branch (per contribution guidelines: translations PR to dev)
- Branches 1-3 (translation fixes) can be merged in any order
- Branches 4-8 (error messages + validation) are independent
- Branches 9-10 (topic/priority list) can be merged in either order since they touch different sections of commandLoader.ts
- No branch dependencies — all are independently mergeable

---

## Deferred Branches (Not Yet Queued)

| Branch Name | PR | Reason Deferred |
|-------------|-----|----------------|
| `pr/response-resolution-time-stats` | PR-01 | Too large combined; split into statLoader only (~150 lines) as alternative |
| `pr/autoclose-autodelete-stat-translations` | PR-10 | Defer until after core stats implementation |
| `pr/verify-bar-permission-docs` | PR-11 | Needs issue before implementation |
| `pr/remote-ticket-docs` | PR-16 | Documentation only, lower priority |
| `pr/docs-link-check-ci` | PR-17 | CI addition, needs workflow review |