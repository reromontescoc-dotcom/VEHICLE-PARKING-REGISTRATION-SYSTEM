<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vehicle Parking System</title>

<style>

/* BODY BACKGROUND */
body {
  font-family: Arial, sans-serif;
  max-width: 800px;
  margin: 2rem auto;
  padding: 1rem;
  min-height: 100vh;

  background-image: url("https://images.unsplash.com/photo-1506521781263-d8422e82f27a");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

/* DARK OVERLAY */
body::before{
  content:"";
  position:fixed;
  top:0;
  left:0;
  width:100%;
  height:100%;
  background:rgba(0,0,0,0.55);
  z-index:-1;
}

/* MENU CARDS */
.menu{
  margin:1.5rem 0;
  padding:1.5rem;
  border-radius:15px;
  background:rgba(255,255,255,0.9);
  backdrop-filter: blur(6px);
  box-shadow:0 10px 25px rgba(0,0,0,0.4);
}

/* HEADINGS */
/* WHITE TITLE FOR H1 */
h1{
  color:white;
  text-align:center;
  font-size:40px;
  letter-spacing:2px;
  text-shadow:2px 2px 10px rgba(0,0,0,0.7);
}

/* H2 NORMAL */
h2{
  color:#2c3e50;
  margin-bottom:1rem;
  text-align:center;
}

/* BUTTONS */
button{
  background:#3498db;
  color:white;
  border:none;
  padding:12px;
  border-radius:8px;
  cursor:pointer;
  margin:6px 0;
  transition:0.3s;
  width:100%;
  font-size:15px;
}

button:hover{
  background:#2980b9;
  transform:scale(1.03);
}

/* INPUT */
input{
  width:100%;
  padding:10px;
  margin:5px 0;
  border-radius:6px;
  border:1px solid #ccc;
}

/* MESSAGES */
.error{ color:red; margin-top:5px; }
.success{ color:green; margin-top:5px; }

/* VEHICLE RECORD */
.record{
  border-bottom:1px solid #ddd;
  padding:8px;
}

</style>
</head>
<body>

<h1>🚗 VEHICLE PARKING SYSTEM</h1>

<!-- MAIN MENU -->
<div id="mainMenu" class="menu">
<h2>Main Menu</h2>
<button onclick="showSection('adminLogin')">Admin Login</button>
<button onclick="showSection('customerRegister')">Customer Register</button>
<button onclick="showSection('customerLogin')">Customer Login</button>
<button onclick="exitSystem()">Exit</button>
</div>

<!-- ADMIN LOGIN -->
<div id="adminLogin" class="menu" style="display:none;">
<h2>Admin Login</h2>
<input type="password" id="adminPass" placeholder="Enter admin password">
<div id="adminError" class="error"></div>
<button onclick="adminLogin()">Login</button>
<button onclick="showSection('mainMenu')">Back</button>
</div>

<!-- ADMIN DASHBOARD -->
<div id="adminDashboard" class="menu" style="display:none;">
<h2>Admin Dashboard</h2>
<button onclick="viewVehicles()">View Vehicles</button>
<button onclick="showSection('mainMenu')">Logout</button>
<div id="vehicleRecords"></div>
</div>

<!-- CUSTOMER REGISTER -->
<div id="customerRegister" class="menu" style="display:none;">
<h2>Customer Registration</h2>
<input type="text" id="regUser" placeholder="Enter username">
<input type="password" id="regPass" placeholder="Enter password">
<div id="regMsg"></div>
<button onclick="registerCustomer()">Register</button>
<button onclick="showSection('mainMenu')">Back</button>
</div>

<!-- CUSTOMER LOGIN -->
<div id="customerLogin" class="menu" style="display:none;">
<h2>Customer Login</h2>
<input type="text" id="loginUser" placeholder="Username">
<input type="password" id="loginPass" placeholder="Password">
<div id="loginMsg" class="error"></div>
<button onclick="loginCustomer()">Login</button>
<button onclick="showSection('mainMenu')">Back</button>
</div>

<!-- CUSTOMER DASHBOARD -->
<div id="customerDashboard" class="menu" style="display:none;">
<h2>Customer Menu</h2>
<button onclick="showSection('vehicleForm')">Register Vehicle</button>
<button onclick="showSection('mainMenu')">Logout</button>
</div>

<!-- VEHICLE REGISTRATION -->
<div id="vehicleForm" class="menu" style="display:none;">
<h2>Vehicle Registration</h2>

<input type="text" id="vName" placeholder="Full Name">
<input type="text" id="vAddress" placeholder="Address">
<input type="text" id="vPhone" placeholder="Phone">
<input type="text" id="vBrand" placeholder="Brand">
<input type="text" id="vModel" placeholder="Model">
<input type="number" id="vYear" placeholder="Year">
<input type="text" id="vPlate" placeholder="Plate Number">
<input type="date" id="vDate">

<p><b>Parking Fee: PHP 20</b></p>

<label><input type="radio" id="payYes" name="payment" value="yes" checked> Pay</label>
<label><input type="radio" id="payNo" name="payment" value="no"> No Pay</label>

<div id="vMsg"></div>

<button onclick="registerVehicle()">Submit Registration</button>
<button onclick="showSection('customerDashboard')">Back</button>
</div>

<script>

/* DATA STORAGE */
function getData(key){ return JSON.parse(localStorage.getItem(key)) || []; }
function setData(key,data){ localStorage.setItem(key, JSON.stringify(data)); }

/* NAVIGATION */
function showSection(id){
document.querySelectorAll('.menu').forEach(x=>x.style.display='none');
let el=document.getElementById(id);
if(el) el.style.display='block';
document.querySelectorAll('.error,.success').forEach(m=>m.textContent='');
}

/* EXIT */
function exitSystem(){
if(confirm("Exit system?")) location.reload();
}

/* ADMIN LOGIN */
function adminLogin(){
let pass=document.getElementById('adminPass').value;
if(pass==="admin123"){
showSection('adminDashboard');
}else{
document.getElementById('adminError').textContent="Wrong password!";
}
}

/* VIEW VEHICLES */
function viewVehicles(){

let vehicles=getData('vehicles');
let box=document.getElementById('vehicleRecords');
box.innerHTML="";

if(vehicles.length===0){
box.innerHTML="No vehicles registered.";
return;
}

vehicles.forEach(v=>{
let div=document.createElement('div');
div.className="record";

div.innerHTML = ${v.name} | ${v.brand} ${v.model} | ${v.plate} | ${v.date} | Fee: PHP ${v.fee} | ${v.paid ? "Paid" : "Not Paid"};

box.appendChild(div);
});

}

/* REGISTER CUSTOMER */
function registerCustomer(){

let u=document.getElementById('regUser').value.trim();
let p=document.getElementById('regPass').value.trim();
let msg=document.getElementById('regMsg');

let users=getData('users');

msg.textContent="";
msg.className="";

if(!u||!p){
msg.textContent="Fill all fields";
msg.className="error";
return;
}

if(users.find(x=>x.u===u)){
msg.textContent="Username already exists";
msg.className="error";
return;
}

users.push({u,p});
setData('users',users);

msg.textContent="Registered successfully!";
msg.className="success";

regUser.value="";
regPass.value="";

}

/* LOGIN CUSTOMER */
function loginCustomer(){

let u=document.getElementById('loginUser').value.trim();
let p=document.getElementById('loginPass').value.trim();

let users=getData('users');

if(users.find(x=>x.u===u&&x.p===p)){
showSection('customerDashboard');
}else{
loginMsg.textContent="Invalid username or password";
}

}

/* REGISTER VEHICLE */
function registerVehicle(){

let v={

name:vName.value.trim(),
address:vAddress.value.trim(),
phone:vPhone.value.trim(),
brand:vBrand.value.trim(),
model:vModel.value.trim(),
year:vYear.value,
plate:vPlate.value.trim(),
date:vDate.value,
fee:20,
paid:document.querySelector('input[name="payment"]:checked').value==="yes"

};

let msg=document.getElementById('vMsg');

msg.textContent="";
msg.className="";

if(Object.values(v).includes("")||!v.year){
msg.textContent="Fill all fields";
msg.className="error";
return;
}

if(!v.paid){
msg.textContent="Payment required!";
msg.className="error";
return;
}

let vehicles=getData('vehicles');

vehicles.push(v);

setData('vehicles',vehicles);

msg.textContent="Vehicle registered successfully!";
msg.className="success";

/* CLEAR FORM */

document.querySelectorAll('#vehicleForm input').forEach(i=>{
if(i.type!="radio") i.value="";
});

payYes.checked=true;

}

/* INIT */

showSection('mainMenu');

</script>

</body>
</html# VEHICLE-PARKING-REGISTRATION-SYSTEM
