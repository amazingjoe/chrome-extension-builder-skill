# Example: Side Panel Notes Extension

Use this example when the user wants a persistent notes, clipping, or prompt-building workspace.

Recommended architecture:

- Manifest V3
- Popup for quick controls
- Content script for selected text/page content
- Background service worker for coordination
- Side panel for persistent workspace
- `chrome.storage.local` for saved captures

Recommended permissions:

- `storage`
- `activeTab`
- `scripting`
- `sidePanel`
- `clipboardWrite` only if copying to clipboard is required
