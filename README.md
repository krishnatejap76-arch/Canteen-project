<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CampusBite | College Canteen</title>

<style>
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

body {
    font-family: Arial, sans-serif;
    background: #f6f7fb;
    color: #202124;
}

button {
    border: none;
    cursor: pointer;
}

.header {
    background: #ffffff;
    padding: 16px 7%;
    display: flex;
    justify-content: space-between;
    align-items: center;
    box-shadow: 0 2px 10px rgba(0,0,0,.08);
    position: sticky;
    top: 0;
    z-index: 10;
}

.logo {
    font-size: 22px;
    font-weight: bold;
    color: #ff5a36;
}

.logo span {
    color: #222;
}

.header-buttons button {
    background: transparent;
    margin-left: 10px;
    padding: 8px 12px;
}

.hero {
    padding: 45px 7%;
    background: linear-gradient(135deg, #fff3ed, #ffffff);
}

.hero h1 {
    font-size: clamp(30px, 5vw, 52px);
    margin-bottom: 12px;
}

.hero h1 span {
    color: #ff5a36;
}

.hero p {
    color: #666;
    max-width: 600px;
    line-height: 1.6;
}

.status {
    margin-top: 20px;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    background: #e9f9ef;
    color: #16843a;
    padding: 10px 15px;
    border-radius: 25px;
    font-size: 14px;
}

.container {
    width: 86%;
    max-width: 1200px;
    margin: 30px auto;
}

.section-title {
    margin-bottom: 18px;
}

.categories {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    margin-bottom: 25px;
}

.category {
    padding: 10px 16px;
    border-radius: 25px;
    background: white;
    border: 1px solid #eee;
}

.category.active {
    background: #ff5a36;
    color: white;
}

.menu {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 20px;
}

.card {
    background: white;
    border-radius: 18px;
    overflow: hidden;
    box-shadow: 0 5px 20px rgba(0,0,0,.06);
    transition: .2s;
}

.card:hover {
    transform: translateY(-4px);
}

.food-image {
    height: 150px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 65px;
    background: #fff0e9;
}

.card-body {
    padding: 17px;
}

.card-body h3 {
    margin-bottom: 7px;
}

.description {
    color: #777;
    font-size: 14px;
    min-height: 38px;
}

.price {
    font-size: 20px;
    font-weight: bold;
    margin: 12px 0;
}

.add {
    width: 100%;
    background: #ff5a36;
    color: white;
    padding: 11px;
    border-radius: 10px;
}

.cart {
    position: fixed;
    right: 20px;
    bottom: 20px;
    background: #222;
    color: white;
    padding: 14px 20px;
    border-radius: 30px;
    box-shadow: 0 5px 20px rgba(0,0,0,.2);
    z-index: 20;
}

.modal {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,.55);
    display: none;
    justify-content: center;
    align-items: center;
    padding: 20px;
    z-index: 50;
}

.modal-box {
    background: white;
    width: 100%;
    max-width: 450px;
    padding: 25px;
    border-radius: 20px;
    max-height: 90vh;
    overflow-y: auto;
}

.modal-box h2 {
    margin-bottom: 20px;
}

.input {
    width: 100%;
    padding: 13px;
    border: 1px solid #ddd;
    border-radius: 10px;
    margin: 8px 0 15px;
}

.primary {
    width: 100%;
    background: #ff5a36;
    color: white;
    padding: 13px;
    border-radius: 10px;
    margin-top: 5px;
}

.secondary {
    width: 100%;
    background: #eee;
    padding: 12px;
    border-radius: 10px;
    margin-top: 10px;
}

.role-buttons {
    display: grid;
    grid-template-columns: repeat(2,1fr);
    gap: 10px;
    margin-bottom: 15px;
}

.role {
    padding: 12px;
    background: #f4f4f4;
    border-radius: 10px;
}

.role.selected {
    background: #ff5a36;
    color: white;
}

.cart-item {
    display: flex;
    justify-content: space-between;
    padding: 12px 0;
    border-bottom: 1px solid #eee;
}

.total {
    font-size: 20px;
    font-weight: bold;
    margin: 18px 0;
}

.admin-panel {
    display: none;
    background: white;
    padding: 25px;
    border-radius: 18px;
    margin-top: 25px;
}

.admin-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 15px 0;
    border-bottom: 1px solid #eee;
}

.small-btn {
    padding: 8px 12px;
    border-radius: 8px;
    background: #222;
    color: white;
}

.warning {
    background: #fff3cd;
    color: #765800;
    padding: 14px;
    border-radius: 10px;
    margin-top: 15px;
    display: none;
}

.footer {
    text-align: center;
    padding: 40px;
    color: #777;
}

/* Mobile */
@media(max-width:600px) {
    .header {
        padding: 14px 5%;
    }

    .container {
        width: 92%;
    }

    .hero {
        padding: 35px 5%;
    }

    .header-buttons button {
        font-size: 12px;
        padding: 6px;
    }
}
</style>
</head>

<body>

<header class="header">
    <div class="logo">🍴 Campus<span>Bite</span></div>

    <div class="header-buttons">
        <button onclick="openLogin()">Login</button>
        <button onclick="openAdmin()">⚙️ Admin</button>
    </div>
</header>

<section class="hero">
    <h1>Your food.<br><span>Your time.</span></h1>

    <p>
        Pre-book your college canteen food, skip the queue,
        and collect it at your selected pickup time.
    </p>

    <div class="status" id="locationStatus">
        ● Checking campus location...
    </div>

    <div class="warning" id="outsideWarning">
        📍 You appear to be outside the campus.
        Pre-booking is currently unavailable.
    </div>
</section>

<main class="container">

    <h2 class="section-title">Today's Menu</h2>

    <div class="categories">
        <button class="category active" onclick="filterMenu('All',this)">All</button>
        <button class="category" onclick="filterMenu('Breakfast',this)">Breakfast</button>
        <button class="category" onclick="filterMenu('Meals',this)">Meals</button>
        <button class="category" onclick="filterMenu('Snacks',this)">Snacks</button>
        <button class="category" onclick="filterMenu('Drinks',this)">Drinks</button>
    </div>

    <div class="menu" id="menu"></div>

    <section class="admin-panel" id="adminPanel">
        <h2>⚙️ Admin Dashboard</h2>
        <p style="margin:10px 0 20px;color:#777;">
            Prototype menu management
        </p>

        <div id="adminItems"></div>
    </section>

</main>

<button class="cart" onclick="openCart()">
    🛒 Cart (<span id="cartCount">0</span>)
</button>

<footer class="footer">
    CampusBite © 2026 · College Canteen
</footer>


<!-- LOGIN MODAL -->
<div class="modal" id="loginModal">
    <div class="modal-box">

        <h2>Welcome 👋</h2>

        <div class="role-buttons">
            <button class="role selected" id="studentRole"
                onclick="selectRole('student')">
                🎓 Student
            </button>

            <button class="role" id="facultyRole"
                onclick="selectRole('faculty')">
                👨‍🏫 Faculty
            </button>
        </div>

        <input
            class="input"
            id="phone"
            placeholder="Enter mobile number"
            maxlength="10"
        >

        <button class="primary" onclick="sendOTP()">
            Send 4-Digit OTP
        </button>

        <div id="otpSection" style="display:none;margin-top:15px;">
            <input
                class="input"
                id="otp"
                placeholder="Enter 4-digit OTP"
                maxlength="4"
            >

            <button class="primary" onclick="verifyOTP()">
                Verify OTP
            </button>
        </div>

        <button class="secondary" onclick="closeModals()">
            Cancel
        </button>

    </div>
</div>


<!-- CART MODAL -->
<div class="modal" id="cartModal">
    <div class="modal-box">

        <h2>🛒 Your Cart</h2>

        <div id="cartItems"></div>

        <div class="total">
            Total: ₹<span id="cartTotal">0</span>
        </div>

        <button class="primary" onclick="checkout()">
            Continue to Payment
        </button>

        <button class="secondary" onclick="closeModals()">
            Continue Shopping
        </button>

    </div>
</div>


<!-- PAYMENT MODAL -->
<div class="modal" id="paymentModal">
    <div class="modal-box">

        <h2>💳 Payment</h2>

        <p style="color:#777;margin-bottom:15px;">
            This is a development/demo payment screen.
        </p>

        <div class="total">
            Amount: ₹<span id="paymentTotal">0</span>
        </div>

        <button class="primary" onclick="makeDemoPayment()">
            Pay ₹<span id="payAmount">0</span>
        </button>

        <button class="secondary" onclick="closeModals()">
            Cancel
        </button>

    </div>
</div>


<!-- ADMIN LOGIN -->
<div class="modal" id="adminModal">
    <div class="modal-box">

        <h2>🔐 Admin Login</h2>

        <input
            class="input"
            id="adminUser"
            placeholder="Admin username"
        >

        <input
            class="input"
            id="adminPassword"
            type="password"
            placeholder="Admin password"
        >

        <button class="primary" onclick="adminLogin()">
            Login
        </button>

        <button class="secondary" onclick="closeModals()">
            Cancel
        </button>

    </div>
</div>


<script>

let currentRole = "student";

let campusAllowed = true;

let cart = [];

let menuItems = [
    {
        id: 1,
        name: "Idli",
        description: "Soft idli served with chutney",
        price: 30,
        category: "Breakfast",
        emoji: "🥣",
        available: true
    },
    {
        id: 2,
        name: "Masala Dosa",
        description: "Crispy dosa with potato masala",
        price: 50,
        category: "Breakfast",
        emoji: "🥞",
        available: true
    },
    {
        id: 3,
        name: "Veg Meals",
        description: "Complete vegetarian college meal",
        price: 80,
        category: "Meals",
        emoji: "🍛",
        available: true
    },
    {
        id: 4,
        name: "Fried Rice",
        description: "Vegetable fried rice",
        price: 70,
        category: "Meals",
        emoji: "🍚",
        available: true
    },
    {
        id: 5,
        name: "Samosa",
        description: "Crispy potato samosa",
        price: 20,
        category: "Snacks",
        emoji: "🥟",
        available: true
    },
    {
        id: 6,
        name: "Tea",
        description: "Hot campus tea",
        price: 15,
        category: "Drinks",
        emoji: "☕",
        available: true
    }
];


// ---------------- MENU ----------------

function renderMenu(category = "All") {

    const menu = document.getElementById("menu");

    menu.innerHTML = "";

    menuItems
        .filter(item =>
            category === "All" || item.category === category
        )
        .forEach(item => {

            if (!item.available) return;

            menu.innerHTML += `
                <div class="card">

                    <div class="food-image">
                        ${item.emoji}
                    </div>

                    <div class="card-body">

                        <h3>${item.name}</h3>

                        <p class="description">
                            ${item.description}
                        </p>

                        <div class="price">
                            ₹${item.price}
                        </div>

                        <button
                            class="add"
                            onclick="addToCart(${item.id})">
                            + Add
                        </button>

                    </div>

                </div>
            `;
        });
}

function filterMenu(category, button) {

    document
        .querySelectorAll(".category")
        .forEach(b => b.classList.remove("active"));

    button.classList.add("active");

    renderMenu(category);
}


// ---------------- CART ----------------

function addToCart(id) {

    if (!campusAllowed) {
        alert("You must be inside the campus to pre-book.");
        return;
    }

    const item = menuItems.find(x => x.id === id);

    const existing = cart.find(x => x.id === id);

    if (existing) {
        existing.quantity++;
    } else {
        cart.push({
            ...item,
            quantity: 1
        });
    }

    updateCart();

    alert(item.name + " added to cart!");
}

function updateCart() {

    let count = 0;

    cart.forEach(item => {
        count += item.quantity;
    });

    document.getElementById("cartCount").innerText = count;
}

function openCart() {

    if (cart.length === 0) {
        alert("Your cart is empty.");
        return;
    }

    let html = "";
    let total = 0;

    cart.forEach(item => {

        let itemTotal = item.price * item.quantity;

        total += itemTotal;

        html += `
            <div class="cart-item">
                <span>
                    ${item.emoji}
                    ${item.name} × ${item.quantity}
                </span>

                <strong>
                    ₹${itemTotal}
                </strong>
            </div>
        `;
    });

    document.getElementById("cartItems").innerHTML = html;
    document.getElementById("cartTotal").innerText = total;

    document.getElementById("cartModal").style.display = "flex";
}

function checkout() {

    if (!campusAllowed) {
        alert("Pre-booking is available only inside campus.");
        return;
    }

    let total = cart.reduce(
        (sum,item) => sum + item.price * item.quantity,
        0
    );

    document.getElementById("paymentTotal").innerText = total;
    document.getElementById("payAmount").innerText = total;

    document.getElementById("cartModal").style.display = "none";
    document.getElementById("paymentModal").style.display = "flex";
}


// ---------------- DEMO PAYMENT ----------------

function makeDemoPayment() {

    let orderNumber =
        "CB" + Math.floor(100000 + Math.random() * 900000);

    alert(
        "✓ DEMO PAYMENT SUCCESSFUL\n\n" +
        "Order: " + orderNumber +
        "\n\nThis is only a prototype."
    );

    cart = [];

    updateCart();

    closeModals();
}


// ---------------- LOGIN ----------------

function openLogin() {
    document.getElementById("loginModal").style.display = "flex";
}

function selectRole(role) {

    currentRole = role;

    document
        .getElementById("studentRole")
        .classList.remove("selected");

    document
        .getElementById("facultyRole")
        .classList.remove("selected");

    document
        .getElementById(
            role === "student"
                ? "studentRole"
                : "facultyRole"
        )
        .classList.add("selected");
}

function sendOTP() {

    const phone =
        document.getElementById("phone").value;

    if (!/^[0-9]{10}$/.test(phone)) {
        alert("Enter a valid 10-digit mobile number.");
        return;
    }

    /*
       DEVELOPMENT ONLY.

       In the real application:
       Phone → Backend → SMS provider → OTP

       Never generate/verify a real OTP only
       inside frontend JavaScript.
    */

    alert("Development OTP: 1234");

    document.getElementById("otpSection").style.display = "block";
}

function verifyOTP() {

    const otp =
        document.getElementById("otp").value;

    if (otp !== "1234") {
        alert("Incorrect OTP.");
        return;
    }

    alert(
        "✓ Phone verified!\n\n" +
        currentRole.toUpperCase() +
        " login successful."
    );

    closeModals();
}


// ---------------- ADMIN ----------------

function openAdmin() {
    document.getElementById("adminModal").style.display = "flex";
}

function adminLogin() {

    const username =
        document.getElementById("adminUser").value;

    const password =
        document.getElementById("adminPassword").value;

    /*
       DEMO ONLY.

       DO NOT use these credentials
       in a real production application.
    */

    if (username === "admin" && password === "admin123") {

        document.getElementById("adminModal").style.display = "none";

        document.getElementById("adminPanel").style.display = "block";

        renderAdmin();

        window.scrollTo({
            top: document.body.scrollHeight,
            behavior: "smooth"
        });

    } else {
        alert("Invalid demo admin credentials.");
    }
}

function renderAdmin() {

    const container =
        document.getElementById("adminItems");

    container.innerHTML = "";

    menuItems.forEach(item => {

        container.innerHTML += `

            <div class="admin-item">

                <div>
                    <strong>${item.emoji} ${item.name}</strong>
                    <br>
                    ₹${item.price}
                </div>

                <button
                    class="small-btn"
                    onclick="changePrice(${item.id})">
                    Change Price
                </button>

            </div>
        `;
    });
}

function changePrice(id) {

    const item =
        menuItems.find(x => x.id === id);

    const newPrice =
        prompt(
            "Enter new price for " +
            item.name,
            item.price
        );

    if (newPrice === null) return;

    const price = Number(newPrice);

    if (!Number.isFinite(price) || price < 0) {
        alert("Invalid price.");
        return;
    }

    item.price = price;

    renderMenu();

    renderAdmin();

    alert("Price updated in this demo.");
}


// ---------------- LOCATION ----------------

function checkCampusLocation() {

    const status =
        document.getElementById("locationStatus");

    const warning =
        document.getElementById("outsideWarning");

    if (!navigator.geolocation) {

        status.innerText =
            "● Location unavailable";

        campusAllowed = false;

        warning.style.display = "block";

        return;
    }

    navigator.geolocation.getCurrentPosition(

        position => {

            /*
               IMPORTANT:

               These coordinates are placeholders.

               Replace them with your actual
               college campus coordinates later.

               This prototype uses a simple
               distance calculation.
            */

            const campusLat = 16.000000;
            const campusLng = 80.000000;

            const distance =
                getDistance(
                    position.coords.latitude,
                    position.coords.longitude,
                    campusLat,
                    campusLng
                );

            /*
               Example campus radius:
               500 metres.

               Replace this after determining
               your actual campus boundary.
            */

            const radius = 500;

            if (distance <= radius) {

                campusAllowed = true;

                status.innerText =
                    "● You are inside campus";

                status.style.background =
                    "#e9f9ef";

                status.style.color =
                    "#16843a";

                warning.style.display = "none";

            } else {

                campusAllowed = false;

                status.innerText =
                    "● Outside campus";

                status.style.background =
                    "#ffe9e9";

                status.style.color =
                    "#b00020";

                warning.style.display = "block";
            }

        },

        error => {

            /*
               If location permission is denied,
               don't automatically grant access.
            */

            campusAllowed = false;

            status.innerText =
                "● Location permission required";

            warning.style.display = "block";
        }
    );
}

function getDistance(lat1, lon1, lat2, lon2) {

    const R = 6371000;

    const p1 = lat1 * Math.PI / 180;
    const p2 = lat2 * Math.PI / 180;

    const dp =
        (lat2 - lat1) * Math.PI / 180;

    const dl =
        (lon2 - lon1) * Math.PI / 180;

    const a =
        Math.sin(dp / 2) ** 2 +
        Math.cos(p1) *
        Math.cos(p2) *
        Math.sin(dl / 2) ** 2;

    const c =
        2 * Math.atan2(
            Math.sqrt(a),
            Math.sqrt(1 - a)
        );

    return R * c;
}


// ---------------- MODALS ----------------

function closeModals() {

    document
        .querySelectorAll(".modal")
        .forEach(modal => {
            modal.style.display = "none";
        });
}


// ---------------- START ----------------

renderMenu();

checkCampusLocation();

</script>

</body>
</html>
