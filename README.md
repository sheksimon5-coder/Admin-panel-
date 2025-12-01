


<html lang="bn">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Admin Panel - Dollar Exchange (Rate Control)</title>
<!-- Firebase SDK -->
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>
<style>
body{font-family: sans-serif;background:#eef2f5;margin:0;padding:0;color:#111}
.topbar{background:#0b75ff;padding:12px 16px;display:flex;align-items:center;justify-content:space-between;color:#fff}
.topbar .brand{display:flex;gap:12px;align-items:center}
.topbar img{width:44px;height:44px;border-radius:8px;object-fit:cover}
.topbar h1{font-size:18px;margin:0}
.status-badge{padding:6px 12px;border-radius:8px;color:#fff;font-weight:700}
.offline{background:#ef4444}
.online{background:#22c55e}

.container{padding:16px;max-width:980px;margin:18px auto}
.grid{display:grid;grid-template-columns:1fr 380px;gap:14px}
.card{background:#fff;padding:14px;border-radius:10px;box-shadow:0 6px 18px rgba(2,6,23,0.06)}
.card h3{margin:0 0 10px 0}
label{font-weight:700;font-size:14px}
input[type=number], input[type=text], input[type=password]{width:100%;padding:10px;border-radius:8px;border:1px solid #e2e8f0;margin-top:6px}
button{padding:10px 12px;border-radius:8px;border:none;cursor:pointer}
.btn-primary{background:#0b75ff;color:#fff}
.btn-ghost{background:transparent;border:1px solid #0b75ff;color:#0b75ff}
.small{font-size:13px;color:#666}
.rate-row{display:flex;gap:8px;align-items:center}
.rate-row > *{flex:1}

.orders-list{max-height:520px;overflow:auto}
.order-item{display:flex;justify-content:space-between;gap:10px;align-items:center;padding:10px;border-radius:8px;border:1px solid #f1f5f9;margin-bottom:10px}
.order-meta{min-width:160px;text-align:right}
.status-pill{padding:6px 8px;border-radius:8px;color:#fff;font-weight:700}
.pending{background:#f59e0b}
.completed{background:#16a34a}
.rejected{background:#ef4444}

.admin-controls{display:flex;gap:8px;flex-wrap:wrap;justify-content:flex-end}
.search{width:100%;padding:8px;border-radius:8px;border:1px solid #e2e8f0;margin-bottom:8px}

.modal{position:fixed;inset:0;background:rgba(0,0,0,0.45);display:flex;align-items:center;justify-content:center;z-index:999;display:none}
.modal .box{background:#fff;padding:18px;border-radius:10px;max-width:520px;width:100%}
.flex{display:flex;gap:8px;align-items:center}
.login-container{display:flex;flex-direction:column;align-items:center;justify-content:center;height:100vh}
.login-card{background:#fff;padding:24px;border-radius:10px;box-shadow:0 6px 18px rgba(2,6,23,0.1);width:100%;max-width:400px}
.login-card h2{margin-top:0;margin-bottom:20px;text-align:center;color:#0b75ff}
.login-card .logo{width:80px;height:80px;border-radius:8px;object-fit:cover;margin:0 auto 16px;display:block}
@media (max-width:900px){ .grid{grid-template-columns:1fr} }
</style>
</head>
<body>

<!-- LOGIN SCREEN (shown by default) -->
<div id="loginScreen" class="login-container">
<div class="login-card">
<img src="https://i.ibb.co/5WWPvnQj/20251128-160723.png" alt="logo" class="logo">
<h2>Admin Login</h2>
<div id="loginError" class="small" style="color:#ef4444;margin-bottom:10px;display:none">Incorrect email or password</div>
<input id="loginEmail" type="text" placeholder="Admin Email" />
<input id="loginPassword" type="password" placeholder="Password" style="margin-top:10px" />
<button class="btn-primary" style="width:100%;margin-top:16px" onclick="signIn()">Sign In</button>
<div class="small" style="margin-top:16px;text-align:center">Use your admin credentials to access the panel</div>
</div>
</div>

<!-- ADMIN PANEL (hidden until login) -->
<div id="adminPanel" style="display:none">
<!-- TOP -->
<div class="topbar">
<div class="brand">
<img src="https://i.ibb.co/5WWPvnQj/20251128-160723.png" alt="logo">
<div>
<h1>Admin Panel — Dollar Exchange</h1>
<div class="small">Fast & Secure Exchange — Rate control & orders</div>
</div>
</div>

<div style="display:flex;gap:12px;align-items:center">
<div id="opStatus" class="status-badge offline">Offline</div>
<div class="small" id="adminEmail">admin@example.com</div>
<button onclick="signOut()" class="btn-ghost">Sign Out</button>
</div>
</div>

<!-- MAIN -->
<div class="container">
<div class="grid">
<!-- LEFT: Orders + Search -->
<div class="card">
<h3>🧾 Orders</h3>
<input id="adminSearch" class="search" placeholder="Search by phone or trx or name" oninput="loadAdmin()" />
<div class="orders-list card" id="adminOrdersContainer" style="padding:12px">
<!-- orders inserted here -->
</div>
</div>

<!-- RIGHT: Rate Controls -->
<div class="card">
<h3>💱 Dollar Rate Control</h3>

<div style="margin-bottom:8px" class="small">সিস্টেমে দুইটি রেট আছে — Payeer এবং Binance। এখানে মান সেট করে Save করুন।</div>

<div style="margin-top:8px">
<label>Payeer Rate (1 USD = ? Tk)</label>
<input id="ratePayeerAdmin" type="number" placeholder="e.g. 95" />

<label style="margin-top:8px">Binance Rate (1 USD = ? Tk)</label>
<input id="rateBinanceAdmin" type="number" placeholder="e.g. 120" />

<div style="margin-top:10px" class="flex">
<button class="btn-primary" onclick="saveRatesAdmin()">Save Rates</button>
<button onclick="resetRates()" class="btn-ghost">Reset to defaults</button>
</div>

<div style="margin-top:14px">
<div class="small">Current stored rates:</div>
<div style="margin-top:6px">
<div>Payeer: <b id="showRatePayeer">—</b> Tk</div>
<div>Binance: <b id="showRateBinance">—</b> Tk</div>
</div>
</div>
</div>

<hr style="margin:14px 0">

<div>
<h4 style="margin:6px 0">Quick actions</h4>
<div class="admin-controls">
<button onclick="approveAllPending()" class="btn-ghost">Approve All Pending (require TX)</button>
<button onclick="exportOrders()" class="btn-ghost">Export Orders (JSON)</button>
<button onclick="clearOrders()" class="btn-ghost">Clear All Orders</button>
</div>
</div>
</div>
</div>
</div>

<!-- Order Modal -->
<div id="orderModal" class="modal">
<div class="box">
<h3 id="modalTitle">Order</h3>
<div id="modalBody" style="margin-top:8px"></div>

<div style="margin-top:12px">
<label>TX ID</label>
<input id="modalTx" type="text" placeholder="Transaction ID" />
</div>

<div style="margin-top:12px" class="flex">
<button class="btn-primary" onclick="saveModalTx()">Save TX</button>
<button onclick="closeOrderModal()" class="btn-ghost">Close</button>
</div>
</div>
</div>
</div>

<script>
// Firebase configuration - Replace with your own config
const firebaseConfig = {
apiKey: "AIzaSyBUMQNzDO6cRdRHFPYOGOBfApzuaidkw2E",
authDomain: "dollar-exchange-bdt.firebaseapp.com",
projectId: "dollar-exchange-bdt",
storageBucket: "dollar-exchange-bdt.firebasestorage.app",
messagingSenderId: "666904772438",
appId: "1:666904772438:web:860c9207c1a1e59a2820f7"
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();
const db = firebase.firestore();

// Global variables
let currentUser = null;
let ordersListener = null;
let ratesListener = null;

// DOM elements
const adminOrdersContainer = document.getElementById('adminOrdersContainer');
const ratePayeerAdmin = document.getElementById('ratePayeerAdmin');
const rateBinanceAdmin = document.getElementById('rateBinanceAdmin');
const showRatePayeer = document.getElementById('showRatePayeer');
const showRateBinanceAdmin = document.getElementById('showRateBinance');
const orderModal = document.getElementById('orderModal');
const modalBody = document.getElementById('modalBody');
const modalTitle = document.getElementById('modalTitle');
const modalTx = document.getElementById('modalTx');
const loginScreen = document.getElementById('loginScreen');
const adminPanel = document.getElementById('adminPanel');
const adminEmail = document.getElementById('adminEmail');
const loginError = document.getElementById('loginError');

let currentEditingOrderId = null;

// Authentication functions
function signIn() {
const email = document.getElementById('loginEmail').value.trim();
const password = document.getElementById('loginPassword').value.trim();

if (!email || !password) {
showLoginError("Please enter email and password");
return;
}

auth.signInWithEmailAndPassword(email, password)
.then((userCredential) => {
// Signed in
currentUser = userCredential.user;
showAdminPanel();
})
.catch((error) => {
showLoginError("Incorrect email or password");
console.error("Login error:", error);
});
}

function signOut() {
auth.signOut()
.then(() => {
currentUser = null;
showLoginScreen();
})
.catch((error) => {
console.error("Sign out error:", error);
});
}

function showLoginError(message) {
loginError.textContent = message;
loginError.style.display = 'block';
setTimeout(() => {
loginError.style.display = 'none';
}, 3000);
}

function showLoginScreen() {
loginScreen.style.display = 'flex';
adminPanel.style.display = 'none';
}

function showAdminPanel() {
loginScreen.style.display = 'none';
adminPanel.style.display = 'block';
adminEmail.textContent = currentUser.email;
loadRates();
loadAdmin();
}

// Listen for auth state changes
auth.onAuthStateChanged((user) => {
if (user) {
currentUser = user;
showAdminPanel();
} else {
showLoginScreen();
}
});

// ONLINE / OFFLINE badge
function updateStatus(){
const hour = new Date().getHours();
const opStatus = document.getElementById('opStatus');
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

// Rates: load / save / show
function loadRates() {
// Set up listener for rates
if (ratesListener) ratesListener();

ratesListener = db.collection('settings').doc('rates').onSnapshot((doc) => {
if (doc.exists) {
const data = doc.data();
ratePayeerAdmin.value = data.payeer || '95';
rateBinanceAdmin.value = data.binance || '120';
showRatePayeer.innerText = data.payeer || '95';
showRateBinanceAdmin.innerText = data.binance || '120';
} else {
// Default rates if document doesn't exist
ratePayeerAdmin.value = '95';
rateBinanceAdmin.value = '120';
showRatePayeer.innerText = '95';
showRateBinanceAdmin.innerText = '120';

// Create default rates document
db.collection('settings').doc('rates').set({
payeer: 95,
binance: 120
});
}
}, (error) => {
console.error("Error loading rates:", error);
});
}

function saveRatesAdmin(){
const r1 = ratePayeerAdmin.value.trim();
const r2 = rateBinanceAdmin.value.trim();
if(!r1 || is(r1) || !r2 || is(r2)){
alert('সঠিক সংখ্যায় দুটি রেট দিন (Payeer ও Binance)');
return;
}

db.collection('settings').doc('rates').update({
payeer: Number(r1),
binance: Number(r2)
})
.then(() => {
alert('✔ Rates updated');
})
.catch((error) => {
console.error("Error updating rates:", error);
alert('Error updating rates');
});
}

// Quick reset
function resetRates(){
if(!confirm('Reset rates to defaults?')) return;

db.collection('settings').doc('rates').update({
payeer: 95,
binance: 120
})
.then(() => {
alert('Rates reset to defaults');
})
.catch((error) => {
console.error("Error resetting rates:", error);
alert('Error resetting rates');
});
}

// Orders: load / render
function loadAdmin(){
const q = document.getElementById('adminSearch').value.trim().toLowerCase();

// Set up listener for orders
if (ordersListener) ordersListener();

let query = db.collection('orders').orderBy('createdAt', 'desc');

ordersListener = query.onSnapshot((snapshot) => {
adminOrdersContainer.innerHTML = '';

if (snapshot.empty) {
adminOrdersContainer.innerHTML = '<div class="small">No orders yet</div>';
return;
}

let all = [];
snapshot.forEach(doc => {
all.push({ id: doc.id, ...doc.data() });
});

const filtered = q ? all.filter(o=>{
return (o.number && o.number.toLowerCase().includes(q)) ||
(o.trx && o.trx.toLowerCase().includes(q)) ||
(o.name && o.name.toLowerCase().includes(q));
}) : all;

if(filtered.length === 0){
adminOrdersContainer.innerHTML = '<div class="small">কোন অর্ডার পাওয়া যায়নি</div>';
return;
}

filtered.forEach(o=>{
const item = document.createElement('div');
item.className = 'order-item';

const left = document.createElement('div');
left.style.flex = '1';
left.innerHTML = `<div><b>${o.name || '—'}</b> <span class="small">• ${new Date(o.createdAt && o.createdAt.toDate ? o.createdAt.toDate() : o.createdAt).toLocaleString()}</span></div>
<div class="small">${o.currency || '—'} • ${o.dollar || '—'} USD → ${o.taka || '—'} TK • ${o.paymentMethod || '—'}</div>
<div class="small">মাধ্যম: ${o.via || '—'}</div>
<div class="small">TX: ${o.trx || '—'}</div>`;

const right = document.createElement('div');
right.className = 'order-meta';
const statusCls = (o.status === 'COMPLETED') ? 'completed' : (o.status === 'REJECTED' ? 'rejected' : 'pending');
const statusText = o.status || 'PENDING';
right.innerHTML = `<div><span class="status-pill ${statusCls}">${statusText}</span></div>`;

// buttons
const btnWrap = document.createElement('div');
btnWrap.style.marginTop = '8px';
btnWrap.innerHTML = `
<div style="display:flex;gap:6px;justify-content:flex-end">
<button class="btn-ghost" onclick="openOrder('${o.id}')">View</button>
<button class="btn-ghost" onclick="adminChangeStatus('${o.id}','PENDING')">Pending</button>
<button class="btn-ghost" onclick="adminChangeStatus('${o.id}','REJECTED')">Reject</button>
<button class="btn-primary" style="background:#16a34a" onclick="adminChangeStatus('${o.id}','COMPLETED')">Approve</button>
</div>`;
right.appendChild(btnWrap);

item.appendChild(left);
item.appendChild(right);
adminOrdersContainer.appendChild(item);
});
}, (error) => {
console.error("Error loading orders:", error);
adminOrdersContainer.innerHTML = '<div class="small">Error loading orders</div>';
});
}

// Change status — require trx for approving
function adminChangeStatus(id, newStatus){
if (!currentUser) {
alert('You must be logged in to perform this action');
return;
}

const orderRef = db.collection('orders').doc(id);

orderRef.get().then((doc) => {
if (!doc.exists) {
alert('Order not found');
return;
}

const orderData = doc.data();

if(newStatus === 'COMPLETED' && (!orderData.trx || orderData.trx.trim() === '')){
return alert('⚠ TXID ছাড়া Approve করা যাবে না!');
}

return orderRef.update({
status: newStatus,
updatedAt: firebase.firestore.FieldValue.serverTimestamp()
});
})
.then(() => {
alert('Status Updated');
})
.catch((error) => {
console.error("Error updating status:", error);
alert('Error updating status');
});
}

// Open order modal
function openOrder(id){
if (!currentUser) {
alert('You must be logged in to perform this action');
return;
}

db.collection('orders').doc(id).get()
.then((doc) => {
if (!doc.exists) {
alert('Order not found');
return;
}

const o = doc.data();
currentEditingOrderId = id;
modalTitle.innerText = `Order — ${o.name || '—'}`;
modalBody.innerHTML = `
<div class="small">Created: ${new Date(o.createdAt && o.createdAt.toDate ? o.createdAt.toDate() : o.createdAt).toLocaleString()}</div>
<div style="margin-top:8px"><b>নাম:</b> ${o.name || '—'}</div>
<div><b>নম্বর:</b> ${o.number || '—'}</div>
<div><b>কারেন্সি:</b> ${o.currency || '—'}</div>
<div><b>ডলার:</b> ${o.dollar || '—'}</div>
<div><b>টাকা:</b> ${o.taka || '—'}</div>
<div style="margin-top:6px"><b>মাধ্যম:</b> ${o.via || '—'}</div>
<div style="margin-top:6px"><b>TX:</b> ${o.trx || '—'}</div>
<div style="margin-top:8px"><b>স্ট্যাটাস:</b> ${o.status || 'PENDING'}</div>
`;
modalTx.value = o.trx || '';
orderModal.style.display = 'flex';
})
.catch((error) => {
console.error("Error fetching order:", error);
alert('Error fetching order details');
});
}

// Save tx from modal
function saveModalTx(){
if (!currentUser) {
alert('You must be logged in to perform this action');
return;
}

if(!currentEditingOrderId) return;

const tx = modalTx.value.trim();

db.collection('orders').doc(currentEditingOrderId).update({
trx: tx,
updatedAt: firebase.firestore.FieldValue.serverTimestamp()
})
.then(() => {
alert('Transaction ID Updated');
closeOrderModal();
})
.catch((error) => {
console.error("Error updating TX ID:", error);
alert('Error updating Transaction ID');
});
}

function closeOrderModal(){
currentEditingOrderId = null;
orderModal.style.display = 'none';
}

// Approve all pending that have trx
function approveAllPending(){
if (!currentUser) {
alert('You must be logged in to perform this action');
return;
}

db.collection('orders').where('status', '==', 'PENDING').get()
.then((querySnapshot) => {
let batch = db.batch();
let changed = 0;

querySnapshot.forEach((doc) => {
const orderData = doc.data();
if(orderData.trx && orderData.trx.trim() !== ''){
batch.update(doc.ref, {
status: 'COMPLETED',
updatedAt: firebase.firestore.FieldValue.serverTimestamp()
});
changed++;
}
});

if(changed > 0) {
return batch.commit();
} else {
alert('No pending orders with TX ID found');
return Promise.resolve();
}
})
.then(() => {
if(changed > 0) {
alert(`Updated ${changed} orders`);
}
})
.catch((error) => {
console.error("Error approving orders:", error);
alert('Error approving orders');
});
}

// Export / clear
function exportOrders(){
if (!currentUser) {
alert('You must be logged in to perform this action');
return;
}

db.collection('orders').get()
.then((querySnapshot) => {
let all = [];
querySnapshot.forEach((doc) => {
let data = doc.data();
// Convert Firestore timestamps to strings for JSON serialization
if (data.createdAt && data.createdAt.toDate) {
data.createdAt = data.createdAt.toDate().toISOString();
}
if (data.updatedAt && data.updatedAt.toDate) {
data.updatedAt = data.updatedAt.toDate().toISOString();
}
all.push({ id: doc.id, ...data });
});

const blob = new Blob([JSON.stringify(all, null, 2)], {type:'application/json'});
const url = URL.createObjectURL(blob);
const a = document.createElement('a');
a.href = url;
a.download = 'orders.json';
a.click();
URL.revokeObjectURL(url);
})
.catch((error) => {
console.error("Error exporting orders:", error);
alert('Error exporting orders');
});
}

function clearOrders(){
if (!currentUser) {
alert('You must be logged in to perform this action');
return;
}

if(!confirm('Are you sure? This will delete all orders.')) return;

db.collection('orders').get()
.then((querySnapshot) => {
let batch = db.batch();
querySnapshot.forEach((doc) => {
batch.delete(doc.ref);
});
return batch.commit();
})
.then(() => {
alert('All orders cleared');
})
.catch((error) => {
console.error("Error clearing orders:", error);
alert('Error clearing orders');
});
}

// Initialize on load
window.addEventListener('load', () => {
// Check if user is already logged in
auth.onAuthStateChanged((user) => {
if (user) {
currentUser = user;
showAdminPanel();
} else {
showLoginScreen();
}
});
});
</script>
</body>
</html>
