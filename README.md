# Vault AI Chat

Vault AI Chat is an Obsidian plugin that adds an AI chat panel inside the editor. The chat can use the active note and relevant vault notes as context, then propose or apply guarded filesystem actions such as creating notes, updating the current note, creating folders, moving files, and deleting notes.

## MVP Features

- Right sidebar chat view.
- OpenAI-compatible chat completions endpoint.
- Active note context.
- Vault-wide keyword retrieval across Markdown notes.
- Source display for retrieved notes.
- AI tools for reading, creating, updating, moving, deleting, and organizing files.
- Confirmation prompts before mutating or destructive actions.

## Development

```bash
npm install
npm run build
```

Copy `main.js`, `manifest.json`, and `styles.css` into:

```text
<your-vault>/.obsidian/plugins/vault-ai-chat/
```

Then enable the plugin in Obsidian.

## Settings

- API key
- Base URL, defaulting to `https://api.openai.com/v1`
- Model
- Max context notes
- Mutating action confirmation
- Delete permission

For local or alternate providers, use an OpenAI-compatible `/chat/completions` endpoint.
