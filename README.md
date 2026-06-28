<div align="center">

# 🧩 Chrome Extension Builder Skill

### An agent skill that turns natural-language ideas into production-ready Chrome Extensions

[![Manifest V3](https://img.shields.io/badge/Manifest-V3-4285F4?style=for-the-badge&logo=googlechrome&logoColor=white)](https://developer.chrome.com/docs/extensions/mv3/intro/)
[![Plain JS](https://img.shields.io/badge/Stack-HTML%20%2F%20CSS%20%2F%20JS-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![License: MIT](https://img.shields.io/badge/License-MIT-A855F7?style=for-the-badge)](LICENSE)

<br/>

<picture>
  <!-- Swap graphic for dark / light GitHub theme -->
  <source media="(prefers-color-scheme: dark)" srcset="https://img.shields.io/badge/Agent%20Skill-ready-00C853?style=for-the-badge&labelColor=1a1a2e">
  <img alt="Agent Skill Ready" src="https://img.shields.io/badge/Agent%20Skill-ready-00C853?style=for-the-badge">
</picture>

---

**Describe what you want → get a working extension.**

The skill handles architecture decisions, manifest configuration, permissions,
content scripts, service workers, messaging, storage, and UI — so you can focus on the idea.

</div>

<br/>

## ✨ What It Does

| Capability | Detail |
|:---|:---|
| 🏗️ **Guided Architecture** | Walks through a short intake flow to nail down UI, permissions, storage, and triggers |
| 📦 **Manifest V3 First** | Generates correct, minimal `manifest.json` files by default |
| 🔒 **Least-Privilege Permissions** | Prefers `activeTab` over broad host permissions; keeps the permission list tight |
| 💬 **Messaging Patterns** | Wires up popup ↔ background ↔ content script communication automatically |
| 🗄️ **Storage Defaults** | Uses `chrome.storage.local` for data, `chrome.storage.sync` for small prefs |
| 🧪 **Delivery Format** | Every output includes architecture summary, file tree, complete code, install steps, and test steps |

<br/>

## 📂 Repository Structure

```
chrome-extension-builder-skill/
│
├── SKILL.md                          # ← Skill definition & instruction set
│
├── examples/                         # Starter extension blueprints
│   ├── simple-popup-extension.md
│   ├── content-script-extension.md
│   └── sidepanel-notes-extension.md
│
└── references/                       # Deep-dive reference docs
    ├── chrome-ext-defaults.md
    ├── chrome-ext-architecture-patterns.md
    ├── chrome-ext-prompt-templates.md
    └── chrome-ext-troubleshooting.md
```

<br/>

## 🚀 Quick Start

### 1 · Install the Skill

**Option A — Install from [Skills Directory](https://skills.directory)**

```bash
openskills install amazingjoe/chrome-extension-builder-skill
```

**Option B — Manual install**

Clone or copy the `chrome-extension-builder-skill/` folder into the skills directory for your agent setup:

| Agent / IDE | Skills path                                      |
| :------------| :-------------------------------------------------|
| Gemini      | `.gemini/skills/chrome-extension-builder-skill/` |
| Claude      | `.claude/skills/chrome-extension-builder-skill/` |
| Cursor      | `.cursor/skills/chrome-extension-builder-skill/` |
| OpenAI      | `.openai/skills/chrome-extension-builder-skill/` |
| Grok        | `.grok/skills/chrome-extension-builder-skill/`   |
| Agent       | `.agent/skills/chrome-extension-builder-skill/`  |

> [!TIP]
> Most agent frameworks auto-discover skills placed in the standard `skills/` root — no registration file needed.

### 2 · Trigger It

Just ask your agent to build a Chrome extension. The skill activates on intent — no slash command required.

```
"Build me a Chrome extension that highlights all links on the current page
 and copies them to the clipboard when I click the toolbar icon."
```

### 3 · Get a Working Extension

The agent will:
1. Confirm the extension name & description
2. Ask 3–6 targeted architecture questions
3. Output a complete, loadable extension with install & test instructions

<br/>

## 📖 Key Reference Docs

| Document | Purpose |
|:---|:---|
| [`chrome-ext-defaults.md`](chrome-extension-builder-skill/references/chrome-ext-defaults.md) | Default code patterns, manifest fields, and boilerplate |
| [`chrome-ext-architecture-patterns.md`](chrome-extension-builder-skill/references/chrome-ext-architecture-patterns.md) | Decision guide for popups, side panels, content scripts, and service workers |
| [`chrome-ext-prompt-templates.md`](chrome-extension-builder-skill/references/chrome-ext-prompt-templates.md) | Ready-made prompt templates for common extension types |
| [`chrome-ext-troubleshooting.md`](chrome-extension-builder-skill/references/chrome-ext-troubleshooting.md) | Fixes for frequent Manifest V3 errors and gotchas |

<br/>

## 🧠 Design Principles

```
  ┌─────────────────────────────────────────────────┐
  │                                                 │
  │   1.  Working code  >  abstract explanation     │
  │   2.  Minimal permissions  >  convenience       │
  │   3.  Plain HTML/CSS/JS  >  framework bloat     │
  │   4.  Targeted questions  >  long questionnaires │
  │   5.  Safe defaults  >  broken icon paths       │
  │                                                 │
  └─────────────────────────────────────────────────┘
```

<br/>

## 🤝 Contributing

Contributions are welcome! If you have improvements for the skill instructions, new reference docs, or additional example blueprints:

1. Fork the repo
2. Create a feature branch (`git checkout -b feat/my-improvement`)
3. Commit your changes (`git commit -m "Add new pattern for devtools extension"`)
4. Push to the branch (`git push origin feat/my-improvement`)
5. Open a Pull Request

<br/>

## 📄 License

This project is licensed under the [MIT License](LICENSE).

<br/>

---

<div align="center">

Made with 💜 for the agent-building community

**[⬆ Back to Top](#-chrome-extension-builder-skill)**

</div>
