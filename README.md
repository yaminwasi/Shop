<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Wasi Premium Store</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Poppins',sans-serif;
}

body{
background:linear-gradient(135deg,#0f2027,#203a43,#2c5364);
color:white;
overflow-x:hidden;
}

header{
display:flex;
justify-content:space-between;
align-items:center;
padding:20px 40px;
background:rgba(0,0,0,0.3);
backdrop-filter:blur(10px);
position:sticky;
top:0;
z-index:1000;
}

.logo{
font-size:22px;
font-weight:700;
letter-spacing:1px;
}

.cart-icon{
cursor:pointer;
position:relative;
font-size:18px;
}

.cart-count{
position:absolute;
top:-8px;
right:-10px;
background:#ff4757;
font-size:12px;
padding:3px 7px;
border-radius:50%;
}

.container{
padding:40px;
display:grid;
grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
gap:30px;
}

.card{
background:rgba(255,255,255,0.08);
padding:20px;
border-radius:20px;
text-align:center;
transition:0.4s;
backdrop-filter:blur(15px);
}

.card:hover{
transform:translateY(-10px) scale(1.03);
box-shadow:0 20px 30px rgba(0,0,0,0.4);
}

.card img{
width:100%;
border-radius:15px;
}

.price{
color:#00ffcc;
margin:10px 0;
font-weight:600;
}

button{
background:#00ffcc;
color:black;
padding:8px 15px;
border:none;
border-radius:25px;
cursor:pointer;
transition:0.3s;
font-weight:600;
}

button:hover{
background:white;
}

.cart-panel{
position:fixed;
right:-400px;
top:0;
width:370px;
height:100%;
background:#111;
padding:20px;
transition:0.4s;
overflow-y:auto;
z-index:2000;
}

.cart-panel.active{
right:0;
}

.cart-item{
margin:15px 0;
border-bottom:1px solid gray;
padding-bottom:10px;
}

.qty-btn{
margin:0 5px;
padding:2px 8px;
border-radius:5px;
}

.remove-btn{
background:#ff4757;
color:white;
margin-top:5px;
}

.total{
margin-top:20px;
font-weight:600;
font-size:18px;
}

.payment-btn{
width:100%;
margin-top:10px;
}

.bkash{
background:#e2136e;
color:white;
}

.nagad{
background:#ff6600;
color:white;
}

.checkout{
background:#00ffcc;
color:black;
}

</style>
</head>
<body>

<header>
<div class="logo">Wasi Premium Store</div>
<div class="cart-icon" onclick="toggleCart()">
🛒 Cart
<span class="cart-count" id="cart-count">0</span>
</div>
</header>

<div class="container">

<div class="card">
<img src="https://via.placeholder.com/300">
<h3>Smart Watch</h3>
<p class="price">৳2500</p>
<button onclick="addToCart('Smart Watch',2500)">Add to Cart</button>
</div>

<div class="card">
<img src="https://via.placeholder.com/300">
<h3>Wireless Headphone</h3>
<p class="price">৳1800</p>
<button onclick="addToCart('Wireless Headphone',1800)">Add to Cart</button>
</div>

<div class="card">
<img src="https://via.placeholder.com/300">
<h3>Gaming Mouse</h3>
<p class="price">৳1200</p>
<button onclick="addToCart('Gaming Mouse',1200)">Add to Cart</button>
</div>

</div>

<div class="cart-panel" id="cart-panel">
<h2>Your Cart</h2>
<div id="cart-items"></div>
<p class="total">Total: ৳<span id="total-price">0</span></p>

<button class="payment-btn checkout" onclick="sendOrder()">Order via Gmail</button>
<button class="payment-btn bkash">Pay with bKash</button>
<button class="payment-btn nagad">Pay with Nagad</button>

</div>

<script>
let cart = JSON.parse(localStorage.getItem("cart")) || [];

function saveCart(){
localStorage.setItem("cart",JSON.stringify(cart));
}

function addToCart(name,price){
let existing = cart.find(item => item.name === name);
if(existing){
existing.qty += 1;
}else{
cart.push({name,price,qty:1});
}
updateCart();
}

function updateCart(){
let cartItems = document.getElementById("cart-items");
let total = 0;
cartItems.innerHTML = "";

cart.forEach((item,index)=>{
total += item.price * item.qty;

cartItems.innerHTML += `
<div class="cart-item">
<b>${item.name}</b><br>
৳${item.price} × ${item.qty}
<br>
<button class="qty-btn" onclick="changeQty(${index},1)">+</button>
<button class="qty-btn" onclick="changeQty(${index},-1)">-</button>
<br>
<button class="remove-btn" onclick="removeItem(${index})">Remove</button>
</div>
`;
});

document.getElementById("total-price").innerText = total;
document.getElementById("cart-count").innerText = cart.length;
saveCart();
}

function changeQty(index,change){
cart[index].qty += change;
if(cart[index].qty <= 0){
cart.splice(index,1);
}
updateCart();
}

function removeItem(index){
cart.splice(index,1);
updateCart();
}

function toggleCart(){
document.getElementById("cart-panel").classList.toggle("active");
}

function sendOrder(){
let orderText = "Order Details:\n\n";
cart.forEach(item=>{
orderText += item.name + " x " + item.qty + "\n";
});
orderText += "\nTotal: ৳" + document.getElementById("total-price").innerText;

window.location.href = "mailto:yourgmail@gmail.com?subject=New Order&body=" + encodeURIComponent(orderText);
}

updateCart();
</script>

</body>
</html>
