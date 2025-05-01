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
      <div class="mb-6">
        <h3 class="text-sm text-gray-400 mb-3">Temperature Trend</h3>
        <div class="bg-gray-900/70 rounded-lg p-3 h-40">
          <!-- Temperature chart -->
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
      <!-- Wind widget -->
      <div class="mb-6">
        <h3 class="text-sm text-gray-400 mb-3">Wind Analysis</h3>
        <div class="bg-gray-900/70 rounded-lg p-3 h-40">
          <!-- Wind chart -->
          <div class="h-full w-full rounded flex items-end space-x-1">
            <div class="h-2/5 w-6 bg-orange-600 rounded-t"></div>
            <div class="h-3/5 w-6 bg-orange-600 rounded-t"></div>
            <div class="h-1/5 w-6 bg-orange-600 rounded-t"></div>
            <div class="h-2/5 w-6 bg-orange-600 rounded-t"></div>
            <div class="h-1/4 w-6 bg-orange-600 rounded-t"></div>
            <div class="h-3/5 w-6 bg-orange-600 rounded-t"></div>
            <div class="h-4/5 w-6 bg-orange-600 rounded-t"></div>
          </div>
          <div class="flex justify-between text-xs text-gray-500 mt-2">
            <div>Mon</div>
            <div>Wed</div>
            <div>Fri</div>
            <div>Sun</div>
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
            <button class="bg-gray-200 bg-opacity-20 backdrop-blur-sm rounded-md px-3 py-1 text-sm text-gray-200 flex items-center hover:bg-opacity-30">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z" clip-rule="evenodd" />
              </svg>
              Markers
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
            Predictive Models
          </button>
          <button class="flex items-center text-gray-400 hover:text-white">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" viewBox="0 0 20 20" fill="currentColor">
              <path d="M2 5a2 2 0 012-2h7a2 2 0 012 2v4a2 2 0 01-2 2H9l-3 3v-3H4a2 2 0 01-2-2V5z" />
              <path d="M15 7v2a4 4 0 01-4 4H9.828l-1.766 1.767c.28.149.599.233.938.233h2l3 3v-3h2a2 2 0 002-2V9a2 2 0 00-2-2h-1z" />
            </svg>
            Communications
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
    // Define the pythonURI since the import might not be working
    import { pythonURI, fetchOptions } from '/QcommVNE_Frontend/assets/js/api/config.js';
    
    // Define fetchFireData function before it's called
    async function fetchFireData() {
      try {
        // Make the actual API request to the endpoint
        const response = await fetch(`${pythonURI}/fire-resource`);
        
        if (!response.ok) {
          throw new Error('Network response was not ok');
        }

        const data = await response.json();
        console.log(data)
        console.log(data.today_incidents)
        console.log(data.category_counts)
        console.log(data.category_counts['HAZARD'])
        console.log(data.category_counts['Life-Threatening Emergency Response'])
        console.log(data.category_counts['Life-Threatening Emergency Response'] + data.category_counts['HAZARD'])
        console.log(typeof data.category_counts)
        let total_incidents = data.category_counts['Life-Threatening Emergency Response'] + data.category_counts['HAZARD'] + data.category_counts['Non-Life-Threatening Response'] + data.category_counts['Urgent Response']
        
        // Update the incident table
        updateIncidentTable(data.today_incidents);
        
        // // Update counters and stats
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

    // Map initialization code
    document.addEventListener('DOMContentLoaded', function() {
      // Find the map container
      const mapContainer = document.querySelector('.bg-gray-900\\/50.rounded-lg.overflow-hidden.h-full.relative.border.border-gray-800');
      
      // Clear placeholder content
      mapContainer.innerHTML = '<div id="map"></div>';
      
      // Initialize the map centered on San Diego
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
      
      // Sample fire data
      const fireData = [
        { id: 1, position: [32.8328, -117.2713], name: "Torrey Pines", risk: "high", containment: "35%" },
        { id: 2, position: [32.7336, -117.1831], name: "Balboa Park", risk: "medium", containment: "70%" },
        { id: 3, position: [32.7638, -117.2273], name: "Mission Bay", risk: "medium", containment: "55%" },
        { id: 4, position: [32.5967, -117.1139], name: "Chula Vista", risk: "low", containment: "90%" },
        { id: 5, position: [33.1192, -117.0864], name: "Escondido", risk: "high", containment: "20%" },
        { id: 6, position: [32.7953, -116.9422], name: "El Cajon", risk: "medium", containment: "45%" },
        { id: 7, position: [32.8328, -116.7764], name: "Alpine", risk: "high", containment: "15%" },
        { id: 8, position: [33.0169, -116.9678], name: "Poway", risk: "low", containment: "85%" }
      ];
      
      // Custom fire icon function
      function createFireIcon(risk) {
        const className = risk === 'high' ? 'map-marker bg-red-600' : 
                        risk === 'medium' ? 'map-marker bg-orange-500' : 
                        'map-marker bg-yellow-500';
        
        const icon = L.divIcon({
          className: className,
          html: `<span>🔥</span>`,
          iconSize: [30, 30],
          iconAnchor: [15, 15]
        });
        
        return icon;
      }
      
      // Add fire markers to map
      fireData.forEach(fire => {
        const marker = L.marker(fire.position, {
          icon: createFireIcon(fire.risk)
        }).addTo(map);
        
        const riskClass = fire.risk === 'high' ? 'risk-high' : 
                         fire.risk === 'medium' ? 'risk-medium' : 
                         'risk-low';
        
        // Custom popup content
        const popupContent = `
          <div class="fire-details">
            <h3 class="font-bold text-lg">${fire.name}</h3>
            <div class="mt-2">
              <p>Risk Level: <span class="${riskClass} font-medium">${fire.risk.toUpperCase()}</span></p>
              <p>Containment: ${fire.containment}</p>
              <div class="mt-2 h-2 bg-gray-800 rounded-full overflow-hidden">
                <div class="h-full bg-orange-500 rounded-full" style="width: ${fire.containment}"></div>
              </div>
            </div>
          </div>
        `;
          
        marker.bindPopup(popupContent, {
          className: 'fire-popup',
          maxWidth: 200
        });
      });
      
      // Draw a fire risk heatmap overlay
      const riskArea = L.polygon([
        [32.8428, -117.2813],
        [32.8528, -117.2513],
        [32.8228, -117.2513],
        [32.8128, -117.2813]
      ], {
        color: '#ef4444',
        fillColor: '#ef4444',
        fillOpacity: 0.3
      }).addTo(map);
      
      // Fix layer and marker buttons event handlers to use proper DOM methods
      const layersButton = document.querySelector('button:nth-child(1)');
      const markersButton = document.querySelector('button:nth-child(2)');
      
      if (layersButton) {
        layersButton.addEventListener('click', function() {
          // In a real app, this would toggle different map layers
          alert('Layer controls would appear here');
        });
      }
      
      if (markersButton) {
        markersButton.addEventListener('click', function() {
          // In a real app, this would toggle marker visibility
          alert('Marker controls would appear here');
        });
      }
      
      // Fetch and display fire incident data
      fetchFireData();
      
      // Add refresh handler
      document.getElementById('refresh-data').addEventListener('click', function() {
        fetchFireData();
        console.log("Refresh clicked, fetching new data...");
      });
    });
</script>

<script type="module">
  import { pythonURI, fetchOptions } from '/QcommVNE_Frontend/assets/js/api/config.js';
  async function fetchWeatherData() {
    try {
      // Make the API request to the weather endpoint
      const response = await fetch(`${pythonURI}/api/get_weather`);
      
      if (!response.ok) {
        throw new Error('Weather API response was not ok');
      }

      const data = await response.json();
      console.log("Weather data:", data);
      
      // Update the temperature trend chart with actual data
      updateTemperatureChart(data);
      
      // Update the wind analysis chart
      updateWindChart(data);
      
      return data;
    } catch (error) {
      console.error('Error fetching weather data:', error);
      return null;
    }
  }

  // Function to update the temperature chart with real data
  function updateTemperatureChart(weatherData) {
    const tempContainer = document.querySelector('.temperature-chart');
    if (!tempContainer) {
      console.error("Temperature chart container not found");
      return;
    }
    
    // Clear existing chart
    tempContainer.innerHTML = '';
    
    // Create line chart for temperature instead of bar chart
    const chart = document.createElement('div');
    chart.className = 'h-full w-full flex items-end relative';
    
    // Get temperature from weather data (assuming it's in Fahrenheit)
    const temperature = weatherData.weather.temperature;
    const timestamp = new Date(weatherData.timestamp * 1000);
    
    // Add temperature value display
    const tempDisplay = document.createElement('div');
    tempDisplay.className = 'absolute top-0 right-0 text-xl font-bold';
    tempDisplay.textContent = `${Math.round(temperature)}°F`;
    
    // Add current conditions
    const conditionsDisplay = document.createElement('div');
    conditionsDisplay.className = 'absolute top-6 right-0 text-sm';
    conditionsDisplay.textContent = weatherData.weather.conditions;
    
    // For a line chart, we'd need multiple data points
    // For now, let's create a temperature gauge instead
    const gaugeContainer = document.createElement('div');
    gaugeContainer.className = 'h-full w-full flex items-center justify-center';
    
    // Create a semicircular gauge
    const gauge = document.createElement('div');
    gauge.className = 'w-32 h-32 relative';
    
    // Calculate temperature color (blue for cold, red for hot)
    const getTemperatureColor = (temp) => {
      if (temp < 40) return 'bg-blue-500';
      if (temp < 60) return 'bg-green-500';
      if (temp < 80) return 'bg-yellow-500';
      if (temp < 90) return 'bg-orange-500';
      return 'bg-red-500';
    };
    
    const tempColor = getTemperatureColor(temperature);
    
    // Create gauge HTML
    gauge.innerHTML = `
      <div class="absolute inset-0 flex items-center justify-center">
        <div class="text-3xl font-bold">${Math.round(temperature)}°</div>
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
          class="${tempColor.replace('bg-', 'stroke-')}"
          stroke-width="6"
          stroke-dasharray="295.31" 
          stroke-dashoffset="${295.31 - (295.31 * (temperature - 0) / (120 - 0))}"
        />
      </svg>
    `;
    
    gaugeContainer.appendChild(gauge);
    chart.appendChild(gaugeContainer);
    chart.appendChild(tempDisplay);
    chart.appendChild(conditionsDisplay);
    
    // Add the chart to the container
    tempContainer.appendChild(chart);
    
    // Update the weather conditions text
    const conditionsElement = document.getElementById('weather-conditions');
    if (conditionsElement) {
      conditionsElement.textContent = weatherData.weather.description;
    }
    
    // Add location information
    const locationElement = document.getElementById('weather-location');
    if (locationElement) {
      locationElement.textContent = weatherData.location.name;
    }
  }

  // Function to update the wind chart with real data
  function updateWindChart(weatherData) {
    const windContainer = document.querySelector('.wind-chart');
    if (!windContainer) {
      console.error("Wind chart container not found");
      return;
    }
    
    // Clear existing chart
    windContainer.innerHTML = '';
    
    // Get wind data
    const windSpeed = weatherData.weather.wind_speed;
    const windDirection = weatherData.weather.wind_direction;
    
    // Create wind display
    const windDisplay = document.createElement('div');
    windDisplay.className = 'h-full w-full flex flex-col items-center justify-center';
    
    // Convert degrees to cardinal direction
    function degreesToCardinal(degrees) {
      const cardinals = ["N", "NNE", "NE", "ENE", "E", "ESE", "SE", "SSE", "S", "SSW", "SW", "WSW", "W", "WNW", "NW", "NNW"];
      const index = Math.round(((degrees % 360) / 22.5));
      return cardinals[index % 16];
    }
    
    const cardinal = degreesToCardinal(windDirection);
    
    // Create HTML for wind display
    windDisplay.innerHTML = `
      <div class="relative w-32 h-32">
        <div class="absolute inset-0 flex items-center justify-center">
          <div class="text-3xl font-bold">${Math.round(windSpeed)}</div>
        </div>
        <div class="absolute bottom-0 left-0 right-0 text-center text-sm">mph</div>
        <svg class="absolute inset-0" viewBox="0 0 100 100">
          <circle cx="50" cy="50" r="45" fill="none" stroke="#374151" stroke-width="2" />
          <path 
            d="M 50 50 L ${50 + 35 * Math.sin(windDirection * Math.PI / 180)} ${50 - 35 * Math.cos(windDirection * Math.PI / 180)}" 
            stroke="${windSpeed > 15 ? '#ef4444' : windSpeed > 8 ? '#f97316' : '#10b981'}" 
            stroke-width="3" 
            stroke-linecap="round" 
          />
          <circle cx="50" cy="50" r="5" fill="${windSpeed > 15 ? '#ef4444' : windSpeed > 8 ? '#f97316' : '#10b981'}" />
        </svg>
      </div>
      <div class="text-xl mt-2">${cardinal}</div>
      <div class="text-sm opacity-70">Direction: ${Math.round(windDirection)}°</div>
    `;
    
    windContainer.appendChild(windDisplay);
  }

  // Call fetchWeatherData when document is loaded
  document.addEventListener('DOMContentLoaded', function() {
    // Update DOM to add classes for weather charts
    const tempChart = document.querySelector('.mb-6:nth-of-type(2) .bg-gray-900\\/70.rounded-lg.p-3.h-40');
    if (tempChart) {
      const tempChartContent = tempChart.querySelector('.h-full.w-full.rounded.flex.items-end.space-x-1');
      if (tempChartContent) {
        tempChartContent.className = 'temperature-chart h-full w-full';
      }
    }
    
    const windChart = document.querySelector('.mb-6:nth-of-type(3) .bg-gray-900\\/70.rounded-lg.p-3.h-40');
    if (windChart) {
      const windChartContent = windChart.querySelector('.h-full.w-full.rounded.flex.items-end.space-x-1');
      if (windChartContent) {
        windChartContent.className = 'wind-chart h-full w-full';
      }
    }
    
    // Add weather condition and location elements
    const tempWidget = document.querySelector('.mb-6:nth-of-type(2)');
    if (tempWidget) {
      const tempHeader = tempWidget.querySelector('h3');
      if (tempHeader) {
        tempHeader.textContent = 'Current Temperature';
        tempHeader.insertAdjacentHTML('afterend', `
          <div class="flex justify-between text-xs text-gray-400 mb-1">
            <div id="weather-location">--</div>
            <div id="weather-conditions">--</div>
          </div>
        `);
      }
    }
    
    // Fetch weather data initially
    fetchWeatherData();
    
    // Add refresh weather data to the refresh button click handler
    const refreshButton = document.getElementById('refresh-data');
    if (refreshButton) {
      const originalClickHandler = refreshButton.onclick;
      refreshButton.onclick = async function(e) {
        if (originalClickHandler) {
          originalClickHandler.call(this, e);
        }
        await fetchWeatherData();
      };
    }
  });
</script>

