<!DOCTYPE html>
<html lang="tg">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BozoriTJ — Бозори Тоҷикистон</title>
  <meta name="description" content="BozoriTJ — бозори хариду фурӯши маҳсулот.">

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f4f6f5;
      color: #222;
    }

    header {
      background: #087f5b;
      color: white;
      padding: 24px 12px;
      text-align: center;
    }

    header h1 {
      margin: 0 0 8px;
      font-size: 32px;
    }

    header p {
      margin: 0;
    }

    .search,
    input,
    textarea,
    select {
      width: 100%;
      padding: 12px;
      margin: 7px 0;
      border: 1px solid #ddd;
      border-radius: 7px;
      font-size: 16px;
    }

    .search {
      display: block;
      width: 90%;
      max-width: 600px;
      margin: 20px auto;
      background: white;
    }

    main {
      max-width: 1000px;
      margin: auto;
      padding: 15px;
    }

    .panel,
    .product {
      background: white;
      padding: 16px;
      margin-bottom: 16px;
      border-radius: 10px;
      box-shadow: 0 2px 8px #0001;
    }

    button {
      background: #087f5b;
      color: white;
      border: 0;
      border-radius: 7px;
      padding: 12px 16px;
      margin: 5px 0;
      cursor: pointer;
      font-size: 15px;
    }

    button:disabled {
      opacity: 0.6;
      cursor: wait;
    }

    .secondary {
      background: #555;
    }

    #products {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
      gap: 14px;
    }

    .product img {
      width: 100%;
      height: 170px;
      object-fit: contain;
      border-radius: 7px;
      background: #f8f8f8;
    }

    .price {
      color: #087f5b;
      font-weight: bold;
      font-size: 18px;
    }

    #status {
      white-space: pre-wrap;
      overflow-wrap: anywhere;
      padding: 8px 0;
    }

    footer {
      text-align: center;
      padding: 20px;
      color: #666;
    }

    [hidden] {
      display: none !important;
    }
  </style>
</head>

<body>
  <header>
    <h1>BozoriTJ</h1>
    <p>Бозори Тоҷикистон — хариду фурӯши маҳсулот</p>
  </header>

  <input
    id="search"
    class="search"
    type="search"
    placeholder="Маҳсулотро ҷустуҷӯ кун..."
    aria-label="Ҷустуҷӯи маҳсулот"
  >

  <main>
    <section class="panel">
      <h2>Маҳсулот</h2>
      <div id="products">Маҳсулот бор шуда истодааст...</div>
    </section>

    <section class="panel">
      <h2>Воридшавии администратор</h2>
      <p>Барои илова кардани маҳсулот ба ҳисоби администратор ворид шав.</p>

      <form id="loginForm">
        <label for="email">Почтаи электронӣ</label>
        <input
          id="email"
          type="email"
          autocomplete="username"
          required
          placeholder="Почтаи электронӣ"
        >

        <label for="password">Рамз</label>
        <input
          id="password"
          type="password"
          autocomplete="current-password"
          required
          placeholder="Рамз"
        >

        <button id="loginBtn" type="submit">Ворид шудан</button>
      </form>

      <button id="logoutBtn" class="secondary" type="button" hidden>
        Баромадан
      </button>

      <p id="loginState">Ҳисоб санҷида мешавад...</p>
    </section>

    <section id="adminPanel" class="panel" hidden>
      <h2>Илова кардани маҳсулот</h2>

      <form id="productForm">
        <label for="name">Номи маҳсулот *</label>
        <input
          id="name"
          required
          maxlength="150"
          placeholder="Масалан: Футболка"
        >

        <label for="description">Тавсифи маҳсулот</label>
        <textarea
          id="description"
          rows="3"
          maxlength="3000"
          placeholder="Маълумот дар бораи маҳсулот"
        ></textarea>

        <label for="price">Нарх (евро) *</label>
        <input
          id="price"
          type="number"
          min="0"
          step="0.01"
          required
          placeholder="Масалан: 15"
        >

        <label for="category">Категория *</label>
        <input
          id="category"
          required
          maxlength="100"
          placeholder="Масалан: Либос"
        >

        <label for="image_url">Суроғаи акси маҳсулот</label>
        <input
          id="image_url"
          type="url"
          placeholder="https://..."
        >

        <label for="stock">Миқдор *</label>
        <input
          id="stock"
          type="number"
          min="0"
          step="1"
          value="1"
          required
        >

        <button id="addProductBtn" type="submit">
          Илова кардани маҳсулот
        </button>
      </form>
    </section>

    <p id="status" role="status" aria-live="polite"></p>
  </main>

  <footer>
    © 2026 BozoriTJ — Бозори Тоҷикистон
  </footer>

  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

  <script>
    const SUPABASE_URL =
      "https://ljidohteqippikkuycfk.supabase.co";

    const SUPABASE_KEY =
      "sb_publishable_6xy2FHR3KwMEpFrmbi7nIQ_UMz97rIC";

    const ADMIN_EMAIL = "firuzhamidov2006@gmail.com";

    const db = window.supabase.createClient(
      SUPABASE_URL,
      SUPABASE_KEY
    );

    const loginForm = document.getElementById("loginForm");
    const logoutBtn = document.getElementById("logoutBtn");
    const productForm = document.getElementById("productForm");
    const adminPanel = document.getElementById("adminPanel");
    const loginState = document.getElementById("loginState");
    const statusElement = document.getElementById("status");
    const loginBtn = document.getElementById("loginBtn");
    const addProductBtn = document.getElementById("addProductBtn");

    let allProducts = [];

    function showStatus(message) {
      statusElement.textContent = message;
    }

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
      if (!value) return "";

      try {
        const url = new URL(value);

        if (url.protocol === "https:" || url.protocol === "http:") {
          return url.href;
        }
      } catch (_) {}

      return "";
    }

    async function updateLoginState() {
      const { data, error } = await db.auth.getSession();

      if (error) {
        loginState.textContent = "Хатои санҷиши ҳисоб.";
        adminPanel.hidden = true;
        logoutBtn.hidden = true;
        loginForm.hidden = false;
        return;
      }

      const user = data.session?.user;

      const isAdmin =
        !!user &&
        user.email?.toLowerCase() === ADMIN_EMAIL.toLowerCase();

      loginState.textContent = isAdmin
        ? "Ту ҳамчун администратор ворид шудӣ."
        : user
          ? "Ин ҳисоб ҳуқуқи администратор надорад."
          : "Ба ҳисоб ворид нашудаӣ.";

      adminPanel.hidden = !isAdmin;
      logoutBtn.hidden = !user;
      loginForm.hidden = !!user;
    }

    async function loadProducts() {
      const container = document.getElementById("products");
      container.textContent = "Маҳсулот бор шуда истодааст...";

      const { data, error } = await db
        .from("products")
        .select("id, name, description, price, category, image_url, stock")
        .eq("is_active", true)
        .order("created_at", { ascending: false });

      if (error) {
        container.textContent =
          "Маҳсулот бор нашуд. Танзимоти Supabase-ро санҷ.";
        showStatus("Хатои бор кардани маҳсулот: " + error.message);
        return;
      }

      allProducts = data || [];
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

      if (filtered.length === 0) {
        container.textContent = searchTerm
          ? "Маҳсулоти мувофиқ ёфт нашуд."
          : "Ҳоло маҳсулот нест.";
        return;
      }

      container.innerHTML = filtered.map(product => {
        const image = safeImageURL(product.image_url);

        return `
          <article class="product">
            ${
              image
                ? `<img src="${escapeHTML(image)}"
                     alt="${escapeHTML(product.name)}"
                     loading="lazy">`
                : ""
            }
            <h3>${escapeHTML(product.name)}</h3>
            <p>${escapeHTML(product.description || "")}</p>
            <p>Категория: ${escapeHTML(product.category)}</p>
            <p class="price">${escapeHTML(product.price)} €</p>
            <p>Миқдор: ${escapeHTML(product.stock)}</p>
          </article>
        `;
      }).join("");
    }

    loginForm.addEventListener("submit", async event => {
      event.preventDefault();

      showStatus("Ба ҳисоб ворид шуда истодаӣ...");
      loginBtn.disabled = true;

      try {
        const email = document.getElementById("email").value.trim();
        const password = document.getElementById("password").value;

        const { error } = await db.auth.signInWithPassword({
          email,
          password
        });

        if (error) {
          showStatus("Ворид нашуд: " + error.message);
          return;
        }

        loginForm.reset();
        await updateLoginState();
        showStatus("Воридшавӣ бомуваффақият анҷом ёфт.");
      } catch (error) {
        showStatus("Хато ҳангоми воридшавӣ: " + error.message);
      } finally {
        loginBtn.disabled = false;
      }
    });

    logoutBtn.addEventListener("click", async () => {
      const { error } = await db.auth.signOut();

      showStatus(
        error ? "Хато: " + error.message : "Аз ҳисоб баромадӣ."
      );

      await updateLoginState();
    });

    productForm.addEventListener("submit", async event => {
      event.preventDefault();

      showStatus("Маҳсулот илова шуда истодааст...");
      addProductBtn.disabled = true;

      try {
        const { data: sessionData, error: sessionError } =
          await db.auth.getSession();

        if (sessionError) {
          showStatus("Хатои санҷиши ҳисоб: " + sessionError.message);
          return;
        }

        const user = sessionData.session?.user;

        if (
          !user ||
          user.email?.toLowerCase() !== ADMIN_EMAIL.toLowerCase()
        ) {
          showStatus("Аввал ҳамчун администратор ворид шав.");
          return;
        }

        const product = {
          name: document.getElementById("name").value.trim(),
          description: document.getElementById("description").value.trim(),
          price: Number(document.getElementById("price").value),
          category: document.getElementById("category").value.trim(),
          image_url:
            document.getElementById("image_url").value.trim() || null,
          stock: Number(document.getElementById("stock").value),
          is_active: true
        };

        if (
          !product.name ||
          !product.category ||
          !Number.isFinite(product.price) ||
          product.price < 0 ||
          !Number.isInteger(product.stock) ||
          product.stock < 0
        ) {
          showStatus("Ном, категория, нарх ва миқдорро дуруст пур кун.");
          return;
        }

        const { error } = await db
          .from("products")
          .insert(product);

        if (error) {
          showStatus("Маҳсулот илова нашуд: " + error.message);
          return;
        }

        showStatus("Маҳсулот бомуваффақият илова шуд!");
        productForm.reset();
        document.getElementById("stock").value = "1";

        await loadProducts();
      } catch (error) {
        showStatus("Хато ҳангоми илова кардани маҳсулот: " + error.message);
      } finally {
        addProductBtn.disabled = false;
      }
    });

    document
      .getElementById("search")
      .addEventListener("input", renderProducts);

    db.auth.onAuthStateChange(() => {
      setTimeout(() => {
        updateLoginState();
      }, 0);
    });

    updateLoginState();
    loadProducts();
  </script>
</body>
</html>
