<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MPI Informatika — Analisis Data | SMP Negeri 1 Pucuk</title>
<style>
  :root{
    --primary:#2563eb;
    --primary-dark:#1d4ed8;
    --secondary:#0ea5e9;
    --accent1:#f59e0b;
    --accent2:#10b981;
    --accent3:#ec4899;
    --accent4:#8b5cf6;
    --danger:#ef4444;
    --ink:#0f172a;
    --font-scale:1.0;
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  html,body{
    width:100%; height:100%;
    overflow:hidden;
    font-family:'Segoe UI', 'Trebuchet MS', Verdana, sans-serif;
    color:var(--ink);
  }
  body{
    background: url('bg-ruang-kelas.jpg') center/cover no-repeat fixed, #f0f9ff;
    display:flex; align-items:center; justify-content:center;
    min-height:100vh;
  }
  #app-wrap{
    position:relative;
    width:100vw; height:100vh;
    display:flex; align-items:center; justify-content:center;
    background: rgba(240, 249, 255, 0.82);
  }
  #stage-16-9{
    position:relative;
    width:min(100vw, 177.78vh);
    height:min(100vh, 56.25vw);
    background:transparent;
    border:none;
    box-shadow:none;
    border-radius:0;
    aspect-ratio:16/9;
    overflow:hidden;
    font-size:calc(15px * var(--font-scale, 1.0));
    display:flex;
    flex-direction:column;
  }
  .page{
    display:none;
    width:100%; height:100%;
    flex-direction:column;
    padding:clamp(10px, 1.8vh, 18px);
    overflow:hidden;
  }
  .page.active{ display:flex; }

  /* ============ TOP NAV ============ */
  #topnav{
    display:flex; align-items:center; justify-content:space-between;
    padding: clamp(4px,0.8vh,8px) clamp(10px,1.5vw,18px);
    background: linear-gradient(135deg, rgba(37,99,235,0.95), rgba(14,165,233,0.95));
    border-radius: 16px;
    box-shadow:0 4px 14px rgba(0,0,0,0.18);
    flex-shrink:0;
    margin-bottom: clamp(8px,1.4vh,14px);
  }
  #topnav .nav-left, #topnav .nav-right{ display:flex; align-items:center; gap:clamp(6px,1vw,12px); }
  .nav-btn{
    display:flex; align-items:center; justify-content:center;
    width:clamp(40px,5.2vh,48px); height:clamp(40px,5.2vh,48px);
    border-radius:50%;
    background:rgba(255,255,255,0.92);
    border:none; cursor:pointer;
    font-size:clamp(18px,2.3vh,23px);
    box-shadow:0 3px 8px rgba(0,0,0,0.2);
    transition:transform .15s ease;
    color:var(--primary-dark);
  }
  .nav-btn:hover{ transform:scale(1.08); }
  .nav-btn:active{ transform:scale(0.94); }
  .nav-home-label{
    color:white; font-weight:800; font-size:clamp(13px,1.7vh,16px);
    background:rgba(255,255,255,0.18); padding:6px 14px; border-radius:20px;
    display:flex; align-items:center; gap:6px; cursor:pointer;
  }
  .zoom-controls{ display:flex; align-items:center; gap:6px; background:rgba(255,255,255,0.18); border-radius:20px; padding:4px 8px;}
  .zoom-controls button{
    width:clamp(28px,3.6vh,34px); height:clamp(28px,3.6vh,34px);
    border-radius:50%; border:none; background:white; color:var(--primary-dark);
    font-weight:900; cursor:pointer; font-size:clamp(13px,1.7vh,16px);
  }

  /* ============ COVER ============ */
  #page-cover{ justify-content:space-between; align-items:center; text-align:center; }
  .cover-header{
    width:100%; display:flex; align-items:center; justify-content:center; gap:16px;
    flex-shrink:0;
  }
  .school-pill{
    display:flex; align-items:center; gap:12px;
    background:rgba(255,255,255,0.85);
    padding:8px 22px 8px 8px;
    border-radius:50px;
    box-shadow:0 4px 14px rgba(0,0,0,0.15);
  }
  .school-pill img{
    height:clamp(50px,6.8vh,68px);
    border-radius:50%;
    box-shadow:0 3px 8px rgba(0,0,0,0.2);
    background:white;
    object-fit:contain;
  }
  .school-pill .school-text{ text-align:left; }
  .school-pill .school-name{ font-weight:900; font-size:clamp(13px,1.8vh,17px); color:var(--primary-dark); }
  .school-pill .school-sub{ font-size:clamp(10px,1.3vh,12px); color:#475569; font-weight:700; }
  .badge-row{ display:flex; gap:8px; }
  .badge-chip{
    background:linear-gradient(135deg, var(--accent1), #fbbf24);
    color:white; font-weight:800; font-size:clamp(10px,1.3vh,12px);
    padding:6px 14px; border-radius:20px; box-shadow:0 3px 8px rgba(0,0,0,0.15);
  }

  .cover-center{ display:flex; flex-direction:column; align-items:center; gap:clamp(10px,1.6vh,16px); flex:1; justify-content:center; }
  .cover-eyebrow{
    font-weight:800; letter-spacing:0.5px; color:var(--primary-dark);
    background:white; padding:6px 20px; border-radius:20px;
    font-size:clamp(12px,1.6vh,15px);
    box-shadow:0 3px 10px rgba(0,0,0,0.12);
  }
  .cover-title{
    font-size:clamp(2.8rem, 6.8vw, 5.2rem);
    font-weight:900;
    line-height:1.02;
    color:var(--primary);
    text-shadow:
      -4px -4px 0 #fff, 4px -4px 0 #fff, -4px 4px 0 #fff, 4px 4px 0 #fff,
      0 8px 18px rgba(37,99,235,0.35);
    letter-spacing:1px;
  }
  .cover-subtitle{
    font-size:clamp(1.4rem, 3.2vw, 2.6rem);
    font-weight:800;
    color:#0ea5e9;
    text-shadow:-2px -2px 0 #fff,2px -2px 0 #fff,-2px 2px 0 #fff,2px 2px 0 #fff;
  }
  .cover-desc{
    max-width:70%;
    font-size:clamp(13px,1.7vh,16px);
    font-weight:700; color:#334155;
    background:rgba(255,255,255,0.7);
    padding:8px 20px; border-radius:14px;
  }
  #btn-mulai{
    margin-top:6px;
    width:clamp(60px,8.5vh,80px); height:clamp(60px,8.5vh,80px);
    border-radius:50%;
    background:radial-gradient(circle at 35% 30%, #34d399, #059669);
    border:4px solid white;
    color:white; font-size:clamp(26px,3.6vh,36px);
    cursor:pointer;
    box-shadow:0 0 0 0 rgba(16,185,129,0.7);
    animation:pulseGlow 1.8s infinite;
    display:flex; align-items:center; justify-content:center;
  }
  #btn-mulai-label{
    font-weight:900; font-size:clamp(18px,2.6vh,26px); color:var(--primary-dark);
    margin-top:6px;
    text-shadow:-2px -2px 0 #fff,2px -2px 0 #fff,-2px 2px 0 #fff,2px 2px 0 #fff;
  }
  @keyframes pulseGlow{
    0%{ box-shadow:0 0 0 0 rgba(16,185,129,0.6);}
    70%{ box-shadow:0 0 0 22px rgba(16,185,129,0);}
    100%{ box-shadow:0 0 0 0 rgba(16,185,129,0);}
  }
  .cover-footer{
    width:100%; display:flex; align-items:center; justify-content:center; gap:10px;
    flex-shrink:0;
    font-size:clamp(11px,1.4vh,13px); font-weight:700; color:#334155;
    background:rgba(255,255,255,0.75); padding:8px 18px; border-radius:14px;
  }

  /* ============ MENU ============ */
  .page-title-bar{
    display:inline-flex; align-self:flex-start; align-items:center; gap:8px;
    font-size:clamp(16px,2.2vh,22px); font-weight:800; color:white;
    background:linear-gradient(135deg,var(--primary),var(--secondary));
    padding:6px 20px; border-radius:20px; margin-bottom:clamp(8px,1.4vh,14px);
    box-shadow:0 3px 10px rgba(0,0,0,0.15); flex-shrink:0;
  }
  #menu-grid{
    flex:1; display:grid; grid-template-columns:repeat(3,1fr); grid-template-rows:repeat(2,1fr);
    gap:clamp(10px,1.6vw,16px); min-height:0;
  }
  .menu-card{
    background:rgba(255,255,255,0.92);
    border-radius:20px;
    padding:clamp(10px,1.6vh,16px);
    display:flex; flex-direction:column; align-items:center; justify-content:center; gap:6px;
    text-align:center;
    box-shadow:0 6px 0 rgba(0,0,0,0.08), 0 8px 16px rgba(0,0,0,0.12);
    cursor:pointer;
    border:3px solid transparent;
    transition:transform .15s ease, border-color .15s ease;
  }
  .menu-card:hover{ transform:translateY(-4px); border-color:var(--secondary); }
  .menu-card:active{ transform:translateY(0px) scale(0.98); }
  .menu-icon-box{
    width:clamp(66px,9vh,88px); height:clamp(66px,9vh,88px);
    border-radius:22px;
    display:flex; align-items:center; justify-content:center;
    font-size:clamp(34px,4.8vh,46px);
    box-shadow:0 4px 10px rgba(0,0,0,0.18);
  }
  .menu-card h3{ font-size:clamp(15px,2vh,20px); font-weight:800; color:var(--ink); }
  .menu-card p{ font-size:clamp(11px,1.4vh,13px); font-weight:600; color:#475569; }
  .mc1 .menu-icon-box{ background:linear-gradient(135deg,#fbbf24,#f59e0b);}
  .mc2 .menu-icon-box{ background:linear-gradient(135deg,#60a5fa,#2563eb);}
  .mc3 .menu-icon-box{ background:linear-gradient(135deg,#34d399,#059669);}
  .mc4 .menu-icon-box{ background:linear-gradient(135deg,#f472b6,#db2777);}
  .mc5 .menu-icon-box{ background:linear-gradient(135deg,#a78bfa,#7c3aed);}
  .mc6 .menu-icon-box{ background:linear-gradient(135deg,#38bdf8,#0ea5e9);}

  /* ============ TUJUAN ============ */
  #page-tujuan{ align-items:center; }
  .tp-card{
    background:rgba(255,255,255,0.94);
    border-radius:24px;
    padding:clamp(16px,2.6vh,26px) clamp(24px,3.4vw,40px);
    max-width:90%;
    box-shadow:0 8px 20px rgba(0,0,0,0.15);
    flex:1; width:100%;
    display:flex; flex-direction:column; justify-content:center; gap:clamp(8px,1.4vh,14px);
    overflow:hidden;
  }
  .tp-heading{
    font-size:clamp(16px,2.2vh,20px); font-weight:900; color:var(--primary-dark);
    text-align:center; margin-bottom:4px;
  }
  .tp-item{
    display:flex; align-items:flex-start; gap:12px;
    background:#f0f9ff; border-radius:14px; padding:clamp(8px,1.3vh,12px) clamp(12px,1.8vw,16px);
    border-left:6px solid var(--secondary);
  }
  .tp-item .tp-ico{ font-size:clamp(20px,2.6vh,26px); flex-shrink:0; }
  .tp-item p{ font-size:clamp(13.5px,1.65vh,16px); font-weight:600; line-height:1.45; color:#1e293b; }

  /* ============ MATERI ============ */
  #materi-grid{
    flex:1; display:grid; grid-template-columns:repeat(3,1fr); grid-template-rows:repeat(2,1fr);
    gap:clamp(8px,1.4vw,14px); min-height:0;
  }
  .materi-card{
    background:rgba(255,255,255,0.94); border-radius:18px; overflow:hidden;
    display:flex; flex-direction:column; box-shadow:0 5px 14px rgba(0,0,0,0.13);
  }
  .materi-visual{
    height:42%; display:flex; align-items:center; justify-content:center;
    font-size:clamp(30px,4.5vh,44px);
    color:white;
  }
  .materi-body{ flex:1; padding:clamp(6px,1vh,10px) clamp(10px,1.4vw,14px); display:flex; flex-direction:column; gap:3px; min-height:0; overflow:hidden; }
  .materi-body h4{ font-size:clamp(13px,1.7vh,16px); font-weight:800; color:var(--ink); }
  .materi-body p{ font-size:clamp(10.5px,1.35vh,12.5px); font-weight:600; color:#475569; line-height:1.35; }
  .mt1 .materi-visual{ background:linear-gradient(135deg,#60a5fa,#2563eb); }
  .mt2 .materi-visual{ background:linear-gradient(135deg,#fbbf24,#f59e0b); }
  .mt3 .materi-visual{ background:linear-gradient(135deg,#34d399,#059669); }
  .mt4 .materi-visual{ background:linear-gradient(135deg,#f472b6,#db2777); }
  .mt5 .materi-visual{ background:linear-gradient(135deg,#a78bfa,#7c3aed); }
  .mt6 .materi-visual{ background:linear-gradient(135deg,#38bdf8,#0ea5e9); }

  /* ============ SIMULASI ============ */
  #page-simulasi{ gap:8px; }
  .sim-wrap{
    flex:1; display:flex; flex-direction:column; background:rgba(255,255,255,0.9);
    border-radius:18px; padding:clamp(8px,1.3vh,14px); min-height:0; box-shadow:0 6px 16px rgba(0,0,0,0.12);
  }
  .sim-controls{
    display:flex; flex-wrap:wrap; align-items:center; gap:10px; margin-bottom:6px; flex-shrink:0;
  }
  .sim-btn{
    background:linear-gradient(135deg,var(--primary),var(--secondary));
    color:white; border:none; border-radius:14px; font-weight:800; cursor:pointer;
    font-size:clamp(13.5px,1.8vh,17px); padding:10px 22px;
    box-shadow:0 4px 10px rgba(37,99,235,0.3);
  }
  .sim-btn.alt{ background:linear-gradient(135deg,#f59e0b,#f97316); box-shadow:0 4px 10px rgba(245,158,11,0.3);}
  .sim-btn.alt2{ background:linear-gradient(135deg,#10b981,#059669); box-shadow:0 4px 10px rgba(16,185,129,0.3);}
  .sim-status{
    margin-left:auto; font-weight:800; font-size:clamp(12px,1.6vh,14px);
    background:#f0f9ff; padding:6px 14px; border-radius:12px; color:var(--primary-dark);
  }
  #sim-svg-holder{ flex:1; min-height:0; display:flex; align-items:center; justify-content:center; }
  #sim-svg{ height:clamp(260px,38vh,420px); width:100%; }

  /* ============ EVALUASI ============ */
  #page-evaluasi{ gap:6px; }
  .eval-top{
    display:flex; align-items:center; justify-content:space-between; flex-shrink:0;
  }
  .eval-progress{ display:flex; gap:6px; }
  .eval-dot{ width:12px; height:12px; border-radius:50%; background:#cbd5e1; }
  .eval-dot.active{ background:var(--primary); }
  .eval-dot.done{ background:var(--accent2); }
  .eval-score-badge{
    font-weight:900; font-size:clamp(13px,1.7vh,15px); color:white;
    background:linear-gradient(135deg,var(--accent1),#f97316);
    padding:6px 16px; border-radius:14px;
  }
  .eval-body{ flex:1; min-height:0; display:flex; flex-direction:column; }
  .eval-panel{ display:none; flex:1; flex-direction:column; min-height:0; }
  .eval-panel.active{ display:flex; }

  .q-card{
    background:rgba(255,255,255,0.94); border-radius:18px; padding:clamp(12px,2vh,20px);
    flex:1; display:flex; flex-direction:column; gap:clamp(8px,1.4vh,14px); min-height:0;
  }
  .q-label{ font-size:clamp(11px,1.4vh,13px); font-weight:800; color:var(--primary-dark); }
  .q-text{ font-size:clamp(14px,1.9vh,18px); font-weight:800; color:var(--ink); line-height:1.35; }
  .q-options{ display:grid; grid-template-columns:1fr 1fr; gap:clamp(8px,1.2vh,12px); flex:1; }
  .opt-btn{
    background:#f0f9ff; border:2.5px solid #bae6fd; border-radius:14px;
    font-size:clamp(13.5px,1.8vh,17px); font-weight:700; padding:10px 22px;
    cursor:pointer; text-align:left; color:#1e293b;
    display:flex; align-items:center;
  }
  .opt-btn:hover{ border-color:var(--primary); }
  .opt-btn.correct{ background:#dcfce7; border-color:var(--accent2); color:#065f46; }
  .opt-btn.wrong{ background:#fee2e2; border-color:var(--danger); color:#991b1b; }
  .q-nav{ display:flex; justify-content:flex-end; }
  .btn-next{
    background:linear-gradient(135deg,var(--primary),var(--secondary)); color:white; border:none;
    border-radius:14px; padding:10px 26px; font-weight:800; cursor:pointer;
    font-size:clamp(13px,1.7vh,16px); box-shadow:0 4px 10px rgba(37,99,235,0.3);
    visibility:hidden;
  }
  .btn-next.show{ visibility:visible; }

  .tf-buttons{ display:flex; gap:16px; justify-content:center; flex:1; align-items:center; }
  .tf-btn{
    width:clamp(90px,13vh,130px); height:clamp(60px,9vh,80px); border-radius:18px; border:none;
    font-weight:900; font-size:clamp(16px,2.2vh,20px); cursor:pointer; color:white;
  }
  .tf-true{ background:linear-gradient(135deg,#34d399,#059669); }
  .tf-false{ background:linear-gradient(135deg,#f87171,#dc2626); }

  #match-svg-holder{ flex:1; min-height:0; }
  #match-svg{ width:100%; height:100%; }
  .match-item-rect{ cursor:pointer; }

  .recap-wrap{
    flex:1; display:flex; flex-direction:column; align-items:center; justify-content:center; gap:clamp(6px,1.2vh,10px);
  }
  .trophy{ font-size:clamp(44px,6vh,60px); filter:drop-shadow(0 4px 6px rgba(0,0,0,0.25)); }
  .recap-total{ font-size:clamp(2.2rem,4.6vw,3.4rem); font-weight:900; color:var(--primary-dark); }
  .recap-breakdown{ display:flex; gap:14px; }
  .recap-chip{ background:#f0f9ff; border-radius:12px; padding:6px 16px; font-weight:800; font-size:clamp(12px,1.5vh,14px); color:#1e293b; }
  .recap-form{ display:flex; gap:8px; align-items:center; }
  .recap-form input{
    border:2px solid #bae6fd; border-radius:10px; padding:8px 12px; font-size:clamp(12px,1.5vh,14px);
    font-weight:600;
  }
  .btn-small{
    background:linear-gradient(135deg,var(--primary),var(--secondary)); color:white; border:none;
    border-radius:12px; padding:8px 18px; font-weight:800; cursor:pointer; font-size:clamp(12px,1.5vh,14px);
  }
  #leaderboard-list{ max-height:12vh; overflow:hidden; font-size:clamp(11px,1.4vh,13px); display:flex; flex-direction:column; gap:3px; width:min(60%,420px); }
  .lb-row{ display:flex; justify-content:space-between; background:white; border-radius:8px; padding:3px 10px; font-weight:700; }

  /* ============ PROFIL ============ */
  #page-profil{ align-items:center; justify-content:center; }
  .profil-card{
    background:rgba(255,255,255,0.94); border-radius:26px;
    padding:clamp(16px,2.6vh,26px) clamp(30px,4vw,50px);
    display:flex; align-items:center; gap:clamp(20px,3vw,40px);
    box-shadow:0 8px 22px rgba(0,0,0,0.16); max-width:92%;
  }
  .profil-photo{
    width:clamp(130px,18vh,175px); height:clamp(130px,18vh,175px);
    border-radius:50%; object-fit:cover; flex-shrink:0;
    border:5px solid transparent;
    background:linear-gradient(white,white) padding-box, linear-gradient(135deg,var(--primary),var(--accent3)) border-box;
    box-shadow:0 8px 18px rgba(0,0,0,0.25);
  }
  .profil-info{ display:flex; flex-direction:column; gap:6px; }
  .profil-name{ font-size:clamp(20px,3vh,24px); font-weight:900; color:var(--ink); }
  .profil-role{ font-size:clamp(13px,1.7vh,15px); font-weight:700; color:var(--primary-dark); }
  .profil-school{ font-size:clamp(13px,1.7vh,15px); font-weight:600; color:#475569; }
  .profil-quote{
    margin-top:8px; font-size:clamp(12px,1.6vh,14px); font-style:italic; font-weight:600;
    color:#334155; background:#f0f9ff; padding:8px 14px; border-radius:12px; max-width:420px;
  }

  /* ============ WAWASAN POPUP ============ */
  #wawasan-overlay{
    position:absolute; inset:0; background:rgba(15,23,42,0.55);
    display:none; align-items:center; justify-content:center; z-index:50;
  }
  #wawasan-overlay.show{ display:flex; }
  .wawasan-box{
    background:white; border-radius:22px; padding:clamp(18px,3vh,28px) clamp(20px,3.4vw,34px);
    max-width:64%; text-align:center; box-shadow:0 12px 30px rgba(0,0,0,0.35);
    display:flex; flex-direction:column; gap:10px; align-items:center;
  }
  .wawasan-icon{ font-size:clamp(34px,5vh,46px); }
  .wawasan-title{ font-size:clamp(15px,2vh,19px); font-weight:900; color:var(--primary-dark); }
  .wawasan-text{ font-size:clamp(13px,1.7vh,16px); font-weight:600; color:#334155; line-height:1.5; }

  .pill-label{ font-weight:800; }
  img[data-fallback]{ }
</style>
</head>
<body>
<div id="app-wrap">
<div id="stage-16-9">

  <!-- ============ PAGE 1: COVER ============ -->
  <div class="page active" id="page-cover">
    <div class="cover-header">
      <div class="school-pill">
        <img src="logo_sekolah.jfif" alt="Logo Sekolah" onerror="this.style.display='none'">
        <div class="school-text">
          <div class="school-name" id="cover-school-name">SMP Negeri 1 Pucuk</div>
          <div class="school-sub">Dinas Pendidikan • Kurikulum Merdeka</div>
        </div>
      </div>
      <div class="badge-row">
        <div class="badge-chip">Kurikulum Merdeka</div>
        <div class="badge-chip">Fase D</div>
      </div>
    </div>

    <div class="cover-center">
      <div class="cover-eyebrow">Media Pembelajaran Interaktif</div>
      <div class="cover-title">Analisis Data</div>
      <div class="cover-subtitle">Informatika — Kelas VIII SMP</div>
      <div class="cover-desc">Belajar mencari, menyaring, memvisualisasikan, dan meringkas data dalam aplikasi pengolah lembar kerja</div>
      <button id="btn-mulai" onclick="playClick(); goTo('menu');">▶</button>
      <div id="btn-mulai-label">MULAI</div>
    </div>

    <div class="cover-footer">
      <span>👩‍🏫 Disusun oleh <b id="cover-guru-name">Karinika Susanto Putri, S.Pd.</b></span>
      <span>•</span>
      <span id="cover-school-name2">SMP Negeri 1 Pucuk</span>
      <span>•</span>
      <span>"Data yang terolah adalah keputusan yang tercerahkan"</span>
    </div>
  </div>

  <!-- ============ PAGE 2: MENU ============ -->
  <div class="page" id="page-menu">
    <div id="topnav">
      <div class="nav-left">
        <button class="nav-btn" onclick="playClick(); goTo('cover');" title="Beranda">🏠</button>
        <div class="nav-home-label" onclick="playClick(); goTo('cover');">BERANDA</div>
      </div>
      <div class="nav-right">
        <div class="zoom-controls">
          <button onclick="changeFont(-1)">A-</button>
          <button onclick="changeFont(1)">A+</button>
        </div>
        <button class="nav-btn" id="btn-sound" onclick="toggleSound()" title="Suara">🔊</button>
      </div>
    </div>
    <div class="page-title-bar">📚 Menu Utama</div>
    <div id="menu-grid">
      <div class="menu-card mc1" onclick="playClick(); goTo('tujuan');">
        <div class="menu-icon-box">🎯</div>
        <h3>Tujuan Pembelajaran</h3>
        <p>Capaian &amp; indikator kompetensi siswa</p>
      </div>
      <div class="menu-card mc2" onclick="playClick(); goTo('materi');">
        <div class="menu-icon-box">📖</div>
        <h3>Materi Pembelajaran</h3>
        <p>Eksplorasi konsep kunci analisis data</p>
      </div>
      <div class="menu-card mc3" onclick="playClick(); goTo('simulasi');">
        <div class="menu-icon-box">🕹️</div>
        <h3>Simulasi Interaktif</h3>
        <p>Laboratorium olah data lembar kerja</p>
      </div>
      <div class="menu-card mc4" onclick="playClick(); goTo('evaluasi');">
        <div class="menu-icon-box">📝</div>
        <h3>Evaluasi Multi-Format</h3>
        <p>3 babak uji pemahaman • 100 poin</p>
      </div>
      <div class="menu-card mc5" onclick="playClick(); goTo('profil');">
        <div class="menu-icon-box">👨‍🏫</div>
        <h3>Profil Guru</h3>
        <p>Informasi fasilitator pendidik</p>
      </div>
      <div class="menu-card mc6" onclick="playClick(); openWawasan();">
        <div class="menu-icon-box">💡</div>
        <h3>Wawasan &amp; Fakta</h3>
        <p>Fakta menarik seputar data</p>
      </div>
    </div>
  </div>

  <!-- ============ PAGE 3: TUJUAN ============ -->
  <div class="page" id="page-tujuan">
    <div id="topnav-tujuan-slot"></div>
    <div class="page-title-bar">🎯 Tujuan Pembelajaran</div>
    <div class="tp-card">
      <div class="tp-heading">Setelah mempelajari materi ini, murid mampu:</div>
      <div class="tp-item"><span class="tp-ico">🔍</span><p>Menjelaskan dan mempraktikkan cara <b>pencarian data</b> (sort &amp; filter) pada aplikasi pengolah lembar kerja secara tepat.</p></div>
      <div class="tp-item"><span class="tp-ico">📊</span><p>Membuat <b>visualisasi data</b> dalam bentuk grafik/diagram yang sesuai untuk menyajikan informasi secara jelas.</p></div>
      <div class="tp-item"><span class="tp-ico">🧮</span><p>Melakukan <b>peringkasan data</b> menggunakan fungsi statistik dasar (SUM, AVERAGE, COUNT, MAX, MIN).</p></div>
      <div class="tp-item"><span class="tp-ico">🧠</span><p>Menginterpretasikan hasil olahan data untuk mendukung pengambilan keputusan sehari-hari.</p></div>
    </div>
  </div>

  <!-- ============ PAGE 4: MATERI ============ -->
  <div class="page" id="page-materi">
    <div id="topnav-materi-slot"></div>
    <div class="page-title-bar">📖 Materi Inti: Analisis Data</div>
    <div id="materi-grid">
      <div class="materi-card mt1">
        <div class="materi-visual">🔍</div>
        <div class="materi-body"><h4>Pencarian Data</h4><p>Menggunakan fitur Find, Filter, dan fungsi lookup untuk menemukan data tertentu dengan cepat.</p></div>
      </div>
      <div class="materi-card mt2">
        <div class="materi-visual">↕️</div>
        <div class="materi-body"><h4>Pengurutan &amp; Penyaringan</h4><p>Sort (A-Z/Z-A) dan Filter membantu menyusun serta menampilkan data sesuai kriteria tertentu.</p></div>
      </div>
      <div class="materi-card mt3">
        <div class="materi-visual">📈</div>
        <div class="materi-body"><h4>Visualisasi Data</h4><p>Grafik batang, garis, dan lingkaran mengubah angka menjadi gambar yang mudah dipahami.</p></div>
      </div>
      <div class="materi-card mt4">
        <div class="materi-visual">Σ</div>
        <div class="materi-body"><h4>Peringkasan Data</h4><p>Fungsi SUM, AVERAGE, COUNT meringkas kumpulan data menjadi informasi inti.</p></div>
      </div>
      <div class="materi-card mt5">
        <div class="materi-visual">📐</div>
        <div class="materi-body"><h4>Fungsi Statistik Dasar</h4><p>MAX, MIN, dan MEDIAN membantu menemukan nilai tertinggi, terendah, dan tengah.</p></div>
      </div>
      <div class="materi-card mt6">
        <div class="materi-visual">🧭</div>
        <div class="materi-body"><h4>Interpretasi Data</h4><p>Membaca pola data untuk mengambil keputusan yang tepat dan berbasis bukti.</p></div>
      </div>
    </div>
  </div>

  <!-- ============ PAGE 5: SIMULASI ============ -->
  <div class="page" id="page-simulasi">
    <div id="topnav-simulasi-slot"></div>
    <div class="page-title-bar">🕹️ Simulasi: Laboratorium Olah Data</div>
    <div class="sim-wrap">
      <div class="sim-controls">
        <button class="sim-btn" onclick="simSort()">↕️ Urutkan Nilai</button>
        <button class="sim-btn alt" onclick="simSearchMax()">🔍 Cari Nilai Tertinggi</button>
        <button class="sim-btn alt2" onclick="simAverage()">Σ Tampilkan Rata-rata</button>
        <button class="sim-btn" style="background:linear-gradient(135deg,#94a3b8,#64748b)" onclick="simReset()">↺ Reset</button>
        <div class="sim-status" id="sim-status">Mode: Data Awal</div>
      </div>
      <div id="sim-svg-holder">
        <svg id="sim-svg" viewBox="0 0 700 360" xmlns="http://www.w3.org/2000/svg"></svg>
      </div>
    </div>
  </div>

  <!-- ============ PAGE 6: EVALUASI ============ -->
  <div class="page" id="page-evaluasi">
    <div id="topnav-evaluasi-slot"></div>
    <div class="eval-top">
      <div class="page-title-bar" style="margin-bottom:0;">📝 Evaluasi Pemahaman</div>
      <div class="eval-progress" id="eval-progress">
        <div class="eval-dot active" data-b="A"></div>
        <div class="eval-dot" data-b="B"></div>
        <div class="eval-dot" data-b="C"></div>
        <div class="eval-dot" data-b="D"></div>
      </div>
      <div class="eval-score-badge">Skor: <span id="eval-score-live">0</span>/100</div>
    </div>
    <div class="eval-body">

      <!-- Babak A -->
      <div class="eval-panel active" id="babak-A">
        <div class="q-card">
          <div class="q-label" id="qa-label">Babak A • Pilihan Ganda — Soal 1 dari 3</div>
          <div class="q-text" id="qa-text"></div>
          <div class="q-options" id="qa-options"></div>
          <div class="q-nav"><button class="btn-next" id="qa-next" onclick="nextA()">Lanjut ➜</button></div>
        </div>
      </div>

      <!-- Babak B -->
      <div class="eval-panel" id="babak-B">
        <div class="q-card">
          <div class="q-label">Babak B • Menjodohkan — Klik istilah lalu klik pasangannya</div>
          <div id="match-svg-holder">
            <svg id="match-svg" viewBox="0 0 700 340" xmlns="http://www.w3.org/2000/svg"></svg>
          </div>
        </div>
      </div>

      <!-- Babak C -->
      <div class="eval-panel" id="babak-C">
        <div class="q-card">
          <div class="q-label" id="qc-label">Babak C • Benar/Salah — Soal 1 dari 3</div>
          <div class="q-text" id="qc-text"></div>
          <div class="tf-buttons">
            <button class="tf-btn tf-true" onclick="answerTF(true)">✓ BENAR</button>
            <button class="tf-btn tf-false" onclick="answerTF(false)">✗ SALAH</button>
          </div>
          <div class="q-nav"><button class="btn-next" id="qc-next" onclick="nextC()">Lanjut ➜</button></div>
        </div>
      </div>

      <!-- Babak D -->
      <div class="eval-panel" id="babak-D">
        <div class="recap-wrap">
          <div class="trophy" id="recap-trophy">🏆</div>
          <div class="recap-total"><span id="recap-total-num">0</span> / 100</div>
          <div class="recap-breakdown">
            <div class="recap-chip">Babak A: <span id="recap-a">0</span>/30</div>
            <div class="recap-chip">Babak B: <span id="recap-b">0</span>/40</div>
            <div class="recap-chip">Babak C: <span id="recap-c">0</span>/30</div>
          </div>
          <div class="recap-form">
            <input type="text" id="student-name-input" placeholder="Nama murid...">
            <button class="btn-small" onclick="saveScore()">💾 Simpan Skor</button>
            <button class="btn-small" style="background:linear-gradient(135deg,#94a3b8,#64748b)" onclick="resetEvaluasi()">↺ Ulangi</button>
          </div>
          <div id="leaderboard-list"></div>
        </div>
      </div>

    </div>
  </div>

  <!-- ============ PAGE 7: PROFIL ============ -->
  <div class="page" id="page-profil">
    <div id="topnav-profil-slot"></div>
    <div class="profil-card">
      <img class="profil-photo" src="foto_guru.jpeg" alt="Foto Guru" onerror="this.style.display='none'">
      <div class="profil-info">
        <div class="profil-name">Karinika Susanto Putri, S.Pd.</div>
        <div class="profil-role">Guru Mata Pelajaran Informatika</div>
        <div class="profil-school">SMP Negeri 1 Pucuk</div>
        <div class="profil-quote">"Data yang diolah dengan baik akan menuntun kita pada keputusan yang lebih bijak."</div>
      </div>
    </div>
  </div>

  <!-- WAWASAN POPUP -->
  <div id="wawasan-overlay">
    <div class="wawasan-box">
      <div class="wawasan-icon" id="wawasan-icon">💡</div>
      <div class="wawasan-title" id="wawasan-title">Tahukah Kamu?</div>
      <div class="wawasan-text" id="wawasan-text"></div>
      <button class="btn-small" onclick="closeWawasan()">Tutup</button>
    </div>
  </div>

</div>
</div>

<script>
/* ============================================================
   NAVIGASI & TOPNAV
============================================================ */
const topnavHTML = `
  <div class="nav-left">
    <button class="nav-btn" onclick="playClick(); goTo('cover');" title="Beranda">🏠</button>
    <div class="nav-home-label" onclick="playClick(); goTo('cover');">BERANDA</div>
  </div>
  <div class="nav-right">
    <div class="zoom-controls">
      <button onclick="changeFont(-1)">A-</button>
      <button onclick="changeFont(1)">A+</button>
    </div>
    <button class="nav-btn" id="btn-sound-clone" onclick="toggleSound()" title="Suara">🔊</button>
  </div>
`;
['tujuan','materi','simulasi','evaluasi','profil'].forEach(id=>{
  const slot = document.getElementById('topnav-'+id+'-slot');
  const nav = document.createElement('div');
  nav.id = 'topnav';
  nav.innerHTML = topnavHTML;
  slot.replaceWith(nav);
});

function goTo(pageId){
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById('page-'+pageId).classList.add('active');
  if(pageId==='simulasi') initSim();
  if(pageId==='evaluasi') { /* keep current state */ }
}

function changeFont(dir){
  let scale = parseFloat(getComputedStyle(document.getElementById('stage-16-9')).getPropertyValue('--font-scale')) || 1.0;
  scale = Math.min(1.25, Math.max(0.85, scale + dir*0.05));
  document.getElementById('stage-16-9').style.setProperty('--font-scale', scale);
}

/* ============================================================
   AUDIO — Web Audio API sintetis (100% offline)
============================================================ */
let soundOn = true;
let actx = null;
function getCtx(){
  if(!actx){ try{ actx = new (window.AudioContext || window.webkitAudioContext)(); }catch(e){ actx=null; } }
  return actx;
}
function toneAt(ctx, freq, start, dur, type, vol){
  const osc = ctx.createOscillator();
  const gain = ctx.createGain();
  osc.type = type || 'sine';
  osc.frequency.setValueAtTime(freq, ctx.currentTime+start);
  gain.gain.setValueAtTime(0, ctx.currentTime+start);
  gain.gain.linearRampToValueAtTime(vol||0.15, ctx.currentTime+start+0.02);
  gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime+start+dur);
  osc.connect(gain); gain.connect(ctx.destination);
  osc.start(ctx.currentTime+start);
  osc.stop(ctx.currentTime+start+dur+0.05);
}
function playClick(){
  if(!soundOn) return;
  const ctx = getCtx(); if(!ctx) return;
  const osc = ctx.createOscillator();
  const gain = ctx.createGain();
  osc.type='sine';
  osc.frequency.setValueAtTime(450, ctx.currentTime);
  osc.frequency.exponentialRampToValueAtTime(880, ctx.currentTime+0.12);
  gain.gain.setValueAtTime(0.16, ctx.currentTime);
  gain.gain.exponentialRampToValueAtTime(0.001, ctx.currentTime+0.14);
  osc.connect(gain); gain.connect(ctx.destination);
  osc.start(); osc.stop(ctx.currentTime+0.15);
}
function playCorrect(){
  if(!soundOn) return;
  const ctx = getCtx(); if(!ctx) return;
  toneAt(ctx,523.25,0,0.28,'triangle',0.15);
  toneAt(ctx,659.25,0.05,0.28,'triangle',0.15);
  toneAt(ctx,783.99,0.1,0.32,'triangle',0.15);
}
function playWrong(){
  if(!soundOn) return;
  const ctx = getCtx(); if(!ctx) return;
  toneAt(ctx,180,0,0.35,'sawtooth',0.14);
  toneAt(ctx,140,0.08,0.35,'sawtooth',0.12);
}
function toggleSound(){
  soundOn = !soundOn;
  const icon = soundOn ? '🔊' : '🔇';
  document.getElementById('btn-sound').textContent = icon;
  const clone = document.getElementById('btn-sound-clone');
  if(clone) clone.textContent = icon;
  document.querySelectorAll('[id^="btn-sound"]').forEach(b=>b.textContent=icon);
  playClick();
}

/* ============================================================
   WAWASAN & FAKTA
============================================================ */
const wawasanFacts = [
  {icon:'💡', title:'Tahukah Kamu?', text:'Aplikasi pengolah lembar kerja digital pertama kali populer sekitar akhir tahun 1970-an dan mengubah cara orang menghitung data secara masif.'},
  {icon:'📊', title:'Fakta Visualisasi', text:'Otak manusia memproses gambar jauh lebih cepat daripada teks, sehingga grafik membantu kita memahami data dalam hitungan detik.'},
  {icon:'🔍', title:'Fakta Pencarian Data', text:'Fitur Filter dan Sort dapat menyaring ribuan baris data hanya dalam beberapa klik, menghemat waktu dibanding memeriksa satu per satu.'},
  {icon:'🧮', title:'Fakta Fungsi Statistik', text:'Fungsi AVERAGE (rata-rata) sering dipakai untuk menilai performa, misalnya nilai rata-rata ulangan satu kelas.'},
  {icon:'🧠', title:'Fakta Pengambilan Keputusan', text:'Banyak keputusan penting di dunia nyata—mulai dari bisnis hingga kebijakan publik—didasarkan pada hasil analisis data yang akurat.'}
];
function openWawasan(){
  const f = wawasanFacts[Math.floor(Math.random()*wawasanFacts.length)];
  document.getElementById('wawasan-icon').textContent = f.icon;
  document.getElementById('wawasan-title').textContent = f.title;
  document.getElementById('wawasan-text').textContent = f.text;
  document.getElementById('wawasan-overlay').classList.add('show');
}
function closeWawasan(){
  playClick();
  document.getElementById('wawasan-overlay').classList.remove('show');
}

/* ============================================================
   SIMULASI — Laboratorium Olah Data (SVG interaktif)
============================================================ */
let simData = [
  {name:'Andi', val:70},
  {name:'Budi', val:85},
  {name:'Citra', val:92},
  {name:'Dedi', val:60},
  {name:'Eka', val:78}
];
let simOriginal = JSON.parse(JSON.stringify(simData));
let simMode = 'awal';

function initSim(){
  simMode='awal';
  document.getElementById('sim-status').textContent = 'Mode: Data Awal';
  renderSim();
}

function renderSim(highlightMax){
  const svg = document.getElementById('sim-svg');
  const W=700,H=360, baseY=300, chartTop=40, chartH=baseY-chartTop;
  const barW=70, gap=(W-40-(barW*simData.length))/(simData.length+1);
  const maxVal = 100;
  let showAvg = simMode==='rata2';
  const avg = simData.reduce((a,b)=>a+b.val,0)/simData.length;

  let svgContent = `<defs>
    <linearGradient id="barGrad" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%" stop-color="#60a5fa"/>
      <stop offset="100%" stop-color="#2563eb"/>
    </linearGradient>
    <linearGradient id="barGradMax" x1="0" y1="0" x2="0" y2="1">
      <stop offset="0%" stop-color="#fbbf24"/>
      <stop offset="100%" stop-color="#f59e0b"/>
    </linearGradient>
  </defs>`;

  svgContent += `<line x1="30" y1="${baseY}" x2="${W-10}" y2="${baseY}" stroke="#94a3b8" stroke-width="3"/>`;

  if(showAvg){
    const avgY = baseY - (avg/maxVal)*chartH;
    svgContent += `<line x1="30" y1="${avgY}" x2="${W-10}" y2="${avgY}" stroke="#ec4899" stroke-width="4" stroke-dasharray="10 8">
      <animate attributeName="stroke-dashoffset" from="0" to="-36" dur="1.2s" repeatCount="indefinite"/>
    </line>
    <rect x="${W-160}" y="${avgY-26}" width="150" height="24" rx="8" fill="white" stroke="#ec4899" stroke-width="1.5"/>
    <text x="${W-85}" y="${avgY-9}" font-size="14" font-weight="800" fill="#be185d" text-anchor="middle">Rata-rata: ${avg.toFixed(1)}</text>`;
  }

  simData.forEach((d,i)=>{
    const x = gap + i*(barW+gap);
    const h = (d.val/maxVal)*chartH;
    const y = baseY - h;
    const isMax = highlightMax && d.val === Math.max(...simData.map(x=>x.val));
    const fill = isMax ? 'url(#barGradMax)' : 'url(#barGrad)';
    svgContent += `<rect x="${x}" y="${baseY}" width="${barW}" height="0" rx="8" fill="${fill}">
        <animate attributeName="y" from="${baseY}" to="${y}" dur="0.6s" fill="freeze" begin="${i*0.08}s"/>
        <animate attributeName="height" from="0" to="${h}" dur="0.6s" fill="freeze" begin="${i*0.08}s"/>
      </rect>`;
    if(isMax){
      svgContent += `<circle cx="${x+barW/2}" cy="${y-14}" r="16" fill="none" stroke="#f59e0b" stroke-width="4">
        <animate attributeName="r" values="14;20;14" dur="1s" repeatCount="indefinite"/>
        <animate attributeName="opacity" values="1;0.4;1" dur="1s" repeatCount="indefinite"/>
      </circle>`;
    }
    svgContent += `<rect x="${x+barW/2-22}" y="${y-38}" width="44" height="22" rx="6" fill="white" stroke="#2563eb" stroke-width="1.5"/>
      <text x="${x+barW/2}" y="${y-22}" font-size="14" font-weight="800" fill="#1e3a8a" text-anchor="middle">${d.val}</text>`;
    svgContent += `<rect x="${x+barW/2-30}" y="${baseY+10}" width="60" height="22" rx="6" fill="white" stroke="#64748b" stroke-width="1.5"/>
      <text x="${x+barW/2}" y="${baseY+26}" font-size="13" font-weight="800" fill="#334155" text-anchor="middle">${d.name}</text>`;
  });

  svg.innerHTML = svgContent;
}

function simSort(){
  playClick();
  simData.sort((a,b)=>b.val-a.val);
  simMode='urut';
  document.getElementById('sim-status').textContent = 'Mode: Data Terurut (Tertinggi → Terendah)';
  renderSim();
}
function simSearchMax(){
  playClick();
  simMode='cari';
  document.getElementById('sim-status').textContent = 'Mode: Nilai Tertinggi Disorot';
  renderSim(true);
}
function simAverage(){
  playClick();
  simMode='rata2';
  document.getElementById('sim-status').textContent = 'Mode: Menampilkan Rata-rata';
  renderSim();
}
function simReset(){
  playClick();
  simData = JSON.parse(JSON.stringify(simOriginal));
  simMode='awal';
  document.getElementById('sim-status').textContent = 'Mode: Data Awal';
  renderSim();
}

/* ============================================================
   EVALUASI
============================================================ */
let scoreA=0, scoreB=0, scoreC=0;
let idxA=0, idxC=0;
let answeredA=false, answeredC=false;

const questionsA = [
  {q:'Fungsi manakah yang digunakan untuk menjumlahkan sekumpulan data angka pada lembar kerja?',
   opts:['SUM','SORT','FILTER','FIND'], correct:0},
  {q:'Fitur apa yang digunakan untuk menampilkan hanya data yang memenuhi kriteria tertentu?',
   opts:['Filter','Average','Chart','Bold'], correct:0},
  {q:'Grafik apa yang paling tepat digunakan untuk membandingkan jumlah antar kategori?',
   opts:['Grafik Batang','Font Miring','Border Sel','Pewarnaan Acak'], correct:0}
];
const questionsC = [
  {q:'Fungsi AVERAGE digunakan untuk mencari nilai rata-rata dari sekumpulan data.', correct:true},
  {q:'Mengurutkan data (Sort) akan mengubah nilai asli dari data tersebut.', correct:false},
  {q:'Visualisasi data dalam bentuk grafik memudahkan orang lain memahami informasi.', correct:true}
];
const matchPairs = [
  {term:'SUM', def:'Menjumlahkan data'},
  {term:'AVERAGE', def:'Merata-ratakan data'},
  {term:'SORT', def:'Mengurutkan data'},
  {term:'FILTER', def:'Menyaring data'}
];

function updateScoreLive(){
  document.getElementById('eval-score-live').textContent = scoreA+scoreB+scoreC;
}

function setBabakDot(letter, state){
  const dot = document.querySelector(`.eval-dot[data-b="${letter}"]`);
  dot.classList.remove('active','done');
  if(state) dot.classList.add(state);
}

function showPanel(letter){
  document.querySelectorAll('.eval-panel').forEach(p=>p.classList.remove('active'));
  document.getElementById('babak-'+letter).classList.add('active');
}

/* ---- Babak A ---- */
function renderA(){
  answeredA=false;
  const item = questionsA[idxA];
  document.getElementById('qa-label').textContent = `Babak A • Pilihan Ganda — Soal ${idxA+1} dari 3`;
  document.getElementById('qa-text').textContent = item.q;
  const optsHolder = document.getElementById('qa-options');
  optsHolder.innerHTML='';
  item.opts.forEach((opt,i)=>{
    const btn = document.createElement('button');
    btn.className='opt-btn';
    btn.textContent = opt;
    btn.onclick=()=>answerA(i,btn);
    optsHolder.appendChild(btn);
  });
  document.getElementById('qa-next').classList.remove('show');
}
function answerA(i,btn){
  if(answeredA) return;
  answeredA=true;
  const item = questionsA[idxA];
  const allBtns = document.querySelectorAll('#qa-options .opt-btn');
  if(i===item.correct){
    btn.classList.add('correct'); scoreA+=10; playCorrect();
  } else {
    btn.classList.add('wrong'); allBtns[item.correct].classList.add('correct'); playWrong();
  }
  updateScoreLive();
  document.getElementById('qa-next').classList.add('show');
}
function nextA(){
  playClick();
  idxA++;
  if(idxA>=questionsA.length){
    document.getElementById('recap-a').textContent = scoreA;
    setBabakDot('A','done'); setBabakDot('B','active');
    idxB_setup();
    showPanel('B');
  } else {
    renderA();
  }
}

/* ---- Babak B (Menjodohkan SVG) ---- */
let bTerms=[], bDefs=[], bSelectedTerm=null, bMatched=[];
function idxB_setup(){
  bTerms = matchPairs.map((p,i)=>({...p, idx:i}));
  bDefs = [...matchPairs].map((p,i)=>({...p, idx:i}));
  // shuffle defs display order
  bDefs = bDefs.map(d=>({...d})).sort(()=>Math.random()-0.5);
  bSelectedTerm=null; bMatched=[];
  renderMatch();
}
function renderMatch(){
  const svg = document.getElementById('match-svg');
  const W=700,H=340;
  let s = `<rect x="0" y="0" width="${W}" height="${H}" fill="transparent"/>`;
  const rowH = H/matchPairs.length;

  bTerms.forEach((t,i)=>{
    const y = 30 + i*rowH;
    const matched = bMatched.includes(t.idx);
    const selected = bSelectedTerm===t.idx;
    const fill = matched ? '#dcfce7' : (selected ? '#dbeafe' : 'white');
    const stroke = matched ? '#10b981' : (selected ? '#2563eb' : '#94a3b8');
    s += `<g class="match-item-rect" onclick="selectTerm(${t.idx})">
      <rect x="60" y="${y}" width="180" height="46" rx="12" fill="${fill}" stroke="${stroke}" stroke-width="2.5"/>
      <text x="150" y="${y+29}" font-size="15" font-weight="800" fill="#1e293b" text-anchor="middle">${t.term}</text>
    </g>`;
  });
  bDefs.forEach((d,i)=>{
    const y = 30 + i*rowH;
    const matched = bMatched.includes(d.idx);
    const fill = matched ? '#dcfce7' : 'white';
    const stroke = matched ? '#10b981' : '#94a3b8';
    s += `<g class="match-item-rect" onclick="selectDef(${d.idx})">
      <rect x="460" y="${y}" width="180" height="46" rx="12" fill="${fill}" stroke="${stroke}" stroke-width="2.5"/>
      <text x="550" y="${y+29}" font-size="13" font-weight="700" fill="#1e293b" text-anchor="middle">${d.def}</text>
    </g>`;
  });
  // draw lines for matched pairs
  bMatched.forEach(idx=>{
    const ti = bTerms.findIndex(t=>t.idx===idx);
    const di = bDefs.findIndex(d=>d.idx===idx);
    const y1 = 30 + ti*rowH + 23;
    const y2 = 30 + di*rowH + 23;
    s += `<line x1="240" y1="${y1}" x2="460" y2="${y2}" stroke="#10b981" stroke-width="3.5" stroke-linecap="round"/>`;
  });
  svg.innerHTML = s;
}
function selectTerm(idx){
  if(bMatched.includes(idx)) return;
  playClick();
  bSelectedTerm = idx;
  renderMatch();
}
function selectDef(idx){
  if(bSelectedTerm===null || bMatched.includes(idx)) return;
  if(bSelectedTerm===idx){
    bMatched.push(idx);
    scoreB+=10; playCorrect();
    updateScoreLive();
    bSelectedTerm=null;
    renderMatch();
    if(bMatched.length===matchPairs.length){
      document.getElementById('recap-b').textContent = scoreB;
      setBabakDot('B','done'); setBabakDot('C','active');
      setTimeout(()=>{ idxC=0; renderC(); showPanel('C'); }, 700);
    }
  } else {
    playWrong();
    bSelectedTerm=null;
    renderMatch();
  }
}

/* ---- Babak C ---- */
function renderC(){
  answeredC=false;
  const item = questionsC[idxC];
  document.getElementById('qc-label').textContent = `Babak C • Benar/Salah — Soal ${idxC+1} dari 3`;
  document.getElementById('qc-text').textContent = item.q;
  document.getElementById('qc-next').classList.remove('show');
}
function answerTF(val){
  if(answeredC) return;
  answeredC=true;
  const item = questionsC[idxC];
  if(val===item.correct){ scoreC+=10; playCorrect(); } else { playWrong(); }
  updateScoreLive();
  document.getElementById('qc-next').classList.add('show');
}
function nextC(){
  playClick();
  idxC++;
  if(idxC>=questionsC.length){
    document.getElementById('recap-c').textContent = scoreC;
    setBabakDot('C','done'); setBabakDot('D','active');
    finishEval();
    showPanel('D');
  } else {
    renderC();
  }
}

function finishEval(){
  const total = scoreA+scoreB+scoreC;
  document.getElementById('recap-total-num').textContent = total;
  document.getElementById('recap-a').textContent = scoreA;
  document.getElementById('recap-b').textContent = scoreB;
  document.getElementById('recap-c').textContent = scoreC;
  renderLeaderboard();
}

function saveScore(){
  playClick();
  const name = document.getElementById('student-name-input').value.trim();
  if(!name) return;
  const total = scoreA+scoreB+scoreC;
  let board = [];
  try{ board = JSON.parse(localStorage.getItem('mpi_leaderboard')||'[]'); }catch(e){ board=[]; }
  board.push({name:name, score:total});
  board.sort((a,b)=>b.score-a.score);
  board = board.slice(0,8);
  try{ localStorage.setItem('mpi_leaderboard', JSON.stringify(board)); }catch(e){}
  document.getElementById('student-name-input').value='';
  renderLeaderboard();
}
function renderLeaderboard(){
  let board=[];
  try{ board = JSON.parse(localStorage.getItem('mpi_leaderboard')||'[]'); }catch(e){ board=[]; }
  const holder = document.getElementById('leaderboard-list');
  holder.innerHTML='';
  board.forEach(b=>{
    const row = document.createElement('div');
    row.className='lb-row';
    row.innerHTML = `<span>${b.name}</span><span>${b.score}</span>`;
    holder.appendChild(row);
  });
}

function resetEvaluasi(){
  playClick();
  scoreA=0; scoreB=0; scoreC=0; idxA=0; idxC=0;
  updateScoreLive();
  setBabakDot('A','active'); setBabakDot('B',''); setBabakDot('C',''); setBabakDot('D','');
  renderA();
  idxB_setup();
  showPanel('A');
}

/* ============================================================
   INIT
============================================================ */
document.addEventListener('DOMContentLoaded', ()=>{
  renderA();
  idxB_setup();
  initSim();
  renderLeaderboard();
});
</script>
</body>
</html>
