[README.md](https://github.com/user-attachments/files/32155515/README.md)
# storygen# InkBloom Atelier — Storybook Generator

> **Turn a single idea into a full illustrated storybook in 30 seconds.**
> Powered by **Pollinations (FLUX)** for art + **Nvidia Nemotron 3 Ultra 550B** via OpenRouter for storytelling.

![License: MIT](https://img.shields.io/badge/License-MIT-pink.svg)
![Model: Nemotron 3 Ultra](https://img.shields.io/badge/Model-nemotron--3--ultra--550b-blue)
![Images: Pollinations](https://img.shields.io/badge/Images-Pollinations%20FLUX-orange)
![PDF: jsPDF](https://img.shields.io/badge/PDF-jsPDF-green)

Live Demo → Open the artifact in this chat or deploy `/src` to Vercel.

---

### ✨ Features

**Story Engine v2**
- 📖 4 / 6 / 8 page storybooks with title page, dedication, TOC, and back cover moral
- 🧠 Uses `nvidia/nemotron-3-ultra-550b-a55b:free` via OpenRouter — fast, creative, long-context
- 🌍 Multi-language: English, Spanish, French, Korean, Japanese
- 🎭 Character Builder: species (human, mouse, dragon, robot, cat, fox) + personality (brave, shy, curious, funny)
- 💌 Dedication & Author name → inside cover

**Art Studio**
- 🎨 6 art styles: Watercolor Wonder, Pixar 3D, Ghibli Magic, Paper Cutout, Vintage Storybook, Cyberpunk Kids
- 🖼️ Pollinations API: `https://image.pollinations.ai/prompt/{prompt}?model=flux&nologo=true`
- ✏️ Editable image prompts, seed lock, regenerate per page, 1024→1536 upscale

**Book Experience**
- 📚 Flip-book viewer with curl animation, dots, keyboard arrows
- 🔊 Read Aloud (Web Speech API) — single page or full book, voice + speed control
- 🗂️ History Shelf — last 12 books in localStorage
- 🌗 Dark / Light theme, confetti on first gen

**Exports**
- **Real PDF** — built with `jspdf` client-side (A4, cover + images + serif text + page numbers)
- JSON (full book object)
- Download All Images
- Copy Text

---

### 🏗️ Tech Stack

- **Frontend:** React + Tailwind CSS (single file artifact, no build step needed for demo)
- **LLM:** OpenRouter API `POST /api/v1/chat/completions` → `nvidia/nemotron-3-ultra-550b-a55b:free`
- **Images:** Pollinations FLUX (no key required)
- **PDF:** `jspdf@2.5.1` via CDN — images converted to base64 via canvas to avoid CORS
- **Storage:** localStorage for history + API key (opt-in)

---

### 🚀 Quick Start

```bash
git clone https://github.com/yourusername/inkbloom-atelier.git
cd inkbloom-atelier
# If you used the Vite version:
npm install
npm run dev
# Or just open index.html if using the single-file artifact
```

#### 1. Get an OpenRouter Key
1. Go to https://openrouter.ai/keys
2. Create a free key
3. Paste it in the app → Settings → OpenRouter API Key
   - Free model used: `nvidia/nemotron-3-ultra-550b-a55b:free` — no credits needed

#### 2. Environment (for Vite/Next version)
Create `.env.local`:
```
VITE_OPENROUTER_MODEL=nvidia/nemotron-3-ultra-550b-a55b:free
# Optional: proxy to avoid exposing key
VITE_OPENROUTER_API_KEY=sk-or-v1-...
```

---

### 🔌 API Usage

**Text Generation:**
```js
const res = await fetch("https://openrouter.ai/api/v1/chat/completions", {
  method: "POST",
  headers: {
    "Authorization": `Bearer ${apiKey}`,
    "Content-Type": "application/json",
    "HTTP-Referer": "https://inkbloom.app",
    "X-Title": "InkBloom Atelier"
  },
  body: JSON.stringify({
    model: "nvidia/nemotron-3-ultra-550b-a55b:free",
    temperature: 0.9,
    messages: [
      { role: "system", content: SYSTEM_PROMPT },
      { role: "user", content: userPrompt }
    ]
  })
});
```

System prompt returns strict JSON:
```json
{
  "title": "...",
  "moral": "...",
  "pages": [
    { "text": "60-80 words...", "imagePrompt": "detailed visual prompt..." }
  ]
}
```

**Image Generation:**
```js
const url = `https://image.pollinations.ai/prompt/${encodeURIComponent(imagePrompt + ', ' + artStyle + ' children book illustration')}?width=1024&height=1024&seed=${seed}&model=flux&nologo=true&enhance=true`;
```

---

### 📄 PDF Export Logic

```js
import { jsPDF } from "jspdf";

async function toDataURL(imgUrl) {
  const img = new Image();
  img.crossOrigin = "anonymous";
  img.src = imgUrl;
  await img.decode();
  const canvas = document.createElement('canvas');
  canvas.width = img.width; canvas.height = img.height;
  canvas.getContext('2d').drawImage(img,0,0);
  return canvas.toDataURL('image/jpeg', 0.85);
}

const pdf = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'a4' });
// Add cover, dedication, then loop pages: addImage + addText + addPage
pdf.save(`${title}.pdf`);
```

No server needed — fully client-side.

---

### 🗺️ Roadmap

- [ ] Streaming text (OpenRouter SSE)
- [ ] ZIP export of images
- [ ] Drawing-to-story: upload kid's doodle → vision model → story
- [ ] Voice cloning narration
- [ ] Shareable link with encoded book
- [ ] PWA + offline

---

### 🤝 Contributing

PRs welcome! Please:
1. Fork
2. `git checkout -b feature/amazing`
3. Commit
4. Open PR

---

### 📜 License

MIT © 2026 InkBloom

> Built with love for parents, teachers, and little dreamers. If you build a book with your kid, tag #InkBloom — we'd love to see it!

---

### 🙏 Credits

- Story: NVIDIA Nemotron 3 Ultra 550B via OpenRouter
- Art: Pollinations FLUX
- PDF: jsPDF
- Fonts: Fraunces & Plus Jakarta Sans
