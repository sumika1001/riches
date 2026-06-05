<html lang="zh-TW">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<meta name="apple-mobile-web-app-capable" content="yes">
<title>投資追蹤</title>
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@300;400;500;700&display=swap" rel="stylesheet">
<style>
*{box-sizing:border-box;margin:0;padding:0;}
body{background:#f2f2f2;color:#1a1d23;font-family:'Noto Sans TC',sans-serif;min-height:100vh;padding-bottom:60px;}
input,button{outline:none;font-family:'Noto Sans TC',sans-serif;}
input[type=number]::-webkit-inner-spin-button{opacity:0.3;}
.app{max-width:480px;margin:0 auto;}
.topbar{display:flex;align-items:center;justify-content:space-between;padding:20px 16px 0;}
.topbar-title{font-size:11px;letter-spacing:0.2em;color:#8892a0;text-transform:uppercase;}
.topbar-date{font-family:monospace;font-size:11px;color:#aab0bb;margin-top:2px;}
.btn-update{background:#fff;border:1px solid #dde1e8;color:#6b7280;font-size:12px;padding:6px 14px;border-radius:8px;cursor:pointer;box-shadow:0 1px 3px rgba(0,0,0,0.06);}
.btn-update.loading{color:#aab0bb;pointer-events:none;}
.price-src{font-size:10px;color:#aab0bb;padding:6px 16px 0;text-align:right;}

/* 大標題：3+3 二排 */
.summary-row{display:grid;gap:10px;padding:10px 16px 0;}
.summary-row-2{grid-template-columns:1fr 1fr;}
.summary-row-3{grid-template-columns:1fr 1fr 1fr;}

.card{background:#fff;border:1px solid #dde1e8;border-radius:12px;padding:14px;border-top-width:2px;box-shadow:0 1px 3px rgba(0,0,0,0.05);}
.card-label{font-size:8px;letter-spacing:0.1em;color:#8892a0;text-transform:uppercase;margin-bottom:4px;}
.card-value{font-family:monospace;font-size:16px;font-weight:700;line-height:1;}
.card-value-sm{font-family:monospace;font-size:12px;font-weight:700;line-height:1;}
.card-note{font-size:8px;color:#aab0bb;margin-top:3px;}
.tabs{display:flex;gap:4px;padding:16px 16px 0;}
.tab{flex:1;padding:8px 0;border-radius:8px;font-size:12px;font-weight:500;cursor:pointer;border:1px solid transparent;background:transparent;color:#8892a0;}
.tab.active{background:#fff;border-color:#dde1e8;color:#1a1d23;box-shadow:0 1px 3px rgba(0,0,0,0.06);}
.section-label{font-size:10px;letter-spacing:0.15em;color:#aab0bb;text-transform:uppercase;margin-bottom:8px;padding:16px 16px 0;}
.stock-card{background:#fff;border:1px solid #dde1e8;border-radius:14px;overflow:hidden;margin:0 16px 10px;box-shadow:0 1px 3px rgba(0,0,0,0.05);}
.stock-card.cleared{opacity:0.5;filter:grayscale(0.3);}
.stock-header{display:flex;align-items:center;justify-content:space-between;padding:14px 16px;cursor:pointer;}
.code-badge{background:#f0f2f5;border:1px solid #dde1e8;font-family:monospace;font-size:11px;color:#6b7280;padding:2px 7px;border-radius:4px;flex-shrink:0;}
.cleared-badge{font-size:9px;background:#f0f2f5;color:#8892a0;padding:2px 6px;border-radius:3px;letter-spacing:0.1em;flex-shrink:0;}

/* 個股 metrics：3+3 */
.metrics-top{display:grid;grid-template-columns:repeat(3,1fr);border-top:1px solid #edf0f4;}
.metrics-bot{display:grid;grid-template-columns:repeat(3,1fr);border-top:1px solid #edf0f4;}
.metric{padding:8px 4px;border-right:1px solid #edf0f4;}
.metric:last-child{border-right:none;}
.metric-label{font-size:8px;letter-spacing:0.05em;color:#aab0bb;text-transform:uppercase;margin-bottom:3px;}
.metric-value{font-family:monospace;font-size:11px;font-weight:700;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.detail{display:none;background:#f7f9fb;border-top:1px solid #edf0f4;padding:16px;}
.detail.open{display:block;}
.signal-box{background:#fff;border:1px solid #dde1e8;border-radius:10px;padding:12px 14px;margin-bottom:12px;}
.pill{display:inline-flex;align-items:center;gap:4px;font-size:11px;padding:3px 10px;border-radius:5px;font-weight:600;}
.add-btn{display:block;width:calc(100% - 32px);margin:0 16px 20px;padding:12px;background:transparent;border:1px dashed #c0c8d4;border-radius:12px;color:#aab0bb;font-size:13px;cursor:pointer;}
.trade-btns{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:12px;}
.btn-buy{padding:10px;background:#dcfce7;border:1px solid #bbf7d0;color:#16a34a;font-size:13px;font-weight:700;border-radius:8px;cursor:pointer;}
.btn-sell{padding:10px;background:#fee2e2;border:1px solid #fecaca;color:#dc2626;font-size:13px;font-weight:700;border-radius:8px;cursor:pointer;}
.btn-delete{width:100%;margin-top:8px;padding:9px;background:transparent;border:1px solid #f0c0c0;color:#dc2626;font-size:12px;border-radius:8px;cursor:pointer;}
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.4);z-index:100;align-items:flex-end;}
.modal-overlay.open{display:flex;}
.modal{background:#fff;border:1px solid #dde1e8;border-radius:20px 20px 0 0;padding:24px 20px 44px;width:100%;max-height:85vh;overflow-y:auto;}
.form-label{font-size:11px;color:#6b7280;margin-bottom:5px;display:block;}
.form-input{width:100%;background:#f7f9fb;border:1px solid #dde1e8;color:#1a1d23;font-size:14px;padding:10px 12px;border-radius:8px;}
.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:12px;}
.btn-cancel{padding:12px 20px;background:#f0f2f5;color:#6b7280;font-size:14px;border:1px solid #dde1e8;border-radius:10px;cursor:pointer;}
.btn-confirm-buy{flex:1;padding:12px;background:#16a34a;color:#fff;font-size:14px;font-weight:700;border:none;border-radius:10px;cursor:pointer;}
.btn-confirm-sell{flex:1;padding:12px;background:#dc2626;color:#fff;font-size:14px;font-weight:700;border:none;border-radius:10px;cursor:pointer;}
.btn-confirm-blue{flex:1;padding:12px;background:#2563eb;color:#fff;font-size:14px;font-weight:700;border:none;border-radius:10px;cursor:pointer;}
.inline-input{background:#fff;border:1px solid #2563eb;color:#1a1d23;font-family:monospace;padding:2px 8px;border-radius:6px;}
/* 交易紀錄 */
.tx-group{background:#fff;border:1px solid #dde1e8;border-radius:14px;overflow:hidden;margin:0 16px 10px;box-shadow:0 1px 3px rgba(0,0,0,0.05);}
.tx-group-header{display:grid;grid-template-columns:1.5fr 1fr 1fr 1fr 28px;align-items:center;padding:12px 14px;cursor:pointer;background:#fff;gap:4px;}
.tx-group-header:hover{background:#f7f9fb;}
.tx-group-body{display:none;border-top:1px solid #edf0f4;}
.tx-group-body.open{display:block;}
.tx-detail-row{display:grid;grid-template-columns:0.8fr 0.6fr 0.8fr 0.8fr 32px;align-items:center;padding:9px 14px;border-bottom:1px solid #edf0f4;gap:4px;}
.tx-detail-row:last-child{border-bottom:none;}
.tx-del-btn{background:transparent;border:none;color:#c0c8d4;font-size:14px;cursor:pointer;padding:0;display:flex;align-items:center;justify-content:center;width:28px;height:28px;border-radius:6px;}
.tx-del-btn:hover{color:#dc2626;background:#fee2e2;}
.tx-empty{padding:28px;text-align:center;font-size:12px;color:#aab0bb;}
.tx-chevron{font-size:11px;color:#aab0bb;transition:transform 0.2s;}
.tx-chevron.open{transform:rotate(90deg);}
/* 分析頁 */
.chart-wrap{padding:0 16px;}
.chart-card{background:#fff;border:1px solid #dde1e8;border-radius:14px;overflow:hidden;margin-bottom:12px;box-shadow:0 1px 3px rgba(0,0,0,0.05);}
.chart-title{font-size:13px;font-weight:600;padding:14px 16px;border-bottom:1px solid #edf0f4;display:flex;align-items:center;justify-content:space-between;}
.pie-container{display:flex;align-items:center;justify-content:space-between;padding:16px;gap:12px;}
.pie-legend{flex:1;min-width:0;}
.legend-item{display:flex;align-items:center;gap:8px;margin-bottom:8px;}
.legend-dot{width:10px;height:10px;border-radius:50%;flex-shrink:0;}
.legend-name{font-size:11px;color:#1a1d23;flex:1;min-width:0;white-space:nowrap;overflow:hidden;text-overflow:ellipsis;}
.legend-pct{font-family:monospace;font-size:11px;font-weight:700;color:#6b7280;flex-shrink:0;}
.rank-row{display:grid;grid-template-columns:1fr 1fr 1fr;align-items:center;padding:10px 16px;border-bottom:1px solid #edf0f4;}
.rank-row:last-child{border-bottom:none;}
.rank-bar-wrap{height:4px;background:#f0f2f5;border-radius:2px;margin-top:4px;overflow:hidden;}
.rank-bar{height:4px;border-radius:2px;}
</style>
</head>
<body>
<div class="app">
  <div class="topbar">
    <div>
      <div class="topbar-title">投資追蹤</div>
      <div class="topbar-date" id="dateLabel"></div>
    </div>
    <div style="display:flex;gap:6px;align-items:center">
      <button class="btn-update" id="autoBtn" onclick="autoFetchPrices()">⟳ 自動更新</button>
      <button class="btn-update" onclick="openPriceModal()">✎ 手動報價</button>
    </div>
  </div>
  <!-- 資料來源說明 -->
  <div class="price-src" id="priceSrc"></div>

  <!-- 大標題卡片：第一排 3張 -->
  <div class="summary-row summary-row-3" style="padding-top:16px">
    <div class="card" style="border-top-color:#2563eb">
      <div class="card-label">總市值</div>
      <div class="card-value-sm" id="s-market" style="color:#2563eb">—</div>
      <div class="card-note">持股總值</div>
    </div>
    <div class="card" style="border-top-color:#1a1d23">
      <div class="card-label">總成本</div>
      <div class="card-value-sm" id="s-cost" style="color:#1a1d23">—</div>
      <div class="card-note">買入總額</div>
    </div>
    <div class="card" style="border-top-color:#ec0f0f">
      <div class="card-label">總收入</div>
      <div class="card-value-sm" id="s-revenue" style="color:#ec0f0f">—</div>
      <div class="card-note">賣出合計</div>
    </div>
  </div>
  <!-- 第二排 3張 -->
  <div class="summary-row summary-row-3">
    <div class="card" style="border-top-color:#fea52e">
      <div class="card-label">已賺</div>
      <div class="card-value-sm" id="s-profit">—</div>
      <div class="card-note">已實現</div>
    </div>
    <div class="card" style="border-top-color:#dc2626">
      <div class="card-label">日損益</div>
      <div class="card-value-sm" id="s-daily">—</div>
      <div class="card-note">當日實現</div>
    </div>
    <div class="card" style="border-top-color:#dc2626">
      <div class="card-label">總報酬</div>
      <div class="card-value-sm" id="s-return">—</div>
      <div class="card-note">市值／成本</div>
    </div>
  </div>

  <div class="tabs">
    <button class="tab active" id="tab-p" onclick="switchTab('p')">持股</button>
    <button class="tab" id="tab-c" onclick="switchTab('c')">分析</button>
    <button class="tab" id="tab-t" onclick="switchTab('t')">交易紀錄</button>
  </div>

  <!-- 持股 -->
  <div id="page-p">
    <div class="section-label">持有中</div>
    <div id="activeList"></div>
    <button class="add-btn" onclick="openAddModal()">＋ 新增股票</button>
    <div id="clearedLabel" class="section-label" style="display:none">已清倉</div>
    <div id="clearedList"></div>
  </div>

  <!-- 分析（只保留市值比例、成本比例、報酬率排行） -->
  <div id="page-c" style="display:none">
    <div class="section-label" style="padding-top:16px">投資組合分析</div>
    <div class="chart-wrap">
      <div class="chart-card">
        <div class="chart-title">持股市值比例</div>
        <div class="pie-container">
          <canvas id="pieCanvas" width="140" height="140" style="flex-shrink:0"></canvas>
          <div class="pie-legend" id="pieLegend"></div>
        </div>
      </div>
      <div class="chart-card">
        <div class="chart-title">持股成本比例</div>
        <div class="pie-container">
          <canvas id="costCanvas" width="140" height="140" style="flex-shrink:0"></canvas>
          <div class="pie-legend" id="costLegend"></div>
        </div>
      </div>
      <div class="chart-card">
        <div class="chart-title">報酬率比例</div>
        <div class="pie-container">
          <canvas id="returnCanvas" width="140" height="140" style="flex-shrink:0"></canvas>
          <div class="pie-legend" id="returnLegend"></div>
        </div>
      </div>
    </div>
  </div>

  <!-- 交易紀錄 -->
  <div id="page-t" style="display:none">
    <div class="section-label" style="padding-top:16px">交易紀錄</div>
    <div id="txList"></div>
  </div>
</div>

<!-- Modals -->
<div class="modal-overlay" id="priceModal">
  <div class="modal">
    <div style="font-size:16px;font-weight:600;margin-bottom:6px">手動更新市價</div>
    <div style="font-size:11px;color:#6b7280;margin-bottom:20px">填入今日收盤價或最新市價</div>
    <div id="priceList"></div>
    <div style="display:flex;gap:10px;margin-top:8px">
      <button class="btn-cancel" onclick="closePriceModal()">取消</button>
      <button class="btn-confirm-blue" onclick="confirmPrices()">確認更新</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="addModal">
  <div class="modal">
    <div style="font-size:16px;font-weight:600;margin-bottom:20px">新增股票</div>
    <div style="margin-bottom:12px">
      <label class="form-label">股票代碼 *</label>
      <input class="form-input" id="f-code" placeholder="例：2330">
    </div>
    <div style="margin-bottom:12px">
      <label class="form-label">股票名稱 *</label>
      <input class="form-input" id="f-name" placeholder="例：台積電">
    </div>
    <div class="form-grid">
      <div><label class="form-label">持有股數</label><input class="form-input" type="number" id="f-shares" placeholder="0"></div>
      <div><label class="form-label">買入均價</label><input class="form-input" type="number" id="f-avg" placeholder="0.00"></div>
      <div><label class="form-label">目前市價</label><input class="form-input" type="number" id="f-price" placeholder="0.00"></div>
      <div><label class="form-label">歷史已賺（選填）</label><input class="form-input" type="number" id="f-profit" placeholder="0"></div>
      <div><label class="form-label">歷史總收入（選填）</label><input class="form-input" type="number" id="f-revenue" placeholder="0"></div>
    </div>
    <div style="display:flex;gap:10px;margin-top:8px">
      <button class="btn-cancel" onclick="closeAddModal()">取消</button>
      <button class="btn-confirm-blue" onclick="confirmAdd()">確認新增</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="buyModal">
  <div class="modal">
    <div style="font-size:16px;font-weight:600;margin-bottom:6px">＋ 買入</div>
    <div style="font-size:13px;color:#6b7280;margin-bottom:20px" id="buy-name"></div>
    <div class="form-grid">
      <div><label class="form-label">買入股數</label><input class="form-input" type="number" id="buy-shares" placeholder="0"></div>
      <div><label class="form-label">買入價格</label><input class="form-input" type="number" id="buy-price" placeholder="0.00"></div>
    </div>
    <div style="font-size:11px;color:#aab0bb;margin-bottom:16px">手續費將自動計算（0.0855%）</div>
    <div style="display:flex;gap:10px">
      <button class="btn-cancel" onclick="closeBuyModal()">取消</button>
      <button class="btn-confirm-buy" onclick="confirmBuy()">確認買入</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="sellModal">
  <div class="modal">
    <div style="font-size:16px;font-weight:600;margin-bottom:6px">－ 賣出</div>
    <div style="font-size:13px;color:#6b7280;margin-bottom:20px" id="sell-name"></div>
    <div class="form-grid">
      <div><label class="form-label">賣出股數</label><input class="form-input" type="number" id="sell-shares" placeholder="0"></div>
      <div><label class="form-label">賣出價格</label><input class="form-input" type="number" id="sell-price" placeholder="0.00"></div>
    </div>
    <div style="font-size:11px;color:#aab0bb;margin-bottom:16px">手續費＋交易稅將自動計算（0.0855% + 0.3%）</div>
    <div style="display:flex;gap:10px">
      <button class="btn-cancel" onclick="closeSellModal()">取消</button>
      <button class="btn-confirm-sell" onclick="confirmSell()">確認賣出</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="deleteModal">
  <div class="modal">
    <div style="font-size:16px;font-weight:600;margin-bottom:8px">確認刪除</div>
    <div style="font-size:13px;color:#6b7280;margin-bottom:24px">確定要刪除 <span id="del-name" style="color:#1a1d23;font-weight:600"></span> 的所有紀錄嗎？此操作無法復原。</div>
    <input type="hidden" id="del-code">
    <div style="display:flex;gap:10px">
      <button class="btn-cancel" onclick="closeDeleteModal()">取消</button>
      <button class="btn-confirm-sell" onclick="confirmDelete()">確認刪除</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="delTradeModal">
  <div class="modal">
    <div style="font-size:16px;font-weight:600;margin-bottom:8px">刪除交易紀錄</div>
    <div style="font-size:13px;color:#6b7280;margin-bottom:24px">確定要刪除這筆 <span id="del-trade-desc" style="color:#1a1d23;font-weight:600"></span> 紀錄嗎？</div>
    <input type="hidden" id="del-trade-idx">
    <div style="display:flex;gap:10px">
      <button class="btn-cancel" onclick="closeDelTradeModal()">取消</button>
      <button class="btn-confirm-sell" onclick="confirmDeleteTrade()">確認刪除</button>
    </div>
  </div>
</div>

<script>
const BUY_FEE  = 0.0855/100, SELL_FEE = 0.0855/100, TAX = 0.3/100;
function calcBuyFee(amt)  { return Math.max(1,Math.floor(amt*BUY_FEE)); }
function calcSellFee(amt) { return Math.max(1,Math.floor(amt*SELL_FEE))+Math.floor(amt*TAX); }
function fmt(n,d=0){ if(n==null||isNaN(n)) return '—'; return n.toLocaleString('zh-TW',{minimumFractionDigits:d,maximumFractionDigits:d}); }

const DS = [
  {code:'0050', name:'元大台灣50',       shares:191,  avg:58.97, price:89.95, totalBuyCost:Math.round(58.97*191+calcBuyFee(58.97*191)),  revenue:0,  profit:0},
  {code:'00919',name:'群益台灣精選高息', shares:573,  avg:22.23, price:23.50, totalBuyCost:Math.round(22.23*573+calcBuyFee(22.23*573)),  revenue:0,  profit:0},
  {code:'00927',name:'群益半導體收益',   shares:250,  avg:27.74, price:29.19, totalBuyCost:Math.round(27.74*250+calcBuyFee(27.74*250)),  revenue:0,  profit:0},
  {code:'00918',name:'大華優利高填息30', shares:292,  avg:23.26, price:23.36, totalBuyCost:Math.round(23.26*292+calcBuyFee(23.26*292)),  revenue:0,  profit:0},
  {code:'5351', name:'鈺創',             shares:2000, avg:24,    price:61.10, totalBuyCost:Math.round(24*2000+calcBuyFee(24*2000)),       revenue:0,  profit:0},
  {code:'2330', name:'台積電',           shares:1,    avg:1851,  price:2185,  totalBuyCost:1851,                                          revenue:0,  profit:0},
  {code:'9105', name:'泰金寶',           shares:6000, avg:3.88,  price:5.62,  totalBuyCost:Math.round(3.88*6000+calcBuyFee(3.88*6000)),  revenue:25800,profit:7800},
  {code:'00878',name:'國泰永續高股息',   shares:0,    avg:21.42, price:23.46, totalBuyCost:Math.round(21.42*547+calcBuyFee(21.42*547)),  revenue:12816,profit:1100},
  {code:'2388', name:'威盛',             shares:0,    avg:77.9,  price:83.7,  totalBuyCost:5145,                                          revenue:5504, profit:359},
  {code:'2486', name:'一詮',             shares:0,    avg:189,   price:198,   totalBuyCost:1891,                                          revenue:1974, profit:83},
];

function load(){
  try{
    const s=localStorage.getItem('stocks4'), tx=localStorage.getItem('trades2');
    return {
      stocks: s?JSON.parse(s):JSON.parse(JSON.stringify(DS)),
      trades: tx?JSON.parse(tx):[],
    };
  }catch(e){return{stocks:JSON.parse(JSON.stringify(DS)),trades:[]};}
}
function save(){
  localStorage.setItem('stocks4',JSON.stringify(stocks));
  localStorage.setItem('trades2',JSON.stringify(trades));
}

let {stocks,trades}=load();
let open={},txOpen={},tab='p',currentCode=null;

const now=new Date();
document.getElementById('dateLabel').textContent=now.toLocaleDateString('zh-TW',{year:'numeric',month:'2-digit',day:'2-digit',weekday:'short'});

// ─── 日損益：當天交易的實現損益 ───────────────────────────────────
function calcDailyPL(){
  const today=new Date().toISOString().slice(0,10);
  // trades 沒有 date 欄位舊資料，只統計有 date 欄位且為今日的
  const todayTrades=trades.filter(t=>t.date===today&&t.type==='sell');
  return todayTrades.reduce((a,t)=>a+(t.realizedPL||0),0);
}

// ─── 個股日損益：該股今日賣出實現損益 ────────────────────────────
function calcStockDailyPL(code){
  const today=new Date().toISOString().slice(0,10);
  return trades.filter(t=>t.date===today&&t.type==='sell'&&t.code===code)
               .reduce((a,t)=>a+(t.realizedPL||0),0);
}

// ─── 自動抓台灣證交所收盤價（永遠回傳最新一筆可用收盤價）──────────
async function autoFetchPrices(){
  const btn=document.getElementById('autoBtn');
  btn.textContent='⟳ 更新中…'; btn.classList.add('loading');
  try{
    const res=await fetch('https://openapi.twse.com.tw/v1/exchangeReport/STOCK_DAY_ALL');
    if(!res.ok) throw new Error('API error');
    const data=await res.json();
    const map={};
    // API 回傳欄位包含 Date（YYYYMMDD）和 ClosingPrice
    let dataDate='';
    for(const item of data){
      const p=parseFloat(item.ClosingPrice);
      if(!isNaN(p)&&p>0){
        map[item.Code]=p;
        if(!dataDate&&item.Date) dataDate=item.Date;
      }
    }
    // 格式化日期：YYYYMMDD → YYYY/MM/DD
    let dateStr=dataDate;
    if(dataDate&&dataDate.length===8){
      dateStr=dataDate.slice(0,4)+'/'+dataDate.slice(4,6)+'/'+dataDate.slice(6,8);
    }
    let updated=0,notFound=[];
    for(const s of stocks){
      if(s.shares>0){
        const p=map[s.code];
        if(p){s.price=p;updated++;}
        else notFound.push(s.code);
      }
    }
    const srcEl=document.getElementById('priceSrc');
    const isToday=dateStr===now.toLocaleDateString('zh-TW',{year:'numeric',month:'2-digit',day:'2-digit'}).replace(/\//g,'/');
    srcEl.textContent=`資料來源：TWSE 收盤價（${dateStr}）${dateStr?'':''}${notFound.length?' ／ '+notFound.join('、')+' 查無收盤價':''}`;
    srcEl.style.color=notFound.length?'#d97706':'#aab0bb';

    btn.textContent=`✓ 已更新 ${updated} 支`;
    setTimeout(()=>{btn.textContent='⟳ 自動更新';btn.classList.remove('loading');},3500);
    render();
  }catch(e){
    document.getElementById('priceSrc').textContent='自動更新失敗，請改用手動報價';
    document.getElementById('priceSrc').style.color='#dc2626';
    btn.textContent='✗ 更新失敗';btn.classList.remove('loading');
    setTimeout(()=>{btn.textContent='⟳ 自動更新';},3000);
  }
}

function switchTab(t){
  tab=t;
  ['p','c','t'].forEach(x=>{
    document.getElementById('page-'+x).style.display=t===x?'block':'none';
    document.getElementById('tab-'+x).className='tab'+(t===x?' active':'');
  });
  if(t==='c') renderCharts();
}

function render(){
  save();
  let mkt=0,cost=0,profit=0,revenue=0,activeCost=0;
  for(const s of stocks){
    if(s.shares>0&&s.price) mkt+=s.shares*s.price;
    if(s.shares>0) activeCost+=s.totalBuyCost;
    cost+=s.totalBuyCost; profit+=s.profit; revenue+=s.revenue;
  }
  const totalReturn=activeCost>0?(mkt-activeCost)/activeCost*100:null;

  document.getElementById('s-market').textContent='$'+fmt(mkt);
  document.getElementById('s-cost').textContent='$'+fmt(cost);
  document.getElementById('s-revenue').textContent='$'+fmt(revenue);

  // 已賺：漲橘跌綠
  const pe=document.getElementById('s-profit');
  if(profit>0){pe.textContent='+$'+fmt(profit);pe.style.color='#fea52e';}
  else if(profit<0){pe.textContent='-$'+fmt(Math.abs(profit));pe.style.color='#4ade80';}
  else{pe.textContent='—';pe.style.color='#aab0bb';}

  // 日損益：漲紅跌綠
  const daily=calcDailyPL();
  const de=document.getElementById('s-daily');
  if(daily>0){de.textContent='+$'+fmt(daily);de.style.color='#dc2626';}
  else if(daily<0){de.textContent='-$'+fmt(Math.abs(daily));de.style.color='#16a34a';}
  else{de.textContent='—';de.style.color='#aab0bb';}

  // 總報酬：漲紅跌綠
  const re=document.getElementById('s-return');
  if(totalReturn!==null){
    re.textContent=(totalReturn>=0?'+':'')+totalReturn.toFixed(1)+'%';
    re.style.color=totalReturn>=0?'#dc2626':'#16a34a';
  }else{re.textContent='—';re.style.color='#aab0bb';}

  const active=stocks.filter(s=>s.shares>0), cleared=stocks.filter(s=>s.shares<=0);
  document.getElementById('activeList').innerHTML=active.map(cardHTML).join('');
  document.getElementById('clearedList').innerHTML=cleared.map(cardHTML).join('');
  document.getElementById('clearedLabel').style.display=cleared.length?'block':'none';
  for(const c in open){if(open[c]){const el=document.getElementById('d-'+c);if(el)el.classList.add('open');}}
  renderTrades();
  if(tab==='c') renderCharts();
}

function cardHTML(s){
  const cl=s.shares<=0;
  const mv=s.shares>0&&s.price?s.shares*s.price:null;
  const unr=mv!==null?mv-s.totalBuyCost:null;
  const pct=s.price&&s.avg?(s.price-s.avg)/s.avg*100:null;
  // 報酬率色：漲紅跌綠
  const retColor=pct===null?'#aab0bb':pct>=0?'#dc2626':'#16a34a';
  const retStr=pct!==null?(pct>=0?'+':'')+pct.toFixed(1)+'%':'—';

  let sig='<span style="color:#aab0bb;font-size:11px">—</span>';
  if(pct!==null){
    if(pct<-5) sig='<span class="pill" style="color:#16a34a;background:#dcfce7;border:1px solid #bbf7d0">▲ 建議買進</span>';
    else if(pct>15) sig='<span class="pill" style="color:#dc2626;background:#fee2e2;border:1px solid #fecaca">▼ 建議賣出</span>';
    else sig='<span class="pill" style="color:#d97706;background:#fef3c7;border:1px solid #fde68a">● 持續持有</span>';
  }
  const bt=s.price?'$'+(s.price*0.95).toFixed(2):'—';
  const st=s.price?'$'+(s.price*1.15).toFixed(2):'—';

  // 已賺：漲橘跌綠
  const profitStyle=s.profit>0?'color:#fea52e':s.profit<0?'color:#4ade80':'color:#aab0bb';
  const profitVal=s.profit>0?'+$'+fmt(s.profit):s.profit<0?'-$'+fmt(Math.abs(s.profit)):'—';

  // 個股日損益：漲紅跌綠
  const sdaily=calcStockDailyPL(s.code);
  let sdailyVal='—', sdailyColor='#aab0bb';
  if(sdaily>0){sdailyVal='+$'+fmt(sdaily);sdailyColor='#dc2626';}
  else if(sdaily<0){sdailyVal='-$'+fmt(Math.abs(sdaily));sdailyColor='#16a34a';}

  // 未實現：漲紅跌綠
  const unrColor=unr===null?'#aab0bb':unr>=0?'#dc2626':'#16a34a';
  const unrVal=unr!==null?(unr>=0?'+':'')+'$'+fmt(unr):'—';

  return `<div class="stock-card${cl?' cleared':''}" id="c-${s.code}">
  <div class="stock-header" onclick="toggleCard('${s.code}')">
    <div style="flex:1;min-width:0">
      <div style="display:flex;align-items:center;gap:6px;margin-bottom:4px;flex-wrap:wrap">
        <span class="code-badge">${s.code}</span>
        <span style="font-size:15px;font-weight:600">${s.name}</span>
        ${cl?'<span class="cleared-badge">已清倉</span>':''}
        <span style="font-family:monospace;font-size:13px;font-weight:700;color:${retColor}">${retStr}</span>
      </div>
      <div style="font-family:monospace;font-size:12px;color:#6b7280">持有 <strong style="color:#1a1d23">${s.shares.toLocaleString()}</strong> 股</div>
    </div>
    <div style="text-align:right;flex-shrink:0;margin-left:10px" onclick="event.stopPropagation();editPrice('${s.code}')">
      <div id="pd-${s.code}" style="font-family:monospace;font-size:18px;font-weight:700;color:${s.price?'#1a1d23':'#aab0bb'}">${s.price?'$'+s.price.toFixed(2):'—'}</div>
      <div style="font-size:10px;color:#aab0bb;margin-top:2px">點擊更新市價</div>
    </div>
  </div>
  <!-- 上排：市值／成本／收入 -->
  <div class="metrics-top">
    <div class="metric"><div class="metric-label">市值</div><div class="metric-value" style="color:#2563eb">${mv!==null?'$'+fmt(mv):'—'}</div></div>
    <div class="metric"><div class="metric-label">成本</div><div class="metric-value" style="color:#1a1d23">$${fmt(s.totalBuyCost)}</div></div>
    <div class="metric"><div class="metric-label">收入</div><div class="metric-value" style="color:#ec0f0f">${s.revenue?'$'+fmt(s.revenue):'—'}</div></div>
  </div>
  <!-- 下排：已賺／日損益／未實現 -->
  <div class="metrics-bot">
    <div class="metric"><div class="metric-label">已賺</div><div class="metric-value" style="${profitStyle}">${profitVal}</div></div>
    <div class="metric"><div class="metric-label">日損益</div><div class="metric-value" style="color:${sdailyColor}">${sdailyVal}</div></div>
    <div class="metric"><div class="metric-label">未實現</div><div class="metric-value" style="color:${unrColor}">${unrVal}</div></div>
  </div>
  <div class="detail" id="d-${s.code}">
    <div class="signal-box">
      <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:10px">
        ${sig}
        <span style="font-family:monospace;font-size:11px;color:#6b7280">均價距現價 ${pct!==null?(pct>=0?'+':'')+pct.toFixed(1)+'%':'—'}</span>
      </div>
      <div style="display:flex;gap:24px">
        <div><div style="font-size:9px;color:#aab0bb;margin-bottom:3px">建議進場</div><div style="font-family:monospace;font-size:14px;font-weight:700;color:#16a34a">${bt}</div><div style="font-size:9px;color:#aab0bb;margin-top:2px">現價 −5%</div></div>
        <div><div style="font-size:9px;color:#aab0bb;margin-bottom:3px">建議退場</div><div style="font-family:monospace;font-size:14px;font-weight:700;color:#dc2626">${st}</div><div style="font-size:9px;color:#aab0bb;margin-top:2px">現價 +15%</div></div>
        ${unr!==null?`<div style="margin-left:auto"><div style="font-size:9px;color:#aab0bb;margin-bottom:3px">未實現損益</div><div style="font-family:monospace;font-size:14px;font-weight:700;color:${unrColor}">${unrVal}</div></div>`:''}
      </div>
    </div>
    <div style="font-size:10px;color:#6b7280;margin-bottom:6px">均價：$${fmt(s.avg,2)}　買入成本：$${fmt(s.totalBuyCost)}</div>
    <div class="trade-btns">
      <button class="btn-buy" onclick="event.stopPropagation();openBuyModal('${s.code}')">＋ 買入</button>
      <button class="btn-sell" onclick="event.stopPropagation();openSellModal('${s.code}')">－ 賣出</button>
    </div>
    <button class="btn-delete" onclick="event.stopPropagation();removeStock('${s.code}')">✕ 刪除此股票</button>
  </div>
</div>`;
}

// ─── 圓餅圖 ──────────────────────────────────────────────────────
const PIE_COLORS=['#f9a8c9','#a8d8f9','#a8f9c9','#f9d8a8','#d8a8f9','#f9f0a8','#a8e8f9','#f9b8a8','#b8f9a8','#e8a8f9'];
// 每支股票固定顏色，依 stocks 陣列順序分配
function getStockColor(code){
  const idx=stocks.findIndex(s=>s.code===code);
  return PIE_COLORS[(idx>=0?idx:0)%PIE_COLORS.length];
}

function drawPie(canvasId,data){
  const canvas=document.getElementById(canvasId); if(!canvas) return;
  const ctx=canvas.getContext('2d'),W=canvas.width,H=canvas.height,cx=W/2,cy=H/2,r=Math.min(W,H)/2-6;
  ctx.clearRect(0,0,W,H);
  const total=data.reduce((a,d)=>a+d.value,0); if(!total) return;
  let angle=-Math.PI/2;
  for(const d of data){
    const slice=(d.value/total)*Math.PI*2;
    ctx.beginPath(); ctx.moveTo(cx,cy); ctx.arc(cx,cy,r,angle,angle+slice); ctx.closePath();
    ctx.fillStyle=d.color; ctx.fill(); angle+=slice;
  }
  ctx.beginPath(); ctx.arc(cx,cy,r*0.52,0,Math.PI*2); ctx.fillStyle='#fff'; ctx.fill();
}

function renderCharts(){
  const active=stocks.filter(s=>s.shares>0);
  const mvData=active.filter(s=>s.price).map(s=>({label:s.name,value:s.shares*s.price,color:getStockColor(s.code)}));
  drawPie('pieCanvas',mvData);
  const mvTotal=mvData.reduce((a,d)=>a+d.value,0);
  document.getElementById('pieLegend').innerHTML=mvData.map(d=>`<div class="legend-item"><div class="legend-dot" style="background:${d.color}"></div><div class="legend-name">${d.label}</div><div class="legend-pct">${mvTotal?((d.value/mvTotal)*100).toFixed(1)+'%':'—'}</div></div>`).join('');

  const costData=active.map(s=>({label:s.name,value:s.totalBuyCost,color:getStockColor(s.code)}));
  drawPie('costCanvas',costData);
  const costTotal=costData.reduce((a,d)=>a+d.value,0);
  document.getElementById('costLegend').innerHTML=costData.map(d=>`<div class="legend-item"><div class="legend-dot" style="background:${d.color}"></div><div class="legend-name">${d.label}</div><div class="legend-pct">${costTotal?((d.value/costTotal)*100).toFixed(1)+'%':'—'}</div></div>`).join('');

  const ranked=active.filter(s=>s.price&&s.avg).map(s=>({name:s.name,code:s.code,pct:(s.price-s.avg)/s.avg*100})).sort((a,b)=>b.pct-a.pct);
  // 報酬率圓餅：用正報酬率作為比例（負的顯示為極小值讓它出現）
  const returnData=ranked.map(r=>({label:r.name,code:r.code,pct:r.pct,value:Math.max(r.pct,0.01),color:getStockColor(r.code)}));
  drawPie('returnCanvas',returnData);
  document.getElementById('returnLegend').innerHTML=returnData.map(d=>{
    const pos=d.pct>=0;
    return `<div class="legend-item"><div class="legend-dot" style="background:${d.color}"></div><div class="legend-name">${d.label}</div><div class="legend-pct" style="color:${pos?'#dc2626':'#16a34a'}">${pos?'+':''}${d.pct.toFixed(1)}%</div></div>`;
  }).join('');
}

// ─── 交易紀錄 ────────────────────────────────────────────────────
function toggleTxGroup(code){
  txOpen[code]=!txOpen[code];
  const body=document.getElementById('txg-'+code),chev=document.getElementById('txc-'+code);
  if(body) body.classList.toggle('open',txOpen[code]);
  if(chev) chev.classList.toggle('open',txOpen[code]);
}

function renderTrades(){
  const el=document.getElementById('txList');
  if(!trades.length){el.innerHTML='<div class="tx-group"><div class="tx-empty">尚無交易紀錄<br><span style="font-size:10px;color:#c0c8d4">買賣後自動產生</span></div></div>';return;}
  const map={};
  trades.forEach((t,idx)=>{
    if(!map[t.code]) map[t.code]={code:t.code,name:t.name,buyShares:0,sellShares:0,rows:[]};
    if(t.type==='buy')  map[t.code].buyShares+=t.shares;
    if(t.type==='sell') map[t.code].sellShares+=t.shares;
    map[t.code].rows.push({...t,origIdx:idx});
  });
  el.innerHTML=Object.values(map).map(g=>{
    const se=stocks.find(s=>s.code===g.code);
    const remaining=se?se.shares:(g.buyShares-g.sellShares);
    const detailRows=[...g.rows].reverse().map(r=>{
      const isBuy=r.type==='buy';
      const unitPrice=r.unitPrice!=null?r.unitPrice:(r.shares>0?r.amt/r.shares:null);
      const plStr=(!isBuy&&r.realizedPL!=null)?(r.realizedPL>=0?` <span style="color:#dc2626">+$${fmt(r.realizedPL)}</span>`:`<span style="color:#16a34a">-$${fmt(Math.abs(r.realizedPL))}</span>`):'';
      return `<div class="tx-detail-row">
        <div>
          <span style="font-size:11px;font-weight:700;padding:2px 7px;border-radius:4px;background:${isBuy?'#dcfce7':'#fee2e2'};color:${isBuy?'#16a34a':'#dc2626'}">${isBuy?'買入':'賣出'}</span>
          ${r.date?`<span style="font-size:9px;color:#aab0bb;margin-left:4px">${r.date.slice(5)}</span>`:''}
        </div>
        <div style="font-family:monospace;font-size:12px;font-weight:700;color:${isBuy?'#16a34a':'#dc2626'};text-align:right">${r.shares.toLocaleString()} 股</div>
        <div style="font-family:monospace;font-size:11px;color:#1a1d23;text-align:right;font-weight:600">${unitPrice?'$'+unitPrice.toFixed(2):'—'}</div>
        <div style="font-family:monospace;font-size:11px;color:#6b7280;text-align:right">$${fmt(r.amt)}${plStr}</div>
        <div style="text-align:center"><button class="tx-del-btn" onclick="askDeleteTrade(${r.origIdx},'${r.name}','${r.type}',${r.shares})">✕</button></div>
      </div>`;
    }).join('');
    return `<div class="tx-group">
      <div class="tx-group-header" onclick="toggleTxGroup('${g.code}')">
        <div><div style="font-family:monospace;font-size:10px;color:#6b7280">${g.code}</div><div style="font-size:13px;font-weight:600">${g.name}</div></div>
        <div style="text-align:right"><div style="font-size:9px;color:#aab0bb">買入總股數</div><div style="font-family:monospace;font-weight:700;color:#16a34a">${g.buyShares>0?g.buyShares.toLocaleString()+' 股':'—'}</div></div>
        <div style="text-align:right"><div style="font-size:9px;color:#aab0bb">賣出總股數</div><div style="font-family:monospace;font-weight:700;color:#dc2626">${g.sellShares>0?g.sellShares.toLocaleString()+' 股':'—'}</div></div>
        <div style="text-align:right"><div style="font-size:9px;color:#aab0bb">剩餘股數</div><div style="font-family:monospace;font-weight:700;color:#2563eb">${remaining>0?remaining.toLocaleString()+' 股':'<span style="color:#aab0bb">已清倉</span>'}</div></div>
        <div style="text-align:center"><span class="tx-chevron${txOpen[g.code]?' open':''}" id="txc-${g.code}">▶</span></div>
      </div>
      <div class="tx-group-body${txOpen[g.code]?' open':''}" id="txg-${g.code}">
        <div style="display:grid;grid-template-columns:0.8fr 0.6fr 0.8fr 0.8fr 32px;padding:6px 14px;background:#f7f9fb;border-bottom:1px solid #edf0f4">
          <div style="font-size:9px;color:#aab0bb;text-transform:uppercase">類型</div>
          <div style="font-size:9px;color:#aab0bb;text-align:right;text-transform:uppercase">股數</div>
          <div style="font-size:9px;color:#aab0bb;text-align:right;text-transform:uppercase">單價</div>
          <div style="font-size:9px;color:#aab0bb;text-align:right;text-transform:uppercase">金額</div>
          <div></div>
        </div>
        ${detailRows}
      </div>
    </div>`;
  }).join('');
}

function askDeleteTrade(idx,name,type,shares){
  document.getElementById('del-trade-idx').value=idx;
  document.getElementById('del-trade-desc').textContent=(type==='buy'?'買入':'賣出')+' '+name+' '+shares.toLocaleString()+' 股';
  document.getElementById('delTradeModal').classList.add('open');
}
function closeDelTradeModal(){ document.getElementById('delTradeModal').classList.remove('open'); }
function confirmDeleteTrade(){
  const idx=parseInt(document.getElementById('del-trade-idx').value);
  if(isNaN(idx)||idx<0||idx>=trades.length) return;
  const t=trades[idx],s=stocks.find(s=>s.code===t.code);
  if(s){
    if(t.type==='buy'){s.shares=Math.max(0,s.shares-t.shares);s.totalBuyCost=Math.max(0,s.totalBuyCost-t.amt);if(s.shares>0)s.avg=s.totalBuyCost/s.shares;}
    else{s.shares+=t.shares;s.revenue=Math.max(0,s.revenue-t.amt);const cps=s.totalBuyCost/Math.max(1,s.shares-t.shares),ec=cps*t.shares;s.profit=s.profit-(t.realizedPL||0);s.totalBuyCost+=ec;}
  }
  trades.splice(idx,1);
  closeDelTradeModal(); render();
}

function toggleCard(code){
  open[code]=!open[code];
  const el=document.getElementById('d-'+code);
  if(el) el.classList.toggle('open',open[code]);
}
function editPrice(code){
  const s=stocks.find(s=>s.code===code);if(!s)return;
  const el=document.getElementById('pd-'+code);
  el.innerHTML=`<input class="inline-input" type="number" value="${s.price||''}" placeholder="0.00" onblur="commitPrice('${code}',this.value)" onkeydown="if(event.key==='Enter')this.blur();if(event.key==='Escape')render();" style="font-size:17px;width:110px">`;
  el.querySelector('input').focus();
}
function commitPrice(code,val){
  const n=parseFloat(val);if(!isNaN(n)&&n>=0){const s=stocks.find(s=>s.code===code);if(s)s.price=n;}render();
}
function openBuyModal(code){
  currentCode=code;const s=stocks.find(s=>s.code===code);
  document.getElementById('buy-name').textContent=s.name+'（'+code+'）';
  document.getElementById('buy-shares').value='';document.getElementById('buy-price').value='';
  document.getElementById('buyModal').classList.add('open');
}
function closeBuyModal(){ document.getElementById('buyModal').classList.remove('open'); }
function confirmBuy(){
  const s=stocks.find(s=>s.code===currentCode);if(!s)return;
  const shares=parseInt(document.getElementById('buy-shares').value),price=parseFloat(document.getElementById('buy-price').value);
  if(!shares||!price)return;
  const amt=price*shares,fee=calcBuyFee(amt),total=amt+fee;
  const oldAmt=s.avg*s.shares,newShares=s.shares+shares;
  s.avg=newShares>0?(oldAmt+amt)/newShares:price;
  s.shares=newShares;s.totalBuyCost+=total;s.price=price;
  const today=new Date().toISOString().slice(0,10);
  trades.push({code:s.code,name:s.name,type:'buy',shares,amt:total,unitPrice:price,date:today});
  closeBuyModal();render();
}
function openSellModal(code){
  currentCode=code;const s=stocks.find(s=>s.code===code);
  document.getElementById('sell-name').textContent=s.name+'（'+code+'）目前持有 '+s.shares.toLocaleString()+' 股';
  document.getElementById('sell-shares').value='';document.getElementById('sell-price').value='';
  document.getElementById('sellModal').classList.add('open');
}
function closeSellModal(){ document.getElementById('sellModal').classList.remove('open'); }
function confirmSell(){
  const s=stocks.find(s=>s.code===currentCode);if(!s)return;
  const shares=parseInt(document.getElementById('sell-shares').value),price=parseFloat(document.getElementById('sell-price').value);
  if(!shares||!price||shares>s.shares)return;
  const amt=price*shares,fee=calcSellFee(amt),netRecv=amt-fee;
  const costRatio=shares/s.shares,thisCost=Math.round(s.totalBuyCost*costRatio);
  const realizedPL=netRecv-thisCost;
  s.shares-=shares;s.totalBuyCost-=thisCost;s.revenue+=netRecv;s.profit+=realizedPL;s.price=price;
  const today=new Date().toISOString().slice(0,10);
  trades.push({code:s.code,name:s.name,type:'sell',shares,amt:netRecv,unitPrice:price,date:today,realizedPL});
  closeSellModal();render();
}
function openPriceModal(){
  const active=stocks.filter(s=>s.shares>0);
  document.getElementById('priceList').innerHTML=active.map(s=>`<div style="display:flex;align-items:center;margin-bottom:12px;gap:12px"><div style="flex:1"><div style="font-size:11px;color:#6b7280">${s.code}</div><div style="font-size:13px;font-weight:500">${s.name}</div></div><input type="number" value="${s.price||''}" data-code="${s.code}" style="width:100px;background:#f7f9fb;border:1px solid #dde1e8;color:#1a1d23;font-family:monospace;font-size:14px;padding:8px 10px;border-radius:8px;text-align:right"></div>`).join('');
  document.getElementById('priceModal').classList.add('open');
}
function closePriceModal(){ document.getElementById('priceModal').classList.remove('open'); }
function confirmPrices(){
  document.querySelectorAll('#priceList input').forEach(inp=>{const n=parseFloat(inp.value);if(!isNaN(n)&&n>=0){const s=stocks.find(s=>s.code===inp.dataset.code);if(s)s.price=n;}});
  closePriceModal();render();
}
function openAddModal(){
  ['f-code','f-name','f-shares','f-avg','f-price','f-profit','f-revenue'].forEach(id=>document.getElementById(id).value='');
  document.getElementById('addModal').classList.add('open');
}
function closeAddModal(){ document.getElementById('addModal').classList.remove('open'); }
function confirmAdd(){
  const code=document.getElementById('f-code').value.trim().toUpperCase(),name=document.getElementById('f-name').value.trim();
  if(!code||!name)return;
  const shares=+document.getElementById('f-shares').value||0,avg=+document.getElementById('f-avg').value||0,price=+document.getElementById('f-price').value||0,profit=+document.getElementById('f-profit').value||0,revenue=+document.getElementById('f-revenue').value||0;
  const buyCost=Math.round(avg*shares+calcBuyFee(avg*shares));
  const isNew=stocks.findIndex(s=>s.code===code)<0;
  const entry={code,name,shares,avg,price,totalBuyCost:buyCost,profit,revenue};
  const idx=stocks.findIndex(s=>s.code===code);
  if(idx>=0)stocks[idx]=entry;else stocks.push(entry);
  const today=new Date().toISOString().slice(0,10);
  if(isNew&&shares>0&&avg>0) trades.push({code,name,type:'buy',shares,amt:buyCost,unitPrice:avg,date:today});
  closeAddModal();render();
}
function removeStock(code){
  const s=stocks.find(s=>s.code===code);if(!s)return;
  document.getElementById('del-name').textContent=s.name+'（'+code+'）';
  document.getElementById('del-code').value=code;
  document.getElementById('deleteModal').classList.add('open');
}
function confirmDelete(){
  const code=document.getElementById('del-code').value;
  stocks=stocks.filter(s=>s.code!==code);
  document.getElementById('deleteModal').classList.remove('open');render();
}
function closeDeleteModal(){ document.getElementById('deleteModal').classList.remove('open'); }

render();
</script>
</body>
</html>
