<html lang="bn">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title id="pageTitle">Dollar Exchange - User Panel</title>
<link id="favicon" rel="icon" href="">
<style>
body{font-family: sans-serif;background:#f2f5f8;margin:0;color:#111;min-height:100vh}

/* Loading Animation Styles */
#loadingScreen {
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  z-index: 99999;
  transition: opacity 0.5s ease;
}
.loader {
  width: 50px;
  height: 50px;
  border: 5px solid #f3f3f3;
  border-top: 5px solid #0b75ff;
  border-radius: 50%;
  animation: spin 1s linear infinite;
}
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
.loading-text {
  margin-top: 15px;
  color: #0b75ff;
  font-weight: bold;
}

/* Typing Animation Styles */
.typing-text {
  display: inline-block;
  border-right: 2px solid #fff; /* White cursor for blue background */
  padding-right: 5px;
  animation: blink-caret 0.75s step-end infinite;
}
@keyframes blink-caret {
  from, to { border-color: transparent }
  50% { border-color: #fff; }
}

.topbar{background:#fff;padding:10px 12px;display:flex;align-items:center;justify-content:space-between;box-shadow:0 2px 6px rgba(0,0,0,0.06);position:sticky;top:0;z-index:100}
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

input,select,button,textarea{width:100%;padding:10px;margin:8px 0;border-radius:8px;border:1px solid #ddd;font-size:15px;box-sizing:border-box}
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

#nav{display:flex;justify-content:space-around;background:#111827;padding:12px;position:sticky;top:60px;z-index:99}
#nav button{width:48%;background:#22b573;color:#fff;border:none;padding:10px;border-radius:8px}

.wa-btn{position:fixed;bottom:22px;left:18px;background:#25d366;width:62px;height:62px;border-radius:50%;display:flex;justify-content:center;align-items:center;box-shadow:0 4px 12px rgba(0,0,0,0.18);z-index:90}
.wa-btn img{width:34px}

.small{font-size:13px;color:#666}
.modal{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.5);display:flex;justify-content:center;align-items:center;padding:20px;z-index:1000;overflow:auto}
.modal .box{width:100%;max-width:520px;background:#fff;border-radius:12px;padding:16px;margin:auto;max-height:90vh;overflow-y:auto}

.id-badge{background:#f3f4f6;padding:8px;border-radius:8px;display:inline-block;width:100%;margin-bottom:6px;}
.order-empty{text-align:center;color:#6b7280;padding:26px}

.account-info{display:flex;gap:10px;align-items:center}
.btn-ghost{background:transparent;border:1px solid #0b75ff;color:#0b75ff;padding:8px 12px;border-radius:8px;cursor:pointer}

/* Maintenance mode styles */
.maintenance-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(255, 255, 255, 0.95);
  z-index: 9999;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  text-align: center;
  padding: 20px;
}

.maintenance-overlay h2 { margin-bottom: 20px; color: #333; }
.maintenance-overlay p { margin-bottom: 30px; color: #666; max-width: 500px; }

/* Global notification styles */
.global-notification {
  position: fixed;
  top: 70px;
  left: 0;
  right: 0;
  padding: 10px;
  text-align: center;
  color: white;
  font-weight: bold;
  z-index: 1000;
  display: none;
}
.notification-info { background-color: #31708f; }
.notification-warning { background-color: #8a6d3b; }
.notification-success { background-color: #5cb85c; }
.notification-error { background-color: #d9534f; }

.currency-option { display: flex; align-items: center; gap: 8px; }
.currency-img { width: 24px; height: 24px; object-fit: contain; }
body.modal-open { overflow: hidden; }

.trade-type-toggle {
  display: flex;
  background: #f1f5f9;
  border-radius: 8px;
  overflow: hidden;
  margin: 15px 0;
}
.trade-type-toggle button {
  flex: 1;
  padding: 12px;
  border: none;
  background: transparent;
  cursor: pointer;
  font-weight: 600;
  transition: all 0.3s ease;
}
.trade-type-toggle button.active {
  background: #0b75ff;
  color: white;
}

/* Copy Button Style */
.copy-btn-inside {
  background-color: #e5e7eb;
  border: none;
  padding: 4px 10px;
  border-radius: 4px;
  cursor: pointer;
  font-size: 13px;
  transition: all 0.2s;
  float: right;
}
.copy-btn-inside.copied {
  background-color: #16a34a;
  color: white;
}

@media (max-width:520px){
.topbar{padding:8px}
.blue-head{padding:24px 12px}
.modal .box{margin:20px;width:calc(100% - 40px)}
}
</style>
</head>
<body>

<!-- Loading Screen -->
<div id="loadingScreen">
  <div class="loader"></div>
  <div class="loading-text">লোড হচ্ছে...</div>
</div>

<!-- Global Notification -->
<div id="globalNotification" class="global-notification"></div>

<!-- Maintenance Mode Overlay -->
<div id="maintenanceOverlay" class="maintenance-overlay" style="display:none;">
  <h2>Site Under Maintenance</h2>
  <p id="maintenanceMessage">We are currently performing maintenance. Please check back later.</p>
</div>

<!-- Account Modal -->
<div id="accountModal" class="modal" style="display:none;">
<div class="box" onclick="event.stopPropagation()">
<h3 id="accTitle">My Account</h3>
<div id="accForms">
<div id="loginForm">
<input id="loginEmail" placeholder="Email" type="email" />
<input id="loginPassword" placeholder="Password" type="password" />
<button class="primary" onclick="loginUser()">Login</button>
<div style="text-align:center;margin-top:8px">
<button class="btn-ghost" onclick="showSignup()">Sign Up</button>
<button class="btn-ghost" onclick="showFisher()">Fisher</button>
<button class="btn-ghost" onclick="showForgotPassword()">Forget Password</button>
</div>
</div>

<div id="signupForm" style="display:none">
<input id="signupName" placeholder="আপনার নাম" />
<input id="signupEmail" placeholder="Email" type="email" />
<input id="signupNumber" placeholder="মোবাইল নম্বর" />
<input id="signupPassword" placeholder="Password" type="password" />
<button class="primary" onclick="signupUser()">Create Account</button>
<div style="text-align:center;margin-top:8px">
<button class="btn-ghost" onclick="showLogin()">Already have account</button>
</div>
</div>

<div id="fisherForm" style="display:none">
<input id="fisherName" placeholder="আপনার নাম" />
<input id="fisherEmail" placeholder="Email" type="email" />
<input id="fisherNumber" placeholder="মোবাইল নম্বর" />
<input id="fisherPassword" placeholder="Password" type="password" />
<button class="primary" onclick="signupFisher()">Create Fisher Account</button>
<div style="text-align:center;margin-top:8px">
<button class="btn-ghost" onclick="showLogin()">Already have account</button>
</div>
</div>

<div id="forgotPasswordForm" style="display:none">
<p style="margin-bottom:10px">Enter your email address to reset your password:</p>
<input id="forgotEmail" placeholder="Email" type="email" />
<button class="primary" onclick="sendPasswordReset()">Send Reset Code</button>
<div id="resetCodeSection" style="display:none;margin-top:10px">
<p style="margin-bottom:10px">Enter the verification code sent to your email:</p>
<input id="resetCode" placeholder="Verification Code" />
<input id="resetNewPassword" placeholder="New Password" type="password" />
<button class="primary" onclick="resetPassword()">Reset Password</button>
</div>
<div style="text-align:center;margin-top:8px">
<button class="btn-ghost" onclick="showLogin()">Back to Login</button>
</div>
</div>
</div>

<div id="accProfile" style="display:none">
<div style="margin-bottom:8px"><b>Logged in as:</b></div>
<div class="account-info">
<div>
<div id="pName" style="font-weight:800"></div>
<div id="pEmail" class="small"></div>
<div id="pNumber" class="small"></div>
</div>
</div>
<div style="margin-top:12px;display:flex;gap:8px">
<button class="primary" onclick="closeAccountModal()">Close</button>
<button class="btn-ghost" onclick="logoutUser()">Logout</button>
</div>
</div>

</div>
</div>

<!-- TOP BAR -->
<div class="topbar">
<div class="logo">
<img id="logoImg" src="https://i.ibb.co.com/DD3h4qjv/file-000000007d947207b10fa3593fc67aa8.png" alt="Logo">
<div>
<div id="siteName" style="font-size:16px;font-weight:800;color:#0037dd">Fast & Secure Exchange</div>
<div id="siteTagline" class="small">সকাল৯ঃ০০টা থেকে রাত১০ঃ০০টা</div>
</div>
</div>

<div id="opStatus" class="status-badge offline">Offline</div>

<div class="top-buttons">
<button id="myAccountBtn" onclick="openAccountModal()">My Account</button>
</div>
</div>

<div class="blue-head">
<h1 id="welcomeTitle">Welcome to Dollar Exchange</h1>
<p id="welcomeSubtitle" class="small">দয়া করে ট্রানজেকশন শুরু করার আগে নিয়মগুলো পড়ে নিন</p>
</div>

<div id="nav">
<button id="navButton" onclick="showUser()">নগদ বিকাশ 5 টাকা সেন্ড মানি ফি কেটে নেওয়া হয়</button>
</div>

<!-- USER AREA -->
<div id="userArea">
<h2 style="text-align:center;margin-top:18px"></h2>

<div class="card">
<div id="orderInstructions" style="margin-bottom:15px; padding:10px; background:#f9f9f9; border-radius:8px; display:none;">
<h4>Order Instructions</h4>
<p id="orderInstructionsText"></p>
</div>

<!-- Trade Type Toggle -->
<div class="trade-type-toggle">
<button id="bhaiButton" class="active" onclick="setTradeType('buy')">buy</button>
<button id="saleButton" onclick="setTradeType('sell')">sell</button>
</div>

<input id="uName" placeholder="আপনার নাম" />
<div style="position:relative">
<select id="uCurrency" onchange="updatePlaceholderText()">
<!-- Currencies will be dynamically loaded here -->
</select>
</div>

<input id="uDollar" type="number" placeholder="কত ডলার সেল দিবেন (minimum 1 dollar)" oninput="calc()" />
<input id="uTaka" placeholder="টাকায় মূল্য" readonly />

<div id="feeInfo" style="margin:8px 0; font-size:13px; color:#666; display:none;">
<div>Transaction Fee: <span id="transactionFeeAmount">0</span> Tk (<span id="transactionFeePercent">0</span>%)</div>
<div>Total Amount: <span id="totalAmount">0</span> Tk</div>
</div>

<select id="uPayment">
<!-- Payment methods will be dynamically loaded here -->
</select>

<input id="uNumber" placeholder="আপনার পেমেন্ট নাম্বার " numberonly />

<select id="uVia">
<option value="bKash">bKash</option>
<option value="Nagad">Nagad</option>
<option value="Rocket">Rocket</option>
</select>

<input id="uTx" placeholder="এই অপশনে কিছু লিখতে হবে না)">

<button class="primary" onclick="placeOrder()">অর্ডার দিন</button>
</div>

<h3 style="text-align:center;margin-top:8px">📦 আপনার অর্ডারসমূহ</h3>
<div id="myOrdersContainer" class="card" style="padding:8px;">
<div id="myOrders"></div>
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

<!-- CONFIRM ORDER MODAL -->
<div id="confirmModal" style="display:none;">
<div class="modal" onclick="closeConfirm(event)">
<div class="box" onclick="event.stopPropagation()">
<h3>Confirm Your Order</h3>
<div id="cBody"></div>
<input id="cTx" placeholder="আপনার Transaction ID লিখুন" />
<button class="primary" onclick="confirmOrder()">Confirm Order</button>
<div style="margin-top:12px;text-align:right"><button onclick="closeConfirm()">Close</button></div>
</div>
</div>
</div>

<!-- WhatsApp Button -->
<a id="whatsappLink" href="https://wa.me/qr/DTBEJ472LPKOA1" class="wa-btn" target="_blank">
<img src="https://i.ibb.co/dnLD0Wf/20251129-064417.jpg" alt="wa">
</a>

<!-- Firebase SDK -->
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>
<script>
// Firebase Config
const firebaseConfig = {
apiKey: "AIzaSyCE57xIZr1igoPT7EkpDz0SIVYvFHle97U",
authDomain: "dollar-exchange-bdt-fa179.firebaseapp.com",
projectId: "dollar-exchange-bdt-fa179",
storageBucket: "dollar-exchange-bdt-fa179.firebasestorage.app",
messagingSenderId: "294819905234",
appId: "1:294819905234:web:4da06ee71d54daeb40770b"
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

// Get DOM elements
const modal = document.getElementById('modal');
const confirmModal = document.getElementById('confirmModal');
const accountModal = document.getElementById('accountModal');

// DEFAULT CURRENCIES
let currencies = [
{ id: 'Payeer', name: 'Payeer', buyRate: 68, sellRate: 70, paymentId: 'P1131698605', image: 'https://i.ibb.co/6yJ1s7Q/payeer-logo.png', minDollar: 5, maxDollar: 500 },
{ id: 'Binance', name: 'Binance', buyRate: 19, sellRate: 20, paymentId: '1188473082', image: 'https://i.ibb.co/k3QJz5w/binance-logo.png', minDollar: 1, maxDollar: 1000 },
{ id: 'Advcash', name: 'Advcash', buyRate: 58, sellRate: 60, paymentId: 'U 1048 5654 4714', image: 'https://i.ibb.co/1n1J7r6/advcash-logo.png', minDollar: 10, maxDollar: 300 }
];

// DEFAULT PAYMENT METHODS
let paymentMethods = [];

// SITE SETTINGS
let siteSettings = {
name: 'Fast & Secure Exchange',
tagline: 'সকাল৯ঃ০০টা থেকে রাত১০ঃ০০টা',
primaryColor: '#0b75ff',
secondaryColor: '#0037dd',
backgroundColor: '#f2f5f8',
workStartTime: '09:00', 
workEndTime: '22:00',
statusOverride: '',
orderInstructions: '',
whatsappLink: 'https://wa.me/qr/DTBEJ472LPKOA1',
contactEmail: '',
contactPhone: '',
minDollarAmount: 1,
maxDollarAmount: 1000,
transactionFee: 0,
transactionFeePercent: 0,
logoUrl: 'https://i.ibb.co.com/DD3h4qjv/file-000000007d947207b10fa3593fc67aa8.png',
faviconUrl: '',
maintenanceMode: false,
maintenanceMessage: 'We are currently performing maintenance. Please check back later.',
requireLogin: false,
showLoading: true
};

// CONTENT SETTINGS
let contentSettings = {
welcomeTitle: 'Welcome to Dollar Exchange',
welcomeSubtitle: 'দয়া করে ট্রানজেকশন শুরু করার আগে নিয়মগুলো পড়ে নিন',
navButtonText: 'নগদ বিকাশ 5 টাকা সেন্ড মানি ফি কেটে নেওয়া হয়',
homeContent: '',
rulesTitle: 'Exchange Rules',
rulesContent: 'Please read all rules before making a transaction.',
globalNotification: '',
notificationType: 'info',
notificationActive: false,
typingText: '',
typingSpeed: 80
};

// Trade type state
let currentTradeType = 'bhai';

// ACCOUNT FUNCTIONS
function openAccountModal(){ accountModal.style.display = "flex"; document.body.classList.add('modal-open'); updateAccountUI(); }
function closeAccountModal(){ accountModal.style.display = "none"; document.body.classList.remove('modal-open'); }
function showSignup(){ loginForm.style.display='none'; signupForm.style.display='block'; fisherForm.style.display='none'; forgotPasswordForm.style.display='none'; accProfile.style.display='none'; accTitle.innerText = 'Sign Up'; }
function showLogin(){ loginForm.style.display='block'; signupForm.style.display='none'; fisherForm.style.display='none'; forgotPasswordForm.style.display='none'; accProfile.style.display='none'; accTitle.innerText = 'Login'; }
function showFisher(){ loginForm.style.display='none'; signupForm.style.display='none'; fisherForm.style.display='block'; forgotPasswordForm.style.display='none'; accProfile.style.display='none'; accTitle.innerText = 'Fisher Sign Up'; }
function showForgotPassword(){ loginForm.style.display='none'; signupForm.style.display='none'; fisherForm.style.display='none'; forgotPasswordForm.style.display='block'; accProfile.style.display='none'; accTitle.innerText = 'Reset Password'; }

async function signupUser(){
const name = signupName.value.trim();
const email = signupEmail.value.trim().toLowerCase();
const number = signupNumber.value.trim();
const pass = signupPassword.value;
if(!name || !email || !pass || !number){ alert('সব ঘর পূরণ করুন'); return; }
try {
const userSnapshot = await db.collection('users').where('email', '==', email).get();
if (!userSnapshot.empty) { alert('এই ইমেইল দিয়ে আগে থেকেই অ্যাকাউন্ট আছে'); return; }
await db.collection('users').add({ name, email, number, password: pass, userType: 'regular', createdAt: new Date().toISOString() });
localStorage.setItem('currentUser', JSON.stringify({ name, email, number, userType: 'regular' }));
signupName.value=''; signupEmail.value=''; signupNumber.value=''; signupPassword.value='';
updateAccountUI(); closeAccountModal(); alert('Account created and logged in ✔'); prefillUserFields(); loadMyOrders();
} catch (error) { console.error("Error creating user:", error); alert("Error creating account. Please try again."); }
}

async function signupFisher(){
const name = fisherName.value.trim();
const email = fisherEmail.value.trim().toLowerCase();
const number = fisherNumber.value.trim();
const pass = fisherPassword.value;
if(!name || !email || !pass || !number){ alert('সব ঘর পূরণ করুন'); return; }
try {
const userSnapshot = await db.collection('users').where('email', '==', email).get();
if (!userSnapshot.empty) { alert('এই ইমেইল দিয়ে আগে থেকেই অ্যাকাউন্ট আছে'); return; }
await db.collection('users').add({ name, email, number, password: pass, userType: 'fisher', createdAt: new Date().toISOString() });
localStorage.setItem('currentUser', JSON.stringify({ name, email, number, userType: 'fisher' }));
fisherName.value=''; fisherEmail.value=''; fisherNumber.value=''; fisherPassword.value='';
updateAccountUI(); closeAccountModal(); alert('Fisher Account created and logged in ✔'); prefillUserFields(); loadMyOrders();
} catch (error) { console.error("Error creating fisher account:", error); alert("Error creating fisher account. Please try again."); }
}

async function loginUser(){
const email = loginEmail.value.trim().toLowerCase();
const pass = loginPassword.value;
try {
const userSnapshot = await db.collection('users').where('email', '==', email).where('password', '==', pass).get();
if (userSnapshot.empty) { alert('Invalid credentials'); return; }
const user = userSnapshot.docs[0].data();
localStorage.setItem('currentUser', JSON.stringify({ name: user.name, email: user.email, number: user.number, userType: user.userType || 'regular' }));
updateAccountUI(); closeAccountModal(); alert('Login successful ✔'); prefillUserFields(); loadMyOrders();
} catch (error) { console.error("Error logging in:", error); alert("Error logging in. Please try again."); }
}

function logoutUser(){ localStorage.removeItem('currentUser'); updateAccountUI(); alert('Logged out'); uName.value=''; uNumber.value=''; loadMyOrders(); }
function getCurrentUser(){ return JSON.parse(localStorage.getItem('currentUser')||'null'); }

function updateAccountUI(){
const u = getCurrentUser();
if(u){
accProfile.style.display='block'; loginForm.style.display='none'; signupForm.style.display='none'; fisherForm.style.display='none'; forgotPasswordForm.style.display='none'; accTitle.innerText = 'Account';
pName.innerText = u.name; pEmail.innerText = u.email; pNumber.innerText = u.number; myAccountBtn.innerText = 'Account ✓';
} else {
accProfile.style.display='none'; loginForm.style.display='block'; signupForm.style.display='none'; fisherForm.style.display='none'; forgotPasswordForm.style.display='none'; accTitle.innerText = 'Login'; myAccountBtn.innerText = 'My Account';
}
}

function prefillUserFields(){
const u = getCurrentUser();
if(u){ uName.value = u.name || ''; uNumber.value = u.number || ''; }
}

async function sendPasswordReset() {
const email = forgotEmail.value.trim().toLowerCase();
if (!email) { alert('Please enter your email address'); return; }
try {
const userSnapshot = await db.collection('users').where('email', '==', email).get();
if (userSnapshot.empty) { alert('No account found with this email address'); return; }
const resetCode = Math.floor(100000 + Math.random() * 900000).toString();
const expiryTime = new Date(Date.now() + 3600000).toISOString();
await db.collection('passwordResets').add({ email, resetCode, expiryTime, used: false });
document.getElementById('resetCodeSection').style.display = 'block';
alert(`A verification code has been sent to your email. For demo purposes, your code is: ${resetCode}`);
} catch (error) { console.error("Error sending password reset:", error); alert("Error sending password reset. Please try again."); }
}

async function resetPassword() {
const email = forgotEmail.value.trim().toLowerCase();
const code = resetCode.value.trim();
const newPassword = resetNewPassword.value;
if (!email || !code || !newPassword) { alert('Please fill all fields'); return; }
try {
const resetSnapshot = await db.collection('passwordResets').where('email', '==', email).where('resetCode', '==', code).where('used', '==', false).get();
if (resetSnapshot.empty) { alert('Invalid or expired verification code'); return; }
const resetDoc = resetSnapshot.docs[0];
const resetData = resetDoc.data();
if (new Date() > new Date(resetData.expiryTime)) { alert('Verification code has expired'); return; }
await db.collection('passwordResets').doc(resetDoc.id).update({ used: true });
const userSnapshot = await db.collection('users').where('email', '==', email).get();
if (!userSnapshot.empty) { const userDoc = userSnapshot.docs[0]; await db.collection('users').doc(userDoc.id).update({ password: newPassword }); }
forgotEmail.value = ''; resetCode.value = ''; resetNewPassword.value = '';
document.getElementById('resetCodeSection').style.display = 'none';
alert('Password reset successful! You can now login with your new password.');
showLogin();
} catch (error) { console.error("Error resetting password:", error); alert("Error resetting password. Please try again."); }
}

// LOAD SETTINGS
async function loadSiteSettings() {
try {
const settingsDoc = await db.collection('settings').doc('site').get();
if (settingsDoc.exists) { siteSettings = { ...siteSettings, ...settingsDoc.data() }; }
} catch (error) { console.error("Error loading site settings:", error); }
applySiteSettings();
}

async function loadContentSettings() {
try {
const contentDoc = await db.collection('settings').doc('content').get();
if (contentDoc.exists) { contentSettings = { ...contentSettings, ...contentDoc.data() }; }
} catch (error) { console.error("Error loading content settings:", error); }
applyContentSettings();
}

async function loadPaymentMethods() {
try {
const paymentMethodsDoc = await db.collection('settings').doc('paymentMethods').get();
if (paymentMethodsDoc.exists) { paymentMethods = paymentMethodsDoc.data().list || paymentMethods; }
} catch (error) { console.error("Error loading payment methods:", error); }
updatePaymentMethods();
}

function applySiteSettings() {
document.title = siteSettings.name || 'Dollar Exchange';
document.getElementById('pageTitle').textContent = siteSettings.name || 'Dollar Exchange';
if (siteSettings.faviconUrl) { document.getElementById('favicon').href = siteSettings.faviconUrl; }
if (siteSettings.logoUrl) { document.getElementById('logoImg').src = siteSettings.logoUrl; }
document.getElementById('siteName').textContent = siteSettings.name;
if(siteSettings.workStartTime && siteSettings.workEndTime) { document.getElementById('siteTagline').textContent = `সকাল ${siteSettings.workStartTime} টা থেকে রাত ${siteSettings.workEndTime} টা`; }
document.documentElement.style.setProperty('--primary-color', siteSettings.primaryColor);
document.documentElement.style.setProperty('--secondary-color', siteSettings.secondaryColor);
document.body.style.backgroundColor = siteSettings.backgroundColor;
document.querySelectorAll('.primary').forEach(el => { el.style.backgroundColor = siteSettings.primaryColor; });
document.querySelectorAll('.top-buttons button').forEach(el => { el.style.borderColor = siteSettings.primaryColor; el.style.color = siteSettings.primaryColor; });
document.querySelector('.blue-head').style.background = `linear-gradient(180deg,${siteSettings.secondaryColor},${siteSettings.primaryColor})`;
if (siteSettings.whatsappLink) { document.getElementById('whatsappLink').href = siteSettings.whatsappLink; }
if (siteSettings.orderInstructions) { document.getElementById('orderInstructions').style.display = 'block'; document.getElementById('orderInstructionsText').textContent = siteSettings.orderInstructions; }
if (siteSettings.maintenanceMode) { document.getElementById('maintenanceOverlay').style.display = 'flex'; document.getElementById('maintenanceMessage').textContent = siteSettings.maintenanceMessage; }
}

function applyContentSettings() {
document.getElementById('welcomeTitle').textContent = contentSettings.welcomeTitle;
document.getElementById('welcomeSubtitle').textContent = contentSettings.welcomeSubtitle;
document.getElementById('navButton').textContent = contentSettings.navButtonText;

if (contentSettings.globalNotification && contentSettings.notificationActive) {
const notificationEl = document.getElementById('globalNotification');
notificationEl.textContent = contentSettings.globalNotification;
notificationEl.className = `global-notification notification-${contentSettings.notificationType}`;
notificationEl.style.display = 'block';
}

// Initialize Typing Text Animation
if (contentSettings.typingText) {
   startTypingAnimation(contentSettings.typingText, contentSettings.typingSpeed || 80);
}
}

function updatePaymentMethods() {
const uPayment = document.getElementById('uPayment');
uPayment.innerHTML = '';
paymentMethods.forEach(method => {
const option = document.createElement('option');
option.value = method.id;
option.textContent = method.name;
uPayment.appendChild(option);
});
}

// CURRENCY MANAGEMENT
async function loadCurrencies(){
try {
const currenciesDoc = await db.collection('settings').doc('currencies').get();
if (currenciesDoc.exists) {
const loadedCurrencies = currenciesDoc.data().list || currencies;
currencies = loadedCurrencies.map(currency => ({ ...currency, minDollar: currency.minDollar || siteSettings.minDollarAmount || 1, maxDollar: currency.maxDollar || siteSettings.maxDollarAmount || 1000 }));
}
} catch (error) { console.error("Error loading currencies:", error); }
const uCurrency = document.getElementById('uCurrency');
uCurrency.innerHTML = '';
currencies.forEach(currency => {
const option = document.createElement('option');
option.value = currency.id;
const rate = currentTradeType === 'bhai' ? (currency.buyRate || currency.rate) : (currency.sellRate || currency.rate);
option.textContent = `${currency.name} (${rate} BDT)`;
uCurrency.appendChild(option);
});
if (uDollar.value) { calc(); }
}

function setTradeType(type) {
currentTradeType = type;
document.getElementById('bhaiButton').classList.toggle('active', type === 'bhai');
document.getElementById('saleButton').classList.toggle('active', type === 'sale');
loadCurrencies();
}

function calc(){
const dollar = Number(uDollar.value) || 0;
const currencyId = uCurrency.value;
const currency = currencies.find(c => c.id === currencyId);
const rate = currency ? (currentTradeType === 'bhai' ? (currency.buyRate || currency.rate) : (currency.sellRate || currency.rate)) : 0;
const taka = dollar * rate;
uTaka.value = taka.toFixed(2);
if (siteSettings.transactionFee > 0 || siteSettings.transactionFeePercent > 0) {
const feeFixed = siteSettings.transactionFee || 0;
const feePercent = siteSettings.transactionFeePercent || 0;
const feeAmount = feeFixed + (taka * feePercent / 100);
const total = taka + feeAmount;
document.getElementById('feeInfo').style.display = 'block';
document.getElementById('transactionFeeAmount').textContent = feeAmount.toFixed(2);
document.getElementById('transactionFeePercent').textContent = feePercent;
document.getElementById('totalAmount').textContent = total.toFixed(2);
} else { document.getElementById('feeInfo').style.display = 'none'; }
}

function updatePlaceholderText() {
const currencyId = uCurrency.value;
const currency = currencies.find(c => c.id === currencyId);
calc();
if (currency) {
const minLimit = currency.minDollar || siteSettings.minDollarAmount;
const maxLimit = currency.maxDollar || siteSettings.maxDollarAmount;
uDollar.placeholder = `কত ডলার সেল দিবেন (${minLimit} থেকে ${maxLimit} ডলার পর্যন্ত)`;
} else { uDollar.placeholder = `কত ডলার সেল দিবেন (minimum ${siteSettings.minDollarAmount} dollar)`; }
}

// PLACE ORDER
function placeOrder(){
const current = getCurrentUser();
if(siteSettings.requireLogin && !current){ alert('অর্ডার দিতে হলে প্রথমে Login/Sign Up করুন'); openAccountModal(); return; }
const name = (uName.value.trim() || current?.name);
const number = (uNumber.value.trim() || current?.number);
const currencyId = uCurrency.value;
const currency = currencies.find(c => c.id === currencyId);
const currencyName = currency ? currency.name : '';
const dollar = parseFloat(uDollar.value);
const taka = uTaka.value;
const payment = uPayment.value;
const via = uVia.value || "";
const tx = uTx.value.trim() || "";

if (currency) {
const minLimit = currency.minDollar || siteSettings.minDollarAmount;
const maxLimit = currency.maxDollar || siteSettings.maxDollarAmount;
if (dollar < minLimit) { alert(`${currencyName} এর জন্য সর্বনিম্ন ${minLimit} ডলার হতে হবে`); return; }
if (dollar > maxLimit) { alert(`${currencyName} এর জন্য সর্বোচ্চ ${maxLimit} ডলার হতে হবে`); return; }
} else {
if (dollar < siteSettings.minDollarAmount) { alert(`Minimum dollar amount is ${siteSettings.minDollarAmount}`); return; }
if (dollar > siteSettings.maxDollarAmount) { alert(`Maximum dollar amount is ${siteSettings.maxDollarAmount}`); return; }
}

if(!name || !number || !dollar){ alert('সব ঘর পূরণ করুন'); return; }

window.tempOrder = { name, number, currency: currencyName, currencyId, dollar, taka, payment, via, tx, tradeType: currentTradeType };

// এখানে this পাস করা হয়েছে এবং ক্লাস যোগ করা হয়েছে
const paymentIds = currencies.map(c => `<div class="id-badge">${c.name} ID: ${c.paymentId}<button class="copy-btn-inside" onclick="copyText('${c.paymentId}', this)">Copy</button></div>`).join('');

cBody.innerHTML = `
<div>নাম: <b>${name}</b></div>
<div>নম্বার: <b>${number}</b></div>
<div>কারেন্সি: <b>${currencyName}</b></div>
<div>ডলার: <b>${dollar}</b> → টাকা: <b>${taka}</b></div>
<div>Trade Type: <b>${currentTradeType === 'bhai' ? 'Bhai Rate' : 'Sale Rate'}</div>
<div style="margin-top:10px">পেমেন্ট পাঠানোর আইডি:</div>
 ${paymentIds}
`;
cTx.value = tx;
confirmModal.style.display = "flex";
document.body.classList.add('modal-open');
}

async function confirmOrder(){
const tx = cTx.value.trim();
const o = tempOrder;
if(!o) return;
try {
const newOrder = {
name: o.name, number: o.number, currency: o.currency, currencyId: o.currencyId, dollar: o.dollar, taka: o.taka, paymentMethod: o.payment, via: o.via || "", trx: tx || "",
status: 'PENDING', createdAt: new Date().toISOString(), userEmail: getCurrentUser() ? getCurrentUser().email : null, userType: getCurrentUser() ? getCurrentUser().userType || 'regular' : 'regular', tradeType: o.tradeType || 'sell'
};
const docRef = await db.collection('orders').add(newOrder);
newOrder.id = docRef.id;
confirmModal.style.display="none"; document.body.classList.remove('modal-open'); tempOrder=null;
loadMyOrders(); alert("অর্ডার Confirm হয়েছে ✔");
uDollar.value=""; uTaka.value=""; uTx.value=""; uVia.value="";
} catch (error) { console.error("Error creating order:", error); alert("Error creating order. Please try again."); }
}

// LOAD MY ORDERS
function getBanglaStatus(status){
if(status === "COMPLETED") return { text: "✔ completed", cls: "completed" };
if(status === "REJECTED") return { text: "✘ বাতিল করা হয়েছে", cls: "rejected" };
return { text: "⌛ অপেক্ষমাণ", cls: "pending" };
}

async function loadMyOrders(){
try {
const current = getCurrentUser();
myOrders.innerHTML = "";
let ordersQuery;
if(current){
ordersQuery = db.collection('orders').where('userEmail', '==', current.email);
const emailSnapshot = await ordersQuery.get();
const numberQuery = db.collection('orders').where('number', '==', current.number);
const numberSnapshot = await numberQuery.get();
const emailOrders = emailSnapshot.docs.map(doc => ({id: doc.id, ...doc.data()}));
const numberOrders = numberSnapshot.docs.map(doc => ({id: doc.id, ...doc.data()}));
const allOrders = [...emailOrders];
numberOrders.forEach(order => { if (!allOrders.find(o => o.id === order.id)) { allOrders.push(order); } });
allOrders.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
renderOrders(allOrders);
} else {
const number = uNumber.value.trim();
if(number) {
const numberQuery = db.collection('orders').where('number', '==', number);
const numberSnapshot = await numberQuery.get();
const orders = numberSnapshot.docs.map(doc => ({id: doc.id, ...doc.data()}));
orders.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
renderOrders(orders);
} else { myOrders.innerHTML='<div class="order-empty">আপনার অর্ডার নেই</div>'; }
}
} catch (error) { console.error("Error loading orders:", error); myOrders.innerHTML='<div class="order-empty">Error loading orders</div>'; }
}

function renderOrders(orders) {
if(orders.length===0){ myOrders.innerHTML='<div class="order-empty">আপনার অর্ডার নেই</div>'; return; }
orders.forEach(o=>{
let currencyName = o.currency;
if (!currencyName && o.currencyId) { const currency = currencies.find(c => c.id === o.currencyId); currencyName = currency ? currency.name : o.currencyId; }
const s = getBanglaStatus(o.status);
const box=document.createElement('div');
box.className='order-box';
box.innerHTML=`
<div class="order-info">
<div><b>${o.name}</b> <span class="small">• ${new Date(o.createdAt).toLocaleString()}</span></div>
<div class="small">${currencyName} • ${o.dollar} USD → ${o.taka} TK • পেমেন্ট: ${o.paymentMethod}</div>
<div class="small">মাধ্যম: ${o.via || 'Not given'}</div>
<div class="small">TXID: ${o.trx || 'Not given'}</div>
<div class="small">Trade Type: ${o.tradeType === 'bhai' ? 'Bhai Rate' : 'Sale Rate'}</div>
</div>
<div class="order-meta">
<div><span class="status ${s.cls}">${s.text}</span></div>
<button onclick="openModal('${o.id}')">View</button>
</div>
`;
myOrders.appendChild(box);
});
}

// MODAL
let currentModalId=null;
async function openModal(id){
currentModalId=id;
try {
const orderDoc = await db.collection('orders').doc(id).get();
if (!orderDoc.exists) return;
const o = {id: orderDoc.id, ...orderDoc.data()};
let currencyName = o.currency;
if (!currencyName && o.currencyId) { const currency = currencies.find(c => c.id === o.currencyId); currencyName = currency ? currency.name : o.currencyId; }
mTitle.innerText="Order — "+o.name;
mBody.innerHTML=`
<div class="small">Created: ${new Date(o.createdAt).toLocaleString()}</div>
<div>নাম: <b>${o.name}</b></div>
<div>নম্বার: <b>${o.number}</b></div>
<div>কারেন্সি: <b>${currencyName}</b></div>
<div>ডলার: <b>${o.dollar}</b> → টাকা: <b>${o.taka}</b></div>
<div>Trade Type: <b>${o.tradeType === 'bhai' ? 'Bhai Rate' : 'Sale Rate'}</div>
<div style="margin-top:8px">বর্তমান স্ট্যাটাস: <b>${o.status}</b></div>
`;
mTx.value = o.trx || "";
modal.style.display="flex";
document.body.classList.add('modal-open');
} catch (error) { console.error("Error opening order:", error); alert("Error loading order details"); }
}

function closeModal(event){
if (!event || event.target === modal || event.target.textContent === 'Close') { modal.style.display='none'; document.body.classList.remove('modal-open'); currentModalId=null; }
}

function closeConfirm(event){
if (!event || event.target === confirmModal || event.target.textContent === 'Close') { confirmModal.style.display='none'; document.body.classList.remove('modal-open'); }
}

async function saveTx(){
if(!currentModalId) return;
const tx = mTx.value.trim();
try {
await db.collection('orders').doc(currentModalId).update({ trx: tx });
loadMyOrders(); alert("Transaction ID Updated"); closeModal();
} catch (error) { console.error("Error updating transaction ID:", error); alert("Error updating transaction ID. Please try again."); }
}

// --- এখানে কপি ফাংশনটি ঠিক করা হয়েছে ---
function copyText(text, btn) {
  navigator.clipboard.writeText(text).then(() => {
    // অ্যালার্ট এর বদলে বাটনের টেক্সট পরিবর্তন করে কনফার্মেশন দিচ্ছি
    if (btn) {
      const originalText = btn.innerText;
      btn.innerText = "Copied!";
      btn.classList.add('copied'); // সবুজ রঙ যোগ করছি
      
      // ১.৫ সেকেন্ড পর আগের অবস্থায় ফেরত
      setTimeout(() => {
        btn.innerText = originalText;
        btn.classList.remove('copied');
      }, 1500);
    }
  }).catch(err => {
    console.error('Could not copy text: ', err);
    alert("Failed to copy text");
  });
}
// --- কপি ফাংশন শেষ ---

// ONLINE/OFFLINE STATUS
function updateStatus(){
  let isOnline = false;
  const current = new Date();
  
  if (siteSettings.statusOverride === 'online') {
    isOnline = true;
  } else if (siteSettings.statusOverride === 'offline') {
    isOnline = false;
  } else {
    if (siteSettings.workStartTime && siteSettings.workEndTime) {
      const nowHours = current.getHours();
      const nowMinutes = current.getMinutes();
      const nowTotalMinutes = nowHours * 60 + nowMinutes;

      const [startH, startM] = siteSettings.workStartTime.split(':').map(Number);
      const startTotalMinutes = startH * 60 + startM;

      const [endH, endM] = siteSettings.workEndTime.split(':').map(Number);
      const endTotalMinutes = endH * 60 + endM;

      if (endTotalMinutes > startTotalMinutes) {
        isOnline = nowTotalMinutes >= startTotalMinutes && nowTotalMinutes < endTotalMinutes;
      } else {
        isOnline = nowTotalMinutes >= startTotalMinutes || nowTotalMinutes < endTotalMinutes;
      }
    } else {
      const hour = current.getHours();
      isOnline = hour >= 9 && hour < 22;
    }
  }

  if(isOnline){ opStatus.innerText='Online'; opStatus.className='status-badge online'; } 
  else { opStatus.innerText='Offline'; opStatus.className='status-badge offline'; }
}
setInterval(updateStatus, 60000);

function showUser(){ userArea.style.display='block'; }

// TYPING ANIMATION FUNCTION
let typingInterval;
function startTypingAnimation(text, speed) {
  const element = document.getElementById('welcomeSubtitle');
  if (!element || !text) return;
  
  if (typingInterval) clearInterval(typingInterval);
  
  element.innerHTML = '';
  element.classList.add('typing-text');
  let charIndex = 0;
  
  function type() {
    if (charIndex < text.length) {
      element.textContent += text.charAt(charIndex);
      charIndex++;
    } else {
      clearInterval(typingInterval);
      setTimeout(() => {
         element.textContent = '';
         charIndex = 0;
         typingInterval = setInterval(type, speed);
      }, 2000);
    }
  }
  typingInterval = setInterval(type, speed);
}

// ON LOAD
window.addEventListener('load',async()=>{
// Load all settings from Firebase
await loadSiteSettings();
await loadContentSettings();
await loadPaymentMethods();
await loadCurrencies();
updateAccountUI();
prefillUserFields();
loadMyOrders();
updatePlaceholderText();
updateStatus();

// Hide loading screen after everything is loaded
const loadingScreen = document.getElementById('loadingScreen');
if (siteSettings.showLoading === false) {
    loadingScreen.style.display = 'none';
} else {
    loadingScreen.style.opacity = '0';
    setTimeout(() => {
      loadingScreen.style.display = 'none';
    }, 500);
}
});

document.getElementById('uCurrency').addEventListener('change', function() { updatePlaceholderText(); });
</script>
</body>
</html>
  
