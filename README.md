<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>MesQue — Developer</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Serif+JP:wght@300;400;700&family=Share+Tech+Mono&family=Zen+Kaku+Gothic+New:wght@300;400;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --ink: #0d0d14;
      --paper: #f5f0e8;
      --muted: #b8b0a0;
      --accent: #c84b31;
      --accent2: #3a86c8;
      --gold: #c9a84c;
      --glow: rgba(200, 75, 49, 0.4);
      --glow2: rgba(58, 134, 200, 0.3);
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    html { scroll-behavior: smooth; }

    body {
      background: var(--ink);
      color: var(--paper);
      font-family: 'Zen Kaku Gothic New', sans-serif;
      overflow-x: hidden;
      cursor: none;
    }

    /* Custom cursor */
    .cursor {
      position: fixed;
      width: 12px; height: 12px;
      background: var(--accent);
      border-radius: 50%;
      pointer-events: none;
      z-index: 9999;
      transform: translate(-50%, -50%);
      transition: transform 0.1s, width 0.2s, height 0.2s, background 0.2s;
      mix-blend-mode: screen;
    }
    .cursor-ring {
      position: fixed;
      width: 36px; height: 36px;
      border: 1px solid rgba(200,75,49,0.5);
      border-radius: 50%;
      pointer-events: none;
      z-index: 9998;
      transform: translate(-50%, -50%);
      transition: transform 0.18s ease-out, width 0.3s, height 0.3s;
    }

    /* Noise overlay */
    body::before {
      content: '';
      position: fixed;
      inset: 0;
      background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='1'/%3E%3C/svg%3E");
      opacity: 0.03;
      pointer-events: none;
      z-index: 1000;
    }

    /* Scanlines */
    body::after {
      content: '';
      position: fixed;
      inset: 0;
      background: repeating-linear-gradient(
        0deg,
        transparent,
        transparent 2px,
        rgba(0,0,0,0.07) 2px,
        rgba(0,0,0,0.07) 4px
      );
      pointer-events: none;
      z-index: 999;
    }

    /* ── HERO ── */
    #hero {
      min-height: 100vh;
      display: grid;
      grid-template-columns: 1fr 1fr;
      position: relative;
      overflow: hidden;
    }

    /* Left panel — ink wash */
    .hero-left {
      display: flex;
      flex-direction: column;
      justify-content: center;
      padding: 80px 60px 80px 80px;
      position: relative;
      z-index: 2;
    }

    /* Ink splash behind left text */
    .hero-left::before {
      content: '';
      position: absolute;
      top: -100px; left: -100px;
      width: 600px; height: 600px;
      background: radial-gradient(ellipse at 30% 40%, rgba(200,75,49,0.12) 0%, transparent 65%),
                  radial-gradient(ellipse at 70% 70%, rgba(58,134,200,0.08) 0%, transparent 60%);
      pointer-events: none;
    }

    .eyebrow {
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.7rem;
      letter-spacing: 0.35em;
      color: var(--accent);
      text-transform: uppercase;
      margin-bottom: 24px;
      opacity: 0;
      animation: fadeSlideUp 0.8s 0.3s forwards;
    }

    .hero-name {
      font-family: 'Noto Serif JP', serif;
      font-size: clamp(3.5rem, 6vw, 6rem);
      font-weight: 700;
      line-height: 1.05;
      letter-spacing: -0.02em;
      margin-bottom: 8px;
      opacity: 0;
      animation: fadeSlideUp 0.9s 0.5s forwards;
    }

    .hero-name span {
      color: var(--accent);
      position: relative;
    }
    .hero-name span::after {
      content: '';
      position: absolute;
      bottom: 4px; left: 0;
      width: 100%; height: 2px;
      background: var(--accent);
      transform: scaleX(0);
      transform-origin: left;
      animation: lineExpand 0.6s 1.4s forwards;
    }

    .jp-sub {
      font-family: 'Noto Serif JP', serif;
      font-size: clamp(1rem, 1.6vw, 1.4rem);
      font-weight: 300;
      color: var(--muted);
      letter-spacing: 0.15em;
      margin-bottom: 32px;
      opacity: 0;
      animation: fadeSlideUp 0.9s 0.7s forwards;
    }

    .hero-tagline {
      font-size: clamp(0.95rem, 1.4vw, 1.1rem);
      color: var(--muted);
      font-weight: 300;
      line-height: 1.7;
      max-width: 420px;
      margin-bottom: 48px;
      border-left: 2px solid var(--accent2);
      padding-left: 18px;
      opacity: 0;
      animation: fadeSlideUp 0.9s 0.9s forwards;
    }

    .hero-cta {
      display: flex;
      gap: 16px;
      flex-wrap: wrap;
      opacity: 0;
      animation: fadeSlideUp 0.9s 1.1s forwards;
    }

    .btn {
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.75rem;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      padding: 14px 28px;
      border: none;
      cursor: none;
      text-decoration: none;
      transition: all 0.25s;
      display: inline-flex;
      align-items: center;
      gap: 8px;
    }
    .btn-primary {
      background: var(--accent);
      color: var(--paper);
      position: relative;
      overflow: hidden;
    }
    .btn-primary::before {
      content: '';
      position: absolute;
      inset: 0;
      background: rgba(255,255,255,0.15);
      transform: translateX(-100%);
      transition: transform 0.3s;
    }
    .btn-primary:hover::before { transform: translateX(0); }
    .btn-primary:hover { box-shadow: 0 0 24px var(--glow); transform: translateY(-2px); }

    .btn-ghost {
      background: transparent;
      color: var(--paper);
      border: 1px solid rgba(255,255,255,0.2);
    }
    .btn-ghost:hover {
      border-color: var(--accent2);
      color: var(--accent2);
      box-shadow: 0 0 18px var(--glow2);
      transform: translateY(-2px);
    }

    /* Right panel — anime art */
    .hero-right {
      position: relative;
      display: flex;
      align-items: center;
      justify-content: center;
      overflow: hidden;
    }

    /* Big kanji background */
    .bg-kanji {
      position: absolute;
      font-family: 'Noto Serif JP', serif;
      font-size: clamp(200px, 28vw, 380px);
      font-weight: 700;
      color: transparent;
      -webkit-text-stroke: 1px rgba(255,255,255,0.04);
      pointer-events: none;
      user-select: none;
      line-height: 1;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      animation: floatKanji 8s ease-in-out infinite;
    }

    /* SVG Avatar Frame */
    .avatar-frame {
      position: relative;
      width: 340px;
      height: 340px;
      z-index: 2;
      opacity: 0;
      animation: fadeIn 1.2s 0.8s forwards;
    }

    .avatar-svg-wrap {
      position: absolute;
      inset: 0;
    }

    .avatar-orb {
      position: absolute;
      inset: 20px;
      border-radius: 50%;
      background: radial-gradient(circle at 35% 30%, rgba(200,75,49,0.25), rgba(58,134,200,0.15) 50%, rgba(0,0,0,0.6));
      display: flex;
      align-items: center;
      justify-content: center;
      border: 1px solid rgba(200,75,49,0.3);
      overflow: hidden;
    }

    /* Character silhouette (CSS drawn) */
    .char-wrap {
      position: relative;
      width: 160px;
      height: 200px;
    }
    .char-head {
      position: absolute;
      width: 70px; height: 70px;
      background: linear-gradient(135deg, #e8c9a0, #d4a574);
      border-radius: 50% 50% 45% 45%;
      top: 0; left: 50%; transform: translateX(-50%);
      box-shadow: 0 0 20px rgba(200,75,49,0.3);
    }
    .char-hair {
      position: absolute;
      width: 82px; height: 45px;
      background: linear-gradient(180deg, #1a1a2e 0%, #2d1b69 100%);
      border-radius: 50% 50% 0 0;
      top: -8px; left: 50%; transform: translateX(-50%);
      z-index: 2;
    }
    .char-hair::after {
      content: '';
      position: absolute;
      width: 20px; height: 55px;
      background: linear-gradient(180deg, #1a1a2e, #2d1b69);
      border-radius: 0 0 50% 50%;
      right: -8px; top: 20px;
    }
    .char-eye-l, .char-eye-r {
      position: absolute;
      width: 10px; height: 12px;
      background: #3a86c8;
      border-radius: 50%;
      top: 34px;
      box-shadow: 0 0 8px #3a86c8;
      z-index: 3;
    }
    .char-eye-l { left: 18px; }
    .char-eye-r { right: 18px; }
    .char-body {
      position: absolute;
      width: 80px; height: 100px;
      background: linear-gradient(160deg, #1c1c2e, #2a2a4a);
      border-radius: 8px 8px 12px 12px;
      top: 62px; left: 50%; transform: translateX(-50%);
      border: 1px solid rgba(58,134,200,0.3);
    }
    .char-body::before {
      content: '</>';
      position: absolute;
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.7rem;
      color: var(--accent2);
      top: 50%; left: 50%;
      transform: translate(-50%, -50%);
      opacity: 0.8;
    }
    .char-arm-l, .char-arm-r {
      position: absolute;
      width: 18px; height: 75px;
      background: linear-gradient(160deg, #1c1c2e, #2a2a4a);
      border-radius: 9px;
      top: 65px;
      border: 1px solid rgba(58,134,200,0.2);
    }
    .char-arm-l { left: 22px; transform: rotate(5deg); }
    .char-arm-r { right: 22px; transform: rotate(-5deg); }

    /* Rotating ring */
    .ring {
      position: absolute;
      border-radius: 50%;
      border: 1px dashed rgba(200,75,49,0.35);
      animation: spin 20s linear infinite;
    }
    .ring-1 { inset: 0; animation-duration: 18s; }
    .ring-2 { inset: -20px; border-color: rgba(58,134,200,0.2); animation-duration: 28s; animation-direction: reverse; }
    .ring-3 { inset: -45px; border-color: rgba(201,168,76,0.15); animation-duration: 40s; }

    /* Floating orbs */
    .orb {
      position: absolute;
      border-radius: 50%;
      pointer-events: none;
    }
    .orb-1 {
      width: 6px; height: 6px;
      background: var(--accent);
      top: 20%; right: 25%;
      box-shadow: 0 0 12px var(--accent);
      animation: orbit1 6s ease-in-out infinite;
    }
    .orb-2 {
      width: 4px; height: 4px;
      background: var(--accent2);
      bottom: 25%; left: 20%;
      box-shadow: 0 0 8px var(--accent2);
      animation: orbit2 8s ease-in-out infinite;
    }
    .orb-3 {
      width: 5px; height: 5px;
      background: var(--gold);
      top: 60%; right: 18%;
      box-shadow: 0 0 10px var(--gold);
      animation: orbit1 5s 2s ease-in-out infinite;
    }

    /* Side vertical text */
    .side-text {
      position: absolute;
      right: 24px; top: 50%;
      transform: translateY(-50%) rotate(90deg);
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.6rem;
      letter-spacing: 0.4em;
      color: rgba(255,255,255,0.15);
      white-space: nowrap;
      z-index: 2;
    }

    /* Floating data particles */
    .particles {
      position: absolute;
      inset: 0;
      pointer-events: none;
      overflow: hidden;
    }
    .particle {
      position: absolute;
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.6rem;
      color: rgba(58,134,200,0.25);
      animation: particleFall linear infinite;
      white-space: nowrap;
    }

    /* ── ABOUT BAND ── */
    #about {
      padding: 100px 80px;
      position: relative;
      overflow: hidden;
    }
    #about::before {
      content: '───────────────────────';
      display: block;
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.65rem;
      color: rgba(255,255,255,0.1);
      letter-spacing: 0.1em;
      margin-bottom: 60px;
    }
    .section-label {
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.65rem;
      letter-spacing: 0.4em;
      color: var(--accent);
      text-transform: uppercase;
      margin-bottom: 16px;
    }
    .section-title {
      font-family: 'Noto Serif JP', serif;
      font-size: clamp(2rem, 4vw, 3.2rem);
      font-weight: 700;
      margin-bottom: 48px;
      line-height: 1.15;
    }
    .section-title em {
      font-style: normal;
      color: var(--accent);
    }

    .interests-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 2px;
      margin-bottom: 60px;
    }
    .interest-card {
      background: rgba(255,255,255,0.02);
      border: 1px solid rgba(255,255,255,0.06);
      padding: 28px 24px;
      transition: all 0.3s;
      position: relative;
      overflow: hidden;
    }
    .interest-card::before {
      content: '';
      position: absolute;
      bottom: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      transform: scaleX(0);
      transition: transform 0.3s;
      transform-origin: left;
    }
    .interest-card:hover { background: rgba(255,255,255,0.04); transform: translateY(-4px); }
    .interest-card:hover::before { transform: scaleX(1); }
    .interest-icon { font-size: 1.8rem; margin-bottom: 12px; display: block; }
    .interest-name {
      font-family: 'Noto Serif JP', serif;
      font-size: 0.95rem;
      font-weight: 400;
      margin-bottom: 4px;
    }
    .interest-jp {
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.6rem;
      color: var(--muted);
      letter-spacing: 0.2em;
    }

    /* ── SKILLS ── */
    #skills {
      padding: 80px 80px 100px;
      position: relative;
    }

    .skills-layout {
      display: grid;
      grid-template-columns: 1.2fr 1fr;
      gap: 80px;
      align-items: start;
    }

    .skill-category { margin-bottom: 36px; }
    .skill-cat-label {
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.6rem;
      letter-spacing: 0.35em;
      color: var(--muted);
      text-transform: uppercase;
      margin-bottom: 14px;
    }
    .skill-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }
    .skill-tag {
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.7rem;
      letter-spacing: 0.1em;
      padding: 7px 14px;
      border: 1px solid rgba(255,255,255,0.12);
      color: var(--paper);
      position: relative;
      transition: all 0.2s;
      cursor: none;
    }
    .skill-tag:hover {
      border-color: var(--accent);
      color: var(--accent);
      box-shadow: 0 0 12px var(--glow);
    }
    .skill-tag.highlight {
      border-color: rgba(58,134,200,0.4);
      color: var(--accent2);
    }
    .skill-tag.highlight:hover {
      border-color: var(--accent2);
      box-shadow: 0 0 12px var(--glow2);
    }

    /* Skill XP bars */
    .xp-bar-list { display: flex; flex-direction: column; gap: 20px; }
    .xp-item {}
    .xp-header {
      display: flex;
      justify-content: space-between;
      align-items: baseline;
      margin-bottom: 8px;
    }
    .xp-name {
      font-family: 'Noto Serif JP', serif;
      font-size: 0.9rem;
      font-weight: 400;
    }
    .xp-level {
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.6rem;
      color: var(--gold);
      letter-spacing: 0.2em;
    }
    .xp-track {
      height: 3px;
      background: rgba(255,255,255,0.07);
      position: relative;
      overflow: visible;
    }
    .xp-fill {
      height: 100%;
      background: linear-gradient(90deg, var(--accent), var(--accent2));
      position: relative;
      transform-origin: left;
      transform: scaleX(0);
      transition: transform 1.2s cubic-bezier(0.16, 1, 0.3, 1);
    }
    .xp-fill.animated { transform: scaleX(1); }
    .xp-fill::after {
      content: '';
      position: absolute;
      right: 0; top: 50%;
      transform: translateY(-50%);
      width: 6px; height: 6px;
      border-radius: 50%;
      background: var(--accent2);
      box-shadow: 0 0 8px var(--accent2);
    }

    /* ── STAT PANEL (RPG style) ── */
    .rpg-card {
      background: rgba(255,255,255,0.02);
      border: 1px solid rgba(255,255,255,0.08);
      padding: 32px;
      position: relative;
    }
    .rpg-card::before {
      content: 'PLAYER.STATUS';
      position: absolute;
      top: -1px; left: 20px;
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.55rem;
      letter-spacing: 0.3em;
      background: var(--ink);
      padding: 0 8px;
      color: var(--gold);
    }
    .rpg-stat {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 12px 0;
      border-bottom: 1px solid rgba(255,255,255,0.04);
    }
    .rpg-stat:last-child { border-bottom: none; }
    .rpg-stat-key {
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.65rem;
      letter-spacing: 0.2em;
      color: var(--muted);
      text-transform: uppercase;
    }
    .rpg-stat-val {
      font-family: 'Noto Serif JP', serif;
      font-size: 0.85rem;
      color: var(--paper);
    }
    .rpg-stat-val.hot { color: var(--accent); }
    .rpg-stat-val.cool { color: var(--accent2); }

    /* ── FOOTER ── */
    footer {
      padding: 60px 80px;
      border-top: 1px solid rgba(255,255,255,0.06);
      display: flex;
      align-items: center;
      justify-content: space-between;
      flex-wrap: wrap;
      gap: 24px;
    }
    .footer-sig {
      font-family: 'Noto Serif JP', serif;
      font-size: 0.9rem;
      color: var(--muted);
    }
    .footer-sig strong { color: var(--paper); }
    .footer-links {
      display: flex;
      gap: 24px;
    }
    .footer-link {
      font-family: 'Share Tech Mono', monospace;
      font-size: 0.65rem;
      letter-spacing: 0.25em;
      color: var(--muted);
      text-decoration: none;
      text-transform: uppercase;
      transition: color 0.2s;
    }
    .footer-link:hover { color: var(--accent); }

    /* ── DIVIDER ── */
    .haiku-strip {
      padding: 48px 80px;
      display: flex;
      align-items: center;
      gap: 48px;
      border-top: 1px solid rgba(255,255,255,0.04);
      border-bottom: 1px solid rgba(255,255,255,0.04);
      overflow: hidden;
      position: relative;
    }
    .haiku-line {
      font-family: 'Noto Serif JP', serif;
      font-size: 0.85rem;
      font-weight: 300;
      color: var(--muted);
      white-space: nowrap;
      letter-spacing: 0.08em;
    }
    .haiku-sep {
      width: 1px; height: 40px;
      background: rgba(255,255,255,0.1);
      flex-shrink: 0;
    }
    .haiku-kanji {
      font-family: 'Noto Serif JP', serif;
      font-size: 1.8rem;
      color: rgba(200,75,49,0.4);
      margin-left: auto;
    }

    /* ── ANIMATIONS ── */
    @keyframes fadeSlideUp {
      from { opacity: 0; transform: translateY(24px); }
      to   { opacity: 1; transform: translateY(0); }
    }
    @keyframes fadeIn {
      from { opacity: 0; } to { opacity: 1; }
    }
    @keyframes lineExpand {
      from { transform: scaleX(0); } to { transform: scaleX(1); }
    }
    @keyframes spin {
      from { transform: rotate(0deg); } to { transform: rotate(360deg); }
    }
    @keyframes floatKanji {
      0%, 100% { transform: translate(-50%, -50%) rotate(-3deg); }
      50%       { transform: translate(-50%, -54%) rotate(3deg); }
    }
    @keyframes orbit1 {
      0%, 100% { transform: translate(0,0); }
      33%       { transform: translate(12px, -8px); }
      66%       { transform: translate(-6px, 10px); }
    }
    @keyframes orbit2 {
      0%, 100% { transform: translate(0,0); }
      50%       { transform: translate(-14px, -10px); }
    }
    @keyframes particleFall {
      0%   { transform: translateY(-20px); opacity: 0; }
      10%  { opacity: 1; }
      90%  { opacity: 0.4; }
      100% { transform: translateY(110vh); opacity: 0; }
    }
    @keyframes pulse {
      0%, 100% { opacity: 1; } 50% { opacity: 0.4; }
    }

    /* ── RESPONSIVE ── */
    @media (max-width: 860px) {
      #hero { grid-template-columns: 1fr; }
      .hero-right { display: none; }
      .hero-left { padding: 80px 32px 60px; }
      #about, #skills, footer, .haiku-strip { padding-left: 32px; padding-right: 32px; }
      .skills-layout { grid-template-columns: 1fr; gap: 48px; }
    }

    /* ── SCROLL REVEAL ── */
    .reveal {
      opacity: 0;
      transform: translateY(30px);
      transition: opacity 0.8s, transform 0.8s;
    }
    .reveal.visible {
      opacity: 1;
      transform: translateY(0);
    }
  </style>
</head>
<body>

  <div class="cursor" id="cursor"></div>
  <div class="cursor-ring" id="cursorRing"></div>

  <!-- ══ HERO ══ -->
  <section id="hero">
    <div class="hero-left">
      <p class="eyebrow">// github.com/MesQue</p>
      <h1 class="hero-name">Mes<span>Que</span></h1>
      <p class="jp-sub">ウェブ開発者 ＆ 創造者</p>
      <p class="hero-tagline">
        Crafting digital worlds one commit at a time — between cups of coffee, 
        manga chapters, and watching clouds drift like rendered meshes across an open sky.
      </p>
      <div class="hero-cta">
        <a href="https://github.com/MesQue" class="btn btn-primary" target="_blank">
          ⬡ View Projects
        </a>
        <a href="#about" class="btn btn-ghost">
          ↓ About Me
        </a>
      </div>
    </div>
    <div class="hero-right">
      <div class="bg-kanji">夢</div>
      <div class="particles" id="particles"></div>

      <div class="avatar-frame">
        <div class="ring ring-1"></div>
        <div class="ring ring-2"></div>
        <div class="ring ring-3"></div>
        <div class="avatar-orb">
          <div class="char-wrap">
            <div class="char-hair"></div>
            <div class="char-head">
              <div class="char-eye-l"></div>
              <div class="char-eye-r"></div>
            </div>
            <div class="char-arm-l"></div>
            <div class="char-arm-r"></div>
            <div class="char-body"></div>
          </div>
        </div>
        <div class="orb orb-1"></div>
        <div class="orb orb-2"></div>
        <div class="orb orb-3"></div>
      </div>

      <div class="side-text">MESQUE · DEVELOPER · 開発者 · v2.0.26</div>
    </div>
  </section>

  <!-- ══ HAIKU STRIP ══ -->
  <div class="haiku-strip">
    <span class="haiku-line">sipping morning brew</span>
    <div class="haiku-sep"></div>
    <span class="haiku-line">clouds compile overhead</span>
    <div class="haiku-sep"></div>
    <span class="haiku-line">commit pushed at last</span>
    <span class="haiku-kanji">夢想</span>
  </div>

  <!-- ══ ABOUT ══ -->
  <section id="about">
    <p class="section-label reveal">// character.info</p>
    <h2 class="section-title reveal">A dev who <em>dreams</em><br>in code &amp; ink</h2>

    <div class="interests-grid">
      <div class="interest-card reveal">
        <span class="interest-icon">☕</span>
        <div class="interest-name">Coffee & Tea</div>
        <div class="interest-jp">BREWED THOUGHTS</div>
      </div>
      <div class="interest-card reveal">
        <span class="interest-icon">📖</span>
        <div class="interest-name">Reading</div>
        <div class="interest-jp">INFINITE WORLDS</div>
      </div>
      <div class="interest-card reveal">
        <span class="interest-icon">☁️</span>
        <div class="interest-name">Cloud Gazing</div>
        <div class="interest-jp">RENDERED SKIES</div>
      </div>
      <div class="interest-card reveal">
        <span class="interest-icon">⛩</span>
        <div class="interest-name">Anime & Manga</div>
        <div class="interest-jp">ANIMATED SOUL</div>
      </div>
      <div class="interest-card reveal">
        <span class="interest-icon">🎨</span>
        <div class="interest-name">Art</div>
        <div class="interest-jp">PIXEL POETRY</div>
      </div>
    </div>
  </section>

  <!-- ══ SKILLS ══ -->
  <section id="skills">
    <p class="section-label reveal">// skills.loadout</p>
    <h2 class="section-title reveal">The <em>Arsenal</em></h2>

    <div class="skills-layout">
      <div>
        <div class="skill-category reveal">
          <div class="skill-cat-label">Frontend</div>
          <div class="skill-tags">
            <span class="skill-tag highlight">JavaScript</span>
            <span class="skill-tag highlight">React</span>
            <span class="skill-tag highlight">Next.js</span>
            <span class="skill-tag highlight">Angular</span>
          </div>
        </div>
        <div class="skill-category reveal">
          <div class="skill-cat-label">Backend</div>
          <div class="skill-tags">
            <span class="skill-tag">Node.js</span>
            <span class="skill-tag">NestJS</span>
            <span class="skill-tag">Express</span>
            <span class="skill-tag">Java</span>
            <span class="skill-tag">C#</span>
            <span class="skill-tag">Python</span>
          </div>
        </div>
        <div class="skill-category reveal">
          <div class="skill-cat-label">Data & AI</div>
          <div class="skill-tags">
            <span class="skill-tag">Machine Learning</span>
            <span class="skill-tag">SQL</span>
            <span class="skill-tag">MongoDB</span>
          </div>
        </div>
        <div class="skill-category reveal">
          <div class="skill-cat-label">Stack</div>
          <div class="skill-tags">
            <span class="skill-tag highlight">MERN</span>
            <span class="skill-tag">REST APIs</span>
            <span class="skill-tag">GraphQL</span>
          </div>
        </div>
      </div>

      <div class="reveal">
        <div class="rpg-card" style="margin-bottom: 24px;">
          <div class="rpg-stat">
            <span class="rpg-stat-key">Class</span>
            <span class="rpg-stat-val hot">Full-Stack Mage</span>
          </div>
          <div class="rpg-stat">
            <span class="rpg-stat-key">Spec</span>
            <span class="rpg-stat-val cool">Web Architect</span>
          </div>
          <div class="rpg-stat">
            <span class="rpg-stat-key">Guild</span>
            <span class="rpg-stat-val">github/MesQue</span>
          </div>
          <div class="rpg-stat">
            <span class="rpg-stat-key">Status</span>
            <span class="rpg-stat-val hot">● Building</span>
          </div>
          <div class="rpg-stat">
            <span class="rpg-stat-key">Buff</span>
            <span class="rpg-stat-val" style="color:var(--gold)">☕ Caffeinated</span>
          </div>
        </div>

        <div class="xp-bar-list">
          <div class="xp-item reveal">
            <div class="xp-header">
              <span class="xp-name">Web Development</span>
              <span class="xp-level">MAX LVL</span>
            </div>
            <div class="xp-track"><div class="xp-fill" data-width="0.95"></div></div>
          </div>
          <div class="xp-item reveal">
            <div class="xp-header">
              <span class="xp-name">JavaScript / TS</span>
              <span class="xp-level">LVL 90</span>
            </div>
            <div class="xp-track"><div class="xp-fill" data-width="0.90"></div></div>
          </div>
          <div class="xp-item reveal">
            <div class="xp-header">
              <span class="xp-name">Backend Systems</span>
              <span class="xp-level">LVL 82</span>
            </div>
            <div class="xp-track"><div class="xp-fill" data-width="0.82"></div></div>
          </div>
          <div class="xp-item reveal">
            <div class="xp-header">
              <span class="xp-name">Machine Learning</span>
              <span class="xp-level">LVL 75</span>
            </div>
            <div class="xp-track"><div class="xp-fill" data-width="0.75"></div></div>
          </div>
          <div class="xp-item reveal">
            <div class="xp-header">
              <span class="xp-name">Coffee Brewing</span>
              <span class="xp-level">LEGENDARY</span>
            </div>
            <div class="xp-track"><div class="xp-fill" data-width="1.0"></div></div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- ══ FOOTER ══ -->
  <footer>
    <div class="footer-sig">
      <strong>MesQue</strong> — crafted with ☕ &amp; ♡ somewhere between clouds
    </div>
    <nav class="footer-links">
      <a href="https://github.com/MesQue" class="footer-link" target="_blank">GitHub</a>
      <a href="#hero" class="footer-link">↑ Top</a>
    </nav>
  </footer>

  <script>
    // ── Cursor ──
    const cursor = document.getElementById('cursor');
    const ring = document.getElementById('cursorRing');
    let mx = 0, my = 0, rx = 0, ry = 0;
    document.addEventListener('mousemove', e => { mx = e.clientX; my = e.clientY; });
    function animCursor() {
      cursor.style.left = mx + 'px'; cursor.style.top = my + 'px';
      rx += (mx - rx) * 0.12; ry += (my - ry) * 0.12;
      ring.style.left = rx + 'px'; ring.style.top = ry + 'px';
      requestAnimationFrame(animCursor);
    }
    animCursor();
    document.querySelectorAll('a, button, .interest-card, .skill-tag').forEach(el => {
      el.addEventListener('mouseenter', () => {
        cursor.style.width = '20px'; cursor.style.height = '20px';
        ring.style.width = '60px'; ring.style.height = '60px';
        ring.style.borderColor = 'rgba(200,75,49,0.5)';
      });
      el.addEventListener('mouseleave', () => {
        cursor.style.width = '12px'; cursor.style.height = '12px';
        ring.style.width = '36px'; ring.style.height = '36px';
        ring.style.borderColor = 'rgba(200,75,49,0.3)';
      });
    });

    // ── Particles ──
    const pContainer = document.getElementById('particles');
    const pTexts = ['01', 'JS', '//','{}','[]','fn','</>','ML','DB','>_','※','∞','⬡','✦'];
    function spawnParticle() {
      const p = document.createElement('div');
      p.className = 'particle';
      p.textContent = pTexts[Math.floor(Math.random() * pTexts.length)];
      p.style.left = Math.random() * 100 + '%';
      p.style.top = '-20px';
      const dur = 6 + Math.random() * 10;
      p.style.animationDuration = dur + 's';
      p.style.animationDelay = Math.random() * -dur + 's';
      pContainer.appendChild(p);
      setTimeout(() => p.remove(), (dur + 2) * 1000);
    }
    for (let i = 0; i < 20; i++) spawnParticle();
    setInterval(spawnParticle, 1200);

    // ── Scroll reveal ──
    const reveals = document.querySelectorAll('.reveal');
    const observer = new IntersectionObserver(entries => {
      entries.forEach(e => {
        if (e.isIntersecting) {
          e.target.classList.add('visible');
          // XP bars
          const fill = e.target.querySelector('.xp-fill') || (e.target.classList.contains('xp-item') ? e.target.querySelector('.xp-fill') : null);
          if (fill) {
            setTimeout(() => {
              fill.style.width = (parseFloat(fill.dataset.width) * 100) + '%';
              fill.classList.add('animated');
            }, 200);
          }
        }
      });
    }, { threshold: 0.15 });
    reveals.forEach(el => observer.observe(el));

    // Trigger XP fills on scroll for all fills
    document.querySelectorAll('.xp-fill').forEach(fill => {
      const itemObserver = new IntersectionObserver(entries => {
        entries.forEach(e => {
          if (e.isIntersecting) {
            setTimeout(() => {
              fill.style.width = (parseFloat(fill.dataset.width) * 100) + '%';
              fill.classList.add('animated');
            }, 300 + Math.random() * 200);
          }
        });
      }, { threshold: 0.5 });
      itemObserver.observe(fill.parentElement);
    });
  </script>
</body>
</html>
