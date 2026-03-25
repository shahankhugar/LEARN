<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SHASHANK // LIFE OS</title>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@300;400;500;700&family=Share+Tech+Mono&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #0a0a0a;
    --bg2: #111111;
    --bg3: #161616;
    --green: #00ff88;
    --green-dim: #00cc6a;
    --green-dark: #003320;
    --amber: #ffaa00;
    --red: #ff4444;
    --blue: #4488ff;
    --text: #c8ffd4;
    --text-dim: #5a8c6a;
    --border: #1a3a28;
    --border-bright: #00ff8840;
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    line-height: 1.6;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* scanline overlay */
  body::before {
    content: '';
    position: fixed;
    top: 0; left: 0; right: 0; bottom: 0;
    background: repeating-linear-gradient(
      0deg,
      transparent,
      transparent 2px,
      rgba(0,0,0,0.08) 2px,
      rgba(0,0,0,0.08) 4px
    );
    pointer-events: none;
    z-index: 1000;
  }

  /* cursor blink */
  @keyframes blink { 0%,100%{opacity:1} 50%{opacity:0} }
  @keyframes fadeUp { from{opacity:0;transform:translateY(12px)} to{opacity:1;transform:translateY(0)} }
  @keyframes glow { 0%,100%{text-shadow:0 0 8px #00ff8880} 50%{text-shadow:0 0 20px #00ff88,0 0 40px #00ff8840} }
  @keyframes scanIn { from{opacity:0;letter-spacing:0.5em} to{opacity:1;letter-spacing:normal} }
  @keyframes pulse { 0%,100%{box-shadow:0 0 0 0 #ff444440} 50%{box-shadow:0 0 0 6px transparent} }

  .header {
    padding: 24px 32px 0;
    animation: fadeUp 0.6s ease;
  }

  .header-top {
    display: flex;
    align-items: baseline;
    justify-content: space-between;
    flex-wrap: wrap;
    gap: 8px;
    border-bottom: 1px solid var(--border);
    padding-bottom: 16px;
    margin-bottom: 16px;
  }

  .logo {
    font-family: 'Share Tech Mono', monospace;
    font-size: 22px;
    color: var(--green);
    animation: glow 3s ease-in-out infinite;
    letter-spacing: 0.05em;
  }

  .logo span { color: var(--text-dim); font-size: 14px; }

  .cursor {
    display: inline-block;
    width: 10px; height: 14px;
    background: var(--green);
    animation: blink 1s step-end infinite;
    vertical-align: middle;
    margin-left: 2px;
  }

  .sys-info {
    font-size: 11px;
    color: var(--text-dim);
    text-align: right;
  }

  .sys-info div { margin-bottom: 2px; }

  .alert-banner {
    margin: 0 32px 12px;
    padding: 10px 16px;
    border: 1px solid var(--red);
    border-left: 3px solid var(--red);
    background: #ff444408;
    font-size: 12px;
    color: var(--red);
    animation: pulse 2s infinite, fadeUp 0.6s ease;
  }

  .nav {
    display: flex;
    gap: 0;
    padding: 0 32px;
    border-bottom: 1px solid var(--border);
    overflow-x: auto;
    scrollbar-width: none;
  }
  .nav::-webkit-scrollbar { display: none; }

  .nav-btn {
    background: none;
    border: none;
    border-bottom: 2px solid transparent;
    color: var(--text-dim);
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    padding: 10px 16px;
    cursor: pointer;
    white-space: nowrap;
    letter-spacing: 0.05em;
    transition: all 0.15s;
  }

  .nav-btn:hover { color: var(--green); }
  .nav-btn.active {
    color: var(--green);
    border-bottom-color: var(--green);
    text-shadow: 0 0 10px #00ff8860;
  }

  .nav-btn::before { content: '> '; }

  .main { padding: 24px 32px; animation: fadeUp 0.4s ease; }

  .section { display: none; }
  .section.active { display: block; }

  .section-title {
    font-size: 11px;
    color: var(--text-dim);
    letter-spacing: 0.15em;
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .section-title::before { content: '//'; color: var(--green); }
  .section-title::after { content: ''; flex: 1; height: 1px; background: var(--border); }

  /* cards */
  .card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-left: 2px solid var(--green);
    padding: 16px 20px;
    margin-bottom: 12px;
  }

  .card-amber { border-left-color: var(--amber); }
  .card-red { border-left-color: var(--red); }
  .card-blue { border-left-color: var(--blue); }

  .card-label {
    font-size: 10px;
    color: var(--text-dim);
    letter-spacing: 0.12em;
    margin-bottom: 6px;
  }

  .card-value {
    font-size: 28px;
    font-weight: 700;
    color: var(--green);
    line-height: 1;
  }

  .card-value.amber { color: var(--amber); }
  .card-value.red { color: var(--red); }

  .grid2 { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .grid3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; }
  .grid4 { display: grid; grid-template-columns: 1fr 1fr 1fr 1fr; gap: 10px; }

  @media (max-width: 600px) {
    .grid4 { grid-template-columns: 1fr 1fr; }
    .grid3 { grid-template-columns: 1fr 1fr; }
    .main { padding: 16px; }
    .header { padding: 16px 16px 0; }
    .nav { padding: 0 16px; }
    .alert-banner { margin: 0 16px 12px; }
  }

  /* schedule */
  .schedule-row {
    display: flex;
    gap: 16px;
    padding: 10px 0;
    border-bottom: 1px solid var(--border);
    align-items: flex-start;
  }
  .schedule-row:last-child { border-bottom: none; }

  .sch-time {
    font-size: 11px;
    color: var(--green);
    min-width: 72px;
    margin-top: 1px;
    flex-shrink: 0;
  }

  .sch-content { flex: 1; }
  .sch-main { font-size: 13px; color: var(--text); }
  .sch-sub { font-size: 11px; color: var(--text-dim); margin-top: 2px; }

  .tag {
    display: inline-block;
    font-size: 9px;
    padding: 1px 6px;
    letter-spacing: 0.08em;
    margin-left: 6px;
    vertical-align: middle;
  }
  .tag-red { background: #ff444420; color: var(--red); border: 1px solid #ff444440; }
  .tag-green { background: #00ff8820; color: var(--green); border: 1px solid #00ff8840; }
  .tag-amber { background: #ffaa0020; color: var(--amber); border: 1px solid #ffaa0040; }

  /* habits */
  .habit-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 11px 0;
    border-bottom: 1px solid var(--border);
    cursor: pointer;
    transition: background 0.1s;
  }
  .habit-row:last-child { border-bottom: none; }
  .habit-row:hover { background: var(--bg3); margin: 0 -20px; padding: 11px 20px; }

  .habit-label { font-size: 13px; }
  .habit-sub { font-size: 11px; color: var(--text-dim); margin-top: 2px; }

  .toggle-wrap { display: flex; align-items: center; gap: 10px; }

  .toggle {
    width: 40px; height: 22px;
    border: 1px solid var(--border);
    border-radius: 11px;
    background: var(--bg3);
    position: relative;
    cursor: pointer;
    transition: all 0.2s;
    flex-shrink: 0;
  }
  .toggle::after {
    content: '';
    position: absolute;
    width: 16px; height: 16px;
    border-radius: 50%;
    background: var(--text-dim);
    top: 2px; left: 2px;
    transition: all 0.2s;
  }
  .toggle.on {
    background: var(--green-dark);
    border-color: var(--green);
    box-shadow: 0 0 8px #00ff8840;
  }
  .toggle.on::after { left: 20px; background: var(--green); }

  .streak { font-size: 11px; color: var(--text-dim); }
  .streak.active { color: var(--green); }

  /* IA prep */
  .subject-row {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 12px 0;
    border-bottom: 1px solid var(--border);
  }
  .subject-row:last-child { border-bottom: none; }

  .sub-info { flex: 1; }
  .sub-name { font-size: 13px; color: var(--text); }
  .sub-meta { font-size: 11px; color: var(--text-dim); margin-top: 2px; }

  .dots-wrap { display: flex; gap: 5px; }
  .dot {
    width: 20px; height: 20px;
    border: 1px solid var(--border);
    border-radius: 50%;
    cursor: pointer;
    transition: all 0.15s;
    background: var(--bg3);
  }
  .dot:hover { border-color: var(--green); }
  .dot.on {
    background: var(--green);
    border-color: var(--green);
    box-shadow: 0 0 6px #00ff8860;
  }

  /* deadlines */
  .deadline-row {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 12px 0;
    border-bottom: 1px solid var(--border);
  }
  .deadline-row:last-child { border-bottom: none; }
  .deadline-row.done { opacity: 0.4; }
  .deadline-row.done .dl-name { text-decoration: line-through; }

  .dl-badge {
    font-size: 9px;
    padding: 3px 8px;
    letter-spacing: 0.08em;
    flex-shrink: 0;
    min-width: 58px;
    text-align: center;
  }

  .dl-info { flex: 1; }
  .dl-name { font-size: 13px; }
  .dl-sub { font-size: 11px; color: var(--text-dim); margin-top: 2px; }

  .check-btn {
    width: 24px; height: 24px;
    border: 1px solid var(--border);
    background: none;
    color: var(--text-dim);
    cursor: pointer;
    font-family: 'JetBrains Mono', monospace;
    font-size: 14px;
    display: flex; align-items: center; justify-content: center;
    flex-shrink: 0;
    transition: all 0.15s;
  }
  .check-btn:hover { border-color: var(--green); color: var(--green); }
  .check-btn.done { background: var(--green-dark); border-color: var(--green); color: var(--green); }

  /* money */
  input[type="number"], input[type="text"] {
    background: var(--bg3);
    border: 1px solid var(--border);
    color: var(--text);
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    padding: 6px 10px;
    outline: none;
    transition: border-color 0.15s;
  }
  input[type="number"]:focus, input[type="text"]:focus {
    border-color: var(--green);
    box-shadow: 0 0 8px #00ff8820;
  }

  .btn {
    background: var(--green-dark);
    border: 1px solid var(--green);
    color: var(--green);
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    padding: 6px 14px;
    cursor: pointer;
    letter-spacing: 0.05em;
    transition: all 0.15s;
  }
  .btn:hover { background: var(--green); color: var(--bg); }

  .btn-red {
    background: #ff444410;
    border-color: var(--red);
    color: var(--red);
  }
  .btn-red:hover { background: var(--red); color: var(--bg); }

  .money-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 7px 0;
    border-bottom: 1px solid var(--border);
    font-size: 12px;
  }
  .money-row:last-child { border-bottom: none; }
  .money-amt { color: var(--red); }

  /* screen time */
  .bar-wrap {
    margin-bottom: 16px;
  }
  .bar-header {
    display: flex;
    justify-content: space-between;
    font-size: 12px;
    margin-bottom: 6px;
  }
  .bar-track {
    height: 6px;
    background: var(--bg3);
    border: 1px solid var(--border);
    overflow: hidden;
  }
  .bar-fill {
    height: 100%;
    transition: width 0.4s ease;
  }

  /* about */
  .about-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
    margin-bottom: 16px;
  }
  @media (max-width: 500px) { .about-grid { grid-template-columns: 1fr; } }

  .about-item {
    background: var(--bg2);
    border: 1px solid var(--border);
    padding: 12px 16px;
  }
  .about-key { font-size: 10px; color: var(--text-dim); letter-spacing: 0.1em; margin-bottom: 4px; }
  .about-val { font-size: 13px; color: var(--text); }

  .prompt-line {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 6px;
  }
  .prompt-line::before {
    content: '$';
    color: var(--green);
  }

  .status-bar {
    position: fixed;
    bottom: 0; left: 0; right: 0;
    background: var(--bg2);
    border-top: 1px solid var(--border);
    padding: 6px 32px;
    display: flex;
    justify-content: space-between;
    font-size: 10px;
    color: var(--text-dim);
    z-index: 100;
    letter-spacing: 0.05em;
  }

  .status-green { color: var(--green); }
  .status-amber { color: var(--amber); }
  .status-red { color: var(--red); }

  /* habit score bar */
  .score-display {
    margin-top: 16px;
    padding: 12px 16px;
    border: 1px solid var(--border);
    background: var(--bg2);
    display: flex;
    align-items: center;
    gap: 16px;
  }
  .score-num {
    font-size: 32px;
    font-weight: 700;
    color: var(--green);
    min-width: 60px;
  }
  .score-msg { font-size: 12px; color: var(--text-dim); }

  /* countdown big */
  .countdown-big {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin-bottom: 20px;
  }
  .cd-box {
    background: var(--bg2);
    border: 1px solid var(--border);
    padding: 16px;
    text-align: center;
  }
  .cd-num {
    font-size: 36px;
    font-weight: 700;
    color: var(--green);
    display: block;
    line-height: 1;
  }
  .cd-num.urgent { color: var(--red); animation: glow 1.5s ease-in-out infinite; }
  .cd-label { font-size: 10px; color: var(--text-dim); margin-top: 6px; letter-spacing: 0.1em; }

  .pb { padding-bottom: 60px; }

  input[type="number"]::-webkit-inner-spin-button { opacity: 0.5; }
</style>
</head>
<body>

<header class="header">
  <div class="header-top">
    <div class="logo">
      SHASHANK <span>// LIFE OS v1.0</span><span class="cursor"></span>
    </div>
    <div class="sys-info">
      <div id="clock">--:-- --</div>
      <div id="dateline">-- --- 2026</div>
      <div>UVCE BENGALURU · SEM 2</div>
    </div>
  </div>
</header>

<div class="alert-banner" id="alert-top">
  [ALERT] 3 DEADLINES IN 7 DAYS — GEN AI ACADEMY + IIMA APP (MAR 31) · IA EXAMS APR 6–8
</div>

<nav class="nav">
  <button class="nav-btn active" onclick="switchTab('today', this)">SCHEDULE</button>
  <button class="nav-btn" onclick="switchTab('ia', this)">IA PREP</button>
  <button class="nav-btn" onclick="switchTab('deadlines', this)">DEADLINES</button>
  <button class="nav-btn" onclick="switchTab('habits', this)">HABITS</button>
  <button class="nav-btn" onclick="switchTab('money', this)">MONEY</button>
  <button class="nav-btn" onclick="switchTab('screen', this)">SCREEN TIME</button>
  <button class="nav-btn" onclick="switchTab('about', this)">ABOUT</button>
</nav>

<main class="main pb">

  <!-- TODAY -->
  <div id="tab-today" class="section active">
    <div class="section-title">DAILY SCHEDULE</div>

    <div class="grid4" style="margin-bottom:16px;">
      <div class="card">
        <div class="card-label">WAKE TARGET</div>
        <div class="card-value" style="font-size:20px;">07:00</div>
      </div>
      <div class="card card-amber">
        <div class="card-label">IA IN</div>
        <div class="card-value amber" id="ia-days-mini">--</div>
      </div>
      <div class="card">
        <div class="card-label">CGPA (SEM 1)</div>
        <div class="card-value">8.68</div>
      </div>
      <div class="card card-amber">
        <div class="card-label">ATTENDANCE</div>
        <div class="card-value amber">~78%</div>
      </div>
    </div>

    <div class="card">
      <div class="schedule-row">
        <div class="sch-time">07:00 AM</div>
        <div class="sch-content">
          <div class="sch-main">WAKE UP <span class="tag tag-red">TARGET</span></div>
          <div class="sch-sub">Bath + music only — no Instagram in bathroom</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">07:50 AM</div>
        <div class="sch-content">
          <div class="sch-main">MESS BREAKFAST</div>
          <div class="sch-sub" id="breakfast-today">Loading...</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">08:00 AM</div>
        <div class="sch-content">
          <div class="sch-main">COLLEGE — MORNING SESSION</div>
          <div class="sch-sub" id="morning-class">Loading...</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">10:30 AM</div>
        <div class="sch-content">
          <div class="sch-main">BREAK</div>
          <div class="sch-sub">10 min — stretch, no scroll</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">10:50 AM</div>
        <div class="sch-content">
          <div class="sch-main">MID SESSION</div>
          <div class="sch-sub" id="mid-class">Loading...</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">01:20 PM</div>
        <div class="sch-content">
          <div class="sch-main">MESS LUNCH</div>
          <div class="sch-sub" id="lunch-today">Loading...</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">02:30 PM</div>
        <div class="sch-content">
          <div class="sch-main">AFTERNOON SESSION</div>
          <div class="sch-sub" id="afternoon-class">Loading...</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">05:00 PM</div>
        <div class="sch-content">
          <div class="sch-main">STUDY BLOCK 1 <span class="tag tag-red">URGENT</span></div>
          <div class="sch-sub">2 hrs minimum — pick 1 IA subject. Phone off.</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">07:00 PM</div>
        <div class="sch-content">
          <div class="sch-main">WORKOUT</div>
          <div class="sch-sub">Pushups + squats + dips — 30 min max</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">07:30 PM</div>
        <div class="sch-content">
          <div class="sch-main">PROTEIN SHAKE</div>
          <div class="sch-sub">Nutrela green — 20g if not taken morning</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">08:00 PM</div>
        <div class="sch-content">
          <div class="sch-main">MESS DINNER</div>
          <div class="sch-sub" id="dinner-today">Loading...</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">08:30 PM</div>
        <div class="sch-content">
          <div class="sch-main">PARENTS CALL</div>
          <div class="sch-sub">15–30 min after dinner</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">09:00 PM</div>
        <div class="sch-content">
          <div class="sch-main">STUDY BLOCK 2</div>
          <div class="sch-sub">1.5 hrs — second IA subject</div>
        </div>
      </div>
      <div class="schedule-row">
        <div class="sch-time">10:30 PM</div>
        <div class="sch-content">
          <div class="sch-main">WIND DOWN → SLEEP 11PM <span class="tag tag-amber">MAX</span></div>
          <div class="sch-sub">Call ≤ 30 min. No doom scroll.</div>
        </div>
      </div>
    </div>
  </div>

  <!-- IA PREP -->
  <div id="tab-ia" class="section">
    <div class="section-title">IA EXAM PREP TRACKER</div>

    <div class="countdown-big">
      <div class="cd-box">
        <span class="cd-num" id="cd6"></span>
        <div class="cd-label">DAYS → APR 6<br>MATHS + ENGLISH</div>
      </div>
      <div class="cd-box">
        <span class="cd-num" id="cd7"></span>
        <div class="cd-label">DAYS → APR 7<br>PHYSICS + BIO</div>
      </div>
      <div class="cd-box">
        <span class="cd-num" id="cd8"></span>
        <div class="cd-label">DAYS → APR 8<br>EM + AI FOUND.</div>
      </div>
    </div>

    <div class="card" style="margin-bottom:16px;">
      <div style="font-size:10px;color:var(--text-dim);margin-bottom:12px;letter-spacing:0.08em;">TAP DOTS TO TRACK CHAPTER PROGRESS (0 OF 5)</div>

      <div class="subject-row">
        <div class="sub-info">
          <div class="sub-name">Linear Algebra &amp; Numerical Analysis</div>
          <div class="sub-meta">APR 6 · Matrices, Eigenvalues, Numerical methods</div>
        </div>
        <div class="dots-wrap" id="d-maths"></div>
      </div>
      <div class="subject-row">
        <div class="sub-info">
          <div class="sub-name">Professional English</div>
          <div class="sub-meta">APR 6 · Writing, comprehension, grammar</div>
        </div>
        <div class="dots-wrap" id="d-english"></div>
      </div>
      <div class="subject-row">
        <div class="sub-info">
          <div class="sub-name">Classical &amp; Quantum Physics</div>
          <div class="sub-meta">APR 7 · Wave mechanics, QM basics</div>
        </div>
        <div class="dots-wrap" id="d-physics"></div>
      </div>
      <div class="subject-row">
        <div class="sub-info">
          <div class="sub-name">Biology for Engineers</div>
          <div class="sub-meta">APR 7 · Cell biology, biotech basics</div>
        </div>
        <div class="dots-wrap" id="d-bio"></div>
      </div>
      <div class="subject-row">
        <div class="sub-info">
          <div class="sub-name">Engineering Mechanics</div>
          <div class="sub-meta">APR 8 · Statics, dynamics, forces</div>
        </div>
        <div class="dots-wrap" id="d-em"></div>
      </div>
      <div class="subject-row">
        <div class="sub-info">
          <div class="sub-name">AI Foundation for Engineers</div>
          <div class="sub-meta">APR 8 · ML basics, AI concepts</div>
        </div>
        <div class="dots-wrap" id="d-ai"></div>
      </div>
    </div>

    <div class="alert-banner" style="border-color:var(--amber);color:var(--amber);background:#ffaa0008;">
      [NOTE] YOUR 19TH BIRTHDAY IS APR 3 — RIGHT BEFORE IAs. PLAN ACCORDINGLY.
    </div>
  </div>

  <!-- DEADLINES -->
  <div id="tab-deadlines" class="section">
    <div class="section-title">ACTIVE DEADLINES</div>

    <div class="grid2" style="margin-bottom:16px;">
      <div class="card card-red">
        <div class="card-label">DAYS TO MAR 31</div>
        <div class="card-value red" id="days-mar31">--</div>
      </div>
      <div class="card card-amber">
        <div class="card-label">OPEN ITEMS</div>
        <div class="card-value amber" id="open-count">--</div>
      </div>
    </div>

    <div class="card" id="deadline-list">
      <!-- rendered by JS -->
    </div>
  </div>

  <!-- HABITS -->
  <div id="tab-habits" class="section">
    <div class="section-title">DAILY HABITS</div>

    <div class="card" id="habit-list">
      <!-- rendered by JS -->
    </div>

    <div class="score-display">
      <div class="score-num" id="habit-score-num">0/8</div>
      <div>
        <div class="bar-track" style="width:200px;margin-bottom:6px;">
          <div class="bar-fill" id="habit-bar" style="background:var(--green);width:0%"></div>
        </div>
        <div class="score-msg" id="habit-score-msg">start checking habits off</div>
      </div>
    </div>

    <div style="font-size:10px;color:var(--text-dim);margin-top:12px;letter-spacing:0.08em;">
      DATA RESETS DAILY AT MIDNIGHT. STREAKS SAVED LOCALLY.
    </div>
  </div>

  <!-- MONEY -->
  <div id="tab-money" class="section">
    <div class="section-title">MONEY TRACKER</div>

    <div class="grid3" style="margin-bottom:16px;">
      <div class="card">
        <div class="card-label">WEEKLY BUDGET</div>
        <div class="card-value">₹500</div>
      </div>
      <div class="card card-red">
        <div class="card-label">SPENT</div>
        <div class="card-value red" id="spent-display">₹0</div>
      </div>
      <div class="card">
        <div class="card-label">REMAINING</div>
        <div class="card-value" id="remaining-display">₹500</div>
      </div>
    </div>

    <div class="card" style="margin-bottom:12px;">
      <div style="display:flex;gap:8px;flex-wrap:wrap;align-items:center;margin-bottom:14px;">
        <input type="number" id="amt-input" placeholder="₹ amount" style="width:110px;" min="0" />
        <input type="text" id="label-input" placeholder="what for?" style="flex:1;min-width:120px;" />
        <button class="btn" onclick="addSpend()">+ LOG</button>
        <button class="btn btn-red" onclick="resetMoney()">RESET WEEK</button>
      </div>
      <div id="spend-log"></div>
      <div id="empty-log" style="color:var(--text-dim);font-size:12px;">no expenses logged yet this week</div>
    </div>

    <div class="alert-banner" style="border-color:var(--amber);color:var(--amber);background:#ffaa0008;">
      [TIP] LOG EVERY RUPEE THIS WEEK. EVEN CHAI. AWARENESS IS THE FIRST STEP.
    </div>
  </div>

  <!-- SCREEN TIME -->
  <div id="tab-screen" class="section">
    <div class="section-title">SCREEN TIME</div>

    <div class="grid2" style="margin-bottom:16px;">
      <div class="card card-red">
        <div class="card-label">LAST WEEK TOTAL</div>
        <div class="card-value red">16 HRS</div>
      </div>
      <div class="card">
        <div class="card-label">TARGET TOTAL</div>
        <div class="card-value">9 HRS</div>
      </div>
    </div>

    <div class="card" style="margin-bottom:12px;">
      <div class="bar-wrap">
        <div class="bar-header">
          <span>INSTAGRAM</span>
          <span id="ig-txt" style="color:var(--red)">8 hrs / target 4 hrs</span>
        </div>
        <div class="bar-track">
          <div class="bar-fill" id="ig-fill" style="width:57%;background:var(--red);"></div>
        </div>
      </div>
      <div class="bar-wrap">
        <div class="bar-header">
          <span>WHATSAPP</span>
          <span id="wa-txt" style="color:var(--red)">8 hrs / target 5 hrs</span>
        </div>
        <div class="bar-track">
          <div class="bar-fill" id="wa-fill" style="width:57%;background:var(--red);"></div>
        </div>
      </div>
      <div class="bar-wrap">
        <div class="bar-header">
          <span>YOUTUBE</span>
          <span id="yt-txt" style="color:var(--text-dim)">0 hrs / target 2 hrs</span>
        </div>
        <div class="bar-track">
          <div class="bar-fill" id="yt-fill" style="width:0%;background:var(--green);"></div>
        </div>
      </div>
    </div>

    <div class="card">
      <div style="font-size:10px;color:var(--text-dim);margin-bottom:10px;letter-spacing:0.08em;">UPDATE THIS WEEK'S HOURS</div>
      <div style="display:flex;flex-wrap:wrap;gap:12px;align-items:center;">
        <span style="font-size:12px;color:var(--text-dim);">INSTA</span>
        <input type="number" id="ig-in" value="8" min="0" max="50" style="width:60px;" oninput="updateScreen()">
        <span style="font-size:12px;color:var(--text-dim);">WHATSAPP</span>
        <input type="number" id="wa-in" value="8" min="0" max="50" style="width:60px;" oninput="updateScreen()">
        <span style="font-size:12px;color:var(--text-dim);">YOUTUBE</span>
        <input type="number" id="yt-in" value="0" min="0" max="50" style="width:60px;" oninput="updateScreen()">
      </div>
    </div>

    <div class="alert-banner" id="screen-verdict" style="margin:12px 0 0;border-color:var(--red);color:var(--red);background:#ff444408;">
      [VERDICT] LOADING...
    </div>
  </div>

  <!-- ABOUT -->
  <div id="tab-about" class="section">
    <div class="section-title">SYSTEM INFO</div>

    <div class="about-grid">
      <div class="about-item"><div class="about-key">NAME</div><div class="about-val">Shashank Hugar</div></div>
      <div class="about-item"><div class="about-key">DOB</div><div class="about-val">03 April 2007</div></div>
      <div class="about-item"><div class="about-key">INSTITUTE</div><div class="about-val">UVCE Bengaluru</div></div>
      <div class="about-item"><div class="about-key">PROGRAM</div><div class="about-val">B.Tech Mechanical Eng.</div></div>
      <div class="about-item"><div class="about-key">BATCH</div><div class="about-val">2023–2027</div></div>
      <div class="about-item"><div class="about-key">ROLE</div><div class="about-val">Class Representative</div></div>
      <div class="about-item"><div class="about-key">CGPA</div><div class="about-val">8.68 (Sem 1)</div></div>
      <div class="about-item"><div class="about-key">LOCATION</div><div class="about-val">Hostel, Bengaluru</div></div>
    </div>

    <div class="section-title" style="margin-top:16px;">ACTIVE PROJECTS</div>

    <div class="about-grid">
      <div class="about-item card-blue" style="border-left:2px solid var(--blue);">
        <div class="about-key">STARTUP</div>
        <div class="about-val">AeroScan AI — Drone Inspection</div>
      </div>
      <div class="about-item" style="border-left:2px solid var(--amber);">
        <div class="about-key">COMPETITION</div>
        <div class="about-val">FKCCI Inno Manthan 2026</div>
      </div>
      <div class="about-item">
        <div class="about-key">SIDE PROJECT</div>
        <div class="about-val">Delver Mythic — 7DOF Robotic Arm</div>
      </div>
      <div class="about-item">
        <div class="about-key">SOCIAL MEDIA</div>
        <div class="about-val">Soujanya Beauty Salon, Dharwad</div>
      </div>
      <div class="about-item">
        <div class="about-key">FILMMAKING</div>
        <div class="about-val">Short film — mystery/dark humor</div>
      </div>
      <div class="about-item">
        <div class="about-key">GITHUB</div>
        <div class="about-val" style="color:var(--green)">shahankhugar.github.io</div>
      </div>
    </div>

    <div class="section-title" style="margin-top:16px;">MISSION</div>
    <div class="card">
      <div class="prompt-line">Be someone who gets hired instantly and can't be replaced.</div>
      <div class="prompt-line">Build things in robotics + AI that actually work.</div>
      <div class="prompt-line">Stop pretending to work. Actually work.</div>
    </div>
  </div>

</main>

<div class="status-bar">
  <div id="status-left">SYSTEM ONLINE</div>
  <div id="status-right" class="status-green">ALL SYSTEMS GO</div>
</div>

<script>
// ── DATA ──────────────────────────────────────────────
const MESS = {
  morning: ['Tea + Puliyogarre, Chatni','Tea + Idly Sambar, Chatni','Tea + Kesari Bath, Chaw-chaw Bath, BisiBele Bath, Kaaraboondi','Tea + Poori, Kabula Channa Masala Kurma','Tea + Bread Palav, Mosaru Bajji','Tea + Chapathi Alu Sabji','Tea + Dose, Chatni, Potato Palya'],
  lunch: ['Rice, Sambar, Rasam, Curd/Pickle/Round Papad','Rice, Veg Sambar, Rasam, Curd/Pickle, Kabul Kadale','Rice, Veg Sambar, Rasam, Hesaru Kallu Palya Curd/Pickle','Rice, Veg Sambar, Rasam, Kadale Kaalu Palya Curd/Pickle','Rice, Mix Veg Sambar, Rasam, Papad, Curd/Pickle','Rice, Sambar, Molake Kaalu Palya, Rasam, Curd/Pickle','Chicken Biriyani, Kabab, Sweets, Mushroom Rice, Ghee Rice'],
  dinner: ['Coffee, Chapathi, Alasande Kalu, Dal, Rice, Egg, Banana, Pickle','Coffee, Chapathi, Egg Bhurji, Veg Carrot Palya, Rice, Veg Sambar, Banana, Pickle','Coffee, Chapathi, Chicken, Dal, Paneer Sweets, Dry Gulab Jamun/Special Sweet Rice, Banana, Pickle','Coffee, Chapathi, Sabji Rice, Veg Samber, Egg, Banana, Pickle','Coffee/Tea, Chapathi, Kadalekalu & Tomato Palya, Rice, Veg Sambar, Banana, Egg, Pickle','Coffee, Chapathi, Brinjal Palya, Dal, Rice, Banana, Egg','Coffee, Egg Rice, Lemon Rice/Tomato Bath, Chatni, Pickle']
};

const TIMETABLE = {
  1: { // Mon
    morning: 'Engineering Mechanics (8:00–10:30)',
    mid: 'Classical & Quantum Physics (10:50–12:30) · AI Foundation for Engineers (12:30–1:20)',
    afternoon: 'Engineering Physics LAB B1 (2:30–5:30)'
  },
  2: { // Tue
    morning: 'Classical & Quantum Physics (8:50–9:40) · Engineering Mechanics (9:40–10:30)',
    mid: 'Engineering Physics LAB B3 (10:50–12:30)',
    afternoon: 'CAD LAB B3 (2:30–5:30)'
  },
  3: { // Wed
    morning: 'Professional English (8:00–8:50) · AI Foundation for Engineers (8:50–10:30)',
    mid: 'Engineering Physics LAB B2 (10:50–12:30)',
    afternoon: '—'
  },
  4: { // Thu
    morning: 'Professional English (8:00–8:50) · Linear Algebra & Numerical Analysis (8:50–10:30)',
    mid: 'Biology for Engineers (10:50–11:40) · CAD (11:40–12:30)',
    afternoon: 'CAD LAB B1 (2:30–5:30)'
  },
  5: { // Fri
    morning: 'Linear Algebra & Numerical Analysis (8:50–10:30)',
    mid: 'Classical & Quantum Physics (10:50–12:30)',
    afternoon: 'CAD LAB B2 (2:30–5:30)'
  },
  6: { morning: '— no classes —', mid: '—', afternoon: '—' },
  0: { morning: '— no classes —', mid: '—', afternoon: '—' }
};

const DEADLINES = [
  { id:'genai', label:'Gen AI Academy Certification', sub:'Project submission + MCQ (60% min) · Gemini/MCP/AlloyDB', date:'Mar 31', type:'red' },
  { id:'iima', label:'IIMA AI Summer Residency', sub:'Fully sponsored · Apply on Unstop before deadline', date:'Mar 31', type:'red' },
  { id:'ia', label:'IA Exams — 6, 7, 8 April', sub:'6: Maths+English · 7: Physics+Bio · 8: EM+AI Foundation', date:'Apr 6', type:'amber' },
  { id:'manthan', label:'Inno Manthan Stage 2', sub:'Business plan competition · Meeting with Siri pending', date:'TBD', type:'blue' },
  { id:'salon', label:'Soujanya Salon — Social Media', sub:'Weekly content · Dharwad · With Navaneet', date:'Weekly', type:'green' },
];

const HABITS = [
  { id:'wake', label:'Wake up by 7:00 AM', sub:'Current: 6:50–8:30 random' },
  { id:'no_insta_bath', label:'No Instagram in bathroom', sub:'Music only. Big one.' },
  { id:'protein', label:'20g protein shake (morning)', sub:'Nutrela green' },
  { id:'pushups', label:'Pushups + squats + dips', sub:'30 min — building towards bulk' },
  { id:'study', label:'Study block done (2+ hrs)', sub:'IA is close. No excuses.' },
  { id:'laundry', label:'Laundry if due', sub:'Every 3 days — manual wash' },
  { id:'call_limit', label:'Call ≤ 30 min', sub:'Was 2–3 hrs. You reduced it. Keep it.' },
  { id:'sleep', label:'Sleep by 11:00 PM', sub:'Current: often 12–2 AM' },
];

// ── STATE ──────────────────────────────────────────────
function loadState() {
  try { return JSON.parse(localStorage.getItem('shashank_os') || '{}'); } catch(e) { return {}; }
}
function saveState(state) {
  localStorage.setItem('shashank_os', JSON.stringify(state));
}

let state = loadState();
if (!state.habits) state.habits = {};
if (!state.dots) state.dots = {};
if (!state.deadlines) state.deadlines = {};
if (!state.spends) state.spends = [];
if (!state.spendTotal) state.spendTotal = 0;
if (!state.lastHabitDate) state.lastHabitDate = '';

// reset habits daily
const today = new Date().toDateString();
if (state.lastHabitDate !== today) {
  state.habits = {};
  state.lastHabitDate = today;
  saveState(state);
}

// ── CLOCK ──────────────────────────────────────────────
function updateClock() {
  const now = new Date();
  const h = now.getHours(), m = now.getMinutes(), s = now.getSeconds();
  const ampm = h >= 12 ? 'PM' : 'AM';
  const hh = h % 12 || 12;
  document.getElementById('clock').textContent = `${String(hh).padStart(2,'0')}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')} ${ampm}`;
  const days = ['SUN','MON','TUE','WED','THU','FRI','SAT'];
  const months = ['JAN','FEB','MAR','APR','MAY','JUN','JUL','AUG','SEP','OCT','NOV','DEC'];
  document.getElementById('dateline').textContent = `${days[now.getDay()]} ${String(now.getDate()).padStart(2,'0')} ${months[now.getMonth()]} ${now.getFullYear()}`;
}
setInterval(updateClock, 1000);
updateClock();

// ── COUNTDOWNS ──────────────────────────────────────────
function daysUntil(month, day) {
  const now = new Date(); now.setHours(0,0,0,0);
  const target = new Date(2026, month-1, day);
  return Math.max(0, Math.round((target - now) / 86400000));
}

function renderCountdowns() {
  const d6 = daysUntil(4,6), d7 = daysUntil(4,7), d8 = daysUntil(4,8);
  document.getElementById('cd6').textContent = d6;
  document.getElementById('cd7').textContent = d7;
  document.getElementById('cd8').textContent = d8;
  if (d6 <= 5) document.getElementById('cd6').classList.add('urgent');
  if (d7 <= 5) document.getElementById('cd7').classList.add('urgent');
  if (d8 <= 5) document.getElementById('cd8').classList.add('urgent');
  document.getElementById('ia-days-mini').textContent = d6;
  const dm31 = daysUntil(3,31);
  document.getElementById('days-mar31').textContent = dm31;
}
renderCountdowns();

// ── MESS + TIMETABLE ──────────────────────────────────────
function renderToday() {
  const d = new Date().getDay();
  const idx = d === 0 ? 6 : d - 1;
  document.getElementById('breakfast-today').textContent = MESS.morning[idx];
  document.getElementById('lunch-today').textContent = MESS.lunch[idx];
  document.getElementById('dinner-today').textContent = MESS.dinner[idx];
  const tt = TIMETABLE[d] || { morning:'—', mid:'—', afternoon:'—' };
  document.getElementById('morning-class').textContent = tt.morning;
  document.getElementById('mid-class').textContent = tt.mid;
  document.getElementById('afternoon-class').textContent = tt.afternoon;
}
renderToday();

// ── DOTS ──────────────────────────────────────────────
const dotSubjects = ['maths','english','physics','bio','em','ai'];
function renderDots() {
  dotSubjects.forEach(subj => {
    const el = document.getElementById('d-' + subj);
    if (!el) return;
    el.innerHTML = '';
    const filled = state.dots[subj] || 0;
    for (let i = 0; i < 5; i++) {
      const d = document.createElement('div');
      d.className = 'dot' + (i < filled ? ' on' : '');
      d.onclick = () => {
        const cur = state.dots[subj] || 0;
        state.dots[subj] = (i + 1 === cur) ? 0 : i + 1;
        saveState(state);
        renderDots();
      };
      el.appendChild(d);
    }
  });
}
renderDots();

// ── DEADLINES ──────────────────────────────────────────
function renderDeadlines() {
  const el = document.getElementById('deadline-list');
  const colors = { red: 'var(--red)', amber: 'var(--amber)', blue: 'var(--blue)', green: 'var(--green)' };
  let open = 0;
  el.innerHTML = DEADLINES.map(dl => {
    const done = state.deadlines[dl.id];
    if (!done) open++;
    return `<div class="deadline-row ${done ? 'done' : ''}">
      <div class="dl-badge tag tag-${dl.type}" style="border-color:${colors[dl.type]};color:${colors[dl.type]};background:${colors[dl.type]}18;">${dl.date}</div>
      <div class="dl-info">
        <div class="dl-name">${dl.label}</div>
        <div class="dl-sub">${dl.sub}</div>
      </div>
      <button class="check-btn ${done ? 'done' : ''}" onclick="toggleDeadline('${dl.id}')">${done ? '✓' : '○'}</button>
    </div>`;
  }).join('');
  document.getElementById('open-count').textContent = open;
}
function toggleDeadline(id) {
  state.deadlines[id] = !state.deadlines[id];
  saveState(state);
  renderDeadlines();
}
renderDeadlines();

// ── HABITS ──────────────────────────────────────────────
function renderHabits() {
  const el = document.getElementById('habit-list');
  el.innerHTML = HABITS.map(h => {
    const on = state.habits[h.id];
    return `<div class="habit-row" onclick="toggleHabit('${h.id}')">
      <div>
        <div class="habit-label">${h.label}</div>
        <div class="habit-sub">${h.sub}</div>
      </div>
      <div class="toggle-wrap">
        <div class="toggle ${on ? 'on' : ''}"></div>
      </div>
    </div>`;
  }).join('');
  const done = Object.values(state.habits).filter(Boolean).length;
  const pct = Math.round(done / HABITS.length * 100);
  document.getElementById('habit-score-num').textContent = done + '/' + HABITS.length;
  document.getElementById('habit-bar').style.width = pct + '%';
  const msgs = ['come on shashank. zero.', 'barely started.', 'getting there.', 'more than half. solid.', 'almost there.', 'proper W. all done.'];
  const mi = Math.min(msgs.length-1, Math.floor(done / HABITS.length * (msgs.length-1) + (done===HABITS.length ? 1 : 0)));
  document.getElementById('habit-score-msg').textContent = done === HABITS.length ? msgs[5] : msgs[Math.min(mi, 4)];
}
function toggleHabit(id) {
  state.habits[id] = !state.habits[id];
  saveState(state);
  renderHabits();
  updateStatusBar();
}
renderHabits();

// ── MONEY ──────────────────────────────────────────────
function renderMoney() {
  const spent = state.spendTotal || 0;
  const remaining = Math.max(0, 500 - spent);
  document.getElementById('spent-display').textContent = '₹' + spent;
  const remEl = document.getElementById('remaining-display');
  remEl.textContent = '₹' + remaining;
  remEl.className = 'card-value' + (remaining < 100 ? ' red' : remaining < 200 ? ' amber' : '');
  const logEl = document.getElementById('spend-log');
  const emptyEl = document.getElementById('empty-log');
  if (state.spends && state.spends.length > 0) {
    emptyEl.style.display = 'none';
    logEl.innerHTML = state.spends.map((s,i) =>
      `<div class="money-row">
        <span style="color:var(--text-dim)">${s.label}</span>
        <div style="display:flex;gap:10px;align-items:center;">
          <span class="money-amt">−₹${s.amt}</span>
          <button class="btn btn-red" style="padding:2px 8px;font-size:10px;" onclick="removeSpend(${i})">×</button>
        </div>
      </div>`
    ).join('');
  } else {
    emptyEl.style.display = 'block';
    logEl.innerHTML = '';
  }
}
function addSpend() {
  const amt = parseInt(document.getElementById('amt-input').value);
  const label = document.getElementById('label-input').value.trim() || 'misc';
  if (!amt || amt <= 0) return;
  if (!state.spends) state.spends = [];
  state.spends.push({ amt, label });
  state.spendTotal = (state.spendTotal || 0) + amt;
  document.getElementById('amt-input').value = '';
  document.getElementById('label-input').value = '';
  saveState(state);
  renderMoney();
}
function removeSpend(idx) {
  const s = state.spends[idx];
  state.spendTotal = Math.max(0, (state.spendTotal || 0) - s.amt);
  state.spends.splice(idx, 1);
  saveState(state);
  renderMoney();
}
function resetMoney() {
  if (!confirm('Reset this week\'s money tracker?')) return;
  state.spends = [];
  state.spendTotal = 0;
  saveState(state);
  renderMoney();
}
renderMoney();

// ── SCREEN TIME ──────────────────────────────────────────
function updateScreen() {
  const ig = parseFloat(document.getElementById('ig-in').value) || 0;
  const wa = parseFloat(document.getElementById('wa-in').value) || 0;
  const yt = parseFloat(document.getElementById('yt-in').value) || 0;
  const maxHrs = 14;
  const igOver = ig > 4, waOver = wa > 5, ytOver = yt > 2;
  document.getElementById('ig-fill').style.width = Math.min(100, ig/maxHrs*100) + '%';
  document.getElementById('ig-fill').style.background = igOver ? 'var(--red)' : 'var(--green)';
  document.getElementById('ig-txt').textContent = ig + ' hrs / target 4 hrs';
  document.getElementById('ig-txt').style.color = igOver ? 'var(--red)' : 'var(--green)';
  document.getElementById('wa-fill').style.width = Math.min(100, wa/maxHrs*100) + '%';
  document.getElementById('wa-fill').style.background = waOver ? 'var(--red)' : 'var(--green)';
  document.getElementById('wa-txt').textContent = wa + ' hrs / target 5 hrs';
  document.getElementById('wa-txt').style.color = waOver ? 'var(--red)' : 'var(--green)';
  document.getElementById('yt-fill').style.width = Math.min(100, yt/maxHrs*100) + '%';
  document.getElementById('yt-fill').style.background = ytOver ? 'var(--red)' : 'var(--green)';
  document.getElementById('yt-txt').textContent = yt + ' hrs / target 2 hrs';
  document.getElementById('yt-txt').style.color = ytOver ? 'var(--amber)' : 'var(--green)';
  const total = ig + wa + yt;
  const vEl = document.getElementById('screen-verdict');
  if (total <= 9) {
    vEl.style.borderColor = 'var(--green)'; vEl.style.color = 'var(--green)'; vEl.style.background = '#00ff8808';
    vEl.textContent = '[GOOD] ' + total + ' HRS TOTAL. UNDER TARGET. KEEP IT.';
  } else if (total <= 14) {
    vEl.style.borderColor = 'var(--amber)'; vEl.style.color = 'var(--amber)'; vEl.style.background = '#ffaa0008';
    vEl.textContent = '[WARN] ' + total + ' HRS TOTAL — ' + (total/7).toFixed(1) + ' HRS/DAY. REDUCE.';
  } else {
    vEl.style.borderColor = 'var(--red)'; vEl.style.color = 'var(--red)'; vEl.style.background = '#ff444408';
    vEl.textContent = '[ALERT] ' + total + ' HRS — THATS ' + (total/7).toFixed(1) + ' HRS EVERY SINGLE DAY. STOP.';
  }
}
updateScreen();

// ── TAB SWITCH ──────────────────────────────────────────
function switchTab(name, btn) {
  document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
  document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
  document.getElementById('tab-' + name).classList.add('active');
  btn.classList.add('active');
}

// ── STATUS BAR ──────────────────────────────────────────
function updateStatusBar() {
  const done = Object.values(state.habits).filter(Boolean).length;
  const pct = Math.round(done / HABITS.length * 100);
  document.getElementById('status-left').textContent = `HABITS ${done}/${HABITS.length} · ${today.toUpperCase()}`;
  const el = document.getElementById('status-right');
  if (pct === 100) { el.textContent = 'ALL SYSTEMS GO'; el.className = 'status-green'; }
  else if (pct >= 50) { el.textContent = `${pct}% OPERATIONAL`; el.className = 'status-amber'; }
  else { el.textContent = `${pct}% — NEEDS ATTENTION`; el.className = 'status-red'; }
}
updateStatusBar();

// enter key for money
document.getElementById('label-input').addEventListener('keydown', e => { if (e.key === 'Enter') addSpend(); });
document.getElementById('amt-input').addEventListener('keydown', e => { if (e.key === 'Enter') document.getElementById('label-input').focus(); });
</script>
</body>
</html>
