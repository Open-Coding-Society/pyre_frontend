---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /historical/
title: Historical
---

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.css" />

<style>
    #map {
      height: 500px;
      width: 100%;
      z-index: 10;
    }
    .map-container {
      position: relative;
    }
    .map-overlay {
      position: absolute;
      top: 10px;
      right: 10px;
      z-index: 1000;
      background-color: white;
      padding: 10px;
      border-radius: 5px;
      box-shadow: 0 1px 5px rgba(0,0,0,0.4);
    }
</style>

<div class="container mx-auto px-4 py-8">
    <header class="mb-8">
      <h1 class="text-3xl font-bold text-gray-800 mb-2">Interactive Map</h1>
      <p class="text-gray-600">Explore locations using this interactive Leaflet.js map.</p>
    </header>
    <div class="bg-white rounded-lg shadow-md p-4 mb-8">
      <div class="map-container">
        <div id="map" class="rounded-lg"></div>
        <div class="map-overlay hidden md:block">
          <h3 class="text-lg font-semibold mb-2">Map Controls</h3>
          <div class="mb-4">
            <label class="block text-sm font-medium text-gray-700 mb-1">Map Style:</label>
            <select id="map-style" class="w-full p-2 border rounded text-sm">
              <option value="streets">Streets</option>
              <option value="satellite">Satellite</option>
              <option value="terrain">Terrain</option>
            </select>
          </div>
          <div>
            <button id="locate-me" class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded text-sm transition">Locate Me</button>
          </div>
        </div>
      </div>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
      <div class="bg-white rounded-lg shadow-md p-4">
        <h2 class="text-xl font-semibold mb-3 text-gray-800">Featured Locations</h2>
        <ul id="locations-list" class="space-y-2">
          <li class="p-2 hover:bg-gray-100 rounded cursor-pointer transition" data-lat="40.7128" data-lng="-74.0060">
            <span class="font-medium">New York City</span>
            <p class="text-sm text-gray-600">The Big Apple</p>
          </li>
          <li class="p-2 hover:bg-gray-100 rounded cursor-pointer transition" data-lat="34.0522" data-lng="-118.2437">
            <span class="font-medium">Los Angeles</span>
            <p class="text-sm text-gray-600">City of Angels</p>
          </li>
          <li class="p-2 hover:bg-gray-100 rounded cursor-pointer transition" data-lat="51.5074" data-lng="-0.1278">
            <span class="font-medium">London</span>
            <p class="text-sm text-gray-600">Capital of England</p>
          </li>
          <li class="p-2 hover:bg-gray-100 rounded cursor-pointer transition" data-lat="35.6762" data-lng="139.6503">
            <span class="font-medium">Tokyo</span>
            <p class="text-sm text-gray-600">Capital of Japan</p>
          </li>
        </ul>
      </div>
      <div class="bg-white rounded-lg shadow-md p-4">
        <h2 class="text-xl font-semibold mb-3 text-gray-800">Current Location</h2>
        <div id="current-location-info" class="text-gray-600">
          <p class="mb-2">Click on a location or use the "Locate Me" button to see details.</p>
          <div id="location-details" class="hidden">
            <p class="mb-1"><span class="font-medium">Name:</span> <span id="location-name">-</span></p>
            <p class="mb-1"><span class="font-medium">Latitude:</span> <span id="location-lat">-</span></p>
            <p class="mb-1"><span class="font-medium">Longitude:</span> <span id="location-lng">-</span></p>
          </div>
        </div>
      </div>
    </div>
    <footer class="text-center text-gray-500 text-sm">
      <p>Created with Leaflet.js and Tailwind CSS</p>
    </footer>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.js"></script>
<script>
    document.addEventListener('DOMContentLoaded', function() {
      // Initialize map
      const map = L.map('map').setView([20, 0], 2);
      // Add default tile layer (OpenStreetMap)
      let currentTileLayer = L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
      }).addTo(map);
      // Tile layer options
      const tileLayers = {
        streets: L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
          attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }),
        satellite: L.tileLayer('https://{s}.google.com/vt/lyrs=s&x={x}&y={y}&z={z}', {
          maxZoom: 20,
          subdomains: ['mt0', 'mt1', 'mt2', 'mt3'],
          attribution: '&copy; Google Maps'
        }),
        terrain: L.tileLayer('https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png', {
          maxZoom: 17,
          attribution: 'Map data: &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, <a href="http://viewfinderpanoramas.org">SRTM</a> | Map style: &copy; <a href="https://opentopomap.org">OpenTopoMap</a>'
        })
      };
      // Map style switcher
      document.getElementById('map-style').addEventListener('change', function(e) {
        const style = e.target.value;
        if (currentTileLayer) {
          map.removeLayer(currentTileLayer);
        }
        currentTileLayer = tileLayers[style].addTo(map);
      });
      // Create a markers layer group
      const markersLayer = L.layerGroup().addTo(map);
      // Function to create a marker
      function createMarker(lat, lng, title, description) {
        markersLayer.clearLayers();
        const marker = L.marker([lat, lng])
          .addTo(markersLayer)
          .bindPopup(`<b>${title}</b><br>${description || ''}`)
          .openPopup();
        // Update location details
        document.getElementById('location-details').classList.remove('hidden');
        document.getElementById('location-name').textContent = title;
        document.getElementById('location-lat').textContent = lat.toFixed(4);
        document.getElementById('location-lng').textContent = lng.toFixed(4);
        // Center map on marker
        map.setView([lat, lng], 10);
      }
      // Set up location list click events
      const locationItems = document.querySelectorAll('#locations-list li');
      locationItems.forEach(item => {
        item.addEventListener('click', function() {
          const lat = parseFloat(this.getAttribute('data-lat'));
          const lng = parseFloat(this.getAttribute('data-lng'));
          const title = this.querySelector('span').textContent;
          const description = this.querySelector('p').textContent;
          createMarker(lat, lng, title, description);
          // Add active class
          locationItems.forEach(li => li.classList.remove('bg-gray-100'));
          this.classList.add('bg-gray-100');
        });
      });
      // Locate me button
      document.getElementById('locate-me').addEventListener('click', function() {
        if ("geolocation" in navigator) {
          navigator.geolocation.getCurrentPosition(function(position) {
            const lat = position.coords.latitude;
            const lng = position.coords.longitude;
            createMarker(lat, lng, "Your Location", "This is your current position");
          }, function(error) {
            alert("Could not get your location: " + error.message);
          });
        } else {
          alert("Geolocation is not supported by your browser");
        }
      });
      // Map click event for custom location
      map.on('click', function(e) {
        createMarker(e.latlng.lat, e.latlng.lng, "Custom Location", "Lat: " + e.latlng.lat.toFixed(4) + ", Lng: " + e.latlng.lng.toFixed(4));
      });
      // Start with New York selected
      locationItems[0].click();
    });
</script>