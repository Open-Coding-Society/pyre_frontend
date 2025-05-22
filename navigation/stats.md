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
  <title>NASA FIRMS VIIRS Data Viewer</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px;
      background-color: #f4f4f9;
      color: #333;
    }

    h1 {
      text-align: center;
    }

    .dropdown-container {
      text-align: center;
      margin-bottom: 20px;
    }

    select {
      font-size: 1rem;
      padding: 10px;
      width: 250px;
    }

    .card {
      display: none;
      padding: 20px;
      margin: 20px auto;
      max-width: 600px;
      background-color: #fff;
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }

    .card.active {
      display: block;
    }

    .stat {
      font-size: 1.5rem;
      margin-bottom: 10px;
    }

    .explanation {
      font-size: 0.95rem;
      color: #555;
    }
  </style>
</head>
<body>

  <h1>NASA FIRMS - VIIRS Data Viewer</h1>

  <div class="dropdown-container">
    <label for="data-select">Choose a Data Point:</label>
    <select id="data-select">
      <option value="">-- Select an option --</option>
      <option value="satellite">Satellite</option>
      <option value="instrument">Instrument</option>
      <option value="scantrack">Scan / Track</option>
      <option value="version">Data Version</option>
      <option value="bright_ti5">Brightness TI5</option>
    </select>
  </div>

  <div id="satellite" class="card">
    <div class="stat" id="satellite-stat"></div>
    <div class="explanation">Shows the satellite platform (e.g., VIIRS). Useful for filtering or verifying source consistency.</div>
  </div>

  <div id="instrument" class="card">
    <div class="stat" id="instrument-stat"></div>
    <div class="explanation">This field specifies the onboard instrument capturing data. Important for sensor-specific analysis.</div>
  </div>

  <div id="scantrack" class="card">
    <div class="stat" id="scantrack-stat"></div>
    <div class="explanation">Scan and track widths define the pixel resolution of data capture, indicating image detail.</div>
  </div>

  <div id="version" class="card">
    <div class="stat" id="version-stat"></div>
    <div class="explanation">Data version helps with backend troubleshooting or comparing revisions of datasets.</div>
  </div>

  <div id="bright_ti5" class="card">
    <div class="stat" id="bright_ti5-stat"></div>
    <div class="explanation">Brightness TI5 is measured in longwave infrared and is used in fire detection or thermal anomaly studies.</div>
  </div>

  <script>
    const apiURL = "https://firms.modaps.eosdis.nasa.gov/usfs/api/area/csv/a5c78a9b1ae831d370494dacf4428024/VIIRS_SNPP_NRT/world/1";

    document.getElementById("data-select").addEventListener("change", function () {
      const selected = this.value;
      document.querySelectorAll(".card").forEach(card => card.classList.remove("active"));
      if (selected) document.getElementById(selected).classList.add("active");
    });

    fetch(apiURL)
      .then(response => response.text())
      .then(csv => {
        const rows = csv.trim().split("\n").slice(1).map(row => row.split(","));
        
        // Get specific columns by their position
        const satelliteData = rows.map(r => r[7]);
        const instrumentData = rows.map(r => r[8]);
        const scanData = rows.map(r => parseFloat(r[4]));
        const trackData = rows.map(r => parseFloat(r[5]));
        const versionData = rows.map(r => r[10]);
        const brightTI5Data = rows.map(r => parseFloat(r[11]));

        // Populate card stats
        document.getElementById("satellite-stat").innerText = 
          "Satellites Detected: " + [...new Set(satelliteData)].join(", ");

        document.getElementById("instrument-stat").innerText = 
          "Instruments Detected: " + [...new Set(instrumentData)].join(", ");

        const avg = arr => (arr.reduce((a,b) => a+b, 0) / arr.length).toFixed(2);

        document.getElementById("scantrack-stat").innerText = 
          `Average Scan: ${avg(scanData)} | Average Track: ${avg(trackData)}`;

        document.getElementById("version-stat").innerText = 
          "Data Versions: " + [...new Set(versionData)].join(", ");

        document.getElementById("bright_ti5-stat").innerText = 
          `Avg Brightness TI5: ${avg(brightTI5Data)} (K)`;
      })
      .catch(error => {
        console.error("Data fetch failed:", error);
        document.body.innerHTML += "<p style='color:red;'>Failed to load data from API.</p>";
      });
  </script>
</body>
</html>
