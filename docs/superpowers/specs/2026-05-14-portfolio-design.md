# Single-Page Portfolio — Design Spec

**Project**: Jeric John Carillo — AI Systems Engineer & Full Stack Developer portfolio
**Date**: 2026-05-14
**Source content**: `portfolio.md`

## Goal

Produce a modern, single-page web portfolio that positions Jeric as an AI Systems Engineer & Full Stack Developer with 8+ years of experience. Delivers content from `portfolio.md` with strong visual personality, fast load, and zero build step.

## Decisions

| Decision | Choice |
|----------|--------|
| Visual style | Dark + Neon Glow (indigo→purple gradient accents on near-black bg) |
| Tech stack | Single `index.html`, Tailwind CDN, vanilla JS |
| Styling approach | Tailwind via CDN + small inline `<style>` for custom keyframes / gradients |
| Sections | Hero → About → Core Expertise → Experience → Featured Projects → Contact (Education dropped) |
| Animation level | Subtle — fade-up on scroll, hover lifts, hero glow, typed tagline |
| Deployment | Static — open `index.html` directly, or drop on GitHub Pages / Netlify |

## Visual System

### Color tokens

| Role | Hex | Notes |
|------|-----|-------|
| Background base | `#0a0a0f` | Near-black with subtle noise |
| Surface (card) | `rgba(255,255,255,0.03)` | Glassy on bg |
| Border | `rgba(255,255,255,0.10)` | Hairline |
| Text primary | `#e6e6ff` | High contrast (8:1 on bg) |
| Text muted | `#94a3b8` | Slate-400, ≥4.5:1 |
| Accent gradient | `#818cf8 → #c084fc → #f0abfc` | Indigo → purple → pink |
| Focus ring | `#a5b4fc` | Indigo-300 |
| Success | `#22c55e` | Used for "available" pulse dot |

### Typography
- **Inter** (300/400/500/600/700) — primary UI font (Google Fonts)
- **JetBrains Mono** (400/500) — tech tags, code-feel labels
- Heading scale: clamp() for fluid: H1 40→72px, H2 28→44px, H3 20→28px
- Body: 16px base, 1.6 line-height

### Effects
- Hero radial glow: 600×600 indigo radial-gradient, top-right offset, animated opacity 0.4 ↔ 0.7 (8s ease-in-out, infinite)
- Card hover: `translate-y(-4px)` + indigo-tinted shadow ring (250ms ease-out)
- Gradient text on key headings via `background-clip: text`
- `prefers-reduced-motion` disables all transitions/animations

## Page Structure

### 1. Sticky navigation
- Fixed top, blur-backdrop (`bg-black/40 backdrop-blur-xl`), 64px tall
- Left: monogram `JC` + name
- Right (desktop): anchor links — About · Stack · Experience · Projects · Contact
- Mobile: hamburger → slide-down sheet
- Active section highlighted via IntersectionObserver

### 2. Hero
- Full viewport min-height (`min-h-dvh`)
- Radial gradient glow bg (animated)
- Eyebrow chip: green pulse dot + "Available for new projects · CDO, Philippines"
- H1: "Building **[gradient: AI-Powered]** Platforms, Automations & Scalable Applications"
- Rotating subtitle (typed effect): cycles "AI Integration", "SaaS Development", "Backend Systems", "Workflow Automation"
- Brief description paragraph (from md)
- 4 info pills: 8+ Years · AI & Automation · SaaS & Backend · CDO Philippines
- Primary CTA: "View GitHub →" linking to github.com/jerixhub
- Secondary CTA: "Email Me" linking to mailto

### 3. About
- 2-column layout (desktop), stacked (mobile)
- Left: heading + paragraph + supporting paragraph (AI-assisted workflow line)
- Right: bullet grid of 8 specialties from md, each with checkmark icon

### 4. Core Expertise
- Section heading + lead-in
- 5 cards grid (responsive: 1/2/3 cols)
- Each card: icon (SVG inline), category name, pill tag list
  - AI & Automation (8 tags)
  - Frontend Development (8 tags)
  - Backend Development (10 tags)
  - Mobile Development (3 tags)
  - Database & DevOps (6 tags)

### 5. Experience
- Section heading
- Vertical timeline with left-side gradient rail, dots at each entry
- 7 entries (reverse chronological): Digitize, Spondise, Pic-a-talk, Avazoo, AppTech, Techcoin/Flowers, NexusBond
- Each entry: company + role + date range + bullet list

### 6. Featured Projects
- Section heading + lead-in
- Grid of 10 cards (1/2/3 cols responsive)
- Each card: gradient-tinted index number, project name, blurb, external-link icon, anchor entire card to URL
- Hover: lift + glow ring

### 7. Contact
- Centered block
- Large gradient heading: "Let's build modern software that scales"
- Lead paragraph from md
- 3 contact methods (cards or pills): email, phone, location
- Primary CTA: "Email Me"
- Secondary: "View GitHub"
- Footer line: © 2026 Jeric John Carillo · Built with Claude Code

## Behavior

### Scroll reveal
IntersectionObserver: each `[data-reveal]` section translates from `translateY(20px) opacity-0` to `translateY(0) opacity-100` over 600ms ease-out. Triggers once per element when 12% visible.

### Typed tagline
~30 lines vanilla JS: iterate subtitle word array, type at 60ms/char, pause 1500ms, backspace 30ms/char, advance.

### Mobile nav
Tap hamburger → open sheet with anchor links. Auto-close on link tap. ESC closes.

### Accessibility
- Semantic landmarks: `<header><nav>`, `<main>`, `<section>`, `<footer>`
- `aria-current` on active nav link
- All interactive elements keyboard-reachable, 2px focus ring
- `prefers-reduced-motion: reduce` cascade disables animations
- Color contrast: text ≥4.5:1, headings ≥7:1
- All links/buttons have accessible names
- `role="region"` + `aria-labelledby` on sections

### Performance
- Tailwind via CDN with `?plugins=forms` only if needed (none — skip)
- Font preconnect to fonts.googleapis.com
- `loading="lazy"` n/a (no images)
- SVG icons inline (no extra requests)
- Total JS ~3kb (vanilla), CSS ~12kb (Tailwind tree-shaken via CDN play mode minimal)

## File output

```
C:\Users\Jerixhub\Documents\portfolio\
├── index.html        ← entire site, single file
├── portfolio.md      ← source content (already exists)
└── docs/
    └── superpowers/
        └── specs/
            └── 2026-05-14-portfolio-design.md  ← this file
```

## Out of scope

- Education section (dropped per user)
- Project screenshots (no images available in md — placeholder gradients used)
- Multi-page routing
- Server-side rendering
- CMS / dynamic data
- Form handling (contact uses `mailto:`)
- Analytics
- Dark/light toggle (dark only, per design choice)
