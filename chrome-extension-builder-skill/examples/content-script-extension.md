# Example: Content Script Extension

Use this example when the user wants to read or modify the current webpage.

Recommended architecture:

- Manifest V3
- Popup or context menu for user trigger
- Content script for webpage DOM access
- Background service worker for coordination
- Message passing between popup/background/content script

Recommended permissions:

- `activeTab` for user-triggered access
- `scripting` if injecting scripts programmatically
- Specific host permissions if automatic site behavior is required
