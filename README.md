<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="theme-color" content="#0a0500">
<title>FireRead Pro</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Serif+Display:ital@0;1&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
/* ═══ VARIABLES ═══ */
:root {
  --bg: #09040a;
  --bg2: #100810;
  --sur: #1c101c;
  --sur2: #281828;
  --bdr: #3a1f3a;
  --f1: #ff3a00;
  --f2: #ff6600;
  --f3: #ff9900;
  --f4: #ffcc00;
  --txt: #fff5ee;
  --txt2: #c8a882;
  --txt3: #7a5050;
  --hl: rgba(255, 102, 0, 0.3);
  --hlw: #ffcc00;
  --r: 12px;
  --r2: 20px;
  --gfire: linear-gradient(135deg, #ff3a00, #ff6600 45%, #ffcc00);
  --gshadow: 0 12px 40px rgba(0, 0, 0, 0.6);
  --safe-bottom: env(safe-area-inset-bottom, 0px);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  -webkit-tap-highlight-color: transparent;
}

html, body {
  height: 100%;
  font-family: 'Syne', sans-serif;
  background: var(--bg);
  color: var(--txt);
  overflow: hidden;
  -webkit-font-smoothing: antialiased;
  text-rendering: optimizeLegibility;
}

/* ═══ PAGE SYSTEM ═══ */
.page {
  position: fixed;
  inset: 0;
  display: flex;
  flex-direction: column;
  background: var(--bg);
  will-change: transform, opacity;
  transform: translateX(100%);
  transition: transform 0.25s cubic-bezier(0.4, 0, 0.2, 1), opacity 0.25s ease;
  z-index: 10;
  overflow: hidden;
  opacity: 0;
}

.page.active {
  transform: translateX(0);
  opacity: 1;
  z-index: 20;
}

.page.behind {
  transform: translateX(-20%);
  opacity: 0.6;
  z-index: 5;
}

#page-home {
  transform: translateX(0);
  opacity: 1;
  z-index: 15;
}

/* ═══ NAV ═══ */
.nav {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 0 16px;
  height: 60px;
  min-height: 60px;
  background: var(--bg2);
  border-bottom: 1px solid var(--bdr);
  flex-shrink: 0;
  z-index: 100;
  padding-top: env(safe-area-inset-top, 0px);
}

.nav-back {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  border: 1px solid var(--bdr);
  background: var(--sur);
  color: var(--txt2);
  font-size: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}

.nav-back:active {
  background: var(--sur2);
  color: var(--f2);
  transform: scale(0.95);
}

.nav-title {
  font-family: 'DM Serif Display', serif;
  font-size: 17px;
  color: var(--txt);
  flex: 1;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.nav-logo {
  font-family: 'DM Serif Display', serif;
  font-size: 24px;
  background: var(--gfire);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: 900;
}

.nav-acts {
  display: flex;
  gap: 8px;
}

.nav-btn {
  width: 38px;
  height: 38px;
  border-radius: 10px;
  border: 1px solid var(--bdr);
  background: var(--sur);
  color: var(--txt2);
  font-size: 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.nav-btn:active {
  color: var(--f2);
  border-color: var(--f2);
}

/* ═══ TABS ═══ */
.tabs {
  display: flex;
  background: var(--bg2);
  border-top: 1px solid var(--bdr);
  height: calc(65px + var(--safe-bottom));
  padding-bottom: var(--safe-bottom);
  flex-shrink: 0;
  z-index: 100;
}

.tab {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 4px;
  cursor: pointer;
  color: var(--txt3);
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.5px;
  text-transform: uppercase;
  border: none;
  background: none;
}

.tab .ti { font-size: 22px; line-height: 1; }
.tab.on { color: var(--f3); }
.tab.on .ti { filter: drop-shadow(0 0 8px var(--f2)); }

/* ═══ READER VIEW ═══ */
.book-scroll {
  flex: 1;
  overflow-y: auto;
  -webkit-overflow-scrolling: touch;
  background: #080408;
  padding: 24px 16px 80px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 32px;
}

.book-page {
  width: 100%;
  max-width: 600px;
  background: #fffdf8;
  border-radius: 4px;
  box-shadow: 
    0 1px 1px rgba(0,0,0,0.1),
    0 10px 30px rgba(0,0,0,0.4),
    inset 0 0 40px rgba(0,0,0,0.02);
  position: relative;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

/* Spine Effect */
.book-page::before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  bottom: 0;
  width: 12px;
  background: linear-gradient(90deg, rgba(0,0,0,0.15) 0%, rgba(0,0,0,0.05) 50%, transparent 100%);
  z-index: 2;
}

.page-header {
  padding: 12px 24px;
  border-bottom: 1px solid rgba(0,0,0,0.05);
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: rgba(0,0,0,0.02);
}

.page-header-title {
  font-size: 11px;
  color: #9a8a70;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
  max-width: 60%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.page-header-num {
  font-family: 'JetBrains Mono', monospace;
  font-size: 11px;
  color: #b0a080;
}

.page-content {
  padding: 32px 32px 40px 40px;
  color: #1a1510;
  line-height: 1.8;
  font-family: 'DM Serif Display', serif;
  font-size: var(--fs, 18px);
}

.page-content p {
  margin-bottom: 1.5em;
}

.page-footer {
  padding: 12px 24px;
  border-top: 1px solid rgba(0,0,0,0.05);
  display: flex;
  justify-content: center;
  background: rgba(0,0,0,0.02);
}

.page-footer-num {
  font-family: 'JetBrains Mono', monospace;
  font-size: 12px;
  color: #9a8a70;
  font-weight: 600;
}

/* Word Highlighting */
.word {
  display: inline;
  cursor: pointer;
  border-radius: 3px;
  padding: 0 1px;
  transition: background 0.1s;
}

.word.speaking {
  background: #ffcc00;
  color: #000;
  box-shadow: 0 0 8px rgba(255, 204, 0, 0.5);
  font-weight: 600;
}

/* ═══ LIBRARY ═══ */
.lib-card {
  display: flex;
  align-items: center;
  gap: 12px;
  background: var(--sur);
  border: 1px solid var(--bdr);
  border-radius: var(--r);
  padding: 14px;
  margin-bottom: 10px;
  transition: transform 0.2s;
}

.lib-card:active { transform: scale(0.98); }
.lib-card.now { border-color: var(--f2); background: var(--sur2); }

.lci { font-size: 28px; flex-shrink: 0; }
.lcb { flex: 1; min-width: 0; cursor: pointer; }
.lcn { font-size: 14px; font-weight: 700; color: var(--txt); white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
.lcm { font-size: 11px; color: var(--txt3); margin-top: 4px; }

.lib-del {
  width: 40px;
  height: 40px;
  border-radius: 10px;
  border: 1px solid rgba(255, 58, 0, 0.2);
  background: rgba(255, 58, 0, 0.05);
  color: var(--f1);
  font-size: 18px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s;
}

.lib-del:hover { background: rgba(255, 58, 0, 0.15); border-color: var(--f1); }
.lib-del:active { transform: scale(0.9); background: var(--f1); color: #fff; }

/* ═══ PLAYBAR ═══ */
.playbar {
  background: var(--bg2);
  border-top: 1px solid var(--bdr);
  padding: 12px 16px calc(12px + var(--safe-bottom));
  flex-shrink: 0;
  box-shadow: 0 -10px 30px rgba(0,0,0,0.5);
}

.pb-row { display: flex; align-items: center; gap: 12px; margin-bottom: 12px; }
.pb-time { font-size: 11px; color: var(--txt3); font-family: 'JetBrains Mono', monospace; min-width: 35px; }
.pb-track { flex: 1; height: 6px; background: var(--bdr); border-radius: 10px; position: relative; cursor: pointer; }
.pb-fill { height: 100%; background: var(--gfire); border-radius: 10px; width: 0%; }
.pb-thumb {
  position: absolute;
  top: 50%;
  left: 0%;
  transform: translate(-50%, -50%);
  width: 16px;
  height: 16px;
  background: #fff;
  border: 3px solid var(--f2);
  border-radius: 50%;
  box-shadow: 0 0 10px var(--f2);
}

.pb-ctrls { display: flex; align-items: center; justify-content: center; gap: 16px; }
.pb-btn {
  width: 44px;
  height: 44px;
  border-radius: 12px;
  border: 1px solid var(--bdr);
  background: var(--sur);
  color: var(--txt2);
  font-size: 18px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.pb-play {
  width: 60px;
  height: 60px;
  border-radius: 20px;
  background: var(--gfire);
  color: #000;
  font-size: 28px;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 6px 20px rgba(255, 80, 0, 0.4);
}

/* ═══ UTILS ═══ */
.fbtn {
  width: 100%;
  padding: 16px;
  border: none;
  border-radius: var(--r);
  font-family: 'Syne', sans-serif;
  font-weight: 700;
  font-size: 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  transition: all 0.2s;
}

.fbtn.fire { background: var(--gfire); color: #000; }
.fbtn.ghost { background: var(--sur); color: var(--txt2); border: 1px solid var(--bdr); }
.fbtn:active { transform: scale(0.98); opacity: 0.9; }

.toast {
  position: fixed;
  bottom: 100px;
  left: 50%;
  transform: translateX(-50%) translateY(20px);
  background: var(--sur2);
  border: 1px solid var(--f2);
  color: #fff;
  padding: 12px 20px;
  border-radius: 30px;
  font-size: 14px;
  font-weight: 600;
  z-index: 1000;
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.toast.show { opacity: 1; transform: translateX(-50%) translateY(0); }

/* ═══ EMBERS ═══ */
.embers { position: fixed; inset: 0; pointer-events: none; z-index: 0; overflow: hidden; }
.ember { position: absolute; bottom: -10px; border-radius: 50%; opacity: 0; animation: rise var(--d,4s) var(--dl,0s) infinite ease-out; }
@keyframes rise {
  0% { opacity: 0.8; transform: translateY(0) scale(1); }
  100% { opacity: 0; transform: translateY(-100vh) translateX(var(--dx, 0px)) scale(0.2); }
}

/* Custom Scrollbar */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--bdr); border-radius: 10px; }

</style>
</head>
<body>

<div class="embers" id="embers"></div>

<!-- ════════════════════════════════
  HOME PAGE
════════════════════════════════ -->
<div class="page active" id="page-home">
  <div class="nav">
    <div class="nav-logo">FireRead</div>
    <div class="nav-acts">
      <button class="nav-btn" onclick="goPage('page-ai')">✨</button>
      <button class="nav-btn" onclick="goPage('page-voice')">🎙️</button>
    </div>
  </div>
  
  <div style="flex:1; overflow-y:auto; padding-bottom: 20px;">
    <!-- Hero Quote -->
    <div style="padding: 30px 20px; text-align: center; background: linear-gradient(180deg, #1a0800, transparent);">
      <div id="qText" style="font-family: 'DM Serif Display', serif; font-size: 20px; font-style: italic; line-height: 1.6; margin-bottom: 10px;">"Ignite your mind. The fire within you never lies."</div>
      <div id="qAuth" style="color: var(--f3); font-weight: 700; font-size: 12px; letter-spacing: 1px;">— FIREREAD</div>
    </div>

    <!-- Quick Add -->
    <div style="padding: 0 16px 24px;">
      <div style="font-size: 11px; font-weight: 800; color: var(--txt3); text-transform: uppercase; letter-spacing: 2px; margin-bottom: 12px;">Ignite New Knowledge</div>
      <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 12px;">
        <div class="fbtn ghost" onclick="document.getElementById('fileIn').click()" style="flex-direction: column; height: 90px; padding: 10px;">
          <span style="font-size: 24px;">📄</span>
          <span style="font-size: 12px;">Upload File</span>
          <input type="file" id="fileIn" hidden accept=".pdf,.txt,.md,.epub" onchange="handleFile(this)">
        </div>
        <div class="fbtn ghost" onclick="goPage('page-url')" style="flex-direction: column; height: 90px; padding: 10px;">
          <span style="font-size: 24px;">🔗</span>
          <span style="font-size: 12px;">Paste URL</span>
        </div>
      </div>
    </div>

    <!-- Library -->
    <div style="padding: 0 16px;">
      <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px;">
        <div style="font-size: 11px; font-weight: 800; color: var(--txt3); text-transform: uppercase; letter-spacing: 2px;">Your Library (<span id="libCount">0</span>)</div>
      </div>
      <div id="libraryList">
        <div style="text-align: center; padding: 40px 20px; color: var(--txt3); border: 1px dashed var(--bdr); border-radius: var(--r);">
          No documents yet. Add one to start reading.
        </div>
      </div>
    </div>
  </div>

  <div class="tabs">
    <button class="tab on" onclick="goHome()"><span class="ti">🏠</span><span>Home</span></button>
    <button class="tab" onclick="goPage('page-saves')"><span class="ti">🔖</span><span>Saves</span></button>
    <button class="tab" onclick="goPage('page-ai')"><span class="ti">✨</span><span>AI Tools</span></button>
    <button class="tab" onclick="goPage('page-voice')"><span class="ti">🎙️</span><span>Voice</span></button>
  </div>
</div>

<!-- ════════════════════════════════
  READER PAGE
════════════════════════════════ -->
<div class="page" id="page-reader">
  <div class="nav">
    <button class="nav-back" onclick="goHome()">←</button>
    <div class="nav-title" id="readerTitle">Reading...</div>
    <div class="nav-acts">
      <button class="nav-btn" onclick="addBookmark()">🔖</button>
      <button class="nav-btn" onclick="goPage('page-ai')">✨</button>
    </div>
  </div>

  <div id="readerMain" style="flex:1; display:flex; flex-direction:column; overflow:hidden;">
    <div style="background: var(--bg2); padding: 8px 16px; border-bottom: 1px solid var(--bdr); display: flex; align-items: center; justify-content: space-between;">
      <div style="display: flex; align-items: center; gap: 8px;">
        <button class="nav-btn" style="width: 32px; height: 32px;" onclick="changeFontSize(-1)">A-</button>
        <span id="fontVal" style="font-family: 'JetBrains Mono', monospace; font-size: 13px; color: var(--f3);">18</span>
        <button class="nav-btn" style="width: 32px; height: 32px;" onclick="changeFontSize(1)">A+</button>
      </div>
      <div id="pgIndicator" style="font-family: 'JetBrains Mono', monospace; font-size: 11px; color: var(--txt3);">p.1 / 1</div>
    </div>

    <div class="book-scroll" id="bookScroll">
      <!-- Pages injected here -->
    </div>
  </div>

  <div class="playbar" id="playbar">
    <div class="pb-row">
      <span class="pb-time" id="pbEl">0:00</span>
      <div class="pb-track" id="pbTrack" onclick="seek(event)">
        <div class="pb-fill" id="pbFill"></div>
        <div class="pb-thumb" id="pbThumb"></div>
      </div>
      <span class="pb-time" id="pbTot">0:00</span>
    </div>
    <div class="pb-ctrls">
      <button class="pb-btn" onclick="skip(-15)">-15s</button>
      <button class="pb-play" id="pbPlay" onclick="togglePlay()">▶</button>
      <button class="pb-btn" onclick="skip(15)">+15s</button>
    </div>
    <div style="margin-top: 12px; display: flex; align-items: center; justify-content: center; gap: 10px;">
      <span style="font-size: 10px; color: var(--txt3);">SPEED</span>
      <input type="range" min="0.5" max="2.5" step="0.1" value="1" oninput="setSpeed(this.value)" style="flex: 0.5; accent-color: var(--f2);">
      <span id="speedVal" style="font-family: 'JetBrains Mono', monospace; font-size: 12px; color: var(--f3); min-width: 35px;">1.0x</span>
    </div>
  </div>
</div>

<!-- ════════════════════════════════
  URL PAGE
════════════════════════════════ -->
<div class="page" id="page-url">
  <div class="nav">
    <button class="nav-back" onclick="goBack()">←</button>
    <div class="nav-title">Fetch Article</div>
  </div>
  <div style="padding: 24px 16px;">
    <div style="margin-bottom: 20px;">
      <div style="font-size: 12px; color: var(--txt2); margin-bottom: 8px;">Paste a link to a blog post or news article:</div>
      <input type="url" id="urlIn" class="fbtn ghost" style="text-align: left; cursor: text;" placeholder="https://example.com/article">
    </div>
    <button class="fbtn fire" onclick="submitURL()">🔥 Fetch & Add</button>
  </div>
</div>

<!-- ════════════════════════════════
  SAVES PAGE
════════════════════════════════ -->
<div class="page" id="page-saves">
  <div class="nav">
    <button class="nav-back" onclick="goBack()">←</button>
    <div class="nav-title">Your Saves</div>
  </div>
  <div style="padding: 16px; flex: 1; overflow-y: auto;">
    <div style="font-size: 11px; font-weight: 800; color: var(--txt3); text-transform: uppercase; letter-spacing: 2px; margin-bottom: 12px;">Bookmarks</div>
    <div id="bookmarksList">
      <div style="text-align: center; padding: 30px; color: var(--txt3);">No bookmarks yet.</div>
    </div>
  </div>
</div>

<!-- ════════════════════════════════
  VOICE PAGE
════════════════════════════════ -->
<div class="page" id="page-voice">
  <div class="nav">
    <button class="nav-back" onclick="goBack()">←</button>
    <div class="nav-title">Voice Settings</div>
  </div>
  <div style="padding: 16px; flex: 1; overflow-y: auto;">
    <div style="font-size: 11px; font-weight: 800; color: var(--txt3); text-transform: uppercase; letter-spacing: 2px; margin-bottom: 12px;">Select Voice</div>
    <select id="voiceSel" onchange="pickVoice()" class="fbtn ghost" style="text-align: left; margin-bottom: 16px;">
      <option>Loading voices...</option>
    </select>
    <button class="fbtn fire" onclick="testVoice()">🔊 Test Voice</button>
  </div>
</div>

<!-- ════════════════════════════════
  AI TOOLS PAGE
════════════════════════════════ -->
<div class="page" id="page-ai">
  <div class="nav">
    <button class="nav-back" onclick="goBack()">←</button>
    <div class="nav-title">AI Study Tools</div>
  </div>
  <div style="padding: 16px; flex: 1; overflow-y: auto;">
    <div style="text-align: center; padding: 40px 20px;">
      <div style="font-size: 40px; margin-bottom: 16px;">✨</div>
      <div style="font-family: 'DM Serif Display', serif; font-size: 22px; margin-bottom: 8px;">AI Insights</div>
      <div style="color: var(--txt2); font-size: 14px; line-height: 1.6;">AI tools are available when a document is active in the reader.</div>
    </div>
  </div>
</div>

<div class="toast" id="toast">🔥 Action Complete</div>

<script>
'use strict';

// ── STATE ──
let library = JSON.parse(localStorage.getItem('fr_library') || '[]');
let currentDoc = null;
let words = [];
let wordIdx = 0;
let isPlaying = false;
let speechRate = 1.0;
let fontSize = 18;
let bookmarks = JSON.parse(localStorage.getItem('fr_bookmarks') || '[]');
let voices = [];
let selVoiceIdx = 0;
let pageHistory = ['page-home'];
const synth = window.speechSynthesis;

// ── INITIALIZATION ──
window.onload = () => {
  spawnEmbers();
  renderLibrary();
  loadVoices();
  initQuotes();
  
  // Resume last session if exists
  const lastId = localStorage.getItem('fr_last_doc');
  if (lastId) {
    const doc = library.find(d => d.id == lastId);
    if (doc) loadDoc(doc);
  }
};

// ── NAVIGATION (Professional & Fast) ──
function goPage(id) {
  const cur = pageHistory[pageHistory.length - 1];
  if (cur === id) return;
  
  const curEl = document.getElementById(cur);
  const nextEl = document.getElementById(id);
  
  if (curEl) curEl.classList.add('behind');
  if (nextEl) {
    nextEl.classList.add('active');
    nextEl.classList.remove('behind');
  }
  
  pageHistory.push(id);
}

function goBack() {
  if (pageHistory.length <= 1) return;
  
  const cur = pageHistory.pop();
  const prev = pageHistory[pageHistory.length - 1];
  
  const curEl = document.getElementById(cur);
  const prevEl = document.getElementById(prev);
  
  if (curEl) curEl.classList.remove('active', 'behind');
  if (prevEl) prevEl.classList.remove('behind');
}

function goHome() {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active', 'behind'));
  document.getElementById('page-home').classList.add('active');
  pageHistory = ['page-home'];
}

// ── LIBRARY & STORAGE ──
function saveStorage() {
  localStorage.setItem('fr_library', JSON.stringify(library));
  localStorage.setItem('fr_bookmarks', JSON.stringify(bookmarks));
}

function renderLibrary() {
  const list = document.getElementById('libraryList');
  document.getElementById('libCount').textContent = library.length;
  
  if (library.length === 0) {
    list.innerHTML = `<div style="text-align: center; padding: 40px 20px; color: var(--txt3); border: 1px dashed var(--bdr); border-radius: var(--r);">No documents yet. Add one to start reading.</div>`;
    return;
  }

  list.innerHTML = library.slice().reverse().map(doc => `
    <div class="lib-card ${currentDoc?.id === doc.id ? 'now' : ''}">
      <div class="lci">📄</div>
      <div class="lcb" onclick="openDoc(${doc.id})">
        <div class="lcn">${escapeHTML(doc.name)}</div>
        <div class="lcm">${calculateReadingTime(doc.text)} · ${doc.progress || 0}% complete</div>
      </div>
      <button class="lib-del" onclick="event.stopPropagation(); delDoc(${doc.id})">🗑️</button>
    </div>
  `).join('');
}

function openDoc(id) {
  const doc = library.find(d => d.id === id);
  if (doc) {
    loadDoc(doc);
    goPage('page-reader');
  }
}

function delDoc(id) {
  if (!confirm('Are you sure you want to delete this document?')) return;
  
  library = library.filter(d => d.id !== id);
  bookmarks = bookmarks.filter(b => b.docId !== id);
  
  if (currentDoc?.id === id) {
    currentDoc = null;
    stopSpeech();
    localStorage.removeItem('fr_last_doc');
  }
  
  saveStorage();
  renderLibrary();
  renderBookmarks();
  showToast('🗑️ Document deleted');
}

// ── READER LOGIC ──
function loadDoc(doc) {
  currentDoc = doc;
  localStorage.setItem('fr_last_doc', doc.id);
  document.getElementById('readerTitle').textContent = doc.name;
  
  // Build pages
  buildPages(doc.text);
  
  // Restore progress
  if (doc.wordIdx) {
    wordIdx = doc.wordIdx;
    updateUI();
    scrollToCurrent();
  } else {
    wordIdx = 0;
    updateUI();
  }
  
  renderLibrary();
}

const WORDS_PER_PAGE = 300;
function buildPages(text) {
  const container = document.getElementById('bookScroll');
  container.innerHTML = '';
  words = [];
  
  const rawWords = text.split(/\s+/).filter(w => w.length > 0);
  const totalPages = Math.ceil(rawWords.length / WORDS_PER_PAGE);
  
  for (let i = 0; i < totalPages; i++) {
    const pageWords = rawWords.slice(i * WORDS_PER_PAGE, (i + 1) * WORDS_PER_PAGE);
    
    const pageEl = document.createElement('div');
    pageEl.className = 'book-page';
    pageEl.id = `page-${i}`;
    
    pageEl.innerHTML = `
      <div class="page-header">
        <div class="page-header-title">${escapeHTML(currentDoc.name)}</div>
        <div class="page-header-num">P. ${i + 1}</div>
      </div>
      <div class="page-content" style="--fs: ${fontSize}px">
        ${pageWords.map((w, idx) => {
          const globalIdx = i * WORDS_PER_PAGE + idx;
          const wordObj = { text: w, page: i, id: globalIdx };
          words.push(wordObj);
          return `<span class="word" id="w-${globalIdx}" onclick="jumpTo(${globalIdx})">${w}</span>`;
        }).join(' ')}
      </div>
      <div class="page-footer">
        <div class="page-footer-num">— ${i + 1} —</div>
      </div>
    `;
    
    container.appendChild(pageEl);
    // Link the word objects to their elements
    pageWords.forEach((_, idx) => {
      const globalIdx = i * WORDS_PER_PAGE + idx;
      words[globalIdx].el = document.getElementById(`w-${globalIdx}`);
    });
  }
  
  document.getElementById('pgIndicator').textContent = `p.1 / ${totalPages}`;
  document.getElementById('pbTot').textContent = formatTime(words.length / 2.5); // Rough estimate
}

function jumpTo(idx) {
  stopSpeech();
  wordIdx = idx;
  updateUI();
  if (isPlaying) resumeSpeech();
}

function updateUI() {
  if (!words.length) return;
  
  // Highlight current word
  document.querySelectorAll('.word.speaking').forEach(el => el.classList.remove('speaking'));
  const currentWord = words[wordIdx];
  if (currentWord && currentWord.el) {
    currentWord.el.classList.add('speaking');
    
    // Update page indicator
    const totalPages = Math.ceil(words.length / WORDS_PER_PAGE);
    document.getElementById('pgIndicator').textContent = `p.${currentWord.page + 1} / ${totalPages}`;
  }
  
  // Update progress bar
  const progress = (wordIdx / words.length) * 100;
  document.getElementById('pbFill').style.width = `${progress}%`;
  document.getElementById('pbThumb').style.left = `${progress}%`;
  document.getElementById('pbEl').textContent = formatTime(wordIdx / 2.5);
  
  // Update state
  if (currentDoc) {
    currentDoc.wordIdx = wordIdx;
    currentDoc.progress = Math.round(progress);
    saveStorage();
  }
}

function scrollToCurrent() {
  const el = words[wordIdx]?.el;
  if (el) el.scrollIntoView({ behavior: 'smooth', block: 'center' });
}

// ── SPEECH ──
let utterance = null;

function togglePlay() {
  if (isPlaying) {
    pauseSpeech();
  } else {
    resumeSpeech();
  }
}

function resumeSpeech() {
  if (!currentDoc || !words.length) return;
  
  synth.cancel();
  isPlaying = true;
  document.getElementById('pbPlay').textContent = '⏸';
  
  const remainingText = words.slice(wordIdx).map(w => w.text).join(' ');
  utterance = new SpeechSynthesisUtterance(remainingText);
  utterance.rate = speechRate;
  if (voices[selVoiceIdx]) utterance.voice = voices[selVoiceIdx];
  
  let lastWordTime = 0;
  utterance.onboundary = (e) => {
    if (e.name === 'word') {
      wordIdx++;
      if (wordIdx >= words.length) wordIdx = words.length - 1;
      updateUI();
      
      // Auto scroll every few words to keep view centered
      if (wordIdx % 5 === 0) scrollToCurrent();
    }
  };
  
  utterance.onend = () => {
    isPlaying = false;
    document.getElementById('pbPlay').textContent = '▶';
  };
  
  synth.speak(utterance);
}

function pauseSpeech() {
  synth.cancel();
  isPlaying = false;
  document.getElementById('pbPlay').textContent = '▶';
}

function stopSpeech() {
  synth.cancel();
  isPlaying = false;
  document.getElementById('pbPlay').textContent = '▶';
}

function skip(seconds) {
  const delta = Math.round(seconds * 2.5); // Approx words per second
  wordIdx = Math.max(0, Math.min(words.length - 1, wordIdx + delta));
  updateUI();
  scrollToCurrent();
  if (isPlaying) resumeSpeech();
}

function setSpeed(val) {
  speechRate = parseFloat(val);
  document.getElementById('speedVal').textContent = speechRate.toFixed(1) + 'x';
  if (isPlaying) resumeSpeech();
}

function seek(e) {
  const track = document.getElementById('pbTrack');
  const rect = track.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const pct = x / rect.width;
  wordIdx = Math.floor(pct * words.length);
  updateUI();
  scrollToCurrent();
  if (isPlaying) resumeSpeech();
}

// ── FILE HANDLING ──
async function handleFile(input) {
  const file = input.files[0];
  if (!file) return;
  
  const reader = new FileReader();
  reader.onload = (e) => {
    const text = e.target.result;
    const newDoc = {
      id: Date.now(),
      name: file.name,
      text: text,
      progress: 0,
      wordIdx: 0
    };
    library.push(newDoc);
    saveStorage();
    renderLibrary();
    openDoc(newDoc.id);
    showToast('🔥 Document added!');
  };
  
  if (file.name.endsWith('.pdf')) {
    showToast('PDF support requires a backend. Try TXT or MD for now.');
  } else {
    reader.readAsText(file);
  }
}

async function submitURL() {
  const url = document.getElementById('urlIn').value;
  if (!url) return;
  
  showToast('Fetching article...');
  // Simulated fetch - in a real app, you'd use a proxy/backend
  setTimeout(() => {
    const newDoc = {
      id: Date.now(),
      name: "Article: " + url.split('/').pop() || "Web Article",
      text: "This is a simulated article content. To fetch real web content, a backend proxy is required to bypass CORS. FireRead Pro is designed to handle this seamlessly in a production environment.",
      progress: 0,
      wordIdx: 0
    };
    library.push(newDoc);
    saveStorage();
    renderLibrary();
    openDoc(newDoc.id);
    goBack();
    showToast('🔥 Article added!');
  }, 1500);
}

// ── VOICES ──
function loadVoices() {
  const getV = () => {
    voices = synth.getVoices().filter(v => v.lang.startsWith('en'));
    const sel = document.getElementById('voiceSel');
    if (voices.length) {
      sel.innerHTML = voices.map((v, i) => `<option value="${i}">${v.name}</option>`).join('');
      // Try to find a "Natural" voice
      const pref = voices.findIndex(v => v.name.includes('Natural') || v.name.includes('Premium'));
      if (pref !== -1) {
        selVoiceIdx = pref;
        sel.selectedIndex = pref;
      }
    }
  };
  getV();
  if (synth.onvoiceschanged !== undefined) synth.onvoiceschanged = getV;
}

function pickVoice() {
  selVoiceIdx = document.getElementById('voiceSel').selectedIndex;
  showToast('Voice updated');
}

function testVoice() {
  synth.cancel();
  const u = new SpeechSynthesisUtterance("Hello, I am your FireRead assistant. Ready to ignite your learning?");
  if (voices[selVoiceIdx]) u.voice = voices[selVoiceIdx];
  synth.speak(u);
}

// ── BOOKMARKS ──
function addBookmark() {
  if (!currentDoc) return;
  const snippet = words.slice(wordIdx, wordIdx + 10).map(w => w.text).join(' ');
  const bm = {
    id: Date.now(),
    docId: currentDoc.id,
    docName: currentDoc.name,
    wordIdx: wordIdx,
    snippet: snippet + "..."
  };
  bookmarks.push(bm);
  saveStorage();
  renderBookmarks();
  showToast('🔖 Bookmark added');
}

function renderBookmarks() {
  const list = document.getElementById('bookmarksList');
  if (!bookmarks.length) {
    list.innerHTML = `<div style="text-align: center; padding: 30px; color: var(--txt3);">No bookmarks yet.</div>`;
    return;
  }
  list.innerHTML = bookmarks.slice().reverse().map(bm => `
    <div class="lib-card" onclick="gotoBookmark(${bm.id})">
      <div class="lcb">
        <div class="lcn" style="font-size: 12px; color: var(--txt2);">${escapeHTML(bm.docName)}</div>
        <div class="lcm" style="color: var(--txt); font-style: italic;">"${escapeHTML(bm.snippet)}"</div>
      </div>
      <button class="lib-del" onclick="event.stopPropagation(); delBookmark(${bm.id})">🗑️</button>
    </div>
  `).join('');
}

function gotoBookmark(id) {
  const bm = bookmarks.find(b => b.id === id);
  if (bm) {
    const doc = library.find(d => d.id === bm.docId);
    if (doc) {
      loadDoc(doc);
      wordIdx = bm.wordIdx;
      updateUI();
      scrollToCurrent();
      goPage('page-reader');
    }
  }
}

function delBookmark(id) {
  bookmarks = bookmarks.filter(b => b.id !== id);
  saveStorage();
  renderBookmarks();
  showToast('Bookmark removed');
}

// ── HELPERS ──
function formatTime(s) {
  const m = Math.floor(s / 60);
  const rs = Math.floor(s % 60);
  return `${m}:${rs < 10 ? '0' : ''}${rs}`;
}

function calculateReadingTime(text) {
  const words = text.split(/\s+/).length;
  const mins = Math.ceil(words / 200);
  return mins + " min read";
}

function escapeHTML(str) {
  const p = document.createElement('p');
  p.textContent = str;
  return p.innerHTML;
}

function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 3000);
}

function spawnEmbers() {
  const c = document.getElementById('embers');
  for (let i = 0; i < 15; i++) {
    const e = document.createElement('div');
    e.className = 'ember';
    const sz = Math.random() * 4 + 2;
    e.style.cssText = `
      left: ${Math.random() * 100}%;
      width: ${sz}px;
      height: ${sz}px;
      background: ${['#ff3a00', '#ff6600', '#ffcc00'][Math.floor(Math.random() * 3)]};
      --d: ${Math.random() * 3 + 3}s;
      --dl: ${Math.random() * 5}s;
      --dx: ${Math.random() * 100 - 50}px;
    `;
    c.appendChild(e);
  }
}

function initQuotes() {
  const quotes = [
    {t: "The mind is not a vessel to be filled but a fire to be kindled.", a: "Plutarch"},
    {t: "An investment in knowledge pays the best interest.", a: "Benjamin Franklin"},
    {t: "The more that you read, the more things you will know.", a: "Dr. Seuss"}
  ];
  let i = 0;
  setInterval(() => {
    i = (i + 1) % quotes.length;
    document.getElementById('qText').textContent = `"${quotes[i].t}"`;
    document.getElementById('qAuth').textContent = `— ${quotes[i].a}`;
  }, 8000);
}

function changeFontSize(delta) {
  fontSize = Math.max(12, Math.min(32, fontSize + delta));
  document.getElementById('fontVal').textContent = fontSize;
  document.querySelectorAll('.page-content').forEach(el => el.style.setProperty('--fs', fontSize + 'px'));
}

</script>
</body>
</html>
