<!doctype html>
<html lang="bn">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Dollar Exchange - Admin Panel</title>
<style>
:root{--primary:#0b75ff;--secondary:#0037dd;--bg:#f2f5f8;--text:#111827}
*{box-sizing:border-box}body{font-family:Arial,sans-serif;background:var(--bg);margin:0;color:var(--text)}
button,input,select,textarea{font:inherit}button{cursor:pointer}
.topbar{background:#fff;padding:10px 14px;display:flex;align-items:center;justify-content:space-between;gap:12px;box-shadow:0 2px 6px #0001;flex-wrap:wrap}
.logo{display:flex;align-items:center;gap:10px}.logo img{width:44px;height:44px;border-radius:8px;object-fit:cover}
.small{font-size:13px;color:#6b7280}.status-badge{padding:6px 12px;border-radius:8px;color:#fff;font-size:14px;font-weight:700;display:inline-block}
.online{background:#16a34a!important}.offline{background:#6b7280!important}
.top-buttons{display:flex;gap:8px}.top-buttons button{padding:8px 12px;border-radius:20px;border:1px solid var(--primary);background:#fff;color:var(--primary);font-weight:700}
.blue-head{background:linear-gradient(180deg,var(--secondary),#006bff);padding:28px 18px;text-align:center;border-radius:0 0 26px 26px;color:#fff}.blue-head h1{margin:0;font-size:26px}
.card{width:calc(100% - 24px);max-width:900px;margin:14px auto;background:#fff;padding:14px;border-radius:10px;box-shadow:0 2px 10px #0000000f}
input,select,textarea{width:100%;padding:10px;margin:6px 0 10px;border-radius:8px;border:1px solid #d1d5db;background:#fff}
button.primary,button.danger,button.success,button.warning{border:0;color:#fff;padding:10px 12px;border-radius:8px;font-weight:700}
button.primary{background:var(--primary)}button.danger{background:#ef4444}button.success{background:#16a34a}button.warning{background:#f59e0b;color:#111827}
button:disabled{opacity:.55;cursor:not-allowed}.nav{display:flex;gap:8px;justify-content:center;flex-wrap:wrap}.nav button{width:auto}
.control-row{display:flex;gap:10px}.control-row>*{flex:1;min-width:0}
.panel-section{display:none}.panel-section.active{display:block}
.order-box,.currency-item,.quote-item{background:#f9fafb;margin:10px 0;padding:12px;border-radius:10px;border:1px solid #eef0f3}
.order-box{display:flex;justify-content:space-between;align-items:flex-start;gap:12px}.order-info{flex:1;min-width:0}.order-actions{min-width:145px;text-align:right}.order-actions button{margin:3px 0;width:100%}
.status{padding:5px 9px;border-radius:6px;color:white;font-weight:700;display:inline-block;font-size:12px}.pending{background:#f59e0b;color:#111}.completed{background:#16a34a}.rejected{background:#ef4444}
.order-empty{text-align:center;color:#6b7280;padding:24px}.currency-item{display:flex;justify-content:space-between;align-items:center;gap:10px}.currency-info{flex:1;min-width:0;overflow-wrap:anywhere}.currency-actions{display:flex;gap:6px}.currency-actions button{width:auto}
.tab-container{display:flex;gap:4px;overflow-x:auto;margin-bottom:12px}.tab-button{width:auto;white-space:nowrap;padding:10px 14px;background:#f3f4f6;border:0;border-bottom:3px solid transparent;border-radius:6px 6px 0 0}.tab-button.active{border-bottom-color:var(--primary);background:#e5f0ff}
.form-section{display:none}.form-section.active{display:block}.toggle-row{display:flex;align-items:center;gap:10px;margin:10px 0}.toggle-row input{width:auto;margin:0}
.stat-grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:10px}.stat{background:#f9fafb;border-radius:10px;padding:14px;text-align:center}.stat h3{margin:4px 0;font-size:24px}
.modal{position:fixed;inset:0;background:#0008;display:flex;justify-content:center;align-items:center;padding:18px;z-index:1000}.modal-box{width:100%;max-width:420px;background:#fff;border-radius:14px;padding:18px}
.loading-overlay{position:fixed;inset:0;background:#ffffffd9;z-index:2000;display:none;justify-content:center;align-items:center;flex-direction:column}.loading-spinner{width:44px;height:44px;border:5px solid #e5e7eb;border-top-color:var(--primary);border-radius:50%;animation:spin 1s linear infinite;margin-bottom:12px}@keyframes spin{to{transform:rotate(360deg)}}
.quote-item{position:relative}.quote-text{font-style:italic;margin-bottom:8px}.quote-author{text-align:right;color:#6b7280}.quote-actions{display:flex;gap:6px;margin-top:8px}
.animated-text-preview{background:linear-gradient(180deg,var(--secondary),#006bff);padding:18px;border-radius:8px;color:#fff;overflow:hidden;white-space:nowrap}.preview-text{display:inline-block;animation:scrollText 20s linear infinite}@keyframes scrollText{0%{transform:translateX(100%)}100%{transform:translateX(-100%)}}
label{font-size:14px;font-weight:600}h2,h3,h4{margin-top:8px}
@media(max-width:600px){.topbar{align-items:flex-start}.top-buttons{width:100%}.top-buttons button{flex:1}.control-row{flex-direction:column;gap:0}.order-box{flex-direction:column}.order-actions{width:100%;display:flex;gap:6px;flex-wrap:wrap}.order-actions button{flex:1;width:auto}.currency-item{align-items:flex-start;flex-direction:column}.currency-actions{width:100%}.currency-actions button{flex:1}.stat-grid{grid-template-columns:1fr 1fr}}
</style>
</head>
<body>
<div id="loadingOverlay" class="loading-overlay"><div class="loading-spinner"></div><b id="loadingTextDisplay">লোড হচ্ছে...</b></div>

<div id="adminLogin" class="modal" style="display:none">
  <div class="modal-box">
    <h2>Admin Login</h2>
    <p class="small">শুধু অনুমোদিত অ্যাডমিন প্রবেশ করুন।</p>
    <label for="adminEmail">Admin Gmail</label><input id="adminEmail" type="email" autocomplete="username" placeholder="Admin Gmail">
    <label for="adminPass">Password</label><input id="adminPass" type="password" autocomplete="current-password" placeholder="Enter Password">
    <label class="toggle-row"><input type="checkbox" id="rememberMe" checked> Keep me logged in</label>
    <button class="primary" style="width:100%" id="loginButton">Login</button>
    <button style="width:100%;padding:10px;margin-top:8px" onclick="location.href='index.html'">Go to User Panel</button>
  </div>
</div>

<header class="topbar">
  <div class="logo"><img id="topLogo" src="https://i.ibb.co.com/DD3h4qjv/file-000000007d947207b10fa3593fc67aa8.png" alt="Dollar Exchange logo">
    <div><div id="topSiteName" style="font-size:16px;font-weight:800;color:#0037dd">Dollar Exchange - Admin</div><div id="topTagline" class="small">Fast &amp; Secure Exchange</div></div>
  </div>
  <span id="opStatus" class="status-badge offline">Offline</span>
  <div class="top-buttons"><button onclick="location.href='index.html'">User Panel</button><button onclick="logoutAdmin()">Logout</button></div>
</header>
<div class="blue-head"><h1>Admin Panel</h1><p class="small" style="color:#e5efff">Manage your currency exchange platform</p></div>

<main id="adminArea" style="display:none">
  <h2 style="text-align:center;margin:18px 8px">🛠 Admin Panel</h2>
  <nav class="card nav">
    <button class="primary" onclick="showSection('dashboard')">Dashboard</button>
    <button class="primary" onclick="showSection('currencies')">Currency Management</button>
    <button class="primary" onclick="showSection('orders')">Order Management</button>
    <button class="primary" onclick="showSection('users')">User Management</button>
    <button class="primary" onclick="showSection('site')">Site Settings</button>
    <button class="primary" onclick="showSection('content')">Content Management</button>
    <button class="primary" onclick="showSection('quotes')">Quote Management</button>
    <button class="primary" onclick="showSection('animations')">Animation Settings</button>
  </nav>

  <section id="section-dashboard" class="card panel-section active"><h3>📊 Admin Dashboard</h3><div id="dashboardStats"><div class="order-empty">লোড হচ্ছে...</div></div></section>

  <section id="section-currencies" class="card panel-section">
    <h3>💱 Currency Management</h3><div id="currencyList"></div><hr>
    <h4>Add New Currency</h4>
    <label>Currency Name</label><input id="newCurrencyName" placeholder="e.g. Payeer">
    <div class="control-row"><div><label>Buy Rate (Tk)</label><input id="newCurrencyBuyRate" type="number" min="0" step="any" placeholder="Buy rate"></div><div><label>Sell Rate (Tk)</label><input id="newCurrencySellRate" type="number" min="0" step="any" placeholder="Sell rate"></div></div>
    <div class="control-row"><div><label>Payment ID</label><input id="newCurrencyId" placeholder="Payment ID"></div><div><label>Image URL</label><input id="newCurrencyImage" placeholder="Image URL (optional)"></div></div>
    <div class="control-row"><div><label>Minimum Dollar</label><input id="newCurrencyMinDollar" type="number" min="0" step="any" placeholder="Default 1"></div><div><label>Maximum Dollar</label><input id="newCurrencyMaxDollar" type="number" min="0" step="any" placeholder="Default 1000"></div></div>
    <button class="primary" onclick="addCurrency()">Add Currency</button>
  </section>

  <section id="section-orders" class="card panel-section"><h3>📦 Order Management</h3><label>Search by user number</label><input id="adminSearch" placeholder="Phone number (blank = all orders)" oninput="debounceOrders()"><div id="adminOrders"></div></section>

  <section id="section-users" class="card panel-section"><h3>👥 User Management</h3><label>Search by email or number</label><input id="userSearch" placeholder="Email or phone number" oninput="debounceUsers()"><div id="userList"></div></section>

  <section id="section-site" class="card panel-section"><h3>⚙️ Site Settings</h3>
    <div class="tab-container" data-tabs="site">
      <button class="tab-button active" onclick="showTab('site','general',this)">General</button><button class="tab-button" onclick="showTab('site','operations',this)">Operations</button><button class="tab-button" onclick="showTab('site','contact',this)">Contact</button><button class="tab-button" onclick="showTab('site','fees',this)">Fees &amp; Limits</button><button class="tab-button" onclick="showTab('site','ui',this)">UI Customization</button><button class="tab-button" onclick="showTab('site','status',this)">Status Control</button>
    </div>
    <div id="site-general" class="form-section active"><label>Site Status Message</label><input id="siteStatus" placeholder="Status message"><label>Site Name</label><input id="siteName" placeholder="Site Name"><label>Tagline</label><input id="siteTagline" placeholder="Tagline"><label class="toggle-row"><input type="checkbox" id="maintenanceMode"> Maintenance Mode</label><label>Maintenance Message</label><textarea id="maintenanceMessage" rows="3"></textarea></div>
    <div id="site-operations" class="form-section"><label>Working Hours (24-hour)</label><div class="control-row"><div><input id="workStartHour" type="number" min="0" max="23" placeholder="Start hour (9)"></div><div><input id="workEndHour" type="number" min="0" max="23" placeholder="End hour (22)"></div></div><label>Order Instructions</label><textarea id="orderInstructions" rows="3"></textarea><label class="toggle-row"><input type="checkbox" id="requireLogin"> Require Login for Orders</label></div>
    <div id="site-contact" class="form-section"><label>WhatsApp Link</label><input id="whatsappLink" placeholder="https://wa.me/..."><label>WhatsApp Button Image URL</label><input id="whatsappImageUrl" placeholder="WhatsApp button image URL (optional)"><label>Contact Email</label><input id="contactEmail" type="email"><label>Contact Phone</label><input id="contactPhone"></div>
    <div id="site-fees" class="form-section"><label>Global Minimum Dollar Amount</label><input id="minDollarAmount" type="number" min="0" step="any"><label>Global Maximum Dollar Amount</label><input id="maxDollarAmount" type="number" min="0" step="any"><label>Transaction Fee (Fixed Tk)</label><input id="transactionFee" type="number" min="0" step="any"><label>Transaction Fee (%)</label><input id="transactionFeePercent" type="number" min="0" step="any"></div>
    <div id="site-ui" class="form-section"><label>Primary Color</label><input id="primaryColor" type="color" value="#0b75ff"><label>Secondary Color</label><input id="secondaryColor" type="color" value="#0037dd"><label>Logo URL</label><input id="logoUrl" placeholder="Logo URL"><label>Favicon URL</label><input id="faviconUrl" placeholder="Favicon URL"><label>Background Color</label><input id="backgroundColor" type="color" value="#f2f5f8"></div>
    <div id="site-status" class="form-section"><label class="toggle-row"><input type="checkbox" id="statusOverrideEnabled"> Enable Manual Status Override</label><div id="manualStatusControl" style="display:none"><label>Manual Status</label><select id="statusOverride"><option value="online">Force Online</option><option value="offline">Force Offline</option></select><p class="small">চালু থাকলে working hours উপেক্ষা করে ম্যানুয়াল স্ট্যাটাস দেখাবে।</p></div><p>Working Hours: <b id="previewHours">9:00 - 22:00</b></p><p>Manual Override: <b id="previewOverride">Disabled</b></p><p>Current Status: <span id="previewStatus" class="status-badge offline">Offline</span></p></div>
    <button class="primary" onclick="saveSiteSettings()">Save Settings</button>
  </section>

  <section id="section-content" class="card panel-section"><h3>📝 Content Management</h3>
    <div class="tab-container" data-tabs="content"><button class="tab-button active" onclick="showTab('content','home',this)">Home Page</button><button class="tab-button" onclick="showTab('content','rules',this)">Rules &amp; Instructions</button><button class="tab-button" onclick="showTab('content','notifications',this)">Notifications</button><button class="tab-button" onclick="showTab('content','payments',this)">Payment Methods</button></div>
    <div id="content-home" class="form-section active"><label>Welcome Title</label><input id="welcomeTitle"><label>Welcome Subtitle</label><input id="welcomeSubtitle"><label>Navigation Button Text</label><input id="navButtonText"><label>Home Page Content</label><textarea id="homeContent" rows="5"></textarea></div>
    <div id="content-rules" class="form-section"><label>Rules Title</label><input id="rulesTitle"><label>Rules Content</label><textarea id="rulesContent" rows="6"></textarea></div>
    <div id="content-notifications" class="form-section"><label>Global Notification</label><input id="globalNotification"><label>Notification Type</label><select id="notificationType"><option value="info">Information</option><option value="warning">Warning</option><option value="success">Success</option><option value="error">Error</option></select><label class="toggle-row"><input type="checkbox" id="notificationActive"> Active</label></div>
    <div id="content-payments" class="form-section"><div id="paymentMethodsList"></div><hr><h4>Add New Payment Method</h4><div class="control-row"><div><label>Payment Method Name</label><input id="newPaymentName" placeholder="bKash / Nagad"></div><div><label>Fee (%)</label><input id="newPaymentFee" type="number" min="0" step="any" placeholder="1.5"></div></div><button class="primary" onclick="addPaymentMethod()">Add Payment Method</button></div>
    <button class="primary" onclick="saveContentSettings()">Save Content</button>
  </section>

  <section id="section-quotes" class="card panel-section"><h3>💬 Quote Management</h3><div id="quotesList"></div><hr><h4>Add New Quote</h4><label>Quote Text</label><textarea id="newQuoteText" rows="3"></textarea><label>Author (optional)</label><input id="newQuoteAuthor"><label class="toggle-row"><input type="checkbox" id="newQuoteFeatured"> Mark as Featured</label><button class="primary" onclick="addQuote()">Add Quote</button></section>

  <section id="section-animations" class="card panel-section"><h3>🎬 Animation Settings</h3>
    <div class="tab-container" data-tabs="animations"><button class="tab-button active" onclick="showTab('animations','loading',this)">Loading Animation</button><button class="tab-button" onclick="showTab('animations','text',this)">Text Animation</button></div>
    <div id="animations-loading" class="form-section active"><label>Loading Text</label><input id="loadingText"><label>Spinner Color</label><input id="spinnerColor" type="color" value="#0b75ff"><label class="toggle-row"><input type="checkbox" id="showLoadingAll" checked> Show Loading on All Operations</label></div>
    <div id="animations-text" class="form-section"><label class="toggle-row"><input type="checkbox" id="animatedTextEnabled"> Enable Animated Text</label><div id="animatedTextControls" style="display:none"><label>Animated Text Content</label><textarea id="animatedText" rows="3"></textarea><label>Animation Speed (seconds)</label><input id="animationSpeed" type="number" min="5" max="60" value="20"><label>Animation Direction</label><select id="animationDirection"><option value="left">Left to Right</option><option value="right">Right to Left</option></select><label>Text Color</label><input id="animatedTextColor" type="color" value="#ffffff"><label>Background Color</label><input id="animatedBgColor" type="color" value="#0037dd"></div><div id="animatedTextPreview" class="animated-text-preview" style="display:none"><span id="previewAnimatedText" class="preview-text">Preview text will appear here</span></div></div>
    <button class="primary" onclick="saveAnimationSettings()">Save Animation Settings</button>
  </section>
</main>

<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>
<script>
'use strict';
// Firebase web config is public by design; protect data with Firestore Security Rules.
const firebaseConfig = {
  apiKey: "AIzaSyAIEGcuV2Os_rqKrpa1ioWy0l0iJI060QM",
  authDomain: "saimon-f4236.firebaseapp.com",
  projectId: "saimon-f4236",
  storageBucket: "saimon-f4236.firebasestorage.app",
  messagingSenderId: "93804376133",
  appId: "1:93804376133:web:38a83389901b4699b0732c"
};
if (!firebase.apps.length) firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

// ================= TELEGRAM NOTIFICATION SETTINGS (সেটআপ সম্পন্ন ✔) =================
const TELEGRAM_BOT_TOKEN = "8842406650:AAESk-VLFq7qwOT2i14SuRAqWDIhNkQ7GNc";
const TELEGRAM_CHAT_ID   = "7427334644";
// ===================================================================

let knownOrderIds = new Set();
let telegramListenerActive = false;

// নতুন অর্ডার ধরার রিয়েল-টাইম লিসেনার
function startTelegramOrderListener(){
  if(telegramListenerActive) return;
  telegramListenerActive = true;
  // পুরনো অর্ডারগুলোর ID মনে রাখি → যাতে পুরনো অর্ডারে নোটিফিকেশন না যায়
  db.collection('orders').get().then(snap=>{
    snap.forEach(doc=>knownOrderIds.add(doc.id));
    db.collection('orders').onSnapshot(snapshot=>{
      snapshot.docChanges().forEach(change=>{
        if(change.type==='added' && !knownOrderIds.has(change.doc.id)){
          knownOrderIds.add(change.doc.id);
          const data = change.doc.data();
          if(!data.telegramSent){ // User Panel থেকে টেলিগ্রাম গেলে ডাবল পাঠাবে না
            sendTelegramOrderNotification({id:change.doc.id, ...data});
            db.collection('orders').doc(change.doc.id).update({telegramSent:true}).catch(()=>{});
          }
        }
      });
    }, err=>console.error('Telegram listener error:', err));
  }).catch(err=>console.error('Order preload error:', err));
}

// টেলিগ্রামে নোটিফিকেশন পাঠানোর ফাংশন
async function sendTelegramOrderNotification(order){
  try{
    const text = [
      "🔔 নতুন Dollar Exchange অর্ডার!",
      "",
      `🆔 Order ID: ${order.id}`,
      `👤 নাম: ${order.name || "উল্লেখ নেই"}`,
      `📱 মোবাইল: ${order.number || "উল্লেখ নেই"}`,
      `💵 পরিমাণ: ${order.dollar ?? "উল্লেখ নেই"} USD`,
      `💰 টাকা: ${order.taka ?? "উল্লেখ নেই"} TK`,
      `💱 কারেন্সি: ${order.currency || "উল্লেখ নেই"}`,
      `💳 পেমেন্ট মাধ্যম: ${order.via || order.paymentMethod || "উল্লেখ নেই"}`,
      `🧾 TXID: ${order.trx || "উল্লেখ নেই"}`,
      `📌 স্ট্যাটাস: ${order.status || "Pending"}`,
    ].join("\n");

    const response = await fetch(
      `https://api.telegram.org/bot${TELEGRAM_BOT_TOKEN}/sendMessage`,
      {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          chat_id: TELEGRAM_CHAT_ID,
          text
        })
      }
    );
    if(!response.ok) console.error('Telegram পাঠানো যায়নি:', response.status, await response.text());
  }catch(e){ console.error('Telegram notification error:', e); }
}
// ================= TELEGRAM NOTIFICATION END =================

// IMPORTANT: This client-side password check is not secure against code inspection.
// Replace with Firebase Authentication + restrictive Firestore Rules before public production use.
const ADMIN_EMAIL = 'sheksimon5@gmail.com';
const ADMIN_PASSWORD = 'saimon500@';

let currencies = [
 {id:'Payeer',name:'Payeer',buyRate:68,sellRate:70,paymentId:'P1131698605',image:'https://i.ibb.co/6yJ1s7Q/payeer-logo.png',minDollar:5,maxDollar:500},
 {id:'Binance',name:'Binance',buyRate:19,sellRate:20,paymentId:'1188473082',image:'https://i.ibb.co/k3QJz5w/binance-logo.png',minDollar:1,maxDollar:1000},
 {id:'Advcash',name:'Advcash',buyRate:58,sellRate:60,paymentId:'U 1048 5654 4714',image:'https://i.ibb.co/1n1J7r6/advcash-logo.png',minDollar:10,maxDollar:300}
];
let paymentMethods=[{id:'bKash',name:'bKash',fee:1.5},{id:'Nagad',name:'Nagad',fee:1.5},{id:'Rocket',name:'Rocket',fee:1.5}];
let quotes=[
 {text:'The best time to plant a tree was 20 years ago. The second best time is now.',author:'Chinese Proverb',featured:true},
 {text:'Money is a terrible master but an excellent servant.',author:'P.T. Barnum',featured:false}
];
let siteSettings={
 status:'',name:'Dollar Exchange',tagline:'Fast & Secure Exchange',primaryColor:'#0b75ff',secondaryColor:'#0037dd',
 workStartHour:9,workEndHour:22,statusOverrideEnabled:false,statusOverride:'online',orderInstructions:'',
 whatsappLink:'https://wa.me/qr/DTBEJ472LPKOA1',whatsappImageUrl:'',contactEmail:'',contactPhone:'',
 minDollarAmount:1,maxDollarAmount:1000,transactionFee:0,transactionFeePercent:0,
 logoUrl:'',faviconUrl:'',backgroundColor:'#f2f5f8',maintenanceMode:false,maintenanceMessage:'',requireLogin:false
};
let animationSettings={loadingText:'লোড হচ্ছে...',spinnerColor:'#0b75ff',showLoadingAll:true,animatedTextEnabled:false,animatedText:'',animationSpeed:20,animationDirection:'right',animatedTextColor:'#ffffff',animatedBgColor:'#0037dd'};
let debounceOrderTimer,debounceUserTimer;

const $ = id => document.getElementById(id);
function showLoading(){ $('loadingTextDisplay').textContent=animationSettings.loadingText||'লোড হচ্ছে...'; if(animationSettings.showLoadingAll!==false)$('loadingOverlay').style.display='flex'; }
function hideLoading(){ $('loadingOverlay').style.display='none'; }
function safeText(value){return value==null?'':String(value);}
function escapeHtml(value){return safeText(value).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));}
function toNumber(value,fallback=0){const n=Number(value);return Number.isFinite(n)?n:fallback;}
function statusClass(status){const s=safeText(status).toUpperCase();return s==='COMPLETED'?'completed':s==='REJECTED'?'rejected':'pending';}
function normalizeStatus(status){const s=safeText(status).toUpperCase();if(s==='COMPLETED'||s==='COMPLETE'||s==='CONFIRMED')return 'COMPLETED';if(s==='REJECTED')return 'REJECTED';return 'PENDING';}
function showSection(name){
 document.querySelectorAll('.panel-section').forEach(el=>el.classList.remove('active'));
 const section=$('section-'+name);if(section)section.classList.add('active');
 if(name==='dashboard')loadDashboard();if(name==='currencies')loadCurrencies();if(name==='orders')loadAdminOrders();if(name==='users'){}if(name==='site')loadSiteSettings();if(name==='content'){loadContentSettings();loadPaymentMethods();}if(name==='quotes')loadQuotes();if(name==='animations')loadAnimationSettings();
}
function showTab(group,name,button){
 const sectionPrefix=group==='site'?'site-':group==='content'?'content-':'animations-';
 document.querySelectorAll('#section-'+group+' .form-section').forEach(el=>el.classList.remove('active'));
 const target=$(sectionPrefix+name);if(target)target.classList.add('active');
 document.querySelectorAll('#section-'+group+' .tab-button').forEach(el=>el.classList.remove('active'));
 if(button)button.classList.add('active');
}
function updateStatus(){
 const hour=new Date().getHours();
 let online;
 if(siteSettings.statusOverrideEnabled) online=siteSettings.statusOverride==='online';
 else {const start=toNumber(siteSettings.workStartHour,9),end=toNumber(siteSettings.workEndHour,22);online=start<=end?(hour>=start&&hour<end):(hour>=start||hour<end);}
 $('opStatus').textContent=online?'Online':'Offline';$('opStatus').className='status-badge '+(online?'online':'offline');
 const p=$('previewStatus');if(p){p.textContent=online?'Online':'Offline';p.className='status-badge '+(online?'online':'offline');}
 $('previewHours').textContent=`${siteSettings.workStartHour}:00 - ${siteSettings.workEndHour}:00`;
 $('previewOverride').textContent=siteSettings.statusOverrideEnabled?'Enabled ('+siteSettings.statusOverride+')':'Disabled';
}
function updateStatusPreview(){
 siteSettings.workStartHour=toNumber($('workStartHour').value,9);siteSettings.workEndHour=toNumber($('workEndHour').value,22);
 siteSettings.statusOverrideEnabled=$('statusOverrideEnabled').checked;siteSettings.statusOverride=$('statusOverride').value;
 $('manualStatusControl').style.display=siteSettings.statusOverrideEnabled?'block':'none';updateStatus();
}
function applyBranding(){
 document.documentElement.style.setProperty('--primary',siteSettings.primaryColor||'#0b75ff');
 document.documentElement.style.setProperty('--secondary',siteSettings.secondaryColor||'#0037dd');
 document.documentElement.style.setProperty('--bg',siteSettings.backgroundColor||'#f2f5f8');
 $('topSiteName').textContent=(siteSettings.name||'Dollar Exchange')+' - Admin';$('topTagline').textContent=siteSettings.tagline||'Fast & Secure Exchange';
 if(siteSettings.logoUrl)$('topLogo').src=siteSettings.logoUrl;
}
async function getSettingsDoc(id){const snap=await db.collection('settings').doc(id).get();return snap.exists?snap.data():null;}
async function saveSettingsDoc(id,data){await db.collection('settings').doc(id).set(data,{merge:true});}

async function loadCurrencies(){
 showLoading();try{const data=await getSettingsDoc('currencies');if(data&&Array.isArray(data.list))currencies=data.list.map(c=>({...c,minDollar:toNumber(c.minDollar,1),maxDollar:toNumber(c.maxDollar,1000)}));}
 catch(e){console.error(e);alert('Currency load error: '+e.message);}finally{hideLoading();}updateCurrencyList();
}
async function saveCurrencies(showMessage=true){
 showLoading();try{await saveSettingsDoc('currencies',{list:currencies});if(showMessage)alert('Currencies updated successfully');}
 catch(e){console.error(e);alert('Currency save error. Firestore Rules/connection check করুন।');}finally{hideLoading();}
}
function updateCurrencyList(){
 const list=$('currencyList');list.innerHTML='';
 if(!currencies.length){list.innerHTML='<div class="order-empty">কোনো currency যোগ করা নেই</div>';return;}
 currencies.forEach((c,i)=>{const item=document.createElement('div');item.className='currency-item';
 item.innerHTML=`<div class="currency-info"><b>${escapeHtml(c.name)}</b><div class="small">Buy Rate: 1 USD = ${toNumber(c.buyRate,c.rate||0)} Tk</div><div class="small">Sell Rate: 1 USD = ${toNumber(c.sellRate,c.rate||0)} Tk</div><div class="small">Payment ID: ${escapeHtml(c.paymentId)}</div><div class="small">Minimum: ${toNumber(c.minDollar,1)} USD · Maximum: ${toNumber(c.maxDollar,1000)} USD</div>${c.image?`<div class="small">Image: ${escapeHtml(c.image)}</div>`:''}</div><div class="currency-actions"><button class="primary" onclick="editCurrency(${i})">Edit</button><button class="danger" onclick="deleteCurrency(${i})">Delete</button></div>`;list.appendChild(item);});
}
async function addCurrency(){
 const name=$('newCurrencyName').value.trim(),buyRate=Number($('newCurrencyBuyRate').value),sellRate=Number($('newCurrencySellRate').value),paymentId=$('newCurrencyId').value.trim(),image=$('newCurrencyImage').value.trim();
 const minRaw=$('newCurrencyMinDollar').value,maxRaw=$('newCurrencyMaxDollar').value,minDollar=minRaw===''?1:Number(minRaw),maxDollar=maxRaw===''?1000:Number(maxRaw);
 if(!name||$('newCurrencyBuyRate').value===''||$('newCurrencySellRate').value===''||!paymentId||!Number.isFinite(buyRate)||!Number.isFinite(sellRate)||buyRate<0||sellRate<0||minDollar<0||maxDollar<minDollar){alert('সব required field ঠিকভাবে পূরণ করুন।');return;}
 const id=name.toLowerCase().replace(/\s+/g,'_');if(currencies.some(c=>String(c.id).toLowerCase()===id)){alert('এই currency আগে থেকেই আছে।');return;}
 currencies.push({id,name,buyRate,sellRate,paymentId,image,minDollar,maxDollar});await saveCurrencies(false);updateCurrencyList();
 ['newCurrencyName','newCurrencyBuyRate','newCurrencySellRate','newCurrencyId','newCurrencyImage','newCurrencyMinDollar','newCurrencyMaxDollar'].forEach(id=>$(id).value='');
}
async function editCurrency(i){
 const c=currencies[i];if(!c)return;
 const name=prompt('Currency name:',c.name);if(name===null)return;if(!name.trim()){alert('Name ফাঁকা রাখা যাবে না।');return;}
 const buy=prompt('Buy Rate (Tk):',c.buyRate??c.rate??0);if(buy===null)return;
 const sell=prompt('Sell Rate (Tk):',c.sellRate??c.rate??0);if(sell===null)return;
 const paymentId=prompt('Payment ID:',c.paymentId||'');if(paymentId===null)return;
 const image=prompt('Image URL:',c.image||'');if(image===null)return;
 const min=prompt('Minimum Dollar:',c.minDollar??1);if(min===null)return;
 const max=prompt('Maximum Dollar:',c.maxDollar??1000);if(max===null)return;
 const vals=[Number(buy),Number(sell),Number(min),Number(max)];if(vals.some(v=>!Number.isFinite(v)||v<0)||vals[3]<vals[2]){alert('Rate/limit সঠিক সংখ্যা হতে হবে।');return;}
 currencies[i]={...c,name:name.trim(),buyRate:vals[0],sellRate:vals[1],paymentId,image,minDollar:vals[2],maxDollar:vals[3]};await saveCurrencies(false);updateCurrencyList();
}
async function deleteCurrency(i){if(!currencies[i]||!confirm('এই currency delete করবেন?'))return;currencies.splice(i,1);await saveCurrencies(false);updateCurrencyList();}

async function loadPaymentMethods(){
 try{const data=await getSettingsDoc('paymentMethods');if(data&&Array.isArray(data.list))paymentMethods=data.list;}catch(e){console.error(e);}
 updatePaymentMethodsList();
}
async function savePaymentMethods(showMessage=true){showLoading();try{await saveSettingsDoc('paymentMethods',{list:paymentMethods});if(showMessage)alert('Payment methods updated successfully');}catch(e){console.error(e);alert('Payment method save error.');}finally{hideLoading();}}
function updatePaymentMethodsList(){
 const list=$('paymentMethodsList');list.innerHTML='';
 paymentMethods.forEach((m,i)=>{const item=document.createElement('div');item.className='currency-item';item.innerHTML=`<div class="currency-info"><b>${escapeHtml(m.name)}</b><div class="small">Fee: ${toNumber(m.fee)}%</div></div><div class="currency-actions"><button class="primary" onclick="editPaymentMethod(${i})">Edit</button><button class="danger" onclick="deletePaymentMethod(${i})">Delete</button></div>`;list.appendChild(item);});
 if(!paymentMethods.length)list.innerHTML='<div class="order-empty">কোনো payment method নেই</div>';
}
async function addPaymentMethod(){
 const name=$('newPaymentName').value.trim(),feeRaw=$('newPaymentFee').value,fee=Number(feeRaw),id=name.toLowerCase().replace(/\s+/g,'_');
 if(!name||feeRaw===''||!Number.isFinite(fee)||fee<0){alert('নাম ও সঠিক fee দিন।');return;}
 if(paymentMethods.some(m=>m.id===id)){alert('এই payment method আগে থেকেই আছে।');return;}
 paymentMethods.push({id,name,fee});await savePaymentMethods(false);updatePaymentMethodsList();$('newPaymentName').value='';$('newPaymentFee').value='';
}
async function editPaymentMethod(i){const m=paymentMethods[i];const name=prompt('Payment method name:',m.name);if(name===null)return;const fee=prompt('Fee (%):',m.fee);if(fee===null)return;if(!name.trim()||!Number.isFinite(Number(fee))||Number(fee)<0){alert('সঠিক তথ্য দিন।');return;}paymentMethods[i]={...m,name:name.trim(),fee:Number(fee)};await savePaymentMethods(false);updatePaymentMethodsList();}
async function deletePaymentMethod(i){if(!confirm('এই payment method delete করবেন?'))return;paymentMethods.splice(i,1);await savePaymentMethods(false);updatePaymentMethodsList();}

async function loadQuotes(){showLoading();try{const data=await getSettingsDoc('quotes');if(data&&Array.isArray(data.list))quotes=data.list;}catch(e){console.error(e);alert('Quotes load error');}finally{hideLoading();}updateQuotesList();}
async function saveQuotes(showMessage=true){showLoading();try{await saveSettingsDoc('quotes',{list:quotes});if(showMessage)alert('Quotes updated successfully');}catch(e){console.error(e);alert('Quote save error');}finally{hideLoading();}}
function updateQuotesList(){const list=$('quotesList');list.innerHTML='';if(!quotes.length){list.innerHTML='<div class="order-empty">কোনো quote নেই</div>';return;}quotes.forEach((q,i)=>{const item=document.createElement('div');item.className='quote-item';item.innerHTML=`<div class="quote-text">“${escapeHtml(q.text)}”</div><div class="quote-author">— ${escapeHtml(q.author||'Anonymous')}</div><div class="quote-actions">${q.featured?'<span class="status pending">Featured</span>':''}<button class="primary" onclick="editQuote(${i})">Edit</button><button class="danger" onclick="deleteQuote(${i})">Delete</button></div>`;list.appendChild(item);});}
async function addQuote(){const text=$('newQuoteText').value.trim(),author=$('newQuoteAuthor').value.trim(),featured=$('newQuoteFeatured').checked;if(!text){alert('Quote text লিখুন।');return;}quotes.push({text,author,featured,createdAt:new Date().toISOString()});await saveQuotes(false);updateQuotesList();$('newQuoteText').value='';$('newQuoteAuthor').value='';$('newQuoteFeatured').checked=false;}
async function editQuote(i){const q=quotes[i],text=prompt('Quote text:',q.text);if(text===null)return;const author=prompt('Author:',q.author||'');if(author===null)return;const featured=confirm('Featured হিসেবে দেখাবেন? OK = Yes, Cancel = No');quotes[i]={...q,text,author,featured};await saveQuotes(false);updateQuotesList();}
async function deleteQuote(i){if(!confirm('এই quote delete করবেন?'))return;quotes.splice(i,1);await saveQuotes(false);updateQuotesList();}

async function loadAdminOrders(){
 const out=$('adminOrders');if(!out)return;showLoading();
 try{
  const q=$('adminSearch').value.trim();let query=db.collection('orders');
  // Firestore equality query avoids downloading every order when a phone number is entered.
  if(q)query=query.where('number','==',q);
  const snap=await query.get();const orders=snap.docs.map(doc=>({id:doc.id,...doc.data()}));
  orders.sort((a,b)=>{const da=Date.parse(a.createdAt)||0,dbb=Date.parse(b.createdAt)||0;return dbb-da;});
  out.innerHTML='';if(!orders.length){out.innerHTML='<div class="order-empty">কোনো অর্ডার পাওয়া যায়নি</div>';return;}
  orders.forEach(o=>{
   const status=normalizeStatus(o.status),created=o.createdAt?new Date(o.createdAt).toLocaleString():'সময় দেওয়া নেই';
   const div=document.createElement('div');div.className='order-box';
   div.innerHTML=`<div class="order-info"><b>${escapeHtml(o.name||'Unknown User')}</b> <span class="small">• ${escapeHtml(created)}</span><div class="small">নম্বর: ${escapeHtml(o.number||'—')} • ${escapeHtml(o.currency||'—')} • ${escapeHtml(o.dollar??'—')} USD → ${escapeHtml(o.taka??'—')} TK</div><div class="small">মাধ্যম: ${escapeHtml(o.via||o.paymentMethod||'Not given')}</div><div class="small">TXID: ${escapeHtml(o.trx||'—')}</div><div class="small">Trade Type: ${escapeHtml(o.tradeType||'sell')}</div><p><span class="status ${statusClass(status)}">${status}</span></p></div><div class="order-actions"><button class="success" onclick="adminChangeStatus('${o.id}','COMPLETED')">Approve</button><button class="danger" onclick="adminChangeStatus('${o.id}','REJECTED')">Reject</button><button class="warning" onclick="adminChangeStatus('${o.id}','PENDING')">Pending</button></div>`;
   out.appendChild(div);
  });
 }catch(e){console.error(e);out.innerHTML='<div class="order-empty">Orders load হয়নি। Firestore Rules/connection পরীক্ষা করুন।</div>';}finally{hideLoading();}
}
function debounceOrders(){clearTimeout(debounceOrderTimer);debounceOrderTimer=setTimeout(loadAdminOrders,350);}
async function adminChangeStatus(id,newStatus){
 const normalized=normalizeStatus(newStatus);if(!['COMPLETED','REJECTED','PENDING'].includes(normalized))return;
 if(normalized==='COMPLETED'){
  try{const doc=await db.collection('orders').doc(id).get();if(!doc.exists){alert('Order not found');return;}const trx=safeText(doc.data().trx).trim();if(!trx){alert('⚠ TXID ছাড়া Approve করা যাবে না!');return;}}
  catch(e){console.error(e);alert('Order যাচাই করা যায়নি।');return;}
 }
 if(!confirm('Order status '+normalized+' করবেন?'))return;
 showLoading();try{await db.collection('orders').doc(id).update({status:normalized});await loadAdminOrders();alert('Status updated');}catch(e){console.error(e);alert('Status update হয়নি। Firestore Rules পরীক্ষা করুন।');}finally{hideLoading();}
}

async function searchUsers(){
 const out=$('userList'),q=$('userSearch').value.trim();out.innerHTML='';
 if(!q){out.innerHTML='<div class="order-empty">Search করতে email বা phone number লিখুন</div>';return;}
 showLoading();
 try{
  const [emailSnap,numberSnap]=await Promise.all([
   db.collection('users').where('email','==',q.toLowerCase()).get(),
   db.collection('users').where('number','==',q).get()
  ]);
  const users=new Map();[...emailSnap.docs,...numberSnap.docs].forEach(d=>users.set(d.id,{id:d.id,...d.data()}));
  if(!users.size){out.innerHTML='<div class="order-empty">কোনো user পাওয়া যায়নি</div>';return;}
  users.forEach(u=>{const div=document.createElement('div');div.className='order-box';div.innerHTML=`<div class="order-info"><b>${escapeHtml(u.name||'Unnamed')}</b> (${escapeHtml(u.userType||'regular')})<div class="small">Email: ${escapeHtml(u.email||'—')}</div><div class="small">Phone: ${escapeHtml(u.number||'—')}</div><div class="small">Joined: ${escapeHtml(u.createdAt?new Date(u.createdAt).toLocaleString():'—')}</div></div><div class="order-actions"><button class="primary" onclick="viewUserOrders('${escapeHtml(u.number||'')}')">View Orders</button><button class="warning" onclick="editUser('${u.id}')">Edit</button><button class="danger" onclick="deleteUser('${u.id}')">Delete</button></div>`;out.appendChild(div);});
 }catch(e){console.error(e);out.innerHTML='<div class="order-empty">Users load হয়নি। Firestore Rules পরীক্ষা করুন।</div>';}finally{hideLoading();}
}
function debounceUsers(){clearTimeout(debounceUserTimer);debounceUserTimer=setTimeout(searchUsers,350);}
function viewUserOrders(number){showSection('orders');$('adminSearch').value=number;loadAdminOrders();}
async function editUser(id){
 showLoading();try{const doc=await db.collection('users').doc(id).get();if(!doc.exists){alert('User not found');return;}const u=doc.data();const name=prompt('User name:',u.name||'');if(name===null)return;const number=prompt('Phone number:',u.number||'');if(number===null)return;const userType=prompt('User type:',u.userType||'regular');if(userType===null)return;await db.collection('users').doc(id).update({name,number,userType});alert('User updated');await searchUsers();}catch(e){console.error(e);alert('User update error');}finally{hideLoading();}
}
async function deleteUser(id){if(!confirm('এই user delete করবেন? এটি ফিরিয়ে আনা যাবে না।'))return;showLoading();try{await db.collection('users').doc(id).delete();alert('User deleted');await searchUsers();}catch(e){console.error(e);alert('User delete error');}finally{hideLoading();}}

async function loadDashboard(){
 showLoading();try{
  const [ordersSnap,usersSnap]=await Promise.all([
   db.collection('orders').get(),db.collection('users').get()
  ]);
  let totalValue=0;ordersSnap.forEach(d=>{const o=d.data();if(normalizeStatus(o.status)==='COMPLETED')totalValue+=toNumber(o.taka);});
  const pending=ordersSnap.docs.filter(d=>normalizeStatus(d.data().status)==='PENDING').length;
  const completed=ordersSnap.docs.filter(d=>normalizeStatus(d.data().status)==='COMPLETED').length;
  const rejected=ordersSnap.docs.filter(d=>normalizeStatus(d.data().status)==='REJECTED').length;
  $('dashboardStats').innerHTML=`<div class="stat-grid"><div class="stat"><h3>${ordersSnap.size}</h3><div>Total Orders</div></div><div class="stat"><h3>${pending}</h3><div>Pending Orders</div></div><div class="stat"><h3>${completed}</h3><div>Completed Orders</div></div><div class="stat"><h3>${rejected}</h3><div>Rejected Orders</div></div><div class="stat"><h3>${usersSnap.size}</h3><div>Total Users</div></div><div class="stat"><h3>${totalValue.toFixed(2)} Tk</h3><div>Total Transaction Value</div></div></div>`;
 }catch(e){console.error(e);$('dashboardStats').innerHTML='<div class="order-empty">Dashboard load হয়নি। Firestore Rules/connection পরীক্ষা করুন।</div>';}finally{hideLoading();}
}

const siteFieldMap={
 status:'siteStatus',name:'siteName',tagline:'siteTagline',primaryColor:'primaryColor',secondaryColor:'secondaryColor',
 workStartHour:'workStartHour',workEndHour:'workEndHour',statusOverrideEnabled:'statusOverrideEnabled',statusOverride:'statusOverride',
 orderInstructions:'orderInstructions',whatsappLink:'whatsappLink',whatsappImageUrl:'whatsappImageUrl',contactEmail:'contactEmail',contactPhone:'contactPhone',
 minDollarAmount:'minDollarAmount',maxDollarAmount:'maxDollarAmount',transactionFee:'transactionFee',transactionFeePercent:'transactionFeePercent',
 logoUrl:'logoUrl',faviconUrl:'faviconUrl',backgroundColor:'backgroundColor',maintenanceMode:'maintenanceMode',maintenanceMessage:'maintenanceMessage',requireLogin:'requireLogin'
};
async function loadSiteSettings(){
 showLoading();try{const data=await getSettingsDoc('site');if(data)siteSettings={...siteSettings,...data};
  Object.entries(siteFieldMap).forEach(([key,id])=>{const el=$(id);if(!el)return;if(el.type==='checkbox')el.checked=!!siteSettings[key];else el.value=siteSettings[key]??'';});
  $('manualStatusControl').style.display=siteSettings.statusOverrideEnabled?'block':'none';applyBranding();updateStatus();
 }catch(e){console.error(e);alert('Site settings load error');}finally{hideLoading();}
}
async function saveSiteSettings(){
 const data={};Object.entries(siteFieldMap).forEach(([key,id])=>{const el=$(id);if(!el)return;if(el.type==='checkbox')data[key]=el.checked;else if(['workStartHour','workEndHour'].includes(key))data[key]=toNumber(el.value,key==='workStartHour'?9:22);else if(['minDollarAmount','maxDollarAmount','transactionFee','transactionFeePercent'].includes(key))data[key]=toNumber(el.value,0);else data[key]=el.value.trim();});
 if(data.workStartHour<0||data.workStartHour>23||data.workEndHour<0||data.workEndHour>23){alert('Working hours 0–23 এর মধ্যে দিন।');return;}
 if(data.minDollarAmount<0||data.maxDollarAmount<data.minDollarAmount){alert('Dollar limits ঠিক করুন।');return;}
 showLoading();try{await saveSettingsDoc('site',data);siteSettings={...siteSettings,...data};applyBranding();updateStatus();alert('Site settings updated successfully');}catch(e){console.error(e);alert('Settings save হয়নি। Firestore Rules পরীক্ষা করুন।');}finally{hideLoading();}
}

async function loadContentSettings(){
 showLoading();try{const data=await getSettingsDoc('content');const c=data||{};
 const defaults={welcomeTitle:'Welcome to Dollar Exchange',welcomeSubtitle:'দয়া করে ট্রানজেকশন শুরু করার আগে নিয়মগুলো পড়ে নিন',navButtonText:'নগদ বিকাশ 5 টাকা সেন্ড মানি ফি কেটে নেওয়া হয়',homeContent:'',rulesTitle:'Exchange Rules',rulesContent:'Please read all rules before making a transaction.',globalNotification:'',notificationType:'info',notificationActive:false};
 Object.entries(defaults).forEach(([key,val])=>{const el=$(key);if(el){if(el.type==='checkbox')el.checked=!!(c[key]??val);else el.value=c[key]??val;}});
 }catch(e){console.error(e);alert('Content settings load error');}finally{hideLoading();}
}
async function saveContentSettings(){
 const data={};['welcomeTitle','welcomeSubtitle','navButtonText','homeContent','rulesTitle','rulesContent','globalNotification','notificationType','notificationActive'].forEach(key=>{const el=$(key);data[key]=el.type==='checkbox'?el.checked:el.value;});
 showLoading();try{await saveSettingsDoc('content',data);alert('Content settings updated successfully');}catch(e){console.error(e);alert('Content save হয়নি। Firestore Rules পরীক্ষা করুন।');}finally{hideLoading();}
}
async function loadAnimationSettings(){
 showLoading();try{const data=await getSettingsDoc('animations');if(data)animationSettings={...animationSettings,...data};
  $('loadingText').value=animationSettings.loadingText;$('spinnerColor').value=animationSettings.spinnerColor;$('showLoadingAll').checked=animationSettings.showLoadingAll!==false;
  $('animatedTextEnabled').checked=!!animationSettings.animatedTextEnabled;$('animatedText').value=animationSettings.animatedText||'';$('animationSpeed').value=animationSettings.animationSpeed||20;$('animationDirection').value=animationSettings.animationDirection||'right';$('animatedTextColor').value=animationSettings.animatedTextColor||'#ffffff';$('animatedBgColor').value=animationSettings.animatedBgColor||'#0037dd';
  $('animatedTextControls').style.display=animationSettings.animatedTextEnabled?'block':'none';$('animatedTextPreview').style.display=animationSettings.animatedTextEnabled?'block':'none';updateAnimatedTextPreview();
 }catch(e){console.error(e);alert('Animation settings load error');}finally{hideLoading();}
}
function updateAnimatedTextPreview(){
 const text=$('animatedText').value||'Preview text will appear here',speed=Math.min(60,Math.max(5,toNumber($('animationSpeed').value,20))),direction=$('animationDirection').value;
 $('previewAnimatedText').textContent=text;$('previewAnimatedText').style.color=$('animatedTextColor').value;$('previewAnimatedText').style.animation=`scrollText ${speed}s linear infinite`;if(direction==='left')$('previewAnimatedText').style.animationDirection='reverse';
 $('animatedTextPreview').style.background=$('animatedBgColor').value;
}
async function saveAnimationSettings(){
 animationSettings={loadingText:$('loadingText').value.trim()||'লোড হচ্ছে...',spinnerColor:$('spinnerColor').value,showLoadingAll:$('showLoadingAll').checked,animatedTextEnabled:$('animatedTextEnabled').checked,animatedText:$('animatedText').value,animationSpeed:Math.min(60,Math.max(5,toNumber($('animationSpeed').value,20))),animationDirection:$('animationDirection').value,animatedTextColor:$('animatedTextColor').value,animatedBgColor:$('animatedBgColor').value};
 showLoading();try{await saveSettingsDoc('animations',animationSettings);document.documentElement.style.setProperty('--primary',siteSettings.primaryColor||'#0b75ff');alert('Animation settings updated successfully');}catch(e){console.error(e);alert('Animation settings save হয়নি।');}finally{hideLoading();}
}

function checkAdmin(){
 const email=$('adminEmail').value.trim().toLowerCase(),pass=$('adminPass').value;
 if(email===ADMIN_EMAIL.toLowerCase()&&pass===ADMIN_PASSWORD){
  $('adminLogin').style.display='none';$('adminArea').style.display='block';
  if($('rememberMe').checked){localStorage.setItem('adminLoggedIn','true');localStorage.setItem('adminEmail',email);sessionStorage.removeItem('adminLoggedIn');}
  else{sessionStorage.setItem('adminLoggedIn','true');localStorage.removeItem('adminLoggedIn');localStorage.removeItem('adminEmail');}
  showAdmin();
 }else alert('❌ Wrong Email or Password!');
}
function showAdmin(){ $('adminLogin').style.display='none';$('adminArea').style.display='block';showSection('dashboard');loadSiteSettings();loadCurrencies();startTelegramOrderListener(); }
function logoutAdmin(){localStorage.removeItem('adminLoggedIn');localStorage.removeItem('adminEmail');sessionStorage.removeItem('adminLoggedIn');$('adminArea').style.display='none';$('adminPass').value='';$('adminLogin').style.display='flex';telegramListenerActive=false;}
function bindEvents(){
 $('loginButton').addEventListener('click',checkAdmin);
 $('adminPass').addEventListener('keydown',e=>{if(e.key==='Enter')checkAdmin();});
 $('statusOverrideEnabled').addEventListener('change',updateStatusPreview);
 $('statusOverride').addEventListener('change',updateStatusPreview);
 $('workStartHour').addEventListener('input',updateStatusPreview);$('workEndHour').addEventListener('input',updateStatusPreview);
 $('animatedTextEnabled').addEventListener('change',()=>{const on=$('animatedTextEnabled').checked;$('animatedTextControls').style.display=on?'block':'none';$('animatedTextPreview').style.display=on?'block':'none';if(on)updateAnimatedTextPreview();});
 ['animatedText','animationSpeed','animatedTextColor','animatedBgColor'].forEach(id=>$(id).addEventListener('input',updateAnimatedTextPreview));
 $('animationDirection').addEventListener('change',updateAnimatedTextPreview);
}
window.addEventListener('load',()=>{
 bindEvents();updateStatus();setInterval(updateStatus,60000);
 const logged=localStorage.getItem('adminLoggedIn')==='true'||sessionStorage.getItem('adminLoggedIn')==='true';
 if(logged){$('adminEmail').value=localStorage.getItem('adminEmail')||ADMIN_EMAIL;showAdmin();}
 else $('adminLogin').style.display='flex';
});
</script>
</body>
</html>
