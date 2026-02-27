<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>ANI_X // SYSTEM ONLINE</title>
<link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&family=VT323&display=swap" rel="stylesheet" />
<style>
  :root {
    --cyan:    #00f5ff;
    --magenta: #ff00cc;
    --green:   #00ff88;
    --red:     #ff2244;
    --yellow:  #ffee00;
    --orange:  #ff7700;
    --bg:      #000005;
    --panel:   #04040f;
  }

  *, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }

  html, body {
    background: var(--bg);
    color: var(--cyan);
    font-family: 'Share Tech Mono', monospace;
    min-height: 100vh;
    overflow-x: hidden;
    cursor: crosshair;
  }

  /* ── SCANLINES ── */
  body::before {
    content: '';
    position: fixed; inset: 0; z-index: 9999; pointer-events: none;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.18) 2px,
      rgba(0,0,0,0.18) 4px
    );
    animation: scanMove 8s linear infinite;
  }
  @keyframes scanMove { from{background-position:0 0} to{background-position:0 100px} }

  /* ── MATRIX RAIN CANVAS ── */
  #matrix { position:fixed; inset:0; z-index:0; opacity:0.18; }

  /* ── GRID FLOOR ── */
  .grid-floor {
    position: fixed; bottom: 0; left: 0; right: 0;
    height: 340px; z-index: 1; pointer-events: none;
    background:
      linear-gradient(to bottom, transparent 0%, rgba(0,245,255,0.04) 100%),
      repeating-linear-gradient(90deg, rgba(0,245,255,0.07) 0 1px, transparent 1px 80px),
      repeating-linear-gradient(0deg,  rgba(0,245,255,0.07) 0 1px, transparent 1px 60px);
    transform: perspective(500px) rotateX(55deg);
    transform-origin: bottom center;
  }

  /* ── STARS ── */
  .stars { position:fixed; inset:0; z-index:0; pointer-events:none; }
  .star {
    position:absolute; background:#fff; border-radius:50%;
    animation: twinkle var(--d) ease-in-out infinite alternate;
  }
  @keyframes twinkle { from{opacity:0.1;transform:scale(1)} to{opacity:0.9;transform:scale(1.4)} }

  /* ── MAIN WRAPPER ── */
  .wrapper {
    position: relative; z-index: 10;
    max-width: 960px; margin: 0 auto;
    padding: 24px 20px 60px;
  }

  /* ── ARCADE CABINET BORDER ── */
  .arcade-frame {
    border: 2px solid var(--cyan);
    box-shadow:
      0 0 8px var(--cyan),
      0 0 24px rgba(0,245,255,0.3),
      0 0 60px rgba(0,245,255,0.1),
      inset 0 0 40px rgba(0,245,255,0.04);
    border-radius: 4px;
    padding: 32px 28px;
    background: rgba(0,0,10,0.85);
    position: relative;
    animation: frameGlow 3s ease-in-out infinite alternate;
  }
  @keyframes frameGlow {
    from { box-shadow: 0 0 8px var(--cyan), 0 0 24px rgba(0,245,255,0.3), 0 0 60px rgba(0,245,255,0.1), inset 0 0 40px rgba(0,245,255,0.04); border-color: var(--cyan); }
    to   { box-shadow: 0 0 14px var(--magenta), 0 0 40px rgba(255,0,204,0.3), 0 0 80px rgba(255,0,204,0.12), inset 0 0 40px rgba(255,0,204,0.04); border-color: var(--magenta); }
  }

  /* corner screws */
  .arcade-frame::before, .arcade-frame::after,
  .corner-tl, .corner-br {
    content:'▪';
    position:absolute;
    font-size:10px;
    color: var(--cyan);
    animation: cornerBlink 2s step-start infinite;
  }
  .arcade-frame::before { top:8px; left:12px; }
  .arcade-frame::after  { top:8px; right:12px; }
  .corner-tl { bottom:8px; left:12px; }
  .corner-br { bottom:8px; right:12px; }
  @keyframes cornerBlink {
    0%,80%{color:var(--cyan)} 40%{color:var(--magenta)}
  }

  /* ── WELCOME BANNER ── */
  .welcome-bar {
    text-align: center;
    font-family: 'VT323', monospace;
    font-size: 1.2rem;
    letter-spacing: 0.35em;
    color: var(--green);
    text-shadow: 0 0 8px var(--green);
    margin-bottom: 8px;
    animation: marqueeGlow 1.5s ease-in-out infinite alternate;
  }
  @keyframes marqueeGlow {
    from { text-shadow: 0 0 6px var(--green), 0 0 14px var(--green); }
    to   { text-shadow: 0 0 12px var(--green), 0 0 30px var(--green), 0 0 60px rgba(0,255,136,0.4); }
  }

  .welcome-line {
    text-align: center;
    font-family: 'Orbitron', monospace;
    font-size: clamp(1rem, 4vw, 1.5rem);
    font-weight: 900;
    letter-spacing: 0.5em;
    color: var(--yellow);
    text-shadow: 0 0 10px var(--yellow), 0 0 30px rgba(255,238,0,0.5);
    margin-bottom: 28px;
    animation: welcomePulse 2s ease-in-out infinite alternate;
  }
  @keyframes welcomePulse {
    from { text-shadow: 0 0 10px var(--yellow), 0 0 30px rgba(255,238,0,0.4); opacity: 0.85; }
    to   { text-shadow: 0 0 18px var(--yellow), 0 0 50px rgba(255,238,0,0.7), 0 0 90px rgba(255,238,0,0.3); opacity: 1; }
  }

  /* ── ANI_X HERO ── */
  .hero {
    text-align: center;
    margin-bottom: 36px;
    position: relative;
  }

  .anix-name {
    font-family: 'Orbitron', monospace;
    font-weight: 900;
    font-size: clamp(3.5rem, 12vw, 7.5rem);
    letter-spacing: 0.1em;
    line-height: 1;
    background: linear-gradient(90deg, var(--cyan), var(--magenta), var(--cyan));
    background-size: 200% auto;
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: nameShift 3s linear infinite, glitchName 7s step-start infinite;
    position: relative;
    display: inline-block;
  }
  @keyframes nameShift { from{background-position:0%} to{background-position:200%} }
  @keyframes glitchName {
    0%,90%,100% { transform:none; filter:none; }
    91% { transform:translate(-3px,0) skewX(-2deg); filter:hue-rotate(90deg); }
    93% { transform:translate(3px,0) skewX(2deg); }
    95% { transform:translate(-2px,0); filter:hue-rotate(-60deg) brightness(1.5); }
    97% { transform:none; filter:none; }
  }

  .anix-name::before {
    content: attr(data-text);
    position: absolute; inset: 0;
    background: linear-gradient(90deg, var(--red), var(--magenta));
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    animation: glitchShadow 7s step-start infinite;
    left: 0; top: 0;
  }
  @keyframes glitchShadow {
    0%,88%,100% { clip-path:none; transform:none; opacity:0; }
    89% { clip-path:polygon(0 30%,100% 30%,100% 50%,0 50%); transform:translate(4px,0); opacity:0.7; }
    91% { clip-path:polygon(0 60%,100% 60%,100% 80%,0 80%); transform:translate(-3px,0); opacity:0.5; }
    93% { opacity:0; }
  }

  .tagline {
    font-family: 'VT323', monospace;
    font-size: clamp(1rem, 3.5vw, 1.5rem);
    letter-spacing: 0.3em;
    color: var(--magenta);
    text-shadow: 0 0 8px var(--magenta), 0 0 20px rgba(255,0,204,0.4);
    margin-top: 10px;
    animation: tagPulse 2.5s ease-in-out infinite alternate;
  }
  @keyframes tagPulse {
    from { opacity:0.7; text-shadow: 0 0 6px var(--magenta); }
    to   { opacity:1;   text-shadow: 0 0 14px var(--magenta), 0 0 35px rgba(255,0,204,0.6); }
  }

  /* ── DIVIDER ── */
  .neon-divider {
    height: 2px;
    margin: 20px 0;
    background: linear-gradient(90deg, transparent, var(--cyan), var(--magenta), var(--green), transparent);
    background-size: 200% auto;
    animation: divSlide 3s linear infinite;
    box-shadow: 0 0 8px var(--cyan);
  }
  @keyframes divSlide { from{background-position:0%} to{background-position:200%} }

  /* ── SECTION HEADERS ── */
  .section-head {
    font-family: 'Orbitron', monospace;
    font-weight: 700;
    font-size: clamp(0.65rem, 2vw, 0.85rem);
    letter-spacing: 0.4em;
    display: flex; align-items: center; gap: 12px;
    margin-bottom: 16px;
  }
  .section-head .label {
    padding: 4px 14px;
    border: 1px solid currentColor;
    box-shadow: 0 0 8px currentColor, inset 0 0 8px rgba(0,0,0,0.5);
    animation: sectionGlow 2s ease-in-out infinite alternate;
  }
  .section-head .line { flex:1; height:1px; background: currentColor; opacity:0.3; }
  @keyframes sectionGlow {
    from { box-shadow: 0 0 6px currentColor; }
    to   { box-shadow: 0 0 16px currentColor, 0 0 30px rgba(255,255,255,0.1); }
  }
  .c-cyan    { color: var(--cyan); }
  .c-magenta { color: var(--magenta); }
  .c-green   { color: var(--green); }
  .c-red     { color: var(--red); }
  .c-yellow  { color: var(--yellow); }
  .c-orange  { color: var(--orange); }

  /* ── PROJECT LOGS ── */
  .log-grid { display: grid; gap: 10px; }

  .log-entry {
    background: rgba(0,245,255,0.03);
    border: 1px solid rgba(0,245,255,0.15);
    border-left: 3px solid var(--cyan);
    padding: 12px 16px;
    position: relative;
    transition: all 0.3s ease;
    animation: entryFadeIn 0.6s ease both;
    cursor: default;
  }
  .log-entry:nth-child(1) { animation-delay:0.05s; border-left-color: var(--cyan); }
  .log-entry:nth-child(2) { animation-delay:0.1s;  border-left-color: var(--magenta); }
  .log-entry:nth-child(3) { animation-delay:0.15s; border-left-color: var(--green); }
  .log-entry:nth-child(4) { animation-delay:0.2s;  border-left-color: var(--red); }
  .log-entry:nth-child(5) { animation-delay:0.25s; border-left-color: var(--yellow); }
  .log-entry:nth-child(6) { animation-delay:0.3s;  border-left-color: var(--orange); }
  .log-entry:nth-child(7) { animation-delay:0.35s; border-left-color: var(--magenta); }

  @keyframes entryFadeIn {
    from { opacity:0; transform:translateX(-20px); }
    to   { opacity:1; transform:translateX(0); }
  }

  .log-entry:hover {
    background: rgba(0,245,255,0.07);
    border-color: var(--cyan);
    box-shadow: 0 0 16px rgba(0,245,255,0.15), inset 0 0 20px rgba(0,245,255,0.04);
    transform: translateX(4px);
  }

  .log-id {
    font-family: 'VT323', monospace;
    font-size: 0.85rem;
    color: var(--magenta);
    margin-right: 8px;
  }
  .log-title {
    font-family: 'Orbitron', monospace;
    font-size: clamp(0.6rem, 1.8vw, 0.75rem);
    font-weight: 700;
    color: var(--cyan);
    letter-spacing: 0.08em;
  }
  .log-desc {
    font-size: 0.72rem;
    color: rgba(0,245,255,0.55);
    margin-top: 5px;
    line-height: 1.5;
  }

  /* ── STACK BADGES ── */
  .stack-section { margin-bottom: 24px; }
  .stack-label {
    font-family: 'VT323', monospace;
    font-size: 0.85rem;
    letter-spacing: 0.25em;
    color: var(--magenta);
    margin-bottom: 8px;
  }
  .badge-row { display: flex; flex-wrap: wrap; gap: 8px; }
  .badge {
    font-family: 'Share Tech Mono', monospace;
    font-size: 0.65rem;
    letter-spacing: 0.1em;
    padding: 4px 12px;
    border: 1px solid;
    border-radius: 2px;
    position: relative;
    cursor: default;
    transition: all 0.25s ease;
    text-transform: uppercase;
  }
  .badge::before {
    content:'';
    position:absolute; inset:0;
    background: currentColor;
    opacity:0.07;
    border-radius: 2px;
  }
  .badge:hover {
    transform: translateY(-2px) scale(1.05);
    box-shadow: 0 4px 20px rgba(0,0,0,0.5), 0 0 12px currentColor;
  }
  .b-cyan    { color: var(--cyan);    border-color: var(--cyan);    text-shadow: 0 0 6px var(--cyan); }
  .b-magenta { color: var(--magenta); border-color: var(--magenta); text-shadow: 0 0 6px var(--magenta); }
  .b-green   { color: var(--green);   border-color: var(--green);   text-shadow: 0 0 6px var(--green); }
  .b-red     { color: var(--red);     border-color: var(--red);     text-shadow: 0 0 6px var(--red); }
  .b-yellow  { color: var(--yellow);  border-color: var(--yellow);  text-shadow: 0 0 6px var(--yellow); }
  .b-orange  { color: var(--orange);  border-color: var(--orange);  text-shadow: 0 0 6px var(--orange); }

  /* ── HUD STATS ── */
  .hud-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
  .hud-panel {
    background: rgba(0,0,0,0.6);
    border: 1px solid rgba(0,245,255,0.2);
    padding: 16px;
    position: relative;
    animation: hudBlink 4s ease-in-out infinite alternate;
  }
  @keyframes hudBlink {
    from { border-color: rgba(0,245,255,0.2); box-shadow: none; }
    to   { border-color: rgba(0,245,255,0.5); box-shadow: 0 0 12px rgba(0,245,255,0.1); }
  }
  .hud-panel-full { grid-column: 1 / -1; }
  .hud-label {
    font-family: 'VT323', monospace;
    font-size: 0.7rem;
    letter-spacing: 0.3em;
    color: var(--magenta);
    margin-bottom: 6px;
    text-shadow: 0 0 6px var(--magenta);
  }

  .stat-bar { margin: 8px 0; }
  .stat-name { font-size: 0.65rem; color: rgba(0,245,255,0.6); margin-bottom: 3px; }
  .stat-track {
    height: 4px;
    background: rgba(0,245,255,0.1);
    border-radius: 2px;
    overflow: hidden;
  }
  .stat-fill {
    height: 100%;
    border-radius: 2px;
    animation: fillLoad 2s cubic-bezier(0.25,0.46,0.45,0.94) both;
    animation-delay: var(--delay, 0s);
    background: var(--col, var(--cyan));
    box-shadow: 0 0 6px var(--col, var(--cyan));
    transform-origin: left;
    width: var(--w, 80%);
  }
  @keyframes fillLoad { from{transform:scaleX(0)} to{transform:scaleX(1)} }

  /* ── ENERGY LINE ── */
  .energy-bar {
    text-align: center;
    font-family: 'VT323', monospace;
    font-size: 1.1rem;
    letter-spacing: 0.4em;
    padding: 12px;
    border-top: 1px solid rgba(255,119,0,0.3);
    border-bottom: 1px solid rgba(255,119,0,0.3);
    color: var(--orange);
    text-shadow: 0 0 10px var(--orange);
    margin: 24px 0;
    animation: energyPulse 2s ease-in-out infinite alternate;
  }
  @keyframes energyPulse {
    from { color: var(--orange); text-shadow: 0 0 8px var(--orange); border-color: rgba(255,119,0,0.3); }
    to   { color: var(--yellow); text-shadow: 0 0 16px var(--yellow), 0 0 40px rgba(255,238,0,0.4); border-color: rgba(255,238,0,0.5); }
  }

  /* ── TICKER ── */
  .ticker-wrap {
    overflow: hidden;
    border-top: 1px solid rgba(0,245,255,0.15);
    border-bottom: 1px solid rgba(0,245,255,0.15);
    padding: 6px 0;
    margin: 20px 0;
    position: relative;
  }
  .ticker-wrap::before, .ticker-wrap::after {
    content:'';
    position:absolute; top:0; bottom:0; width:60px; z-index:2;
  }
  .ticker-wrap::before { left:0;  background: linear-gradient(90deg, var(--bg), transparent); }
  .ticker-wrap::after  { right:0; background: linear-gradient(-90deg, var(--bg), transparent); }
  .ticker {
    display: flex; gap: 64px;
    white-space: nowrap;
    animation: tickerRun 18s linear infinite;
    font-family: 'VT323', monospace;
    font-size: 0.8rem;
    letter-spacing: 0.2em;
    color: rgba(0,245,255,0.5);
  }
  .ticker span { flex-shrink:0; }
  @keyframes tickerRun { from{transform:translateX(0)} to{transform:translateX(-50%)} }

  /* ── FOOTER ── */
  .footer {
    text-align: center;
    margin-top: 32px;
    padding-top: 20px;
    border-top: 1px solid rgba(0,245,255,0.1);
  }
  .footer-status {
    font-family: 'VT323', monospace;
    font-size: clamp(1rem, 3vw, 1.3rem);
    letter-spacing: 0.4em;
    color: var(--green);
    text-shadow: 0 0 10px var(--green);
    animation: statusBlink 3s ease-in-out infinite alternate;
  }
  @keyframes statusBlink {
    from { opacity:0.7; text-shadow: 0 0 6px var(--green); }
    to   { opacity:1;   text-shadow: 0 0 16px var(--green), 0 0 40px rgba(0,255,136,0.5); }
  }
  .cursor {
    display: inline-block;
    animation: blink 0.8s step-start infinite;
    color: var(--cyan);
  }
  @keyframes blink { 0%,49%{opacity:1} 50%,100%{opacity:0} }

  /* ── CORNER DECORATIONS ── */
  .deco-line {
    position: absolute;
    background: var(--cyan);
    opacity: 0.4;
    animation: decoGlow 2.5s ease-in-out infinite alternate;
  }
  .deco-h { height:1px; width:40px; }
  .deco-v { width:1px;  height:40px; }
  @keyframes decoGlow { from{opacity:0.2} to{opacity:0.8; box-shadow:0 0 6px var(--cyan)} }

  /* ── RESPONSIVE ── */
  @media(max-width:600px){
    .hud-grid { grid-template-columns:1fr; }
    .anix-name { font-size: 3.5rem; }
  }
</style>
</head>
<body>

<canvas id="matrix"></canvas>
<div class="stars" id="stars"></div>
<div class="grid-floor"></div>

<div class="wrapper">

  <!-- ARCADE FRAME -->
  <div class="arcade-frame">
    <span class="corner-tl"></span>
    <span class="corner-br"></span>

    <!-- DECO CORNERS -->
    <div class="deco-line deco-h" style="top:20px;left:40px;"></div>
    <div class="deco-line deco-v" style="top:0px; left:20px;"></div>
    <div class="deco-line deco-h" style="top:20px;right:40px;"></div>
    <div class="deco-line deco-v" style="top:0px; right:20px;"></div>
    <div class="deco-line deco-h" style="bottom:20px;left:40px;"></div>
    <div class="deco-line deco-v" style="bottom:0px;left:20px;"></div>
    <div class="deco-line deco-h" style="bottom:20px;right:40px;"></div>
    <div class="deco-line deco-v" style="bottom:0px;right:20px;"></div>

    <!-- WELCOME HEADER -->
    <div class="welcome-bar">▸▸▸&nbsp;&nbsp;SYSTEM INITIALIZED&nbsp;&nbsp;◂◂◂</div>
    <div class="welcome-line">WELCOME, USER</div>

    <!-- ANI_X HERO -->
    <div class="hero">
      <div class="anix-name" data-text="ANI_X">ANI_X</div>
      <div class="tagline">AI &nbsp;·&nbsp; SYSTEMS &nbsp;·&nbsp; WEB3 &nbsp;·&nbsp; FULL-STACK</div>
    </div>

    <div class="neon-divider"></div>

    <!-- TICKER -->
    <div class="ticker-wrap">
      <div class="ticker">
        <span>ANI_X // SYSTEM ONLINE</span>
        <span>⬡ NEURAL PIPELINES ACTIVE</span>
        <span>⬡ WEB3 NODE CONNECTED</span>
        <span>⬡ STACK LOADED</span>
        <span>⬡ CORE STABLE</span>
        <span>⬡ SIGNAL OPEN</span>
        <span>ANI_X // SYSTEM ONLINE</span>
        <span>⬡ NEURAL PIPELINES ACTIVE</span>
        <span>⬡ WEB3 NODE CONNECTED</span>
        <span>⬡ STACK LOADED</span>
        <span>⬡ CORE STABLE</span>
        <span>⬡ SIGNAL OPEN</span>
      </div>
    </div>

    <!-- PROJECTS -->
    <div class="section-head c-cyan" style="margin-top:24px;">
      <div class="line"></div>
      <span class="label">PROJECTS // NEON ARCHIVES</span>
      <div class="line"></div>
    </div>

    <div class="log-grid">
      <div class="log-entry">
        <div><span class="log-id">[01]</span><span class="log-title">EEG-BASED EMOTION &amp; COGNITIVE STATE DETECTION</span></div>
        <div class="log-desc">Neural signal acquisition → frequency decomposition (δ → γ) &nbsp;·&nbsp; ML-based affect &amp; stress inference &nbsp;·&nbsp; Hardware-aware EEG pipeline → real-time classification</div>
      </div>
      <div class="log-entry">
        <div><span class="log-id">[02]</span><span class="log-title">COGNITIVE MONITORING &amp; NEURAL DECODING &nbsp;<span style="color:var(--magenta);font-size:0.65rem;">UROP</span></span></div>
        <div class="log-desc">Research-tier neural decoding for safety &amp; well-being &nbsp;·&nbsp; Brain-signal interpretation &nbsp;·&nbsp; Inference under uncertainty</div>
      </div>
      <div class="log-entry">
        <div><span class="log-id">[03]</span><span class="log-title">OPTIMAL TRAFFIC CONGESTION DETECTION</span></div>
        <div class="log-desc">Merge Sort · K-Means · Closest Pair (Min-Max) &nbsp;·&nbsp; Flask architecture &nbsp;·&nbsp; Algorithmic congestion modeling</div>
      </div>
      <div class="log-entry">
        <div><span class="log-id">[04]</span><span class="log-title">UNIhub — CAMPUS SOCIAL PLATFORM</span></div>
        <div class="log-desc">Full-stack system &nbsp;·&nbsp; SQL schema design &amp; normalization &nbsp;·&nbsp; Platform logic &nbsp;·&nbsp; Architected for scale</div>
      </div>
      <div class="log-entry">
        <div><span class="log-id">[05]</span><span class="log-title">STOCK PREDICTION SYSTEM</span></div>
        <div class="log-desc">Signal-based market modeling &nbsp;·&nbsp; Probabilistic inference &nbsp;·&nbsp; Pattern recognition under noise</div>
      </div>
      <div class="log-entry">
        <div><span class="log-id">[06]</span><span class="log-title">INDUSTRIAL METAVERSE &nbsp;<span style="color:var(--orange);font-size:0.65rem;">PRYALABS</span></span></div>
        <div class="log-desc">Immersive virtual environments &nbsp;·&nbsp; Interaction layer design &nbsp;·&nbsp; Applied spatial computing</div>
      </div>
      <div class="log-entry">
        <div><span class="log-id">[07]</span><span class="log-title">CLOUD &amp; INFRASTRUCTURE &nbsp;<span style="color:var(--magenta);font-size:0.65rem;">AWS</span></span></div>
        <div class="log-desc">Cloud-native deployment &nbsp;·&nbsp; Managed services orchestration &nbsp;·&nbsp; Scalability-aware architecture</div>
      </div>
    </div>

    <div class="neon-divider" style="margin-top:28px;"></div>

    <!-- TECH STACK -->
    <div class="section-head c-magenta" style="margin-top:24px;">
      <div class="line"></div>
      <span class="label">STACK // CORE LOADOUT</span>
      <div class="line"></div>
    </div>

    <div class="stack-section">
      <div class="stack-label">// LANGUAGES</div>
      <div class="badge-row">
        <span class="badge b-cyan">Python</span>
        <span class="badge b-magenta">TypeScript</span>
        <span class="badge b-green">JavaScript</span>
        <span class="badge b-cyan">Solidity</span>
        <span class="badge b-red">C++</span>
        <span class="badge b-yellow">SQL</span>
      </div>
    </div>
    <div class="stack-section">
      <div class="stack-label">// FRAMEWORKS</div>
      <div class="badge-row">
        <span class="badge b-cyan">React</span>
        <span class="badge b-magenta">Next.js</span>
        <span class="badge b-green">Flask</span>
        <span class="badge b-orange">TensorFlow</span>
        <span class="badge b-red">PyTorch</span>
        <span class="badge b-yellow">Hardhat</span>
      </div>
    </div>
    <div class="stack-section">
      <div class="stack-label">// TOOLS &amp; INFRA</div>
      <div class="badge-row">
        <span class="badge b-orange">AWS</span>
        <span class="badge b-cyan">Docker</span>
        <span class="badge b-green">PostgreSQL</span>
        <span class="badge b-magenta">Linux</span>
        <span class="badge b-red">Git</span>
        <span class="badge b-yellow">Ethereum</span>
      </div>
    </div>

    <div class="neon-divider"></div>

    <!-- HUD STATS -->
    <div class="section-head c-green" style="margin-top:24px;">
      <div class="line"></div>
      <span class="label">GITHUB STATS // LIVE HUD</span>
      <div class="line"></div>
    </div>

    <div class="hud-grid">
      <div class="hud-panel">
        <div class="hud-label">// ACTIVITY METRICS</div>
        <div class="stat-bar"><div class="stat-name">COMMITS</div><div class="stat-track"><div class="stat-fill" style="--w:92%;--col:var(--cyan);--delay:0.3s"></div></div></div>
        <div class="stat-bar"><div class="stat-name">PULL REQUESTS</div><div class="stat-track"><div class="stat-fill" style="--w:78%;--col:var(--magenta);--delay:0.5s"></div></div></div>
        <div class="stat-bar"><div class="stat-name">ISSUES</div><div class="stat-track"><div class="stat-fill" style="--w:65%;--col:var(--green);--delay:0.7s"></div></div></div>
        <div class="stat-bar"><div class="stat-name">CODE REVIEWS</div><div class="stat-track"><div class="stat-fill" style="--w:83%;--col:var(--yellow);--delay:0.9s"></div></div></div>
      </div>
      <div class="hud-panel">
        <div class="hud-label">// LANGUAGE DISTRIBUTION</div>
        <div class="stat-bar"><div class="stat-name">PYTHON</div><div class="stat-track"><div class="stat-fill" style="--w:88%;--col:var(--cyan);--delay:0.4s"></div></div></div>
        <div class="stat-bar"><div class="stat-name">TYPESCRIPT</div><div class="stat-track"><div class="stat-fill" style="--w:72%;--col:var(--magenta);--delay:0.6s"></div></div></div>
        <div class="stat-bar"><div class="stat-name">SOLIDITY</div><div class="stat-track"><div class="stat-fill" style="--w:55%;--col:var(--green);--delay:0.8s"></div></div></div>
        <div class="stat-bar"><div class="stat-name">C++</div><div class="stat-track"><div class="stat-fill" style="--w:48%;--col:var(--red);--delay:1s"></div></div></div>
      </div>
      <div class="hud-panel hud-panel-full">
        <div class="hud-label">// STREAK &amp; CONTRIBUTION VELOCITY</div>
        <div style="display:flex;gap:32px;flex-wrap:wrap;margin-top:8px;">
          <div>
            <div style="font-size:0.6rem;color:rgba(0,245,255,0.4);letter-spacing:0.2em;">CURRENT STREAK</div>
            <div style="font-family:'VT323',monospace;font-size:2.2rem;color:var(--cyan);text-shadow:0 0 12px var(--cyan);line-height:1.1;animation:countUp 1.5s ease both 1.2s;">∞</div>
          </div>
          <div>
            <div style="font-size:0.6rem;color:rgba(0,245,255,0.4);letter-spacing:0.2em;">REPOSITORIES</div>
            <div style="font-family:'VT323',monospace;font-size:2.2rem;color:var(--magenta);text-shadow:0 0 12px var(--magenta);line-height:1.1;">40+</div>
          </div>
          <div>
            <div style="font-size:0.6rem;color:rgba(0,245,255,0.4);letter-spacing:0.2em;">DOMAINS</div>
            <div style="font-family:'VT323',monospace;font-size:2.2rem;color:var(--green);text-shadow:0 0 12px var(--green);line-height:1.1;">4</div>
          </div>
          <div style="flex:1;min-width:180px;">
            <div style="font-size:0.6rem;color:rgba(0,245,255,0.4);letter-spacing:0.2em;margin-bottom:8px;">LIVE STATS</div>
            <div style="font-size:0.65rem;color:rgba(0,245,255,0.4);">
              Replace <code style="color:var(--magenta);">ANI_X</code> below with your GitHub username to activate live stats cards:
            </div>
            <div style="font-size:0.6rem;color:rgba(0,245,255,0.3);margin-top:6px;line-height:1.8;">
              github-readme-stats.vercel.app/api?username=ANI_X<br/>
              github-readme-streak-stats.herokuapp.com/?user=ANI_X
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- ENERGY LINE -->
    <div class="energy-bar">
      ⚡ &nbsp; BUILT FOR SUSTAINED VELOCITY &nbsp; // &nbsp; HIGH PERFORMANCE. NO NOISE. &nbsp; ⚡
    </div>

    <!-- FOOTER -->
    <div class="footer">
      <div class="footer-status">
        ANI_X &nbsp;·&nbsp; ONLINE &nbsp;·&nbsp; SYSTEM STABLE &nbsp;·&nbsp; CORE ACTIVE
        <span class="cursor">█</span>
      </div>
      <div style="margin-top:10px;font-family:'VT323',monospace;font-size:0.75rem;letter-spacing:0.3em;color:rgba(0,245,255,0.25);">
        NEON LIT &nbsp;·&nbsp; SYSTEMS HOT &nbsp;·&nbsp; CODE IN MOTION
      </div>
    </div>

  </div><!-- /arcade-frame -->
</div><!-- /wrapper -->

<script>
// ── STARS ──
const starsEl = document.getElementById('stars');
for(let i=0;i<120;i++){
  const s = document.createElement('div');
  s.className = 'star';
  const size = Math.random()*2+0.5;
  s.style.cssText = `
    width:${size}px;height:${size}px;
    top:${Math.random()*70}%;left:${Math.random()*100}%;
    --d:${2+Math.random()*4}s;
    animation-delay:${Math.random()*4}s;
  `;
  starsEl.appendChild(s);
}

// ── MATRIX RAIN ──
const canvas = document.getElementById('matrix');
const ctx = canvas.getContext('2d');
function resize(){ canvas.width=window.innerWidth; canvas.height=window.innerHeight; }
resize();
window.addEventListener('resize', resize);

const cols = Math.floor(window.innerWidth / 20);
const drops = Array(cols).fill(1);
const chars = 'アイウエオカキクケコサシスセソABCDEF0123456789⬡◈▸△◻';

function drawMatrix(){
  ctx.fillStyle = 'rgba(0,0,5,0.05)';
  ctx.fillRect(0,0,canvas.width,canvas.height);
  ctx.font = '14px Share Tech Mono';
  drops.forEach((y,i)=>{
    const c = chars[Math.floor(Math.random()*chars.length)];
    const hue = Math.random() > 0.85 ? '#ff00cc' : '#00f5ff';
    ctx.fillStyle = hue;
    ctx.fillText(c, i*20, y*20);
    if(y*20 > canvas.height && Math.random()>0.975) drops[i]=0;
    drops[i]++;
  });
}
setInterval(drawMatrix, 60);

// ── GLITCH FLICKER ──
setInterval(()=>{
  if(Math.random()>0.94){
    document.querySelector('.arcade-frame').style.filter='brightness(1.3)';
    setTimeout(()=>{ document.querySelector('.arcade-frame').style.filter=''; }, 80);
  }
}, 1800);
</script>
</body>
</html>
