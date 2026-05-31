<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Weekend Volleyball Signup</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #F5F3EF;
    --card: #FFFFFF;
    --text: #1a1a18;
    --muted: #7a7870;
    --border: #E0DDD8;
    --accent: #2E7D32;
    --accent-light: #E8F5E9;
    --accent-text: #1B5E20;
    --blue: #1565C0;
    --blue-light: #E3F2FD;
    --blue-text: #0D47A1;
    --danger: #C62828;
    --danger-light: #FFEBEE;
    --warning: #E65100;
    --warning-light: #FFF3E0;
    --radius: 12px;
    --radius-sm: 8px;
    --shadow: 0 1px 3px rgba(0,0,0,0.08), 0 4px 16px rgba(0,0,0,0.05);
  }

  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #141412; --card: #1E1E1C; --text: #F0EDE8; --muted: #888680;
      --border: #2C2C2A; --accent: #4CAF50; --accent-light: #1B3A1C;
      --accent-text: #A5D6A7; --blue: #42A5F5; --blue-light: #0D2A4A;
      --blue-text: #90CAF9; --danger: #EF9A9A; --danger-light: #3B1010;
      --warning: #FFB74D; --warning-light: #2A1A00;
    }
  }

  body { font-family: 'DM Sans', sans-serif; background: var(--bg); color: var(--text); min-height: 100vh; padding: 2rem 1rem; }
  .page { max-width: 580px; margin: 0 auto; }

  /* ADMIN PANEL TOGGLE */
  .admin-toggle {
    display: flex;
    justify-content: flex-end;
    margin-bottom: 1rem;
  }
  .btn-admin {
    padding: 7px 16px;
    border-radius: 999px;
    border: 1px solid var(--border);
    background: var(--card);
    font-family: 'DM Sans', sans-serif;
    font-size: 12px;
    font-weight: 600;
    cursor: pointer;
    color: var(--muted);
    transition: all 0.12s;
  }
  .btn-admin:hover { color: var(--text); border-color: var(--text); }

  /* ADMIN PANEL */
  .admin-panel {
    background: var(--card);
    border: 1.5px solid var(--warning);
    border-radius: var(--radius);
    padding: 1.5rem;
    margin-bottom: 1rem;
    box-shadow: var(--shadow);
    display: none;
  }
  .admin-panel.show { display: block; }

  .admin-panel h3 {
    font-size: 13px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.8px;
    color: var(--warning);
    font-family: 'DM Mono', monospace;
    margin-bottom: 1.25rem;
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .admin-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .admin-grid .field.full { grid-column: 1 / -1; }

  .section-label {
    font-size: 11px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.6px;
    color: var(--muted);
    font-family: 'DM Mono', monospace;
    grid-column: 1 / -1;
    margin-top: 6px;
    padding-bottom: 6px;
    border-bottom: 1px solid var(--border);
  }

  /* HERO */
  .hero { text-align: center; padding: 2rem 0 2rem; }
  .hero-ball { font-size: 52px; display: block; margin-bottom: 1rem; animation: bounce 2s infinite; }
  @keyframes bounce { 0%,100%{transform:translateY(0)} 50%{transform:translateY(-8px)} }
  .hero h1 { font-size: 1.9rem; font-weight: 600; letter-spacing: -0.5px; }
  .hero p { font-size: 15px; color: var(--muted); margin-top: 6px; }

  /* CARD */
  .card { background: var(--card); border: 1px solid var(--border); border-radius: var(--radius); padding: 1.5rem; box-shadow: var(--shadow); margin-bottom: 1rem; }
  .card-head { display: flex; align-items: center; gap: 10px; margin-bottom: 1.25rem; padding-bottom: 1rem; border-bottom: 1px solid var(--border); }
  .card-head h2 { font-size: 15px; font-weight: 600; flex: 1; }
  .badge { font-size: 11px; font-weight: 600; font-family: 'DM Mono', monospace; padding: 3px 10px; border-radius: 999px; background: var(--accent-light); color: var(--accent-text); }

  /* FORM */
  .form-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 12px; }
  .field { display: flex; flex-direction: column; gap: 5px; }
  .field.full { grid-column: 1 / -1; }
  .field label { font-size: 11px; font-weight: 600; color: var(--muted); letter-spacing: 0.5px; text-transform: uppercase; font-family: 'DM Mono', monospace; display: flex; align-items: center; gap: 4px; }
  .field label .req { color: var(--danger); font-size: 13px; line-height: 1; }
  .field input, .field select {
    padding: 0 12px; height: 40px; border: 1px solid var(--border); border-radius: var(--radius-sm);
    background: var(--bg); font-family: 'DM Sans', sans-serif; font-size: 14px; color: var(--text);
    outline: none; transition: border 0.15s, box-shadow 0.15s; width: 100%;
  }
  .field input:focus, .field select:focus { border-color: var(--accent); box-shadow: 0 0 0 3px rgba(46,125,50,0.12); }
  .field input::placeholder { color: var(--muted); }
  .field input.error, .field select.error { border-color: var(--danger); box-shadow: 0 0 0 3px rgba(198,40,40,0.1); }
  .field-error { font-size: 11px; color: var(--danger); display: none; }
  .field-error.show { display: block; }

  .btn-signup {
    width: 100%; height: 46px; border-radius: var(--radius-sm); border: none;
    background: var(--accent); color: #fff; font-family: 'DM Sans', sans-serif;
    font-size: 15px; font-weight: 600; cursor: pointer; margin-top: 2px;
    transition: all 0.15s; display: flex; align-items: center; justify-content: center; gap: 8px;
  }
  .btn-signup:hover { filter: brightness(1.1); }
  .btn-signup:active { transform: scale(0.98); }
  .btn-signup:disabled { opacity: 0.6; cursor: not-allowed; transform: none; filter: none; }

  /* PLAYER LIST */
  .player-list { display: flex; flex-direction: column; gap: 6px; }
  .player-row {
    display: flex; align-items: center; gap: 10px; padding: 10px 12px;
    border-radius: var(--radius-sm); background: var(--bg); border: 1px solid var(--border);
    animation: slideIn 0.2s ease;
  }
  @keyframes slideIn { from{opacity:0;transform:translateY(-4px)} to{opacity:1;transform:translateY(0)} }
  .player-num { font-size: 11px; font-weight: 500; color: var(--muted); font-family: 'DM Mono', monospace; min-width: 16px; text-align: right; flex-shrink: 0; }
  .avatar { width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 11px; font-weight: 700; flex-shrink: 0; }
  .player-info { flex: 1; min-width: 0; }
  .player-name { font-size: 14px; font-weight: 600; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .player-meta { font-size: 11px; color: var(--muted); font-family: 'DM Mono', monospace; margin-top: 1px; }
  .paid-badge {
    font-size: 10px; font-weight: 700; padding: 3px 9px; border-radius: 999px;
    font-family: 'DM Mono', monospace; flex-shrink: 0; cursor: pointer; border: none;
    transition: all 0.12s;
  }
  .paid-badge.unpaid { background: var(--danger-light); color: var(--danger); }
  .paid-badge.paid { background: var(--accent-light); color: var(--accent-text); }
  .date-badge {
    font-size: 11px; font-weight: 600; padding: 3px 9px; border-radius: 999px;
    background: var(--blue-light); color: var(--blue-text); flex-shrink: 0;
    font-family: 'DM Mono', monospace;
  }
  .btn-remove { background: none; border: none; cursor: pointer; color: var(--muted); padding: 4px; border-radius: 6px; font-size: 15px; line-height: 1; transition: color 0.12s; flex-shrink: 0; }
  .btn-remove:hover { color: var(--danger); }

  .empty { text-align: center; padding: 2rem; color: var(--muted); font-size: 14px; }
  .empty .emoji { font-size: 28px; display: block; margin-bottom: 8px; }

  .total-bar { display: flex; align-items: center; justify-content: space-between; gap: 8px; font-size: 13px; color: var(--muted); margin-top: 1rem; padding-top: 1rem; border-top: 1px solid var(--border); }
  .total-count { display: flex; align-items: center; gap: 6px; }
  .total-dot { width: 6px; height: 6px; border-radius: 50%; background: var(--accent); }
  .total-amount { font-family: 'DM Mono', monospace; font-size: 12px; font-weight: 600; color: var(--accent-text); background: var(--accent-light); padding: 3px 10px; border-radius: 999px; }

  /* TEAMS */
  .teams-card { display: none; }
  .teams-card.show { display: block; }
  .teams-head { display: flex; align-items: center; gap: 10px; margin-bottom: 1rem; padding-bottom: 1rem; border-bottom: 1px solid var(--border); }
  .teams-head h2 { font-size: 15px; font-weight: 600; flex: 1; }
  .btn-shuffle { padding: 6px 14px; border-radius: 999px; border: 1px solid var(--border); background: transparent; font-family: 'DM Sans', sans-serif; font-size: 12px; font-weight: 500; cursor: pointer; color: var(--muted); transition: all 0.12s; }
  .btn-shuffle:hover { border-color: var(--text); color: var(--text); }
  .teams-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .team-block { padding: 14px; border-radius: var(--radius-sm); }
  .team-a { background: var(--blue-light); } .team-b { background: var(--accent-light); }
  .team-label { font-size: 10px; font-weight: 700; letter-spacing: 1px; margin-bottom: 10px; font-family: 'DM Mono', monospace; }
  .team-a .team-label { color: var(--blue); } .team-b .team-label { color: var(--accent); }
  .team-member { font-size: 13px; font-weight: 500; padding: 3px 0; }
  .team-a .team-member { color: var(--blue-text); } .team-b .team-member { color: var(--accent-text); }
  .teams-note { font-size: 12px; color: var(--muted); text-align: center; margin-top: 10px; }

  /* TOAST */
  .toast { position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%) translateY(20px); background: var(--text); color: var(--bg); padding: 10px 22px; border-radius: 999px; font-size: 13px; font-weight: 500; opacity: 0; transition: all 0.25s; pointer-events: none; z-index: 999; white-space: nowrap; max-width: 90vw; text-align: center; }
  .toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }

  /* SETUP NOTICE */
  .setup-notice { background: var(--warning-light); border: 1px solid var(--warning); border-radius: var(--radius-sm); padding: 12px 16px; font-size: 13px; color: var(--warning); margin-bottom: 1rem; line-height: 1.6; }
  .setup-notice strong { font-weight: 700; }
  .setup-notice code { font-family: 'DM Mono', monospace; font-size: 12px; background: rgba(0,0,0,0.07); padding: 1px 5px; border-radius: 4px; }
</style>
</head>
<body>
<div class="page">

  <!-- Setup notice (shown until EmailJS is configured) -->
  <div class="setup-notice" id="setupNotice">
    ⚙️ <strong>One-time setup needed:</strong> Open the Admin panel below → fill in your EmailJS keys and payment details. <a href="https://www.emailjs.com" target="_blank" style="color:inherit;font-weight:600;">Create a free EmailJS account</a>, add a service (Gmail/Outlook), create a template, and paste the IDs in. Everything else is automatic.
  </div>

  <div class="admin-toggle">
    <button class="btn-admin" onclick="toggleAdmin()">⚙️ Admin settings</button>
  </div>

  <!-- ADMIN PANEL -->
  <div class="admin-panel" id="adminPanel">
    <h3>⚙️ Admin settings</h3>
    <div class="admin-grid">

      <div class="section-label">EmailJS credentials</div>

      <div class="field full">
        <label>EmailJS Public Key <span class="req">*</span></label>
        <input type="text" id="cfgPublicKey" placeholder="e.g. user_xxxxxxxxxxxxxxxx" />
      </div>
      <div class="field">
        <label>Service ID <span class="req">*</span></label>
        <input type="text" id="cfgServiceId" placeholder="e.g. service_abc123" />
      </div>
      <div class="field">
        <label>Template ID <span class="req">*</span></label>
        <input type="text" id="cfgTemplateId" placeholder="e.g. template_xyz789" />
      </div>

      <div class="section-label">Email addresses</div>

      <div class="field">
        <label>Your (host) email <span class="req">*</span></label>
        <input type="email" id="cfgHostEmail" placeholder="you@email.com" />
      </div>
      <div class="field">
        <label>Reply-to name</label>
        <input type="text" id="cfgHostName" placeholder="e.g. Weekend Volleyball" />
      </div>

      <div class="section-label">Payment details</div>

      <div class="field">
        <label>Fee per player (Rs.) <span class="req">*</span></label>
        <input type="number" id="cfgFee" placeholder="e.g. 500" min="0" />
      </div>
      <div class="field">
        <label>JazzCash number</label>
        <input type="text" id="cfgJazzcash" placeholder="03xx-xxxxxxx" />
      </div>
      <div class="field">
        <label>NayaPay ID</label>
        <input type="text" id="cfgNayapay" placeholder="@username or number" />
      </div>
      <div class="field">
        <label>SadaPay ID</label>
        <input type="text" id="cfgSadapay" placeholder="@username or number" />
      </div>
      <div class="field full">
        <label>Bank account details</label>
        <input type="text" id="cfgBank" placeholder="Bank name · Account number · Account title" />
      </div>
      <div class="field full">
        <label>Payment deadline / note</label>
        <input type="text" id="cfgPayNote" placeholder="e.g. Please pay 24 hrs before the game" />
      </div>

      <div class="field full" style="margin-top:4px;">
        <button class="btn-signup" onclick="saveConfig()" style="background:var(--warning);">💾 Save settings</button>
      </div>
    </div>
  </div>

  <!-- HERO -->
  <div class="hero">
    <span class="hero-ball">🏐</span>
    <h1>Weekend Volleyball</h1>
    <p>Sign up below — a confirmation email will be sent to you.</p>
  </div>

  <!-- SIGNUP FORM -->
  <div class="card">
    <div class="card-head">
      <h2>Sign up to play</h2>
      <span class="badge" id="playerBadge">0 players</span>
    </div>
    <div class="form-grid">
      <div class="field">
        <label>Name <span class="req">*</span></label>
        <input type="text" id="fieldName" placeholder="Your full name" maxlength="40" autocomplete="off" />
        <span class="field-error" id="errName">Please enter your name</span>
      </div>
      <div class="field">
        <label>Email <span class="req">*</span></label>
        <input type="email" id="fieldEmail" placeholder="your@email.com" />
        <span class="field-error" id="errEmail">Please enter a valid email</span>
      </div>
      <div class="field">
        <label>Phone <span class="req">*</span></label>
        <input type="tel" id="fieldPhone" placeholder="03xx-xxxxxxx" maxlength="20" />
        <span class="field-error" id="errPhone">Please enter your number</span>
      </div>
      <div class="field">
        <label>Date <span class="req">*</span></label>
        <input type="date" id="fieldDate" />
        <span class="field-error" id="errDate">Please pick a date</span>
      </div>
    </div>
    <button class="btn-signup" id="signupBtn" onclick="addPlayer()">
      <span id="btnText">✓ Sign me up</span>
    </button>
  </div>

  <!-- PLAYER LIST -->
  <div class="card">
    <div class="card-head">
      <h2>Who's playing</h2>
    </div>
    <div class="player-list" id="playerList"></div>
    <div class="total-bar" id="totalBar" style="display:none;">
      <div class="total-count">
        <div class="total-dot"></div>
        <span id="totalText"></span>
      </div>
      <span class="total-amount" id="totalAmount"></span>
    </div>
  </div>

  <!-- TEAMS -->
  <div class="card teams-card" id="teamsCard">
    <div class="teams-head">
      <h2>🏆 Auto Teams</h2>
      <button class="btn-shuffle" onclick="shuffleTeams()">⟳ Reshuffle</button>
    </div>
    <div class="teams-grid" id="teamsGrid"></div>
    <p class="teams-note" id="teamsNote"></p>
  </div>

</div>
<div class="toast" id="toast"></div>

<script>
  const PALETTES = [
    {bg:'#FDE8E8',fg:'#7B1F1F'},{bg:'#E8F0FE',fg:'#1A3A6B'},{bg:'#E6F4EA',fg:'#1B5E20'},
    {bg:'#FFF3E0',fg:'#7C3C00'},{bg:'#F3E5F5',fg:'#4A1458'},{bg:'#E0F7FA',fg:'#00363A'},
    {bg:'#FFFDE7',fg:'#665C00'},{bg:'#FCE4EC',fg:'#6A0020'}
  ];

  const CFG_KEY = 'vb_config';
  const PLAYERS_KEY = 'vb_players_v3';

  function loadConfig() {
    try { return JSON.parse(localStorage.getItem(CFG_KEY)) || {}; } catch { return {}; }
  }
  function saveConfigData(cfg) {
    try { localStorage.setItem(CFG_KEY, JSON.stringify(cfg)); } catch {}
  }
  function loadPlayers() {
    try { return JSON.parse(localStorage.getItem(PLAYERS_KEY)) || []; } catch { return []; }
  }
  function savePlayers(p) {
    try { localStorage.setItem(PLAYERS_KEY, JSON.stringify(p)); } catch {}
  }

  let cfg = loadConfig();
  let players = loadPlayers();

  // Init EmailJS if configured
  function initEmailJS() {
    if (cfg.publicKey) {
      emailjs.init({ publicKey: cfg.publicKey });
    }
  }
  initEmailJS();

  // Populate admin fields
  function populateAdmin() {
    const fields = ['publicKey','serviceId','templateId','hostEmail','hostName','fee','jazzcash','nayapay','sadapay','bank','payNote'];
    fields.forEach(f => {
      const el = document.getElementById('cfg' + f.charAt(0).toUpperCase() + f.slice(1));
      if (el && cfg[f]) el.value = cfg[f];
    });
    if (cfg.publicKey && cfg.serviceId && cfg.templateId && cfg.hostEmail && cfg.fee) {
      document.getElementById('setupNotice').style.display = 'none';
    }
  }
  populateAdmin();

  function saveConfig() {
    cfg = {
      publicKey: document.getElementById('cfgPublicKey').value.trim(),
      serviceId: document.getElementById('cfgServiceId').value.trim(),
      templateId: document.getElementById('cfgTemplateId').value.trim(),
      hostEmail: document.getElementById('cfgHostEmail').value.trim(),
      hostName: document.getElementById('cfgHostName').value.trim() || 'Weekend Volleyball',
      fee: document.getElementById('cfgFee').value.trim(),
      jazzcash: document.getElementById('cfgJazzcash').value.trim(),
      nayapay: document.getElementById('cfgNayapay').value.trim(),
      sadapay: document.getElementById('cfgSadapay').value.trim(),
      bank: document.getElementById('cfgBank').value.trim(),
      payNote: document.getElementById('cfgPayNote').value.trim(),
    };
    if (!cfg.publicKey || !cfg.serviceId || !cfg.templateId || !cfg.hostEmail || !cfg.fee) {
      toast('Please fill in all required fields ✋'); return;
    }
    saveConfigData(cfg);
    initEmailJS();
    document.getElementById('setupNotice').style.display = 'none';
    toggleAdmin();
    toast('Settings saved! ✓');
    render();
  }

  function toggleAdmin() {
    document.getElementById('adminPanel').classList.toggle('show');
  }

  // Helpers
  function initials(n) { return n.trim().split(/\s+/).map(p=>p[0]).join('').toUpperCase().slice(0,2); }
  function palette(n) { let h=0; for(const c of n) h=(h*31+c.charCodeAt(0))&0xffff; return PALETTES[h%PALETTES.length]; }
  function esc(s) { return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }
  function formatDate(v) {
    if (!v) return '';
    const d = new Date(v + 'T00:00:00');
    return d.toLocaleDateString('en-US', { weekday:'short', month:'short', day:'numeric', year:'numeric' });
  }

  // Validation
  function validate() {
    const name = document.getElementById('fieldName').value.trim();
    const email = document.getElementById('fieldEmail').value.trim();
    const phone = document.getElementById('fieldPhone').value.trim();
    const date = document.getElementById('fieldDate').value;
    let valid = true;
    const checks = [
      { id:'fieldName', errId:'errName', ok: name.length > 0 },
      { id:'fieldEmail', errId:'errEmail', ok: /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email) },
      { id:'fieldPhone', errId:'errPhone', ok: phone.length > 0 },
      { id:'fieldDate', errId:'errDate', ok: date.length > 0 },
    ];
    checks.forEach(c => {
      document.getElementById(c.id).classList.toggle('error', !c.ok);
      document.getElementById(c.errId).classList.toggle('show', !c.ok);
      if (!c.ok) valid = false;
    });
    return valid ? { name, email, phone, date } : null;
  }

  ['fieldName','fieldEmail','fieldPhone','fieldDate'].forEach(id => {
    document.getElementById(id).addEventListener('input', () => {
      document.getElementById(id).classList.remove('error');
    });
  });

  // Build payment instructions string
  function buildPaymentInstructions() {
    const lines = [];
    if (cfg.jazzcash) lines.push('JazzCash: ' + cfg.jazzcash);
    if (cfg.nayapay) lines.push('NayaPay: ' + cfg.nayapay);
    if (cfg.sadapay) lines.push('SadaPay: ' + cfg.sadapay);
    if (cfg.bank) lines.push('Bank: ' + cfg.bank);
    return lines.join('\n');
  }

  async function sendEmails(player) {
    if (!cfg.publicKey || !cfg.serviceId || !cfg.templateId) return;

    const payInstructions = buildPaymentInstructions();
    const feeDisplay = 'Rs. ' + Number(cfg.fee).toLocaleString();
    const dateDisplay = formatDate(player.date);

    const commonParams = {
      player_name: player.name,
      player_email: player.email,
      player_phone: player.phone,
      game_date: dateDisplay,
      fee: feeDisplay,
      payment_instructions: payInstructions,
      pay_note: cfg.payNote || '',
      host_name: cfg.hostName,
      host_email: cfg.hostEmail,
    };

    // Email to player
    await emailjs.send(cfg.serviceId, cfg.templateId, {
      ...commonParams,
      to_email: player.email,
      to_name: player.name,
      email_type: 'player_confirmation',
      subject: 'You\'re signed up for Weekend Volleyball — ' + dateDisplay,
    });

    // Email to host
    await emailjs.send(cfg.serviceId, cfg.templateId, {
      ...commonParams,
      to_email: cfg.hostEmail,
      to_name: cfg.hostName,
      email_type: 'host_notification',
      subject: 'New signup: ' + player.name + ' — ' + dateDisplay,
    });
  }

  async function addPlayer() {
    const data = validate();
    if (!data) { toast('Please fill in all fields ✋'); return; }

    if (!cfg.fee) {
      toast('Please configure settings first ⚙️'); return;
    }

    if (players.find(p => p.email.toLowerCase() === data.email.toLowerCase() && p.date === data.date)) {
      toast('This email is already signed up for this date!'); return;
    }

    const btn = document.getElementById('signupBtn');
    const btnText = document.getElementById('btnText');
    btn.disabled = true;
    btnText.textContent = '⏳ Sending confirmation...';

    const player = { ...data, paid: false, addedAt: new Date().toLocaleTimeString('en-US',{hour:'numeric',minute:'2-digit'}) };
    players.push(player);
    savePlayers(players);

    try {
      await sendEmails(player);
      toast('Signed up! Confirmation email sent 📧');
    } catch (e) {
      console.error(e);
      toast('Signed up! (Email failed — check EmailJS settings)');
    }

    btn.disabled = false;
    btnText.textContent = '✓ Sign me up';

    document.getElementById('fieldName').value = '';
    document.getElementById('fieldEmail').value = '';
    document.getElementById('fieldPhone').value = '';
    document.getElementById('fieldDate').value = '';
    document.getElementById('fieldName').focus();

    render();
  }

  function togglePaid(i) {
    players[i].paid = !players[i].paid;
    savePlayers(players);
    render();
    toast(players[i].name + (players[i].paid ? ' marked as paid ✓' : ' marked as unpaid'));
  }

  function removePlayer(i) {
    const name = players[i].name;
    players.splice(i, 1);
    savePlayers(players);
    render();
    toast(name + ' removed');
  }

  function render() {
    const list = document.getElementById('playerList');
    const badge = document.getElementById('playerBadge');
    const totalBar = document.getElementById('totalBar');
    const totalText = document.getElementById('totalText');
    const totalAmount = document.getElementById('totalAmount');
    const fee = Number(cfg.fee) || 0;

    badge.textContent = players.length + ' player' + (players.length !== 1 ? 's' : '');

    if (players.length === 0) {
      list.innerHTML = `<div class="empty"><span class="emoji">👋</span>No one signed up yet — be first!</div>`;
      totalBar.style.display = 'none';
    } else {
      list.innerHTML = players.map((p, i) => {
        const col = palette(p.name);
        return `<div class="player-row">
          <span class="player-num">${i + 1}</span>
          <div class="avatar" style="background:${col.bg};color:${col.fg}">${initials(p.name)}</div>
          <div class="player-info">
            <div class="player-name">${esc(p.name)}</div>
            <div class="player-meta">${esc(p.phone)} · ${esc(p.email)}</div>
          </div>
          <span class="date-badge">${esc(formatDate(p.date))}</span>
          <button class="paid-badge ${p.paid ? 'paid' : 'unpaid'}" onclick="togglePaid(${i})" title="Click to toggle payment status">
            ${p.paid ? '✓ Paid' : '✗ Unpaid'}
          </button>
          <button class="btn-remove" onclick="removePlayer(${i})" aria-label="Remove ${esc(p.name)}">✕</button>
        </div>`;
      }).join('');

      const paidCount = players.filter(p => p.paid).length;
      const collected = paidCount * fee;
      const total = players.length * fee;
      totalBar.style.display = 'flex';
      totalText.textContent = players.length + ' signed up · ' + paidCount + ' paid';
      if (fee > 0) totalAmount.textContent = 'Rs. ' + collected.toLocaleString() + ' / Rs. ' + total.toLocaleString() + ' collected';
      else totalAmount.textContent = '';
    }

    renderTeams();
  }

  function renderTeams() {
    const card = document.getElementById('teamsCard');
    if (players.length < 2) { card.classList.remove('show'); return; }
    card.classList.add('show');
    const grid = document.getElementById('teamsGrid');
    const note = document.getElementById('teamsNote');
    const half = Math.ceil(players.length / 2);
    const a = players.slice(0, half), b = players.slice(half);
    grid.innerHTML = `
      <div class="team-block team-a">
        <div class="team-label">TEAM A</div>
        ${a.map(p=>`<div class="team-member">• ${esc(p.name)}</div>`).join('')}
      </div>
      <div class="team-block team-b">
        <div class="team-label">TEAM B</div>
        ${b.map(p=>`<div class="team-member">• ${esc(p.name)}</div>`).join('')}
      </div>`;
    note.textContent = players.length % 2 !== 0 ? 'Odd number — one player rotates in each round' : '';
  }

  function shuffleTeams() {
    for (let i = players.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [players[i], players[j]] = [players[j], players[i]];
    }
    render(); toast('Teams reshuffled! 🎲');
  }

  let toastTimer;
  function toast(msg) {
    const el = document.getElementById('toast');
    el.textContent = msg; el.classList.add('show');
    clearTimeout(toastTimer);
    toastTimer = setTimeout(() => el.classList.remove('show'), 2500);
  }

  render();
</script>
</body>
</html>
