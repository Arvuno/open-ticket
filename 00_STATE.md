# Repository State

**Repository**: open-discord-bots/open-ticket
**Local path**: /root/star-first-1/repos/open-ticket
**Fork**: Arvuno/open-ticket
**Current version**: v4.2.0

## Research Status

- [x] README.md — fully read
- [x] CONTRIBUTING.md — fully read (.github/CONTRIBUTING.md)
- [x] LICENSE.md — fully read (GPL-3.0)
- [x] .github/ directory — all files enumerated
- [x] .github/ISSUE_TEMPLATE/ — all 8 templates enumerated
- [x] src/ directory — all files enumerated (truncated, but structure understood)
- [x] package.json — fully read
- [x] tsconfig.json — fully read
- [x] config/ directory — all config files read (general.jsonc, options.jsonc)
- [x] .gitignore — fully read
- [x] plugins/example-plugin/ — fully read
- [x] index.ts — fully read (968 lines)
- [x] eventLoader.ts — fully read (309 lines)
- [x] api.ts — fully read (42 lines)

## Research Notes

- No ESLint or Prettier config found
- No .env.example file found
- No docs/ directory found (documentation is external)
- No GitHub Actions workflows defined
- CONTRIBUTING.md is located at `.github/CONTRIBUTING.md` (not repo root)
- LICENSE is at `LICENSE.md` (GPL-3.0-only)
- SECURITY.md exists with version support matrix
- CODE_OF_CONDUCT.md is Contributor Covenant v2.0

## Files Created

- `/root/star-first-1/repos/open-ticket/01_REPO_MAP.md` — comprehensive repository map
- `/root/star-first-1/repos/open-ticket/00_STATE.md` — this file

## Key Entry Points Identified

| File | Purpose |
|------|---------|
| `src/index.ts` | Main bootstrap, exports `opendiscord` |
| `src/core/main.ts` | ODOpenTicketMain class |
| `src/core/api.ts` | Full public API re-export |
| `src/data/framework/eventLoader.ts` | 150+ lifecycle events registration |
| `src/data/framework/commandLoader.ts` | Slash/text/context command loading |
| `src/data/openticket/ticketLoader.ts` | Ticket entity loading |

## Config Structure

- `config/general.jsonc` — main bot config (token, language, permissions, ticketSystem)
- `config/options.jsonc` — button option definitions (ticket/website/role/sub-panel)
- `config/panels.jsonc` — panel messages with buttons
- `config/questions.jsonc` — modal question definitions
- `config/transcripts.jsonc` — transcript config

## Database Structure

JSON files: `tickets.json`, `users.json`, `options.json`, `stats.json`, `states.json`, `global.json`

## Startup Sequence Summary

1. Events → 2. Error handling → 3. Migration (part 1) → 4. Plugins → 5. Flags → 
6. Progress bars → 7. Configs → 8. Databases → 9. Languages → 10. Migration (part 2) →
11. Config checker → 12. CLI mode (if --cli) → 13. Client configure → 14. Client ready →
15. Slashcmds/Contextmenus/Textcmds → 16. States/Statistics/Tasks → 17. Startscreens → 18. Ready

## Risk Areas

- **Channel creation**: Ticket channels created dynamically with permission overwrites
- **Permission resolution**: Complex permission system with per-command granularity
- **Data storage**: Plain JSON with no encryption
- **Plugin execution**: Plugins run arbitrary code with bot access
- **Transcripts**: HTML generation from message content (XSS potential if user content not escaped)
- **Evaluations**: Fuses system gates major functionality on boolean flags

## Known Fuse Flags (from index.ts)

`pluginLoading`, `pluginClassLoading`, `flagLoading`, `flagInitiating`, `debugLoading`, `silentLoading`, `progressBarRendererLoading`, `progressBarLoading`, `configLoading`, `configInitiating`, `emojiTitleStyleLoading`, `databaseLoading`, `databaseInitiating`, `sessionLoading`, `languageLoading`, `languageInitiating`, `languageSelection`, `checkerLoading`, `checkerFunctionLoading`, `checkerExecution`, `checkerTranslationLoading`, `checkerRendering`, `checkerQuit`, `clientLoading`, `clientReady`, `clientMultiGuildWarning`, `clientActivityLoading`, `clientActivityInitiating`, `priorityLoading`, `slashCommandLoading`, `forceSlashCommandRegistration`, `slashCommandRegistering`, `allowSlashCommandRemoval`, `contextMenuLoading`, `forceContextMenuRegistration`, `contextMenuRegistering`, `allowContextMenuRemoval`, `allowDumpCommand`, `textCommandLoading`, `statisticInitiating`, `taskLoading`, `taskExecution`, `liveStatusLoading`, `startScreenLoading`, `startScreenRendering`

## Plugin System

- Plugin directory: `./plugins/`
- Example plugin: `./plugins/example-plugin/` with `index.ts`, `plugin.json`, `config.json`, `README.md`
- Plugin registration via `onConfigLoad` event
- TypeScript support via `declare module "#opendiscord-types"`
- Events: `onTicketCreate`, `afterTicketCreated`, `afterPluginBeforeClientLoaded`, etc.
