---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /dashboard/
title: Dashboard
---

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.3/leaflet.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.3/leaflet.js"></script>

<style>
    #map {
      width: 100%;
      height: 100%;
      background-color: #1f2937;
      border-radius: 0.5rem;
    }
    .leaflet-container {
      background-color: #1f2937;
    }
    .map-marker {
      display: flex;
      align-items: center;
      justify-content: center;
      width: 2rem;
      height: 2rem;
      background: linear-gradient(to right, #f97316, #dc2626);
      color: white;
      border-radius: 50%;
      font-weight: bold;
      border: 2px solid white;
      box-shadow: 0 2px 4px rgba(0,0,0,0.3);
    }
    .fire-popup {
      background-color: rgba(17, 24, 39, 0.95);
      color: white;
      border: 1px solid #374151;
      border-radius: 0.375rem;
      padding: 0.5rem;
    }
    .fire-popup .leaflet-popup-content-wrapper {
      background-color: transparent;
      color: white;
    }
    .fire-popup .leaflet-popup-tip {
      background-color: #374151;
    }
    .risk-high {
      color: #ef4444;
    }
    .risk-medium {
      color: #f97316;
    }
    .risk-low {
      color: #eab308;
    }
    
    /* Added styles for incident table */
    .incidents-table {
      width: 100%;
      border-collapse: separate;
      border-spacing: 0;
    }
    .incidents-table th,
    .incidents-table td {
      padding: 0.75rem 1rem;
      text-align: left;
    }
    .incidents-table th {
      background-color: rgba(17, 24, 39, 0.7);
      font-weight: 500;
      text-transform: uppercase;
      font-size: 0.75rem;
      letter-spacing: 0.05em;
    }
    .incidents-table tr {
      border-bottom: 1px solid rgba(55, 65, 81, 0.5);
    }
    .incidents-table tbody tr:hover {
      background-color: rgba(17, 24, 39, 0.5);
    }
    .pulse {
      animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
    }
    @keyframes pulse {
      0%, 100% {
        opacity: 1;
      }
      50% {
        opacity: 0.5;
      }
    }
</style>

<div class="min-h-screen bg-gray-950 text-gray-200">
    <br>
    <br>
  <!-- Dashboard content -->
  <div class="flex h-screen overflow-hidden pt-16 -mt-16">
    <!-- Left sidebar -->
    <div class="w-72 bg-gray-900/50 border-r border-gray-800 p-4 overflow-y-auto">
      <h2 class="text-lg font-medium mb-4">Analytics Overview</h2>
      <div class="mb-6">
        <div class="text-sm text-gray-400 mb-1">Total Incidents (Past day)</div>
        <div class="text-4xl font-bold" id="total-incidents">--</div>
        <div class="mt-4">
          <h3 class="text-sm text-gray-400 mb-2">Incident Categories</h3>
          <div id="category-stats" class="space-y-3">
            <!-- Category stats will be inserted here -->
            <div class="animate-pulse">
              <div class="flex justify-between mb-1">
                <span class="bg-gray-700 h-4 w-24 rounded"></span>
                <span class="bg-gray-700 h-4 w-8 rounded"></span>
              </div>
              <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
                <div class="h-full bg-gray-700 rounded-full w-3/4"></div>
              </div>
            </div>
            <div class="animate-pulse">
              <div class="flex justify-between mb-1">
                <span class="bg-gray-700 h-4 w-32 rounded"></span>
                <span class="bg-gray-700 h-4 w-8 rounded"></span>
              </div>
              <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
                <div class="h-full bg-gray-700 rounded-full w-1/2"></div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- Temperature widget -->
      <!-- <div class="mb-6">
        <h3 class="text-sm text-gray-400 mb-3">Temperature Trend</h3>
        <div class="bg-gray-900/70 rounded-lg p-3 h-40">
          <div class="h-full w-full rounded flex items-end space-x-1">
            <div class="h-1/4 w-8 bg-orange-600 rounded-t"></div>
            <div class="h-2/5 w-8 bg-orange-600 rounded-t"></div>
            <div class="h-3/5 w-8 bg-red-500 rounded-t"></div>
            <div class="h-4/5 w-8 bg-red-500 rounded-t"></div>
            <div class="h-4/5 w-8 bg-red-500 rounded-t"></div>
            <div class="h-3/5 w-8 bg-red-500 rounded-t"></div>
            <div class="h-2/5 w-8 bg-orange-600 rounded-t"></div>
          </div>
          <div class="flex justify-between text-xs text-gray-500 mt-2">
            <div>Mon</div>
            <div>Wed</div>
            <div>Fri</div>
            <div>Sun</div>
          </div>
        </div>
      </div>
      <div class="mb-6">
        <h3 class="text-sm text-gray-400 mb-3">Wind Analysis</h3>
        <div class="bg-gray-900/70 rounded-lg p-3">
          <div class="flex items-center justify-between mb-2">
            <div class="text-sm">Current Wind Speed</div>
            <div id="current-wind-speed" class="font-medium">-- kph</div>
          </div>
          <div class="flex items-center justify-between mb-2">
            <div class="text-sm">Humidity</div>
            <div id="current-humidity" class="font-medium">--%</div>
          </div>
          <div class="flex items-center justify-between">
            <div class="text-sm">Feels Like</div>
            <div id="current-feels-like" class="font-medium">--°F</div>
          </div>
        </div>
      </div> -->
      <div class="mb-6">
        <h3 class="text-sm text-gray-400 mb-3">Current Temperature</h3>
        <div class="flex justify-between text-xs text-gray-400 mb-1">
          <div id="weather-location">--</div>
          <div id="weather-conditions">--</div>
        </div>
        <div class="bg-gray-900/70 rounded-lg p-3 h-40">
          <!-- Temperature gauge -->
          <div class="h-full w-full flex items-center justify-center">
            <div class="w-32 h-32 relative">
              <div class="absolute inset-0 flex items-center justify-center">
                <div id="current-temperature" class="text-3xl font-bold">--°</div>
              </div>
              <svg class="absolute inset-0" viewBox="0 0 100 100">
                <path 
                  d="M 50,50 m 0,47 a 47,47 0 1 1 0,-94 a 47,47 0 1 1 0,94" 
                  fill="none" 
                  stroke="#374151" 
                  stroke-width="6"
                />
                <path 
                  id="gauge-path"
                  d="M 50,50 m 0,47 a 47,47 0 1 1 0,-94 a 47,47 0 1 1 0,94" 
                  fill="none" 
                  stroke-linecap="round"
                  class="stroke-blue-500"
                  stroke-width="6"
                  stroke-dasharray="295.31" 
                  stroke-dashoffset="220"
                />
              </svg>
            </div>
          </div>
        </div>
      </div>
      <!-- Wind widget - Replace the existing widget with this -->
      <div class="mb-6">
        <h3 class="text-sm text-gray-400 mb-3">Wind Analysis</h3>
        <div class="bg-gray-900/70 rounded-lg p-3">
          <div class="flex items-center justify-between mb-2">
            <div class="text-sm">Current Wind Speed</div>
            <div id="current-wind-speed" class="font-medium">-- kph</div>
          </div>
          <div class="flex items-center justify-between mb-2">
            <div class="text-sm">Humidity</div>
            <div id="current-humidity" class="font-medium">--%</div>
          </div>
          <div class="flex items-center justify-between">
            <div class="text-sm">Feels Like</div>
            <div id="current-feels-like" class="font-medium">--°F</div>
          </div>
        </div>
      </div>
    </div>
    <!-- Main content area with map -->
    <div class="flex-1 overflow-hidden flex flex-col">
      <div class="flex-1 p-4 overflow-hidden">
        <!-- Map container -->
        <div class="bg-gray-900/50 rounded-lg overflow-hidden h-full relative border border-gray-800">
          <!-- Map placeholder -->
          <div class="w-full h-full bg-gray-800/50"></div>
          <!-- Map controls -->
          <div class="absolute top-4 right-4 flex space-x-2">
            <button class="bg-gray-200 bg-opacity-20 backdrop-blur-sm rounded-md px-3 py-1 text-sm text-gray-200 flex items-center hover:bg-opacity-30">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M3 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z" clip-rule="evenodd"/>
              </svg>
              Layers
            </button>
            <button id="toggle-climate-data" class="bg-gray-200 bg-opacity-20 backdrop-blur-sm rounded-md px-3 py-1 text-sm text-gray-200 flex items-center hover:bg-opacity-30">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M5 2a1 1 0 011 1v1h1a1 1 0 010 2H6v1a1 1 0 01-2 0V6H3a1 1 0 010-2h1V3a1 1 0 011-1zm0 10a1 1 0 011 1v1h1a1 1 0 110 2H6v1a1 1 0 11-2 0v-1H3a1 1 0 110-2h1v-1a1 1 0 011-1zM12 2a1 1 0 01.967.744L14.146 7.2 17.5 9.134a1 1 0 010 1.732l-3.354 1.935-1.18 4.455a1 1 0 01-1.933 0L9.854 12.8 6.5 10.866a1 1 0 010-1.732l3.354-1.935 1.18-4.455A1 1 0 0112 2z" clip-rule="evenodd" />
              </svg>
              Toggle Climate Data
            </button>
          </div>
        </div>
      </div>
      <!-- Bottom toolbar -->
      <div class="bg-black border-t border-gray-800 py-3 px-6 flex justify-between items-center">
        <div class="flex space-x-6">
          <button class="flex items-center text-gray-400 hover:text-white">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
              <path d="M2 10a8 8 0 018-8v8h8a8 8 0 11-16 0z" />
              <path d="M12 2.252A8.014 8.014 0 0117.748 8H12V2.252z" />
            </svg>
            Weather Forecast
          </button>
          <button class="flex items-center text-gray-400 hover:text-white">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
              <path d="M2 5a2 2 0 012-2h7a2 2 0 012 2v4a2 2 0 01-2 2H9l-3 3v-3H4a2 2 0 01-2-2V5z" />
              <path d="M15 7v2a4 4 0 01-4 4H9.828l-1.766 1.767c.28.149.599.233.938.233h2l3 3v-3h2a2 2 0 002-2V9a2 2 0 00-2-2h-1z" />
            </svg>
            Alerts
          </button>
        </div>
        <div class="flex items-center space-x-4">
          <div class="text-sm text-gray-400">Last updated: <span id="last-updated">--</span></div>
          <button class="bg-gradient-to-r from-orange-600 to-red-600 hover:from-orange-500 hover:to-red-500 text-white px-3 py-1 rounded flex items-center text-sm" id="refresh-data">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M4 2a1 1 0 011 1v2.101a7.002 7.002 0 0111.601 2.566 1 1 0 11-1.885.666A5.002 5.002 0 005.999 7H9a1 1 0 010 2H4a1 1 0 01-1-1V3a1 1 0 011-1zm.008 9.057a1 1 0 011.276.61A5.002 5.002 0 0014.001 13H11a1 1 0 110-2h5a1 1 0 011 1v5a1 1 0 11-2 0v-2.101a7.002 7.002 0 01-11.601-2.566 1 1 0 01.61-1.276z" clip-rule="evenodd" />
            </svg>
            Refresh Data
          </button>
          <button class="bg-gradient-to-r from-orange-600 to-red-600 hover:from-orange-500 hover:to-red-500 text-white px-3 py-1 rounded flex items-center text-sm">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M3 17a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm3.293-7.707a1 1 0 011.414 0L9 10.586V3a1 1 0 112 0v7.586l1.293-1.293a1 1 0 111.414 1.414l-3 3a1 1 0 01-1.414 0l-3-3a1 1 0 010-1.414z" clip-rule="evenodd" />
            </svg>
            Export Report
          </button>
        </div>
      </div>
    </div>
    <!-- Right sidebar - Replaced with Incidents Table -->
    <div class="w-72 bg-gray-900/50 border-l border-gray-800 p-4 overflow-y-auto">
      <div class="mb-4 flex justify-between items-center">
        <h2 class="text-lg font-medium">Incident Reports</h2>
        <div class="text-xs text-gray-400 flex items-center">
          <span id="incident-count" class="mr-1">--</span> incidents
        </div>
      </div>
      <!-- Incidents Table -->
      <div class="overflow-y-auto max-h-full">
        <table class="incidents-table text-sm">
          <thead>
            <tr>
              <th class="sticky top-0 z-10">Category</th>
              <th class="sticky top-0 z-10">Time</th>
              <th class="sticky top-0 z-10">Location</th>
            </tr>
          </thead>
          <tbody id="incidents-table-body">
            <!-- Loading placeholder -->
            <tr class="animate-pulse">
              <td><div class="h-4 bg-gray-700 rounded w-20"></div></td>
              <td><div class="h-4 bg-gray-700 rounded w-16"></div></td>
              <td><div class="h-4 bg-gray-700 rounded w-12"></div></td>
            </tr>
            <tr class="animate-pulse">
              <td><div class="h-4 bg-gray-700 rounded w-20"></div></td>
              <td><div class="h-4 bg-gray-700 rounded w-16"></div></td>
              <td><div class="h-4 bg-gray-700 rounded w-12"></div></td>
            </tr>
            <tr class="animate-pulse">
              <td><div class="h-4 bg-gray-700 rounded w-20"></div></td>
              <td><div class="h-4 bg-gray-700 rounded w-16"></div></td>
              <td><div class="h-4 bg-gray-700 rounded w-12"></div></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</div>

<!-- Map initialization and data loading script -->
<script type="module">
  // Import dependencies
  import { pythonURI, fetchOptions } from '/QcommVNE_Frontend/assets/js/api/config.js';

  // Main Fire Dashboard Script with ML Integration
  document.addEventListener('DOMContentLoaded', function() {
    
    // ============ FIRE INCIDENT DATA FUNCTIONS ============
    
    // Fetch fire incident data from backend
    async function fetchFireData() {
      try {
        // Make the actual API request to the endpoint
        const response = await fetch(`${pythonURI}/fire-resource`);
        
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }

        const data = await response.json();
        
        // Calculate total incidents
        let total_incidents = data.category_counts['Life-Threatening Emergency Response'] + 
                            data.category_counts['HAZARD'] + 
                            data.category_counts['Non-Life-Threatening Response'] + 
                            data.category_counts['Urgent Response'];
        
        // Update the incident table
        updateIncidentTable(data.today_incidents);
        
        // Update counters and stats
        document.getElementById('total-incidents').textContent = total_incidents;
        document.getElementById('incident-count').textContent = total_incidents;
        document.getElementById('last-updated').textContent = data.last_update;
        
        // Update category stats
        updateCategoryStats(data.category_counts, total_incidents);
        
      } catch (error) {
        console.error('Error fetching fire data:', error);
        document.getElementById('incidents-table-body').innerHTML = `
          <tr><td colspan="3" class="text-center py-4">Error loading data</td></tr>
        `;
      }
    }

    // Update the incident table with the data
    function updateIncidentTable(incidents) {
      const tableBody = document.getElementById('incidents-table-body');
      tableBody.innerHTML = '';
      
      incidents.forEach(incident => {
        const row = document.createElement('tr');
        row.innerHTML = `
          <td>${incident.problem}</td>
          <td>${incident.date_response}</td>
          <td>${incident.address_city}, ${incident.address_zip}</td>
        `;
        tableBody.appendChild(row);
      });
    }

    // Update the category statistics
    function updateCategoryStats(categories, total) {
      const statsContainer = document.getElementById('category-stats');
      statsContainer.innerHTML = '';
      
      Object.entries(categories).forEach(([category, count]) => {
        const percentage = Math.round((count / total) * 100);
        
        const categoryEl = document.createElement('div');
        categoryEl.innerHTML = `
          <div class="flex justify-between mb-1">
            <span class="text-sm">${category}</span>
            <span class="text-sm">${count}</span>
          </div>
          <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
            <div class="h-full bg-orange-500 rounded-full" style="width: ${percentage}%"></div>
          </div>
        `;
        statsContainer.appendChild(categoryEl);
      });
    }

    // ============ MAP AND FIRE VISUALIZATION FUNCTIONS ============
    
    // Find the map container and initialize it if it doesn't exist
    const mapContainer = document.querySelector('.bg-gray-900\\/50.rounded-lg.overflow-hidden.h-full.relative.border.border-gray-800');
    if (mapContainer && !document.getElementById('map')) {
      mapContainer.innerHTML = '<div id="map" style="width:100%;height:100%;"></div>';
    }
    
    // Initialize map centered on San Diego
    const map = L.map('map', {
      center: [32.7157, -117.1611], // San Diego coordinates
      zoom: 11,
      zoomControl: false // We'll add custom controls
    });
    
    // Add dark-themed map tiles
    L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
      attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> &copy; <a href="https://carto.com/attributions">CARTO</a>',
      subdomains: 'abcd',
      maxZoom: 19
    }).addTo(map);
    
    // Add zoom control to top-right
    L.control.zoom({
      position: 'topright'
    }).addTo(map);
    
    // Function to fetch fire data from FIRMS API
    async function fetchFIRMSData() {
      try {
        const response = await fetch('https://firms.modaps.eosdis.nasa.gov/api/country/csv/60d99d6a7687be4ce5cd594d754872df/VIIRS_SNPP_NRT/USA/2');
        
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }
        
        const data = await response.text();
        return parseCSV(data);
      } catch (error) {
        console.error('Error fetching FIRMS fire data:', error);
        return [];
      }
    }

    // Function to parse CSV data into usable format
    function parseCSV(csvText) {
      const lines = csvText.trim().split('\n');
      const headers = lines[0].split(',');
      
      const fireData = [];
      let id = 1;
      
      for (let i = 1; i < lines.length; i++) {
        const values = lines[i].split(',');
        if (values.length !== headers.length) continue;
        
        const dataObj = {};
        for (let j = 0; j < headers.length; j++) {
          dataObj[headers[j]] = values[j];
        }
        
        // Generate a unique name based on location and date
        const locationName = `Fire-${dataObj.latitude.substring(0, 5)}-${dataObj.longitude.substring(0, 6)}`;
        
        fireData.push({
          id: id++,
          position: [parseFloat(dataObj.latitude), parseFloat(dataObj.longitude)],
          name: locationName,
          confidence: dataObj.confidence,
          intensity: dataObj.frp, // Using FRP (Fire Radiative Power) for intensity
          acq_date: dataObj.acq_date,
          acq_time: dataObj.acq_time,
          daynight: dataObj.daynight,
          latitude: parseFloat(dataObj.latitude),
          longitude: parseFloat(dataObj.longitude)
        });
      }
      
      return fireData;
    }

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

    // ============ ML PREDICTION FUNCTIONS ============
    
    // Get the day of week from a date string
    function getDayOfWeek(dateStr) {
      const date = new Date(dateStr);
      const days = ['sun', 'mon', 'tue', 'wed', 'thu', 'fri', 'sat'];
      return days[date.getDay()];
    }
    
    // Get the month abbreviation from a date string
    function getMonthAbbr(dateStr) {
      const date = new Date(dateStr);
      const months = ['jan', 'feb', 'mar', 'apr', 'may', 'jun', 'jul', 'aug', 'sep', 'oct', 'nov', 'dec'];
      return months[date.getMonth()];
    }

    // Predict fire size using ML model
    async function predictFireSize(fireData, weatherData) {
      try {
        // Format date for ML model
        const day = getDayOfWeek(fireData.acq_date);
        const month = getMonthAbbr(fireData.acq_date);
        
        // Build the prediction request body
        const predictionData = {
          X: fireData.latitude,
          Y: fireData.longitude,
          month: month,
          day: day,
          temp: weatherData.temperature,
          RH: weatherData.humidity || 45, // Default humidity if not available
          wind: weatherData.wind_speed,
          rain: 0.0 // Assuming no rain data available, default to 0
        };
        
        console.log("Sending prediction request:", predictionData);
        
        // Make request to ML model endpoint
        const response = await fetch(`${pythonURI}/api/forest-fire/predict`, {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(predictionData)
        });
        
        if (!response.ok) {
          throw new Error('ML prediction API response was not ok');
        }
        
        const predictionResult = await response.json();
        console.log("Received prediction:", predictionResult);
        return predictionResult.predicted_area || 0.5; // Assuming the API returns { predicted_size: X }
      } catch (error) {
        console.error('Error predicting fire size:', error);
        // Return a default prediction based on fire intensity if ML fails
        return (parseFloat(fireData.intensity) * 2) || 0.5;
      }
    }

    // ============ VISUALIZATION FUNCTIONS ============
    
    // Create a fire icon with size based on predicted size
    function createFireIcon(intensity, predictedSize) {
      // Determine icon size based on predicted hectares
      let iconSize = 30; // Default size
      
      if (predictedSize > 50) {
        iconSize = 50; // Very large fire
      } else if (predictedSize > 20) {
        iconSize = 45; // Large fire
      } else if (predictedSize > 10) {
        iconSize = 40; // Medium-large fire
      } else if (predictedSize > 5) {
        iconSize = 35; // Medium fire
      }
      
      // Determine color based on intensity
      const intensityValue = parseFloat(intensity);
      let bgColor = 'bg-yellow-500'; // Low intensity
      
      if (intensityValue > 1.0) {
        bgColor = 'bg-red-600'; // High intensity
      } else if (intensityValue > 0.5) {
        bgColor = 'bg-orange-500'; // Medium intensity
      }
      
      // Create the icon
      const icon = L.divIcon({
        className: `map-marker ${bgColor}`,
        html: `<span>🔥</span>`,
        iconSize: [iconSize, iconSize],
        iconAnchor: [iconSize/2, iconSize/2]
      });
      
      return icon;
    }

    // Add fire markers to map with prediction info
    async function addFireMarkersToMapWithPrediction() {
      // Clear existing markers
      map.eachLayer(layer => {
        if (layer instanceof L.Marker || layer instanceof L.Circle) {
          map.removeLayer(layer);
        }
      });
      
      // Fetch fire data
      const fireData = await fetchFIRMSData();
      console.log("Fetched FIRMS fire data:", fireData);
      
      // Process each fire with prediction
      for (const fire of fireData) {
        // Get weather for this specific fire location
        const weatherData = await getWeatherForLocation(fire.latitude, fire.longitude);
        console.log(`Weather for fire ${fire.name}:`, weatherData);
        
        // Predict fire size
        const predictedSize = await predictFireSize(fire, weatherData);
        console.log(`Predicted size for fire ${fire.name}: ${predictedSize} hectares`);
        
        // Store prediction with fire data
        fire.predictedSize = predictedSize;
        
        // Create marker with size based on prediction
        const marker = L.marker(fire.position, {
          icon: createFireIcon(fire.intensity, predictedSize)
        }).addTo(map);
        
        // Determine risk class based on predicted size
        let riskLevel = 'low';
        if (predictedSize > 20) {
          riskLevel = 'high';
        } else if (predictedSize > 5) {
          riskLevel = 'medium';
        }
        
        // Format intensity for display
        const intensityValue = parseFloat(fire.intensity);
        const intensityPercent = Math.min(Math.round(intensityValue * 100), 100) + '%';
        
        // Custom popup content with prediction info
        const popupContent = `
          <div class="fire-details">
            <h3 class="font-bold text-lg">${fire.name}</h3>
            <div class="mt-2">
              <p>Confidence: ${fire.confidence}</p>
              <p>Intensity: ${fire.intensity}</p>
              <p>Date: ${fire.acq_date} Time: ${fire.acq_time}</p>
              <p>Day/Night: ${fire.daynight}</p>
              <p class="font-bold mt-1">Predicted Size: ${predictedSize.toFixed(2)} hectares</p>
              <p>Risk Level: <span class="text-${riskLevel === 'high' ? 'red' : riskLevel === 'medium' ? 'orange' : 'yellow'}-500">${riskLevel.toUpperCase()}</span></p>
              <div class="mt-2 h-2 bg-gray-800 rounded-full overflow-hidden">
                <div class="h-full bg-orange-500 rounded-full" style="width: ${intensityPercent}"></div>
              </div>
            </div>
          </div>
        `;
        
        marker.bindPopup(popupContent);
        
        // Add circle overlay to visualize predicted fire size
        const radiusInMeters = Math.sqrt(predictedSize * 10000 / Math.PI); // Convert hectares to radius in meters
        L.circle(fire.position, {
          radius: radiusInMeters,
          fillColor: riskLevel === 'high' ? '#ef4444' : 
                  riskLevel === 'medium' ? '#f97316' : 
                  '#eab308',
          fillOpacity: 0.2,
          color: riskLevel === 'high' ? '#dc2626' : 
              riskLevel === 'medium' ? '#ea580c' : 
              '#ca8a04',
          weight: 1
        }).addTo(map);
      }
      
      return fireData;
    }

    // ============ INITIALIZATION AND EVENT HANDLERS ============
    
    // Function to initialize all dashboard components
    async function initializeDashboard() {
      // Get user's weather data for dashboard display
      await getUserLocationWeather();
      
      // Fetch fire incident data for table
      await fetchFireData();
      
      // Add fire markers to map with ML predictions
      await addFireMarkersToMapWithPrediction();
      
      // Add event listeners for control buttons
      setupControlButtons();
    }
    
    // Set up control button event listeners
    function setupControlButtons() {
      // Layer button handler
      const layersButton = document.querySelector('button:nth-child(1)');
      if (layersButton) {
        layersButton.addEventListener('click', function() {
          // In a real app, this would toggle different map layers
          alert('Layer controls would appear here');
        });
      }
      
      // Markers button handler
      const markersButton = document.querySelector('button:nth-child(2)');
      if (markersButton) {
        markersButton.addEventListener('click', function() {
          // In a real app, this would toggle marker visibility
          alert('Marker controls would appear here');
        });
      }
      
      // Refresh button handler
      const refreshButton = document.getElementById('refresh-data');
      if (refreshButton) {
        refreshButton.addEventListener('click', async function() {
          console.log("Refresh clicked, fetching new data...");
          // Update all data components
          await getUserLocationWeather();
          await fetchFireData();
          await addFireMarkersToMapWithPrediction();
        });
      }
    }
    
    // Initialize the dashboard when document is ready
    initializeDashboard();
  });
</script>

<a href="/QcommVNE_Frontend/help/" class="fixed bottom-4 right-4 bg-green-600 text-white rounded-full p-3 shadow-lg hover:bg-green-700 transition duration-200 flex items-center justify-center" title="Help Center" style="font-size:1.05em;">
    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9 5.25h.008v.008H12v-.008z"/>
    </svg>
    <span class="ml-1 font-medium">Help</span>
  </a>