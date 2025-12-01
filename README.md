<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Admin Panel</title>
<style>
    body{font-family:Arial;background:#f2f2f2;margin:0;padding:0}
    .login-box,.admin-box{
        width:350px;margin:70px auto;background:#fff;padding:25px;
        border-radius:10px;box-shadow:0 0 10px rgba(0,0,0,0.2);
    }
    input{width:100%;padding:10px;margin:8px 0;border:1px solid #ccc;border-radius:5px}
    button{padding:10px 16px;background:#007bff;color:#fff;border:none;border-radius:6px;cursor:pointer}
    button:hover{background:#0056b3}
    .order-item{
        background:#fff;margin:10px 0;padding:15px;border-radius:8px;
        box-shadow:0 0 5px rgba(0,0,0,0.1)
    }
</style>
</head>
<body>

<!-- LOGIN SCREEN -->
<div id="login" class="login-box">
    <h3>Admin Login</h3>
    <input id="email" placeholder="Email">
    <input id="password" type="password" placeholder="Password">
    <button onclick="login()">Login</button>
</div>

<!-- ADMIN PANEL -->
<div id="admin" class="admin-box" style="display:none;">
    <h3>Admin Panel</h3>
    <button onclick="logout()">Logout</button>
    <button onclick="exportOrders()">Export Orders</button>
    <button onclick="clearOrders()" style="background:red">Clear Orders</button>

    <h3>Orders List</h3>
    <div id="orders"></div>
</div>

<!-- FIREBASE -->
<script src="https://www.gstatic.com/firebasejs/9.6.10/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.10/firebase-auth-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.6.10/firebase-firestore-compat.js"></script>

<script>
// ---------------------------------------
// 🔥 Firebase Config (এটা নিজেরটা বসাবেন)
// ---------------------------------------
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_DOMAIN",
    projectId: "YOUR_PROJECT",
    storageBucket: "YOUR_BUCKET",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};

firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();
const db = firebase.firestore();

let currentUser = null;

// ---------------------------------------
// LOGIN
// ---------------------------------------
function login() {
    let email = document.getElementById("email").value;
    let pass  = document.getElementById("password").value;

    auth.signInWithEmailAndPassword(email, pass)
        .then(() => {
            showAdminPanel();
        })
        .catch(err => alert(err.message));
}

function logout() {
    auth.signOut();
}

// ---------------------------------------
// UI CHANGE
// ---------------------------------------
function showLoginScreen(){
    document.getElementById("login").style.display = "block";
    document.getElementById("admin").style.display = "none";
}

function showAdminPanel(){
    document.getElementById("login").style.display = "none";
    document.getElementById("admin").style.display = "block";
    loadOrders();
}

// ---------------------------------------
// LOAD ORDERS
// ---------------------------------------
function loadOrders(){
    db.collection("orders").orderBy("createdAt","desc").onSnapshot(snapshot=>{
        let html = "";
        snapshot.forEach(doc=>{
            let o = doc.data();
            html += `
                <div class="order-item">
                    <b>Name:</b> ${o.name}<br>
                    <b>Dollar:</b> ${o.amount}<br>
                    <b>Method:</b> ${o.method}<br>
                    <b>Phone:</b> ${o.phone}<br>
                </div>
            `;
        });
        document.getElementById("orders").innerHTML = html;
    });
}

// ---------------------------------------
// EXPORT ORDERS (আপনার পাঠানো কোড - একই)
// ---------------------------------------
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

// ---------------------------------------
// CLEAR ORDERS (আপনার কোড - ঠিক আছে)
// ---------------------------------------
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

// ---------------------------------------
// CHECK LOGIN STATUS
// ---------------------------------------
window.addEventListener('load', () => {
  auth.onAuthStateChanged((user) => {
    if (user) {
      currentUser = user;
      showAdminPanel();
    } else {
      currentUser = null;
      showLoginScreen();
    }
  });
});
</script>

</body>
</html>
