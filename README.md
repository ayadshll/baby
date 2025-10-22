<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <title>عناية الطفل</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body {
      font-family: 'Cairo', Arial, sans-serif;
      background: #f3faff;
      margin: 0;
      padding: 0;
      color: #023047;
    }
    header {
      background: linear-gradient(90deg, #8ecae6, #219ebc);
      color: #fff;
      text-align: center;
      padding: 25px 15px;
      border-bottom: 4px solid #ffb703;
    }
    h1 { margin: 0; font-size: 2em; }
    nav {
      background: #023047;
      color: white;
      display: flex;
      justify-content: center;
      gap: 25px;
      padding: 10px;
    }
    nav a { color: #fff; text-decoration: none; font-weight: 600; }
    nav a:hover { text-decoration: underline; }
    section {
      max-width: 950px;
      margin: 30px auto;
      background: #fff;
      border-radius: 12px;
      box-shadow: 0 3px 12px rgba(0,0,0,0.1);
      padding: 25px;
    }
    h2 { color: #219ebc; margin-bottom: 10px; }

    .products {
      display: flex;
      flex-wrap: wrap;
      gap: 18px;
      justify-content: center;
    }
    .product {
      background: #f7fbfc;
      border-radius: 10px;
      box-shadow: 0 1px 6px rgba(0,0,0,0.1);
      width: 210px;
      text-align: center;
      padding: 15px;
      transition: 0.3s;
    }
    .product:hover { transform: translateY(-4px); }
    .product img {
      width: 100%;
      height: 130px;
      object-fit: cover;
      border-radius: 8px;
    }
    .product-name { margin-top: 8px; font-size: 1.1em; font-weight: bold; }
    .product-price { color: #43aa8b; font-weight: bold; }

    #adminBtn {
      background: #ffb703;
      color: #023047;
      border: none;
      padding: 8px 18px;
      border-radius: 8px;
      cursor: pointer;
      font-weight: 600;
    }
    #adminBtn:hover { background: #fb8500; color: #fff; }

    #adminPanel {
      display: none;
      background: #f1faee;
      border: 2px solid #a7c957;
      border-radius: 12px;
      padding: 20px;
      margin: 25px auto;
      max-width: 700px;
    }
    #adminPanel input {
      margin: 5px;
      padding: 8px;
      border: 1px solid #bbb;
      border-radius: 6px;
      width: calc(33% - 12px);
    }
    #adminPanel button {
      background: #219ebc;
      color: white;
      border: none;
      padding: 8px 14px;
      border-radius: 6px;
      cursor: pointer;
      font-weight: 600;
    }

    .admin-actions button {
      background: #219ebc;
      border: none;
      color: white;
      margin: 4px;
      border-radius: 6px;
      padding: 5px 8px;
      cursor: pointer;
    }
    .admin-actions button.delete { background: #e63946; }
  </style>
</head>
<body>
  <header>
    <h1>عناية الطفل</h1>
    <p>منتجات ونصائح لرعاية طفلك بأفضل شكل ❤️</p>
    <button id="adminBtn" onclick="openAdminPanel()">لوحة الإدارة</button>
  </header>

  <section id="products">
    <h2>منتجات أطفال</h2>
    <div class="products" id="productsContainer"></div>
  </section>

  <div id="adminPanel">
    <h3>لوحة تحكم المدير</h3>
    <input id="productName" placeholder="اسم المنتج" />
    <input id="productPrice" type="number" placeholder="السعر بالدينار" />
    <input id="productImage" placeholder="رابط الصورة (URL)" />
    <button onclick="addProduct()">➕ إضافة المنتج</button>
  </div>

  <footer style="background:#8ecae6;text-align:center;padding:15px;margin-top:40px;color:#023047;font-weight:bold;">
    © 2025 عناية الطفل
  </footer>

  <!-- Firebase -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
    import { getFirestore, collection, addDoc, getDocs, deleteDoc, doc, updateDoc } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-firestore.js";

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

    async function loadProducts() {
      const container = document.getElementById("productsContainer");
      container.innerHTML = "";
      const querySnapshot = await getDocs(collection(db, "products"));
      querySnapshot.forEach(docSnap => {
        const p = docSnap.data();
        const div = document.createElement("div");
        div.className = "product";
        div.innerHTML = `
          <img src="${p.image}" alt="${p.name}">
          <div class="product-name">${p.name}</div>
          <div class="product-price">${p.price} د.ع</div>
          ${isAdmin ? `
          <div class="admin-actions">
            <button onclick="editProduct('${docSnap.id}', '${p.name}', '${p.price}', '${p.image}')">✏️ تعديل</button>
            <button class="delete" onclick="deleteProduct('${docSnap.id}')">🗑️ حذف</button>
          </div>` : ""}
        `;
        container.appendChild(div);
      });
    }

    async function addProduct() {
      const name = document.getElementById("productName").value.trim();
      const price = document.getElementById("productPrice").value.trim();
      const image = document.getElementById("productImage").value.trim();
      if (!name || !price || !image) return alert("يرجى إدخال جميع الحقول");

      await addDoc(collection(db, "products"), { name, price, image });
      alert("✅ تمت إضافة المنتج");
      document.getElementById("productName").value = "";
      document.getElementById("productPrice").value = "";
      document.getElementById("productImage").value = "";
      loadProducts();
    }

    async function deleteProduct(id) {
      if (confirm("هل تريد حذف هذا المنتج؟")) {
        await deleteDoc(doc(db, "products", id));
        loadProducts();
      }
    }

    async function editProduct(id, oldName, oldPrice, oldImage) {
      const newName = prompt("اسم المنتج:", oldName);
      const newPrice = prompt("السعر:", oldPrice);
      const newImage = prompt("رابط الصورة:", oldImage);
      if (newName && newPrice && newImage) {
        await updateDoc(doc(db, "products", id), { name: newName, price: newPrice, image: newImage });
        alert("تم تحديث المنتج ✅");
        loadProducts();
      }
    }

    function openAdminPanel() {
      const pass = prompt("أدخل كلمة المرور:");
      if (pass === "2000blal2006") {
        document.getElementById("adminPanel").style.display = "block";
        isAdmin = true;
        loadProducts();
      } else alert("❌ كلمة المرور غير صحيحة");
    }

    // 👇 هذا هو المفتاح حتى يعمل الزر
    window.openAdminPanel = openAdminPanel;
    window.addProduct = addProduct;
    window.editProduct = editProduct;
    window.deleteProduct = deleteProduct;

    loadProducts();
  </script>
</body>
</html>
