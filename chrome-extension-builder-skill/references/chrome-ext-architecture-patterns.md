# Chrome Extension Architecture Patterns

Use this file to decide the shape of a Chrome Extension when the user describes a feature.

## Pattern 1: Quick Popup Tool

Use when the user wants quick toolbar controls.

Flow:

```txt
popup → background service worker → storage or active tab
```

Use for:

- Toggle settings
- Run one action on the current tab
- Copy data
- Open a dashboard or side panel

Do not use a popup for persistent work because popups close when they lose focus.

## Pattern 2: Webpage Reader or Modifier

Use when the extension needs to read or modify webpage content.

Flow:

```txt
popup/side panel → background service worker → content script → webpage DOM
```

Use for:

- Reading selected text
- Highlighting text
- Adding overlays
- Scraping visible content
- Watching DOM changes

The content script handles the page DOM. The background service worker coordinates.

## Pattern 3: Persistent Notes or Research Workspace

Use when captured data should accumulate while the user browses.

Flow:

```txt
content script → background service worker → chrome.storage.local → side panel or extension page
```

Use for:

- Web clipping
- Prompt building
- Research notes
- AI reading assistants
- Highlight collections

Default UI: side panel.

## Pattern 4: Right-Click Capture

Use when the user wants to act on selected text from the context menu.

Flow:

```txt
context menu → background service worker → content script or selection info → storage
```

Use for:

- Save selected text
- Summarize selected text
- Add selected text to notes
- Send selection to AI

Requires `contextMenus`. May require `activeTab`, `scripting`, and/or host permissions depending on behavior.

## Pattern 5: Keyboard Shortcut Workflow

Use when the user wants a hotkey.

Flow:

```txt
commands → background service worker → active tab/content script/storage
```

Use for:

- Capture selection
- Toggle mode
- Open side panel
- Copy all notes

Requires `commands` in `manifest.json`.

## Pattern 6: AI Extension

Use when the extension sends content to an AI model or external API.

Preferred flow with private key:

```txt
popup/side panel → background service worker → backend proxy → AI API
```

Local-only flow if user accepts API key exposure risk:

```txt
options page stores API key → background service worker → AI API
```

Always explain the tradeoff if an API key is stored locally in the extension.

## Pattern 7: Site-Specific Automation

Use when the extension should run automatically on a specific website.

Flow:

```txt
content script matches target host → webpage DOM → background service worker/storage
```

Use specific host permissions instead of `<all_urls>`.

## Pattern 8: Full Dashboard Extension

Use when the extension needs a larger interface than a popup or side panel.

Flow:

```txt
extension page/new tab → chrome.storage.local/sync → background service worker as needed
```

Use for:

- Dashboards
- Searchable archives
- Settings-heavy tools
- Export/import workflows
