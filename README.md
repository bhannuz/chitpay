<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>AK Chits & Enterprises</title>
<link href="https://fonts.googleapis.com/css2?family=Sora:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<style>
:root{
  --bg:#0a0f1e;--surface:#111827;--surface2:#1a2235;
  --accent:#f59e0b;--text:#f1f5f9;--muted:#64748b;
  --border:rgba(255,255,255,0.08);
  --gold:linear-gradient(135deg,#f59e0b,#fbbf24,#d97706);
  --green:linear-gradient(135deg,#10b981,#34d399);
}
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
body{font-family:'Sora',sans-serif;background:var(--bg);color:var(--text);min-height:100vh;max-width:430px;margin:0 auto;overflow-x:hidden;}
body::before{content:'';position:fixed;top:-200px;right:-200px;width:500px;height:500px;background:radial-gradient(circle,rgba(245,158,11,.12) 0%,transparent 70%);pointer-events:none;z-index:0;}
body::after{content:'';position:fixed;bottom:-150px;left:-150px;width:400px;height:400px;background:radial-gradient(circle,rgba(16,185,129,.1) 0%,transparent 70%);pointer-events:none;z-index:0;}

/* TOPBAR */
.topbar{position:sticky;top:0;z-index:100;background:rgba(10,15,30,.97);backdrop-filter:blur(20px);border-bottom:1px solid var(–border);}
.topbar-main{padding:12px 20px;display:flex;align-items:center;justify-content:space-between;}
.topbar-brand{display:flex;flex-direction:column;}
.brand-name{font-size:16px;font-weight:800;background:var(–gold);-webkit-background-clip:text;-webkit-text-fill-color:transparent;line-height:1.1;}
.brand-sub{font-size:9px;color:var(–muted);font-weight:600;letter-spacing:.8px;text-transform:uppercase;margin-top:1px;}
.topbar-right{display:flex;align-items:center;gap:8px;}
.admin-badge{background:rgba(245,158,11,.12);border:1px solid rgba(245,158,11,.3);border-radius:8px;padding:5px 10px;font-size:11px;color:var(–accent);font-weight:700;}
.sync-badge{font-size:11px;font-weight:600;padding:4px 10px;border-radius:8px;transition:all .3s;}
.sync-badge.synced{background:rgba(16,185,129,.15);color:#34d399;border:1px solid rgba(16,185,129,.3);}
.sync-badge.syncing{background:rgba(245,158,11,.15);color:#fbbf24;border:1px solid rgba(245,158,11,.3);}
.sync-badge.error{background:rgba(239,68,68,.15);color:#ef4444;border:1px solid rgba(239,68,68,.3);}

/* PAGES */
.page{display:none;animation:fadeUp .3s ease;}
.page.active{display:block;}
@keyframes fadeUp{from{opacity:0;transform:translateY(12px)}to{opacity:1;transform:translateY(0)}}
.content{padding:20px 20px 170px;}

/* BOTTOM NAV */
.bottom-nav{position:fixed;bottom:0;left:50%;transform:translateX(-50%);width:100%;max-width:430px;background:rgba(17,24,39,.97);backdrop-filter:blur(20px);border-top:1px solid var(–border);display:flex;flex-direction:column;z-index:100;}
.quick-add-bar{display:flex;gap:8px;padding:8px 12px;border-top:1px solid var(–border);background:rgba(10,15,30,.95);}
.quick-btn{flex:1;display:flex;align-items:center;justify-content:center;gap:6px;padding:9px;border-radius:12px;font-size:12px;font-weight:700;cursor:pointer;border:1px solid;transition:all .2s;}
.quick-btn:active{transform:scale(.97);}
.quick-btn.member{background:rgba(245,158,11,.08);border-color:rgba(245,158,11,.3);color:var(–accent);}
.quick-btn.group{background:rgba(16,185,129,.08);border-color:rgba(16,185,129,.3);color:#34d399;}
.bottom-nav-btns{display:flex;padding:8px 0 calc(8px + env(safe-area-inset-bottom));}
.nav-item{flex:1;display:flex;flex-direction:column;align-items:center;gap:4px;cursor:pointer;padding:6px 0;transition:all .2s;}
.nav-icon{font-size:20px;transition:transform .2s;}
.nav-label{font-size:10px;font-weight:500;color:var(–muted);transition:color .2s;}
.nav-item.active .nav-label{color:var(–accent);}
.nav-item.active .nav-icon{transform:scale(1.15);}

/* CARDS & COMMON */
.card{background:var(–surface);border:1px solid var(–border);border-radius:20px;padding:20px;margin-bottom:16px;}
.section-title{font-size:12px;font-weight:700;color:var(–muted);text-transform:uppercase;letter-spacing:1px;margin-bottom:12px;}
.form-label{font-size:11px;color:var(–muted);font-weight:700;margin-bottom:8px;display:block;letter-spacing:.6px;text-transform:uppercase;}
.form-input{width:100%;background:var(–surface2);border:1px solid var(–border);border-radius:12px;padding:13px 16px;color:var(–text);font-family:‘Sora’,sans-serif;font-size:14px;outline:none;transition:border-color .2s;appearance:none;}
.form-input:focus{border-color:rgba(245,158,11,.5);}
.form-input option{background:#1a2235;}
.form-input:disabled{opacity:.4;cursor:not-allowed;}
.form-group{margin-bottom:14px;}

/* CONNECTION STATUS BANNER */
.conn-banner{border-radius:14px;padding:14px 16px;margin-bottom:16px;display:flex;align-items:center;gap:10px;font-size:13px;font-weight:600;}
.conn-banner.ok{background:rgba(16,185,129,.1);border:1px solid rgba(16,185,129,.3);color:#34d399;}
.conn-banner.fail{background:rgba(239,68,68,.1);border:1px solid rgba(239,68,68,.3);color:#ef4444;}
.conn-banner.checking{background:rgba(245,158,11,.1);border:1px solid rgba(245,158,11,.3);color:#fbbf24;}

/* HOME STATS ROW */
.stats-row{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:16px;}
.stat-box{background:var(–surface);border:1px solid var(–border);border-radius:14px;padding:14px 10px;text-align:center;}
.stat-val{font-size:18px;font-weight:800;font-family:‘JetBrains Mono’,monospace;}
.stat-lbl{font-size:9px;color:var(–muted);font-weight:600;text-transform:uppercase;letter-spacing:.4px;margin-top:3px;}

/* ACTION BTN */
.action-btn{border-radius:20px;padding:20px 16px;cursor:pointer;border:1px solid;display:flex;align-items:center;gap:14px;transition:transform .15s;margin-bottom:14px;}
.action-btn:active{transform:scale(.97);}
.action-btn.add{background:linear-gradient(135deg,#1c1a0a,#1a160a);border-color:rgba(245,158,11,.4);}
.ab-icon{font-size:28px;flex-shrink:0;}
.ab-text .ab-title{font-size:14px;font-weight:700;}
.ab-text .ab-sub{font-size:11px;color:var(–muted);margin-top:3px;}
.action-btn.add .ab-title{background:var(–gold);-webkit-background-clip:text;-webkit-text-fill-color:transparent;}

/* SELECTOR CARD */
.selector-card{background:var(–surface);border:1px solid var(–border);border-radius:18px;padding:16px;margin-bottom:14px;}

/* GROUP PROGRESS */
.group-progress-card{background:var(–surface);border:1px solid var(–border);border-radius:18px;padding:16px;margin-bottom:14px;}
.gp-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:12px;}
.gp-name{font-size:15px;font-weight:700;}
.gp-badge{font-size:10px;font-weight:700;padding:3px 8px;border-radius:6px;background:rgba(245,158,11,.15);color:var(–accent);border:1px solid rgba(245,158,11,.3);}
.gp-stats{display:grid;grid-template-columns:repeat(3,1fr);gap:8px;margin-bottom:12px;}
.gp-stat{background:var(–surface2);border-radius:10px;padding:10px 8px;text-align:center;}
.gp-stat-val{font-size:14px;font-weight:800;font-family:‘JetBrains Mono’,monospace;}
.gp-stat-lbl{font-size:9px;color:var(–muted);font-weight:600;text-transform:uppercase;letter-spacing:.4px;margin-top:3px;}
.progress-bar{height:6px;background:var(–surface2);border-radius:6px;overflow:hidden;}
.progress-fill{height:100%;border-radius:6px;background:linear-gradient(90deg,#f59e0b,#fbbf24);transition:width .6s ease;}
.progress-labels{display:flex;justify-content:space-between;margin-top:5px;}
.pl-txt{font-size:10px;color:var(–muted);}
.pl-pct{font-size:10px;color:var(–accent);font-weight:700;}

/* MEMBER PANEL */
.member-panel{display:none;}
.member-panel.visible{display:block;animation:fadeUp .3s ease;}
.amt-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:14px;}
.amt-box{background:var(–surface);border:1px solid var(–border);border-radius:14px;padding:14px;}
.amt-val{font-size:18px;font-weight:800;font-family:‘JetBrains Mono’,monospace;margin-top:4px;}
.amt-lbl{font-size:10px;color:var(–muted);font-weight:600;text-transform:uppercase;letter-spacing:.4px;}

/* TABLE */
.pay-table-wrap{border-radius:14px;overflow:hidden;border:1px solid var(–border);margin-bottom:14px;overflow-x:auto;}
.pay-table{width:100%;border-collapse:collapse;font-size:11px;}
.pay-table th{background:var(–surface2);padding:9px 8px;text-align:left;font-size:9px;color:var(–muted);font-weight:700;text-transform:uppercase;letter-spacing:.4px;border-bottom:1px solid var(–border);white-space:nowrap;}
.pay-table td{padding:9px 8px;border-bottom:1px solid var(–border);color:var(–text);white-space:nowrap;}
.pay-table tr:last-child td{border-bottom:none;}
.pay-table tr:nth-child(even) td{background:rgba(255,255,255,.02);}

/* MEMBERS PAGE */
.search-bar{background:var(–surface2);border:1px solid var(–border);border-radius:14px;padding:12px 16px;display:flex;align-items:center;gap:10px;margin-bottom:14px;}
.search-bar input{flex:1;background:none;border:none;outline:none;color:var(–text);font-family:‘Sora’,sans-serif;font-size:14px;}
.search-bar input::placeholder{color:var(–muted);}
.member-card{background:var(–surface);border:1px solid var(–border);border-radius:16px;padding:14px;margin-bottom:10px;display:flex;align-items:center;gap:12px;cursor:pointer;transition:background .2s;}
.member-card:active{background:var(–surface2);}
.mc-avatar{width:44px;height:44px;border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:16px;font-weight:700;flex-shrink:0;}
.mc-info{flex:1;min-width:0;}
.mc-name{font-size:14px;font-weight:600;}
.mc-sub{font-size:11px;color:var(–muted);margin-top:2px;}
.mc-right{text-align:right;}
.mc-paid{font-size:13px;font-weight:700;color:#34d399;font-family:‘JetBrains Mono’,monospace;}
.mini-bar{height:4px;background:var(–surface2);border-radius:4px;margin-top:6px;overflow:hidden;}
.mini-fill{height:100%;border-radius:4px;background:var(–gold);}

/* IMPORT */
.import-zone{border:2px dashed rgba(245,158,11,.4);border-radius:20px;padding:30px 20px;text-align:center;cursor:pointer;transition:all .2s;background:rgba(245,158,11,.03);margin-bottom:16px;position:relative;}
.import-zone input[type=file]{position:absolute;inset:0;opacity:0;cursor:pointer;width:100%;height:100%;}
.iz-icon{font-size:44px;margin-bottom:10px;}
.iz-title{font-size:16px;font-weight:700;color:var(–accent);}
.iz-sub{font-size:12px;color:var(–muted);margin-top:6px;line-height:1.5;}
.template-btn{width:100%;background:var(–surface2);border:1px solid rgba(245,158,11,.3);border-radius:14px;padding:14px;color:var(–accent);font-family:‘Sora’,sans-serif;font-size:14px;font-weight:600;cursor:pointer;display:flex;align-items:center;justify-content:center;gap:8px;margin-bottom:16px;}
.preview-table-wrap{border-radius:14px;overflow:hidden;border:1px solid var(–border);margin-bottom:14px;}
.preview-table{width:100%;border-collapse:collapse;font-size:12px;}
.preview-table th{background:var(–surface2);padding:9px 10px;text-align:left;font-size:10px;color:var(–muted);font-weight:700;text-transform:uppercase;border-bottom:1px solid var(–border);}
.preview-table td{padding:9px 10px;border-bottom:1px solid var(–border);color:var(–text);}
.preview-table tr:last-child td{border-bottom:none;}
.import-actions{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:12px;}
.imp-btn{border:none;border-radius:14px;padding:13px;font-family:‘Sora’,sans-serif;font-size:13px;font-weight:700;cursor:pointer;}
.imp-btn.replace{background:linear-gradient(135deg,#ef4444,#dc2626);color:#fff;}
.imp-btn.append{background:linear-gradient(135deg,#10b981,#059669);color:#fff;}
.col-map{background:var(–surface);border:1px solid var(–border);border-radius:16px;padding:16px;margin-bottom:14px;}
.col-row{display:flex;align-items:center;gap:10px;margin-bottom:10px;}
.col-row:last-child{margin-bottom:0;}
.col-label{font-size:12px;color:var(–muted);font-weight:600;width:110px;flex-shrink:0;}
.col-select{flex:1;background:var(–surface2);border:1px solid var(–border);border-radius:10px;padding:9px 12px;color:var(–text);font-family:‘Sora’,sans-serif;font-size:13px;outline:none;}
.import-status{border-radius:14px;padding:14px;margin-bottom:14px;display:none;}
.import-status.success{background:rgba(16,185,129,.1);border:1px solid rgba(16,185,129,.3);}
.import-status.error{background:rgba(239,68,68,.1);border:1px solid rgba(239,68,68,.3);}

/* REPORTS */
.report-tabs{display:flex;background:var(–surface2);border-radius:12px;padding:4px;margin-bottom:18px;}
.report-tab{flex:1;text-align:center;padding:9px;border-radius:10px;font-size:13px;font-weight:600;cursor:pointer;transition:all .2s;color:var(–muted);}
.report-tab.active{background:var(–surface);color:var(–text);box-shadow:0 2px 8px rgba(0,0,0,.3);}
.sum-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:16px;}
.sum-box{background:var(–surface);border:1px solid var(–border);border-radius:14px;padding:14px;}
.sb-lbl{font-size:10px;color:var(–muted);font-weight:700;text-transform:uppercase;letter-spacing:.5px;}
.sb-val{font-size:19px;font-weight:800;margin-top:5px;font-family:‘JetBrains Mono’,monospace;}

/* MODALS */
.modal-overlay{position:fixed;inset:0;background:rgba(0,0,0,.8);backdrop-filter:blur(8px);z-index:200;display:none;align-items:flex-end;justify-content:center;}
.modal-overlay.open{display:flex;}
.modal{background:var(–surface);border-radius:24px 24px 0 0;width:100%;max-width:430px;padding:24px 20px calc(24px + env(safe-area-inset-bottom));border-top:1px solid var(–border);animation:slideUp .3s cubic-bezier(.32,.72,0,1);max-height:90vh;overflow-y:auto;}
@keyframes slideUp{from{transform:translateY(100%)}to{transform:translateY(0)}}
.modal-handle{width:40px;height:4px;background:var(–border);border-radius:4px;margin:0 auto 18px;}
.modal-title{font-size:18px;font-weight:700;margin-bottom:4px;}
.modal-sub{font-size:13px;color:var(–muted);margin-bottom:18px;}
.primary-btn{width:100%;background:linear-gradient(135deg,#f59e0b,#d97706);border:none;border-radius:14px;padding:15px;color:#000;font-family:‘Sora’,sans-serif;font-size:15px;font-weight:700;cursor:pointer;margin-top:4px;}
.primary-btn.green{background:linear-gradient(135deg,#10b981,#059669);color:#fff;}
.primary-btn.blue{background:linear-gradient(135deg,#6366f1,#4f46e5);color:#fff;}
.secondary-btn{width:100%;background:var(–surface2);border:1px solid var(–border);border-radius:14px;padding:13px;color:var(–text);font-family:‘Sora’,sans-serif;font-size:14px;font-weight:600;cursor:pointer;margin-top:10px;}
.detail-row{display:flex;justify-content:space-between;align-items:center;padding:11px 0;border-bottom:1px solid var(–border);}
.detail-row:last-of-type{border-bottom:none;}
.dk{font-size:13px;color:var(–muted);}
.dv{font-size:13px;font-weight:600;}

/* CONFIRM */
.confirm-overlay{position:fixed;inset:0;background:rgba(0,0,0,.8);backdrop-filter:blur(8px);z-index:300;display:none;align-items:center;justify-content:center;}
.confirm-overlay.open{display:flex;}
.confirm-modal{background:var(–surface);border-radius:24px;padding:28px 24px;width:calc(100% - 40px);max-width:360px;border:1px solid var(–border);animation:popIn .25s ease;}
@keyframes popIn{from{opacity:0;transform:scale(.9)}to{opacity:1;transform:scale(1)}}

/* LOADING */
.loading-overlay{position:fixed;inset:0;background:rgba(10,15,30,.95);z-index:500;display:none;align-items:center;justify-content:center;flex-direction:column;gap:16px;}
.loading-overlay.show{display:flex;}
.spinner{width:44px;height:44px;border:3px solid rgba(245,158,11,.2);border-top-color:var(–accent);border-radius:50%;animation:spin .8s linear infinite;}
@keyframes spin{to{transform:rotate(360deg)}}

/* TOAST */
.toast{position:fixed;top:76px;left:50%;transform:translateX(-50%) translateY(-20px);background:#1a3a2a;border:1px solid rgba(16,185,129,.4);border-radius:12px;padding:12px 20px;color:#34d399;font-size:13px;font-weight:600;z-index:400;opacity:0;transition:all .3s;pointer-events:none;white-space:nowrap;}
.toast.show{opacity:1;transform:translateX(-50%) translateY(0);}
.toast.err{background:#3a1a1a;border-color:rgba(239,68,68,.4);color:#ef4444;}
::-webkit-scrollbar{width:0;}
</style>

</head>
<body>

<!-- TOPBAR with AK Chits branding -->

<div class="topbar">
  <div class="topbar-main">
    <div class="topbar-brand">
      <div class="brand-name">🏆 AK Chits &amp; Enterprises</div>
      <div class="brand-sub">Chit Fund Management Portal</div>
    </div>
    <div class="topbar-right">
      <div class="sync-badge synced" id="syncBadge">☁️ Synced</div>
      <div class="admin-badge">🔐 Admin</div>
    </div>
  </div>
</div>

<!-- ══ HOME ══ -->

<div class="page active" id="page-home">
  <div class="content">

```
<!-- Connection status -->
<div class="conn-banner checking" id="connBanner">⏳ Connecting to database…</div>

<!-- Welcome -->
<div style="margin-bottom:16px;">
  <div style="font-size:13px;color:var(--muted);">Welcome back 👋</div>
  <div style="font-size:22px;font-weight:800;margin-top:2px;">Bhanu Prasad</div>
  <div style="font-size:12px;color:var(--muted);margin-top:4px;">Chit Fund Administrator</div>
</div>

<!-- Live stats -->
<div class="stats-row" id="homeStats">
  <div class="stat-box"><div class="stat-val" id="st-members">—</div><div class="stat-lbl">Members</div></div>
  <div class="stat-box"><div class="stat-val" id="st-groups">—</div><div class="stat-lbl">Groups</div></div>
  <div class="stat-box"><div class="stat-val" id="st-payments">—</div><div class="stat-lbl">Payments</div></div>
</div>

<!-- Add Payment button -->
<div class="action-btn add" onclick="openPaymentModal()">
  <div class="ab-icon">💳</div>
  <div class="ab-text">
    <div class="ab-title">Add Payment</div>
    <div class="ab-sub">Record a member payment</div>
  </div>
</div>

<!-- Group selector -->
<div class="selector-card">
  <label class="form-label">Select Group — View Progress</label>
  <select class="form-input" id="homeGroupSelect" onchange="onHomeGroupChange()">
    <option value="">-- Select a Group --</option>
  </select>
</div>

<!-- Group progress panel -->
<div id="groupProgressPanel"></div>

<!-- Member selector -->
<div class="selector-card">
  <label class="form-label">Select Member — View Payments</label>
  <select class="form-input" id="homeMemberSelect" onchange="onHomeMemberChange()">
    <option value="">-- Select a Member --</option>
  </select>
</div>

<!-- Member detail panel -->
<div class="member-panel" id="memberPanel">
  <div id="memberHeader" style="background:var(--surface);border:1px solid var(--border);border-radius:18px;padding:16px;margin-bottom:14px;"></div>
  <div class="amt-grid" id="amtGrid"></div>
  <div id="groupPayTables"></div>
</div>
```

  </div>
</div>

<!-- ══ MEMBERS ══ -->

<div class="page" id="page-members">
  <div class="content">
    <div style="margin-bottom:18px;">
      <div class="section-title">All Members</div>
      <div style="font-size:22px;font-weight:800;" id="memberCountTitle">0 Members</div>
    </div>
    <div class="search-bar"><span>🔍</span><input type="text" placeholder="Search members…" oninput="filterMembers(this.value)"></div>
    <div id="membersContainer"></div>
  </div>
</div>

<!-- ══ IMPORT ══ -->

<div class="page" id="page-import">
  <div class="content">
    <div style="margin-bottom:18px;">
      <div class="section-title">Excel Import</div>
      <div style="font-size:22px;font-weight:800;">Import Members</div>
      <div style="font-size:13px;color:var(--muted);margin-top:4px;">Bulk upload from Excel</div>
    </div>
    <button class="template-btn" onclick="downloadTemplate()">📥 Download Template</button>
    <div class="import-zone" id="importZone">
      <input type="file" accept=".xlsx,.xls,.csv" onchange="handleFileUpload(this)">
      <div class="iz-icon">📂</div>
      <div class="iz-title">Tap to Upload Excel File</div>
      <div class="iz-sub">Supports .xlsx · .xls · .csv</div>
    </div>
    <div class="import-status" id="importStatus"></div>
    <div id="colMapSection" style="display:none;">
      <div class="section-title">Map Columns</div>
      <div class="col-map">
        <div class="col-row"><span class="col-label">Member Name</span><select class="col-select" id="map-name"></select></div>
        <div class="col-row"><span class="col-label">Group Name</span><select class="col-select" id="map-group"></select></div>
        <div class="col-row"><span class="col-label">Phone</span><select class="col-select" id="map-phone"></select></div>
      </div>
    </div>
    <div id="previewSection" style="display:none;">
      <div class="section-title">Preview <span id="previewCount" style="color:var(--accent);"></span></div>
      <div class="preview-table-wrap">
        <table class="preview-table">
          <thead><tr><th>Name</th><th>Group</th><th>Phone</th></tr></thead>
          <tbody id="previewBody"></tbody>
        </table>
      </div>
      <div class="import-actions">
        <button class="imp-btn replace" onclick="confirmImport('replace')">🔄 Replace All</button>
        <button class="imp-btn append" onclick="confirmImport('append')">➕ Add to List</button>
      </div>
      <button class="secondary-btn" onclick="resetImport()">✕ Cancel</button>
    </div>
  </div>
</div>

<!-- ══ REPORTS ══ -->

<div class="page" id="page-report">
  <div class="content">
    <div style="margin-bottom:18px;">
      <div class="section-title">Analytics</div>
      <div style="font-size:22px;font-weight:800;">Group Report</div>
    </div>
    <div class="report-tabs">
      <div class="report-tab active" onclick="switchRTab(this,'summary')">Summary</div>
      <div class="report-tab" onclick="switchRTab(this,'defaulters')">Defaulters</div>
    </div>
    <div id="reportContent"></div>
  </div>
</div>

<!-- BOTTOM NAV -->

<nav class="bottom-nav">
  <div class="quick-add-bar">
    <div class="quick-btn member" onclick="openAddMemberModal()">➕ New Member</div>
    <div class="quick-btn group" onclick="openAddGroupModal()">🏦 New Group</div>
  </div>
  <div class="bottom-nav-btns">
    <div class="nav-item active" onclick="navigate('home',this)"><span class="nav-icon">🏠</span><span class="nav-label">Home</span></div>
    <div class="nav-item" onclick="navigate('members',this)"><span class="nav-icon">👥</span><span class="nav-label">Members</span></div>
    <div class="nav-item" onclick="navigate('import',this)"><span class="nav-icon">📥</span><span class="nav-label">Import</span></div>
    <div class="nav-item" onclick="navigate('report',this)"><span class="nav-icon">📊</span><span class="nav-label">Reports</span></div>
  </div>
</nav>

<!-- ADD PAYMENT MODAL -->

<div class="modal-overlay" id="paymentModal" onclick="outsideClose(event,'paymentModal')">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">💳 Add Payment</div>
    <div class="modal-sub">Record a new payment entry</div>
    <div class="form-group">
      <label class="form-label">Member Name</label>
      <select class="form-input" id="pay-member" onchange="loadMemberGroups()">
        <option value="">-- Select Member --</option>
      </select>
    </div>
    <div class="form-group">
      <label class="form-label">Group Name</label>
      <select class="form-input" id="pay-group" disabled>
        <option value="">-- Select Group --</option>
      </select>
    </div>
    <div class="form-group">
      <label class="form-label">Chit Value (₹)</label>
      <input class="form-input" type="number" id="pay-chitval" placeholder="e.g. 50000" min="0" oninput="calcBalance()">
    </div>
    <div class="form-group">
      <label class="form-label">Amount Paid (₹)</label>
      <input class="form-input" type="number" id="pay-amount" placeholder="e.g. 22000" min="0" oninput="calcBalance()">
    </div>
    <div class="form-group">
      <label class="form-label">Balance Amt (₹) — Auto calculated</label>
      <input class="form-input" type="number" id="pay-balance" readonly style="opacity:.7;">
    </div>
    <div class="form-group">
      <label class="form-label">Paid On (Date)</label>
      <input class="form-input" type="date" id="pay-date">
    </div>
    <div class="form-group">
      <label class="form-label">Paid By</label>
      <input class="form-input" type="text" id="pay-paidby" placeholder="e.g. Cash / UPI / NEFT">
    </div>
    <div class="form-group">
      <label class="form-label">Chit Picked</label>
      <input class="form-input" type="text" id="pay-chitpicked" placeholder="e.g. Yes / No / Member Name">
    </div>
    <button class="primary-btn" onclick="savePayment()">✓ Save Payment</button>
    <button class="secondary-btn" onclick="closeModal('paymentModal')">Cancel</button>
  </div>
</div>

<!-- ADD MEMBER MODAL -->

<div class="modal-overlay" id="addMemberModal" onclick="outsideClose(event,'addMemberModal')">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">👤 New Member</div>
    <div class="modal-sub">Add a new chit fund member</div>
    <div class="form-group"><label class="form-label">Full Name</label><input class="form-input" type="text" id="nm-name" placeholder="e.g. Venkata Subba Rao"></div>
    <div class="form-group">
      <label class="form-label">Select Group</label>
      <select class="form-input" id="nm-group"><option value="">-- Select Group --</option></select>
    </div>
    <div class="form-group"><label class="form-label">Phone Number</label><input class="form-input" type="tel" id="nm-phone" placeholder="e.g. 9876543210"></div>
    <button class="primary-btn" onclick="addMember()">✓ Add Member</button>
    <button class="secondary-btn" onclick="closeModal('addMemberModal')">Cancel</button>
  </div>
</div>

<!-- ADD GROUP MODAL -->

<div class="modal-overlay" id="addGroupModal" onclick="outsideClose(event,'addGroupModal')">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-title">🏦 New Group</div>
    <div class="modal-sub">Create a new chit fund group</div>
    <div class="form-group"><label class="form-label">Group Name</label><input class="form-input" type="text" id="ng-name" placeholder="e.g. Group A"></div>
    <div class="form-group"><label class="form-label">Total Months</label><input class="form-input" type="number" id="ng-months" placeholder="e.g. 21" value="21" min="1"></div>
    <div class="form-group"><label class="form-label">Start Date</label><input class="form-input" type="date" id="ng-date"></div>
    <button class="primary-btn blue" onclick="addGroup()">✓ Create Group</button>
    <button class="secondary-btn" onclick="closeModal('addGroupModal')">Cancel</button>
  </div>
</div>

<!-- MEMBER DETAIL MODAL -->

<div class="modal-overlay" id="memberModal" onclick="outsideClose(event,'memberModal')">
  <div class="modal">
    <div class="modal-handle"></div>
    <div id="memberModalBody"></div>
    <button class="secondary-btn" onclick="closeModal('memberModal')">Close</button>
  </div>
</div>

<!-- CONFIRM MODAL -->

<div class="confirm-overlay" id="confirmOverlay">
  <div class="confirm-modal">
    <div style="font-size:28px;text-align:center;margin-bottom:12px;" id="confirmIcon">⚠️</div>
    <div style="font-size:17px;font-weight:700;text-align:center;margin-bottom:8px;" id="confirmTitle"></div>
    <div style="font-size:13px;color:var(--muted);text-align:center;line-height:1.5;margin-bottom:24px;" id="confirmMsg"></div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;">
      <button style="background:var(--surface2);border:1px solid var(--border);border-radius:12px;padding:13px;color:var(--text);font-family:'Sora',sans-serif;font-size:14px;font-weight:600;cursor:pointer;" onclick="closeConfirm()">Cancel</button>
      <button style="border:none;border-radius:12px;padding:13px;font-family:'Sora',sans-serif;font-size:14px;font-weight:700;cursor:pointer;" id="confirmOkBtn">Confirm</button>
    </div>
  </div>
</div>

<!-- LOADING -->

<div class="loading-overlay" id="loadingOverlay">
  <div class="spinner"></div>
  <div style="font-size:14px;color:var(--muted);" id="loadingMsg">Loading…</div>
</div>

<div class="toast" id="toast"></div>

<script>
// ═══════════════════════════════════════
// SUPABASE CONFIG
// ═══════════════════════════════════════
const SB_URL = 'https://fxmulysdirqrkixdqusg.supabase.co';
const SB_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImZ4bXVseXNkaXJxcmtpeGRxdXNnIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzE4NDEzMTMsImV4cCI6MjA4NzQxNzMxM30.Vlptcq0oI5-oA4CtpshjqAvG_yEM-t1TuXfKXpfxgf4';
const H = {
  'Content-Type':'application/json',
  'apikey': SB_KEY,
  'Authorization': 'Bearer ' + SB_KEY,
  'Prefer': 'return=representation'
};

// ═══════════════════════════════════════
// API HELPERS
// ═══════════════════════════════════════
async function sbGet(table, query=''){
  const res = await fetch(`${SB_URL}/rest/v1/${table}?${query}&order=created_at.asc`, {headers: H});
  if(!res.ok){ const t = await res.text(); throw new Error(`${res.status}: ${t}`); }
  return res.json();
}
async function sbPost(table, data){
  const res = await fetch(`${SB_URL}/rest/v1/${table}`, {method:'POST', headers: H, body: JSON.stringify(data)});
  if(!res.ok){ const t = await res.text(); throw new Error(`${res.status}: ${t}`); }
  return res.json();
}
async function sbDelete(table, id){
  await fetch(`${SB_URL}/rest/v1/${table}?id=eq.${id}`, {method:'DELETE', headers: H});
}

// ═══════════════════════════════════════
// STATE
// ═══════════════════════════════════════
let members=[], groups=[], payments=[];
let parsedImportData=[], pendingImportMode='';
const COLORS=['#7c3aed','#0891b2','#065f46','#92400e','#be185d','#1e40af','#6d28d9','#b45309','#047857','#9d174d'];
function rCol(i){return COLORS[i%COLORS.length];}
function ini(n){return (n||'?').split(' ').map(x=>x[0]||'').join('').toUpperCase().slice(0,2)||'??';}
function fmtAmt(v){ return v>=100000 ? '₹'+(v/100000).toFixed(1)+'L' : v>=1000 ? '₹'+(v/1000).toFixed(0)+'K' : '₹'+v; }

// ═══════════════════════════════════════
// UI HELPERS
// ═══════════════════════════════════════
function showLoading(msg='Loading…'){ document.getElementById('loadingOverlay').classList.add('show'); document.getElementById('loadingMsg').textContent=msg; }
function hideLoading(){ document.getElementById('loadingOverlay').classList.remove('show'); }
function setSyncBadge(s){
  const b=document.getElementById('syncBadge');
  if(s==='syncing'){b.className='sync-badge syncing';b.textContent='⏳ Syncing…';}
  else if(s==='error'){b.className='sync-badge error';b.textContent='❌ Offline';}
  else{b.className='sync-badge synced';b.textContent='☁️ Synced';}
}
function setConnBanner(state, msg){
  const b=document.getElementById('connBanner');
  b.className='conn-banner '+state;
  b.textContent=msg;
  b.style.display='flex';
  if(state==='ok') setTimeout(()=>b.style.display='none', 3000);
}
function showToast(msg,err=false){
  const t=document.getElementById('toast');
  t.textContent=msg; t.className=`toast${err?' err':''} show`;
  setTimeout(()=>t.classList.remove('show'),3000);
}
function closeModal(id){ document.getElementById(id).classList.remove('open'); }
function outsideClose(e,id){ if(e.target.id===id) closeModal(id); }
function closeConfirm(){ document.getElementById('confirmOverlay').classList.remove('open'); }

// ═══════════════════════════════════════
// INIT
// ═══════════════════════════════════════
async function init(){
  showLoading('Connecting to AK Chits database…');
  setSyncBadge('syncing');
  setConnBanner('checking','⏳ Connecting to database…');
  try{
    [members, groups, payments] = await Promise.all([
      sbGet('members'),
      sbGet('groups'),
      sbGet('payments')
    ]);
    if(!Array.isArray(members)) members=[];
    if(!Array.isArray(groups))  groups=[];
    if(!Array.isArray(payments)) payments=[];
    setSyncBadge('synced');
    setConnBanner('ok', `✅ Connected — ${members.length} members, ${groups.length} groups, ${payments.length} payments loaded`);
    refreshAll();
  } catch(e){
    console.error('Init error:', e);
    setSyncBadge('error');
    setConnBanner('fail', '❌ Database connection failed. Check Supabase setup.');
    members=[]; groups=[]; payments=[];
    refreshAll();
  }
  hideLoading();
}

function refreshAll(){
  updateHomeStats();
  populateHomeGroupDropdown();
  populateHomeDropdown();
  populateGroupDropdowns();
  updateMemberCount();
}

// ═══════════════════════════════════════
// HOME STATS
// ═══════════════════════════════════════
function updateHomeStats(){
  document.getElementById('st-members').textContent = members.length;
  document.getElementById('st-groups').textContent  = groups.length;
  document.getElementById('st-payments').textContent= payments.length;
}

// ═══════════════════════════════════════
// HOME — GROUP PROGRESS
// ═══════════════════════════════════════
function populateHomeGroupDropdown(){
  const sel=document.getElementById('homeGroupSelect');
  const cur=sel.value;
  sel.innerHTML='<option value="">-- Select a Group --</option>';
  groups.forEach(g=>{
    const o=document.createElement('option');
    o.value=g.id;
    o.textContent=g.name;
    sel.appendChild(o);
  });
  if(cur) sel.value=cur;
}

function onHomeGroupChange(){
  const id=document.getElementById('homeGroupSelect').value;
  const panel=document.getElementById('groupProgressPanel');
  if(!id){ panel.innerHTML=''; return; }
  const g=groups.find(x=>x.id===id);
  if(!g){ panel.innerHTML=''; return; }
  const gPays=payments.filter(p=>p.group_name===g.name);
  const totalPaid=gPays.reduce((s,p)=>s+(p.amount_paid||0),0);
  const totalBal =gPays.reduce((s,p)=>s+(p.balance_amt||0),0);
  const memberCount=[...new Set(gPays.map(p=>p.member_id))].length;
  const payCount=gPays.length;
  const totalMonths=g.total_months||21;
  let elapsed=0;
  if(g.start_date){
    const diff=new Date()-new Date(g.start_date);
    elapsed=Math.max(0,Math.min(totalMonths,Math.floor(diff/(1000*60*60*24*30.44))));
  }
  const pct=totalMonths>0?Math.min(100,Math.round(elapsed/totalMonths*100)):0;

  panel.innerHTML=`
    <div class="group-progress-card">
      <div class="gp-header">
        <div class="gp-name">📂 ${g.name}</div>
        <div class="gp-badge">${pct}% Complete</div>
      </div>
      <div class="gp-stats">
        <div class="gp-stat"><div class="gp-stat-val" style="color:#34d399;">${fmtAmt(totalPaid)}</div><div class="gp-stat-lbl">Collected</div></div>
        <div class="gp-stat"><div class="gp-stat-val" style="color:#f59e0b;">${fmtAmt(totalBal)}</div><div class="gp-stat-lbl">Pending</div></div>
        <div class="gp-stat"><div class="gp-stat-val" style="color:#a5b4fc;">${memberCount}</div><div class="gp-stat-lbl">Members</div></div>
      </div>
      <div class="progress-bar"><div class="progress-fill" style="width:${pct}%"></div></div>
      <div class="progress-labels">
        <span class="pl-txt">Month ${elapsed} of ${totalMonths}</span>
        <span class="pl-pct">${payCount} payments recorded</span>
      </div>
    </div>`;
}

// ═══════════════════════════════════════
// HOME — MEMBER DETAIL
// ═══════════════════════════════════════
function populateHomeDropdown(){
  const sel=document.getElementById('homeMemberSelect');
  const cur=sel.value;
  sel.innerHTML='<option value="">-- Select a Member --</option>';
  members.forEach(m=>{const o=document.createElement('option');o.value=m.id;o.textContent=m.name;sel.appendChild(o);});
  if(cur) sel.value=cur;
}

function onHomeMemberChange(){
  const id=document.getElementById('homeMemberSelect').value;
  const panel=document.getElementById('memberPanel');
  if(!id){panel.classList.remove('visible');return;}
  const m=members.find(x=>x.id===id);
  if(!m) return;
  panel.classList.add('visible');
  const mPays=payments.filter(p=>p.member_id===m.id);
  const totalPaid=mPays.reduce((s,p)=>s+(p.amount_paid||0),0);
  const totalBal =mPays.reduce((s,p)=>s+(p.balance_amt||0),0);

  document.getElementById('memberHeader').innerHTML=`
    <div style="display:flex;align-items:center;gap:14px;">
      <div style="width:52px;height:52px;border-radius:14px;background:${m.color||'#7c3aed'}22;border:2px solid ${m.color||'#7c3aed'}55;color:${m.color||'#7c3aed'};display:flex;align-items:center;justify-content:center;font-size:18px;font-weight:800;flex-shrink:0;">${m.initials||ini(m.name)}</div>
      <div style="flex:1;"><div style="font-size:16px;font-weight:700;">${m.name}</div>
      <div style="font-size:12px;color:var(--muted);">${m.phone||'No phone'} · ${mPays.length} payments</div>
      <div style="display:inline-flex;gap:4px;border-radius:6px;padding:3px 8px;font-size:10px;font-weight:700;margin-top:6px;background:rgba(16,185,129,.15);color:#34d399;border:1px solid rgba(16,185,129,.3);">🟢 Active</div></div>
    </div>`;

  document.getElementById('amtGrid').innerHTML=`
    <div class="amt-box" style="border-top:2px solid #34d399;"><div class="amt-lbl">Total Paid</div><div class="amt-val" style="color:#34d399;">₹${totalPaid.toLocaleString('en-IN')}</div></div>
    <div class="amt-box" style="border-top:2px solid #f59e0b;"><div class="amt-lbl">Total Balance</div><div class="amt-val" style="color:#f59e0b;">₹${totalBal.toLocaleString('en-IN')}</div></div>`;

  // group payments by group name
  const grouped={};
  mPays.forEach(p=>{const g=p.group_name||'Unknown';if(!grouped[g])grouped[g]=[];grouped[g].push(p);});
  const container=document.getElementById('groupPayTables');
  if(!Object.keys(grouped).length){
    container.innerHTML=`<div class="card" style="text-align:center;color:var(--muted);font-size:13px;">No payments recorded yet</div>`;
    return;
  }
  container.innerHTML=Object.entries(grouped).map(([grp,pays])=>`
    <div class="card" style="margin-bottom:14px;padding:16px;">
      <div class="section-title">📂 ${grp}</div>
      <div class="pay-table-wrap">
        <table class="pay-table">
          <thead><tr><th>Date</th><th>Chit Val</th><th>Paid</th><th>Balance</th><th>Paid By</th><th>Chit Picked</th></tr></thead>
          <tbody>
            ${pays.map(p=>`<tr>
              <td>${p.paid_on?new Date(p.paid_on).toLocaleDateString('en-IN',{day:'2-digit',month:'short',year:'2-digit'}):'—'}</td>
              <td style="color:#a5b4fc;">₹${(p.chit_value||0).toLocaleString('en-IN')}</td>
              <td style="color:#34d399;">₹${(p.amount_paid||0).toLocaleString('en-IN')}</td>
              <td style="color:#f59e0b;">₹${(p.balance_amt||0).toLocaleString('en-IN')}</td>
              <td style="color:var(--muted);">${p.paid_by||'—'}</td>
              <td style="color:var(--muted);">${p.chit_picked||'—'}</td>
            </tr>`).join('')}
            <tr style="font-weight:700;background:rgba(255,255,255,.03);">
              <td colspan="2" style="color:var(--muted);">Total</td>
              <td style="color:#34d399;">₹${pays.reduce((s,p)=>s+(p.amount_paid||0),0).toLocaleString('en-IN')}</td>
              <td style="color:#f59e0b;">₹${pays.reduce((s,p)=>s+(p.balance_amt||0),0).toLocaleString('en-IN')}</td>
              <td colspan="2"></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>`).join('');
}

// ═══════════════════════════════════════
// ADD PAYMENT
// ═══════════════════════════════════════
function openPaymentModal(){
  const sel=document.getElementById('pay-member');
  sel.innerHTML='<option value="">-- Select Member --</option>';
  members.forEach(m=>{const o=document.createElement('option');o.value=m.id;o.textContent=m.name;sel.appendChild(o);});
  document.getElementById('pay-group').innerHTML='<option value="">-- Select Group --</option>';
  document.getElementById('pay-group').disabled=true;
  ['pay-chitval','pay-amount','pay-balance','pay-paidby','pay-chitpicked'].forEach(id=>document.getElementById(id).value='');
  document.getElementById('pay-date').value=new Date().toISOString().split('T')[0];
  document.getElementById('paymentModal').classList.add('open');
}
function loadMemberGroups(){
  const id=document.getElementById('pay-member').value;
  const gsel=document.getElementById('pay-group');
  gsel.innerHTML='<option value="">-- Select Group --</option>';
  gsel.disabled=!id;
  if(!id) return;
  const existing=[...new Set(payments.filter(p=>p.member_id===id).map(p=>p.group_name))];
  const all=groups.map(g=>g.name);
  [...new Set([...existing,...all])].forEach(g=>{const o=document.createElement('option');o.value=g;o.textContent=g;gsel.appendChild(o);});
}
function calcBalance(){
  const chit=parseFloat(document.getElementById('pay-chitval').value)||0;
  const paid=parseFloat(document.getElementById('pay-amount').value)||0;
  document.getElementById('pay-balance').value=Math.max(0,chit-paid)||'';
}
async function savePayment(){
  const memberId=document.getElementById('pay-member').value;
  const group=document.getElementById('pay-group').value;
  const chitVal=parseFloat(document.getElementById('pay-chitval').value)||0;
  const amount=parseFloat(document.getElementById('pay-amount').value)||0;
  const balance=parseFloat(document.getElementById('pay-balance').value)||0;
  const date=document.getElementById('pay-date').value;
  const paidBy=document.getElementById('pay-paidby').value.trim();
  const chitPicked=document.getElementById('pay-chitpicked').value.trim();
  if(!memberId){showToast('❌ Select a member',true);return;}
  if(!group){showToast('❌ Select a group',true);return;}
  if(!amount){showToast('❌ Enter amount paid',true);return;}
  if(!date){showToast('❌ Enter date',true);return;}
  const m=members.find(x=>x.id===memberId);
  setSyncBadge('syncing');
  try{
    const rec=await sbPost('payments',{member_id:memberId,name:m.name,group_name:group,chit_value:chitVal,amount_paid:amount,balance_amt:balance,paid_on:date,paid_by:paidBy,chit_picked:chitPicked});
    const saved=Array.isArray(rec)?rec[0]:rec;
    if(saved&&saved.id){
      payments.push(saved);
      updateHomeStats();
      setSyncBadge('synced');
      showToast(`✅ Payment saved for ${m.name}!`);
      closeModal('paymentModal');
      if(document.getElementById('homeMemberSelect').value===memberId) onHomeMemberChange();
      const gv=document.getElementById('homeGroupSelect').value;
      if(gv) onHomeGroupChange();
    } else throw new Error('No record returned: '+JSON.stringify(rec));
  } catch(e){ console.error('savePayment:',e); setSyncBadge('error'); showToast('❌ Failed to save: '+e.message,true); }
}

// ═══════════════════════════════════════
// ADD MEMBER
// ═══════════════════════════════════════
function openAddMemberModal(){
  populateGroupDropdowns();
  document.getElementById('nm-name').value='';
  document.getElementById('nm-phone').value='';
  document.getElementById('addMemberModal').classList.add('open');
}
async function addMember(){
  const name=document.getElementById('nm-name').value.trim();
  const group=document.getElementById('nm-group').value;
  const phone=document.getElementById('nm-phone').value.trim();
  if(!name){showToast('❌ Enter member name',true);return;}
  setSyncBadge('syncing');
  try{
    const rec=await sbPost('members',{name,group_name:group,phone,color:rCol(members.length),initials:ini(name)});
    const saved=Array.isArray(rec)?rec[0]:rec;
    if(saved&&saved.id){
      members.push(saved);
      populateHomeDropdown(); updateMemberCount(); updateHomeStats();
      setSyncBadge('synced'); showToast(`✅ ${name} added!`);
      closeModal('addMemberModal');
    } else throw new Error(JSON.stringify(rec));
  } catch(e){ console.error('addMember:',e); setSyncBadge('error'); showToast('❌ Failed: '+e.message,true); }
}

// ═══════════════════════════════════════
// ADD GROUP — FIXED
// ═══════════════════════════════════════
function openAddGroupModal(){
  document.getElementById('ng-name').value='';
  document.getElementById('ng-months').value='21';
  document.getElementById('ng-date').value=new Date().toISOString().split('T')[0];
  document.getElementById('addGroupModal').classList.add('open');
}
async function addGroup(){
  const name=document.getElementById('ng-name').value.trim();
  const months=parseInt(document.getElementById('ng-months').value)||21;
  const startDate=document.getElementById('ng-date').value;
  if(!name){showToast('❌ Enter group name',true);return;}
  setSyncBadge('syncing');
  try{
    const rec=await sbPost('groups',{name,total_months:months,start_date:startDate||null});
    const saved=Array.isArray(rec)?rec[0]:rec;
    if(saved&&saved.id){
      groups.push(saved);
      populateGroupDropdowns(); populateHomeGroupDropdown(); updateHomeStats();
      setSyncBadge('synced'); showToast(`✅ Group "${name}" created!`);
      closeModal('addGroupModal');
    } else throw new Error('Unexpected response: '+JSON.stringify(rec));
  } catch(e){ console.error('addGroup:',e); setSyncBadge('error'); showToast('❌ Failed: '+e.message,true); }
}

function populateGroupDropdowns(){
  const sel=document.getElementById('nm-group');
  if(!sel) return;
  sel.innerHTML='<option value="">-- Select Group --</option>';
  groups.forEach(g=>{const o=document.createElement('option');o.value=g.name;o.textContent=g.name;sel.appendChild(o);});
}

// ═══════════════════════════════════════
// MEMBERS PAGE
// ═══════════════════════════════════════
function updateMemberCount(){ document.getElementById('memberCountTitle').textContent=`${members.length} Members`; }
function renderMembers(list){
  document.getElementById('membersContainer').innerHTML=!list.length
    ?`<div style="text-align:center;padding:40px 20px;color:var(--muted);"><div style="font-size:40px;margin-bottom:12px;">👥</div><div>No members yet. Use ➕ New Member below.</div></div>`
    :list.map(m=>{
      const mp=payments.filter(p=>p.member_id===m.id);
      const tp=mp.reduce((s,p)=>s+(p.amount_paid||0),0);
      return`<div class="member-card" onclick="openMemberDetail('${m.id}')">
        <div class="mc-avatar" style="background:${m.color||'#7c3aed'}22;border:1.5px solid ${m.color||'#7c3aed'}55;color:${m.color||'#7c3aed'};">${m.initials||ini(m.name)}</div>
        <div class="mc-info"><div class="mc-name">${m.name}</div><div class="mc-sub">${m.phone||'No phone'} · ${mp.length} payments</div>
        <div class="mini-bar"><div class="mini-fill" style="width:${Math.min(mp.length*10,100)}%"></div></div></div>
        <div class="mc-right"><div class="mc-paid">${fmtAmt(tp)}</div><div style="font-size:10px;color:var(--muted);">paid</div></div>
      </div>`;}).join('');
}
function filterMembers(q){ renderMembers(members.filter(m=>m.name.toLowerCase().includes(q.toLowerCase()))); }
function openMemberDetail(id){
  const m=members.find(x=>x.id===id);
  const mp=payments.filter(p=>p.member_id===id);
  const tp=mp.reduce((s,p)=>s+(p.amount_paid||0),0);
  const tb=mp.reduce((s,p)=>s+(p.balance_amt||0),0);
  document.getElementById('memberModalBody').innerHTML=`
    <div style="display:flex;align-items:center;gap:14px;margin-bottom:18px;">
      <div style="width:54px;height:54px;border-radius:14px;background:${m.color||'#7c3aed'}22;border:2px solid ${m.color||'#7c3aed'}55;color:${m.color||'#7c3aed'};display:flex;align-items:center;justify-content:center;font-size:20px;font-weight:800;">${m.initials||ini(m.name)}</div>
      <div><div style="font-size:18px;font-weight:700;">${m.name}</div><div style="font-size:12px;color:var(--muted);">${m.phone||'No phone'}</div></div>
    </div>
    <div class="detail-row"><span class="dk">Total Payments</span><span class="dv">${mp.length}</span></div>
    <div class="detail-row"><span class="dk">Total Paid</span><span class="dv" style="color:#34d399;">₹${tp.toLocaleString('en-IN')}</span></div>
    <div class="detail-row"><span class="dk">Total Balance</span><span class="dv" style="color:#f59e0b;">₹${tb.toLocaleString('en-IN')}</span></div>
    <div class="detail-row"><span class="dk">Groups</span><span class="dv">${[...new Set(mp.map(p=>p.group_name))].filter(Boolean).join(', ')||'—'}</span></div>`;
  document.getElementById('memberModal').classList.add('open');
}

// ═══════════════════════════════════════
// EXCEL IMPORT
// ═══════════════════════════════════════
function handleFileUpload(input){
  const file=input.files[0]; if(!file) return;
  const reader=new FileReader();
  reader.onload=e=>{
    try{
      const wb=XLSX.read(new Uint8Array(e.target.result),{type:'array'});
      const rows=XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]],{header:1,defval:''});
      if(rows.length<2){showImportStatus('error','❌ File appears empty.');return;}
      const headers=rows[0].map(h=>String(h).trim());
      parsedImportData=rows.slice(1).filter(r=>r.some(c=>c!=='')).map(r=>{const o={};headers.forEach((h,i)=>{o[h]=String(r[i]||'').trim();});return o;});
      populateColMap(headers); autoPreview();
    }catch(e){showImportStatus('error','❌ Could not read file.');}
  };
  reader.readAsArrayBuffer(file);
}
function populateColMap(headers){
  const maps=['map-name','map-group','map-phone'];
  const hints=[['name','member'],['group'],['phone','mobile']];
  maps.forEach((id,i)=>{
    const sel=document.getElementById(id);
    sel.innerHTML='<option value="">-- Skip --</option>';
    headers.forEach(h=>{const o=document.createElement('option');o.value=h;o.textContent=h;sel.appendChild(o);});
    const match=headers.find(h=>hints[i].some(hint=>h.toLowerCase().includes(hint)));
    if(match) sel.value=match;
  });
  document.getElementById('colMapSection').style.display='block';
  maps.forEach(id=>{document.getElementById(id).onchange=autoPreview;});
  autoPreview();
}
function autoPreview(){
  const nc=document.getElementById('map-name').value;
  const gc=document.getElementById('map-group').value;
  const pc=document.getElementById('map-phone').value;
  if(!nc){document.getElementById('previewSection').style.display='none';return;}
  document.getElementById('previewBody').innerHTML=parsedImportData.slice(0,5).map(r=>`<tr><td>${r[nc]||'—'}</td><td>${gc?r[gc]||'—':'—'}</td><td>${pc?r[pc]||'—':'—'}</td></tr>`).join('');
  document.getElementById('previewCount').textContent=`(${parsedImportData.length} rows)`;
  document.getElementById('previewSection').style.display='block';
  showImportStatus('success',`✅ ${parsedImportData.length} members ready to import`);
}
function confirmImport(mode){
  pendingImportMode=mode;
  const count=parsedImportData.length;
  document.getElementById('confirmIcon').textContent=mode==='replace'?'🔄':'➕';
  document.getElementById('confirmTitle').textContent=mode==='replace'?'Replace All Members?':'Add to Existing List?';
  document.getElementById('confirmMsg').innerHTML=mode==='replace'
    ?`Deletes all <strong>${members.length} existing members</strong> and imports <strong>${count} new</strong>.`
    :`Adds <strong>${count} new members</strong> to existing <strong>${members.length}</strong>.`;
  const btn=document.getElementById('confirmOkBtn');
  btn.style.cssText=mode==='replace'?'background:linear-gradient(135deg,#ef4444,#dc2626);color:#fff;':'background:linear-gradient(135deg,#10b981,#059669);color:#fff;';
  btn.onclick=executeImport;
  document.getElementById('confirmOverlay').classList.add('open');
}
async function executeImport(){
  closeConfirm();
  const nc=document.getElementById('map-name').value;
  const gc=document.getElementById('map-group').value;
  const pc=document.getElementById('map-phone').value;
  if(!nc){showToast('❌ Map Member Name column',true);return;}
  showLoading('Importing members…');
  setSyncBadge('syncing');
  try{
    if(pendingImportMode==='replace'){ for(const m of members) await sbDelete('members',m.id); members=[]; }
    let added=0;
    for(let i=0;i<parsedImportData.length;i++){
      const r=parsedImportData[i];
      const name=r[nc]?.trim();
      if(!name) continue;
      const rec=await sbPost('members',{name,group_name:gc?r[gc]||'':'',phone:pc?r[pc]||'':'',color:rCol(members.length+i),initials:ini(name)});
      const saved=Array.isArray(rec)?rec[0]:rec;
      if(saved&&saved.id){members.push(saved);added++;}
    }
    populateHomeDropdown(); updateMemberCount(); updateHomeStats();
    setSyncBadge('synced'); showToast(`✅ ${added} members imported!`);
    showImportStatus('success',`✅ ${added} members saved!`);
    document.getElementById('previewSection').style.display='none';
    document.getElementById('colMapSection').style.display='none';
    parsedImportData=[];
    document.getElementById('importZone').querySelector('input[type=file]').value='';
  } catch(e){ setSyncBadge('error'); showToast('❌ Import failed: '+e.message,true); }
  hideLoading();
}
function resetImport(){
  parsedImportData=[];
  ['previewSection','colMapSection'].forEach(id=>document.getElementById(id).style.display='none');
  document.getElementById('importStatus').style.display='none';
  document.getElementById('importZone').querySelector('input[type=file]').value='';
}
function showImportStatus(t,msg){
  const el=document.getElementById('importStatus');
  el.className=`import-status ${t}`; el.style.display='block';
  el.innerHTML=`<div style="font-size:13px;font-weight:600;color:${t==='success'?'#34d399':'#ef4444'};">${msg}</div>`;
}
function downloadTemplate(){
  const wb=XLSX.utils.book_new();
  const ws=XLSX.utils.aoa_to_sheet([
    ['Member Name','Group Name','Phone Number'],
    ['Rama Krishna','Group A','9876543210'],
    ['Sita Devi','Group A','9876543211'],
    ['Venkata Rao','Group B','9876543212'],
  ]);
  ws['!cols']=[{wch:20},{wch:12},{wch:14}];
  XLSX.utils.book_append_sheet(wb,ws,'Members');
  XLSX.writeFile(wb,'AKChits_Template.xlsx');
  showToast('📥 Template downloaded!');
}

// ═══════════════════════════════════════
// REPORTS
// ═══════════════════════════════════════
let activeRTab='summary';
function switchRTab(el,tab){
  document.querySelectorAll('.report-tab').forEach(t=>t.classList.remove('active'));
  el.classList.add('active'); activeRTab=tab; renderReport();
}
function renderReport(){
  const c=document.getElementById('reportContent');
  const totalPaid=payments.reduce((s,p)=>s+(p.amount_paid||0),0);
  const totalBal =payments.reduce((s,p)=>s+(p.balance_amt||0),0);
  if(activeRTab==='summary'){
    const grpSet=[...new Set(payments.map(p=>p.group_name).filter(Boolean))];
    c.innerHTML=`<div class="sum-grid">
      <div class="sum-box"><div class="sb-lbl">Members</div><div class="sb-val">${members.length}</div></div>
      <div class="sum-box"><div class="sb-lbl">Groups</div><div class="sb-val">${groups.length}</div></div>
      <div class="sum-box"><div class="sb-lbl">Collected</div><div class="sb-val" style="color:#34d399;font-size:15px;">${fmtAmt(totalPaid)}</div></div>
      <div class="sum-box"><div class="sb-lbl">Balance</div><div class="sb-val" style="color:#f59e0b;font-size:15px;">${fmtAmt(totalBal)}</div></div>
      <div class="sum-box"><div class="sb-lbl">Payments</div><div class="sb-val">${payments.length}</div></div>
      <div class="sum-box"><div class="sb-lbl">Active Groups</div><div class="sb-val" style="color:#a5b4fc;">${grpSet.length}</div></div>
    </div>
    <div class="card"><div class="section-title">Group-wise Summary</div>
      ${!grpSet.length?'<div style="color:var(--muted);font-size:13px;text-align:center;padding:10px;">No payments yet</div>':
        grpSet.map(g=>{
          const gp=payments.filter(p=>p.group_name===g);
          const gPaid=gp.reduce((s,p)=>s+(p.amount_paid||0),0);
          const gBal =gp.reduce((s,p)=>s+(p.balance_amt||0),0);
          return`<div class="detail-row"><span class="dk">📂 ${g}</span><div style="text-align:right;"><div style="font-size:13px;font-weight:600;color:#34d399;">₹${gPaid.toLocaleString('en-IN')}</div><div style="font-size:10px;color:var(--muted);">Bal: ₹${gBal.toLocaleString('en-IN')}</div></div></div>`;
        }).join('')}
    </div>`;
  } else {
    const withBal=members.filter(m=>payments.filter(p=>p.member_id===m.id).reduce((s,p)=>s+(p.balance_amt||0),0)>0)
      .sort((a,b)=>{
        const ba=payments.filter(p=>p.member_id===a.id).reduce((s,p)=>s+(p.balance_amt||0),0);
        const bb=payments.filter(p=>p.member_id===b.id).reduce((s,p)=>s+(p.balance_amt||0),0);
        return bb-ba;
      });
    c.innerHTML=`<div class="card" style="background:rgba(239,68,68,.05);border-color:rgba(239,68,68,.2);">
      <div class="section-title" style="color:#ef4444;">⚠️ Members with Balance — ${withBal.length}</div>
      ${!withBal.length?'<p style="color:var(--muted);font-size:13px;text-align:center;padding:10px;">🎉 No pending balances!</p>':
        withBal.map(m=>{
          const bal=payments.filter(p=>p.member_id===m.id).reduce((s,p)=>s+(p.balance_amt||0),0);
          return`<div class="member-card" style="margin-bottom:8px;" onclick="openMemberDetail('${m.id}')">
            <div class="mc-avatar" style="background:${m.color||'#7c3aed'}22;border:1.5px solid ${m.color||'#7c3aed'}55;color:${m.color||'#7c3aed'};">${m.initials||ini(m.name)}</div>
            <div class="mc-info"><div class="mc-name">${m.name}</div><div class="mc-sub" style="color:#ef4444;">Balance pending</div></div>
            <div class="mc-right"><div class="mc-paid" style="color:#f59e0b;">₹${bal.toLocaleString('en-IN')}</div></div>
          </div>`;}).join('')}
    </div>`;
  }
}

// ═══════════════════════════════════════
// NAV
// ═══════════════════════════════════════
function navigate(page,el){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById('page-'+page).classList.add('active');
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  el.classList.add('active');
  if(page==='members') renderMembers(members);
  if(page==='report')  renderReport();
}

// ═══════════════════════════════════════
// START
// ═══════════════════════════════════════
init();
</script>

</body>
</html>
