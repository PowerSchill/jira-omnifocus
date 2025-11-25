# Jira-OmniFocus Sync Plugin - AI Coding Instructions

## Project Overview

This is an **OmniFocus plugin** (not a standalone app) that syncs Jira tickets with OmniFocus tasks. The entire application is contained in a single JavaScript file: `jira.omnifocusjs`.

**Critical Architecture Points:**
- Single-file OmniFocus plugin using the OmniFocus Plug-In API
- No build system, no dependencies, no package.json
- Runs directly in OmniFocus as an action plugin (`.omnifocusjs` extension)
- Uses OmniFocus global objects: `PlugIn`, `Credentials`, `Preferences`, `Tag`, `Task`, `tags`, `flattenedTags`, `inbox`

## Plugin Metadata Block

The file starts with a JSON metadata comment block that OmniFocus reads:
```javascript
/*{
  "type": "action",
  "targets": ["omnifocus"],
  "author": "Mark E. Schill",
  "identifier": "net.cmschill.omnifocus.sync.jira",
  "version": "1.4.1",
  ...
}*/
```

**Version bumping:** When releasing, update BOTH the version in this block AND let release-please handle `CHANGELOG.md`. The `release-please-config.json` specifies `jira.omnifocusjs` as a version file.

## Key Business Logic

**Sync Flow:**
1. Authenticate with Jira using user ID + API key (stored via `Credentials` API)
2. Query Jira using JQL (default: `assignee=currentuser() and resolution is empty`)
3. Fetch OmniFocus tasks tagged with the sync tag (default: "jira")
4. Match by ticket key prefix (e.g., "PROJ-123") at start of task name
5. Create new tasks for unmatched Jira issues
6. Reopen (mark incomplete) existing tasks if still in Jira
7. Complete tasks that no longer appear in Jira (if they have no incomplete subtasks)

**Critical Pattern - Task Identification:**
```javascript
const re = new RegExp(`^[A-Za-z0-9-]+`);
taskId = task.name.match(re)[0]; // Extracts "PROJ-123" from "PROJ-123 Fix login bug"
```
Task names MUST start with the Jira ticket key. Users can edit everything after the key.

**Tag Resolution:**
- Supports absolute tag paths with " : " separator (e.g., "Work : Jira")
- Falls back to flat tag name search
- Creates tag if not found

## OmniFocus Plug-In API Patterns

**Form validation pattern:**
```javascript
inputForm.validate = (formObject) => {
  return formObject.values["field1"] && formObject.values["field2"] ? true : false;
};
```

**Credential reset:** Hold Control key when running to prompt for credential removal.

**Error handling:** Always wrap in try-catch and check for "cancelled" string to avoid showing alert when user cancels form.

## Jira API Integration

- Uses Jira REST API v3 (`/rest/api/3/search/jql`)
- Basic auth: `Data.fromString(userID + ":" + apiKey).toBase64()`
- Pagination: `maxResultsPerPage = 25`, recursive fetching using `startAt` parameter
- Fields requested: `key`, `summary`, `description`

**API Response Handling:**
```javascript
const issues = jsonResponse.values || jsonResponse.issues || []; // Handles both v2/v3 responses
```

## Development Workflow

**No build system:** Edit `jira.omnifocusjs` directly. Test by installing in OmniFocus:
- macOS: `~/Library/Mobile Documents/iCloud~com~omnigroup~OmniFocus/Documents/Plug-Ins/`
- iOS: Install via Files app

**Debugging:** Set `shouldLogToConsole = true` at top of file. View logs in Console.app (macOS) filtering for OmniFocus.

**Release process:**
1. Make changes to `jira.omnifocusjs`
2. Commit with conventional commit messages (e.g., `fix:`, `feat:`)
3. Release-please will create PR updating version in metadata block and CHANGELOG.md
4. Merge PR to trigger release

## Code Conventions

- Use `async/await` for all OmniFocus API calls (forms, alerts)
- Check `Device.iPad || Device.iOS` for platform-specific UI adjustments
- Disable autocapitalization for credential fields on mobile
- Use OmniFocus API directly - no external libraries or frameworks

## Common Modifications

**Change default Jira query:** Modify the default in line ~80:
```javascript
jiraQuery = preferences.readString("jiraQuery") || "YOUR_QUERY_HERE";
```

**Add new Jira fields:** Update `jiraFields` array and modify task note creation:
```javascript
const jiraFields = ["key", "summary", "description", "customfield_10001"];
```

**Modify task completion logic:** Currently tasks with incomplete subtasks won't auto-complete. Logic is in the "Close Tasks" section (~line 250).

## Repository Info

- Original author: Bastian-Kuhn (acknowledged in README)
- Current maintainer: Mark E. Schill (PowerSchill on GitHub)
- License: GNU GPL (see LICENSE.txt)
- Repo: https://github.com/PowerSchill/jira-omnifocus
