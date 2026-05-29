# Repository Map: open-discord-bots/open-ticket

## What The Bot Does

**Open Ticket** is the most advanced self-hosted Discord ticket bot available. It features:
- **350+ configurable settings** covering appearance and behavior
- **HTML transcripts** for ticket conversations
- **Plugin system** with community plugin ecosystem
- **Modal questions & forms** before ticket creation
- **Ticket claiming, pinning, priorities, participants**
- **Autoclose/autodelete** automation
- **Detailed statistics** (50+ metrics)
- **38+ translated languages**
- **Buttons, dropdowns, slash/text commands & modals**
- **Reaction roles and URL buttons**
- **Pterodactyl and Docker deployment support**

## Tech Stack

| Component | Technology |
|-----------|------------|
| Runtime | Node.js v20+ |
| Language | TypeScript (strict: false) |
| Discord Library | discord.js v14.26.4 |
| Framework | @open-discord-bots/framework v1.0.0 |
| Package Manager | npm |
| Database | JSON files (no SQL by default), SQLite via plugin |
| Licensing | GPL-3.0-only |
| Exit Code | ES2024 |

## Source Directory Structure

```
src/
├── index.ts                    # Main entry point (v4.2.0)
├── livestatus.json             # Live status source definitions
├── core/
│   ├── api.ts                  # Exports all Open Ticket API modules
│   ├── main.ts                 # ODOpenTicketMain class definition
│   ├── cli/
│   │   ├── cli.ts              # CLI handler
│   │   └── quickSetup.ts       # Interactive Quick Setup CLI
│   ├── startup/
│   │   ├── manageMigration.ts  # Migration management
│   │   └── migration.ts        # Migration logic
│   ├── api/                    # Ticket system API modules
│   │   ├── ticket.ts
│   │   ├── panel.ts
│   │   ├── question.ts
│   │   ├── option.ts
│   │   ├── role.ts
│   │   ├── priority.ts
│   │   ├── blacklist.ts
│   │   └── transcript.ts
│   └── mappings/              # Core type mappings (33 files)
│       ├── action.ts, base.ts, builder.ts, checker.ts
│       ├── client.ts, component.ts, config.ts, console.ts
│       ├── cooldown.ts, database.ts, event.ts, flag.ts
│       ├── fuse.ts, helpmenu.ts, language.ts, permission.ts
│       ├── plugin.ts, post.ts, progressbar.ts, responder.ts
│       ├── session.ts, startscreen.ts, state.ts, statistic.ts
│       ├── task.ts, verifybar.ts
│       └── (more)
│
├── data/                      # Loader/dispatcher modules
│   ├── framework/             # Open Discord Framework loaders (17 files)
│   │   ├── eventLoader.ts     # Registers 150+ lifecycle events
│   │   ├── commandLoader.ts   # Slash/text/context menu loading
│   │   ├── configLoader.ts    # Config file loading
│   │   ├── databaseLoader.ts  # JSON database loading
│   │   ├── flagLoader.ts      # Flag/option loading
│   │   ├── languageLoader.ts  # i18n language loading
│   │   ├── permissionLoader.ts
│   │   ├── checkerLoader.ts   # Config validation checker
│   │   ├── progressBarLoader.ts
│   │   ├── taskLoader.ts      # Background tasks
│   │   ├── statisticLoader.ts
│   │   ├── stateLoader.ts
│   │   ├── cooldownLoader.ts
│   │   ├── helpMenuLoader.ts
│   │   ├── liveStatusLoader.ts
│   │   ├── startScreenLoader.ts
│   │   └── postLoader.ts
│   └── openticket/            # Open Ticket specific loaders
│       ├── ticketLoader.ts
│       ├── panelLoader.ts
│       ├── questionLoader.ts
│       ├── optionLoader.ts
│       ├── roleLoader.ts
│       ├── priorityLoader.ts
│       ├── blacklistLoader.ts
│       └── transcriptLoader.ts
```

## Key Entry Points

- **`index.ts`**: Main bootstrap. Exports `opendiscord` (ODOpenTicketMain). Handles full startup sequence: events → plugins → flags → configs → databases → languages → commands → client → ready.
- **`src/core/main.ts`**: `ODOpenTicketMain` class — the central bot instance exporting all managers (configs, databases, events, languages, clients, plugins, tickets, etc.)
- **`src/core/api.ts`**: Re-exports the entire public API: all framework + Open Ticket mappings + API modules.

## Config Files (./config/)

| File | Purpose |
|------|---------|
| `general.jsonc` | Bot token, language, prefix, serverId, globalAdmins, status, logs, ticketSystem settings, permissions |
| `options.jsonc` | Button option definitions: ticket, website, role, sub-panel types |
| `panels.jsonc` | Panel messages with buttons/dropdowns that spawn ticket creation |
| `questions.jsonc` | Modal questions presented to users before ticket creation |
| `transcripts.jsonc` | Transcript template/configuration |

## Database Files (./database/)

JSON-based storage: `tickets.json`, `users.json`, `options.json`, `stats.json`, `states.json`, `global.json`

## Package.json Scripts

| Script | Purpose |
|--------|---------|
| `npm start` | `node index.js` — production start |
| `npm run setup` | `node index.js --cli` — Quick Setup CLI |
| `npm run build` | `node index.js --compile-only` — TypeScript compile to dist/ |
| `npm run startnc` | `node index.js --no-compile` — skip compilation |
| `npm test` | Dev-mode with dev-config, dev-database, soft-plugins |
| `npm run testsetup` | CLI + dev config |
| `npm run testnc` | Dev mode, no compile |
| `npm run tools:mergelang` | Merge translations |
| `npm run tools:sponsors` | Generate sponsors |
| `npm run tools:contributors` | Generate contributors |
| `npm run docker:build` | Build Docker image |

## Build/Test/Lint

- **No ESLint/Prettier config found** — project does not use explicit lint/styling tools
- **TypeScript**: tsconfig.json with `strict: false`, `strictNullChecks: true`, `skipLibCheck: true`
- TypeScript output goes to `dist/` directory

## Entry Point Startup Sequence (from index.ts)

1. Load all events (150+ named events)
2. Load error handling
3. Migration management (part 1)
4. Load plugins
5. Load plugin classes
6. Load flags → init flags
7. Load debug/silent mode
8. Load progress bars
9. Load configs → init configs
10. Load databases → init databases
11. Load sessions
12. Load languages → select language
13. Migration part 2
14. Config checker → render → quit if invalid
15. CLI mode (if `--cli` flag) — Quick Setup
16. Client setup (intents, privileges, partials, permissions)
17. Client ready: verify bot in server, permissions, status, priority levels
18. Register slash commands, context menus, text commands
19. Load states, statistics, tasks, livestatus, startscreen
20. Ready for usage

## Risk-Sensitive Areas

### Permission Handling
- `src/core/mappings/permission.ts` — permission resolution logic
- `src/data/framework/permissionLoader.ts` — loads permission configs
- Permissions specified per-command in `config/general.jsonc` (none/everyone/admin/roleId)
- Bot requires: `AddReactions`, `AttachFiles`, `CreatePrivateThreads`, `CreatePublicThreads`, `EmbedLinks`, `ManageChannels`, `ManageGuild`, `ManageMessages`, `ChangeNickname`, `ManageRoles`, `ManageThreads`, `ManageWebhooks`, `MentionEveryone`, `ReadMessageHistory`, `SendMessages`, `SendMessagesInThreads`, `UseApplicationCommands`, `UseExternalEmojis`, `ViewAuditLog`, `ViewChannel`
- Privileged intents: `MessageContent`, `GuildMembers`
- Bot validates it has permissions in main server on startup

### Data Storage
- JSON files in `./database/` — `tickets.json`, `users.json`, `options.json`, `stats.json`, `states.json`, `global.json`
- No encryption — plain JSON
- Tickets store: creator, participants, claims, pins, close state, topic, priority, option-related metadata
- No SQL by default; SQLite available via community plugin

### Channel Creation
- `src/core/api/ticket.ts` — ticket creation API
- `src/data/openticket/ticketLoader.ts` — loads ticket data
- Bot creates text channels in categories when tickets are opened
- Channels get permission overwrites for ticket creator + admins
- Configurable prefix/suffix pattern for channel names
- Backup category support when category exceeds 50 channels

### Ticket System
- `src/core/api/ticket.ts`, `src/core/api/panel.ts`, `src/core/api/option.ts`, `src/core/api/question.ts`
- Modal questions support via `src/core/api/question.ts`
- autoclose/autodelete background tasks
- Claiming, pinning, priority levels, participants, transfers

### Plugin System
- Plugins in `./plugins/` directory
- Example plugin at `./plugins/example-plugin/`
- Plugin `plugin.json` defines: name, id, version, startFile, supportedVersions, enabled, priority, events, npmDependencies, requiredPlugins, incompatiblePlugins
- Plugin API exposes events like `onTicketCreate`, `afterTicketCreated`, etc.
- `declare module "#opendiscord-types"` for TypeScript autocomplete in plugins
- Shared fuses control plugin loading stages

## Safe PR Areas

- **Translations** — `./languages/*.json` — 38+ language files, well-structured JSON
- **Documentation** — README.md already links to external docs at otdocs.dj-dj.be
- **Error messages** — part of language files
- **Config validation** — checkerLoader.ts system
- **Command documentation** — help menu loader
- **Example plugin** — template at `./plugins/example-plugin/`
- **Issue templates** — well-defined templates in `.github/ISSUE_TEMPLATE/`
- **Pterodactyl eggs** — JSON configs in `.github/pterodactyl-eggs/`
- **Tests** — test npm scripts with `--dev-config --dev-database --soft-plugins` flags

## CI/CD / GitHub Workflows

**No GitHub Actions workflows found** in `.github/workflows/` — none defined.

## Issue Templates

Located in `.github/ISSUE_TEMPLATE/`:
- `bug_report.md` — requires otdebug.txt file
- `feature_request.md` — labeled "Concept"
- `documentation.md`
- `updated_language.md`
- `new_language.md`
- `question.md`
- `plugin_request.md`
- `html_transcripts.md`

## Contribution Flow

From `.github/CONTRIBUTING.md`:
- **Translations**: PR to `dev` branch. Copy `english.json`, translate, add to list, mention @DJj123dj for new language codes. Translators get credits in README.md, changelog, docs, and Discord role.
- **Plugins**: PR to `open-discord-plugins` repo or contact DJj123dj
- **Features**: Don't submit PRs for features — only bugs, small fixes, translations, plugins. Feature requests via Discord server, GitHub issues, DM, or email.
- **Bug fixes**: PRs welcome. Security vulnerabilities must be reported privately via DM or email.
- **Pull requests**: Only accepted for bugs, small fixes, translation, and plugins

## Key Dependencies

```
@discordjs/rest: ^2.6.1
@open-discord-bots/framework: ^1.0.0
discord.js: ^14.26.4
typescript: ^6.0.3
terminal-kit: ^3.1.2
ansis: ^4.2.0
formatted-json-stringify: ^1.3.2
@types/node: ^22.5.0
@types/terminal-kit: ^2.5.7
```

## .gitignore

Excludes: `node_modules/`, `package-lock.json`, `.vscode/`, `devconfig/`, `devdatabase/`, `plugins/*` (except example-plugin), `database/openticket.sqlite`, `dist/`, `otdebug.txt`, `.DS_Store`, `.backup/`, `.tools/` (except specific files)

## Version

Current: **v4.2.0** (development)

Supported versions (from SECURITY.md):
- 4.2.x: In Development
- 4.1.3: LTS until September 2026
- 4.1.2, 4.1.1, 4.1.0: Maintenance mode
- 4.0.7 and below: Deprecated

## Documentation

External documentation at: https://otdocs.dj-dj.be
