---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /models/
---

<br>

<div class="min-h-screen bg-black text-white">
  <!-- Header -->
  <header class="text-center py-16">
    <h1 class="text-4xl font-bold mb-2">Data Sources</h1>
    <p class="text-xl text-gray-300">Comprehensive data collection for accurate wildfire prediction and monitoring</p>
  </header>

  <!-- Main Content -->
  <div class="container mx-auto px-4 pb-16">
    <!-- Primary Data Sources Section -->
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-12">
      <div class="bg-gray-900 rounded-lg p-6">
        <div class="flex items-center mb-4">
          <span class="text-2xl mr-3">🛰️</span>
          <h2 class="text-2xl font-semibold">Satellite Imagery</h2>
        </div>
        <ul class="space-y-2 text-gray-300">
          <li><span class="font-semibold text-orange-400">MODIS:</span> Moderate Resolution Imaging Spectroradiometer</li>
          <li><span class="font-semibold text-orange-400">VIIRS:</span> Visible Infrared Imaging Radiometer Suite</li>
          <li><span class="font-semibold text-orange-400">GOES:</span> Geostationary Operational Environmental Satellites</li>
          <li><span class="font-semibold text-orange-400">Landsat:</span> High-resolution earth observation data</li>
        </ul>
        <p class="mt-3 text-gray-400">Provides fire locations, radiative power, and temporal data for real-time tracking.</p>
      </div>

      <div class="bg-gray-900 rounded-lg p-6">
        <div class="flex items-center mb-4">
          <span class="text-2xl mr-3">📡</span>
          <h2 class="text-2xl font-semibold">Ground Sensors</h2>
        </div>
        <ul class="space-y-2 text-gray-300">
          <li><span class="font-semibold text-orange-400">ALERTWildfire:</span> Network of high-definition cameras</li>
          <li><span class="font-semibold text-orange-400">FireWatch:</span> Early detection sensor network</li>
          <li><span class="font-semibold text-orange-400">FireScout:</span> AI-powered camera systems</li>
        </ul>
        <p class="mt-3 text-gray-400">Delivers video feeds and IoT sensor data for ground-level detection and verification.</p>
      </div>

      <div class="bg-gray-900 rounded-lg p-6">
        <div class="flex items-center mb-4">
          <span class="text-2xl mr-3">🌤️</span>
          <h2 class="text-2xl font-semibold">Weather Data</h2>
        </div>
        <ul class="space-y-2 text-gray-300">
          <li><span class="font-semibold text-orange-400">NIFC:</span> National Interagency Fire Center data</li>
          <li><span class="font-semibold text-orange-400">NOAA:</span> National Oceanic and Atmospheric Administration</li>
          <li><span class="font-semibold text-orange-400">CAL FIRE:</span> California Department of Forestry data</li>
        </ul>
        <p class="mt-3 text-gray-400">Provides temperature, humidity, wind speed, and fuel moisture measurements.</p>
      </div>

      <div class="bg-gray-900 rounded-lg p-6">
        <div class="flex items-center mb-4">
          <span class="text-2xl mr-3">🏔️</span>
          <h2 class="text-2xl font-semibold">Terrain Data</h2>
        </div>
        <ul class="space-y-2 text-gray-300">
          <li><span class="font-semibold text-orange-400">USGS:</span> Topographical information</li>
          <li><span class="font-semibold text-orange-400">Vegetation Maps:</span> Density and fuel type analysis</li>
        </ul>
        <p class="mt-3 text-gray-400">Enables terrain-aware fire spread prediction and risk assessment.</p>
      </div>
    </div>

    <!-- Government Data Sources -->
    <div class="bg-gray-900 rounded-lg p-6 mb-12">
      <div class="flex items-center mb-6">
        <span class="text-2xl mr-3">🏛️</span>
        <h2 class="text-2xl font-semibold">Government Datasets (Data.Gov)</h2>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <div class="bg-gray-800 rounded-lg p-4">
          <h3 class="font-semibold mb-2 text-orange-400">California Fire Perimeters</h3>
          <p class="text-sm text-gray-300 mb-3">Historical wildfire burn data throughout California</p>
          <a href="https://catalog.data.gov/dataset/california-fire-perimeters-all-5b4ee" class="text-blue-400 hover:underline text-sm">Access Dataset →</a>
        </div>
        <div class="bg-gray-800 rounded-lg p-4">
          <h3 class="font-semibold mb-2 text-orange-400">Combined Wildfire Datasets</h3>
          <p class="text-sm text-gray-300 mb-3">Historical fire locations across the United States</p>
          <a href="https://catalog.data.gov/dataset/combined-wildfire-datasets-for-the-united-states-and-certain-territories-1800s-present" class="text-blue-400 hover:underline text-sm">Access Dataset →</a>
        </div>
        <div class="bg-gray-800 rounded-lg p-4">
          <h3 class="font-semibold mb-2 text-orange-400">USFS Fire Occurrence</h3>
          <p class="text-sm text-gray-300 mb-3">Point of occurrence data for fire ignition analysis</p>
          <a href="https://catalog.data.gov/dataset/national-usfs-fire-occurrence-point-feature-layer-d3233" class="text-blue-400 hover:underline text-sm">Access Dataset →</a>
        </div>
      </div>
    </div>

    <!-- Specialized Datasets -->
    <div class="mb-12">
      <h2 class="text-2xl font-semibold mb-6 border-b border-gray-700 pb-2">Specialized Datasets</h2>
      
      <div class="mb-8">
        <h3 class="text-xl font-semibold mb-4 text-orange-400">1. Satellite-Based Wildfire Detection</h3>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">NASA FIRMS</h4>
            <p class="text-sm text-gray-300 mb-2">Global fire detection from MODIS and VIIRS satellites</p>
            <p class="text-xs text-gray-400">Format: CSV, GeoTIFF, KML, JSON</p>
          </div>
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">Sentinel-2 Fire Dataset (ESA)</h4>
            <p class="text-sm text-gray-300 mb-2">High-resolution imagery with burned area segmentation</p>
            <p class="text-xs text-gray-400">Format: GeoTIFF</p>
          </div>
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">Landsat Burned Area Data</h4>
            <p class="text-sm text-gray-300 mb-2">Burned area mapping from Landsat 8</p>
            <p class="text-xs text-gray-400">Format: GeoTIFF</p>
          </div>
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">California Fire Perimeters</h4>
            <p class="text-sm text-gray-300 mb-2">Polygon shapefiles of historical fire perimeters</p>
            <p class="text-xs text-gray-400">Format: Shapefile, KML, GeoJSON</p>
          </div>
        </div>
      </div>
      
      <div class="mb-8">
        <h3 class="text-xl font-semibold mb-4 text-orange-400">2. Ground-Based & Aerial Fire Imagery</h3>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">ALERTWildfire Public Dataset</h4>
            <p class="text-sm text-gray-300 mb-2">High-resolution live fire detection camera feeds</p>
            <p class="text-xs text-gray-400">Use: Train ML models for early detection</p>
          </div>
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">FireNet Dataset (Kaggle)</h4>
            <p class="text-sm text-gray-300 mb-2">40,000+ annotated wildfire images</p>
            <p class="text-xs text-gray-400">Format: PNG/JPEG</p>
          </div>
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">Global Wildfire Smoke Detection Dataset</h4>
            <p class="text-sm text-gray-300 mb-2">High-resolution smoke images for ML training</p>
          </div>
        </div>
      </div>
      
      <div class="mb-8">
        <h3 class="text-xl font-semibold mb-4 text-orange-400">3. Fire Weather & Sensor Data</h3>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">NOAA Historical Fire Weather Data</h4>
            <p class="text-sm text-gray-300 mb-2">Wind, humidity, and temperature data related to historical fires</p>
            <p class="text-xs text-gray-400">Use: Train ML models to predict fire ignition likelihood</p>
          </div>
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">RAWS Weather Stations Data</h4>
            <p class="text-sm text-gray-300 mb-2">Live and historical weather sensor data from wildfire-prone regions</p>
            <p class="text-xs text-gray-400">Data: Wind speed, humidity, temperature, fuel moisture</p>
          </div>
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">UCI Wildfire Prediction Dataset</h4>
            <p class="text-sm text-gray-300 mb-2">Focused on predicting wildfire ignition events</p>
            <p class="text-xs text-gray-400">Data: Sensor and climate data, labeled by fire occurrences</p>
          </div>
        </div>
      </div>
      
      <div>
        <h3 class="text-xl font-semibold mb-4 text-orange-400">4. Fire Spread Simulation & Modeling</h3>
        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">FARSITE Fire Behavior Prediction Data</h4>
            <p class="text-sm text-gray-300 mb-2">Fire spread model data used in the FARSITE simulator</p>
            <p class="text-xs text-gray-400">Use: ML-based fire propagation simulation</p>
          </div>
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">GEOMAC Wildfire Perimeter Data</h4>
            <p class="text-sm text-gray-300 mb-2">Fire perimeters tracked over time using geospatial data</p>
            <p class="text-xs text-gray-400">Use: Training fire spread models with past real-world fire progression</p>
          </div>
          <div class="bg-gray-900 rounded-lg p-4">
            <h4 class="font-bold">FireCast (Global Fire Risk Prediction)</h4>
            <p class="text-sm text-gray-300 mb-2">AI-powered fire probability maps updated in near real-time</p>
            <p class="text-xs text-gray-400">Data: Fire likelihood based on climate, topography, and satellite</p>
          </div>
        </div>
      </div>
    </div>

    <!-- Mission Banner -->
    <div class="bg-gradient-to-r from-red-900 to-orange-800 rounded-lg p-8">
      <div class="flex items-center mb-4">
        <span class="text-2xl mr-3">🔥</span>
        <h2 class="text-2xl font-bold">Our Data Mission</h2>
      </div>
      <p class="text-lg">
        Pyre AI combines these diverse data sources through advanced machine learning algorithms to create a comprehensive wildfire prediction and monitoring system. By integrating satellite imagery, ground sensors, weather patterns, and historical data, we provide the most accurate and timely information to protect communities and natural resources.
      </p>
    </div>
  </div>
</div>