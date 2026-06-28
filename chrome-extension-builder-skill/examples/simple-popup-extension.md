# Example: Simple Popup Extension

Use this example when the user wants a small toolbar popup with quick controls and no webpage DOM interaction.

Recommended architecture:

- Manifest V3
- Popup UI
- `chrome.storage.local` for saved state if needed
- No content script unless page interaction is added
- Background service worker only if coordination or events are needed

Recommended permissions:

- `storage` only if saving data
