<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no">
<meta name="theme-color" content="#0d9488">
<title>نظام الكاشير والمخازن</title>
<style>
:root{
  --bg:#f1f5f9;--card:#fff;--pri:#0d9488;--pri-d:#0f766e;--txt:#0f172a;
  --mut:#64748b;--bd:#e2e8f0;--dan:#dc2626;--ok:#16a34a;--warn:#d97706;
}
*{box-sizing:border-box;margin:0;padding:0;font-family:"Segoe UI",Tahoma,Arial,sans-serif;-webkit-tap-highlight-color:transparent}
body{background:var(--bg);color:var(--txt);padding-bottom:40px}
header{background:linear-gradient(135deg,#0d9488,#0f766e);color:#fff;padding:12px 16px;display:flex;justify-content:space-between;align-items:center;position:sticky;top:0;z-index:50;box-shadow:0 2px 10px rgba(0,0,0,.2)}
header h1{font-size:17px;font-weight:700}
header .hdr-btns{display:flex;gap:6px}
header .hdr-btns button{background:rgba(255,255,255,.2);border:none;color:#fff;padding:7px 10px;border-radius:8px;font-size:13px;cursor:pointer}
nav{display:flex;gap:6px;overflow-x:auto;background:#fff;padding:8px;position:sticky;top:53px;z-index:40;border-bottom:1px solid var(--bd);scrollbar-width:none}
nav::-webkit-scrollbar{display:none}
nav button{white-space:nowrap;border:none;background:#eef2f6;padding:9px 15px;border-radius:20px;font-size:14px;cursor:pointer;color:var(--mut);transition:.2s}
nav button.active{background:var(--pri);color:#fff;font-weight:600}
main{padding:12px;max-width:1100px;margin:0 auto}
.card{background:var(--card);border-radius:14px;padding:14px;margin-bottom:14px;box-shadow:0 1px 4px rgba(0,0,0,.07)}
.card h2{font-size:15px;margin-bottom:12px;color:var(--pri-d);display:flex;align-items:center;gap:6px}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:8px}
input,select{width:100%;padding:11px;border:1px solid var(--bd);border-radius:9px;font-size:15px;background:#fff;color:var(--txt);outline:none}
input:focus,select:focus{border-color:var(--pri);box-shadow:0 0 0 3px rgba(13,148,136,.12)}
.btn{padding:11px 15px;border:none;border-radius:9px;background:#e2e8f0;color:var(--txt);cursor:pointer;font-size:14px;font-weight:600;transition:.15s}
.btn:active{transform:scale(.97)}
.btn.primary{background:var(--pri);color:#fff}
.btn.danger{background:var(--dan);color:#fff}
.btn.ok{background:var(--ok);color:#fff}
.btn.sm{padding:6px 10px;font-size:12px;border-radius:7px}
table{width:100%;border-collapse:collapse;font-size:13px}
th,td{padding:9px 8px;border-bottom:1px solid var(--bd);text-align:right;white-space:nowrap}
th{background:#f8fafc;color:var(--mut);font-weight:600;font-size:12px}
tr:hover td{background:#fafcff}
.tw{overflow-x:auto;border-radius:10px}
.stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:10px;margin-bottom:14px}
.stat{background:#fff;border-radius:14px;padding:14px;border-right:5px solid var(--pri);box-shadow:0 1px 4px rgba(0,0,0,.07)}
.stat b{display:block;font-size:19px;margin-top:5px;color:var(--pri-d)}
.stat span{font-size:12px;color:var(--mut)}
.totals{background:#f0fdfa;border:1px solid #ccfbf1;padding:14px;border-radius:12px;display:grid;gap:8px;font-size:14px}
.totals div{display:flex;justify-content:space-between}
.totals b{color:var(--pri-d);font-size:16px}
.empty{text-align:center;color:var(--mut);padding:22px;font-size:13px}
.tag{padding:3px 9px;border-radius:20px;font-size:11px;font-weight:700}
.tag.ok{background:#dcfce7;color:#166534}
.tag.warn{background:#fef3c7;color:#92400e}
.tag.dan{background:#fee2e2;color:#991b1b}
/* Modal */
#overlay{position:fixed;inset:0;background:rgba(15,23,42,.55);display:none;align-items:center;justify-content:center;z-index:100;padding:14px}
#overlay.show{display:flex}
#modal{background:#fff;border-radius:16px;max-width:560px;width:100%;max-height:88vh;overflow:auto;padding:18px}
#modal h3{color:var(--pri-d);margin-bottom:12px;font-size:16px}
#modal .mfoot{display:flex;gap:8px;margin-top:14px;justify-content:flex-start}
/* Invoice */
.inv{font-size:13px;color:#0f172a}
.inv .ihead{text-align:center;border-bottom:2px dashed #cbd5e1;padding-bottom:10px;margin-bottom:10px}
.inv .ihead h2{color:var(--pri-d);font-size:19px;margin-bottom:4px}
.inv .irow{display:flex;justify-content:space-between;padding:4px 0;font-size:13px}
.inv table{font-size:12px;margin:10px 0}
.inv .itot{border-top:2px dashed #cbd5e1;padding-top:10px;margin-top:6px}
.inv .itot .irow b{font-size:15px;color:var(--pri-d)}
/* Print */
#printArea{display:none}
@media print{
  body *{visibility:hidden!important}
  #printArea,#printArea *{visibility:visible!important}
  #printArea{display:block!important;position:absolute;top:0;right:0;left:0;width:100%;padding:10px;background:#fff}
  @page{margin:8mm}
}
</style>
</head>
<body>

<header>
  <h1>🧾 نظام الكاشير</h1>
  <div class="hdr-btns">
    <button onclick="exportData()">⬇ تصدير</button>
  </div>
</header>

<nav id="nav">
  <button data-tab="dash" class="active">لوحة التحكم</button>
  <button data-tab="products">المخزون</button>
  <button data-tab="purchases">المشتريات</button>
  <button data-tab="sales">المبيعات</button>
  <button data-tab="invoices">الفواتير</button>
  <button data-tab="install">الأقساط</button>
  <button data-tab="settings">الإعدادات</button>
</nav>

<main>

<!-- ============ لوحة التحكم ============ -->
<section id="tab-dash" class="tab">
  <div class="stats" id="dashStats"></div>
  <div class="card">
    <h2>⚠️ تنبيهات المخزون (كمية ≤ 5)</h2>
    <div class="tw" id="lowStock"></div>
  </div>
  <div class="card">
    <h2>📊 آخر العمليات</h2>
    <div class="tw" id="lastOps"></div>
  </div>
</section>

<!-- ============ المخزون ============ -->
<section id="tab-products" class="tab" style="display:none">
  <div class="card">
    <h2>➕ إضافة منتج جديد</h2>
    <div class="grid">
      <input id="pName" placeholder="اسم المنتج">
      <input id="pBuy" type="number" step="0.01" placeholder="سعر الشراء">
      <input id="pSell" type="number" step="0.01" placeholder="سعر البيع">
      <input id="pQty" type="number" step="1" placeholder="الكمية" value="0">
      <button class="btn primary" onclick="addProduct()">إضافة</button>
    </div>
  </div>
  <div class="card">
    <h2>📦 قائمة المنتجات</h2>
    <div class="tw" id="prodTable"></div>
  </div>
</section>

<!-- ============ المشتريات ============ -->
<section id="tab-purchases" class="tab" style="display:none">
  <div class="card">
    <h2>🛒 فاتورة مشتريات جديدة</h2>
    <div class="grid">
      <input id="purSupplier" placeholder="اسم المورد">
      <select id="purProduct" onchange="fillPurPrice()"></select>
      <input id="purQty" type="number" value="1" placeholder="الكمية">
      <input id="purPrice" type="number" step="0.01" placeholder="سعر الشراء">
      <button class="btn" onclick="addPurchaseItem()">+ إضافة للفاتورة</button>
    </div>
    <div class="tw" id="purCart" style="margin-top:12px"></div>
    <div style="margin-top:12px">
      <button class="btn primary" onclick="savePurchase()">💾 حفظ فاتورة المشتريات</button>
    </div>
  </div>
  <div class="card">
    <h2>📋 سجل المشتريات</h2>
    <div class="tw" id="purList"></div>
  </div>
</section>

<!-- ============ المبيعات ============ -->
<section id="tab-sales" class="tab" style="display:none">
  <div class="card">
    <h2>🧾 فاتورة بيع جديدة</h2>
    <div class="grid">
      <input id="saleCustomer" placeholder="اسم العميل">
      <input id="salePhone" placeholder="رقم الهاتف">
      <select id="saleType" onchange="toggleInst()">
        <option value="cash">💰 بيع كاش</option>
        <option value="installment">📆 بيع بالتقسيط</option>
      </select>
      <select id="saleProduct" onchange="fillSalePrice()"></select>
      <input id="saleQty" type="number" value="1" placeholder="الكمية">
      <input id="salePrice" type="number" step="0.01" placeholder="سعر البيع">
      <button class="btn" onclick="addSaleItem()">+ إضافة للفاتورة</button>
    </div>

    <div class="tw" id="saleCart" style="margin-top:12px"></div>

    <div id="instBox" style="display:none;margin-top:12px" class="grid">
      <input id="saleDown" type="number" step="0.01" placeholder="المقدم (اختياري)" value="0" oninput="computeSale()">
      <input id="saleMonths" type="number" value="12" placeholder="عدد الأشهر" oninput="computeSale()">
    </div>

    <div class="totals" style="margin-top:12px">
      <div><span>ثمن الكاش (بدون زيادة)</span><b id="saleCash">0.00</b></div>
      <div><span>إجمالي البيع</span><b id="saleTotal">0.00</b></div>
      <div><span>القسط الشهري</span><b id="saleMonthly">0.00</b></div>
      <div><span>المتبقي بعد المقدم</span><b id="saleRemain">0.00</b></div>
    </div>

    <div style="margin-top:12px">
      <button class="btn primary" onclick="saveSale()">💾 حفظ الفاتورة</button>
    </div>
  </div>

  <div class="card">
    <h2>📋 سجل المبيعات</h2>
    <div class="tw" id="salesList"></div>
  </div>
</section>

<!-- ============ الفواتير ============ -->
<section id="tab-invoices" class="tab" style="display:none">
  <div class="card">
    <h2>🧾 كل الفواتير</h2>
    <div class="grid" style="margin-bottom:10px">
      <input id="invSearch" placeholder="بحث باسم العميل أو رقم الفاتورة" oninput="renderInvoices()">
    </div>
    <div class="tw" id="invList"></div>
  </div>
</section>

<!-- ============ الأقساط ============ -->
<section id="tab-install" class="tab" style="display:none">
  <div class="card">
    <h2>📆 متابعة الأقساط</h2>
    <div class="stats" id="instStats"></div>
    <div class="tw" id="instList"></div>
  </div>
</section>

<!-- ============ الإعدادات ============ -->
<section id="tab-settings" class="tab" style="display:none">
  <div class="card">
    <h2>⚙️ إعدادات النظام</h2>
    <div class="grid">
      <div>
        <label style="font-size:12px;color:var(--mut)">اسم المتجر</label>
        <input id="setShop" placeholder="اسم المتجر">
      </div>
      <div>
        <label style="font-size:12px;color:var(--mut)">العملة</label>
        <input id="setCurrency" placeholder="ج.م">
      </div>
      <div>
        <label style="font-size:12px;color:var(--mut)">نسبة الربح في التقسيط %</label>
        <input id="setProfit" type="number" step="0.1" value="30">
      </div>
      <div>
        <label style="font-size:12px;color:var(--mut)">عدد أشهر التقسيط</label>
        <input id="setMonths" type="number" value="12">
      </div>
    </div>
    <div style="margin-top:12px;display:flex;gap:8px;flex-wrap:wrap">
      <button class="btn primary" onclick="saveSettings()">💾 حفظ الإعدادات</button>
      <button class="btn" onclick="document.getElementById('importFile').click()">⬆ استيراد نسخة</button>
      <input type="file" id="importFile" accept=".json" style="display:none" onchange="importData(event)">
      <button class="btn danger" onclick="resetData()">🗑 حذف كل البيانات</button>
    </div>
  </div>
  <div class="card">
    <h2>ℹ️ طريقة الحساب</h2>
    <p style="font-size:14px;line-height:2;color:var(--mut)">
      • ثمن الكاش = مجموع (الكمية × سعر البيع).<br>
      • إجمالي التقسيط = ثمن الكاش + نسبة الربح (30%).<br>
      • القسط الشهري = (إجمالي التقسيط − المقدم) ÷ عدد الأشهر (12).<br>
      • المتبقي = إجمالي التقسيط − المقدم − مجموع الأقساط المدفوعة.
    </p>
  </div>
</section>

</main>

<!-- Modal -->
<div id="overlay" onclick="if(event.target===this)closeModal()">
  <div id="modal"></div>
</div>

<!-- Print area -->
<div id="printArea"></div>

<script>
/* ============================================================
   البيانات والتخزين
============================================================ */
const KEY = 'cashier_pos_v1';
const $ = id => document.getElementById(id);
const fmt = n => (Number(n)||0).toLocaleString('en-US',{minimumFractionDigits:2,maximumFractionDigits:2});
const today = () => new Date().toISOString().slice(0,10);
const uid = () => Date.now().toString(36)+Math.random().toString(36).slice(2,6);
const esc = s => String(s??'').replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));

let db;
function loadDB(){
  try{
    const raw = localStorage.getItem(KEY);
    if(raw) return JSON.parse(raw);
  }catch(e){}
  return {
    products:[], purchases:[], sales:[],
    settings:{ shop:'متجري', currency:'ج.م', profit:30, months:12 },
    counter:{ sale:1, pur:1 }
  };
}
function saveDB(){ localStorage.setItem(KEY, JSON.stringify(db)); }
db = loadDB();
if(!db.settings) db.settings = {shop:'متجري',currency:'ج.م',profit:30,months:12};
if(!db.counter) db.counter = {sale:1,pur:1};

const CUR = () => db.settings.currency || 'ج.م';

/* ============================================================
   التنقل بين التبويبات
============================================================ */
document.querySelectorAll('#nav button').forEach(b=>{
  b.onclick = () => {
    document.querySelectorAll('#nav button').forEach(x=>x.classList.remove('active'));
    b.classList.add('active');
    document.querySelectorAll('.tab').forEach(t=>t.style.display='none');
    $('tab-'+b.dataset.tab).style.display='block';
    window.scrollTo({top:0,behavior:'smooth'});
    renderAll();
  };
});

/* ============================================================
   Modal
============================================================ */
function openModal(title, bodyHTML){
  $('modal').innerHTML = '<h3>'+title+'</h3>'+bodyHTML+
    '<div class="mfoot"><button class="btn" onclick="closeModal()">إغلاق</button></div>';
  $('overlay').classList.add('show');
}
function closeModal(){ $('overlay').classList.remove('show'); }

/* ============================================================
   المخزون
============================================================ */
function addProduct(){
  const name = $('pName').value.trim();
  const buy  = +$('pBuy').value || 0;
  const sell = +$('pSell').value || 0;
  const qty  = +$('pQty').value || 0;
  if(!name) return alert('اكتب اسم المنتج');
  if(sell <= 0) return alert('سعر البيع يجب أن يكون أكبر من صفر');
  db.products.push({id:uid(), name, buy, sell, qty});
  saveDB();
  $('pName').value=''; $('pBuy').value=''; $('pSell').value=''; $('pQty').value='0';
  renderAll();
}

function deleteProduct(id){
  if(!confirm('حذف هذا المنتج؟')) return;
  db.products = db.products.filter(p=>p.id!==id);
  saveDB(); renderAll();
}

function editProduct(id){
  const p = db.products.find(x=>x.id===id); if(!p) return;
  openModal('تعديل المنتج',
    '<div class="grid">'+
      '<input id="eName" value="'+esc(p.name)+'" placeholder="الاسم">'+
      '<input id="eBuy" type="number" step="0.01" value="'+p.buy+'" placeholder="سعر الشراء">'+
      '<input id="eSell" type="number" step="0.01" value="'+p.sell+'" placeholder="سعر البيع">'+
      '<input id="eQty" type="number" value="'+p.qty+'" placeholder="الكمية">'+
    '</div>'+
    '<div class="mfoot"><button class="btn primary" onclick="saveEditProduct(\''+id+'\')">حفظ</button></div>'
  );
}
function saveEditProduct(id){
  const p = db.products.find(x=>x.id===id); if(!p) return;
  p.name = $('eName').value.trim() || p.name;
  p.buy  = +$('eBuy').value || 0;
  p.sell = +$('eSell').value || 0;
  p.qty  = +$('eQty').value || 0;
  saveDB(); closeModal(); renderAll();
}

function renderProducts(){
  const t = $('prodTable');
  if(!db.products.length){ t.innerHTML = '<div class="empty">لا توجد منتجات بعد</div>'; return; }
  let html = '<table><thead><tr><th>#</th><th>المنتج</th><th>سعر الشراء</th><th>سعر البيع</th><th>الكمية</th><th>الربح/وحدة</th><th>إجراءات</th></tr></thead><tbody>';
  db.products.forEach((p,i)=>{
    const low = p.qty <= 5;
    html += '<tr>'+
      '<td>'+(i+1)+'</td>'+
      '<td><b>'+esc(p.name)+'</b></td>'+
      '<td>'+fmt(p.buy)+'</td>'+
      '<td>'+fmt(p.sell)+'</td>'+
      '<td style="color:'+(low?'var(--dan)':'inherit')+';font-weight:'+(low?'700':'400')+'">'+p.qty+'</td>'+
      '<td>'+(p.sell-p.buy>=0?'+':'')+fmt(p.sell-p.buy)+'</td>'+
      '<td>'+
        '<button class="btn sm" onclick="editProduct(\''+p.id+'\')">تعديل</button> '+
        '<button class="btn sm danger" onclick="deleteProduct(\''+p.id+'\')">حذف</button>'+
      '</td></tr>';
  });
  t.innerHTML = html+'</tbody></table>';
}

function renderProductOptions(){
  const opts = db.products.map(p=>'<option value="'+p.id+'">'+esc(p.name)+' (متاح: '+p.qty+')</option>').join('');
  $('purProduct').innerHTML = '<option value="">— اختر منتج —</option>'+opts;
  $('saleProduct').innerHTML = '<option value="">— اختر منتج —</option>'+opts;
}

/* ============================================================
   المشتريات
============================================================ */
let purchaseCart = [];

function fillPurPrice(){
  const p = db.products.find(x=>x.id===$('purProduct').value);
  if(p) $('purPrice').value = p.buy || '';
}

function addPurchaseItem(){
  const pid = $('purProduct').value;
  if(!pid) return alert('اختر منتج أولاً');
  const p = db.products.find(x=>x.id===pid);
  const qty = +$('purQty').value || 0;
  const price = +$('purPrice').value || 0;
  if(qty <= 0) return alert('كمية غير صحيحة');
  if(price <= 0) return alert('سعر غير صحيح');
  purchaseCart.push({pid, name:p.name, qty, price});
  $('purQty').value = 1; $('purPrice').value = '';
  renderPurchaseCart();
}

function renderPurchaseCart(){
  const t = $('purCart');
  if(!purchaseCart.length){ t.innerHTML = '<div class="empty">لم تُضف أصناف بعد</div>'; return; }
  let html = '<table><thead><tr><th>الصنف</th><th>الكمية</th><th>السعر</th><th>الإجمالي</th><th></th></tr></thead><tbody>';
  let total = 0;
  purchaseCart.forEach((it,i)=>{
    const sub = it.qty*it.price; total += sub;
    html += '<tr><td>'+esc(it.name)+'</td><td>'+it.qty+'</td><td>'+fmt(it.price)+'</td><td>'+fmt(sub)+'</td>'+
      '<td><button class="btn sm danger" onclick="removePurchaseItem('+i+')">✕</button></td></tr>';
  });
  html += '</tbody></table><div style="margin-top:8px;font-weight:700;color:var(--pri-d)">الإجمالي: '+fmt(total)+' '+CUR()+'</div>';
  t.innerHTML = html;
}

function removePurchaseItem(i){ purchaseCart.splice(i,1); renderPurchaseCart(); }

function savePurchase(){
  if(!purchaseCart.length) return alert('أضف أصناف للفاتورة أولاً');
  const total = purchaseCart.reduce((s,i)=>s+i.qty*i.price,0);
  const rec = {
    id:uid(), no:db.counter.pur++, date:today(),
    supplier: $('purSupplier').value.trim() || 'مورد نقدي',
    items: purchaseCart.map(x=>({...x})), total
  };
  // زيادة المخزون وتحديث سعر الشراء
  rec.items.forEach(it=>{
    const p = db.products.find(x=>x.id===it.pid);
    if(p){ p.qty += it.qty; p.buy = it.price; }
  });
  db.purchases.unshift(rec);
  saveDB();
  purchaseCart = [];
  $('purSupplier').value = '';
  renderAll();
  alert('تم حفظ فاتورة المشتريات رقم '+rec.no);
}

function renderPurchases(){
  const t = $('purList');
  if(!db.purchases.length){ t.innerHTML = '<div class="empty">لا توجد مشتريات</div>'; return; }
  let html = '<table><thead><tr><th>رقم</th><th>التاريخ</th><th>المورد</th><th>الأصناف</th><th>الإجمالي</th><th></th></tr></thead><tbody>';
  db.purchases.forEach(p=>{
    html += '<tr><td>#'+p.no+'</td><td>'+p.date+'</td><td>'+esc(p.supplier)+'</td>'+
      '<td>'+p.items.length+'</td><td>'+fmt(p.total)+' '+CUR()+'</td>'+
      '<td><button class="btn sm" onclick="viewPurchase(\''+p.id+'\')">عرض</button></td></tr>';
  });
  t.innerHTML = html+'</tbody></table>';
}

function viewPurchase(id){
  const p = db.purchases.find(x=>x.id===id); if(!p) return;
  let rows = p.items.map(it=>'<tr><td>'+esc(it.name)+'</td><td>'+it.qty+'</td><td>'+fmt(it.price)+'</td><td>'+fmt(it.qty*it.price)+'</td></tr>').join('');
  openModal('فاتورة مشتريات #'+p.no,
    '<div class="inv"><div class="irow"><span>التاريخ: '+p.date+'</span><span>المورد: '+esc(p.supplier)+'</span></div>'+
    '<table><thead><tr><th>الصنف</th><th>الكمية</th><th>السعر</th><th>الإجمالي</th></tr></thead><tbody>'+rows+'</tbody></table>'+
    '<div class="irow"><b>الإجمالي: '+fmt(p.total)+' '+CUR()+'</b></div></div>'
  );
}

/* ============================================================
   المبيعات (كاش + تقسيط)
============================================================ */
let saleCart = [];

function fillSalePrice(){
  const p = db.products.find(x=>x.id===$('saleProduct').value);
  if(p) $('salePrice').value = p.sell || '';
}

function toggleInst(){
  const isInst = $('saleType').value === 'installment';
  $('instBox').style.display = isInst ? 'grid' : 'none';
  if(isInst && !+$('saleMonths').value) $('saleMonths').value = db.settings.months || 12;
  computeSale();
}

function addSaleItem(){
  const pid = $('saleProduct').value;
  if(!pid) return alert('اختر منتج أولاً');
  const p = db.products.find(x=>x.id===pid);
  const qty = +$('saleQty').value || 0;
  const price = +$('salePrice').value || p.sell;
  if(qty <= 0) return alert('كمية غير صحيحة');
  // التحقق من المخزون (مع مراعاة ما في السلة)
  const inCart = saleCart.filter(x=>x.pid===pid).reduce((s,x)=>s+x.qty,0);
  if(qty + inCart > p.qty) return alert('الكمية المتاحة في المخزون: '+p.qty);
  saleCart.push({pid, name:p.name, qty, price, cost:p.buy});
  $('saleQty').value = 1; $('salePrice').value = '';
  renderSaleCart();
  computeSale();
}

function removeSaleItem(i){ saleCart.splice(i,1); renderSaleCart(); computeSale(); }

function renderSaleCart(){
  const t = $('saleCart');
  if(!saleCart.length){ t.innerHTML = '<div class="empty">لم تُضف أصناف بعد</div>'; return; }
  let html = '<table><thead><tr><th>الصنف</th><th>الكمية</th><th>السعر</th><th>الإجمالي</th><th></th></tr></thead><tbody>';
  saleCart.forEach((it,i)=>{
    html += '<tr><td>'+esc(it.name)+'</td><td>'+it.qty+'</td><td>'+fmt(it.price)+'</td><td>'+fmt(it.qty*it.price)+'</td>'+
      '<td><button class="btn sm danger" onclick="removeSaleItem('+i+')">✕</button></td></tr>';
  });
  t.innerHTML = html+'</tbody></table>';
}

function saleVals(){
  const cashTotal = saleCart.reduce((s,i)=>s+i.qty*i.price,0);
  const type = $('saleType').value;
  const rate = 1 + (db.settings.profit||0)/100;
  const total = type === 'installment' ? cashTotal*rate : cashTotal;

  let months = parseInt($('saleMonths').value) || db.settings.months || 12;
  if(months < 1) months = 1;

  let down = type === 'installment' ? (+$('saleDown').value || 0) : total;
  if(down > total) down = total;
  if(down < 0) down = 0;

  const monthly = type === 'installment' ? (total - down) / months : 0;
  return { cashTotal, type, total, months, down, monthly, remain: total - down };
}

function computeSale(){
  const v = saleVals();
  $('saleCash').textContent    = fmt(v.cashTotal)+' '+CUR();
  $('saleTotal').textContent   = fmt(v.total)+' '+CUR();
  $('saleMonthly').textContent = v.type==='installment' ? fmt(v.monthly)+' '+CUR() : '—';
  $('saleRemain').textContent  = fmt(v.remain)+' '+CUR();
}

function saveSale(){
  if(!saleCart.length) return alert('أضف أصناف للفاتورة أولاً');
  const v = saleVals();
  const customer = $('saleCustomer').value.trim() || (v.type==='cash' ? 'عميل نقدي' : 'عميل تقسيط');

  const rec = {
    id: uid(),
    no: db.counter.sale++,
    date: today(),
    customer,
    phone: $('salePhone').value.trim(),
    type: v.type,
    items: saleCart.map(x=>({...x})),
    cashTotal: v.cashTotal,
    total: v.total,
    down: v.down,
    months: v.type==='installment' ? v.months : 0,
    monthly: v.monthly,
    payments: []
  };

  // خصم المخزون
  rec.items.forEach(it=>{
    const p = db.products.find(x=>x.id===it.pid);
    if(p) p.qty -= it.qty;
  });

  db.sales.unshift(rec);
  saveDB();

  saleCart = [];
  $('saleCustomer').value=''; $('salePhone').value='';
  $('saleDown').value='0'; $('saleType').value='cash'; $('instBox').style.display='none';
  renderAll();
  showInvoice(rec.id);
}

function renderSales(){
  const t = $('salesList');
  if(!db.sales.length){ t.innerHTML = '<div class="empty">لا توجد مبيعات</div>'; return; }
  let html = '<table><thead><tr><th>رقم</th><th>التاريخ</th><th>العميل</th><th>النوع</th><th>الإجمالي</th><th>المدفوع</th><th>المتبقي</th><th></th></tr></thead><tbody>';
  db.sales.slice(0,60).forEach(s=>{
    const paid = getPaid(s), rem = s.total - paid;
    html += '<tr>'+
      '<td>#'+s.no+'</td><td>'+s.date+'</td><td>'+esc(s.customer)+'</td>'+
      '<td>'+(s.type==='cash'?'<span class="tag ok">كاش</span>':'<span class="tag warn">تقسيط</span>')+'</td>'+
      '<td>'+fmt(s.total)+'</td><td>'+fmt(paid)+'</td>'+
      '<td style="color:'+(rem>0.01?'var(--dan)':'var(--ok)')+';font-weight:700">'+fmt(rem)+'</td>'+
      '<td><button class="btn sm" onclick="showInvoice(\''+s.id+'\')">فاتورة</button></td>'+
    '</tr>';
  });
  t.innerHTML = html+'</tbody></table>';
}

/* ============================================================
   حسابات الفواتير
============================================================ */
function getPaid(s){
  const paySum = (s.payments||[]).reduce((a,p)=>a+(+p.amount||0),0);
  return (+s.down||0) + paySum;
}
function getRemaining(s){ return Math.max(0, (+s.total||0) - getPaid(s)); }

function invoiceHTML(s, forPrint){
  const paid = getPaid(s), rem = s.total - paid;
  let rows = s.items.map(it=>
    '<tr><td>'+esc(it.name)+'</td><td>'+it.qty+'</td><td>'+fmt(it.price)+'</td><td>'+fmt(it.qty*it.price)+'</td></tr>'
  ).join('');

  let instBlock = '';
  if(s.type === 'installment'){
    instBlock =
      '<div class="irow"><span>ثمن الكاش</span><b>'+fmt(s.cashTotal)+' '+CUR()+'</b></div>'+
      '<div class="irow"><span>إجمالي التقسيط (+'+db.settings.profit+'%)</span><b>'+fmt(s.total)+' '+CUR()+'</b></div>'+
      '<div class="irow"><span>المقدم المدفوع</span><b>'+fmt(s.down)+' '+CUR()+'</b></div>'+
      '<div class="irow"><span>القسط الشهري ('+s.months+' شهر)</span><b>'+fmt(s.monthly)+' '+CUR()+'</b></div>'+
      '<div class="irow"><span>عدد الأقساط المدفوعة</span><b>'+(s.payments||[]).length+' / '+s.months+'</b></div>'+
      '<div class="irow"><span>إجمالي المدفوع</span><b>'+fmt(paid)+' '+CUR()+'</b></div>'+
      '<div class="irow" style="color:#b91c1c"><span>المتبقي</span><b>'+fmt(rem)+' '+CUR()+'</b></div>';
  } else {
    instBlock = '<div class="irow"><span>نوع البيع</span><b>كاش</b></div>'+
      '<div class="irow"><span>الإجمالي المدفوع</span><b>'+fmt(s.total)+' '+CUR()+'</b></div>';
  }

  return '<div class="inv">'+
    '<div class="ihead"><h2>'+esc(db.settings.shop||'متجري')+'</h2>'+
      '<div style="font-size:12px;color:#64748b">فاتورة رقم #'+s.no+' — '+s.date+'</div></div>'+
    '<div class="irow"><span>العميل: <b>'+esc(s.customer)+'</b></span><span>'+(s.phone?'هاتف: '+esc(s.phone):'')+'</span></div>'+
    '<table><thead><tr><th>الصنف</th><th>كمية</th><th>سعر</th><th>إجمالي</th></tr></thead><tbody>'+rows+'</tbody></table>'+
    '<div class="itot">'+instBlock+'</div>'+
    (forPrint ? '<div style="text-align:center;margin-top:18px;font-size:11px;color:#64748b">شكراً لتعاملكم معنا</div>' : '')+
  '</div>';
}

function showInvoice(id){
  const s = db.sales.find(x=>x.id===id); if(!s) return;
  $('printArea').innerHTML = invoiceHTML(s, true);
  openModal('فاتورة رقم #'+s.no,
    invoiceHTML(s,false)+
    '<div style="margin-top:14px;display:flex;gap:8px">'+
      '<button class="btn primary" onclick="window.print()">🖨 طباعة</button>'+
      (s.type==='installment' && getRemaining(s)>0.01
        ? '<button class="btn ok" onclick="closeModal();payInstallment(\''+s.id+'\')">💵 تسجيل قسط</button>' : '')+
    '</div>'
  );
}

/* ============================================================
   الفواتير
============================================================ */
function renderInvoices(){
  const q = ($('invSearch')?.value || '').trim().toLowerCase();
  let list = db.sales;
  if(q) list = list.filter(s => String(s.no).includes(q) || (s.customer||'').toLowerCase().includes(q));
  const t = $('invList');
  if(!list.length){ t.innerHTML = '<div class="empty">لا توجد فواتير</div>'; return; }
  let html = '<table><thead><tr><th>رقم</th><th>التاريخ</th><th>العميل</th><th>النوع</th><th>الإجمالي</th><th>المتبقي</th><th>الحالة</th><th></th></tr></thead><tbody>';
  list.forEach(s=>{
    const rem = getRemaining(s);
    const status = rem <= 0.01 ? '<span class="tag ok">مسدد</span>'
                  : s.type==='installment' ? '<span class="tag warn">قيد التحصيل</span>'
                  : '<span class="tag dan">غير مسدد</span>';
    html += '<tr><td>#'+s.no+'</td><td>'+s.date+'</td><td>'+esc(s.customer)+'</td>'+
      '<td>'+(s.type==='cash'?'كاش':'تقسيط')+'</td>'+
      '<td>'+fmt(s.total)+'</td><td>'+fmt(rem)+'</td><td>'+status+'</td>'+
      '<td><button class="btn sm" onclick="showInvoice(\''+s.id+'\')">عرض</button> '+
      '<button class="btn sm danger" onclick="deleteSale(\''+s.id+'\')">حذف</button></td></tr>';
  });
  t.innerHTML = html+'</tbody></table>';
}

function deleteSale(id){
  if(!confirm('حذف الفاتورة وإرجاع الكميات للمخزون؟')) return;
  const s = db.sales.find(x=>x.id===id); if(!s) return;
  s.items.forEach(it=>{
    const p = db.products.find(x=>x.id===it.pid);
    if(p) p.qty += it.qty;
  });
  db.sales = db.sales.filter(x=>x.id!==id);
  saveDB(); renderAll();
}

/* ============================================================
   الأقساط
============================================================ */
function renderInstallments(){
  const list = db.sales.filter(s=>s.type==='installment');
  const t = $('instList');

  // إحصائيات
  const totalInst = list.reduce((a,s)=>a+s.total,0);
  const totalPaid = list.reduce((a,s)=>a+getPaid(s),0);
  const totalRem  = list.reduce((a,s)=>a+getRemaining(s),0);
  const active    = list.filter(s=>getRemaining(s)>0.01).length;

  $('instStats').innerHTML =
    '<div class="stat"><span>عدد عقود التقسيط</span><b>'+list.length+'</b></div>'+
    '<div class="stat"><span>إجمالي التقسيط</span><b>'+fmt(totalInst)+'</b></div>'+
    '<div class="stat"><span>المحصّل</span><b style="color:var(--ok)">'+fmt(totalPaid)+'</b></div>'+
    '<div class="stat"><span>المتبقي</span><b style="color:var(--dan)">'+fmt(totalRem)+'</b></div>'+
    '<div class="stat"><span>عقود جارية</span><b>'+active+'</b></div>';

  if(!list.length){ t.innerHTML = '<div class="empty">لا توجد مبيعات بالتقسيط</div>'; return; }

  let html = '<table><thead><tr>'+
    '<th>رقم</th><th>العميل</th><th>الكاش</th><th>إجمالي التقسيط</th><th>المقدم</th>'+
    '<th>القسط الشهري</th><th>الأقساط</th><th>المدفوع</th><th>المتبقي</th><th>إجراءات</th>'+
    '</tr></thead><tbody>';

  list.forEach(s=>{
    const paid = getPaid(s), rem = getRemaining(s);
    const paidCount = (s.payments||[]).length;
    html += '<tr>'+
      '<td>#'+s.no+'</td>'+
      '<td><b>'+esc(s.customer)+'</b>'+(s.phone?'<br><span style="font-size:11px;color:#64748b">'+esc(s.phone)+'</span>':'')+'</td>'+
      '<td>'+fmt(s.cashTotal)+'</td>'+
      '<td>'+fmt(s.total)+'</td>'+
      '<td>'+fmt(s.down)+'</td>'+
      '<td>'+fmt(s.monthly)+'</td>'+
      '<td>'+paidCount+' / '+s.months+'</td>'+
      '<td style="color:var(--ok);font-weight:700">'+fmt(paid)+'</td>'+
      '<td style="color:'+(rem>0.01?'var(--dan)':'var(--ok)')+';font-weight:700">'+fmt(rem)+'</td>'+
      '<td>'+
        (rem>0.01 ? '<button class="btn sm ok" onclick="payInstallment(\''+s.id+'\')">تسديد</button> ' : '')+
        '<button class="btn sm" onclick="showInvoice(\''+s.id+'\')">فاتورة</button>'+
      '</td></tr>';
  });
  t.innerHTML = html+'</tbody></table>';
}

function payInstallment(id){
  const s = db.sales.find(x=>x.id===id); if(!s) return;
  const rem = getRemaining(s);
  openModal('تسديد قسط — '+esc(s.customer),
    '<div class="totals" style="margin-bottom:12px">'+
      '<div><span>القسط الشهري</span><b>'+fmt(s.monthly)+' '+CUR()+'</b></div>'+
      '<div><span>المتبقي الحالي</span><b>'+fmt(rem)+' '+CUR()+'</b></div>'+
    '</div>'+
    '<div class="grid">'+
      '<input id="payAmount" type="number" step="0.01" value="'+Math.min(s.monthly,rem).toFixed(2)+'" placeholder="المبلغ">'+
      '<input id="payDate" type="date" value="'+today()+'">'+
      '<input id="payNote" placeholder="ملاحظة (اختياري)">'+
    '</div>'+
    '<div class="mfoot"><button class="btn primary" onclick="savePayment(\''+id+'\')">تسجيل الدفع</button></div>'
  );
}

function savePayment(id){
  const s = db.sales.find(x=>x.id===id); if(!s) return;
  const amount = +$('payAmount').value || 0;
  if(amount <= 0) return alert('أدخل مبلغ صحيح');
  const rem = getRemaining(s);
  if(amount > rem + 0.01) return alert('المبلغ أكبر من المتبقي ('+fmt(rem)+')');
  s.payments = s.payments || [];
  s.payments.push({ date: $('payDate').value || today(), amount, note: $('payNote').value.trim() });
  saveDB(); closeModal(); renderAll();
  alert('تم تسجيل الدفع بنجاح');
}

/* ============================================================
   لوحة التحكم
============================================================ */
function renderDashboard(){
  const stockValue = db.products.reduce((a,p)=>a + p.qty*(p.buy||0), 0);
  const salesTotal = db.sales.reduce((a,s)=>a + s.total, 0);
  const purTotal   = db.purchases.reduce((a,p)=>a + p.total, 0);
  const costSold   = db.sales.reduce((a,s)=> a + s.items.reduce((x,i)=>x + i.qty*(i.cost||0), 0), 0);
  const profit     = salesTotal - costSold;
  const dueInst    = db.sales.filter(s=>s.type==='installment').reduce((a,s)=>a+getRemaining(s), 0);
  const cashIn     = db.sales.reduce((a,s)=>a+getPaid(s), 0);

  $('dashStats').innerHTML =
    '<div class="stat"><span>عدد المنتجات</span><b>'+db.products.length+'</b></div>'+
    '<div class="stat"><span>قيمة المخزون (بسعر الشراء)</span><b>'+fmt(stockValue)+'</b></div>'+
    '<div class="stat"><span>إجمالي المبيعات</span><b>'+fmt(salesTotal)+'</b></div>'+
    '<div class="stat"><span>إجمالي المشتريات</span><b>'+fmt(purTotal)+'</b></div>'+
    '<div class="stat"><span>الربح التقديري</span><b style="color:var(--ok)">'+fmt(profit)+'</b></div>'+
    '<div class="stat"><span>المحصّل فعلياً</span><b>'+fmt(cashIn)+'</b></div>'+
    '<div class="stat"><span>مستحقات الأقساط</span><b style="color:var(--dan)">'+fmt(dueInst)+'</b></div>'+
    '<div class="stat"><span>عدد الفواتير</span><b>'+db.sales.length+'</b></div>';

  // تنبيهات المخزون
  const low = db.products.filter(p=>p.qty<=5);
  $('lowStock').innerHTML = low.length
    ? '<table><thead><tr><th>المنتج</th><th>الكمية المتبقية</th></tr></thead><tbody>'+
      low.map(p=>'<tr><td>'+esc(p.name)+'</td><td style="color:var(--dan);font-weight:700">'+p.qty+'</td></tr>').join('')+
      '</tbody></table>'
    : '<div class="empty">✅ كل الكميات جيدة</div>';

  // آخر العمليات
  const ops = db.sales.slice(0,8).map(s=>
    '<tr><td>فاتورة بيع #'+s.no+'</td><td>'+esc(s.customer)+'</td><td>'+s.date+'</td>'+
    '<td style="color:'+(s.type==='cash'?'var(--ok)':'var(--warn)')+'">'+(s.type==='cash'?'كاش':'تقسيط')+'</td>'+
    '<td>'+fmt(s.total)+'</td></tr>'
  ).join('');
  $('lastOps').innerHTML = ops
    ? '<table><thead><tr><th>العملية</th><th>العميل</th><th>التاريخ</th><th>النوع</th><th>المبلغ</th></tr></thead><tbody>'+ops+'</tbody></table>'
    : '<div class="empty">لا توجد عمليات بعد</div>';
}

/* ============================================================
   الإعدادات
============================================================ */
function loadSettingsUI(){
  $('setShop').value = db.settings.shop || '';
  $('setCurrency').value = db.settings.currency || 'ج.م';
  $('setProfit').value = db.settings.profit ?? 30;
  $('setMonths').value = db.settings.months ?? 12;
}
function saveSettings(){
  db.settings.shop = $('setShop').value.trim() || 'متجري';
  db.settings.currency = $('setCurrency').value.trim() || 'ج.م';
  db.settings.profit = +$('setProfit').value || 0;
  db.settings.months = Math.max(1, parseInt($('setMonths').value) || 12);
  saveDB(); renderAll();
  alert('تم حفظ الإعدادات ✅');
}
function resetData(){
  if(!confirm('سيتم حذف كل المنتجات والفواتير نهائياً. متأكد؟')) return;
  if(!confirm('تأكيد أخير: حذف كل البيانات؟')) return;
  localStorage.removeItem(KEY);
  db = loadDB();
  saveDB(); loadSettingsUI(); renderAll();
}
function exportData(){
  const blob = new Blob([JSON.stringify(db,null,2)],{type:'application/json'});
  const a = document.createElement('a');
  a.href = URL.createObjectURL(blob);
  a.download = 'cashier-backup-'+today()+'.json';
  a.click();
}
function importData(e){
  const f = e.target.files[0]; if(!f) return;
  const r = new FileReader();
  r.onload = ev => {
    try{
      const d = JSON.parse(ev.target.result);
      if(!d.products || !d.sales) throw 0;
      db = d; saveDB(); loadSettingsUI(); renderAll();
      alert('تم استيراد البيانات ✅');
    }catch(err){ alert('ملف غير صالح'); }
  };
  r.readAsText(f);
  e.target.value = '';
}

/* ============================================================
   التشغيل
============================================================ */
function renderAll(){
  renderDashboard();
  renderProducts();
  renderProductOptions();
  renderPurchaseCart();
  renderPurchases();
  renderSaleCart();
  computeSale();
  renderSales();
  renderInvoices();
  renderInstallments();
}

loadSettingsUI();
renderAll();
</script>
</body>
</html># -
برنامج كاشير 
