# Chrome Extension Prompt Templates

Use these templates when helping a user instruct a coding LLM to build a Chrome Extension.

## New Extension Prompt

```md
Build a Manifest V3 Chrome Extension named [EXTENSION NAME].

Description:
[DESCRIPTION]

Architecture defaults:
- Use Manifest V3.
- Use plain HTML/CSS/JavaScript unless a framework is clearly needed.
- Use minimal permissions.
- Use a content script only if webpage DOM access is required.
- Use a background service worker for coordination.
- Use chrome.storage.local for saved user data.
- Do not use inline scripts, remote scripts, eval, or unnecessary <all_urls> permissions.

Please provide:
1. Architecture summary
2. File tree
3. Complete code for each file
4. Local installation steps
5. Testing steps
6. Explanation of permissions
```

## Webpage Capture Extension Prompt

```md
Build a Manifest V3 Chrome Extension that captures selected text from the current webpage.

Use:
- Popup for quick controls
- Content script to read selected text from the page
- Background service worker for coordination
- chrome.storage.local to save captured items
- activeTab and scripting permissions where appropriate

Each captured item should include:
- id
- selected text
- page title
- page URL
- timestamp
- optional note/tags/rating if useful

Provide complete working files and installation instructions.
```

## Side Panel Notes Extension Prompt

```md
Build a Manifest V3 Chrome Extension with a persistent side panel notes workspace.

The extension should:
- Capture selected text from webpages
- Save captures to chrome.storage.local
- Display saved captures in the side panel
- Include a Copy All button that formats the notes into a single prompt
- Use message passing between popup, background service worker, content script, and side panel
- Use minimal permissions
- Avoid inline scripts and remote code

Provide complete working files and testing steps.
```

## Debugging Prompt

```md
Debug this Manifest V3 Chrome Extension.

Problem:
[DESCRIBE ERROR]

Please check:
- manifest.json permissions and paths
- popup script behavior
- background service worker lifecycle
- content script loading/matches
- message passing
- storage usage
- CSP issues such as inline scripts

Explain the likely cause and provide corrected code.
```
