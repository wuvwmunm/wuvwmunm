<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>소비 선택 시스템</title>
<link href="https://fonts.googleapis.com/css2?family=Pretendard:wght@400;500;600&family=DM+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #f7f6f2;
    --surface: #ffffff;
    --border: rgba(0,0,0,0.09);
    --border-med: rgba(0,0,0,0.16);
    --text-primary: #1a1a1a;
    --text-secondary: #6b6b6b;
    --text-hint: #aaaaaa;
    --accent: #2d6a4f;
    --accent-light: #e8f5ee;
    --warn1: #b37c00;
    --warn2: #c0392b;
    --food: #1a6fa8;
    --hobby-var: #6d4fa8;
    --hobby-sel: #a8724f;
    --radius: 12px;
    --radius-sm: 8px;
    --mono: 'DM Mono', monospace;
    --sans: 'Pretendard', -apple-system, sans-serif;
    /* asset palette */
    --a1: #2563a8; --a2: #2d8f6f; --a3: #a85c2d; --a4: #7c3a9e;
    --a5: #b34a4a; --a6: #4a7a4a; --a7: #8a6a2a; --a8: #3a6a8a;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    font-family: var(--sans);
    background: var(--bg);
    color: var(--text-primary);
    min-height: 100vh;
    font-size: 15px;
    line-height: 1.5;
  }
  /* Layout */
  .shell {
    max-width: 560px;
    margin: 0 auto;
    padding: 24px 16px 80px;
  }
  /* Header */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 24px;
  }
  .date-block .date-main {
    font-size: 18px;
    font-weight: 600;
    letter-spacing: -0.3px;
  }
  .date-block .date-sub {
    font-size: 12px;
    color: var(--text-hint);
    margin-top: 2px;
  }
  .btn-reset {
    font-family: var(--sans);
    font-size: 12px;
    color: var(--text-secondary);
    background: var(--surface);
    border: 0.5px solid var(--border-med);
    border-radius: 20px;
    padding: 6px 14px;
    cursor: pointer;
    transition: background 0.15s;
  }
  .btn-reset:hover { background: #f0ede6; }

  /* Main budget card */
  .budget-hero {
    background: var(--surface);
    border-radius: var(--radius);
    border: 0.5px solid var(--border);
    padding: 20px 22px 16px;
    margin-bottom: 12px;
  }
  .budget-label {
    font-size: 11px;
    letter-spacing: 0.6px;
    text-transform: uppercase;
    color: var(--text-hint);
    margin-bottom: 6px;
  }
  .budget-amount {
    font-family: var(--mono);
    font-size: 40px;
    font-weight: 500;
    letter-spacing: -1px;
    transition: color 0.4s;
    color: var(--text-primary);
  }
  .budget-amount.warn1 { color: var(--warn1); }
  .budget-amount.warn2 { color: var(--warn2); }
  .budget-status {
    font-size: 12px;
    margin-top: 4px;
    color: var(--text-secondary);
  }
  .budget-divider {
    border: none;
    border-top: 0.5px solid var(--border);
    margin: 14px 0;
  }
  .budget-meta {
    display: flex;
    gap: 20px;
  }
  .meta-item {
    font-size: 12px;
    color: var(--text-secondary);
  }
  .meta-item span {
    display: block;
    font-size: 11px;
    color: var(--text-hint);
    margin-bottom: 1px;
  }

  /* Asset card */
  .asset-card {
    background: var(--surface);
    border-radius: var(--radius);
    border: 0.5px solid var(--border);
    padding: 14px 22px;
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    cursor: pointer;
    transition: background 0.12s;
  }
  .asset-card:hover { background: #faf9f6; }
  .asset-left { display: flex; flex-direction: column; gap: 2px; }
  .asset-label { font-size: 11px; letter-spacing: 0.6px; text-transform: uppercase; color: var(--text-hint); }
  .asset-total { font-family: var(--mono); font-size: 26px; font-weight: 500; letter-spacing: -0.5px; color: var(--text-primary); }
  .asset-sub { font-size: 11px; color: var(--text-hint); margin-top: 1px; }
  .asset-right { display: flex; align-items: center; gap: 12px; }
  .asset-arrow { font-size: 16px; color: var(--text-hint); }

  /* Asset detail modal */
  .asset-modal-overlay {
    display: none; position: fixed; inset: 0;
    background: rgba(0,0,0,0.45); z-index: 200;
    align-items: flex-end; justify-content: center;
  }
  .asset-modal-overlay.open { display: flex; }
  .asset-modal {
    background: var(--surface);
    border-radius: 18px 18px 0 0;
    width: 100%; max-width: 560px;
    max-height: 92vh; overflow-y: auto;
    padding-bottom: 40px;
  }
  .asset-modal-handle {
    width: 36px; height: 4px;
    background: var(--border-med);
    border-radius: 99px;
    margin: 12px auto 0;
  }
  .asset-modal-header {
    display: flex; justify-content: space-between; align-items: center;
    padding: 16px 20px 0;
  }
  .asset-modal-title { font-size: 16px; font-weight: 600; }
  .asset-modal-close { background: none; border: none; font-size: 22px; color: var(--text-hint); cursor: pointer; line-height: 1; }
  .donut-wrap { display: flex; flex-direction: column; align-items: center; padding: 24px 20px 8px; gap: 2px; }
  .donut-total-label { font-size: 11px; color: var(--text-hint); letter-spacing: 0.4px; text-transform: uppercase; }
  .donut-total-amt { font-family: var(--mono); font-size: 28px; font-weight: 500; letter-spacing: -0.5px; margin-top: 4px; }
  .asset-legend {
    display: grid; grid-template-columns: 1fr 1fr; gap: 8px;
    padding: 0 20px 16px;
  }
  .legend-item {
    display: flex; align-items: center; gap: 8px;
    background: var(--bg); border-radius: var(--radius-sm); padding: 8px 12px;
  }
  .legend-dot { width: 8px; height: 8px; border-radius: 50%; flex-shrink: 0; }
  .legend-info { display: flex; flex-direction: column; gap: 1px; min-width: 0; overflow: hidden; }
  .legend-name { font-size: 12px; font-weight: 500; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; }
  .legend-amt { font-family: var(--mono); font-size: 11px; color: var(--text-secondary); }
  .legend-pct { font-size: 10px; color: var(--text-hint); }
  .asset-edit-section { padding: 16px 20px 0; border-top: 0.5px solid var(--border); }
  .asset-edit-title { font-size: 12px; font-weight: 500; color: var(--text-secondary); margin-bottom: 10px; }
  .asset-item-row { display: flex; gap: 8px; align-items: center; margin-bottom: 8px; }
  .asset-item-row input {
    font-family: var(--sans); font-size: 13px;
    border: 0.5px solid var(--border-med); border-radius: var(--radius-sm);
    padding: 8px 10px; outline: none;
    background: var(--bg); color: var(--text-primary);
  }
  .asset-item-name { flex: 1; }
  .asset-item-amt { width: 120px; font-family: var(--mono) !important; }
  .asset-item-del { background: none; border: none; color: var(--text-hint); font-size: 18px; cursor: pointer; padding: 0; line-height: 1; }
  .asset-item-del:hover { color: var(--warn2); }
  .btn-add-asset-item {
    font-family: var(--sans); font-size: 12px; color: var(--accent);
    background: none; border: 0.5px dashed var(--accent);
    border-radius: var(--radius-sm); padding: 7px 14px;
    cursor: pointer; margin-top: 4px; width: 100%;
  }
  .btn-save-asset {
    font-family: var(--sans); font-size: 13px; font-weight: 500;
    width: 100%; padding: 11px;
    background: var(--text-primary); color: #fff;
    border: none; border-radius: var(--radius-sm);
    cursor: pointer; margin-top: 14px;
  }
  .btn-save-asset:hover { opacity: 0.85; }
  .asset-empty { text-align: center; padding: 32px 20px; color: var(--text-hint); font-size: 13px; }

  /* Carryover banner */
  .carryover-banner {
    background: var(--accent-light);
    border-radius: var(--radius-sm);
    border: 0.5px solid rgba(45,106,79,0.2);
    padding: 8px 14px;
    margin-bottom: 12px;
    font-size: 12px;
    color: var(--accent);
    display: none;
  }
  .carryover-banner.visible { display: block; }

  /* Category cards */
  .section-title {
    font-size: 11px;
    letter-spacing: 0.6px;
    text-transform: uppercase;
    color: var(--text-hint);
    margin-bottom: 8px;
    margin-top: 4px;
  }
  .cat-list { display: flex; flex-direction: column; gap: 8px; margin-bottom: 12px; }
  .cat-card {
    background: var(--surface);
    border-radius: var(--radius-sm);
    border: 0.5px solid var(--border);
    padding: 12px 16px;
  }
  .cat-header {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 7px;
  }
  .cat-name {
    font-size: 13px;
    font-weight: 500;
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .cat-dot {
    width: 7px; height: 7px;
    border-radius: 50%;
    display: inline-block;
  }
  .cat-dot.food { background: var(--food); }
  .cat-dot.hobby_variable { background: var(--hobby-var); }
  .cat-dot.hobby_select { background: var(--hobby-sel); }
  .cat-available {
    font-size: 12px;
    color: var(--text-secondary);
    font-family: var(--mono);
  }
  .cat-bar-bg {
    height: 4px;
    background: #eeece8;
    border-radius: 99px;
    overflow: hidden;
  }
  .cat-bar-fill {
    height: 100%;
    border-radius: 99px;
    transition: width 0.3s ease;
  }
  .cat-bar-fill.food { background: var(--food); }
  .cat-bar-fill.hobby_variable { background: var(--hobby-var); }
  .cat-bar-fill.hobby_select { background: var(--hobby-sel); }
  .cat-numbers {
    display: flex;
    justify-content: space-between;
    margin-top: 6px;
    font-size: 11px;
    color: var(--text-hint);
    font-family: var(--mono);
  }

  /* Input row */
  .input-zone {
    background: var(--surface);
    border-radius: var(--radius);
    border: 0.5px solid var(--border);
    padding: 14px 16px;
    margin-bottom: 12px;
  }
  .input-row {
    display: flex;
    gap: 8px;
    align-items: stretch;
    flex-wrap: wrap;
  }
  .input-amount {
    font-family: var(--mono);
    font-size: 17px;
    border: none;
    outline: none;
    background: transparent;
    color: var(--text-primary);
    width: 130px;
    padding: 4px 0;
    border-bottom: 1.5px solid var(--border-med);
  }
  .input-amount::placeholder { color: var(--text-hint); font-size: 14px; }
  .input-memo {
    font-family: var(--sans);
    font-size: 13px;
    border: none;
    outline: none;
    background: transparent;
    color: var(--text-primary);
    flex: 1;
    min-width: 80px;
    padding: 4px 0;
    border-bottom: 1.5px solid var(--border-med);
  }
  .input-memo::placeholder { color: var(--text-hint); }
  .cat-btn-group {
    display: flex;
    gap: 5px;
    margin-top: 10px;
    flex-wrap: wrap;
  }
  .cat-btn {
    font-family: var(--sans);
    font-size: 11px;
    padding: 5px 11px;
    border-radius: 20px;
    border: 0.5px solid var(--border-med);
    background: transparent;
    color: var(--text-secondary);
    cursor: pointer;
    transition: all 0.12s;
  }
  .cat-btn:hover { background: #f0ede6; }
  .cat-btn.active-food { background: #e6f0f8; color: var(--food); border-color: var(--food); }
  .cat-btn.active-hobby_variable { background: #f0eaf8; color: var(--hobby-var); border-color: var(--hobby-var); }
  .cat-btn.active-hobby_select { background: #f8f0e8; color: var(--hobby-sel); border-color: var(--hobby-sel); }
  .btn-add {
    font-family: var(--sans);
    font-size: 13px;
    font-weight: 500;
    padding: 8px 18px;
    background: var(--text-primary);
    color: #fff;
    border: none;
    border-radius: var(--radius-sm);
    cursor: pointer;
    margin-top: 10px;
    width: 100%;
    transition: opacity 0.15s;
  }
  .btn-add:hover { opacity: 0.85; }

  /* Ledger */
  .ledger {
    background: var(--surface);
    border-radius: var(--radius);
    border: 0.5px solid var(--border);
    overflow: hidden;
    margin-bottom: 12px;
  }
  .ledger-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 16px;
    border-bottom: 0.5px solid var(--border);
  }
  .ledger-title { font-size: 12px; font-weight: 500; color: var(--text-secondary); }
  .ledger-total {
    font-family: var(--mono);
    font-size: 13px;
    color: var(--text-secondary);
  }
  .ledger-row {
    display: flex;
    align-items: center;
    padding: 10px 16px;
    border-bottom: 0.5px solid var(--border);
    gap: 10px;
    animation: fadeIn 0.2s ease;
  }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(-4px); } to { opacity: 1; transform: none; } }
  .ledger-row:last-child { border-bottom: none; }
  .ledger-dot { width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; }
  .ledger-memo { flex: 1; font-size: 13px; color: var(--text-primary); }
  .ledger-cat { font-size: 11px; color: var(--text-hint); }
  .ledger-amt {
    font-family: var(--mono);
    font-size: 14px;
    color: var(--text-primary);
    font-weight: 500;
    flex-shrink: 0;
  }
  .ledger-del {
    background: none;
    border: none;
    color: var(--text-hint);
    cursor: pointer;
    font-size: 16px;
    padding: 0 2px;
    line-height: 1;
  }
  .ledger-del:hover { color: var(--warn2); }
  .ledger-empty {
    padding: 24px 16px;
    text-align: center;
    color: var(--text-hint);
    font-size: 13px;
  }

  /* Category edit modal */
  .modal-overlay {
    display: none;
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.4);
    z-index: 100;
    align-items: flex-end;
    justify-content: center;
  }
  .modal-overlay.open { display: flex; }
  .modal {
    background: var(--surface);
    border-radius: 16px 16px 0 0;
    width: 100%;
    max-width: 560px;
    padding: 24px 20px 32px;
    max-height: 80vh;
    overflow-y: auto;
  }
  .modal-title {
    font-size: 15px;
    font-weight: 600;
    margin-bottom: 16px;
  }
  .modal-close {
    float: right;
    background: none;
    border: none;
    font-size: 20px;
    cursor: pointer;
    color: var(--text-hint);
    line-height: 1;
    margin-top: -2px;
  }
  .cat-edit-row {
    display: flex;
    gap: 8px;
    align-items: center;
    margin-bottom: 10px;
  }
  .cat-edit-row input {
    font-family: var(--sans);
    font-size: 13px;
    border: 0.5px solid var(--border-med);
    border-radius: var(--radius-sm);
    padding: 8px 12px;
    outline: none;
    background: var(--bg);
    color: var(--text-primary);
  }
  .cat-edit-name { flex: 1; }
  .cat-edit-budget { width: 100px; font-family: var(--mono); }
  .cat-edit-carry {
    font-size: 11px;
    color: var(--text-hint);
  }
  .cat-del-btn {
    background: none;
    border: none;
    color: var(--text-hint);
    cursor: pointer;
    font-size: 18px;
    padding: 0;
  }
  .cat-del-btn:hover { color: var(--warn2); }
  .btn-add-cat {
    font-family: var(--sans);
    font-size: 12px;
    color: var(--accent);
    background: none;
    border: 0.5px dashed var(--accent);
    border-radius: var(--radius-sm);
    padding: 7px 14px;
    cursor: pointer;
    margin-top: 4px;
  }
  .modal-save {
    font-family: var(--sans);
    font-size: 13px;
    font-weight: 500;
    width: 100%;
    padding: 11px;
    background: var(--text-primary);
    color: #fff;
    border: none;
    border-radius: var(--radius-sm);
    cursor: pointer;
    margin-top: 16px;
  }
  .modal-save:hover { opacity: 0.85; }
  .modal-check { accent-color: var(--accent); }

  /* Backup bar */
  .backup-bar {
    display: flex;
    gap: 6px;
    justify-content: flex-end;
    margin-bottom: 16px;
  }
  .btn-backup {
    font-family: var(--sans);
    font-size: 11px;
    color: var(--text-secondary);
    background: var(--surface);
    border: 0.5px solid var(--border-med);
    border-radius: 20px;
    padding: 5px 12px;
    cursor: pointer;
    transition: background 0.12s;
    display: flex;
    align-items: center;
    gap: 4px;
  }
  .btn-backup:hover { background: #f0ede6; }
  .btn-backup.import { color: var(--accent); border-color: rgba(45,106,79,0.35); }
  .btn-backup.import:hover { background: var(--accent-light); }
  /* Toast notification */
  .toast {
    position: fixed;
    bottom: 32px;
    left: 50%;
    transform: translateX(-50%) translateY(20px);
    background: var(--text-primary);
    color: #fff;
    font-size: 13px;
    padding: 10px 20px;
    border-radius: 99px;
    opacity: 0;
    pointer-events: none;
    transition: opacity 0.2s, transform 0.2s;
    z-index: 999;
    white-space: nowrap;
  }
  .toast.show {
    opacity: 1;
    transform: translateX(-50%) translateY(0);
  }

  /* Link buttons */
  .link-btn {
    background: none;
    border: none;
    font-family: var(--sans);
    font-size: 11px;
    color: var(--text-hint);
    cursor: pointer;
    text-decoration: underline;
    padding: 0;
  }
  .link-btn:hover { color: var(--text-secondary); }

  /* Section row with action */
  .section-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 8px;
    margin-top: 4px;
  }
</style>
</head>
<body>
<div class="shell">

  <!-- Header -->
  <div class="header">
    <div class="date-block">
      <div class="date-main" id="dateMain">2025년 6월 1일 일요일</div>
      <div class="date-sub" id="dateMonth">2025년 6월</div>
    </div>
    <button class="btn-reset" id="btnReset">월 마감 →</button>
  </div>

  <!-- Backup bar -->
  <div class="backup-bar">
    <button class="btn-backup import" id="btnImport">↑ 불러오기</button>
    <button class="btn-backup" id="btnExport">↓ 내보내기</button>
  </div>
  <input type="file" id="importFile" accept=".json" style="display:none">

  <!-- Toast -->
  <div class="toast" id="toast"></div>

  <!-- Total Asset Card -->
  <div class="asset-card" id="assetCard">
    <div class="asset-left">
      <div class="asset-label">총자산</div>
      <div class="asset-total" id="assetTotalDisplay">—</div>
      <div class="asset-sub" id="assetSubDisplay">탭하여 자산 상세 보기</div>
    </div>
    <div class="asset-right">
      <canvas id="assetMiniDonut" width="48" height="48"></canvas>
      <div class="asset-arrow">›</div>
    </div>
  </div>

  <!-- Asset detail modal -->
  <div class="asset-modal-overlay" id="assetModalOverlay">
    <div class="asset-modal">
      <div class="asset-modal-handle"></div>
      <div class="asset-modal-header">
        <div class="asset-modal-title">자산 현황</div>
        <button class="asset-modal-close" id="assetModalClose">×</button>
      </div>
      <!-- Donut chart -->
      <div class="donut-wrap">
        <canvas id="assetDonut" width="200" height="200"></canvas>
        <div class="donut-total-label" style="margin-top:12px">총자산</div>
        <div class="donut-total-amt" id="assetModalTotal">0원</div>
      </div>
      <!-- Legend -->
      <div class="asset-legend" id="assetLegend"></div>
      <!-- Edit section -->
      <div class="asset-edit-section">
        <div class="asset-edit-title">자산 항목 편집</div>
        <div id="assetItemList"></div>
        <button class="btn-add-asset-item" id="btnAddAssetItem">+ 항목 추가</button>
        <button class="btn-save-asset" id="btnSaveAsset">저장</button>
      </div>
    </div>
  </div>

  <!-- Carryover banner -->
  <div class="carryover-banner" id="carryoverBanner"></div>

  <!-- Budget hero -->
  <div class="budget-hero">
    <div class="budget-label">이번 달 선택 가능 금액</div>
    <div class="budget-amount" id="budgetAmount">250,000원</div>
    <div class="budget-status" id="budgetStatus">여유 있어요 · 마음껏 선택하세요</div>
    <hr class="budget-divider">
    <div class="budget-meta">
      <div class="meta-item"><span>한도</span><span id="metaLimit">250,000원</span></div>
      <div class="meta-item"><span>사용</span><span id="metaUsed">0원</span></div>
      <div class="meta-item"><span>이월잔액</span><span id="metaCarry">0원</span></div>
    </div>
  </div>

  <!-- Category bars -->
  <div class="section-row">
    <div class="section-title" style="margin:0">카테고리별</div>
    <button class="link-btn" id="btnEditCat">카테고리 수정</button>
  </div>
  <div class="cat-list" id="catList"></div>

  <!-- Input zone -->
  <div class="input-zone">
    <div class="input-row">
      <input class="input-amount" type="number" id="inputAmt" placeholder="금액" min="0">
      <input class="input-memo" type="text" id="inputMemo" placeholder="메모 (선택)">
    </div>
    <div class="cat-btn-group" id="catBtnGroup"></div>
    <button class="btn-add" id="btnAdd">기록하기</button>
  </div>

  <!-- Ledger -->
  <div class="section-title">소비 내역</div>
  <div class="ledger">
    <div class="ledger-header">
      <div class="ledger-title">이번 달</div>
      <div class="ledger-total" id="ledgerTotal">합계 0원</div>
    </div>
    <div id="ledgerBody"></div>
  </div>

</div>

<!-- Category edit modal -->
<div class="modal-overlay" id="modalOverlay">
  <div class="modal">
    <button class="modal-close" id="modalClose">×</button>
    <div class="modal-title">카테고리 설정</div>
    <div id="catEditList"></div>
    <button class="btn-add-cat" id="btnAddCat">+ 카테고리 추가</button>
    <div style="margin-top:12px; font-size:12px; color:var(--text-hint);">
      이월 허용 체크시 남은 금액이 다음 달로 이월됩니다
    </div>
    <button class="modal-save" id="btnSaveCat">저장</button>
  </div>
</div>

<script>
// ─── 기본 카테고리 설정 ───
const DEFAULT_CATS = [
  { id: 'food',          name: '식비',       budget: 100000, carryover: false, color: '#1a6fa8' },
  { id: 'hobby_variable',name: '소모형 취미', budget: 50000,  carryover: true,  color: '#6d4fa8' },
  { id: 'hobby_select',  name: '선택형 취미', budget: 100000, carryover: true,  color: '#a8724f' },
];
const MAX_CARRYOVER = 300000; // 최대 이월 30만

// ─── localStorage 키 ───
const KEY_STATE = 'bcs_state_v2';

// ─── 상태 구조 ───
function defaultState() {
  const now = new Date();
  return {
    month: now.getFullYear() * 100 + (now.getMonth() + 1),
    categories: JSON.parse(JSON.stringify(DEFAULT_CATS)),
    carryover: 0,      // 이전 달에서 가져온 이월 금액 (양수=남음, 음수=초과)
    entries: [],
  };
}

function loadState() {
  try {
    const raw = localStorage.getItem(KEY_STATE);
    if (!raw) return defaultState();
    return JSON.parse(raw);
  } catch(e) { return defaultState(); }
}

function saveState(state) {
  localStorage.setItem(KEY_STATE, JSON.stringify(state));
}

// ─── 계산 헬퍼 ───
function totalBudget(state) {
  return state.categories.reduce((s, c) => s + c.budget, 0);
}

function usedByCategory(state) {
  const map = {};
  state.categories.forEach(c => { map[c.id] = 0; });
  state.entries.forEach(e => { if (map[e.catId] !== undefined) map[e.catId] += e.amount; });
  return map;
}

function totalUsed(state) {
  return state.entries.reduce((s, e) => s + e.amount, 0);
}

function effectiveLimit(state) {
  // 총 한도 + 이월 잔액(음수면 차감)
  return totalBudget(state) + state.carryover;
}

function remaining(state) {
  return effectiveLimit(state) - totalUsed(state);
}

// ─── 날짜 표시 ───
const DAYS = ['일','월','화','수','목','금','토'];
function renderDate() {
  const now = new Date();
  const y = now.getFullYear(), m = now.getMonth()+1, d = now.getDate();
  const day = DAYS[now.getDay()];
  document.getElementById('dateMain').textContent = `${y}년 ${m}월 ${d}일 ${day}요일`;
  document.getElementById('dateMonth').textContent = `${y}년 ${m}월`;
}

// ─── UI 렌더 ───
function fmt(n) { return Math.abs(n).toLocaleString('ko-KR') + '원'; }
function fmtSigned(n) { return (n < 0 ? '-' : '') + fmt(n); }

function renderBudgetHero(state) {
  const lim = effectiveLimit(state);
  const rem = remaining(state);
  const amtEl = document.getElementById('budgetAmount');
  const statusEl = document.getElementById('budgetStatus');
  const ratio = lim > 0 ? rem / lim : 0;

  amtEl.textContent = fmtSigned(rem);
  amtEl.className = 'budget-amount';

  if (rem < 0) {
    amtEl.classList.add('warn2');
    statusEl.textContent = '한도 초과 · 잠깐, 다시 생각해볼까요';
  } else if (ratio < 0.25) {
    amtEl.classList.add('warn2');
    statusEl.textContent = '위험 · 선택지가 많지 않아요';
  } else if (ratio < 0.5) {
    amtEl.classList.add('warn1');
    statusEl.textContent = '보통 · 신중하게 선택해요';
  } else {
    statusEl.textContent = '여유 있어요 · 마음껏 선택하세요';
  }

  document.getElementById('metaLimit').textContent = fmt(lim);
  document.getElementById('metaUsed').textContent = fmt(totalUsed(state));
  document.getElementById('metaCarry').textContent = (state.carryover >= 0 ? '+' : '') + fmtSigned(state.carryover);

  // carryover banner
  const banner = document.getElementById('carryoverBanner');
  if (state.carryover !== 0) {
    banner.classList.add('visible');
    if (state.carryover > 0) {
      banner.textContent = `지난 달 절약분 +${fmt(state.carryover)} 이월됨`;
    } else {
      banner.textContent = `지난 달 초과분 ${fmtSigned(state.carryover)} 차감됨`;
    }
  } else {
    banner.classList.remove('visible');
  }
}

function renderCatList(state) {
  const used = usedByCategory(state);
  const container = document.getElementById('catList');
  container.innerHTML = '';
  state.categories.forEach(c => {
    const u = used[c.id] || 0;
    const avail = c.budget - u;
    const pct = c.budget > 0 ? Math.min(100, (u / c.budget) * 100) : 0;
    const card = document.createElement('div');
    card.className = 'cat-card';
    card.innerHTML = `
      <div class="cat-header">
        <div class="cat-name">
          <span class="cat-dot" style="background:${c.color}"></span>
          ${c.name}
        </div>
        <div class="cat-available" style="color:${avail < 0 ? 'var(--warn2)' : 'var(--text-secondary)'}">
          ${avail >= 0 ? '선택 가능 ' + fmt(avail) : '초과 ' + fmt(-avail)}
        </div>
      </div>
      <div class="cat-bar-bg">
        <div class="cat-bar-fill" style="width:${pct}%;background:${c.color}"></div>
      </div>
      <div class="cat-numbers">
        <span>${fmt(u)} 사용</span>
        <span>한도 ${fmt(c.budget)}</span>
      </div>
    `;
    container.appendChild(card);
  });
}

function renderCatBtns(state, selectedId) {
  const group = document.getElementById('catBtnGroup');
  group.innerHTML = '';
  state.categories.forEach(c => {
    const btn = document.createElement('button');
    btn.className = 'cat-btn' + (c.id === selectedId ? ' active-' + c.id : '');
    btn.dataset.id = c.id;
    btn.textContent = c.name;
    btn.onclick = () => {
      document.querySelectorAll('.cat-btn').forEach(b => b.className = 'cat-btn');
      btn.className = 'cat-btn active-' + c.id;
    };
    group.appendChild(btn);
  });
}

function renderLedger(state) {
  const body = document.getElementById('ledgerBody');
  const total = document.getElementById('ledgerTotal');
  body.innerHTML = '';
  const sorted = [...state.entries].reverse();
  if (sorted.length === 0) {
    body.innerHTML = '<div class="ledger-empty">아직 기록이 없어요</div>';
  } else {
    sorted.forEach((e) => {
      const cat = state.categories.find(c => c.id === e.catId);
      const color = cat ? cat.color : '#aaa';
      const catName = cat ? cat.name : e.catId;
      const row = document.createElement('div');
      row.className = 'ledger-row';
      row.innerHTML = `
        <div class="ledger-dot" style="background:${color}"></div>
        <div>
          <div class="ledger-memo">${e.memo || '(메모 없음)'}</div>
          <div class="ledger-cat">${catName} · ${e.date}</div>
        </div>
        <div class="ledger-amt" style="margin-left:auto">-${fmt(e.amount)}</div>
        <button class="ledger-del" data-eid="${e.id}">×</button>
      `;
      body.appendChild(row);
    });
  }
  total.textContent = '합계 ' + fmt(totalUsed(state));

  // delete handlers
  body.querySelectorAll('.ledger-del').forEach(btn => {
    btn.onclick = () => {
      const eid = btn.dataset.eid;
      state.entries = state.entries.filter(e => e.id !== eid);
      saveState(state);
      renderAll(state);
    };
  });
}

function renderAll(state) {
  renderBudgetHero(state);
  renderCatList(state);
  const selected = document.querySelector('.cat-btn[class*="active-"]');
  const selId = selected ? selected.dataset.id : null;
  renderCatBtns(state, selId);
  renderLedger(state);
}

// ─── 추가 버튼 ───
function initAdd(state) {
  document.getElementById('btnAdd').onclick = () => {
    const amtRaw = parseFloat(document.getElementById('inputAmt').value);
    if (!amtRaw || amtRaw <= 0) { alert('금액을 입력해주세요'); return; }
    const selected = document.querySelector('.cat-btn[class*="active-"]');
    if (!selected) { alert('카테고리를 선택해주세요'); return; }
    const memo = document.getElementById('inputMemo').value.trim();
    const now = new Date();
    const dateStr = `${now.getMonth()+1}/${now.getDate()}`;
    const entry = {
      id: Date.now().toString(),
      catId: selected.dataset.id,
      amount: Math.round(amtRaw),
      memo: memo,
      date: dateStr,
    };
    state.entries.push(entry);
    saveState(state);
    document.getElementById('inputAmt').value = '';
    document.getElementById('inputMemo').value = '';
    renderAll(state);
  };

  // Enter key on memo
  document.getElementById('inputMemo').addEventListener('keydown', e => {
    if (e.key === 'Enter') document.getElementById('btnAdd').click();
  });
}

// ─── 월 마감 ───
function initReset(state) {
  document.getElementById('btnReset').onclick = () => {
    const rem = remaining(state);
    if (!confirm(`월 마감을 실행합니다.\n남은 금액: ${fmtSigned(rem)}\n${rem > 0 ? '이월 처리됩니다.' : '다음 달에 차감됩니다.'}\n계속하시겠어요?`)) return;

    // 이월 가능 카테고리의 남은 금액 합산
    const used = usedByCategory(state);
    let carryAmt = 0;
    state.categories.forEach(c => {
      if (c.carryover) {
        carryAmt += (c.budget - (used[c.id] || 0));
      }
    });
    // 음수면 차감, 양수면 이월 (MAX_CARRYOVER 제한)
    const newCarry = Math.max(-MAX_CARRYOVER, Math.min(MAX_CARRYOVER, carryAmt));

    const now = new Date();
    const newState = {
      month: now.getFullYear() * 100 + (now.getMonth() + 1),
      categories: JSON.parse(JSON.stringify(state.categories)),
      carryover: newCarry,
      entries: [],
    };
    // Update state in place
    Object.assign(state, newState);
    saveState(state);
    renderAll(state);
  };
}

// ─── 카테고리 편집 모달 ───
function initCatEdit(state) {
  const overlay = document.getElementById('modalOverlay');
  const closeBtn = document.getElementById('modalClose');
  const editList = document.getElementById('catEditList');
  const addCatBtn = document.getElementById('btnAddCat');
  const saveBtn = document.getElementById('btnSaveCat');

  document.getElementById('btnEditCat').onclick = () => {
    renderCatEditList(state.categories);
    overlay.classList.add('open');
  };
  closeBtn.onclick = () => overlay.classList.remove('open');
  overlay.onclick = (e) => { if (e.target === overlay) overlay.classList.remove('open'); };

  addCatBtn.onclick = () => {
    const newCat = {
      id: 'cat_' + Date.now(),
      name: '새 카테고리',
      budget: 50000,
      carryover: false,
      color: '#888888',
    };
    state.categories.push(newCat);
    renderCatEditList(state.categories);
  };

  saveBtn.onclick = () => {
    // read from DOM
    const rows = editList.querySelectorAll('.cat-edit-row');
    const updated = [];
    rows.forEach(row => {
      const id = row.dataset.id;
      const name = row.querySelector('.cat-edit-name').value.trim() || '카테고리';
      const budget = parseInt(row.querySelector('.cat-edit-budget').value) || 0;
      const carryover = row.querySelector('.modal-check').checked;
      const existing = state.categories.find(c => c.id === id);
      updated.push({
        id,
        name,
        budget,
        carryover,
        color: existing ? existing.color : '#888888',
      });
    });
    state.categories = updated;
    saveState(state);
    overlay.classList.remove('open');
    renderAll(state);
  };

  function renderCatEditList(cats) {
    editList.innerHTML = '';
    cats.forEach(c => {
      const row = document.createElement('div');
      row.className = 'cat-edit-row';
      row.dataset.id = c.id;
      row.innerHTML = `
        <input class="cat-edit-name" type="text" value="${c.name}" placeholder="이름">
        <input class="cat-edit-budget" type="number" value="${c.budget}" placeholder="예산">
        <label style="font-size:11px;color:var(--text-hint);display:flex;align-items:center;gap:3px;white-space:nowrap">
          <input type="checkbox" class="modal-check" ${c.carryover ? 'checked' : ''}> 이월
        </label>
        <button class="cat-del-btn">×</button>
      `;
      row.querySelector('.cat-del-btn').onclick = () => {
        state.categories = state.categories.filter(x => x.id !== c.id);
        renderCatEditList(state.categories);
      };
      editList.appendChild(row);
    });
  }
}

// ─── 총자산 시스템 ───
const KEY_ASSETS = 'bcs_assets_v1';
const ASSET_PALETTE = ['#2563a8','#2d8f6f','#a85c2d','#7c3a9e','#b34a4a','#4a7a4a','#8a6a2a','#3a6a8a'];

function loadAssets() {
  try {
    const raw = localStorage.getItem(KEY_ASSETS);
    if (!raw) return [];
    return JSON.parse(raw);
  } catch(e) { return []; }
}

function saveAssets(items) {
  localStorage.setItem(KEY_ASSETS, JSON.stringify(items));
}

function totalAssets(items) {
  return items.reduce((s, i) => s + (i.amount || 0), 0);
}

// 원형 그래프 그리기 (canvas)
function drawDonut(canvasId, items, opts = {}) {
  const canvas = document.getElementById(canvasId);
  if (!canvas) return;
  const ctx = canvas.getContext('2d');
  const W = canvas.width, H = canvas.height;
  ctx.clearRect(0, 0, W, H);

  const total = totalAssets(items);
  const cx = W / 2, cy = H / 2;
  const r = opts.r || (W / 2 - (opts.gap || 4));
  const innerR = opts.innerR || r * 0.58;

  if (total === 0 || items.length === 0) {
    ctx.beginPath();
    ctx.arc(cx, cy, r, 0, Math.PI * 2);
    ctx.strokeStyle = '#e8e6e0';
    ctx.lineWidth = r - innerR;
    ctx.stroke();
    return;
  }

  let startAngle = -Math.PI / 2;
  items.forEach((item, i) => {
    const slice = (item.amount / total) * Math.PI * 2;
    const color = ASSET_PALETTE[i % ASSET_PALETTE.length];
    const midR = (r + innerR) / 2;
    const gap = opts.gapAngle || 0.025;

    ctx.beginPath();
    ctx.arc(cx, cy, midR, startAngle + gap, startAngle + slice - gap);
    ctx.strokeStyle = color;
    ctx.lineWidth = r - innerR;
    ctx.lineCap = 'round';
    ctx.stroke();

    startAngle += slice;
  });
}

function renderAssetCard(items) {
  const total = totalAssets(items);
  document.getElementById('assetTotalDisplay').textContent =
    total > 0 ? total.toLocaleString('ko-KR') + '원' : '—';
  const count = items.length;
  document.getElementById('assetSubDisplay').textContent =
    count > 0 ? `${count}개 항목 · 탭하여 상세 보기` : '탭하여 자산 입력';
  drawDonut('assetMiniDonut', items, { r: 22, innerR: 13, gap: 2, gapAngle: 0.04 });
}

function renderAssetModal(items) {
  const total = totalAssets(items);
  document.getElementById('assetModalTotal').textContent =
    total > 0 ? total.toLocaleString('ko-KR') + '원' : '0원';
  drawDonut('assetDonut', items, { r: 88, innerR: 52, gap: 4, gapAngle: 0.03 });

  // Legend
  const legend = document.getElementById('assetLegend');
  legend.innerHTML = '';
  if (items.length === 0) {
    legend.innerHTML = '<div class="asset-empty" style="grid-column:1/-1">아직 자산 항목이 없어요</div>';
  } else {
    items.forEach((item, i) => {
      const color = ASSET_PALETTE[i % ASSET_PALETTE.length];
      const pct = total > 0 ? ((item.amount / total) * 100).toFixed(1) : '0.0';
      const div = document.createElement('div');
      div.className = 'legend-item';
      div.innerHTML = `
        <div class="legend-dot" style="background:${color}"></div>
        <div class="legend-info">
          <div class="legend-name">${item.name}</div>
          <div class="legend-amt">${(item.amount || 0).toLocaleString('ko-KR')}원</div>
          <div class="legend-pct">${pct}%</div>
        </div>
      `;
      legend.appendChild(div);
    });
  }
}

function renderAssetEditList(items) {
  const list = document.getElementById('assetItemList');
  list.innerHTML = '';
  items.forEach((item, i) => {
    const color = ASSET_PALETTE[i % ASSET_PALETTE.length];
    const row = document.createElement('div');
    row.className = 'asset-item-row';
    row.dataset.idx = i;
    row.innerHTML = `
      <div style="width:10px;height:10px;border-radius:50%;background:${color};flex-shrink:0"></div>
      <input class="asset-item-name" type="text" value="${item.name}" placeholder="항목명 (예: 유동자산)">
      <input class="asset-item-amt" type="number" value="${item.amount}" placeholder="금액">
      <button class="asset-item-del">×</button>
    `;
    row.querySelector('.asset-item-del').onclick = () => {
      items.splice(i, 1);
      renderAssetEditList(items);
      renderAssetModal(items);
    };
    // Live update on change
    const nameInput = row.querySelector('.asset-item-name');
    const amtInput = row.querySelector('.asset-item-amt');
    nameInput.oninput = () => { item.name = nameInput.value; renderAssetModal(items); };
    amtInput.oninput = () => { item.amount = parseInt(amtInput.value) || 0; renderAssetModal(items); };
    list.appendChild(row);
  });
}

function initAsset() {
  let items = loadAssets();

  function refresh() {
    renderAssetCard(items);
    renderAssetModal(items);
    renderAssetEditList(items);
  }

  // Open modal
  document.getElementById('assetCard').onclick = () => {
    items = loadAssets();
    refresh();
    document.getElementById('assetModalOverlay').classList.add('open');
  };

  // Close modal
  document.getElementById('assetModalClose').onclick = () => {
    document.getElementById('assetModalOverlay').classList.remove('open');
  };
  document.getElementById('assetModalOverlay').onclick = (e) => {
    if (e.target === document.getElementById('assetModalOverlay'))
      document.getElementById('assetModalOverlay').classList.remove('open');
  };

  // Add item
  document.getElementById('btnAddAssetItem').onclick = () => {
    items.push({ name: '새 항목', amount: 0 });
    renderAssetEditList(items);
    renderAssetModal(items);
  };

  // Save
  document.getElementById('btnSaveAsset').onclick = () => {
    // Read from DOM inputs
    const rows = document.getElementById('assetItemList').querySelectorAll('.asset-item-row');
    const updated = [];
    rows.forEach(row => {
      const name = row.querySelector('.asset-item-name').value.trim() || '항목';
      const amount = parseInt(row.querySelector('.asset-item-amt').value) || 0;
      updated.push({ name, amount });
    });
    items = updated;
    saveAssets(items);
    renderAssetCard(items);
    renderAssetModal(items);
    document.getElementById('assetModalOverlay').classList.remove('open');
  };

  // Initial render
  refresh();
}

// ─── JSON 내보내기 / 불러오기 ───
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2200);
}

function initBackup() {
  // 내보내기
  document.getElementById('btnExport').onclick = () => {
    const now = new Date();
    const stamp = `${now.getFullYear()}${String(now.getMonth()+1).padStart(2,'0')}${String(now.getDate()).padStart(2,'0')}`;
    const payload = {
      exportedAt: now.toISOString(),
      version: 2,
      state: loadState(),
      assets: loadAssets(),
    };
    const blob = new Blob([JSON.stringify(payload, null, 2)], { type: 'application/json' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `budget-backup-${stamp}.json`;
    a.click();
    URL.revokeObjectURL(url);
    showToast('백업 파일을 저장했어요');
  };

  // 불러오기 — 버튼 클릭 → 파일 선택창
  document.getElementById('btnImport').onclick = () => {
    document.getElementById('importFile').click();
  };

  document.getElementById('importFile').onchange = (e) => {
    const file = e.target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = (ev) => {
      try {
        const data = JSON.parse(ev.target.result);
        // 버전 체크 및 필드 검증
        if (!data.state || !data.assets) throw new Error('형식 오류');
        if (!confirm('현재 데이터를 백업 파일로 덮어씁니다.\n계속하시겠어요?')) return;
        saveState(data.state);
        saveAssets(data.assets);
        // 앱 전체 재초기화
        location.reload();
      } catch(err) {
        showToast('파일을 읽을 수 없어요. JSON 형식을 확인해주세요');
      }
    };
    reader.readAsText(file);
    // 같은 파일 재선택 가능하도록 초기화
    e.target.value = '';
  };
}

// ─── 초기화 ───
function init() {
  const state = loadState();
  renderDate();
  renderAll(state);
  initAdd(state);
  initReset(state);
  initCatEdit(state);
  initAsset();
  initBackup();
}

init();
</script>
</body>
</html>
