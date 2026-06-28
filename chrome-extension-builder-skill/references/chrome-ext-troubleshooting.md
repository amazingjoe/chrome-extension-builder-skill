# Chrome Extension Troubleshooting

Use this file when debugging Chrome Extension problems.

## Receiving end does not exist

Likely causes:

- Content script is not loaded on the current tab.
- The tab URL does not match the content script `matches` pattern.
- The script has not been injected yet.
- The target page is a restricted page such as `chrome://extensions`.

Fixes:

- Verify `content_scripts.matches` in `manifest.json`.
- Use `chrome.scripting.executeScript` after user action.
- Check that the active tab has a normal `http` or `https` URL.
- Reload the extension and the target page.

## Service worker goes inactive

This is normal in Manifest V3.

Fixes:

- Do not rely on in-memory background variables.
- Store important state in `chrome.storage`.
- Reinitialize event listeners at top level in the service worker.

## Popup loses state

This is normal. Popups close when focus changes.

Fixes:

- Persist state in `chrome.storage`.
- Use a side panel or extension page for persistent workflows.

## chrome is not defined

Likely causes:

- Code is running outside the extension context.
- The script is being tested directly in a normal webpage.
- Build tooling is executing extension code in the wrong environment.

Fixes:

- Load the project as an unpacked extension.
- Run extension APIs only inside extension files or content scripts.
- Guard code when needed.

## Manifest is invalid

Likely causes:

- Invalid JSON.
- Wrong paths.
- Missing required fields.
- Manifest V2 syntax used in Manifest V3.

Fixes:

- Validate JSON.
- Confirm all referenced files exist.
- Use `background.service_worker`, not old background scripts.
- Reload the extension after changing the manifest.

## Inline script blocked by CSP

Likely cause:

- JavaScript inside HTML file.

Fix:

- Move JavaScript into a separate `.js` file and reference it with `<script src="...">`.

## Cannot access webpage DOM from service worker

This is expected.

Fix:

- Use a content script for DOM access.
- Send messages between the service worker and content script.

## Permission denied

Likely causes:

- Missing permission in `manifest.json`.
- Missing host permission.
- Trying to access a restricted Chrome page.
- Using a permission that requires user gesture.

Fixes:

- Add the minimal required permission.
- Prefer `activeTab` for user-triggered access.
- Use specific host permissions for automatic access.

## Changes not appearing

Fixes:

- Reload the extension from `chrome://extensions`.
- Refresh the target webpage.
- Reopen the popup or side panel.
- Inspect the correct context: popup, service worker, content script, or extension page.
