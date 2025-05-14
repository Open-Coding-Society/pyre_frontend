---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /earthquakes/
title: Earthquakes
---

<title>California Earthquakes Map</title>
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.4.1/papaparse.min.js"></script>

<style>
  #map { height: 80vh; width: 100%; }
</style>

<div class="p-4 text-center">
  <h1 class="text-2xl font-bold mb-2">California Earthquakes</h1>
</div>
<div id="map"></div>

<script>
  const map = L.map('map').setView([36.7783, -119.4179], 6);

  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '© OpenStreetMap contributors'
  }).addTo(map);

  fetch('/earthquakes.csv') // Make sure this is the correct relative path
    .then(response => response.text())
    .then(csvText => {
      Papa.parse(csvText, {
        header: true,
        skipEmptyLines: true,
        complete: function(results) {
          results.data.forEach(eq => {
            const lat = parseFloat(eq.latitude);
            const lng = parseFloat(eq.longitude);
            const mag = parseFloat(eq.mag);
            if (!isNaN(lat) && !isNaN(lng)) {
              const marker = L.circleMarker([lat, lng], {
                radius: Math.max(4, mag || 4),
                color: 'red',
                fillColor: 'red',
                fillOpacity: 0.5,
                weight: 1
              }).addTo(map);

              const popupContent = `
                <div class="text-sm">
                  <strong>Location:</strong> ${eq.place || 'N/A'}<br/>
                  <strong>Magnitude:</strong> ${eq.mag || 'N/A'}<br/>
                  <strong>Depth:</strong> ${eq.depth || 'N/A'} km<br/>
                  <strong>Time:</strong> ${new Date(eq.time).toLocaleString()}
                </div>
              `;
              marker.bindPopup(popupContent);
            }
          });
        }
      });
    });
</script>
