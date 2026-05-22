# Svetly Website Redesign — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rebuild svetly.shop as a polished one-page shop with cart & order system, responsive design, and smooth animations.

**Architecture:** Single `index.html` file with inline `<style>` and `<script>`. Mobile-first CSS with desktop overrides at 768px. Cart state in localStorage, order form sends to Telegram.

**Tech Stack:** HTML5, CSS3 (custom properties, grid, flexbox), vanilla JS (ES6+), Google Fonts, GitHub Pages.

**Spec:** `docs/superpowers/specs/2026-05-22-svetly-website-redesign-design.md`

---

## File Structure

Single file:
- **Modify:** `index.html` — complete rewrite, all HTML + CSS + JS inline

Supporting (unchanged):
- `CNAME` — svetly.shop domain (already exists, don't touch)

---

### Task 1: Foundation — HTML head, CSS variables, reset, body

**Files:**
- Modify: `index.html` (complete rewrite — replace entire file)

Start the file from scratch. This task creates the boilerplate that all subsequent tasks build upon.

- [ ] **Step 1: Write the HTML boilerplate with head, CSS variables, and body reset**

Replace the entire `index.html` with:

```html
<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="color-scheme" content="light only">
<title>Svetly — Христианская печатная продукция</title>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,300;1,400;1,500&family=Lora:ital,wght@0,400;0,500;1,400&family=Nunito:wght@300;400;500&display=swap" rel="stylesheet">
<style>
:root {
  --cream: #F7F2E8;
  --cream-dark: #EDE5D4;
  --parchment: #E8DCC8;
  --sage: #8A9E7A;
  --sage-light: #B5C4A8;
  --sage-dark: #5C7250;
  --terra: #C4845A;
  --dusty-rose: #D4A89A;
  --ink: #2C2018;
  --ink-soft: #5C4A38;
  --deep-sage: #2A3328;
  --max-w: 1200px;
}

* { margin: 0; padding: 0; box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

html {
  scroll-behavior: smooth;
  color-scheme: light only;
}

body {
  background: var(--cream);
  color: var(--ink);
  font-family: 'Nunito', sans-serif;
  overflow-x: hidden;
  font-size: 15px;
  -webkit-text-size-adjust: 100%;
}

/* Paper texture overlay */
body::before {
  content: '';
  position: fixed;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
  pointer-events: none;
  z-index: 999;
  opacity: 0.5;
}

/* Shared layout */
.container {
  max-width: var(--max-w);
  margin: 0 auto;
  padding: 0 20px;
}

/* Animations */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}

.reveal {
  opacity: 0;
  transform: translateY(24px);
  transition: all 0.7s ease;
}

.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}
</style>
</head>
<body>

<!-- Content will be added in subsequent tasks -->

</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

Open `index.html` in a browser. Verify:
- Cream background (#F7F2E8) fills the page
- Subtle paper texture visible (faint noise pattern)
- No console errors
- Google Fonts load (check Network tab)

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: foundation — HTML boilerplate, CSS variables, paper texture"
```

---

### Task 2: Navbar + Mobile Menu

**Files:**
- Modify: `index.html` — add nav CSS (inside `<style>`) and nav HTML + JS (inside `<body>`)

- [ ] **Step 1: Add navbar CSS to the `<style>` block**

Add after the `.reveal.visible` rule, before `</style>`:

```css
/* ── NAV ── */
nav {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 200;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 20px;
  background: rgba(247,242,232,0.96);
  backdrop-filter: blur(14px);
  -webkit-backdrop-filter: blur(14px);
  box-shadow: 0 1px 0 rgba(44,32,24,0.08);
  transition: all 0.4s;
}

nav.scrolled {
  background: rgba(247,242,232,0.98);
  box-shadow: 0 1px 16px rgba(44,32,24,0.09);
  padding: 12px 20px;
}

.nav-logo {
  font-family: 'Cormorant Garamond', serif;
  font-size: 26px;
  font-weight: 500;
  color: var(--ink);
  text-decoration: none;
  letter-spacing: 0.02em;
}

.nav-logo em { font-style: italic; color: var(--terra); }

.nav-links {
  display: none;
  list-style: none;
  gap: 32px;
  align-items: center;
}

.nav-links a {
  font-size: 14px;
  color: var(--ink-soft);
  text-decoration: none;
  transition: color 0.3s;
  padding-bottom: 2px;
}

.nav-links a:hover { color: var(--ink); border-bottom: 2px solid var(--terra); }

.nav-right { display: flex; align-items: center; gap: 12px; }

.nav-cart-btn {
  display: flex;
  align-items: center;
  gap: 6px;
  background: var(--ink);
  color: var(--cream);
  border: none;
  border-radius: 30px;
  padding: 9px 18px;
  font-size: 13px;
  cursor: pointer;
  font-family: 'Nunito', sans-serif;
  transition: background 0.3s;
}

.nav-cart-btn:hover { background: var(--terra); }

.cart-badge {
  background: var(--terra);
  color: white;
  width: 20px; height: 20px;
  border-radius: 50%;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-size: 11px;
  font-weight: 600;
}

.nav-menu-btn {
  display: flex;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
}

.nav-menu-btn span {
  display: block;
  width: 22px;
  height: 1.5px;
  background: var(--ink);
  border-radius: 2px;
  transition: all 0.3s;
}

/* ── MOBILE MENU ── */
.mobile-menu {
  position: fixed;
  inset: 0;
  background: var(--cream);
  z-index: 300;
  display: flex;
  flex-direction: column;
  padding: 80px 32px 40px;
  transform: translateX(100%);
  transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
}

.mobile-menu.open { transform: translateX(0); }

.menu-close {
  position: absolute;
  top: 18px; right: 20px;
  background: none;
  border: none;
  font-size: 28px;
  color: var(--ink-soft);
  cursor: pointer;
  line-height: 1;
}

.menu-link {
  display: block;
  font-family: 'Cormorant Garamond', serif;
  font-size: 32px;
  font-weight: 400;
  color: var(--ink);
  text-decoration: none;
  padding: 12px 0;
  border-bottom: 1px solid var(--parchment);
  transition: color 0.3s;
}

.menu-link:hover { color: var(--terra); }

.menu-verse {
  margin-top: auto;
  font-family: 'Lora', serif;
  font-style: italic;
  font-size: 14px;
  color: var(--sage-dark);
  line-height: 1.6;
  padding-top: 32px;
}

/* ── DESKTOP NAV ── */
@media (min-width: 768px) {
  nav { padding: 20px 40px; }
  nav.scrolled { padding: 14px 40px; }
  .nav-links { display: flex; }
  .nav-menu-btn { display: none; }
}
```

- [ ] **Step 2: Add navbar and mobile menu HTML**

Replace the `<!-- Content will be added in subsequent tasks -->` comment with:

```html
<!-- NAV -->
<nav id="navbar">
  <a href="#" class="nav-logo">Svet<em>ly</em></a>
  <ul class="nav-links">
    <li><a href="#categories">Каталог</a></li>
    <li><a href="#about">О нас</a></li>
    <li><a href="#footer">Контакты</a></li>
  </ul>
  <div class="nav-right">
    <button class="nav-cart-btn" id="navCartBtn">🛒 <span class="cart-badge" id="navCartCount">0</span></button>
    <button class="nav-menu-btn" id="menuToggle" aria-label="Меню">
      <span></span><span></span><span></span>
    </button>
  </div>
</nav>

<!-- MOBILE MENU -->
<div class="mobile-menu" id="mobileMenu">
  <button class="menu-close" id="menuClose">×</button>
  <a href="#categories" class="menu-link" onclick="closeMobileMenu()">Каталог</a>
  <a href="#about" class="menu-link" onclick="closeMobileMenu()">О нас</a>
  <a href="#footer" class="menu-link" onclick="closeMobileMenu()">Контакты</a>
  <p class="menu-verse">«Слово Твоё — светильник ноге моей и свет стезе моей» · Пс. 118:105</p>
</div>
```

- [ ] **Step 3: Add nav JS before `</body>`**

```html
<script>
// Nav scroll behavior
const navbar = document.getElementById('navbar');
window.addEventListener('scroll', () => {
  navbar.classList.toggle('scrolled', window.scrollY > 40);
});

// Mobile menu
const menuToggle = document.getElementById('menuToggle');
const menuClose = document.getElementById('menuClose');
const mobileMenu = document.getElementById('mobileMenu');

menuToggle.addEventListener('click', () => {
  mobileMenu.classList.add('open');
  document.body.style.overflow = 'hidden';
});

function closeMobileMenu() {
  mobileMenu.classList.remove('open');
  document.body.style.overflow = '';
}

menuClose.addEventListener('click', closeMobileMenu);
</script>
```

- [ ] **Step 4: Verify in browser**

- Navbar visible at top, logo "Svetly" with italic terra "ly"
- Desktop (>768px): horizontal links visible, burger hidden
- Mobile (<768px): burger visible, links hidden
- Click burger → fullscreen menu slides in from right
- Click × → menu closes
- Scroll down → nav gets tighter padding and shadow

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: navbar with sticky scroll, mobile menu"
```

---

### Task 3: Hero Section

**Files:**
- Modify: `index.html` — add hero CSS + HTML

- [ ] **Step 1: Add hero CSS before the `@media` block**

```css
/* ── HERO ── */
.hero {
  min-height: 100svh;
  padding: 90px 20px 40px;
  display: flex;
  flex-direction: column;
  position: relative;
  overflow: hidden;
}

.hero-botanical {
  position: absolute;
  top: 0; right: -20px;
  width: 200px;
  opacity: 0.15;
  pointer-events: none;
}

.hero-botanical-left {
  position: absolute;
  bottom: 20px; left: -10px;
  width: 140px;
  opacity: 0.12;
  pointer-events: none;
}

.hero-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  background: var(--cream-dark);
  border: 1px solid var(--parchment);
  border-radius: 30px;
  padding: 7px 16px;
  font-size: 11px;
  color: var(--ink-soft);
  letter-spacing: 0.1em;
  text-transform: uppercase;
  width: fit-content;
  margin-bottom: 24px;
  animation: fadeUp 0.7s ease both;
}

.hero-badge::before { content: '✦'; color: var(--terra); font-size: 9px; }

.hero-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 44px;
  font-weight: 400;
  line-height: 1.05;
  color: var(--ink);
  margin-bottom: 16px;
  animation: fadeUp 0.7s 0.1s ease both;
}

.hero-title em { font-style: italic; color: var(--terra); }

.hero-verse {
  font-family: 'Lora', serif;
  font-style: italic;
  font-size: 13px;
  color: var(--sage-dark);
  padding-left: 14px;
  border-left: 2px solid var(--sage-light);
  margin-bottom: 20px;
  line-height: 1.6;
  animation: fadeUp 0.7s 0.2s ease both;
}

.hero-desc {
  font-size: 15px;
  color: var(--ink-soft);
  line-height: 1.7;
  font-weight: 300;
  margin-bottom: 32px;
  animation: fadeUp 0.7s 0.3s ease both;
}

.hero-btns {
  display: flex;
  gap: 14px;
  align-items: center;
  flex-wrap: wrap;
  margin-bottom: 40px;
  animation: fadeUp 0.7s 0.4s ease both;
}

.btn-primary {
  background: var(--ink);
  color: var(--cream);
  padding: 14px 28px;
  border-radius: 40px;
  font-size: 13px;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  border: none;
  cursor: pointer;
  font-family: 'Nunito', sans-serif;
  text-decoration: none;
  display: inline-block;
  transition: background 0.3s;
}

.btn-primary:hover { background: var(--terra); }

.btn-ghost {
  font-size: 13px;
  color: var(--ink-soft);
  text-decoration: none;
  letter-spacing: 0.06em;
  display: flex;
  align-items: center;
  gap: 6px;
  transition: color 0.3s;
}

.btn-ghost::after { content: '→'; }
.btn-ghost:hover { color: var(--terra); }

/* Hero cards horizontal scroll (mobile) */
.hero-cards-wrap {
  margin: 0 -20px;
  overflow-x: auto;
  scrollbar-width: none;
  -ms-overflow-style: none;
  padding: 0 20px 16px;
  animation: fadeUp 0.7s 0.5s ease both;
}

.hero-cards-wrap::-webkit-scrollbar { display: none; }

.hero-cards {
  display: flex;
  gap: 14px;
  width: max-content;
}

.hero-card {
  width: 150px;
  background: white;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(44,32,24,0.1);
  flex-shrink: 0;
  transition: transform 0.3s;
  cursor: pointer;
}

.hero-card:hover { transform: translateY(-4px); }

.hc-img {
  width: 100%;
  aspect-ratio: 3/4;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32px;
}

.hc-info { padding: 10px 12px 12px; }

.hc-name {
  font-family: 'Cormorant Garamond', serif;
  font-size: 14px;
  font-weight: 500;
  color: var(--ink);
  margin-bottom: 2px;
}

.hc-price {
  font-size: 13px;
  color: var(--terra);
  font-weight: 500;
}

.scroll-hint {
  display: flex;
  align-items: center;
  gap: 6px;
  font-size: 11px;
  color: var(--ink-soft);
  letter-spacing: 0.08em;
  opacity: 0.5;
  padding-left: 20px;
  margin-bottom: 8px;
}

.scroll-hint::after {
  content: '';
  display: block;
  width: 24px;
  height: 1px;
  background: var(--ink-soft);
}
```

Add the hero desktop overrides inside the existing `@media (min-width: 768px)` block:

```css
  .hero {
    display: grid;
    grid-template-columns: 1fr 1fr;
    min-height: 100vh;
    padding: 0;
  }

  .hero-left {
    padding: 120px 60px 80px 80px;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .hero-cards-wrap {
    padding: 80px 40px 80px 20px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
    overflow: visible;
    height: 100%;
    align-content: center;
    margin: 0;
    background: var(--cream-dark);
  }

  .hero-cards { display: contents; }

  .hero-card { width: auto; }
  .hero-card:nth-child(1) { margin-top: 24px; }
  .hero-card:nth-child(4) { margin-top: -24px; }

  .hero-botanical { width: 260px; opacity: 0.2; }
  .hero-title { font-size: 68px; }
  .scroll-hint { display: none; }
```

- [ ] **Step 2: Add hero HTML after the mobile menu div**

```html
<!-- HERO -->
<section class="hero">
  <svg class="hero-botanical" viewBox="0 0 200 300" fill="none">
    <path d="M160 10 Q100 60 80 120 Q60 180 40 260" stroke="#5C7250" stroke-width="1.5" fill="none"/>
    <path d="M80 120 Q55 100 40 75" stroke="#5C7250" stroke-width="1" fill="none"/>
    <ellipse cx="55" cy="82" rx="18" ry="10" fill="#8A9E7A" transform="rotate(-30 55 82)" opacity="0.8"/>
    <ellipse cx="72" cy="125" rx="16" ry="9" fill="#8A9E7A" transform="rotate(15 72 125)" opacity="0.7"/>
    <ellipse cx="60" cy="168" rx="14" ry="8" fill="#A0B490" transform="rotate(-10 60 168)" opacity="0.6"/>
    <ellipse cx="48" cy="210" rx="12" ry="7" fill="#8A9E7A" transform="rotate(20 48 210)" opacity="0.5"/>
    <circle cx="158" cy="12" r="6" fill="#C4845A" opacity="0.7"/>
    <circle cx="38" cy="258" r="4" fill="#D4A89A" opacity="0.5"/>
  </svg>

  <svg class="hero-botanical-left" viewBox="0 0 150 200" fill="none">
    <path d="M10 200 Q50 150 70 100 Q90 60 120 10" stroke="#5C7250" stroke-width="1.2" fill="none"/>
    <ellipse cx="58" cy="135" rx="14" ry="8" fill="#8A9E7A" transform="rotate(25 58 135)" opacity="0.7"/>
    <ellipse cx="80" cy="95" rx="12" ry="7" fill="#A0B490" transform="rotate(-15 80 95)" opacity="0.6"/>
    <circle cx="118" cy="13" r="5" fill="#D4A89A" opacity="0.5"/>
  </svg>

  <div class="hero-left" style="position:relative;z-index:2;">
    <div class="hero-badge">Христианская полиграфия</div>
    <h1 class="hero-title">Слово Божье<br>в каждом<br><em>изделии</em></h1>
    <p class="hero-verse">«Слово Твоё — светильник ноге моей» — Пс. 118:105</p>
    <p class="hero-desc">Постеры, журналы, раскраски и наклейки для христианских семей. С любовью из Краснодарского края.</p>
    <div class="hero-btns">
      <a href="#products" class="btn-primary">Смотреть каталог</a>
      <a href="#about" class="btn-ghost">О нас</a>
    </div>
  </div>

  <div class="hero-cards-wrap">
    <div class="hero-cards">
      <div class="hero-card"><div class="hc-img" style="background:linear-gradient(135deg,#E8E0D0,#D4C8B0);">📓</div><div class="hc-info"><div class="hc-name">Блокнот «Свет»</div><div class="hc-price">520 ₽</div></div></div>
      <div class="hero-card"><div class="hc-img" style="background:linear-gradient(135deg,#D4E0CC,#B8CBA8);">🎨</div><div class="hc-info"><div class="hc-name">Раскраска «Сад»</div><div class="hc-price">320 ₽</div></div></div>
      <div class="hero-card"><div class="hc-img" style="background:linear-gradient(135deg,#E8D4C8,#D4A898);">✨</div><div class="hc-info"><div class="hc-name">Наклейки «Вера»</div><div class="hc-price">180 ₽</div></div></div>
      <div class="hero-card"><div class="hc-img" style="background:linear-gradient(135deg,#E0D8CC,#C8B898);">🖼</div><div class="hc-info"><div class="hc-name">Плакат «Псалом»</div><div class="hc-price">450 ₽</div></div></div>
    </div>
  </div>
</section>

<div class="scroll-hint"><span>листай</span></div>
```

- [ ] **Step 3: Verify in browser**

- Desktop: two-column layout, text left, cards grid right on cream-dark background
- Mobile: single column, text → horizontal scroll cards → "листай" hint
- Botanical SVG decorations visible as subtle semi-transparent branches
- fadeUp animations play on load
- Hero cards hover lifts them slightly

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: hero section — split layout, botanical SVGs, fadeUp"
```

---

### Task 4: Trust Pills + Scripture Quote

**Files:**
- Modify: `index.html` — add CSS + HTML for two decorative sections

- [ ] **Step 1: Add CSS for trust pills and verse band**

```css
/* ── TRUST PILLS ── */
.trust-pills {
  display: flex;
  gap: 8px;
  overflow-x: auto;
  scrollbar-width: none;
  padding: 20px 20px;
  border-top: 1px solid var(--parchment);
  border-bottom: 1px solid var(--parchment);
  background: var(--cream-dark);
}

.trust-pills::-webkit-scrollbar { display: none; }

.trust-pill {
  display: flex;
  align-items: center;
  gap: 6px;
  background: var(--cream);
  border: 1px solid var(--parchment);
  border-radius: 30px;
  padding: 8px 16px;
  font-size: 12px;
  color: var(--ink-soft);
  white-space: nowrap;
  flex-shrink: 0;
}

/* ── VERSE BAND ── */
.verse-band {
  background: var(--cream-dark);
  border-bottom: 1px solid var(--parchment);
  padding: 48px 24px;
  text-align: center;
  overflow: hidden;
}

.verse-deco {
  display: block;
  font-size: 22px;
  color: var(--sage-light);
  margin-bottom: 16px;
  letter-spacing: 8px;
}

.verse-text {
  font-family: 'Cormorant Garamond', serif;
  font-size: 24px;
  font-style: italic;
  font-weight: 400;
  color: var(--ink);
  line-height: 1.45;
  margin-bottom: 14px;
  max-width: 800px;
  margin-left: auto;
  margin-right: auto;
}

.verse-ref {
  font-size: 12px;
  color: var(--ink-soft);
  letter-spacing: 0.14em;
  text-transform: uppercase;
}
```

Desktop overrides (inside `@media`):

```css
  .trust-pills {
    justify-content: center;
    overflow: visible;
    flex-wrap: wrap;
  }

  .verse-text { font-size: 32px; }
```

- [ ] **Step 2: Add HTML after the scroll-hint div**

```html
<!-- TRUST PILLS -->
<div class="trust-pills">
  <div class="trust-pill">🌿 Сделано с молитвой</div>
  <div class="trust-pill">📦 Доставка по России</div>
  <div class="trust-pill">✦ Малый живой тираж</div>
  <div class="trust-pill">💛 Для семьи и детей</div>
</div>

<!-- VERSE BAND -->
<div class="verse-band reveal">
  <span class="verse-deco">✦ ✦ ✦</span>
  <p class="verse-text">«Всё, что только истинно, что честно, что справедливо, что чисто, что любезно... о сём помышляйте»</p>
  <p class="verse-ref">Филиппийцам 4:8</p>
</div>
```

- [ ] **Step 3: Add scroll reveal JS** (if not already added — append to the `<script>` block)

```js
// Scroll reveal
const reveals = document.querySelectorAll('.reveal');
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); });
}, { threshold: 0.08 });
reveals.forEach(el => observer.observe(el));
```

- [ ] **Step 4: Verify and commit**

Verify trust pills scroll horizontally on mobile, wrap on desktop. Verse animates in on scroll.

```bash
git add index.html
git commit -m "feat: trust pills and scripture quote section"
```

---

### Task 5: Categories Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add categories CSS**

```css
/* ── SECTION COMMON ── */
.section {
  padding: 48px 20px;
  max-width: var(--max-w);
  margin: 0 auto;
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  margin-bottom: 24px;
}

.section-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 36px;
  font-weight: 400;
  line-height: 1.1;
  color: var(--ink);
}

.section-title em { font-style: italic; color: var(--terra); }

.section-link {
  font-size: 12px;
  color: var(--ink-soft);
  text-decoration: none;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  border-bottom: 1px solid var(--parchment);
  padding-bottom: 1px;
  white-space: nowrap;
  transition: color 0.3s;
}

.section-link:hover { color: var(--terra); border-color: var(--terra); }

/* ── CATEGORIES ── */
.cat-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

.cat-card {
  border-radius: 18px;
  overflow: hidden;
  aspect-ratio: 4/5;
  position: relative;
  cursor: pointer;
  transition: transform 0.3s, box-shadow 0.3s;
}

.cat-card:nth-child(2n) { margin-top: 24px; }

.cat-card:hover { transform: scale(1.02); box-shadow: 0 8px 30px rgba(44,32,24,0.15); }

.cat-bg {
  width: 100%; height: 100%;
  display: flex;
  align-items: flex-end;
  position: relative;
}

.cat-icon {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -65%);
  font-size: 44px;
  opacity: 0.4;
}

.cat-info {
  position: relative;
  z-index: 2;
  width: 100%;
  padding: 14px;
  background: linear-gradient(transparent, rgba(44,32,24,0.4));
}

.cat-name {
  font-family: 'Cormorant Garamond', serif;
  font-size: 18px;
  font-weight: 500;
  color: white;
}

.cat-count {
  font-size: 11px;
  color: rgba(255,255,255,0.75);
}
```

Desktop overrides:

```css
  .section { padding: 60px 48px; max-width: var(--max-w); margin: 0 auto; }
  .section-title { font-size: 48px; }

  .cat-grid { grid-template-columns: repeat(3, 1fr); gap: 16px; }
  .cat-card:nth-child(2n) { margin-top: 0; }
  .cat-card:nth-child(2) { margin-top: 32px; }
  .cat-card:nth-child(5) { margin-top: -32px; }
```

- [ ] **Step 2: Add categories HTML after verse-band**

```html
<!-- CATEGORIES -->
<div class="section reveal" id="categories">
  <div class="section-header">
    <h2 class="section-title">Что мы<br><em>создаём</em></h2>
    <a href="#" class="section-link">Все категории</a>
  </div>
  <div class="cat-grid">
    <div class="cat-card"><div class="cat-bg" style="background:linear-gradient(160deg,#E0D8CC,#C4B498);"><div class="cat-icon">🖼</div><div class="cat-info"><div class="cat-name">Постеры</div><div class="cat-count">Библейские стихи</div></div></div></div>
    <div class="cat-card"><div class="cat-bg" style="background:linear-gradient(160deg,#CDD9C4,#A8C098);"><div class="cat-icon">📔</div><div class="cat-info"><div class="cat-name">Prayer Journals</div><div class="cat-count">Планеры для мам</div></div></div></div>
    <div class="cat-card"><div class="cat-bg" style="background:linear-gradient(160deg,#E4D0C8,#C8A090);"><div class="cat-icon">🎨</div><div class="cat-info"><div class="cat-name">Раскраски</div><div class="cat-count">Для детей по Библии</div></div></div></div>
    <div class="cat-card"><div class="cat-bg" style="background:linear-gradient(160deg,#E8D8CC,#D4B8A0);"><div class="cat-icon">✨</div><div class="cat-info"><div class="cat-name">Наклейки</div><div class="cat-count">Закладки, открытки</div></div></div></div>
    <div class="cat-card"><div class="cat-bg" style="background:linear-gradient(160deg,#E0DCC8,#C4B888);"><div class="cat-icon">🎄</div><div class="cat-info"><div class="cat-name">Праздники</div><div class="cat-count">Пасха, Рождество</div></div></div></div>
    <div class="cat-card"><div class="cat-bg" style="background:linear-gradient(160deg,#D4DCCC,#B0C0A0);"><div class="cat-icon">📚</div><div class="cat-info"><div class="cat-name">Activity Books</div><div class="cat-count">Задания для детей</div></div></div></div>
  </div>
</div>
```

- [ ] **Step 3: Verify and commit**

Desktop: 3×2 grid with staggered 2nd and 5th cards. Mobile: 2×3 with even columns shifted down. Hover scales cards.

```bash
git add index.html
git commit -m "feat: categories section — 6 cards, responsive grid"
```

---

### Task 6: Products Section

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add products CSS**

```css
/* ── PRODUCTS ── */
.products-scroll {
  margin: 0 -20px;
  overflow-x: auto;
  scrollbar-width: none;
  padding: 4px 20px 20px;
}

.products-scroll::-webkit-scrollbar { display: none; }

.products-row {
  display: flex;
  gap: 14px;
  width: max-content;
}

.prod-card {
  width: 230px;
  background: var(--cream-dark);
  border: 1px solid var(--parchment);
  border-radius: 20px;
  overflow: hidden;
  flex-shrink: 0;
  transition: transform 0.3s, box-shadow 0.3s;
}

.prod-card:hover { transform: translateY(-4px); box-shadow: 0 12px 32px rgba(44,32,24,0.12); }

.prod-img {
  width: 100%;
  aspect-ratio: 1/1;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 56px;
}

.prod-info { padding: 16px 18px 18px; }

.prod-tag {
  display: inline-block;
  font-size: 10px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--sage-dark);
  background: rgba(138,158,122,0.15);
  padding: 3px 10px;
  border-radius: 20px;
  margin-bottom: 8px;
}

.prod-name {
  font-family: 'Cormorant Garamond', serif;
  font-size: 20px;
  font-weight: 500;
  color: var(--ink);
  margin-bottom: 6px;
}

.prod-desc {
  font-size: 12px;
  color: var(--ink-soft);
  line-height: 1.6;
  margin-bottom: 14px;
}

.prod-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.prod-price {
  font-family: 'Cormorant Garamond', serif;
  font-size: 22px;
  color: var(--terra);
}

.btn-add {
  background: var(--ink);
  color: var(--cream);
  border: none;
  border-radius: 24px;
  padding: 9px 18px;
  font-size: 12px;
  cursor: pointer;
  font-family: 'Nunito', sans-serif;
  transition: background 0.3s;
}

.btn-add:hover { background: var(--terra); }
```

Desktop overrides:

```css
  .products-scroll { margin: 0; overflow: visible; padding: 4px 0 0; }
  .products-row {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
    width: auto;
  }
  .prod-card { width: auto; }
```

- [ ] **Step 2: Add products HTML after categories section**

```html
<!-- PRODUCTS -->
<div class="section reveal" id="products" style="padding-top:0;">
  <div class="section-header">
    <h2 class="section-title">Популярные<br><em>товары</em></h2>
    <a href="#" class="section-link">Все товары</a>
  </div>
  <div class="products-scroll">
    <div class="products-row">
      <div class="prod-card">
        <div class="prod-img" style="background:linear-gradient(135deg,#E4DCC8,#C8B898);">🖼</div>
        <div class="prod-info">
          <span class="prod-tag">Постер</span>
          <div class="prod-name">«Не бойся» А3</div>
          <div class="prod-desc">Плакат с Иисус Навин 1:9. Акварельная ботаника, тёплые тона.</div>
          <div class="prod-footer">
            <div class="prod-price">490 ₽</div>
            <button class="btn-add" data-id="poster-ne-boysya" data-name="«Не бойся» А3" data-price="490" data-tag="Постер">В корзину</button>
          </div>
        </div>
      </div>
      <div class="prod-card">
        <div class="prod-img" style="background:linear-gradient(135deg,#D0DCCC,#B0C4A8);">📔</div>
        <div class="prod-info">
          <span class="prod-tag">Журнал</span>
          <div class="prod-name">Prayer Journal «Утро»</div>
          <div class="prod-desc">Блокнот для молитв и размышлений. Акварельная обложка, 120 листов.</div>
          <div class="prod-footer">
            <div class="prod-price">620 ₽</div>
            <button class="btn-add" data-id="journal-utro" data-name="Prayer Journal «Утро»" data-price="620" data-tag="Журнал">В корзину</button>
          </div>
        </div>
      </div>
      <div class="prod-card">
        <div class="prod-img" style="background:linear-gradient(135deg,#E0CCC8,#C8A8A0);">🎨</div>
        <div class="prod-info">
          <span class="prod-tag">Детям</span>
          <div class="prod-name">«Добрый пастырь»</div>
          <div class="prod-desc">Раскраска для детей 4–8 лет с историями из Библии и заданиями.</div>
          <div class="prod-footer">
            <div class="prod-price">380 ₽</div>
            <button class="btn-add" data-id="raskraska-pastyr" data-name="«Добрый пастырь»" data-price="380" data-tag="Детям">В корзину</button>
          </div>
        </div>
      </div>
      <div class="prod-card">
        <div class="prod-img" style="background:linear-gradient(135deg,#E8E0D0,#D4C8B0);">🃏</div>
        <div class="prod-info">
          <span class="prod-tag">Карточки</span>
          <div class="prod-name">«Обетования» 30 шт</div>
          <div class="prod-desc">Набор карточек с обетованиями из Писания. Плотная бумага, красивый дизайн.</div>
          <div class="prod-footer">
            <div class="prod-price">350 ₽</div>
            <button class="btn-add" data-id="kartochki-obetovaniya" data-name="«Обетования» 30 шт" data-price="350" data-tag="Карточки">В корзину</button>
          </div>
        </div>
      </div>
      <div class="prod-card">
        <div class="prod-img" style="background:linear-gradient(135deg,#E8D4C8,#D4A898);">✨</div>
        <div class="prod-info">
          <span class="prod-tag">Наклейки</span>
          <div class="prod-name">Наклейки «Вера»</div>
          <div class="prod-desc">Набор виниловых наклеек с библейскими мотивами. Подходят для ежедневников.</div>
          <div class="prod-footer">
            <div class="prod-price">180 ₽</div>
            <button class="btn-add" data-id="nakleyki-vera" data-name="Наклейки «Вера»" data-price="180" data-tag="Наклейки">В корзину</button>
          </div>
        </div>
      </div>
      <div class="prod-card">
        <div class="prod-img" style="background:linear-gradient(135deg,#E0DCC8,#C8B898);">🐣</div>
        <div class="prod-info">
          <span class="prod-tag">Праздники</span>
          <div class="prod-name">Пасхальный набор</div>
          <div class="prod-desc">Раскраска + наклейки + открытка для детей. Подарочная упаковка.</div>
          <div class="prod-footer">
            <div class="prod-price">550 ₽</div>
            <button class="btn-add" data-id="paskhalny-nabor" data-name="Пасхальный набор" data-price="550" data-tag="Праздники">В корзину</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
```

- [ ] **Step 3: Verify and commit**

Desktop: 3×2 grid of all 6 products. Mobile: horizontal scroll. Hover lifts cards. Buttons have `data-*` attributes for cart JS (added in Task 8).

```bash
git add index.html
git commit -m "feat: products section — 6 cards with data attributes for cart"
```

---

### Task 7: About + Newsletter + Footer

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Add CSS for about, newsletter, and footer**

```css
/* ── ABOUT ── */
.about {
  background: var(--deep-sage);
  padding: 56px 24px;
  color: var(--cream);
}

.about-inner {
  max-width: var(--max-w);
  margin: 0 auto;
}

.about-eyebrow {
  font-size: 11px;
  letter-spacing: 0.18em;
  text-transform: uppercase;
  color: var(--terra);
  margin-bottom: 16px;
}

.about-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 34px;
  font-weight: 400;
  line-height: 1.15;
  margin-bottom: 18px;
  color: var(--cream);
}

.about-title em { font-style: italic; color: var(--dusty-rose); }

.about-text {
  font-size: 14px;
  line-height: 1.8;
  color: rgba(247,242,232,0.68);
  font-weight: 300;
  margin-bottom: 32px;
}

.values-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
  margin-bottom: 32px;
}

.val-card {
  background: rgba(247,242,232,0.06);
  border: 1px solid rgba(247,242,232,0.1);
  border-radius: 16px;
  padding: 18px;
}

.val-icon { font-size: 22px; margin-bottom: 8px; }

.val-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 18px;
  color: var(--cream);
  margin-bottom: 4px;
}

.val-text {
  font-size: 12px;
  color: rgba(247,242,232,0.55);
  line-height: 1.5;
}

.btn-outline {
  display: inline-block;
  border: 1px solid rgba(247,242,232,0.3);
  color: var(--cream);
  padding: 13px 28px;
  border-radius: 40px;
  font-size: 13px;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  text-decoration: none;
  transition: border-color 0.3s, background 0.3s;
}

.btn-outline:hover { background: rgba(247,242,232,0.08); border-color: rgba(247,242,232,0.5); }

/* ── NEWSLETTER ── */
.newsletter {
  padding: 52px 24px;
  background: var(--cream-dark);
  border-top: 1px solid var(--parchment);
  text-align: center;
}

.nl-inner {
  max-width: 480px;
  margin: 0 auto;
}

.nl-deco {
  font-size: 16px;
  color: var(--sage-light);
  letter-spacing: 12px;
  margin-bottom: 20px;
  display: block;
}

.nl-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 34px;
  font-weight: 400;
  color: var(--ink);
  margin-bottom: 10px;
}

.nl-title em { font-style: italic; color: var(--terra); }

.nl-text {
  font-size: 14px;
  color: var(--ink-soft);
  font-weight: 300;
  line-height: 1.6;
  margin-bottom: 28px;
}

.nl-form {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.nl-input {
  width: 100%;
  padding: 14px 20px;
  border: 1px solid var(--parchment);
  border-radius: 40px;
  font-size: 14px;
  background: var(--cream);
  color: var(--ink);
  outline: none;
  font-family: 'Nunito', sans-serif;
  text-align: center;
}

.nl-input::placeholder { color: var(--ink-soft); opacity: 0.5; }
.nl-input:focus { border-color: var(--terra); }

/* ── FOOTER ── */
footer {
  background: var(--deep-sage);
  color: var(--cream);
  padding: 48px 24px 32px;
}

.footer-inner {
  max-width: var(--max-w);
  margin: 0 auto;
}

.footer-brand {
  font-family: 'Cormorant Garamond', serif;
  font-size: 30px;
  font-weight: 500;
  margin-bottom: 10px;
}

.footer-brand em { font-style: italic; color: var(--terra); }

.footer-tagline {
  font-size: 13px;
  color: rgba(247,242,232,0.55);
  line-height: 1.7;
  font-weight: 300;
  margin-bottom: 24px;
}

.footer-social {
  display: flex;
  gap: 10px;
  margin-bottom: 40px;
}

.soc-btn {
  width: 38px; height: 38px;
  border-radius: 50%;
  border: 1px solid rgba(247,242,232,0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 13px;
  color: rgba(247,242,232,0.7);
  text-decoration: none;
  transition: border-color 0.3s, color 0.3s;
}

.soc-btn:hover { border-color: var(--terra); color: var(--terra); }

.footer-cols {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 32px;
  margin-bottom: 36px;
}

.footer-col-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 18px;
  color: var(--cream);
  margin-bottom: 14px;
}

.footer-links { list-style: none; }
.footer-links li { margin-bottom: 9px; }
.footer-links a {
  text-decoration: none;
  font-size: 13px;
  color: rgba(247,242,232,0.5);
  font-weight: 300;
  transition: color 0.3s;
}

.footer-links a:hover { color: var(--terra); }

.footer-bottom {
  border-top: 1px solid rgba(247,242,232,0.1);
  padding-top: 20px;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.footer-copy {
  font-size: 12px;
  color: rgba(247,242,232,0.35);
}

.footer-verse {
  font-family: 'Lora', serif;
  font-style: italic;
  font-size: 13px;
  color: rgba(247,242,232,0.4);
}
```

Desktop overrides:

```css
  .about {
    padding: 80px 48px;
  }

  .about-inner {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 64px;
    align-items: center;
  }

  .about-title { font-size: 42px; }
  .values-grid { margin-bottom: 0; }

  .newsletter { padding: 70px 48px; }
  .nl-form { flex-direction: row; }
  .nl-input { text-align: left; }

  footer { padding: 60px 48px 40px; }

  .footer-top {
    display: grid;
    grid-template-columns: 2fr 1fr 1fr;
    gap: 60px;
    margin-bottom: 40px;
  }

  .footer-cols { display: contents; }

  .footer-bottom {
    flex-direction: row;
    justify-content: space-between;
  }
```

- [ ] **Step 2: Add HTML for about, newsletter, and footer after products section**

```html
<!-- ABOUT -->
<div class="about reveal" id="about">
  <div class="about-inner">
    <div>
      <p class="about-eyebrow">Наша история</p>
      <h2 class="about-title">Сделано <em>с верой</em> в Краснодарском крае</h2>
      <p class="about-text">Мы — христианская семья из Белореченска. Каждое изделие рождается из желания наполнить дом красотой и Словом Божьим. Мы верим, что красота служит истине.</p>
    </div>
    <div>
      <div class="values-grid">
        <div class="val-card"><div class="val-icon">✦</div><div class="val-title">Вера</div><div class="val-text">Каждый продукт создаётся с молитвой</div></div>
        <div class="val-card"><div class="val-icon">🌿</div><div class="val-title">Красота</div><div class="val-text">Современный дизайн для Слова</div></div>
        <div class="val-card"><div class="val-icon">👨‍👩‍👧</div><div class="val-title">Семья</div><div class="val-text">Для детей и взрослых</div></div>
        <div class="val-card"><div class="val-icon">🤍</div><div class="val-title">Качество</div><div class="val-text">Профессиональная типография</div></div>
      </div>
      <br>
      <a href="#" class="btn-outline">Читать нашу историю</a>
    </div>
  </div>
</div>

<!-- NEWSLETTER -->
<div class="newsletter reveal">
  <div class="nl-inner">
    <span class="nl-deco">✦ ✦ ✦</span>
    <h2 class="nl-title">Новинки <em>первыми</em></h2>
    <p class="nl-text">Подпишитесь — и узнавайте о новых продуктах и вдохновении раньше всех</p>
    <div class="nl-form">
      <input class="nl-input" type="email" placeholder="ваш@email.ru">
      <button class="btn-primary" style="width:100%">Подписаться</button>
    </div>
  </div>
</div>

<!-- FOOTER -->
<footer id="footer">
  <div class="footer-inner">
    <div class="footer-top">
      <div>
        <div class="footer-brand">Svet<em>ly</em></div>
        <p class="footer-tagline">Христианская печатная продукция для всей семьи. Сделано с любовью в Белореченске.</p>
        <div class="footer-social">
          <a href="#" class="soc-btn">VK</a>
          <a href="#" class="soc-btn">TG</a>
          <a href="#" class="soc-btn">✉</a>
        </div>
      </div>
      <div class="footer-cols">
        <div>
          <div class="footer-col-title">Каталог</div>
          <ul class="footer-links">
            <li><a href="#">Постеры</a></li>
            <li><a href="#">Раскраски</a></li>
            <li><a href="#">Журналы</a></li>
            <li><a href="#">Наклейки</a></li>
          </ul>
        </div>
        <div>
          <div class="footer-col-title">Магазин</div>
          <ul class="footer-links">
            <li><a href="#">Доставка</a></li>
            <li><a href="#">Оплата</a></li>
            <li><a href="#">О нас</a></li>
            <li><a href="#">Контакты</a></li>
          </ul>
        </div>
      </div>
    </div>
    <div class="footer-bottom">
      <span class="footer-copy">© 2025 Svetly. Все права защищены.</span>
      <span class="footer-verse">«Слово Твоё — светильник» · Пс. 118:105</span>
    </div>
  </div>
</footer>
```

- [ ] **Step 3: Verify and commit**

All static content is now visible. About section has deep-sage background. Footer matches. Newsletter form centered. Desktop layouts use grids.

```bash
git add index.html
git commit -m "feat: about, newsletter, footer sections"
```

---

### Task 8: Cart System — localStorage + Slide-in Panel

**Files:**
- Modify: `index.html` — add cart panel HTML, cart CSS, cart JS

- [ ] **Step 1: Add cart CSS**

```css
/* ── CART PANEL ── */
.cart-overlay {
  position: fixed;
  inset: 0;
  background: rgba(44,32,24,0.4);
  z-index: 400;
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s;
}

.cart-overlay.open { opacity: 1; pointer-events: auto; }

.cart-panel {
  position: fixed;
  top: 0; right: 0; bottom: 0;
  width: 100%;
  max-width: 420px;
  background: var(--cream);
  z-index: 401;
  display: flex;
  flex-direction: column;
  transform: translateX(100%);
  transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: -8px 0 30px rgba(0,0,0,0.15);
}

.cart-panel.open { transform: translateX(0); }

.cart-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 24px;
  border-bottom: 1px solid var(--parchment);
}

.cart-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 22px;
  color: var(--ink);
}

.cart-close {
  background: none;
  border: none;
  font-size: 24px;
  color: var(--ink-soft);
  cursor: pointer;
}

.cart-back {
  background: none;
  border: none;
  font-size: 14px;
  color: var(--ink-soft);
  cursor: pointer;
  font-family: 'Nunito', sans-serif;
}

.cart-items {
  flex: 1;
  overflow-y: auto;
  padding: 0 24px;
}

.cart-item {
  display: flex;
  gap: 14px;
  padding: 14px 0;
  border-bottom: 1px solid var(--parchment);
}

.cart-item-img {
  width: 64px; height: 64px;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  flex-shrink: 0;
}

.cart-item-info { flex: 1; }

.cart-item-name {
  font-family: 'Cormorant Garamond', serif;
  font-size: 15px;
  color: var(--ink);
  margin-bottom: 2px;
}

.cart-item-tag {
  font-size: 12px;
  color: var(--ink-soft);
  margin-bottom: 8px;
}

.cart-item-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cart-qty {
  display: flex;
  align-items: center;
  gap: 10px;
}

.cart-qty-btn {
  width: 28px; height: 28px;
  border-radius: 50%;
  border: 1px solid var(--parchment);
  background: none;
  font-size: 14px;
  color: var(--ink-soft);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: border-color 0.3s;
}

.cart-qty-btn:hover { border-color: var(--terra); color: var(--terra); }

.cart-qty-num {
  font-size: 14px;
  color: var(--ink);
  min-width: 16px;
  text-align: center;
}

.cart-item-price {
  font-family: 'Cormorant Garamond', serif;
  font-size: 16px;
  color: var(--terra);
}

.cart-empty {
  text-align: center;
  padding: 60px 20px;
  color: var(--ink-soft);
  font-size: 14px;
}

.cart-footer {
  padding: 20px 24px;
  border-top: 1px solid var(--parchment);
  background: var(--cream-dark);
}

.cart-total-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
}

.cart-total-label {
  font-size: 14px;
  color: var(--ink-soft);
}

.cart-total-price {
  font-family: 'Cormorant Garamond', serif;
  font-size: 24px;
  color: var(--ink);
}

.cart-checkout-btn {
  width: 100%;
  background: var(--ink);
  color: var(--cream);
  border: none;
  padding: 14px;
  border-radius: 40px;
  font-size: 14px;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  cursor: pointer;
  font-family: 'Nunito', sans-serif;
  transition: background 0.3s;
}

.cart-checkout-btn:hover { background: var(--terra); }

/* ── STICKY CART (mobile) ── */
.sticky-cart {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 150;
  background: var(--ink);
  color: var(--cream);
  padding: 13px 28px;
  border-radius: 50px;
  font-size: 13px;
  letter-spacing: 0.06em;
  display: flex;
  align-items: center;
  gap: 8px;
  box-shadow: 0 8px 30px rgba(44,32,24,0.25);
  cursor: pointer;
  border: none;
  font-family: 'Nunito', sans-serif;
  opacity: 0;
  transition: opacity 0.3s, transform 0.3s, background 0.3s;
  transform: translateX(-50%) translateY(80px);
}

.sticky-cart.visible {
  opacity: 1;
  transform: translateX(-50%) translateY(0);
}

.sticky-cart:hover { background: var(--terra); }
```

Desktop override:

```css
  .sticky-cart { display: none; }
```

- [ ] **Step 2: Add cart panel HTML before `</body>` (before `<script>`)**

```html
<!-- CART OVERLAY + PANEL -->
<div class="cart-overlay" id="cartOverlay"></div>
<div class="cart-panel" id="cartPanel">
  <div class="cart-header" id="cartHeader">
    <span class="cart-title">Корзина</span>
    <button class="cart-close" id="cartClose">×</button>
  </div>
  <div class="cart-items" id="cartItems"></div>
  <div class="cart-footer" id="cartFooter" style="display:none;">
    <div class="cart-total-row">
      <span class="cart-total-label">Итого:</span>
      <span class="cart-total-price" id="cartTotal">0 ₽</span>
    </div>
    <button class="cart-checkout-btn" id="cartCheckoutBtn">Оформить заказ</button>
  </div>
</div>

<!-- STICKY CART (mobile) -->
<button class="sticky-cart" id="stickyCart">🛒 Корзина · <span id="stickyCount">0</span></button>
```

- [ ] **Step 3: Replace the entire `<script>` block with the full JS including cart logic**

```html
<script>
// ═══════════════════════════════════════
// NAV
// ═══════════════════════════════════════
const navbar = document.getElementById('navbar');
window.addEventListener('scroll', () => {
  navbar.classList.toggle('scrolled', window.scrollY > 40);
});

// Mobile menu
const menuToggle = document.getElementById('menuToggle');
const menuClose = document.getElementById('menuClose');
const mobileMenu = document.getElementById('mobileMenu');

menuToggle.addEventListener('click', () => {
  mobileMenu.classList.add('open');
  document.body.style.overflow = 'hidden';
});

function closeMobileMenu() {
  mobileMenu.classList.remove('open');
  document.body.style.overflow = '';
}

menuClose.addEventListener('click', closeMobileMenu);

// Scroll reveal
const reveals = document.querySelectorAll('.reveal');
const observer = new IntersectionObserver((entries) => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('visible'); });
}, { threshold: 0.08 });
reveals.forEach(el => observer.observe(el));

// ═══════════════════════════════════════
// CART
// ═══════════════════════════════════════
const CART_KEY = 'svetly-cart';

function getCart() {
  try { return JSON.parse(localStorage.getItem(CART_KEY)) || []; }
  catch { return []; }
}

function saveCart(cart) {
  localStorage.setItem(CART_KEY, JSON.stringify(cart));
  updateCartUI();
}

function addToCart(id, name, price, tag) {
  const cart = getCart();
  const existing = cart.find(item => item.id === id);
  if (existing) {
    existing.qty++;
  } else {
    cart.push({ id, name, price: Number(price), qty: 1, tag });
  }
  saveCart(cart);
}

function changeQty(id, delta) {
  const cart = getCart();
  const item = cart.find(i => i.id === id);
  if (!item) return;
  item.qty += delta;
  if (item.qty <= 0) {
    cart.splice(cart.indexOf(item), 1);
  }
  saveCart(cart);
}

function getCartTotal() {
  return getCart().reduce((sum, item) => sum + item.price * item.qty, 0);
}

function getCartCount() {
  return getCart().reduce((sum, item) => sum + item.qty, 0);
}

// Cart UI
const cartOverlay = document.getElementById('cartOverlay');
const cartPanel = document.getElementById('cartPanel');
const cartItems = document.getElementById('cartItems');
const cartFooter = document.getElementById('cartFooter');
const cartTotal = document.getElementById('cartTotal');
const cartClose = document.getElementById('cartClose');
const cartCheckoutBtn = document.getElementById('cartCheckoutBtn');
const navCartCount = document.getElementById('navCartCount');
const stickyCart = document.getElementById('stickyCart');
const stickyCount = document.getElementById('stickyCount');
const navCartBtn = document.getElementById('navCartBtn');
const cartHeader = document.getElementById('cartHeader');

function openCart() {
  showCartView();
  cartOverlay.classList.add('open');
  cartPanel.classList.add('open');
  document.body.style.overflow = 'hidden';
}

function closeCart() {
  cartOverlay.classList.remove('open');
  cartPanel.classList.remove('open');
  document.body.style.overflow = '';
}

function updateCartUI() {
  const count = getCartCount();
  navCartCount.textContent = count;
  stickyCount.textContent = count;

  // Sticky cart visibility (mobile)
  if (count > 0) {
    stickyCart.classList.add('visible');
  } else {
    stickyCart.classList.remove('visible');
  }
}

function showCartView() {
  const cart = getCart();
  cartHeader.innerHTML = '<span class="cart-title">Корзина</span><button class="cart-close" onclick="closeCart()">×</button>';

  if (cart.length === 0) {
    cartItems.innerHTML = '<div class="cart-empty">Корзина пуста</div>';
    cartFooter.style.display = 'none';
    return;
  }

  cartItems.innerHTML = cart.map(item => `
    <div class="cart-item">
      <div class="cart-item-img" style="background:var(--cream-dark);">${getEmoji(item.tag)}</div>
      <div class="cart-item-info">
        <div class="cart-item-name">${item.name}</div>
        <div class="cart-item-tag">${item.tag}</div>
        <div class="cart-item-row">
          <div class="cart-qty">
            <button class="cart-qty-btn" onclick="changeQty('${item.id}',-1)">−</button>
            <span class="cart-qty-num">${item.qty}</span>
            <button class="cart-qty-btn" onclick="changeQty('${item.id}',1)">+</button>
          </div>
          <div class="cart-item-price">${(item.price * item.qty).toLocaleString('ru-RU')} ₽</div>
        </div>
      </div>
    </div>
  `).join('');

  cartFooter.style.display = 'block';
  cartTotal.textContent = getCartTotal().toLocaleString('ru-RU') + ' ₽';
}

function getEmoji(tag) {
  const map = { 'Постер': '🖼', 'Журнал': '📔', 'Детям': '🎨', 'Карточки': '🃏', 'Наклейки': '✨', 'Праздники': '🐣' };
  return map[tag] || '📦';
}

// Add to cart buttons
document.querySelectorAll('.btn-add').forEach(btn => {
  btn.addEventListener('click', function() {
    const { id, name, price, tag } = this.dataset;
    addToCart(id, name, price, tag);

    const orig = this.textContent;
    this.textContent = '✓ Добавлено';
    this.style.background = 'var(--sage)';
    setTimeout(() => {
      this.textContent = orig;
      this.style.background = '';
    }, 1800);
  });
});

// Open/close cart events
navCartBtn.addEventListener('click', openCart);
stickyCart.addEventListener('click', openCart);
cartOverlay.addEventListener('click', closeCart);
cartCheckoutBtn.addEventListener('click', showOrderForm);

// Init cart UI on load
updateCartUI();
// Refresh cart view if panel is open
if (cartPanel.classList.contains('open')) showCartView();
</script>
```

- [ ] **Step 4: Verify cart flow**

1. Click "В корзину" → button turns green "✓ Добавлено", badge updates
2. Click cart button → panel slides in from right, shows items
3. ± buttons change quantity, total updates
4. Reduce to 0 removes item
5. Close with × or overlay click
6. Reload page → cart items persist (localStorage)
7. Mobile: sticky cart button appears at bottom when items in cart

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: cart system — localStorage, slide-in panel, add/remove items"
```

---

### Task 9: Order Form

**Files:**
- Modify: `index.html` — add order form CSS and `showOrderForm()` JS function

- [ ] **Step 1: Add order form CSS**

```css
/* ── ORDER FORM ── */
.order-form { padding: 24px; }

.order-field {
  margin-bottom: 16px;
}

.order-label {
  font-size: 12px;
  color: var(--ink-soft);
  margin-bottom: 6px;
  display: block;
}

.order-input {
  width: 100%;
  padding: 12px 16px;
  border: 1px solid var(--parchment);
  border-radius: 12px;
  font-size: 14px;
  background: white;
  color: var(--ink);
  outline: none;
  font-family: 'Nunito', sans-serif;
}

.order-input:focus { border-color: var(--terra); }

.order-textarea {
  width: 100%;
  padding: 12px 16px;
  border: 1px solid var(--parchment);
  border-radius: 12px;
  font-size: 14px;
  background: white;
  color: var(--ink);
  outline: none;
  font-family: 'Nunito', sans-serif;
  resize: none;
  height: 70px;
}

.order-textarea:focus { border-color: var(--terra); }

.order-summary {
  background: var(--cream-dark);
  border-radius: 12px;
  padding: 14px 16px;
  margin-bottom: 20px;
}

.order-summary-title {
  font-size: 12px;
  color: var(--ink-soft);
  margin-bottom: 8px;
}

.order-summary-items {
  font-size: 13px;
  color: var(--ink);
  line-height: 1.8;
}

.order-summary-total {
  border-top: 1px solid var(--parchment);
  margin-top: 8px;
  padding-top: 8px;
  display: flex;
  justify-content: space-between;
}

.order-summary-total-label {
  font-size: 14px;
  color: var(--ink);
  font-weight: 500;
}

.order-summary-total-price {
  font-family: 'Cormorant Garamond', serif;
  font-size: 18px;
  color: var(--terra);
}

.order-submit-btn {
  width: 100%;
  background: var(--deep-sage);
  color: var(--cream);
  border: none;
  padding: 14px;
  border-radius: 40px;
  font-size: 14px;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  cursor: pointer;
  font-family: 'Nunito', sans-serif;
  transition: background 0.3s;
}

.order-submit-btn:hover { background: var(--terra); }

.order-note {
  text-align: center;
  margin-top: 12px;
  font-size: 11px;
  color: var(--sage);
}
```

- [ ] **Step 2: Add `showOrderForm()` function to the `<script>` block** (before the closing `</script>`)

```js
// ═══════════════════════════════════════
// ORDER FORM
// ═══════════════════════════════════════
function showOrderForm() {
  const cart = getCart();
  if (cart.length === 0) return;

  cartHeader.innerHTML = '<button class="cart-back" onclick="showCartView()">← Назад</button><span class="cart-title">Оформление</span><button class="cart-close" onclick="closeCart()">×</button>';

  const itemsHtml = cart.map(item =>
    `${item.name} × ${item.qty} — ${(item.price * item.qty).toLocaleString('ru-RU')} ₽`
  ).join('<br>');

  cartItems.innerHTML = `
    <div class="order-form">
      <div class="order-field">
        <label class="order-label">Ваше имя</label>
        <input class="order-input" id="orderName" placeholder="Анна">
      </div>
      <div class="order-field">
        <label class="order-label">Телефон или email</label>
        <input class="order-input" id="orderContact" placeholder="+7 900 123-45-67">
      </div>
      <div class="order-field">
        <label class="order-label">Город</label>
        <input class="order-input" id="orderCity" placeholder="Краснодар">
      </div>
      <div class="order-field">
        <label class="order-label">Комментарий</label>
        <textarea class="order-textarea" id="orderComment" placeholder="Пожелания к заказу..."></textarea>
      </div>
      <div class="order-summary">
        <div class="order-summary-title">Ваш заказ:</div>
        <div class="order-summary-items">${itemsHtml}</div>
        <div class="order-summary-total">
          <span class="order-summary-total-label">Итого</span>
          <span class="order-summary-total-price">${getCartTotal().toLocaleString('ru-RU')} ₽</span>
        </div>
      </div>
      <button class="order-submit-btn" onclick="submitOrder()">Отправить заказ</button>
      <p class="order-note">Мы свяжемся с вами для подтверждения</p>
    </div>
  `;

  cartFooter.style.display = 'none';
}

function submitOrder() {
  const name = document.getElementById('orderName').value || 'Не указано';
  const contact = document.getElementById('orderContact').value || 'Не указан';
  const city = document.getElementById('orderCity').value || 'Не указан';
  const comment = document.getElementById('orderComment').value;
  const cart = getCart();

  let text = `🛒 Новый заказ с svetly.shop\n\n`;
  text += `👤 ${name}\n📱 ${contact}\n🏙 ${city}\n`;
  if (comment) text += `💬 ${comment}\n`;
  text += `\n📦 Товары:\n`;
  cart.forEach(item => {
    text += `• ${item.name} × ${item.qty} = ${(item.price * item.qty).toLocaleString('ru-RU')} ₽\n`;
  });
  text += `\n💰 Итого: ${getCartTotal().toLocaleString('ru-RU')} ₽`;

  // Open Telegram with pre-filled message
  const tgUrl = `https://t.me/share/url?url=${encodeURIComponent('svetly.shop')}&text=${encodeURIComponent(text)}`;
  window.open(tgUrl, '_blank');

  // Clear cart
  localStorage.removeItem(CART_KEY);
  updateCartUI();
  closeCart();
}
```

- [ ] **Step 3: Verify order flow**

1. Add items to cart
2. Open cart → click "Оформить заказ"
3. Cart view replaced by order form (← Назад works)
4. Fill in fields → click "Отправить заказ"
5. Telegram opens with formatted order text
6. Cart clears, panel closes

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: order form — fields, summary, Telegram submission"
```

---

### Task 10: Final Polish + Push to Production

**Files:**
- Modify: `index.html` — add hero stagger animation for mobile, fine-tune responsive

- [ ] **Step 1: Add hero card stagger animation JS** (at end of script block)

```js
// Hero card stagger on mobile
if (window.innerWidth < 768) {
  document.querySelectorAll('.hero-card').forEach((card, i) => {
    card.style.animation = `fadeUp 0.6s ${0.3 + i * 0.1}s ease both`;
  });
}
```

- [ ] **Step 2: Verify entire site on desktop and mobile**

Desktop checklist:
- [ ] Nav: horizontal links, sticky scroll, cart badge
- [ ] Hero: split layout, botanical SVGs, fadeUp animations
- [ ] Trust pills: centered, wrapped
- [ ] Verse: centered italic quote
- [ ] Categories: 3×2 grid with stagger, hover scale
- [ ] Products: 3×2 grid, hover lift, add-to-cart works
- [ ] About: two columns on deep-sage background
- [ ] Newsletter: inline form
- [ ] Footer: 3 columns on deep-sage
- [ ] Cart: slide-in panel, ±qty, order form → Telegram
- [ ] Max-width 1200px on all sections

Mobile checklist:
- [ ] Nav: burger menu, fullscreen slide-in
- [ ] Hero: single column, horizontal scroll cards
- [ ] Trust pills: horizontal scroll
- [ ] Categories: 2×3 grid with stagger
- [ ] Products: horizontal scroll
- [ ] About: single column
- [ ] Newsletter: stacked form
- [ ] Footer: single column
- [ ] Sticky cart button at bottom
- [ ] Cart panel fullscreen

- [ ] **Step 3: Commit and push to production**

```bash
git add index.html
git commit -m "feat: final polish — mobile animations, responsive verification"
git push origin main
```

Site deploys automatically to svetly.shop via GitHub Pages. Verify at https://svetly.shop after ~2 minutes.
