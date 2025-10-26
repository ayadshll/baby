<html lang="ar" dir="rtl">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title></title>
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css"
    />
    <style>
      body {
        font-family: "Cairo", sans-serif;
        background: linear-gradient(135deg, #ff4d88, #ffcce0);
        margin: 0;
        padding: 0;
        color: #333;
      }

      /* شريط التنقل */
      nav {
        position: fixed;
        top: 0;
        width: 100%;
        background: rgba(255, 255, 255, 0.9);
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 10px 25px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        z-index: 1000;
        backdrop-filter: blur(8px);
      }
      nav .logo {
        font-size: 1.5em;
        font-weight: bold;
        color: #ff4d88;
      }
      nav ul {
        list-style: none;
        display: flex;
        gap: 20px;
        margin: 0;
      }
      nav ul li a {
        text-decoration: none;
        color: #444;
        font-weight: 600;
        transition: 0.3s;
      }
      nav ul li a:hover {
        color: #ff4d88;
      }
      .search-bar input {
        padding: 6px 12px;
        border-radius: 20px;
        border: 1px solid #ccc;
        outline: none;
        width: 160px;
        transition: 0.3s;
      }
      .search-bar input:focus {
        width: 200px;
        border-color: #ff4d88;
      }

      main {
        padding: 120px 20px 40px;
        max-width: 1000px;
        margin: auto;
      }

      h2 {
        text-align: center;
        color: #ff4d88;
      }

      .products {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        gap: 20px;
      }
      .product {
        background: #fff;
        border-radius: 15px;
        box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
        width: 220px;
        text-align: center;
        padding: 15px;
        transition: 0.3s;
        animation: fadeInUp 0.5s ease;
      }
      .product:hover {
        transform: translateY(-5px);
      }
      .product img {
        width: 100%;
        height: 140px;
        object-fit: cover;
        border-radius: 10px;
      }
      .product-name {
        font-weight: bold;
        margin-top: 8px;
      }
      .product-price {
        color: #43aa8b;
        font-weight: bold;
        margin-bottom: 8px;
      }
      .edit-btn,
      .delete-btn {
        border: none;
        border-radius: 50%;
        width: 35px;
        height: 35px;
        color: white;
        cursor: pointer;
        transition: 0.3s;
        margin: 3px;
      }
      .edit-btn {
        background: #ff66a3;
      }
      .edit-btn:hover {
        background: #ff3385;
        transform: scale(1.1);
      }
      .delete-btn {
        background: #e63946;
      }
      .delete-btn:hover {
        transform: scale(1.1);
      }

      /* لوحة الإدارة */
      #adminPanel {
        display: none;
        background: #fff;
        padding: 20px;
        border-radius: 12px;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        margin-top: 30px;
      }
      #adminPanel input {
        margin: 5px;
        padding: 8px;
        border: 1px solid #ccc;
        border-radius: 6px;
      }
      #adminPanel button {
        background: #ff4d88;
        color: white;
        border: none;
        padding: 8px 16px;
        border-radius: 6px;
        cursor: pointer;
        font-weight: 600;
        transition: 0.3s;
      }
      #adminPanel button:hover {
        background: #ff3385;
      }

      /* المودال */
      .modal {
        display: none;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: rgba(0, 0, 0, 0.6);
        justify-content: center;
        align-items: center;
        z-index: 2000;
      }
      .modal-content {
        background: #fff;
        padding: 25px;
        border-radius: 12px;
        width: 320px;
        text-align: center;
        animation: fadeInUp 0.4s ease;
      }
      .modal-content input {
        width: 90%;
        margin: 6px 0;
        padding: 8px;
        border-radius: 8px;
        border: 1px solid #ccc;
      }
      .modal-content button {
        margin-top: 10px;
        background: #ff4d88;
        color: white;
        border: none;
        padding: 8px 14px;
        border-radius: 8px;
        cursor: pointer;
      }
      .modal-content button:hover {
        background: #ff3385;
      }

      footer {
        text-align: center;
        padding: 15px;
        background: #fff;
        color: #555;
        font-weight: bold;
        border-top: 2px solid #ffb6c1;
      }

      @keyframes fadeInUp {
        from {
          opacity: 0;
          transform: translateY(10px);
        }
        to {
          opacity: 1;
          transform: translateY(0);
        }
      }
    </style>
  </head>

  <body>
    <nav>
      <div class="logo">عناية الطفل</div>
      <ul>
        <li><a href="#">الرئيسية</a></li>
        <li><a href="#" onclick="filterProducts('food')">الأغذية</a></li>
        <li><a href="#" onclick="filterProducts('care')">العناية</a></li>
        <li><a href="#" onclick="filterProducts('other')">أخرى</a></li>
        <li><a href="#" onclick="openAdminPanel()">⚙️ لوحة الإدارة</a></li>
      </ul>
      <div class="search-bar">
        <input type="text" id="searchInput" placeholder="ابحث عن منتج..." onkeyup="searchProducts()" />
      </div>
    </nav>

    <main>
      <div class="products" id="productsContainer"></div>

      <div id="adminPanel">
        <h3>لوحة تحكم المدير</h3>
        <input id="productName" placeholder="اسم المنتج" />
        <input id="productPrice" type="number" placeholder="السعر بالدينار" />
        <input id="productImage" placeholder="رابط الصورة" />
        <input id="productCategory" placeholder="فئة المنتج (food/care/other)" />
        <button onclick="addProduct()">➕ إضافة المنتج</button>
      </div>
    </main>

    <div class="modal" id="editModal">
      <div class="modal-content">
        <h3>تعديل المنتج</h3>
        <input id="editName" placeholder="اسم المنتج" />
        <input id="editPrice" placeholder="السعر" />
        <input id="editImage" placeholder="رابط الصورة" />
        <input id="editCategory" placeholder="الفئة" />
        <button onclick="saveEdit()">💾 حفظ</button>
      </div>
    </div>

    <footer>© 2025 عناية الطفل</footer>

    <!-- Firebase -->
    <script type="module">
      import {
        initializeApp
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
      let currentEditId = null;

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
            ${
              isAdmin
                ? `<div>
                    <button class='edit-btn' onclick="openEditModal('${p.id}')"><i class='fa fa-pen'></i></button>
                    <button class='delete-btn' onclick="deleteProduct('${p.id}')"><i class='fa fa-trash'></i></button>
                  </div>`
                : ""
            }
          `;
          container.appendChild(div);
        });
      }

      async function addProduct() {
        const name = document.getElementById("productName").value.trim();
        const price = document.getElementById("productPrice").value.trim();
        const image = document.getElementById("productImage").value.trim();
        const category = document
          .getElementById("productCategory")
          .value.trim()
          .toLowerCase();
        if (!name || !price || !image || !category)
          return alert("يرجى إدخال جميع الحقول");

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

      // فتح نافذة التعديل
      window.openEditModal = (id) => {
        const p = allProducts.find((x) => x.id === id);
        if (!p) return;
        currentEditId = id;
        document.getElementById("editName").value = p.name;
        document.getElementById("editPrice").value = p.price;
        document.getElementById("editImage").value = p.image;
        document.getElementById("editCategory").value = p.category || "";
        document.getElementById("editModal").style.display = "flex";
      };

      // حفظ التعديلات
      window.saveEdit = async () => {
        const newName = document.getElementById("editName").value.trim();
        const newPrice = document.getElementById("editPrice").value.trim();
        const newImage = document.getElementById("editImage").value.trim();
        const newCategory = document
          .getElementById("editCategory")
          .value.trim()
          .toLowerCase();

        if (!newName || !newPrice || !newImage || !newCategory)
          return alert("يرجى إدخال جميع الحقول");

        await updateDoc(doc(db, "products", currentEditId), {
          name: newName,
          price: newPrice,
          image: newImage,
          category: newCategory,
        });
        document.getElementById("editModal").style.display = "none";
        alert("✅ تم تحديث المنتج");
        loadProducts();
      };

      // إغلاق المودال عند الضغط بالخارج
      document.getElementById("editModal").addEventListener("click", (e) => {
        if (e.target.id === "editModal") e.target.style.display = "none";
      });

      function openAdminPanel() {
        const pass = prompt("أدخل كلمة المرور:");
        if (pass === "2000blal2006") {
          document.getElementById("adminPanel").style.display = "block";
          isAdmin = true;
          loadProducts();
        } else alert("❌ كلمة المرور غير صحيحة");
      }

      function filterProducts(category) {
        const filtered = allProducts.filter((p) => p.category === category);
        displayProducts(filtered);
      }

      function searchProducts() {
        const term = document
          .getElementById("searchInput")
          .value.toLowerCase()
          .trim();
        const filtered = allProducts.filter((p) =>
          p.name.toLowerCase().includes(term)
        );
        displayProducts(filtered);
      }

      loadProducts();
      window.deleteProduct = deleteProduct;
      window.addProduct = addProduct;
      window.openAdminPanel = openAdminPanel;
      window.filterProducts = filterProducts;
      window.searchProducts = searchProducts;
    </script>
  </body>
</html>
