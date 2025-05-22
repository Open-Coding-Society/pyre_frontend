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
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.css" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet.heat/0.2.0/leaflet-heat.min.js"></script>

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
                    <select id="yearSelect" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                        <option value="2023">2023</option>
                        <option value="2024">2024</option>
                        <option value="2025">2025</option>
                    </select>
                </div>
                <div class="flex-1 min-w-200">
                    <label for="monthSelect" class="block text-sm font-medium text-gray-700 mb-2">Month</label>
                    <select id="monthSelect" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
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
                    <select id="mapType" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                        <option value="markers">Earthquake Locations</option>
                        <option value="heatmap">Heat Map</option>
                        <option value="magnitude">Magnitude Intensity</option>
                    </select>
                </div>
                <div class="flex-1 min-w-200">
                    <label class="block text-sm font-medium text-gray-700 mb-2">&nbsp;</label>
                    <button id="loadData" class="w-full bg-blue-600 text-white px-6 py-2 rounded-md shadow-sm hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 transition duration-200">
                        Load Data
                    </button>
                </div>
            </div>
        </div>
        <!-- Stats Cards -->
        <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
            <div class="bg-white rounded-lg shadow-sm p-6">
                <div class="flex items-center">
                    <div class="p-2 bg-red-100 rounded-lg">
                        <div class="w-6 h-6 bg-red-600 rounded"></div>
                    </div>
                    <div class="ml-4">
                        <h3 class="text-sm font-medium text-gray-500">Total Earthquakes</h3>
                        <p id="totalEarthquakes" class="text-2xl font-semibold text-gray-900">-</p>
                    </div>
                </div>
            </div>
            <div class="bg-white rounded-lg shadow-sm p-6">
                <div class="flex items-center">
                    <div class="p-2 bg-orange-100 rounded-lg">
                        <div class="w-6 h-6 bg-orange-600 rounded"></div>
                    </div>
                    <div class="ml-4">
                        <h3 class="text-sm font-medium text-gray-500">Avg Magnitude</h3>
                        <p id="avgMagnitude" class="text-2xl font-semibold text-gray-900">-</p>
                    </div>
                </div>
            </div>
            <div class="bg-white rounded-lg shadow-sm p-6">
                <div class="flex items-center">
                    <div class="p-2 bg-yellow-100 rounded-lg">
                        <div class="w-6 h-6 bg-yellow-600 rounded"></div>
                    </div>
                    <div class="ml-4">
                        <h3 class="text-sm font-medium text-gray-500">Avg Depth</h3>
                        <p id="avgDepth" class="text-2xl font-semibold text-gray-900">-</p>
                    </div>
                </div>
            </div>
            <div class="bg-white rounded-lg shadow-sm p-6">
                <div class="flex items-center">
                    <div class="p-2 bg-green-100 rounded-lg">
                        <div class="w-6 h-6 bg-green-600 rounded"></div>
                    </div>
                    <div class="ml-4">
                        <h3 class="text-sm font-medium text-gray-500">Major Quakes (5.0+)</h3>
                        <p id="majorQuakes" class="text-2xl font-semibold text-gray-900">-</p>
                    </div>
                </div>
            </div>
        </div>
        <!-- Loading Indicator -->
        <div id="loadingIndicator" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
            <div class="bg-white rounded-lg p-6 flex items-center space-x-3">
                <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-600"></div>
                <span class="text-gray-700">Loading earthquake data...</span>
            </div>
        </div>
        <!-- Main Content -->
        <div class="grid grid-cols-1 gap-8">
            <!-- Map -->
            <div class="bg-white rounded-lg shadow-md p-6">
                <h2 class="text-xl font-semibold text-gray-800 mb-4">Earthquake Locations</h2>
                <div id="map" class="h-96 rounded-lg border"></div>
            </div>
            <!-- Earthquake Count by Day -->
            <div class="bg-white rounded-lg shadow-md p-6">
                <h2 class="text-xl font-semibold text-gray-800 mb-4">Daily Earthquake Count</h2>
                <canvas id="earthquakeCountChart" class="w-full h-64"></canvas>
            </div>
            <!-- Magnitude Distribution -->
            <div class="bg-white rounded-lg shadow-md p-6">
                <h2 class="text-xl font-semibold text-gray-800 mb-4">Magnitude Distribution</h2>
                <canvas id="magnitudeChart" class="w-full h