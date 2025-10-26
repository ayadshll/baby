<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>عناية الطفل</title>
<style>
  body {
    margin: 0;
    font-family: 'Tajawal', sans-serif;
    background: linear-gradient(to bottom, #ff4d94, #ffb6c1);
    color: #333;
  }

  header {
    background: linear-gradient(to right, #ff4d94, #ffb6c1);
    color: white;
    text-align: center;
    padding: 15px 0;
    font-size: 1.8em;
    font-weight: bold;
    position: fixed;
    top: 0;
    width: 100%;
    z-index: 1000;
  }

  nav {
    display: flex;
    justify-content: space-around;
    background: #ffe6ef;
    padding: 10px;
    position: fixed;
    top: 60px;
    width: 100%;
    z-index: 999;
  }

  nav button {
    background: none;
    border: none;
    font-size: 1em;
    color: #ff4d94;
    font-weight: bold;
    cursor: pointer;
  }

  .search-box {
    margin: 120px auto 10px;
    width: 90%;
    text-align: center;
  }

  .search-box input {
    width: 100%;
    padding: 10px;
    border-radius: 25px;
    border: none;
    font-size: 1em;
    outline: none;
    text-align: center;
  }

  .products {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    padding: 10px;
  }

  .product {
    background: white;
    border-radius: 15px;
    box-shadow: 0 3px 6px rgba(0,0,0,0.1);
    width: 90%;
    max-width: 300px;
    margin: 10px;
    text-align: center;
    padding: 10px;
    transition: transform 0.2s;
  }

  .product:hover {
    transform: scale(1.03);
  }

  .product img {
    width: 100%;
    border-radius: 10px;
    height: 200px;
    object-fit: cover;
  }

  .product h3 {
    color: #ff4d94;
    margin: 10px 0 5px;
  }

  .product p {
    font-weight: bold;
    color: #444;
  }

  footer {
    text-align: center;
    color: white;
    padding: 20px;
  }
</style>
</head>
<body>

<header>عناية الطفل</header>

<nav>
  <button onclick="showSection('home')">الرئيسية</button>
  <button onclick="showSection('food')">الأغذية</button>
  <button onclick="showSection('care')">العناية</button>
</nav>

<div class="search-box">
  <input type="text" id="search" placeholder="ابحث عن منتج..." oninput="searchProducts()">
</div>

<div class="products" id="product-list"></div>

<footer>© 2025 عناية الطفل</footer>

<script>
const allProducts = [
  {name: "سيريلاك قمح", price: "5000 د.ع", img: "https://i.imgur.com/m9fGZ3m.jpg", category: "food"},
  {name: "سيريلاك حليب", price: "5500 د.ع", img: "https://i.imgur.com/9l4Ww6X.jpg", category: "food"},
  {name: "زيت جونسون", price: "7000 د.ع", img: "https://i.imgur.com/Y2yG6Ki.jpg", category: "care"},
  {name: "شامبو كودمو", price: "6000 د.ع", img: "https://i.imgur.com/OGa2fQs.jpg", category: "care"},
  {name: "عطر سلفر فش", price: "3000 د.ع", img: "https://i.imgur.com/BmEtxZL.jpg", category: "care"},
];

let currentSection = 'home';

function showSection(section) {
  currentSection = section;
  displayProducts();
}

function displayProducts() {
  const container = document.getElementById("product-list");
  container.innerHTML = "";

  let filtered = allProducts;
  if (currentSection !== 'home') {
    filtered = allProducts.filter(p => p.category === currentSection);
  } else {
    filtered = allProducts.sort(() => 0.5 - Math.random()).slice(0, 4);
  }

  filtered.forEach(p => {
    const card = document.createElement("div");
    card.className = "product";
    card.innerHTML = `
      <img src="${p.img}" alt="${p.name}">
      <h3>${p.name}</h3>
      <p>${p.price}</p>
    `;
    container.appendChild(card);
  });
}

function searchProducts() {
  const query = document.getElementById("search").value.toLowerCase();
  const container = document.getElementById("product-list");
  container.innerHTML = "";

  allProducts.filter(p => p.name.toLowerCase().includes(query))
  .forEach(p => {
    const card = document.createElement("div");
    card.className = "product";
    card.innerHTML = `
      <img src="${p.img}" alt="${p.name}">
      <h3>${p.name}</h3>
      <p>${p.price}</p>
    `;
    container.appendChild(card);
  });
}

// تحديث تلقائي بدون إعادة تحميل
setInterval(displayProducts, 15000);

displayProducts();
</script>

</body>
</html>
