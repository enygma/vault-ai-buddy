# Vault AI Chat — Bootstrap Process

The bootstrap wizard runs automatically the first time a user opens the chat. It collects the
information needed to personalise every future AI session and writes two files to the vault root.

## Flow

### Step 1 — Identity
Ask the user who they are, what they do (professionally or personally), and what they primarily
use Obsidian for. Wait for a full response before continuing.

### Step 2 — Personality & Tone
Ask what kind of personality and tone they would like the assistant to use.
Offer three example styles:
- `professional / courteous / efficient`
- `conversational / friendly / casual`
- `empathetic / supportive / patient`

Let them know they can describe their own style instead.

### Step 3 — Vault Analysis
Without asking the user anything, use `list_folder` and `read_note` tools to silently explore the
vault. Read 8–12 representative notes and identify:
- Recurring topics and themes
- Common note types and structures
- How information is organised
- Notable patterns or areas of focus

### Step 4 — Review & Confirm
Present a summary covering identity, preferred tone, and vault findings. Ask for confirmation or
corrections. Repeat if changes are requested.

### Step 5 — Write Files
Once confirmed, call the `write_bootstrap_files` tool to write both output files to the vault.

---

## Output Files

Both files are written to the vault root and injected into every AI system prompt. They should be
authored with an AI language model as the target audience.

### IDENTITY.md
Documents the user's identity, background, Obsidian usage, and communication preferences.
Written in second person (e.g. "The user is…").

Example structure:
```
# User Identity

## About the User
The user is a [role] who [background].

## Obsidian Usage
They use Obsidian primarily for [purpose]. Their vault contains [content types].

## Communication Preferences
The user prefers a [tone] style. [Specific notes on how to communicate with them.]
```

### KNOWLEDGE.md
Documents the vault's content areas, note structures, recurring topics, and organisational
patterns. Written in second person.

Example structure:
```
# Vault Knowledge

## Primary Topics
The vault focuses on [topics].

## Note Types & Structure
Notes commonly follow [patterns/templates].

## Key Themes & Connections
[Recurring themes, how areas relate to each other.]
```

---

## Re-running Bootstrap

To reset and re-run the wizard:
1. Open Obsidian developer tools (Cmd+Option+I)
2. In the console: `app.plugins.plugins['vault-ai-chat'].settings.bootstrapComplete = false`
   then `app.plugins.plugins['vault-ai-chat'].saveSettings()`
3. Delete `IDENTITY.md` and `KNOWLEDGE.md` from the vault root (optional but recommended)
4. Close and reopen the chat panel
