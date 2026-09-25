# Handoff: Portfolio "Trace" redesign — saganchowdhury.com

## Overview
A single-page redesign of Sagan Chowdhury's portfolio. The visual concept is "the site as a run log": a dark terminal aesthetic that echoes his Reliflow project (JSONL traces, CLI). The page has these sections: sticky header, hero, selected work (expandable rows), playground, about + experience + resume, and contact with an interactive mini-shell.

Target repo: `Saganwazed/portfilio-1.0` (Next.js App Router, Tailwind, moving from Cloudflare Pages to **Vercel**). Once the work is implemented and the **Deployment checklist** at the end of this file passes, `git push origin main` deploys it.

## About the Design Files
`Portfolio Trace.dc.html` is a **design reference built in HTML**. It is a working prototype of the intended look and behavior, not production code to copy. Open it directly in a browser (it needs `support.js` beside it). Recreate it in the existing Next.js + Tailwind codebase using its patterns: App Router pages, `next/image`, `next/font`, and client components where interactivity is needed.

## Fidelity
**High-fidelity.** Colors, type, spacing, copy and interactions are final. Recreate them pixel-accurately.

## Implementation plan for the repo
1. Replace `app/page.tsx` with the new single page. Build it from sections: `Header`, `Hero`, `WorkList`, `Playground`, `About`, `Contact`, `Shell`.
2. Remove the old home-page dependencies, `FloatingDockNav`, `SparklesPreview` and `DecryptedText`, if nothing else uses them. Keep `/blog/[slug]`, which the Playground "Blog & weeklies" card can link to.
3. `app/about`, `app/resume` and `app/contact` can redirect to `/#about`, `/#about` and `/#contact` respectively, or stay as they are.
4. Put `resume.pdf` at `public/resume.pdf`. The existing `/resume` page already expects that path. `public/images/about.jpg` already exists.
5. Load the fonts with `next/font/google`: **JetBrains Mono** (400/500/700) and **IBM Plex Sans** (400/500). Update the Tailwind `fontFamily` to `mono` = JetBrains Mono and `sans` = IBM Plex Sans.
6. Update `app/layout.tsx` metadata: the title is "Sagan Chowdhury", and the description is taken from the hero paragraph.

## Screen: Home (single page)

Global: the page background is `#0c0d0c` and the base text is `#e6e8e3`. Horizontal padding is `clamp(20px, 4vw, 48px)`. Content blocks have a max width of 1280px (hero). Set `html { scroll-behavior: smooth }`. Anchor targets use `scroll-margin-top: 60px`. The `::selection` background is the accent and the selection text is `#0c0d0c`.

### Header (sticky)
- `position: sticky; top: 0; z-index: 40`. Background `rgba(12,13,12,.88)` with `backdrop-filter: blur(8px)`. Bottom border `1px solid #222522`. Padding `18px` vertical. Flex, space-between, wraps with a 12px/28px gap.
- Font: JetBrains Mono 13px in `#8b918a`.
- Left: an 8px accent dot, then `sagan@san-francisco:~`, then a live clock `HH:MM:SS PT` in `#e6e8e3`. The clock uses 24-hour time, the `America/Los_Angeles` time zone, and updates every second.
- Right nav (8px/24px gap, wraps): `./work` → `#work`, `./playground`, `./about`, `resume.pdf` (opens `/resume.pdf` in a new tab), `./contact`. On hover the color changes to `#e6e8e3`.

### Hero
- Padding: top `clamp(64px, 12vw, 140px)`, bottom `clamp(64px, 8vw, 96px)`. Column layout with a 32px gap.
- **Boot log** (min-height 88px, JetBrains Mono 13px, `#8b918a`). The four lines appear one at a time every 380ms, and each is prefixed with `›` in the accent color. The last line's prefix is `✓`.
  1. `resolving plan … 3 projects, 3 experiments`
  2. `cost guard: $0.00 / visit`
  3. `loading portrait … ok`
  4. `ready.`
- **Name h1**: `SAGAN_CHOWDHURY` in JetBrains Mono 700, `clamp(40px, 8.6vw, 112px)`, line-height .95, letter-spacing -0.04em, `overflow-wrap: anywhere`.
  - Scramble effect: on mount, characters not yet revealed show random glyphs from `01<>/{}[]#$%&*+=?`. Underscores never scramble. The reveal pointer advances 0.34 characters per 40ms tick, so the full reveal takes about 1.8s. The effect replays on `mouseenter`.
  - Respect `prefers-reduced-motion` by showing the name immediately.
- **Intro row** (flex, wrap, gap 40px/64px):
  - Paragraph: flex `1 1 380px`, max-width 600px, IBM Plex Sans `clamp(18px, 1.8vw, 22px)`, line-height 1.5, `#c9ccc6`. Copy: "CS student at CSU East Bay and AI research assistant. I build reliable LLM systems and vibe code for fun, as a creative outlet to explore ideas and build things that feel uniquely mine."
  - Key/value list: flex `1 1 300px`, max-width 440px, JetBrains Mono 14px. Keys are `#8b918a` and values `#e6e8e3`. Rows are separated by `1px dashed #2b2f2b` with 10px padding. The rows are:
    - `status` / `● building reliflow` (the value is in the accent color)
    - `focus` / `agents · evals · local LLMs`
    - `based` / `Bay Area, CA`

### Selected work (`#work`)
- Section label bar: JetBrains Mono 13px `#8b918a`, bottom border `1px solid #2b2f2b`, 14px bottom padding. Left text `// selected work`, right text `press 1–3 to open`.
- Each project is a row with a bottom border of `1px solid #222522`. The clickable header is flex and wraps. Gap 8px/24px, padding 26px 8px, margin 0 -8px. Hover background is `#121412`, cursor pointer. It contains:
  - Step `[01]`: 48px wide, mono 13px, `#8b918a`.
  - Name: flex basis 240px, mono 700, `clamp(22px, 2.4vw, 28px)`, letter-spacing -0.02em.
  - Kind: flex `1 1 220px`, Plex 18px, `#c9ccc6`.
  - Right-aligned group: status in mono 13px accent, then the `+`/`−` sign in mono 22px `#8b918a`.
- Expanded panel (only one open at a time; the first is open by default): flex wrap, gap 24px/40px, padding `0 0 36px 72px`.
  - Left column (`1 1 360px`): the blurb in Plex 18px/1.55 `#e6e8e3`, then the detail in 16px/1.55 `#8b918a`.
  - Right column (`1 1 300px`, mono 13px): a 16:9 screenshot box (border `1px solid #2b2f2b`; currently a striped placeholder, to be replaced with real screenshots). Below it, the stack as `{"stack": "…"}`. Then the link `→ view source on github` in the accent color, underlined on hover.
- Clicking a header toggles that row. Keys `1`–`3` toggle rows too, but are ignored while an input is focused.
- Suggested: animate the expand with a height/opacity transition of about 200ms ease-out. The prototype expands instantly.

The three projects, with exact copy:
| # | Name | Kind | Status | Stack |
|---|---|---|---|---|
| 01 | Reliflow | Agent orchestration framework | in progress | Python · Pydantic v2 · LiteLLM · SQLite · Typer |
| 02 | Candor | AI news bias detection | deployed | TypeScript · Next.js · Anthropic API · Supabase · Zod |
| 03 | Alaka | Local LLM desktop client | daily driver | TypeScript · React · Electron · Ollama |

- Reliflow blurb: "A pip-installable framework for multi-step LLM agent workflows, with mandatory reflexion, LLM-verified plans and hard cost guards." Detail: "Event-sourced state over SQLite with time-travel replay and a JSONL trace emitter. Install to completed run in under 15 minutes."
- Candor blurb: "Real-time rhetorical and bias analysis of any news article URL, end to end in under 10 seconds." Detail: "Serverless extraction with Readability + JSDOM on Vercel edge. Schema-validated output via structured prompting, Zod and retry on parse failure."
- Alaka blurb: "A cross-platform desktop app that runs LLMs fully offline via Ollama, with streaming chat and image attachments." Detail: "Standalone macOS and Windows binaries with auto-detection of the Ollama runtime. Still in regular use."
- The GitHub links currently point to `https://github.com/Saganwazed`. Replace them with each repo's URL.

### Playground (`#playground`)
- Label `// playground`, styled like the work label bar.
- Grid `repeat(auto-fit, minmax(260px, 1fr))` with 1px hairlines: the grid gap is 1px over a `#222522` background, with a bottom border in the same color.
- Cell: `#0c0d0c` background, padding 28px 24px 32px, column layout with a 12px gap. Hover background `#121412` with a .2s transition. Each cell has a tag (mono 13px, accent), a title (Plex 500, 22px) and a description (Plex 16px/1.5, `#8b918a`):
  - `models` / Fine-tuned LLMs / "Small models fine-tuned and run locally on my own devices."
  - `hardware` / Raspberry Pi / "Home experiments with Pis and self-hosted tools."
  - `writing` / Blog & weeklies / "How I built this site, plus weekly notes on what I’m learning." Link this card to the blog.

### About (`#about`)
Flex, wraps, 48px gap. It has three children:
1. Portrait `/images/about.jpg`: flex `0 1 320px`, min-width 240px, aspect ratio 4/5, `object-fit: cover`, border `1px solid #2b2f2b`. Filter `grayscale(1) contrast(1.1)`, which changes to `none` on hover.
2. Text (`1 1 380px`, 18px gap):
   - Label `// about`.
   - Paragraph in Plex 20px/1.55 `#e6e8e3`: "I'm a Computer Science student at CSU East Bay, deeply curious about how things work, especially in tech. I spend a lot of time learning, experimenting, playing with Raspberry Pis and pushing myself to grow."
   - Paragraph in 18px `#8b918a`: "My main passion is artificial intelligence. I've fine-tuned a few large language models and enjoy running them locally on my own devices."
3. Experience (`1 1 300px`, max 400px, mono 13px):
   - Label `// experience`.
   - Each row has a 1px `#222522` top border and 14px padding. The role is 14px `#e6e8e3`, and the line below reads `org · dates` in `#8b918a`:
     - AI Research Assistant — CSU East Bay · Mar 2026 – Now
     - Software & Systems Intern — Women's Cancer Resource Center · May – Aug 2026
     - Teaching Assistant, Python — City College of SF · Jan – May 2024
     - B.S. Computer Science · GPA 4.0 — CSU East Bay · 2025 – 2027
   - Download button: a `/resume.pdf` link with the `download` attribute. Margin-top 14px, padding 14px 16px, 1px accent border, accent text. Label `resume.pdf` on the left and `↓` on the right. On hover the background becomes the accent and the text `#0c0d0c`.

### Contact (`#contact`, footer)
- Padding: top `clamp(56px, 7vw, 80px)`, bottom 48px. 1px `#222522` top border. Column layout with a 36px gap.
- Label: `// open to collaborations, chit chats, and opportunities`.
- Email button `saganwc@gmail.com`: mono 700, `clamp(28px, 5.4vw, 64px)`, letter-spacing -0.03em. On hover it turns the accent color. Clicking copies the address to the clipboard, and the inline hint (14px `#8b918a`) changes from `click to copy` to `copied ✓` for 1.8s. Include a fallback for when the clipboard is unavailable, like the old `FloatingDockNav`.
- **Interactive shell**:
  - Box: border `1px solid #2b2f2b`, background `#090a09`, padding 18px 20px, mono 14px/1.7, max-width 880px. Clicking anywhere in the box focuses the input with `preventScroll`.
  - Header line: `interactive shell · type help`.
  - Input row: an accent `$`, then a transparent input with no border and an accent caret.
  - On Enter, the command is echoed as `$ <cmd>` with the output lines below. Keep the last 14 lines. Commands are case-insensitive:
    - `help` → `whoami · ls · open <project> · resume · email · socials · clear`
    - `whoami` → `sagan chowdhury — cs @ csu east bay, ai research assistant`
    - `ls` / `ls projects` → `reliflow   candor   alaka`
    - `open <key>` → prints `opening <key> …`, expands that project and smooth-scrolls to `#work` (offset 60px). An unknown key prints `open: no such project. try ls`.
    - `resume` → prints `opening resume.pdf …` and opens `/resume.pdf` in a new tab.
    - `email` → copies the email and prints `saganwc@gmail.com copied to clipboard`.
    - `socials` → one line per link, printed as `label: url-without-https`.
    - `sudo hire sagan` → `permission granted. email me →  saganwc@gmail.com` (easter egg).
    - `clear` → empties the output.
    - Anything else → `command not found: <cmd>. type help`.
- Bottom row (mono 13px `#8b918a`, flex wrap, space-between):
  - Left: links `github ↗`, `linkedin ↗`, `x ↗` and `instagram ↗`, each opening in a new tab with `rel="noopener noreferrer"`. They change to `#e6e8e3` on hover.
    - github: https://github.com/Saganwazed
    - linkedin: https://www.linkedin.com/in/saganwc
    - x: https://x.com/saganwc
    - instagram: https://instagram.com/saganchowdhury
  - Right: `© 2026 sagan chowdhury · exit 0`.

## Optional effects (prototype tweak toggles)
- `scanlines` (default off): a fixed full-viewport overlay with `pointer-events: none`, z-index 50, and background `repeating-linear-gradient(0deg, rgba(255,255,255,.025) 0 1px, transparent 1px 3px)`.
- `scramble` (default on): the name scramble effect.
- `bootSequence` (default on): the typed boot log.
- `expandAll` (default off): all project rows open.

The chosen settings are scanlines off, scramble on and expandAll off. Ship those as hard-coded defaults. There's no need for runtime toggles.

## State (client component)
- `now: Date`, updated every 1s, drives the clock.
- `reveal: number` for the name scramble.
- `bootCount: number` for the boot log.
- `openIndex: number | -1`, default 0.
- `copied: boolean`, reset after 1.8s.
- `shellLines: {prompt, text}[]` and `shellInput: string`.
- A global `keydown` listener handles the 1–3 shortcuts. Clean up all intervals and listeners on unmount.
- Avoid a hydration mismatch in the clock and the scramble: render the static values on the server (the plain name and `--:--:--`), then start the effects in `useEffect`.

## Design tokens
| Token | Value | Use |
|---|---|---|
| bg | `#0c0d0c` | page |
| bg-raised | `#121412` | row/card hover |
| bg-sunken | `#090a09` | shell |
| line | `#222522` | dividers |
| line-strong | `#2b2f2b` | section rules, image border, dashed kv |
| ink | `#e6e8e3` | primary text |
| ink-2 | `#c9ccc6` | secondary text |
| muted | `#8b918a` | labels, meta |
| accent | `oklch(0.82 0.17 145)`, about `#6ee37a` as a fallback hex | status, links, prompt, dot |

- Fonts: JetBrains Mono (400/500/700) for UI, labels and headings; IBM Plex Sans (400/500) for body text.
- Type scale: 13, 14, 16, 18, 20, 22, 28px, plus the fluid sizes noted above.
- Radius: none; everything is square. There are no shadows.

## Assets
- `public/images/about.jpg`, the portrait, already in the repo.
- `resume.pdf`, the user's current resume. Place it at `public/resume.pdf`.
- Project screenshots are still to be supplied: Reliflow trace CLI, Candor article analysis view, Alaka chat window. Each is 16:9.

## Files in this bundle
- `Portfolio Trace.dc.html`: the hi-fi prototype. Open it in a browser alongside `support.js`.
- `support.js`: the runtime the prototype needs. It is only for viewing and should not be ported.
- `public/images/about.jpg`, `resume.pdf`: the assets.


## Deployment checklist: Vercel (must pass before pushing to main)

Hosting moves from Cloudflare Pages to **Vercel Hobby** (free). Vercel builds and deploys on every push to `main` with no workflow file. Domain: `www.saganchowdhury.com` (canonical).

### 1. Remove Cloudflare setup
- Delete `.github/workflows/deploy.yml` (otherwise every push also runs a failing Cloudflare deploy).
- Delete `wrangler.toml`, `deploy.sh`, and `public/_headers` if present.
- `package.json`: remove the `deploy`, `preview`, `cf:login`, `cf:deploy`, `cf:tail` scripts and the `wrangler` devDependency. Run `npm install` so `package-lock.json` updates.

### 2. Simplify `next.config.js`
Custom `distDir: 'out'` breaks Vercel builds. Replace the whole file with:
```js
/** @type {import('next').NextConfig} */
const nextConfig = { poweredByHeader: false };
module.exports = nextConfig;
```
Drop `output: 'export'`, `distDir`, `trailingSlash`, `images.unoptimized`, and `outputFileTracingRoot`. Vercel runs Next natively and optimises images.

### 3. Build safety
- Mark the page component `'use client'`.
- Only use browser APIs (`window`, `document`, `navigator.clipboard`, `localStorage`, `matchMedia`, key listeners for the 1–3 shortcuts, command box, scanline toggle, boot animation) inside `useEffect` or event handlers. Otherwise prerendering crashes the build.
- Delete old routes the single-page redesign replaces, or make sure they still build.

### 4. Asset paths (absolute)
- Photo: `/images/about.jpg` (file at `public/images/about.jpg`). Use `next/image`.
- Resume: `/resume.pdf` (copy `resume.pdf` from this package to `public/resume.pdf`). Update the download link and the shell's `resume` command.
- Fonts: JetBrains Mono + IBM Plex Sans via `next/font/google`.

### 5. Domain placeholders
- `scripts/generate-sitemap.js`: `DOMAIN = 'https://www.saganchowdhury.com'`, and set `staticPages` to routes that exist (probably just `''`).
- `public/robots.txt`: `Sitemap: https://www.saganchowdhury.com/sitemap.xml`
- `app/layout.tsx`: `metadataBase: new URL('https://www.saganchowdhury.com')`, plus title, description, and an Open Graph image.

### 6. Verify locally before pushing
```bash
npm install
npm run build && npm start     # http://localhost:3000
```
Check the following:
- the build finishes with no errors
- the boot animation plays
- keys 1–3 open projects
- the command box runs `help`, `resume`, and `email`
- the scanline toggle works
- the resume downloads and the photo loads
- the mobile width (375px) looks right
- the console is clean

### 7. One-time Vercel setup (owner, in the browser)
1. vercel.com → sign up with GitHub → **Add New → Project** → import `Saganwazed/portfilio-1.0` → **Deploy** (defaults are correct).
2. Project → **Settings → Domains** → add `saganchowdhury.com` and `www.saganchowdhury.com` (set `www.saganchowdhury.com` as primary; the apex redirects to it). At the domain registrar, add the records Vercel shows (typically `A @ 76.76.21.21` and `CNAME www cname.vercel-dns.com`). HTTPS is automatic.
3. If the domain was previously attached to Cloudflare Pages, remove it there first.
4. After the next push, confirm the deployment is green in Vercel, open `https://www.saganchowdhury.com`, and repeat the checks in step 6.
