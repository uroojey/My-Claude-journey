# AI Shark Tank Simulator — Day 25 Build Log

**#60DayClaudeChallenge | Day 25/60**

An interactive web app where you pitch a startup to four AI "sharks" — each with a distinct investing personality — answer their live questions, and receive a full investment verdict: scores, valuation, funding offer, and a final decision (invest, acquire, later, or rejected).

Built single-file in HTML/CSS/JS, calling the Claude API directly from the browser for question generation and final judging.

---

## 1. The Generated HTML Application

The full single-file app — UI, animations, and Claude API calls — is below.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AI Shark Tank Simulator</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@300;400;500;600;700;800&display=swap');

  :root {
    --bg: #050A14;
    --surface: #0D1F3C;
    --surface2: #112240;
    --teal: #00E5FF;
    --teal-dim: rgba(0,229,255,0.12);
    --red: #FF3D57;
    --gold: #FFD700;
    --green: #00E676;
    --slate: #8899AA;
    --white: #E8F0FE;
    --border: rgba(0,229,255,0.18);
  }

  * { margin:0; padding:0; box-sizing:border-box; }

  body {
    background: var(--bg);
    color: var(--white);
    font-family: 'Inter', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── ANIMATED BG ── */
  .bg-grid {
    position: fixed; inset:0; z-index:0;
    background-image:
      linear-gradient(rgba(0,229,255,0.04) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,229,255,0.04) 1px, transparent 1px);
    background-size: 60px 60px;
    pointer-events:none;
  }
  .bg-glow {
    position:fixed; width:600px; height:600px; border-radius:50%;
    background: radial-gradient(circle, rgba(0,229,255,0.06) 0%, transparent 70%);
    top:-200px; left:-200px; pointer-events:none; z-index:0;
    animation: driftGlow 12s ease-in-out infinite alternate;
  }
  @keyframes driftGlow {
    to { transform: translate(300px,200px); }
  }

  /* ── LAYOUT ── */
  .app { position:relative; z-index:1; max-width:900px; margin:0 auto; padding:20px; }

  /* ── HEADER ── */
  .header {
    text-align:center; padding: 30px 0 20px;
  }
  .header-eyebrow {
    font-size:11px; letter-spacing:4px; color:var(--teal);
    text-transform:uppercase; margin-bottom:8px;
  }
  .header h1 {
    font-family:'Bebas Neue', sans-serif;
    font-size: clamp(42px,8vw,80px);
    line-height:0.9;
    background: linear-gradient(135deg, #fff 30%, var(--teal) 100%);
    -webkit-background-clip: text; -webkit-text-fill-color: transparent;
    letter-spacing:2px;
  }
  .header-sub {
    color:var(--slate); font-size:14px; margin-top:10px;
    letter-spacing:0.5px;
  }

  /* ── SHARK FINS DECORATION ── */
  .fins-row {
    display:flex; justify-content:center; gap:12px; margin:14px 0;
  }
  .fin {
    width:0; height:0;
    border-left:10px solid transparent;
    border-right:10px solid transparent;
    border-bottom:24px solid var(--teal);
    opacity:0.3;
    animation: finBob 2s ease-in-out infinite;
  }
  .fin:nth-child(2){ animation-delay:.3s; opacity:.5; }
  .fin:nth-child(3){ animation-delay:.6s; opacity:.7; }
  .fin:nth-child(4){ animation-delay:.9s; opacity:.5; }
  .fin:nth-child(5){ animation-delay:1.2s; opacity:.3; }
  @keyframes finBob { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-6px)} }

  /* ── STAGES ── */
  .stage { display:none; animation: fadeIn .4s ease; }
  .stage.active { display:block; }
  @keyframes fadeIn { from{opacity:0;transform:translateY(16px)} to{opacity:1;transform:translateY(0)} }

  /* ── FORM ── */
  .form-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius:16px;
    padding:32px;
    margin-top:10px;
  }
  .form-title {
    font-family:'Bebas Neue',sans-serif;
    font-size:28px; letter-spacing:2px;
    color:var(--teal); margin-bottom:24px;
  }
  .form-grid { display:grid; grid-template-columns:1fr 1fr; gap:16px; }
  .form-group { display:flex; flex-direction:column; gap:6px; }
  .form-group.full { grid-column:1/-1; }
  label { font-size:11px; letter-spacing:2px; text-transform:uppercase; color:var(--slate); }
  input, textarea {
    background: rgba(255,255,255,0.04);
    border: 1px solid var(--border);
    border-radius:8px;
    color: var(--white);
    font-family:'Inter',sans-serif;
    font-size:14px;
    padding:12px 14px;
    transition: border-color .2s, box-shadow .2s;
    width:100%;
    resize:vertical;
  }
  input:focus, textarea:focus {
    outline:none;
    border-color:var(--teal);
    box-shadow:0 0 0 3px rgba(0,229,255,0.1);
  }
  textarea { min-height:80px; }

  /* ── BUTTON ── */
  .btn {
    display:inline-flex; align-items:center; gap:8px;
    padding:14px 28px; border-radius:8px; border:none; cursor:pointer;
    font-family:'Inter',sans-serif; font-size:14px; font-weight:700;
    letter-spacing:1px; text-transform:uppercase;
    transition: all .2s;
  }
  .btn-primary {
    background: linear-gradient(135deg, var(--teal), #0099bb);
    color:#050A14;
  }
  .btn-primary:hover { transform:translateY(-2px); box-shadow:0 8px 24px rgba(0,229,255,0.35); }
  .btn-primary:disabled { opacity:.4; cursor:not-allowed; transform:none; }
  .btn-ghost {
    background:transparent; color:var(--slate);
    border:1px solid var(--border);
  }
  .btn-ghost:hover { border-color:var(--teal); color:var(--teal); }
  .btn-gold {
    background: linear-gradient(135deg,var(--gold),#cc9900);
    color:#050A14;
  }
  .btn-gold:hover { transform:translateY(-2px); box-shadow:0 8px 24px rgba(255,215,0,0.35); }
  .btn-danger { background:linear-gradient(135deg,var(--red),#cc0022); color:#fff; }
  .form-footer { margin-top:24px; text-align:center; }

  /* ── PITCH DISPLAY ── */
  .pitch-header {
    text-align:center; margin-bottom:24px;
  }
  .startup-name-big {
    font-family:'Bebas Neue',sans-serif;
    font-size:clamp(36px,6vw,64px);
    background:linear-gradient(135deg,var(--gold),#ffaa00);
    -webkit-background-clip:text; -webkit-text-fill-color:transparent;
    letter-spacing:3px;
  }
  .pitch-details {
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:12px;
    padding:20px 24px;
    margin-bottom:20px;
    display:grid; grid-template-columns:1fr 1fr; gap:12px;
  }
  .pitch-field { display:flex; flex-direction:column; gap:4px; }
  .pitch-field.full { grid-column:1/-1; }
  .pitch-label { font-size:10px; letter-spacing:3px; text-transform:uppercase; color:var(--teal); }
  .pitch-value { font-size:14px; color:var(--white); line-height:1.5; }

  /* ── JUDGES ── */
  .judges-grid {
    display:grid; grid-template-columns:repeat(2,1fr); gap:16px;
    margin-bottom:24px;
  }
  .judge-card {
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:12px;
    padding:18px;
    transition: all .3s;
    position:relative; overflow:hidden;
  }
  .judge-card.speaking {
    border-color:var(--teal);
    box-shadow:0 0 24px rgba(0,229,255,0.25);
    animation: speakPulse 1.5s ease-in-out infinite;
  }
  @keyframes speakPulse {
    0%,100%{box-shadow:0 0 24px rgba(0,229,255,0.25)}
    50%{box-shadow:0 0 40px rgba(0,229,255,0.45)}
  }
  .judge-card.invested { border-color:var(--green); box-shadow:0 0 20px rgba(0,230,118,0.2); }
  .judge-card.rejected { border-color:var(--red); box-shadow:0 0 20px rgba(255,61,87,0.2); }
  .judge-avatar {
    font-size:32px; margin-bottom:8px;
  }
  .judge-name {
    font-family:'Bebas Neue',sans-serif;
    font-size:20px; letter-spacing:1px;
    color:var(--white);
  }
  .judge-role {
    font-size:11px; letter-spacing:2px; text-transform:uppercase;
    color:var(--slate); margin-bottom:10px;
  }
  .judge-focus {
    font-size:12px; color:var(--teal);
    font-style:italic; margin-bottom:12px;
  }
  .judge-bubble {
    background:rgba(0,229,255,0.06);
    border:1px solid rgba(0,229,255,0.15);
    border-radius:8px;
    padding:10px 12px;
    font-size:13px;
    line-height:1.5;
    color:var(--white);
    min-height:50px;
    display:none;
  }
  .judge-bubble.visible { display:block; animation:fadeIn .3s ease; }
  .judge-decision-badge {
    display:inline-block; margin-top:10px;
    padding:4px 12px; border-radius:20px;
    font-size:11px; font-weight:700; letter-spacing:1px; text-transform:uppercase;
  }
  .badge-invest { background:rgba(0,230,118,0.15); color:var(--green); border:1px solid var(--green); }
  .badge-reject { background:rgba(255,61,87,0.15); color:var(--red); border:1px solid var(--red); }
  .badge-acquire { background:rgba(255,215,0,0.15); color:var(--gold); border:1px solid var(--gold); }
  .badge-later { background:rgba(136,153,170,0.15); color:var(--slate); border:1px solid var(--slate); }

  /* ── Q&A SECTION ── */
  .qa-section {
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:12px;
    padding:20px 24px;
    margin-bottom:20px;
  }
  .qa-title { font-family:'Bebas Neue',sans-serif; font-size:22px; letter-spacing:2px; color:var(--gold); margin-bottom:16px; }
  .qa-question {
    font-size:14px; color:var(--white); margin-bottom:10px;
    padding:10px 14px;
    background:rgba(0,229,255,0.06);
    border-left:3px solid var(--teal);
    border-radius:0 8px 8px 0;
  }
  .qa-asker { font-size:11px; color:var(--teal); letter-spacing:1px; margin-bottom:6px; }
  .qa-answer-input {
    width:100%;
    background:rgba(255,255,255,0.04);
    border:1px solid var(--border);
    border-radius:8px;
    color:var(--white);
    font-family:'Inter',sans-serif;
    font-size:14px;
    padding:12px 14px;
    resize:vertical; min-height:70px;
    margin-bottom:10px;
    transition:border-color .2s;
  }
  .qa-answer-input:focus { outline:none; border-color:var(--teal); }

  /* ── SCORING ── */
  .scores-grid {
    display:grid; grid-template-columns:1fr 1fr; gap:16px;
    margin-bottom:24px;
  }
  .score-card {
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:12px;
    padding:20px;
  }
  .score-label { font-size:11px; letter-spacing:3px; text-transform:uppercase; color:var(--slate); margin-bottom:10px; }
  .score-bar-wrap { background:rgba(255,255,255,0.06); border-radius:20px; height:8px; overflow:hidden; margin-bottom:6px; }
  .score-bar {
    height:100%; border-radius:20px;
    background:linear-gradient(90deg, var(--teal), #0099bb);
    width:0; transition:width 1.2s cubic-bezier(0.23,1,0.32,1);
  }
  .score-value {
    font-family:'Bebas Neue',sans-serif; font-size:36px;
    color:var(--teal); letter-spacing:1px;
  }
  .score-total { font-size:12px; color:var(--slate); }

  /* ── OVERALL SCORE ── */
  .overall-score {
    text-align:center;
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:16px;
    padding:28px;
    margin-bottom:24px;
    position:relative; overflow:hidden;
  }
  .overall-score::before {
    content:'';
    position:absolute; inset:0;
    background:radial-gradient(ellipse at center, rgba(0,229,255,0.06) 0%, transparent 70%);
    pointer-events:none;
  }
  .overall-label { font-size:11px; letter-spacing:4px; text-transform:uppercase; color:var(--slate); margin-bottom:6px; }
  .overall-number {
    font-family:'Bebas Neue',sans-serif;
    font-size:clamp(72px,12vw,120px);
    line-height:1;
    background:linear-gradient(135deg, var(--gold), #ff9500);
    -webkit-background-clip:text; -webkit-text-fill-color:transparent;
  }
  .overall-suffix { font-family:'Bebas Neue',sans-serif; font-size:28px; color:var(--slate); }

  /* ── DECISION CARD ── */
  .decision-card {
    border-radius:16px;
    padding:28px;
    margin-bottom:24px;
    text-align:center;
    border:2px solid;
    position:relative; overflow:hidden;
  }
  .decision-card.invest { border-color:var(--green); background:rgba(0,230,118,0.06); }
  .decision-card.reject { border-color:var(--red); background:rgba(255,61,87,0.06); }
  .decision-card.acquire { border-color:var(--gold); background:rgba(255,215,0,0.06); }
  .decision-card.later { border-color:var(--slate); background:rgba(136,153,170,0.06); }
  .decision-icon { font-size:52px; margin-bottom:10px; }
  .decision-verdict {
    font-family:'Bebas Neue',sans-serif;
    font-size:clamp(36px,6vw,56px);
    letter-spacing:4px;
  }
  .decision-card.invest .decision-verdict { color:var(--green); }
  .decision-card.reject .decision-verdict { color:var(--red); }
  .decision-card.acquire .decision-verdict { color:var(--gold); }
  .decision-card.later .decision-verdict { color:var(--slate); }
  .decision-valuation { font-size:13px; color:var(--slate); margin-top:6px; }
  .decision-valuation strong { color:var(--white); font-size:16px; }
  .decision-reasoning {
    margin-top:16px;
    font-size:14px; line-height:1.6; color:var(--white);
    background:rgba(255,255,255,0.04);
    border-radius:8px; padding:14px;
    text-align:left;
  }

  /* ── LEADERBOARD ── */
  .leaderboard {
    background:var(--surface);
    border:1px solid var(--border);
    border-radius:12px;
    padding:20px;
    margin-bottom:20px;
  }
  .lb-title { font-family:'Bebas Neue',sans-serif; font-size:24px; letter-spacing:2px; color:var(--gold); margin-bottom:16px; }
  .lb-row {
    display:flex; align-items:center; gap:12px;
    padding:10px 0; border-bottom:1px solid rgba(255,255,255,0.06);
    font-size:14px;
  }
  .lb-rank { font-family:'Bebas Neue',sans-serif; font-size:20px; color:var(--slate); width:28px; }
  .lb-rank.gold { color:var(--gold); }
  .lb-rank.silver { color:#C0C0C0; }
  .lb-rank.bronze { color:#CD7F32; }
  .lb-startup { flex:1; font-weight:600; }
  .lb-score { font-family:'Bebas Neue',sans-serif; font-size:22px; color:var(--teal); }
  .lb-decision { font-size:11px; margin-left:4px; }

  /* ── ACTION BUTTONS ROW ── */
  .actions-row { display:flex; gap:10px; flex-wrap:wrap; justify-content:center; margin:20px 0; }

  /* ── LOADING ── */
  .loading-overlay {
    display:none; position:fixed; inset:0; z-index:999;
    background:rgba(5,10,20,0.85);
    backdrop-filter:blur(6px);
    flex-direction:column;
    align-items:center; justify-content:center; gap:16px;
  }
  .loading-overlay.visible { display:flex; }
  .spinner {
    width:56px; height:56px; border-radius:50%;
    border:3px solid rgba(0,229,255,0.2);
    border-top-color:var(--teal);
    animation:spin 0.8s linear infinite;
  }
  @keyframes spin { to{transform:rotate(360deg)} }
  .loading-text { color:var(--teal); font-size:14px; letter-spacing:2px; }

  /* ── CONFETTI ── */
  #confetti-canvas { position:fixed; inset:0; z-index:998; pointer-events:none; }

  /* ── PROGRESS INDICATOR ── */
  .stage-progress {
    display:flex; align-items:center; justify-content:center; gap:8px;
    margin-bottom:24px;
  }
  .stage-dot {
    width:8px; height:8px; border-radius:50%;
    background:var(--border); transition:all .3s;
  }
  .stage-dot.done { background:var(--teal); }
  .stage-dot.active { background:var(--teal); box-shadow:0 0 8px var(--teal); width:24px; border-radius:4px; }
  .stage-line { flex:1; max-width:40px; height:1px; background:var(--border); }

  /* ── RESPONSIVE ── */
  @media(max-width:600px){
    .form-grid, .judges-grid, .scores-grid, .pitch-details { grid-template-columns:1fr; }
    .form-group.full { grid-column:1; }
    .pitch-field.full { grid-column:1; }
    .app { padding:12px; }
    .form-card { padding:20px; }
  }

  /* scrollbar */
  ::-webkit-scrollbar { width:6px; }
  ::-webkit-scrollbar-track { background:var(--bg); }
  ::-webkit-scrollbar-thumb { background:var(--surface2); border-radius:3px; }
</style>
</head>
<body>

<div class="bg-grid"></div>
<div class="bg-glow"></div>
<canvas id="confetti-canvas"></canvas>

<div class="loading-overlay" id="loadingOverlay">
  <div class="spinner"></div>
  <div class="loading-text" id="loadingText">SUMMONING THE SHARKS...</div>
</div>

<div class="app">

  <!-- HEADER -->
  <div class="header">
    <div class="header-eyebrow">Powered by Claude AI</div>
    <h1>AI Shark Tank</h1>
    <div class="fins-row">
      <div class="fin"></div><div class="fin"></div><div class="fin"></div>
      <div class="fin"></div><div class="fin"></div>
    </div>
    <div class="header-sub">Pitch your startup to 4 AI sharks. Get funded or get eaten.</div>
  </div>

  <!-- STAGE PROGRESS -->
  <div class="stage-progress" id="stageProgress">
    <div class="stage-dot active" id="dot1"></div>
    <div class="stage-line"></div>
    <div class="stage-dot" id="dot2"></div>
    <div class="stage-line"></div>
    <div class="stage-dot" id="dot3"></div>
    <div class="stage-line"></div>
    <div class="stage-dot" id="dot4"></div>
  </div>

  <!-- STAGE 1: INPUT -->
  <div class="stage active" id="stage1">
    <div class="form-card">
      <div class="form-title">🎯 Your Startup Pitch</div>
      <div class="form-grid">
        <div class="form-group">
          <label>Startup Name *</label>
          <input type="text" id="startupName" placeholder="e.g. EduBot AI" maxlength="60">
        </div>
        <div class="form-group">
          <label>Funding Ask *</label>
          <input type="text" id="fundingAsk" placeholder="e.g. ₹50 Lakh for 10%" maxlength="60">
        </div>
        <div class="form-group full">
          <label>Problem Statement *</label>
          <textarea id="problem" placeholder="What critical problem are you solving? Be specific."></textarea>
        </div>
        <div class="form-group full">
          <label>Your Solution *</label>
          <textarea id="solution" placeholder="How does your product solve this problem?"></textarea>
        </div>
        <div class="form-group">
          <label>Revenue Model *</label>
          <textarea id="revenueModel" placeholder="How do you make money?" style="min-height:60px"></textarea>
        </div>
        <div class="form-group">
          <label>Target Audience *</label>
          <textarea id="targetAudience" placeholder="Who are your customers?" style="min-height:60px"></textarea>
        </div>
      </div>
      <div class="form-footer">
        <button class="btn btn-primary" onclick="startPitch()" id="pitchBtn">
          🦈 Enter the Tank
        </button>
      </div>
    </div>
  </div>

  <!-- STAGE 2: PITCH ROUND -->
  <div class="stage" id="stage2">
    <div class="pitch-header">
      <div class="header-eyebrow">Now Pitching</div>
      <div class="startup-name-big" id="displayName"></div>
    </div>

    <div class="pitch-details" id="pitchDetails"></div>

    <div class="judges-grid" id="judgesGrid"></div>

    <div class="qa-section" id="qaSection">
      <div class="qa-title">⚡ Shark Questions</div>
      <div id="qaContainer"></div>
    </div>

    <div class="actions-row">
      <button class="btn btn-primary" onclick="submitAnswers()" id="submitAnswersBtn">
        📊 Submit &amp; Get Verdict
      </button>
    </div>
  </div>

  <!-- STAGE 3: SCORING -->
  <div class="stage" id="stage3">
    <div class="pitch-header">
      <div class="header-eyebrow">Shark Verdict</div>
      <div class="startup-name-big" id="displayName3"></div>
    </div>

    <div class="overall-score">
      <div class="overall-label">Overall Score</div>
      <div class="overall-number" id="overallScoreNum">0</div>
      <span class="overall-suffix">/ 100</span>
    </div>

    <div class="scores-grid" id="scoresGrid"></div>

    <div class="decision-card" id="decisionCard">
      <div class="decision-icon" id="decisionIcon"></div>
      <div class="decision-verdict" id="decisionVerdict"></div>
      <div class="decision-valuation" id="decisionValuation"></div>
      <div class="decision-reasoning" id="decisionReasoning"></div>
    </div>

    <div class="judges-grid" id="judgesGrid3"></div>

    <div class="actions-row">
      <button class="btn btn-gold" onclick="downloadReport()">📄 Download Report</button>
      <button class="btn btn-ghost" onclick="shareResult()">🔗 Share Result</button>
      <button class="btn btn-primary" onclick="resetApp()">🔄 Pitch Again</button>
    </div>

    <div class="leaderboard" id="leaderboard">
      <div class="lb-title">🏆 Hall of Sharks — Leaderboard</div>
      <div id="lbRows"></div>
    </div>
  </div>

</div><!-- /app -->

<script>
// ── JUDGES CONFIG ──
const JUDGES = [
  {
    id:'vc',
    emoji:'💼',
    name:'Victoria Chen',
    role:'Venture Capitalist',
    focus:'Market size & scalability',
    personality:'data-driven, asks about TAM, growth metrics, exit strategy'
  },
  {
    id:'founder',
    emoji:'🚀',
    name:'Marcus Rivera',
    role:'Serial Founder',
    focus:'Execution & team',
    personality:'pragmatic, challenges assumptions, asks about traction and team'
  },
  {
    id:'customer',
    emoji:'👥',
    name:'Priya Sharma',
    role:'Customer Advocate',
    focus:'User value & adoption',
    personality:'empathetic, asks about real user pain, willingness to pay, retention'
  },
  {
    id:'angel',
    emoji:'👼',
    name:'James Okafor',
    role:'Angel Investor',
    focus:'Profitability & ROI',
    personality:'conservative, asks about unit economics, path to profit, risk'
  }
];

// ── STATE ──
let pitchData = {};
let judgeQuestions = [];
let verdictData = {};
let leaderboard = JSON.parse(localStorage.getItem('stLeaderboard')||'[]');

// ── HELPERS ──
function showLoading(msg='THINKING...'){
  document.getElementById('loadingText').textContent = msg;
  document.getElementById('loadingOverlay').classList.add('visible');
}
function hideLoading(){
  document.getElementById('loadingOverlay').classList.remove('visible');
}
function setStage(n){
  document.querySelectorAll('.stage').forEach(s=>s.classList.remove('active'));
  document.getElementById('stage'+n).classList.add('active');
  // update dots
  for(let i=1;i<=4;i++){
    const d=document.getElementById('dot'+i);
    if(!d)continue;
    d.classList.remove('active','done');
    if(i<n) d.classList.add('done');
    if(i===n) d.classList.add('active');
  }
  window.scrollTo({top:0,behavior:'smooth'});
}

// ── ANTHROPIC API CALL ──
async function callClaude(prompt, maxTokens=1200){
  const res = await fetch('https://api.anthropic.com/v1/messages',{
    method:'POST',
    headers:{'Content-Type':'application/json'},
    body:JSON.stringify({
      model:'claude-sonnet-4-6',
      max_tokens: maxTokens,
      messages:[{role:'user',content:prompt}]
    })
  });
  const data = await res.json();
  if(data.error) throw new Error(data.error.message);
  return data.content.map(c=>c.text||'').join('');
}

// ── STAGE 1 → 2 ──
async function startPitch(){
  const name = document.getElementById('startupName').value.trim();
  const funding = document.getElementById('fundingAsk').value.trim();
  const problem = document.getElementById('problem').value.trim();
  const solution = document.getElementById('solution').value.trim();
  const revenue = document.getElementById('revenueModel').value.trim();
  const audience = document.getElementById('targetAudience').value.trim();

  if(!name||!problem||!solution||!revenue||!audience||!funding){
    alert('Please fill in all fields before entering the Tank!');
    return;
  }

  pitchData = {name,funding,problem,solution,revenue,audience};

  showLoading('WAKING UP THE SHARKS...');
  try {
    const questionsRaw = await callClaude(`
You are running a Shark Tank simulation. A startup is pitching:
- Name: ${name}
- Problem: ${problem}
- Solution: ${solution}
- Revenue Model: ${revenue}
- Target Audience: ${audience}
- Funding Ask: ${funding}

Generate exactly 2 sharp, distinct questions for EACH of these 4 judges (8 total):
1. Victoria Chen (VC) - focuses on market size & scalability
2. Marcus Rivera (Serial Founder) - focuses on execution & team
3. Priya Sharma (Customer Advocate) - focuses on user value & adoption
4. James Okafor (Angel Investor) - focuses on profitability & ROI

Output ONLY valid JSON array, no markdown, no backticks:
[
  {"judge":"vc","q":"question 1"},
  {"judge":"vc","q":"question 2"},
  {"judge":"founder","q":"question 3"},
  {"judge":"founder","q":"question 4"},
  {"judge":"customer","q":"question 5"},
  {"judge":"customer","q":"question 6"},
  {"judge":"angel","q":"question 7"},
  {"judge":"angel","q":"question 8"}
]
`, 800);

    let cleaned = questionsRaw.trim().replace(/```json|```/g,'').trim();
    judgeQuestions = JSON.parse(cleaned);

    hideLoading();
    buildStage2();
    setStage(2);
  } catch(e){
    hideLoading();
    alert('Error: '+e.message);
  }
}

function buildStage2(){
  document.getElementById('displayName').textContent = pitchData.name;

  // Pitch details
  const pd = document.getElementById('pitchDetails');
  pd.innerHTML = `
    <div class="pitch-field full">
      <div class="pitch-label">Problem</div>
      <div class="pitch-value">${pitchData.problem}</div>
    </div>
    <div class="pitch-field full">
      <div class="pitch-label">Solution</div>
      <div class="pitch-value">${pitchData.solution}</div>
    </div>
    <div class="pitch-field">
      <div class="pitch-label">Revenue Model</div>
      <div class="pitch-value">${pitchData.revenue}</div>
    </div>
    <div class="pitch-field">
      <div class="pitch-label">Target Audience</div>
      <div class="pitch-value">${pitchData.audience}</div>
    </div>
    <div class="pitch-field full">
      <div class="pitch-label">Funding Ask</div>
      <div class="pitch-value">${pitchData.funding}</div>
    </div>
  `;

  // Judge cards
  const jg = document.getElementById('judgesGrid');
  jg.innerHTML = JUDGES.map(j=>`
    <div class="judge-card" id="jcard-${j.id}">
      <div class="judge-avatar">${j.emoji}</div>
      <div class="judge-name">${j.name}</div>
      <div class="judge-role">${j.role}</div>
      <div class="judge-focus">Focus: ${j.focus}</div>
      <div class="judge-bubble" id="jbubble-${j.id}"></div>
    </div>
  `).join('');

  // Q&A
  const qc = document.getElementById('qaContainer');
  qc.innerHTML = '';
  judgeQuestions.forEach((q,i)=>{
    const judge = JUDGES.find(j=>j.id===q.judge);
    qc.innerHTML += `
      <div style="margin-bottom:18px">
        <div class="qa-asker">${judge.emoji} ${judge.name} asks:</div>
        <div class="qa-question">${q.q}</div>
        <textarea class="qa-answer-input" id="ans-${i}" placeholder="Your answer..." rows="2"></textarea>
      </div>
    `;
  });

  // Animate judge intros
  JUDGES.forEach((j,i)=>{
    setTimeout(()=>{
      const card = document.getElementById('jcard-'+j.id);
      const bubble = document.getElementById('jbubble-'+j.id);
      card.classList.add('speaking');
      bubble.textContent = getJudgeIntro(j);
      bubble.classList.add('visible');
      setTimeout(()=>card.classList.remove('speaking'),2000);
    },i*600);
  });
}

function getJudgeIntro(j){
  const intros = {
    vc: "Let's see if this can become a billion-dollar company. I'm looking at your market size.",
    founder: "Talk is cheap. Show me you can actually build and ship this thing.",
    customer: "I want to know if real people will actually pay for and love this product.",
    angel: "Numbers don't lie. I need to see a clear path to profitability and returns."
  };
  return intros[j.id];
}

// ── STAGE 2 → 3 ──
async function submitAnswers(){
  const answers = judgeQuestions.map((q,i)=>{
    const val = document.getElementById('ans-'+i).value.trim();
    return {judge:q.judge, question:q.q, answer:val||'[No answer provided]'};
  });

  showLoading('SHARKS ARE DELIBERATING...');
  try {
    const qaText = answers.map(a=>`[${a.judge.toUpperCase()}] Q: ${a.question}\nA: ${a.answer}`).join('\n\n');

    const verdictRaw = await callClaude(`
You are a Shark Tank judge panel AI. Evaluate this startup pitch:

STARTUP: ${pitchData.name}
PROBLEM: ${pitchData.problem}
SOLUTION: ${pitchData.solution}
REVENUE MODEL: ${pitchData.revenue}
TARGET AUDIENCE: ${pitchData.audience}
FUNDING ASK: ${pitchData.funding}

Q&A TRANSCRIPT:
${qaText}

Provide a complete verdict. Output ONLY valid JSON, no markdown, no backticks:
{
  "scores": {
    "marketPotential": <0-100>,
    "innovation": <0-100>,
    "businessModel": <0-100>,
    "execution": <0-100>,
    "investmentWorthiness": <0-100>
  },
  "overallScore": <0-100, weighted average>,
  "decision": "<invest|reject|acquire|later>",
  "valuation": "<suggested company valuation as string>",
  "fundingOffer": "<funding amount offered or 'No offer'>",
  "reasoning": "<3-4 sentences explaining the verdict honestly>",
  "judgeReactions": {
    "vc": {"reaction":"<1-2 sentence honest reaction>","decision":"<invest|reject|acquire|later>"},
    "founder": {"reaction":"<1-2 sentence honest reaction>","decision":"<invest|reject|acquire|later>"},
    "customer": {"reaction":"<1-2 sentence honest reaction>","decision":"<invest|reject|acquire|later>"},
    "angel": {"reaction":"<1-2 sentence honest reaction>","decision":"<invest|reject|acquire|later>"}
  }
}
`, 1000);

    let cleaned = verdictRaw.trim().replace(/```json|```/g,'').trim();
    verdictData = JSON.parse(cleaned);
    hideLoading();
    buildStage3();
    setStage(3);

    if(verdictData.decision==='invest'||verdictData.decision==='acquire'){
      setTimeout(launchConfetti,400);
    }

    // Save to leaderboard
    const entry = {
      name: pitchData.name,
      score: verdictData.overallScore,
      decision: verdictData.decision,
      date: new Date().toLocaleDateString()
    };
    leaderboard.push(entry);
    leaderboard.sort((a,b)=>b.score-a.score);
    leaderboard = leaderboard.slice(0,10);
    localStorage.setItem('stLeaderboard', JSON.stringify(leaderboard));

  } catch(e){
    hideLoading();
    alert('Error: '+e.message);
  }
}

function buildStage3(){
  document.getElementById('displayName3').textContent = pitchData.name;

  // Animate overall score
  const s = verdictData;
  let current=0; const target=s.overallScore;
  const el=document.getElementById('overallScoreNum');
  const interval=setInterval(()=>{
    current=Math.min(current+2,target);
    el.textContent=current;
    if(current>=target)clearInterval(interval);
  },20);

  // Score cards
  const scoreKeys=[
    {key:'marketPotential',label:'Market Potential'},
    {key:'innovation',label:'Innovation'},
    {key:'businessModel',label:'Business Model'},
    {key:'execution',label:'Execution'},
    {key:'investmentWorthiness',label:'Investment Worthiness'}
  ];
  const sg=document.getElementById('scoresGrid');
  sg.innerHTML = scoreKeys.map(sk=>`
    <div class="score-card">
      <div class="score-label">${sk.label}</div>
      <div class="score-bar-wrap"><div class="score-bar" id="bar-${sk.key}"></div></div>
      <div class="score-value" id="sv-${sk.key}">0</div>
      <div class="score-total">/ 100</div>
    </div>
  `).join('');

  // Animate bars
  setTimeout(()=>{
    scoreKeys.forEach((sk,i)=>{
      setTimeout(()=>{
        const v = s.scores[sk.key];
        document.getElementById('bar-'+sk.key).style.width=v+'%';
        animateNum('sv-'+sk.key, v, 800);
      },i*150);
    });
  },200);

  // Decision card
  const dc = document.getElementById('decisionCard');
  const icons = {invest:'🤝',reject:'🦈',acquire:'💰',later:'⏰'};
  const labels = {invest:'FUNDED!',reject:'OUT.',acquire:'ACQUIRED!',later:'COME BACK LATER'};
  dc.className='decision-card '+s.decision;
  document.getElementById('decisionIcon').textContent=icons[s.decision]||'⚡';
  document.getElementById('decisionVerdict').textContent=labels[s.decision]||s.decision.toUpperCase();
  document.getElementById('decisionValuation').innerHTML=`
    Valuation: <strong>${s.valuation}</strong> &nbsp;|&nbsp; Offer: <strong>${s.fundingOffer}</strong>
  `;
  document.getElementById('decisionReasoning').textContent=s.reasoning;

  // Judge reactions
  const jg3=document.getElementById('judgesGrid3');
  jg3.innerHTML=JUDGES.map(j=>{
    const jr=s.judgeReactions[j.id]||{reaction:'No comment.',decision:'later'};
    const bClass={'invest':'badge-invest','reject':'badge-reject','acquire':'badge-acquire','later':'badge-later'}[jr.decision]||'badge-later';
    const dLabel={'invest':'💚 IN','reject':'❌ OUT','acquire':'💰 ACQUIRE','later':'⏳ LATER'}[jr.decision]||jr.decision;
    const cardClass={'invest':'invested','reject':'rejected','acquire':'invested','later':''}[jr.decision]||'';
    return `
      <div class="judge-card ${cardClass}">
        <div class="judge-avatar">${j.emoji}</div>
        <div class="judge-name">${j.name}</div>
        <div class="judge-role">${j.role}</div>
        <div class="judge-bubble visible">${jr.reaction}</div>
        <span class="judge-decision-badge ${bClass}">${dLabel}</span>
      </div>
    `;
  }).join('');

  // Leaderboard
  buildLeaderboard();
}

function animateNum(id, target, duration){
  let current=0; const el=document.getElementById(id);
  const step=Math.ceil(target/(duration/20));
  const interval=setInterval(()=>{
    current=Math.min(current+step,target);
    el.textContent=current;
    if(current>=target)clearInterval(interval);
  },20);
}

function buildLeaderboard(){
  const rows=document.getElementById('lbRows');
  if(leaderboard.length===0){
    rows.innerHTML='<div style="color:var(--slate);font-size:13px;text-align:center;padding:12px">No pitches yet. You're first!</div>';
    return;
  }
  const rankClasses=['gold','silver','bronze'];
  const decisionEmoji={invest:'✅',reject:'❌',acquire:'💰',later:'⏳'};
  rows.innerHTML=leaderboard.map((e,i)=>`
    <div class="lb-row">
      <div class="lb-rank ${rankClasses[i]||''}">${i===0?'🥇':i===1?'🥈':i===2?'🥉':i+1}</div>
      <div class="lb-startup">${e.name}</div>
      <div class="lb-score">${e.score}</div>
      <div class="lb-decision">${decisionEmoji[e.decision]||''}</div>
    </div>
  `).join('');
}

// ── CONFETTI ──
function launchConfetti(){
  const canvas=document.getElementById('confetti-canvas');
  const ctx=canvas.getContext('2d');
  canvas.width=window.innerWidth; canvas.height=window.innerHeight;
  const pieces=[];
  const colors=['#00E5FF','#FFD700','#00E676','#FF3D57','#ffffff','#a78bfa'];
  for(let i=0;i<160;i++){
    pieces.push({
      x:Math.random()*canvas.width,
      y:Math.random()*canvas.height-canvas.height,
      w:Math.random()*10+4, h:Math.random()*6+2,
      color:colors[Math.floor(Math.random()*colors.length)],
      r:Math.random()*Math.PI*2,
      rv:(Math.random()-.5)*.2,
      dx:(Math.random()-.5)*3,
      dy:Math.random()*4+2,
      alpha:1
    });
  }
  let frame=0;
  function draw(){
    ctx.clearRect(0,0,canvas.width,canvas.height);
    pieces.forEach(p=>{
      ctx.save();
      ctx.translate(p.x,p.y);
      ctx.rotate(p.r);
      ctx.globalAlpha=p.alpha;
      ctx.fillStyle=p.color;
      ctx.fillRect(-p.w/2,-p.h/2,p.w,p.h);
      ctx.restore();
      p.x+=p.dx; p.y+=p.dy; p.r+=p.rv;
      if(frame>100) p.alpha-=0.012;
    });
    frame++;
    if(frame<220) requestAnimationFrame(draw);
    else ctx.clearRect(0,0,canvas.width,canvas.height);
  }
  draw();
}

// ── DOWNLOAD REPORT ──
function downloadReport(){
  const s=verdictData;
  const report=`
AI SHARK TANK — PITCH REPORT
════════════════════════════════════════════════════

STARTUP: ${pitchData.name}
FUNDING ASK: ${pitchData.funding}
DATE: ${new Date().toLocaleDateString()}

────────────────────────────────────────────────────
PITCH SUMMARY
────────────────────────────────────────────────────
Problem:        ${pitchData.problem}
Solution:       ${pitchData.solution}
Revenue Model:  ${pitchData.revenue}
Target Market:  ${pitchData.audience}

────────────────────────────────────────────────────
SCORES
────────────────────────────────────────────────────
Market Potential:       ${s.scores.marketPotential}/100
Innovation:             ${s.scores.innovation}/100
Business Model:         ${s.scores.businessModel}/100
Execution:              ${s.scores.execution}/100
Investment Worthiness:  ${s.scores.investmentWorthiness}/100

OVERALL SCORE: ${s.overallScore}/100

────────────────────────────────────────────────────
VERDICT
────────────────────────────────────────────────────
Decision:    ${s.decision.toUpperCase()}
Valuation:   ${s.valuation}
Offer:       ${s.fundingOffer}

Reasoning:
${s.reasoning}

────────────────────────────────────────────────────
JUDGE REACTIONS
────────────────────────────────────────────────────
${JUDGES.map(j=>{
  const jr=s.judgeReactions[j.id]||{};
  return `${j.emoji} ${j.name} (${j.role})\n  Decision: ${(jr.decision||'').toUpperCase()}\n  "${jr.reaction}"`;
}).join('\n\n')}

════════════════════════════════════════════════════
Generated by AI Shark Tank Simulator | #60DayClaudeChallenge
  `.trim();

  const blob=new Blob([report],{type:'text/plain'});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download=`${pitchData.name.replace(/\s+/g,'_')}_PitchReport.txt`;
  a.click();
}

// ── SHARE ──
function shareResult(){
  const dec={invest:'got FUNDED 🤝',reject:'got REJECTED 🦈',acquire:'got ACQUIRED 💰',later:'was told to Come Back Later ⏳'};
  const text=`🦈 "${pitchData.name}" ${dec[verdictData.decision]||''} with a score of ${verdictData.overallScore}/100 on the AI Shark Tank Simulator!\n\n#AISharkTank #60DayClaudeChallenge #StartupLife`;
  if(navigator.share){
    navigator.share({title:'AI Shark Tank Result',text}).catch(()=>{});
  } else {
    navigator.clipboard.writeText(text).then(()=>alert('Result copied to clipboard! Paste it anywhere to share.'));
  }
}

// ── RESET ──
function resetApp(){
  verdictData={};
  document.getElementById('qaContainer').innerHTML='';
  setStage(1);
}
</script>
</body>
</html>
```

---

## 2. Sample Startup Evaluation

*(Demo walkthrough using the app above — not a real fundraise.)*

**Pitch submitted:**

| Field | Input |
|---|---|
| Startup Name | EduBot AI |
| Funding Ask | ₹50 Lakh for 10% |
| Problem | Students in tier-2/3 cities have no one to ask when they get stuck on homework after school hours. |
| Solution | A 24/7 AI tutor chatbot trained on the NCERT curriculum that explains concepts step-by-step instead of just giving answers. |
| Revenue Model | Freemium — free for basic Q&A, ₹199/month for unlimited doubt-solving and mock tests. |
| Target Audience | Class 9–12 students preparing for board exams in non-metro India. |

**Sample questions the sharks generated:**

- 🦈 **Victoria Chen (VC):** "EdTech tutoring apps are crowded — what stops a student from just using ChatGPT for free?"
- 🦈 **Marcus Rivera (Founder):** "You're a team of three. How are you building and maintaining curriculum coverage for every board and state syllabus?"
- 🦈 **Priya Sharma (Customer Advocate):** "Will a 16-year-old actually pay ₹199/month, or is it the parent's wallet you're really selling to?"
- 🦈 **James Okafor (Angel):** "What's your CAC in tier-2/3 markets where digital payment friction is still high?"

## 3. Investment Decision

| Score Dimension | Result |
|---|---|
| Market Potential | 78 / 100 |
| Innovation | 65 / 100 |
| Business Model | 72 / 100 |
| Execution | 60 / 100 |
| Investment Worthiness | 68 / 100 |
| **Overall Score** | **69 / 100** |

**Verdict: ⏳ COME BACK LATER**
**Suggested Valuation:** ₹4 Crore &nbsp;|&nbsp; **Offer:** No offer

**Panel reasoning:** Strong, real problem and a believable willingness-to-pay story in underserved markets, but the curriculum-coverage and CAC questions exposed execution gaps the team hasn't solved yet. The sharks want to see traction numbers before writing a check.

| Judge | Reaction | Decision |
|---|---|---|
| Victoria Chen (VC) | "Big market, but you didn't convince me of a moat against free LLM tools." | ❌ OUT |
| Marcus Rivera (Founder) | "Three people covering every state board is a content nightmare — show me you've shipped it first." | ⏳ LATER |
| Priya Sharma (Customer Advocate) | "I like that you separated the buyer from the user — that's rare in this space." | 💚 IN |
| James Okafor (Angel) | "Unit economics aren't there yet. Come back with 1,000 paying users." | ⏳ LATER |

---

## 4. Key Learnings

**Personas change the output, not just the tone.**
Giving each judge a name and a narrow focus (market size, execution, customer love, ROI) made the questions genuinely conflict with each other — which is what makes a real Shark Tank panel interesting. A single generic "AI evaluator" prompt never produced that tension.

**Structure is the hard part, not intelligence.**
Getting Claude to reason well about a pitch was easy. Getting it to reliably return clean, parseable JSON — every time, with no markdown fences slipping in — was the actual engineering problem, since the entire UI (score bars, badges, leaderboard) depends on that structure holding.

**Staging the reveal matters as much as the content.**
Animated score counters, a confetti burst only on "invest"/"acquire," and judges who "speak" one at a time — none of that changes the verdict, but it's the difference between a number on a page and something that actually feels like a verdict.

**Small persistent state goes a long way.**
A simple `localStorage` leaderboard (top 10 pitches by score) turned a one-shot tool into something with a reason to come back and try again.

---

*Day 25 of 60 — #60DayClaudeChallenge*
