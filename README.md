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
.top-buttons{display:flex;gap:8px}
.top-buttons button{padding:6px 12px;border-radius:20px;border:1px solid #0b75ff;background:#fff;color:#0b75ff;font-weight:700}

.blue-head{background:linear-gradient(180deg,#0037dd,#006bff);padding:36px 18px;text-align:center;border-bottom-left-radius:26px;border-bottom-right-radius:26px;color:#fff}
.blue-head h1{margin:0;font-size:26px;letter-spacing:0.2px}

.card{width:92%;max-width:720px;margin:14px auto;background:#fff;padding:14px;border-radius:10px;box-shadow:0 2px 10px rgba(0,0,0,0.06)}

input,select,button,textarea{width:100%;padding:10px;margin:8px 0;border-radius:8px;border:1px solid #ddd;font-size:15px;box-sizing:border-box}
button.primary{background:#0b75ff;color:#fff;border:none;padding:11px;border-radius:8px;cursor:pointer}
button.danger{background:#ef4444;color:#fff;border:none;padding:11px;border-radius:8px;cursor:pointer}
button.success{background:#16a34a;color:#fff;border:none;padding:11px;border-radius:8px;cursor:pointer}
button.warning{background:#f59e0b;color:#fff;border:none;padding:11px;border-radius:8px;cursor:pointer}

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

.quote-item{background:#f9fafb;padding:15px;border-radius:8px;margin-bottom:15px;position:relative}
.quote-text{font-style:italic;margin-bottom:10px;color:#333}
.quote-author{font-weight:bold;text-align:right;color:#666}
.quote-actions{position:absolute;top:10px;right:10px;display:flex;gap:5px}

.tab-container{display:flex;overflow-x:auto;margin-bottom:10px}
.tab-button{padding:10px 15px;background:#f3f4f6;border:none;border-bottom:3px solid transparent;cursor:pointer;white-space:nowrap}
.tab-button.active{border-bottom-color:#0b75ff;background:#e5f0ff}

.form-section{display:none}
.form-section.active{display:block}

.color-preview{width:30px;height:30px;border-radius:4px;display:inline-block;margin-left:10px;border:1px solid #ddd}

.toggle-switch{position:relative;display:inline-block;width:60px;height:34px}
.toggle-switch input{opacity:0;width:0;height:0}
.slider{position:absolute;cursor:pointer;top:0;left:0;right:0;bottom:0;background-color:#ccc;transition:.4s;border-radius:34px}
.slider:before{position:absolute;content:"";height:26px;width:26px;left:4px;bottom:4px;background-color:white;transition:.4s;border-radius:50%}
input:checked + .slider{background-color:#0b75ff}
input:checked + .slider:before{transform:translateX(26px)}

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

/* Loading Spinner */
.loading-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(255,255,255,0.9);
    z-index: 10000;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    transition: opacity 0.3s;
}

.loading-spinner {
    width: 50px;
    height: 50px;
    border: 5px solid #f3f3f3;
    border-top: 5px solid #0b75ff;
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin-bottom: 15px;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

/* Animated Text Preview */
.animated-text-preview {
    background: linear-gradient(180deg,#0037dd,#006bff);
    padding: 20px;
    border-radius: 8px;
    margin-top: 15px;
    color: white;
    overflow: hidden;
}

.animated-text-preview .preview-text {
    display: inline-block;
    animation: scrollText 20s linear infinite;
    white-space: nowrap;
}

@keyframes scrollText {
    0% { transform: translateX(100%); }
    100% { transform: translateX(-100%); }
}

/* Switch Styles */
.switch-container {
    display: flex;
    align-items: center;
    gap: 10px;
    margin: 10px 0;
}

.switch-label {
    font-weight: 600;
    min-width: 150px;
}

@media (max-width:520px){
.topbar{padding:8px}
.blue-head{padding:24px 12px}
}
</style>
</head>
<body>

<!-- Loading Overlay -->
<div id="loadingOverlay" class="loading-overlay" style="display: none;">
    <div class="loading-spinner"></div>
    <div class="loading-text">লোড হচ্ছে...</div>
</div>

<!-- Admin Login (hidden by default) -->
<div id="adminLogin" class="modal" style="display:none;">
<div class="box">
<h3>Admin Login</h3>
<input id="adminEmail" type="email" placeholder="Admin Gmail">
<input id="adminPass" type="password" placeholder="Enter Password">
<div style="display:flex; align-items:center; margin:10px 0;">
<input type="checkbox" id="rememberMe" checked style="width:auto; margin-right:8px;">
<label for="rememberMe">Keep me logged in</label>
</div>
<button class="primary" onclick="checkAdmin()">Login</button>
<div style="text-align:right;margin-top:8px"><button onclick="window.location.href='index.html'">Go to User Panel</button></div>
</div>
</div>

<!-- TOP BAR -->
<div class="topbar">
<div class="logo">
<img src="https://i.ibb.co.com/DD3h4qjv/file-000000007d947207b10fa3593fc67aa8.png" alt="file-000000007d947207b10fa3593fc67aa8" border="0">
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
<div style="display:flex;gap:10px;justify-content:center;flex-wrap:wrap">
<button class="primary" onclick="showAdminDashboard()">Dashboard</button>
<button class="primary" onclick="showCurrencyManagement()">Currency Management</button>
<button class="primary" onclick="showOrderManagement()">Order Management</button>
<button class="primary" onclick="showUserManagement()">User Management</button>
<button class="primary" onclick="showSiteSettings()">Site Settings</button>
<button class="primary" onclick="showContentManagement()">Content Management</button>
<button class="primary" onclick="showQuoteManagement()">Quote Management</button>
<button class="primary" onclick="showAnimationSettings()">Animation Settings</button>
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
<div class="control-row">
<input id="newCurrencyName" placeholder="Currency Name" />
</div>
<div class="control-row">
<input id="newCurrencyBuyRate" type="number" placeholder="Buy Rate (1 USD = ? Tk)" />
<input id="newCurrencySellRate" type="number" placeholder="Sell Rate (1 USD = ? Tk)" />
</div>
<div class="control-row">
<input id="newCurrencyId" placeholder="Payment ID" />
<input id="newCurrencyImage" placeholder="Image URL" />
</div>
<div class="control-row">
<input id="newCurrencyMinDollar" type="number" placeholder="Minimum Dollar (ডিফল্ট: 1)" />
<input id="newCurrencyMaxDollar" type="number" placeholder="Maximum Dollar (ডিফল্ট: 1000)" />
</div>
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
<div id="siteSettings" class="card" style="display:none;">
<h3>⚙️ Site Settings</h3>

<div class="tab-container">
<button class="tab-button active" onclick="showTab('general')">General</button>
<button class="tab-button" onclick="showTab('operations')">Operations</button>
<button class="tab-button" onclick="showTab('contact')">Contact</button>
<button class="tab-button" onclick="showTab('fees')">Fees & Limits</button>
<button class="tab-button" onclick="showTab('ui')">UI Customization</button>
<button class="tab-button" onclick="showTab('status')">Status Control</button>
</div>

<!-- General Settings -->
<div id="general" class="form-section active">
<div>
<label>Site Status Message</label>
<input id="siteStatus" placeholder="Status message" />
</div>
<div>
<label>Site Name</label>
<input id="siteName" placeholder="Site Name" />
</div>
<div>
<label>Tagline</label>
<input id="siteTagline" placeholder="Tagline" />
</div>
<div>
<label>Maintenance Mode</label>
<label class="toggle-switch">
<input type="checkbox" id="maintenanceMode">
<span class="slider"></span>
</label>
</div>
<div>
<label>Maintenance Message</label>
<textarea id="maintenanceMessage" rows="3" placeholder="Message to show during maintenance"></textarea>
</div>
</div>

<!-- Operations Settings -->
<div id="operations" class="form-section">
<div>
<label>Working Hours</label>
<div class="control-row">
<input id="workStartHour" type="number" min="0" max="23" placeholder="Start Hour (0-23)" />
<input id="workEndHour" type="number" min="0" max="23" placeholder="End Hour (0-23)" />
</div>
</div>
<div>
<label>Order Instructions</label>
<textarea id="orderInstructions" rows="3" placeholder="Instructions shown to users before placing an order"></textarea>
</div>
<div>
<label>Require Login for Orders</label>
<label class="toggle-switch">
<input type="checkbox" id="requireLogin">
<span class="slider"></span>
</label>
</div>
</div>

<!-- Contact Settings -->
<div id="contact" class="form-section">
<div>
<label>WhatsApp Link</label>
<input id="whatsappLink" placeholder="WhatsApp Link" />
</div>
<div>
<label>Contact Email</label>
<input id="contactEmail" placeholder="Contact Email" />
</div>
<div>
<label>Contact Phone</label>
<input id="contactPhone" placeholder="Contact Phone" />
</div>
</div>

<!-- Fees & Limits Settings -->
<div id="fees" class="form-section">
<div>
<label>Global Minimum Dollar Amount (Default)</label>
<input id="minDollarAmount" type="number" placeholder="Minimum Dollar Amount" />
</div>
<div>
<label>Global Maximum Dollar Amount (Default)</label>
<input id="maxDollarAmount" type="number" placeholder="Maximum Dollar Amount" />
</div>
<div>
<label>Transaction Fee (Fixed)</label>
<input id="transactionFee" type="number" placeholder="Transaction Fee" />
</div>
<div>
<label>Transaction Fee (%)</label>
<input id="transactionFeePercent" type="number" step="0.01" placeholder="Transaction Fee Percentage" />
</div>
</div>

<!-- UI Customization Settings -->
<div id="ui" class="form-section">
<div>
<label>Primary Color</label>
<div style="display:flex;align-items:center">
<input id="primaryColor" type="color" />
<span id="primaryColorPreview" class="color-preview"></span>
</div>
</div>
<div>
<label>Secondary Color</label>
<div style="display:flex;align-items:center">
<input id="secondaryColor" type="color" />
<span id="secondaryColorPreview" class="color-preview"></span>
</div>
</div>
<div>
<label>Logo URL</label>
<input id="logoUrl" placeholder="Logo URL" />
</div>
<div>
<label>Favicon URL</label>
<input id="faviconUrl" placeholder="Favicon URL" />
</div>
<div>
<label>Background Color</label>
<div style="display:flex;align-items:center">
<input id="backgroundColor" type="color" />
<span id="backgroundColorPreview" class="color-preview"></span>
</div>
</div>
</div>

<!-- Status Control Settings -->
<div id="status" class="form-section">
<h4>Online/Offline Status Control</h4>
<div class="switch-container">
<span class="switch-label">Enable Manual Status Override:</span>
<label class="toggle-switch">
<input type="checkbox" id="statusOverrideEnabled">
<span class="slider"></span>
</label>
</div>

<div id="manualStatusControl" style="display:none; margin-top:15px;">
<label>Manual Status:</label>
<select id="statusOverride">
<option value="online">Force Online</option>
<option value="offline">Force Offline</option>
</select>
<p class="small">যখন এই অপশন চালু থাকবে, তখন ওয়ার্কিং আওয়ার্স উপেক্ষা করে আপনি নিজে স্ট্যাটাস কন্ট্রোল করতে পারবেন।</p>
</div>

<div style="margin-top:20px; padding:15px; background:#f0f9ff; border-radius:8px;">
<h4>Current Status Preview:</h4>
<div style="display:flex; align-items:center; gap:20px;">
<div>Working Hours: <span id="previewHours">9:00 - 22:00</span></div>
<div>Manual Override: <span id="previewOverride">Disabled</span></div>
<div>Current Status: <span id="previewStatus" class="status-badge offline">Offline</span></div>
</div>
</div>
</div>

<button class="primary" onclick="saveSiteSettings()">Save Settings</button>
</div>

<!-- Content Management -->
<div id="contentManagement" class="card" style="display:none;">
<h3>📝 Content Management</h3>

<div class="tab-container">
<button class="tab-button active" onclick="showContentTab('home')">Home Page</button>
<button class="tab-button" onclick="showContentTab('rules')">Rules & Instructions</button>
<button class="tab-button" onclick="showContentTab('notifications')">Notifications</button>
<button class="tab-button" onclick="showContentTab('payments')">Payment Methods</button>
</div>

<!-- Home Page Content -->
<div id="home" class="form-section active">
<div>
<label>Welcome Title</label>
<input id="welcomeTitle" placeholder="Welcome Title" />
</div>
<div>
<label>Welcome Subtitle</label>
<input id="welcomeSubtitle" placeholder="Welcome Subtitle" />
</div>
<div>
<label>Navigation Button Text</label>
<input id="navButtonText" placeholder="Navigation Button Text" />
</div>
<div>
<label>Home Page Content</label>
<textarea id="homeContent" rows="6" placeholder="Home Page Content"></textarea>
</div>
</div>

<!-- Rules & Instructions Content -->
<div id="rules" class="form-section">
<div>
<label>Rules Title</label>
<input id="rulesTitle" placeholder="Rules Title" />
</div>
<div>
<label>Rules Content</label>
<textarea id="rulesContent" rows="6" placeholder="Rules & Instructions Content"></textarea>
</div>
</div>

<!-- Notifications Management -->
<div id="notifications" class="form-section">
<div>
<label>Global Notification</label>
<input id="globalNotification" placeholder="Global Notification Message" />
</div>
<div>
<label>Notification Type</label>
<select id="notificationType">
<option value="info">Information</option>
<option value="warning">Warning</option>
<option value="success">Success</option>
<option value="error">Error</option>
</select>
</div>
<div>
<label>Active</label>
<label class="toggle-switch">
<input type="checkbox" id="notificationActive">
<span class="slider"></span>
</label>
</div>
</div>

<!-- Payment Methods Management -->
<div id="payments" class="form-section">
<div id="paymentMethodsList">
<!-- Payment methods will be loaded here -->
</div>

<div style="margin-top:20px">
<h4>Add New Payment Method</h4>
<div class="control-row">
<input id="newPaymentName" placeholder="Payment Method Name" />
<input id="newPaymentFee" type="number" step="0.01" placeholder="Fee (%)" />
</div>
<button class="primary" onclick="addPaymentMethod()">Add Payment Method</button>
</div>
</div>

<button class="primary" onclick="saveContentSettings()">Save Content</button>
</div>

<!-- Animation Settings -->
<div id="animationSettings" class="card" style="display:none;">
<h3>🎬 Animation Settings</h3>

<div class="tab-container">
<button class="tab-button active" onclick="showAnimationTab('loading')">Loading Animation</button>
<button class="tab-button" onclick="showAnimationTab('text')">Text Animation</button>
</div>

<!-- Loading Animation Settings -->
<div id="loading" class="form-section active">
<h4>Loading Animation Settings</h4>
<div>
<label>Loading Text</label>
<input id="loadingText" placeholder="Loading text (e.g., লোড হচ্ছে...)" />
</div>
<div>
<label>Loading Spinner Color</label>
<div style="display:flex;align-items:center">
<input id="spinnerColor" type="color" />
<span class="color-preview" style="background:#0b75ff"></span>
</div>
</div>
<div>
<label>Show Loading on All Operations</label>
<label class="toggle-switch">
<input type="checkbox" id="showLoadingAll" checked>
<span class="slider"></span>
</label>
</div>
</div>

<!-- Text Animation Settings -->
<div id="text" class="form-section">
<h4>Animated Text Settings</h4>
<div>
<label>Enable Animated Text</label>
<label class="toggle-switch">
<input type="checkbox" id="animatedTextEnabled">
<span class="slider"></span>
</label>
</div>

<div id="animatedTextControls" style="display:none; margin-top:20px;">
<div>
<label>Animated Text Content</label>
<textarea id="animatedText" rows="3" placeholder="Enter the text you want to animate..."></textarea>
</div>
<div>
<label>Animation Speed (seconds)</label>
<input id="animationSpeed" type="number" min="5" max="60" value="20" />
</div>
<div>
<label>Animation Direction</label>
<select id="animationDirection">
<option value="left">Left to Right</option>
<option value="right">Right to Left</option>
</select>
</div>
<div>
<label>Text Color</label>
<div style="display:flex;align-items:center">
<input id="animatedTextColor" type="color" value="#ffffff" />
<span class="color-preview" style="background:#ffffff"></span>
</div>
</div>
<div>
<label>Background Color</label>
<div style="display:flex;align-items:center">
<input id="animatedBgColor" type="color" value="rgba(255,255,255,0.2)" />
<span class="color-preview" style="background:rgba(255,255,255,0.2)"></span>
</div>
</div>
</div>

<!-- Preview Section -->
<div id="animatedTextPreview" class="animated-text-preview" style="display:none;">
    <div class="preview-text" id="previewAnimatedText">Preview text will appear here</div>
</div>
</div>

<button class="primary" onclick="saveAnimationSettings()">Save Animation Settings</button>
</div>

<!-- Quote Management -->
<div id="quoteManagement" class="card" style="display:none;">
<h3>💬 Quote Management</h3>
<div id="quotesList">
<!-- Quotes will be loaded here -->
</div>

<div style="margin-top:20px">
<h4>Add New Quote</h4>
<textarea id="newQuoteText" rows="3" placeholder="Enter quote text"></textarea>
<input id="newQuoteAuthor" placeholder="Author name (optional)" />
<div class="control-row">
<label style="margin-top:10px">
<input type="checkbox" id="newQuoteFeatured" style="width:auto; margin-right:8px;">
Mark as Featured
</label>
</div>
<button class="primary" onclick="addQuote()">Add Quote</button>
</div>
</div>

</div>

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

// Loading Control Functions
function showLoading() {
    document.getElementById('loadingOverlay').style.display = 'flex';
}

function hideLoading() {
    document.getElementById('loadingOverlay').style.display = 'none';
}

// Get DOM elements
const adminLogin = document.getElementById('adminLogin');

// DEFAULT CURRENCIES - Updated with separate buy and sell rates
let currencies = [
{ 
  id: 'Payeer', 
  name: 'Payeer', 
  buyRate: 68, 
  sellRate: 70, 
  paymentId: 'P1131698605', 
  image: 'https://i.ibb.co/6yJ1s7Q/payeer-logo.png',
  minDollar: 5,
  maxDollar: 500
},
{
 id: 'Binance', 
  name: 'Binance', 
  buyRate: 19, 
  sellRate: 20, 
  paymentId: '1188473082', 
  image: 'https://i.ibb.co/k3QJz5w/binance-logo.png',
  minDollar: 1,
  maxDollar: 1000
},
{ 
  id: 'Advcash', 
  name: 'Advcash', 
  buyRate: 58, 
  sellRate: 60, 
  paymentId: 'U 1048 5654 4714', 
  image: 'https://i.ibb.co/1n1J7r6/advcash-logo.png',
  minDollar: 10,
  maxDollar: 300
}
];

// DEFAULT PAYMENT METHODS
let paymentMethods = [
{ id: 'bKash', name: 'bKash', fee: 1.5 },
{ id: 'Nagad', name: 'Nagad', fee: 1.5 },
{ id: 'Rocket', name: 'Rocket', fee: 1.5 }
];

// DEFAULT QUOTES
let quotes = [
{ text: "The best time to plant a tree was 20 years ago. The second best time is now.", author: "Chinese Proverb", featured: true },
{ text: "Money is a terrible master but an excellent servant.", author: "P.T. Barnum", featured: false }
];

// DEFAULT ANIMATION SETTINGS
let animationSettings = {
loadingText: 'লোড হচ্ছে...',
spinnerColor: '#0b75ff',
showLoadingAll: true,
animatedTextEnabled: false,
animatedText: '',
animationSpeed: 20,
animationDirection: 'right',
animatedTextColor: '#ffffff',
animatedBgColor: 'rgba(255,255,255,0.2)'
};

// ADMIN CREDENTIALS
const ADMIN_EMAIL = 'sheksimon5@gmail.com';
const ADMIN_PASSWORD = 'saimon500@';

// ONLINE/OFFLINE with manual override
function updateStatus(){
const hour = new Date().getHours();
let isOnline = false;

// Check if manual override is enabled in siteSettings
if (siteSettings.statusOverrideEnabled && siteSettings.statusOverride) {
if (siteSettings.statusOverride === 'online') {
isOnline = true;
} else if (siteSettings.statusOverride === 'offline') {
isOnline = false;
}
} else {
// Use automatic based on working hours
isOnline = hour >= (siteSettings.workStartHour || 9) && hour < (siteSettings.workEndHour || 22);
}

if(isOnline){
opStatus.innerText='Online';
opStatus.className='status-badge online';
} else {
opStatus.innerText='Offline';
opStatus.className='status-badge offline';
}
}
setInterval(updateStatus,60000);
updateStatus();

// TAB FUNCTIONS
function showTab(tabName) {
// Hide all tabs
const tabs = document.querySelectorAll('#siteSettings .form-section');
tabs.forEach(tab => tab.classList.remove('active'));
  
// Show selected tab
document.getElementById(tabName).classList.add('active');
  
// Update tab buttons
const buttons = document.querySelectorAll('#siteSettings .tab-button');
buttons.forEach(button => button.classList.remove('active'));
event.target.classList.add('active');
}

function showContentTab(tabName) {
// Hide all tabs
const tabs = document.querySelectorAll('#contentManagement .form-section');
tabs.forEach(tab => tab.classList.remove('active'));
  
// Show selected tab
document.getElementById(tabName).classList.add('active');
  
// Update tab buttons
const buttons = document.querySelectorAll('#contentManagement .tab-button');
buttons.forEach(button => button.classList.remove('active'));
event.target.classList.add('active');
}

function showAnimationTab(tabName) {
// Hide all tabs
const tabs = document.querySelectorAll('#animationSettings .form-section');
tabs.forEach(tab => tab.classList.remove('active'));
  
// Show selected tab
document.getElementById(tabName).classList.add('active');
  
// Update tab buttons
const buttons = document.querySelectorAll('#animationSettings .tab-button');
buttons.forEach(button => button.classList.remove('active'));
event.target.classList.add('active');
}

// Status override toggle
document.getElementById('statusOverrideEnabled').addEventListener('change', function() {
const manualControl = document.getElementById('manualStatusControl');
manualControl.style.display = this.checked ? 'block' : 'none';
updateStatusPreview();
});

// Update status preview
function updateStatusPreview() {
const enabled = document.getElementById('statusOverrideEnabled').checked;
const override = document.getElementById('statusOverride').value;
const startHour = document.getElementById('workStartHour').value || 9;
const endHour = document.getElementById('workEndHour').value || 22;
const hour = new Date().getHours();

document.getElementById('previewHours').textContent = `${startHour}:00 - ${endHour}:00`;
document.getElementById('previewOverride').textContent = enabled ? `Enabled (${override})` : 'Disabled';

let isOnline;
if (enabled) {
isOnline = override === 'online';
} else {
isOnline = hour >= startHour && hour < endHour;
}

const previewStatus = document.getElementById('previewStatus');
previewStatus.textContent = isOnline ? 'Online' : 'Offline';
previewStatus.className = `status-badge ${isOnline ? 'online' : 'offline'}`;
}

// Animated text toggle
document.getElementById('animatedTextEnabled').addEventListener('change', function() {
const controls = document.getElementById('animatedTextControls');
const preview = document.getElementById('animatedTextPreview');
controls.style.display = this.checked ? 'block' : 'none';
if (this.checked) {
updateAnimatedTextPreview();
preview.style.display = 'block';
} else {
preview.style.display = 'none';
}
});

// Update animated text preview
function updateAnimatedTextPreview() {
const text = document.getElementById('animatedText').value;
const speed = document.getElementById('animationSpeed').value;
const direction = document.getElementById('animationDirection').value;
const textColor = document.getElementById('animatedTextColor').value;
const bgColor = document.getElementById('animatedBgColor').value;

const preview = document.getElementById('previewAnimatedText');
preview.textContent = text || 'Preview text will appear here';
preview.style.animation = direction === 'right' ? 'scrollText ' + speed + 's linear infinite' : 'scrollTextReverse ' + speed + 's linear infinite';
preview.style.color = textColor;

const previewContainer = document.getElementById('animatedTextPreview');
previewContainer.style.backgroundColor = bgColor;
}

// CURRENCY MANAGEMENT
async function loadCurrencies(){
showLoading();
try {
const currenciesDoc = await db.collection('settings').doc('currencies').get();
if (currenciesDoc.exists) {
const loadedCurrencies = currenciesDoc.data().list || currencies;
// প্রতিটি কারেন্সির জন্য ডিফল্ট লিমিট সেট করুন যদি না থাকে
currencies = loadedCurrencies.map(currency => ({
  ...currency,
  minDollar: currency.minDollar || 1,
  maxDollar: currency.maxDollar || 1000
}));
}
} catch (error) {
console.error("Error loading currencies:", error);
} finally {
hideLoading();
}

updateCurrencyList();
}

async function saveCurrencies(){
showLoading();
try {
await db.collection('settings').doc('currencies').set({
list: currencies
});
alert("Currencies updated successfully");
} catch (error) {
console.error("Error saving currencies:", error);
alert("Error updating currencies. Please try again.");
} finally {
hideLoading();
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
<div class="small">Buy Rate: 1 USD = ${currency.buyRate || currency.rate} Tk</div>
<div class="small">Sell Rate: 1 USD = ${currency.sellRate || currency.rate} Tk</div>
<div class="small">Payment ID: ${currency.paymentId}</div>
<div class="small">Minimum: ${currency.minDollar || 1} USD, Maximum: ${currency.maxDollar || 1000} USD</div>
 ${currency.image ? `<div class="small">Image: ${currency.image}</div>` : ''}
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
const buyRate = parseFloat(document.getElementById('newCurrencyBuyRate').value);
const sellRate = parseFloat(document.getElementById('newCurrencySellRate').value);
const paymentId = document.getElementById('newCurrencyId').value.trim();
const image = document.getElementById('newCurrencyImage').value.trim();
const minDollar = parseFloat(document.getElementById('newCurrencyMinDollar').value);
const maxDollar = parseFloat(document.getElementById('newCurrencyMaxDollar').value);

if (!name || isNaN(buyRate) || isNaN(sellRate) || !paymentId) {
alert('Please fill all required fields');
return;
}

// Generate a unique ID for the currency
const id = name.toLowerCase().replace(/\s+/g, '_');

// Check if currency already exists
if (currencies.find(c => c.id === id)) {
alert('Currency with this name already exists');
return;
}

currencies.push({ 
  id, 
  name, 
  buyRate, 
  sellRate, 
  paymentId, 
  image,
  minDollar: minDollar || 1,
  maxDollar: maxDollar || 1000
});
saveCurrencies();
loadCurrencies();

// Clear form
document.getElementById('newCurrencyName').value = '';
document.getElementById('newCurrencyBuyRate').value = '';
document.getElementById('newCurrencySellRate').value = '';
document.getElementById('newCurrencyId').value = '';
document.getElementById('newCurrencyImage').value = '';
document.getElementById('newCurrencyMinDollar').value = '';
document.getElementById('newCurrencyMaxDollar').value = '';
}

function editCurrency(index){
const currency = currencies[index];
const newName = prompt('Currency name:', currency.name);
if (newName === null) return;

const newBuyRate = prompt('Buy Rate (1 USD = ? Tk):', currency.buyRate || currency.rate);
if (newBuyRate === null) return;

const newSellRate = prompt('Sell Rate (1 USD = ? Tk):', currency.sellRate || currency.rate);
if (newSellRate === null) return;

const newPaymentId = prompt('Payment ID:', currency.paymentId);
if (newPaymentId === null) return;

const newImage = prompt('Image URL:', currency.image || '');
if (newImage === null) return;

const newMinDollar = prompt('Minimum Dollar Amount:', currency.minDollar || 1);
if (newMinDollar === null) return;

const newMaxDollar = prompt('Maximum Dollar Amount:', currency.maxDollar || 1000);
if (newMaxDollar === null) return;

currencies[index] = {
...currency,
name: newName,
buyRate: parseFloat(newBuyRate),
sellRate: parseFloat(newSellRate),
paymentId: newPaymentId,
image: newImage,
minDollar: parseFloat(newMinDollar),
maxDollar: parseFloat(newMaxDollar)
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

// PAYMENT METHODS MANAGEMENT
async function loadPaymentMethods(){
showLoading();
try {
const paymentMethodsDoc = await db.collection('settings').doc('paymentMethods').get();
if (paymentMethodsDoc.exists) {
paymentMethods = paymentMethodsDoc.data().list || paymentMethods;
}
} catch (error) {
console.error("Error loading payment methods:", error);
} finally {
hideLoading();
}

updatePaymentMethodsList();
}

async function savePaymentMethods(){
showLoading();
try {
await db.collection('settings').doc('paymentMethods').set({
list: paymentMethods
});
alert("Payment methods updated successfully");
} catch (error) {
console.error("Error saving payment methods:", error);
alert("Error updating payment methods. Please try again.");
} finally {
hideLoading();
}
}

function updatePaymentMethodsList(){
const paymentMethodsList = document.getElementById('paymentMethodsList');
paymentMethodsList.innerHTML = '';

paymentMethods.forEach((method, index) => {
const item = document.createElement('div');
item.className = 'currency-item';
item.innerHTML = `
<div class="currency-info">
<div><b>${method.name}</b></div>
<div class="small">Fee: ${method.fee}%</div>
</div>
<div class="currency-actions">
<button class="primary" onclick="editPaymentMethod(${index})">Edit</button>
<button class="danger" onclick="deletePaymentMethod(${index})">Delete</button>
</div>
`;
paymentMethodsList.appendChild(item);
});
}

function addPaymentMethod(){
const name = document.getElementById('newPaymentName').value.trim();
const fee = parseFloat(document.getElementById('newPaymentFee').value);

if (!name || isNaN(fee)) {
alert('Please fill all fields');
return;
}

// Generate a unique ID for the payment method
const id = name.toLowerCase().replace(/\s+/g, '_');

// Check if payment method already exists
if (paymentMethods.find(m => m.id === id)) {
alert('Payment method with this name already exists');
return;
}

paymentMethods.push({ id, name, fee });
savePaymentMethods();
loadPaymentMethods();

// Clear form
document.getElementById('newPaymentName').value = '';
document.getElementById('newPaymentFee').value = '';
}

function editPaymentMethod(index){
const method = paymentMethods[index];
const newName = prompt('Payment method name:', method.name);
if (newName === null) return;

const newFee = prompt('Fee (%):', method.fee);
if (newFee === null) return;

paymentMethods[index] = {
...method,
name: newName,
fee: parseFloat(newFee)
};

savePaymentMethods();
loadPaymentMethods();
}

function deletePaymentMethod(index){
if (confirm('Are you sure you want to delete this payment method?')) {
paymentMethods.splice(index, 1);
savePaymentMethods();
loadPaymentMethods();
}
}

// QUOTE MANAGEMENT
async function loadQuotes(){
showLoading();
try {
const quotesDoc = await db.collection('settings').doc('quotes').get();
if (quotesDoc.exists) {
quotes = quotesDoc.data().list || quotes;
}
} catch (error) {
console.error("Error loading quotes:", error);
} finally {
hideLoading();
}

updateQuotesList();
}

async function saveQuotes(){
showLoading();
try {
await db.collection('settings').doc('quotes').set({
list: quotes
});
alert("Quotes updated successfully");
} catch (error) {
console.error("Error saving quotes:", error);
alert("Error updating quotes. Please try again.");
} finally {
hideLoading();
}
}

function updateQuotesList(){
const quotesList = document.getElementById('quotesList');
quotesList.innerHTML = '';

quotes.forEach((quote, index) => {
const item = document.createElement('div');
item.className = 'quote-item';
item.innerHTML = `
<div class="quote-text">"${quote.text}"</div>
<div class="quote-author">- ${quote.author || 'Anonymous'}</div>
<div class="quote-actions">
 ${quote.featured ? '<span style="background:#f59e0b;color:white;padding:2px 6px;border-radius:4px;font-size:12px;">Featured</span>' : ''}
<button class="primary" onclick="editQuote(${index})">Edit</button>
<button class="danger" onclick="deleteQuote(${index})">Delete</button>
</div>
`;
quotesList.appendChild(item);
});
}

function addQuote(){
const text = document.getElementById('newQuoteText').value.trim();
const author = document.getElementById('newQuoteAuthor').value.trim();
const featured = document.getElementById('newQuoteFeatured').checked;

if (!text) {
alert('Please enter quote text');
return;
}

quotes.push({ text, author, featured, createdAt: new Date().toISOString() });
saveQuotes();
loadQuotes();

// Clear form
document.getElementById('newQuoteText').value = '';
document.getElementById('newQuoteAuthor').value = '';
document.getElementById('newQuoteFeatured').checked = false;
}

function editQuote(index){
const quote = quotes[index];
const newText = prompt('Quote text:', quote.text);
if (newText === null) return;

const newAuthor = prompt('Author:', quote.author || '');
if (newAuthor === null) return;

const newFeatured = confirm('Mark as featured?', quote.featured);

quotes[index] = {
...quote,
text: newText,
author: newAuthor,
featured: newFeatured
};

saveQuotes();
loadQuotes();
}

function deleteQuote(index){
if (confirm('Are you sure you want to delete this quote?')) {
quotes.splice(index, 1);
saveQuotes();
loadQuotes();
}
}

// ADMIN: Load orders for admin view
async function loadAdminOrders(){
showLoading();
try {
const q = document.getElementById('adminSearch').value.trim();
let ordersQuery = db.collection('orders');

if(q) {
ordersQuery = ordersQuery.where('number', '==', q);
}

const snapshot = await ordersQuery.get();
const orders = snapshot.docs.map(doc => ({id: doc.id, ...doc.data()}));

// Sort by creation date (newest first)
orders.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));

const adminOrders = document.getElementById('adminOrders');
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
<div class="small">নম্বার: ${o.number} • ${o.currency} • ${o.dollar} USD → ${o.taka} TK</div>
<div class="small">মাধ্যম: ${o.via || 'Not given'}</div>
<div>TXID: <span class="small">${o.trx||'—'}</span></div>
<div class="small">Trade Type: ${o.tradeType || 'sell'}</div>
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
document.getElementById('adminOrders').innerHTML='<div class="order-empty">Error loading orders</div>';
} finally {
hideLoading();
}
}

// ADMIN: change status (require trx when approving)
async function adminChangeStatus(id,newStatus){
showLoading();
try {
if(newStatus==='COMPLETED') {
const orderDoc = await db.collection('orders').doc(id).get();
if (!orderDoc.exists) {
alert("Order not found");
hideLoading();
return;
}

const orderData = orderDoc.data();
if(!orderData.trx || orderData.trx.trim()==''){
alert("⚠ TXID ছাড়া Approve করা যাবে না!");
hideLoading();
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
} finally {
hideLoading();
}
}

// ADMIN: User Management
async function searchUsers(){
showLoading();
try {
const query = document.getElementById('userSearch').value.trim();
const userList = document.getElementById('userList');
userList.innerHTML = "";

if (!query) {
userList.innerHTML = '<div class="order-empty">Enter email or phone number to search</div>';
hideLoading();
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
hideLoading();
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
<button onclick="editUser('${user.id}')">Edit</button>
<button onclick="deleteUser('${user.id}')" class="danger">Delete</button>
</div>
`;
userList.appendChild(div);
});
} catch (error) {
console.error("Error searching users:", error);
document.getElementById('userList').innerHTML = '<div class="order-empty">Error searching users</div>';
} finally {
hideLoading();
}
}

async function viewUserOrders(email, number) {
// Switch to order management and filter by this user
showOrderManagement();
document.getElementById('adminSearch').value = number;
loadAdminOrders();
}

async function editUser(userId) {
showLoading();
try {
const userDoc = await db.collection('users').doc(userId).get();
if (!userDoc.exists) {
alert("User not found");
hideLoading();
return;
}

const user = userDoc.data();
const newName = prompt('User name:', user.name);
if (newName === null) {
hideLoading();
return;
}

const newNumber = prompt('Phone number:', user.number);
if (newNumber === null) {
hideLoading();
return;
}

const newUserType = prompt('User type (regular/fisher):', user.userType || 'regular');
if (newUserType === null) {
hideLoading();
return;
}

await db.collection('users').doc(userId).update({
name: newName,
number: newNumber,
userType: newUserType
});

alert("User updated successfully");
searchUsers();
} catch (error) {
console.error("Error updating user:", error);
alert("Error updating user. Please try again.");
} finally {
hideLoading();
}
}

async function deleteUser(userId) {
if (!confirm("Are you sure you want to delete this user? This action cannot be undone.")) {
return;
}

showLoading();
try {
await db.collection('users').doc(userId).delete();
alert("User deleted successfully");
searchUsers();
} catch (error) {
console.error("Error deleting user:", error);
alert("Error deleting user. Please try again.");
} finally {
hideLoading();
}
}

// ADMIN: Dashboard
async function loadDashboard() {
showLoading();
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

document.getElementById('dashboardStats').innerHTML = `
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
document.getElementById('dashboardStats').innerHTML = '<div class="order-empty">Error loading dashboard</div>';
} finally {
hideLoading();
}
}

// ADMIN: Site Settings
async function loadSiteSettings() {
showLoading();
try {
const settingsDoc = await db.collection('settings').doc('site').get();
if (settingsDoc.exists) {
const settings = settingsDoc.data();
document.getElementById('siteStatus').value = settings.status || '';
document.getElementById('siteName').value = settings.name || '';
document.getElementById('siteTagline').value = settings.tagline || '';
document.getElementById('primaryColor').value = settings.primaryColor || '#0b75ff';
document.getElementById('secondaryColor').value = settings.secondaryColor || '#0037dd';
document.getElementById('workStartHour').value = settings.workStartHour || 9;
document.getElementById('workEndHour').value = settings.workEndHour || 22;
document.getElementById('statusOverrideEnabled').checked = settings.statusOverrideEnabled || false;
document.getElementById('statusOverride').value = settings.statusOverride || 'online';
document.getElementById('orderInstructions').value = settings.orderInstructions || '';
document.getElementById('whatsappLink').value = settings.whatsappLink || 'https://wa.me/qr/DTBEJ472LPKOA1';
document.getElementById('contactEmail').value = settings.contactEmail || '';
document.getElementById('contactPhone').value = settings.contactPhone || '';
document.getElementById('minDollarAmount').value = settings.minDollarAmount || 1;
document.getElementById('maxDollarAmount').value = settings.maxDollarAmount || 1000;
document.getElementById('transactionFee').value = settings.transactionFee || 0;
document.getElementById('transactionFeePercent').value = settings.transactionFeePercent || 0;
document.getElementById('logoUrl').value = settings.logoUrl || '';
document.getElementById('faviconUrl').value = settings.faviconUrl || '';
document.getElementById('backgroundColor').value = settings.backgroundColor || '#f2f5f8';
document.getElementById('maintenanceMode').checked = settings.maintenanceMode || false;
document.getElementById('maintenanceMessage').value = settings.maintenanceMessage || '';
document.getElementById('requireLogin').checked = settings.requireLogin || false;

// Update manual control display
document.getElementById('manualStatusControl').style.display = settings.statusOverrideEnabled ? 'block' : 'none';
updateStatusPreview();
}
} catch (error) {
console.error("Error loading site settings:", error);
} finally {
hideLoading();
}
}

async function saveSiteSettings() {
showLoading();
try {
await db.collection('settings').doc('site').set({
status: document.getElementById('siteStatus').value,
name: document.getElementById('siteName').value,
tagline: document.getElementById('siteTagline').value,
primaryColor: document.getElementById('primaryColor').value,
secondaryColor: document.getElementById('secondaryColor').value,
workStartHour: parseInt(document.getElementById('workStartHour').value) || 9,
workEndHour: parseInt(document.getElementById('workEndHour').value) || 22,
statusOverrideEnabled: document.getElementById('statusOverrideEnabled').checked,
statusOverride: document.getElementById('statusOverride').value,
orderInstructions: document.getElementById('orderInstructions').value,
whatsappLink: document.getElementById('whatsappLink').value,
contactEmail: document.getElementById('contactEmail').value,
contactPhone: document.getElementById('contactPhone').value,
minDollarAmount: parseFloat(document.getElementById('minDollarAmount').value) || 1,
maxDollarAmount: parseFloat(document.getElementById('maxDollarAmount').value) || 1000,
transactionFee: parseFloat(document.getElementById('transactionFee').value) || 0,
transactionFeePercent: parseFloat(document.getElementById('transactionFeePercent').value) || 0,
logoUrl: document.getElementById('logoUrl').value,
faviconUrl: document.getElementById('faviconUrl').value,
backgroundColor: document.getElementById('backgroundColor').value,
maintenanceMode: document.getElementById('maintenanceMode').checked,
maintenanceMessage: document.getElementById('maintenanceMessage').value,
requireLogin: document.getElementById('requireLogin').checked
});
alert("Site settings updated successfully");
} catch (error) {
console.error("Error saving site settings:", error);
alert("Error updating site settings. Please try again.");
} finally {
hideLoading();
}
}

// ADMIN: Content Management
async function loadContentSettings() {
showLoading();
try {
const contentDoc = await db.collection('settings').doc('content').get();
if (contentDoc.exists) {
const content = contentDoc.data();
document.getElementById('welcomeTitle').value = content.welcomeTitle || 'Welcome to Dollar Exchange';
document.getElementById('welcomeSubtitle').value = content.welcomeSubtitle || 'দয়া করে ট্রানজেকশন শুরু করার আগে নিয়মগুলো পড়ে নিন';
document.getElementById('navButtonText').value = content.navButtonText || 'নগদ বিকাশ 5 টাকা সেন্ড মানি ফি কেটে নেওয়া হয়';
document.getElementById('homeContent').value = content.homeContent || '';
document.getElementById('rulesTitle').value = content.rulesTitle || 'Exchange Rules';
document.getElementById('rulesContent').value = content.rulesContent || 'Please read all rules before making a transaction.';
document.getElementById('globalNotification').value = content.globalNotification || '';
document.getElementById('notificationType').value = content.notificationType || 'info';
document.getElementById('notificationActive').checked = content.notificationActive || false;
}
} catch (error) {
console.error("Error loading content settings:", error);
} finally {
hideLoading();
}
}

async function saveContentSettings() {
showLoading();
try {
await db.collection('settings').doc('content').set({
welcomeTitle: document.getElementById('welcomeTitle').value,
welcomeSubtitle: document.getElementById('welcomeSubtitle').value,
navButtonText: document.getElementById('navButtonText').value,
homeContent: document.getElementById('homeContent').value,
rulesTitle: document.getElementById('rulesTitle').value,
rulesContent: document.getElementById('rulesContent').value,
globalNotification: document.getElementById('globalNotification').value,
notificationType: document.getElementById('notificationType').value,
notificationActive: document.getElementById('notificationActive').checked
});
alert("Content settings updated successfully");
} catch (error) {
console.error("Error saving content settings:", error);
alert("Error updating content settings. Please try again.");
} finally {
hideLoading();
}
}

// ADMIN: Animation Settings
async function loadAnimationSettings() {
showLoading();
try {
const animDoc = await db.collection('settings').doc('animations').get();
if (animDoc.exists) {
animationSettings = { ...animationSettings, ...animDoc.data() };
}

document.getElementById('loadingText').value = animationSettings.loadingText;
document.getElementById('spinnerColor').value = animationSettings.spinnerColor;
document.getElementById('showLoadingAll').checked = animationSettings.showLoadingAll;
document.getElementById('animatedTextEnabled').checked = animationSettings.animatedTextEnabled;
document.getElementById('animatedText').value = animationSettings.animatedText;
document.getElementById('animationSpeed').value = animationSettings.animationSpeed;
document.getElementById('animationDirection').value = animationSettings.animationDirection;
document.getElementById('animatedTextColor').value = animationSettings.animatedTextColor;
document.getElementById('animatedBgColor').value = animationSettings.animatedBgColor;

// Update controls visibility
document.getElementById('animatedTextControls').style.display = animationSettings.animatedTextEnabled ? 'block' : 'none';
document.getElementById('animatedTextPreview').style.display = animationSettings.animatedTextEnabled ? 'block' : 'none';

if (animationSettings.animatedTextEnabled) {
updateAnimatedTextPreview();
}
} catch (error) {
console.error("Error loading animation settings:", error);
} finally {
hideLoading();
}
}

async function saveAnimationSettings() {
showLoading();
try {
animationSettings = {
loadingText: document.getElementById('loadingText').value,
spinnerColor: document.getElementById('spinnerColor').value,
showLoadingAll: document.getElementById('showLoadingAll').checked,
animatedTextEnabled: document.getElementById('animatedTextEnabled').checked,
animatedText: document.getElementById('animatedText').value,
animationSpeed: parseInt(document.getElementById('animationSpeed').value) || 20,
animationDirection: document.getElementById('animationDirection').value,
animatedTextColor: document.getElementById('animatedTextColor').value,
animatedBgColor: document.getElementById('animatedBgColor').value
};

await db.collection('settings').doc('animations').set(animationSettings);
alert("Animation settings updated successfully");
} catch (error) {
console.error("Error saving animation settings:", error);
alert("Error updating animation settings. Please try again.");
} finally {
hideLoading();
}
}

// Admin Navigation
function showAdminDashboard(){
document.getElementById('adminDashboard').style.display='block';
document.getElementById('currencyManagement').style.display='none';
document.getElementById('orderManagement').style.display='none';
document.getElementById('userManagement').style.display='none';
document.getElementById('siteSettings').style.display='none';
document.getElementById('contentManagement').style.display='none';
document.getElementById('quoteManagement').style.display='none';
document.getElementById('animationSettings').style.display='none';
loadDashboard();
}

function showCurrencyManagement(){
document.getElementById('adminDashboard').style.display='none';
document.getElementById('currencyManagement').style.display='block';
document.getElementById('orderManagement').style.display='none';
document.getElementById('userManagement').style.display='none';
document.getElementById('siteSettings').style.display='none';
document.getElementById('contentManagement').style.display='none';
document.getElementById('quoteManagement').style.display='none';
document.getElementById('animationSettings').style.display='none';
updateCurrencyList();
}

function showOrderManagement(){
document.getElementById('adminDashboard').style.display='none';
document.getElementById('currencyManagement').style.display='none';
document.getElementById('orderManagement').style.display='block';
document.getElementById('userManagement').style.display='none';
document.getElementById('siteSettings').style.display='none';
document.getElementById('contentManagement').style.display='none';
document.getElementById('quoteManagement').style.display='none';
document.getElementById('animationSettings').style.display='none';
loadAdminOrders();
}

function showUserManagement(){
document.getElementById('adminDashboard').style.display='none';
document.getElementById('currencyManagement').style.display='none';
document.getElementById('orderManagement').style.display='none';
document.getElementById('userManagement').style.display='block';
document.getElementById('siteSettings').style.display='none';
document.getElementById('contentManagement').style.display='none';
document.getElementById('quoteManagement').style.display='none';
document.getElementById('animationSettings').style.display='none';
}

function showSiteSettings(){
document.getElementById('adminDashboard').style.display='none';
document.getElementById('currencyManagement').style.display='none';
document.getElementById('orderManagement').style.display='none';
document.getElementById('userManagement').style.display='none';
document.getElementById('siteSettings').style.display='block';
document.getElementById('contentManagement').style.display='none';
document.getElementById('quoteManagement').style.display='none';
document.getElementById('animationSettings').style.display='none';
loadSiteSettings();
}

function showContentManagement(){
document.getElementById('adminDashboard').style.display='none';
document.getElementById('currencyManagement').style.display='none';
document.getElementById('orderManagement').style.display='none';
document.getElementById('userManagement').style.display='none';
document.getElementById('siteSettings').style.display='none';
document.getElementById('contentManagement').style.display='block';
document.getElementById('quoteManagement').style.display='none';
document.getElementById('animationSettings').style.display='none';
loadContentSettings();
loadPaymentMethods();
}

function showQuoteManagement(){
document.getElementById('adminDashboard').style.display='none';
document.getElementById('currencyManagement').style.display='none';
document.getElementById('orderManagement').style.display='none';
document.getElementById('userManagement').style.display='none';
document.getElementById('siteSettings').style.display='none';
document.getElementById('contentManagement').style.display='none';
document.getElementById('quoteManagement').style.display='block';
document.getElementById('animationSettings').style.display='none';
loadQuotes();
}

function showAnimationSettings(){
document.getElementById('adminDashbd').style.display='none';
document.getElementById('currencyManagement').style.display='none';
document.getElementById('orderManagement').style.display='none';
document.getElementById('userManagement').style.display='none';
document.getElementById('siteSettings').style.display='none';
document.getElementById('contentManagement').style.display='none';
document.getElementById('quoteManagement').style.display='none';
document.getElementById('animationSettings').style.display='block';
loadAnimationSettings();
}

// ADMIN LOGIN CHECK
function checkAdmin(){
const email = document.getElementById('adminemail').value.trim();
const pass = document.getElementById('adminpass').value.trim();
const rememberMe = document.getElementById('rememberMe').checked;

if(email === ADMIN_EMAIL && pass === ADMIN_PASSWORD){ 
document.getElementById('adminLogin').style.display="none"; 
showAdmin(); 

// Save login state if "Remember Me" is checked
if (rememberMe) {
localStorage.setItem('adminLoggedIn', 'true');
localStorage.setItem('adminEmail', email);
} else {
sessionStorage.setItem('adminLoggedIn', 'true');
}
} else { 
alert("❌ Wrong Email or Password!"); 
}
}

function showAdmin(){ 
document.getElementById('adminArea').style.display='block'; 
showAdminDashboard();
loadSiteSettings();
loadCurrencies();
}

function logoutAdmin() {
document.getElementById('adminLogin').style.display="flex";
document.getElementById('adminArea').style.display='none';
// Clear both localStorage and sessionStorage
localStorage.removeItem('adminLoggedIn');
localStorage.removeItem('adminEmail');
sessionStorage.removeItem('adminLoggedIn');
}

// ON LOAD
window.addEventListener('load',()=>{
// Check if already logged in (either in localStorage or sessionStorage)
const isLoggedIn = localStorage.getItem('adminLoggedIn') === 'true' || sessionStorage.getItem('adminLoggedIn') === 'true';
if (isLoggedIn) {
document.getElementById('adminLogin').style.display="none";
showAdmin();
// If email is saved in localStorage, fill the email field
const savedEmail = localStorage.getItem('adminEmail');
if (savedEmail) {
document.getElementById('adminEmail').value = savedEmail;
}
} else {
// Show login form if not logged in
document.getElementById('adminLogin').style.display="flex";
}

// Add event listeners
document.getElementById('statusOverrideEnabled').addEventListener('change', function() {
document.getElementById('manualStatusControl').style.display = this.checked ? 'block' : 'none';
updateStatusPreview();
});

document.getElementById('workStartHour').addEventListener('input', updateStatusPreview);
document.getElementById('workEndHour').addEventListener('input', updateStatusPreview);
document.getElementById('statusOverride').addEventListener('change', updateStatusPreview);

document.getElementById('animatedTextEnabled').addEventListener('change', function() {
document.getElementById('animatedTextControls').style.display = this.checked ? 'block' : 'none';
document.getElementById('animatedTextPreview').style.display = this.checked ? 'block' : 'none';
if (this.checked) {
updateAnimatedTextPreview();
}
});

document.getElementById('animatedText').addEventListener('input', updateAnimatedTextPreview);
document.getElementById('animationSpeed').addEventListener('input', updateAnimatedTextPreview);
document.getElementById('animationDirection').addEventListener('change', updateAnimatedTextPreview);
document.getElementById('animatedTextColor').addEventListener('input', updateAnimatedTextPreview);
document.getElementById('animatedBgColor').addEventListener('input', updateAnimatedTextPreview);
});
</script>

</body>
</html>
