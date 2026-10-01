<div align="center">

# 🕷️ Spider-Chat

**A Spider-Verse themed AI chat that runs in a single HTML file.**

Corner webs · a swaying spider · NYC skyline · THWIP send effect · comic-panel tables · streaming replies

[![Single File](https://img.shields.io/badge/single--file-HTML-e62429?style=flat-square)](#)
[![No Build](https://img.shields.io/badge/build-none-2c5cdd?style=flat-square)](#)
[![License: MIT](https://img.shields.io/badge/license-MIT-ffd60a?style=flat-square)](LICENSE)
[![OpenRouter](https://img.shields.io/badge/OpenRouter-compatible-2c5cdd?style=flat-square)](https://openrouter.ai)
[![DeepSeek](https://img.shields.io/badge/DeepSeek-compatible-e62429?style=flat-square)](https://api.deepseek.com)

</div>

---

## What is this?

Spider-Chat is a complete, production-grade chat interface wrapped in a Spider-Verse skin. It's **one HTML file** — open it in a browser, paste your API key, and you're talking to DeepSeek, Claude, GPT, Gemini, or any OpenAI-compatible model through OpenRouter or the official DeepSeek API.

No npm. No build step. No server. Just open the file.

```
index.html  →  double-click  →  chat.
```

---

## Screenshots

> Drop your screenshots in a `screenshots/` folder and reference them here.

| Dark | Light |
|---|---|
| `screenshots/dark.png` | `screenshots/light.png` |

| Tables | Copy / Regenerate |
|---|---|
| `screenshots/table.png` | `screenshots/actions.png` |

---

## Features

### 🕸️ Spider-Verse theme
- **Corner webs** — hand-drawn SVG webs in the top-left and top-right of the main area
- **Hanging spider** — a spider on a silvery thread that sways on a 6s loop and stops when you hover it
- **NYC skyline** — two-layer SVG skyline at the bottom with flickering windows and a red glow
- **Halftone comic dots** — a subtle drifting dot pattern behind everything
- **THWIP effect** — a red/white radial burst that fires from the center when you hit send
- **Swing-in messages** — every bubble drops in on a spring curve like it's on a web strand
- **Comic-panel tables** — red-rimmed panels with gradient headers and hover rows
- **Full light mode** — comic-book paper aesthetic with a cream background

### 💬 Chat
- **Streaming replies** — token-by-token via Server-Sent Events
- **Reasoning panel** — collapsible `<details>` for DeepSeek R1 / reasoning models
- **Copy full response** — one click copies the entire message (raw markdown source)
- **Regenerate** — re-sends the paired user prompt and drops the old answer
- **Multi-file attachments** — images (base64 vision), text/code (inlined), binaries (noted)
- **Drag & drop or paste** — drop files on the composer, or paste an image directly
- **Per-conversation history** — saved to `localStorage`, survives reloads

### 🎨 UI
- **Real Copilot-inspired layout** — sidebar with Search / Library / Notebooks / More
- **Day-grouped chat list** — Today, Yesterday, Previous 7 days, Older
- **Div-based tables** — CSS Grid, no `<table>` tags, per-column alignment
- **Dark / light toggle** — the entire palette flips with a smooth 300ms transition
- **Smooth transitions everywhere** — spring hovers, fade-slide message entry, glow focus rings
- **Responsive** — sidebar collapses to a drawer under 820px

---

## Quick start

### 1. Clone

```bash
git clone https://github.com/YOUR_USERNAME/spider-chat.git
cd spider-chat
```

### 2. Open

Double-click `index.html`, or serve it:

```bash
# Python
python -m http.server 8000

# Node
npx serve
```

Then visit `http://localhost:8000`.

### 3. Configure

Click the gear icon (bottom of the sidebar) and fill in:

| Field | Value |
|---|---|
| **Provider preset** | `OpenRouter` |
| **API Base URL** | `https://openrouter.ai/api/v1` |
| **API Key** | your `sk-or-v1-…` key from [openrouter.ai/keys](https://openrouter.ai/keys) |
| **Model** | `deepseek/deepseek-chat-v3-0324:free` (or any slug you like) |

Hit **Test connection** to verify the model is reachable, then **Save**.

> **Note on the API key:** it's stored only in your browser's `localStorage`. It never leaves your machine except to go directly to the provider you configured.

---

## Supported providers

| Provider | Base URL | Model example |
|---|---|---|
| **OpenRouter** | `https://openrouter.ai/api/v1` | `deepseek/deepseek-chat-v3-0324:free` |
| **DeepSeek official** | `https://api.deepseek.com/v1` | `deepseek-chat` |
| **Custom / any OpenAI-compatible** | your own | your own |

Any endpoint that speaks the OpenAI `/chat/completions` shape with `stream: true` will work.

---

## How it works

Everything lives in one file. The JavaScript is organized into clear sections:

```
Markdown module       →  parse markdown + GFM tables (div-based)
Storage               →  localStorage read/write with base64 stripping
Sidebar               →  chat list, day grouping, delete
Messages              →  build DOM nodes, stream updates, copy, regenerate
Files                 →  classify, read, stage
Streaming API         →  fetch + SSE parser for /chat/completions
Send                  →  build payload, stream, handle errors
Composer              →  textarea autoresize, send button state
Settings              →  modal, presets, test connection
```

### Why div-tables?

Regular `<table>` inside a chat bubble is a pain to style and hard to make responsive. Spider-Chat emits:

```html
<div class="md-table-scroll">
  <div class="md-table" style="--md-cols: repeat(4, minmax(120px, 1fr))">
    <div class="md-table-row">
      <div class="md-table-cell head">Column A</div>
      …
    </div>
    …
  </div>
</div>
```

The parent uses `display: grid`, each row uses `display: contents` (so cells become direct grid items and align across rows), and hover highlight works via `:not(:first-child):hover > .md-table-cell`. Perfect column alignment, native horizontal scroll, no table quirks.

---

## Tech stack

| Layer | What |
|---|---|
| Markup | HTML5, single file |
| Styling | Vanilla CSS with custom properties for theming |
| Scripting | Vanilla ES2020, no framework, no bundler |
| Storage | `localStorage` |
| Streaming | `fetch` + `ReadableStream` + `TextDecoder` |
| Tables | CSS Grid + `display: contents` |
| Decorations | Inline SVG (webs, spider, skyline, emblem) |

Zero dependencies. Zero build.

---

## File structure

```
spider-chat/
├── index.html        ← the whole app
├── README.md         ← this file
├── LICENSE           ← MIT
└── screenshots/      ← optional
    ├── dark.png
    ├── light.png
    ├── table.png
    └── actions.png
```

---

## Roadmap

- [ ] Conversation export to JSON / Markdown
- [ ] System prompt library with presets
- [ ] Multiple API keys per provider
- [ ] Model switcher in the top bar
- [ ] Attach whole folders (File System Access API)
- [ ] Syntax highlighting for code blocks
- [ ] Optional PWA install
- [ ] Optional Kraven / Venom / Gwen themes

PRs welcome.

---

## Browser support

Tested on the latest Chrome, Edge, Firefox, and Safari.

Requires:
- `ReadableStream` for streaming (all modern browsers)
- `navigator.clipboard` for the copy button (falls back to `execCommand`)
- `localStorage` for persistence

---

## Contributing

1. Fork the repo
2. Create a branch: `git checkout -b feature/your-thing`
3. Commit: `git commit -m "Add your thing"`
4. Push: `git push origin feature/your-thing`
5. Open a Pull Request

Please keep the single-file constraint — no build step, no npm.

---

## License

MIT — see [LICENSE](LICENSE).

---

## Credits & disclaimer

- **Spider-Man, Spider-Verse, and all related characters are trademarks of Marvel / Sony.** This is an unofficial fan project built for fun. No Marvel assets are shipped — all decorations are hand-drawn SVGs.
- Layout inspired by Microsoft Copilot's chat interface.
- API integration works with [OpenRouter](https://openrouter.ai) and [DeepSeek](https://api.deepseek.com).

**Not affiliated with Marvel, Sony, Microsoft, OpenRouter, or DeepSeek.**

---

<div align="center">

Made with 🕸️ and a lot of CSS.

*With great power comes great responsibility — use your API key wisely.*

</div>
