
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="subhamt" content="width=device-width, initial-scale=1" />
<title>Color Trading Game by [Tera Naam]</first>
<style>
  body { font-family: Arial, sans-serif; padding: 20px; background: #f0f0f0; }
  h2 { text-align: center; }
  .color-list { display: flex; flex-wrap: wrap; justify-content: center; gap: 15px; margin-top: 20px; }
  .color-card { width: 100px; padding: 15px; border-radius: 8px; color: white; text-align: center; font-weight: bold; cursor: pointer; }
  .red { background: #e74c3c; }
  .green { background: #27ae60; }
  .blue { background: #2980b9; }
  .yellow { background: #f1c40f; color: #333; }
  button { margin-top: 10px; padding: 8px 15px; border: none; border-radius: 5px; cursor: pointer; }
  #balance { font-size: 18px; margin-top: 20px; text-align: center; }
  .message { text-align: center; margin-top: 10px; color: #333; }
  footer { text-align: center; margin-top: 30px; color: #666; font-size: 14px; }
</style>
</head>
<body>

<h2>Color Trading Game</h2>
<div id="balance">Balance: $1000</div>

<div class="color-list" id="colors"></div>

<div class="message" id="message"></div>

<footer>Made by [Tera Naam]</footer>

<script>
  let balance = 1000;

  const colors = [
    { name: "Red", price: 100, class: "red", owned: 0 },
    { name: "Green", price: 150, class: "green", owned: 0 },
    { name: "Blue", price: 200, class: "blue", owned: 0 },
    { name: "Yellow", price: 80, class: "yellow", owned: 0 }
  ];

  const colorsDiv = document.getElementById("colors");
  const balanceDiv = document.getElementById("balance");
  const messageDiv = document.getElementById("message");

  function updateBalance() {
    balanceDiv.textContent = `Balance: $${balance}`;
  }

  function showMessage(text) {
    messageDiv.textContent = text;
    setTimeout(() => {
      messageDiv.textContent = "";
    }, 3000);
  }

  function createColorCard(color, index) {
    const card = document.createElement("div");
    card.className = `color-card ${color.class}`;
    card.innerHTML = `
      ${color.name} <br/>
      Price: $${color.price} <br/>
      Owned: ${color.owned} <br/>
      <button onclick="buyColor(${index})">Buy</button> 
      <button onclick="sellColor(${index})">Sell</button>
    `;
    return card;
  }

  function renderColors() {
    colorsDiv.innerHTML = "";
    colors.forEach((color, idx) => {
      colorsDiv.appendChild(createColorCard(color, idx));
    });
  }

  function buyColor(index) {
    const color = colors[index];
    if (balance >= color.price) {
      balance -= color.price;
      color.owned++;
      color.price = Math.floor(color.price * 1.1);
      updateBalance();
      renderColors();
      showMessage(`You bought 1 ${color.name}`);
    } else {
      showMessage("Not enough balance!");
    }
  }

  function sellColor(index) {
    const color = colors[index];
    if (color.owned > 0) {
      balance += color.price;
      color.owned--;
      color.price = Math.max(50, Math.floor(color.price * 0.9));
      updateBalance();
      renderColors();
      showMessage(`You sold 1 ${color.name}`);
    } else {
      showMessage(`You don't own any ${color.name}`);
    }
  }

  function randomPriceChange() {
    colors.forEach(color => {
      let changePercent = (Math.random() * 20) - 10;
      let changeAmount = Math.floor(color.price * (changePercent / 100));
      color.price = Math.max(50, color.price + changeAmount);
    });
    renderColors();
  }

  setInterval(randomPriceChange, 5000);

  // Initial render
  updateBalance();
  renderColors();
</script>

</body>
</html>
