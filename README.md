
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

<!-- Admin Login (hidden) -->
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

<!-- Admin Navigation -->
<div class="card">
<div style="display:flex;gap:10px;justify-content:center">
<button class="primary" onclick="showAdminDashboard()">Dashboard</button>
<button class="primary" onclick="showCurrencyManagement()">Currency Management</button>
<button class="primary" onclick="showOrderManagement()">Order Management</button>
<button class="primary" onclick="showUserManagement()">User Management</button>
</div>
</div>

<!-- Admin Dashboard -->
<div id="adminDashboard" class="card">
<h3>📊 Admin Dashboard</h3>
<div id="dashboardStats">
<!-- Stats will be loaded here -->
</div>
</div>

<!-- Currency Management -->
<div id="currencyManagement" class="card" style="display:none;">
<h3>💱 Currency Management</h3>
<div id="currencyList">
<!-- Currencies will be loaded here -->
</div>

<div style="margin-top:20px">
<h4>Add New Currency</h4>
<input id="newCurrencyName" placeholder="Currency Name" />
<input id="newCurrencyRate" type="number" placeholder="Rate (1 USD = ? Tk)" />
<input id="newCurrencyId" placeholder="Payment ID" />
<button class="primary" onclick="addCurrency()">Add Currency</button>
</div>
</div>

<!-- Order Management -->
<div id="orderManagement" class="card" style="display:none;">
<h3>📦 Order Management</h3>
<div style="margin-bottom:10px"><b>Search by user number:</b>
<input id="adminSearch" placeholder="phone number" oninput="loadAdminOrders()" />
</div>
<div id="adminOrders"></div>
</div>

<!-- User Management -->
<div id="userManagement" class="card" style="display:none;">
<h3>👥 User Management</h3>
<div style="margin-bottom:10px"><b>Search by email or number:</b>
<input id="userSearch" placeholder="email or phone number" oninput="searchUsers()" />
</div>
<div id="userList"></div>
</div>

<!-- Site Settings -->
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

// Initialize Firebase
firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

// Get DOM elements
const adminLogin = document.getElementById('adminLogin');

// DEFAULT CURRENCIES
let currencies = [
{ id: 'Payeer', name: 'Payeer', rate: 70, paymentId: 'P1131698605' },
{ id: 'Binance', name: 'Binance', rate: 20, paymentId: '1188473082' },
{ id: 'Advcash', name: 'Advcash', rate: 60, paymentId: 'U 1048 5654 4714' }
];

// ADMIN CREDENTIALS
const ADMIN_EMAIL = 'sheksimon5@gmail.com';
const ADMIN_PASSWORD = 'saimon500';

// ONLINE/OFFLINE
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

// CURRENCY MANAGEMENT
async function loadCurrencies(){
try {
const currenciesDoc = await db.collection('settings').doc('currencies').get();
if (currenciesDoc.exists) {
currencies = currenciesDoc.data().list || currencies;
}
} catch (error) {
console.error("Error loading currencies:", error);
}

updateCurrencyList();
}

async function saveCurrencies(){
try {
await db.collection('settings').doc('currencies').set({
list: currencies
});
alert("Currencies updated successfully");
} catch (error) {
console.error("Error saving currencies:", error);
alert("Error updating currencies. Please try again.");
}
}

function updateCurrencyList(){
const currencyList = document.getElementById('currencyList');
currencyList.innerHTML = '';

currencies.forEach((currency, index) => {
const item = document.createElement('div');
item.className = 'currency-item';
item.innerHTML = `
<div class="currency-info">
<div><b>${currency.name}</b></div>
<div class="small">Rate: 1 USD = ${currency.rate} Tk</div>
<div class="small">Payment ID: ${currency.paymentId}</div>
</div>
<div class="currency-actions">
<button class="primary" onclick="editCurrency(${index})">Edit</button>
<button class="danger" onclick="deleteCurrency(${index})">Delete</button>
</div>
`;
currencyList.appendChild(item);
});
}

function addCurrency(){
const name = document.getElementById('newCurrencyName').value.trim();
const rate = parseFloat(document.getElementById('newCurrencyRate').value);
const paymentId = document.getElementById('newCurrencyId').value.trim();

if (!name || isNaN(rate) || !paymentId) {
alert('Please fill all fields');
return;
}

// Generate a unique ID for the currency
const id = name.toLowerCase().replace(/\s+/g, '_');

// Check if currency already exists
if (currencies.find(c => c.id === id)) {
alert('Currency with this name already exists');
return;
}

currencies.push({ id, name, rate, paymentId });
saveCurrencies();
loadCurrencies();

// Clear form
document.getElementById('newCurrencyName').value = '';
document.getElementById('newCurrencyRate').value = '';
document.getElementById('newCurrencyId').value = '';
}

function editCurrency(index){
const currency = currencies[index];
const newName = prompt('Currency name:', currency.name);
if (newName === null) return;

const newRate = prompt('Rate (1 USD = ? Tk):', currency.rate);
if (newRate === null) return;

const newPaymentId = prompt('Payment ID:', currency.paymentId);
if (newPaymentId === null) return;

currencies[index] = {
...currency,
name: newName,
rate: parseFloat(newRate),
paymentId: newPaymentId
};

saveCurrencies();
loadCurrencies();
}

function deleteCurrency(index){
if (confirm('Are you sure you want to delete this currency?')) {
currencies.splice(index, 1);
saveCurrencies();
loadCurrencies();
}
}

// ADMIN: Load orders for admin view
async function loadAdminOrders(){
try {
const q = adminSearch.value.trim();
let ordersQuery = db.collection('orders');

if(q) {
ordersQuery = ordersQuery.where('number', '==', q);
}

const snapshot = await ordersQuery.get();
const orders = snapshot.docs.map(doc => ({id: doc.id, ...doc.data()}));

// Sort by creation date (newest first)
orders.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));

adminOrders.innerHTML="";

if(orders.length===0){
adminOrders.innerHTML='<div class="order-empty">কোন অর্ডার পাওয়া যায়নি</div>';
return;
}

orders.forEach(o=>{
const div=document.createElement('div');
div.className='order-box';

div.innerHTML=`
<div style="flex:1">
<b>${o.name}</b> <span class="small">• ${new Date(o.createdAt).toLocaleString()}</span>
<div class="small">নম্বর: ${o.number} • ${o.currency} • ${o.dollar} USD → ${o.taka} TK</div>
<div class="small">মাধ্যম: ${o.via || 'Not given'}</div>
<div>TXID: <span class="small">${o.trx||'—'}</span></div>
</div>

<div style="min-width:150px;text-align:right">
<div><span class="status ${o.status==='Pending'?'pending':o.status==='COMPLETED'?'completed':'rejected'}">${o.status}</span></div>

<button onclick="adminChangeStatus('${o.id}','COMPLETED')">Approve</button>
<button onclick="adminChangeStatus('${o.id}','REJECTED')">Reject</button>
<button onclick="adminChangeStatus('${o.id}','PENDING')">Pending</button>
</div>
`;

adminOrders.appendChild(div);
});
} catch (error) {
console.error("Error loading admin orders:", error);
adminOrders.innerHTML='<div class="order-empty">Error loading orders</div>';
}
}

// ADMIN: change status (require trx when approving)
async function adminChangeStatus(id,newStatus){
try {
if(newStatus==='COMPLETED') {
const orderDoc = await db.collection('orders').doc(id).get();
if (!orderDoc.exists) {
alert("Order not found");
return;
}

const orderData = orderDoc.data();
if(!orderData.trx || orderData.trx.trim()==''){
alert("⚠ TXID ছাড়া Approve করা যাবে না!");
return;
}
}

await db.collection('orders').doc(id).update({
status: newStatus
});

loadAdminOrders();
alert("Status Updated");
} catch (error) {
console.error("Error updating status:", error);
alert("Error updating status. Please try again.");
}
}

// ADMIN: User Management
async function searchUsers(){
try {
const query = userSearch.value.trim();
userList.innerHTML = "";

if (!query) {
userList.innerHTML = '<div class="order-empty">Enter email or phone number to search</div>';
return;
}

// Search by email
const emailQuery = db.collection('users').where('email', '==', query.toLowerCase());
const emailSnapshot = await emailQuery.get();

// Search by number
const numberQuery = db.collection('users').where('number', '==', query);
const numberSnapshot = await numberQuery.get();

// Combine results
const emailUsers = emailSnapshot.docs.map(doc => ({id: doc.id, ...doc.data()}));
const numberUsers = numberSnapshot.docs.map(doc => ({id: doc.id, ...doc.data()}));

// Merge arrays, removing duplicates by ID
const allUsers = [...emailUsers];
numberUsers.forEach(user => {
if (!allUsers.find(u => u.id === user.id)) {
allUsers.push(user);
}
});

if (allUsers.length === 0) {
userList.innerHTML = '<div class="order-empty">No users found</div>';
return;
}

allUsers.forEach(user => {
const div = document.createElement('div');
div.className = 'order-box';
div.innerHTML = `
<div style="flex:1">
<b>${user.name}</b> (${user.userType})
<div class="small">Email: ${user.email}</div>
<div class="small">Phone: ${user.number}</div>
<div class="small">Joined: ${new Date(user.createdAt).toLocaleString()}</div>
</div>
<div style="min-width:100px;text-align:right">
<button onclick="viewUserOrders('${user.email}', '${user.number}')">View Orders</button>
</div>
`;
userList.appendChild(div);
});
} catch (error) {
console.error("Error searching users:", error);
userList.innerHTML = '<div class="order-empty">Error searching users</div>';
}
}

async function viewUserOrders(email, number) {
// Switch to order management and filter by this user
showOrderManagement();
adminSearch.value = number;
loadAdminOrders();
}

// ADMIN: Dashboard
async function loadDashboard() {
try {
// Get stats
const totalOrdersSnapshot = await db.collection('orders').get();
const totalOrders = totalOrdersSnapshot.size;

const pendingOrdersSnapshot = await db.collection('orders').where('status', '==', 'PENDING').get();
const pendingOrders = pendingOrdersSnapshot.size;

const completedOrdersSnapshot = await db.collection('orders').where('status', '==', 'COMPLETED').get();
const completedOrders = completedOrdersSnapshot.size;

const rejectedOrdersSnapshot = await db.collection('orders').where('status', '==', 'REJECTED').get();
const rejectedOrders = rejectedOrdersSnapshot.size;

const usersSnapshot = await db.collection('users').get();
const totalUsers = usersSnapshot.size;

// Calculate total transaction value
let totalValue = 0;
totalOrdersSnapshot.forEach(doc => {
const order = doc.data();
if (order.status === 'COMPLETED') {
totalValue += parseFloat(order.taka) || 0;
}
});

dashboardStats.innerHTML = `
<div class="control-row">
<div class="card" style="text-align:center">
<h3>${totalOrders}</h3>
<p>Total Orders</p>
</div>
<div class="card" style="text-align:center">
<h3>${pendingOrders}</h3>
<p>Pending Orders</p>
</div>
</div>
<div class="control-row">
<div class="card" style="text-align:center">
<h3>${completedOrders}</h3>
<p>Completed Orders</p>
</div>
<div class="card" style="text-align:center">
<h3>${rejectedOrders}</h3>
<p>Rejected Orders</p>
</div>
</div>
<div class="control-row">
<div class="card" style="text-align:center">
<h3>${totalUsers}</h3>
<p>Total Users</p>
</div>
<div class="card" style="text-align:center">
<h3>${totalValue.toFixed(2)} Tk</h3>
<p>Total Transaction Value</p>
</div>
</div>
`;
} catch (error) {
console.error("Error loading dashboard:", error);
dashboardStats.innerHTML = '<div class="order-empty">Error loading dashboard</div>';
}
}

// ADMIN: Site Settings
async function loadSiteSettings() {
try {
const settingsDoc = await db.collection('settings').doc('site').get();
if (settingsDoc.exists) {
const settings = settingsDoc.data();
siteStatus.value = settings.status || '';
}
} catch (error) {
console.error("Error loading site settings:", error);
}
}

async function saveSiteSettings() {
try {
await db.collection('settings').doc('site').set({
status: siteStatus.value
});
alert("Site settings updated successfully");
} catch (error) {
console.error("Error saving site settings:", error);
alert("Error updating site settings. Please try again.");
}
}

// Admin Navigation
function showAdminDashboard(){
adminDashboard.style.display='block';
currencyManagement.style.display='none';
orderManagement.style.display='none';
userManagement.style.display='none';
loadDashboard();
}

function showCurrencyManagement(){
adminDashboard.style.display='none';
currencyManagement.style.display='block';
orderManagement.style.display='none';
userManagement.style.display='none';
updateCurrencyList();
}

function showOrderManagement(){
adminDashboard.style.display='none';
currencyManagement.style.display='none';
orderManagement.style.display='block';
userManagement.style.display='none';
loadAdminOrders();
}

function showUserManagement(){
adminDashboard.style.display='none';
currencyManagement.style.display='none';
orderManagement.style.display='none';
userManagement.style.display='block';
}

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

function showAdmin(){ 
adminArea.style.display='block'; 
showAdminDashboard();
loadSiteSettings();
loadCurrencies();
}

function logoutAdmin() {
adminLogin.style.display="flex";
adminArea.style.display='none';
}

// ON LOAD
window.addEventListener('load',()=>{
// Check if already logged in
const isLoggedIn = localStorage.getItem('adminLoggedIn') === 'true';
if (isLoggedIn) {
adminLogin.style.display="none";
showAdmin();
}
});
</script>

</body>
</html>
