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
</style>

<div class="min-h-screen bg-gray-950 text-gray-200">
    <br>
    <br>
  <!-- Dashboard content -->
  <div class="flex h-screen overflow-hidden pt-16 -mt-16">
    <!-- Left sidebar -->
    <div class="w-72 bg-gray-900/50 border-r border-gray-800 p-4 overflow-y-auto">
      <h2 class="text-lg font-medium mb-4">Analytics Overview</h2>
      
      <!-- Active fires widget -->
      <div class="mb-6">
        <div class="text-sm text-gray-400 mb-1">Active Fires</div>
        <div id="active-fires-count" class="text-4xl font-bold">0</div>
        <div class="mt-2">
          <div class="text-sm text-gray-400 mb-1">Risk Level</div>
          <div id="highest-risk-level" class="text-xl font-medium text-red-500">N/A</div>
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
                <path fill-rule="evenodd" d="M3 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z" clip-rule="evenodd" />
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
          <div class="text-sm text-gray-400">Last updated: 2 min ago</div>
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
        <h2 class="text-lg font-medium mb-4">Region Statistics</h2>
        
        <div class="space-y-4">
          <div>
            <div class="flex justify-between mb-1">
              <span class="text-gray-400">Northern Region</span>
              <span class="font-medium">12 active</span>
            </div>
            <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
              <div class="h-full bg-orange-500 rounded-full" style="width: 75%"></div>
            </div>
          </div>
          
          <div>
            <div class="flex justify-between mb-1">
              <span class="text-gray-400">Southern Region</span>
              <span class="font-medium">8 active</span>
            </div>
            <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
              <div class="h-full bg-orange-500 rounded-full" style="width: 45%"></div>
            </div>
          </div>
        </div>
      </div>
      
      <div>
        <h2 class="text-lg font-medium mb-4">Resource Status</h2>
        
        <div class="space-y-4">
          <div>
            <div class="flex justify-between mb-1">
              <span class="text-gray-400">Fire Trucks</span>
              <span class="font-medium">18/24 deployed</span>
            </div>
          </div>
          
          <div>
            <div class="flex justify-between mb-1">
              <span class="text-gray-400">Helicopters</span>
              <span class="font-medium">4/6 active</span>
            </div>
          </div>
          
          <div>
            <div class="flex justify-between mb-1">
              <span class="text-gray-400">Personnel</span>
              <span class="font-medium">156 on field</span>
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
      
      // Fetch fire data from the API
      fetch('/api/fire_incidents')
        .then(response => response.json())
        .then(data => {
          // Update active fires count and highest risk level
          const activeFiresCount = data.length;
          const highestRiskLevel = data.reduce((highest, fire) => {
            if (fire.risk === 'high') return 'High';
            if (fire.risk === 'medium' && highest !== 'High') return 'Medium';
            if (fire.risk === 'low' && highest === 'N/A') return 'Low';
            return highest;
          }, 'N/A');
          
          document.getElementById('active-fires-count').textContent = activeFiresCount;
          document.getElementById('highest-risk-level').textContent = highestRiskLevel;
          
          // Add fire markers to the map
          data.forEach(fire => {
            const marker = L.marker([fire.latitude, fire.longitude], {
              icon: L.divIcon({
                className: `map-marker ${fire.risk === 'high' ? 'bg-red-600' : fire.risk === 'medium' ? 'bg-orange-500' : 'bg-yellow-500'}`,
                html: `<span>🔥</span>`,
                iconSize: [30, 30],
                iconAnchor: [15, 15]
              })
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
        })
        .catch(error => console.error('Error fetching fire data:', error));
    });
</script>