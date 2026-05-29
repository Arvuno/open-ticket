# Quality Audit — open-discord-bots/open-ticket

## Findings Overview

| Category | Count | Risk Level |
|----------|-------|------------|
| Missing input validation | 4 | Medium |
| Unclear error messages | 3 | Low |
| Missing documentation | 5 | Low |
| Stale links in docs | 1 | Low |
| Plugin system documentation gaps | 2 | Medium |
| Missing error handling in event handlers | 2 | Medium |
| Translation/i18n gaps | 3 | Low |
| TODOs in code | 10 | Low |
| HTML transcript potential XSS (audit only) | 1 | High (audit only, not to be fixed without explicit issue) |

---

## 1. Missing Input Validation

### 1.1 Verify Button ID length not validated in all paths
**File:** `src/builders/buttons.ts:17`
```typescript
if (params.verifyButtonId.length > 40) throw new api.ODSystemError(...)
```
Only checked in one path. Other button builders may not validate length consistently.

### 1.2 Panel button row length not validated
**Reference:** Issue #200 mentions `panelButtonRowLength` setting but no validation seen in config loader for button row length limits (Discord max: 5 buttons per row).

### 1.3 Option name max length discrepancy
**Reference:** Issue #151 (fixed) — option name was validated to 50 chars in checker but Discord modal title restricts to 45 chars. Checker may have been updated but consistency across all option types should be verified.

### 1.4 No validation for transcript compiler init failure recovery
**File:** `src/actions/createTranscript.ts:25-67`
Transcript init can fail (network, service unavailable) but no user-facing error message is generated for the ticket creator. The system only logs and cancels deletion.

---

## 2. Unclear Error Messages

### 2.1 "Unknown backup language" error
**File:** `src/index.ts:223`
```typescript
throw new api.ODSystemError("Unknown backup language '"+backupLanguageId+"'!")
```
Error is cryptic — user has no idea what backup language means or which config controls it.

### 2.2 "Server Id Missing" error
**File:** `src/index.ts:370`
```typescript
if (!serverId) throw new api.ODSystemError("Server Id Missing!")
```
Does not indicate which config file or setting is missing or how to fix it.

### 2.3 Generic "Unable to create panel dropdown" error
**File:** `src/builders/dropdowns.ts:58`
```typescript
throw new api.ODSystemError("Unable to create panel dropdown with options that don't match: ticket, role, sub-panel!")
```
Does not explain what the actual mismatch was or which option caused it.

---

## 3. Missing Documentation

### 3.1 Quick Setup CLI coverage gaps
**Reference:** Issue #200 — "add more quick setup CLI questions for all new properties of v4.1.0 & v4.2.0". No documentation on what questions are currently asked vs. what should be asked.

### 3.2 Plugin system — event lifecycle not documented
**Reference:** `./plugins/example-plugin/` — plugin API exposes events like `onTicketCreate`, `afterTicketCreated` but no documentation on when each fires, in what order, or what data is available.

### 3.3 Permissions documentation — verify bar permission check missing
**Reference:** Issue #200 mentions "Add a permission check before accessing the button verify bar. It's currently very confusing for users" — no documentation on current behavior or intended behavior.

### 3.4 Ticket limit — "closed tickets not counting" setting not documented
**Reference:** Issue #200 mentions "Add the ability to make closed tickets not count towards the ticket limit" — no docs on current limit behavior or new setting.

### 3.5 Remote ticket opening (`/ticket <user>`) not documented
**Reference:** Issue #200 — remote ticket opening via `/ticket <user>` mentioned but no docs on syntax, permissions required, or audit trail.

---

## 4. Stale Links in Documentation

### 4.1 External docs link may be outdated
**Reference:** README.md links to `https://otdocs.dj-dj.be` — no verification that this URL returns correct content, or that it covers v4.2.0 features. The link is referenced as the canonical docs location but no link-check CI exists.

---

## 5. Plugin System Documentation Gaps

### 5.1 Plugin.json schema not validated at load time
**Reference:** `./plugins/example-plugin/plugin.json` — multiple authors, versions, npmDependencies, requiredPlugins, incompatiblePlugins fields exist but no schema validation produces clear errors if malformed.

### 5.2 Plugin loading stages not documented
**Reference:** `src/index.ts:660-771` — `sharedComponentsLoading`, `messageComponentsLoading`, `modalComponentsLoading` fuse points are placeholders (`//TODO!!`). Plugin developers have no guidance on what hooks are available during each stage.

---

## 6. Missing Error Handling in Event Handlers

### 6.1 Event handler errors may silently fail
**Reference:** `src/index.ts:516`
```typescript
await opendiscord.client.login().catch((reason) => process.emit("uncaughtException",new api.ODSystemError(reason)))
```
Many event handlers use `.catch()` that only logs — no retry logic, no dead-letter queue for failed actions.

### 6.2 Ticket action race conditions
**Reference:** `src/actions/utilities.ts:163-164` — `replyMessageMustBeSentBeforeClose()` checks `hasPerms` but doesn't handle the case where permissions change between check and actual close operation.

---

## 7. Translation/i18n Gaps

### 7.1 Missing language keys for new stats
**Reference:** `src/data/framework/statisticLoader.ts:139-140`
```typescript
//TODO: opendiscord:response-time --> lang.getTranslation("stats.properties.responseTime")
//TODO: opendiscord:resolution-time --> lang.getTranslation("stats.properties.resolutionTime")
```
Translation keys `stats.properties.responseTime` and `stats.properties.resolutionTime` don't exist in any language file. These stats are documented in issue #200 but not implemented.

### 7.2 Japanese, Korean, Simplified Chinese translations appear incomplete
**Reference:** Language files show `urlInvalidHttp` and `urlInvalidProtocol` messages with English text instead of translated text in some files (e.g., japanese.json, korean.json, simplified-chinese.json). These appear to have been copied from english.json without translation.

### 7.3 Autoclose/autodelete stat labels not in language files
**Reference:** Issue #200 — `opendiscord:autoclose` and `opendiscord:autodelete` stats mentioned but no corresponding keys in `stats.properties` section of any language file.

---

## 8. TODOs in Code

### 8.1 Component loading TODOs (index.ts:660, 668, 771)
Three placeholder `//TODO!!` blocks in the startup sequence for shared components, message components, and another stage. These are non-trivial loading hooks that appear incomplete.

### 8.2 Command loader TODOs (commandLoader.ts:576, 613, 1160, 1178)
- `/topic list` subcommand — TODO (v4.2)
- `/priority list` subcommand — TODO (v4.2)
- `//TODO: topic list (v4.2)` and `//TODO: priority list (v4.2)`

These are explicitly marked for v4.2 but not yet implemented.

### 8.3 Statistic loader TODOs (statisticLoader.ts:139-140)
`opendiscord:response-time` and `opendiscord:resolution-time` stats referenced in issue #200 but not implemented.

### 8.4 Verifybar modifier TODO (verifybarModifiers.ts:27)
```typescript
//    //TODO
```
Single commented TODO in verifybarModifiers with no context.

---

## 9. HTML Transcript XSS Audit (OBSERVATION ONLY — DO NOT FIX WITHOUT EXPLICIT ISSUE)

### 9.1 Potential for unescaped user content in transcript HTML
**Reference:** `src/core/api/transcript.ts` — transcripts compile user messages ( usernames, message content, channel names) into HTML. If any of these are rendered without proper sanitization, XSS is possible. **This is an audit observation only — do not modify without an explicit security issue filed.**

---

## Summary of Quality Findings

| Priority | Finding | Recommendation |
|----------|---------|----------------|
| High | HTML Transcript XSS potential | Audit only — wait for explicit security issue |
| Medium | Missing panel button row length validation | Add config validation in checkerLoader |
| Medium | Plugin loading stages undocumented | Add plugin dev docs or enhance example plugin |
| Medium | Event handler error recovery missing | Add retry/queuing for failed actions |
| Low | 10 TODOs in code | Feature-complete cleanup for v4.2 |
| Low | 3 language files with untranslated URL error messages | Complete translations for japanese, korean, simplified-chinese |
| Low | Missing stats translation keys | Add `responseTime` and `resolutionTime` to all language files |
| Low | Stale external docs link | Add link-check to CI |
| Low | Cryptic error messages | Improve error messages with config path hints |