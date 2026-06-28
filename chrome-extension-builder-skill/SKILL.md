---
name: chrome-extension-builder-skill
version: 1.0.0 
description: Use when the user wants to create, modify, plan, debug, prompt, or understand a Chrome Extension. Covers Manifest V3 architecture, permissions, content scripts, service workers, messaging, storage, and UI patterns.
author: amazingjoe
tags: [chrome-extension, manifest-v3, javascript, html, css, service-worker, content-script, browser-extension, chrome-api, permissions, messaging, storage, side-panel, popup, web-development]
---

# Chrome Extension Builder Skill

Use this skill when the user wants to create, modify, plan, debug, prompt, or understand a Chrome Extension.

Default to Manifest V3 unless the user explicitly asks otherwise.

Read `references/chrome-ext-defaults.md` before making architecture or code decisions. Use `references/chrome-ext-architecture-patterns.md` when deciding how extension parts should communicate. Use `references/chrome-ext-troubleshooting.md` when debugging common Chrome Extension errors.

## Core Behavior

For every extension, identify:

1. User-facing UI: popup, side panel, options page, or extension page
2. Webpage interaction: content script or none
3. Background coordination: service worker or none
4. Storage: local, sync, backend, or none
5. Permissions: minimal required set
6. Messaging flow between extension parts

Prefer:

- Plain HTML/CSS/JS unless the user requests a framework
- Minimal permissions
- `activeTab` before broad host permissions
- `chrome.storage.local` for user data
- `chrome.storage.sync` for small synced preferences
- Side panel for persistent workflows
- Content scripts for DOM/page interaction
- Background service worker for extension coordination
- Working code over abstract explanation

Avoid:

- Manifest V2
- Inline scripts
- Remote hosted JavaScript
- `eval`
- `new Function`
- Unnecessary `<all_urls>` permissions
- Assuming the service worker is always running
- Putting webpage DOM logic in the background service worker
- Putting long-running state inside the popup

## Initial Intake Flow

When the user initiates the Chrome Extension Builder Skill, first determine whether the following two details are already present:

1. **Extension name**
2. **Extension description**

If either detail is missing, ask for it before generating architecture or code.

Use this format:

```md
Great — let’s define the extension first.

What is the extension name?

What should the extension do? A 1–3 sentence description is enough.
```

If the user has already provided one or both details, confirm what is known and ask only for what is missing.

Example:

```md
I have the extension concept: it captures selected webpage text and saves it into a running prompt builder.

What should we call the extension?
```

If both details are already present, do not ask again. Confirm them briefly and move to gap-filling questions.

Example:

```md
Got it.

Extension name: Prompt Clipper  
Description: Captures selected text from webpages, lets the user mark it with a quick rating, and appends it to a reusable prompt-building workspace.

Now I’ll identify the few architecture details that affect how we build it.
```

## Gap-Filling Question Rules

After the extension name and description are known, infer the likely architecture.

Identify whether the extension likely needs:

- Popup
- Content script
- Background service worker
- Side panel
- Options page
- Extension page/new tab
- Local storage
- Synced settings
- Backend/API calls
- Context menu
- Keyboard shortcuts
- Host permissions
- `activeTab`
- `scripting`
- Clipboard access

Ask follow-up questions only when the answer changes architecture, permissions, storage, security, or user experience.

Do not ask a long questionnaire by default.

Prefer 3–6 targeted questions.

## Default Gap-Filling Questions

Choose only the questions relevant to the user's extension idea.

### Website Access

Ask this if the extension interacts with webpages:

```md
Should this work on all websites, or only specific domains?
```

Default if unspecified: use `activeTab` for user-triggered access, or specific host permissions for automatic site behavior.

### Main UI

Ask this if the user has not described the interface:

```md
Should the main interface be a small popup, a persistent side panel, or a full extension page/new tab?
```

Default if unspecified:

- Popup for quick controls
- Side panel for persistent workflows
- Extension page/new tab for larger dashboards

### Page Interaction

Ask this if the extension may read, highlight, scrape, or modify webpages:

```md
Does the extension need to read or change the current webpage, such as selected text, page content, highlights, buttons, or overlays?
```

Default if yes: use a content script.

### Storage

Ask this if the extension saves anything:

```md
Should saved data stay local on the user's browser, sync across Chrome browsers, or go to a backend/database?
```

Default if unspecified: use `chrome.storage.local`.

### External API / AI

Ask this if the extension calls AI or an external service:

```md
Will this call an external API or AI model? If yes, should it use a backend proxy, or is it acceptable for the user to provide an API key locally?
```

Default if a secret API key is involved: recommend a backend proxy.

### Triggers

Ask this if the feature could be activated multiple ways:

```md
How should the user trigger the main action: popup button, right-click menu, keyboard shortcut, page button, automatic detection, or some combination?
```

Default if unspecified: popup button for explicit user action.

### Publishing

Ask this if the user seems likely to distribute the extension:

```md
Is this just for local use, or do you want it prepared for Chrome Web Store publishing?
```

Default if unspecified: local unpacked extension, while avoiding patterns that would block future publishing.

## Intake State Rule

Do not repeatedly ask for information the user has already provided.

Before asking a question, check whether the answer is already available from the conversation.

If the user gives an informal concept but not a polished description, create a concise draft description and ask them to confirm or revise it.

Example:

```md
Based on what you described, I would summarize it this way:

“An extension that lets users select text on a webpage, classify it with quick buttons, and append the selection into a reusable prompt-building document.”

Does that description work, or should I adjust it?
```

If the user gives a clear name and description, move directly into architecture planning.

## Intake Output

After intake, summarize the proposed architecture before writing code.

Use this format:

```md
Proposed architecture:

- Manifest: Manifest V3
- UI: Popup for quick controls; side panel for persistent notes
- Page access: Content script
- Background: Service worker for coordination
- Storage: chrome.storage.local
- Permissions: storage, activeTab, scripting, sidePanel
- Messaging: popup → background → content script → storage → side panel
```

Then ask for confirmation only if there is meaningful ambiguity. Otherwise, proceed with the best default.

## Icon Rule

When generating a Chrome extension, ensure any icon paths declared in `manifest.json` point to real files included in the project.

If real icon files are not provided by the user, use one of these safe defaults:

1. Omit the `icons` field and `action.default_icon` until icons are available, or
2. Generate simple placeholder PNG icons at 16x16, 32x32, 48x48, and 128x128.

Do not declare icon files that do not exist. Broken icon paths can prevent smooth unpacked-extension loading and create confusing warnings.

## Delivery Format

When delivering code, provide:

1. A short architecture summary
2. The file tree
3. Complete code for each file
4. Local installation steps
5. Testing steps
6. Permission explanation
7. Any limitations or next improvements

Prefer working code over abstract explanation.
