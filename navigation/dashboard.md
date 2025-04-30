---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /dashboard/
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
    .weather-info-panel {
      position: absolute;
      bottom: 20px;
      left: 20px;
      background-color: rgba(17, 24, 39, 0.85);
      border: 1px solid #374151;
      border-radius: 0.5rem;
      padding: 1rem;
      color: white;
      max-width: 300px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
      z-index: 1000;
      backdrop-filter: blur(8px);
      transition: opacity 0.3s ease;
    }
    .weather-info-loading {
      opacity: 0.7;
    }
    .loader {
      border: 3px solid rgba(255, 255, 255, 0.3);
      border-radius: 50%;
      border-top: 3px solid #fff;
      width: 20px;
      height: 20px;
      animation: spin 1s linear infinite;
      display: inline-block;
      vertical-align: middle;
      margin-right: 8px;
    }
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
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
      
      <!-- Temperature widget -->
      <div class="mb-6">
        <div class="text-sm text-gray-400 mb-1">Current Temperature</div>
        <div id="current-temperature" class="text-4xl font-bold">--°F</div>
        <div class="mt-2">
          <div class="text-sm text-gray-400 mb-1">Condition</div>
          <div id="current-condition" class="text-xl font-medium">--</div>
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
          
          <!-- Weather info panel (initially hidden) -->
          <div id="weather-info-panel" class="weather-info-panel" style="display: none;">
            <h3 class="font-medium text-lg mb-2">Climate Data</h3>
            <div id="weather-loading" style="display: none;">
              <span class="loader"></span>
              <span>Loading climate data...</span>
            </div>
            <div id="weather-content">
              <div class="flex items-center justify-between mb-2">
                <span class="text-gray-300">Location:</span>
                <span id="hover-location" class="font-medium">--</span>
              </div>
              <div class="flex items-center justify-between mb-2">
                <span class="text-gray-300">Temperature:</span>
                <span id="hover-temperature" class="font-medium">--</span>
              </div>
              <div class="flex items-center justify-between mb-2">
                <span class="text-gray-300">Condition:</span>
                <span id="hover-condition" class="font-medium">--</span>
              </div>
              <div class="flex items-center justify-between mb-2">
                <span class="text-gray-300">Wind:</span>
                <span id="hover-wind" class="font-medium">--</span>
              </div>
              <div class="flex items-center justify-between">
                <span class="text-gray-300">Humidity:</span>
                <span id="hover-humidity" class="font-medium">--</span>
              </div>
            </div>
          </div>
          
          <!-- Map controls -->
          <div class="absolute top-4 right-4 flex space-x-2">
            <button class="bg-gray-200 bg-opacity-20 backdrop-blur-sm rounded-md px-3 py-1 text-sm text-gray-200 flex items-center hover:bg-opacity-30">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M3 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z" clip-rule="evenodd" />
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
          <div id="last-updated-text" class="text-sm text-gray-400">Last updated: --</div>
          <button class="bg-gradient-to-r from-orange-600 to-red-600 hover:from-orange-500 hover:to-red-500 text-white px-3 py-1 rounded flex items-center text-sm">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M3 17a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm3.293-7.707a1 1 0 011.414 0L9 10.586V3a1 1 0 112 0v7.586l1.293-1.293a1 1 0 111.414 1.414l-3 3a1 1 0 01-1.414 0l-3-3a1 1 0 010-1.414z" clip-rule="evenodd" />
            </svg>
            Export Report
          </button>
        </div>
      </div>
    </div>
    
    <!-- Right sidebar -->
    <div class="w-72 bg-gray-900/50 border-l border-gray-800 p-4 overflow-y-auto">
      <div class="mb-6">
        <h2 class="text-lg font-medium mb-4">Location Details</h2>
        
        <div class="space-y-4">
          <div>
            <div class="flex justify-between mb-1">
              <span class="text-gray-400">Current Location</span>
              <span id="detail-location" class="font-medium">San Diego, CA</span>
            </div>
          </div>
          
          <div>
            <div class="flex justify-between mb-1">
              <span class="text-gray-400">Coordinates</span>
              <span id="detail-coordinates" class="font-medium">32.7157, -117.1611</span>
            </div>
          </div>
        </div>
      </div>
      
      <div>
        <h2 class="text-lg font-medium mb-4">Climate Status</h2>
        
        <div class="space-y-4">
          <div>
            <div class="flex justify-between mb-1">
              <span class="text-gray-400">Temperature</span>
              <span id="detail-temperature" class="font-medium">--°F</span>
            </div>
          </div>
          
          <div>
            <div class="flex justify-between mb-1">
              <span class="text-gray-400">Feels Like</span>
              <span id="detail-feels-like" class="font-medium">--°F</span>
            </div>
          </div>
          
          <div>
            <div class="flex justify-between mb-1">
              <span class="text-gray-400">Wind Speed</span>
              <span id="detail-wind-speed" class="font-medium">-- kph</span>
            </div>
          </div>
          
          <div>
            <div class="flex justify-between mb-1">
              <span class="text-gray-400">Humidity</span>
              <span id="detail-humidity" class="font-medium">--%</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  // Find the map container
  const mapContainer = document.querySelector('.bg-gray-900\\/50.rounded-lg.overflow-hidden.h-full.relative.border.border-gray-800');
  
  // Clear placeholder content
  mapContainer.innerHTML = '<div id="map"></div><div id="weather-info-panel" class="weather-info-panel" style="display: none;"><h3 class="font-medium text-lg mb-2">Climate Data</h3><div id="weather-loading" style="display: none;"><span class="loader"></span><span>Loading climate data...</span></div><div id="weather-content"><div class="flex items-center justify-between mb-2"><span class="text-gray-300">Location:</span><span id="hover-location" class="font-medium">--</span></div><div class="flex items-center justify-between mb-2"><span class="text-gray-300">Temperature:</span><span id="hover-temperature" class="font-medium">--</span></div><div class="flex items-center justify-between mb-2"><span class="text-gray-300">Condition:</span><span id="hover-condition" class="font-medium">--</span></div><div class="flex items-center justify-between mb-2"><span class="text-gray-300">Wind:</span><span id="hover-wind" class="font-medium">--</span></div><div class="flex items-center justify-between"><span class="text-gray-300">Humidity:</span><span id="hover-humidity" class="font-medium">--</span></div></div></div>';
  
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
  
  // Variables for throttling API calls
  let lastFetchTime = 0;
  let lastPosition = null;
  let weatherPanel = document.getElementById('weather-info-panel');
  let weatherLoading = document.getElementById('weather-loading');
  let weatherContent = document.getElementById('weather-content');
  let climateDataEnabled = false;
  
  // Throttle function to limit API calls
  function throttle(callback, delay) {
    let lastCall = 0;
    return function(...args) {
      const now = new Date().getTime();
      if (now - lastCall < delay) {
        return;
      }
      lastCall = now;
      return callback(...args);
    };
  }
  
  // Toggle climate data button
  const toggleButton = document.getElementById('toggle-climate-data');
  if (toggleButton) {
    toggleButton.addEventListener('click', function() {
      climateDataEnabled = !climateDataEnabled;
      if (climateDataEnabled) {
        weatherPanel.style.display = 'block';
        this.classList.add('bg-gradient-to-r', 'from-orange-600', 'to-red-600');
        this.classList.remove('bg-gray-200', 'bg-opacity-20');
      } else {
        weatherPanel.style.display = 'none';
        this.classList.remove('bg-gradient-to-r', 'from-orange-600', 'to-red-600');
        this.classList.add('bg-gray-200', 'bg-opacity-20');
      }
    });
  }
  
  // Function to fetch weather data for specific coordinates
  async function fetchWeatherForCoordinates(lat, lng) {
    try {
      if (weatherLoading) weatherLoading.style.display = 'block';
      if (weatherContent) weatherContent.style.opacity = '0.5';
      
      // Use the PUBLIC endpoint instead of authenticated endpoint
      const response = await fetch(`/api/weather/public/at?lat=${lat}&lng=${lng}`);
      
      if (!response.ok) {
        throw new Error('API request failed with status: ' + response.status);
      }
      
      const data = await response.json();
      
      // Update the hover panel
      const hoverLocation = document.getElementById('hover-location');
      const hoverTemperature = document.getElementById('hover-temperature');
      const hoverCondition = document.getElementById('hover-condition');
      const hoverWind = document.getElementById('hover-wind');
      const hoverHumidity = document.getElementById('hover-humidity');
      
      if (hoverLocation) hoverLocation.textContent = data.location || 'Unknown';
      if (hoverTemperature) hoverTemperature.textContent = `${data.temperature_f}°F (${data.temperature_c}°C)`;
      if (hoverCondition) hoverCondition.textContent = data.condition || 'Unknown';
      if (hoverWind) hoverWind.textContent = `${data.wind_kph} kph`;
      if (hoverHumidity) hoverHumidity.textContent = `${data.humidity}%`;
      
      if (weatherLoading) weatherLoading.style.display = 'none';
      if (weatherContent) weatherContent.style.opacity = '1';
      
      return data;
    } catch (error) {
      console.error('Error fetching weather data:', error);
      if (weatherLoading) weatherLoading.style.display = 'none';
      if (weatherContent) weatherContent.style.opacity = '1';
      
      // Show error in panel
      const hoverLocation = document.getElementById('hover-location');
      const hoverTemperature = document.getElementById('hover-temperature');
      const hoverCondition = document.getElementById('hover-condition');
      const hoverWind = document.getElementById('hover-wind');
      const hoverHumidity = document.getElementById('hover-humidity');
      
      if (hoverLocation) hoverLocation.textContent = 'Error fetching data';
      if (hoverTemperature) hoverTemperature.textContent = '--';
      if (hoverCondition) hoverCondition.textContent = '--';
      if (hoverWind) hoverWind.textContent = '--';
      if (hoverHumidity) hoverHumidity.textContent = '--';
    }
  }
  
  // Throttled version of fetchWeatherForCoordinates
  const throttledFetchWeather = throttle(fetchWeatherForCoordinates, 1000);
  
  // Event handler for mouse movement on map
  map.on('mousemove', function(e) {
    if (!climateDataEnabled) return;
    
    const lat = e.latlng.lat.toFixed(4);
    const lng = e.latlng.lng.toFixed(4);
    
    // Only fetch if position changed significantly
    if (!lastPosition || 
        Math.abs(lastPosition.lat - lat) > 0.01 || 
        Math.abs(lastPosition.lng - lng) > 0.01) {
      
      lastPosition = { lat, lng };
      const detailCoordinates = document.getElementById('detail-coordinates');
      if (detailCoordinates) detailCoordinates.textContent = `${lat}, ${lng}`;
      
      // Fetch weather data for this location
      throttledFetchWeather(lat, lng);
    }
  });
  
  // Update fetchInitialWeatherData to use the public endpoint
  async function fetchInitialWeatherData() {
    try {
      // Use the PUBLIC endpoint instead of authenticated endpoint
      const response = await fetch('/api/weather/public/current');
      
      if (!response.ok) {
        throw new Error('API request failed with status: ' + response.status);
      }
      
      const data = await response.json();
      
      // Handle null or undefined values by providing defaults
      const safeData = {
        condition: data.condition || 'Unknown',
        feelslike_c: data.feelslike_c !== undefined ? data.feelslike_c : '--',
        feelslike_f: data.feelslike_f !== undefined ? data.feelslike_f : '--',
        humidity: data.humidity !== undefined ? data.humidity : '--',
        last_updated: data.last_updated || 'Unknown',
        location: data.location || 'Unknown',
        temperature_c: data.temperature_c !== undefined ? data.temperature_c : '--',
        temperature_f: data.temperature_f !== undefined ? data.temperature_f : '--',
        wind_kph: data.wind_kph !== undefined ? data.wind_kph : '--'
      };
      
      // Update the UI with the weather data - safely check if elements exist
      const currentTemperature = document.getElementById('current-temperature');
      const currentCondition = document.getElementById('current-condition');
      const currentWindSpeed = document.getElementById('current-wind-speed');
      const currentHumidity = document.getElementById('current-humidity');
      const currentFeelsLike = document.getElementById('current-feels-like');
      const detailLocation = document.getElementById('detail-location');
      const detailTemperature = document.getElementById('detail-temperature');
      const detailFeelsLike = document.getElementById('detail-feels-like');
      const detailWindSpeed = document.getElementById('detail-wind-speed');
      const detailHumidity = document.getElementById('detail-humidity');
      const lastUpdatedText = document.getElementById('last-updated-text');
      
      // Update UI elements if they exist
      if (currentTemperature) currentTemperature.textContent = `${safeData.temperature_f}°F`;
      if (currentCondition) currentCondition.textContent = safeData.condition;
      if (currentWindSpeed) currentWindSpeed.textContent = `${safeData.wind_kph} kph`;
      if (currentHumidity) currentHumidity.textContent = `${safeData.humidity}%`;
      if (currentFeelsLike) currentFeelsLike.textContent = `${safeData.feelslike_f}°F`;
      
      // Update right sidebar details
      if (detailLocation) detailLocation.textContent = safeData.location;
      if (detailTemperature) detailTemperature.textContent = `${safeData.temperature_f}°F`;
      if (detailFeelsLike) detailFeelsLike.textContent = `${safeData.feelslike_f}°F`;
      if (detailWindSpeed) detailWindSpeed.textContent = `${safeData.wind_kph} kph`;
      if (detailHumidity) detailHumidity.textContent = `${safeData.humidity}%`;
      
      // Update last updated text
      if (lastUpdatedText) lastUpdatedText.textContent = `Last updated: ${safeData.last_updated}`;
      
      console.log('Weather data received:', data);
      
    } catch (error) {
      console.error('Error fetching initial weather data:', error);
      
      // Show error in UI
      const currentTemperature = document.getElementById('current-temperature');
      const currentCondition = document.getElementById('current-condition');
      const currentWindSpeed = document.getElementById('current-wind-speed');
      const currentHumidity = document.getElementById('current-humidity');
      const currentFeelsLike = document.getElementById('current-feels-like');
      const lastUpdatedText = document.getElementById('last-updated-text');
      
      if (currentTemperature) currentTemperature.textContent = '--';
      if (currentCondition) currentCondition.textContent = 'Error loading data';
      if (currentWindSpeed) currentWindSpeed.textContent = '--';
      if (currentHumidity) currentHumidity.textContent = '--';
      if (currentFeelsLike) currentFeelsLike.textContent = '--';
      if (lastUpdatedText) lastUpdatedText.textContent = `Last updated: Error fetching data`;
    }
  }
  
  // Fetch initial weather data
  fetchInitialWeatherData();
  
  // Add some fire incident markers to the map for demonstration
  const fireIncidents = [
    { lat: 32.7353, lng: -117.1490, risk: 'high', name: 'North Park Fire' },
    { lat: 32.7155, lng: -117.1902, risk: 'medium', name: 'Mission Hills Incident' },
    { lat: 32.6859, lng: -117.1831, risk: 'low', name: 'Coronado Brush Fire' }
  ];
  
  fireIncidents.forEach(incident => {
    // Create custom marker
    const markerHtml = `<div class="map-marker">${incident.risk.charAt(0).toUpperCase()}</div>`;
    const icon = L.divIcon({
      html: markerHtml,
      className: '',
      iconSize: [30, 30],
      iconAnchor: [15, 15]
    });
    
    // Add marker to map
    const marker = L.marker([incident.lat, incident.lng], {icon: icon}).addTo(map);
    
    // Add popup
    let riskClass = '';
    if (incident.risk === 'high') riskClass = 'risk-high';
    else if (incident.risk === 'medium') riskClass = 'risk-medium';
    else riskClass = 'risk-low';
    
    const popupContent = `
      <div class="p-2">
        <h3 class="font-medium mb-1">${incident.name}</h3>
        <div class="mb-1">
          <span class="text-gray-300">Risk Level:</span>
          <span class="${riskClass} font-medium">${incident.risk.toUpperCase()}</span>
        </div>
        <div class="mb-1">
          <span class="text-gray-300">Coordinates:</span>
          <span>${incident.lat.toFixed(4)}, ${incident.lng.toFixed(4)}</span>
        </div>
        <div class="mt-2">
          <button class="bg-gradient-to-r from-orange-600 to-red-600 hover:from-orange-500 hover:to-red-500 text-white px-2 py-1 rounded text-xs">
            View Details
          </button>
        </div>
      </div>
    `;
    
    const popup = L.popup({
      className: 'fire-popup',
      closeButton: true,
      autoClose: true,
      closeOnEscapeKey: true,
      closeOnClick: true
    }).setContent(popupContent);
    
    marker.bindPopup(popup);
  });
  
  // Set up a regular interval to update weather data every 5 minutes
  setInterval(fetchInitialWeatherData, 300000);
  
  // Add event listener for export report button
  const exportButton = document.querySelector('.bg-gradient-to-r.from-orange-600.to-red-600.hover\\:from-orange-500.hover\\:to-red-500.text-white.px-3.py-1.rounded.flex.items-center.text-sm');
  if (exportButton) {
    exportButton.addEventListener('click', function() {
      alert('Exporting report... This would generate a PDF or CSV in a real application.');
    });
  }
  
  // Add event listener for layers button
  const layersButton = document.querySelector('button.bg-gray-200.bg-opacity-20.backdrop-blur-sm.rounded-md.px-3.py-1.text-sm.text-gray-200.flex.items-center.hover\\:bg-opacity-30');
  if (layersButton) {
    layersButton.addEventListener('click', function() {
      alert('This would open a layer control panel in a real application.');
    });
  }
  
  // Add heat map layer for demonstration
  const heatData = [];
  for (let i = 0; i < 100; i++) {
    // Create random points around San Diego
    const lat = 32.7157 + (Math.random() - 0.5) * 0.1;
    const lng = -117.1611 + (Math.random() - 0.5) * 0.1;
    const intensity = Math.random() * 0.5; // Random intensity between 0 and 0.5
    heatData.push([lat, lng, intensity]);
  }
  
  // Check if the heat map library is available and add heat map
  if (typeof L.heatLayer === 'function') {
    const heat = L.heatLayer(heatData, {
      radius: 25,
      blur: 15,
      maxZoom: 17,
      gradient: {0.4: 'blue', 0.65: 'lime', 0.85: 'yellow', 1: 'red'}
    }).addTo(map);
  } else {
    console.warn('Heat map plugin not available. Include leaflet-heat.js to enable this feature.');
  }
  
  // Add a geolocation button
  const locateControl = L.control({position: 'topright'});
  locateControl.onAdd = function(map) {
    const div = L.DomUtil.create('div', 'leaflet-bar leaflet-control');
    div.innerHTML = `
      <a href="#" title="Show my location" style="display: flex; align-items: center; justify-content: center; width: 30px; height: 30px; background-color: white; color: #333;">
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor" width="16" height="16">
          <path d="M12 8c-2.21 0-4 1.79-4 4s1.79 4 4 4 4-1.79 4-4-1.79-4-4-4zm8.94 3A8.994 8.994 0 0 0 13 3.06V1h-2v2.06A8.994 8.994 0 0 0 3.06 11H1v2h2.06A8.994 8.994 0 0 0 11 20.94V23h2v-2.06A8.994 8.994 0 0 0 20.94 13H23v-2h-2.06zM12 19c-3.87 0-7-3.13-7-7s3.13-7 7-7 7 3.13 7 7-3.13 7-7 7z"/>
        </svg>
      </a>
    `;
    
    div.querySelector('a').addEventListener('click', function(e) {
      e.preventDefault();
      if (navigator.geolocation) {
        map.locate({setView: true, maxZoom: 16});
      } else {
        alert('Geolocation is not supported by your browser');
      }
    });
    
    return div;
  };
  locateControl.addTo(map);
  
  // Handle location found event
  map.on('locationfound', function(e) {
    const radius = e.accuracy / 2;
    
    // Create location marker
    L.marker(e.latlng).addTo(map)
      .bindPopup(`You are within ${radius} meters from this point`).openPopup();
    
    // Draw accuracy circle
    L.circle(e.latlng, radius).addTo(map);
    
    // Update detail coordinates
    const detailCoordinates = document.getElementById('detail-coordinates');
    if (detailCoordinates) {
      detailCoordinates.textContent = `${e.latlng.lat.toFixed(4)}, ${e.latlng.lng.toFixed(4)}`;
    }
    
    // Fetch weather for current location
    fetchWeatherForCoordinates(e.latlng.lat, e.latlng.lng);
  });
  
  // Handle location error
  map.on('locationerror', function(e) {
    alert('Error finding your location: ' + e.message);
  });
});