<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#111827">
<title>Campus Canteen — Pre-Booking</title>

<!--
  CAMPUS CANTEEN — SINGLE FILE GITHUB PAGES BUILD
  ------------------------------------------------
  This file is a functional FRONT-END prototype.

  Demo credentials:
    Student/Faculty sign-in:
      Student ID: STU1001
      Faculty ID: FAC1001
      OTP: 1234
      Password: create any password during first sign-in

    Admin:
      ID: admin
      Password: admin123

  IMPORTANT FOR REAL COLLEGE DEPLOYMENT:
  A browser-only HTML file cannot securely enforce:
    - real SMS OTP
    - permanent one-ID/one-phone binding
    - real online payment verification
    - trusted admin authorization
    - server-side QR validation
  Those must be moved to a backend/database before accepting real money.
  The UI and flow below are structured so those services can be connected later.
-->

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">

<!-- QR generator + camera scanner libraries. GitHub Pages can load these over HTTPS. -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<script src="https://unpkg.com/html5-qrcode" defer></script>

<style>
:root{
  --bg:#f4f7fb;--card:#fff;--text:#111827;--muted:#6b7280;
  --primary:#2563eb;--primary2:#1d4ed8;--success:#16a34a;--danger:#dc2626;
  --warning:#d97706;--border:#e5e7eb;--shadow:0 12px 35px rgba(15,23,42,.08);
  --radius:18px;
}
*{box-sizing:border-box}
body{margin:0;font-family:Inter,system-ui,sans-serif;background:var(--bg);color:var(--text)}
button,input,select{font:inherit}
button{cursor:pointer}
.hidden{display:none!important}
.topbar{position:sticky;top:0;z-index:20;background:#111827;color:#fff;padding:14px 18px;box-shadow:0 4px 18px #0002}
.topbar-inner{max-width:1180px;margin:auto;display:flex;align-items:center;justify-content:space-between;gap:12px}
.brand{font-weight:800;letter-spacing:-.4px}
.brand small{display:block;color:#9ca3af;font-size:10px;font-weight:500;margin-top:2px}
.nav-actions{display:flex;gap:8px;flex-wrap:wrap}
.btn{border:0;border-radius:12px;padding:11px 15px;font-weight:700;transition:.15s}
.btn:hover{transform:translateY(-1px)}
.btn-primary{background:var(--primary);color:#fff}.btn-primary:hover{background:var(--primary2)}
.btn-dark{background:#1f2937;color:#fff}.btn-light{background:#fff;color:#111827;border:1px solid var(--border)}
.btn-danger{background:#fee2e2;color:#991b1b}.btn-success{background:#dcfce7;color:#166534}
.btn-warning{background:#fef3c7;color:#92400e}
.container{max-width:1180px;margin:auto;padding:24px 16px 50px}
.hero{background:linear-gradient(135deg,#111827,#1e3a8a);color:#fff;border-radius:26px;padding:30px;margin-bottom:20px;box-shadow:var(--shadow)}
.hero h1{margin:0 0 8px;font-size:clamp(28px,5vw,44px);letter-spacing:-1.5px}
.hero p{color:#dbeafe;max-width:700px;line-height:1.6;margin:0}
.grid{display:grid;grid-template-columns:repeat(12,1fr);gap:16px}
.col-12{grid-column:span 12}.col-8{grid-column:span 8}.col-6{grid-column:span 6}.col-4{grid-column:span 4}
.card{background:var(--card);border:1px solid var(--border);border-radius:var(--radius);padding:20px;box-shadow:0 7px 24px rgba(15,23,42,.04)}
.card h2,.card h3{margin-top:0}
.muted{color:var(--muted)}
.center{text-align:center}
.badge{display:inline-flex;align-items:center;border-radius:999px;padding:5px 9px;font-size:12px;font-weight:800;background:#eff6ff;color:#1d4ed8}
.badge.green{background:#dcfce7;color:#166534}.badge.red{background:#fee2e2;color:#991b1b}.badge.orange{background:#ffedd5;color:#9a3412}
.notice{padding:12px 14px;border-radius:12px;background:#eff6ff;color:#1e40af;border:1px solid #bfdbfe;line-height:1.5}
.notice.warn{background:#fffbeb;color:#92400e;border-color:#fde68a}
.notice.danger{background:#fef2f2;color:#991b1b;border-color:#fecaca}
.form-group{margin-bottom:14px}.form-group label{display:block;font-size:13px;font-weight:700;margin-bottom:7px}
.input{width:100%;padding:12px 13px;border:1px solid #d1d5db;border-radius:12px;outline:none;background:#fff}
.input:focus{border-color:var(--primary);box-shadow:0 0 0 3px #2563eb1a}
.auth-shell{min-height:calc(100vh - 65px);display:grid;place-items:center;padding:22px}
.auth-card{width:min(470px,100%);background:#fff;border:1px solid var(--border);border-radius:24px;padding:24px;box-shadow:var(--shadow)}
.tabs{display:flex;gap:6px;background:#f3f4f6;padding:5px;border-radius:13px;margin-bottom:18px}
.tab{flex:1;border:0;background:transparent;padding:10px;border-radius:9px;font-weight:800;color:#6b7280}
.tab.active{background:#fff;color:#111827;box-shadow:0 2px 8px #0000000d}
.role-row{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:16px}
.role-btn{border:1px solid var(--border);background:#fff;border-radius:12px;padding:12px;font-weight:800}
.role-btn.active{border-color:#2563eb;background:#eff6ff;color:#1d4ed8}
.menu-toolbar{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:16px}
.search{flex:1;min-width:220px}
.categories{display:flex;gap:8px;overflow:auto;padding-bottom:4px}
.cat{border:1px solid var(--border);background:#fff;border-radius:999px;padding:9px 13px;font-weight:700;white-space:nowrap}
.cat.active{background:#111827;color:#fff;border-color:#111827}
.food-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(215px,1fr));gap:14px}
.food{border:1px solid var(--border);border-radius:16px;overflow:hidden;background:#fff;display:flex;flex-direction:column}
.food-img{height:130px;background:linear-gradient(135deg,#dbeafe,#fef3c7);display:grid;place-items:center;font-size:45px}
.food-body{padding:14px;display:flex;flex-direction:column;gap:8px;flex:1}
.food-name{font-weight:800}.food-desc{font-size:12px;color:var(--muted);line-height:1.4}.food-bottom{margin-top:auto;display:flex;align-items:center;justify-content:space-between;gap:8px}
.price{font-weight:800}
.cart-list{display:flex;flex-direction:column;gap:10px}.cart-row{display:flex;align-items:center;justify-content:space-between;gap:8px;border-bottom:1px solid var(--border);padding-bottom:10px}
.qty{display:flex;align-items:center;gap:6px}.qty button{width:28px;height:28px;border:1px solid var(--border);border-radius:8px;background:#fff}
.total{font-size:21px;font-weight:900;display:flex;justify-content:space-between;margin:16px 0}
.table-wrap{overflow:auto}.table{width:100%;border-collapse:collapse}.table th,.table td{text-align:left;padding:12px;border-bottom:1px solid var(--border);font-size:13px;white-space:nowrap}
.kpis{display:grid;grid-template-columns:repeat(4,1fr);gap:12px}.kpi{background:#f8fafc;border:1px solid var(--border);border-radius:14px;padding:15px}.kpi strong{display:block;font-size:25px;margin-top:5px}
.qr-box{display:grid;place-items:center;padding:20px;border:1px dashed #cbd5e1;border-radius:18px;background:#f8fafc}
#qrcode img,#qrcode canvas{margin:auto!important}
.ticket{max-width:460px;margin:0 auto}.ticket-head{display:flex;justify-content:space-between;gap:12px;align-items:start}.ticket-id{font-weight:900;font-size:22px}
.scanner-box{min-height:300px;border:2px dashed #94a3b8;border-radius:18px;display:grid;place-items:center;padding:16px}
.scan-result{border-radius:18px;padding:18px;margin-top:14px}
.scan-valid{background:#dcfce7;border:1px solid #86efac;color:#166534}.scan-invalid{background:#fee2e2;border:1px solid #fca5a5;color:#991b1b}
.empty{text-align:center;padding:35px;color:var(--muted)}
.footer{text-align:center;color:#9ca3af;font-size:12px;margin-top:35px}
.modal{position:fixed;inset:0;background:#0008;z-index:100;display:grid;place-items:center;padding:16px}
.modal-card{background:#fff;border-radius:22px;padding:22px;width:min(540px,100%);max-height:90vh;overflow:auto}
.close-row{display:flex;justify-content:space-between;align-items:center}
.toast{position:fixed;right:16px;bottom:16px;background:#111827;color:#fff;padding:13px 16px;border-radius:12px;box-shadow:var(--shadow);z-index:200;max-width:360px}
.small{font-size:12px}
@media(max-width:850px){.col-8,.col-6,.col-4{grid-column:span 12}.kpis{grid-template-columns:repeat(2,1fr)}}
@media(max-width:520px){.container{padding:14px 10px 40px}.hero{padding:22px}.auth-shell{padding:12px}.kpis{grid-template-columns:1fr 1fr}.topbar-inner{align-items:flex-start}}
</style>
</head>
<body>

<header class="topbar">
  <div class="topbar-inner">
    <div class="brand">🍱 Campus Canteen <small>Pre-book • Pay • Scan • Collect</small></div>
    <div class="nav-actions" id="navActions"></div>
  </div>
</header>

<main id="app"></main>

<div id="modal" class="modal hidden">
  <div class="modal-card" id="modalCard"></div>
</div>
<div id="toast" class="toast hidden"></div>

<script>
/* =========================
   CONFIGURATION
   ========================= */
const CONFIG = {
  campusName: "My College",
  // Replace these with the college campus coordinates for the final version.
  campus: { lat: 16.000000, lng: 80.000000, radiusMeters: 500 },
  demoOtp: "1234",
  adminId: "admin",
  adminPassword: "admin123",
  currency: "₹"
};

/* =========================
   DEMO DATABASE
   ========================= */
const defaultUsers = {
  "STU1001": {
    id:"STU1001", type:"student", name:"Demo Student",
    phone:"", phoneVerified:false, password:"", registered:false
  },
  "FAC1001": {
    id:"FAC1001", type:"faculty", name:"Demo Faculty",
    phone:"", phoneVerified:false, password:"", registered:false
  }
};

const defaultMenu = [
  {id:"F001",name:"Veg Fried Rice",desc:"Fresh vegetables with seasoned rice.",price:70,cat:"Rice",available:true,emoji:"🍚"},
  {id:"F002",name:"Chicken Biryani",desc:"Aromatic biryani served hot.",price:120,cat:"Meals",available:true,emoji:"🍛"},
  {id:"F003",name:"Masala Dosa",desc:"Crispy dosa with chutney and sambar.",price:60,cat:"Breakfast",available:true,emoji:"🥞"},
  {id:"F004",name:"Idli Sambar",desc:"Soft idlis with hot sambar.",price:45,cat:"Breakfast",available:true,emoji:"🥣"},
  {id:"F005",name:"Veg Noodles",desc:"Stir-fried noodles with vegetables.",price:65,cat:"Noodles",available:true,emoji:"🍜"},
  {id:"F006",name:"Samosa",desc:"Crispy snack with spiced filling.",price:20,cat:"Snacks",available:true,emoji:"🥟"},
  {id:"F007",name:"Tea",desc:"Hot campus tea.",price:15,cat:"Drinks",available:true,emoji:"☕"},
  {id:"F008",name:"Fresh Lime",desc:"Refreshing lime drink.",price:25,cat:"Drinks",available:true,emoji:"🍋"}
];

const DB = {
  get users(){ return JSON.parse(localStorage.getItem("cc_users") || JSON.stringify(defaultUsers)); },
  set users(v){ localStorage.setItem("cc_users", JSON.stringify(v)); },
  get menu(){ return JSON.parse(localStorage.getItem("cc_menu") || JSON.stringify(defaultMenu)); },
  set menu(v){ localStorage.setItem("cc_menu", JSON.stringify(v)); },
  get orders(){ return JSON.parse(localStorage.getItem("cc_orders") || "[]"); },
  set orders(v){ localStorage.setItem("cc_orders", JSON.stringify(v)); },
  get session(){ return JSON.parse(sessionStorage.getItem("cc_session") || "null"); },
  set session(v){ if(v) sessionStorage.setItem("cc_session",JSON.stringify(v)); else sessionStorage.removeItem("cc_session"); }
};

let state = {
  authRole: "student",
  authMode: "login",
  otpTarget: null,
  cart: {},
  search: "",
  category: "All",
  currentTicket: null,
  scanner: null
};

/* =========================
   HELPERS
   ========================= */
const $ = id => document.getElementById(id);
const money = n => CONFIG.currency + Number(n).toFixed(2);

function toast(msg){
  $("toast").textContent = msg;
  $("toast").classList.remove("hidden");
  clearTimeout(window.__toast);
  window.__toast = setTimeout(()=>$("toast").classList.add("hidden"),3000);
}
function esc(s){ return String(s).replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#039;"}[c])); }
function uid(prefix="ORD"){
  return prefix + "-" + Date.now().toString(36).toUpperCase() + "-" + Math.random().toString(36).slice(2,8).toUpperCase();
}
function currentUser(){ return DB.session; }
function isAdmin(){ return DB.session?.role === "admin"; }

function showModal(html){
  $("modalCard").innerHTML = html;
  $("modal").classList.remove("hidden");
}
function closeModal(){
  $("modal").classList.add("hidden");
  if(state.scanner){ try{state.scanner.stop();}catch(e){} state.scanner=null; }
}
$("modal").addEventListener("click", e=>{if(e.target.id==="modal") closeModal();});

function distanceMeters(lat1,lon1,lat2,lon2){
  const R=6371000, p=Math.PI/180;
  const a=.5-Math.cos((lat2-lat1)*p)/2+
    Math.cos(lat1*p)*Math.cos(lat2*p)*(1-Math.cos((lon2-lon1)*p))/2;
  return 2*R*Math.asin(Math.sqrt(a));
}

async function campusCheck(){
  if(!navigator.geolocation) return {ok:false,reason:"Location is unavailable on this device."};
  return new Promise(resolve=>{
    navigator.geolocation.getCurrentPosition(pos=>{
      const d=distanceMeters(CONFIG.campus.lat,CONFIG.campus.lng,pos.coords.latitude,pos.coords.longitude);
      resolve({ok:d<=CONFIG.campus.radiusMeters,distance:Math.round(d)});
    },()=>resolve({ok:false,reason:"Location permission was denied or unavailable."}),{
      enableHighAccuracy:true,timeout:8000,maximumAge:30000
    });
  });
}

/* =========================
   AUTH
   ========================= */
function renderAuth(){
  $("navActions").innerHTML = `<button class="btn btn-light" onclick="openAdminLogin()">Admin Login</button>`;
  $("app").innerHTML = `
  <section class="auth-shell">
    <div class="auth-card">
      <div class="center">
        <div style="font-size:45px">🍱</div>
        <h1 style="margin:8px 0 5px">Campus Canteen</h1>
        <p class="muted">Secure pre-booking for students and faculty</p>
      </div>

      <div class="tabs">
        <button class="tab ${state.authMode==="login"?"active":""}" onclick="setAuthMode('login')">Login</button>
        <button class="tab ${state.authMode==="signup"?"active":""}" onclick="setAuthMode('signup')">Sign In</button>
      </div>

      <div class="role-row">
        <button class="role-btn ${state.authRole==="student"?"active":""}" onclick="setAuthRole('student')">🎓 Student</button>
        <button class="role-btn ${state.authRole==="faculty"?"active":""}" onclick="setAuthRole('faculty')">👨‍🏫 Faculty</button>
      </div>

      <div class="notice small">
        ${state.authMode==="signup"
          ? "Your college ID is the permanent identity. During real deployment, the backend will bind exactly one verified phone number to that ID."
          : "Every login requires a fresh OTP after the ID and password are accepted."}
      </div>

      <form onsubmit="authSubmit(event)" style="margin-top:17px">
        <div class="form-group">
          <label>${state.authRole==="student"?"Student ID":"Faculty ID"}</label>
          <input class="input" id="authId" placeholder="${state.authRole==="student"?"STU1001":"FAC1001"}" required>
        </div>

        ${state.authMode==="signup" ? `
          <div class="form-group">
            <label>Mobile Number</label>
            <input class="input" id="authPhone" type="tel" inputmode="numeric" placeholder="10-digit mobile number" maxlength="10" required>
          </div>
        ` : `
          <div class="form-group">
            <label>Password</label>
            <input class="input" id="authPassword" type="password" placeholder="Your password" required>
          </div>
        `}

        <button class="btn btn-primary" style="width:100%">
          ${state.authMode==="signup"?"Continue to OTP":"Login & Verify OTP"}
        </button>
      </form>

      <p class="small muted center" style="margin-bottom:0">
        Demo OTP: <b>${CONFIG.demoOtp}</b> · Demo IDs: STU1001 / FAC1001
      </p>
    </div>
  </section>`;
}

function setAuthMode(mode){state.authMode=mode;renderAuth();}
function setAuthRole(role){state.authRole=role;renderAuth();}

function authSubmit(e){
  e.preventDefault();
  const id=$("authId").value.trim().toUpperCase();
  const users=DB.users;
  const expectedPrefix=state.authRole==="student"?"STU":"FAC";

  if(!id.startsWith(expectedPrefix)){
    toast(`Use a valid ${state.authRole} ID format.`);
    return;
  }

  if(state.authMode==="signup"){
    const phone=$("authPhone").value.replace(/\D/g,"");
    if(phone.length!==10){toast("Enter a valid 10-digit mobile number.");return;}

    if(users[id] && users[id].registered){
      toast("This ID is already registered. Use Login.");
      return;
    }

    // In production: this OTP must be sent by an SMS provider.
    state.otpTarget={mode:"signup",id,role:state.authRole,phone};
    showOtpModal();
  }else{
    const user=users[id];
    if(!user || !user.registered){toast("Account not found. Please use Sign In first.");return;}
    if(user.type!==state.authRole){toast("This ID belongs to a different login section.");return;}
    if(user.password!==$("authPassword").value){toast("Incorrect ID or password.");return;}

    // In production: OTP is sent to user.phone by SMS provider.
    state.otpTarget={mode:"login",id,role:user.type,phone:user.phone};
    showOtpModal();
  }
}

function showOtpModal(){
  showModal(`
    <div class="close-row"><h2>Verify mobile number</h2><button class="btn btn-light" onclick="closeModal()">✕</button></div>
    <p class="muted">Enter the 4-digit OTP sent to <b>${esc(state.otpTarget.phone)}</b>.</p>
    <div class="notice small">Demo mode OTP: <b>${CONFIG.demoOtp}</b>. Real deployment must send this through an SMS provider.</div>
    <div class="form-group" style="margin-top:15px">
      <label>4-digit OTP</label>
      <input class="input" id="otpInput" inputmode="numeric" maxlength="4" placeholder="1234">
    </div>
    <button class="btn btn-primary" style="width:100%" onclick="verifyOtp()">Verify OTP</button>
  `);
}

function verifyOtp(){
  const otp=$("otpInput").value.trim();
  if(otp!==CONFIG.demoOtp){toast("Incorrect OTP.");return;}

  const t=state.otpTarget;
  const users=DB.users;

  if(t.mode==="signup"){
    // Permanent ID -> one phone binding in this demo storage.
    users[t.id]={
      id:t.id,type:t.role,name:t.id==="STU1001"?"Demo Student":"Demo Faculty",
      phone:t.phone,phoneVerified:true,password:"",registered:false
    };
    DB.users=users;

    showModal(`
      <div class="close-row"><h2>OTP verified ✓</h2><button class="btn btn-light" onclick="closeModal()">✕</button></div>
      <p class="muted">Your verified phone is now linked to <b>${esc(t.id)}</b>. Set your account password.</p>
      <div class="form-group">
        <label>Create password</label>
        <input class="input" id="newPassword" type="password" minlength="6" placeholder="Minimum 6 characters">
      </div>
      <div class="form-group">
        <label>Confirm password</label>
        <input class="input" id="newPassword2" type="password" minlength="6" placeholder="Repeat password">
      </div>
      <button class="btn btn-primary" style="width:100%" onclick="finishSignup()">Create Account</button>
    `);
  }else{
    closeModal();
    DB.session={id:t.id,role:t.role,name:users[t.id].name};
    state.cart={};
    renderApp();
    toast("Login successful.");
  }
}

function finishSignup(){
  const p=$("newPassword").value,p2=$("newPassword2").value;
  if(p.length<6){toast("Password must be at least 6 characters.");return;}
  if(p!==p2){toast("Passwords do not match.");return;}
  const users=DB.users;
  users[state.otpTarget.id].password=p;
  users[state.otpTarget.id].registered=true;
  DB.users=users;
  closeModal();
  state.authMode="login";
  renderAuth();
  toast("Account created. Return to Login and use ID + password + OTP.");
}

function openAdminLogin(){
  showModal(`
    <div class="close-row"><h2>Admin / Counter Login</h2><button class="btn btn-light" onclick="closeModal()">✕</button></div>
    <div class="notice warn small">For real deployment, replace the demo admin credentials with secure server-side admin authentication.</div>
    <div class="form-group" style="margin-top:15px"><label>Admin ID</label><input class="input" id="adminId" value="admin"></div>
    <div class="form-group"><label>Password</label><input class="input" id="adminPw" type="password" value="admin123"></div>
    <button class="btn btn-primary" style="width:100%" onclick="adminLogin()">Login</button>
  `);
}
function adminLogin(){
  if($("adminId").value===CONFIG.adminId && $("adminPw").value===CONFIG.adminPassword){
    DB.session={id:"ADMIN",role:"admin",name:"Canteen Admin"};
    closeModal();renderApp();toast("Admin login successful.");
  }else toast("Invalid admin credentials.");
}
function logout(){DB.session=null;state.cart={};renderAuth();}

/* =========================
   MAIN APP
   ========================= */
function renderApp(){
  if(!DB.session){renderAuth();return;}
  if(isAdmin()) renderAdmin();
  else renderStudentFaculty();
  renderNav();
}
function renderNav(){
  $("navActions").innerHTML=`
    <span class="btn btn-dark">${esc(DB.session.name)} · ${esc(DB.session.role)}</span>
    <button class="btn btn-light" onclick="logout()">Logout</button>
  `;
}

function renderStudentFaculty(){
  const menu=DB.menu;
  const cats=["All",...new Set(menu.map(x=>x.cat))];
  const filtered=menu.filter(x=>
    (state.category==="All"||x.cat===state.category) &&
    (x.name.toLowerCase().includes(state.search.toLowerCase()) || x.desc.toLowerCase().includes(state.search.toLowerCase()))
  );

  $("app").innerHTML=`
    <div class="container">
      <section class="hero">
        <h1>Order before you reach the counter.</h1>
        <p>Search the live menu, add food to your cart, pay online, receive a QR ticket, then show it at the canteen counter for fast collection.</p>
      </section>

      <div class="grid">
        <section class="card col-8">
          <div style="display:flex;justify-content:space-between;gap:10px;align-items:start;flex-wrap:wrap">
            <div>
              <h2 style="margin-bottom:4px">Available Menu</h2>
              <div class="muted small">Prices and availability are controlled by the admin.</div>
            </div>
            <button class="btn btn-light" onclick="checkCampus()">📍 Check Campus</button>
          </div>

          <div class="menu-toolbar" style="margin-top:16px">
            <input class="input search" value="${esc(state.search)}" placeholder="Search food..." oninput="state.search=this.value;renderStudentFaculty()">
          </div>

          <div class="categories">
            ${cats.map(c=>`<button class="cat ${state.category===c?"active":""}" onclick="state.category='${esc(c)}';renderStudentFaculty()">${esc(c)}</button>`).join("")}
          </div>

          <div class="food-grid" style="margin-top:15px">
            ${filtered.length?filtered.map(foodCard).join(""):`<div class="empty col-12">No matching items.</div>`}
          </div>
        </section>

        <aside class="card col-4" id="cartCard">
          ${cartHTML()}
        </aside>

        <section class="card col-12">
          <h2>My Orders</h2>
          ${ordersHTML()}
        </section>
      </div>

      <div class="footer">Campus Canteen prototype · Secure production backend required before real payments.</div>
    </div>
  `;
}
function foodCard(f){
  return `<article class="food">
    <div class="food-img">${f.emoji}</div>
    <div class="food-body">
      <div class="food-name">${esc(f.name)}</div>
      <div class="food-desc">${esc(f.desc)}</div>
      <div><span class="badge">${esc(f.cat)}</span></div>
      <div class="food-bottom">
        <span class="price">${money(f.price)}</span>
        ${f.available
          ? `<button class="btn btn-primary" onclick="addCart('${f.id}')">Add</button>`
          : `<span class="badge red">Unavailable</span>`}
      </div>
    </div>
  </article>`;
}
function addCart(id){
  state.cart[id]=(state.cart[id]||0)+1;
  renderStudentFaculty();
  toast("Added to cart.");
}
function changeQty(id,delta){
  state.cart[id]=(state.cart[id]||0)+delta;
  if(state.cart[id]<=0) delete state.cart[id];
  renderStudentFaculty();
}
function cartItems(){
  return Object.entries(state.cart).map(([id,q])=>{
    const f=DB.menu.find(x=>x.id===id);
    return f?{...f,qty:q}:null;
  }).filter(Boolean);
}
function cartTotal(){return cartItems().reduce((s,x)=>s+x.price*x.qty,0);}
function cartHTML(){
  const items=cartItems();
  if(!items.length) return `<h2>Your Cart</h2><div class="empty">Your cart is empty.<br>Add food from the menu.</div>`;
  return `<h2>Your Cart</h2>
    <div class="cart-list">${items.map(x=>`
      <div class="cart-row">
        <div><b>${esc(x.name)}</b><div class="small muted">${money(x.price)} × ${x.qty}</div></div>
        <div class="qty">
          <button onclick="changeQty('${x.id}',-1)">−</button><b>${x.qty}</b><button onclick="changeQty('${x.id}',1)">+</button>
        </div>
      </div>`).join("")}</div>
    <div class="total"><span>Total</span><span>${money(cartTotal())}</span></div>
    <button class="btn btn-primary" style="width:100%" onclick="startCheckout()">Book & Pay Online</button>
    <div class="notice warn small" style="margin-top:10px">Demo payment is simulated. Connect a payment gateway + server verification before using real money.</div>`;
}

async function checkCampus(){
  toast("Checking campus location…");
  const r=await campusCheck();
  if(r.ok) toast(`You are inside the campus area (${r.distance} m).`);
  else toast(r.reason || `You are outside the campus area (${r.distance} m).`);
}

/* =========================
   CHECKOUT / PAYMENT
   ========================= */
async function startCheckout(){
  const items=cartItems();
  if(!items.length){toast("Cart is empty.");return;}

  const loc=await campusCheck();
  if(!loc.ok){
    showModal(`<div class="close-row"><h2>Booking unavailable</h2><button class="btn btn-light" onclick="closeModal()">✕</button></div>
      <div class="notice danger">Pre-booking is available only inside the college campus.</div>
      <p class="muted small">${esc(loc.reason||"Your current location is outside the campus boundary.")}</p>`);
    return;
  }

  const total=cartTotal();
  showModal(`
    <div class="close-row"><h2>Confirm Booking</h2><button class="btn btn-light" onclick="closeModal()">✕</button></div>
    <div class="card" style="padding:14px;margin:12px 0;background:#f8fafc">
      ${items.map(x=>`<div style="display:flex;justify-content:space-between;margin:7px 0"><span>${esc(x.name)} × ${x.qty}</span><b>${money(x.price*x.qty)}</b></div>`).join("")}
      <hr style="border:0;border-top:1px solid #e5e7eb">
      <div style="display:flex;justify-content:space-between;font-size:19px;font-weight:900"><span>Total</span><span>${money(total)}</span></div>
    </div>
    <div class="notice small">Payment gateway placeholder: click below to simulate a successful online payment.</div>
    <button class="btn btn-primary" style="width:100%;margin-top:12px" onclick="simulatePayment()">Pay ${money(total)}</button>
  `);
}

function simulatePayment(){
  // Production: create payment order on server, redirect to gateway, verify server-side,
  // then create the QR ticket only after verified payment.
  closeModal();
  const user=DB.session;
  const items=cartItems();
  const total=cartTotal();
  const order={
    id:uid("ORD"),
    qrToken:uid("TKT"),
    userId:user.id,
    userName:user.name,
    items:items.map(x=>({id:x.id,name:x.name,qty:x.qty,price:x.price})),
    total,
    paymentStatus:"PAID",
    status:"PAID",
    createdAt:new Date().toISOString(),
    collectedAt:null
  };
  const orders=DB.orders;
  orders.unshift(order);
  DB.orders=orders;
  state.cart={};
  state.currentTicket=order.id;
  renderStudentFaculty();
  openTicket(order.id);
}

function ordersHTML(){
  const mine=DB.orders.filter(o=>o.userId===DB.session.id);
  if(!mine.length) return `<div class="empty">No orders yet.</div>`;
  return `<div class="table-wrap"><table class="table"><thead><tr><th>Order</th><th>Items</th><th>Total</th><th>Payment</th><th>Status</th><th>Action</th></tr></thead><tbody>
    ${mine.map(o=>`<tr>
      <td><b>${esc(o.id)}</b><div class="small muted">${new Date(o.createdAt).toLocaleString()}</div></td>
      <td>${o.items.map(x=>esc(x.name)+" × "+x.qty).join(", ")}</td>
      <td>${money(o.total)}</td>
      <td><span class="badge green">${esc(o.paymentStatus)}</span></td>
      <td><span class="badge ${o.status==="COLLECTED"?"green":"orange"}">${esc(o.status)}</span></td>
      <td>${o.status!=="COLLECTED"?`<button class="btn btn-light" onclick="openTicket('${o.id}')">Show QR</button>`:"—"}</td>
    </tr>`).join("")}
  </tbody></table></div>`;
}

/* =========================
   QR TICKET
   ========================= */
function openTicket(orderId){
  const order=DB.orders.find(o=>o.id===orderId && o.userId===DB.session.id);
  if(!order){toast("Order not found.");return;}
  showModal(`
    <div class="close-row"><h2>Order QR Ticket</h2><button class="btn btn-light" onclick="closeModal()">✕</button></div>
    <div class="ticket">
      <div class="ticket-head">
        <div><div class="small muted">ORDER NUMBER</div><div class="ticket-id">${esc(order.id)}</div></div>
        <span class="badge ${order.status==="COLLECTED"?"green":"orange"}">${esc(order.status)}</span>
      </div>
      <div class="qr-box" style="margin-top:16px">
        <div id="qrcode"></div>
        <div class="small muted center" style="margin-top:10px">Show this QR to the canteen counter.</div>
      </div>
      <div class="card" style="margin-top:14px;background:#f8fafc">
        ${order.items.map(x=>`<div style="display:flex;justify-content:space-between;padding:6px 0"><span>${esc(x.name)} × ${x.qty}</span><b>${money(x.price*x.qty)}</b></div>`).join("")}
        <hr style="border:0;border-top:1px solid #e5e7eb">
        <div style="display:flex;justify-content:space-between;font-weight:900"><span>Total Paid</span><span>${money(order.total)}</span></div>
      </div>
      <div class="notice small" style="margin-top:12px">
        QR contains only a random order token in this prototype. The counter should validate the token against the server before delivering food.
      </div>
    </div>
  `);
  setTimeout(()=>{
    if(window.QRCode){
      new QRCode($("qrcode"),{text:order.qrToken,width:220,height:220,correctLevel:QRCode.CorrectLevel.M});
    }
  },50);
}

/* =========================
   ADMIN / COUNTER
   ========================= */
function renderAdmin(){
  const orders=DB.orders;
  const paid=orders.filter(o=>o.paymentStatus==="PAID").length;
  const collected=orders.filter(o=>o.status==="COLLECTED").length;
  const revenue=orders.reduce((s,o)=>s+Number(o.total||0),0);

  $("app").innerHTML=`
  <div class="container">
    <section class="hero">
      <h1>Admin & Counter</h1>
      <p>Manage the menu and use the QR scanner to validate paid orders and confirm collection.</p>
    </section>

    <div class="kpis">
      <div class="kpi">Orders<strong>${orders.length}</strong></div>
      <div class="kpi">Paid<strong>${paid}</strong></div>
      <div class="kpi">Collected<strong>${collected}</strong></div>
      <div class="kpi">Revenue<strong>${money(revenue)}</strong></div>
    </div>

    <div class="grid" style="margin-top:16px">
      <section class="card col-6">
        <h2>Counter QR Scanner</h2>
        <p class="muted small">The canteen operator only needs this screen: scan → see order → deliver → confirm.</p>
        <button class="btn btn-primary" style="width:100%;padding:15px" onclick="openScanner()">📷 Scan Student QR</button>
        <button class="btn btn-light" style="width:100%;margin-top:8px" onclick="manualToken()">Enter QR Token Manually</button>
      </section>

      <section class="card col-6">
        <h2>Menu Management</h2>
        <p class="muted small">Change price, availability, name and category.</p>
        <button class="btn btn-primary" onclick="openMenuManager()">Manage Menu</button>
      </section>

      <section class="card col-12">
        <h2>Orders</h2>
        ${adminOrdersHTML()}
      </section>
    </div>

    <div class="footer">Admin controls are demo-only in this single-file build. Secure production authorization must be server-side.</div>
  </div>`;
}

function adminOrdersHTML(){
  if(!DB.orders.length) return `<div class="empty">No orders.</div>`;
  return `<div class="table-wrap"><table class="table"><thead><tr><th>Order</th><th>Customer</th><th>Items</th><th>Total</th><th>Payment</th><th>Status</th><th>Action</th></tr></thead><tbody>
  ${DB.orders.map(o=>`<tr>
    <td><b>${esc(o.id)}</b><div class="small muted">${new Date(o.createdAt).toLocaleString()}</div></td>
    <td>${esc(o.userId)}<div class="small muted">${esc(o.userName)}</div></td>
    <td>${o.items.map(x=>esc(x.name)+" × "+x.qty).join(", ")}</td>
    <td>${money(o.total)}</td>
    <td><span class="badge green">${esc(o.paymentStatus)}</span></td>
    <td><span class="badge ${o.status==="COLLECTED"?"green":"orange"}">${esc(o.status)}</span></td>
    <td>${o.status!=="COLLECTED"?`<button class="btn btn-light" onclick="openOrderForCounter('${o.id}')">Open</button>`:"✓ Done"}</td>
  </tr>`).join("")}
  </tbody></table></div>`;
}

function openScanner(){
  showModal(`
    <div class="close-row"><h2>Scan Order QR</h2><button class="btn btn-light" onclick="closeModal()">✕</button></div>
    <div id="reader" style="width:100%;margin-top:12px"></div>
    <div class="notice small" style="margin-top:12px">Allow camera access. On GitHub Pages, the camera works only over HTTPS and depends on browser permissions.</div>
  `);

  setTimeout(()=>{
    if(!window.Html5Qrcode){
      toast("Scanner library is still loading. Try again.");
      return;
    }
    state.scanner=new Html5Qrcode("reader");
    state.scanner.start(
      {facingMode:"environment"},
      {fps:10,qrbox:{width:250,height:250}},
      decodedText=>{
        try{state.scanner.stop();}catch(e){}
        state.scanner=null;
        closeModal();
        validateQr(decodedText);
      },
      ()=>{}
    ).catch(err=>toast("Camera could not start: "+err));
  },250);
}

function manualToken(){
  showModal(`
    <div class="close-row"><h2>Manual QR Token</h2><button class="btn btn-light" onclick="closeModal()">✕</button></div>
    <p class="muted small">This is a backup for camera problems.</p>
    <input class="input" id="manualQr" placeholder="Paste / type token">
    <button class="btn btn-primary" style="width:100%;margin-top:12px" onclick="manualValidate()">Validate</button>
  `);
}
function manualValidate(){
  const t=$("manualQr").value.trim();
  closeModal();validateQr(t);
}

function validateQr(token){
  // IMPORTANT: Production must send this token to the backend.
  // Never trust an order object supplied by the browser.
  const order=DB.orders.find(o=>o.qrToken===token);
  if(!order){
    showScanResult(false,null,"Invalid QR token.");
    return;
  }
  if(order.paymentStatus!=="PAID"){
    showScanResult(false,order,"Payment is not verified.");
    return;
  }
  if(order.status==="COLLECTED"){
    showScanResult(false,order,"This order has already been collected.");
    return;
  }
  showScanResult(true,order,"Valid paid order. Deliver the listed items, then confirm.");
}

function showScanResult(valid,order,message){
  showModal(`
    <div class="close-row"><h2>${valid?"✅ VALID ORDER":"❌ DO NOT DELIVER"}</h2><button class="btn btn-light" onclick="closeModal();renderAdmin()">✕</button></div>
    <div class="scan-result ${valid?"scan-valid":"scan-invalid"}">
      <h3 style="margin:0 0 7px">${esc(message)}</h3>
      ${order?`
        <div style="margin-top:12px">
          <b>Order:</b> ${esc(order.id)}<br>
          <b>Customer ID:</b> ${esc(order.userId)}<br>
          <b>Items:</b> ${order.items.map(x=>esc(x.name)+" × "+x.qty).join(", ")}<br>
          <b>Total:</b> ${money(order.total)}
        </div>`:""}
    </div>
    ${valid?`<button class="btn btn-success" style="width:100%;margin-top:12px;padding:14px" onclick="confirmCollection('${order.id}')">Food Delivered — Confirm Collection</button>`:""}
  `);
}

function confirmCollection(orderId){
  const orders=DB.orders;
  const idx=orders.findIndex(o=>o.id===orderId);
  if(idx<0){toast("Order not found.");return;}
  if(orders[idx].status==="COLLECTED"){toast("Already collected.");return;}
  orders[idx].status="COLLECTED";
  orders[idx].collectedAt=new Date().toISOString();
  DB.orders=orders;
  closeModal();renderAdmin();toast("Order marked COLLECTED.");
}

function openOrderForCounter(orderId){
  const o=DB.orders.find(x=>x.id===orderId);
  if(!o)return;
  showScanResult(o.paymentStatus==="PAID"&&o.status!=="COLLECTED",o,
    o.status==="COLLECTED"?"Already collected.":"Valid paid order. Deliver and confirm.");
}

/* =========================
   MENU ADMIN
   ========================= */
function openMenuManager(){
  const menu=DB.menu;
  showModal(`
    <div class="close-row"><h2>Menu Manager</h2><button class="btn btn-light" onclick="closeModal()">✕</button></div>
    <button class="btn btn-primary" onclick="openAddFood()" style="margin:8px 0 14px">+ Add Item</button>
    <div class="table-wrap"><table class="table"><thead><tr><th>Item</th><th>Category</th><th>Price</th><th>Available</th><th>Action</th></tr></thead><tbody>
      ${menu.map(f=>`<tr>
        <td>${f.emoji} <b>${esc(f.name)}</b></td>
        <td>${esc(f.cat)}</td>
        <td>${money(f.price)}</td>
        <td>${f.available?'<span class="badge green">Yes</span>':'<span class="badge red">No</span>'}</td>
        <td><button class="btn btn-light" onclick="editFood('${f.id}')">Edit</button></td>
      </tr>`).join("")}
    </tbody></table></div>
  `);
}
function openAddFood(){
  showModal(foodFormHTML(null));
}
function foodFormHTML(f){
  f=f||{id:"",name:"",desc:"",price:"",cat:"Snacks",available:true,emoji:"🍽️"};
  return `<div class="close-row"><h2>${f.id?"Edit Item":"Add Item"}</h2><button class="btn btn-light" onclick="openMenuManager()">✕</button></div>
    <div class="form-group"><label>Name</label><input class="input" id="fName" value="${esc(f.name)}"></div>
    <div class="form-group"><label>Description</label><input class="input" id="fDesc" value="${esc(f.desc)}"></div>
    <div class="grid">
      <div class="form-group col-6"><label>Price</label><input class="input" id="fPrice" type="number" min="0" value="${f.price}"></div>
      <div class="form-group col-6"><label>Category</label><input class="input" id="fCat" value="${esc(f.cat)}"></div>
    </div>
    <div class="form-group"><label>Emoji</label><input class="input" id="fEmoji" value="${esc(f.emoji)}"></div>
    <label style="display:flex;gap:8px;align-items:center;margin-bottom:15px"><input id="fAvailable" type="checkbox" ${f.available?"checked":""}> Available for booking</label>
    <button class="btn btn-primary" style="width:100%" onclick="saveFood('${f.id}')">Save Item</button>`;
}
function editFood(id){
  const f=DB.menu.find(x=>x.id===id);if(f)showModal(foodFormHTML(f));
}
function saveFood(id){
  const name=$("fName").value.trim(),desc=$("fDesc").value.trim(),price=Number($("fPrice").value);
  const cat=$("fCat").value.trim()||"Other",emoji=$("fEmoji").value.trim()||"🍽️",available=$("fAvailable").checked;
  if(!name||price<0){toast("Enter a valid name and price.");return;}
  const menu=DB.menu;
  if(id){
    const f=menu.find(x=>x.id===id);
    Object.assign(f,{name,desc,price,cat,emoji,available});
  }else{
    menu.push({id:uid("F"),name,desc,price,cat,emoji,available});
  }
  DB.menu=menu;
  openMenuManager();
  renderAdmin();
  toast("Menu updated.");
}

/* =========================
   RESET / DEMO TOOLS
   ========================= */
function resetDemo(){
  if(confirm("Reset demo data on this browser?")){
    localStorage.removeItem("cc_users");
    localStorage.removeItem("cc_menu");
    localStorage.removeItem("cc_orders");
    sessionStorage.removeItem("cc_session");
    location.reload();
  }
}

/* Start */
if(DB.session) renderApp(); else renderAuth();
</script>
</body>
</html>
