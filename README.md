<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>NovBank — Modern Banking</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet"/>
<style>
  :root {
    --bg: #0a0d14;
    --surface: #111520;
    --surface2: #181d2e;
    --border: rgba(255,255,255,0.07);
    --accent: #c8f04e;
    --accent2: #7dd3fc;
    --text: #e8eaf0;
    --muted: #6b7280;
    --danger: #f87171;
    --success: #4ade80;
    --card-radius: 20px;
    --transition: cubic-bezier(0.4, 0, 0.2, 1);
  }

  * { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* ── NOISE OVERLAY ── */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='0.04'/%3E%3C/svg%3E");
    pointer-events: none;
    z-index: 999;
    opacity: 0.4;
  }

  /* ── PAGES ── */
  .page { display: none; min-height: 100vh; animation: fadeIn 0.5s var(--transition) forwards; }
  .page.active { display: flex; }
  @keyframes fadeIn { from { opacity:0; transform: translateY(12px); } to { opacity:1; transform: translateY(0); } }
  @keyframes slideUp { from { opacity:0; transform: translateY(30px); } to { opacity:1; transform: translateY(0); } }
  @keyframes scaleIn { from { opacity:0; transform: scale(0.93); } to { opacity:1; transform: scale(1); } }

  /* ══════════════════════════════
     AUTH PAGE
  ══════════════════════════════ */
  #auth-page {
    flex-direction: column;
    align-items: center;
    justify-content: center;
    position: relative;
    padding: 24px;
  }

  .auth-glow {
    position: fixed;
    width: 600px; height: 600px;
    border-radius: 50%;
    filter: blur(120px);
    pointer-events: none;
  }
  .glow-1 { background: rgba(200,240,78,0.08); top:-200px; left:-200px; }
  .glow-2 { background: rgba(125,211,252,0.06); bottom:-200px; right:-200px; }

  .auth-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 28px;
    padding: 48px 44px;
    width: 100%;
    max-width: 440px;
    position: relative;
    z-index: 1;
    animation: scaleIn 0.5s var(--transition);
  }

  .brand {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 36px;
  }
  .brand-icon {
    width: 40px; height: 40px;
    background: var(--accent);
    border-radius: 12px;
    display: grid;
    place-items: center;
  }
  .brand-icon svg { color: #0a0d14; }
  .brand-name {
    font-family: 'DM Serif Display', serif;
    font-size: 22px;
    color: var(--text);
    letter-spacing: -0.3px;
  }

  .auth-title {
    font-family: 'DM Serif Display', serif;
    font-size: 30px;
    line-height: 1.2;
    margin-bottom: 8px;
    color: var(--text);
  }
  .auth-sub { font-size: 14px; color: var(--muted); margin-bottom: 32px; }

  .tab-group {
    display: flex;
    background: var(--bg);
    border-radius: 12px;
    padding: 4px;
    margin-bottom: 28px;
  }
  .tab-btn {
    flex: 1;
    padding: 9px;
    border: none;
    border-radius: 9px;
    background: transparent;
    color: var(--muted);
    font-family: 'DM Sans', sans-serif;
    font-size: 14px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.25s var(--transition);
  }
  .tab-btn.active {
    background: var(--surface2);
    color: var(--text);
    box-shadow: 0 1px 4px rgba(0,0,0,0.4);
  }

  .form-group { margin-bottom: 18px; }
  .form-label {
    display: block;
    font-size: 12px;
    font-weight: 600;
    color: var(--muted);
    letter-spacing: 0.06em;
    text-transform: uppercase;
    margin-bottom: 8px;
  }
  .form-input {
    width: 100%;
    background: var(--bg);
    border: 1.5px solid var(--border);
    border-radius: 12px;
    padding: 13px 16px;
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-size: 15px;
    outline: none;
    transition: border-color 0.2s;
  }
  .form-input:focus { border-color: var(--accent); }
  .form-input::placeholder { color: var(--muted); }

  .btn-primary {
    width: 100%;
    padding: 15px;
    background: var(--accent);
    color: #0a0d14;
    border: none;
    border-radius: 14px;
    font-family: 'DM Sans', sans-serif;
    font-size: 15px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s var(--transition);
    letter-spacing: 0.01em;
    margin-top: 4px;
    position: relative;
    overflow: hidden;
  }
  .btn-primary::after {
    content: '';
    position: absolute;
    inset: 0;
    background: white;
    opacity: 0;
    transition: opacity 0.2s;
  }
  .btn-primary:hover::after { opacity: 0.08; }
  .btn-primary:active { transform: scale(0.98); }

  .auth-error {
    background: rgba(248,113,113,0.1);
    border: 1px solid rgba(248,113,113,0.25);
    color: var(--danger);
    border-radius: 10px;
    padding: 11px 14px;
    font-size: 13px;
    margin-bottom: 16px;
    display: none;
  }
  .auth-success {
    background: rgba(74,222,128,0.1);
    border: 1px solid rgba(74,222,128,0.25);
    color: var(--success);
    border-radius: 10px;
    padding: 11px 14px;
    font-size: 13px;
    margin-bottom: 16px;
    display: none;
  }

  /* ══════════════════════════════
     DASHBOARD LAYOUT
  ══════════════════════════════ */
  #dashboard-page {
    flex-direction: row;
    min-height: 100vh;
  }

  /* Sidebar */
  .sidebar {
    width: 240px;
    min-width: 240px;
    background: var(--surface);
    border-right: 1px solid var(--border);
    display: flex;
    flex-direction: column;
    padding: 28px 20px;
    position: sticky;
    top: 0;
    height: 100vh;
  }
  .sidebar .brand { margin-bottom: 40px; }

  .nav-item {
    display: flex;
    align-items: center;
    gap: 12px;
    padding: 12px 14px;
    border-radius: 12px;
    color: var(--muted);
    font-size: 14px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s var(--transition);
    margin-bottom: 4px;
    border: none;
    background: transparent;
    width: 100%;
    text-align: left;
  }
  .nav-item svg { flex-shrink: 0; }
  .nav-item:hover { background: var(--surface2); color: var(--text); }
  .nav-item.active { background: rgba(200,240,78,0.1); color: var(--accent); }
  .nav-item.active svg { stroke: var(--accent); }

  .nav-spacer { flex: 1; }

  .user-info {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 12px 14px;
    background: var(--surface2);
    border-radius: 12px;
  }
  .avatar {
    width: 34px; height: 34px;
    border-radius: 50%;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    display: grid;
    place-items: center;
    font-size: 13px;
    font-weight: 700;
    color: #0a0d14;
    flex-shrink: 0;
  }
  .user-name { font-size: 13px; font-weight: 600; }
  .user-role { font-size: 11px; color: var(--muted); }

  /* Main content */
  .main-content {
    flex: 1;
    overflow-y: auto;
    padding: 36px 40px;
  }

  /* ── PANELS ── */
  .panel { display: none; animation: slideUp 0.4s var(--transition) forwards; }
  .panel.active { display: block; }

  .panel-header {
    margin-bottom: 32px;
  }
  .panel-header h1 {
    font-family: 'DM Serif Display', serif;
    font-size: 28px;
    margin-bottom: 4px;
  }
  .panel-header p { color: var(--muted); font-size: 14px; }

  /* ── OVERVIEW PANEL ── */
  .balance-hero {
    background: linear-gradient(135deg, #1a2040 0%, #0f1525 100%);
    border: 1px solid var(--border);
    border-radius: var(--card-radius);
    padding: 36px;
    margin-bottom: 28px;
    position: relative;
    overflow: hidden;
  }
  .balance-hero::before {
    content: '';
    position: absolute;
    width: 300px; height: 300px;
    background: rgba(200,240,78,0.06);
    border-radius: 50%;
    top: -100px; right: -60px;
    filter: blur(60px);
  }
  .balance-label { font-size: 12px; font-weight: 600; color: var(--muted); text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 12px; }
  .balance-amount {
    font-family: 'DM Serif Display', serif;
    font-size: 52px;
    color: var(--text);
    letter-spacing: -1px;
    line-height: 1;
    margin-bottom: 20px;
  }
  .balance-amount span { color: var(--accent); }
  .balance-meta { display: flex; gap: 24px; flex-wrap: wrap; }
  .balance-chip {
    background: rgba(255,255,255,0.05);
    border-radius: 8px;
    padding: 8px 14px;
    font-size: 12px;
    color: var(--muted);
  }
  .balance-chip strong { color: var(--text); display: block; font-size: 15px; font-weight: 600; }

  .cards-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 16px;
    margin-bottom: 28px;
  }
  .stat-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 22px;
    transition: transform 0.2s var(--transition), border-color 0.2s;
  }
  .stat-card:hover { transform: translateY(-2px); border-color: rgba(255,255,255,0.12); }
  .stat-icon {
    width: 38px; height: 38px;
    border-radius: 10px;
    display: grid;
    place-items: center;
    margin-bottom: 14px;
  }
  .stat-card-label { font-size: 12px; color: var(--muted); margin-bottom: 6px; }
  .stat-card-value { font-size: 22px; font-weight: 600; }

  /* transactions */
  .section-title {
    font-size: 16px;
    font-weight: 600;
    margin-bottom: 16px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .section-title a { font-size: 12px; color: var(--accent); text-decoration: none; font-weight: 500; }

  .tx-list { display: flex; flex-direction: column; gap: 10px; }
  .tx-item {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 16px 20px;
    display: flex;
    align-items: center;
    gap: 16px;
    transition: border-color 0.2s;
  }
  .tx-item:hover { border-color: rgba(255,255,255,0.12); }
  .tx-icon {
    width: 42px; height: 42px;
    border-radius: 12px;
    display: grid;
    place-items: center;
    flex-shrink: 0;
  }
  .tx-info { flex: 1; }
  .tx-name { font-size: 14px; font-weight: 500; margin-bottom: 3px; }
  .tx-date { font-size: 12px; color: var(--muted); }
  .tx-amount { font-size: 15px; font-weight: 600; }
  .tx-amount.pos { color: var(--success); }
  .tx-amount.neg { color: var(--danger); }

  /* ── DEPOSIT PANEL ── */
  .deposit-layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 24px;
  }

  @media (max-width: 900px) {
    .deposit-layout { grid-template-columns: 1fr; }
  }

  .deposit-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--card-radius);
    padding: 32px;
  }
  .deposit-card h2 {
    font-family: 'DM Serif Display', serif;
    font-size: 22px;
    margin-bottom: 6px;
  }
  .deposit-card .sub { font-size: 13px; color: var(--muted); margin-bottom: 28px; }

  .amount-input-wrap {
    position: relative;
    margin-bottom: 20px;
  }
  .currency-symbol {
    position: absolute;
    left: 16px;
    top: 50%;
    transform: translateY(-50%);
    font-size: 20px;
    font-weight: 600;
    color: var(--accent);
  }
  .amount-input {
    width: 100%;
    background: var(--bg);
    border: 2px solid var(--border);
    border-radius: 14px;
    padding: 18px 16px 18px 36px;
    font-size: 28px;
    font-weight: 600;
    color: var(--text);
    font-family: 'DM Serif Display', serif;
    outline: none;
    transition: border-color 0.2s;
  }
  .amount-input:focus { border-color: var(--accent); }

  .quick-amounts {
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    margin-bottom: 20px;
  }
  .quick-btn {
    padding: 8px 16px;
    background: var(--bg);
    border: 1.5px solid var(--border);
    border-radius: 10px;
    color: var(--text);
    font-size: 13px;
    font-weight: 500;
    cursor: pointer;
    font-family: 'DM Sans', sans-serif;
    transition: all 0.2s;
  }
  .quick-btn:hover { border-color: var(--accent); color: var(--accent); }

  .btn-deposit {
    width: 100%;
    padding: 15px;
    background: var(--accent);
    color: #0a0d14;
    border: none;
    border-radius: 14px;
    font-family: 'DM Sans', sans-serif;
    font-size: 15px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s var(--transition);
    margin-top: 8px;
    position: relative;
    overflow: hidden;
  }
  .btn-deposit:hover { opacity: 0.9; transform: translateY(-1px); }
  .btn-deposit:active { transform: scale(0.98); }

  .balance-display {
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 24px;
    margin-bottom: 20px;
    text-align: center;
  }
  .balance-display-label { font-size: 12px; color: var(--muted); text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 10px; }
  .balance-display-amount {
    font-family: 'DM Serif Display', serif;
    font-size: 42px;
    color: var(--accent);
    letter-spacing: -0.5px;
  }

  .deposit-history { margin-top: 20px; }
  .dep-item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 13px 0;
    border-bottom: 1px solid var(--border);
  }
  .dep-item:last-child { border-bottom: none; }
  .dep-left { font-size: 14px; }
  .dep-date { font-size: 11px; color: var(--muted); margin-top: 2px; }
  .dep-amt { font-size: 15px; font-weight: 600; color: var(--success); }

  /* toast */
  .toast {
    position: fixed;
    bottom: 32px; left: 50%; transform: translateX(-50%) translateY(80px);
    background: var(--surface2);
    border: 1px solid rgba(74,222,128,0.3);
    color: var(--success);
    border-radius: 14px;
    padding: 14px 24px;
    font-size: 14px;
    font-weight: 500;
    z-index: 1000;
    transition: transform 0.4s var(--transition);
    white-space: nowrap;
  }
  .toast.show { transform: translateX(-50%) translateY(0); }

  /* ── WITHDRAW PANEL ── */
  .btn-withdraw {
    width: 100%;
    padding: 15px;
    background: transparent;
    border: 2px solid var(--danger);
    color: var(--danger);
    border-radius: 14px;
    font-family: 'DM Sans', sans-serif;
    font-size: 15px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
    margin-top: 8px;
  }
  .btn-withdraw:hover { background: rgba(248,113,113,0.08); }
  .btn-withdraw:active { transform: scale(0.98); }

  /* ── SCROLLBAR ── */
  ::-webkit-scrollbar { width: 6px; }
  ::-webkit-scrollbar-track { background: transparent; }
  ::-webkit-scrollbar-thumb { background: var(--surface2); border-radius: 10px; }

  /* ── RESPONSIVE ── */
  @media (max-width: 768px) {
    .sidebar { display: none; }
    .main-content { padding: 24px 20px; }
    .balance-amount { font-size: 36px; }
    .deposit-layout { grid-template-columns: 1fr; }
  }
</style>
</head>
<body>

<!-- ════════════════════════ AUTH PAGE ════════════════════════ -->
<div id="auth-page" class="page active">
  <div class="auth-glow glow-1"></div>
  <div class="auth-glow glow-2"></div>
  <div class="auth-card">
    <div class="brand">
      <div class="brand-icon">
        <svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24">
          <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/>
          <polyline points="9 22 9 12 15 12 15 22"/>
        </svg>
      </div>
      <div class="brand-name">NovBank</div>
    </div>

    <div class="tab-group">
      <button class="tab-btn active" onclick="switchTab('login')">Sign In</button>
      <button class="tab-btn" onclick="switchTab('register')">Create Account</button>
    </div>

    <!-- Login Form -->
    <div id="login-form">
      <h2 class="auth-title">Welcome back</h2>
      <p class="auth-sub">Sign in to your NovBank account</p>
      <div id="login-error" class="auth-error"></div>
      <div class="form-group">
        <label class="form-label">Email</label>
        <input class="form-input" type="email" id="login-email" placeholder="you@example.com"/>
      </div>
      <div class="form-group">
        <label class="form-label">Password</label>
        <input class="form-input" type="password" id="login-pass" placeholder="••••••••"/>
      </div>
      <button class="btn-primary" onclick="handleLogin()">Sign In →</button>
    </div>

    <!-- Register Form -->
    <div id="register-form" style="display:none">
      <h2 class="auth-title">Open an account</h2>
      <p class="auth-sub">Join NovBank — takes less than a minute</p>
      <div id="reg-error" class="auth-error"></div>
      <div id="reg-success" class="auth-success"></div>
      <div class="form-group">
        <label class="form-label">Full Name</label>
        <input class="form-input" type="text" id="reg-name" placeholder="Full Name"/>
      </div>
      <div class="form-group">
        <label class="form-label">Email</label>
        <input class="form-input" type="email" id="reg-email" placeholder="you@example.com"/>
      </div>
      <div class="form-group">
        <label class="form-label">Password</label>
        <input class="form-input" type="password" id="reg-pass" placeholder="Min. 6 characters"/>
      </div>
      <div class="form-group">
        <label class="form-label">Confirm Password</label>
        <input class="form-input" type="password" id="reg-pass2" placeholder="Repeat password"/>
      </div>
      <button class="btn-primary" onclick="handleRegister()">Create Account →</button>
    </div>
  </div>
</div>

<!-- ════════════════════════ DASHBOARD PAGE ════════════════════════ -->
<div id="dashboard-page" class="page">
  <!-- Sidebar -->
  <aside class="sidebar">
    <div class="brand">
      <div class="brand-icon">
        <svg width="20" height="20" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24">
          <path d="M3 9l9-7 9 7v11a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/>
          <polyline points="9 22 9 12 15 12 15 22"/>
        </svg>
      </div>
      <div class="brand-name">NovBank</div>
    </div>

    <button class="nav-item active" onclick="switchPanel('overview', this)">
      <svg width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
        <rect x="3" y="3" width="7" height="7" rx="1"/><rect x="14" y="3" width="7" height="7" rx="1"/>
        <rect x="14" y="14" width="7" height="7" rx="1"/><rect x="3" y="14" width="7" height="7" rx="1"/>
      </svg>
      Overview
    </button>
    <button class="nav-item" onclick="switchPanel('deposit', this)">
      <svg width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
        <circle cx="12" cy="12" r="9"/><line x1="12" y1="8" x2="12" y2="16"/><line x1="8" y1="12" x2="16" y2="12"/>
      </svg>
      Deposit
    </button>
    <button class="nav-item" onclick="switchPanel('withdraw', this)">
      <svg width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
        <circle cx="12" cy="12" r="9"/><line x1="8" y1="12" x2="16" y2="12"/>
      </svg>
      Withdraw
    </button>
    <button class="nav-item" onclick="switchPanel('history', this)">
      <svg width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
        <polyline points="22 12 18 12 15 21 9 3 6 12 2 12"/>
      </svg>
      Transactions
    </button>

    <div class="nav-spacer"></div>

    <div class="user-info">
      <div class="avatar" id="user-avatar">JD</div>
      <div>
        <div class="user-name" id="user-display-name">Jane Doe</div>
        <div class="user-role">Personal Account</div>
      </div>
    </div>

    <button class="nav-item" style="margin-top:10px" onclick="handleLogout()">
      <svg width="17" height="17" fill="none" stroke="currentColor" stroke-width="2" viewBox="0 0 24 24">
        <path d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"/>
        <polyline points="16 17 21 12 16 7"/><line x1="21" y1="12" x2="9" y2="12"/>
      </svg>
      Sign Out
    </button>
  </aside>

  <!-- Main -->
  <main class="main-content">

    <!-- OVERVIEW -->
    <div id="panel-overview" class="panel active">
      <div class="panel-header">
        <h1>Good morning, <span id="greet-name">Jane</span> 👋</h1>
        <p>Here's what's happening with your account today.</p>
      </div>

      <div class="balance-hero">
        <div class="balance-label">Total Balance</div>
        <div class="balance-amount" id="hero-balance"><span>$</span>0.00</div>
        <div class="balance-meta">
          <div class="balance-chip">
            <strong id="chip-deposits">$0.00</strong>
            Total Deposited
          </div>
          <div class="balance-chip">
            <strong id="chip-withdrawn">$0.00</strong>
            Total Withdrawn
          </div>
          <div class="balance-chip">
            <strong id="chip-txcount">0</strong>
            Transactions
          </div>
        </div>
      </div>

      <div class="cards-grid">
        <div class="stat-card">
          <div class="stat-icon" style="background:rgba(200,240,78,0.1)">
            <svg width="18" height="18" fill="none" stroke="#c8f04e" stroke-width="2" viewBox="0 0 24 24">
              <polyline points="23 6 13.5 15.5 8.5 10.5 1 18"/><polyline points="17 6 23 6 23 12"/>
            </svg>
          </div>
          <div class="stat-card-label">Last Deposit</div>
          <div class="stat-card-value" id="last-deposit-val">—</div>
        </div>
        <div class="stat-card">
          <div class="stat-icon" style="background:rgba(248,113,113,0.1)">
            <svg width="18" height="18" fill="none" stroke="#f87171" stroke-width="2" viewBox="0 0 24 24">
              <polyline points="23 18 13.5 8.5 8.5 13.5 1 6"/><polyline points="17 18 23 18 23 12"/>
            </svg>
          </div>
          <div class="stat-card-label">Last Withdrawal</div>
          <div class="stat-card-value" id="last-withdraw-val">—</div>
        </div>
        <div class="stat-card">
          <div class="stat-icon" style="background:rgba(125,211,252,0.1)">
            <svg width="18" height="18" fill="none" stroke="#7dd3fc" stroke-width="2" viewBox="0 0 24 24">
              <rect x="2" y="3" width="20" height="14" rx="2"/><line x1="8" y1="21" x2="16" y2="21"/>
              <line x1="12" y1="17" x2="12" y2="21"/>
            </svg>
          </div>
          <div class="stat-card-label">Account Number</div>
          <div class="stat-card-value" id="acct-number" style="font-size:15px;letter-spacing:1px">—</div>
        </div>
      </div>

      <div class="section-title">
        Recent Transactions
        <a href="#" onclick="switchPanel('history', document.querySelector('.nav-item:nth-child(4)'));return false">View all →</a>
      </div>
      <div class="tx-list" id="overview-tx-list">
        <div style="color:var(--muted);font-size:14px;padding:20px 0;text-align:center">No transactions yet. Make your first deposit!</div>
      </div>
    </div>

    <!-- DEPOSIT -->
    <div id="panel-deposit" class="panel">
      <div class="panel-header">
        <h1>Deposit Funds</h1>
        <p>Add money to your NovBank account instantly.</p>
      </div>
      <div class="deposit-layout">
        <div class="deposit-card">
          <h2>Make a Deposit</h2>
          <p class="sub">Enter the amount you'd like to add</p>

          <div class="amount-input-wrap">
            <span class="currency-symbol">$</span>
            <input class="amount-input" type="number" id="deposit-amount" placeholder="0.00" min="1"/>
          </div>

          <div class="quick-amounts">
            <button class="quick-btn" onclick="setAmount(100)">$100</button>
            <button class="quick-btn" onclick="setAmount(500)">$500</button>
            <button class="quick-btn" onclick="setAmount(1000)">$1,000</button>
            <button class="quick-btn" onclick="setAmount(5000)">$5,000</button>
          </div>

          <div class="form-group">
            <label class="form-label">Note (optional)</label>
            <input class="form-input" type="text" id="deposit-note" placeholder="e.g. Monthly savings"/>
          </div>

          <button class="btn-deposit" onclick="handleDeposit()">
            Deposit Funds ↑
          </button>
        </div>

        <div class="deposit-card">
          <h2>Current Balance</h2>
          <p class="sub">Your available funds</p>
          <div class="balance-display">
            <div class="balance-display-label">Available Balance</div>
            <div class="balance-display-amount" id="deposit-panel-balance">$0.00</div>
          </div>

          <div class="section-title" style="margin-top:8px">Recent Deposits</div>
          <div id="deposit-history" class="deposit-history">
            <div style="color:var(--muted);font-size:13px;padding:12px 0;text-align:center">No deposits yet</div>
          </div>
        </div>
      </div>
    </div>

    <!-- WITHDRAW -->
    <div id="panel-withdraw" class="panel">
      <div class="panel-header">
        <h1>Withdraw Funds</h1>
        <p>Transfer money out of your NovBank account.</p>
      </div>
      <div class="deposit-layout">
        <div class="deposit-card">
          <h2>Withdraw Funds</h2>
          <p class="sub">Enter the amount to withdraw</p>

          <div class="amount-input-wrap">
            <span class="currency-symbol" style="color:var(--danger)">$</span>
            <input class="amount-input" type="number" id="withdraw-amount" placeholder="0.00" min="1" style="border-color:rgba(248,113,113,0.2)"/>
          </div>

          <div class="quick-amounts">
            <button class="quick-btn" onclick="setWithdrawAmount(50)">$50</button>
            <button class="quick-btn" onclick="setWithdrawAmount(200)">$200</button>
            <button class="quick-btn" onclick="setWithdrawAmount(500)">$500</button>
            <button class="quick-btn" onclick="setWithdrawAmount(1000)">$1,000</button>
          </div>

          <div class="form-group">
            <label class="form-label">Reason (optional)</label>
            <input class="form-input" type="text" id="withdraw-note" placeholder="e.g. Rent payment"/>
          </div>

          <button class="btn-withdraw" onclick="handleWithdraw()">Withdraw Funds ↓</button>
        </div>

        <div class="deposit-card">
          <h2>Current Balance</h2>
          <p class="sub">Funds available for withdrawal</p>
          <div class="balance-display">
            <div class="balance-display-label">Available Balance</div>
            <div class="balance-display-amount" id="withdraw-panel-balance" style="color:var(--text)">$0.00</div>
          </div>

          <div class="section-title" style="margin-top:8px">Recent Withdrawals</div>
          <div id="withdraw-history" class="deposit-history">
            <div style="color:var(--muted);font-size:13px;padding:12px 0;text-align:center">No withdrawals yet</div>
          </div>
        </div>
      </div>
    </div>

    <!-- HISTORY -->
    <div id="panel-history" class="panel">
      <div class="panel-header">
        <h1>Transaction History</h1>
        <p>A complete record of all your account activity.</p>
      </div>
      <div class="tx-list" id="full-tx-list">
        <div style="color:var(--muted);font-size:14px;padding:40px 0;text-align:center">No transactions yet. Make your first deposit!</div>
      </div>
    </div>

  </main>
</div>

<!-- Toast -->
<div class="toast" id="toast">✓ Deposit successful!</div>

<script>
// ── STATE ──────────────────────────────────────────
const STORAGE_KEY = 'novbank_data';

function loadData() {
  try { return JSON.parse(localStorage.getItem(STORAGE_KEY)) || { users: [], currentUser: null }; }
  catch { return { users: [], currentUser: null }; }
}
function saveData(d) { localStorage.setItem(STORAGE_KEY, JSON.stringify(d)); }

function getUser(email) {
  const d = loadData();
  return d.users.find(u => u.email === email);
}

function currentUser() {
  const d = loadData();
  if (!d.currentUser) return null;
  return d.users.find(u => u.email === d.currentUser);
}

function updateUser(updatedUser) {
  const d = loadData();
  d.users = d.users.map(u => u.email === updatedUser.email ? updatedUser : u);
  saveData(d);
}

function fmtMoney(n) {
  return '$' + n.toLocaleString('en-US', { minimumFractionDigits: 2, maximumFractionDigits: 2 });
}

function fmtDate(iso) {
  const d = new Date(iso);
  return d.toLocaleDateString('en-US', { month: 'short', day: 'numeric', year: 'numeric', hour:'2-digit', minute:'2-digit' });
}

function genAccountNumber() {
  return 'NOV-' + Math.floor(100000 + Math.random() * 900000);
}

function initUser(name, email) {
  return { name, email, accountNumber: genAccountNumber(), balance: 0, transactions: [] };
}

// ── AUTH ───────────────────────────────────────────
function switchTab(tab) {
  document.querySelectorAll('.tab-btn').forEach((b,i) => {
    b.classList.toggle('active', (i===0 && tab==='login') || (i===1 && tab==='register'));
  });
  document.getElementById('login-form').style.display = tab==='login' ? '' : 'none';
  document.getElementById('register-form').style.display = tab==='register' ? '' : 'none';
  clearErrors();
}

function clearErrors() {
  ['login-error','reg-error','reg-success'].forEach(id => {
    const el = document.getElementById(id);
    el.style.display = 'none';
    el.textContent = '';
  });
}

function showError(id, msg) {
  const el = document.getElementById(id);
  el.textContent = msg;
  el.style.display = 'block';
}

function handleLogin() {
  clearErrors();
  const email = document.getElementById('login-email').value.trim();
  const pass  = document.getElementById('login-pass').value;

  if (!email || !pass) { showError('login-error', 'Please fill in all fields.'); return; }

  const user = getUser(email);
  if (!user || user.password !== pass) {
    showError('login-error', 'Invalid email or password.');
    return;
  }

  const d = loadData();
  d.currentUser = email;
  saveData(d);
  enterDashboard(user);
}

function handleRegister() {
  clearErrors();
  const name  = document.getElementById('reg-name').value.trim();
  const email = document.getElementById('reg-email').value.trim();
  const pass  = document.getElementById('reg-pass').value;
  const pass2 = document.getElementById('reg-pass2').value;

  if (!name || !email || !pass || !pass2) { showError('reg-error', 'Please fill in all fields.'); return; }
  if (pass.length < 6) { showError('reg-error', 'Password must be at least 6 characters.'); return; }
  if (pass !== pass2) { showError('reg-error', 'Passwords do not match.'); return; }
  if (getUser(email)) { showError('reg-error', 'An account with this email already exists.'); return; }

  const d = loadData();
  const user = { ...initUser(name, email), password: pass };
  d.users.push(user);
  d.currentUser = email;
  saveData(d);

  const el = document.getElementById('reg-success');
  el.textContent = '✓ Account created! Redirecting…';
  el.style.display = 'block';

  setTimeout(() => enterDashboard(user), 900);
}

function handleLogout() {
  const d = loadData();
  d.currentUser = null;
  saveData(d);
  document.getElementById('dashboard-page').classList.remove('active');
  document.getElementById('dashboard-page').style.display = 'none';
  document.getElementById('auth-page').classList.add('active');
  document.getElementById('auth-page').style.display = 'flex';
  switchTab('login');
  document.getElementById('login-email').value = '';
  document.getElementById('login-pass').value = '';
}

function enterDashboard(user) {
  document.getElementById('auth-page').classList.remove('active');
  document.getElementById('auth-page').style.display = 'none';
  document.getElementById('dashboard-page').classList.add('active');
  document.getElementById('dashboard-page').style.display = 'flex';
  renderDashboard();
}

// ── NAVIGATION ─────────────────────────────────────
function switchPanel(name, btn) {
  document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(b => b.classList.remove('active'));
  const panel = document.getElementById('panel-' + name);
  panel.classList.remove('active');
  void panel.offsetWidth; // reflow for animation replay
  panel.classList.add('active');
  if (btn) btn.classList.add('active');
  renderDashboard();
}

// ── RENDER ─────────────────────────────────────────
function renderDashboard() {
  const user = currentUser();
  if (!user) return;

  // user info
  const initials = user.name.split(' ').map(w=>w[0]).join('').toUpperCase().slice(0,2);
  document.getElementById('user-avatar').textContent = initials;
  document.getElementById('user-display-name').textContent = user.name.split(' ')[0];
  document.getElementById('greet-name').textContent = user.name.split(' ')[0];
  document.getElementById('acct-number').textContent = user.accountNumber;

  // balance
  const bal = user.balance;
  document.getElementById('hero-balance').innerHTML = '<span>$</span>' + bal.toLocaleString('en-US', {minimumFractionDigits:2,maximumFractionDigits:2});

  const deposits   = user.transactions.filter(t=>t.type==='deposit');
  const withdraws  = user.transactions.filter(t=>t.type==='withdraw');
  const totalDep   = deposits.reduce((s,t)=>s+t.amount,0);
  const totalWith  = withdraws.reduce((s,t)=>s+t.amount,0);

  document.getElementById('chip-deposits').textContent  = fmtMoney(totalDep);
  document.getElementById('chip-withdrawn').textContent = fmtMoney(totalWith);
  document.getElementById('chip-txcount').textContent   = user.transactions.length;

  document.getElementById('last-deposit-val').textContent  = deposits.length  ? fmtMoney(deposits[deposits.length-1].amount)   : '—';
  document.getElementById('last-withdraw-val').textContent = withdraws.length ? fmtMoney(withdraws[withdraws.length-1].amount) : '—';

  // deposit panel balance
  document.getElementById('deposit-panel-balance').textContent  = fmtMoney(bal);
  document.getElementById('withdraw-panel-balance').textContent = fmtMoney(bal);

  // overview tx list (last 5)
  renderTxList('overview-tx-list', user.transactions.slice(-5).reverse(), true);
  renderTxList('full-tx-list', [...user.transactions].reverse(), false);

  // deposit history
  renderDepHistory(deposits.slice(-4).reverse());
  renderWithHistory(withdraws.slice(-4).reverse());
}

function renderTxList(containerId, txs, short) {
  const el = document.getElementById(containerId);
  if (!txs.length) {
    el.innerHTML = `<div style="color:var(--muted);font-size:14px;padding:${short?'20':'40'}px 0;text-align:center">No transactions yet. Make your first deposit!</div>`;
    return;
  }
  el.innerHTML = txs.map(tx => {
    const isPos = tx.type === 'deposit';
    const icon  = isPos
      ? `<div class="tx-icon" style="background:rgba(74,222,128,0.1)"><svg width="18" height="18" fill="none" stroke="#4ade80" stroke-width="2" viewBox="0 0 24 24"><polyline points="23 6 13.5 15.5 8.5 10.5 1 18"/></svg></div>`
      : `<div class="tx-icon" style="background:rgba(248,113,113,0.1)"><svg width="18" height="18" fill="none" stroke="#f87171" stroke-width="2" viewBox="0 0 24 24"><polyline points="23 18 13.5 8.5 8.5 13.5 1 6"/></svg></div>`;
    return `<div class="tx-item">
      ${icon}
      <div class="tx-info">
        <div class="tx-name">${tx.note || (isPos ? 'Deposit' : 'Withdrawal')}</div>
        <div class="tx-date">${fmtDate(tx.date)}</div>
      </div>
      <div class="tx-amount ${isPos?'pos':'neg'}">${isPos?'+':'-'}${fmtMoney(tx.amount)}</div>
    </div>`;
  }).join('');
}

function renderDepHistory(txs) {
  const el = document.getElementById('deposit-history');
  if (!txs.length) { el.innerHTML = `<div style="color:var(--muted);font-size:13px;padding:12px 0;text-align:center">No deposits yet</div>`; return; }
  el.innerHTML = txs.map(tx => `<div class="dep-item">
    <div class="dep-left">
      <div>${tx.note || 'Deposit'}</div>
      <div class="dep-date">${fmtDate(tx.date)}</div>
    </div>
    <div class="dep-amt">+${fmtMoney(tx.amount)}</div>
  </div>`).join('');
}

function renderWithHistory(txs) {
  const el = document.getElementById('withdraw-history');
  if (!txs.length) { el.innerHTML = `<div style="color:var(--muted);font-size:13px;padding:12px 0;text-align:center">No withdrawals yet</div>`; return; }
  el.innerHTML = txs.map(tx => `<div class="dep-item">
    <div class="dep-left">
      <div>${tx.note || 'Withdrawal'}</div>
      <div class="dep-date">${fmtDate(tx.date)}</div>
    </div>
    <div class="dep-amt" style="color:var(--danger)">-${fmtMoney(tx.amount)}</div>
  </div>`).join('');
}

// ── DEPOSIT / WITHDRAW ─────────────────────────────
function setAmount(val) { document.getElementById('deposit-amount').value = val; }
function setWithdrawAmount(val) { document.getElementById('withdraw-amount').value = val; }

function handleDeposit() {
  const raw = parseFloat(document.getElementById('deposit-amount').value);
  const note = document.getElementById('deposit-note').value.trim();
  if (!raw || raw <= 0) { showToast('Please enter a valid amount.', true); return; }

  const user = currentUser();
  user.balance += raw;
  user.transactions.push({ type:'deposit', amount:raw, note, date: new Date().toISOString() });
  updateUser(user);

  document.getElementById('deposit-amount').value = '';
  document.getElementById('deposit-note').value = '';
  showToast('✓ Deposited ' + fmtMoney(raw) + ' successfully!');
  renderDashboard();
}

function handleWithdraw() {
  const raw  = parseFloat(document.getElementById('withdraw-amount').value);
  const note = document.getElementById('withdraw-note').value.trim();
  const user = currentUser();

  if (!raw || raw <= 0) { showToast('Please enter a valid amount.', true); return; }
  if (raw > user.balance) { showToast('Insufficient funds.', true); return; }

  user.balance -= raw;
  user.transactions.push({ type:'withdraw', amount:raw, note, date: new Date().toISOString() });
  updateUser(user);

  document.getElementById('withdraw-amount').value = '';
  document.getElementById('withdraw-note').value = '';
  showToast('✓ Withdrew ' + fmtMoney(raw) + ' successfully!');
  renderDashboard();
}

// ── TOAST ──────────────────────────────────────────
function showToast(msg, isError) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.style.borderColor = isError ? 'rgba(248,113,113,0.3)' : 'rgba(74,222,128,0.3)';
  t.style.color = isError ? 'var(--danger)' : 'var(--success)';
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 3000);
}

// ── INIT ───────────────────────────────────────────
(function init() {
  const d = loadData();
  if (d.currentUser) {
    const user = d.users.find(u => u.email === d.currentUser);
    if (user) { enterDashboard(user); }
  }
})();
</script>
</body>
</html>
