<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>AWIFS - Mock Stock Competition</title>
  <style>
    /* RESET */
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body, html { font-family: 'Arial', sans-serif; background: #f4f7f9; color: #333; }

    /* SIDEBAR */
    .sidebar {
      position: fixed;
      top: 0;
      left: 0;
      width: 220px;
      height: 100vh;
      background-color: #2c3e50;
      color: #ecf0f1;
      padding: 20px;
      overflow-y: auto;
    }
    .sidebar h2 {
      text-align: center;
      margin-bottom: 30px;
      font-size: 24px;
      letter-spacing: 2px;
    }
    .sidebar ul {
      list-style: none;
    }
    .sidebar ul li {
      padding: 15px 20px;
      margin-bottom: 10px;
      cursor: pointer;
      border-radius: 3px;
    }
    .sidebar ul li:hover, .sidebar ul li.active {
      background-color: #34495e;
    }

    /* MAIN CONTENT */
    .main-content {
      margin-left: 240px;
      padding: 20px;
    }
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
    }
    .header .market-indices {
      display: flex;
      gap: 20px;
    }
    .header .market-indices div {
      background: #ecf0f1;
      padding: 10px 15px;
      border-radius: 5px;
      font-weight: bold;
    }
    .header .search-bar input {
      padding: 8px;
      width: 200px;
      border: 1px solid #ccc;
      border-radius: 3px;
    }
    .header .search-bar button {
      padding: 8px 12px;
      margin-left: 5px;
      border: none;
      background-color: #2980b9;
      color: #fff;
      border-radius: 3px;
      cursor: pointer;
    }

    /* CARDS */
    .card {
      background: #fff;
      padding: 20px;
      margin-bottom: 20px;
      border-radius: 5px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    h2.section-title {
      margin-bottom: 15px;
    }

    /* TABLE STYLES */
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
    }
    th, td {
      padding: 10px;
      border: 1px solid #bdc3c7;
      text-align: left;
    }
    th { background: #ecf0f1; }

    /* NEWS FEED */
    .news-feed {
      background: #f7f7f7;
      padding: 10px;
      border-radius: 5px;
      max-height: 300px;
      overflow-y: auto;
    }

    /* TABS (for additional navigation if needed) */
    .nav-tabs {
      display: flex;
      gap: 15px;
      margin-bottom: 20px;
    }
    .nav-tabs button {
      padding: 10px 15px;
      border: none;
      background-color: #bdc3c7;
      cursor: pointer;
      border-radius: 3px;
    }
    .nav-tabs button.active {
      background-color: #2980b9;
      color: #fff;
    }

    /* FORM STYLES */
    form label { display: block; margin: 10px 0 5px; }
    form input, form select { padding: 8px; width: 100%; border: 1px solid #ccc; border-radius: 3px; }
    form button { margin-top: 15px; padding: 10px 15px; border: none; background-color: #27ae60; color: #fff; border-radius: 3px; cursor: pointer; }
  </style>
</head>
<body>
  <!-- Sidebar -->
  <div class="sidebar">
    <h2>AWIFS<br>
    Mock Stock</h2>
    <ul>
      <li class="active" onclick="showSection('dashboard')">Dashboard</li>
      <li onclick="showSection('trade')">Trade</li>
      <li onclick="showSection('portfolio')">Portfolio</li>
      <li onclick="showSection('news')">News</li>
      <li onclick="showSection('leaderboard')">Leaderboard</li>
    </ul>
  </div>

  <!-- Main Content -->
  <div class="main-content">
    <!-- Dashboard Section -->
    <div id="dashboard" class="section">
      <div class="header">
        <div class="market-indices">
          <div>Index A: <span id="indexA">1000</span></div>
          <div>Index B: <span id="indexB">2000</span></div>
          <div>Index C: <span id="indexC">3000</span></div>
        </div>
        <div class="search-bar">
          <input type="text" id="searchInput" placeholder="Search stocks...">
          <button onclick="searchStock()">Search</button>
        </div>
      </div>

      <div class="card">
        <h2 class="section-title">Watchlist</h2>
        <table id="watchlistTable">
          <thead>
            <tr>
              <th>Stock</th>
              <th>Price</th>
              <th>% Change</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>THC</td>
              <td>$100</td>
              <td>+1%</td>
            </tr>
            <tr>
              <td>Fuel Zone</td>
              <td>$200</td>
              <td>-0.5%</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div class="card">
        <h2 class="section-title">Recent Transactions</h2>
        <table id="transactionsTable">
          <thead>
            <tr>
              <th>Time</th>
              <th>Stock</th>
              <th>Type</th>
              <th>Quantity</th>
              <th>Price</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>12:01</td>
              <td>THC</td>
              <td>Buy</td>
              <td>10</td>
              <td>$100</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Trade Section -->
    <div id="trade" class="section" style="display: none;">
      <h2 class="section-title">Trade</h2>
      <div class="card">
        <h3>Stock Information</h3>
        <div id="stockInfo">
          <p>Select a stock from your watchlist or search for one.</p>
        </div>
      </div>
      <div class="card">
        <h3>Order Form</h3>
        <form id="orderForm" onsubmit="submitOrder(event)">
          <label for="orderType">Order Type:</label>
          <select id="orderType">
            <option value="buy">Buy</option>
            <option value="sell">Sell</option>
          </select>
          <label for="stockSymbol">Stock Symbol:</label>
          <input type="text" id="stockSymbol" required>
          <label for="quantity">Quantity:</label>
          <input type="number" id="quantity" required>
          <label for="price">Price (for limit orders):</label>
          <input type="number" id="price">
          <button type="submit">Submit Order</button>
        </form>
      </div>
    </div>

    <!-- Portfolio Section -->
    <div id="portfolio" class="section" style="display: none;">
      <h2 class="section-title">Portfolio</h2>
      <div class="card">
        <h3>Your Holdings</h3>
        <table id="portfolioTable">
          <thead>
            <tr>
              <th>Stock</th>
              <th>Quantity</th>
              <th>Purchase Price</th>
              <th>Current Price</th>
              <th>Total Value</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>Fuel Zone</td>
              <td>50</td>
              <td>$100</td>
              <td>$105</td>
              <td>$5250</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="card">
        <h3>Portfolio Performance</h3>
        <canvas id="performanceChart" width="400" height="200"></canvas>
      </div>
    </div>

    <!-- News Section -->
    <div id="news" class="section" style="display: none;">
      <h2 class="section-title">Market News</h2>
      <div class="news-feed" id="newsFeed">
        <p>Chat corner temporarily closed down</p>
      </div>
    </div>

    <!-- Leaderboard Section -->
    <div id="leaderboard" class="section" style="display: none;">
      <h2 class="section-title">Leaderboard</h2>
      <div class="card">
        <table id="leaderboardTable">
          <thead>
            <tr>
              <th>Rank</th>
              <th>User</th>
              <th>Portfolio Value</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>1</td>
              <td>Kamya Gupta</td>
              <td>$50,000</td>
            </tr>
            <tr>
              <td>2</td>
              <td>Neeraja Asnani</td>
              <td>$47,000</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- SCRIPTS -->
  <script>
    // Function to switch between sections
    function showSection(section) {
      // Remove active class from sidebar items
      document.querySelectorAll('.sidebar ul li').forEach(item => {
        item.classList.remove('active');
      });
      // Mark the clicked sidebar item as active (simple approach, you may improve this based on your structure)
      document.querySelector(`.sidebar ul li[onclick*="${section}"]`).classList.add('active');

      document.querySelectorAll('.section').forEach(sec => sec.style.display = 'none');
      document.getElementById(section).style.display = 'block';
    }

    // Dummy search function
    function searchStock() {
      const query = document.getElementById('searchInput').value;
      alert('Search for: ' + query + ' (Functionality under development)');
    }

    // Submit order through Google Apps Script endpoint
    function submitOrder(event) {
      event.preventDefault();
      const orderType = document.getElementById('orderType').value;
      const stockSymbol = document.getElementById('stockSymbol').value;
      const quantity = document.getElementById('quantity').value;
      const price = document.getElementById('price').value;

      fetch('https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          action: 'submitOrder',
          orderType,
          stockSymbol,
          quantity,
          price
        })
      })
      .then(response => response.text())
      .then(data => {
        alert('Order submitted: ' + data);
      })
      .catch(error => {
        console.error('Error:', error);
      });
    }

    // Load news (dummy fetch; replace with your API endpoint)
    function loadNews() {
      fetch('https://script.google.com/macros/s/YOUR_SCRIPT_ID/exec?action=getNews')
      .then(response => response.json())
      .then(news => {
        const newsFeed = document.getElementById('newsFeed');
        newsFeed.innerHTML = news.map(item => `<p>${item}</p>`).join('');
      })
      .catch(error => {
        console.error('Error fetching news:', error);
      });
    }

    // Load a sample performance chart (requires Chart.js)
    function loadChart() {
      const ctx = document.getElementById('performanceChart').getContext('2d');
      new Chart(ctx, {
        type: 'line',
        data: {
          labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
          datasets: [{
            label: 'Portfolio Value',
            data: [30000, 32000, 31000, 33000, 34000, 35000],
            backgroundColor: 'rgba(41, 128, 185, 0.2)',
            borderColor: 'rgba(41, 128, 185, 1)',
            borderWidth: 2
          }]
        },
        options: { scales: { y: { beginAtZero: false } } }
      });
    }

    // Initial load
    window.onload = function() {
      loadNews();
      loadChart();
    };
  </script>
  <!-- Chart.js CDN for performance chart -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
</body>
</html>
