---
layout: tailwind 
title: Statistics
search_exclude: true
permalink: /stats/
---

  <title>Wildfire Statistics Dashboard</title>
  <style>
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #000;
      color: #eee;
    }
    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
    }
    .grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
      gap: 20px;
      margin-top: 20px;
    }
    .card {
      background: #111;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
      padding: 20px;
      transition: transform 0.3s, box-shadow 0.3s;
    }
    .card-header {
      display: flex;
      align-items: center;
      margin-bottom: 15px;
      color: #ff5500;
    }
    .card-header h2 {
      margin: 0 0 0 10px;
      font-size: 20px;
      color: #eee;
    }
    .stats-container {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 15px;
    }
    .stat-box {
      background: #fff4ee;
      border-radius: 8px;
      padding: 15px;
      text-align: center;
      border-left: 4px solid #ff5500;
    }
    .stat-number {
      font-size: 32px;
      font-weight: 800;
      color: #ff5500;
      margin: 0;
    }
    .stat-label {
      font-size: 14px;
      color: #666;
      margin: 5px 0 0;
    }
    .toggle-btn {
      margin: 2rem 0 1rem 0;
      padding: 0.5rem 1.5rem;
      background: #ff5500;
      color: #fff;
      border: none;
      border-radius: 6px;
      font-size: 1rem;
      cursor: pointer;
      transition: background 0.2s;
    }
    .toggle-btn:hover {
      background: #ff3300;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 1rem;
      background: #fff;
      color: #222;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }
    th, td {
      padding: 0.75rem;
      border: 1px solid #ccc;
      text-align: right;
    }
    th {
      background-color: #f0f0f0;
    }
    .footer {
      text-align: center;
      margin-top: 30px;
      padding: 20px;
      font-size: 12px;
      color: #777;
      border-top: 1px solid #eee;
    }
    @media (max-width: 768px) {
      .grid {
        grid-template-columns: 1fr;
      }
    }
  </style>
  <div class="container">
    <h1 style="color:#ff5500; margin-bottom:0.5em;">Wildfire Statistics Dashboard</h1>
    
    <!-- NEW: Major Limitations and Resource Management Failures Cards -->
    <div class="grid">
      <div class="card">
        <div class="card-header">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <path d="M12 2L2 7v7c0 5 4 9 10 9s10-4 10-9V7L12 2z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            <circle cx="12" cy="13" r="4" stroke="currentColor" stroke-width="2"/>
          </svg>
          <h2>Major Limitations</h2>
        </div>
        <ul style="padding-left: 1.2em; color: #eee;">
          <li><b>Resource shortages:</b> Many regions face a 40–50% drop in firefighter applications and staffing.</li>
          <li><b>Climate change:</b> Longer, hotter fire seasons and earlier spring peaks.</li>
          <li><b>Urban expansion:</b> More homes and infrastructure in fire-prone areas.</li>
          <li><b>Old detection systems:</b> Over 60% of detection infrastructure is outdated.</li>
        </ul>
      </div>
      <div class="card">
        <div class="card-header">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <path d="M18 6L6 18" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M6 6L18 18" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <h2>Resource Management Failures</h2>
        </div>
        <ul style="padding-left: 1.2em; color: #eee;">
          <li><b>Suppression focus:</b> 75% of funding goes to suppression, only 25% to prevention.</li>
          <li><b>Prescribed burns:</b> Less than 25% of recommended burns are completed annually.</li>
          <li><b>Data silos:</b> 58% of agencies report difficulty sharing critical information.</li>
          <li><b>Policy inconsistency:</b> Fragmented response due to varying local and state policies.</li>
        </ul>
      </div>
    </div>
    <!-- END NEW CARDS -->

    <div class="grid">
      <div class="card">
        <div class="card-header">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none"><path d="M12 2L4 8.5V20H20V8.5L12 2Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/><path d="M12 15C13.6569 15 15 13.6569 15 12C15 10.3431 13.6569 9 12 9C10.3431 9 9 10.3431 9 12C9 13.6569 10.3431 15 12 15Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
          <h2>About Wildfires</h2>
        </div>
        <p>
          This dashboard presents annual wildland fire and acreage statistics for the United States (1983–2024), highlighting trends, averages, and record years. Data is sourced from the National Interagency Fire Center and U.S. Forest Service.
        </p>
      </div>
      <div class="card">
        <div class="card-header">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none"><path d="M16 6L12 2L8 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/><path d="M12 2V15" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/><path d="M22 14L18 18L14 14" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/><path d="M18 18V12" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/><path d="M10 14L6 18L2 14" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/><path d="M6 18V12" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
          <h2>Key Statistics</h2>
        </div>
        <div class="stats-container">
          <div class="stat-box">
            <p class="stat-number" id="stat-avg-fires">--</p>
            <p class="stat-label">Avg. Annual Fires</p>
          </div>
          <div class="stat-box">
            <p class="stat-number" id="stat-avg-acres">--</p>
            <p class="stat-label">Avg. Acres Burned</p>
          </div>
          <div class="stat-box">
            <p class="stat-number" id="stat-max-fires">--</p>
            <p class="stat-label">Most Fires (Year)</p>
          </div>
          <div class="stat-box">
            <p class="stat-number" id="stat-max-acres">--</p>
            <p class="stat-label">Most Acres (Year)</p>
          </div>
        </div>
      </div>
    </div>
    <button id="toggle-table-btn" class="toggle-btn">Show Data Table</button>
    <div id="table-container" style="display:none;">
      <table>
        <thead>
          <tr>
            <th>Year</th>
            <th>Fires</th>
            <th>Acres Burned</th>
          </tr>
        </thead>
        <tbody id="data-table"></tbody>
      </table>
    </div>

    <!-- Weather Card Grid -->
    <div class="grid" id="weather-grid">
      <div class="card" id="weather-card">
        <div class="card-header">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
            <circle cx="12" cy="12" r="10" stroke="#ff5500" stroke-width="2"/>
            <path d="M12 6v6l4 2" stroke="#ff5500" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
          <h2>San Diego Weather (Live)</h2>
        </div>
        <div style="color:#eee;">
          <div style="font-size:2em; font-weight:700;">
            <span id="current-temperature">--°</span>
            <span style="font-size:0.5em; color:#ff5500;" id="weather-conditions"></span>
          </div>
          <div style="margin: 8px 0;">
            <b>Feels Like:</b> <span id="current-feels-like">--°F</span>
          </div>
          <div>
            <b>Humidity:</b> <span id="current-humidity">--%</span>
            &nbsp;|&nbsp;
            <b>Wind:</b> <span id="current-wind-speed">-- kph</span>
          </div>
          <div style="margin-top:8px; color:#aaa;">
            <b>Location:</b> <span id="weather-location">San Diego</span>
          </div>
        </div>
      </div>
    </div>

    <div class="footer">
      Data sources: National Interagency Fire Center, U.S. Forest Service, Congressional Research Service (2023-2024)
    </div>
  </div>
  <script>
    // --- DATA AND LOGIC FROM stats.md ---
    const rawData = `
2024	64897	8924884
2023	56580	2693910
2022	68988	7577183
2021	58985	7125643
2020	58950	10122336
2019	50477	4664364
2018	58083	8767492
2017	71499	10026086
2016	67743	5509995
2015	68151	10125149
2014	63312	3595613
2013	47579	4319546
2012	67774	9326238
2011	74126	8711367
2010	71971	3422724
2009	78792	5921786
2008	78979	5292468
2007	85705	9328045
2006	96385	9873745
2005	66753	8689389
2004	65461	8097880
2003	63629	3960842
2002	73457	7184712
2001	84079	3570911
2000	92250	7393493
1999	92487	5626093
1998	81043	1329704
1997	66196	2856959
1996	96363	6065998
1995	82234	1840546
1994	79107	4073579
1993	58810	1797574
1992	87394	2069929
1991	75754	2953578
1990	66481	4621621
1989	48949	1827310
1988	72750	5009290
1987	71300	2447296
1986	85907	2719162
1985	82591	2896147
1984	20493	1148409
1983	18229	1323666
    `;

    const data = rawData.trim().split('\n').map(row => {
      const [year, fires, acres] = row.split('\t');
      return {
        year: parseInt(year),
        fires: parseInt(fires.replace(/\*/g, '')),
        acres: parseInt(acres.replace(/\*/g, ''))
      };
    });

    // Render table
    function renderTable() {
      const tableBody = document.getElementById("data-table");
      tableBody.innerHTML = "";
      data.forEach(row => {
        const tr = document.createElement("tr");
        tr.innerHTML = `<td>${row.year}</td><td>${row.fires.toLocaleString()}</td><td>${row.acres.toLocaleString()}</td>`;
        tableBody.appendChild(tr);
      });
    }

    // Calculate stats
    const totalYears = data.length;
    const totalFires = data.reduce((sum, row) => sum + row.fires, 0);
    const totalAcres = data.reduce((sum, row) => sum + row.acres, 0);
    const avgFires = Math.round(totalFires / totalYears);
    const avgAcres = Math.round(totalAcres / totalYears);

    const maxFireYear = data.reduce((max, row) => row.fires > max.fires ? row : max);
    const maxAcresYear = data.reduce((max, row) => row.acres > max.acres ? row : max);

    // Fill stats in cards
    document.getElementById("stat-avg-fires").textContent = avgFires.toLocaleString();
    document.getElementById("stat-avg-acres").textContent = avgAcres.toLocaleString();
    document.getElementById("stat-max-fires").textContent = `${maxFireYear.fires.toLocaleString()} (${maxFireYear.year})`;
    document.getElementById("stat-max-acres").textContent = `${maxAcresYear.acres.toLocaleString()} (${maxAcresYear.year})`;

    // Toggle table visibility
    const toggleBtn = document.getElementById("toggle-table-btn");
    const tableContainer = document.getElementById("table-container");
    let tableVisible = false;
    toggleBtn.addEventListener("click", () => {
      tableVisible = !tableVisible;
      if (tableVisible) {
        renderTable();
        tableContainer.style.display = "block";
        toggleBtn.textContent = "Hide Data Table";
      } else {
        tableContainer.style.display = "none";
        toggleBtn.textContent = "Show Data Table";
      }
    });

    // ============ WEATHER DATA FUNCTIONS ============
    
    // Function to get weather data for a specific location
    async function getWeatherForLocation(lat, lon) {
      try {
        const response = await fetch(`${pythonURI}/api/get_weather?lat=${lat}&lon=${lon}`);
        
        if (!response.ok) {
          throw new Error('Weather API response was not ok');
        }

        const data = await response.json();
        return data.weather;
      } catch (error) {
        console.error(`Error fetching weather for location [${lat}, ${lon}]:`, error);
        // Return default values if weather data fetch fails
        return {
          temperature: 25,
          wind_speed: 10,
          humidity: 45,
          conditions: "Unknown"
        };
      }
    }

    // Function to get user's local weather for dashboard display
    async function getUserLocationWeather() {
      // Check if we have stored coordinates
      const storedLat = localStorage.getItem('weather_lat');
      const storedLon = localStorage.getItem('weather_lon');
      
      // If we have stored coordinates, use them
      if (storedLat && storedLon) {
        return getWeatherForLocation(parseFloat(storedLat), parseFloat(storedLon))
          .then(weatherData => {
            updateWeatherDisplay({
              weather: weatherData,
              location: { name: localStorage.getItem('location_name') || "Current Location" }
            });
            return weatherData;
          });
      }
      
      // Try to get location from browser geolocation API
      if (navigator.geolocation) {
        try {
          const position = await new Promise((resolve, reject) => {
            navigator.geolocation.getCurrentPosition(resolve, reject, {
              enableHighAccuracy: true,
              timeout: 5000,
              maximumAge: 0
            });
          });
          
          const lat = position.coords.latitude;
          const lon = position.coords.longitude;
          
          // Store coordinates for future use
          localStorage.setItem('weather_lat', lat);
          localStorage.setItem('weather_lon', lon);
          
          return getWeatherForLocation(lat, lon)
            .then(weatherData => {
              updateWeatherDisplay({
                weather: weatherData,
                location: { name: "Current Location" }
              });
              return weatherData;
            });
        } catch (error) {
          console.warn("Could not get user location automatically:", error);
          // If geolocation fails, use default San Diego location
          return getDefaultLocationWeather();
        }
      } else {
        console.warn("Geolocation is not supported by this browser");
        return getDefaultLocationWeather();
      }
    }

    // Function to get weather for default location (San Diego)
    async function getDefaultLocationWeather() {
      const defaultLat = 32.7157; // San Diego default
      const defaultLon = -117.1611;
      
      // Store default coordinates
      localStorage.setItem('weather_lat', defaultLat);
      localStorage.setItem('weather_lon', defaultLon);
      localStorage.setItem('location_name', "San Diego");
      
      return getWeatherForLocation(defaultLat, defaultLon)
        .then(weatherData => {
          updateWeatherDisplay({
            weather: weatherData,
            location: { name: "San Diego" }
          });
          return weatherData;
        });
    }

    // Function to update all weather-related displays
    function updateWeatherDisplay(weatherData) {
      // Update temperature display
      updateTemperatureDisplay(weatherData);
      
      // Update wind analysis display
      updateWindDisplay(weatherData);
    }

    // Function to update the temperature display
    function updateTemperatureDisplay(weatherData) {
      // Update the temperature gauge
      const temperature = weatherData.weather.temperature;
      const tempElement = document.getElementById('current-temperature');
      if (tempElement) {
        tempElement.textContent = `${Math.round(temperature)}°`;
      }
      
      // Update conditions
      const conditionsEl = document.getElementById('weather-conditions');
      if (conditionsEl) {
        conditionsEl.textContent = weatherData.weather.conditions;
      }
      
      // Update location
      const locationEl = document.getElementById('weather-location');
      if (locationEl) {
        locationEl.textContent = weatherData.location.name;
      }
    }

    // Function to update the wind analysis display
    function updateWindDisplay(weatherData) {
      // Update wind speed
      const windSpeedEl = document.getElementById('current-wind-speed');
      if (windSpeedEl) {
        windSpeedEl.textContent = `${Math.round(weatherData.weather.wind_speed)} kph`;
      }
      
      // Update humidity
      const humidityEl = document.getElementById('current-humidity');
      if (humidityEl) {
        humidityEl.textContent = `${weatherData.weather.humidity}%`;
      }
      
      // Update feels like
      const feelsLikeEl = document.getElementById('current-feels-like');
      if (feelsLikeEl) {
        feelsLikeEl.textContent = `${Math.round(weatherData.weather.feels_like)}°F`;
      }
    }
  </script>

