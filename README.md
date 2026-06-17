[index.html.html](https://github.com/user-attachments/files/29033040/index.html.html)
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>구용회 고객관리 · 스케줄</title>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
  <style>
    *,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
    :root{
      --bg:#f5f5f7;--surface:#fff;--surface2:#fafafa;
      --border:#e4e4ea;--border2:#d0d0da;
      --gold:#b8963a;--gold-dk:#9a7a28;
      --black:#111;--text:#1a1a1a;
      --text-dim:#555566;--text-lt:#999aaa;
      --danger:#c0392b;--success:#1a8a4a;--blue:#1a6aa0;
      --purple:#7a3db8;--orange:#c05800;--green:#0f6e35;
      --radius:14px;--shadow:0 2px 16px rgba(0,0,0,.09);
      --font:'Noto Sans KR','Apple SD Gothic Neo',sans-serif;
    }
    html,body{min-height:100%;background:var(--bg);color:var(--text);
      font-family:var(--font);font-size:15px;line-height:1.6;overflow-x:hidden;}
    .view{display:none;animation:vIn .18s ease;}
    .view.active{display:block;}
    @keyframes vIn{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:translateY(0)}}
    .page-wrap{max-width:780px;margin:0 auto;padding:0 14px 110px;}
    .app-header{
      position:sticky;top:0;z-index:100;
      background:linear-gradient(135deg,#1a1a1a 0%,#2c2c2c 40%,#1a1a1a 100%);
      padding:0 20px;height:62px;
      display:flex;align-items:center;justify-content:space-between;
      box-shadow:0 1px 0 rgba(255,255,255,.06), 0 4px 24px rgba(0,0,0,.45);
      border-bottom:1px solid rgba(200,200,200,.13);
    }
    /* 크롬 실버 하이라이트 라인 */
    .app-header::after{
      content:"";position:absolute;left:0;right:0;bottom:0;height:1px;
      background:linear-gradient(90deg,transparent,rgba(210,210,210,.35) 30%,rgba(255,255,255,.55) 50%,rgba(210,210,210,.35) 70%,transparent);
      pointer-events:none;
    }
    .app-header-left{display:flex;align-items:center;gap:14px;}
    /* 벤츠 로고 이미지 */
    .app-logo{
      width:40px;height:40px;border-radius:50%;flex-shrink:0;
      overflow:hidden;
      box-shadow:0 0 0 1.5px rgba(255,255,255,.22), 0 2px 10px rgba(0,0,0,.55);
    }
    .app-title{
      font-size:15px;font-weight:700;color:#fff;letter-spacing:.3px;
      text-shadow:0 1px 3px rgba(0,0,0,.5);
    }
    .app-sub{font-size:10px;color:rgba(200,200,200,.75);margin-top:1px;letter-spacing:.2px;}
    .view-badge{
      font-size:10px;font-weight:700;letter-spacing:.4px;
      background:linear-gradient(135deg,rgba(255,255,255,.08),rgba(255,255,255,.04));
      color:rgba(210,210,210,.9);
      border-radius:4px;padding:4px 10px;
      border:1px solid rgba(255,255,255,.15);
      text-transform:uppercase;
    }
    .bottom-nav{
      position:fixed;bottom:14px;left:50%;transform:translateX(-50%);
      width:calc(100% - 24px);max-width:480px;height:68px;
      background:rgba(255,255,255,.97);
      backdrop-filter:blur(20px);-webkit-backdrop-filter:blur(20px);
      border-radius:22px;
      border:1px solid rgba(180,180,180,.35);
      box-shadow:0 8px 32px rgba(0,0,0,.18),0 1px 0 rgba(255,255,255,.9) inset,0 -1px 0 rgba(0,0,0,.06) inset;
      display:flex;align-items:center;justify-content:space-around;
      padding:0 8px;z-index:200;
    }
    .nav-btn{display:flex;flex-direction:column;align-items:center;gap:3px;flex:1;
      padding:8px 4px;border:none;background:transparent;cursor:pointer;
      font-family:var(--font);color:var(--text-lt);transition:color .15s;}
    .nav-icon{font-size:22px;line-height:1;}
    .nav-label{font-size:10px;font-weight:600;}
    .nav-btn.active{color:#1a1a1a;}
    .nav-btn-plus{
      display:flex;align-items:center;justify-content:center;
      width:52px;height:52px;border-radius:50%;
      background:linear-gradient(145deg,#2a2a2a,#111);
      border:1.5px solid rgba(200,200,200,.22);cursor:pointer;
      box-shadow:0 4px 18px rgba(0,0,0,.38),inset 0 1px 0 rgba(255,255,255,.1);
      flex-shrink:0;transition:transform .12s,box-shadow .15s;margin:0 4px;
    }
    .nav-btn-plus:active{transform:scale(.92);box-shadow:0 2px 8px rgba(0,0,0,.3);}
    .nav-btn-plus.active{background:linear-gradient(145deg,#c9a227,#b8963a);}
    .nav-btn-plus svg{width:22px;height:22px;}
    @media(min-width:641px){
      .bottom-nav{display:none;}
      .page-wrap{padding-bottom:40px;}
      .pc-tabs{display:flex;gap:4px;margin:14px 0;
        background:var(--surface);border-radius:var(--radius);
        padding:6px;box-shadow:var(--shadow);border:1px solid var(--border);}
      .pc-tab-btn{flex:1;padding:11px 8px;border:none;border-radius:10px;
        background:transparent;cursor:pointer;font-family:var(--font);
        font-size:13px;font-weight:600;color:var(--text-dim);
        transition:background .15s,color .15s;}
      .pc-tab-btn.active{
        background:linear-gradient(135deg,#1a1a1a,#2e2e2e);
        color:#fff;
        box-shadow:0 2px 8px rgba(0,0,0,.2);
      }
    }
    @media(max-width:640px){.pc-tabs{display:none;}}

    .section-heading{font-size:13px;font-weight:700;color:var(--text-dim);
      letter-spacing:.3px;margin:18px 0 10px;display:flex;align-items:center;gap:6px;}
    .dot{width:6px;height:6px;border-radius:50%;background:var(--gold);}
    .stat-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:9px;margin-bottom:4px;}
    .stat-card{background:var(--surface);border:1px solid var(--border);
      border-radius:var(--radius);padding:13px 7px 11px;text-align:center;
      box-shadow:var(--shadow);cursor:pointer;
      transition:transform .12s,box-shadow .12s;-webkit-tap-highlight-color:transparent;}
    .stat-card:active{transform:scale(.95);box-shadow:none;}
    .stat-num{font-size:24px;font-weight:800;line-height:1.1;}
    .stat-label{font-size:10px;color:var(--text-dim);margin-top:5px;font-weight:600;}
    .care-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:18px;}
    .care-card{background:var(--surface);border:1px solid var(--border);
      border-radius:12px;padding:14px 10px 12px;text-align:center;
      box-shadow:0 1px 8px rgba(0,0,0,.06);
      cursor:pointer;transition:transform .12s,box-shadow .12s;
      -webkit-tap-highlight-color:transparent;}
    .care-card:hover{transform:translateY(-2px);box-shadow:0 4px 16px rgba(0,0,0,.12);}
    .care-card:active{transform:scale(.95);box-shadow:none;}
    .care-card.bday{border-top:3px solid var(--purple);}
    .care-card.warn{border-top:3px solid var(--orange);}
    .care-card.info{border-top:3px solid var(--green);}
    .care-card.alert{border-top:3px solid var(--danger);}
    .care-num{font-size:24px;font-weight:800;line-height:1.1;}
    .care-label{font-size:10px;color:var(--text-dim);margin-top:5px;font-weight:600;}
    .sales-grid{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:4px;}
    .sales-card{position:relative;overflow:hidden;border-radius:var(--radius);
      padding:15px 10px 13px;text-align:center;
      box-shadow:0 2px 12px rgba(0,0,0,.09);border:1px solid transparent;}
    .sales-card.s-mc{background:linear-gradient(145deg,#e8f0fc,#d8e8f8);border-color:#b8d0ef;}
    .sales-card.s-md{background:linear-gradient(145deg,#e4f5ec,#d0eedd);border-color:#9ed4b5;}
    .sales-card.s-yc{background:linear-gradient(145deg,#f0e8ff,#e4d4ff);border-color:#c8a8f0;}
    .sales-card.s-yd{background:linear-gradient(145deg,#fdf5e0,#f8e8b8);border-color:#e0c060;}
    .sales-num{font-size:28px;font-weight:800;line-height:1.1;letter-spacing:-.5px;}
    .sales-unit{font-size:13px;font-weight:700;margin-left:1px;}
    .sales-label{font-size:10px;font-weight:700;margin-top:6px;letter-spacing:.1px;}
    .sales-month-lbl{font-size:9px;font-weight:600;margin-top:2px;opacity:.65;}
    .contact-empty{background:var(--surface);border:1px solid var(--border);
      border-radius:var(--radius);padding:18px;text-align:center;color:var(--text-lt);font-size:13px;}
    .contact-item{background:var(--surface);border:1px solid var(--border);
      border-radius:var(--radius);padding:13px 15px;margin-bottom:8px;
      display:flex;align-items:center;justify-content:space-between;
      box-shadow:0 1px 6px rgba(0,0,0,.05);}
    .c-name{font-size:15px;font-weight:700;color:var(--black);}
    .c-meta{font-size:12px;color:var(--text-dim);margin-top:2px;}
    .call-btn{white-space:nowrap;word-break:keep-all;flex-shrink:0;
      min-width:76px;height:44px;padding:0 16px;border-radius:24px;
      border:none;background:var(--black);color:#fff;text-decoration:none;
      display:inline-flex;align-items:center;justify-content:center;
      gap:5px;font-size:13px;font-weight:800;line-height:1;}
    .clickable-name{cursor:pointer;display:inline-block;transition:color .15s,transform .1s;}
    .clickable-name:hover{color:var(--gold-dk);}
    .customer-detail-overlay{display:none;position:fixed;inset:0;z-index:600;
      background:rgba(0,0,0,.55);backdrop-filter:blur(2px);
      overflow-y:auto;-webkit-overflow-scrolling:touch;}
    .customer-detail-overlay.open{display:flex;align-items:flex-start;justify-content:center;padding:18px 10px 40px;}
    .customer-detail-modal{width:100%;max-width:640px;background:var(--bg);
      border-radius:20px;overflow:hidden;box-shadow:0 12px 48px rgba(0,0,0,.28);animation:vIn .2s ease;}
    .an-year-filter{display:flex;gap:7px;flex-wrap:wrap;margin-bottom:14px;padding:2px 0;}
    .an-yr-btn{padding:7px 16px;border-radius:20px;border:1.5px solid var(--border2);
      background:#fff;color:var(--text-dim);font-family:var(--font);font-size:12px;font-weight:700;
      cursor:pointer;transition:background .13s,color .13s,border-color .13s;}
    .an-yr-btn.active{background:var(--black);border-color:var(--black);color:#fff;}
    .an-yr-badge{font-size:11px;font-weight:600;color:var(--text-lt);margin-left:4px;}
    .analytics-summary{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:18px;}
    .an-card{background:var(--surface);border:1px solid var(--border);
      border-radius:var(--radius);padding:16px 10px 13px;text-align:center;box-shadow:var(--shadow);}
    .an-num{font-size:26px;font-weight:800;line-height:1.1;color:var(--gold);}
    .an-label{font-size:10px;color:var(--text-dim);margin-top:5px;font-weight:600;}
    .chart-card{background:var(--surface);border:1px solid var(--border);
      border-radius:var(--radius);padding:16px;box-shadow:var(--shadow);margin-bottom:16px;overflow:hidden;}
    .chart-title{font-size:13px;font-weight:800;color:var(--black);margin-bottom:14px;display:flex;align-items:center;gap:6px;}
    .chart-wrap{position:relative;width:100%;}
    .view-header{padding:16px 0 6px;}
    .view-title{font-size:20px;font-weight:800;color:var(--black);}
    .view-sub{font-size:12px;color:var(--text-dim);margin-top:2px;}
    .form-card{background:var(--surface);border:1px solid var(--border);
      border-radius:var(--radius);overflow:hidden;box-shadow:var(--shadow);margin-bottom:14px;}
    .form-card-header{padding:13px 17px;background:var(--surface2);
      border-bottom:1px solid var(--border);font-size:13px;font-weight:700;color:var(--black);}
    .form-card-header.blue{background:#edf5fc;border-bottom-color:#c5dff0;color:var(--blue);}
    .form-card-header.care{background:#f5f0ff;border-bottom-color:#d5b8f5;color:var(--purple);}
    .form-body{padding:16px;}
    .grid-2{display:grid;grid-template-columns:1fr 1fr;gap:12px;}
    .field{display:flex;flex-direction:column;gap:5px;}
    .field.full{grid-column:1/-1;}
    label{font-size:12px;font-weight:600;color:var(--text-dim);}
    label .req{color:var(--gold);margin-left:2px;}
    input[type="text"],input[type="tel"],input[type="date"],select,textarea{
      width:100%;background:#fff;border:1px solid var(--border2);
      border-radius:9px;color:var(--text);font-family:var(--font);
      font-size:15px;padding:12px 14px;outline:none;
      transition:border-color .18s,box-shadow .18s;-webkit-appearance:none;appearance:none;}
    input:focus,select:focus,textarea:focus{border-color:var(--gold);box-shadow:0 0 0 3px rgba(184,150,58,.12);}
    select{cursor:pointer;
      background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath fill='%23888899' d='M6 8L0 0h12z'/%3E%3C/svg%3E");
      background-repeat:no-repeat;background-position:right 13px center;padding-right:36px;}
    select:disabled{background-color:#f5f5f8;color:var(--text-lt);cursor:not-allowed;}
    textarea{resize:vertical;min-height:90px;}
    input[type="date"]::-webkit-calendar-picker-indicator{cursor:pointer;opacity:.6;}
    .care-input{background:#f8f5ff!important;border-color:#c8a8f0!important;}
    .care-input:focus{border-color:var(--purple)!important;box-shadow:0 0 0 3px rgba(122,61,184,.1)!important;}
    .auto-field{background:var(--surface2)!important;border:1px dashed var(--border2)!important;
      color:var(--text-dim)!important;cursor:default;}
    .btn-submit{width:100%;margin-top:18px;padding:16px;border:none;
      border-radius:var(--radius);background:var(--black);color:#fff;
      font-family:var(--font);font-size:16px;font-weight:700;cursor:pointer;
      transition:background .18s,transform .1s;position:relative;overflow:hidden;min-height:54px;}
    .btn-submit:hover{background:#2a2a2a;}
    .btn-submit:active{transform:scale(.99);}
    .btn-submit:disabled{opacity:.5;cursor:not-allowed;transform:none;}
    .btn-submit.blue-btn{background:var(--blue);}
    .btn-submit.blue-btn:hover{background:#155a8a;}
    .st-msg{display:none;margin-top:13px;padding:14px 16px;border-radius:var(--radius);
      font-size:14px;font-weight:600;text-align:center;animation:vIn .2s ease;}
    .st-msg.ok{display:block;background:#edfaf3;border:1px solid #a8dfc0;color:var(--success);}
    .st-msg.err{display:block;background:#fdf0ef;border:1px solid #f0b8b2;color:var(--danger);}
    .badge{display:inline-block;font-size:11px;font-weight:700;padding:3px 9px;border-radius:20px;white-space:nowrap;}
    .badge.s0{background:#ebebf0;color:#444;}
    .badge.s1{background:#e8e8ea;color:#3a3a45;}
    .badge.s2{background:#fff0e0;color:#c05800;border:1px solid #ffd8a8;}
    .badge.s3{background:#e3f0ff;color:#1055a8;border:1px solid #b0cef5;}
    .badge.s4{background:#f0e5ff;color:#6020a0;border:1px solid #d5b8f5;}
    .badge.s5{background:#fffbe0;color:#8a6b00;border:1px solid #ffe97a;}
    .badge.s6{background:#e3f7ec;color:#0f6e35;border:1px solid #90ddb0;}
    .badge.s7{background:#d4f0e0;color:#0a5528;border:1px solid #5fc890;}
    .badge.s8{background:#ebebeb;color:#404040;border:1px solid #c8c8c8;}
    .badge.s9{background:#fde8e8;color:#a01515;border:1px solid #f0a0a0;}
    .preview-box{display:none;margin-top:10px;background:#edf5fc;border:1px solid #b5d5f0;
      border-radius:var(--radius);padding:13px 15px;}
    .preview-box.show{display:block;animation:vIn .2s ease;}
    .preview-name{font-size:15px;font-weight:800;color:var(--black);}
    .preview-meta{font-size:12px;color:var(--text-dim);margin-top:3px;}
    .nofound{display:none;margin-top:10px;background:#fff3f3;border:1px solid #f0b8b8;
      border-radius:var(--radius);padding:12px 15px;color:var(--danger);font-size:13px;font-weight:600;text-align:center;}
    .nofound.show{display:block;}
    .list-topbar{display:flex;align-items:center;justify-content:space-between;padding:14px 0 4px;}
    .list-count-badge{font-size:12px;font-weight:700;background:var(--gold);color:#fff;border-radius:20px;padding:2px 10px;}
    .search-bar{display:flex;gap:8px;margin:8px 0;}
    .search-bar input{flex:1;background:var(--surface);border:1px solid var(--border);
      border-radius:24px;padding:11px 18px;font-size:14px;box-shadow:0 1px 6px rgba(0,0,0,.05);}
    .filter-bar{display:none;align-items:center;gap:8px;background:var(--surface);border:1px solid var(--border);
      border-radius:var(--radius);padding:11px 15px;margin:8px 0;box-shadow:0 1px 6px rgba(0,0,0,.05);}
    .filter-label{font-size:13px;font-weight:700;color:var(--blue);flex:1;}
    .filter-clear{padding:5px 12px;border-radius:20px;border:1px solid var(--border2);
      background:var(--surface);font-size:12px;font-weight:600;color:var(--text-dim);cursor:pointer;font-family:var(--font);}
    .cust-card{background:var(--surface);border:1px solid var(--border);
      border-radius:var(--radius);padding:16px;margin-bottom:12px;box-shadow:0 1px 8px rgba(0,0,0,.06);}
    .cust-card.updated{border-left:3px solid var(--gold);}
    .card-top{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:10px;}
    .cust-name{font-size:17px;font-weight:800;color:var(--black);}
    .cust-sub{font-size:12px;color:var(--text-dim);margin-top:2px;}
    .chip-row{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:8px;}
    .chip{font-size:11px;font-weight:600;color:var(--text-dim);background:var(--bg);border-radius:6px;padding:3px 8px;}
    .tel-link{color:var(--gold-dk);text-decoration:none;font-weight:700;}
    .tel-link:hover{text-decoration:underline;}
    .tog-btn{display:flex;align-items:center;gap:5px;width:100%;
      background:#f5f5f8;border:1px solid var(--border);border-radius:8px;
      padding:8px 13px;cursor:pointer;font-family:var(--font);
      font-size:12px;font-weight:600;color:var(--text-dim);margin-top:10px;transition:background .15s;}
    .tog-btn:hover{background:#ebebf0;}
    .tog-arr{transition:transform .2s;font-size:10px;margin-left:auto;}
    .tog-btn.open .tog-arr{transform:rotate(180deg);}
    .detail-body{display:none;margin-top:10px;}
    .detail-body.open{display:block;}
    .care-tog{display:flex;align-items:center;gap:5px;width:100%;
      background:#f5f0ff;border:1px solid #d5b8f5;border-radius:8px;
      padding:8px 13px;cursor:pointer;font-family:var(--font);
      font-size:12px;font-weight:700;color:var(--purple);margin-top:10px;transition:background .15s;}
    .care-tog:hover{background:#ece5ff;}
    .care-detail{display:none;margin-top:8px;}
    .care-detail.open{display:block;}
    .d-sec{font-size:10px;font-weight:700;letter-spacing:1px;color:var(--gold);
      text-transform:uppercase;margin:11px 0 6px;padding-bottom:4px;border-bottom:1px solid var(--border);}
    .d-sec.care{color:var(--purple);border-bottom-color:#d5b8f5;}
    .d-row{display:flex;gap:6px;margin-bottom:5px;font-size:13px;align-items:flex-start;}
    .d-key{flex-shrink:0;width:82px;color:var(--text-dim);font-weight:600;font-size:12px;}
    .d-val{color:var(--text);word-break:break-all;line-height:1.5;white-space:pre-wrap;}
    .dd{display:inline-block;font-size:10px;font-weight:700;padding:1px 6px;border-radius:6px;margin-left:4px;vertical-align:middle;}
    .dd.urg{background:#fde8e8;color:#a01515;}
    .dd.warn{background:#fff0d0;color:#b06000;}
    .dd.ok{background:#e3f7ec;color:#0f6e35;}
    .empty-state{text-align:center;padding:44px 20px;background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);}
    .empty-icon{font-size:38px;margin-bottom:11px;}
    .empty-state p{font-size:14px;color:var(--text-lt);}
    .year-filter-bar{display:none;overflow-x:auto;-webkit-overflow-scrolling:touch;
      padding:8px 0 4px;margin-bottom:6px;gap:6px;white-space:nowrap;scrollbar-width:none;}
    .year-filter-bar::-webkit-scrollbar{display:none;}
    .year-filter-bar.visible{display:block;}
    .yr-btn{display:inline-block;padding:6px 13px;border-radius:20px;border:1.5px solid var(--border2);
      background:#fff;color:var(--text-dim);font-family:var(--font);font-size:12px;font-weight:700;
      cursor:pointer;margin-right:5px;white-space:nowrap;}
    .yr-btn.active{background:var(--blue);border-color:var(--blue);color:#fff;}
    .detail-custom-wrap{display:none;margin-top:6px;}
    .detail-custom-wrap.visible{display:block;}
    .detail-custom-input{width:100%;padding:11px 14px;border:1.5px solid var(--blue);
      border-radius:var(--radius);font-family:var(--font);font-size:14px;
      color:var(--black);background:#f0f6ff;box-sizing:border-box;}
    .detail-custom-input:focus{outline:none;border-color:var(--gold);background:#fffdf0;}
    .detail-custom-hint{font-size:11px;color:var(--blue);margin-top:4px;font-weight:600;}
    .edit-btn{display:inline-flex;align-items:center;gap:4px;padding:6px 13px;border-radius:20px;
      border:1.5px solid var(--gold);background:#fff;color:var(--gold-dk);
      font-family:var(--font);font-size:12px;font-weight:700;cursor:pointer;transition:background .15s,color .15s;}
    .edit-btn:hover{background:var(--gold);color:#fff;}
    .edit-overlay{display:none;position:fixed;inset:0;z-index:500;
      background:rgba(0,0,0,.55);backdrop-filter:blur(2px);overflow-y:auto;-webkit-overflow-scrolling:touch;}
    .edit-overlay.open{display:flex;align-items:flex-start;justify-content:center;padding:16px 0 40px;}
    .edit-modal{background:var(--bg);width:100%;max-width:720px;border-radius:20px;
      overflow:hidden;box-shadow:0 12px 48px rgba(0,0,0,.28);margin:0 10px;animation:vIn .2s ease;}
    .edit-modal-header{position:sticky;top:0;z-index:10;background:var(--black);padding:14px 18px;
      display:flex;align-items:center;justify-content:space-between;}
    .edit-modal-title{font-size:16px;font-weight:800;color:#fff;letter-spacing:-.3px;}
    .edit-modal-close{width:34px;height:34px;border-radius:50%;border:none;
      background:rgba(255,255,255,.15);color:#fff;font-size:18px;
      cursor:pointer;display:flex;align-items:center;justify-content:center;}
    .edit-modal-body{padding:16px 14px 8px;}
    .edit-modal-footer{padding:12px 14px 16px;display:flex;gap:10px;}
    .btn-edit-save{flex:1;padding:15px;border:none;border-radius:var(--radius);
      background:var(--gold);color:#fff;font-family:var(--font);font-size:15px;font-weight:700;cursor:pointer;}
    .btn-edit-save:hover{background:var(--gold-dk);}
    .btn-edit-save:disabled{opacity:.5;cursor:not-allowed;}
    .btn-edit-cancel{padding:15px 20px;border:1.5px solid var(--border2);border-radius:var(--radius);
      background:#fff;color:var(--text-dim);font-family:var(--font);font-size:15px;font-weight:700;cursor:pointer;}
    .st-msg.edit-save-msg{margin:0 14px 12px;}

    /* 데이터 백업/복원 */
    .backup-bar{display:flex;gap:8px;flex-wrap:wrap;margin:8px 0 14px;}
    .backup-btn{display:inline-flex;align-items:center;gap:5px;padding:8px 14px;
      border-radius:20px;border:1.5px solid var(--border2);background:#fff;
      color:var(--text-dim);font-family:var(--font);font-size:12px;font-weight:700;
      cursor:pointer;transition:background .13s,color .13s;}
    .backup-btn:hover{background:var(--black);color:#fff;border-color:var(--black);}
    .backup-btn.danger:hover{background:var(--danger);border-color:var(--danger);color:#fff;}
    .total-stat{background:var(--surface);border:1px solid var(--border);
      border-radius:var(--radius);padding:13px 16px;margin-bottom:12px;
      display:flex;align-items:center;justify-content:space-between;
      box-shadow:0 1px 6px rgba(0,0,0,.05);}
    .total-stat-label{font-size:12px;color:var(--text-dim);font-weight:600;}
    .total-stat-num{font-size:18px;font-weight:800;color:var(--gold);}

    @media(max-width:640px){
      .page-wrap{padding:0 12px 110px;}
      .grid-2{grid-template-columns:1fr;}
      .field.full{grid-column:1;}
      .form-body{padding:14px;}
      .stat-grid{gap:7px;} .stat-num{font-size:20px;}
      .stat-card{padding:11px 5px 9px;} .stat-label{font-size:9px;}
      .care-grid{grid-template-columns:repeat(2,1fr);gap:8px;}
      .care-num{font-size:22px;} .care-card{padding:13px 8px 11px;}
      .sales-grid{grid-template-columns:repeat(2,1fr);gap:8px;}
      .sales-num{font-size:24px;}
      input[type="date"]{font-size:16px;padding:13px 14px;}
      .contact-item{gap:10px;}
      .contact-item>div:first-child{min-width:0;flex:1;}
    }
    @media(max-width:380px){
      .stat-num{font-size:17px;} .stat-label{font-size:8px;}
    }

    /* ── 캘린더 ── */
    .cal-header{display:flex;align-items:center;justify-content:space-between;margin:16px 0 14px;}
    .cal-title{font-size:20px;font-weight:800;color:var(--black);}
    .cal-nav-btn{width:38px;height:38px;border-radius:50%;border:1.5px solid var(--border2);background:#fff;cursor:pointer;font-size:20px;display:flex;align-items:center;justify-content:center;transition:background .15s;}
    .cal-nav-btn:hover{background:var(--black);color:#fff;border-color:var(--black);}
    .cal-legend{display:flex;flex-wrap:wrap;gap:8px;margin-bottom:12px;}
    .cal-leg-item{display:flex;align-items:center;gap:4px;font-size:11px;font-weight:600;color:var(--text-dim);}
    .cal-leg-dot{width:9px;height:9px;border-radius:50%;flex-shrink:0;}
    .cal-grid{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);overflow:hidden;box-shadow:var(--shadow);}
    .cal-weekrow{display:grid;grid-template-columns:repeat(7,1fr);background:var(--black);}
    .cal-weekday{text-align:center;padding:10px 2px;font-size:11px;font-weight:700;color:#aaa;}
    .cal-weekday.sun{color:#ff8888;}.cal-weekday.sat{color:#88aaff;}
    .cal-body{display:grid;grid-template-columns:repeat(7,1fr);}
    .cal-cell{min-height:90px;border-right:1px solid var(--border);border-bottom:1px solid var(--border);padding:5px 4px 4px;transition:background .1s;vertical-align:top;}
    .cal-cell:nth-child(7n){border-right:none;}
    .cal-cell:hover{background:#f9f9fb;}
    .cal-cell.other-month{background:#f8f8fa;}
    .cal-cell.today{background:linear-gradient(135deg,#fffbe6,#fff8d6);}
    .cal-date{font-size:12px;font-weight:700;color:var(--text-dim);margin-bottom:3px;line-height:1;}
    .cal-cell.today .cal-date{color:var(--gold);font-size:13px;font-weight:900;}
    .cal-cell.sun .cal-date{color:#e05050;}
    .cal-cell.sat .cal-date{color:#4466cc;}
    .cal-cell.other-month .cal-date{color:#ccc;}
    .cal-dots{display:flex;flex-direction:column;gap:2px;margin-top:2px;}
    .cal-event{font-size:10px;font-weight:600;line-height:1.3;padding:2px 4px;border-radius:4px;cursor:pointer;overflow:hidden;white-space:nowrap;text-overflow:ellipsis;max-width:100%;}
    .cal-event:hover{opacity:.8;}
    .cal-event.type-contact{background:#e3f0ff;color:#1055a8;}
    .cal-event.type-insurance{background:#fff0e0;color:#c05800;}
    .cal-event.type-inspection{background:#fff0e0;color:#a03800;}
    .cal-event.type-finance{background:#fde8e8;color:#a01515;}
    .cal-event.type-service{background:#f0e5ff;color:#6020a0;}
    .cal-event.type-birthday{background:#f5e8ff;color:#7a3db8;}
    .cal-event.type-delivery1{background:#e3f7ec;color:#0f6e35;}
    .cal-event.type-delivery3{background:#fde8e8;color:#a01515;}
    .cal-more{font-size:9px;color:var(--text-lt);font-weight:600;margin-top:2px;cursor:pointer;padding:1px 4px;}
    .cal-more:hover{color:var(--blue);}
    .cal-popup-overlay{display:none;position:fixed;inset:0;z-index:700;background:rgba(0,0,0,.45);backdrop-filter:blur(2px);}
    .cal-popup-overlay.open{display:flex;align-items:center;justify-content:center;padding:20px;}
    .cal-popup{background:#fff;border-radius:20px;padding:22px 20px 18px;max-width:380px;width:100%;box-shadow:0 12px 48px rgba(0,0,0,.28);animation:vIn .18s ease;max-height:80vh;overflow-y:auto;}
    .cal-popup-date{font-size:16px;font-weight:800;color:var(--black);margin-bottom:14px;padding-bottom:10px;border-bottom:1px solid var(--border);}
    .cal-popup-item{display:flex;align-items:flex-start;gap:10px;padding:9px 0;border-bottom:1px solid var(--border);}
    .cal-popup-item:last-child{border-bottom:none;}
    .cal-popup-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0;margin-top:5px;}
    .cal-popup-name{font-size:14px;font-weight:700;color:var(--black);}
    .cal-popup-desc{font-size:12px;color:var(--text-dim);margin-top:3px;}
    .cal-popup-close{width:100%;margin-top:14px;padding:12px;border:none;border-radius:var(--radius);background:var(--black);color:#fff;font-family:var(--font);font-size:14px;font-weight:700;cursor:pointer;}
    @media(max-width:640px){.cal-cell{min-height:68px;}.cal-event{font-size:9px;}}
    @media(max-width:380px){.cal-cell{min-height:56px;}}

    /* ── 스케줄 전용 ── */
    .stat-bar{display:grid;grid-template-columns:repeat(4,1fr);gap:12px;margin-bottom:20px;}
    .stat-card{background:var(--surface);border:1px solid var(--border);border-radius:var(--radius);padding:14px 16px;box-shadow:0 2px 8px rgba(0,0,0,.05);display:flex;align-items:center;gap:12px;}
    .stat-icon{font-size:22px;}
    .stat-num{font-size:22px;font-weight:800;color:var(--black);line-height:1;}
    .stat-label{font-size:11px;color:var(--text-dim);font-weight:600;margin-top:3px;}
    .add-bar{background:var(--surface);border:1.5px solid var(--border);border-radius:var(--radius);padding:13px 16px;margin-bottom:16px;display:flex;align-items:center;gap:10px;box-shadow:0 2px 10px rgba(0,0,0,.05);}
    .add-bar-icon{font-size:17px;flex-shrink:0;}
    .add-input{flex:1;border:none;outline:none;font-family:var(--font);font-size:14px;color:var(--text);background:transparent;}
    .add-input::placeholder{color:var(--text-lt);}
    .add-date-input{border:1px solid var(--border2);border-radius:8px;padding:7px 10px;font-family:var(--font);font-size:13px;color:var(--text);background:#f8f8fa;outline:none;}
    .add-date-input:focus{border-color:var(--gold);background:#fff;}
    .add-color-select{border:1px solid var(--border2);border-radius:8px;padding:7px 10px;font-family:var(--font);font-size:12px;color:var(--text);background:#f8f8fa;outline:none;cursor:pointer;}
    .add-btn{padding:8px 18px;border-radius:20px;border:none;background:linear-gradient(135deg,#1a1a22,#2e2e3a);color:#fff;font-family:var(--font);font-size:13px;font-weight:700;cursor:pointer;white-space:nowrap;box-shadow:0 2px 8px rgba(0,0,0,.18);transition:opacity .15s;}
    .add-btn:hover{opacity:.85;}
    .legend{display:flex;flex-wrap:wrap;gap:8px;margin-bottom:14px;padding:11px 14px;background:var(--surface);border:1px solid var(--border);border-radius:10px;box-shadow:0 1px 4px rgba(0,0,0,.05);}
    .legend-item{display:flex;align-items:center;gap:4px;font-size:11px;font-weight:600;color:var(--text-dim);}
    .legend-dot{width:9px;height:9px;border-radius:50%;flex-shrink:0;}
    /* 스케줄 이벤트 색상 (CRM cal-event 위에 덮어씀) */
    .ev-contact{background:#ddeeff;color:#1a4a8a;border-left:3px solid #3a7acc;}
    .ev-insurance{background:#fff0dd;color:#8a4a00;border-left:3px solid #cc7a22;}
    .ev-inspection{background:#ffeedd;color:#7a3800;border-left:3px solid #bb6611;}
    .ev-finance{background:#ffe0e0;color:#8a1818;border-left:3px solid #cc3333;}
    .ev-service{background:#ede0ff;color:#520090;border-left:3px solid #8833cc;}
    .ev-birthday{background:#f5e8ff;color:#620080;border-left:3px solid #9933cc;}
    .ev-delivery1{background:#ddf5e8;color:#0a5528;border-left:3px solid #22aa55;}
    .ev-delivery3{background:#fde8e8;color:#8a0000;border-left:3px solid #cc2222;}
    .ev-custom-0{background:#e8f0ff;color:#1a3a8a;border-left:3px solid #4466cc;}
    .ev-custom-1{background:#fff0e8;color:#8a3a00;border-left:3px solid #cc6622;}
    .ev-custom-2{background:#e8ffe8;color:#0a5a0a;border-left:3px solid #22aa22;}
    .ev-custom-3{background:#fff8e0;color:#7a6000;border-left:3px solid #ccaa22;}
    .ev-custom-4{background:#f0e8ff;color:#4a0a8a;border-left:3px solid #7733cc;}
    .ev-custom-5{background:#e8ffff;color:#0a5a5a;border-left:3px solid #22aaaa;}
    .ev-done{opacity:.42;text-decoration:line-through;}
    .cal-event .del-btn{display:none;position:absolute;right:3px;top:50%;transform:translateY(-50%);font-size:9px;color:rgba(0,0,0,.45);cursor:pointer;font-weight:900;background:rgba(255,255,255,.8);border-radius:50%;width:13px;height:13px;align-items:center;justify-content:center;line-height:1;}
    .cal-event{position:relative;}
    .cal-event:hover .del-btn{display:flex;}
    /* 팝업 */
    .sched-popup-overlay{display:none;position:fixed;inset:0;z-index:700;background:rgba(0,0,0,.38);backdrop-filter:blur(4px);}
    .sched-popup-overlay.open{display:flex;align-items:center;justify-content:center;padding:20px;}
    .sched-popup{background:#fff;border-radius:20px;padding:24px 22px 20px;max-width:340px;width:100%;box-shadow:0 16px 56px rgba(0,0,0,.22);animation:vIn .18s ease;max-height:80vh;overflow-y:auto;}
    .sched-popup-date{font-size:16px;font-weight:800;color:var(--black);margin-bottom:14px;padding-bottom:12px;border-bottom:1px solid var(--border);}
    .sched-popup-item{display:flex;align-items:flex-start;gap:10px;padding:9px 0;border-bottom:1px solid var(--border);}
    .sched-popup-item:last-child{border-bottom:none;}
    .sched-popup-dot{width:8px;height:8px;border-radius:50%;flex-shrink:0;margin-top:5px;}
    .sched-popup-text{font-size:13px;font-weight:700;color:var(--black);flex:1;}
    .sched-popup-text.done{opacity:.42;text-decoration:line-through;}
    .sched-popup-phone{font-size:11px;color:var(--gold);font-weight:700;text-decoration:none;margin-top:2px;display:block;}
    .sched-check{width:22px;height:22px;border-radius:50%;border:2px solid var(--border2);background:transparent;cursor:pointer;flex-shrink:0;display:flex;align-items:center;justify-content:center;font-size:11px;transition:all .15s;}
    .sched-check.checked{background:var(--green);border-color:var(--green);color:#fff;}
    .sched-popup-close{width:100%;margin-top:14px;padding:12px;border:none;border-radius:var(--radius);background:var(--black);color:#fff;font-family:var(--font);font-size:14px;font-weight:700;cursor:pointer;}
    /* 스케줄 뷰 캘린더 셀 */
    .scheduleView .cal-cell{min-height:120px;}
    .scheduleView .cal-month-title{font-size:22px;font-weight:800;color:var(--black);letter-spacing:-.5px;}
    .scheduleView .cal-month-sub{font-size:12px;color:var(--text-dim);}
    .scheduleView .cal-nav{display:flex;align-items:center;justify-content:space-between;margin-bottom:18px;}
    .scheduleView .cal-nav-center{display:flex;align-items:center;gap:14px;}
    .today-btn{padding:0 18px;height:38px;border-radius:20px;border:1.5px solid var(--border2);background:var(--surface);cursor:pointer;font-family:var(--font);font-size:12px;font-weight:700;color:var(--text-dim);transition:all .15s;}
    .today-btn:hover{background:var(--black);color:#fff;border-color:var(--black);}
    @media(max-width:900px){.stat-bar{grid-template-columns:repeat(2,1fr);}}
    @media(max-width:640px){.add-bar{flex-wrap:wrap;gap:8px;}.stat-bar{gap:8px;}.scheduleView .cal-cell{min-height:80px;}}
  </style>
</head>
<body>

<div class="app-header">
  <div class="app-header-left">
    <div class="app-logo">
      <img src="data:image/png;base64,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" alt="Mercedes-Benz" style="width:38px;height:38px;object-fit:cover;border-radius:50%;display:block;"/>
    </div>
    <div>
      <div class="app-title">구용회 고객관리</div>
      <div class="app-sub">Mercedes-Benz 의정부</div>
    </div>
  </div>
  <div class="view-badge" id="viewBadge">대시보드</div>
</div>

<div class="page-wrap">
  <div class="pc-tabs">
    <button class="pc-tab-btn active" onclick="showView('home')">🏠 대시보드</button>
    <button class="pc-tab-btn" onclick="showView('schedule')">🗓 스케줄</button>
    <button class="pc-tab-btn" onclick="showView('calendar')">📅 캘린더</button>
    <button class="pc-tab-btn" onclick="showView('new')">✏️ 신규등록</button>
    <button class="pc-tab-btn" onclick="showView('consult')">📋 추가상담</button>
    <button class="pc-tab-btn" onclick="showView('list')">👥 고객목록</button>
    <button class="pc-tab-btn" onclick="showView('analytics')">📊 분석</button>
  </div>

  <!-- 대시보드 -->
  <div class="view active" id="homeView">

    <div class="section-heading"><span class="dot"></span> 고객 현황</div>
    <div class="stat-grid">
      <div class="stat-card" onclick="goFilter('')"><div class="stat-num" id="d-total" style="color:var(--gold)">0</div><div class="stat-label">전체</div></div>
      <div class="stat-card" onclick="goFilter('신규문의')"><div class="stat-num" id="d-new" style="color:#3a3a45">0</div><div class="stat-label">신규문의</div></div>
      <div class="stat-card" onclick="goFilter('A급가망')"><div class="stat-num" id="d-a" style="color:#c05800">0</div><div class="stat-label">A급가망</div></div>
      <div class="stat-card" onclick="goFilter('상담중')"><div class="stat-num" id="d-c" style="color:#1055a8">0</div><div class="stat-label">상담중</div></div>
      <div class="stat-card" onclick="goFilter('계약유력')"><div class="stat-num" id="d-l" style="color:#6020a0">0</div><div class="stat-label">계약유력</div></div>
      <div class="stat-card" onclick="goFilter('계약')"><div class="stat-num" id="d-k" style="color:#0f6e35">0</div><div class="stat-label">계약</div></div>
      <div class="stat-card" onclick="goFilter('출고')"><div class="stat-num" id="d-d" style="color:#0a5528">0</div><div class="stat-label">출고</div></div>
      <div class="stat-card" onclick="goFilter('해약')"><div class="stat-num" id="d-x" style="color:#a01515">0</div><div class="stat-label">해약</div></div>
    </div>
    <div class="section-heading"><span class="dot"></span> 📈 판매실적</div>
    <div class="sales-grid">
      <div class="sales-card s-mc"><div style="color:#1a5aa0;">
        <div class="sales-month-lbl" id="sp-mc-lbl">— 월</div>
        <div><span class="sales-num" id="sp-mc">0</span><span class="sales-unit" style="color:#1a5aa0;">대</span></div>
        <div class="sales-label">🤝 계약</div>
      </div></div>
      <div class="sales-card s-md"><div style="color:#0f6e35;">
        <div class="sales-month-lbl" id="sp-md-lbl">— 월</div>
        <div><span class="sales-num" id="sp-md">0</span><span class="sales-unit" style="color:#0f6e35;">대</span></div>
        <div class="sales-label">🚗 출고</div>
      </div></div>
      <div class="sales-card s-yc"><div style="color:#5a10a0;">
        <div class="sales-month-lbl" id="sp-yc-lbl">— 년</div>
        <div><span class="sales-num" id="sp-yc">0</span><span class="sales-unit" style="color:#5a10a0;">대</span></div>
        <div class="sales-label">🤝 계약 누계</div>
      </div></div>
      <div class="sales-card s-yd"><div style="color:#8a6000;">
        <div class="sales-month-lbl" id="sp-yd-lbl">— 년</div>
        <div><span class="sales-num" id="sp-yd">0</span><span class="sales-unit" style="color:#8a6000;">대</span></div>
        <div class="sales-label">🚗 출고 누계</div>
      </div></div>
    </div>
    <div class="section-heading"><span class="dot"></span> 차량관리 알림</div>
    <div class="care-grid">
      <div class="care-card bday" onclick="goCareFilter('birthday7')"><div class="care-num" id="dc-bday" style="color:var(--purple)">0</div><div class="care-label">🎂 생일 D-7</div></div>
      <div class="care-card warn" onclick="goCareFilter('insurance30')"><div class="care-num" id="dc-ins" style="color:var(--orange)">0</div><div class="care-label">🛡 보험만기 30일</div></div>
      <div class="care-card warn" onclick="goCareFilter('maintenance')"><div class="care-num" id="dc-insp" style="color:var(--orange)">0</div><div class="care-label">🔧 검사/정기점검</div></div>
      <div class="care-card warn" onclick="goCareFilter('finance90')"><div class="care-num" id="dc-fin" style="color:var(--orange)">0</div><div class="care-label">💳 금융종료 90일</div></div>
      <div class="care-card info" onclick="goCareFilter('delivery1yr')"><div class="care-num" id="dc-1yr" style="color:var(--green)">0</div><div class="care-label">🚗 출고 1년↑</div></div>
      <div class="care-card alert" onclick="goCareFilter('delivery3yr')"><div class="care-num" id="dc-3yr" style="color:var(--danger)">0</div><div class="care-label">🚗 출고 3년↑</div></div>
    </div>
    <div class="section-heading"><span class="dot"></span> 오늘 연락할 고객</div>
    <div id="todayList"><div class="contact-empty">오늘 연락할 고객이 없습니다</div></div>
    <div class="section-heading"><span class="dot"></span> 이번주 연락할 고객</div>
    <div id="weekList"><div class="contact-empty">이번주 연락할 고객이 없습니다</div></div>
  </div>



  <!-- 스케줄 -->
  <div class="view scheduleView" id="scheduleView">
    <!-- 통계 바 -->
    <div class="stat-bar">
      <div class="stat-card"><div class="stat-icon">📋</div><div><div class="stat-num" id="stat-total">0</div><div class="stat-label">이번달 전체 일정</div></div></div>
      <div class="stat-card"><div class="stat-icon">✅</div><div><div class="stat-num" id="stat-done">0</div><div class="stat-label">완료</div></div></div>
      <div class="stat-card"><div class="stat-icon">📞</div><div><div class="stat-num" id="stat-contact">0</div><div class="stat-label">고객 연락</div></div></div>
      <div class="stat-card"><div class="stat-icon">⚠️</div><div><div class="stat-num" id="stat-today-left">0</div><div class="stat-label">오늘 남은 일정</div></div></div>
    </div>
    <!-- 월 네비 -->
    <div class="cal-nav">
      <div class="cal-nav-center">
        <button class="nav-btn" onclick="schedMove(-1)">&#8249;</button>
        <div>
          <div class="cal-month-title" id="schedMonthTitle"></div>
          <div class="cal-month-sub" id="schedMonthSub"></div>
        </div>
        <button class="nav-btn" onclick="schedMove(1)">&#8250;</button>
      </div>
      <button class="today-btn" onclick="schedGoToday()">오늘로</button>
    </div>
    <!-- 빠른 일정 추가 -->
    <div class="add-bar">
      <div class="add-bar-icon">✏️</div>
      <input class="add-input" id="schedAddText" placeholder="할 일 입력... (Enter)" onkeydown="if(event.key==='Enter')schedAddTask()"/>
      <input class="add-date-input" type="date" id="schedAddDate"/>
      <select class="add-color-select" id="schedAddColor">
        <option value="0">🔵 파랑</option>
        <option value="1">🟠 주황</option>
        <option value="2">🟢 초록</option>
        <option value="3">🟡 노랑</option>
        <option value="4">🟣 보라</option>
        <option value="5">🩵 하늘</option>
      </select>
      <button class="add-btn" onclick="schedAddTask()">+ 추가</button>
    </div>
    <!-- 범례 -->
    <div class="legend">
      <div class="legend-item"><div class="legend-dot" style="background:#3a7acc;"></div>다음연락일</div>
      <div class="legend-item"><div class="legend-dot" style="background:#cc7a22;"></div>보험만기</div>
      <div class="legend-item"><div class="legend-dot" style="background:#bb6611;"></div>자동차검사</div>
      <div class="legend-item"><div class="legend-dot" style="background:#cc3333;"></div>금융종료</div>
      <div class="legend-item"><div class="legend-dot" style="background:#8833cc;"></div>정기점검</div>
      <div class="legend-item"><div class="legend-dot" style="background:#9933cc;"></div>생일</div>
      <div class="legend-item"><div class="legend-dot" style="background:#22aa55;"></div>출고1년↑</div>
      <div class="legend-item"><div class="legend-dot" style="background:#cc2222;"></div>출고3년↑</div>
      <div class="legend-item"><div class="legend-dot" style="background:#4466cc;"></div>직접 입력</div>
    </div>
    <!-- 달력 -->
    <div class="cal-grid">
      <div class="cal-weekrow">
        <div class="cal-weekday sun">일</div><div class="cal-weekday">월</div><div class="cal-weekday">화</div>
        <div class="cal-weekday">수</div><div class="cal-weekday">목</div><div class="cal-weekday">금</div>
        <div class="cal-weekday sat">토</div>
      </div>
      <div class="cal-body" id="schedBody"></div>
    </div>
  </div>

  <!-- 스케줄 팝업 -->
  <div class="sched-popup-overlay" id="schedPopupOverlay" onclick="closeSchedPopup(event)">
    <div class="sched-popup">
      <div class="sched-popup-date" id="schedPopupDate"></div>
      <div id="schedPopupList"></div>
      <button class="sched-popup-close" onclick="document.getElementById('schedPopupOverlay').classList.remove('open');document.body.style.overflow='';">닫기</button>
    </div>
  </div>

  <!-- 캘린더 -->
  <div class="view" id="calendarView">
    <div class="cal-header">
      <button class="cal-nav-btn" onclick="calMove(-1)">&#8249;</button>
      <div class="cal-title" id="calTitle"></div>
      <button class="cal-nav-btn" onclick="calMove(1)">&#8250;</button>
    </div>
    <div class="cal-legend">
      <div class="cal-leg-item"><div class="cal-leg-dot" style="background:#1055a8;"></div>다음연락일</div>
      <div class="cal-leg-item"><div class="cal-leg-dot" style="background:#c05800;"></div>보험만기</div>
      <div class="cal-leg-item"><div class="cal-leg-dot" style="background:#a03800;"></div>자동차검사</div>
      <div class="cal-leg-item"><div class="cal-leg-dot" style="background:#a01515;"></div>금융종료</div>
      <div class="cal-leg-item"><div class="cal-leg-dot" style="background:#6020a0;"></div>정기점검</div>
      <div class="cal-leg-item"><div class="cal-leg-dot" style="background:#7a3db8;"></div>생일</div>
      <div class="cal-leg-item"><div class="cal-leg-dot" style="background:#0f6e35;"></div>출고1년↑</div>
      <div class="cal-leg-item"><div class="cal-leg-dot" style="background:#c0392b;"></div>출고3년↑</div>
    </div>
    <div class="cal-grid">
      <div class="cal-weekrow">
        <div class="cal-weekday sun">일</div>
        <div class="cal-weekday">월</div>
        <div class="cal-weekday">화</div>
        <div class="cal-weekday">수</div>
        <div class="cal-weekday">목</div>
        <div class="cal-weekday">금</div>
        <div class="cal-weekday sat">토</div>
      </div>
      <div class="cal-body" id="calBody"></div>
    </div>
  </div>

  <!-- 캘린더 팝업 -->
  <div class="cal-popup-overlay" id="calPopupOverlay" onclick="closeCalPopup(event)">
    <div class="cal-popup" id="calPopup">
      <div class="cal-popup-date" id="calPopupDate"></div>
      <div id="calPopupList"></div>
      <button class="cal-popup-close" onclick="document.getElementById('calPopupOverlay').classList.remove('open');document.body.style.overflow='';">닫기</button>
    </div>
  </div>
  <!-- 신규등록 -->
  <div class="view" id="newView">
    <div class="view-header"><div class="view-title">✏️ 신규고객 등록</div></div>
    <div class="form-card">
      <div class="form-card-header">고객 기본 정보</div>
      <div class="form-body"><div class="grid-2">
        <div class="field"><label>고객명 <span class="req">*</span></label><input type="text" id="customerName" placeholder="홍길동"/></div>
        <div class="field"><label>법인명</label><input type="text" id="companyName" placeholder="(주)회사명"/></div>
        <div class="field"><label>연락처 <span class="req">*</span></label><input type="tel" id="phone" placeholder="010-0000-0000" maxlength="13"/></div>
        <div class="field"><label>거주지역</label><input type="text" id="region" placeholder="예: 서울 강남구"/></div>
        <div class="field"><label>생년월일</label><input type="date" id="birthDate"/></div>
      </div></div>
    </div>
    <div class="form-card">
      <div class="form-card-header">차량 관심 정보</div>
      <div class="form-body"><div class="grid-2">
        <div class="field"><label>모델명</label>
          <select id="modelName" onchange="onModelChange()">
            <option value="">— 모델 선택 —</option>
            <option>A-Class</option><option>CLA</option><option>GLA / GLB</option>
            <option>C-Class</option><option>E-Class</option><option>S-Class</option>
            <option>CLE</option><option>GLC</option><option>GLE</option><option>GLS</option>
            <option>G-Class</option><option>AMG GT · SL</option><option>EQ</option>
            <option>Mercedes-Maybach</option>
          </select>
        </div>
        <div class="field"><label>세부모델</label>
          <select id="detailModel" disabled onchange="onDetailModelChange('detailModel','detailCustom')">
            <option value="">— 모델을 먼저 선택하세요 —</option>
          </select>
          <div class="detail-custom-wrap" id="detailCustomWrap">
            <input type="text" id="detailCustom" class="detail-custom-input" placeholder="세부모델명 직접 입력"/>
            <div class="detail-custom-hint">✏️ 직접 입력한 값이 저장됩니다</div>
          </div>
        </div>
      </div></div>
    </div>
    <div class="form-card">
      <div class="form-card-header">구매 정보</div>
      <div class="form-body"><div class="grid-2">
        <div class="field"><label>구매방식</label>
          <select id="purchaseMethod">
            <option value="">— 선택 —</option>
            <option>일시불</option><option>할부</option><option>리스</option>
            <option>장기렌트</option><option>법인할부</option><option>법인리스</option><option>미정</option>
          </select>
        </div>
        <div class="field"><label>구매예산</label><input type="text" id="budget" placeholder="예: 1억5천, 2억 이상"/></div>
        <div class="field"><label>구매예정시기</label><input type="text" id="purchaseTiming" placeholder="예: 2026년 12월 이내"/></div>
      </div></div>
    </div>
    <div class="form-card">
      <div class="form-card-header">상담 정보</div>
      <div class="form-body"><div class="grid-2">
        <div class="field"><label>유입경로</label>
          <select id="leadSource">
            <option value="">— 선택 —</option>
            <option>전시장방문및전화</option><option>GETCHA</option><option>지인소개</option>
            <option>에이전시</option><option>유튜브</option><option>기존고객</option>
            <option>고객소개</option><option>블로그</option><option>인스타그램</option><option>기타</option>
          </select>
        </div>
        <div class="field"><label>현재상태</label>
          <select id="status">
            <option value="">— 선택 —</option>
            <option>신규문의</option><option>A급가망</option><option>상담중</option>
            <option>계약유력</option><option>구매보류</option><option>계약</option>
            <option>출고</option><option>상담종료</option><option>해약</option>
          </select>
        </div>
        <div class="field full"><label>상담내용</label><textarea id="consultation" placeholder="상담 내용을 입력하세요."></textarea></div>
        <div class="field"><label>다음연락일</label><input type="date" id="nextContact"/></div>
      </div></div>
    </div>
    <div class="form-card">
      <div class="form-card-header care">🚗 차량관리 / 금융관리</div>
      <div class="form-body"><div class="grid-2">
        <div class="field"><label>보험가입일</label><input type="date" id="insuranceStartDate" class="care-input"/></div>
        <div class="field"><label>보험만기일</label><input type="date" id="insuranceExpireDate" class="care-input"/></div>
        <div class="field"><label>자동차검사일</label><input type="date" id="inspectionDueDate" class="care-input"/></div>
        <div class="field"><label>금융시작일</label><input type="date" id="financeStartDate" class="care-input"/></div>
        <div class="field"><label>금융종료일</label><input type="date" id="financeEndDate" class="care-input"/></div>
      </div></div>
    </div>
    <div class="form-card">
      <div class="form-card-header care">🔧 벤츠 정기점검 관리</div>
      <div class="form-body"><div class="grid-2">
        <div class="field"><label>최근 서비스점검일</label><input type="date" id="lastServiceDate" class="care-input" oninput="autoNextService()"/></div>
        <div class="field"><label>최근 점검 주행거리</label><input type="text" id="lastServiceMileage" class="care-input" placeholder="예: 15000" oninput="fmtMileage(this);autoNextService()"/></div>
        <div class="field"><label>현재 주행거리 확인일</label><input type="date" id="currentMileageDate" class="care-input"/></div>
        <div class="field"><label>현재 주행거리</label><input type="text" id="currentMileage" class="care-input" placeholder="예: 14000" oninput="fmtMileage(this)"/></div>
        <div class="field"><label>다음 무상점검 예정일 <span style="font-size:10px;color:var(--text-lt);">(자동)</span></label><input type="date" id="nextServiceDate" class="auto-field" readonly/></div>
        <div class="field"><label>다음 무상점검 예정거리 <span style="font-size:10px;color:var(--text-lt);">(자동)</span></label><input type="text" id="nextServiceMileage" class="auto-field" readonly/></div>
      </div></div>
    </div>
    <div class="form-card">
      <div class="form-card-header">계약 / 출고 정보</div>
      <div class="form-body"><div class="grid-2">
        <div class="field"><label>계약일</label><input type="date" id="contractDate"/></div>
        <div class="field"><label>출고일</label><input type="date" id="deliveryDate" oninput="autoDeliveryMonth()" onchange="autoDeliveryMonth()"/></div>
        <div class="field"><label>출고월 <span style="font-size:10px;color:var(--text-lt);">(자동)</span></label><input type="text" id="deliveryMonth" class="auto-field" readonly/></div>
      </div></div>
    </div>
    <div class="form-card">
      <div class="form-card-header">기타</div>
      <div class="form-body"><div class="field"><label>태그</label><input type="text" id="tags" placeholder="예: VIP, 재구매, 법인"/></div></div>
    </div>
    <button class="btn-submit" id="submitBtn" onclick="submitNew()">고객 정보 저장</button>
    <div class="st-msg" id="msg-new"></div>
  </div>

  <!-- 추가상담 -->
  <div class="view" id="consultView">
    <div class="view-header"><div class="view-title">📋 기존고객 추가상담</div><div class="view-sub">상담내용이 자동 누적됩니다</div></div>
    <div class="form-card">
      <div class="form-card-header blue">고객 확인 (연락처로 검색)</div>
      <div class="form-body"><div class="grid-2">
        <div class="field"><label>고객명</label><input type="text" id="u-name" placeholder="홍길동"/></div>
        <div class="field"><label>연락처 <span class="req">*</span></label><input type="tel" id="u-phone" placeholder="010-0000-0000" maxlength="13" oninput="onUPhoneInput(this)"/></div>
      </div>
      <div class="preview-box" id="preview-box">
        <div class="preview-name" id="prev-name">—</div>
        <div class="preview-meta" id="prev-meta">—</div>
        <div id="prev-status"></div>
      </div>
      <div class="nofound" id="nofound">⚠ 기존 고객을 찾을 수 없습니다. 신규고객으로 등록해주세요.</div>
      </div>
    </div>
    <div class="form-card">
      <div class="form-card-header blue">추가상담 내용</div>
      <div class="form-body"><div class="grid-2">
        <div class="field"><label>현재상태</label>
          <select id="u-status">
            <option value="">— 선택 (변경 시) —</option>
            <option>신규문의</option><option>A급가망</option><option>상담중</option>
            <option>계약유력</option><option>구매보류</option><option>계약</option>
            <option>출고</option><option>상담종료</option><option>해약</option>
          </select>
        </div>
        <div class="field"><label>다음연락일</label><input type="date" id="u-next"/></div>
        <div class="field full"><label>추가상담내용 <span class="req">*</span></label>
          <textarea id="addConsultation" placeholder="이번 상담 내용을 입력하세요." style="min-height:130px;"></textarea></div>
        <div class="field"><label>현재 주행거리 확인일</label><input type="date" id="u-mileageDate"/></div>
        <div class="field"><label>현재 주행거리</label><input type="text" id="u-mileage" placeholder="예: 14000" oninput="fmtMileage(this)"/></div>
      </div></div>
    </div>
    <button class="btn-submit blue-btn" id="updateBtn" onclick="submitUpdate()">추가상담 저장</button>
    <div class="st-msg" id="msg-update"></div>
  </div>

  <!-- 고객목록 -->
  <div class="view" id="listView">
    <div class="list-topbar">
      <div class="view-title">👥 고객목록</div>
      <div class="list-count-badge" id="list-count">0명</div>
    </div>
    <div class="search-bar">
      <input type="text" id="searchInput" placeholder="🔍 이름 또는 연락처 검색" oninput="renderList()"/>
    </div>
    <div class="backup-bar">
      <button class="backup-btn" onclick="exportData()">📤 데이터 내보내기</button>
      <button class="backup-btn" onclick="document.getElementById('importFile').click()">📥 데이터 가져오기</button>
      <input type="file" id="importFile" accept=".json" style="display:none" onchange="importData(event)"/>
      <button class="backup-btn danger" onclick="clearAllData()">🗑 전체 삭제</button>
    </div>
    <div class="filter-bar" id="filterBar">
      <div class="filter-label" id="filterLabel">전체 고객</div>
      <button class="filter-clear" onclick="clearFilter()">전체 보기 ✕</button>
    </div>
    <div class="year-filter-bar" id="yearFilterBar"></div>
    <div id="listContainer"></div>
  </div>

  <!-- 데이터분석 -->
  <div class="view" id="analyticsView">
    <div class="list-topbar"><div class="view-title">📊 영업 대시보드</div></div>
    <div class="an-year-filter" id="an-year-filter"></div>
    <div class="section-heading"><span class="dot"></span><span id="an-section-label">2026 실적 요약</span></div>
    <div class="analytics-summary">
      <div class="an-card" style="border-top:3px solid var(--green);">
        <div class="an-num" id="an-delivery-cnt" style="color:var(--green)">0</div>
        <div class="an-label" id="an-delivery-lbl">출고대수</div>
      </div>
      <div class="an-card" style="border-top:3px solid var(--blue);">
        <div class="an-num" id="an-contract-cnt" style="color:var(--blue)">0</div>
        <div class="an-label" id="an-contract-lbl">계약대수</div>
      </div>
      <div class="an-card" style="border-top:3px solid var(--purple);cursor:pointer;" onclick="showRepurchaseList()">
        <div class="an-num" id="an-repurchase-cnt" style="color:var(--purple)">0</div>
        <div class="an-label">재구매 가능 고객</div>
        <div style="font-size:10px;color:var(--text-dim);margin-top:3px;">클릭하여 목록 보기</div>
      </div>
    </div>
    <div id="repurchase-panel" style="display:none;margin-bottom:14px;">
      <div class="chart-card" style="padding:14px;">
        <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:10px;">
          <div class="chart-title" style="margin:0;">🔄 재구매 가능 고객 목록</div>
          <button onclick="document.getElementById('repurchase-panel').style.display='none'"
            style="border:none;background:var(--border);border-radius:20px;padding:4px 12px;font-size:12px;cursor:pointer;">닫기</button>
        </div>
        <div id="repurchase-list"></div>
      </div>
    </div>
    <div class="chart-card">
      <div class="chart-title">🎯 고객 파이프라인 (현재 전체)</div>
      <div id="pipeline-chart"></div>
    </div>
    <div class="chart-card">
      <div class="chart-title">🔍 유입경로별 계약전환율 <span class="an-yr-badge" id="an-lead-yr"></span></div>
      <div id="lead-conv-chart"></div>
    </div>
    <div class="chart-card">
      <div class="chart-title">🏎 차종별 계약전환율 <span class="an-yr-badge" id="an-model-conv-yr"></span></div>
      <div id="model-conv-chart"></div>
    </div>
    <div class="chart-card">
      <div class="chart-title">🤝 월별 계약대수 <span class="an-yr-badge" id="an-contract-yr"></span></div>
      <div class="chart-wrap"><canvas id="chart-monthly-contract"></canvas></div>
    </div>
    <div class="chart-card">
      <div class="chart-title">🚗 월별 출고대수 <span class="an-yr-badge" id="an-delivery-yr"></span></div>
      <div class="chart-wrap"><canvas id="chart-monthly-delivery"></canvas></div>
    </div>
  </div>
</div>

<!-- 수정 모달 -->
<div class="edit-overlay" id="editOverlay" onclick="editOverlayClick(event)">
  <div class="edit-modal" id="editModal">
    <div class="edit-modal-header">
      <div class="edit-modal-title">✏️ 고객 정보 수정</div>
      <button class="edit-modal-close" onclick="closeEdit()">✕</button>
    </div>
    <div class="edit-modal-body">
      <div class="form-card">
        <div class="form-card-header">고객 기본 정보</div>
        <div class="form-body"><div class="grid-2">
          <div class="field"><label>고객명 <span class="req">*</span></label><input type="text" id="e-customerName"/></div>
          <div class="field"><label>법인명</label><input type="text" id="e-companyName"/></div>
          <div class="field"><label>연락처 <span class="req">*</span></label><input type="tel" id="e-phone" maxlength="13"/></div>
          <div class="field"><label>거주지역</label><input type="text" id="e-region"/></div>
          <div class="field"><label>생년월일</label><input type="date" id="e-birthDate"/></div>
        </div></div>
      </div>
      <div class="form-card">
        <div class="form-card-header">차량 관심 정보</div>
        <div class="form-body"><div class="grid-2">
          <div class="field"><label>모델명</label>
            <select id="e-modelName" onchange="onEditModelChange()">
              <option value="">— 모델 선택 —</option>
              <option>A-Class</option><option>CLA</option><option>GLA / GLB</option>
              <option>C-Class</option><option>E-Class</option><option>S-Class</option>
              <option>CLE</option><option>GLC</option><option>GLE</option><option>GLS</option>
              <option>G-Class</option><option>AMG GT · SL</option><option>EQ</option>
              <option>Mercedes-Maybach</option>
            </select>
          </div>
          <div class="field"><label>세부모델</label>
            <select id="e-detailModel" onchange="onDetailModelChange('e-detailModel','e-detailCustom')">
              <option value="">— 모델을 먼저 선택하세요 —</option>
            </select>
            <div class="detail-custom-wrap" id="e-detailCustomWrap">
              <input type="text" id="e-detailCustom" class="detail-custom-input" placeholder="세부모델명 직접 입력"/>
              <div class="detail-custom-hint">✏️ 직접 입력한 값이 저장됩니다</div>
            </div>
          </div>
        </div></div>
      </div>
      <div class="form-card">
        <div class="form-card-header">구매 정보</div>
        <div class="form-body"><div class="grid-2">
          <div class="field"><label>구매방식</label>
            <select id="e-purchaseMethod">
              <option value="">— 선택 —</option>
              <option>일시불</option><option>할부</option><option>리스</option>
              <option>장기렌트</option><option>법인할부</option><option>법인리스</option><option>미정</option>
            </select>
          </div>
          <div class="field"><label>구매예산</label><input type="text" id="e-budget"/></div>
          <div class="field full"><label>구매예정시기</label><input type="text" id="e-purchaseTiming"/></div>
        </div></div>
      </div>
      <div class="form-card">
        <div class="form-card-header">상담 정보</div>
        <div class="form-body"><div class="grid-2">
          <div class="field"><label>유입경로</label>
            <select id="e-leadSource">
              <option value="">— 선택 —</option>
              <option>전시장방문및전화</option><option>GETCHA</option><option>지인소개</option>
              <option>에이전시</option><option>유튜브</option><option>기존고객</option>
              <option>고객소개</option><option>블로그</option><option>인스타그램</option><option>기타</option>
            </select>
          </div>
          <div class="field"><label>현재상태</label>
            <select id="e-status">
              <option value="">— 선택 —</option>
              <option>신규문의</option><option>A급가망</option><option>상담중</option>
              <option>계약유력</option><option>구매보류</option><option>계약</option>
              <option>출고</option><option>상담종료</option><option>해약</option>
            </select>
          </div>
          <div class="field full"><label>상담내용</label><textarea id="e-consultation" style="min-height:100px;"></textarea></div>
          <div class="field"><label>다음연락일</label><input type="date" id="e-nextContact"/></div>
        </div></div>
      </div>
      <div class="form-card">
        <div class="form-card-header care">🚗 차량관리 / 금융관리</div>
        <div class="form-body"><div class="grid-2">
          <div class="field"><label>보험가입일</label><input type="date" id="e-insuranceStartDate" class="care-input"/></div>
          <div class="field"><label>보험만기일</label><input type="date" id="e-insuranceExpireDate" class="care-input"/></div>
          <div class="field"><label>자동차검사일</label><input type="date" id="e-inspectionDueDate" class="care-input"/></div>
          <div class="field"><label>금융시작일</label><input type="date" id="e-financeStartDate" class="care-input"/></div>
          <div class="field"><label>금융종료일</label><input type="date" id="e-financeEndDate" class="care-input"/></div>
        </div></div>
      </div>
      <div class="form-card">
        <div class="form-card-header care">🔧 벤츠 정기점검 관리</div>
        <div class="form-body"><div class="grid-2">
          <div class="field"><label>최근 서비스점검일</label><input type="date" id="e-lastServiceDate" class="care-input" oninput="autoEditNextService()"/></div>
          <div class="field"><label>최근 점검 주행거리</label><input type="text" id="e-lastServiceMileage" class="care-input" oninput="fmtMileage(this);autoEditNextService()"/></div>
          <div class="field"><label>현재 주행거리 확인일</label><input type="date" id="e-currentMileageDate" class="care-input"/></div>
          <div class="field"><label>현재 주행거리</label><input type="text" id="e-currentMileage" class="care-input" oninput="fmtMileage(this)"/></div>
          <div class="field"><label>다음 무상점검 예정일</label><input type="date" id="e-nextServiceDate" class="auto-field" readonly/></div>
          <div class="field"><label>다음 무상점검 예정거리</label><input type="text" id="e-nextServiceMileage" class="auto-field" readonly/></div>
        </div></div>
      </div>
      <div class="form-card">
        <div class="form-card-header">계약 / 출고 정보</div>
        <div class="form-body"><div class="grid-2">
          <div class="field"><label>계약일</label><input type="date" id="e-contractDate"/></div>
          <div class="field"><label>출고일</label><input type="date" id="e-deliveryDate" oninput="autoEditDeliveryMonth()" onchange="autoEditDeliveryMonth()"/></div>
          <div class="field"><label>출고월 <span style="font-size:10px;color:var(--text-lt);">(자동)</span></label><input type="text" id="e-deliveryMonth" class="auto-field" readonly/></div>
        </div></div>
      </div>
      <div class="form-card">
        <div class="form-card-header">기타</div>
        <div class="form-body"><div class="field"><label>태그</label><input type="text" id="e-tags"/></div></div>
      </div>
    </div>
    <div class="st-msg edit-save-msg" id="edit-save-msg"></div>
    <div class="edit-modal-footer">
      <button class="btn-edit-cancel" onclick="closeEdit()">취소</button>
      <button class="btn-edit-save" id="editSaveBtn" onclick="submitEdit()">✅ 수정 저장</button>
    </div>
  </div>
</div>

<!-- 고객 상세 오버레이 -->
<div class="customer-detail-overlay" id="custDetailOverlay" onclick="custDetailOverlayClick(event)">
  <div class="customer-detail-modal">
    <div class="edit-modal-header">
      <div class="edit-modal-title">👤 고객 상세 정보</div>
      <button class="edit-modal-close" onclick="closeCustDetail()">✕</button>
    </div>
    <div class="edit-modal-body" id="custDetailBody" style="padding:18px 16px 20px;"></div>
  </div>
</div>

<nav class="bottom-nav">
  <button class="nav-btn active" id="nb-home" onclick="showView('home')"><span class="nav-icon">🏠</span><span class="nav-label">홈</span></button>
  <button class="nav-btn" id="nb-schedule" onclick="showView('schedule')"><span class="nav-icon">🗓</span><span class="nav-label">스케줄</span></button>
  <button class="nav-btn-plus" id="nb-plus" onclick="showView('new')">
    <svg viewBox="0 0 24 24" fill="none"><path d="M12 4v16M4 12h16" stroke="#fff" stroke-width="2.5" stroke-linecap="round"/></svg>
  </button>
  <button class="nav-btn" id="nb-list" onclick="showView('list')"><span class="nav-icon">👥</span><span class="nav-label">고객목록</span></button>
  <button class="nav-btn" id="nb-more" onclick="showView('analytics')"><span class="nav-icon">📊</span><span class="nav-label">분석</span></button>
</nav>

<script>
var LS_KEY = "gy_crm_v1";
var currentFilter = "";
var currentCareFilter = "";
var currentDeliveryYear = "";
var renderedCustomers = [];

// ── 세부모델 데이터 ──────────────────────────────────────────
var DETAIL_MODELS = {
  "A-Class":["A 220 Hatch","A 220 Sedan","Mercedes-AMG A 35 4MATIC","Mercedes-AMG A 45 S 4MATIC+"],
  "CLA":["CLA 250 4MATIC AMG Line","Mercedes-AMG CLA 45 S 4MATIC+","Mercedes-AMG CLA 45 S 4MATIC+ Final Edition"],
  "GLA / GLB":["GLA 250 4MATIC AMG Line","GLB 200 d","GLB 250 4MATIC","Mercedes-AMG GLB 35 4MATIC"],
  "C-Class":["C 200 AVANTGARDE","C 200 AMG Line"],
  "E-Class":["E 200 AVANTGARDE","E 200 EXCLUSIVE","E 200 AMG Line","E 200 AMG Line RoF Edition",
    "E 220 d 4MATIC EXCLUSIVE","E 300 4MATIC EXCLUSIVE","E 300 4MATIC AMG Line",
    "E 300 4MATIC 140주년 Edition","E 350 e 4MATIC EXCLUSIVE",
    "E 450 4MATIC AMG Line","E 450 4MATIC EXCLUSIVE","Mercedes-AMG E 53 HYBRID 4MATIC+"],
  "S-Class":["S 350 d 4MATIC","S 450 4MATIC SWB","S 450 4MATIC Long","S 500 4MATIC","S 580 4MATIC",
    "Mercedes-AMG S 63 E Performance","S 450 4MATIC Long Exclusive",
    "S 450 4MATIC Long AMG Line","S 500 4MATIC Long","S 580 4MATIC Long"],
  "CLE":["CLE 200 Coupé AMG Line","CLE 450 4MATIC Coupé AMG Line","Mercedes-AMG CLE 53 4MATIC+ Coupé",
    "CLE 200 Cabriolet AMG Line","CLE 450 4MATIC Cabriolet AMG Line","Mercedes-AMG CLE 53 4MATIC+ Cabriolet"],
  "GLC":["GLC 220 d 4MATIC","GLC 300 4MATIC AVANTGARDE","GLC 300 4MATIC AMG Line",
    "Mercedes-AMG GLC 43 4MATIC","GLC 300 4MATIC Coupé AVANTGARDE",
    "GLC 300 4MATIC Coupé AMG Line","Mercedes-AMG GLC 43 4MATIC Coupé"],
  "GLE":["GLE 350 4MATIC","GLE 300 d 4MATIC","GLE 450 4MATIC","GLE 450 4MATIC AMG Line",
    "Mercedes-AMG GLE 53 4MATIC+","GLE 450 d 4MATIC Coupé AMG Line",
    "GLE 450 4MATIC Coupé AMG Line","Mercedes-AMG GLE 53 4MATIC+ Coupé"],
  "GLS":["GLS 450 d 4MATIC AMG Line","GLS 450 4MATIC AMG Line","GLS 450 d 4MATIC AMG Line Premium",
    "GLS 450 4MATIC AMG Line Premium","GLS 580 4MATIC AMG Line","Mercedes-AMG GLS 63 4MATIC+"],
  "G-Class":["G 450 d","G 450 d MANUFAKTUR","Mercedes-AMG G 63","Mercedes-AMG G 63 MANUFAKTUR"],
  "AMG GT · SL":["Mercedes-AMG SL 43","Mercedes-AMG SL 63 4MATIC+","Mercedes-AMG GT 55 4MATIC+",
    "Mercedes-AMG GT 63 S E Performance","Mercedes-AMG GT 43 4MATIC+"],
  "EQ":["EQA 250 Progressive","EQA 250 AMG Line","EQB 300 4MATIC Progressive",
    "EQB 300 4MATIC AMG Line","EQE 350+ SUV","EQE 350+","EQE 500 4MATIC SUV",
    "EQS 450 4MATIC SUV","EQS 580 4MATIC SUV","G 580 with EQ Technology"],
  "Mercedes-Maybach":["Mercedes-Maybach S 580","Mercedes-Maybach S 680",
    "Mercedes-Maybach S 680 V12 Edition","Mercedes-Maybach SL 680","Mercedes-Maybach GLS 600",
    "Mercedes-Maybach GLS 600 MANUFAKTUR","Mercedes-Maybach EQS 680 SUV"]
};

function buildDetailModelOptions(sel, modelName){
  sel.innerHTML='<option value="">— 세부모델 선택 —</option>';
  (DETAIL_MODELS[modelName]||[]).forEach(function(v){
    var o=document.createElement("option");
    o.value=v; o.textContent=v; sel.appendChild(o);
  });
  var sep=document.createElement("option"); sep.value=""; sep.disabled=true; sep.textContent="─────────────"; sel.appendChild(sep);
  var custom=document.createElement("option"); custom.value="__custom__"; custom.textContent="✏️ 직접입력…"; sel.appendChild(custom);
}
function onDetailModelChange(selectId, inputId){
  var sel=document.getElementById(selectId);
  var wrapId=selectId==="detailModel"?"detailCustomWrap":"e-detailCustomWrap";
  var wrap=document.getElementById(wrapId);
  var inp=document.getElementById(inputId);
  if(!wrap||!inp)return;
  if(sel.value==="__custom__"){wrap.classList.add("visible");inp.focus();}
  else{wrap.classList.remove("visible");inp.value="";}
}
function getDetailModelValue(selectId, inputId){
  var customVal=(document.getElementById(inputId)||{}).value||"";
  customVal=customVal.trim();
  if(customVal)return customVal;
  var selEl=document.getElementById(selectId);
  var selVal=selEl?selEl.value:"";
  return selVal==="__custom__"?"":selVal;
}
function onModelChange(){
  var m=document.getElementById("modelName").value;
  var sel=document.getElementById("detailModel");
  var wrap=document.getElementById("detailCustomWrap");
  var inp=document.getElementById("detailCustom");
  sel.disabled=true;
  if(wrap)wrap.classList.remove("visible");
  if(inp)inp.value="";
  if(!m){sel.innerHTML='<option value="">— 모델을 먼저 선택하세요 —</option>';return;}
  buildDetailModelOptions(sel,m); sel.disabled=false;
}
function onEditModelChange(){
  var m=document.getElementById("e-modelName").value;
  var sel=document.getElementById("e-detailModel");
  var wrap=document.getElementById("e-detailCustomWrap");
  var inp=document.getElementById("e-detailCustom");
  sel.disabled=true;
  if(wrap)wrap.classList.remove("visible");
  if(inp)inp.value="";
  if(!m){sel.innerHTML='<option value="">— 모델을 먼저 선택하세요 —</option>';return;}
  buildDetailModelOptions(sel,m); sel.disabled=false;
}

// ── 날짜 유틸 ────────────────────────────────────────────────
function normalizeDateStr(v){
  if(v===null||v===undefined)return "";
  if(v instanceof Date){
    if(isNaN(v.getTime()))return "";
    var y=v.getFullYear(),m=v.getMonth()+1,d=v.getDate();
    return y+"-"+String(m).padStart(2,"0")+"-"+String(d).padStart(2,"0");
  }
  if(typeof v==="number")return "";
  var s=String(v).trim(); if(!s)return "";
  var y,m,d;
  var r1=s.match(/^(\d{4})[-\/](\d{1,2})[-\/](\d{1,2})/);
  if(r1){y=+r1[1];m=+r1[2];d=+r1[3];}
  if(!y){var r2=s.match(/^(\d{4})\.\s*(\d{1,2})\.\s*(\d{1,2})/);if(r2){y=+r2[1];m=+r2[2];d=+r2[3];}}
  if(!y){var r3=s.match(/^(\d{1,2})\/(\d{1,2})\/(\d{4})/);if(r3){m=+r3[1];d=+r3[2];y=+r3[3];}}
  if(!y){try{var dt=new Date(s);if(!isNaN(dt.getTime())){y=dt.getFullYear();m=dt.getMonth()+1;d=dt.getDate();}}catch(e){}}
  if(!y||isNaN(y)||isNaN(m)||isNaN(d))return "";
  if(y<1900||y>2200||m<1||m>12||d<1||d>31)return "";
  return y+"-"+String(m).padStart(2,"0")+"-"+String(d).padStart(2,"0");
}
function todayD(){
  var kst=new Date(new Date().getTime()+9*3600000);
  return new Date(kst.getUTCFullYear(),kst.getUTCMonth(),kst.getUTCDate());
}
function parseD(s){
  var norm=normalizeDateStr(s);
  if(!norm)return null;
  var p=norm.split("-");
  return new Date(parseInt(p[0]),parseInt(p[1])-1,parseInt(p[2]));
}
function dFrom(ds){var d=parseD(ds);if(!d)return null;return Math.round((d-todayD())/86400000);}
function getNow(){return new Date().toLocaleString("ko-KR",{timeZone:"Asia/Seoul"});}
function getTs(){
  var d=new Date(),k=new Date(d.getTime()+9*3600000);
  var y=k.getUTCFullYear(),mo=String(k.getUTCMonth()+1).padStart(2,"0");
  var dy=String(k.getUTCDate()).padStart(2,"0"),h=String(k.getUTCHours()).padStart(2,"0");
  var mi=String(k.getUTCMinutes()).padStart(2,"0");
  return "["+y+"-"+mo+"-"+dy+" "+h+":"+mi+" 추가상담]";
}
function bdayDiff(birthDate){
  if(!birthDate)return null;
  var b=parseD(birthDate);if(!b)return null;
  var now=todayD();
  var thisYr=new Date(now.getFullYear(),b.getMonth(),b.getDate());
  var diff=Math.round((thisYr-now)/86400000);
  if(diff<0){var nextYr=new Date(now.getFullYear()+1,b.getMonth(),b.getDate());diff=Math.round((nextYr-now)/86400000);}
  return diff;
}
function ddTag(days,warnDays){
  if(days===null)return"";
  warnDays=warnDays||30;
  if(days<0)return'<span class="dd urg">D+'+Math.abs(days)+'</span>';
  if(days===0)return'<span class="dd urg">D-Day</span>';
  if(days<=warnDays)return'<span class="dd warn">D-'+days+'</span>';
  return'<span class="dd ok">D-'+days+'</span>';
}
function parseMileage(s){
  if(!s)return null;
  var n=parseInt(String(s).replace(/[^0-9]/g,""),10);
  return isNaN(n)?null:n;
}
function fmtMileage(el){
  var raw=el.value.replace(/[^0-9]/g,"");
  if(!raw){el.value="";return;}
  var num=parseInt(raw,10);
  el.value=num.toLocaleString("ko-KR")+"km";
  var len=el.value.length-2;
  try{el.setSelectionRange(len,len);}catch(e){}
}
function isMaintenanceDue(c){
  var inD=dFrom(c.inspectionDueDate);if(inD!==null&&inD>=0&&inD<=30)return true;
  var sD=dFrom(c.nextServiceDate);if(sD!==null&&sD>=0&&sD<=30)return true;
  var cur=parseMileage(c.currentMileage);
  var nxt=parseMileage(c.nextServiceMileage);
  if(cur!==null&&nxt!==null&&(nxt-cur)<=1000&&(nxt-cur)>=0)return true;
  return false;
}
function autoDeliveryMonth(){
  var d=document.getElementById("deliveryDate").value;
  var m=document.getElementById("deliveryMonth");
  if(d&&d.length>=7)m.value=d.substring(0,7); else m.value="";
  autoNextService();
}
function autoEditDeliveryMonth(){
  var d=document.getElementById("e-deliveryDate").value;
  var m=document.getElementById("e-deliveryMonth");
  if(d&&d.length>=7)m.value=d.substring(0,7); else m.value="";
  autoEditNextService();
}
function autoNextService(){
  var baseDate=document.getElementById("lastServiceDate").value;
  if(!baseDate)baseDate=document.getElementById("deliveryDate").value;
  var nextDateEl=document.getElementById("nextServiceDate");
  if(baseDate&&baseDate.length>=10){var bd=new Date(baseDate);bd.setFullYear(bd.getFullYear()+1);nextDateEl.value=bd.toISOString().substring(0,10);}
  else nextDateEl.value="";
  var lastKm=parseMileage(document.getElementById("lastServiceMileage").value);
  var nextKm=(lastKm!==null)?lastKm+15000:15000;
  document.getElementById("nextServiceMileage").value=nextKm.toLocaleString("ko-KR")+"km";
}
function autoEditNextService(){
  var baseDate=document.getElementById("e-lastServiceDate").value;
  if(!baseDate)baseDate=document.getElementById("e-deliveryDate").value;
  var nextDateEl=document.getElementById("e-nextServiceDate");
  if(baseDate&&baseDate.length>=10){var bd=new Date(baseDate);bd.setFullYear(bd.getFullYear()+1);nextDateEl.value=bd.toISOString().substring(0,10);}
  else nextDateEl.value="";
  var lastKm=parseMileage(document.getElementById("e-lastServiceMileage").value);
  var nextKm=(lastKm!==null)?lastKm+15000:15000;
  document.getElementById("e-nextServiceMileage").value=nextKm.toLocaleString("ko-KR")+"km";
}

// ── localStorage ─────────────────────────────────────────────
function load(){try{return JSON.parse(localStorage.getItem(LS_KEY))||[];}catch(e){return[];}}
function save(l){localStorage.setItem(LS_KEY,JSON.stringify(l));}
function normP(p){return(p||"").replace(/\D/g,"");}
function findC(phone){
  var list=load(),np=normP(phone);
  for(var i=0;i<list.length;i++)if(normP(list[i].phone)===np)return{idx:i,c:list[i]};
  return null;
}

// ── 상태 매핑 ────────────────────────────────────────────────
function sc(s){
  if(s==="신규문의")return"s1";if(s==="A급가망")return"s2";
  if(s==="상담중")return"s3";if(s==="계약유력")return"s4";
  if(s==="구매보류")return"s5";if(s==="계약")return"s6";
  if(s==="출고")return"s7";if(s==="상담종료")return"s8";
  if(s==="해약")return"s9";return"s0";
}
function esc(s){if(!s)return"";return String(s).replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;").replace(/"/g,"&quot;");}

// ── 화면 전환 ────────────────────────────────────────────────
var VIEWS={home:"homeView",schedule:"scheduleView",calendar:"calendarView",new:"newView",consult:"consultView",list:"listView",analytics:"analyticsView"};
var VNAMES={home:"대시보드",schedule:"스케줄",calendar:"캘린더",new:"신규등록",consult:"추가상담",list:"고객목록",analytics:"데이터분석"};
var NBS={home:"nb-home",schedule:"nb-schedule",list:"nb-list",analytics:"nb-more"};
var _calYear,_calMonth;
(function(){var k=new Date(new Date().getTime()+9*3600000);_calYear=k.getUTCFullYear();_calMonth=k.getUTCMonth();})();

function showView(v){
  Object.values(VIEWS).forEach(function(id){var el=document.getElementById(id);if(el)el.classList.remove("active");});
  var t=document.getElementById(VIEWS[v]);if(t)t.classList.add("active");
  document.getElementById("viewBadge").textContent=VNAMES[v]||"";
  Object.keys(NBS).forEach(function(k){var b=document.getElementById(NBS[k]);if(b)b.classList.toggle("active",k===v);});
  var plus=document.getElementById("nb-plus");if(plus)plus.classList.toggle("active",v==="new");
  var pbs=document.querySelectorAll(".pc-tab-btn");
  ["home","schedule","calendar","new","consult","list","analytics"].forEach(function(k,i){if(pbs[i])pbs[i].classList.toggle("active",k===v);});
  if(v==="list")renderList();
  if(v==="home"){updateDash();renderHomeContacts();}
  if(v==="analytics")renderAnalytics();
  if(v==="calendar")renderCalendar();
  if(v==="schedule"){schedInitDate();renderSchedCalendar();}
  window.scrollTo({top:0,behavior:"smooth"});
  hideMsg("msg-new");hideMsg("msg-update");
}

/* ══ 캘린더 ══ */
function calMove(dir){
  _calMonth+=dir;
  if(_calMonth>11){_calMonth=0;_calYear++;}
  if(_calMonth<0){_calMonth=11;_calYear--;}
  renderCalendar();
}

function buildCalEvents(list){
  var evMap={};
  function addEv(dateStr,ev){var n=normalizeDateStr(dateStr);if(!n||n.length<10)return;if(!evMap[n])evMap[n]=[];evMap[n].push(ev);}
  var now=todayD();
  list.forEach(function(c){
    var name=c.customerName||"(이름없음)",phone=c.phone||"";
    if(c.nextContact)addEv(c.nextContact,{type:"contact",name:name,phone:phone,label:"📞 연락: "+name,dotColor:"#1055a8"});
    if(c.insuranceExpireDate)addEv(c.insuranceExpireDate,{type:"insurance",name:name,phone:phone,label:"🛡 보험만기: "+name,dotColor:"#c05800"});
    if(c.inspectionDueDate)addEv(c.inspectionDueDate,{type:"inspection",name:name,phone:phone,label:"🔧 자동차검사: "+name,dotColor:"#a03800"});
    if(c.financeEndDate)addEv(c.financeEndDate,{type:"finance",name:name,phone:phone,label:"💳 금융종료: "+name,dotColor:"#a01515"});
    if(c.nextServiceDate)addEv(c.nextServiceDate,{type:"service",name:name,phone:phone,label:"🔧 정기점검: "+name,dotColor:"#6020a0"});
    if(c.birthDate){
      var b=parseD(c.birthDate);
      if(b){
        var bThisYear=new Date(_calYear,b.getMonth(),b.getDate());
        addEv(normalizeDateStr(bThisYear),{type:"birthday",name:name,phone:phone,label:"🎂 생일: "+name,dotColor:"#7a3db8"});
      }
    }
    if(c.deliveryDate){
      var del=parseD(c.deliveryDate);
      if(del){
        var annYrs=_calYear-del.getFullYear();
        if(annYrs>=1){
          var ann=new Date(_calYear,del.getMonth(),del.getDate());
          var evType=annYrs>=3?"delivery3":"delivery1";
          addEv(normalizeDateStr(ann),{type:evType,name:name,phone:phone,label:"🚗 출고"+annYrs+"년: "+name,dotColor:annYrs>=3?"#c0392b":"#0f6e35"});
        }
      }
    }
  });
  return evMap;
}

function renderCalendar(){
  document.getElementById("calTitle").textContent=_calYear+"년 "+(_calMonth+1)+"월";
  var list=load();
  var evMap=buildCalEvents(list);
  var body=document.getElementById("calBody");
  body.innerHTML="";
  var firstDay=new Date(_calYear,_calMonth,1).getDay();
  var lastDate=new Date(_calYear,_calMonth+1,0).getDate();
  var prevLastDate=new Date(_calYear,_calMonth,0).getDate();
  var kst=new Date(new Date().getTime()+9*3600000);
  var todayStr=(_calYear===kst.getUTCFullYear()&&_calMonth===kst.getUTCMonth())?String(kst.getUTCDate()):"";
  var totalCells=Math.ceil((firstDay+lastDate)/7)*7;
  var MAX_VISIBLE=3;
  for(var i=0;i<totalCells;i++){
    var cell=document.createElement("div");
    cell.className="cal-cell";
    var dateNum,isOther=false,mo=_calMonth,yr=_calYear;
    if(i<firstDay){dateNum=prevLastDate-firstDay+i+1;isOther=true;mo=_calMonth-1;if(mo<0){mo=11;yr--;}}
    else if(i-firstDay>=lastDate){dateNum=i-firstDay-lastDate+1;isOther=true;mo=_calMonth+1;if(mo>11){mo=0;yr++;}}
    else{dateNum=i-firstDay+1;}
    var dow=i%7;
    if(isOther)cell.classList.add("other-month");
    if(!isOther&&String(dateNum)===todayStr)cell.classList.add("today");
    if(dow===0)cell.classList.add("sun");
    if(dow===6)cell.classList.add("sat");
    var dateDiv=document.createElement("div");
    dateDiv.className="cal-date";
    dateDiv.textContent=dateNum;
    cell.appendChild(dateDiv);
    var fullDate=yr+"-"+String(mo+1).padStart(2,"0")+"-"+String(dateNum).padStart(2,"0");
    var evs=evMap[fullDate]||[];
    if(evs.length>0){
      var dotsDiv=document.createElement("div");
      dotsDiv.className="cal-dots";
      var visible=evs.slice(0,MAX_VISIBLE),hidden=evs.slice(MAX_VISIBLE);
      visible.forEach(function(ev){
        var evEl=document.createElement("div");
        evEl.className="cal-event type-"+ev.type;
        evEl.textContent=ev.label;
        evEl.title=ev.label;
        evEl.onclick=(function(d,evArr){return function(e){e.stopPropagation();openCalPopup(d,evArr);};})(fullDate,evs);
        dotsDiv.appendChild(evEl);
      });
      if(hidden.length>0){
        var moreEl=document.createElement("div");
        moreEl.className="cal-more";
        moreEl.textContent="+"+hidden.length+"개 더";
        moreEl.onclick=(function(d,evArr){return function(e){e.stopPropagation();openCalPopup(d,evArr);};})(fullDate,evs);
        dotsDiv.appendChild(moreEl);
      }
      cell.appendChild(dotsDiv);
      cell.style.cursor="pointer";
      cell.onclick=(function(d,evArr){return function(){openCalPopup(d,evArr);};})(fullDate,evs);
    }
    body.appendChild(cell);
  }
}

function openCalPopup(dateStr,evs){
  var p=dateStr.split("-");
  document.getElementById("calPopupDate").textContent=parseInt(p[0])+"년 "+parseInt(p[1])+"월 "+parseInt(p[2])+"일";
  var TYPE_COLORS={contact:"#1055a8",insurance:"#c05800",inspection:"#a03800",finance:"#a01515",service:"#6020a0",birthday:"#7a3db8",delivery1:"#0f6e35",delivery3:"#c0392b"};
  var html="";
  evs.forEach(function(ev){
    var color=TYPE_COLORS[ev.type]||"#666";
    html+='<div class="cal-popup-item"><div class="cal-popup-dot" style="background:'+color+';"></div><div><div class="cal-popup-name">'+esc(ev.label)+'</div>'+(ev.phone?'<div class="cal-popup-desc"><a href="tel:'+esc(ev.phone)+'" style="color:var(--gold-dk);font-weight:700;text-decoration:none;">📞 '+esc(ev.phone)+'</a></div>':'')+' </div></div>';
  });
  document.getElementById("calPopupList").innerHTML=html;
  document.getElementById("calPopupOverlay").classList.add("open");
  document.body.style.overflow="hidden";
}
function closeCalPopup(e){if(e.target===document.getElementById("calPopupOverlay")){document.getElementById("calPopupOverlay").classList.remove("open");document.body.style.overflow="";}}
function goFilter(st){currentFilter=st;currentCareFilter="";currentDeliveryYear="";showView("list");}
function goCareFilter(type){currentFilter="";currentCareFilter=type;currentDeliveryYear="";showView("list");}
function clearFilter(){currentFilter="";currentCareFilter="";currentDeliveryYear="";document.getElementById("filterBar").style.display="none";renderList();}
function setDeliveryYear(yr){currentDeliveryYear=yr;renderList();}

// ── 대시보드 ────────────────────────────────────────────────
function updateDash(){
  var list=load(),now=todayD();
  var tot=list.length,ni=0,ag=0,co=0,li=0,kt=0,de=0,ca=0;
  var bday=0,ins=0,maint=0,fin=0,y1=0,y3=0;
  var kst=new Date(new Date().getTime()+9*3600000);
  var thisYear=kst.getUTCFullYear(),thisMon=kst.getUTCMonth()+1;
  var yyyyMM=thisYear+"-"+String(thisMon).padStart(2,"0");
  var yyyyStr=String(thisYear);
  var mc=0,md=0,yc=0,yd=0;
  list.forEach(function(x){
    var v=x.status||"";
    if(v==="신규문의")ni++;else if(v==="A급가망")ag++;
    else if(v==="상담중")co++;else if(v==="계약유력")li++;
    else if(v==="계약")kt++;else if(v==="출고")de++;
    else if(v==="해약")ca++;
    if(x.birthDate){var bd=bdayDiff(x.birthDate);if(bd!==null&&bd>=0&&bd<=7)bday++;}
    var iD=dFrom(x.insuranceExpireDate);if(iD!==null&&iD>=0&&iD<=30)ins++;
    if(isMaintenanceDue(x))maint++;
    var fD=dFrom(x.financeEndDate);if(fD!==null&&fD>=0&&fD<=90)fin++;
    var del=parseD(x.deliveryDate);
    if(del){var el=Math.round((now-del)/86400000);if(el>=365)y1++;if(el>=1095)y3++;}
    try{var cd=normalizeDateStr(x.contractDate);if(cd.length>=7){if(cd.substring(0,7)===yyyyMM)mc++;if(cd.substring(0,4)===yyyyStr)yc++;}}catch(e){}
    try{var dd=normalizeDateStr(x.deliveryDate);if(dd.length>=7){if(dd.substring(0,7)===yyyyMM)md++;if(dd.substring(0,4)===yyyyStr)yd++;}}catch(e){}
  });
  function set(id,v){var e=document.getElementById(id);if(e)e.textContent=v;}
  set("d-total",tot);set("d-new",ni);set("d-a",ag);set("d-c",co);
  set("d-l",li);set("d-k",kt);set("d-d",de);set("d-x",ca);
  set("dc-bday",bday);set("dc-ins",ins);set("dc-insp",maint);
  set("dc-fin",fin);set("dc-1yr",y1);set("dc-3yr",y3);
  set("list-count",tot+"명");
  set("sp-mc",mc);set("sp-md",md);set("sp-yc",yc);set("sp-yd",yd);
  set("sp-mc-lbl",thisMon+"월");set("sp-md-lbl",thisMon+"월");
  set("sp-yc-lbl",thisYear+"년");set("sp-yd-lbl",thisYear+"년");
}

// ── 오늘/이번주 연락 ────────────────────────────────────────
function renderHomeContacts(){
  var list=load();
  var kst=new Date(new Date().getTime()+9*3600000);
  var now=new Date(kst.getUTCFullYear(),kst.getUTCMonth(),kst.getUTCDate());
  var today=[],week=[];
  list.forEach(function(c){
    var nc=String(c.nextContact||"").trim();
    var norm=normalizeDateStr(nc);
    if(!norm||norm.length<10)return;
    var p=norm.split("-");
    var nd=new Date(parseInt(p[0]),parseInt(p[1])-1,parseInt(p[2]));
    var df=Math.round((nd-now)/86400000);
    if(df===0)today.push({c:c,df:df});
    else if(df>0&&df<=7)week.push({c:c,df:df});
  });
  var te=document.getElementById("todayList");
  if(te)te.innerHTML=today.length?today.map(function(x){return cItem(x.c,x.df);}).join(""):'<div class="contact-empty">오늘 연락할 고객이 없습니다</div>';
  week.sort(function(a,b){return a.df-b.df;});
  var we=document.getElementById("weekList");
  if(we)we.innerHTML=week.length?week.map(function(x){return cItem(x.c,x.df);}).join(""):'<div class="contact-empty">이번주 연락할 고객이 없습니다</div>';
}
function cItem(c,df){
  var model=(c.modelName||"")+(c.detailModel?" · "+c.detailModel:"");
  var sd=c.status||"—";
  var safePhone=(c.phone||"").replace(/"/g,"&quot;");
  var safeName=(c.customerName||"").replace(/"/g,"&quot;");
  return'<div class="contact-item">'+
    '<div style="min-width:0;flex:1;">'+
      '<div class="c-name clickable-name" data-phone="'+safePhone+'" data-cname="'+safeName+'">'+esc(c.customerName)+'</div>'+
      '<div class="c-meta">'+esc(model||"모델미정")+' | <span class="badge '+sc(c.status)+'">'+esc(sd)+'</span> | D-'+df+'일</div>'+
    '</div>'+
    '<a class="call-btn" href="tel:'+esc(c.phone)+'">📞 전화</a>'+
  '</div>';
}
document.addEventListener("click",function(e){
  var el=e.target.closest(".clickable-name");
  if(!el)return;
  e.preventDefault();e.stopPropagation();
  openCustomerDetailFromDashboard(el.getAttribute("data-phone")||"",el.getAttribute("data-cname")||"");
});

// ── 고객목록 렌더링 ──────────────────────────────────────────
var CARE_LABELS={"birthday7":"🎂 생일 D-7 고객","insurance30":"🛡 보험만기 30일 이내","maintenance":"🔧 검사/정기점검 임박","finance90":"💳 금융종료 90일 이내","delivery1yr":"🚗 출고 1년 이상","delivery3yr":"🚗 출고 3년 이상"};

function renderList(){
  var list=load();
  var q=(document.getElementById("searchInput")||{}).value||"";
  q=q.trim().toLowerCase();
  var now=todayD();
  var fb=document.getElementById("filterBar");
  var fl=document.getElementById("filterLabel");
  if(currentFilter){fb.style.display="flex";fl.textContent=currentFilter+" 고객";}
  else if(currentCareFilter){fb.style.display="flex";fl.textContent=CARE_LABELS[currentCareFilter]||currentCareFilter;}
  else{fb.style.display="none";}

  var yfBar=document.getElementById("yearFilterBar");
  if(yfBar){
    if(currentFilter==="출고"){
      var nowKST2=new Date(new Date().getTime()+9*3600000);
      var thisYr2=nowKST2.getUTCFullYear();
      var yrBtns='<button class="yr-btn'+(currentDeliveryYear===""?" active":"")+'" onclick="setDeliveryYear(\'\')">전체</button>';
      for(var yr=thisYr2;yr>=2017;yr--)
        yrBtns+='<button class="yr-btn'+(currentDeliveryYear===String(yr)?" active":"")+'" onclick="setDeliveryYear(\''+yr+'\')">'+yr+'</button>';
      yfBar.innerHTML=yrBtns;yfBar.classList.add("visible");
    } else {yfBar.classList.remove("visible");yfBar.innerHTML="";}
  }

  var filtered=list.filter(function(c){
    if(currentFilter&&c.status!==currentFilter)return false;
    if(currentCareFilter){
      if(currentCareFilter==="birthday7"){var bd=bdayDiff(c.birthDate);if(bd===null||bd<0||bd>7)return false;}
      else if(currentCareFilter==="insurance30"){var iD=dFrom(c.insuranceExpireDate);if(iD===null||iD<0||iD>30)return false;}
      else if(currentCareFilter==="maintenance"){if(!isMaintenanceDue(c))return false;}
      else if(currentCareFilter==="finance90"){var fD=dFrom(c.financeEndDate);if(fD===null||fD<0||fD>90)return false;}
      else if(currentCareFilter==="delivery1yr"){var del=parseD(c.deliveryDate);if(!del||Math.round((now-del)/86400000)<365)return false;}
      else if(currentCareFilter==="delivery3yr"){var del3=parseD(c.deliveryDate);if(!del3||Math.round((now-del3)/86400000)<1095)return false;}
    }
    if(currentDeliveryYear){var dyYear=String(c.deliveryDate||c.deliveryMonth||"").trim().substring(0,4);if(dyYear!==currentDeliveryYear)return false;}
    if(q){var nm=(c.customerName||"").toLowerCase();var ph=normP(c.phone);if(nm.indexOf(q)<0&&ph.indexOf(q.replace(/\D/g,""))<0)return false;}
    return true;
  });

  filtered.sort(function(a,b){
    var ta=a._updatedAt||a._savedAt||"";var tb=b._updatedAt||b._savedAt||"";
    return tb.localeCompare(ta);
  });
  renderedCustomers=filtered;
  document.getElementById("list-count").textContent=filtered.length+"명";
  var con=document.getElementById("listContainer");
  if(!filtered.length){
    con.innerHTML='<div class="empty-state"><div class="empty-icon">🔍</div><p>'+(currentFilter||currentCareFilter||q?"조건에 맞는 고객이 없습니다":"아직 등록된 고객이 없습니다")+'</p></div>';
    return;
  }
  con.innerHTML=filtered.map(function(c,i){return buildCard(c,i);}).join("");
}

function buildCard(c,idx){
  var sd=c.status||"—";
  var model=(c.modelName||"")+(c.detailModel?" · "+c.detailModel:"");
  var isUpd=!!c._updatedAt;
  var did="det-"+idx,cid="care-"+idx;
  var hasCare=c.birthDate||c.insuranceStartDate||c.insuranceExpireDate||
    c.inspectionDueDate||c.financeStartDate||c.financeEndDate||
    c.contractDate||c.deliveryDate||c.deliveryMonth||c.lastServiceDate||c.nextServiceDate;
  var html='<div class="cust-card'+(isUpd?" updated":"")+'">';
  html+='<div class="card-top">'+
    '<div>'+
      '<div class="cust-name">'+esc(c.customerName)+'</div>'+
      (c.companyName?'<div class="cust-sub">'+esc(c.companyName)+'</div>':'')+
      '<div class="cust-sub">'+(isUpd?"수정: "+esc(c._updatedAt):"등록: "+esc(c._savedAt||""))+'</div>'+
    '</div>'+
    '<div style="display:flex;flex-direction:column;align-items:flex-end;gap:7px;">'+
      '<span class="badge '+sc(c.status)+'">'+esc(sd)+'</span>'+
      '<button class="edit-btn" onclick="openEditByIdx('+idx+')">✏️ 수정</button>'+
    '</div>'+
  '</div>';
  html+='<div class="chip-row">';
  if(c.phone)html+='<a class="tel-link" href="tel:'+esc(c.phone)+'">📞 '+esc(c.phone)+'</a>';
  if(model)html+='<span class="chip">'+esc(model)+'</span>';
  if(c.region)html+='<span class="chip">📍'+esc(c.region)+'</span>';
  if(c.nextContact)html+='<span class="chip">📅 '+esc(c.nextContact)+'</span>';
  if(c.leadSource)html+='<span class="chip">'+esc(c.leadSource)+'</span>';

  if(currentCareFilter==="birthday7"&&c.birthDate){var bd=bdayDiff(c.birthDate);html+='<span class="chip" style="background:#f0e5ff;color:var(--purple);font-weight:700;">🎂 생일 '+esc(c.birthDate)+(bd!==null?' D-'+bd:'')+'</span>';}
  else if(currentCareFilter==="insurance30"&&c.insuranceExpireDate){var iDc=dFrom(c.insuranceExpireDate);html+='<span class="chip" style="background:#fff0e0;color:var(--orange);font-weight:700;">🛡 보험만기 '+esc(c.insuranceExpireDate)+ddTag(iDc,30)+'</span>';}
  else if(currentCareFilter==="maintenance"){
    var inDc=dFrom(c.inspectionDueDate);if(inDc!==null&&inDc>=0&&inDc<=30)html+='<span class="chip" style="background:#fff0e0;color:var(--orange);font-weight:700;">🔧 자동차검사 '+esc(c.inspectionDueDate)+ddTag(inDc,30)+'</span>';
    var sDc=dFrom(c.nextServiceDate);if(sDc!==null&&sDc>=0&&sDc<=30)html+='<span class="chip" style="background:#fff0e0;color:var(--orange);font-weight:700;">🔧 정기점검 '+esc(c.nextServiceDate)+ddTag(sDc,30)+'</span>';
    var curKm=parseMileage(c.currentMileage);var nxtKm=parseMileage(c.nextServiceMileage);
    if(curKm!==null&&nxtKm!==null&&(nxtKm-curKm)<=1000&&(nxtKm-curKm)>=0)html+='<span class="chip" style="background:#fde8e8;color:var(--danger);font-weight:700;">⚠ 잔여 '+(nxtKm-curKm).toLocaleString()+'km</span>';
  }
  else if(currentCareFilter==="finance90"&&c.financeEndDate){var fDc=dFrom(c.financeEndDate);html+='<span class="chip" style="background:#fff0e0;color:var(--orange);font-weight:700;">💳 금융종료 '+esc(c.financeEndDate)+ddTag(fDc,90)+'</span>';}
  else if((currentCareFilter==="delivery1yr"||currentCareFilter==="delivery3yr")&&c.deliveryDate){var delD=parseD(c.deliveryDate);var elp2=delD?Math.round((todayD()-delD)/86400000):0;html+='<span class="chip" style="background:#e3f7ec;color:var(--green);font-weight:700;">🚗 출고 '+esc(c.deliveryDate)+' ('+Math.floor(elp2/365)+'년 경과)</span>';}
  html+='</div>';

  html+='<button class="tog-btn" onclick="togDet(this,\''+did+'\')" aria-expanded="false">상세정보 보기 <span class="tog-arr">▼</span></button>';
  html+='<div class="detail-body" id="'+did+'">';
  html+='<div class="d-sec">구매 정보</div>';
  if(c.purchaseMethod)html+=dr("구매방식",c.purchaseMethod);
  if(c.budget)html+=dr("구매예산",c.budget);
  if(c.purchaseTiming)html+=dr("구매예정시기",c.purchaseTiming);
  html+='<div class="d-sec">상담</div>';
  if(c.consultation)html+='<div class="d-row"><span class="d-key">상담내용</span><span class="d-val">'+esc(c.consultation)+'</span></div>';
  if(c.tags)html+=dr("태그",c.tags);
  if(hasCare){
    html+='<button class="care-tog tog-btn" onclick="togCare(this,\''+cid+'\')" aria-expanded="false">🚗 차량관리 / 점검 정보 보기 <span class="tog-arr">▼</span></button>';
    html+='<div class="care-detail" id="'+cid+'">';
    html+='<div class="d-sec care">차량관리 / 금융관리</div>';
    if(c.birthDate){var bd2=bdayDiff(c.birthDate);var bdLbl=esc(c.birthDate)+(bd2!==null&&bd2<=7?'<span class="dd urg"> D-'+bd2+'</span>':"");html+='<div class="d-row"><span class="d-key">생년월일</span><span class="d-val">'+bdLbl+'</span></div>';}
    if(c.insuranceStartDate)html+=dr("보험가입일",c.insuranceStartDate);
    if(c.insuranceExpireDate){var iD2=dFrom(c.insuranceExpireDate);html+='<div class="d-row"><span class="d-key">보험만기일</span><span class="d-val">'+esc(c.insuranceExpireDate)+ddTag(iD2,30)+'</span></div>';}
    if(c.inspectionDueDate){var inD2=dFrom(c.inspectionDueDate);html+='<div class="d-row"><span class="d-key">자동차검사일</span><span class="d-val">'+esc(c.inspectionDueDate)+ddTag(inD2,30)+'</span></div>';}
    if(c.financeStartDate)html+=dr("금융시작일",c.financeStartDate);
    if(c.financeEndDate){var fD2=dFrom(c.financeEndDate);html+='<div class="d-row"><span class="d-key">금융종료일</span><span class="d-val">'+esc(c.financeEndDate)+ddTag(fD2,90)+'</span></div>';}
    html+='<div class="d-sec care">계약 / 출고</div>';
    if(c.contractDate)html+=dr("계약일",c.contractDate);
    if(c.deliveryDate){var del2=parseD(c.deliveryDate);var elp=del2?Math.round((todayD()-del2)/86400000):0;var delLbl=esc(c.deliveryDate);if(elp>=1095)delLbl+='<span class="dd urg"> 3년↑</span>';else if(elp>=365)delLbl+='<span class="dd warn"> 1년↑</span>';html+='<div class="d-row"><span class="d-key">출고일</span><span class="d-val">'+delLbl+'</span></div>';}
    if(c.deliveryMonth)html+=dr("출고월",c.deliveryMonth);
    var hasService=c.lastServiceDate||c.lastServiceMileage||c.currentMileage||c.nextServiceDate||c.nextServiceMileage;
    if(hasService){
      html+='<div class="d-sec care">🔧 벤츠 정기점검</div>';
      if(c.lastServiceDate)html+=dr("최근점검일",c.lastServiceDate);
      if(c.lastServiceMileage)html+=dr("최근점검km",c.lastServiceMileage);
      if(c.currentMileageDate)html+=dr("주행확인일",c.currentMileageDate);
      if(c.currentMileage)html+=dr("현재주행km",c.currentMileage);
      if(c.nextServiceDate){var sD2=dFrom(c.nextServiceDate);html+='<div class="d-row"><span class="d-key">다음점검일</span><span class="d-val">'+esc(c.nextServiceDate)+ddTag(sD2,30)+'</span></div>';}
      if(c.nextServiceMileage){var curK=parseMileage(c.currentMileage);var nxtK=parseMileage(c.nextServiceMileage);var remKm=(curK!==null&&nxtK!==null)?(nxtK-curK):null;var remLbl=esc(c.nextServiceMileage);if(remKm!==null&&remKm>=0)remLbl+=' <span class="dd '+(remKm<=1000?"urg":"warn")+'">잔여 '+remKm.toLocaleString()+'km</span>';html+='<div class="d-row"><span class="d-key">다음점검km</span><span class="d-val">'+remLbl+'</span></div>';}
    }
    html+='</div>';
  }
  html+='</div></div>';
  return html;
}
function dr(k,v){if(!v)return"";return'<div class="d-row"><span class="d-key">'+k+'</span><span class="d-val">'+esc(v)+'</span></div>';}
function togDet(btn,id){
  var d=document.getElementById(id);var open=d.classList.contains("open");
  d.classList.toggle("open",!open);btn.classList.toggle("open",!open);
  btn.querySelector(".tog-arr").textContent=open?"▼":"▲";
  btn.childNodes[0].textContent=open?"상세정보 보기 ":"상세정보 닫기 ";
}
function togCare(btn,id){
  var d=document.getElementById(id);var open=d.classList.contains("open");
  d.classList.toggle("open",!open);btn.classList.toggle("open",!open);
  btn.querySelector(".tog-arr").textContent=open?"▼":"▲";
  btn.childNodes[0].textContent=open?"🚗 차량관리 / 점검 정보 보기 ":"🚗 차량관리 / 점검 정보 닫기 ";
}

// ── 전화번호 자동 하이픈 ──────────────────────────────────────
function fmtP(el){
  var v=el.value.replace(/\D/g,"");
  if(v.length<=3)el.value=v;
  else if(v.length<=7)el.value=v.slice(0,3)+"-"+v.slice(3);
  else el.value=v.slice(0,3)+"-"+v.slice(3,7)+"-"+v.slice(7,11);
}
document.getElementById("phone").addEventListener("input",function(){fmtP(this);});
document.getElementById("u-phone").addEventListener("input",function(){fmtP(this);});

// ── 추가상담 전화번호 → 프리뷰 ──────────────────────────────
var prevTimer=null;
function onUPhoneInput(el){
  fmtP(el);
  clearTimeout(prevTimer);
  prevTimer=setTimeout(function(){
    var np=normP(el.value);
    var pb=document.getElementById("preview-box");
    var nf=document.getElementById("nofound");
    pb.classList.remove("show");nf.classList.remove("show");
    if(np.length<10)return;
    var r=findC(el.value);
    if(r){
      var c=r.c,sd=c.status||"—";
      document.getElementById("prev-name").textContent=c.customerName||"—";
      document.getElementById("prev-meta").textContent=(c.modelName?c.modelName+(c.detailModel?" · "+c.detailModel:""):"모델미정")+" | "+(c.leadSource||"—");
      document.getElementById("prev-status").innerHTML='<span class="badge '+sc(c.status)+'">'+esc(sd)+'</span>';
      var ne=document.getElementById("u-name");if(!ne.value)ne.value=c.customerName||"";
      pb.classList.add("show");
    } else if(np.length>=11){nf.classList.add("show");}
  },400);
}

// ── 메시지 ───────────────────────────────────────────────────
function showMsg(id,type,text){
  var el=document.getElementById(id);if(!el)return;
  el.className="st-msg "+type;el.textContent=text;
  el.scrollIntoView({behavior:"smooth",block:"nearest"});
}
function hideMsg(id){var el=document.getElementById(id);if(!el)return;el.className="st-msg";el.textContent="";}

// ── 신규 고객 저장 ───────────────────────────────────────────
function submitNew(){
  hideMsg("msg-new");
  autoDeliveryMonth();
  var name=document.getElementById("customerName").value.trim();
  var phone=document.getElementById("phone").value.trim();
  if(!name){showMsg("msg-new","err","⚠ 고객명을 입력해주세요.");return;}
  if(!phone){showMsg("msg-new","err","⚠ 연락처를 입력해주세요.");return;}
  var deliveryDate=document.getElementById("deliveryDate").value;
  var deliveryMonth=deliveryDate&&deliveryDate.length>=7?deliveryDate.substring(0,7):"";
  var data={
    _savedAt:getNow(),
    customerName:name,companyName:document.getElementById("companyName").value.trim(),
    phone:phone,region:document.getElementById("region").value.trim(),
    birthDate:document.getElementById("birthDate").value,
    modelName:document.getElementById("modelName").value,
    detailModel:getDetailModelValue("detailModel","detailCustom"),
    purchaseMethod:document.getElementById("purchaseMethod").value,
    budget:document.getElementById("budget").value,
    purchaseTiming:document.getElementById("purchaseTiming").value.trim(),
    leadSource:document.getElementById("leadSource").value,
    status:document.getElementById("status").value,
    consultation:document.getElementById("consultation").value.trim(),
    nextContact:document.getElementById("nextContact").value,
    insuranceStartDate:document.getElementById("insuranceStartDate").value,
    insuranceExpireDate:document.getElementById("insuranceExpireDate").value,
    inspectionDueDate:document.getElementById("inspectionDueDate").value,
    financeStartDate:document.getElementById("financeStartDate").value,
    financeEndDate:document.getElementById("financeEndDate").value,
    contractDate:document.getElementById("contractDate").value,
    deliveryDate:deliveryDate,deliveryMonth:deliveryMonth,
    tags:document.getElementById("tags").value.trim(),
    lastServiceDate:document.getElementById("lastServiceDate").value,
    lastServiceMileage:document.getElementById("lastServiceMileage").value,
    currentMileageDate:document.getElementById("currentMileageDate").value,
    currentMileage:document.getElementById("currentMileage").value,
    nextServiceDate:document.getElementById("nextServiceDate").value,
    nextServiceMileage:document.getElementById("nextServiceMileage").value
  };
  var list=load();
  var np=normP(phone),idx=-1;
  for(var i=0;i<list.length;i++){if(normP(list[i].phone)===np){idx=i;break;}}
  if(idx>=0){list[idx]=Object.assign(list[idx],data);}else{list.push(data);}
  save(list);updateDash();
  showMsg("msg-new","ok","✅ 저장되었습니다.");
  resetNewForm();
}
function resetNewForm(){
  ["customerName","companyName","phone","region","birthDate","budget","purchaseTiming",
   "consultation","nextContact","insuranceStartDate","insuranceExpireDate","inspectionDueDate",
   "financeStartDate","financeEndDate","contractDate","deliveryDate","tags",
   "lastServiceDate","lastServiceMileage","currentMileageDate","currentMileage"].forEach(function(id){
    var el=document.getElementById(id);if(el)el.value="";
  });
  document.getElementById("deliveryMonth").value="";
  document.getElementById("nextServiceDate").value="";
  document.getElementById("nextServiceMileage").value="";
  document.getElementById("modelName").value="";
  var dm=document.getElementById("detailModel");
  dm.innerHTML='<option value="">— 모델을 먼저 선택하세요 —</option>';dm.disabled=true;
  var dcInp=document.getElementById("detailCustom");var dcWrap=document.getElementById("detailCustomWrap");
  if(dcInp)dcInp.value="";if(dcWrap)dcWrap.classList.remove("visible");
  ["purchaseMethod","leadSource","status"].forEach(function(id){var el=document.getElementById(id);if(el)el.selectedIndex=0;});
}

// ── 추가상담 저장 ────────────────────────────────────────────
function submitUpdate(){
  hideMsg("msg-update");
  var phone=document.getElementById("u-phone").value.trim();
  var addC=document.getElementById("addConsultation").value.trim();
  if(!phone){showMsg("msg-update","err","⚠ 연락처를 입력해주세요.");return;}
  if(!addC){showMsg("msg-update","err","⚠ 추가상담내용을 입력해주세요.");return;}
  var r=findC(phone);
  if(!r){showMsg("msg-update","err","⚠ 기존 고객을 찾을 수 없습니다.");return;}
  var now=getNow();
  var entry=getTs()+"\n"+addC;
  var list=load();
  var c=list[r.idx];
  var prev=c.consultation||"";
  c.consultation=prev?prev+"\n\n"+entry:entry;
  var st=document.getElementById("u-status").value;
  var nc=document.getElementById("u-next").value;
  var md=document.getElementById("u-mileageDate").value;
  var mm=document.getElementById("u-mileage").value;
  if(st)c.status=st;
  if(nc)c.nextContact=nc;
  if(md)c.currentMileageDate=md;
  if(mm)c.currentMileage=mm;
  c._updatedAt=now;
  save(list);updateDash();
  showMsg("msg-update","ok","✅ 추가상담이 저장되었습니다.");
  ["u-name","u-phone","u-next","addConsultation","u-mileageDate","u-mileage"].forEach(function(id){var el=document.getElementById(id);if(el)el.value="";});
  document.getElementById("u-status").selectedIndex=0;
  document.getElementById("preview-box").classList.remove("show");
  document.getElementById("nofound").classList.remove("show");
}

// ── 수정 모달 ────────────────────────────────────────────────
var _editIdx=-1;
function openEditByIdx(idx){
  _editIdx=idx;
  var c=renderedCustomers[idx];if(!c){alert("고객 정보를 찾을 수 없습니다.");return;}
  function sv(id,v){var el=document.getElementById(id);if(el)el.value=v||"";}
  sv("e-customerName",c.customerName);sv("e-companyName",c.companyName);
  sv("e-phone",c.phone);sv("e-region",c.region);sv("e-birthDate",c.birthDate);
  sv("e-purchaseMethod",c.purchaseMethod);sv("e-budget",c.budget);
  sv("e-purchaseTiming",c.purchaseTiming);sv("e-leadSource",c.leadSource);
  sv("e-status",c.status);sv("e-consultation",c.consultation);
  sv("e-nextContact",c.nextContact);sv("e-insuranceStartDate",c.insuranceStartDate);
  sv("e-insuranceExpireDate",c.insuranceExpireDate);sv("e-inspectionDueDate",c.inspectionDueDate);
  sv("e-financeStartDate",c.financeStartDate);sv("e-financeEndDate",c.financeEndDate);
  sv("e-contractDate",c.contractDate);sv("e-deliveryDate",c.deliveryDate);
  var fm=c.deliveryMonth||"";if(!fm&&c.deliveryDate&&c.deliveryDate.length>=7)fm=c.deliveryDate.substring(0,7);
  sv("e-deliveryMonth",fm);sv("e-tags",c.tags);
  sv("e-lastServiceDate",c.lastServiceDate);sv("e-lastServiceMileage",c.lastServiceMileage);
  sv("e-currentMileageDate",c.currentMileageDate);sv("e-currentMileage",c.currentMileage);
  sv("e-nextServiceDate",c.nextServiceDate);sv("e-nextServiceMileage",c.nextServiceMileage);
  var mSel=document.getElementById("e-modelName");
  if(mSel){
    mSel.value=c.modelName||"";onEditModelChange();
    setTimeout(function(){
      var dm=document.getElementById("e-detailModel");
      var dcWrap=document.getElementById("e-detailCustomWrap");
      var dcInp=document.getElementById("e-detailCustom");
      var savedVal=c.detailModel||"";
      if(!savedVal)return;
      var found=false;
      if(dm)for(var oi=0;oi<dm.options.length;oi++){if(dm.options[oi].value===savedVal){found=true;break;}}
      if(found){if(dm)dm.value=savedVal;if(dcWrap)dcWrap.classList.remove("visible");if(dcInp)dcInp.value="";}
      else{if(dm)dm.value="__custom__";if(dcWrap)dcWrap.classList.add("visible");if(dcInp)dcInp.value=savedVal;}
    },0);
  }
  var msg=document.getElementById("edit-save-msg");
  if(msg){msg.className="st-msg edit-save-msg";msg.textContent="";}
  document.getElementById("editOverlay").classList.add("open");
  document.body.style.overflow="hidden";
  document.getElementById("editOverlay").scrollTop=0;
}
function closeEdit(){
  document.getElementById("editOverlay").classList.remove("open");
  document.body.style.overflow="";
}
function editOverlayClick(e){if(e.target===document.getElementById("editOverlay"))closeEdit();}
function submitEdit(){
  var msgEl=document.getElementById("edit-save-msg");
  var btn=document.getElementById("editSaveBtn");
  if(msgEl){msgEl.className="st-msg edit-save-msg";msgEl.textContent="";}
  var eName=document.getElementById("e-customerName").value.trim();
  var ePhone=document.getElementById("e-phone").value.trim();
  if(!eName){if(msgEl){msgEl.className="st-msg edit-save-msg err";msgEl.textContent="⚠ 고객명을 입력해주세요.";}return;}
  if(!ePhone){if(msgEl){msgEl.className="st-msg edit-save-msg err";msgEl.textContent="⚠ 연락처를 입력해주세요.";}return;}
  autoEditDeliveryMonth();
  var deliveryDate=document.getElementById("e-deliveryDate").value;
  var deliveryMonth=deliveryDate&&deliveryDate.length>=7?deliveryDate.substring(0,7):"";

  // 원본 고객을 렌더된 인덱스의 연락처로 찾기
  var origPhone=renderedCustomers[_editIdx]?normP(renderedCustomers[_editIdx].phone):"";
  var list=load();
  var targetIdx=-1;
  if(origPhone){for(var i=0;i<list.length;i++){if(normP(list[i].phone)===origPhone){targetIdx=i;break;}}}
  // 못 찾으면 새 연락처로도 검색
  if(targetIdx<0){var np2=normP(ePhone);for(var j=0;j<list.length;j++){if(normP(list[j].phone)===np2){targetIdx=j;break;}}}
  var now=getNow();
  var updated={
    _savedAt:targetIdx>=0?(list[targetIdx]._savedAt||now):now,_updatedAt:now,
    customerName:eName,companyName:document.getElementById("e-companyName").value.trim(),
    phone:ePhone,region:document.getElementById("e-region").value.trim(),
    birthDate:document.getElementById("e-birthDate").value,
    modelName:document.getElementById("e-modelName").value,
    detailModel:getDetailModelValue("e-detailModel","e-detailCustom"),
    purchaseMethod:document.getElementById("e-purchaseMethod").value,
    budget:document.getElementById("e-budget").value,
    purchaseTiming:document.getElementById("e-purchaseTiming").value.trim(),
    leadSource:document.getElementById("e-leadSource").value,
    status:document.getElementById("e-status").value,
    consultation:document.getElementById("e-consultation").value.trim(),
    nextContact:document.getElementById("e-nextContact").value,
    insuranceStartDate:document.getElementById("e-insuranceStartDate").value,
    insuranceExpireDate:document.getElementById("e-insuranceExpireDate").value,
    inspectionDueDate:document.getElementById("e-inspectionDueDate").value,
    financeStartDate:document.getElementById("e-financeStartDate").value,
    financeEndDate:document.getElementById("e-financeEndDate").value,
    contractDate:document.getElementById("e-contractDate").value,
    deliveryDate:deliveryDate,deliveryMonth:deliveryMonth,
    tags:document.getElementById("e-tags").value.trim(),
    lastServiceDate:document.getElementById("e-lastServiceDate").value,
    lastServiceMileage:document.getElementById("e-lastServiceMileage").value,
    currentMileageDate:document.getElementById("e-currentMileageDate").value,
    currentMileage:document.getElementById("e-currentMileage").value,
    nextServiceDate:document.getElementById("e-nextServiceDate").value,
    nextServiceMileage:document.getElementById("e-nextServiceMileage").value
  };
  if(targetIdx>=0){list[targetIdx]=updated;}else{list.push(updated);}
  save(list);updateDash();renderHomeContacts();renderList();
  if(msgEl){msgEl.className="st-msg edit-save-msg ok";msgEl.textContent="✅ 수정이 저장되었습니다.";}
  setTimeout(function(){closeEdit();},800);
}

// ── 대시보드 고객 상세 ────────────────────────────────────────
function openCustomerDetailFromDashboard(phone,name){
  var list=load();
  var np=(phone||"").replace(/\D/g,"");
  var customer=null;
  if(np)customer=list.find(function(c){return(c.phone||"").replace(/\D/g,"")===np;});
  if(!customer&&name)customer=list.find(function(c){return(c.customerName||"")===name;});
  if(!customer){alert("고객 정보를 찾을 수 없습니다.");return;}
  var body=document.getElementById("custDetailBody");
  var rows=[["고객명",customer.customerName],["연락처",customer.phone],["법인명",customer.companyName],
    ["거주지역",customer.region],["모델명",customer.modelName],["세부모델",customer.detailModel],
    ["현재상태",customer.status],["다음연락일",customer.nextContact],
    ["보험만기일",customer.insuranceExpireDate],["자동차검사일",customer.inspectionDueDate],
    ["금융종료일",customer.financeEndDate],["출고일",customer.deliveryDate]];
  var html='<table style="width:100%;border-collapse:collapse;font-size:14px;">';
  rows.forEach(function(r){
    if(!r[1]&&r[1]!==0)return;
    html+='<tr style="border-bottom:1px solid var(--border);"><td style="padding:9px 6px 9px 0;color:var(--text-dim);white-space:nowrap;width:90px;vertical-align:top;font-size:12px;">'+esc(r[0])+'</td><td style="padding:9px 0 9px 8px;color:var(--black);font-weight:600;"><strong>'+esc(r[1])+'</strong></td></tr>';
  });
  html+='</table>';
  var raw=(customer.consultation||"").trim();
  if(raw){
    var entries=raw.split(/\n(?=\[\d{4}-\d{2}-\d{2})/);
    html+='<div style="margin-top:18px;"><div style="font-size:13px;font-weight:800;color:var(--black);margin-bottom:10px;">💬 상담 이력 <span style="font-size:11px;background:var(--border);padding:2px 8px;border-radius:20px;">'+entries.length+'건</span></div>';
    entries.slice().reverse().forEach(function(entry,i){
      var lines=entry.trim().split("\n");
      var header=lines[0]||"";var body_text=lines.slice(1).join("\n").trim();
      var dateMatch=header.match(/\[(\d{4}-\d{2}-\d{2}\s+\d{2}:\d{2})/);
      var dateLabel=dateMatch?dateMatch[1]:header.replace(/[\[\]]/g,"");
      var isFirst=(i===0);
      html+='<div style="background:'+(isFirst?'#fdf6e3':'#fff')+';border:1px solid '+(isFirst?'var(--gold)':'var(--border)')+';border-radius:12px;padding:12px 14px;margin-bottom:8px;">'+
        '<div style="font-size:11px;font-weight:700;color:'+(isFirst?'var(--gold-dk)':'var(--text-dim)')+';margin-bottom:6px;">📅 '+esc(dateLabel)+(isFirst?' <span style="font-size:10px;padding:1px 6px;background:var(--gold);color:#fff;border-radius:10px;margin-left:4px;">최신</span>':'')+'</div>'+
        '<div style="font-size:13px;color:var(--black);line-height:1.6;white-space:pre-wrap;">'+esc(body_text||header)+'</div></div>';
    });
    html+='</div>';
  }
  body.innerHTML=html;
  var ov=document.getElementById("custDetailOverlay");
  ov.classList.add("open");document.body.style.overflow="hidden";ov.scrollTop=0;
}
function closeCustDetail(){
  var ov=document.getElementById("custDetailOverlay");
  if(ov)ov.classList.remove("open");
  document.body.style.overflow="";
}
function custDetailOverlayClick(e){if(e.target===document.getElementById("custDetailOverlay"))closeCustDetail();}

// ── 데이터 내보내기 / 가져오기 ────────────────────────────────
function exportData(){
  var list=load();
  var blob=new Blob([JSON.stringify(list,null,2)],{type:"application/json"});
  var url=URL.createObjectURL(blob);
  var a=document.createElement("a");
  var d=new Date();
  var fname="gy_crm_"+d.getFullYear()+String(d.getMonth()+1).padStart(2,"0")+String(d.getDate()).padStart(2,"0")+".json";
  a.href=url;a.download=fname;a.click();
  URL.revokeObjectURL(url);
}
function importData(event){
  var file=event.target.files[0];
  if(!file)return;
  var reader=new FileReader();
  reader.onload=function(e){
    try{
      var data=JSON.parse(e.target.result);
      if(!Array.isArray(data)){alert("올바른 데이터 파일이 아닙니다.");return;}
      if(!confirm("가져오기 시 기존 데이터와 병합됩니다. (같은 연락처는 덮어씁니다)\n계속하시겠습니까?"))return;
      var list=load();
      data.forEach(function(newC){
        var np=normP(newC.phone);var found=false;
        for(var i=0;i<list.length;i++){if(normP(list[i].phone)===np){list[i]=newC;found=true;break;}}
        if(!found)list.push(newC);
      });
      save(list);updateDash();renderList();
      alert("✅ "+data.length+"건을 가져왔습니다.");
    }catch(err){alert("파일을 읽는 중 오류가 발생했습니다.");}
  };
  reader.readAsText(file);
  event.target.value="";
}
function clearAllData(){
  if(!confirm("⚠ 전체 고객 데이터를 삭제합니다.\n이 작업은 되돌릴 수 없습니다!\n\n정말 삭제하시겠습니까?"))return;
  if(!confirm("마지막 확인: 정말로 모든 데이터를 삭제하시겠습니까?"))return;
  localStorage.removeItem(LS_KEY);
  updateDash();renderList();renderHomeContacts();
  alert("✅ 모든 데이터가 삭제되었습니다.");
}

// ── 데이터분석 ───────────────────────────────────────────────
var _chartInstances={};
var _anYear="";
var AN_COLORS=["#1a1a2e","#16213e","#0f3460","#533483","#e94560","#1d7a8a","#2d6a4f","#b5838d","#6d6875","#c9a227","#457b9d","#a8dadc","#e63946","#606c38","#dda15e"];
function anColor(i){return AN_COLORS[i%AN_COLORS.length];}
function makeChart(id,config){
  if(_chartInstances[id]){try{_chartInstances[id].destroy();}catch(e){}delete _chartInstances[id];}
  var canvas=document.getElementById(id);if(!canvas)return;
  _chartInstances[id]=new Chart(canvas.getContext("2d"),config);
}
function barOpts(){
  return{responsive:true,maintainAspectRatio:true,aspectRatio:1.6,
    plugins:{legend:{display:false},tooltip:{callbacks:{label:function(ctx){return" "+ctx.parsed.y+"대";}}}},
    scales:{x:{grid:{color:"rgba(0,0,0,.04)"},ticks:{font:{size:11},color:"#555"}},
      y:{grid:{color:"rgba(0,0,0,.06)"},ticks:{font:{size:11},color:"#555",stepSize:1,callback:function(v){return Number.isInteger(v)?v:""}},beginAtZero:true}},
    animation:{duration:400}};
}
function isConverted(status){return status==="계약"||status==="출고";}
var PIPELINE_STAGES=["신규문의","A급가망","상담중","계약유력","계약","출고"];
var PIPELINE_COLORS={"신규문의":"#6d6875","A급가망":"#457b9d","상담중":"#c9a227","계약유력":"#e94560","계약":"#2d6a4f","출고":"#1a5aa0"};
function renderPipelineChart(list){
  var el=document.getElementById("pipeline-chart");if(!el)return;
  var counts={};PIPELINE_STAGES.forEach(function(s){counts[s]=0;});
  list.forEach(function(c){var s=c.status||"";if(counts[s]!==undefined)counts[s]++;});
  var max=Math.max.apply(null,PIPELINE_STAGES.map(function(s){return counts[s];}));if(!max)max=1;
  var html='<div style="display:flex;flex-direction:column;gap:8px;padding:4px 0;">';
  PIPELINE_STAGES.forEach(function(stage){
    var cnt=counts[stage];var pct=Math.round(cnt/max*100);var color=PIPELINE_COLORS[stage]||"#888";
    html+='<div style="display:flex;align-items:center;gap:10px;">'+
      '<div style="width:70px;text-align:right;font-size:13px;font-weight:700;color:var(--black);flex-shrink:0;">'+stage+'</div>'+
      '<div style="flex:1;background:var(--border);border-radius:6px;overflow:hidden;height:28px;">'+
        '<div style="height:100%;width:'+pct+'%;background:'+color+';border-radius:6px;display:flex;align-items:center;justify-content:flex-end;padding-right:8px;min-width:'+(cnt>0?'32px':'0')+';">'+
          (cnt>0?'<span style="font-size:12px;font-weight:800;color:#fff;">'+cnt+'명</span>':'')+
        '</div></div>'+
      '<div style="width:36px;text-align:left;font-size:13px;font-weight:800;color:'+color+';flex-shrink:0;">'+cnt+'명</div>'+
    '</div>';
  });
  html+='</div><div style="margin-top:10px;font-size:11px;color:var(--text-dim);text-align:right;">전체 고객 '+list.length+'명 (전체 기간)</div>';
  el.innerHTML=html;
}
function renderLeadConvChart(list,yrStr){
  var el=document.getElementById("lead-conv-chart");if(!el)return;
  var filtered=yrStr?list.filter(function(c){var reg=normalizeDateStr(c._savedAt||"");return reg.substring(0,4)===yrStr;}):list;
  var leadMap={};
  filtered.forEach(function(c){var k=c.leadSource||"미기재";if(!leadMap[k])leadMap[k]={total:0,conv:0};leadMap[k].total++;if(isConverted(c.status))leadMap[k].conv++;});
  var keys=Object.keys(leadMap).filter(function(k){return leadMap[k].total>0;});
  keys.sort(function(a,b){return(leadMap[b].conv/leadMap[b].total)-(leadMap[a].conv/leadMap[a].total);});
  if(!keys.length){el.innerHTML='<div style="padding:20px;text-align:center;color:var(--text-lt);font-size:13px;">데이터 없음</div>';return;}
  var html='<div style="display:flex;flex-direction:column;gap:10px;padding:4px 0;">';
  keys.forEach(function(k){
    var d=leadMap[k];var pct=d.total>0?Math.round(d.conv/d.total*100):0;
    var color=pct>=30?"#2d6a4f":pct>=15?"#c9a227":"#e94560";
    html+='<div><div style="display:flex;justify-content:space-between;align-items:baseline;margin-bottom:4px;"><span style="font-size:13px;font-weight:700;color:var(--black);">'+esc(k)+'</span><span style="font-size:12px;color:var(--text-dim);">'+d.total+'명 유입 · '+d.conv+'명 계약</span></div>'+
      '<div style="display:flex;align-items:center;gap:8px;"><div style="flex:1;background:var(--border);border-radius:6px;overflow:hidden;height:22px;"><div style="height:100%;width:'+Math.min(pct,100)+'%;background:'+color+';border-radius:6px;"></div></div><div style="width:46px;text-align:right;font-size:14px;font-weight:800;color:'+color+';">'+pct+'%</div></div></div>';
  });
  html+='</div><div style="margin-top:8px;font-size:11px;color:var(--text-dim);">전환율 = (계약+출고) ÷ 유입고객 × 100</div>';
  el.innerHTML=html;
}
function renderModelConvChart(list,yrStr){
  var el=document.getElementById("model-conv-chart");if(!el)return;
  var filtered=yrStr?list.filter(function(c){var reg=normalizeDateStr(c._savedAt||"");return reg.substring(0,4)===yrStr;}):list;
  var modelMap={};
  filtered.forEach(function(c){var k=(c.modelName||"미기재").trim();if(!k)k="미기재";if(!modelMap[k])modelMap[k]={total:0,conv:0};modelMap[k].total++;if(isConverted(c.status))modelMap[k].conv++;});
  var keys=Object.keys(modelMap).filter(function(k){return modelMap[k].total>0;});
  keys.sort(function(a,b){return(modelMap[b].conv/modelMap[b].total)-(modelMap[a].conv/modelMap[a].total);});
  if(!keys.length){el.innerHTML='<div style="padding:20px;text-align:center;color:var(--text-lt);font-size:13px;">데이터 없음</div>';return;}
  var html='<div style="display:flex;flex-direction:column;gap:10px;padding:4px 0;">';
  keys.forEach(function(k,i){
    var d=modelMap[k];var pct=d.total>0?Math.round(d.conv/d.total*100):0;var color=anColor(i);
    html+='<div><div style="display:flex;justify-content:space-between;align-items:baseline;margin-bottom:4px;"><span style="font-size:13px;font-weight:700;color:var(--black);">'+esc(k)+'</span><span style="font-size:12px;color:var(--text-dim);">'+d.total+'명 관심 · '+d.conv+'명 계약</span></div>'+
      '<div style="display:flex;align-items:center;gap:8px;"><div style="flex:1;background:var(--border);border-radius:6px;overflow:hidden;height:22px;"><div style="height:100%;width:'+Math.min(pct,100)+'%;background:'+color+';border-radius:6px;"></div></div><div style="width:46px;text-align:right;font-size:14px;font-weight:800;color:'+color+';">'+pct+'%</div></div></div>';
  });
  el.innerHTML=html;
}
function showRepurchaseList(){
  var panel=document.getElementById("repurchase-panel");
  if(!panel)return;
  if(panel.style.display!=="none"){panel.style.display="none";return;}
  var list=load();var now=todayD();
  function daysFromNow(ds){var d=parseD(ds);if(!d)return null;return Math.round((d-now)/86400000);}
  var repurchase=list.filter(function(c){
    if(c.deliveryDate){var df=daysFromNow(c.deliveryDate);if(df!==null&&df<=-1095)return true;}
    if(c.financeEndDate){var ff=daysFromNow(c.financeEndDate);if(ff!==null&&ff>=0&&ff<=90)return true;}
    if(c.insuranceExpireDate){var inf=daysFromNow(c.insuranceExpireDate);if(inf!==null&&inf>=0&&inf<=30)return true;}
    return false;
  });
  var listEl=document.getElementById("repurchase-list");
  if(!listEl)return;
  if(!repurchase.length){listEl.innerHTML='<div style="text-align:center;color:var(--text-lt);font-size:13px;padding:16px;">해당 고객이 없습니다</div>';}
  else{listEl.innerHTML=repurchase.map(function(c){
    var tags=[];
    if(c.deliveryDate){var df=daysFromNow(c.deliveryDate);if(df!==null&&df<=-1095)tags.push("🚗 출고"+Math.abs(Math.round(df/365))+"년");}
    if(c.financeEndDate){var ff=daysFromNow(c.financeEndDate);if(ff!==null&&ff>=0&&ff<=90)tags.push("💳 금융 D-"+ff);}
    if(c.insuranceExpireDate){var inf=daysFromNow(c.insuranceExpireDate);if(inf!==null&&inf>=0&&inf<=30)tags.push("🛡 보험 D-"+inf);}
    return'<div style="display:flex;align-items:center;justify-content:space-between;padding:10px 0;border-bottom:1px solid var(--border);">'+
      '<div><div style="font-size:14px;font-weight:700;color:var(--black);">'+esc(c.customerName||"—")+'</div>'+
      '<div style="font-size:11px;color:var(--text-dim);margin-top:2px;">'+tags.join(" · ")+'</div></div>'+
      (c.phone?'<a href="tel:'+esc(c.phone)+'" style="padding:6px 14px;background:var(--black);color:#fff;border-radius:20px;text-decoration:none;font-size:12px;font-weight:700;white-space:nowrap;">📞 전화</a>':'')+
    '</div>';
  }).join("");}
  panel.style.display="block";
}
function setAnYear(yr){_anYear=yr;renderAnalytics();}
function renderAnalytics(){
  var list=load();
  var kst=new Date(new Date().getTime()+9*3600000);
  var thisYear=kst.getUTCFullYear();
  if(!_anYear)_anYear=String(thisYear);
  var yfEl=document.getElementById("an-year-filter");
  if(yfEl){
    var btns='<button class="an-yr-btn'+(_anYear===String(thisYear)?" active":"")+'" onclick="setAnYear(\''+thisYear+'\')">'+thisYear+'</button>';
    for(var py=thisYear-1;py>=thisYear-3;py--)btns+='<button class="an-yr-btn'+(_anYear===String(py)?" active":"")+'" onclick="setAnYear(\''+py+'\')">'+py+'</button>';
    btns+='<button class="an-yr-btn'+(_anYear===""?" active":"")+'" onclick="setAnYear(\'\')">전체</button>';
    yfEl.innerHTML=btns;
  }
  var yrStr=_anYear;
  function setEl(id,v){var e=document.getElementById(id);if(e)e.textContent=v;}
  function setBadge(id){setEl(id,_anYear?"("+_anYear+"년)":"(전체)");}
  setEl("an-section-label",(_anYear||"전체")+" 실적 요약");
  function matchYear(dateStr){if(!yrStr)return true;var n=normalizeDateStr(dateStr);return n.substring(0,4)===yrStr;}
  function getDelYM(c){var raw=c.deliveryDate||c.deliveryMonth||"";var n=normalizeDateStr(raw);return n.length>=7?n.substring(0,7):"";}
  function getYM(dateStr){var n=normalizeDateStr(dateStr);return n.length>=7?n.substring(0,7):"";}
  var deliveredAll=list.filter(function(c){return c.status==="출고";});
  var deliveredYear=deliveredAll.filter(function(c){return matchYear(c.deliveryDate||c.deliveryMonth||"");});
  var contractYear=list.filter(function(c){return c.contractDate&&matchYear(c.contractDate);});
  setEl("an-delivery-cnt",deliveredYear.length+"대");setEl("an-contract-cnt",contractYear.length+"대");
  setEl("an-delivery-lbl",(_anYear||"전체")+" 출고대수");setEl("an-contract-lbl",(_anYear||"전체")+" 계약대수");
  var now=todayD();
  function daysFromNow(ds){var d=parseD(ds);if(!d)return null;return Math.round((d-now)/86400000);}
  var repCnt=list.filter(function(c){
    if(c.deliveryDate){var df=daysFromNow(c.deliveryDate);if(df!==null&&df<=-1095)return true;}
    if(c.financeEndDate){var ff=daysFromNow(c.financeEndDate);if(ff!==null&&ff>=0&&ff<=90)return true;}
    if(c.insuranceExpireDate){var inf=daysFromNow(c.insuranceExpireDate);if(inf!==null&&inf>=0&&inf<=30)return true;}
    return false;
  }).length;
  setEl("an-repurchase-cnt",repCnt+"명");
  function makeMonthRange(yr){
    var months=[];
    if(yr){for(var mm=1;mm<=12;mm++)months.push(yr+"-"+String(mm).padStart(2,"0"));}
    else{var end=new Date(kst.getUTCFullYear(),kst.getUTCMonth());for(var i=17;i>=0;i--){var d=new Date(end.getFullYear(),end.getMonth()-i);months.push(d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,"0"));}}
    return months;
  }
  var months12=makeMonthRange(yrStr);
  function mLabel(ym){return yrStr?parseInt(ym.substring(5))+"월":ym.replace("-","년 ")+"월";}
  renderPipelineChart(list);
  setBadge("an-lead-yr");renderLeadConvChart(list,yrStr);
  setBadge("an-model-conv-yr");renderModelConvChart(list,yrStr);
  setBadge("an-contract-yr");
  (function(){var monthMap={};months12.forEach(function(m){monthMap[m]=0;});
    list.forEach(function(c){if(!c.contractDate||!matchYear(c.contractDate))return;var ym=getYM(c.contractDate);if(ym&&monthMap[ym]!==undefined)monthMap[ym]++;});
    makeChart("chart-monthly-contract",{type:"bar",data:{labels:months12.map(mLabel),datasets:[{data:months12.map(function(m){return monthMap[m]||0;}),backgroundColor:"#1a5aa0",borderRadius:5}]},options:barOpts()});
  })();
  setBadge("an-delivery-yr");
  (function(){var monthMap={};months12.forEach(function(m){monthMap[m]=0;});
    deliveredYear.forEach(function(c){var ym=getDelYM(c);if(ym&&monthMap[ym]!==undefined)monthMap[ym]++;});
    makeChart("chart-monthly-delivery",{type:"bar",data:{labels:months12.map(mLabel),datasets:[{data:months12.map(function(m){return monthMap[m]||0;}),backgroundColor:"#0f6e35",borderRadius:5}]},options:barOpts()});
  })();
}
function renderAnalyticsIfVisible(){
  var av=document.getElementById("analyticsView");if(av&&av.classList.contains("active"))renderAnalytics();
}

// ── 초기 실행 ────────────────────────────────────────────────
(function(){
  var d=new Date();

  document.addEventListener("keydown",function(e){
    if(e.key==="Escape"){
      var ov=document.getElementById("editOverlay");if(ov&&ov.classList.contains("open"))closeEdit();
      var ov2=document.getElementById("custDetailOverlay");if(ov2&&ov2.classList.contains("open"))closeCustDetail();
      var ov3=document.getElementById("calPopupOverlay");if(ov3&&ov3.classList.contains("open")){ov3.classList.remove("open");document.body.style.overflow="";}
      var ov4=document.getElementById("schedPopupOverlay");if(ov4&&ov4.classList.contains("open")){ov4.classList.remove("open");document.body.style.overflow="";}
    }
  });
  // localStorage 출고월 자동 보정
  var list=load();var changed=false;
  list.forEach(function(c){
    if((!c.deliveryMonth||c.deliveryMonth==="")&&c.deliveryDate&&c.deliveryDate.length>=7){c.deliveryMonth=c.deliveryDate.substring(0,7);changed=true;}
  });
  if(changed)save(list);
  updateDash();renderHomeContacts();
})();
/* ══════════════════════════════════════
   스케줄 기능
══════════════════════════════════════ */
var SCHED_KEY = 'gy_schedule_v1';
var _sYear, _sMonth;

function schedInitDate(){
  if(_sYear) return; // 이미 초기화됨
  var kst=new Date(new Date().getTime()+9*3600000);
  _sYear=kst.getUTCFullYear(); _sMonth=kst.getUTCMonth();
}
function loadSched(){try{return JSON.parse(localStorage.getItem(SCHED_KEY))||[];}catch(e){return[];}}
function saveSched(l){localStorage.setItem(SCHED_KEY,JSON.stringify(l));}

function schedMove(dir){
  _sMonth+=dir;
  if(_sMonth>11){_sMonth=0;_sYear++;}
  if(_sMonth<0){_sMonth=11;_sYear--;}
  renderSchedCalendar();
}
function schedGoToday(){
  var kst=new Date(new Date().getTime()+9*3600000);
  _sYear=kst.getUTCFullYear();_sMonth=kst.getUTCMonth();
  renderSchedCalendar();
}

function buildSchedCRMEvents(list, year){
  var evMap={};
  function addEv(dateStr,ev){var n=normalizeDateStr(dateStr);if(!n||n.length<10)return;if(!evMap[n])evMap[n]=[];evMap[n].push(ev);}
  list.forEach(function(c){
    var name=c.customerName||'(이름없음)',phone=c.phone||'';
    if(c.nextContact)         addEv(c.nextContact,         {type:'contact',   label:'📞 연락: '+name,  phone:phone,cls:'ev-contact',  crm:true});
    if(c.insuranceExpireDate) addEv(c.insuranceExpireDate, {type:'insurance', label:'🛡 보험: '+name,  phone:phone,cls:'ev-insurance',crm:true});
    if(c.inspectionDueDate)   addEv(c.inspectionDueDate,   {type:'inspection',label:'🔧 검사: '+name,  phone:phone,cls:'ev-inspection',crm:true});
    if(c.financeEndDate)      addEv(c.financeEndDate,      {type:'finance',   label:'💳 금융: '+name,  phone:phone,cls:'ev-finance',  crm:true});
    if(c.nextServiceDate)     addEv(c.nextServiceDate,     {type:'service',   label:'🔧 점검: '+name,  phone:phone,cls:'ev-service',  crm:true});
    if(c.birthDate){
      var b=parseD(c.birthDate);
      if(b){var bStr=year+'-'+String(b.getMonth()+1).padStart(2,'0')+'-'+String(b.getDate()).padStart(2,'0');
        addEv(bStr,{type:'birthday',label:'🎂 생일: '+name,phone:phone,cls:'ev-birthday',crm:true});}
    }
    if(c.deliveryDate){
      var del=parseD(c.deliveryDate);
      if(del){var yrs=year-del.getFullYear();
        if(yrs>=1){var aStr=year+'-'+String(del.getMonth()+1).padStart(2,'0')+'-'+String(del.getDate()).padStart(2,'0');
          addEv(aStr,{type:'delivery',label:'🚗 출고'+yrs+'년: '+name,phone:phone,cls:yrs>=3?'ev-delivery3':'ev-delivery1',crm:true});}}
    }
  });
  return evMap;
}

function schedTodayStr(){
  var k=new Date(new Date().getTime()+9*3600000);
  return k.getUTCFullYear()+'-'+String(k.getUTCMonth()+1).padStart(2,'0')+'-'+String(k.getUTCDate()).padStart(2,'0');
}

function renderSchedCalendar(){
  var crm=load(), custom=loadSched();
  var crmEvMap=buildSchedCRMEvents(crm,_sYear);
  var mons=['1월','2월','3월','4월','5월','6월','7월','8월','9월','10월','11월','12월'];
  var dCnt=new Date(_sYear,_sMonth+1,0).getDate();
  document.getElementById('schedMonthTitle').textContent=_sYear+'년 '+mons[_sMonth];
  document.getElementById('schedMonthSub').textContent=dCnt+'일';

  var firstDay=new Date(_sYear,_sMonth,1).getDay();
  var lastDate=new Date(_sYear,_sMonth+1,0).getDate();
  var prevLast=new Date(_sYear,_sMonth,0).getDate();
  var kst=new Date(new Date().getTime()+9*3600000);
  var todayNum=(_sYear===kst.getUTCFullYear()&&_sMonth===kst.getUTCMonth())?kst.getUTCDate():null;
  var todayFull=schedTodayStr();
  var totalCells=Math.ceil((firstDay+lastDate)/7)*7;
  var MAX_VIS=4;
  var body=document.getElementById('schedBody');
  body.innerHTML='';

  var statTotal=0,statDone=0,statContact=0,statTodayLeft=0;

  for(var i=0;i<totalCells;i++){
    var dateNum,isOther=false,mo=_sMonth,yr=_sYear;
    if(i<firstDay){dateNum=prevLast-firstDay+i+1;isOther=true;mo=_sMonth-1;if(mo<0){mo=11;yr--;}}
    else if(i-firstDay>=lastDate){dateNum=i-firstDay-lastDate+1;isOther=true;mo=_sMonth+1;if(mo>11){mo=0;yr++;}}
    else{dateNum=i-firstDay+1;}
    var dow=i%7;
    var fullDate=yr+'-'+String(mo+1).padStart(2,'0')+'-'+String(dateNum).padStart(2,'0');

    var crmEvs=(!isOther&&crmEvMap[fullDate])||[];
    var custEvs=custom.filter(function(t){return t.date===fullDate;});
    var allEvs=crmEvs.concat(custEvs.map(function(t){return{type:'custom',label:t.text,cls:'ev-custom-'+t.color,id:t.id,done:t.done,custom:true};}));

    if(!isOther){
      statTotal+=allEvs.length;
      statDone+=allEvs.filter(function(e){return e.done;}).length;
      statContact+=allEvs.filter(function(e){return e.type==='contact';}).length;
      if(fullDate===todayFull)statTodayLeft+=allEvs.filter(function(e){return !e.done;}).length;
    }

    var cell=document.createElement('div');
    cell.className='cal-cell'+(isOther?' other-month':'')+((!isOther&&dateNum===todayNum)?' today':'');

    var dateDiv=document.createElement('div');dateDiv.className='cal-date';
    var dateNumEl=document.createElement('div');dateNumEl.className='cal-date-num';
    if(!isOther&&dateNum===todayNum){
      dateNumEl.style.background='linear-gradient(135deg,#b8963a,#d4aa50)';
      dateNumEl.style.color='#fff';
      dateNumEl.style.borderRadius='50%';
      dateNumEl.style.width='26px';dateNumEl.style.height='26px';
      dateNumEl.style.display='flex';dateNumEl.style.alignItems='center';dateNumEl.style.justifyContent='center';
      dateNumEl.style.fontSize='13px';dateNumEl.style.fontWeight='800';
    }
    if(dow===0&&!isOther)dateNumEl.style.color='#e05050';
    if(dow===6&&!isOther)dateNumEl.style.color='#4466cc';
    dateNumEl.textContent=dateNum;
    dateDiv.appendChild(dateNumEl);cell.appendChild(dateDiv);

    if(allEvs.length>0){
      var evsDiv=document.createElement('div');evsDiv.className='cal-events';
      var vis=allEvs.slice(0,MAX_VIS),hid=allEvs.slice(MAX_VIS);
      vis.forEach(function(ev){
        var el=document.createElement('div');
        el.className='cal-event '+ev.cls+(ev.done?' ev-done':'');
        el.title=ev.label;el.textContent=ev.label;
        if(ev.custom){
          var del=document.createElement('span');del.className='del-btn';del.textContent='✕';
          del.onclick=(function(id){return function(e){e.stopPropagation();schedDeleteTask(id);};})(ev.id);
          el.appendChild(del);
        }
        el.onclick=(function(d,a){return function(){openSchedPopup(d,a);};})(fullDate,allEvs);
        evsDiv.appendChild(el);
      });
      if(hid.length>0){
        var moreEl=document.createElement('div');moreEl.className='cal-more';
        moreEl.textContent='+'+hid.length+'개 더';
        moreEl.onclick=(function(d,a){return function(){openSchedPopup(d,a);};})(fullDate,allEvs);
        evsDiv.appendChild(moreEl);
      }
      cell.appendChild(evsDiv);
      cell.style.cursor='pointer';
      cell.onclick=(function(d,a){return function(e){if(e.target===cell||e.target===dateNumEl||e.target===dateDiv)openSchedPopup(d,a);};})(fullDate,allEvs);
    }
    body.appendChild(cell);
  }

  function setEl(id,v){var e=document.getElementById(id);if(e)e.textContent=v;}
  setEl('stat-total',statTotal);setEl('stat-done',statDone);
  setEl('stat-contact',statContact);setEl('stat-today-left',statTodayLeft);
}

/* 일정 추가 */
function schedAddTask(){
  var text=document.getElementById('schedAddText').value.trim();
  var date=document.getElementById('schedAddDate').value;
  var color=document.getElementById('schedAddColor').value;
  if(!text){document.getElementById('schedAddText').focus();return;}
  if(!date){date=schedTodayStr();document.getElementById('schedAddDate').value=date;}
  var list=loadSched();
  list.push({id:Date.now()+'_'+Math.random().toString(36).slice(2),text:text,date:date,color:+color,done:false});
  saveSched(list);
  document.getElementById('schedAddText').value='';
  renderSchedCalendar();
}

/* 일정 삭제 */
function schedDeleteTask(id){
  if(!confirm('이 일정을 삭제할까요?'))return;
  saveSched(loadSched().filter(function(t){return t.id!==id;}));
  document.getElementById('schedPopupOverlay').classList.remove('open');
  document.body.style.overflow='';
  renderSchedCalendar();
}

/* 완료 토글 */
function schedToggleDone(id){
  var list=loadSched();
  var t=list.find(function(x){return x.id===id;});
  if(t)t.done=!t.done;
  saveSched(list);
  renderSchedCalendar();
  var btn=document.querySelector('[data-sched-done="'+id+'"]');
  if(btn){btn.classList.toggle('checked',!!t&&t.done);btn.textContent=t&&t.done?'✓':'';
    var txt=btn.parentElement.querySelector('.sched-popup-text');
    if(txt)txt.classList.toggle('done',!!t&&t.done);}
}

/* 팝업 */
var EV_COLORS_S={contact:'#3a7acc',insurance:'#cc7a22',inspection:'#bb6611',finance:'#cc3333',service:'#8833cc',birthday:'#9933cc',delivery:'#22aa55',custom:'#4466cc'};
function openSchedPopup(dateStr,evs){
  var p=dateStr.split('-');
  document.getElementById('schedPopupDate').textContent=+p[0]+'년 '+parseInt(p[1])+'월 '+parseInt(p[2])+'일';
  var html='';
  evs.forEach(function(ev){
    var color=EV_COLORS_S[ev.type]||'#666';
    html+='<div class="sched-popup-item">';
    html+='<div class="sched-popup-dot" style="background:'+color+';"></div>';
    html+='<div style="flex:1;min-width:0;">';
    html+='<div class="sched-popup-text'+(ev.done?' done':'')+'">'+esc(ev.label)+'</div>';
    if(ev.phone)html+='<a class="sched-popup-phone" href="tel:'+esc(ev.phone)+'">📞 '+esc(ev.phone)+'</a>';
    html+='</div>';
    if(ev.custom)html+='<button class="sched-check'+(ev.done?' checked':'')+'" data-sched-done="'+ev.id+'" onclick="schedToggleDone(''+ev.id+'')">'+(ev.done?'✓':'')+'</button>';
    html+='</div>';
  });
  document.getElementById('schedPopupList').innerHTML=html;
  document.getElementById('schedPopupOverlay').classList.add('open');
  document.body.style.overflow='hidden';
}
function closeSchedPopup(e){if(e.target===document.getElementById('schedPopupOverlay')){document.getElementById('schedPopupOverlay').classList.remove('open');document.body.style.overflow='';}}

/* 초기 날짜 세팅 */
(function(){
  var kst=new Date(new Date().getTime()+9*3600000);
  _sYear=kst.getUTCFullYear();_sMonth=kst.getUTCMonth();
  var today=schedTodayStr();
  var sd=document.getElementById('schedAddDate');
  if(sd)sd.value=today;
})();

</script>
</body>
</html>
