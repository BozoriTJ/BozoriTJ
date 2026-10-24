
<!DOCTYPE html>
<html lang="tg">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>BozoriTJ — Расонидани хӯрок</title>

<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#f4f7f5;color:#222}
header{background:#087f5b;color:white;text-align:center;padding:24px 12px}
header h1{margin:0 0 8px}
nav{display:flex;justify-content:center;gap:22px;padding:14px;background:white;position:sticky;top:0;z-index:5;flex-wrap:wrap}
nav a{color:#087f5b;text-decoration:none;font-weight:bold}
main{max-width:950px;margin:auto;padding:16px}
.panel,.product{background:white;padding:16px;border-radius:12px;margin-bottom:16px;box-shadow:0 2px 8px #0000000b}
.products{display:grid;grid-template-columns:repeat(auto-fit,minmax(210px,1fr));gap:14px}
.emoji{font-size:44px}
.price{font-weight:bold;color:#087f5b}
button{background:#087f5b;color:white;border:0;border-radius:8px;padding:12px 16px;font-size:15px;cursor:pointer}
button:disabled{opacity:.55;cursor:not-allowed}
.remove{background:#b42318;padding:8px 10px}
input,textarea{display:block;width:100%;padding:12px;margin:8px 0 14px;border:1px solid #ccc;border-radius:8px;font-size:16px}
label{font-weight:bold}
#message,#reset-message{white-space:pre-wrap;overflow-wrap:anywhere}
.success{color:#087f5b}
.error{color:#b42318}
footer{text-align:center;padding:24px;color:#666}
#reset-password-panel{max-width:500px;margin:24px auto;padding:20px}
</style>
</head>

<body>
<header id="site-header">
  <h1>🍽️ BozoriTJ</h1>
  <p>Хӯрок фармоиш деҳ — ба хонаат расонем!</p>
</header>

<nav id="site-nav">
  <a href="#menu">Меню</a>
  <a href="#cart">Сабад</a>
  <a href="#order">Фармоиш</a>
</nav>

<!-- Шакли барқароркунии парол -->
<section class="panel" id="reset-password-panel" style="display:none">
  <h2>🔐 Пароли нави администратор</h2>
  <p>Пароли навро ворид кун ва онро бори дигар тасдиқ намо.</p>

  <form id="reset-password-form">
    <label for="new-password">Пароли нав (на кам аз 8 аломат)</label>
    <input
      id="new-password"
      type="password"
      minlength="8"
      required
      autocomplete="new-password"
      placeholder="Пароли нав"
    >

    <label for="confirm-password">Паролро такрор кун</label>
    <input
      id="confirm-password"
      type="password"
      minlength="8"
      required
      autocomplete="new-password"
      placeholder="Паролро такрор кун"
    >

    <button id="reset-submit-button" type="submit">
      Сабт кардани пароли нав
    </button>
  </form>

  <p id="reset-message" role="status" aria-live="polite"></p>
</section>

<main id="site-main">
  <section class="panel">
    <h2>Салом! 👋</h2>
    <p>Хӯроки дӯстдоштаатро интихоб кун.</p>
    <p id="connection">Пайвастшавӣ ба база санҷида мешавад...</p>
  </section>

  <h2 id="menu">🍕 Менюи хӯрок</h2>

  <section class="products">
    <article class="product">
      <div class="emoji">🍕</div>
      <h3>Питса</h3>
      <p>Питсаи болаззат</p>
      <p class="price">60 сомонӣ</p>
      <button type="button" onclick="addItem('Питса',60)">Ба сабад</button>
    </article>

    <article class="product">
      <div class="emoji">🍔</div>
      <h3>Бургер</h3>
      <p>Бургери гӯштӣ</p>
      <p class="price">35 сомонӣ</p>
      <button type="button" onclick="addItem('Бургер',35)">Ба сабад</button>
    </article>

    <article class="product">
      <div class="emoji">🍗</div>
      <h3>Мурғи бирён</h3>
      <p>Мурғи бирён бо ҳанут</p>
      <p class="price">45 сомонӣ</p>
      <button type="button" onclick="addItem('Мурғи бирён',45)">Ба сабад</button>
    </article>

    <article class="product">
      <div class="emoji">🥤</div>
      <h3>Нӯшокӣ</h3>
      <p>Нӯшокии хунук</p>
      <p class="price">10 сомонӣ</p>
      <button type="button" onclick="addItem('Нӯшокӣ',10)">Ба сабад</button>
    </article>
  </section>

  <section class="panel" id="cart">
    <h2>🛒 Сабади шумо</h2>
    <div id="cart-items">Сабад холӣ аст.</div>
    <h3>Ҳамагӣ: <span id="total">0</span> сомонӣ</h3>
    <button class="remove" type="button" onclick="clearCart()">Холӣ кардан</button>
  </section>

  <section class="panel" id="order">
    <h2>📦 Фармоиш додан</h2>

    <form id="order-form">
      <label for="customer-name">Ному насаб</label>
      <input id="customer-name" required maxlength="100"
             autocomplete="name" placeholder="Номи шумо">

      <label for="phone">Рақами телефон</label>
      <input id="phone" type="tel" required maxlength="30"
             autocomplete="tel" placeholder="+992...">

      <label for="address">Суроғаи расонидан</label>
      <textarea id="address" required maxlength="500"
                placeholder="Шаҳр, кӯча, хона"></textarea>

      <label for="notes">Шарҳи иловагӣ</label>
      <textarea id="notes" maxlength="1000"
                placeholder="Масалан: пеш аз расидан занг занед"></textarea>

      <button id="submit-button" type="submit">
        Фиристодани фармоиш
      </button>
    </form>

    <p id="message" role="status" aria-live="polite"></p>
  </section>
</main>

<footer id="site-footer">© BozoriTJ — Расонидани хӯрок</footer>

<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>

<script>
const SUPABASE_URL = "https://ljidohteqippikkuycfk.supabase.co";
const SUPABASE_KEY = "sb_publishable_6xy2FHR3KwMEpFrmbi7nIQ_UMz97rIC";

const db = window.supabase
  ? window.supabase.createClient(SUPABASE_URL, SUPABASE_KEY)
  : null;

let cart = [];
let sending = false;
let recoveryStarted = false;

/* ---------------- САБАД ---------------- */

function addItem(name, price) {
  const existing = cart.find(item => item.name === name);

  if (existing) {
    existing.quantity++;
  } else {
    cart.push({name, price, quantity: 1});
  }

  renderCart();
}

function clearCart() {
  cart = [];
  renderCart();
}

function renderCart() {
  const area = document.getElementById("cart-items");
  area.replaceChildren();

  if (cart.length === 0) {
    area.textContent = "Сабад холӣ аст.";
  }

  cart.forEach((item, index) => {
    const row = document.createElement("p");

    row.appendChild(document.createTextNode(
      `${item.name} × ${item.quantity} — ${item.price * item.quantity} сомонӣ `
    ));

    const button = document.createElement("button");
    button.type = "button";
    button.className = "remove";
    button.textContent = "Нест кардан";

    button.onclick = () => {
      cart.splice(index, 1);
      renderCart();
    };

    row.appendChild(button);
    area.appendChild(row);
  });

  const total = cart.reduce(
    (sum, item) => sum + item.price * item.quantity, 0
  );

  document.getElementById("total").textContent = total;
}

function showMessage(text, type) {
  const el = document.getElementById("message");
  el.textContent = text;
  el.className = type || "";
}

/* ---------------- ПАЙВАСТШАВӢ ---------------- */

async function checkConnection() {
  const el = document.getElementById("connection");

  if (!db) {
    el.textContent = "Хатогӣ: Supabase дастрас нест.";
    return;
  }

  try {
    const {error} = await db.auth.getSession();

    if (error) throw error;

    el.textContent = "Пайвастшавӣ ба Supabase дастрас аст.";
  } catch (error) {
    console.error("Санҷиши пайвастшавӣ:", error);
    el.textContent = "Ҳолати пайвастшавиро санҷидан лозим аст.";
  }
}

/* ---------------- БАРҚАРОРКУНИИ ПАРОЛ ---------------- */

const resetPanel = document.getElementById("reset-password-panel");
const resetForm = document.getElementById("reset-password-form");
const resetMessage = document.getElementById("reset-message");
const resetButton = document.getElementById("reset-submit-button");

function showRecoveryScreen() {
  resetPanel.style.display = "block";
  document.getElementById("site-header").style.display = "none";
  document.getElementById("site-nav").style.display = "none";
  document.getElementById("site-main").style.display = "none";
  document.getElementById("site-footer").style.display = "none";
}

async function checkPasswordRecovery() {
  if (!db || recoveryStarted) return;

  const hashParams = new URLSearchParams(
    window.location.hash.replace(/^#/, "")
  );
  const queryParams = new URLSearchParams(window.location.search);

  const recoveryType =
    hashParams.get("type") === "recovery" ||
    queryParams.get("type") === "recovery";

  const hasAuthTokens =
    hashParams.has("access_token") ||
    queryParams.has("token_hash");

  if (!recoveryType && !hasAuthTokens) return;

  recoveryStarted = true;
  showRecoveryScreen();
  resetMessage.textContent = "Пайванди барқароркунӣ санҷида мешавад...";

  try {
    // Supabase одатан токенро аз URL худкор коркард мекунад.
    const {data, error} = await db.auth.getSession();

    if (error) throw error;

    if (data.session) {
      resetMessage.textContent =
        "Пайванд тасдиқ шуд. Пароли навро ворид кун.";
      return;
    }

    resetMessage.textContent =
      "Сессияи барқароркунӣ пайдо нашуд. " +
      "Эҳтимол пайванд нодуруст ё муҳлаташ гузаштааст. " +
      "Аз Supabase паёми нав фирист ва пайванди охиринро кушо.";

  } catch (error) {
    console.error("Хатои барқароркунии парол:", error);
    resetMessage.textContent =
      "Хатои санҷиши пайванд. Паёми барқароркуниро аз нав фирист.";
  }
}

resetForm.addEventListener("submit", async function(event) {
  event.preventDefault();

  if (!db) {
    resetMessage.textContent = "Supabase дастрас нест.";
    return;
  }

  const password = document.getElementById("new-password").value;
  const confirmPassword = document.getElementById("confirm-password").value;

  if (password.length < 8) {
    resetMessage.textContent =
      "Парол бояд на кам аз 8 аломат бошад.";
    return;
  }

  if (password !== confirmPassword) {
    resetMessage.textContent = "Ду парол якхела нестанд.";
    return;
  }

  resetButton.disabled = true;
  resetMessage.textContent = "Парол сабт шуда истодааст...";

  try {
    const {data: sessionData, error: sessionError} =
      await db.auth.getSession();

    if (sessionError) throw sessionError;

    if (!sessionData.session) {
      throw new Error(
        "Сессияи барқароркунӣ нест. Пайванди навро аз почта кушо."
      );
    }

    const {error} = await db.auth.updateUser({
      password: password
    });

    if (error) throw error;

    resetMessage.textContent =
      "✅ Парол бомуваффақият иваз шуд!\n" +
      "Акнун ба панели администратор ворид шав.";

    resetForm.reset();

    // Токенҳои барқароркунӣ аз сатри суроға пок мешаванд.
    window.history.replaceState(
      null,
      document.title,
      window.location.pathname
    );

    const adminLink = document.createElement("p");
    const link = document.createElement("a");
    link.href = "/BozoriTJ/admin.html";
    link.textContent = "Кушодани панели администратор";
    link.style.color = "#087f5b";
    link.style.fontWeight = "bold";

    adminLink.appendChild(link);
    resetPanel.appendChild(adminLink);

  } catch (error) {
    console.error("Хатои иваз кардани парол:", error);
    resetMessage.textContent =
      "Парол иваз нашуд: " + (error.message || "Хатои номаълум");
  } finally {
    resetButton.disabled = false;
  }
});

/* ---------------- ФИРИСТОДАНИ ФАРМОИШ ---------------- */

document.getElementById("order-form").addEventListener(
  "submit",
  async function(event) {
    event.preventDefault();

    if (sending) return;

    if (!db) {
      showMessage("Пайвастшавӣ ба база дастрас нест.", "error");
      return;
    }

    if (cart.length === 0) {
      showMessage("Аввал аз меню хӯрок интихоб кун.", "error");
      return;
    }

    const customerName =
      document.getElementById("customer-name").value.trim();
    const phone =
      document.getElementById("phone").value.trim();
    const address =
      document.getElementById("address").value.trim();
    const notes =
      document.getElementById("notes").value.trim();

    if (!customerName || !phone || !address) {
      showMessage("Ном, телефон ва суроғаро пур кун.", "error");
      return;
    }

    const items = cart.map(item => ({
      name: item.name,
      quantity: item.quantity,
      price: item.price
    }));

    sending = true;

    const button = document.getElementById("submit-button");
    button.disabled = true;
    showMessage("Фармоиш фиристода мешавад...", "");

    try {
      const {data, error} = await db.rpc("submit_food_order", {
        p_customer_name: customerName,
        p_phone: phone,
        p_address: address,
        p_notes: notes,
        p_items: items
      });

      if (error) throw error;

      if (!data || data.success !== true || !data.order_id) {
        throw new Error(
          "База тасдиқи сабти фармоишро барнагардонд."
        );
      }

      showMessage(
        "✅ Фармоиши шумо сабт шуд!\n" +
        "Рақами фармоиш: " + data.order_id +
        "\nҲамагӣ: " + data.total + " сомонӣ",
        "success"
      );

      cart = [];
      renderCart();
      document.getElementById("order-form").reset();

    } catch (error) {
      console.error("Хатои фармоиш:", error);
      showMessage(
        "Фармоиш сабт нашуд.\nСабаб: " +
        (error.message || "Хатои номаълум"),
        "error"
      );
    } finally {
      sending = false;
      button.disabled = false;
    }
  }
);

/* ---------------- ОҒОЗ ---------------- */

renderCart();
checkConnection();
checkPasswordRecovery();

</script>
</body>
</html>
