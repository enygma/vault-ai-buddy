# Vault AI Chat

Vault AI Chat brings an AI assistant directly into Obsidian. It lives in a sidebar panel and understands your vault — it can search your notes for relevant context, read and edit files on your behalf, and connect to external tools via MCP servers. Think of it as a research and writing partner that already knows what's in your vault.

---

## What it does

- **Answers questions using your vault** — every message automatically searches your notes for relevant content and includes it as context, so the AI can reference what you've actually written.
- **Works with your active note** — mention "active note" or "current note" in a message and the AI will read what you have open and can suggest or apply edits.
- **Takes actions in your vault** — create notes, update content, move files, create folders, and more. Destructive actions ask for confirmation first.
- **Connects to MCP servers** — extend the AI with external tools by configuring Model Context Protocol servers. The included GitHub MCP support lets you query repos, issues, and pull requests directly from the chat.
- **Remembers things** — ask the AI to "remember" something and it saves it to your vault for use in all future conversations.
- **Learns about you** — a one-time setup wizard builds a personal profile and vault knowledge base, which the AI uses to tailor every response.
- **Multiple sessions** — run several conversations in parallel using the tab strip at the top of the panel.

---

## Getting started

### Installation

1. Copy `main.js`, `manifest.json`, and `styles.css` into your vault at:
   ```
   <your-vault>/.obsidian/plugins/vault-ai-chat/
   ```
2. Open Obsidian → **Settings → Community plugins** → enable **Vault AI Chat**.
3. Go to **Settings → Vault AI Chat** and paste in your API key.

### First launch

The first time you open the chat panel (click the message icon in the left ribbon, or use the **Open Vault AI Chat** command), you'll be walked through a short setup wizard that:

1. Asks who you are and how you use Obsidian
2. Asks what tone and personality you'd like the AI to use
3. Silently scans a sample of your vault notes to identify topics and patterns
4. Asks you to confirm before saving anything

The wizard writes two files to your vault root — `IDENTITY.md` and `KNOWLEDGE.md` — which are injected into every AI request as personalisation context. You can edit these files at any time to adjust what the AI knows about you.

---

## Using the chat

### Sending messages

Type in the text box at the bottom of the panel and press **Enter** (or **Shift+Enter** for a new line). The AI searches your vault on every message and includes the most relevant notes as background context.

### Referencing your active note

The AI does not automatically read your open note on every message — only when you ask for it. Use phrases like:

- *"Summarise the active note"*
- *"Can you edit the current note to add a section on X?"*
- *"What does this note say about Y?"*

### Asking the AI to remember something

Say "remember that…" and the AI will save the information to `KNOWLEDGE.md` in your vault, where it will be included in all future conversations.

> *"Remember that I prefer code examples in TypeScript."*
> *"Remember that my project deadline is end of June."*

### Vault file actions

The AI can create, update, move, and delete notes and folders. By default it will ask for confirmation before making any change. You can turn this off in settings if you prefer.

### Managing sessions

- **New** — starts a fresh conversation
- **Sessions** — shows all open conversations; click one to switch, or close it with ×
- **MCPs** — shows connected MCP servers and their available tools (expand a server to see the full tool list)

---

## MCP servers

MCP (Model Context Protocol) servers extend the AI with additional tools — GitHub, web search, databases, and anything else that exposes an MCP interface.

### Adding a server

1. Go to **Settings → Vault AI Chat → MCP Servers → Add MCP Server**
2. Give it a name, choose a transport type, and fill in the details:
   - **stdio** — a local process. Enter the command (e.g. `docker` or `npx`) and one argument per line.
   - **HTTP** — a running server. Enter the base URL.
3. Save. The server starts immediately — no need to close and reopen the panel.

### GitHub MCP example

```
Transport: stdio
Command:   docker
Arguments:
  run
  -i
  --rm
  -e
  GITHUB_PERSONAL_ACCESS_TOKEN
  ghcr.io/github/github-mcp-server
Environment variables:
  GITHUB_PERSONAL_ACCESS_TOKEN=ghp_your_token_here
```

> **Note:** You must have Docker installed and the image pulled locally before the server will start. Run the docker command manually once to pull the image if needed.

---

## Settings reference

| Setting | Description |
|---|---|
| **API key** | Your OpenAI (or compatible) API key. Stored in plain text in `data.json` — see the security note below. |
| **Base URL** | The `/chat/completions` endpoint. Defaults to `https://api.openai.com/v1`. Change this to use a local model or alternative provider. |
| **Model** | The model name to request, e.g. `gpt-4o-mini`, `gpt-4o`. |
| **Max context notes** | How many vault notes to include as search context per message (1–20). |
| **Allowed root** | Restrict AI file actions to a specific folder. Leave blank for no restriction. |
| **Require confirmation** | Ask before creating, editing, moving, or deleting files. On by default. |
| **Allow deletes** | When off, the AI cannot delete notes even if asked. Off by default. |

---

## Security

- **API key storage** — your API key and any MCP environment variables (including tokens) are stored in plain text in `.obsidian/plugins/vault-ai-chat/data.json`. Obsidian has no built-in secure key storage. Do not store your vault in a shared, publicly synced, or unencrypted location if this is a concern.
- **Vault access** — the AI can read any note in your vault as context and, if you grant it, modify files. Use the **Allowed root** setting to restrict file actions to a specific folder.
- **MCP tools** — external MCP tools can have real-world side effects (pushing code, creating issues, etc.). The AI will only use them when directly relevant to your request, but review what tools a server exposes before adding it.
- **Re-running setup** — to reset the personalisation wizard and start fresh, delete `IDENTITY.md` and `KNOWLEDGE.md` from your vault root, then run this in the Obsidian developer console (`Cmd+Option+I`):
  ```js
  const p = app.plugins.plugins['vault-ai-chat'];
  p.settings.bootstrapComplete = false;
  await p.saveSettings();
  ```

---

## Development

```bash
npm install
npm run dev     # watch mode
npm run build   # production build
```

The plugin is written in TypeScript and bundled with esbuild. Source is in `main.ts`; styles are in `styles.css`.
