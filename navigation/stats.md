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
  <title>NASA FIRMS Data Visualizer</title>
  <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
  <script src="https://d3js.org/d3.v7.min.js"></script>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 40px;
      background-color: #f5f5f5;
      color: #000;
    }
    h1 {
      text-align: center;
    }
    .dropdown-container {
      text-align: center;
      margin: 20px;
    }
    select {
      font-size: 1rem;
      padding: 10px;
      width: 300px;
    }
    #chart {
      width: 90%;
      max-width: 1000px;
      margin: 0 auto;
    }
  </style>
</head>
<body>

  <h1>NASA Fire Data Visualizer</h1>

  <div class="dropdown-container">
    <label for="data-select">Choose a Visualization:</label>
    <select id="data-select">
      <option value="">-- Select an option --</option>
      <option value="map">Latitude & Longitude (Map)</option>
      <option value="bright_ti4">Brightness TI4</option>
      <option value="bright_ti5">Brightness TI5</option>
      <option value="scan_track">Scan / Track</option>
      <option value="confidence">Confidence</option>
      <option value="frp">Fire Radiative Power (FRP)</option>
      <option value="satellite">Satellite</option>
      <option value="instrument">Instrument</option>
      <option value="daynight">Day / Night</option>
      <option value="version">Data Version</option>
      <option value="acq_date">Acquisition Dates</option>
    </select>
  </div>

  <div id="chart"></div>

  <script>
    const filePath = "2025-03-01.txt";
    d3.csv(filePath).then(data => {
      const dropdown = document.getElementById("data-select");
      dropdown.addEventListener("change", () => {
        const selected = dropdown.value;
        document.getElementById("chart").innerHTML = "";

        if (selected === "map") {
          const lats = data.map(d => +d.latitude);
          const lons = data.map(d => +d.longitude);

          Plotly.newPlot("chart", [{
            type: "scattergeo",
            mode: "markers",
            lat: lats,
            lon: lons,
            marker: { color: 'red', size: 4 },
            name: "Fires"
          }], {
            geo: {
              projection: { type: "natural earth" },
              showland: true
            },
            title: "Fire Locations"
          });
        }

        else if (selected === "bright_ti4" || selected === "bright_ti5" || selected === "frp") {
          const values = data.map(d => +d[selected]);
          Plotly.newPlot("chart", [{
            type: "histogram",
            x: values,
            marker: { color: "#0077b6" }
          }], {
            title: `${selected.replace("_", " ").toUpperCase()} Distribution`,
            xaxis: { title: selected },
            yaxis: { title: "Count" }
          });
        }

        else if (selected === "scan_track") {
          Plotly.newPlot("chart", [{
            x: data.map(d => +d.scan),
            y: data.map(d => +d.track),
            mode: "markers",
            type: "scatter",
            marker: { size: 6, color: "#ff8800" }
          }], {
            title: "Scan vs. Track Width",
            xaxis: { title: "Scan" },
            yaxis: { title: "Track" }
          });
        }

        else if (selected === "confidence" || selected === "satellite" || selected === "instrument" || selected === "daynight" || selected === "version") {
          const counts = {};
          data.forEach(row => {
            const key = row[selected];
            counts[key] = (counts[key] || 0) + 1;
          });
          const labels = Object.keys(counts);
          const values = Object.values(counts);

          Plotly.newPlot("chart", [{
            type: "bar",
            x: labels,
            y: values,
            marker: { color: "#0096c7" }
          }], {
            title: `Distribution of ${selected}`,
            xaxis: { title: selected },
            yaxis: { title: "Count" }
          });
        }

        else if (selected === "acq_date") {
          const dateCounts = {};
          data.forEach(d => {
            dateCounts[d.acq_date] = (dateCounts[d.acq_date] || 0) + 1;
          });

          const dates = Object.keys(dateCounts);
          const counts = Object.values(dateCounts);

          Plotly.newPlot("chart", [{
            x: dates,
            y: counts,
            type: "bar",
            marker: { color: "#6a4c93" }
          }], {
            title: "Fires by Acquisition Date",
            xaxis: { title: "Date" },
            yaxis: { title: "Number of Fires" }
          });
        }
      });
    }).catch(err => {
      document.getElementById("chart").innerHTML = "<p style='color:red;'>Failed to load data file.</p>";
      console.error("CSV load error:", err);
    });
  </script>
</body>
</html>
