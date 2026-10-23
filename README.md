<!DOCTYPE html>
<html lang="tg">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#116b45">
  <title>BozoriTJ — Бозори Тоҷикистон</title>

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f4f7f5;
      color: #222;
    }

    header {
      background: #116b45;
      color: white;
      padding: 22px 16px;
      text-align: center;
    }

    header h1 {
      margin: 0 0 8px;
      font-size: 30px;
    }

    header p {
      margin: 0;
    }

    main {
      max-width: 1100px;
      margin: auto;
      padding: 20px 14px;
    }

    .panel {
      background: white;
      padding: 18px;
      margin-bottom: 22px;
      border-radius: 12px;
      box-shadow: 0 2px 9px #0000000d;
    }

    h2 {
      margin-top: 0;
      color: #116b45;
    }

    input,
    textarea,
    select,
    button {
      width: 100%;
      padding: 12px;
      margin: 7px 0;
      border-radius: 7px;
      font: inherit;
    }

    input,
    textarea,
    select {
      border: 1px solid #ccc;
      background: white;
    }

    textarea {
      min-height: 85px;
      resize: vertical;
    }

    button {
      border: none;
      background: #116b45;
      color: white;
      cursor: pointer;
      font-weight: bold;
    }

    button:hover {
      background: #0b5033;
    }

    button:disabled {
      opacity: 0.6;
      cursor: wait;
    }

    .secondary-button {
      background: #555;
    }

    .delete-button {
      background: #b42318;
    }

    .delete-button:hover {
      background: #8e1c13;
    }

    .hidden {
      display: none !important;
    }

    .status {
      margin: 10px 0;
      overflow-wrap: anywhere;
    }

    .error {
      color: #b42318;
    }

    .success {
      color: #116b45;
    }

    .top-row {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      flex-wrap: wrap;
    }

    .top-row h2 {
      margin-bottom: 0;
    }

    .top-row button {
      width: auto;
    }

    #products {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 18px;
      margin-top: 18px;
    }

    .product {
      background: white;
      border-radius: 12px;
      overflow: hidden;
      border: 1px solid #e4e8e5;
      padding-bottom: 12px;
    }

    .product img {
      width: 100%;
      height: 190px;
      object-fit: cover;
      display: block;
      background: #eee;
    }

    .product h3,
    .product p {
      margin: 12px;
      overflow-wrap: anywhere;
    }

    .product h3 {
      font-size: 19px;
    }

    .price {
      color: #116b45;
      font-size: 21px;
      font-weight: bold;
    }

    .product .delete-button {
      width: calc(100% - 24px);
      margin: 0 12px;
    }

    footer {
      text-align: center;
      padding: 22px 12px;
      color: #666;
    }

    @media (max-width: 500px) {
      header h1 {
        font-size: 25px;
      }

      main {
        padding: 12px 9px;
      }

      .panel {
        padding: 14px;
      }

      #products {
        grid-template-columns: repeat(2, minmax(0, 1fr));
        gap: 10px;
      }

      .product img {
        height: 135px;
      }

      .product h3 {
        font-size: 16px;
      }

      .product p {
        font-size: 14px;
        margin: 9px;
      }

      .product .delete-button {
        width: calc(100% - 18px);
        margin: 0 9px;
        padding: 9px 5px;
        font-size: 12px;
      }
    }
  </style>
</head>

<body>
  <header>
    <h1>BozoriTJ</h1>
    <p>Бозори онлайн — маҳсулоти худро пайдо кунед</p>
  </header>

  <main>
    <section class="panel">
      <label for="search"><strong>Ҷустуҷӯи маҳсулот</strong></label>
      <input
        id="search"
        type="search"
        placeholder="Номи маҳсулот ё категория..."
      >
    </section>

    <section class="panel">
      <div class="top-row">
        <h2>Маҳсулот</h2>
        <button id="refreshButton" type="button">
          Навсозӣ
        </button>
      </div>

      <p id="productsStatus" class="status" aria-live="polite">
        Маҳсулот бор шуда истодааст...
      </p>

      <div id="products"></div>
    </section>

    <section class="panel" id="loginPanel">
      <h2>Воридшавии администратор</h2>

      <form id="loginForm">
        <label for="email">Почтаи электронӣ</label>
        <input
          id="email"
          type="email"
          placeholder="Почтаи электронии шумо"
          autocomplete="username"
          required
        >

        <label for="password">Парол</label>
        <input
          id="password"
          type="password"
          placeholder="Паролро ворид кунед"
          autocomplete="current-password"
          required
        >

        <button id="loginButton" type="submit">
          Ворид шудан
        </button>
      </form>

      <p id="loginStatus" class="status" aria-live="polite"></p>
    </section>

    <section class="panel hidden" id="adminPanel">
      <div class="top-row">
        <h2>Идоракунии маҳсулот</h2>
        <button id="logoutButton" class="secondary-button" type="button">
          Баромадан
        </button>
      </div>

      <p id="adminStatus" class="status" aria-live="polite"></p>

      <h3>Илова кардани маҳсулоти нав</h3>

      <form id="productForm">
        <label for="productName">Номи маҳсулот *</label>
        <input
          id="productName"
          type="text"
          maxlength="150"
          placeholder="Масалан: Себ"
          required
        >

        <label for="productDescription">Тавсифи маҳсулот</label>
        <textarea
          id="productDescription"
          maxlength="3000"
          placeholder="Маълумот дар бораи маҳсулот"
        ></textarea>

        <label for="productPrice">Нарх бо евро (€) *</label>
        <input
          id="productPrice"
          type="number"
          min="0"
          step="0.01"
          placeholder="10.00"
          required
        >

        <label for="productCategory">Категория *</label>
        <input
          id="productCategory"
          type="text"
          maxlength="100"
          placeholder="Масалан: Мева"
          required
        >

        <label for="productImage">Суроғаи сурат (URL)</label>
        <input
          id="productImage"
          type="url"
          placeholder="https://example.com/image.jpg"
        >

        <label for="productStock">Миқдор *</label>
        <input
          id="productStock"
          type="number"
          min="0"
          step="1"
          value="1"
          required
        >

        <button id="addProductButton" type="submit">
          Илова кардани маҳсулот
        </button>
      </form>
    </section>
  </main>

  <footer>
    <p>© BozoriTJ — Бозори онлайн</p>
  </footer>

  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

  <script>
    "use strict";

    const SUPABASE_URL =
      "https://ljidohteqippikkuycfk.supabase.co";

    const SUPABASE_KEY =
      "sb_publishable_6xy2FHR3KwMEpFrmbi7nIQ_UMz97rIC";

    const ADMIN_EMAIL =
      "firuzhamidov2006@gmail.com";

    const supabaseClient = window.supabase.createClient(
      SUPABASE_URL,
      SUPABASE_KEY
    );

    let allProducts = [];
    let currentUser = null;

    function escapeHTML(value) {
      return String(value ?? "").replace(/[&<>"']/g, character => ({
        "&": "&amp;",
        "<": "&lt;",
        ">": "&gt;",
        '"': "&quot;",
        "'": "&#39;"
      })[character]);
    }

    function safeImageURL(value) {
      if (!value) {
        return "";
      }

      try {
        const url = new URL(value);

        if (url.protocol === "https:" || url.protocol === "http:") {
          return url.href;
        }
      } catch (error) {
        return "";
      }

      return "";
    }

    function setStatus(elementId, message, type = "") {
      const element = document.getElementById(elementId);

      element.textContent = message;
      element.className = "status";

      if (type) {
        element.classList.add(type);
      }
    }

    function updateLoginState() {
      const isAdmin = Boolean(
        currentUser &&
        currentUser.email &&
        currentUser.email.toLowerCase() === ADMIN_EMAIL.toLowerCase()
      );

      document.getElementById("loginPanel")
        .classList.toggle("hidden", isAdmin);

      document.getElementById("adminPanel")
        .classList.toggle("hidden", !isAdmin);

      renderProducts();
    }

    async function loadProducts() {
      const status = document.getElementById("productsStatus");
      const container = document.getElementById("products");

      status.textContent = "Маҳсулот бор шуда истодааст...";

      const { data, error } = await supabaseClient
        .from("products")
        .select("id, name, description, price, category, image_url, stock, is_active, created_at")
        .eq("is_active", true)
        .order("created_at", { ascending: false });

      if (error) {
        console.error("Load products error:", error);
        allProducts = [];
        container.replaceChildren();

        status.textContent =
          "Маҳсулот бор нашуд. Танзимоти Supabase ва пайвасти интернетро санҷед.";

        return;
      }

      allProducts = data || [];

      status.textContent = allProducts.length
        ? "Ҳамагӣ " + allProducts.length + " маҳсулот."
        : "Ҳоло маҳсулот нест.";

      renderProducts();
    }

    function renderProducts() {
      const container = document.getElementById("products");

      const searchTerm = document
        .getElementById("search")
        .value
        .trim()
        .toLowerCase();

      const filtered = allProducts.filter(product => {
        const text = [
          product.name,
          product.description,
          product.category
        ].join(" ").toLowerCase();

        return text.includes(searchTerm);
      });

      container.replaceChildren();

      if (filtered.length === 0) {
        const message = document.createElement("p");

        message.textContent = searchTerm
          ? "Маҳсулоти мувофиқ ёфт нашуд."
          : "Ҳоло маҳсулот нест.";

        container.appendChild(message);
        return;
      }

      filtered.forEach(product => {
        const card = document.createElement("article");
        card.className = "product";

        const imageURL = safeImageURL(product.image_url);

        if (imageURL) {
          const image = document.createElement("img");

          image.src = imageURL;
          image.alt = product.name || "Маҳсулот";
          image.loading = "lazy";
          image.referrerPolicy = "no-referrer";

          image.addEventListener("error", () => {
            image.remove();
          });

          card.appendChild(image);
        }

        const name = document.createElement("h3");
        name.textContent = product.name || "Бе ном";
        card.appendChild(name);

        const description = document.createElement("p");
        description.textContent = product.description || "";
        card.appendChild(description);

        const category = document.createElement("p");
        category.textContent =
          "Категория: " + (product.category || "Муайян нест");
        card.appendChild(category);

        const price = document.createElement("p");
        price.className = "price";

        const numericPrice = Number(product.price);

        price.textContent = Number.isFinite(numericPrice)
          ? numericPrice.toLocaleString("de-DE", {
              minimumFractionDigits: 2,
              maximumFractionDigits: 2
            }) + " €"
          : "Нарх муайян нест";

        card.appendChild(price);

        const stock = document.createElement("p");
        stock.textContent = "Миқдор: " + (product.stock ?? 0);
        card.appendChild(stock);

        const isAdmin = Boolean(
          currentUser &&
          currentUser.email &&
          currentUser.email.toLowerCase() === ADMIN_EMAIL.toLowerCase()
        );

        if (isAdmin) {
          const deleteButton = document.createElement("button");

          deleteButton.type = "button";
          deleteButton.className = "delete-button";
          deleteButton.textContent = "Нест кардани маҳсулот";

          deleteButton.addEventListener("click", () => {
            deleteProduct(product.id, product.name);
          });

          card.appendChild(deleteButton);
        }

        container.appendChild(card);
      });
    }

    document.getElementById("search").addEventListener("input", () => {
      renderProducts();
    });

    document.getElementById("refreshButton").addEventListener("click", () => {
      loadProducts();
    });

    document.getElementById("loginForm").addEventListener("submit", async event => {
      event.preventDefault();

      const email = document.getElementById("email").value.trim();
      const password = document.getElementById("password").value;
      const button = document.getElementById("loginButton");

      if (email.toLowerCase() !== ADMIN_EMAIL.toLowerCase()) {
        setStatus(
          "loginStatus",
          "Ин почтаи электронӣ барои администратор иҷозат надорад.",
          "error"
        );
        return;
      }

      button.disabled = true;
      setStatus("loginStatus", "Ворид шуда истодаед...");

      const { data, error } = await supabaseClient.auth.signInWithPassword({
        email,
        password
      });

      button.disabled = false;

      if (error) {
        console.error("Login error:", error);

        setStatus(
          "loginStatus",
          "Воридшавӣ нашуд. Почтаи электронӣ ва паролро санҷед.",
          "error"
        );

        return;
      }

      currentUser = data.user;

      document.getElementById("loginForm").reset();

      setStatus(
        "adminStatus",
        "Шумо ҳамчун администратор ворид шудед.",
        "success"
      );

      setStatus("loginStatus", "");
      updateLoginState();
    });

    document.getElementById("logoutButton").addEventListener("click", async () => {
      const { error } = await supabaseClient.auth.signOut();

      if (error) {
        console.error("Logout error:", error);

        setStatus(
          "adminStatus",
          "Баромадан муваффақ нашуд. Аз нав кӯшиш кунед.",
          "error"
        );

        return;
      }

      currentUser = null;
      updateLoginState();

      setStatus("loginStatus", "Шумо аз ҳисоб баромадед.", "success");
      setStatus("adminStatus", "");
    });

    document.getElementById("productForm").addEventListener("submit", async event => {
      event.preventDefault();

      if (
        !currentUser ||
        !currentUser.email ||
        currentUser.email.toLowerCase() !== ADMIN_EMAIL.toLowerCase()
      ) {
        setStatus(
          "adminStatus",
          "Аввал ҳамчун администратор ворид шавед.",
          "error"
        );
        return;
      }

      const name = document.getElementById("productName").value.trim();
      const description = document.getElementById("productDescription").value.trim();
      const category = document.getElementById("productCategory").value.trim();
      const imageInput = document.getElementById("productImage").value.trim();
      const price = Number(document.getElementById("productPrice").value);
      const stock = Number(document.getElementById("productStock").value);
      const button = document.getElementById("addProductButton");

      if (!name || !category) {
        setStatus(
          "adminStatus",
          "Номи маҳсулот ва категорияро пур кунед.",
          "error"
        );
        return;
      }

      if (
        !Number.isFinite(price) ||
        price < 0 ||
        !Number.isFinite(stock) ||
        stock < 0 ||
        !Number.isInteger(stock)
      ) {
        setStatus(
          "adminStatus",
          "Нарх ё миқдор нодуруст аст.",
          "error"
        );
        return;
      }

      let imageURL = null;

      if (imageInput) {
        imageURL = safeImageURL(imageInput);

        if (!imageURL) {
          setStatus(
            "adminStatus",
            "Суроғаи сурат бояд URL-и дурусти HTTP ё HTTPS бошад.",
            "error"
          );
          return;
        }
      }

      button.disabled = true;

      setStatus("adminStatus", "Маҳсулот илова шуда истодааст...");

      const { error } = await supabaseClient
        .from("products")
        .insert({
          name,
          description,
          price,
          category,
          image_url: imageURL,
          stock,
          is_active: true
        });

      button.disabled = false;

      if (error) {
        console.error("Insert product error:", error);

        setStatus(
          "adminStatus",
          "Маҳсулот илова нашуд. Иҷозатҳои RLS ва танзимоти Supabase-ро санҷед.",
          "error"
        );

        return;
      }

      document.getElementById("productForm").reset();

      setStatus(
        "adminStatus",
        "Маҳсулот бомуваффақият илова шуд.",
        "success"
      );

      await loadProducts();
    });

    async function deleteProduct(id, name) {
      const confirmed = window.confirm(
        'Маҳсулоти "' + name + '" нест карда шавад?'
      );

      if (!confirmed) {
        return;
      }

      setStatus("adminStatus", "Маҳсулот нест шуда истодааст...");

      const { error } = await supabaseClient
        .from("products")
        .delete()
        .eq("id", id);

      if (error) {
        console.error("Delete product error:", error);

        setStatus(
          "adminStatus",
          "Маҳсулот нест нашуд. Иҷозати DELETE дар Supabase-ро санҷед.",
          "error"
        );

        return;
      }

      setStatus(
        "adminStatus",
        "Маҳсулот нест карда шуд.",
        "success"
      );

      await loadProducts();
    }

    async function initializeApp() {
      if (!window.supabase) {
        setStatus(
          "productsStatus",
          "Supabase бор нашуд. Интернетро санҷед ва саҳифаро нав кунед.",
          "error"
        );
        return;
      }

      const { data, error } = await supabaseClient.auth.getSession();

      if (error) {
        console.error("Session error:", error);
      }

      currentUser = data?.session?.user || null;

      updateLoginState();
      await loadProducts();

      supabaseClient.auth.onAuthStateChange((_event, session) => {
        currentUser = session?.user || null;
        updateLoginState();
      });
    }

    initializeApp();
  </script>
</body>
</html>
