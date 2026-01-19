---
layout: post
title: Learning Game Login
permalink: /learninggame/login
comments: True
---

<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #020617 0%, #0f172a 50%, #1e1b4b 100%);
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    overflow-x: hidden;
    padding: 40px 20px;
    position: relative;
  }

  /* Animated stars background */
  @keyframes twinkle {
    0%, 100% { opacity: 0.3; }
    50% { opacity: 1; }
  }

  .stars {
    position: fixed;
    inset: 0;
    overflow: hidden;
    z-index: 0;
  }

  .star {
    position: absolute;
    width: 2px;
    height: 2px;
    background: white;
    border-radius: 50%;
    animation: twinkle 3s infinite;
  }

  /* Ambient glow effects */
  body::before {
    content: '';
    position: fixed;
    top: 10%;
    left: 10%;
    width: 400px;
    height: 400px;
    background: radial-gradient(circle, rgba(6,182,212,0.15), transparent 70%);
    filter: blur(60px);
    z-index: 0;
  }

  body::after {
    content: '';
    position: fixed;
    bottom: 10%;
    right: 10%;
    width: 400px;
    height: 400px;
    background: radial-gradient(circle, rgba(168,85,247,0.15), transparent 70%);
    filter: blur(60px);
    z-index: 0;
  }

  .container {
    position: relative;
    z-index: 1;
    width: 95vw;
    max-width: 520px;
  }

  /* Outer frame (cyan border, rounded, glow) */
  .frame {
    background: rgba(15, 23, 42, 0.85);
    backdrop-filter: blur(20px);
    border-radius: 24px;
    border: 2px solid rgba(6,182,212,0.4);
    box-shadow: 0 0 60px rgba(6,182,212,0.25);
    overflow: hidden;
    padding: 18px;
  }

  /* Inner panel (emerald accent border) */
  .panel {
    background: rgba(2, 6, 23, 0.5);
    backdrop-filter: blur(10px);
    border-radius: 20px;
    border: 2px solid rgba(16,185,129,0.3);
    padding: 40px 32px;
    position: relative;
  }

  .header {
    text-align: center;
    margin-bottom: 32px;
  }

  .badge {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    width: 80px;
    height: 80px;
    border-radius: 50%;
    background: linear-gradient(135deg, #06b6d4 0%, #3b82f6 100%);
    box-shadow: 0 0 30px rgba(6,182,212,0.6);
    color: white;
    font-size: 32px;
    margin-bottom: 16px;
    animation: pulse 2s infinite;
  }

  @keyframes pulse {
    0%, 100% { box-shadow: 0 0 30px rgba(6,182,212,0.6); }
    50% { box-shadow: 0 0 50px rgba(6,182,212,0.9); }
  }

  .title {
    color: #06b6d4;
    font-size: 28px;
    font-weight: 900;
    text-transform: uppercase;
    letter-spacing: 4px;
    margin-bottom: 8px;
    text-shadow: 0 0 20px rgba(6,182,212,0.5);
  }

  .subtitle {
    color: rgba(103,232,249,0.7);
    font-size: 13px;
    letter-spacing: 0.5px;
    font-family: 'Courier New', monospace;
  }

  /* Tabs styled like holographic buttons */
  .tab-buttons {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin: 24px 0 32px;
  }

  .tab-btn {
    padding: 14px 16px;
    border-radius: 12px;
    border: 1px solid rgba(6,182,212,0.3);
    background: rgba(2, 6, 23, 0.4);
    color: rgba(103,232,249,0.8);
    cursor: pointer;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 1.5px;
    font-size: 13px;
    transition: all 0.2s ease;
    position: relative;
    overflow: hidden;
  }

  .tab-btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(90deg, transparent, rgba(6,182,212,0.1), transparent);
    transform: translateX(-100%);
    transition: transform 0.3s ease;
  }

  .tab-btn:hover::before {
    transform: translateX(100%);
  }

  .tab-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 0 20px rgba(6,182,212,0.2);
    border-color: rgba(103,232,249,0.6);
  }

  .tab-btn.active {
    background: linear-gradient(135deg, rgba(6,182,212,0.3), rgba(59,130,246,0.3));
    border: 2px solid #06b6d4;
    color: #ffffff;
    box-shadow: 0 0 30px rgba(6,182,212,0.4);
  }

  .form-container { 
    display: none;
    animation: fadeIn 0.3s ease;
  }
  .form-container.active { display: block; }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .form-group { margin-bottom: 20px; }

  .form-label {
    display: block;
    color: rgba(103,232,249,0.9);
    margin-bottom: 8px;
    font-size: 11px;
    font-weight: 800;
    text-transform: uppercase;
    letter-spacing: 1.5px;
  }

  .form-input {
    width: 100%;
    padding: 14px 16px;
    border-radius: 12px;
    border: 1px solid rgba(6,182,212,0.3);
    background: rgba(2, 6, 23, 0.5);
    color: #ffffff;
    outline: none;
    transition: all 0.2s ease;
  }

  .form-input::placeholder { color: rgba(148,163,184,0.5); }

  .form-input:focus {
    border-color: #06b6d4;
    box-shadow: 0 0 0 3px rgba(6,182,212,0.2);
    background: rgba(2, 6, 23, 0.7);
  }

  .submit-btn {
    width: 100%;
    padding: 16px 20px;
    border: none;
    border-radius: 12px;
    cursor: pointer;
    font-weight: 900;
    text-transform: uppercase;
    letter-spacing: 2px;
    background: linear-gradient(135deg, #06b6d4 0%, #3b82f6 100%);
    color: #ffffff;
    box-shadow: 0 0 25px rgba(6,182,212,0.4);
    transition: all 0.2s ease;
    position: relative;
    overflow: hidden;
  }

  .submit-btn::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
    transform: translateX(-100%);
    transition: transform 0.5s ease;
  }

  .submit-btn:hover::before {
    transform: translateX(100%);
  }

  .submit-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 0 40px rgba(6,182,212,0.6);
  }

  .submit-btn:active { transform: translateY(0); }

  /* Danger button variant */
  .submit-btn.danger {
    background: linear-gradient(135deg, #ef4444 0%, #dc2626 100%);
    box-shadow: 0 0 25px rgba(239,68,68,0.3);
  }
  .submit-btn.danger:hover {
    box-shadow: 0 0 40px rgba(239,68,68,0.5);
  }

  .message {
    margin-top: 20px;
    padding: 14px 16px;
    border-radius: 12px;
    text-align: center;
    font-weight: 700;
    font-size: 14px;
    display: none;
    border: 1px solid transparent;
    background: rgba(2, 6, 23, 0.5);
    color: rgba(226,232,240,0.92);
  }

  .message.success {
    border-color: rgba(16,185,129,0.7);
    background: rgba(16,185,129,0.1);
    color: #10b981;
    box-shadow: 0 0 20px rgba(16,185,129,0.2);
  }

  .message.error {
    border-color: rgba(239,68,68,0.7);
    background: rgba(239,68,68,0.1);
    color: #ef4444;
    box-shadow: 0 0 20px rgba(239,68,68,0.2);
  }

  .loading {
    display: none;
    text-align: center;
    margin-top: 16px;
  }
  .loading.active { display: block; }

  .loading-spinner {
    display: inline-block;
    width: 32px;
    height: 32px;
    border-radius: 50%;
    border: 3px solid rgba(6,182,212,0.2);
    border-top: 3px solid #06b6d4;
    animation: spin 0.8s linear infinite;
  }

  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }

  .footer-hint {
    margin-top: 24px;
    padding-top: 24px;
    border-top: 1px solid rgba(6,182,212,0.2);
    text-align: center;
    color: rgba(6,182,212,0.5);
    font-size: 11px;
    font-family: 'Courier New', monospace;
  }

  @media (max-width: 520px) {
    .panel { padding: 32px 24px; }
    .title { font-size: 24px; }
  }
</style>

<!-- Stars background -->
<div class="stars" id="stars"></div>

<div class="container">
  <div class="frame">
    <div class="panel">
      <div class="header">
        <div class="badge">🚀</div>
        <div class="title">Cadet Academy</div>
        <div class="subtitle">Authorization Required // Station Access Control</div>
      </div>

      <div class="tab-buttons">
        <button class="tab-btn active" onclick="switchTab('register', event)">
          <span style="position: relative; z-index: 1;">Register</span>
        </button>
        <button class="tab-btn" onclick="switchTab('login', event)">
          <span style="position: relative; z-index: 1;">Login</span>
        </button>
      </div>

      <!-- Registration Form -->
      <div id="register-form" class="form-container active">
        <div onsubmit="handleRegister(event); return false;">
          <div class="form-group">
            <label class="form-label">First Name</label>
            <input type="text" class="form-input" id="reg-firstname" placeholder="Enter first name" required>
          </div>

          <div class="form-group">
            <label class="form-label">Last Name</label>
            <input type="text" class="form-input" id="reg-lastname" placeholder="Enter last name" required>
          </div>

          <div class="form-group">
            <label class="form-label">Cadet ID (GitHub)</label>
            <input type="text" class="form-input" id="reg-github" placeholder="Enter GitHub username" required>
          </div>

          <div class="form-group">
            <label class="form-label">Access Code</label>
            <input type="password" class="form-input" id="reg-password" placeholder="Create access code" required>
          </div>

          <button type="button" class="submit-btn" onclick="handleRegister(event)">
            <span style="position: relative; z-index: 1;">Initialize Cadet Profile</span>
          </button>
        </div>
      </div>

      <!-- Login Form -->
      <div id="login-form" class="form-container">
        <div onsubmit="handleLogin(event); return false;">
          <div class="form-group">
            <label class="form-label">Cadet ID (GitHub)</label>
            <input type="text" class="form-input" id="login-github" placeholder="Enter GitHub username" required>
          </div>

          <div class="form-group">
            <label class="form-label">Access Code</label>
            <input type="password" class="form-input" id="login-password" placeholder="Enter access code" required>
          </div>

          <button type="button" class="submit-btn" onclick="handleLogin(event)">
            <span style="position: relative; z-index: 1;">Access Station Systems</span>
          </button>
        </div>
      </div>

      <div class="loading">
        <div class="loading-spinner"></div>
      </div>

      <div id="message" class="message"></div>

      <div id="continue-section" style="display: none; margin-top: 20px;">
        <button class="submit-btn" onclick="continueToGame()">
          <span style="position: relative; z-index: 1;">Continue to Station</span>
        </button>
        <button class="submit-btn danger" onclick="signOut()" style="margin-top: 12px;">
          <span style="position: relative; z-index: 1;">Sign Out</span>
        </button>
      </div>

      <div class="footer-hint">
        Authorized Personnel Only // Security Level: Cadet
      </div>
    </div>
  </div>
</div>

<script>
  // Generate stars
  const starsContainer = document.getElementById('stars');
  for (let i = 0; i < 100; i++) {
    const star = document.createElement('div');
    star.className = 'star';
    star.style.left = Math.random() * 100 + '%';
    star.style.top = Math.random() * 100 + '%';
    star.style.animationDelay = Math.random() * 3 + 's';
    star.style.opacity = Math.random() * 0.7 + 0.3;
    starsContainer.appendChild(star);
  }

  const API_URL = (location.hostname === "localhost")
      ? "http://localhost:8587/api"
      : `${window.location.origin}/api`;

  function switchTab(tab, ev) {
    document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
    if (ev?.target) ev.target.classList.add('active');

    document.querySelectorAll('.form-container').forEach(form => form.classList.remove('active'));
    document.getElementById(tab + '-form').classList.add('active');

    hideMessage();
  }

  function showMessage(text, type) {
    const messageDiv = document.getElementById('message');
    messageDiv.textContent = text;
    messageDiv.className = 'message ' + type;
    messageDiv.style.display = 'block';
  }

  function hideMessage() {
    document.getElementById('message').style.display = 'none';
  }

  function showLoading() {
    document.querySelector('.loading').classList.add('active');
  }

  function hideLoading() {
    document.querySelector('.loading').classList.remove('active');
  }

  function getSession() {
    try {
      return JSON.parse(localStorage.getItem("userSession") || "{}");
    } catch (e) {
      return {};
    }
  }

  function setSession(githubId, extra = {}) {
    localStorage.setItem("userSession", JSON.stringify({
      githubId,
      loginTime: new Date().toISOString(),
      ...extra
    }));
  }

  function clearSession() {
    localStorage.removeItem("userSession");
  }

  // --- RPG REGISTER ---
  async function handleRegister(event) {
    event.preventDefault();
    hideMessage();
    showLoading();

    const firstName = document.getElementById('reg-firstname').value.trim();
    const lastName = document.getElementById('reg-lastname').value.trim();
    const githubId = document.getElementById('reg-github').value.trim();
    const password = document.getElementById('reg-password').value;

    const userData = { FirstName: firstName, LastName: lastName, GitHubID: githubId, Password: password };

    try {
      const response = await fetch(`${API_URL}/rpg/data`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(userData)
      });

      const data = await response.json().catch(() => ({}));
      hideLoading();

      if (response.ok) {
        showMessage(`✅ Cadet ${firstName} registered! Access granted.`, 'success');
        document.getElementById('reg-firstname').value = '';
        document.getElementById('reg-lastname').value = '';
        document.getElementById('reg-github').value = '';
        document.getElementById('reg-password').value = '';
        setTimeout(() => document.querySelectorAll('.tab-btn')[1].click(), 900);
      } else {
        if (response.status === 409 || (data.message && data.message.includes('already exists'))) {
          showMessage(`⚠️ Cadet profile exists. Please login.`, 'error');
        } else {
          showMessage(`⚠️ ${data.message || 'Registration failed. Please try again.'}`, 'error');
        }
      }
    } catch (error) {
      hideLoading();
      showMessage('⚠️ Connection failed. Station systems offline.', 'error');
      console.error(error);
    }
  }

  // --- RPG LOGIN ---
  async function handleLogin(event) {
    event.preventDefault();
    hideMessage();
    showLoading();

    const githubId = document.getElementById('login-github').value.trim();
    const password = document.getElementById('login-password').value;

    try {
      const response = await fetch(`${API_URL}/rpg/login`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ GitHubID: githubId, Password: password })
      });

      const data = await response.json().catch(() => ({}));
      hideLoading();

      if (!response.ok) {
        showMessage(`⚠️ ${data.message || 'Access denied. Check credentials.'}`, 'error');
        return;
      }

      setSession(githubId, { name: data?.user?.name });

      showMessage(`✅ Welcome, Cadet ${githubId}. Accessing station...`, 'success');
      setTimeout(() => window.location.href = '/learninggame/home', 750);

    } catch (error) {
      hideLoading();
      showMessage('⚠️ Connection failed. Station systems offline.', 'error');
      console.error(error);
    }
  }

  window.addEventListener('load', () => {
    const s = getSession();
    if (s?.githubId) {
      showMessage(`✅ Already logged in as Cadet ${s.githubId}.`, 'success');

      document.querySelectorAll('.form-container').forEach(form => form.style.display = 'none');
      document.querySelector('.tab-buttons').style.display = 'none';
      document.getElementById('continue-section').style.display = 'block';
    }
  });

  function continueToGame() {
    window.location.href = '/learninggame/home';
  }

  async function signOut() {
    clearSession();
    showMessage('✅ Signed out. Clearance revoked.', 'success');
    setTimeout(() => location.reload(), 650);
  }
</script>