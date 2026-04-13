# Features

> Complete feature documentation for OpenCami — a powerful web client for OpenClaw.

## Table of Contents

- [Chat & Messaging](#chat--messaging)
- [Voice Features](#voice-features)
- [Search & Navigation](#search--navigation)
- [Organization](#organization)
- [Customization](#customization)
- [Smart Features](#smart-features)
- [Workspace](#workspace)
- [File Management](#file-management)
- [Mobile & PWA](#mobile--pwa)
- [Desktop App](#desktop-app)

---

## Chat & Messaging

### 💬 Multi-Session Chat

Manage multiple conversations with full session support.

**Features:**
- Sidebar showing all conversations
- Create new sessions with ⌘K
- Switch between sessions instantly
- Session context menu (rename, delete, export)
- Automatic session persistence

**Screenshot placeholder:** `![Chat interface](screenshots/chat-interface.png)`

### ⚡ Real-Time Streaming

Watch AI responses appear token by token in real-time.

**How it works:**
1. Persistent WebSocket connection to OpenClaw Gateway
2. Server-Sent Events (SSE) forward deltas to browser
3. Smooth text appearance as the model generates
4. Tool call indicators during agent processing

**Features:**
- Token-by-token text rendering
- Tool execution visualization
- Thinking/reasoning block display
- Fast-polling fallback for resilience

### 🖼️ Image Attachments

Send images directly in chat with automatic optimization.

**Features:**
- Drag & drop upload
- Clipboard paste (⌘V)
- File picker button
- Auto-compression (fits 512KB WebSocket limit)
- Supports PNG, JPG, GIF, WebP
- Preview before sending
- Max input size: 10MB

**Usage:**
1. Drag an image into the chat composer
2. Or paste from clipboard
3. Or click the attachment button
4. Preview appears — click ✕ to remove
5. Send with your message

### 📎 File Attachments

Attach non-image files directly from the composer.

**Features:**
- Attach button and drag & drop support
- Accepted types include PDF, TXT/MD, CSV/JSON/XML/YAML, and common code files
- Non-image files are uploaded via HTTP to `/uploads/`
- Composer auto-inserts `📎 Uploaded file: /uploads/...` references for agent context
- Files can then be read by the agent with the `read` tool

**Upload behavior:**
- **Images** → Base64 inline attachment (vision flow)
- **Other supported files** → HTTP upload + path reference

### 📄 Uploaded File Cards

Uploaded file references are rendered as clickable cards in chat.

**Features:**
- Shows filename, file icon, and formatted file size
- Click opens the file in **File Explorer**
- Uses `/api/files/info` to load accurate file metadata
- Regex parsing is scoped to leading `📎 Uploaded file:` lines to avoid false positives in normal message text

### 📝 Markdown Rendering

Full markdown support with syntax highlighting.

**Supported elements:**
- Headers (H1-H6)
- **Bold**, *italic*, ~~strikethrough~~
- Code blocks with language detection
- Inline `code`
- Tables
- Ordered and unordered lists
- Blockquotes
- Links and images
- Task lists

**Code blocks:**
- Syntax highlighting via Shiki
- Copy button
- Language label
- Line numbers (optional)

### 🔎 Search Sources Badge

See where search results come from.

**Features:**
- Expandable badge on search-enhanced messages
- Favicons for each source website
- Toggle visibility in Settings
- Collapses to save space

**Usage:** Click the search badge to expand/collapse source list.

---

## Voice Features

### 🔊 Text-to-Speech (TTS)

Listen to AI responses with high-quality voice synthesis.

**Provider cascade (auto-fallback):**
1. 🥇 **ElevenLabs** — Best quality (requires API key)
2. 🥈 **OpenAI TTS** — High quality (requires API key)
3. 🥉 **Edge TTS** — Free, always works, no API key needed

**Features:**
- 🔊 Speaker button on every AI message
- Play/stop controls with loading indicator
- Voice selection (alloy/echo/fable/onyx/nova/shimmer for OpenAI)
- Toggle on/off in Settings
- Reads TTS config from OpenClaw Gateway

**Usage:**
1. Enable TTS in Settings → Voice
2. Click the 🔊 button on any assistant message
3. Audio plays automatically
4. Click again to stop

### 🎤 Speech-to-Text (STT)

Dictate messages with voice input.

**Provider cascade (auto-fallback):**
1. 🥇 **ElevenLabs Scribe v2** — Best accuracy (requires API key)
2. 🥈 **OpenAI Whisper** — High accuracy (requires API key)
3. 🥉 **Browser Web Speech API** — Free, works offline

**Features:**
- 🎤 Microphone button in chat composer
- Recording UI with timer and pulse animation
- Auto-stop at 120 seconds
- Transcribed text inserted into composer
- Edit before sending

**Usage:**
1. Click the 🎤 button in the composer
2. Speak your message
3. Click stop or wait for silence detection
4. Edit the transcription if needed
5. Send

### 🔧 Provider Settings

Configure your preferred voice providers.

**Settings → Voice:**
- **TTS Provider:** Auto / ElevenLabs / OpenAI / Edge TTS
- **STT Provider:** Auto / ElevenLabs / OpenAI / Browser
- **Voice selection:** Choose from available voices
- All preferences stored locally

---

## Search & Navigation

### 🔍 Conversation Search

Find anything across your conversations.

| Shortcut | Scope |
|----------|-------|
| **⌘F** | Search current conversation |
| **⌘⇧F** | Global search across all sessions |

**Features:**
- Instant results with highlighted matches
- Jump to message on click
- Message context preview
- Session name in global results

**Usage:**
1. Press ⌘F for current chat or ⌘⇧F for global
2. Type your search query
3. Click a result to jump to that message
4. Press Esc to close

### ⌨️ Keyboard Shortcuts

Full keyboard navigation for power users.

| Shortcut | Action |
|----------|--------|
| `⌘K` | New chat |
| `⌘/` | Focus input |
| `⌘F` | Search current chat |
| `⌘⇧F` | Global search |
| `⌘⇧C` | Copy last response |
| `⌘B` | Toggle sidebar |
| `Esc` | Close dialogs |
| `?` | Show shortcuts help |

**Usage:** Press `?` anywhere to see the full shortcuts list.

### 💬 Slash Commands

Built-in commands for quick actions.

**Usage:** Type `/` in the composer to see available commands.

Common commands:
- `/help` — Show available commands
- `/persona <name>` — Switch persona
- `/clear` — Clear conversation
- `/model <name>` — Switch model

---

## Organization

### 📁 Session Folders

Organized sidebar with collapsible groups.

**Folder categories:**
- 💬 **Chats** — Regular conversations (always expanded)
- 🤖 **Sub-agents** — Spawned sub-agent sessions
- ⏰ **Cron / Isolated** — Scheduled task sessions
- 📁 **Other** — Everything else

**Features:**
- Auto-detection from session key pattern
- Count badges show items per folder
- Collapse/expand state persisted
- Chats folder stays at top

### 📌 Pin Sessions

Keep important sessions at the top.

**Features:**
- Pin/unpin from session context menu (right-click)
- 📌 icon on pinned sessions
- Pinned sessions always above folders
- Multiple pins supported

**Usage:** Right-click a session → Pin/Unpin

### 🗑️ Bulk Session Delete

Clean up old sessions efficiently.

**Features:**
- **Select** button in sidebar header
- Click sessions to select/deselect
- **Select All** / **Delete Selected** / **Cancel** buttons
- Concurrent deletion (max 10 parallel)
- Confirmation dialog before deleting

**Protection:**
- Protected sessions cannot be deleted
- Sessions are archived, not permanently destroyed

**Usage:**
1. Click **Select** in sidebar header
2. Click sessions to select them
3. Click **Delete Selected**
4. Confirm in the dialog

### 🛡️ Protected Sessions

Automatic deletion protection for critical sessions.

**Always protected:**
- **Main session** (`agent:main:main`)
- **Channel-bound sessions:**
  - Telegram (`:telegram:`)
  - Discord (`:discord:`)
  - Signal (`:signal:`)
  - WhatsApp (`:whatsapp:`)
  - Slack (`:slack:`)
  - iMessage (`:imessage:`)

**Behavior:**
- Delete button hidden for protected sessions
- Cannot select in bulk delete mode
- Prevents accidental deletion of active integrations

### 📥 Conversation Export

Export any conversation in multiple formats.

**Formats:**
- 📝 **Markdown** (.md) — Human-readable with formatting
- 📋 **JSON** (.json) — Structured data for processing
- 📄 **Plain Text** (.txt) — Simple text format

**Usage:** Click the ⋯ menu on a session → Export → Choose format

---

## Customization

### 🎨 Model Selector

Switch AI models on the fly.

**Features:**
- Dropdown in chat composer
- Shows all configured models from Gateway
- Per-message model override
- Remembers your selection

**Usage:** Click the model name in the composer to switch.

### 🎭 Persona Picker

Switch between AI personalities instantly.

**Requirements:** [Personas skill](https://www.clawhub.ai/robbyczgw-cla/personas) installed

**Features:**
- 20 expert personas across 7 categories
- One click to activate
- Sends `/persona` commands automatically
- Active persona indicator
- Auto-detects skill installation

**Persona categories:**
- Development (Coder, Reviewer, etc.)
- Writing (Editor, Copywriter)
- Creative (Artist, Designer)
- Professional (Chef Marco, Dr. Med, etc.)
- And more...

**Usage:**
1. Enable in Settings → Features → Persona Picker
2. Click the 🎭 button in the composer
3. Select a persona
4. Chat with the new personality

### 🎨 Themes

Multiple theme options for your preference.

**Available themes:**
- ☀️ **Light** — Clean, bright interface
- 🌙 **Dark** — Easy on the eyes
- 💻 **System** — Follows OS preference
- 🦎 **Chameleon** — Nature-inspired sage/mint
- ❄️ **Ice** — Glassmorphism with accent colors (always light in v1.7.0)

**Ice theme features:**
- Translucent panels with backdrop blur
- Accent color glow effects
- Always light (does not switch with system dark mode)
- Premium glass aesthetic

**Usage:** Settings → Appearance → Theme

### 🎨 Accent Colors

Customize your interface accent color.

**Usage:** Settings → Appearance → Accent Color

The accent color affects:
- Selected sessions
- Buttons and links
- User message bubbles
- Focus indicators

### 🔤 Text Size

Adjust chat text to your preference.

**Options:** S / M / L / XL

**Usage:** Settings → Appearance → Text Size

### 🧠 Thinking Level

Control AI reasoning depth per message.

**Levels:**
- **Off** — No explicit reasoning
- **Low** — Brief reasoning
- **Medium** — Moderate reasoning
- **High** — Deep reasoning

**Usage:** Toggle in composer before sending message.

---

## Smart Features

### 🏷️ Smart Session Titles

AI-generated titles that describe your conversations.

**How it works:**
1. After the first exchange, an LLM analyzes the conversation
2. It generates a concise, descriptive title
3. The generated title replaces generic session names automatically

**Features:**
- Automatic title generation after chat starts
- Better session organization in the sidebar
- Works alongside the LLM Features Model selector

**Usage:** Enable in Settings → LLM Features → Smart Titles

### 💡 AI Follow-up Suggestions

Get 3 AI-generated suggestions for what to ask next after each assistant response.

**How it works:**
1. After a response finishes streaming, OpenCami analyzes the conversation
2. It generates 3 contextual follow-up questions
3. Suggestions appear below the message
4. One click sends any suggestion instantly

**Features:**
- 3 contextual follow-up questions per response
- Toggleable in Settings
- Suggestions stay aligned with the conversation language
- Fast, one-click continuation of the chat

**Usage:** Enable in Settings → LLM Features → Smart Follow-ups

### 🤖 LLM Features Model Selector

Choose which agent handles AI-powered titles and follow-up suggestions.

**What it controls:**
- Smart session title generation
- AI follow-up suggestion generation

**Features:**
- Pick the model/agent used for LLM-powered UI features
- Friendly model names in Settings
- Shared configuration for titles and follow-ups

**Configuration:** Settings → LLM Features → Model

---


## Workspace

### 🔧 Workspace

Workspace centralizes tool visibility controls in Settings.

**Included tools:**
- 📂 **Files** (`/files`)
- 🧠 **Memory** (`/memory`)
- 🤖 **Agents** (`/agents`)
- 🧩 **Skills** (`/skills`)
- ⏰ **Cron Jobs** (`/bots`)

**Navigation:**
- Files, Memory, Agents, Skills, and Cron Jobs keep their existing sidebar links
- Routes stay unchanged (`/files`, `/memory`, `/agents`, `/skills`, `/bots`)

**Settings → Workspace controls:**
- **File Explorer:** enabled by default (`opencami-file-explorer`)
- **Memory Viewer:** always available (enabled by default)
- **Agent Manager:** optional (`opencami-agent-manager`)
- **Skills Browser:** optional (`opencami-skills-browser`)
- **Cron Jobs Panel:** optional (`opencami-cron-manager`)

## File Management

### 📂 File Explorer

Browse and edit files directly in the browser.

**Features:**
- **Sidebar navigation** with breadcrumb trail
- **Upload, download, rename, delete** operations
- **Built-in text editor** with syntax highlighting
- **Right-click context menu**
- **Path jailing** with symlink protection

**Supported file types (30+):**
- Code: js, ts, tsx, py, rb, go, rs, java, c, cpp, h, swift, kt
- Config: json, yaml, yml, toml, ini, env
- Docs: md, txt, html, xml, csv
- And more...

**Keyboard shortcuts:**
- **Ctrl+S** — Save file
- **Esc** — Close editor

**Security:**
- All paths jailed to `FILES_ROOT`
- Symlink escape protection
- Directory traversal blocked

**Usage:**
1. Set `FILES_ROOT` environment variable
2. Navigate to Files in sidebar
3. Browse, edit, upload as needed

---

## Mobile & PWA

### 📱 Progressive Web App

Install OpenCami as a native-like app.

**Features:**
- Install to Home Screen
- Native app feel (no browser chrome)
- Offline support (cached shell)
- Auto-update in background

**Installation:**
- **Android (Chrome):** Menu → Add to Home Screen
- **iOS (Safari):** Share → Add to Home Screen

### 👉 Gesture Navigation

Native-feeling touch gestures.

**Gestures:**
- **Swipe right from left edge** — Opens sidebar
- **Swipe left on sidebar** — Closes sidebar
- **Tap backdrop** — Closes sidebar

**Features:**
- 40px edge detection zone
- Direction lock (no scroll interference)
- Desktop unaffected

### 📱 Native Android App

Full native APK built with Capacitor.

**Features:**
- Native WebView shell
- Transparent status bar
- Branded splash screen
- Full icon set (hdpi to xxxhdpi)
- Keyboard handling optimized

**Package:** `com.opencami.app`

---

## Desktop App

### 🖥️ Tauri Desktop App (Beta)

Native macOS/Windows/Linux app.

**Requirements:**
- Node.js 18+
- Rust toolchain (`rustup`)

**Build:**
```bash
npm run build
npm run tauri:build
```

**Features:**
- Tray icon (hide to tray on close)
- Native notifications
- Auto-start on login
- Custom titlebar
- Multiple windows (⌘N)
- Clipboard integration

**Output locations:**
- macOS: `.app`, `.dmg`
- Windows: `.exe`, `.msi`
- Linux: `.deb`, `.AppImage`

**Custom Gateway URL:**
```bash
OPENCAMI_REMOTE_URL="https://your-server.example.com" npm run tauri:build
```

---

## Feature Toggles

Control features in Settings:

| Feature | Default | Description |
|---------|---------|-------------|
| Tool Messages | On | Show tool execution |
| Reasoning Blocks | On | Show AI thinking |
| Search Sources | On | Show source badges |
| Inline File Preview | On | Preview files in chat |
| TTS | Off | Voice playback |
| STT | Off | Voice input |
| Persona Picker | Off | Personality switching |
| Smart Titles | On | LLM-generated titles |
| Smart Follow-ups | On | Suggestion bubbles |

---

## Related Documentation

- [Architecture](./ARCHITECTURE.md) — Technical deep dive
- [Deployment](./DEPLOYMENT.md) — Self-hosting guide
- [Theming](./THEMING.md) — Theme customization
- [Contributing](./CONTRIBUTING.md) — Development setup
