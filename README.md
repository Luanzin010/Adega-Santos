<!DOCTYPE html>
<html lang="pt-BR">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Adega Santos</title>
  <base href="https://adegasantos.github.io/Adega-Santos/">
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background: #fff;
    }

    header {
      background: #333;
      color: #fff;
      padding: 1rem;
      text-align: center;
    }

    .container {
      display: flex;
      flex-wrap: wrap;
      gap: 20px;
      padding: 20px;
      justify-content: center;
    }

    .product {
      border: 1px solid #ccc;
      border-radius: 8px;
      width: 200px;
      text-align: center;
      padding: 10px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }

    .product img {
      width: 100%;
      border-radius: 8px;
    }

    .buy-button {
      background: #333;
      color: #fff;
      border: none;
      padding: 10px;
      margin-top: 10px;
      cursor: pointer;
      border-radius: 5px;
    }

    .cart {
      position: fixed;
      top: 10px;
      right: 10px;
      background: #fff;
      border: 1px solid #ccc;
      padding: 10px;
      width: 300px;
      max-height: 400px;
      overflow-y: auto;
      border-radius: 8px;
      display: none;
      flex-direction: column;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
    }

    .cart h2 {
      margin-top: 0;
    }

    .cart-item {
      display: flex;
      justify-content: space-between;
      margin-bottom: 5px;
    }

    .remove-item {
      background: red;
      color: #fff;
      border: none;
      border-radius: 50%;
      cursor: pointer;
      padding: 2px 6px;
    }

    .checkout {
      background: green;
      color: #fff;
      border: none;
      padding: 10px;
      margin-top: 10px;
      cursor: pointer;
      border-radius: 5px;
    }

    .payment-modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      display: none;
      justify-content: center;
      align-items: center;
    }

    .payment-content {
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      width: 300px;
      text-align: center;
    }

    .payment-option {
      margin: 10px 0;
      padding: 10px;
      border: 1px solid #333;
      cursor: pointer;
      border-radius: 5px;
    }

    .payment-option.selected {
      background: #333;
      color: #fff;
    }
  </style>
</head>

<body>

  <header>
    <h1>Adega Santos</h1>
  </header>

  <div class="container">
    <div class="product">
      <img src="https://imgur.com/hp4BkXr.jpg" alt="Whisky Jack Daniels Maçã Verde">
      <h3>Whisky Jack Daniels Maçã Verde</h3>
      <p>Categoria: Whisky</p>
      <span>R$ 129,90</span>
      <button class="buy-button">Comprar</button>
    </div>

    <div class="product">
      <img src="https://imgur.com/lIUw1mm.jpg" alt="Whisky Royal Salut 21 anos">
      <h3>Whisky Royal Salut 21 anos</h3>
      <p>Categoria: Whisky</p>
      <span>R$ 1.250,90</span>
      <button class="buy-button">Comprar</button>
    </div>

    <div class="product">
      <img src="https://imgur.com/XzxAQtM.jpg" alt="Whisky Gold Label Reserve">
      <h3>Whisky Gold Label Reserve</h3>
      <p>Categoria: Whisky</p>
      <span>R$ 259,99</span>
      <button class="buy-button">Comprar</button>
    </div>

    <div class="product">
      <img src="https://imgur.com/t30YUsZ.jpg" alt="Gin Tanqueray Bossa Nova Lime">
      <h3>Gin Tanqueray Bossa Nova Lime</h3>
      <p>Categoria: Gin</p>
      <span>R$ 129,90</span>
      <button class="buy-button">Comprar</button>
    </div>

    <div class="product">
      <img src="https://imgur.com/IgPpFoq.jpg" alt="Gin Tanqueray Royale">
      <h3>Gin Tanqueray Royale</h3>
      <p>Categoria: Gin</p>
      <span>R$ 129,90</span>
      <button class="buy-button">Comprar</button>
    </div>

    <div class="product">
      <img src="https://imgur.com/yBmbK3Z.jpg" alt="Gin Tanqueray Royale">
      <h3>Gin Tanqueray Royale</h3>
      <p>Categoria: Gin</p>
      <span>R$ 129,90</span>
      <button class="buy-button">Comprar</button>
    </div>
  </div>

  <div class="cart" id="cart">
    <h2>Carrinho</h2>
    <div id="cart-items"></div>
    <p id="cart-total">Total: R$ 0,00</p>
    <button id="checkout-btn" class="checkout">Finalizar Compra</button>
    <button id="close-cart">Fechar</button>
  </div>

  <div class="payment-modal" id="payment-modal" aria-hidden="true">
    <div class="payment-content">
      <h2>Escolha o pagamento</h2>
      <div id="pix-btn" class="payment-option">PIX</div>
      <div id="card-btn" class="payment-option">Cartão</div>
      <p id="payment-info"></p>
      <button id="confirm-payment" class="checkout">Confirmar Pagamento</button>
      <button id="close-payment">Fechar</button>
    </div>
  </div>

  <script>
    const cart = [];
    const productCards = document.querySelectorAll(".product");
    const cartEl = document.getElementById("cart");
    const cartItemsEl = document.getElementById("cart-items");
    const cartTotalEl = document.getElementById("cart-total");
    const checkoutBtn = document.getElementById("checkout-btn");
    const closeCartBtn = document.getElementById("close-cart");

    const paymentModal = document.getElementById("payment-modal");
    const closePaymentBtn = document.getElementById("close-payment");
    const pixBtn = document.getElementById("pix-btn");
    const cardBtn = document.getElementById("card-btn");
    const paymentInfoEl = document.getElementById("payment-info");
    const confirmPaymentBtn = document.getElementById("confirm-payment");

    function formatPrice(price) {
      return price.toLocaleString("pt-BR", { style: "currency", currency: "BRL" });
    }

    function parsePrice(priceStr) {
      return Number(priceStr.replace(/[^\d,]/g, "").replace(",", "."));
    }

    function updateCartDisplay() {
      if (cart.length === 0) {
        cartItemsEl.innerHTML = "<p>Carrinho vazio.</p>";
        cartTotalEl.textContent = "Total: R$ 0,00";
        checkoutBtn.disabled = true;
        return;
      }

      checkoutBtn.disabled = false;
      cartItemsEl.innerHTML = "";
      let total = 0;

      cart.forEach((item, index) => {
        total += item.price * item.quantity;

        const itemEl = document.createElement("div");
        itemEl.classList.add("cart-item");

        const nameSpan = document.createElement("span");
        nameSpan.textContent = `${item.name} x${item.quantity}`;
        itemEl.appendChild(nameSpan);

        const priceSpan = document.createElement("span");
        priceSpan.textContent = formatPrice(item.price * item.quantity);
        priceSpan.style.marginLeft = "10px";
        itemEl.appendChild(priceSpan);

        const removeBtn = document.createElement("button");
        removeBtn.textContent = "x";
        removeBtn.classList.add("remove-item");
        removeBtn.setAttribute("aria-label", `Remover ${item.name} do carrinho`);
        removeBtn.addEventListener("click", () => removeFromCart(index));

        itemEl.appendChild(removeBtn);
        cartItemsEl.appendChild(itemEl);
      });

      cartTotalEl.textContent = `Total: ${formatPrice(total)}`;
    }

    function addToCart(name, price) {
      const existingIndex = cart.findIndex(item => item.name === name);
      if (existingIndex > -1) {
        cart[existingIndex].quantity++;
      } else {
        cart.push({ name, price, quantity: 1 });
      }
      updateCartDisplay();
      showCart();
    }

    function removeFromCart(index) {
      cart.splice(index, 1);
      updateCartDisplay();
      if (cart.length === 0) hideCart();
    }

    function showCart() {
      cartEl.style.display = "flex";
    }

    function hideCart() {
      cartEl.style.display = "none";
    }

    closeCartBtn.addEventListener("click", hideCart);

    productCards.forEach(card => {
      const btn = card.querySelector(".buy-button");
      const name = card.querySelector("h3").textContent.trim();
      const priceText = card.querySelector("span").textContent.trim();
      const price = parsePrice(priceText);

      btn.addEventListener("click", () => addToCart(name, price));
    });

    checkoutBtn.addEventListener("click", () => openPaymentModal());

    function openPaymentModal() {
      paymentModal.style.display = "flex";
      paymentInfoEl.textContent = "";
      confirmPaymentBtn.disabled = true;
      selectedPayment = null;
      pixBtn.classList.remove("selected");
      cardBtn.classList.remove("selected");
    }

    function closePaymentModal() {
      paymentModal.style.display = "none";
    }

    closePaymentBtn.addEventListener("click", closePaymentModal);

    let selectedPayment = null;

    pixBtn.addEventListener("click", () => {
      selectedPayment = "PIX";
      pixBtn.classList.add("selected");
      cardBtn.classList.remove("selected");
      paymentInfoEl.textContent = "Chave PIX: adegasantos@pix.com.br\n[QR Code fictício]";
      confirmPaymentBtn.disabled = false;
    });

    cardBtn.addEventListener("click", () => {
      selectedPayment = "Cartão";
      cardBtn.classList.add("selected");
      pixBtn.classList.remove("selected");
      paymentInfoEl.textContent = "Você será redirecionado para o pagamento com cartão.";
      confirmPaymentBtn.disabled = false;
    });

    confirmPaymentBtn.addEventListener("click", () => {
      if (!selectedPayment) return;
      alert(`Pagamento via ${selectedPayment} confirmado! Obrigado pela compra.`);
      cart.length = 0;
      updateCartDisplay();
      hideCart();
      closePaymentModal();
    });
  </script>

</body>

</html>
