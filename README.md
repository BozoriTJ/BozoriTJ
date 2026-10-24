
<!DOCTYPE html>
<html lang="tg">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BozoriTJ — Фармоиши хӯрок</title>
  <meta name="description" content="Фармоиши хӯрок ва расонидан дар Душанбе">
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: #f4f6f8;
      color: #20252b;
    }
    header {
      background: #087f5b;
      color: white;
      padding: 22px 15px;
      text-align: center;
    }
    header h1 { margin: 0 0 8px; }
    main { max-width: 850px; margin: auto; padding: 16px; }
    .card {
      background: white;
      border-radius: 12px;
      padding: 16px;
      margin-bottom: 16px;
      box-shadow: 0 2px 8px #0000000d;
    }
    .products {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
      gap: 12px;
    }
    .product {
      border: 1px solid #e2e6e9;
      padding: 12px;
      border-radius: 10px;
    }
    .emoji { font-size: 38px; }
    button {
      border: 0;
      border-radius: 8px;
      padding: 11px 14px;
      background: #087f5b;
      color: white;
      font-size: 15px;
      cursor: pointer;
    }
    button:disabled { opacity: .55; cursor: wait; }
    input, textarea {
      display: block;
      width: 100%;
      padding: 12px;
      margin: 9px 0 14px;
      border: 1px solid #ccd2d8;
      border-radius: 8px;
      font-size: 16px;
    }
    .item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 8px;
      padding: 10px 0;
      border-bottom: 1px solid #eee;
    }
    .item button { padding: 7px 11px; }
    .message { white-space: pre-wrap; overflow-wrap: anywhere; }
    .success { color: #087f5b; }
    .error { color: #c92a2a; }
    .muted { color: #667085; font-size: 14px; }
  </style>
</head>
<body>
<header>
  <h1>🍔 BozoriTJ</h1>
  <div>Фармоиши хӯрок дар Душанбе</div>
</header>

<main>
  <section class="card">
    <h2>Менюи хӯрок</h2>
    <p class="muted">Нархҳо барои санҷиш мебошанд.</p>
    <div id="products" class="products"></div>
  </section>

  <section class="card">
    <h2>🛒 Сабади шумо</h2>
    <div id="cart">Сабад холӣ аст.</div>
    <h3>Ҳамагӣ: <span id="total">0</span> сомонӣ</h3>
  </section>

  <section class="card">
    <h2>Маълумоти фармоиш</h2>
    <form id="orderForm">
      <label for="customerName">Ному насаб *</label>
      <input id="customerName" required maxlength="100"
        autocomplete="name" placeholder="Ному насаб">

      <label for="phone">Рақами телефон *</label>
      <input id="phone" required type="tel" maxlength="30"
        autocomplete="tel" placeholder="+992 ...">

      <label for="address">Суроғаи расонидан *</label>
      <input id="address" required maxlength="300"
        autocomplete="street-address" placeholder="Суроға дар Душанбе">

      <label for="notes">Шарҳи иловагӣ</label>
      <textarea id="notes" maxlength="500"
        placeholder="Масалан, назди даромадгоҳи бино занг занед"></textarea>

      <button id="submitButton" type="submit">Фармоиш додан</button>
      <p id="message" class="message" role="status"></p>
    </form>
  </section>
</main>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<script>
(() => {
  const SUPABASE_URL = "https://ljidohteqippikkuycfk.supabase.co";
  const SUPABASE_KEY = "sb_publishable_6xy2FHR3KwMEpFrmbi7nIQ_UMz97rIC";

  const db = window.supabase.createClient(
    SUPABASE_URL,
    SUPABASE_KEY
  );

  const products = [
    { id: "pizza", name: "Пицца", price: 60, emoji: "🍕" },
    { id: "burger", name: "Бургер", price: 35, emoji: "🍔" },
    { id: "chicken", name: "Мурғи бирён", price: 45, emoji: "🍗" },
    { id: "drink", name: "Нӯшокӣ", price: 10, emoji: "🥤" }
  ];

  const cart = {};
  const productsEl = document.getElementById("products");
  const cartEl = document.getElementById("cart");
  const totalEl = document.getElementById("total");
  const messageEl = document.getElementById("message");
  const submitButton = document.getElementById("submitButton");

  function renderProducts() {
    productsEl.innerHTML = products.map(p => `
      <div class="product">
        <div class="emoji">${p.emoji}</div>
        <h3>${p.name}</h3>
        <p>${p.price} сомонӣ</p>
        <button type="button" data-add="${p.id}">
          Ба сабад
        </button>
      </div>
    `).join("");
  }

  function renderCart() {
    const items = products.filter(p => cart[p.id] > 0);

    if (!items.length) {
      cartEl.textContent = "Сабад холӣ аст.";
      totalEl.textContent = "0";
      return;
    }

    cartEl.innerHTML = items.map(p => {
      const lineTotal = p.price * cart[p.id];
      return `
        <div class="item">
          <div>
            <strong>${p.name}</strong><br>
            ${p.price} × ${cart[p.id]} = ${lineTotal} сомонӣ
          </div>
          <div>
            <button type="button" data-minus="${p.id}">−</button>
            <button type="button" data-plus="${p.id}">+</button>
          </div>
        </div>
      `;
    }).join("");

    totalEl.textContent = items.reduce(
      (sum, p) => sum + p.price * cart[p.id], 0
    );
  }

  productsEl.addEventListener("click", event => {
    const button = event.target.closest("[data-add]");
    if (!button) return;

    const id = button.dataset.add;
    cart[id] = (cart[id] || 0) + 1;
    renderCart();
  });

  cartEl.addEventListener("click", event => {
    const button = event.target.closest("button");
    if (!button) return;

    const id = button.dataset.plus || button.dataset.minus;
    if (!id) return;

    if (button.dataset.plus) {
      cart[id] = (cart[id] || 0) + 1;
    } else {
      cart[id] = Math.max(0, (cart[id] || 0) - 1);
    }

    renderCart();
  });

  document.getElementById("orderForm").addEventListener(
    "submit", async event => {
      event.preventDefault();

      const items = products
        .filter(p => cart[p.id] > 0)
        .map(p => ({
          name: p.name,
          quantity: cart[p.id],
          price: p.price
        }));

      if (!items.length) {
        messageEl.className = "message error";
        messageEl.textContent = "Аввал аз меню хӯрок интихоб кун.";
        return;
      }

      if (items.some(item => item.quantity > 999)) {
        messageEl.className = "message error";
        messageEl.textContent = "Миқдори хӯрок аз ҳад зиёд аст.";
        return;
      }

      submitButton.disabled = true;
      messageEl.className = "message";
      messageEl.textContent = "Фармоиш фиристода мешавад...";

      try {
        const { data, error } = await db.rpc("submit_food_order", {
          p_customer_name: document.getElementById("customerName").value.trim(),
          p_phone: document.getElementById("phone").value.trim(),
          p_address: document.getElementById("address").value.trim(),
          p_notes: document.getElementById("notes").value.trim(),
          p_items: items
        });

        if (error) throw error;
        if (!data || data.success !== true) {
          throw new Error("Фармоиш тасдиқ нашуд.");
        }

        messageEl.className = "message success";
        messageEl.textContent =
          "✅ Фармоиш сабт шуд!\n" +
          "Рақами фармоиш: " + data.order_id + "\n" +
          "Ҳамагӣ: " + data.total + " сомонӣ";

        for (const id of Object.keys(cart)) delete cart[id];
        renderCart();
        document.getElementById("orderForm").reset();

      } catch (error) {
        console.error("Order submission error:", error);
        messageEl.className = "message error";
        messageEl.textContent =
          "Фармоиш сабт нашуд.\nХатогӣ: " +
          (error.message || "Хатои номаълум");
      } finally {
        submitButton.disabled = false;
      }
    }
  );

  renderProducts();
  renderCart();
})();
</script>
</body>
</html>
