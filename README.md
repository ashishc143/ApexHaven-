<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Luxury Hotel Booking</title>

<style>
body{
    margin:0;
    font-family: Arial, Helvetica, sans-serif;
    background:linear-gradient(120deg,#0f2027,#203a43,#2c5364);
}
.container{
    width:380px;
    margin:30px auto;
    background:white;
    padding:20px;
    border-radius:10px;
    box-shadow:0 15px 30px rgba(0,0,0,0.3);
}
h2{
    text-align:center;
    margin-top:0;
}
input,select,button{
    width:100%;
    padding:10px;
    margin:8px 0;
    border-radius:5px;
    border:1px solid #ccc;
    font-size:14px;
}
button{
    background:#2c5364;
    color:white;
    border:none;
    cursor:pointer;
    font-weight:bold;
}
button:hover{
    background:#203a43;
}
.link{
    text-align:center;
    color:blue;
    cursor:pointer;
    font-size:14px;
}
.hidden{display:none;}
.bill{
    background:#f2f2f2;
    padding:10px;
    border-radius:5px;
    font-size:14px;
}
.success{
    text-align:center;
}
</style>
</head>

<body>

<!-- REGISTER -->
<div class="container" id="register">
    <h2>Create Account</h2>
    <input type="text" id="regName" placeholder="Full Name" required>
    <input type="email" id="regEmail" placeholder="Email" required>
    <input type="password" id="regPass" placeholder="Password" required>
    <button onclick="register()">Register</button>
    <div class="link" onclick="showLogin()">Already account? Login</div>
</div>

<!-- LOGIN -->
<div class="container hidden" id="login">
    <h2>Login</h2>
    <input type="email" id="logEmail" placeholder="Email">
    <input type="password" id="logPass" placeholder="Password">
    <button onclick="login()">Login</button>
    <div class="link" onclick="showRegister()">Create account</div>
</div>

<!-- HOME / BOOKING -->
<div class="container hidden" id="home">
    <h2>Book Your Room</h2>

    <label>Room Type</label>
    <select id="room">
        <option value="1000">Standard - ₹1000/night</option>
        <option value="2000">Deluxe - ₹2000/night</option>
        <option value="3500">Suite - ₹3500/night</option>
    </select>

    <label>Check In</label>
    <input type="date" id="checkin">

    <label>Check Out</label>
    <input type="date" id="checkout">

    <button onclick="calculateBill()">Generate Bill</button>

    <div id="billBox" class="bill hidden"></div>
</div>

<!-- PAYMENT -->
<div class="container hidden" id="payment">
    <h2>Payment</h2>
    <input type="text" id="card" placeholder="Card Number (16 digit)">
    <input type="text" placeholder="Card Holder Name">
    <input type="text" id="cvv" placeholder="CVV">
    <button onclick="payNow()">Pay Now</button>
</div>

<!-- SUCCESS -->
<div class="container hidden" id="success">
    <div class="success">
        <h2>Booking Confirmed 🎉</h2>
        <p id="receipt"></p>
        <button onclick="logout()">Logout</button>
    </div>
</div>

<script>
let user = {};
let totalAmount = 0;
let nights = 0;

function hideAll(){
    document.querySelectorAll(".container").forEach(c=>c.classList.add("hidden"));
}

function showRegister(){ hideAll(); document.getElementById("register").classList.remove("hidden"); }
function showLogin(){ hideAll(); document.getElementById("login").classList.remove("hidden"); }
function showHome(){ hideAll(); document.getElementById("home").classList.remove("hidden"); }

function register(){
    user.name = document.getElementById("regName").value;
    user.email = document.getElementById("regEmail").value;
    user.pass = document.getElementById("regPass").value;

    if(user.name && user.email && user.pass){
        alert("Registration Successful");
        showLogin();
    }else{
        alert("Please fill all fields");
    }
}

function login(){
    let e = document.getElementById("logEmail").value;
    let p = document.getElementById("logPass").value;

    if(e === user.email && p === user.pass){
        alert("Login Successful");
        showHome();
    }else{
        alert("Invalid Details");
    }
}

function calculateBill(){
    let price = parseInt(document.getElementById("room").value);
    let inDate = new Date(document.getElementById("checkin").value);
    let outDate = new Date(document.getElementById("checkout").value);

    nights = (outDate - inDate) / (1000*60*60*24);

    if(nights > 0){
        totalAmount = price * nights;
        document.getElementById("billBox").classList.remove("hidden");
        document.getElementById("billBox").innerHTML =
            "Nights: " + nights + "<br>Total: ₹" + totalAmount +
            "<br><br><button onclick='goPayment()'>Proceed Payment</button>";
    }else{
        alert("Invalid dates");
    }
}

function goPayment(){
    hideAll();
    document.getElementById("payment").classList.remove("hidden");
}

function payNow(){
    let card = document.getElementById("card").value;
    let cvv = document.getElementById("cvv").value;

    if(card.length===16 && cvv.length>=3){
        hideAll();
        document.getElementById("success").classList.remove("hidden");

        document.getElementById("receipt").innerHTML =
        "Name: "+user.name+
        "<br>Email: "+user.email+
        "<br>Nights: "+nights+
        "<br>Paid: ₹"+totalAmount+
        "<br>Booking ID: HTL"+Math.floor(Math.random()*10000);
    }else{
        alert("Invalid card details");
    }
}

function logout(){
    location.reload();
}
</script>

</body>
</html>
