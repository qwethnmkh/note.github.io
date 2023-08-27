<!DOCTYPE html>
<html>
<head>
  <title>Bitcoin Price Tracker</title>
  <style>
    /* CSS styles for the table */
    body {
      font-family: Arial, sans-serif;
      background-color: #cfcfcf;
      margin: 0;
      padding: 20px;
    }

    h1 {
      color: orange;
      margin-bottom: 20px;
      text-align: center;
    }

    .container {
      max-width: 600px;
      margin: 0 auto;
      background-color: #f1f1f1;
      border-radius: 4px;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
      padding: 20px;
    }

    form {
      display: flex;
      flex-direction: column;
      margin-bottom: 20px;
    }

    label {
      margin-bottom: 8px;
      font-weight: bold;
    }

    input[type="date"],
    input[type="number"] {
      padding: 8px;
      border: 1px solid #ddd;
      border-radius: 4px;
      margin-bottom: 12px;
    }

    button {
      padding: 10px 16px;
      background-color: #4caf50;
      color: #fff;
      border: none;
      border-radius: 4px;
      font-size: 14px;
      cursor: pointer;
      transition: background-color 0.3s;
    }

    button:hover {
      background-color: orange;
    }

    button:disabled {
      background-color: #4e4e4e;
      cursor: not-allowed;
    }

    table {
      border-collapse: collapse;
      width: 100%;
    }

    th, td {
      padding: 12px 8px;
      text-align: left;
      border-bottom: 1px solid #ddd;
    }

    th {
      background-color: #bebebe;
      font-weight: bold;
      color: orange;
    }

    #priceTable {
      background: #bfbfbf;
    }
    
    #cutton {
      width: 50px;
      height: 20px;
      font-size: 7px;
      background: red;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Bitcoin Price Note</h1>
    <img src="https://i.postimg.cc/MGZ0HNJr/imagine-Ai.png" width="330" height="290">
    <button id="cutton" onclick="confirmReset()">Reset</button>
    <form id="priceForm">
      <label for="dateInput">Date:</label>
      <input type="date" id="dateInput" required>

      <label for="priceInput">Price (USD):</label>
      <input type="number" id="priceInput" required>

      <button type="button" id="saveButton" onclick="saveData()" disabled>Save</button>
    </form>

    <table id="priceTable">
      <tr>
        <th>Date</th>
        <th>Price (USD $)</th>
      </tr>
    </table>
  </div>

  <script>
    // JavaScript to populate the table with data
    var priceTable = document.getElementById("priceTable");
    var priceForm = document.getElementById("priceForm");
    var saveButton = document.getElementById("saveButton");

    priceForm.addEventListener("input", function(event) {
      var dateInput = document.getElementById("dateInput");
      var priceInput = document.getElementById("priceInput");

      saveButton.disabled = !dateInput.value || !priceInput.value;
    });

    priceForm.addEventListener("submit", function(event) {
      event.preventDefault(); // Prevent form submission

      // Get input values
      var dateInput = document.getElementById("dateInput");
      var priceInput = document.getElementById("priceInput");

      // Create table row
      var row = document.createElement("tr");
      var dateCell = document.createElement("td");
      var priceCell = document.createElement("td");

      // Set cell values
      dateCell.textContent = dateInput.value;
      priceCell.textContent = priceInput.value;

      // Append cells to row
      row.appendChild(dateCell);
      row.appendChild(priceCell);

      // Append row to table
      priceTable.appendChild(row);

      // Save data to local storage
      saveToLocalStorage(dateInput.value, priceInput.value);

      // Clear input fields
      dateInput.value = "";
      priceInput.value = "";

      saveButton.disabled = true;
    });

    function saveData() {
      // Get input values
      var dateInput = document.getElementById("dateInput");
      var priceInput = document.getElementById("priceInput");

      // Create table row
      var row = document.createElement("tr");
      var dateCell = document.createElement("td");
      var priceCell = document.createElement("td");

      // Set cell values
      dateCell.textContent = dateInput.value;
      priceCell.textContent = priceInput.value;

      // Append cells to row
      row.appendChild(dateCell);
      row.appendChild(priceCell);

      // Append row to table
      priceTable.appendChild(row);

      // Save data to local storage
      saveToLocalStorage(dateInput.value, priceInput.value);

      // Clear input fields
      dateInput.value = "";
      priceInput.value = "";

      saveButton.disabled = true;
    }

    function saveToLocalStorage(date, price) {
      // Get existing data from local storage or initialize an empty array
      var existingData = JSON.parse(localStorage.getItem("bitcoinData")) || [];

      // Create a new entry object
      var entry = {
        date: date,
        price: price
      };

      // Add the new entry to the existing data array
      existingData.push(entry);

      // Store the updated data array in local storage
      localStorage.setItem("bitcoinData", JSON.stringify(existingData));
    }

    function resetData() {
      // Clear the table
      priceTable.innerHTML = '<tr><th>Date</th><th>Price (USD $)</th></tr>';

      // Clear local storage
      localStorage.removeItem("bitcoinData");
    }

    function confirmReset() {
      var confirmation = confirm("Are you sure that you want to delete your data? When you click 'ok', the notes of the prices will not display.");

      if (confirmation) {
        resetData();
      }
    }

    // Load data from local storage on page load
    window.addEventListener("DOMContentLoaded", function() {
      var existingData = JSON.parse(localStorage.getItem("bitcoinData")) || [];

      // Populate the table with existing data
      existingData.forEach(function(entry) {
        var row = document.createElement("tr");
        var dateCell = document.createElement("td");
        var priceCell = document.createElement("td");

        dateCell.textContent = entry.date;
        priceCell.textContent = entry.price;

        row.appendChild(dateCell);
        row.appendChild(priceCell);

        priceTable.appendChild(row);
      });
    });
  </script>
</body>
</html>
