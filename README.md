<html lang="ar">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Moj Store</title>

    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css"
    />

    <style>
      body {
        font-family: "Cairo", Arial, sans-serif;
        background: #fffafc;
        margin: 0;
        padding: 0;
        color: #333;
        direction: rtl;
      }

      header {
        background: linear-gradient(90deg, #ffb6c1, #ff69b4);
        color: white;
        text-align: center;
        padding: 25px 15px;
        position: relative;
        border-bottom: 4px solid #ffb6c1;
      }

      header h1 {
        margin: 0;
        font-size: 2.2em;
        letter-spacing: 1px;
      }

      header p {
        margin: 5px 0 15px;
      }

      .social-icons {
        position: absolute;
        top: 20px;
        left: 20px;
        display: flex;
        gap: 12px;
      }

      .social-icons a {
        color: white;
        font-size: 1.5em;
        transition: 0.3s;
      }

      .social-icons a:hover {
        color: #ffe4ec;
        transform: scale(1.1);
      }

      #adminBtn {
        background: white;
        color: #ff69b4;
        border: none;
        padding: 8px 18px;
        border-radius: 8px;
        cursor: pointer;
        font-weight: bold;
      }

      #adminBtn:hover {
        background: #ffe4ec;
      }

      nav {
        background: #fff0f6;
        display: flex;
        justify-content: center;
        flex-wrap: wrap;
        gap: 10px;
        padding: 12px 0;
      }

      nav button {
        background: #ff69b4;
        color: white;
        border: none;
        padding: 8px 16px;
        border-radius: 6px;
        cursor: pointer;
        font-weight: 600;
      }

      nav button:hover {
        background: #ff85c2;
      }

      section {
        max-width: 1000px;
        margin: 30px auto;
        background: #fff;
        border-radius: 12px;
        box-shadow: 0 3px 12px rgba(0, 0, 0, 0.1);
        padding: 25px;
      }

      h2 {
        color: #ff69b4;
        margin-bottom: 15px;
        border-bottom: 2px solid #ffd3e0;
        display: inline-block;
        padding-bottom: 4px;
      }

      .products {
        display: flex;
        flex-wrap: wrap;
        gap: 18px;
        justify-content: center;
      }

      .product {
        background: #fff8fa;
        border-radius: 10px;
        box-shadow: 0 1px 6px rgba(0, 0, 0, 0.1);
        width: 210px;
        text-align: center;
        padding: 15px;
        transition: 0.3s;
      }

      .product:hover {
        transform: translateY(-4px);
      }

      .product img {
        width: 100%;
        height: 130px;
        object-fit: cover;
        border-radius: 8px;
      }

      .product-name {
        margin-top: 8px;
        font-size: 1.1em;
        font-weight: bold;
      }

      .product-price {
        color: #43aa8b;
        font-weight: bold;
      }

      #adminPanel {
        display: none;
        background: #fff0f6;
        border: 2px solid #ffc2d6;
        border-radius: 12px;
        padding: 20px;
        margin: 25px auto;
        max-width: 700px;
      }

      #adminPanel input,
      #adminPanel select {
        margin: 5px;
        padding: 8px;
        border: 1px solid #bbb;
        border-radius: 6px;
        width: calc(33% - 12px);
      }

      #adminPanel button {
        background: #ff69b4;
        color: white;
        border: none;
        padding: 8px 14px;
        border-radius: 6px;
        cursor: pointer;
        font-weight: 600;
      }

      .admin-actions button {
        background: #ff69b4;
        border: none;
        color: white;
        margin: 4px;
        border-radius: 6px;
        padding: 5px 8px;
        cursor: pointer;
      }

      .admin-actions button.delete {
        background: #e63946;
      }

      footer {
        background: linear-gradient(90deg, #ffb6c1, #ff69b4);
        text-align: center;
        padding: 15px;
        margin-top: 40px;
        color: white;
        font-weight: bold;
      }
    </style>
  </head>

  <body>
    <header>
      <div class="social-icons">
        <a href="https://wa.me/9647829215612" target="_blank" title="واتساب"
          ><i class="fab fa-whatsapp"></i
        ></a>
        <a
          href="https://www.instagram.com/baby.careiq"
          target="_blank"
          title="إنستغرام"
          ><i class="fab fa-instagram"></i
        ></a>
      </div>
      <h1>Moj Store</h1>
      <p>متجر ملابس ومنتجات أطفال بأناقة وراحة 🌸</p>
      <button id="adminBtn" onclick="openAdminPanel()">لوحة الإدارة</button>
    </header>

    <nav>
      <button onclick="filterCategory('حفاضات')">🍼 الحفاضات</button>
      <button onclick="filterCategory('حليب')">🥛 الحليب</button>
      <button onclick="filterCategory('أطعمة')">🍎 الأطعمة</button>
      <button onclick="filterCategory('منظفات')">🧴 المنظفات</button>
      <button onclick="filterCategory('أخرى')">🎁 أخرى</button>
      <button onclick="filterCategory('الكل')">عرض الكل</button>
    </nav>

    <section id="products">
      <h2>المنتجات</h2>
      <div class="products" id="productsContainer"></div>
    </section>

    <div id="adminPanel">
      <h3>لوحة تحكم المدير</h3>
      <input id="productName" placeholder="اسم المنتج" />
      <input id="productPrice" type="number" placeholder="السعر بالدينار" />
      <input id="productImage" placeholder="رابط الصورة (URL)" />
      <select id="productCategory">
        <option value="حفاضات">حفاضات</option>
        <option value="حليب">حليب</option>
        <option value="أطعمة">أطعمة</option>
        <option value="منظفات">منظفات</option>
        <option value="أخرى">أخرى</option>
      </select>
      <button onclick="addProduct()">➕ إضافة المنتج</button>
    </div>

    <footer>© 2025 Moj Store</footer>

    <script type="module">
      import {
        initializeApp,
      } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-app.js";
      import {
        getFirestore,
        collection,
        addDoc,
        getDocs,
        deleteDoc,
        doc,
        updateDoc,
      } from "https://www.gstatic.com/firebasejs/11.0.1/firebase-firestore.js";

      const firebaseConfig = {
        apiKey: "AIzaSyAWwb775H_z2ZASvPzpaLMIbVQIFhT_I48",
        authDomain: "baby-a0fa1.firebaseapp.com",
        projectId: "baby-a0fa1",
        storageBucket: "baby-a0fa1.firebasestorage.app",
        messagingSenderId: "80891249885",
        appId: "1:80891249885:web:5bfa8038a9951ce2138734",
      };

      const app = initializeApp(firebaseConfig);
      const db = getFirestore(app);

      let isAdmin = false;
      let allProducts = [];

      async function loadProducts() {
        const container = document.getElementById("productsContainer");
        container.innerHTML = "";
        const querySnapshot = await getDocs(collection(db, "products"));
        allProducts = [];
        querySnapshot.forEach((docSnap) => {
          const p = { id: docSnap.id, ...docSnap.data() };
          allProducts.push(p);
        });
        displayProducts(allProducts);
      }

      function displayProducts(products) {
        const container = document.getElementById("productsContainer");
        container.innerHTML = "";
        products.forEach((p) => {
          const div = document.createElement("div");
          div.className = "product";
          div.innerHTML = `
            <img src="${p.image}" alt="${p.name}">
            <div class="product-name">${p.name}</div>
            <div class="product-price">${p.price} د.ع</div>
            <div class="product-category">${p.category || ""}</div>
            ${
              isAdmin
                ? `
            <div class="admin-actions">
              <button onclick="editProduct('${p.id}', '${p.name}', '${p.price}', '${p.image}', '${p.category}')">✏️ تعديل</button>
              <button class="delete" onclick="deleteProduct('${p.id}')">🗑️ حذف</button>
            </div>`
                : ""
            }
          `;
          container.appendChild(div);
        });
      }

      function filterCategory(cat) {
        if (cat === "الكل") displayProducts(allProducts);
        else
          displayProducts(allProducts.filter((p) => (p.category || "") === cat));
      }

      async function addProduct() {
        const name = productName.value.trim();
        const price = productPrice.value.trim();
        const image = productImage.value.trim();
        const category = productCategory.value;
        if (!name || !price || !image) return alert("يرجى إدخال جميع الحقول");

        await addDoc(collection(db, "products"), {
          name,
          price,
          image,
          category,
        });
        alert("✅ تمت إضافة المنتج");
        loadProducts();
      }

      async function deleteProduct(id) {
        if (confirm("هل تريد حذف هذا المنتج؟")) {
          await deleteDoc(doc(db, "products", id));
          loadProducts();
        }
      }

      async function editProduct(id, oldName, oldPrice, oldImage, oldCat) {
        const newName = prompt("اسم المنتج:", oldName);
        const newPrice = prompt("السعر:", oldPrice);
        const newImage = prompt("رابط الصورة:", oldImage);
        const newCategory = prompt("القسم:", oldCat);
        if (newName && newPrice && newImage) {
          await updateDoc(doc(db, "products", id), {
            name: newName,
            price: newPrice,
            image: newImage,
            category: newCategory,
          });
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

      window.openAdminPanel = openAdminPanel;
      window.addProduct = addProduct;
      window.editProduct = editProduct;
      window.deleteProduct = deleteProduct;
      window.filterCategory = filterCategory;

      loadProducts();
    </script>
  </body>
</html>
