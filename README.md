<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>عناية الطفل</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body {
      font-family: 'Cairo', Arial, sans-serif;
      background: #fff5f8;
      margin: 0;
      padding: 0;
      color: #333;
    }
    header {
      background: linear-gradient(90deg, #ff5fa2, #ff8ac9);
      color: #fff;
      text-align: center;
      padding: 30px 15px;
      position: relative;
    }
    h1 { margin: 0; font-size: 2em; }
    nav {
      background: #ff5fa2;
      position: sticky;
      top: 0;
      display: flex;
      justify-content: center;
      gap: 10px;
      flex-wrap: wrap;
      padding: 10px;
      z-index: 999;
    }
    nav button {
      background: white;
      color: #ff5fa2;
      border: none;
      border-radius: 20px;
      padding: 8px 16px;
      cursor: pointer;
      font-weight: 600;
      transition: 0.3s;
    }
    nav button:hover {
      background: #ff8ac9;
      color: white;
    }
    #searchBox {
      display: block;
      margin: 15px auto;
      width: 80%;
      max-width: 400px;
      padding: 8px;
      border-radius: 20px;
      border: 2px solid #ff5fa2;
      text-align: center;
      outline: none;
    }
    .products {
      display: flex;
      flex-wrap: wrap;
      gap: 18px;
      justify-content: center;
      margin: 20px auto;
      max-width: 1000px;
    }
    .product {
      background: #fff;
      border-radius: 15px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
      width: 180px;
      text-align: center;
      padding: 10px;
      transition: 0.3s;
    }
    .product:hover { transform: translateY(-4px); }
    .product img {
      width: 100%;
      height: 130px;
      object-fit: cover;
      border-radius: 10px;
    }
    .product-name { margin-top: 6px; font-size: 1em; font-weight: bold; }
    .product-price { color: #43aa8b; font-weight: bold; }
    .admin-actions button {
      margin: 3px;
      border: none;
      border-radius: 6px;
      padding: 5px 8px;
      color: white;
      cursor: pointer;
    }
    .edit { background: #ffb703; }
    .delete { background: #e63946; }

    #adminPanel {
      display: none;
      background: #ffe5ef;
      border: 2px solid #ff8ac9;
      border-radius: 12px;
      padding: 15px;
      margin: 20px auto;
      max-width: 700px;
      text-align: center;
    }
    #adminPanel input {
      margin: 5px;
      padding: 8px;
      border: 1px solid #bbb;
      border-radius: 6px;
      width: calc(25% - 12px);
    }
    #adminPanel select {
      margin: 5px;
      padding: 8px;
      border: 1px solid #bbb;
      border-radius: 6px;
    }
    #adminPanel button {
      background: #ff5fa2;
      color: white;
      border: none;
      padding: 8px 14px;
      border-radius: 8px;
      cursor: pointer;
      font-weight: 600;
    }
    footer {
      background: linear-gradient(90deg, #ff5fa2, #ff8ac9);
      color: white;
      text-align: center;
      padding: 15px;
      margin-top: 30px;
      font-weight: bold;
    }
  </style>
</head>
<body>
  <header>
    <h1>عناية الطفل</h1>
    <p>منتجات ونصائح لرعاية طفلك بأفضل شكل ❤️</p>
  </header>

  <nav>
    <button onclick="filterCategory('all')">🏠 الرئيسية</button>
    <button onclick="filterCategory('food')">🍎 الأغذية</button>
    <button onclick="filterCategory('care')">🧴 العناية</button>
    <button onclick="filterCategory('other')">🎀 أخرى</button>
    <button onclick="openAdminPanel()">⚙️ الإدارة</button>
  </nav>

  <input type="text" id="searchBox" placeholder="ابحث عن منتج..." oninput="searchProducts()">

  <div class="products" id="productsContainer"></div>

  <div id="adminPanel">
    <h3>لوحة تحكم المدير</h3>
    <input id="productName" placeholder="اسم المنتج">
    <input id="productPrice" type="number" placeholder="السعر">
    <input id="productImage" placeholder="رابط الصورة">
    <select id="productCategory">
      <option value="food">أغذية</option>
      <option value="care">عناية</option>
      <option value="other">أخرى</option>
    </select>
    <button onclick="addProduct()">➕ إضافة المنتج</button>
  </div>

  <footer>© 2025 عناية الطفل</footer>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
    import { getFirestore, collection, addDoc, getDocs, deleteDoc, updateDoc, doc, onSnapshot } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-firestore.js";

    const firebaseConfig = {
      apiKey: "AIzaSyAWwb775H_z2ZASvPzpaLMIbVQIFhT_I48",
      authDomain: "baby-a0fa1.firebaseapp.com",
      projectId: "baby-a0fa1",
      storageBucket: "baby-a0fa1.firebasestorage.app",
      messagingSenderId: "80891249885",
      appId: "1:80891249885:web:5bfa8038a9951ce2138734"
    };

    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);
    let isAdmin = false;
    let currentCategory = "all";
    let searchQuery = "";

    const productsContainer = document.getElementById("productsContainer");

    function openAdminPanel() {
      const pass = prompt("أدخل كلمة المرور:");
      if (pass === "2000blal2006") {
        document.getElementById("adminPanel").style.display = "block";
        isAdmin = true;
      } else {
        alert("❌ كلمة المرور غير صحيحة");
      }
    }

    async function addProduct() {
      const name = productName.value.trim();
      const price = productPrice.value.trim();
      const image = productImage.value.trim();
      const category = productCategory.value;
      if (!name || !price || !image) return alert("يرجى إدخال جميع الحقول");

      await addDoc(collection(db, "products"), { name, price, image, category });
      productName.value = "";
      productPrice.value = "";
      productImage.value = "";
    }

    async function deleteProduct(id) {
      await deleteDoc(doc(db, "products", id));
    }

    async function editProduct(id, oldName, oldPrice, oldImage, oldCategory) {
      const newName = prompt("اسم المنتج:", oldName);
      const newPrice = prompt("السعر:", oldPrice);
      const newImage = prompt("رابط الصورة:", oldImage);
      const newCategory = prompt("الفئة (food/care/other):", oldCategory);
      if (newName && newPrice && newImage && newCategory)
        await updateDoc(doc(db, "products", id), { name: newName, price: newPrice, image: newImage, category: newCategory });
    }

    function renderProducts(snapshot) {
      productsContainer.innerHTML = "";
      snapshot.forEach(docSnap => {
        const p = docSnap.data();
        if ((currentCategory === "all" || p.category === currentCategory) &&
            (p.name.includes(searchQuery) || p.category.includes(searchQuery))) {
          const div = document.createElement("div");
          div.className = "product";
          div.innerHTML = `
            <img src="${p.image}" alt="${p.name}">
            <div class="product-name">${p.name}</div>
            <div class="product-price">${p.price} د.ع</div>
            ${isAdmin ? `
              <div class="admin-actions">
                <button class="edit" onclick="editProduct('${docSnap.id}', '${p.name}', '${p.price}', '${p.image}', '${p.category}')">✏️</button>
                <button class="delete" onclick="deleteProduct('${docSnap.id}')">🗑️</button>
              </div>` : ""}
          `;
          productsContainer.appendChild(div);
        }
      });
    }

    function filterCategory(cat) {
      currentCategory = cat;
      loadProducts();
    }

    function searchProducts() {
      searchQuery = document.getElementById("searchBox").value.trim();
      loadProducts();
    }

    function loadProducts() {
      onSnapshot(collection(db, "products"), renderProducts);
    }

    window.openAdminPanel = openAdminPanel;
    window.addProduct = addProduct;
    window.deleteProduct = deleteProduct;
    window.editProduct = editProduct;
    window.filterCategory = filterCategory;
    window.searchProducts = searchProducts;

    loadProducts();
  </script>
</body>
</html>
