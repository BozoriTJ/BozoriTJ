
<!DOCTYPE html>
<html lang="tg">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>BozoriTJ — Бозори онлайн</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f4f6f8;
      color: #222;
    }
    header {
      background: #087f5b;
      color: white;
      padding: 22px 12px;
      text-align: center;
    }
    .search, input, textarea, select {
      box-sizing: border-box;
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
    }
    main {
      max-width: 1000px;
      margin: auto;
      padding: 15px;
    }
    .panel, .product {
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
    }
    .secondary { background: #555; }
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
    }
    .price { color: #087f5b; font-weight: bold; }
    #status { white-space: pre-wrap; }
    footer { text-align: center; padding: 20px; color: #666; }
    [hidden] { display: none !important; }
  </style>
</head>
<body>
  <header>
    <h1>BozoriTJ</h1>
    <p>Бозори онлайни Тоҷикистон</p>
  </header>

  <main>
    <section class="panel">
      <h2>Воридшавии администратор</h2>
      <form id="loginForm">
        <input id="email" type="email"
          placeholder="Email" required>
        <input id="password" type="password"
          placeholder="Парол" required>
        <button type="submit">Ворид шудан</button>
      </form>
      <p id="loginInfo">Барои илова кардани маҳсулот ворид шав.</p>
      <button id="logoutBtn" class="secondary" hidden>
        Баромадан
      </button>
    </section>

    <section class="panel" id="adminPanel" hidden>
      <h2>Маҳсулоти нав илова кардан</h2>
      <form id="productForm">
        <input id="name" placeholder="Номи маҳсулот" required>
        <textarea id="description"
          placeholder="Тавсифи маҳсулот"></textarea>
        <input id="price" type="number" min="0" step="0.01"
          placeholder="Нарх бо сомонӣ" required>
        <input id="category" placeholder="Категория" required>
        <input id="image_url" type="url"
          placeholder="Пайванди акси маҳсулот (URL)">
        <input id="stock" type="number" min="0" step="1"
          value="1" placeholder="Миқдор">
        <button type="submit">Илова кардани маҳсулот</button>
      </form>
    </section>

    <p id="status" role="status"></p>

    <input class="search" id="search"
      placeholder="Ҷустуҷӯи маҳсулот...">

    <h2>Маҳсулот</h2>
    <div id="products">Маҳсулот бор шуда истодааст...</div>
  </main>

  <footer>BozoriTJ © 2026</footer>

  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <script>
    const SUPABASE_URL =
      "https://ljidohteqippikkuycfk.supabase.co";

    // Калиди Publishable-и худро дар дохили нохунак гузор.
    const SUPABASE_PUBLISHABLE_KEY = "sb_publishable_6xy2FHR3KwMEpFrmbi7nIQ_UMz97rIC";

    const ADMIN_EMAIL = "firuzhamidov2006@gmail.com";

    const db = window.supabase.createClient(
      SUPABASE_URL,
      SUPABASE_PUBLISHABLE_KEY
    );

    const statusBox = document.getElementById("status");
    const adminPanel = document.getElementById("adminPanel");
    const loginForm = document.getElementById("loginForm");
    const logoutBtn = document.getElementById("logoutBtn");
    const productForm = document.getElementById("productForm");
    const productsBox = document.getElementById("products");
    let allProducts = [];

    function showStatus(message) {
      statusBox.textContent = message;
    }

    function makeElement(tag, text, className) {
      const el = document.createElement(tag);
      el.textContent = text || "";
      if (className) el.className = className;
      return el;
    }

    function renderProducts() {
      const query = document.getElementById("search")
        .value.trim().toLowerCase();

      productsBox.replaceChildren();

      const filtered = allProducts.filter(p =>
        (p.name || "").toLowerCase().includes(query) ||
        (p.category || "").toLowerCase().includes(query)
      );

      if (!filtered.length) {
        productsBox.appendChild(makeElement(
          "p", "Маҳсулот ёфт нашуд."
        ));
        return;
      }

      filtered.forEach(p => {
        const card = makeElement("article", "", "product");

        if (p.image_url) {
          try {
            const url = new URL(p.image_url);
            if (url.protocol === "https:" ||
                url.protocol === "http:") {
              const img = document.createElement("img");
              img.src = url.href;
              img.alt = p.name || "Маҳсулот";
              img.loading = "lazy";
              img.onerror = () => img.remove();
              card.appendChild(img);
            }
          } catch (_) {}
        }

        card.appendChild(makeElement("h3", p.name));
        card.appendChild(makeElement("p", p.category));
        card.appendChild(makeElement(
          "p", (p.description || "")
        ));
        card.appendChild(makeElement(
          "p", Number(p.price).toLocaleString("tg-TJ") +
            " сомонӣ", "price"
        ));
        productsBox.appendChild(card);
      });
    }

    async function loadProducts() {
      productsBox.textContent = "Бор шуда истодааст...";

      const { data, error } = await db
        .from("products")
        .select("id,name,description,price,category,image_url,stock")
        .eq("is_active", true)
        .order("created_at", { ascending: false });

      if (error) {
        productsBox.textContent = "Хатои боркунии маҳсулот.";
        showStatus("Хато: " + error.message);
        return;
      }

      allProducts = data || [];
      renderProducts();
    }

    async function updateLoginState() {
      const { data } = await db.auth.getSession();
      const user = data.session?.user;
      const isAdmin = user &&
        user.email?.toLowerCase() === ADMIN_EMAIL.toLowerCase();

      loginForm.hidden = !!user;
      logoutBtn.hidden = !user;
      adminPanel.hidden = !isAdmin;

      document.getElementById("loginInfo").textContent =
        !user ? "Барои илова кардани маҳсулот ворид шав."
        : isAdmin ? "Ту ҳамчун администратор ворид шудӣ."
        : "Ин ҳисоб ҳуқуқи илова кардани маҳсулот надорад.";

      if (user && !isAdmin) {
        await db.auth.signOut();
        showStatus("Ин ҳисоб администратор нест.");
      }
    }

    loginForm.addEventListener("submit", async event => {
      event.preventDefault();
      showStatus("Ворид шуда истодааст...");

      const { error } = await db.auth.signInWithPassword({
        email: document.getElementById("email").value.trim(),
        password: document.getElementById("password").value
      });

      if (error) {
        showStatus("Ворид нашуд: " + error.message);
      } else {
        showStatus("Воридшавӣ бомуваффақият анҷом ёфт.");
        loginForm.reset();
      }
      await updateLoginState();
    });

    logoutBtn.addEventListener("click", async () => {
      const { error } = await db.auth.signOut();
      showStatus(error ? error.message : "Аз ҳисоб баромадӣ.");
      await updateLoginState();
    });

    productForm.addEventListener("submit", async event => {
      event.preventDefault();
      showStatus("Маҳсулот илова шуда истодааст...");

      const { data: sessionData } = await db.auth.getSession();
      const user = sessionData.session?.user;

      if (!user ||
          user.email?.toLowerCase() !== ADMIN_EMAIL.toLowerCase()) {
        showStatus("Аввал ҳамчун администратор ворид шав.");
        return;
      }

      const product = {
        name: document.getElementById("name").value.trim(),
        description:
          document.getElementById("description").value.trim(),
        price: Number(document.getElementById("price").value),
        category: document.getElementById("category").value.trim(),
        image_url:
          document.getElementById("image_url").value.trim() || null,
        stock: Number(document.getElementById("stock").value),
        is_active: true
      };

      if (!product.name || !product.category ||
          !Number.isFinite(product.price) || product.price < 0 ||
          !Number.isInteger(product.stock) || product.stock < 0) {
        showStatus("Ном, категория, нарх ва миқдорро дуруст пур кун.");
        return;
      }

      const { error } = await db.from("products").insert(product);

      if (error) {
        showStatus("Илова нашуд: " + error.message);
        return;
      }

      showStatus("Маҳсулот бомуваффақият илова шуд!");
      productForm.reset();
      document.getElementById("stock").value = "1";
      await loadProducts();
    });

    document.getElementById("search")
      .addEventListener("input", renderProducts);

    db.auth.onAuthStateChange(() => {
      setTimeout(() => updateLoginState(), 0);
    });

    updateLoginState();
    loadProducts();
  </script>
</body>
</html>
