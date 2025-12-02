<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Currency Exchange Admin Panel</title>
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
button.success{background:#22c55e;color:#fff;border:none;padding:11px;border-radius:8px;cursor:pointer}

.order-box{background:#fff;margin:10px;border-radius:10px;padding:12px;box-shadow:0 1px 6px rgba(0,0,0,0.06);display:flex;justify-content:space-between;align-items:center;gap:8px}
.order-info{flex:1}
.order-meta{min-width:110px;text-align:right}
.status{padding:6px 10px;border-radius:6px;color:#fff;font-weight:700}
.pending{background:#f59e0b}
.completed{background:#16a34a}
.rejected{background:#ef4444}

#nav{display:flex;justify-content:space-around;background:#111827;padding:12px}
#nav button{width:48%;background:#22b573;color:#fff;border:none;padding:10px;border-radius:8px}

.wa-btn{position:fixed;bottom:22px;left:18px;background:#25d366;width:62px;height:62px;border-radius:50%;display:flex;justify-content:center;align-items:center;box-shadow:0 4px 12px rgba(0,0,0,0.18)}
.wa-btn img{width:34px}

.small{font-size:13px;color:#666}
.modal{position:fixed;inset:0;background:rgba(0,0,0,0.5);display:flex;justify-content:center;align-items:flex-end;padding:20px;z-index:999}
.modal .box{width:100%;max-width:520px;background:#fff;border-radius:12px;padding:16px}

.id-badge{background:#f3f4f6;padding:8px;border-radius:8px;display:inline-block;width:100%}
.order-empty{text-align:center;color:#6b7280;padding:26px}

.currency-item{display:flex;align-items:center;gap:10px;margin-bottom:10px}
.currency-item input{flex:1}
.currency-item button{width:auto;padding:8px 12px}

@media (max-width:520px){
.topbar{padding:8px}
.blue-head{padding:24px 12px}
}
</style>
</head>
<body>

<!-- TOP BAR -->
<div class="topbar">
<div class="logo">
<img src="https://i.ibb.co/5WWPvnQj/20251128-160723.png" alt="logo">
<div>
<div style="font-size:16px;font-weight:800;color:#0037dd">Currency Exchange Admin</div>
<div class="small">Management Panel</div>
</div>
</div>

<div id="opStatus" class="status-badge online">Online</div>

<div class="top-buttons">
<button id="refreshBtn" onclick="refreshData()">Refresh Data</button>
</div>
</div>

<div class="blue-head">
<h1>Currency Exchange Admin Panel</h1>
<p class="small">Manage currencies, rates, and orders</p>
</div>

<div id="nav">
<button onclick="showCurrencies()">Manage Currencies</button>
<button onclick="showOrders()">Manage Orders</button>
</div>

<!-- CURRENCIES AREA -->
<div id="currenciesArea">
<h2 style="text-align:center;margin-top:18px">💱 Currency Management</h2>

<div class="card">
<h3>Add New Currency</h3>
<div class="control-row">
<input id="newCurrencyName" placeholder="Currency Name (e.g., PayPal)" />
<input id="newCurrencyId" placeholder="Currency ID (e.g., U123456789)" />
<input id="newCurrencyRate" type="number" placeholder="Exchange Rate (1 USD = ? Tk)" />
</div>
<button class="primary" onclick="addCurrency()">Add Currency</button>
</div>

<div class="card">
<h3>Current Currencies</h3>
<div id="currenciesList"></div>
<button class="primary" onclick="saveAllCurrencies()">Save All Changes</button>
</div>
</div>

<!-- ORDERS AREA -->
<div id="ordersArea" style="display:none;">
<h2 style="text-align:center;margin-top:18px">📦 Order Management</h2>

<div class="card">
<div style="margin-bottom:10px"><b>Search by user number:</b>
<input id="adminSearch" placeholder="phone number" oninput="loadOrders()" />
</div>
<div id="adminOrders"></div>
</div>
</div>

<!-- ORDER DETAILS MODAL -->
<div id="modal" style="display:none;">
<div class="modal" onclick="closeModal(event)">
<div class="box" onclick="event.stopPropagation()">
<h3 id="mTitle">Order Details</h3>
<div id="mBody"></div>

<div style="margin-top:10px;display:flex;gap:8px">
<input id="mTx" placeholder="ট্রানজেকশন আইডি দিন">
<button class="primary" onclick="saveTx()">সংরক্ষণ</button>
</div>

<div style="margin-top:12px;text-align:right"><button onclick="closeModal()">Close</button></div>
</div>
</div>
</div>

<!-- WhatsApp Button -->
<a href="https://wa.me/qr/DTBEJ472LPKOA1" class="wa-btn" target="_blank">
<img src="https://i.ibb.co/dnLD0Wf/20251129-064417.jpg" alt="wa">
</a>

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
const modal = document.getElementById('modal');
const currenciesArea = document.getElementById('currenciesArea');
const ordersArea = document.getElementById('ordersArea');

// DEFAULT CURRENCIES
let currencies = [
{ name: 'Payeer', id: 'P1131698605', rate: 70 },
{ name: 'Binance', id: '1188473082', rate: 20 },
{ name: 'Advcash', id: 'U 1048 5654 4714', rate: 60 }
];

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

// VIEW SWITCH
function showCurrencies(){ 
currenciesArea.style.display='block'; 
ordersArea.style.display='none';
loadCurrencies();
}

function showOrders(){ 
currenciesArea.style.display='none'; 
ordersArea.style.display='block';
loadOrders();
}

// CURRENCY MANAGEMENT
async function loadCurrencies(){
try {
const currenciesDoc = await db.collection('settings').doc('currencies').get();
if (currenciesDoc.exists) {
currencies = currenciesDoc.data().list || currencies;
}
renderCurrencies();
} catch (error) {
console.error("Error loading currencies:", error);
renderCurrencies();
}
}

function renderCurrencies(){
const currenciesList = document.getElementById('currenciesList');
currenciesList.innerHTML = '';

if(currencies.length === 0){
currenciesList.innerHTML = '<div class="order-empty">No currencies found</div>';
return;
}

currencies.forEach((currency, index) => {
const currencyItem = document.createElement('div');
currencyItem.className = 'currency-item';
currencyItem.innerHTML = `
<input id="currencyName${index}" value="${currency.name}" placeholder="Currency Name" />
<input id="currencyId${index}" value="${currency.id}" placeholder="Currency ID" />
<input id="currencyRate${index}" type="number" value="${currency.rate}" placeholder="Exchange Rate" />
<button class="danger" onclick="removeCurrency(${index})">Remove</button>
`;
currenciesList.appendChild(currencyItem);
});
}

function addCurrency(){
const name = document.getElementById('newCurrencyName').value.trim();
const id = document.getElementById('newCurrencyId').value.trim();
const rate = parseFloat(document.getElementById('newCurrencyRate').value);

if(!name || !id || isNaN(rate)){
alert('Please fill all fields correctly');
return;
}

currencies.push({ name, id, rate });
renderCurrencies();

// Clear input fields
document.getElementById('newCurrencyName').value = '';
document.getElementById('newCurrencyId').value = '';
document.getElementById('newCurrencyRate').value = '';
}

function removeCurrency(index){
currencies.splice(index, 1);
renderCurrencies();
}

async function saveAllCurrencies(){
try {
// Update currency values from input fields
currencies = currencies.map((currency, index) => ({
name: document.getElementById(`currencyName${index}`).value,
id: document.getElementById(`currencyId${index}`).value,
rate: parseFloat(document.getElementById(`currencyRate${index}`).value)
}));

await db.collection('settings').doc('currencies').set({
list: currencies
});

alert("Currencies updated successfully!");
} catch (error) {
console.error("Error saving currencies:", error);
alert("Error updating currencies. Please try again.");
}
}

// ORDER MANAGEMENT
async function loadOrders(){
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

renderOrders(orders);
} catch (error) {
console.error("Error loading orders:", error);
adminOrders.innerHTML='<div class="order-empty">Error loading orders</div>';
}
}

function renderOrders(orders) {
adminOrders.innerHTML = "";

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

<button onclick="changeStatus('${o.id}','COMPLETED')">Approve</button>
<button onclick="changeStatus('${o.id}','REJECTED')">Reject</button>
<button onclick="changeStatus('${o.id}','PENDING')">Pending</button>
</div>
`;

adminOrders.appendChild(div);
});
}

// Change order status
async function changeStatus(id,newStatus){
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

loadOrders();
alert("Status Updated");
} catch (error) {
console.error("Error updating status:", error);
alert("Error updating status. Please try again.");
}
}

// MODAL for specific order
let currentModalId=null;
async function openModal(id){
currentModalId=id;

try {
const orderDoc = await db.collection('orders').doc(id).get();
if (!orderDoc.exists) return;

const o = {id: orderDoc.id, ...orderDoc.data()};

mTitle.innerText="Order — "+o.name;

mBody.innerHTML=`
<div class="small">Created: ${new Date(o.createdAt).toLocaleString()}</div>
<div>নাম: <b>${o.name}</b></div>
<div>নম্বর: <b>${o.number}</b></div>
<div>কারেন্সি: <b>${o.currency}</b></div>
<div>ডলার: <b>${o.dollar}</b> → টাকা: <b>${o.taka}</b></div>

<div style="margin-top:8px">বর্তমান স্ট্যাটাস: <b>${o.status}</b></div>
`;

mTx.value = o.trx || "";
modal.style.display="block";
} catch (error) {
console.error("Error opening order:", error);
alert("Error loading order details");
}
}

function closeModal(event){
// Check if the click was on the modal background or the close button
if (!event || event.target === modal || event.target.textContent === 'Close') {
modal.style.display='none'; 
currentModalId=null;
}
}

async function saveTx(){
if(!currentModalId) return;

const tx = mTx.value.trim();

try {
await db.collection('orders').doc(currentModalId).update({
trx: tx
});

loadOrders();
alert("Transaction ID Updated");

closeModal();
} catch (error) {
console.error("Error updating transaction ID:", error);
alert("Error updating transaction ID. Please try again.");
}
}

// Refresh data
function refreshData(){
loadCurrencies();
loadOrders();
alert("Data refreshed successfully!");
}

// ON LOAD
window.addEventListener('load',()=>{
loadCurrencies();
loadOrders();
});
</script>

</body>
</html>
