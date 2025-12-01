
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

input,select,button,textarea{width:100%;padding:10px;margin:8px 0;border-radius:8px;border:1px solid #ddd;font-size:15px;box-sizing:border-box;}
button.primary{background:#0b75ff;color:#fff;border:none;padding:11px;border-radius:8px;cursor:pointer}

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

.account-info{display:flex;gap:10px;align-items:center}
.btn-ghost{background:transparent;border:1px solid #0b75ff;color:#0b75ff;padding:8px 12px;border-radius:8px;cursor:pointer}

.control-row{display:flex;gap:10px}
.control-row > *{flex:1}

/* --- New Styles for Login Page --- */
.login-container {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    padding: 20px;
    box-sizing: border-box;
}
.login-form {
    background: #fff;
    padding: 40px;
    border-radius: 12px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.1);
    width: 100%;
    max-width: 400px;
    text-align: center;
}
.login-form .logo {
    justify-content: center;
    margin-bottom: 25px;
}
.login-form h2 {
    margin-top: 0;
    margin-bottom: 25px;
    color: #0037dd;
}
.error-message {
    color: #ef4444;
    font-size: 14px;
    margin-top: 15px;
    display: none; /* Hidden by default */
}

@media (max-width:520px){
.topbar{padding:8px}
.blue-head{padding:24px 12px}
}
</style>
</head>
<body>

<!-- LOGIN VIEW (Initially Hidden) -->
<div id="loginView" class="login-container" style="display: none;">
    <div class="login-form">
        <div class="logo">
            <img src="https://i.ibb.co/5WWPvnQj/20251128-160723.png" alt="logo">
            <div>
                <div style="font-size:16px;font-weight:800;color:#0037dd">Dollar Exchange</div>
                <div class="small">Admin Login</div>
            </div>
        </div>
        <h2>Admin Login</h2>
        <form id="loginForm">
            <input type="email" id="email" placeholder="Email Address" required>
            <input type="password" id="password" placeholder="Password" required>
            <button type="submit" class="primary">Login</button>
        </form>
        <div id="loginError" class="error-message">Invalid email or password. Please try again.</div>
    </div>
</div>

<!-- ADMIN DASHBOARD VIEW (Initially Hidden) -->
<div id="dashboardView" style="display: none;">

<!-- TOP BAR -->
<div class="topbar">
<div class="logo">
<img src="https://i.ibb.co/5WWPvnQj/20251128-160723.png" alt="logo">
<div>
<div style="font-size:16px;font-weight:800;color:#0037dd">Dollar Exchange Admin</div>
<div class="small">Management Panel</div>
</div>
</div>

<div id="opStatus" class="status-badge online">Admin</div>

<div class="top-buttons">
<button onclick="logoutAdmin()">Logout</button>
</div>
</div>

<div class="blue-head">
<h1>Admin Dashboard</h1>
<p class="small">Manage orders, rates and user transactions</p>
</div>

<!-- ADMIN NAVIGATION -->
<div id="nav">
<button onclick="showOrders()">Manage Orders</button>
<button onclick="showRates()">Update Rates</button>
</div>

<!-- ADMIN AREA -->
<div id="adminArea">
<h2 style="text-align:center;margin-top:18px">🛠 Admin Panel</h2>

<!-- ORDERS SECTION -->
<div id="ordersSection">
<div class="card">
<div style="margin-bottom:10px"><b>Search by user number:</b>
<input id="adminSearch" placeholder="phone number" oninput="loadAdmin()" />
</div>
<div id="adminOrders"></div>
</div>
</div>

<!-- RATES SECTION (Initially Hidden) -->
<div id="ratesSection" style="display:none;">
<div class="card">
<h3>💱 Dollar Rate Control</h3>

<label>Payeer Rate (1 USD = ? Tk)</label>
<input id="ratePayeer" type="number"/>

<label>Binance Rate (1 USD = ? Tk)</label>
<input id="rateBinance" type="number"/>

<button class="primary" onclick="saveRates()">💾 Save Rates</button>
</div>
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

<div style="margin-top:12px;display:flex;gap:8px">
<button class="primary" onclick="adminChangeStatus(currentModalId,'COMPLETED')">Approve</button>
<button class="btn-ghost" onclick="adminChangeStatus(currentModalId,'REJECTED')">Reject</button>
</div>

<div style="margin-top:12px;text-align:right"><button onclick="closeModal()">Close</button></div>
</div>
</div>
</div>

<!-- WhatsApp Button -->
<a href="https://wa.me/01629066867" class="wa-btn" target="_blank">
<img src="https://i.ibb.co/dnLD0Wf/20251129-064417.jpg" alt="wa">
</a>

</div> <!-- End of dashboardView -->

<!-- Firebase SDK -->
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>
<!-- Add Firebase Auth SDK -->
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth-compat.js"></script>
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
const auth = firebase.auth();

// Payment IDs
const PAYEER_ID = 'P1131698605';
const BINANCE_ID = '1188473082';

// DEFAULT RATES
let rates = {
Payeer: 70,
Binance: 20
};

// --- AUTHENTICATION & VIEW LOGIC ---

function showLoginView() {
    document.getElementById('loginView').style.display = 'flex';
    document.getElementById('dashboardView').style.display = 'none';
}

function showDashboardView() {
    document.getElementById('loginView').style.display = 'none';
    document.getElementById('dashboardView').style.display = 'block';
}

// Handle login form submission
document.getElementById('loginForm').addEventListener('submit', async (e) => {
    e.preventDefault();
    const email = document.getElementById('email').value;
    const password = document.getElementById('password').value;
    const errorDiv = document.getElementById('loginError');
    errorDiv.style.display = 'none'; // Hide previous errors

    try {
        await auth.signInWithEmailAndPassword(email, password);
        // The onAuthStateChanged listener will handle showing the dashboard
    } catch (error) {
        console.error("Login failed:", error);
        errorDiv.style.display = 'block'; // Show error message
    }
});

// MODIFIED LOGOUT FUNCTION
function logoutAdmin(){
    if(confirm("Are you sure you want to logout?")){
        auth.signOut().then(() => {
            // The onAuthStateChanged listener will handle showing the login screen
        }).catch(error => {
            console.error('Logout error:', error);
            alert("Error during logout");
        });
    }
}

// MODIFIED ON LOAD LOGIC
window.addEventListener('load', () => {
    // Check authentication state on load
    auth.onAuthStateChanged(user => {
        if (user) {
            // User is signed in, show the dashboard
            showDashboardView();
            // Load the data for the dashboard
            loadAdmin();
            loadRates();
        } else {
            // No user is signed in, show the login form
            showLoginView();
        }
    });
});


// --- YOUR ORIGINAL FUNCTIONS ---

// RATES
async function loadRates(){
try {
const ratesDoc = await db.collection('settings').doc('rates').get();
if (ratesDoc.exists) {
rates = ratesDoc.data();
ratePayeer.value = rates.Payeer;
rateBinance.value = rates.Binance;
} else {
ratePayeer.value = rates.Payeer;
rateBinance.value = rates.Binance;
}
} catch (error) {
console.error("Error loading rates:", error);
ratePayeer.value = rates.Payeer;
rateBinance.value = rates.Binance;
}
}

async function saveRates(){ 
try {
await db.collection('settings').doc('rates').set({
Payeer: Number(ratePayeer.value),
Binance: Number(rateBinance.value)
});

rates.Payeer = Number(ratePayeer.value);
rates.Binance = Number(rateBinance.value);

alert("✔ Dollar Rates Updated");
} catch (error) {
console.error("Error saving rates:", error);
alert("Error updating rates. Please try again.");
}
}

// ADMIN: Load orders for admin view
async function loadAdmin(){
try {
const q = adminSearch.value.trim();
let ordersQuery = db.collection('orders');

if(q) {
ordersQuery = ordersQuery.where('number', '==', q);
}

const snapshot = await ordersQuery.get();
const orders = snapshot.docs.map(doc => ({id: doc.id, ...doc.data()}));

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

<button onclick="openModal('${o.id}')">View/Edit</button>
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

loadAdmin();
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

function closeModal(){ modal.style.display='none'; currentModalId=null; }

async function saveTx(){
if(!currentModalId) return;
const tx = mTx.value.trim();
try {
await db.collection('orders').doc(currentModalId).update({trx: tx});
loadAdmin();
alert("Transaction ID Updated");
closeModal();
} catch (error) {
console.error("Error updating transaction ID:", error);
alert("Error updating transaction ID. Please try again.");
}
}

// COPY
function copyText(text){ 
navigator.clipboard.writeText(text).then(() => {
alert("Copied: " + text);
}).catch(err => {
console.error('Could not copy text: ', err);
alert("Failed to copy text");
});
}

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
function showOrders(){ 
ordersSection.style.display='block'; 
ratesSection.style.display='none'; 
loadAdmin(); 
}

function showRates(){ 
ordersSection.style.display='none'; 
ratesSection.style.display='block'; 
loadRates(); 
}
</script>

</body>
</html>
