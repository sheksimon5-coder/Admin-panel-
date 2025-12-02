<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Dollar Exchange - Admin Panel</title>
<style>
body{font-family: sans-serif;background:#f2f5f8;margin:0;color:#111}
.topbar{background:#fff;padding:10px 12px;display:flex;align-items:center;justify-content:space-between;box-shadow:0 2px 6px rgba(0,0,0,0.06)}
.logo{display:flex;align-items:center;gap:10px}
.logo img{width:44px;height:44px;border-radius:8px;object-fit:cover}
.status-badge{padding:6px 12px;border-radius:8px;color:#fff;font-size:14px;font-weight:700}
.offline{background:#ef4444}
.online{background:#22c55e}
.top-buttons{display:flex;gap:8px}
.top-buttons button{padding:6px 12px;border-radius:20px;border:1px solid #0b75ff;background:#fff;color:#0b75ff;font-weight:700}

.blue-head{background:linear-gradient(180deg,#0037dd,#006bff);padding:36px 18px;text-align:center;border-bottom-left-radius:26px;border-bottom-right-radius:26px;color:#fff}
.blue-head h1{margin:0;font-size:26px;letter-spacing:0.2px}

.card{width:92%;max-width:720px;margin:14px auto;background:#fff;padding:14px;border-radius:10px;box-shadow:0 2px 10px rgba(0,0,0,0.06)}

input,select,button,textarea{width:100%;padding:10px;margin:8px 0;border-radius:8px;border:1px solid #ddd;font-size:15px}
button.primary{background:#0b75ff;color:#fff;border:none;padding:11px;border-radius:8px;cursor:pointer}
button.danger{background:#ef4444;color:#fff;border:none;padding:11px;border-radius:8px;cursor:pointer}
button.success{background:#16a34a;color:#fff;border:none;padding:11px;border-radius:8px;cursor:pointer}

.order-box{background:#fff;margin:10px;border-radius:10px;padding:12px;box-shadow:0 1px 6px rgba(0,0,0,0.06);display:flex;justify-content:space-between;align-items:center;gap:8px}
.order-info{flex:1}
.order-meta{min-width:110px;text-align:right}
.status{padding:6px 10px;border-radius:6px;color:#fff;font-weight:700}
.pending{background:#f59e0b}
.completed{background:#16a34a}
.rejected{background:#ef4444}

.small{font-size:13px;color:#666}
.modal{position:fixed;inset:0;background:rgba(0,0,0,0.5);display:flex;justify-content:center;align-items:flex-end;padding:20px;z-index:999}
.modal .box{width:100%;max-width:520px;background:#fff;border-radius:12px;padding:16px}

.id-badge{background:#f3f4f6;padding:8px;border-radius:8px;display:inline-block;width:100%}
.order-empty{text-align:center;color:#6b7280;padding:26px}

.control-row{display:flex;gap:10px}
.control-row > *{flex:1}

.currency-item{background:#f9fafb;padding:10px;border-radius:8px;margin-bottom:10px;display:flex;justify-content:space-between;align-items:center}
.currency-info{flex:1}
.currency-actions{display:flex;gap:5px}

@media (max-width:520px){
.topbar{padding:8px}
.blue-head{padding:24px 12px}
}
</style>
</head>
<body>

<!-- Admin Login -->
<div id="adminLogin" class="modal" style="display:flex;">
<div class="box">
<h3>Admin Login</h3>
<input id="adminEmail" type="email" placeholder="Admin Gmail">
<input id="adminPass" type="password" placeholder="Enter Password">
<button class="primary" onclick="checkAdmin()">Login</button>
<div style="text-align:right;margin-top:8px"><button onclick="window.location.href='index.html'">Go to User Panel</button></div>
</div>
</div>

<!-- TOP BAR -->
<div class="topbar">
<div class="logo">
<img src="https://i.ibb.co/5WWPvnQj/20251128-160723.png" alt="logo">
<div>
<div style="font-size:16px;font-weight:800;color:#0037dd">dollar bay sall - Admin</div>
<div class="small">Fast & Secure Exchange</div>
</div>
</div>

<div id="opStatus" class="status-badge offline">Offline</div>

<div class="top-buttons">
<button onclick="window.location.href='index.html'">User Panel</button>
<button onclick="logoutAdmin()">Logout</button>
</div>
</div>

<div class="blue-head">
<h1>Admin Panel</h1>
<p class="small">Manage your currency exchange platform</p>
</div>

<!-- ADMIN AREA -->
<div id="adminArea" style="display:none;">
<h2 style="text-align:center;margin-top:18px">🛠 Admin Panel</h2>

<div class="card">
<div style="display:flex;gap:10px;justify-content:center">
<button class="primary" onclick="showAdminDashboard()">Dashboard</button>
<button class="primary" onclick="showCurrencyManagement()">Currency Management</button>
<button class="primary" onclick="showOrderManagement()">Order Management</button>
<button class="primary" onclick="showUserManagement()">User Management</button>
</div>
</div>

<div id="adminDashboard" class="card">
<h3>📊 Admin Dashboard</h3>
<div id="dashboardStats"></div>
</div>

<div id="currencyManagement" class="card" style="display:none;">
<h3>💱 Currency Management</h3>
<div id="currencyList"></div>
<div style="margin-top:20px">
<h4>Add New Currency</h4>
<input id="newCurrencyName" placeholder="Currency Name" />
<input id="newCurrencyRate" type="number" placeholder="Rate (1 USD = ? Tk)" />
<input id="newCurrencyId" placeholder="Payment ID" />
<button class="primary" onclick="addCurrency()">Add Currency</button>
</div>
</div>

<div id="orderManagement" class="card" style="display:none;">
<h3>📦 Order Management</h3>
<div style="margin-bottom:10px"><b>Search by user number:</b>
<input id="adminSearch" placeholder="phone number" oninput="loadAdminOrders()" />
</div>
<div id="adminOrders"></div>
</div>

<div id="userManagement" class="card" style="display:none;">
<h3>👥 User Management</h3>
<div style="margin-bottom:10px"><b>Search by email or number:</b>
<input id="userSearch" placeholder="email or phone number" oninput="searchUsers()" />
</div>
<div id="userList"></div>
</div>

<div class="card">
<h3>⚙️ Site Settings</h3>
<div>
<label>Site Status Message</label>
<input id="siteStatus" placeholder="Status message" />
<button class="primary" onclick="saveSiteSettings()">Save Settings</button>
</div>
</div>
</div>

<!-- Firebase SDK -->
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-messaging-compat.js"></script>

<script>
// Firebase Config
const firebaseConfig = {
apiKey: "AIzaSyBUMQNzDO6cRdRHFPYOGOBfApzuaidkw2E",
authDomain: "dollar-exchange-bdt.firebaseapp.com",
projectId: "dollar-exchange-bdt",
storageBucket: "dollar-exchange-bdt.firebasestorage.app",
messagingSenderId: "666904772438",
appId: "1:666904772438:web:860c9207c1a1e59a2820f7"
};
firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

// ADMIN CREDENTIALS
const ADMIN_EMAIL = 'sheksimon5@gmail.com';
const ADMIN_PASSWORD = 'saimon500';

// ONLINE/OFFLINE STATUS
function updateStatus(){
const hour = new Date().getHours();
if(hour>=22 || hour<9){
opStatus.innerText='Offline';
opStatus.className='status-badge offline';
} else {
opStatus.innerText='Online';
opStatus.className='status-badge online';
}
}
setInterval(updateStatus,60000);
updateStatus();

// DEFAULT CURRENCIES
let currencies = [
{ id: 'Payeer', name: 'Payeer', rate: 70, paymentId: 'P1131698605' },
{ id: 'Binance', name: 'Binance', rate: 20, paymentId: '1188473082' },
{ id: 'Advcash', name: 'Advcash', rate: 60, paymentId: 'U 1048 5654 4714' }
];

// ADMIN LOGIN CHECK
function checkAdmin(){
const email = adminEmail.value.trim();
const pass = adminPass.value.trim();
if(email === ADMIN_EMAIL && pass === ADMIN_PASSWORD){ 
adminLogin.style.display="none"; 
showAdmin(); 
} else { 
alert("❌ Wrong Email or Password!"); 
}
}
function logoutAdmin() {
adminLogin.style.display="flex";
adminArea.style.display='none';
}

// SHOW ADMIN + LOAD DATA
function showAdmin(){ 
adminArea.style.display='block'; 
showAdminDashboard();
loadSiteSettings();
loadCurrencies();
requestNotificationPermission(); // Push notification
}

// 🔔 PUSH NOTIFICATION FUNCTIONS
const messaging = firebase.messaging();
async function requestNotificationPermission() {
    try {
        const permission = await Notification.requestPermission();
        if(permission === "granted"){
            const token = await messaging.getToken({
                vapidKey: "Voluntary Application Server Identification" // আপনার VAPID KEY বসাবেন
            });
            db.collection("adminTokens").doc("main").set({ token });
            console.log("Admin token saved:", token);
        }
    } catch(e) {
        console.error("Notification permission error:", e);
    }
}

function showAdminPopup(msg){
    alert("🔔 নতুন অর্ডার এসেছে!\n" + msg);
}

// LISTEN FOR NEW ORDERS IN REALTIME
db.collection("orders").orderBy("createdAt","desc").limit(1)
.onSnapshot(snapshot=>{
    snapshot.docChanges().forEach(change=>{
        if(change.type==='added'){
            const o = change.doc.data();
            const message = `${o.name} • ${o.dollar} USD • ${o.taka} TK`;
            showAdminPopup(message);
            if("serviceWorker" in navigator){
                navigator.serviceWorker.ready.then(reg=>{
                    reg.showNotification("নতুন অর্ডার এসেছে!",{
                        body: message,
                        icon: "https://i.ibb.co/5WWPvnQj/20251128-160723.png",
                        badge: "https://i.ibb.co/5WWPvnQj/20251128-160723.png"
                    });
                });
            }
        }
    });
});

// --- Your previous functions (loadCurrencies, addCurrency, editCurrency, deleteCurrency, loadAdminOrders, adminChangeStatus, searchUsers, viewUserOrders, loadDashboard, loadSiteSettings, saveSiteSettings, showAdminDashboard, showCurrencyManagement, showOrderManagement, showUserManagement) ---
// এই অংশ ঠিক যেমন আছে আপনার মূল কোডের মতো রাখুন

// ON LOAD CHECK
window.addEventListener('load',()=>{
const isLoggedIn = localStorage.getItem('adminLoggedIn') === 'true';
if(isLoggedIn){ adminLogin.style.display="none"; showAdmin(); }
});
</script>

</body>
</html>
