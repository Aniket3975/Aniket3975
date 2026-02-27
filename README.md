<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>ANI_X // SYSTEM ONLINE</title>

  <!-- FONTS -->
  <link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Orbitron:wght@400;700;900&display=swap" rel="stylesheet">

  <style>
    :root {
      --cyan: #00f5ff;
      --green: #00ff88;
      --bg: #000005;
      --panel: #04040f;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    html, body {
      height: 100%;
      background: var(--bg);
      color: var(--cyan);
      font-family: 'Share Tech Mono', monospace;
      overflow-x: hidden;
      cursor: crosshair;
    }

    /* ───────── SCANLINES ───────── */
    body::before {
      content: "";
      position: fixed;
      inset: 0;
      pointer-events: none;
      background: repeating-linear-gradient(
        0deg,
        transparent,
        transparent 2px,
        rgba(0,0,0,0.18) 2px,
        rgba(0,0,0,0.18) 4px
      );
      z-index: 999;
      animation: scanMove 8s linear infinite;
    }

    @keyframes scanMove {
      from { background-position: 0 0; }
      to   { background-position: 0 100px; }
    }

    /* ───────── MATRIX CANVAS ───────── */
    canvas {
      position: fixed;
      inset: 0;
      z-index: 0;
      opacity: 0.25;
    }

    /* ───────── MAIN PANEL ───────── */
    .container {
      position: relative;
      z-index: 2;
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
    }

    .panel {
      background: linear-gradient(180deg, rgba(0,245,255,0.05), rgba(0,0,0,0.9));
      border: 1px solid rgba(0,245,255,0.4);
      padding: 40px 50px;
      box-shadow: 0 0 40px rgba(0,245,255,0.25);
      max-width: 520px;
    }

    h1 {
      font-family: 'Orbitron', sans-serif;
      font-size: 2.2rem;
      letter-spacing: 4px;
      margin-bottom: 10px;
    }

    h2 {
      font-size: 0.9rem;
      color: var(--green);
      margin-bottom: 25px;
    }

    .status {
      font-size: 0.85rem;
      margin-bottom: 30px;
      color: #88ffff;
    }

    .status span {
      color: var(--green);
      animation: blink 1.4s infinite;
    }

    @keyframes blink {
      0%,100% { opacity: 1; }
      50% { opacity: 0; }
    }

    .links a {
      display: block;
      text-decoration: none;
      color: var(--cyan);
      border: 1px solid rgba(0,245,255,0.4);
      padding: 10px;
      margin: 10px 0;
      transition: all 0.25s ease;
    }

    .links a:hover {
      background: rgba(0,245,255,0.15);
      box-shadow: 0 0 12px rgba(0,245,255,0.7);
    }

    /* ───────── GRID FLOOR ───────── */
    .grid-floor {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      height: 320px;
      pointer-events: none;
      z-index: 1;
      background:
        linear-gradient(to bottom, transparent 0%, rgba(0,245,255,0.05) 100%),
        repeating-linear-gradient(90deg,
          rgba(0,245,255,0.08) 0 1px,
          transparent 1px 80px),
        repeating-linear-gradient(0deg,
          rgba(0,245,255,0.08) 0 1px,
          transparent 1px 60px);
      transform: perspective(400px) rotateX(65deg);
      transform-origin: bottom;
    }
  </style>
</head>

<body>

  <!-- MATRIX CANVAS -->
  <canvas id="matrix"></canvas>

  <!-- CONTENT -->
  <div class="container">
    <div class="panel">
      <h1>ANI_X</h1>
      <h2>NEURAL INTERFACE NODE</h2>

      <div class="status">
        SYSTEM STATUS: <span>ONLINE</span>
      </div>

      <div class="links">
        <a href="https://github.com/Aniket3975" target="_blank">GITHUB</a>
        <a href="#" target="_blank">PROJECTS</a>
        <a href="#" target="_blank">CONTACT</a>
      </div>
    </div>
  </div>

  <!-- GRID -->
  <div class="grid-floor"></div>

  <!-- MATRIX SCRIPT -->
  <script>
    const canvas = document.getElementById("matrix");
    const ctx = canvas.getContext("2d");

    function resize() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
    window.addEventListener("resize", resize);
    resize();

    const letters = "01アカサタナハマヤラワABCDEFGHIJKLMNOPQRSTUVWXYZ";
    const fontSize = 14;
    let columns = Math.floor(canvas.width / fontSize);
    let drops = Array(columns).fill(1);

    function drawMatrix() {
      ctx.fillStyle = "rgba(0,0,5,0.05)";
      ctx.fillRect(0, 0, canvas.width, canvas.height);

      ctx.fillStyle = "#00ff88";
      ctx.font = fontSize + "px monospace";

      for (let i = 0; i < drops.length; i++) {
        const text = letters[Math.floor(Math.random() * letters.length)];
        ctx.fillText(text, i * fontSize, drops[i] * fontSize);

        if (drops[i] * fontSize > canvas.height && Math.random() > 0.975) {
          drops[i] = 0;
        }
        drops[i]++;
      }
    }

    setInterval(drawMatrix, 40);
  </script>

</body>
</html>
