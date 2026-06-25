# ADV-mannger
<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ADV 腳本管理</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Serif+TC:wght@300;400;600&family=Noto+Sans+TC:wght@300;400;500&display=swap');

:root {
  --bg: #12100E;
  --surface: #1A1714;
  --surface2: #221F1B;
  --border: #2E2820;
  --accent: #E8752A;
  --accent2: #C4521A;
  --accent-glow: rgba(232,117,42,0.18);
  --text: #EDE3D4;
  --text-muted: #8A7A68;
  --text-dim: #4A3E30;
  --side: #D46A8A;   /* 支線 */
  --fun: #7EB8C0;    /* 角色趣聞 */
}

* { box-sizing: border-box; margin: 0; padding: 0; }
html, body { height: 100%; }
body {
  background: var(--bg);
  color: var(--text);
  font-family: 'Noto Sans TC', sans-serif;
  font-weight: 300;
  display: flex;
  flex-direction: column;
  height: 100vh;
  overflow: hidden;
}

/* ── Header ── */
header {
  flex-shrink: 0;
  padding: 10px 16px;
  border-bottom: 1px solid var(--border);
  display: flex;
  align-items: center;
  gap: 10px;
  background: var(--surface);
  z-index: 50;
  flex-wrap: wrap;
}
header h1 {
  font-family: 'Noto Serif TC', serif;
  font-weight: 300;
  font-size: 0.95rem;
  letter-spacing: 0.18em;
  color: var(--accent);
  white-space: nowrap;
}
.tabs { display: flex; gap: 2px; }
.tab-btn {
  background: none; border: none;
  color: var(--text-muted);
  font-family: 'Noto Sans TC', sans-serif;
  font-size: 0.72rem;
  padding: 5px 10px; border-radius: 4px; cursor: pointer; white-space: nowrap;
}
.tab-btn:hover { color: var(--text); background: rgba(255,255,255,0.03); }
.tab-btn.active { color: var(--accent); background: rgba(232,117,42,0.1); }
.header-right { margin-left: auto; display: flex; gap: 5px; align-items: center; }

/* ── Buttons ── */
.btn {
  background: none; border: 1px solid var(--accent); color: var(--accent);
  font-family: 'Noto Sans TC', sans-serif; font-size: 0.7rem;
  padding: 5px 11px; border-radius: 4px; cursor: pointer; white-space: nowrap;
}
.btn:hover { background: rgba(232,117,42,0.1); }
.btn-filled { background: var(--accent); color: #12100E; font-weight: 500; border-color: var(--accent); }
.btn-filled:hover { background: #f08040; }
.btn-danger { border-color: #c05060; color: #c05060; }
.btn-danger:hover { background: rgba(192,80,96,0.1); }

/* ── Layout ── */
.app-body { display: flex; flex: 1; overflow: hidden; }
.panel { flex: 1; overflow: hidden; display: none; flex-direction: column; }
.panel.active { display: flex; }

/* ══════════════════════════════
   TIMELINE
══════════════════════════════ */
.tl-toolbar {
  flex-shrink: 0; display: flex; align-items: center; gap: 10px;
  padding: 7px 16px; border-bottom: 1px solid var(--border); background: var(--surface);
}
.progress-bar-wrap { flex: 1; height: 3px; background: var(--surface2); border-radius: 2px; overflow: hidden; }
.progress-bar-fill { height: 100%; background: linear-gradient(90deg, var(--accent2), var(--accent)); border-radius: 2px; transition: width 0.4s; }
.progress-label { font-size: 0.68rem; color: var(--text-muted); white-space: nowrap; }

.timeline-scroll { flex: 1; overflow: auto; }
.timeline-inner { padding: 16px; min-width: max-content; }

.tl-month-header {
  display: flex; margin-left: 120px; margin-bottom: 4px;
  border-bottom: 1px solid var(--border); padding-bottom: 5px;
}
.tl-month-cell {
  width: 76px; flex-shrink: 0; text-align: center;
  font-size: 0.62rem; color: var(--text-muted); line-height: 1.4;
}
.tl-month-cell.cross-year { color: var(--accent2); }

.tl-row { display: flex; align-items: center; margin-bottom: 6px; }
.tl-label {
  width: 120px; flex-shrink: 0; padding-right: 10px;
  display: flex; align-items: center; gap: 6px;
}
.tl-stripe { width: 3px; height: 30px; border-radius: 2px; flex-shrink: 0; }
.tl-loop-name { font-size: 0.8rem; font-weight: 400; color: var(--text); }
.tl-loop-num { font-size: 0.6rem; color: var(--text-dim); }

.tl-track { display: flex; align-items: center; position: relative; }
.tl-track::before {
  content: ''; position: absolute; top: 50%; left: 0; right: 0;
  height: 1px; background: var(--border); z-index: 0;
}
.tl-cell {
  width: 76px; flex-shrink: 0; display: flex;
  justify-content: center; align-items: center;
  position: relative; z-index: 1; padding: 16px 0;
}
.tl-node {
  width: 24px; height: 24px; border-radius: 50%; cursor: pointer;
  display: flex; align-items: center; justify-content: center;
  font-size: 0.6rem; position: relative; transition: transform 0.15s;
  -webkit-tap-highlight-color: transparent;
}
.tl-node:active { transform: scale(0.88); }
.tl-node.done { background: var(--accent); box-shadow: 0 0 8px var(--accent-glow); color: #12100E; font-weight: 600; }
.tl-node.partial { background: var(--surface2); border: 2px solid var(--accent); color: var(--accent); }
.tl-node.empty { background: var(--surface); border: 1.5px dashed var(--border); color: var(--text-dim); }
.tl-chapter-label {
  position: absolute; top: calc(100% + 2px); left: 50%;
  transform: translateX(-50%); font-size: 0.52rem; color: var(--text-dim);
  white-space: nowrap; max-width: 72px; overflow: hidden;
  text-overflow: ellipsis; text-align: center; pointer-events: none;
}
.tl-node.done .tl-chapter-label { color: var(--accent); }
.tl-node.partial .tl-chapter-label { color: var(--accent2); }

.tl-legend {
  flex-shrink: 0; display: flex; gap: 14px; padding: 7px 16px;
  border-top: 1px solid var(--border); background: var(--surface);
}
.legend-item { display: flex; align-items: center; gap: 5px; font-size: 0.65rem; color: var(--text-muted); }
.legend-dot { width: 10px; height: 10px; border-radius: 50%; }
.legend-dot.done { background: var(--accent); }
.legend-dot.partial { background: var(--surface2); border: 1.5px solid var(--accent); }
.legend-dot.empty { background: var(--surface); border: 1.5px dashed var(--border); }

/* ══════════════════════════════
   CHAPTER MODAL (outline writing)
══════════════════════════════ */
.outline-modal {
  display: flex; flex-direction: column; height: 100%;
}
.outline-header {
  display: flex; align-items: baseline; gap: 10px; margin-bottom: 14px;
}
.outline-title {
  font-family: 'Noto Serif TC', serif; font-weight: 300;
  font-size: 1rem; letter-spacing: 0.1em; color: var(--accent); flex: 1;
}
.word-count { font-size: 0.68rem; color: var(--text-dim); white-space: nowrap; }

.outline-name-input {
  width: 100%; background: transparent;
  border: none; border-bottom: 1px solid var(--border);
  color: var(--text); font-family: 'Noto Serif TC', serif;
  font-size: 1rem; font-weight: 300; letter-spacing: 0.05em;
  padding: 4px 0 8px; outline: none; margin-bottom: 14px;
}
.outline-name-input:focus { border-bottom-color: var(--accent); }
.outline-name-input::placeholder { color: var(--text-dim); }

.status-row { display: flex; gap: 6px; flex-wrap: wrap; margin-bottom: 14px; }
.status-opt {
  padding: 4px 12px; border-radius: 20px; font-size: 0.7rem;
  cursor: pointer; border: 1px solid var(--border); color: var(--text-muted);
  background: none; font-family: 'Noto Sans TC', sans-serif;
  -webkit-tap-highlight-color: transparent;
}
.status-opt.s-done { border-color: var(--accent); color: var(--accent); background: rgba(232,117,42,0.1); }
.status-opt.s-partial { border-color: var(--accent2); color: var(--accent2); background: rgba(196,82,26,0.08); }
.status-opt.s-empty { border-color: var(--text-dim); color: var(--text-dim); }

.outline-textarea {
  flex: 1; width: 100%; background: var(--bg);
  border: 1px solid var(--border); border-radius: 6px;
  color: var(--text); font-family: 'Noto Sans TC', sans-serif;
  font-size: 0.9rem; font-weight: 300; line-height: 1.9;
  padding: 14px 16px; outline: none; resize: none;
  transition: border-color 0.2s;
  min-height: 200px;
}
.outline-textarea:focus { border-color: var(--accent); }
.outline-textarea::placeholder { color: var(--text-dim); }

.outline-footer {
  display: flex; justify-content: space-between; align-items: center;
  margin-top: 12px; gap: 8px;
}
.outline-wordcount { font-size: 0.68rem; color: var(--text-dim); }

/* SENTENCE LABELER */
.labeler-wrap { display: flex; flex-direction: column; height: 100%; }
.labeler-progress { font-size: 0.7rem; color: var(--text-muted); letter-spacing: 0.08em; text-align: right; margin-bottom: 10px; flex-shrink: 0; }
.labeler-sentence { width: 100%; background: var(--bg); border: 1px solid var(--accent); border-radius: 6px; color: var(--text); font-family: "Noto Sans TC", sans-serif; font-size: 1rem; font-weight: 300; line-height: 1.9; padding: 14px 16px; outline: none; resize: none; flex-shrink: 0; min-height: 100px; }
.labeler-hint { font-size: 0.65rem; color: var(--text-dim); text-align: right; margin: 4px 0 12px; flex-shrink: 0; }
.labeler-chars { display: grid; grid-template-columns: repeat(4,1fr); gap: 8px; margin-bottom: 14px; flex-shrink: 0; }
.labeler-char-btn { display: flex; flex-direction: column; align-items: center; padding: 10px 4px 8px; border: 1px solid var(--border); border-radius: 5px; background: var(--surface2); cursor: pointer; font-family: inherit; gap: 4px; transition: border-color 0.15s; -webkit-tap-highlight-color: transparent; }
.labeler-char-btn:hover { border-color: var(--accent); }
.labeler-char-btn.lbl-sel { border-color: var(--accent); background: var(--accent); }
.labeler-char-letter { font-size: 1.1rem; font-weight: 600; line-height: 1; color: var(--text); }
.labeler-char-btn.lbl-sel .labeler-char-letter { color: #12100E; }
.labeler-char-name { font-size: 0.65rem; color: var(--text-muted); }
.labeler-char-btn.lbl-sel .labeler-char-name { color: #12100E; opacity: 0.8; }
.labeler-nav { display: flex; gap: 8px; flex-shrink: 0; }
.labeler-nav-btn { flex: 1; padding: 10px; border: 1px solid var(--border); border-radius: 4px; background: var(--surface2); color: var(--text-muted); font-family: inherit; font-size: 0.8rem; cursor: pointer; letter-spacing: 0.06em; }
.labeler-nav-btn:hover { border-color: var(--accent); color: var(--accent); }
.labeler-nav-btn:disabled { opacity: 0.3; cursor: default; }
.labeler-nav-btn.lbl-primary { background: var(--accent); color: #12100E; border-color: var(--accent); font-weight: 500; }
.labeler-nav-btn.lbl-primary:hover { background: #f08040; }

/* ══════════════════════════════
   MATERIAL (支線/角色趣聞)
══════════════════════════════ */
.mat-panel { flex: 1; overflow-y: auto; padding: 16px; }
.mat-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; }
.mat-title { font-family: 'Noto Serif TC', serif; font-weight: 300; font-size: 0.95rem; letter-spacing: 0.1em; }
.mat-title small { font-family: 'Noto Sans TC', sans-serif; font-size: 0.62rem; color: var(--text-dim); margin-left: 8px; }

/* Char folders */
.folder-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(140px, 1fr)); gap: 10px; margin-bottom: 20px; }
.folder-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: 8px; padding: 14px 12px; cursor: pointer;
  display: flex; flex-direction: column; gap: 5px; position: relative;
  transition: border-color 0.2s; -webkit-tap-highlight-color: transparent;
}
.folder-card:hover { border-color: rgba(232,117,42,0.35); }
.folder-icon { font-size: 1.4rem; }
.folder-name { font-size: 0.82rem; font-weight: 400; }
.folder-count { font-size: 0.62rem; color: var(--text-dim); }
.folder-del { position: absolute; top: 6px; right: 6px; background: none; border: none; color: var(--text-dim); cursor: pointer; font-size: 0.75rem; opacity: 0; }
.folder-card:hover .folder-del { opacity: 1; }

/* Add folder */
.folder-add {
  background: none; border: 1.5px dashed var(--border);
  border-radius: 8px; padding: 14px 12px; cursor: pointer;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
  gap: 5px; color: var(--text-dim); font-size: 0.75rem; min-height: 80px;
  transition: border-color 0.2s, color 0.2s;
}
.folder-add:hover { border-color: var(--accent); color: var(--accent); }

/* Inside folder view */
.folder-view-header {
  display: flex; align-items: center; gap: 10px; margin-bottom: 16px;
}
.back-btn {
  background: none; border: none; color: var(--text-muted); cursor: pointer;
  font-size: 0.8rem; padding: 4px 8px; border-radius: 4px;
}
.back-btn:hover { color: var(--accent); background: rgba(232,117,42,0.08); }
.folder-view-name { font-family: 'Noto Serif TC', serif; font-weight: 300; font-size: 1rem; letter-spacing: 0.1em; }

.mat-type-label {
  font-size: 0.65rem; letter-spacing: 0.1em; color: var(--text-dim);
  border-bottom: 1px solid var(--border); padding-bottom: 5px; margin-bottom: 10px;
  display: flex; align-items: center; justify-content: space-between;
}

.items-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(220px, 1fr)); gap: 10px; margin-bottom: 20px; }
.item-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: 7px; padding: 12px 14px; cursor: pointer; position: relative;
  transition: border-color 0.2s; -webkit-tap-highlight-color: transparent;
}
.item-card:hover { border-color: rgba(232,117,42,0.3); }
.item-type-dot {
  width: 7px; height: 7px; border-radius: 50%; display: inline-block; margin-right: 6px; vertical-align: middle;
}
.item-type-dot.side { background: var(--side); }
.item-type-dot.fun { background: var(--fun); }
.item-tag { font-size: 0.6rem; letter-spacing: 0.07em; }
.item-tag.side { color: var(--side); }
.item-tag.fun { color: var(--fun); }
.item-name { font-size: 0.88rem; font-weight: 400; margin: 6px 0 4px; }
.item-sub { font-size: 0.72rem; color: var(--text-muted); line-height: 1.5; }
.item-actions { position: absolute; top: 8px; right: 8px; display: flex; gap: 4px; opacity: 0; }
.item-card:hover .item-actions { opacity: 1; }
.icon-btn {
  background: var(--surface2); border: 1px solid var(--border); color: var(--text-muted);
  width: 22px; height: 22px; border-radius: 3px; cursor: pointer;
  display: flex; align-items: center; justify-content: center; font-size: 0.68rem;
}
.icon-btn:hover { color: var(--accent); border-color: var(--accent); }
.icon-btn.del:hover { color: #c05060; border-color: #c05060; }

/* ══════════════════════════════
   MODAL
══════════════════════════════ */
.modal-overlay {
  display: none; position: fixed; inset: 0;
  background: rgba(8,6,4,0.88); z-index: 200;
  align-items: flex-end; justify-content: center; padding: 0;
}
.modal-overlay.open { display: flex; }

/* Full-screen chapter modal */
.modal-overlay.fullscreen { align-items: stretch; padding: 0; }
.modal-overlay.fullscreen .modal {
  width: 100%; max-width: 100%; border-radius: 0; border-left: none; border-right: none;
  height: 100vh; max-height: 100vh; display: flex; flex-direction: column;
}

/* Regular bottom sheet */
.modal {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: 12px 12px 0 0; padding: 20px 20px 32px;
  width: 100%; max-width: 560px; max-height: 88vh;
  overflow-y: auto; position: relative;
}
.modal-drag { width: 36px; height: 3px; background: var(--border); border-radius: 2px; margin: 0 auto 16px; }
.modal-title { font-family: 'Noto Serif TC', serif; font-weight: 300; font-size: 0.95rem; letter-spacing: 0.1em; margin-bottom: 18px; color: var(--accent); }
.modal-close { position: absolute; top: 12px; right: 14px; background: none; border: none; color: var(--text-muted); cursor: pointer; font-size: 1rem; }

.form-group { margin-bottom: 13px; }
.form-label { display: block; font-size: 0.68rem; color: var(--text-muted); letter-spacing: 0.06em; margin-bottom: 4px; }
.form-input, .form-textarea, .form-select {
  width: 100%; background: var(--bg); border: 1px solid var(--border); color: var(--text);
  font-family: 'Noto Sans TC', sans-serif; font-size: 0.82rem; padding: 8px 11px;
  border-radius: 4px; outline: none; -webkit-appearance: none;
}
.form-input:focus, .form-textarea:focus, .form-select:focus { border-color: var(--accent); }
.form-textarea { resize: vertical; min-height: 80px; line-height: 1.7; }
.form-actions { display: flex; gap: 7px; justify-content: flex-end; margin-top: 18px; padding-top: 14px; border-top: 1px solid var(--border); }

.type-selector { display: flex; gap: 7px; }
.type-opt {
  flex: 1; padding: 7px; border-radius: 5px; text-align: center;
  cursor: pointer; border: 1px solid var(--border); background: none;
  font-family: 'Noto Sans TC', sans-serif; font-size: 0.75rem; color: var(--text-muted);
}
.type-opt.sel-side { border-color: var(--side); color: var(--side); background: rgba(212,106,138,0.08); }
.type-opt.sel-fun { border-color: var(--fun); color: var(--fun); background: rgba(126,184,192,0.08); }

/* export textarea */
.export-box { min-height: 180px; font-size: 0.68rem; font-family: monospace; line-height: 1.5; }

::-webkit-scrollbar { width: 4px; height: 4px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }
</style>
</head>
<body>

<header>
  <h1>軌跡管理</h1>
  <div class="tabs">
    <button class="tab-btn active" onclick="switchTab('timeline')">⏱ 時間軸</button>
    <button class="tab-btn" onclick="switchTab('material')">📁 素材庫</button>
  </div>
  <div class="header-right">
    <button class="btn" onclick="openLoopManager()">⚙ 輪迴</button>
    <button class="btn" onclick="exportData()">💾 匯出</button>
    <button class="btn" onclick="importData()">📂 匯入</button>
  </div>
</header>

<div class="app-body">
  <!-- Timeline -->
  <div class="panel active" id="panel-timeline">
    <div class="tl-toolbar">
      <div class="progress-bar-wrap"><div class="progress-bar-fill" id="prog-fill" style="width:0%"></div></div>
      <div class="progress-label" id="prog-label">0 / 0</div>
    </div>
    <div class="timeline-scroll">
      <div class="timeline-inner" id="timeline-inner"></div>
    </div>
    <div class="tl-legend">
      <div class="legend-item"><div class="legend-dot done"></div>已完成</div>
      <div class="legend-item"><div class="legend-dot partial"></div>草稿中</div>
      <div class="legend-item"><div class="legend-dot empty"></div>未撰寫</div>
    </div>
  </div>

  <!-- Material -->
  <div class="panel" id="panel-material">
    <div class="mat-panel" id="mat-panel-inner"></div>
  </div>
</div>

<!-- Modal -->
<div class="modal-overlay" id="modal-overlay">
  <div class="modal" id="modal-box">
    <div class="modal-drag"></div>
    <button class="modal-close" onclick="closeModal()">✕</button>
    <div id="modal-content"></div>
  </div>
</div>

<script>
// ══════════════════════════════
// DATA
// ══════════════════════════════
var LOOP_COLORS = ['#E8752A','#D45A1A','#E8A030','#C47820','#D46A8A','#A05880','#8A70C0','#5A90C0','#50A898'];
var MONTH_SEQ = [9,10,11,12,1,2,3,4,5,6];

var db = {
  loops: [
    { id:'L1', name:'燎線', index:1 },
    { id:'L2', name:'第二線', index:2 },
    { id:'L3', name:'第三線', index:3 }
  ],
  chapters: [],   // { id, loopId, month, name, status, outline, tags }
  charFolders: [], // { id, name }
  items: []        // { id, folderId, type:'side'|'fun', title, content }
};

function uid() { return Math.random().toString(36).slice(2,9); }

// ══════════════════════════════
// TAB
// ══════════════════════════════
var currentTab = 'timeline';
function switchTab(tab) {
  currentTab = tab;
  ['timeline','material'].forEach(function(t) {
    document.getElementById('panel-'+t).classList.toggle('active', t === tab);
  });
  document.querySelectorAll('.tab-btn').forEach(function(b, i) {
    b.classList.toggle('active', ['timeline','material'][i] === tab);
  });
  if (tab === 'timeline') renderTimeline();
  else renderMaterial();
}

// ══════════════════════════════
// TIMELINE
// ══════════════════════════════
function renderTimeline() {
  var total = db.loops.length * MONTH_SEQ.length;
  var done = 0;
  for (var i = 0; i < db.chapters.length; i++) { if (db.chapters[i].status === 'done') done++; }
  var pct = total > 0 ? Math.round(done/total*100) : 0;
  document.getElementById('prog-fill').style.width = pct + '%';
  document.getElementById('prog-label').textContent = done + ' / ' + total + ' (' + pct + '%)';

  var html = '';
  // month header
  var mHead = '';
  for (var mi = 0; mi < MONTH_SEQ.length; mi++) {
    var mn = MONTH_SEQ[mi];
    var cross = mn <= 6;
    mHead += '<div class="tl-month-cell' + (cross ? ' cross-year' : '') + '">' + mn + '月' + (cross ? '<br><span style="font-size:0.5rem">次年</span>' : '') + '</div>';
  }
  html += '<div class="tl-month-header">' + mHead + '</div>';

  db.loops.forEach(function(loop, li) {
    var color = LOOP_COLORS[li % LOOP_COLORS.length];
    var cells = '';
    for (var mi = 0; mi < MONTH_SEQ.length; mi++) {
      var m = MONTH_SEQ[mi];
      var ch = findChapter(loop.id, m);
      var status = ch ? ch.status : 'empty';
      var icon = status === 'done' ? '✓' : status === 'partial' ? '…' : '+';
      var label = ch && ch.name ? ch.name : '';
      cells += '<div class="tl-cell"><div class="tl-node ' + status + '" onclick="openChapterModal(\'' + loop.id + '\',' + m + ')">' + icon + '<div class="tl-chapter-label">' + escHtml(label) + '</div></div></div>';
    }
    html += '<div class="tl-row"><div class="tl-label"><div class="tl-stripe" style="background:' + color + '"></div><div><div class="tl-loop-name">' + escHtml(loop.name) + '</div><div class="tl-loop-num">第 ' + loop.index + ' 輪</div></div></div><div class="tl-track">' + cells + '</div></div>';
  });

  document.getElementById('timeline-inner').innerHTML = html;
}

function findChapter(loopId, month) {
  for (var i = 0; i < db.chapters.length; i++) {
    if (db.chapters[i].loopId === loopId && db.chapters[i].month === month) return db.chapters[i];
  }
  return null;
}

// ── Chapter modal ──
var _chStatus = 'empty';
var _labelerLoopId = null;
var _labelerMonth = null;

// ── CHARS config (for labeler) ──
var CHARS = [
  { letter:'d', name:'千秋' },
  { letter:'g', name:'說那' },
  { letter:'m', name:'万霞' },
  { letter:'s', name:'春朔' },
  { letter:'k', name:'燎' },
  { letter:'b', name:'蘭紗' },
  { letter:'f', name:'柊' },
  { letter:'n', name:'旁白' }
];

function openChapterModal(loopId, monthNum) {
  monthNum = parseInt(monthNum);
  _labelerLoopId = loopId;
  _labelerMonth = monthNum;
  var loop = findById(db.loops, loopId);
  if (!loop) return;
  var ch = findChapter(loopId, monthNum);
  _chStatus = ch ? ch.status : 'empty';
  var monthLabel = monthNum <= 6 ? monthNum + '月（次年）' : monthNum + '月';

  var statusHtml = ['done','partial','empty'].map(function(s) {
    var labels = {done:'已完成', partial:'草稿中', empty:'未撰寫'};
    return '<button class="status-opt' + (_chStatus === s ? ' s-'+s : '') + '" onclick="pickStatus(\'' + s + '\')">' + labels[s] + '</button>';
  }).join('');

  var outline = ch ? (ch.outline || '') : '';
  var name = ch ? (ch.name || '') : '';
  var wordCount = countChars(outline);

  document.getElementById('modal-content').innerHTML =
    '<div class="outline-modal">' +
    '<div class="outline-header"><div class="outline-title">' + escHtml(loop.name) + '・' + monthLabel + '</div></div>' +
    '<input class="outline-name-input" id="ch-name" value="' + escAttr(name) + '" placeholder="章節標題（選填）">' +
    '<div class="status-row" id="status-row">' + statusHtml + '</div>' +
    '<div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:6px">' +
    '<span class="form-label" style="margin:0">大綱內容</span>' +
    '<button class="btn" style="font-size:0.65rem;padding:3px 9px;border-color:var(--text-dim);color:var(--text-muted)" onclick="startLabeler()">✦ 句子標記</button>' +
    '</div>' +
    '<textarea class="outline-textarea" id="ch-outline" placeholder="寫下這個月發生的事…場景、對話、情緒、轉折都可以。" oninput="updateWordCount(this)" style="min-height:280px;flex:1">' + escHtml(outline) + '</textarea>' +
    '<div class="outline-footer">' +
    '<span class="outline-wordcount" id="wc-display">' + wordCount + ' 字</span>' +
    '<div style="display:flex;gap:7px">' +
    (ch ? '<button class="btn btn-danger" onclick="deleteChapter(\'' + loopId + '\',' + monthNum + ')">刪除</button>' : '') +
    '<button class="btn" onclick="closeModal()">取消</button>' +
    '<button class="btn btn-filled" onclick="saveChapter(\'' + loopId + '\',' + monthNum + ')">儲存</button>' +
    '</div></div></div>';

  openModal(true);
}

function updateWordCount(el) {
  var wc = document.getElementById('wc-display');
  if (wc) wc.textContent = countChars(el.value) + ' 字';
}

function countChars(str) {
  return str ? str.replace(/\s/g,'').length : 0;
}

function pickStatus(s) {
  _chStatus = s;
  var all = ['done','partial','empty'];
  document.querySelectorAll('#status-row .status-opt').forEach(function(b, i) {
    b.className = 'status-opt' + (all[i] === s ? ' s-'+s : '');
  });
}

function saveChapter(loopId, monthNum) {
  monthNum = parseInt(monthNum);
  var name = document.getElementById('ch-name').value.trim();
  var outline = document.getElementById('ch-outline').value;
  var ch = findChapter(loopId, monthNum);
  if (ch) {
    ch.name = name; ch.outline = outline; ch.status = _chStatus;
  } else {
    db.chapters.push({ id:uid(), loopId:loopId, month:monthNum, name:name, outline:outline, status:_chStatus });
  }
  closeModal();
  renderTimeline();
}

function deleteChapter(loopId, monthNum) {
  monthNum = parseInt(monthNum);
  db.chapters = db.chapters.filter(function(c){ return !(c.loopId===loopId && c.month===monthNum); });
  closeModal();
  renderTimeline();
}

// ══════════════════════════════
// SENTENCE LABELER
// ══════════════════════════════
var _lblSentences = [];
var _lblLabels = [];
var _lblCurrent = 0;

function splitSentences(text) {
  var lines = text.split(/\n+/).map(function(l){ return l.trim(); }).filter(function(l){ return l.length > 0; });
  var result = [];
  for (var li = 0; li < lines.length; li++) {
    var line = lines[li];
    if (!/[「『」』]/.test(line)) { result.push(line); continue; }
    var tokens = [];
    var i = 0;
    while (i < line.length) {
      if (line[i] === '「' || line[i] === '『') {
        var open = line[i] === '「' ? '「' : '『';
        var close = open === '「' ? '」' : '』';
        var depth = 0, j = i;
        while (j < line.length) {
          if (line[j] === open) depth++;
          if (line[j] === close) { depth--; if (depth === 0) { j++; break; } }
          j++;
        }
        tokens.push({ type:'quote', text: line.slice(i, j) });
        i = j;
      } else {
        var j2 = i;
        while (j2 < line.length && line[j2] !== '「' && line[j2] !== '『') j2++;
        tokens.push({ type:'plain', text: line.slice(i, j2) });
        i = j2;
      }
    }
    var buffer = '';
    for (var ti = 0; ti < tokens.length; ti++) {
      var tok = tokens[ti];
      if (tok.type === 'quote') {
        buffer += tok.text;
      } else {
        var parts = tok.text.split(/(?<=[。！？…])/);
        for (var pi = 0; pi < parts.length; pi++) {
          buffer += parts[pi];
          if (/[。！？…」』]$/.test(buffer) && buffer.trim()) {
            result.push(buffer.trim()); buffer = '';
          }
        }
      }
      if (tok.type === 'quote' && /[」』][。！？…]?$/.test(buffer.trim())) {
        result.push(buffer.trim()); buffer = '';
      }
    }
    if (buffer.trim()) result.push(buffer.trim());
  }
  return result.length > 0 ? result : lines;
}

function startLabeler() {
  var ta = document.getElementById('ch-outline');
  if (!ta) return;
  var text = ta.value.trim();
  if (!text) { alert('請先輸入大綱內容再進行標記'); return; }
  _lblSentences = splitSentences(text);
  _lblLabels = new Array(_lblSentences.length).fill(null);
  _lblCurrent = 0;
  renderLabeler();
}

function renderLabeler() {
  var total = _lblSentences.length;
  var cur = _lblCurrent;
  var isLast = cur === total - 1;

  var charsHtml = CHARS.map(function(ch) {
    var sel = _lblLabels[cur] === ch.letter;
    return '<button class="labeler-char-btn' + (sel ? ' lbl-sel' : '') + '" onclick="lblSelect(\'' + ch.letter + '\')">' +
      '<span class="labeler-char-letter">' + ch.letter.toUpperCase() + '</span>' +
      '<span class="labeler-char-name">' + ch.name + '</span>' +
      '</button>';
  }).join('');

  document.getElementById('modal-content').innerHTML =
    '<div class="labeler-wrap">' +
    '<div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:10px">' +
    '<div style="font-size:0.82rem;color:var(--accent);font-family:Noto Serif TC,serif">句子標記</div>' +
    '<div class="labeler-progress">' + (cur+1) + ' / ' + total + '</div>' +
    '</div>' +
    '<textarea class="labeler-sentence" id="lbl-sentence" rows="4">' + escHtml(_lblSentences[cur]) + '</textarea>' +
    '<div class="labeler-hint">可直接編輯句子內容</div>' +
    '<div class="labeler-chars">' + charsHtml + '</div>' +
    '<div class="labeler-nav">' +
    '<button class="labeler-nav-btn" onclick="lblNav(-1)"' + (cur===0?' disabled':'') + '>← 上一句</button>' +
    '<button class="labeler-nav-btn lbl-primary" onclick="lblNav(1)">' + (isLast ? '完成 ✓' : '下一句 →') + '</button>' +
    '</div></div>';
}

function lblSaveEdit() {
  var box = document.getElementById('lbl-sentence');
  if (box) _lblSentences[_lblCurrent] = box.value;
}

function lblSelect(letter) {
  lblSaveEdit();
  _lblLabels[_lblCurrent] = _lblLabels[_lblCurrent] === letter ? null : letter;
  renderLabeler();
}

function lblNav(dir) {
  lblSaveEdit();
  if (dir === 1 && _lblCurrent === _lblSentences.length - 1) {
    lblFinish(); return;
  }
  _lblCurrent = Math.max(0, Math.min(_lblSentences.length-1, _lblCurrent + dir));
  renderLabeler();
}

function replaceQuotes(s) {
  if (/[「『」』]/.test(s)) {
    return s.replace(/[「『]/g, "'").replace(/[」』]/g, "'");
  }
  return "'" + s + "'";
}

var _lblMode = 'chapter'; // 'chapter' | 'item'

function lblFinish() {
  var result = _lblSentences.map(function(s, i) {
    var cleaned = replaceQuotes(s);
    var lbl = _lblLabels[i];
    return lbl ? lbl + ' ' + cleaned : cleaned;
  }).join('\n');

  if (_lblMode === 'item') {
    // Write back to item content and reopen item form
    var it = _itemEditId ? findById(db.items, _itemEditId) : null;
    renderItemForm(it ? Object.assign({}, it, {content: result}) : {title:'', content:result}, _itemFolderId);
    // manually set textarea value since escHtml might differ
    var ta = document.getElementById('item-content');
    if (ta) ta.value = result;
    var wc = document.getElementById('item-wc');
    if (wc) wc.textContent = countChars(result) + ' 字';
    return;
  }

  // chapter mode
  var loopId = _labelerLoopId;
  var monthNum = _labelerMonth;
  var loop = findById(db.loops, loopId);
  if (!loop) { closeModal(); return; }
  var ch = findChapter(loopId, monthNum);
  if (ch) {
    ch.outline = result;
  } else {
    db.chapters.push({ id:uid(), loopId:loopId, month:monthNum, name:'', outline:result, status:_chStatus });
  }
  openChapterModal(loopId, monthNum);
}

// ── Loop manager ──
function openLoopManager() {
  renderLoopManagerHTML();
  openModal(false);
}
function renderLoopManagerHTML() {
  var items = db.loops.map(function(l, i) {
    return '<div style="display:flex;align-items:center;gap:8px;background:var(--surface2);border:1px solid var(--border);border-radius:5px;padding:9px 12px;margin-bottom:7px">' +
      '<div style="width:9px;height:9px;border-radius:50%;background:' + LOOP_COLORS[i%LOOP_COLORS.length] + ';flex-shrink:0"></div>' +
      '<div style="flex:1;font-size:0.82rem">' + escHtml(l.name) + '</div>' +
      '<div style="font-size:0.62rem;color:var(--text-dim)">第' + l.index + '輪</div>' +
      '<button class="icon-btn" onclick="editLoopPrompt(\'' + l.id + '\')">✎</button>' +
      '<button class="icon-btn del" onclick="deleteLoop(\'' + l.id + '\')">✕</button>' +
      '</div>';
  }).join('');
  document.getElementById('modal-content').innerHTML =
    '<div class="modal-title">管理輪迴線</div>' +
    items +
    '<div class="form-group" style="margin-top:10px"><label class="form-label">新增輪迴線名稱</label>' +
    '<input class="form-input" id="new-loop-name" placeholder="如：燎線、鏡線…"></div>' +
    '<button class="btn btn-filled" style="width:100%" onclick="addLoop()">＋ 新增</button>' +
    '<div class="form-actions"><button class="btn btn-filled" onclick="closeModal()">完成</button></div>';
}
function addLoop() {
  var el = document.getElementById('new-loop-name');
  var name = el ? el.value.trim() : '';
  if (!name) return;
  db.loops.push({ id:uid(), name:name, index: db.loops.length+1 });
  renderLoopManagerHTML();
  renderTimeline();
}
function editLoopPrompt(id) {
  var loop = findById(db.loops, id);
  document.getElementById('modal-content').innerHTML =
    '<div class="modal-title">編輯輪迴線</div>' +
    '<div class="form-group"><label class="form-label">名稱</label>' +
    '<input class="form-input" id="edit-loop-name" value="' + escAttr(loop.name) + '"></div>' +
    '<div class="form-group"><label class="form-label">輪迴編號</label>' +
    '<input class="form-input" id="edit-loop-idx" type="number" value="' + loop.index + '"></div>' +
    '<div class="form-actions">' +
    '<button class="btn" onclick="openLoopManager()">取消</button>' +
    '<button class="btn btn-filled" onclick="confirmEditLoop(\'' + id + '\')">儲存</button>' +
    '</div>';
}
function confirmEditLoop(id) {
  var loop = findById(db.loops, id);
  var name = document.getElementById('edit-loop-name').value.trim();
  var idx = parseInt(document.getElementById('edit-loop-idx').value);
  if (name) loop.name = name;
  if (!isNaN(idx)) loop.index = idx;
  renderLoopManagerHTML();
  renderTimeline();
}
function deleteLoop(id) {
  var loop = findById(db.loops, id);
  document.getElementById('modal-content').innerHTML =
    '<div class="modal-title">刪除輪迴線</div>' +
    '<p style="font-size:0.82rem;color:var(--text-muted);line-height:1.8;margin-bottom:20px">確定刪除「' + escHtml(loop?loop.name:'') + '」及其所有章節？</p>' +
    '<div class="form-actions">' +
    '<button class="btn" onclick="openLoopManager()">取消</button>' +
    '<button class="btn btn-danger" onclick="confirmDeleteLoop(\'' + id + '\')">確定刪除</button>' +
    '</div>';
}
function confirmDeleteLoop(id) {
  db.loops = db.loops.filter(function(l){ return l.id !== id; });
  db.chapters = db.chapters.filter(function(c){ return c.loopId !== id; });
  closeModal();
  renderLoopManagerHTML();
  renderTimeline();
}

// ══════════════════════════════
// MATERIAL (素材庫)
// ══════════════════════════════
var _currentFolderId = null;

function renderMaterial() {
  if (_currentFolderId) {
    renderFolderView(_currentFolderId);
  } else {
    renderFolderList();
  }
}

function renderFolderList() {
  var folders = db.charFolders;
  var folderCards = folders.map(function(f) {
    var count = db.items.filter(function(it){ return it.folderId === f.id; }).length;
    return '<div class="folder-card" onclick="openFolder(\'' + f.id + '\')">' +
      '<button class="folder-del" onclick="event.stopPropagation();deleteFolder(\'' + f.id + '\')">✕</button>' +
      '<div class="folder-icon">📂</div>' +
      '<div class="folder-name">' + escHtml(f.name) + '</div>' +
      '<div class="folder-count">' + count + ' 筆</div>' +
      '</div>';
  }).join('');

  document.getElementById('mat-panel-inner').innerHTML =
    '<div class="mat-header"><div class="mat-title">角色夾 <small>' + folders.length + ' 個</small></div></div>' +
    '<div class="folder-grid">' + folderCards +
    '<button class="folder-add" onclick="addFolder()"><span style="font-size:1.4rem">＋</span><span>新增角色夾</span></button>' +
    '</div>';
}

function addFolder() {
  document.getElementById('modal-content').innerHTML =
    '<div class="modal-title">新增角色夾</div>' +
    '<div class="form-group"><label class="form-label">角色名稱</label>' +
    '<input class="form-input" id="new-folder-name" placeholder="例：燎、千秋…" autofocus></div>' +
    '<div class="form-actions">' +
    '<button class="btn" onclick="closeModal()">取消</button>' +
    '<button class="btn btn-filled" onclick="confirmAddFolder()">新增</button>' +
    '</div>';
  openModal(false);
  setTimeout(function(){ var el=document.getElementById('new-folder-name'); if(el) el.focus(); }, 100);
}
function confirmAddFolder() {
  var el = document.getElementById('new-folder-name');
  var name = el ? el.value.trim() : '';
  if (!name) { alert('請輸入角色名稱'); return; }
  db.charFolders.push({ id:uid(), name:name });
  closeModal();
  renderFolderList();
}
function deleteFolder(id) {
  var folder = findById(db.charFolders, id);
  document.getElementById('modal-content').innerHTML =
    '<div class="modal-title">刪除角色夾</div>' +
    '<p style="font-size:0.82rem;color:var(--text-muted);line-height:1.8;margin-bottom:20px">確定刪除「' + escHtml(folder?folder.name:'') + '」及其所有內容？</p>' +
    '<div class="form-actions">' +
    '<button class="btn" onclick="closeModal()">取消</button>' +
    '<button class="btn btn-danger" onclick="confirmDeleteFolder(\'' + id + '\')">確定刪除</button>' +
    '</div>';
  openModal(false);
}
function confirmDeleteFolder(id) {
  db.charFolders = db.charFolders.filter(function(f){ return f.id !== id; });
  db.items = db.items.filter(function(it){ return it.folderId !== id; });
  closeModal();
  _currentFolderId = null;
  renderFolderList();
}
function openFolder(id) {
  _currentFolderId = id;
  renderFolderView(id);
}

function renderFolderView(folderId) {
  var folder = findById(db.charFolders, folderId);
  if (!folder) { _currentFolderId = null; renderFolderList(); return; }

  var sides = db.items.filter(function(it){ return it.folderId === folderId && it.type === 'side'; });
  var funs = db.items.filter(function(it){ return it.folderId === folderId && it.type === 'fun'; });

  function itemCard(it) {
    return '<div class="item-card" onclick="openItemDetail(\'' + it.id + '\')">' +
      '<div class="item-actions"><button class="icon-btn" onclick="event.stopPropagation();openItemEdit(\'' + it.id + '\')">✎</button><button class="icon-btn del" onclick="event.stopPropagation();deleteItem(\'' + it.id + '\')">✕</button></div>' +
      '<div><span class="item-type-dot ' + it.type + '"></span><span class="item-tag ' + it.type + '">' + (it.type==='side'?'支線':'角色趣聞') + '</span></div>' +
      '<div class="item-name">' + escHtml(it.title) + '</div>' +
      '<div class="item-sub">' + escHtml((it.content||'').slice(0,60)) + (it.content && it.content.length>60?'…':'') + '</div>' +
      '</div>';
  }

  var sidesHtml = sides.length
    ? '<div class="items-grid">' + sides.map(itemCard).join('') + '</div>'
    : '<div style="font-size:0.75rem;color:var(--text-dim);padding:8px 0 14px">尚無支線內容</div>';
  var funsHtml = funs.length
    ? '<div class="items-grid">' + funs.map(itemCard).join('') + '</div>'
    : '<div style="font-size:0.75rem;color:var(--text-dim);padding:8px 0 14px">尚無角色趣聞內容</div>';

  document.getElementById('mat-panel-inner').innerHTML =
    '<div class="folder-view-header">' +
    '<button class="back-btn" onclick="backToFolders()">← 返回</button>' +
    '<div class="folder-view-name">📂 ' + escHtml(folder.name) + '</div>' +
    '<button class="btn btn-filled" style="margin-left:auto" onclick="openItemAdd(\'' + folderId + '\')">＋ 新增</button>' +
    '</div>' +
    '<div class="mat-type-label"><span style="color:var(--side)">◆ 支線</span><span style="font-size:0.6rem;color:var(--text-dim)">' + sides.length + ' 筆</span></div>' +
    sidesHtml +
    '<div class="mat-type-label" style="margin-top:4px"><span style="color:var(--fun)">◆ 角色趣聞</span><span style="font-size:0.6rem;color:var(--text-dim)">' + funs.length + ' 筆</span></div>' +
    funsHtml;
}

function backToFolders() {
  _currentFolderId = null;
  renderFolderList();
}

// ── Item CRUD ──
var _itemType = 'side';

var _itemFolderId = null;
var _itemEditId = null;

function openItemAdd(folderId) {
  _itemType = 'side';
  _itemFolderId = folderId;
  _itemEditId = null;
  renderItemForm(null, folderId);
  openModal(true);
}
function openItemEdit(id) {
  var it = findById(db.items, id);
  if (!it) return;
  _itemType = it.type;
  _itemFolderId = it.folderId;
  _itemEditId = id;
  renderItemForm(it, it.folderId);
  openModal(true);
}
function openItemDetail(id) {
  var it = findById(db.items, id);
  if (!it) return;
  document.getElementById('modal-content').innerHTML =
    '<div class="modal-title">' + escHtml(it.title) + '</div>' +
    '<div style="display:flex;align-items:center;gap:6px;margin-bottom:14px">' +
    '<span class="item-type-dot ' + it.type + '"></span>' +
    '<span class="item-tag ' + it.type + '" style="font-size:0.72rem">' + (it.type==='side'?'支線':'角色趣聞') + '</span>' +
    '</div>' +
    '<div style="font-size:0.85rem;line-height:1.9;color:var(--text);white-space:pre-wrap">' + escHtml(it.content||'') + '</div>' +
    '<div class="form-actions"><button class="btn" onclick="closeModal()">關閉</button><button class="btn btn-filled" onclick="closeModal();openItemEdit(\'' + id + '\')">編輯</button></div>';
  openModal(false);
}

function renderItemForm(item, folderId) {
  var title = item ? (item.title||'') : '';
  var itemContent = item ? (item.content||'') : '';
  var isSide = _itemType === 'side';
  var typeHtml = '<div class="type-selector" style="margin-bottom:13px">' +
    '<button class="type-opt' + (isSide?' sel-side':'') + '" onclick="pickItemType(\'side\',this)">支線</button>' +
    '<button class="type-opt' + (!isSide?' sel-fun':'') + '" onclick="pickItemType(\'fun\',this)">角色趣聞</button>' +
    '</div>';
  var labelRow = '<div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:6px">' +
    '<span class="form-label" style="margin:0">內容</span>' +
    (isSide ? '<button class="btn" style="font-size:0.65rem;padding:3px 9px;border-color:var(--text-dim);color:var(--text-muted)" onclick="startItemLabeler()">✦ 句子標記</button>' : '') +
    '</div>';
  var wc = countChars(itemContent);
  document.getElementById('modal-content').innerHTML =
    '<div class="outline-modal">' +
    '<div class="outline-header"><div class="outline-title">' + (item?'編輯':'新增') + '內容</div></div>' +
    typeHtml +
    '<div class="form-group"><label class="form-label">標題</label><input class="form-input" id="item-title" value="' + escAttr(title) + '" placeholder="簡短標題"></div>' +
    labelRow +
    '<textarea class="outline-textarea" id="item-content" placeholder="寫下內容…" oninput="updateItemWordCount(this)" style="min-height:280px;flex:1">' + escHtml(itemContent) + '</textarea>' +
    '<div class="outline-footer">' +
    '<span class="outline-wordcount" id="item-wc">' + wc + ' 字</span>' +
    '<div style="display:flex;gap:7px">' +
    (item && item.id ? '<button class="btn btn-danger" onclick="deleteItem(\'' + item.id + '\')">刪除</button>' : '') +
    '<button class="btn" onclick="closeModal()">取消</button>' +
    '<button class="btn btn-filled" onclick="saveItem(\'' + (item&&item.id?item.id:'') + '\',\'' + folderId + '\')">儲存</button>' +
    '</div></div></div>';
}

function updateItemWordCount(el) {
  var wc = document.getElementById('item-wc');
  if (wc) wc.textContent = countChars(el.value) + ' 字';
}

function startItemLabeler() {
  var ta = document.getElementById('item-content');
  if (!ta) return;
  var text = ta.value.trim();
  if (!text) { alert('請先輸入內容再進行標記'); return; }
  _lblSentences = splitSentences(text);
  _lblLabels = new Array(_lblSentences.length).fill(null);
  _lblCurrent = 0;
  _lblMode = 'item';
  renderLabeler();
}

function pickItemType(type, btn) {
  _itemType = type;
  // save current values before re-render
  var titleEl = document.getElementById('item-title');
  var contentEl = document.getElementById('item-content');
  var savedTitle = titleEl ? titleEl.value : '';
  var savedContent = contentEl ? contentEl.value : '';
  var folderId = _itemFolderId;
  var itemId = _itemEditId;
  var fakeItem = itemId ? { id:itemId, title:savedTitle, content:savedContent, type:type, folderId:folderId } : null;
  renderItemForm(fakeItem, folderId);
  // restore content
  var ta = document.getElementById('item-content');
  if (ta) { ta.value = savedContent; updateItemWordCount(ta); }
  var ti = document.getElementById('item-title');
  if (ti) ti.value = savedTitle;
}

function saveItem(id, folderId) {
  var title = document.getElementById('item-title').value.trim();
  var content = document.getElementById('item-content').value;
  if (!title) { alert('請輸入標題'); return; }
  if (id) {
    var it = findById(db.items, id);
    if (it) { it.title = title; it.content = content; it.type = _itemType; }
  } else {
    db.items.push({ id:uid(), folderId:folderId, type:_itemType, title:title, content:content });
  }
  closeModal();
  renderFolderView(folderId || _currentFolderId);
}

function deleteItem(id) {
  var it = findById(db.items, id);
  var fid = it ? it.folderId : _currentFolderId;
  document.getElementById('modal-content').innerHTML =
    '<div class="modal-title">確定刪除？</div>' +
    '<p style="font-size:0.82rem;color:var(--text-muted);line-height:1.8;margin-bottom:20px">「' + escHtml(it?it.title:'') + '」刪除後無法復原。</p>' +
    '<div class="form-actions">' +
    '<button class="btn" onclick="closeModal()">取消</button>' +
    '<button class="btn btn-danger" onclick="confirmDeleteItem(\'' + id + '\',\'' + fid + '\')">確定刪除</button>' +
    '</div>';
  openModal(false);
}
function confirmDeleteItem(id, fid) {
  db.items = db.items.filter(function(x){ return x.id !== id; });
  closeModal();
  renderFolderView(fid);
}

// ══════════════════════════════
// EXPORT / IMPORT
// ══════════════════════════════
function exportData() {
  var json = JSON.stringify(db, null, 2);
  document.getElementById('modal-content').innerHTML =
    '<div class="modal-title">💾 匯出資料</div>' +
    '<p style="font-size:0.75rem;color:var(--text-muted);margin-bottom:10px;line-height:1.7">全選複製以下文字，貼到備忘錄存檔。下次用「匯入」貼回。</p>' +
    '<textarea class="form-textarea export-box" id="export-box" readonly>' + json + '</textarea>' +
    '<div class="form-actions"><button class="btn" onclick="closeModal()">關閉</button><button class="btn btn-filled" onclick="selCopy()">全選複製</button></div>';
  openModal(false);
  setTimeout(function(){ var b=document.getElementById('export-box'); if(b) b.select(); },100);
}
function selCopy() {
  var b=document.getElementById('export-box');
  if(b){b.select();try{document.execCommand('copy');alert('已複製！');}catch(e){alert('請手動全選複製');}}
}
function importData() {
  document.getElementById('modal-content').innerHTML =
    '<div class="modal-title">📂 匯入資料</div>' +
    '<p style="font-size:0.75rem;color:var(--text-muted);margin-bottom:10px;line-height:1.7">將備份文字貼到下方，點確認匯入。</p>' +
    '<textarea class="form-textarea export-box" id="import-box" placeholder="貼上備份 JSON…"></textarea>' +
    '<div class="form-actions"><button class="btn" onclick="closeModal()">取消</button><button class="btn btn-filled" onclick="confirmImport()">確認匯入</button></div>';
  openModal(false);
}
function confirmImport() {
  var text = document.getElementById('import-box').value.trim();
  if (!text) { alert('請貼上備份文字'); return; }
  try {
    var parsed = JSON.parse(text);
    if (parsed && parsed.loops) {
      db = parsed;
      if (!db.charFolders) db.charFolders = [];
      if (!db.items) db.items = [];
      closeModal();
      renderTimeline();
      alert('匯入成功！');
    } else { alert('格式不正確'); }
  } catch(e) { alert('解析失敗：' + e.message); }
}

// ══════════════════════════════
// MODAL
// ══════════════════════════════
function openModal(fullscreen) {
  var overlay = document.getElementById('modal-overlay');
  overlay.classList.add('open');
  overlay.classList.toggle('fullscreen', !!fullscreen);
}
function closeModal() {
  document.getElementById('modal-overlay').classList.remove('open','fullscreen');
}
document.getElementById('modal-overlay').addEventListener('click', function(e) {
  if (e.target === this) closeModal();
});

// ══════════════════════════════
// UTIL
// ══════════════════════════════
function findById(arr, id) {
  for (var i=0;i<arr.length;i++) { if(arr[i].id===id||arr[i]._id===id) return arr[i]; }
  return null;
}
function escHtml(s) {
  if (!s) return '';
  return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;');
}
function escAttr(s) { return escHtml(s); }

// ── Init ──
renderTimeline();
</script>
</body>
</html>
