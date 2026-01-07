<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Scent & Style</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{margin:0;font-family:Arial;background:#0f0f0f;color:#fff}
header{background:#000;padding:20px;text-align:center;border-bottom:2px solid gold}
h1{color:gold;margin:0}
.container{padding:20px}
input,textarea,select{width:100%;padding:8px;margin:6px 0;border-radius:5px;border:none}
button{padding:8px 12px;border:none;border-radius:5px;cursor:pointer}
.gold{background:gold;color:#000;font-weight:bold}
.red{background:#c0392b;color:#fff}
.blue{background:#2980b9;color:#fff}
.products{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:15px}
.card{background:#1c1c1c;padding:15px;border-radius:8px}
.card img{width:100%;height:170px;object-fit:cover;border-radius:5px}
.price{color:gold;font-weight:bold}
.admin{display:none;background:#111;padding:15px;border:1px solid gold;margin-bottom:20px}
.search-filter{display:flex;gap:10px;margin-bottom:20px}
footer{background:#000;text-align:center;padding:10px;border-top:2px solid gold}
</style>
</head>

<body>

<header>
<h1>Scent & Style</h1>
<p>Where Fragrance Meets Fashion</p>
<button class="gold" onclick="login()">Admin Login</button>
</header>

<div class="container">

<!-- ADMIN PANEL -->
<div class="admin" id="admin">
<h3>Admin Panel</h3>
<input id="name" placeholder="Product Name">
<input id="price" placeholder="Price (AED)">
<input type="file" id="imageFile" accept="image/*">
<select id="category">
<option>Perfumes</option>
<option>Watches</option>
<option>Glasses</option>
<option>Belts</option>
<option>Bags</option>
</select>
<textarea id="details" placeholder="Product Details"></textarea>
<button class="gold" onclick="saveProduct()">Save Product</button>
</div>

<!-- SEARCH & FILTER -->
<div class="search-filter">
<input id="search" placeholder="Search..." onkeyup="displayProducts()">
<select id="filter" onchange="displayProducts()">
<option value="">All Categories</option>
<option>Perfumes</option>
<option>Watches</option>
<option>Glasses</option>
<option>Belts</option>
<option>Bags</option>
</select>
</div>

<!-- PRODUCTS -->
<div class="products" id="products"></div>

</div>

<footer>
<p>📍 UAE | 🚚 Cash on Delivery | 📲 WhatsApp Orders</p>
</footer>

<script>
const ADMIN_PASS="admin123";
let products=JSON.parse(localStorage.getItem("products"))||[];
let editIndex=null;

function login(){
 let p=prompt("Enter Admin Password");
 if(p===ADMIN_PASS){
  document.getElementById("admin").style.display="block";
 }else alert("Wrong password");
}

function saveProduct(){
 let file=imageFile.files[0];
 if(file){
  let reader=new FileReader();
  reader.onload=()=>{
   storeProduct(reader.result);
  };
  reader.readAsDataURL(file);
 }else{
  storeProduct(null);
 }
}

function storeProduct(img){
 let p={
  name:name.value,
  price:price.value,
  image:img || (editIndex!==null ? products[editIndex].image : ""),
  category:category.value,
  details:details.value
 };

 if(editIndex!==null){products[editIndex]=p;editIndex=null}
 else products.push(p);

 localStorage.setItem("products",JSON.stringify(products));
 clearForm();displayProducts();
}

function displayProducts(){
 let list=document.getElementById("products");
 let s=search.value.toLowerCase();
 let f=filter.value;
 list.innerHTML="";
 products.forEach((p,i)=>{
  if((!f||p.category===f)&&p.name.toLowerCase().includes(s)){
   list.innerHTML+=`
   <div class="card">
    <img src="${p.image || 'https://via.placeholder.com/300'}">
    <h3>${p.name}</h3>
    <p>${p.details}</p>
    <p class="price">AED ${p.price}</p>
    <a href="https://wa.me/971XXXXXXXXX?text=I want to buy ${p.name}" target="_blank">
      <button class="gold">Order on WhatsApp</button>
    </a><br><br>
    <button class="blue" onclick="edit(${i})">Edit</button>
    <button class="red" onclick="del(${i})">Delete</button>
   </div>`;
  }
 });
}

function edit(i){
 let p=products[i];
 name.value=p.name;
 price.value=p.price;
 category.value=p.category;
 details.value=p.details;
 editIndex=i;
}

function del(i){
 if(confirm("Delete product?")){
  products.splice(i,1);
  localStorage.setItem("products",JSON.stringify(products));
  displayProducts();
 }
}

function clearForm(){
 name.value="";price.value="";
 imageFile.value="";details.value="";
}

displayProducts();
</script>

</body>
</html>