[index.html](https://github.com/user-attachments/files/25481183/index.html)
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0" />
  <meta name="theme-color" content="#0f0f0f" />
  <meta name="apple-mobile-web-app-capable" content="yes" />
  <meta name="apple-mobile-web-app-status-bar-style" content="black" />
  <title>レシートTRACKER</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; -webkit-tap-highlight-color: transparent; }
    body { background: #0f0f0f; color: #f0ece0; font-family: 'Hiragino Kaku Gothic ProN', 'Noto Sans JP', sans-serif; min-height: 100vh; }
    .app { max-width: 480px; margin: 0 auto; padding: 28px 16px 100px; }
    .header { margin-bottom: 20px; }
    .header h1 { font-size: 28px; font-weight: 900; letter-spacing: -1px; color: #f5e642; display: inline; }
    .header span { font-size: 14px; color: #666; margin-left: 8px; letter-spacing: .1em; }
    .header p { font-size: 12px; color: #555; margin-top: 4px; }

    /* Tabs */
    .tabs { display: flex; gap: 3px; margin-bottom: 4px; }
    .tab-btn { flex: 1; padding: 11px 0; border: none; border-radius: 10px; font-size: 18px; cursor: pointer; background: #1a1a1a; color: #777; font-family: inherit; transition: background .15s; }
    .tab-btn.active { background: #f5e642; color: #0f0f0f; }
    .tab-labels { display: flex; gap: 3px; margin-bottom: 24px; }
    .tab-label { flex: 1; text-align: center; font-size: 10px; color: #555; padding-top: 4px; }
    .tab-label.active { color: #f5e642; }

    /* Buttons */
    .btn-primary { width: 100%; padding: 16px; background: #f5e642; color: #0f0f0f; border: none; border-radius: 12px; font-size: 16px; font-weight: 800; cursor: pointer; font-family: inherit; display: flex; align-items: center; justify-content: center; gap: 12px; margin-bottom: 12px; }
    .btn-secondary { width: 100%; padding: 13px; background: none; color: #666; border: 1px solid #333; border-radius: 12px; font-size: 14px; cursor: pointer; font-family: inherit; }
    .btn-danger { background: #ff4444; color: #fff; border: none; border-radius: 8px; padding: 10px; font-weight: 700; cursor: pointer; font-family: inherit; flex: 1; font-size: 14px; }
    .btn-cancel { background: none; color: #888; border: 1px solid #444; border-radius: 8px; padding: 10px; cursor: pointer; font-family: inherit; flex: 1; font-size: 14px; }

    /* Cards */
    .card { background: #141414; border-radius: 14px; border: 1px solid #222; margin-bottom: 10px; overflow: hidden; }
    .card-header { padding: 14px 16px; cursor: pointer; }
    .card-body { border-top: 1px solid #1e1e1e; padding: 12px 16px; }
    .item-row { display: flex; justify-content: space-between; align-items: center; padding: 8px 0; border-bottom: 1px solid #1a1a1a; font-size: 13px; }
    .item-row:last-child { border-bottom: none; }
    .price { color: #f5e642; font-weight: 600; }
    .store-name { font-size: 15px; font-weight: 700; color: #f5e642; }
    .meta { font-size: 12px; color: #555; margin-top: 3px; }

    /* Scan */
    .preview-img { width: 100%; max-height: 280px; object-fit: contain; background: #111; display: block; border-radius: 12px; margin-bottom: 16px; border: 1px solid #333; }
    .scan-result { background: #141414; border-radius: 16px; padding: 20px; border: 1px solid #222; margin-bottom: 12px; }
    .result-store { font-size: 20px; font-weight: 800; color: #f5e642; }
    .result-date { font-size: 13px; color: #666; margin-top: 4px; }
    .result-items { border-top: 1px solid #222; margin-top: 14px; padding-top: 14px; }
    .result-item { display: flex; justify-content: space-between; padding: 9px 0; border-bottom: 1px solid #1a1a1a; font-size: 14px; }
    .result-total { display: flex; justify-content: space-between; padding-top: 12px; }
    .total-label { color: #777; font-size: 14px; }
    .total-value { font-size: 20px; font-weight: 800; }

    /* Loading */
    .loading { text-align: center; padding: 40px; }
    .spinner { font-size: 40px; display: inline-block; animation: spin .8s linear infinite; }
    @keyframes spin { to { transform: rotate(360deg); } }

    /* Error */
    .error-box { background: #1e0808; border: 1px solid #ff4444; border-radius: 10px; padding: 14px; font-size: 13px; color: #ff8888; margin-bottom: 12px; }

    /* Dup warning */
    .dup-box { background: #1a0a00; border: 2px solid #ff6b35; border-radius: 14px; padding: 20px; margin-bottom: 16px; }
    .dup-title { font-size: 18px; color: #ff6b35; margin-bottom: 8px; }
    .dup-meta { font-size: 13px; color: #aaa; margin-bottom: 16px; }
    .dup-items { background: #111; border-radius: 10px; padding: 12px; margin-bottom: 16px; }

    /* Search */
    .search-row { display: flex; gap: 8px; margin-bottom: 20px; }
    .search-input { flex: 1; padding: 14px 16px; background: #161616; border: 1px solid #2a2a2a; border-radius: 12px; color: #f0ece0; font-size: 15px; outline: none; font-family: inherit; }
    .search-btn { padding: 14px 20px; background: #f5e642; color: #0f0f0f; border: none; border-radius: 12px; font-size: 15px; font-weight: 700; cursor: pointer; font-family: inherit; }

    /* Confirm delete */
    .confirm-box { background: #2a0a0a; padding: 12px 16px; border-bottom: 1px solid #ff4444; }
    .confirm-msg { font-size: 13px; color: #ff8888; margin-bottom: 10px; }
    .confirm-btns { display: flex; gap: 8px; }

    /* Edit store */
    .edit-row { display: flex; align-items: center; gap: 6px; flex: 1; }
    .edit-input { flex: 1; background: #222; border: 1px solid #f5e642; border-radius: 8px; padding: 6px 10px; color: #f0ece0; font-size: 15px; font-weight: 700; outline: none; font-family: inherit; }
    .edit-confirm { background: #f5e642; color: #0f0f0f; border: none; border-radius: 8px; padding: 6px 12px; font-weight: 700; cursor: pointer; font-size: 13px; font-family: inherit; }
    .edit-cancel { background: none; color: #666; border: 1px solid #444; border-radius: 8px; padding: 6px 10px; cursor: pointer; font-size: 13px; }
    .edit-btn { background: none; border: 1px solid #444; color: #777; border-radius: 6px; padding: 2px 7px; cursor: pointer; font-size: 11px; font-family: inherit; margin-left: 6px; }
    .delete-btn { background: #2a0a0a; border: 1px solid #ff4444; color: #ff6666; border-radius: 8px; padding: 5px 10px; cursor: pointer; font-size: 14px; font-family: inherit; }

    .empty { text-align: center; padding: 60px 20px; color: #555; }
    .empty-icon { font-size: 48px; margin-bottom: 12px; }
    .header-row { display: flex; align-items: center; gap: 8px; margin-bottom: 4px; }
    .avg-box { background: #181800; border-radius: 12px; padding: 12px 16px; display: flex; justify-content: space-between; margin-top: 8px; }

    /* Store name edit in scan */
    .store-edit-row { display: flex; align-items: center; gap: 8px; margin-bottom: 4px; }
    .store-edit-input { background: #222; border: 1px solid #f5e642; border-radius: 8px; padding: 6px 10px; color: #f0ece0; font-size: 18px; font-weight: 800; outline: none; font-family: inherit; width: 100%; }

    /* Config */
    .config-box { background: #141414; border-radius: 14px; border: 1px solid #222; padding: 20px; margin-bottom: 16px; }
    .config-label { font-size: 13px; color: #888; margin-bottom: 8px; }
    .config-input { width: 100%; padding: 12px 14px; background: #0f0f0f; border: 1px solid #333; border-radius: 10px; color: #f0ece0; font-size: 14px; outline: none; font-family: inherit; margin-bottom: 12px; }
    .config-save { width: 100%; padding: 13px; background: #f5e642; color: #0f0f0f; border: none; border-radius: 10px; font-size: 15px; font-weight: 700; cursor: pointer; font-family: inherit; }
    .config-ok { color: #4caf50; font-size: 13px; margin-top: 8px; }
  </style>
</head>
<body>
<div class="app">
  <div class="header">
    <h1>レシート</h1><span>TRACKER</span>
    <p>レシートを撮影して家計を記録</p>
  </div>

  <div class="tabs">
    <button class="tab-btn active" onclick="switchTab('scan')" id="tab-scan">📷</button>
    <button class="tab-btn" onclick="switchTab('search')" id="tab-search">🔍</button>
    <button class="tab-btn" onclick="switchTab('receipts')" id="tab-receipts">🧾</button>
    <button class="tab-btn" onclick="switchTab('list')" id="tab-list">📋</button>
    <button class="tab-btn" onclick="switchTab('config')" id="tab-config">⚙️</button>
  </div>
  <div class="tab-labels">
    <div class="tab-label active" id="label-scan">スキャン</div>
    <div class="tab-label" id="label-search">検索</div>
    <div class="tab-label" id="label-receipts">レシート</div>
    <div class="tab-label" id="label-list">明細</div>
    <div class="tab-label" id="label-config">設定</div>
  </div>

  <!-- SCAN -->
  <div id="page-scan">
    <div id="scan-buttons">
      <label class="btn-primary" style="margin-bottom:12px">
        <input type="file" accept="image/*" capture="environment" id="camera-input" style="display:none" />
        <span style="font-size:32px">📷</span> カメラで撮影する
      </label>
      <label class="btn-primary" style="background:#1e1e1e;color:#f0ece0;border:2px solid #333">
        <input type="file" accept="image/*" id="gallery-input" style="display:none" />
        <span style="font-size:28px">🖼️</span> 写真から選ぶ
      </label>
    </div>
    <div id="scan-loading" style="display:none" class="loading">
      <div class="spinner">⟳</div>
      <p style="color:#777;font-size:14px;margin-top:12px">AIが解析中...</p>
    </div>
    <div id="scan-error" style="display:none" class="error-box"></div>
    <img id="scan-preview" class="preview-img" style="display:none" />
    <div id="scan-dup" style="display:none" class="dup-box"></div>
    <div id="scan-result" style="display:none"></div>
  </div>

  <!-- SEARCH -->
  <div id="page-search" style="display:none">
    <div class="search-row">
      <input class="search-input" id="search-input" placeholder="例：醤油、牛乳、パン..." />
      <button class="search-btn" onclick="doSearch()">検索</button>
    </div>
    <div id="search-results"></div>
  </div>

  <!-- RECEIPTS -->
  <div id="page-receipts" style="display:none">
    <div id="receipts-list"></div>
  </div>

  <!-- LIST -->
  <div id="page-list" style="display:none">
    <div id="items-list"></div>
  </div>

  <!-- CONFIG -->
  <div id="page-config" style="display:none">
    <div class="config-box">
      <div class="config-label">⚙️ Cloudflare Worker URL</div>
      <input class="config-input" id="worker-url" placeholder="https://your-worker.your-name.workers.dev" />
      <div class="config-label">🔑 Anthropic API Key</div>
      <input class="config-input" id="api-key-input" type="password" placeholder="sk-ant-api03-..." />
      <button class="config-save" onclick="saveConfig()">💾 保存する</button>
      <div id="config-msg"></div>
    </div>
    <div class="config-box" style="font-size:13px;color:#666;line-height:1.8">
      <p style="color:#f5e642;font-weight:700;margin-bottom:8px">📋 設定手順</p>
      <p>1. Cloudflare Workers でプロキシを作成</p>
      <p>2. Worker URLをここに入力</p>
      <p>3. Anthropic API Keyを入力</p>
      <p>4. 「保存する」をタップ</p>
    </div>
  </div>
</div>

<script>
const STORAGE_KEY = 'receipt-items-v3';
const CONFIG_KEY = 'receipt-config';

// ── Config ──────────────────────────────────
function loadConfig() {
  try { return JSON.parse(localStorage.getItem(CONFIG_KEY) || '{}'); } catch { return {}; }
}
function saveConfig() {
  const url = document.getElementById('worker-url').value.trim();
  const key = document.getElementById('api-key-input').value.trim();
  localStorage.setItem(CONFIG_KEY, JSON.stringify({ workerUrl: url, apiKey: key }));
  document.getElementById('config-msg').innerHTML = '<div class="config-ok">✅ 保存しました！</div>';
  setTimeout(() => document.getElementById('config-msg').innerHTML = '', 2000);
}

// ── Data ────────────────────────────────────
function loadItems() {
  try { return JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]'); } catch { return []; }
}
function saveItems(items) {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(items));
}

// ── Tabs ─────────────────────────────────────
const TABS = ['scan','search','receipts','list','config'];
function switchTab(name) {
  TABS.forEach(t => {
    document.getElementById('page-' + t).style.display = t === name ? '' : 'none';
    document.getElementById('tab-' + t).classList.toggle('active', t === name);
    document.getElementById('label-' + t).classList.toggle('active', t === name);
  });
  if (name === 'receipts') renderReceipts();
  if (name === 'list') renderList();
  if (name === 'config') {
    const cfg = loadConfig();
    document.getElementById('worker-url').value = cfg.workerUrl || '';
    document.getElementById('api-key-input').value = cfg.apiKey || '';
  }
}

// ── Scan ────────────────────────────────────
let currentParsed = null;
let currentStore = '';

document.getElementById('camera-input').addEventListener('change', handleFileInput);
document.getElementById('gallery-input').addEventListener('change', handleFileInput);
document.getElementById('search-input').addEventListener('keydown', e => { if (e.key === 'Enter') doSearch(); });

async function handleFileInput(e) {
  const file = e.target.files?.[0];
  if (!file) return;
  e.target.value = '';

  const cfg = loadConfig();
  if (!cfg.workerUrl || !cfg.apiKey) {
    alert('⚙️ 設定タブでWorker URLとAPI Keyを入力してください');
    switchTab('config');
    return;
  }

  // Show preview
  const preview = document.getElementById('scan-preview');
  preview.src = URL.createObjectURL(file);
  preview.style.display = 'block';

  // Hide buttons, show loading
  document.getElementById('scan-buttons').style.display = 'none';
  document.getElementById('scan-loading').style.display = '';
  document.getElementById('scan-error').style.display = 'none';
  document.getElementById('scan-result').style.display = 'none';
  document.getElementById('scan-dup').style.display = 'none';

  try {
    const base64 = await fileToBase64(file);
    const parsed = await analyzeReceipt(base64, file.type || 'image/jpeg', cfg);
    currentParsed = parsed;
    currentStore = parsed.store || '';

    const items = loadItems();
    if (isDuplicate(items, parsed)) {
      showDupWarning(parsed);
    } else {
      showResult(parsed);
    }
  } catch (err) {
    document.getElementById('scan-error').textContent = '読み取りに失敗しました: ' + err.message;
    document.getElementById('scan-error').style.display = '';
    document.getElementById('scan-buttons').style.display = '';
  }
  document.getElementById('scan-loading').style.display = 'none';
}

function fileToBase64(file) {
  return new Promise((res, rej) => {
    const r = new FileReader();
    r.onload = () => res(r.result.split(',')[1]);
    r.onerror = rej;
    r.readAsDataURL(file);
  });
}

async function analyzeReceipt(base64, mediaType, cfg) {
  const res = await fetch(cfg.workerUrl, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json', 'x-api-key': cfg.apiKey },
    body: JSON.stringify({
      model: 'claude-sonnet-4-20250514',
      max_tokens: 1000,
      messages: [{ role: 'user', content: [
        { type: 'image', source: { type: 'base64', media_type: mediaType, data: base64 } },
        { type: 'text', text: 'このレシート画像から情報をJSON形式で抽出してください。JSONのみ返してください。重要：割引・値引き・クーポン行は直前の商品価格から差し引き、割引行自体・消費税・小計・合計はitemsに含めないでください。{"date":"YYYY-MM-DD or null","time":"HH:MM or null","store":"店名","items":[{"name":"商品名","price":数値}]}' }
      ]}]
    })
  });
  const data = await res.json();
  const text = (data.content || []).map(b => b.text || '').join('');
  return JSON.parse(text.replace(/```json|```/g, '').trim());
}

function isDuplicate(existing, parsed) {
  if (!parsed.store || !parsed.date || !parsed.items?.length) return false;
  const newTotal = parsed.items.reduce((s, i) => s + (i.price || 0), 0);
  const groups = {};
  for (const item of existing) {
    if (item.store !== parsed.store || item.date !== parsed.date) continue;
    const k = item.receiptId || (item.date + '_' + item.store + '_' + item.time);
    groups[k] = (groups[k] || 0) + (item.price || 0);
  }
  for (const t of Object.values(groups)) {
    if (Math.abs(t - newTotal) <= 50) return true;
  }
  const same = existing.filter(i => i.store === parsed.store && i.date === parsed.date);
  if (!same.length) return false;
  const names = new Set(same.map(i => i.name));
  const newNames = parsed.items.map(i => i.name);
  return newNames.filter(n => names.has(n)).length / newNames.length >= 0.7;
}

function showDupWarning(parsed) {
  const total = parsed.items?.reduce((s, i) => s + (i.price || 0), 0) || 0;
  const itemsHtml = (parsed.items || []).map(i =>
    `<div class="item-row"><span>${i.name}</span><span style="color:#ff6b35">¥${(i.price||0).toLocaleString()}</span></div>`
  ).join('');
  document.getElementById('scan-dup').innerHTML = `
    <div class="dup-title">🚫 重複レシート検出！</div>
    <div class="dup-meta"><strong>${parsed.store}</strong>（${parsed.date}）の同じ内容がすでに登録されています。</div>
    <div class="dup-items">${itemsHtml}</div>
    <div style="display:flex;gap:8px">
      <button class="btn-primary" style="flex:2;margin:0;padding:12px" onclick="resetScan()">✕ 登録しない</button>
      <button class="btn-cancel" style="flex:1" onclick="doSave(true)">強制登録</button>
    </div>
  `;
  document.getElementById('scan-dup').style.display = '';
}

function showResult(parsed) {
  const total = parsed.items?.reduce((s, i) => s + (i.price || 0), 0) || 0;
  const itemsHtml = (parsed.items || []).map(i =>
    `<div class="result-item"><span>${i.name}</span><span class="price">¥${(i.price||0).toLocaleString()}</span></div>`
  ).join('');
  document.getElementById('scan-result').innerHTML = `
    <div class="scan-result">
      <div style="display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:14px">
        <div style="flex:1">
          <div class="store-edit-row">
            <input class="store-edit-input" id="store-input" value="${escHtml(currentStore)}" placeholder="店名" />
          </div>
          <div class="result-date">${parsed.date || '日付不明'} ${parsed.time || ''}</div>
        </div>
        <span style="font-size:28px;margin-left:8px">🏪</span>
      </div>
      <div class="result-items">
        ${itemsHtml}
        <div class="result-total">
          <span class="total-label">合計</span>
          <span class="total-value">¥${total.toLocaleString()}</span>
        </div>
      </div>
    </div>
    <button class="btn-primary" onclick="doSave(false)">💾 登録する</button>
    <button class="btn-secondary" onclick="resetScan()">やり直す</button>
  `;
  document.getElementById('scan-result').style.display = '';
}

function doSave(force) {
  if (!currentParsed) return;
  const storeInput = document.getElementById('store-input');
  const storeName = storeInput ? storeInput.value.trim() : currentStore;
  const items = loadItems();
  if (!force && isDuplicate(items, currentParsed)) return;
  const rid = currentParsed.date + '_' + storeName + '_' + currentParsed.time + '_' + Date.now();
  const entries = (currentParsed.items || []).map(item => ({
    id: Date.now() + Math.random(), receiptId: rid,
    date: currentParsed.date, time: currentParsed.time,
    store: storeName, name: item.name, price: item.price,
  }));
  saveItems([...items, ...entries]);
  alert('✅ ' + entries.length + '件を登録しました！');
  resetScan();
}

function resetScan() {
  currentParsed = null; currentStore = '';
  document.getElementById('scan-buttons').style.display = '';
  document.getElementById('scan-preview').style.display = 'none';
  document.getElementById('scan-result').style.display = 'none';
  document.getElementById('scan-dup').style.display = 'none';
  document.getElementById('scan-error').style.display = 'none';
}

// ── Search ──────────────────────────────────
function doSearch() {
  const q = document.getElementById('search-input').value.trim().toLowerCase();
  if (!q) return;
  const items = loadItems();
  const found = items.filter(i => i.name.toLowerCase().includes(q)).reverse();
  const el = document.getElementById('search-results');
  if (!found.length) {
    el.innerHTML = `<div class="empty"><div class="empty-icon">🔍</div><p>「${escHtml(q)}」は見つかりませんでした</p></div>`;
    return;
  }
  const avg = Math.round(found.reduce((s, i) => s + (i.price || 0), 0) / found.length);
  const rows = found.map(i => `
    <div class="card" style="padding:14px 16px">
      <div style="display:flex;justify-content:space-between;align-items:flex-start">
        <div>
          <div style="font-weight:700;font-size:15px">${escHtml(i.name)}</div>
          <div style="font-size:12px;color:#777;margin-top:3px">${escHtml(i.store||'')}</div>
          <div style="font-size:12px;color:#555">${i.date||''} ${i.time||''}</div>
        </div>
        <span class="price" style="font-size:20px;font-weight:800">¥${(i.price||0).toLocaleString()}</span>
      </div>
    </div>
  `).join('');
  el.innerHTML = `
    <p style="font-size:13px;color:#666;margin-bottom:12px">「${escHtml(q)}」— ${found.length}件</p>
    ${rows}
    ${found.length > 1 ? `<div class="avg-box"><span style="color:#777;font-size:14px">平均価格</span><span class="price" style="font-weight:700">¥${avg.toLocaleString()}</span></div>` : ''}
  `;
}

// ── Receipts ────────────────────────────────
let confirmKey = null;
let editKey = null;

function groupReceipts(items) {
  const map = {};
  for (const item of items) {
    const k = item.receiptId || (item.date + '_' + item.store + '_' + item.time);
    if (!map[k]) map[k] = { key: k, date: item.date, time: item.time, store: item.store, items: [] };
    map[k].items.push(item);
  }
  return Object.values(map).sort((a, b) => (a.date||'') < (b.date||'') ? 1 : -1);
}

function renderReceipts() {
  const items = loadItems();
  const receipts = groupReceipts(items);
  const el = document.getElementById('receipts-list');
  if (!receipts.length) {
    el.innerHTML = `<div class="empty"><div class="empty-icon">🧾</div><p>まだデータがありません</p></div>`;
    return;
  }
  el.innerHTML = `<p style="font-size:13px;color:#666;margin-bottom:12px">登録済み: ${receipts.length}枚</p>` +
    receipts.map(r => renderReceiptCard(r)).join('');
}

function renderReceiptCard(r) {
  const total = r.items.reduce((s, i) => s + (i.price || 0), 0);
  const isConfirm = confirmKey === r.key;
  const isEdit = editKey === r.key;
  const isOpen = document.getElementById('body-' + CSS.escape(r.key)) ? document.getElementById('body-' + CSS.escape(r.key)).style.display !== 'none' : false;

  const confirmHtml = isConfirm ? `
    <div class="confirm-box">
      <div class="confirm-msg">「${escHtml(r.store)}」(${r.date}) を削除しますか？</div>
      <div class="confirm-btns">
        <button class="btn-danger" onclick="doDeleteReceipt('${escAttr(r.key)}')">削除する</button>
        <button class="btn-cancel" onclick="setConfirm(null)">キャンセル</button>
      </div>
    </div>
  ` : '';

  const headerHtml = isEdit ? `
    <div class="edit-row">
      <input class="edit-input" id="edit-input-${escAttr(r.key)}" value="${escHtml(r.store||'')}" />
      <button class="edit-confirm" onclick="doRenameStore('${escAttr(r.key)}')">確定</button>
      <button class="edit-cancel" onclick="setEdit(null)">✕</button>
    </div>
  ` : `
    <div class="header-row" style="flex:1">
      <span class="store-name">${escHtml(r.store||'店名不明')}</span>
      <button class="edit-btn" onclick="setEdit('${escAttr(r.key)}')">✏️</button>
    </div>
    <span style="font-size:15px;font-weight:800;margin-right:4px">¥${total.toLocaleString()}</span>
    <button class="delete-btn" onclick="setConfirm('${escAttr(r.key)}')">🗑️</button>
    <span style="color:#555;font-size:12px;cursor:pointer;margin-left:4px" onclick="toggleBody('${escAttr(r.key)}')">${isOpen?'▲':'▼'}</span>
  `;

  const itemsHtml = r.items.map(i =>
    `<div class="item-row"><span style="color:#ccc">${escHtml(i.name)}</span><span class="price">¥${(i.price||0).toLocaleString()}</span></div>`
  ).join('');

  return `
    <div class="card" id="card-${escAttr(r.key)}" style="border:${isConfirm?'2px solid #ff4444':'1px solid #222'}">
      ${confirmHtml}
      <div class="card-header" style="cursor:default">
        <div style="display:flex;align-items:center;gap:8px;margin-bottom:4px">
          ${headerHtml}
        </div>
        <div class="meta" onclick="toggleBody('${escAttr(r.key)}')" style="cursor:pointer">${r.date||'日付不明'} ${r.time||''} · ${r.items.length}品</div>
      </div>
      <div class="card-body" id="body-${escAttr(r.key)}" style="display:none">
        ${itemsHtml}
        <div class="item-row" style="border:none;padding-top:8px">
          <span style="color:#666;font-size:13px">合計</span>
          <span style="font-weight:700">¥${total.toLocaleString()}</span>
        </div>
      </div>
    </div>
  `;
}

function toggleBody(key) {
  const el = document.getElementById('body-' + key);
  if (!el) return;
  el.style.display = el.style.display === 'none' ? '' : 'none';
}

function setConfirm(key) { confirmKey = key; renderReceipts(); }
function setEdit(key) { editKey = key; renderReceipts(); }

function doDeleteReceipt(key) {
  const items = loadItems().filter(i => {
    const k = i.receiptId || (i.date + '_' + i.store + '_' + i.time);
    return k !== key;
  });
  saveItems(items);
  confirmKey = null;
  renderReceipts();
}

function doRenameStore(key) {
  const input = document.getElementById('edit-input-' + key);
  if (!input) return;
  const newStore = input.value.trim();
  const items = loadItems().map(i => {
    const k = i.receiptId || (i.date + '_' + i.store + '_' + i.time);
    return k === key ? { ...i, store: newStore, receiptId: i.receiptId || key } : i;
  });
  saveItems(items);
  editKey = null;
  renderReceipts();
}

// ── List ────────────────────────────────────
function renderList() {
  const items = [...loadItems()].reverse();
  const el = document.getElementById('items-list');
  if (!items.length) {
    el.innerHTML = `<div class="empty"><div class="empty-icon">📋</div><p>まだデータがありません</p></div>`;
    return;
  }
  el.innerHTML = `<p style="font-size:13px;color:#666;margin-bottom:12px">登録済み: ${items.length}件</p>` +
    items.map(i => `
      <div class="card" style="padding:12px 16px;display:flex;justify-content:space-between;align-items:center">
        <div>
          <div style="font-size:14px;font-weight:600">${escHtml(i.name)}</div>
          <div style="font-size:12px;color:#555;margin-top:2px">${escHtml(i.store||'')} · ${i.date||''}</div>
        </div>
        <div style="display:flex;align-items:center;gap:10px">
          <span class="price" style="font-weight:700">¥${(i.price||0).toLocaleString()}</span>
          <button onclick="doDeleteItem('${i.id}')" style="background:none;border:none;color:#444;cursor:pointer;font-size:18px;padding:4px 6px">✕</button>
        </div>
      </div>
    `).join('');
}

function doDeleteItem(id) {
  const items = loadItems().filter(i => String(i.id) !== String(id));
  saveItems(items);
  renderList();
}

// ── Utils ────────────────────────────────────
function escHtml(s) { return String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;'); }
function escAttr(s) { return String(s||'').replace(/'/g,"\\'"); }
</script>
</body>
</html>
