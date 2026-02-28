<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Wasi Pro Store</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif;}
body{background:#0f0f1a;color:white;}
header{position:fixed;width:100%;top:0;left:0;
display:flex;justify-content:space-between;
padding:20px 40px;background:rgba(0,0,0,0.4);
backdrop-filter:blur(10px);z-index:1000;}
.logo{font-weight:700;}
nav a{color:white;margin-left:20px;text-decoration:none;}
.cart-icon{cursor:pointer;position:relative;}
.cart-count{position:absolute;top:-8px;right:-10px;background:red;font-size:12px;padding:3px 7px;border-radius:50%;}
.section{padding:100px 40px;}
.products{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:25px;}
.card{background:rgba(255,255,255,0.05);padding:20px;border-radius:15px;text-align:center;}
.card img{width:100%;border-radius:10px;}
button{padding:8px 15px;border:none;border-radius:20px;cursor:pointer;font-weight:600;margin-top:5px;}
.cart-panel{position:fixed;right:-400px;top:0;width:350px;height:100%;background:#000;padding:20px;overflow-y:auto;transition:.4s;}
.cart-panel.active{right:0;}
.admin-panel{display:none;background:#111;padding:20px;border-radius:15px;margin-bottom:30px;}
input{width:100%;padding:8px;margin:5px 0;border-radius:8px;border:none;}
footer{text-align:center;padding:20px;background:#000;}
</style>
</head>
<body>

<header>
<div class="logo">Wasi Pro Store</div>
<nav>
<a href="#">Home</a>
<a href="#products">Products</a>
<a href="#" onclick="showAdmin()">Admin</a>
</nav>
<div class="cart-icon" onclick="toggleCart()">🛒
<span class="cart-count" id="cart-count">0</span>
</div>
</header>

<div class="section">

<!-- ADMIN LOGIN -->
<div id="loginBox">
<h2>Admin Login</h2>
<input type="text" id="adminUser" placeholder="Username">
<input type="password" id="adminPass" placeholder="Password">
<button onclick="adminLogin()">Login</button>
</div>

<!-- ADMIN PANEL -->
<div class="admin-panel" id="adminPanel">
<h2>Add Product</h2>
<input type="text" id="pname" placeholder="Product Name">
<input type="number" id="pprice" placeholder="Price">
<input type="text" id="pimg" placeholder="Image URL">
<button onclick="addProduct()">Add Product</button>
</div>

<h2 style="margin-top:30px;">Our Products</h2>
<div class="products" id="productList"></div>

</div>

<!-- CART -->
<div class="cart-panel" id="cartPanel">
<h2>Your Cart</h2>
<div id="cartItems"></div>
<p>Total: ৳<span id="total">0</span></p>
<button onclick="sendOrder()">Order via Gmail</button>
<button style="background:#e2136e;color:white;">bKash</button>
<button style="background:#ff6600;color:white;">Nagad</button>
</div>

<footer>© 2026 Wasi Pro Store</footer>

<script>

let products = JSON.parse(localStorage.getItem("products")) || [
{name:"Smart Watch",price:2500,img:"https://via.placeholder.com/300"},
{name:"Headphone",price:1800,img:"https://via.placeholder.com/300"}
];

let cart = JSON.parse(localStorage.getItem("cart")) || [];

function saveProducts(){localStorage.setItem("products",JSON.stringify(products));}
function saveCart(){localStorage.setItem("cart",JSON.stringify(cart));}

function renderProducts(){
let list=document.getElementById("productList");
list.innerHTML="";
products.forEach((p,i)=>{
list.innerHTML+=`
<div class="card">
<img src="${p.img}">
<h3>${p.name}</h3>
<p>৳${p.price}</p>
<button onclick="addToCart('${p.name}',${p.price})">Add to Cart</button>
<button onclick="deleteProduct(${i})">Delete</button>
</div>`;
});
}

function addProduct(){
let name=document.getElementById("pname").value;
let price=document.getElementById("pprice").value;
let img=document.getElementById("pimg").value;
products.push({name,price,img});
saveProducts();
renderProducts();
}

function deleteProduct(i){
products.splice(i,1);
saveProducts();
renderProducts();
}

function addToCart(name,price){
let existing=cart.find(item=>item.name===name);
if(existing){existing.qty+=1;}
else{cart.push({name,price,qty:1});}
updateCart();
}

function updateCart(){
let items=document.getElementById("cartItems");
let total=0;items.innerHTML="";
cart.forEach((item,i)=>{
total+=item.price*item.qty;
items.innerHTML+=`
<div>
${item.name} x ${item.qty}
<button onclick="changeQty(${i},1)">+</button>
<button onclick="changeQty(${i},-1)">-</button>
<button onclick="removeItem(${i})">Remove</button>
</div>`;
});
document.getElementById("total").innerText=total;
document.getElementById("cart-count").innerText=cart.length;
saveCart();
}

function changeQty(i,c){
cart[i].qty+=c;
if(cart[i].qty<=0){cart.splice(i,1);}
updateCart();
}

function removeItem(i){cart.splice(i,1);updateCart();}

function toggleCart(){document.getElementById("cartPanel").classList.toggle("active");}

function sendOrder(){
let text="Order Details:\n";
cart.forEach(i=>{text+=i.name+" x "+i.qty+"\n";});
window.location.href="mailto:yourgmail@gmail.com?subject=Order&body="+encodeURIComponent(text);
}

function adminLogin(){
let u=document.getElementById("adminUser").value;
let p=document.getElementById("adminPass").value;
if(u==="admin" && p==="1234"){
document.getElementById("adminPanel").style.display="block";
alert("Admin Login Successful");
}else{
alert("Wrong Credentials");
}
}

function showAdmin(){
document.getElementById("loginBox").scrollIntoView({behavior:'smooth'});
}

renderProducts();
updateCart();

</script>

</body>
</html>
</html>
