---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /historical-earthquake/
title: Historical Earthquakes
---

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.css" />
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet.heat/0.2.0/leaflet-heat.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js"></script>

<div class="container mx-auto px-4 py-8">
  <!-- Header -->
  <div class="text-center mb-8">
    <h1 class="text-4xl font-bold text-white mb-2">Historical Earthquake Data Visualization Dashboard</h1>
    <p class="text-slate-600">Explore earthquake incidents and patterns</p>
  </div>

  <!-- Controls -->
  <div class="bg-white rounded-lg shadow-md p-6 mb-8">
    <div class="flex flex-wrap items-center gap-4">
      <div class="flex-1 min-w-200">
        <label for="yearSelect" class="block text-sm font-medium text-gray-700 mb-2">Year</label>
        <select id="yearSelect" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm">
          <option value="2023">2023</option>
          <option value="2024">2024</option>
          <option value="2025">2025</option>
        </select>
      </div>
      <div class="flex-1 min-w-200">
        <label for="monthSelect" class="block text-sm font-medium text-gray-700 mb-2">Month</label>
        <select id="monthSelect" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm">
          <option value="01">January</option>
          <option value="02">February</option>
          <option value="03">March</option>
          <option value="04">April</option>
          <option value="05">May</option>
          <option value="06">June</option>
          <option value="07">July</option>
          <option value="08">August</option>
          <option value="09">September</option>
          <option value="10">October</option>
          <option value="11">November</option>
          <option value="12">December</option>
        </select>
      </div>
      <div class="flex-1 min-w-200">
        <label for="mapType" class="block text-sm font-medium text-gray-700 mb-2">Map Type</label>
        <select id="mapType" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm">
          <option value="markers">Earthquake Locations</option>
          <option value="heatmap">Heat Map</option>
        </select>
      </div>
      <div class="flex-1 min-w-200">
        <label class="block text-sm font-medium text-gray-700 mb-2">&nbsp;</label>
        <button id="loadData" class="w-full bg-blue-600 text-white px-6 py-2 rounded-md">Load Data</button>
      </div>
    </div>
  </div>

  <!-- Stats Cards -->
  <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
    <div class="bg-white rounded-lg shadow-sm p-6 text-center">
      <h3 class="text-sm text-gray-500">Total Earthquakes</h3>
      <p id="totalEarthquakes" class="text-2xl font-semibold text-gray-900">-</p>
    </div>
    <div class="bg-white rounded-lg shadow-sm p-6 text-center">
      <h3 class="text-sm text-gray-500">Avg Magnitude</h3>
      <p id="avgMagnitude" class="text-2xl font-semibold text-gray-900">-</p>
    </div>
    <div class="bg-white rounded-lg shadow-sm p-6 text-center">
      <h3 class="text-sm text-gray-500">Avg Depth</h3>
      <p id="avgDepth" class="text-2xl font-semibold text-gray-900">-</p>
    </div>
    <div class="bg-white rounded-lg shadow-sm p-6 text-center">
      <h3 class="text-sm text-gray-500">Major Quakes (5.0+)</h3>
      <p id="majorQuakes" class="text-2xl font-semibold text-gray-900">-</p>
    </div>
  </div>

  <!-- Map -->
  <div class="bg-white rounded-lg shadow-md p-6">
    <h2 class="text-xl font-semibold text-gray-800 mb-4">Earthquake Locations</h2>
    <div id="map" class="h-96 rounded-lg border"></div>
  </div>
</div>

<!-- Loading Indicator -->
<div id="loadingIndicator" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
  <div class="bg-white rounded-lg p-6 flex items-center space-x-3">
    <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-600"></div>
    <span class="text-gray-700">Loading earthquake data...</span>
  </div>
</div>

<a href="/pyre_frontend/help/" class="fixed bottom-4 right-4 bg-green-600 text-white rounded-full p-3 shadow-lg hover:bg-green-700 transition duration-200 flex items-center justify-center" title="Help Center" style="font-size:1.05em;">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9 5.25h.008v.008H12v-.008z"/>
      </svg>
      <span class="ml-1 font-medium">Help</span>
    </a>

<script>
  const map = L.map('map').setView([20, 0], 2);
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    maxZoom: 18,
  }).addTo(map);

  async function loadData() {
    const year = document.getElementById("yearSelect").value;
    const month = document.getElementById("monthSelect").value;
    const mapType = document.getElementById("mapType").value;

    document.getElementById("loadingIndicator").classList.remove("hidden");

    fetch("/static/past_earthquakes.csv")
    const data = await response.json();

    map.eachLayer(layer => {
      if (layer instanceof L.CircleMarker || layer instanceof L.HeatLayer || layer instanceof L.Marker) {
        map.removeLayer(layer);
      }
    });

    if (mapType === 'markers') {
      data.forEach(quake => {
        if (quake.latitude && quake.longitude) {
          L.circleMarker([quake.latitude, quake.longitude], {
            radius: 4 + (quake.mag || 0),
            color: "red",
            weight: 1
          }).addTo(map).bindPopup(`Mag ${quake.mag}, Depth ${quake.depth} km`);
        }
      });
    } else if (mapType === 'heatmap') {
      const heatData = data.map(r => [r.latitude, r.longitude, r.mag || 0]);
      L.heatLayer(heatData, { radius: 25, blur: 15, maxZoom: 7 }).addTo(map);
    }

    // Stats
    document.getElementById("totalEarthquakes").textContent = data.length;
    document.getElementById("avgMagnitude").textContent = (
      data.reduce((sum, q) => sum + (parseFloat(q.mag) || 0), 0) / data.length
    ).toFixed(2);
    document.getElementById("avgDepth").textContent = (
      data.reduce((sum, q) => sum + (parseFloat(q.depth) || 0), 0) / data.length
    ).toFixed(2);
    document.getElementById("majorQuakes").textContent = data.filter(q => q.mag >= 5).length;

    document.getElementById("loadingIndicator").classList.add("hidden");
  }

  document.getElementById("loadData").addEventListener("click", loadData);
</script>

