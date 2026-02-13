<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Daily Earnings Tracker</title>

<style>
body {
  font-family: Arial, sans-serif;
  background: #f2f5f9;
  margin: 0;
  padding: 20px;
}

.container {
  max-width: 500px;
  margin: auto;
  background: white;
  padding: 20px;
  border-radius: 12px;
  box-shadow: 0 5px 20px rgba(0,0,0,0.1);
}

h1 {
  text-align: center;
}

.balance-box {
  background: #111;
  color: white;
  padding: 15px;
  border-radius: 8px;
  text-align: center;
  margin-bottom: 20px;
}

form {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}

form input, form select {
  flex: 1;
  padding: 8px;
}

button {
  padding: 8px 12px;
  cursor: pointer;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  padding: 8px;
  border-bottom: 1px solid #eee;
}

.income {
  color: green;
}

.expense {
  color: red;
}

.delete-btn {
  background: red;
  color: white;
  border: none;
  padding: 4px 8px;
  cursor: pointer;
  border-radius: 4px;
}
</style>
</head>

<body>

<div class="container">
  <h1>Daily Earnings & Spending</h1>

  <div class="balance-box">
    <h2>Balance: ৳ <span id="balance">0</span></h2>
  </div>

  <form id="transactionForm">
    <input type="text" id="description" placeholder="Description" required>
    <input type="number" id="amount" placeholder="Amount" required>
    <select id="type">
      <option value="income">Income</option>
      <option value="expense">Expense</option>
    </select>
    <button type="submit">Add</button>
  </form>

  <ul id="transactionList"></ul>
</div>

<script>
const form = document.getElementById("transactionForm");
const list = document.getElementById("transactionList");
const balanceEl = document.getElementById("balance");

let transactions = JSON.parse(localStorage.getItem("transactions")) || [];

function updateLocalStorage() {
  localStorage.setItem("transactions", JSON.stringify(transactions));
}

function calculateBalance() {
  const balance = transactions.reduce((acc, tx) => {
    return tx.type === "income" ? acc + tx.amount : acc - tx.amount;
  }, 0);
  balanceEl.textContent = balance;
}

function renderTransactions() {
  list.innerHTML = "";

  transactions.forEach((tx, index) => {
    const li = document.createElement("li");
    li.classList.add(tx.type);

    li.innerHTML = `
      <span>${tx.description} - ৳${tx.amount}</span>
      <button class="delete-btn" onclick="deleteTransaction(${index})">X</button>
    `;

    list.appendChild(li);
  });

  calculateBalance();
}

function deleteTransaction(index) {
  transactions.splice(index, 1);
  updateLocalStorage();
  renderTransactions();
}

form.addEventListener("submit", function(e) {
  e.preventDefault();

  const description = document.getElementById("description").value;
  const amount = parseFloat(document.getElementById("amount").value);
  const type = document.getElementById("type").value;

  transactions.push({ description, amount, type });

  updateLocalStorage();
  renderTransactions();
  form.reset();
});

renderTransactions();
</script>

</body>
</html>
