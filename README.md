<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fish Marketing Notebook</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #eef6fb;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #2c3e50;
    }
    .container {
      max-width: 900px;
      margin: auto;
      background: #fff;
      padding: 20px;
      border-radius: 15px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    }
    label {
      display: block;
      margin-top: 10px;
      font-weight: bold;
    }
    input {
      width: 100%;
      padding: 10px;
      margin-top: 5px;
      border: 1px solid #bbb;
      border-radius: 8px;
    }
    button {
      margin-top: 15px;
      padding: 12px 20px;
      border: none;
      background: #3498db;
      color: white;
      font-size: 16px;
      border-radius: 8px;
      cursor: pointer;
      transition: 0.3s;
    }
    button:hover {
      background: #2980b9;
    }
    table {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
    }
    th, td {
      border: 1px solid #ddd;
      padding: 10px;
      text-align: center;
    }
    th {
      background: #3498db;
      color: white;
    }
    .total {
      margin-top: 15px;
      font-size: 20px;
      font-weight: bold;
      text-align: right;
      color: #2c3e50;
    }
  </style>
</head>
<body>
  <h1>🐟 Fish Marketing Notebook</h1>
  <div class="container">
    <label>Seller Name</label>
    <input type="text" id="seller" placeholder="Enter seller name">

    <label>Weight (Kg)</label>
    <input type="number" id="weight" placeholder="Enter weight in Kg">

    <label>Rate per Kg</label>
    <input type="number" id="rate" placeholder="Enter rate per Kg">

    <button onclick="addEntry()">➕ Add Entry</button>
    <button onclick="clearData()" style="background:#e74c3c;">🗑 Clear All</button>

    <table id="records">
      <thead>
        <tr>
          <th>Seller</th>
          <th>Weight (Kg)</th>
          <th>Rate (₹)</th>
          <th>Total (₹)</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>

    <div class="total" id="grandTotal">Grand Total: ₹0</div>
  </div>

  <script>
    let grandTotal = 0;
    let records = JSON.parse(localStorage.getItem("fishRecords")) || [];

    function displayRecords() {
      const table = document.getElementById("records").getElementsByTagName("tbody")[0];
      table.innerHTML = "";
      grandTotal = 0;

      records.forEach(entry => {
        const newRow = table.insertRow();
        newRow.innerHTML = `
          <td>${entry.seller}</td>
          <td>${entry.weight}</td>
          <td>${entry.rate}</td>
          <td>${entry.total}</td>
        `;
        grandTotal += entry.total;
      });

      document.getElementById("grandTotal").innerText = "Grand Total: ₹" + grandTotal;
    }

    function addEntry() {
      const seller = document.getElementById("seller").value.trim();
      const weight = parseFloat(document.getElementById("weight").value);
      const rate = parseFloat(document.getElementById("rate").value);

      if (!seller || isNaN(weight) || isNaN(rate)) {
        alert("⚠️ Please enter all details correctly.");
        return;
      }

      const total = weight * rate;
      const entry = { seller, weight, rate, total };

      records.push(entry);
      localStorage.setItem("fishRecords", JSON.stringify(records));

      displayRecords();

      document.getElementById("seller").value = "";
      document.getElementById("weight").value = "";
      document.getElementById("rate").value = "";
    }

    function clearData() {
      if (confirm("Are you sure you want to clear all records?")) {
        records = [];
        localStorage.removeItem("fishRecords");
        displayRecords();
      }
    }

    displayRecords();
  </script>
</body>
</html>
