# Chrome Extension Defaults

These are the default assumptions and best practices to use when building Chrome Extensions unless the user specifies otherwise.

## 1. Default Architecture

Build all Chrome extensions using **Manifest V3**.

Default extension structure:

```txt
extension-name/
  manifest.json
  src/
    background/
      service-worker.js
    content/
      content.js
      content.css
    popup/
      popup.html
      popup.js
      popup.css
    options/
      options.html
      options.js
      options.css
    sidepanel/
      sidepanel.html
      sidepanel.js
      sidepanel.css
    shared/
      constants.js
      storage.js
      messaging.js
      utils.js
  assets/
    icons/
      icon16.png
      icon32.png
      icon48.png
      icon128.png
  README.md
```

Only create files that are needed for the requested feature. Do not create unnecessary screens or architecture.

## 2. Default Extension Parts

Use these defaults when the user does not specify otherwise:

- Use a **popup** for quick controls.
- Use a **content script** for reading or modifying the current webpage.
- Use a **background service worker** for extension-wide coordination.
- Use an **options page** for settings.
- Use a **side panel** for persistent workflows, notes, AI chat, or dashboards.
- Use a full **extension page** or new tab for larger interfaces.

Do not put all logic into one file unless the extension is extremely small.

## 3. Manifest Defaults

Always create a valid `manifest.json`.

Default manifest style:

```json
{
  "manifest_version": 3,
  "name": "Extension Name",
  "version": "0.1.0",
  "description": "A short description of what the extension does.",
  "action": {
    "default_title": "Extension Name",
    "default_popup": "src/popup/popup.html"
  },
  "background": {
    "service_worker": "src/background/service-worker.js",
    "type": "module"
  },
  "permissions": [
    "storage"
  ]
}
```

Add permissions only when required by the requested feature. Add icon declarations only when the referenced icon files actually exist.

## 4. Icon Defaults

Chrome extensions often fail or show warnings when `manifest.json` references icon files that do not exist. When creating an unpacked extension, every icon path declared in the manifest must point to a real file in the extension folder.

Default icon folder when icons are included:

```txt
assets/
  icons/
    icon16.png
    icon32.png
    icon48.png
    icon128.png
```

Default manifest icon declaration when the files exist:

```json
{
  "icons": {
    "16": "assets/icons/icon16.png",
    "32": "assets/icons/icon32.png",
    "48": "assets/icons/icon48.png",
    "128": "assets/icons/icon128.png"
  },
  "action": {
    "default_title": "Extension Name",
    "default_popup": "src/popup/popup.html",
    "default_icon": {
      "16": "assets/icons/icon16.png",
      "32": "assets/icons/icon32.png",
      "48": "assets/icons/icon48.png",
      "128": "assets/icons/icon128.png"
    }
  }
}
```

If actual icon artwork is not available, choose one of these defaults.

### Preferred default for code-only generation

Omit icon declarations from `manifest.json`. This avoids broken paths and lets the extension load successfully without icons.

```json
{
  "action": {
    "default_title": "Extension Name",
    "default_popup": "src/popup/popup.html"
  }
}
```

### Preferred default when the coding environment can create image files

Generate simple placeholder PNG icons in all required sizes:

- `icon16.png`
- `icon32.png`
- `icon48.png`
- `icon128.png`

The placeholder icons can be simple colored squares with one or two letters from the extension name. PNG is the safest default for extension icons. Do not use SVG files for extension icons unless the target browser and manifest usage are confirmed to support them for that icon field.

### Important rule

Never include this in the manifest unless the files actually exist:

```json
"icons": {
  "16": "assets/icons/icon16.png",
  "32": "assets/icons/icon32.png",
  "48": "assets/icons/icon48.png",
  "128": "assets/icons/icon128.png"
}
```

If the user wants a production-ready extension, ask whether they already have icon assets. If not, create temporary placeholder PNGs and clearly label them as replaceable.

## 5. Permissions Defaults

Use the minimum permissions necessary.

Preferred permissions:

- `storage` for saved settings and local data.
- `activeTab` when the extension only needs access after a user action.
- `scripting` when injecting scripts or CSS programmatically.
- `contextMenus` when adding right-click menu actions.
- `sidePanel` when using the Chrome side panel.
- `tabs` only when tab querying or tab management is truly needed.
- `alarms` for scheduled tasks.
- `notifications` only when user-visible notifications are required.
- `clipboardWrite` only when copying to clipboard is required.

Avoid broad host permissions such as `<all_urls>` unless the user clearly needs the extension to run on every site.

Prefer specific host permissions:

```json
"host_permissions": [
  "https://example.com/*"
]
```

over broad host permissions:

```json
"host_permissions": [
  "<all_urls>"
]
```

When unsure, ask whether the extension should run on all websites or only specific domains.

## 6. Content Script Rules

Use content scripts for anything involving the current webpage's DOM.

Content scripts are appropriate for:

- Reading selected text.
- Highlighting text.
- Adding overlays or buttons to a webpage.
- Scraping visible page content.
- Watching DOM changes.
- Injecting page-level UI.
- Sending page content back to the extension.

Do not try to access webpage DOM directly from the popup or background service worker.

Default content script pattern:

```js
chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  if (message.type === "GET_SELECTION") {
    const selectedText = window.getSelection().toString();
    sendResponse({ selectedText });
  }
});
```

## 7. Background Service Worker Rules

Use the background service worker for event handling and coordination.

The service worker should handle:

- Install/update events.
- Context menu setup.
- Communication between popup, content scripts, side panel, and extension pages.
- Calls to privileged Chrome APIs.
- Opening tabs or side panels.
- Managing extension-level state.
- Coordinating API calls when appropriate.

Important: Manifest V3 service workers are not persistent. Do not assume background state stays in memory forever.

Persist important data in `chrome.storage`.

## 8. Messaging Defaults

Use message passing between extension contexts.

Use:

- `chrome.runtime.sendMessage` for extension-wide messages.
- `chrome.tabs.sendMessage` to message a content script in a specific tab.
- Shared message type constants when the project has more than a few messages.

Default pattern:

```js
// popup.js
const [tab] = await chrome.tabs.query({ active: true, currentWindow: true });

const response = await chrome.tabs.sendMessage(tab.id, {
  type: "GET_SELECTION"
});
```

```js
// content.js
chrome.runtime.onMessage.addListener((message, sender, sendResponse) => {
  if (message.type === "GET_SELECTION") {
    sendResponse({
      selectedText: window.getSelection().toString()
    });
  }
});
```

Use clear message names like:

```txt
GET_SELECTION
SAVE_NOTE
OPEN_SIDE_PANEL
TOGGLE_HIGHLIGHT_MODE
COPY_ALL_NOTES
```

Do not use vague message names like `DO_THING` or `ACTION`.

## 9. Storage Defaults

Use `chrome.storage.local` by default.

Use `chrome.storage.local` for:

- Captured notes.
- User-created data.
- Extension state.
- Logs.
- Larger local datasets.

Use `chrome.storage.sync` only for small preferences that should sync across browsers, such as:

- Theme.
- Default mode.
- User settings.
- Toggle preferences.

Create a small storage wrapper when the project has repeated storage operations.

Example:

```js
export async function getStoredValue(key, fallbackValue = null) {
  const result = await chrome.storage.local.get(key);
  return result[key] ?? fallbackValue;
}

export async function setStoredValue(key, value) {
  await chrome.storage.local.set({ [key]: value });
}
```

## 10. UI Defaults

Use plain HTML, CSS, and JavaScript unless the user asks for a framework.

Default UI approach:

- Keep popup UI small and fast.
- Use side panel or extension page for larger workflows.
- Avoid heavy frameworks unless the UI genuinely needs them.
- Do not use React unless requested or clearly beneficial.
- Use accessible buttons, labels, and keyboard-friendly interactions.
- Include empty states, success states, and error states.

For popup UI:

- Keep it narrow and simple.
- Use buttons and toggles.
- Avoid long forms.
- Do not rely on popup state staying alive after it closes.

For side panel UI:

- Use it for persistent workflows.
- Good for notes, AI assistants, page review, research capture, and dashboards.

## 11. Security Defaults

Follow secure extension practices.

Do:

- Use Manifest V3.
- Request minimal permissions.
- Use HTTPS for external API calls.
- Keep scripts in local files.
- Validate and sanitize user-generated content before inserting into HTML.
- Use `textContent` instead of `innerHTML` unless HTML rendering is required.
- Store secrets carefully.
- Keep API keys out of committed source code.

Do not:

- Use inline JavaScript in HTML.
- Use remote hosted JavaScript.
- Use `eval`.
- Use `new Function`.
- Inject unsanitized HTML.
- Ask for `<all_urls>` unless truly needed.
- Put sensitive API keys directly in frontend code unless the user accepts that risk.

Prefer this:

```js
element.textContent = userProvidedText;
```

Avoid this:

```js
element.innerHTML = userProvidedText;
```

## 12. API Call Defaults

If the extension calls an external API:

- Use `fetch`.
- Use HTTPS only.
- Handle loading, success, and error states.
- Do not expose API keys unless the user explicitly wants a local-only extension and understands the tradeoff.
- Prefer a backend proxy for private API keys.
- Add clear comments explaining where the user should place their API key or backend URL.

Default external API design:

```txt
Extension UI → background service worker → external API
```

Use the background service worker for API coordination when it keeps the architecture cleaner.

## 13. Clipboard Defaults

If copying text to clipboard:

- Prefer the Clipboard API from visible extension pages.
- Use `clipboardWrite` permission only when needed.
- Provide a visible success message after copying.
- Provide a fallback if clipboard access fails.

Example:

```js
await navigator.clipboard.writeText(textToCopy);
```

## 14. Context Menu Defaults

Use context menus when the user wants right-click workflows.

Examples:

- Save selected text.
- Summarize selected text.
- Add selection to notes.
- Send selection to side panel.
- Mark selected text as important.

Context menus should usually be created in the background service worker during install.

## 15. Keyboard Shortcut Defaults

Use the `commands` manifest key when the user requests hotkeys.

Good default shortcut actions:

- Capture selection.
- Toggle extension mode.
- Open side panel.
- Copy all notes.
- Run current page action.

Always make shortcuts configurable when possible.

## 16. Error Handling Defaults

Include basic error handling.

For each major action:

- Validate required inputs.
- Catch errors.
- Log developer-facing errors with `console.error`.
- Show user-facing error messages in the UI.
- Avoid silent failure.

Example:

```js
try {
  await chrome.storage.local.set({ notes });
  showStatus("Saved.");
} catch (error) {
  console.error("Failed to save note:", error);
  showStatus("Could not save note. Please try again.", "error");
}
```

## 17. Debugging Defaults

Include a short debugging section in the README.

Mention:

- Go to `chrome://extensions`.
- Enable Developer Mode.
- Click “Load unpacked.”
- Select the extension folder.
- Inspect popup by right-clicking the popup and choosing Inspect.
- Inspect service worker from the extension details page.
- Reload the extension after manifest changes.

## 18. README Defaults

Always include a simple `README.md` with:

- What the extension does.
- How to install locally.
- Required permissions.
- How to use it.
- How to configure settings.
- Known limitations.
- Development notes.

## 19. Code Style Defaults

Use modern JavaScript.

Default style:

- Use ES modules when possible.
- Use `const` and `let`, not `var`.
- Use async/await.
- Keep files focused.
- Name functions by what they do.
- Add comments only where helpful.
- Avoid over-engineering.

Prefer:

```js
async function saveNote(note) {
  const { notes = [] } = await chrome.storage.local.get("notes");
  notes.push(note);
  await chrome.storage.local.set({ notes });
}
```

## 20. Framework Defaults

Do not use a frontend framework by default.

Use plain HTML/CSS/JS unless:

- The user asks for React, Vue, Svelte, or another framework.
- The UI is complex enough to justify it.
- The project already uses a build system.

If using React, Vite, Tailwind, or TypeScript, include build instructions and make sure the final extension output can be loaded unpacked by Chrome.

## 21. Build System Defaults

Avoid build tools for simple extensions.

Use no-build plain JavaScript by default.

Use Vite or another build system only when:

- The user requests React or TypeScript.
- The extension has a complex UI.
- Shared modules, bundling, or npm dependencies are needed.

If a build system is used, provide:

```txt
npm install
npm run build
Load the dist/ folder as unpacked extension
```

## 22. Data Model Defaults

For note-taking, clipping, research, or capture extensions, use a structured data model.

Default captured item:

```js
{
  id: crypto.randomUUID(),
  text: "",
  pageTitle: "",
  pageUrl: "",
  createdAt: new Date().toISOString(),
  tags: [],
  rating: null,
  note: ""
}
```

Do not store unstructured strings if the data may later need filtering, exporting, searching, or formatting.

## 23. Chrome Extension Project Defaults

When creating a new extension, include:

- `manifest.json`
- At least one visible UI entry point: popup, side panel, or extension page
- Any required content scripts
- A background service worker if coordination is needed
- Storage helper if saving data
- README instructions
- Minimal permissions
- No unused files
- No fake API keys
- No placeholder features that do not work

## 23. Default Recommendation Logic

When the user describes a feature, choose the architecture this way:

- Needs to modify the current webpage → use a content script.
- Needs quick controls → use a popup.
- Needs persistent workspace → use a side panel.
- Needs settings → use an options page.
- Needs background events or coordination → use a service worker.
- Needs saved data → use `chrome.storage.local`.
- Needs synced preferences → use `chrome.storage.sync`.
- Needs right-click action → use context menus.
- Needs hotkey → use commands.
- Needs external API with secret key → recommend backend proxy.
- Needs current tab access only after user action → use `activeTab`.
- Needs to run automatically on specific sites → use host permissions and content script matches.
