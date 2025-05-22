---
layout: tailwind 
title: Statistics 
search_exclude: true
permalink: /stats/
---
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>🔥 Wildfire Statistics Dashboard</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      margin: 0;
      padding: 2rem;
      background-color: #fdfdfd;
      color: #333;
    }

    h1, h2 {
      color: #b22222;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin: 1rem 0;
      background-color: #fff;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }

    th, td {
      padding: 0.75rem;
      border: 1px solid #ccc;
      text-align: center;
    }

    th {
      background-color: #f4f4f4;
      font-weight: bold;
    }

    .section {
      margin-bottom: 3rem;
    }

    .loader {
      text-align: center;
      color: #888;
    }
  </style>
</head>
<body>

  <h1>🔥 Wildfire Statistics Dashboard</h1>

  <div class="section">
    <h2>Human-Caused Fires (NIFC)</h2>
    <div id="human-loader" class="loader">Loading...</div>
    <table id="human-table" style="display:none">
      <thead>
        <tr>
          <th>Year</th>
          <th>Human-Caused Fires</th>
          <th>Acres Burned</th>
        </tr>
      </thead>
      <tbody id="human-data"></tbody>
    </table>
  </div>

  <div class="section">
    <h2>Total Wildfires (NIFC)</h2>
    <div id="total-loader" class="loader">Loading...</div>
    <table id="total-table" style="display:none">
      <thead>
        <tr>
          <th>Year</th>
          <th>Total Fires</th>
          <th>Total Acres</th>
        </tr>
      </thead>
      <tbody id="total-data"></tbody>
    </table>
  </div>

  <script>
    async function loadData(url, tableId, loaderId, tbodyId, keys) {
      try {
        const response = await fetch(url);
        const data = await response.json();

        if (data.data && Array.isArray(data.data)) {
          const tbody = document.getElementById(tbodyId);
          data.data.forEach(item => {
            const row = document.createElement('tr');
            keys.forEach(key => {
              const td = document.createElement('td');
              td.textContent = item[key];
              row.appendChild(td);
            });
            tbody.appendChild(row);
          });

          document.getElementById(loaderId).style.display = 'none';
          document.getElementById(tableId).style.display = 'table';
        }
      } catch (error) {
        document.getElementById(loaderId).textContent = 'Error loading data';
        console.error('Error fetching:', url, error);
      }
    }

    // Load data from both endpoints
    loadData('/api/fire/human_caused', 'human-table', 'human-loader', 'human-data', ['year', 'human_caused_fires', 'acres_burned']);
    loadData('/api/fire/total_wildfires', 'total-table', 'total-loader', 'total-data', ['year', 'total_fires', 'total_acres']);
  </script>

</body>
</html>
