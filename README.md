<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Dollar Exchange - Admin Panel</title>
<style>
:root {
    --primary: #4361ee;
    --primary-dark: #3a0ca3;
    --secondary: #7209b7;
    --success: #06ffa5;
    --warning: #ffbe0b;
    --danger: #fb5607;
    --light: #f8f9fa;
    --dark: #212529;
    --sidebar-width: 260px;
    --header-height: 70px;
    --card-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    --transition: all 0.3s ease;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

body {
    background-color: #f5f7fb;
    color: #333;
    overflow-x: hidden;
}

/* Layout Structure */
.admin-container {
    display: flex;
    min-height: 100vh;
}

.sidebar {
    width: var(--sidebar-width);
    background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
    color: white;
    position: fixed;
    height: 100vh;
    overflow-y: auto;
    z-index: 1000;
    transition: var(--transition);
    box-shadow: 2px 0 10px rgba(0, 0, 0, 0.1);
}

.sidebar.collapsed {
    width: 70px;
}

.sidebar-header {
    padding: 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.logo {
    display: flex;
    align-items: center;
    gap: 10px;
}

.logo img {
    width: 40px;
    height: 40px;
    border-radius: 8px;
    object-fit: cover;
}

.logo-text {
    font-size: 18px;
    font-weight: 700;
    transition: var(--transition);
}

.sidebar.collapsed .logo-text {
    display: none;
}

.toggle-btn {
    background: rgba(255, 255, 255, 0.2);
    border: none;
    color: white;
    width: 30px;
    height: 30px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
}

.sidebar-menu {
    padding: 20px 0;
}

.menu-item {
    padding: 15px 20px;
    display: flex;
    align-items: center;
    gap: 15px;
    cursor: pointer;
    transition: var(--transition);
    position: relative;
}

.menu-item:hover {
    background: rgba(255, 255, 255, 0.1);
}

.menu-item.active {
    background: rgba(255, 255, 255, 0.2);
}

.menu-item.active::before {
    content: '';
    position: absolute;
    left: 0;
    top: 0;
    height: 100%;
    width: 4px;
    background: var(--success);
}

.menu-icon {
    font-size: 20px;
    width: 24px;
    text-align: center;
}

.menu-text {
    transition: var(--transition);
}

.sidebar.collapsed .menu-text {
    display: none;
}

.main-content {
    flex: 1;
    margin-left: var(--sidebar-width);
    transition: var(--transition);
    display: flex;
    flex-direction: column;
}

.sidebar.collapsed + .main-content {
    margin-left: 70px;
}

.header {
    height: var(--header-height);
    background: white;
    box-shadow: var(--card-shadow);
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 30px;
    position: sticky;
    top: 0;
    z-index: 100;
}

.search-bar {
    display: flex;
    align-items: center;
    background: #f1f5f9;
    border-radius: 30px;
    padding: 8px 15px;
    width: 300px;
}

.search-bar input {
    border: none;
    background: none;
    outline: none;
    width: 100%;
    padding: 5px;
}

.header-actions {
    display: flex;
    align-items: center;
    gap: 15px;
}

.notification-btn {
    background: none;
    border: none;
    font-size: 20px;
    color: var(--dark);
    position: relative;
    cursor: pointer;
}

.notification-badge {
    position: absolute;
    top: -5px;
    right: -5px;
    background: var(--danger);
    color: white;
    font-size: 10px;
    width: 18px;
    height: 18px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.user-profile {
    display: flex;
    align-items: center;
    gap: 10px;
    cursor: pointer;
}

.user-avatar {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    object-fit: cover;
}

.user-info {
    display: flex;
    flex-direction: column;
}

.user-name {
    font-weight: 600;
    font-size: 14px;
}

.user-role {
    font-size: 12px;
    color: #6c757d;
}

.content {
    padding: 30px;
    flex: 1;
}

.page-title {
    font-size: 24px;
    font-weight: 700;
    margin-bottom: 30px;
    color: var(--dark);
}

/* Cards */
.stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 20px;
    margin-bottom: 30px;
}

.stat-card {
    background: white;
    border-radius: 12px;
    padding: 25px;
    box-shadow: var(--card-shadow);
    position: relative;
    overflow: hidden;
    transition: var(--transition);
}

.stat-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
}

.stat-card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 5px;
    height: 100%;
    background: var(--primary);
}

.stat-card.success::before {
    background: var(--success);
}

.stat-card.warning::before {
    background: var(--warning);
}

.stat-card.danger::before {
    background: var(--danger);
}

.stat-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 15px;
}

.stat-title {
    font-size: 14px;
    color: #6c757d;
    text-transform: uppercase;
    letter-spacing: 1px;
}

.stat-icon {
    width: 40px;
    height: 40px;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 20px;
}

.stat-icon.primary {
    background: rgba(67, 97, 238, 0.1);
    color: var(--primary);
}

.stat-icon.success {
    background: rgba(6, 255, 165, 0.1);
    color: var(--success);
}

.stat-icon.warning {
    background: rgba(255, 190, 11, 0.1);
    color: var(--warning);
}

.stat-icon.danger {
    background: rgba(251, 86, 7, 0.1);
    color: var(--danger);
}

.stat-value {
    font-size: 28px;
    font-weight: 700;
    margin-bottom: 10px;
}

.stat-change {
    font-size: 12px;
    display: flex;
    align-items: center;
    gap: 5px;
}

.stat-change.positive {
    color: var(--success);
}

.stat-change.negative {
    color: var(--danger);
}

/* Tables */
.card {
    background: white;
    border-radius: 12px;
    box-shadow: var(--card-shadow);
    margin-bottom: 30px;
    overflow: hidden;
}

.card-header {
    padding: 20px 25px;
    border-bottom: 1px solid #f1f5f9;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.card-title {
    font-size: 18px;
    font-weight: 600;
}

.card-actions {
    display: flex;
    gap: 10px;
}

.card-body {
    padding: 20px 25px;
}

.table-container {
    overflow-x: auto;
}

table {
    width: 100%;
    border-collapse: collapse;
}

th {
    text-align: left;
    padding: 12px 15px;
    font-weight: 600;
    color: #6c757d;
    font-size: 14px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
    border-bottom: 2px solid #f1f5f9;
}

td {
    padding: 15px;
    border-bottom: 1px solid #f1f5f9;
}

tr:hover {
    background: #f8f9fa;
}

.status-badge {
    padding: 5px 12px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 600;
    display: inline-block;
}

.status-badge.pending {
    background: rgba(255, 190, 11, 0.1);
    color: var(--warning);
}

.status-badge.completed {
    background: rgba(6, 255, 165, 0.1);
    color: var(--success);
}

.status-badge.rejected {
    background: rgba(251, 86, 7, 0.1);
    color: var(--danger);
}

/* Forms */
.form-group {
    margin-bottom: 20px;
}

.form-label {
    display: block;
    margin-bottom: 8px;
    font-weight: 600;
    font-size: 14px;
}

.form-control {
    width: 100%;
    padding: 12px 15px;
    border: 1px solid #e1e8ed;
    border-radius: 8px;
    font-size: 14px;
    transition: var(--transition);
}

.form-control:focus {
    outline: none;
    border-color: var(--primary);
    box-shadow: 0 0 0 3px rgba(67, 97, 238, 0.1);
}

.form-row {
    display: flex;
    gap: 20px;
}

.form-row .form-group {
    flex: 1;
}

/* Buttons */
.btn {
    padding: 10px 20px;
    border: none;
    border-radius: 8px;
    font-weight: 600;
    font-size: 14px;
    cursor: pointer;
    transition: var(--transition);
    display: inline-flex;
    align-items: center;
    gap: 8px;
}

.btn-primary {
    background: var(--primary);
    color: white;
}

.btn-primary:hover {
    background: var(--primary-dark);
}

.btn-success {
    background: var(--success);
    color: white;
}

.btn-danger {
    background: var(--danger);
    color: white;
}

.btn-warning {
    background: var(--warning);
    color: white;
}

.btn-outline {
    background: transparent;
    border: 1px solid #e1e8ed;
    color: var(--dark);
}

.btn-outline:hover {
    background: #f1f5f9;
}

.btn-sm {
    padding: 6px 12px;
    font-size: 12px;
}

/* Modals */
.modal {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 2000;
    opacity: 0;
    visibility: hidden;
    transition: var(--transition);
}

.modal.show {
    opacity: 1;
    visibility: visible;
}

.modal-content {
    background: white;
    border-radius: 12px;
    width: 90%;
    max-width: 500px;
    max-height: 90vh;
    overflow-y: auto;
    transform: scale(0.8);
    transition: var(--transition);
}

.modal.show .modal-content {
    transform: scale(1);
}

.modal-header {
    padding: 20px 25px;
    border-bottom: 1px solid #f1f5f9;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.modal-title {
    font-size: 18px;
    font-weight: 600;
}

.modal-close {
    background: none;
    border: none;
    font-size: 20px;
    cursor: pointer;
    color: #6c757d;
}

.modal-body {
    padding: 25px;
}

.modal-footer {
    padding: 15px 25px;
    border-top: 1px solid #f1f5f9;
    display: flex;
    justify-content: flex-end;
    gap: 10px;
}

/* Login Screen */
.login-container {
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
}

.login-card {
    background: white;
    border-radius: 16px;
    width: 100%;
    max-width: 450px;
    padding: 40px;
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.1);
}

.login-header {
    text-align: center;
    margin-bottom: 30px;
}

.login-logo {
    width: 80px;
    height: 80px;
    margin: 0 auto 20px;
    border-radius: 50%;
    object-fit: cover;
}

.login-title {
    font-size: 24px;
    font-weight: 700;
    color: var(--dark);
    margin-bottom: 10px;
}

.login-subtitle {
    color: #6c757d;
    font-size: 14px;
}

.login-form {
    margin-bottom: 20px;
}

.login-footer {
    text-align: center;
    font-size: 14px;
    color: #6c757d;
}

.login-footer a {
    color: var(--primary);
    text-decoration: none;
}

/* Currency Management */
.currency-item {
    display: flex;
    align-items: center;
    padding: 15px;
    border: 1px solid #e1e8ed;
    border-radius: 8px;
    margin-bottom: 15px;
}

.currency-icon {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    background: #f1f5f9;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 20px;
    margin-right: 15px;
}

.currency-info {
    flex: 1;
}

.currency-name {
    font-weight: 600;
    margin-bottom: 5px;
}

.currency-rate {
    color: #6c757d;
    font-size: 14px;
}

.currency-actions {
    display: flex;
    gap: 10px;
}

/* Chart Container */
.chart-container {
    height: 300px;
    margin-bottom: 30px;
}

/* Responsive */
@media (max-width: 992px) {
    .sidebar {
        transform: translateX(-100%);
    }
    
    .sidebar.show {
        transform: translateX(0);
    }
    
    .main-content {
        margin-left: 0;
    }
    
    .stats-grid {
        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    }
}

@media (max-width: 768px) {
    .header {
        padding: 0 15px;
    }
    
    .search-bar {
        width: 200px;
    }
    
    .user-info {
        display: none;
    }
    
    .content {
        padding: 20px 15px;
    }
    
    .form-row {
        flex-direction: column;
        gap: 0;
    }
}

/* Loading Spinner */
.spinner {
    width: 40px;
    height: 40px;
    border: 4px solid #f1f5f9;
    border-top: 4px solid var(--primary);
    border-radius: 50%;
    animation: spin 1s linear infinite;
    margin: 20px auto;
}

@keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
}

/* Notification Toast */
.toast {
    position: fixed;
    bottom: 20px;
    right: 20px;
    background: white;
    padding: 15px 20px;
    border-radius: 8px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
    display: flex;
    align-items: center;
    gap: 10px;
    transform: translateX(100%);
    transition: transform 0.3s ease;
    z-index: 3000;
    max-width: 350px;
}

.toast.show {
    transform: translateX(0);
}

.toast-icon {
    font-size: 20px;
}

.toast.success .toast-icon {
    color: var(--success);
}

.toast.error .toast-icon {
    color: var(--danger);
}

.toast.warning .toast-icon {
    color: var(--warning);
}

.toast-message {
    flex: 1;
}

.toast-close {
    background: none;
    border: none;
    cursor: pointer;
    color: #6c757d;
}

/* Tab Navigation */
.tab-nav {
    display: flex;
    border-bottom: 1px solid #e1e8ed;
    margin-bottom: 20px;
}

.tab-btn {
    padding: 12px 20px;
    background: none;
    border: none;
    border-bottom: 2px solid transparent;
    font-weight: 600;
    color: #6c757d;
    cursor: pointer;
    transition: var(--transition);
}

.tab-btn.active {
    color: var(--primary);
    border-bottom-color: var(--primary);
}

.tab-content {
    display: none;
}

.tab-content.active {
    display: block;
}

/* User Area (Hidden by default) */
#userArea {
    display: none;
}
</style>
</head>
<body>
<!-- Login Screen -->
<div id="loginScreen" class="login-container">
    <div class="login-card">
        <div class="login-header">
            <img src="https://i.ibb.co/5WWPvnQj/20251128-160723.png" alt="Logo" class="login-logo">
            <h1 class="login-title">Admin Panel</h1>
            <p class="login-subtitle">Dollar Exchange Management System</p>
        </div>
        
        <div class="login-form">
            <div class="form-group">
                <label class="form-label">Gmail Address</label>
                <input type="email" id="adminEmail" class="form-control" placeholder="admin@gmail.com" required>
            </div>
            
            <div class="form-group">
                <label class="form-label">Password</label>
                <input type="password" id="adminPassword" class="form-control" placeholder="Enter your password" required>
            </div>
            
            <div class="form-group">
                <label>
                    <input type="checkbox" id="rememberMe"> Remember me
                </label>
            </div>
            
            <button class="btn btn-primary" style="width: 100%;" onclick="adminLogin()">Sign In</button>
        </div>
        
        <div class="login-footer">
            <p>Forgot your password? <a href="#">Reset it here</a></p>
        </div>
    </div>
</div>

<!-- Admin Panel -->
<div id="adminPanel" class="admin-container" style="display: none;">
    <!-- Sidebar -->
    <aside class="sidebar" id="sidebar">
        <div class="sidebar-header">
            <div class="logo">
                <img src="https://i.ibb.co/5WWPvnQj/20251128-160723.png" alt="Logo">
                <span class="logo-text">Dollar Exchange</span>
            </div>
            <button class="toggle-btn" onclick="toggleSidebar()">
                <i class="fas fa-bars"></i>
            </button>
        </div>
        
        <nav class="sidebar-menu">
            <div class="menu-item active" onclick="showPage('dashboard')">
                <span class="menu-icon"><i class="fas fa-tachometer-alt"></i></span>
                <span class="menu-text">Dashboard</span>
            </div>
            
            <div class="menu-item" onclick="showPage('orders')">
                <span class="menu-icon"><i class="fas fa-shopping-cart"></i></span>
                <span class="menu-text">Orders</span>
            </div>
            
            <div class="menu-item" onclick="showPage('currencies')">
                <span class="menu-icon"><i class="fas fa-coins"></i></span>
                <span class="menu-text">Currencies</span>
            </div>
            
            <div class="menu-item" onclick="showPage('users')">
                <span class="menu-icon"><i class="fas fa-users"></i></span>
                <span class="menu-text">Users</span>
            </div>
            
            <div class="menu-item" onclick="showPage('transactions')">
                <span class="menu-icon"><i class="fas fa-exchange-alt"></i></span>
                <span class="menu-text">Transactions</span>
            </div>
            
            <div class="menu-item" onclick="showPage('payments')">
                <span class="menu-icon"><i class="fas fa-credit-card"></i></span>
                <span class="menu-text">Payment Methods</span>
            </div>
            
            <div class="menu-item" onclick="showPage('notifications')">
                <span class="menu-icon"><i class="fas fa-bell"></i></span>
                <span class="menu-text">Notifications</span>
            </div>
            
            <div class="menu-item" onclick="showPage('settings')">
                <span class="menu-icon"><i class="fas fa-cog"></i></span>
                <span class="menu-text">Settings</span>
            </div>
            
            <div class="menu-item" onclick="showPage('logs')">
                <span class="menu-icon"><i class="fas fa-file-alt"></i></span>
                <span class="menu-text">System Logs</span>
            </div>
            
            <div class="menu-item" onclick="adminLogout()">
                <span class="menu-icon"><i class="fas fa-sign-out-alt"></i></span>
                <span class="menu-text">Logout</span>
            </div>
        </nav>
    </aside>
    
    <!-- Main Content -->
    <main class="main-content">
        <!-- Header -->
        <header class="header">
            <div class="search-bar">
                <i class="fas fa-search"></i>
                <input type="text" placeholder="Search...">
            </div>
            
            <div class="header-actions">
                <button class="notification-btn">
                    <i class="fas fa-bell"></i>
                    <span class="notification-badge">3</span>
                </button>
                
                <div class="user-profile">
                    <img src="https://picsum.photos/seed/admin/40/40.jpg" alt="Admin" class="user-avatar">
                    <div class="user-info">
                        <div class="user-name">Admin User</div>
                        <div class="user-role">Administrator</div>
                    </div>
                </div>
            </div>
        </header>
        
        <!-- Content Area -->
        <div class="content">
            <!-- Dashboard Page -->
            <div id="dashboardPage" class="page-content">
                <h1 class="page-title">Dashboard</h1>
                
                <!-- Stats Cards -->
                <div class="stats-grid">
                    <div class="stat-card">
                        <div class="stat-header">
                            <div class="stat-title">Total Orders</div>
                            <div class="stat-icon primary">
                                <i class="fas fa-shopping-cart"></i>
                            </div>
                        </div>
                        <div class="stat-value" id="totalOrders">0</div>
                        <div class="stat-change positive">
                            <i class="fas fa-arrow-up"></i> 12% from last month
                        </div>
                    </div>
                    
                    <div class="stat-card success">
                        <div class="stat-header">
                            <div class="stat-title">Completed Orders</div>
                            <div class="stat-icon success">
                                <i class="fas fa-check-circle"></i>
                            </div>
                        </div>
                        <div class="stat-value" id="completedOrders">0</div>
                        <div class="stat-change positive">
                            <i class="fas fa-arrow-up"></i> 8% from last month
                        </div>
                    </div>
                    
                    <div class="stat-card warning">
                        <div class="stat-header">
                            <div class="stat-title">Pending Orders</div>
                            <div class="stat-icon warning">
                                <i class="fas fa-clock"></i>
                            </div>
                        </div>
                        <div class="stat-value" id="pendingOrders">0</div>
                        <div class="stat-change negative">
                            <i class="fas fa-arrow-down"></i> 3% from last month
                        </div>
                    </div>
                    
                    <div class="stat-card danger">
                        <div class="stat-header">
                            <div class="stat-title">Total Revenue</div>
                            <div class="stat-icon danger">
                                <i class="fas fa-dollar-sign"></i>
                            </div>
                        </div>
                        <div class="stat-value" id="totalRevenue">0</div>
                        <div class="stat-change positive">
                            <i class="fas fa-arrow-up"></i> 15% from last month
                        </div>
                    </div>
                </div>
                
                <!-- Charts -->
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Revenue Overview</h3>
                        <div class="card-actions">
                            <select class="form-control" style="width: auto;">
                                <option>Last 7 days</option>
                                <option>Last 30 days</option>
                                <option>Last 3 months</option>
                                <option>Last year</option>
                            </select>
                        </div>
                    </div>
                    <div class="card-body">
                        <div class="chart-container">
                            <canvas id="revenueChart"></canvas>
                        </div>
                    </div>
                </div>
                
                <!-- Recent Orders -->
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Recent Orders</h3>
                        <button class="btn btn-primary btn-sm">View All</button>
                    </div>
                    <div class="card-body">
                        <div class="table-container">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Order ID</th>
                                        <th>Customer</th>
                                        <th>Currency</th>
                                        <th>Amount</th>
                                        <th>Status</th>
                                        <th>Date</th>
                                        <th>Actions</th>
                                    </tr>
                                </thead>
                                <tbody id="recentOrdersTable">
                                    <!-- Orders will be loaded here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Orders Page -->
            <div id="ordersPage" class="page-content" style="display: none;">
                <h1 class="page-title">Order Management</h1>
                
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">All Orders</h3>
                        <div class="card-actions">
                            <div class="form-group" style="margin: 0; width: 200px;">
                                <input type="text" id="orderSearch" class="form-control" placeholder="Search by name or number" oninput="filterOrders()">
                            </div>
                            <button class="btn btn-primary btn-sm" onclick="refreshOrders()">
                                <i class="fas fa-sync-alt"></i> Refresh
                            </button>
                        </div>
                    </div>
                    <div class="card-body">
                        <div class="table-container">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Order ID</th>
                                        <th>Customer</th>
                                        <th>Currency</th>
                                        <th>Amount</th>
                                        <th>Payment Method</th>
                                        <th>Status</th>
                                        <th>Date</th>
                                        <th>Actions</th>
                                    </tr>
                                </thead>
                                <tbody id="ordersTable">
                                    <!-- Orders will be loaded here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Currencies Page -->
            <div id="currenciesPage" class="page-content" style="display: none;">
                <h1 class="page-title">Currency Management</h1>
                
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">All Currencies</h3>
                        <button class="btn btn-primary btn-sm" onclick="showAddCurrencyModal()">
                            <i class="fas fa-plus"></i> Add Currency
                        </button>
                    </div>
                    <div class="card-body">
                        <div id="currenciesList">
                            <!-- Currencies will be loaded here -->
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Users Page -->
            <div id="usersPage" class="page-content" style="display: none;">
                <h1 class="page-title">User Management</h1>
                
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">All Users</h3>
                        <div class="card-actions">
                            <div class="form-group" style="margin: 0; width: 200px;">
                                <input type="text" id="userSearch" class="form-control" placeholder="Search by name or email" oninput="filterUsers()">
                            </div>
                            <button class="btn btn-primary btn-sm" onclick="refreshUsers()">
                                <i class="fas fa-sync-alt"></i> Refresh
                            </button>
                        </div>
                    </div>
                    <div class="card-body">
                        <div class="table-container">
                            <table>
                                <thead>
                                    <tr>
                                        <th>User ID</th>
                                        <th>Name</th>
                                        <th>Email</th>
                                        <th>Phone</th>
                                        <th>User Type</th>
                                        <th>Join Date</th>
                                        <th>Actions</th>
                                    </tr>
                                </thead>
                                <tbody id="usersTable">
                                    <!-- Users will be loaded here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Transactions Page -->
            <div id="transactionsPage" class="page-content" style="display: none;">
                <h1 class="page-title">Transaction History</h1>
                
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">All Transactions</h3>
                        <div class="card-actions">
                            <select class="form-control" style="width: auto;">
                                <option>All Status</option>
                                <option>Completed</option>
                                <option>Pending</option>
                                <option>Failed</option>
                            </select>
                            <button class="btn btn-primary btn-sm" onclick="exportTransactions()">
                                <i class="fas fa-download"></i> Export
                            </button>
                        </div>
                    </div>
                    <div class="card-body">
                        <div class="table-container">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Transaction ID</th>
                                        <th>User</th>
                                        <th>Type</th>
                                        <th>Amount</th>
                                        <th>Currency</th>
                                        <th>Status</th>
                                        <th>Date</th>
                                        <th>Actions</th>
                                    </tr>
                                </thead>
                                <tbody id="transactionsTable">
                                    <!-- Transactions will be loaded here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Payment Methods Page -->
            <div id="paymentsPage" class="page-content" style="display: none;">
                <h1 class="page-title">Payment Methods</h1>
                
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">All Payment Methods</h3>
                        <button class="btn btn-primary btn-sm" onclick="showAddPaymentModal()">
                            <i class="fas fa-plus"></i> Add Payment Method
                        </button>
                    </div>
                    <div class="card-body">
                        <div id="paymentMethodsList">
                            <!-- Payment methods will be loaded here -->
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Notifications Page -->
            <div id="notificationsPage" class="page-content" style="display: none;">
                <h1 class="page-title">Notifications</h1>
                
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Send Notification</h3>
                    </div>
                    <div class="card-body">
                        <div class="form-group">
                            <label class="form-label">Notification Type</label>
                            <select id="notificationType" class="form-control">
                                <option value="all">All Users</option>
                                <option value="specific">Specific Users</option>
                                <option value="userType">By User Type</option>
                            </select>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">Title</label>
                            <input type="text" id="notificationTitle" class="form-control" placeholder="Notification title">
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">Message</label>
                            <textarea id="notificationMessage" class="form-control" rows="4" placeholder="Notification message"></textarea>
                        </div>
                        
                        <button class="btn btn-primary" onclick="sendNotification()">
                            <i class="fas fa-paper-plane"></i> Send Notification
                        </button>
                    </div>
                </div>
                
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Notification History</h3>
                    </div>
                    <div class="card-body">
                        <div class="table-container">
                            <table>
                                <thead>
                                    <tr>
                                        <th>ID</th>
                                        <th>Title</th>
                                        <th>Type</th>
                                        <th>Recipients</th>
                                        <th>Sent Date</th>
                                        <th>Status</th>
                                    </tr>
                                </thead>
                                <tbody id="notificationsTable">
                                    <!-- Notifications will be loaded here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Settings Page -->
            <div id="settingsPage" class="page-content" style="display: none;">
                <h1 class="page-title">Settings</h1>
                
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">General Settings</h3>
                    </div>
                    <div class="card-body">
                        <div class="form-row">
                            <div class="form-group">
                                <label class="form-label">Site Name</label>
                                <input type="text" id="siteName" class="form-control" value="Dollar Exchange">
                            </div>
                            <div class="form-group">
                                <label class="form-label">Site Email</label>
                                <input type="email" id="siteEmail" class="form-control" value="admin@dollarexchange.com">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label class="form-label">Exchange Fee (%)</label>
                                <input type="number" id="exchangeFee" class="form-control" value="2" min="0" max="100" step="0.1">
                            </div>
                            <div class="form-group">
                                <label class="form-label">Minimum Exchange Amount (USD)</label>
                                <input type="number" id="minExchange" class="form-control" value="5" min="1">
                            </div>
                        </div>
                        
                        <div class="form-row">
                            <div class="form-group">
                                <label class="form-label">Maximum Exchange Amount (USD)</label>
                                <input type="number" id="maxExchange" class="form-control" value="1000" min="1">
                            </div>
                            <div class="form-group">
                                <label class="form-label">Business Hours</label>
                                <input type="text" id="businessHours" class="form-control" value="9:00 AM - 6:00 PM">
                            </div>
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">Site Status</label>
                            <select id="siteStatus" class="form-control">
                                <option value="online">Online</option>
                                <option value="offline">Offline</option>
                                <option value="maintenance">Maintenance</option>
                            </select>
                        </div>
                        
                        <button class="btn btn-primary" onclick="saveSettings()">
                            <i class="fas fa-save"></i> Save Settings
                        </button>
                    </div>
                </div>
                
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Admin Account</h3>
                    </div>
                    <div class="card-body">
                        <div class="form-group">
                            <label class="form-label">Admin Email</label>
                            <input type="email" id="adminEmailSetting" class="form-control" value="admin@gmail.com">
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">New Password</label>
                            <input type="password" id="adminPasswordSetting" class="form-control" placeholder="Leave blank to keep current password">
                        </div>
                        
                        <div class="form-group">
                            <label class="form-label">Confirm Password</label>
                            <input type="password" id="adminPasswordConfirm" class="form-control" placeholder="Confirm new password">
                        </div>
                        
                        <button class="btn btn-primary" onclick="updateAdminAccount()">
                            <i class="fas fa-user-shield"></i> Update Admin Account
                        </button>
                    </div>
                </div>
            </div>
            
            <!-- System Logs Page -->
            <div id="logsPage" class="page-content" style="display: none;">
                <h1 class="page-title">System Logs</h1>
                
                <div class="card">
                    <div class="card-header">
                        <h3 class="card-title">Activity Logs</h3>
                        <div class="card-actions">
                            <select class="form-control" style="width: auto;">
                                <option>All Levels</option>
                                <option>Info</option>
                                <option>Warning</option>
                                <option>Error</option>
                            </select>
                            <button class="btn btn-primary btn-sm" onclick="exportLogs()">
                                <i class="fas fa-download"></i> Export
                            </button>
                        </div>
                    </div>
                    <div class="card-body">
                        <div class="table-container">
                            <table>
                                <thead>
                                    <tr>
                                        <th>ID</th>
                                        <th>Timestamp</th>
                                        <th>Level</th>
                                        <th>Message</th>
                                        <th>User</th>
                                        <th>IP Address</th>
                                    </tr>
                                </thead>
                                <tbody id="logsTable">
                                    <!-- Logs will be loaded here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </main>
</div>

<!-- User Area (Hidden by default) -->
<div id="userArea">
    <!-- Original user panel content would go here -->
</div>

<!-- Modals -->
<!-- Add Currency Modal -->
<div id="addCurrencyModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 class="modal-title">Add New Currency</h3>
            <button class="modal-close" onclick="closeModal('addCurrencyModal')">&times;</button>
        </div>
        <div class="modal-body">
            <div class="form-group">
                <label class="form-label">Currency Name</label>
                <input type="text" id="newCurrencyName" class="form-control" placeholder="e.g., Bitcoin">
            </div>
            
            <div class="form-group">
                <label class="form-label">Currency Code</label>
                <input type="text" id="newCurrencyCode" class="form-control" placeholder="e.g., BTC">
            </div>
            
            <div class="form-group">
                <label class="form-label">Exchange Rate (1 USD = ?)</label>
                <input type="number" id="newCurrencyRate" class="form-control" placeholder="Exchange rate" step="0.01">
            </div>
            
            <div class="form-group">
                <label class="form-label">Payment ID</label>
                <input type="text" id="newCurrencyPaymentId" class="form-control" placeholder="Payment ID">
            </div>
            
            <div class="form-group">
                <label class="form-label">Status</label>
                <select id="newCurrencyStatus" class="form-control">
                    <option value="active">Active</option>
                    <option value="inactive">Inactive</option>
                </select>
            </div>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline" onclick="closeModal('addCurrencyModal')">Cancel</button>
            <button class="btn btn-primary" onclick="addCurrency()">Add Currency</button>
        </div>
    </div>
</div>

<!-- Edit Currency Modal -->
<div id="editCurrencyModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 class="modal-title">Edit Currency</h3>
            <button class="modal-close" onclick="closeModal('editCurrencyModal')">&times;</button>
        </div>
        <div class="modal-body">
            <input type="hidden" id="editCurrencyId">
            
            <div class="form-group">
                <label class="form-label">Currency Name</label>
                <input type="text" id="editCurrencyName" class="form-control">
            </div>
            
            <div class="form-group">
                <label class="form-label">Currency Code</label>
                <input type="text" id="editCurrencyCode" class="form-control">
            </div>
            
            <div class="form-group">
                <label class="form-label">Exchange Rate (1 USD = ?)</label>
                <input type="number" id="editCurrencyRate" class="form-control" step="0.01">
            </div>
            
            <div class="form-group">
                <label class="form-label">Payment ID</label>
                <input type="text" id="editCurrencyPaymentId" class="form-control">
            </div>
            
            <div class="form-group">
                <label class="form-label">Status</label>
                <select id="editCurrencyStatus" class="form-control">
                    <option value="active">Active</option>
                    <option value="inactive">Inactive</option>
                </select>
            </div>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline" onclick="closeModal('editCurrencyModal')">Cancel</button>
            <button class="btn btn-primary" onclick="updateCurrency()">Update Currency</button>
        </div>
    </div>
</div>

<!-- Add Payment Method Modal -->
<div id="addPaymentModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 class="modal-title">Add Payment Method</h3>
            <button class="modal-close" onclick="closeModal('addPaymentModal')">&times;</button>
        </div>
        <div class="modal-body">
            <div class="form-group">
                <label class="form-label">Method Name</label>
                <input type="text" id="newPaymentName" class="form-control" placeholder="e.g., bKash">
            </div>
            
            <div class="form-group">
                <label class="form-label">Method Type</label>
                <select id="newPaymentType" class="form-control">
                    <option value="mobile">Mobile Banking</option>
                    <option value="bank">Bank Transfer</option>
                    <option value="crypto">Cryptocurrency</option>
                    <option value="other">Other</option>
                </select>
            </div>
            
            <div class="form-group">
                <label class="form-label">Account Details</label>
                <textarea id="newPaymentDetails" class="form-control" rows="3" placeholder="Account number, instructions, etc."></textarea>
            </div>
            
            <div class="form-group">
                <label class="form-label">Status</label>
                <select id="newPaymentStatus" class="form-control">
                    <option value="active">Active</option>
                    <option value="inactive">Inactive</option>
                </select>
            </div>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline" onclick="closeModal('addPaymentModal')">Cancel</button>
            <button class="btn btn-primary" onclick="addPaymentMethod()">Add Payment Method</button>
        </div>
    </div>
</div>

<!-- Toast Notification -->
<div id="toast" class="toast">
    <span class="toast-icon"></span>
    <span class="toast-message"></span>
    <button class="toast-close" onclick="hideToast()">&times;</button>
</div>

<!-- Firebase SDK -->
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.1/firebase-auth-compat.js"></script>
<!-- Font Awesome for icons -->
<script src="https://kit.fontawesome.com/a076d05399.js" crossorigin="anonymous"></script>
<!-- Chart.js for charts -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

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

// Default currencies
let currencies = [
    { id: 'Payeer', name: 'Payeer', code: 'PYR', rate: 70, paymentId: 'P1131698605', status: 'active' },
    { id: 'Binance', name: 'Binance', code: 'BNC', rate: 20, paymentId: '1188473082', status: 'active' },
    { id: 'Advcash', name: 'Advcash', code: 'ADC', rate: 60, paymentId: 'U 1048 5654 4714', status: 'active' }
];

// Default payment methods
let paymentMethods = [
    { id: 'bKash', name: 'bKash', type: 'mobile', details: '017XXXXXXXX', status: 'active' },
    { id: 'Nagad', name: 'Nagad', type: 'mobile', details: '017XXXXXXXX', status: 'active' }
];

// Default site settings
let siteSettings = {
    siteName: 'Dollar Exchange',
    siteEmail: 'admin@dollarexchange.com',
    exchangeFee: 2,
    minExchange: 5,
    maxExchange: 1000,
    businessHours: '9:00 AM - 6:00 PM',
    siteStatus: 'online',
    adminEmail: 'admin@gmail.com'
};

// Admin credentials (for demo purposes)
const ADMIN_EMAIL = 'admin@gmail.com';
const ADMIN_PASSWORD = 'admin123';

// Current user
let currentUser = null;

// Check if user is already logged in
window.addEventListener('load', () => {
    // Check if admin is already logged in
    const savedEmail = localStorage.getItem('adminEmail');
    const savedPassword = localStorage.getItem('adminPassword');
    const rememberMe = localStorage.getItem('rememberMe') === 'true';
    
    if (rememberMe && savedEmail && savedPassword) {
        document.getElementById('adminEmail').value = savedEmail;
        document.getElementById('adminPassword').value = savedPassword;
        document.getElementById('rememberMe').checked = true;
    }
    
    // Check if user is already authenticated
    auth.onAuthStateChanged(user => {
        if (user) {
            currentUser = user;
            showAdminPanel();
        } else {
            showLoginScreen();
        }
    });
    
    // Load initial data
    loadCurrencies();
    loadPaymentMethods();
    loadSiteSettings();
});

// Login function
function adminLogin() {
    const email = document.getElementById('adminEmail').value;
    const password = document.getElementById('adminPassword').value;
    const rememberMe = document.getElementById('rememberMe').checked;
    
    if (!email || !password) {
        showToast('Please enter both email and password', 'error');
        return;
    }
    
    // For demo purposes, we'll use simple email/password check
    // In production, use Firebase Auth
    if (email === ADMIN_EMAIL && password === ADMIN_PASSWORD) {
        // Save credentials if remember me is checked
        if (rememberMe) {
            localStorage.setItem('adminEmail', email);
            localStorage.setItem('adminPassword', password);
            localStorage.setItem('rememberMe', 'true');
        } else {
            localStorage.removeItem('adminEmail');
            localStorage.removeItem('adminPassword');
            localStorage.setItem('rememberMe', 'false');
        }
        
        // Set current user
        currentUser = { email: email };
        
        // Show admin panel
        showAdminPanel();
        
        // Show success message
        showToast('Login successful!', 'success');
    } else {
        showToast('Invalid email or password', 'error');
    }
}

// Logout function
function adminLogout() {
    if (confirm('Are you sure you want to logout?')) {
        auth.signOut().then(() => {
            currentUser = null;
            showLoginScreen();
            showToast('Logged out successfully', 'success');
        }).catch(error => {
            console.error('Logout error:', error);
            showToast('Error logging out', 'error');
        });
    }
}

// Show login screen
function showLoginScreen() {
    document.getElementById('loginScreen').style.display = 'flex';
    document.getElementById('adminPanel').style.display = 'none';
}

// Show admin panel
function showAdminPanel() {
    document.getElementById('loginScreen').style.display = 'none';
    document.getElementById('adminPanel').style.display = 'flex';
    
    // Load dashboard data
    loadDashboard();
}

// Toggle sidebar
function toggleSidebar() {
    const sidebar = document.getElementById('sidebar');
    sidebar.classList.toggle('collapsed');
}

// Show page
function showPage(pageName) {
    // Hide all pages
    const pages = document.querySelectorAll('.page-content');
    pages.forEach(page => {
        page.style.display = 'none';
    });
    
    // Show selected page
    document.getElementById(pageName + 'Page').style.display = 'block';
    
    // Update active menu item
    const menuItems = document.querySelectorAll('.menu-item');
    menuItems.forEach(item => {
        item.classList.remove('active');
    });
    
    // Find and activate the clicked menu item
    for (let i = 0; i < menuItems.length; i++) {
        const onclickAttr = menuItems[i].getAttribute('onclick');
        if (onclickAttr && onclickAttr.includes(pageName)) {
            menuItems[i].classList.add('active');
            break;
        }
    }
    
    // Load page-specific data
    if (pageName === 'dashboard') {
        loadDashboard();
    } else if (pageName === 'orders') {
        loadOrders();
    } else if (pageName === 'currencies') {
        loadCurrencies();
    } else if (pageName === 'users') {
        loadUsers();
    } else if (pageName === 'transactions') {
        loadTransactions();
    } else if (pageName === 'payments') {
        loadPaymentMethods();
    } else if (pageName === 'notifications') {
        loadNotifications();
    } else if (pageName === 'settings') {
        loadSiteSettings();
    } else if (pageName === 'logs') {
        loadLogs();
    }
}

// Load dashboard data
async function loadDashboard() {
    try {
        // Get orders statistics
        const ordersSnapshot = await db.collection('orders').get();
        const totalOrders = ordersSnapshot.size;
        
        const pendingOrdersSnapshot = await db.collection('orders').where('status', '==', 'PENDING').get();
        const pendingOrders = pendingOrdersSnapshot.size;
        
        const completedOrdersSnapshot = await db.collection('orders').where('status', '==', 'COMPLETED').get();
        const completedOrders = completedOrdersSnapshot.size;
        
        // Calculate total revenue
        let totalRevenue = 0;
        completedOrdersSnapshot.forEach(doc => {
            const order = doc.data();
            totalRevenue += parseFloat(order.taka) || 0;
        });
        
        // Update dashboard stats
        document.getElementById('totalOrders').textContent = totalOrders;
        document.getElementById('pendingOrders').textContent = pendingOrders;
        document.getElementById('completedOrders').textContent = completedOrders;
        document.getElementById('totalRevenue').textContent = totalRevenue.toFixed(2);
        
        // Load recent orders
        const recentOrdersSnapshot = await db.collection('orders')
            .orderBy('createdAt', 'desc')
            .limit(5)
            .get();
        
        const recentOrdersTable = document.getElementById('recentOrdersTable');
        recentOrdersTable.innerHTML = '';
        
        if (recentOrdersSnapshot.empty) {
            recentOrdersTable.innerHTML = '<tr><td colspan="7" style="text-align: center;">No recent orders</td></tr>';
        } else {
            recentOrdersSnapshot.forEach(doc => {
                const order = { id: doc.id, ...doc.data() };
                const row = document.createElement('tr');
                
                const statusClass = order.status === 'COMPLETED' ? 'completed' : 
                                   order.status === 'REJECTED' ? 'rejected' : 'pending';
                
                row.innerHTML = `
                    <td>${order.id.substring(0, 8)}...</td>
                    <td>${order.name}</td>
                    <td>${order.currency}</td>
                    <td>$${order.dollar} → ${order.taka} Tk</td>
                    <td><span class="status-badge ${statusClass}">${order.status}</span></td>
                    <td>${new Date(order.createdAt).toLocaleDateString()}</td>
                    <td>
                        <button class="btn btn-primary btn-sm" onclick="viewOrder('${order.id}')">View</button>
                    </td>
                `;
                
                recentOrdersTable.appendChild(row);
            });
        }
        
        // Create revenue chart
        createRevenueChart();
    } catch (error) {
        console.error('Error loading dashboard:', error);
        showToast('Error loading dashboard data', 'error');
    }
}

// Create revenue chart
function createRevenueChart() {
    const ctx = document.getElementById('revenueChart');
    
    // If chart already exists, destroy it
    if (window.revenueChartInstance) {
        window.revenueChartInstance.destroy();
    }
    
    // Generate sample data for the last 7 days
    const labels = [];
    const data = [];
    
    for (let i = 6; i >= 0; i--) {
        const date = new Date();
        date.setDate(date.getDate() - i);
        labels.push(date.toLocaleDateString('en-US', { month: 'short', day: 'numeric' }));
        
        // Generate random data for demo
        data.push(Math.floor(Math.random() * 5000) + 1000);
    }
    
    window.revenueChartInstance = new Chart(ctx, {
        type: 'line',
        data: {
            labels: labels,
            datasets: [{
                label: 'Revenue (Tk)',
                data: data,
                backgroundColor: 'rgba(67, 97, 238, 0.1)',
                borderColor: 'rgba(67, 97, 238, 1)',
                borderWidth: 2,
                tension: 0.4,
                fill: true
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: {
                    display: false
                }
            },
            scales: {
                y: {
                    beginAtZero: true,
                    ticks: {
                        callback: function(value) {
                            return value + ' Tk';
                        }
                    }
                }
            }
        }
    });
}

// Load orders
async function loadOrders() {
    try {
        const ordersSnapshot = await db.collection('orders').get();
        const orders = [];
        
        ordersSnapshot.forEach(doc => {
            orders.push({ id: doc.id, ...doc.data() });
        });
        
        // Sort by creation date (newest first)
        orders.sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
        
        const ordersTable = document.getElementById('ordersTable');
        ordersTable.innerHTML = '';
        
        if (orders.length === 0) {
            ordersTable.innerHTML = '<tr><td colspan="8" style="text-align: center;">No orders found</td></tr>';
        } else {
            orders.forEach(order => {
                const row = document.createElement('tr');
                
                const statusClass = order.status === 'COMPLETED' ? 'completed' : 
                                   order.status === 'REJECTED' ? 'rejected' : 'pending';
                
                row.innerHTML = `
                    <td>${order.id.substring(0, 8)}...</td>
                    <td>${order.name}</td>
                    <td>${order.currency}</td>
                    <td>$${order.dollar} → ${order.taka} Tk</td>
                    <td>${order.paymentMethod}</td>
                    <td><span class="status-badge ${statusClass}">${order.status}</span></td>
                    <td>${new Date(order.createdAt).toLocaleDateString()}</td>
                    <td>
                        <button class="btn btn-primary btn-sm" onclick="viewOrder('${order.id}')">View</button>
                        ${order.status === 'PENDING' ? `
                            <button class="btn btn-success btn-sm" onclick="updateOrderStatus('${order.id}', 'COMPLETED')">Approve</button>
                            <button class="btn btn-danger btn-sm" onclick="updateOrderStatus('${order.id}', 'REJECTED')">Reject</button>
                        ` : ''}
                    </td>
                `;
                
                ordersTable.appendChild(row);
            });
        }
    } catch (error) {
        console.error('Error loading orders:', error);
        showToast('Error loading orders', 'error');
    }
}

// Filter orders
function filterOrders() {
    const searchTerm = document.getElementById('orderSearch').value.toLowerCase();
    const rows = document.querySelectorAll('#ordersTable tr');
    
    rows.forEach(row => {
        const name = row.cells[1]?.textContent.toLowerCase() || '';
        const number = row.cells[2]?.textContent.toLowerCase() || '';
        
        if (name.includes(searchTerm) || number.includes(searchTerm)) {
            row.style.display = '';
        } else {
            row.style.display = 'none';
        }
    });
}

// Refresh orders
function refreshOrders() {
    loadOrders();
    showToast('Orders refreshed', 'success');
}

// View order details
async function viewOrder(orderId) {
    try {
        const orderDoc = await db.collection('orders').doc(orderId).get();
        if (!orderDoc.exists) {
            showToast('Order not found', 'error');
            return;
        }
        
        const order = { id: orderDoc.id, ...orderDoc.data() };
        
        // Show order details in a modal
        const modal = document.createElement('div');
        modal.className = 'modal show';
        modal.innerHTML = `
            <div class="modal-content">
                <div class="modal-header">
                    <h3 class="modal-title">Order Details</h3>
                    <button class="modal-close" onclick="this.closest('.modal').remove()">&times;</button>
                </div>
                <div class="modal-body">
                    <div class="form-group">
                        <label class="form-label">Order ID</label>
                        <input type="text" class="form-control" value="${order.id}" readonly>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Customer Name</label>
                        <input type="text" class="form-control" value="${order.name}" readonly>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Phone Number</label>
                        <input type="text" class="form-control" value="${order.number}" readonly>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Currency</label>
                        <input type="text" class="form-control" value="${order.currency}" readonly>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Amount</label>
                        <input type="text" class="form-control" value="$${order.dollar} → ${order.taka} Tk" readonly>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Payment Method</label>
                        <input type="text" class="form-control" value="${order.paymentMethod}" readonly>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Status</label>
                        <select id="orderStatus" class="form-control">
                            <option value="PENDING" ${order.status === 'PENDING' ? 'selected' : ''}>Pending</option>
                            <option value="COMPLETED" ${order.status === 'COMPLETED' ? 'selected' : ''}>Completed</option>
                            <option value="REJECTED" ${order.status === 'REJECTED' ? 'selected' : ''}>Rejected</option>
                        </select>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Transaction ID</label>
                        <input type="text" id="orderTrx" class="form-control" value="${order.trx || ''}">
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label">Created At</label>
                        <input type="text" class="form-control" value="${new Date(order.createdAt).toLocaleString()}" readonly>
                    </div>
                </div>
                <div class="modal-footer">
                    <button class="btn btn-outline" onclick="this.closest('.modal').remove()">Cancel</button>
                    <button class="btn btn-primary" onclick="updateOrderDetails('${order.id}')">Update</button>
                </div>
            </div>
        `;
        
        document.body.appendChild(modal);
    } catch (error) {
        console.error('Error viewing order:', error);
        showToast('Error loading order details', 'error');
    }
}

// Update order status
async function updateOrderStatus(orderId, status) {
    try {
        await db.collection('orders').doc(orderId).update({
            status: status
        });
        
        loadOrders();
        showToast(`Order status updated to ${status}`, 'success');
    } catch (error) {
        console.error('Error updating order status:', error);
        showToast('Error updating order status', 'error');
    }
}

// Update order details
async function updateOrderDetails(orderId) {
    try {
        const status = document.getElementById('orderStatus').value;
        const trx = document.getElementById('orderTrx').value;
        
        await db.collection('orders').doc(orderId).update({
            status: status,
            trx: trx
        });
        
        // Close modal
        document.querySelector('.modal').remove();
        
        // Reload orders
        loadOrders();
        
        showToast('Order details updated', 'success');
    } catch (error) {
        console.error('Error updating order details:', error);
        showToast('Error updating order details', 'error');
    }
}

// Load currencies
async function loadCurrencies() {
    try {
        // Try to load currencies from Firestore
        const currenciesDoc = await db.collection('settings').doc('currencies').get();
        if (currenciesDoc.exists) {
            currencies = currenciesDoc.data().list || currencies;
        }
        
        // Update currency list
        const currenciesList = document.getElementById('currenciesList');
        currenciesList.innerHTML = '';
        
        currencies.forEach(currency => {
            const item = document.createElement('div');
            item.className = 'currency-item';
            
            const statusBadge = currency.status === 'active' ? 
                '<span class="status-badge completed">Active</span>' : 
                '<span class="status-badge rejected">Inactive</span>';
            
            item.innerHTML = `
                <div class="currency-icon">
                    <i class="fas fa-coins"></i>
                </div>
                <div class="currency-info">
                    <div class="currency-name">${currency.name} (${currency.code})</div>
                    <div class="currency-rate">1 USD = ${currency.rate} Tk</div>
                    <div class="currency-rate">Payment ID: ${currency.paymentId}</div>
                    <div>${statusBadge}</div>
                </div>
                <div class="currency-actions">
                    <button class="btn btn-primary btn-sm" onclick="showEditCurrencyModal('${currency.id}')">Edit</button>
                    <button class="btn btn-danger btn-sm" onclick="deleteCurrency('${currency.id}')">Delete</button>
                </div>
            `;
            
            currenciesList.appendChild(item);
        });
    } catch (error) {
        console.error('Error loading currencies:', error);
        showToast('Error loading currencies', 'error');
    }
}

// Show add currency modal
function showAddCurrencyModal() {
    document.getElementById('addCurrencyModal').classList.add('show');
}

// Show edit currency modal
function showEditCurrencyModal(currencyId) {
    const currency = currencies.find(c => c.id === currencyId);
    if (!currency) {
        showToast('Currency not found', 'error');
        return;
    }
    
    // Fill form with currency data
    document.getElementById('editCurrencyId').value = currency.id;
    document.getElementById('editCurrencyName').value = currency.name;
    document.getElementById('editCurrencyCode').value = currency.code;
    document.getElementById('editCurrencyRate').value = currency.rate;
    document.getElementById('editCurrencyPaymentId').value = currency.paymentId;
    document.getElementById('editCurrencyStatus').value = currency.status;
    
    // Show modal
    document.getElementById('editCurrencyModal').classList.add('show');
}

// Close modal
function closeModal(modalId) {
    document.getElementById(modalId).classList.remove('show');
}

// Add currency
async function addCurrency() {
    try {
        const name = document.getElementById('newCurrencyName').value.trim();
        const code = document.getElementById('newCurrencyCode').value.trim();
        const rate = parseFloat(document.getElementById('newCurrencyRate').value);
        const paymentId = document.getElementById('newCurrencyPaymentId').value.trim();
        const status = document.getElementById('newCurrencyStatus').value;
        
        if (!name || !code || isNaN(rate) || !paymentId) {
            showToast('Please fill all fields', 'error');
            return;
        }
        
        // Generate ID from name
        const id = name.toLowerCase().replace(/\s+/g, '_');
        
        // Check if currency already exists
        if (currencies.find(c => c.id === id)) {
            showToast('Currency with this name already exists', 'error');
            return;
        }
        
        // Add currency to array
        currencies.push({ id, name, code, rate, paymentId, status });
        
        // Save to Firestore
        await db.collection('settings').doc('currencies').set({
            list: currencies
        });
        
        // Reload currencies
        loadCurrencies();
        
        // Close modal
        closeModal('addCurrencyModal');
        
        // Clear form
        document.getElementById('newCurrencyName').value = '';
        document.getElementById('newCurrencyCode').value = '';
        document.getElementById('newCurrencyRate').value = '';
        document.getElementById('newCurrencyPaymentId').value = '';
        document.getElementById('newCurrencyStatus').value = 'active';
        
        showToast('Currency added successfully', 'success');
    } catch (error) {
        console.error('Error adding currency:', error);
        showToast('Error adding currency', 'error');
    }
}

// Update currency
async function updateCurrency() {
    try {
        const id = document.getElementById('editCurrencyId').value;
        const name = document.getElementById('editCurrencyName').value.trim();
        const code = document.getElementById('editCurrencyCode').value.trim();
        const rate = parseFloat(document.getElementById('editCurrencyRate').value);
        const paymentId = document.getElementById('editCurrencyPaymentId').value.trim();
        const status = document.getElementById('editCurrencyStatus').value;
        
        if (!name || !code || isNaN(rate) || !paymentId) {
            showToast('Please fill all fields', 'error');
            return;
        }
        
        // Find currency index
        const index = currencies.findIndex(c => c.id === id);
        if (index === -1) {
            showToast('Currency not found', 'error');
            return;
        }
        
        // Update currency
        currencies[index] = { id, name, code, rate, paymentId, status };
        
        // Save to Firestore
        await db.collection('settings').doc('currencies').set({
            list: currencies
        });
        
        // Reload currencies
        loadCurrencies();
        
        // Close modal
        closeModal('editCurrencyModal');
        
        showToast('Currency updated successfully', 'success');
    } catch (error) {
        console.error('Error updating currency:', error);
        showToast('Error updating currency', 'error');
    }
}

// Delete currency
async function deleteCurrency(currencyId) {
    if (!confirm('Are you sure you want to delete this currency?')) {
        return;
    }
    
    try {
        // Find currency index
        const index = currencies.findIndex(c => c.id === currencyId);
        if (index === -1) {
            showToast('Currency not found', 'error');
            return;
        }
        
        // Remove currency
        currencies.splice(index, 1);
        
        // Save to Firestore
        await db.collection('settings').doc('currencies').set({
            list: currencies
        });
        
        // Reload currencies
        load
