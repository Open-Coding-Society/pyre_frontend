---
layout: base_chat
search_exclude: true
hide: true
show_reading_time: false
permalink: /earthquakesdashboard/
title: Earthquake Dashboard
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Seismic Monitor Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
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
        /* Hide zoom controls */
        .leaflet-control-zoom {
            display: none !important;
        }
        /* Adjust attribution z-index to prevent overlap with popups */
        .leaflet-control-attribution {
            z-index: 400 !important;
        }
        /* Ensure popups appear above attribution */
        .help-overlay, 
        .prediction-overlay {
            z-index: 1000;
        }
        .map-marker {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 2rem;
            height: 2rem;
            background: linear-gradient(to right, #3b82f6, #2563eb);
            color: white;
            border-radius: 50%;
            font-weight: bold;
            border: 2px solid white;
            box-shadow: 0 2px 4px rgba(0,0,0,0.3);
        }
        .earthquake-popup {
            background-color: rgba(17, 24, 39, 0.95);
            color: white;
            border: 1px solid #374151;
            border-radius: 0.375rem;
            padding: 0.5rem;
        }
        .earthquake-popup h3,
        .earthquake-popup p {
            color: white !important;
        }
        .earthquake-popup p span {
            color: white !important;
        }
        .earthquake-popup p span.magnitude-high,
        .earthquake-popup p span.magnitude-medium,
        .earthquake-popup p span.magnitude-low {
            font-weight: bold;
        }
        .magnitude-high { color: #ef4444; }
        .magnitude-medium { color: #f97316; }
        .magnitude-low { color: #3b82f6; }
        
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
            cursor: pointer;
        }
        .pulse {
            animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }
        
        .help-overlay {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: rgba(0, 0, 0, 0.7);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .help-popup {
            background-color: #1f2937;
            border: 1px solid #374151;
            border-radius: 0.5rem;
            max-width: 600px;
            width: 90%;
            max-height: 90vh;
            overflow-y: auto;
            padding: 1.5rem;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.5);
            position: relative;
        }
        .close-popup {
            position: absolute;
            top: 1rem;
            right: 1rem;
            background: none;
            border: none;
            color: #9ca3af;
            cursor: pointer;
            font-size: 1.5rem;
            line-height: 1;
        }
        .close-popup:hover { color: #f9fafb; }
        .help-feature {
            border-left: 2px solid #3b82f6;
            padding-left: 1rem;
            margin-bottom: 1rem;
        }
        .dashboard-title {
            background: linear-gradient(to right, #3b82f6, #2563eb);
            padding: 0.75rem 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .help-button {
            background-color: rgba(255, 255, 255, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            color: white;
            padding: 0.375rem 0.75rem;
            border-radius: 0.375rem;
            font-size: 0.875rem;
            cursor: pointer;
            display: flex;
            align-items: center;
            transition: all 0.2s;
        }
        .help-button:hover {
            background-color: rgba(255, 255, 255, 0.2);
        }
        .loading {
            opacity: 0.5;
            pointer-events: none;
        }
        .prediction-form {
            background-color: #374151;
            border-radius: 0.5rem;
            padding: 1rem;
            margin-bottom: 1rem;
        }
        .prediction-form label {
            color: white !important;
        }
        .prediction-form input {
            background-color: #1f2937;
            border: 1px solid #4b5563;
            color: white;
        }
        .prediction-form .text-sm {
            color: white !important;
        }
        .prediction-result {
            background-color: #1f2937;
            border: 1px solid #3b82f6;
            border-radius: 0.5rem;
            padding: 1rem;
            margin-top: 1rem;
        }
        .prediction-result h3 {
            color: white !important;
            font-size: 1.125rem;
            font-weight: bold;
            margin-bottom: 0.5rem;
        }
        .prediction-result p:not(.text-blue-400) {
            color: white !important;
        }
    </style>
</head>
<body class="min-h-screen bg-gray-950 text-gray-200">
    <!-- Dashboard title bar -->
    <div class="dashboard-title">
        <div class="flex items-center">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
            </svg>
            <h1 class="text-xl font-bold">Seismic Monitor Dashboard</h1>
        </div>
        <div class="flex space-x-2">
            <button id="prediction-button" class="help-button" type="button">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z" />
                </svg>
                ML Predict
            </button>
            <button id="help-button" class="help-button" type="button">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                </svg>
                Help
            </button>
        </div>
    </div>

    <!-- Help overlay and popup -->
    <div id="help-overlay" class="help-overlay" style="display: none;">
        <div class="help-popup" style="color: #fff;">
            <button id="close-popup" class="close-popup" type="button" style="color: #fff;">&times;</button>
            <h2 class="text-xl font-bold mb-4" style="color: #fff;">Seismic Monitor Dashboard - API Integration</h2>
            <p class="mb-4" style="color: #fff;">This dashboard is now fully integrated with your Flask backend API. Here's what's available:</p>
            
            <div class="space-y-4">
                <div class="help-feature" style="color: #fff;">
                    <h3 class="text-lg font-medium mb-2" style="color: #fff;">Real-time Data</h3>
                    <p style="color: #fff;">Live earthquake data fetched from your Flask API at <code>/api/earthquake/resource</code></p>
                </div>
                
                <div class="help-feature" style="color: #fff;">
                    <h3 class="text-lg font-medium mb-2" style="color: #fff;">ML Predictions</h3>
                    <p style="color: #fff;">Click "ML Predict" to use your trained model for earthquake magnitude prediction using the <code>/api/earthquake/predict</code> endpoint.</p>
                </div>
                
                <div class="help-feature" style="color: #fff;">
                    <h3 class="text-lg font-medium mb-2" style="color: #fff;">Risk Factor Analysis</h3>
                    <p style="color: #fff;">Automatic calculation of seismic intensity, ground acceleration, and liquefaction potential using <code>/api/earthquake/calculate-risk-factors</code></p>
                </div>
                
                <div class="help-feature" style="color: #fff;">
                    <h3 class="text-lg font-medium mb-2" style="color: #fff;">Data Management</h3>
                    <ul class="list-disc ml-5 mt-2 space-y-1" style="color: #fff;">
                        <li>Create new earthquake records</li>
                        <li>Update existing records</li>
                        <li>Delete records</li>
                        <li>Restore data from backups</li>
                    </ul>
                </div>
            </div>
        </div>
    </div>

    <!-- Prediction Modal -->
    <div id="prediction-overlay" class="help-overlay" style="display: none;">
        <div class="help-popup" style="color: #fff; max-width: 500px;">
            <button id="close-prediction" class="close-popup" type="button" style="color: #fff;">&times;</button>
            <h2 class="text-xl font-bold mb-4" style="color: #fff;">Earthquake Magnitude Prediction</h2>
            
            <div class="prediction-form">
                <div class="grid grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-medium mb-1">Latitude</label>
                        <input type="number" id="pred-latitude" step="0.00001" class="w-full p-2 rounded bg-gray-800 border border-gray-600 text-white" placeholder="40.7128">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Longitude</label>
                        <input type="number" id="pred-longitude" step="0.00001" class="w-full p-2 rounded bg-gray-800 border border-gray-600 text-white" placeholder="-74.0060">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Depth (km)</label>
                        <input type="number" id="pred-depth" step="0.1" class="w-full p-2 rounded bg-gray-800 border border-gray-600 text-white" placeholder="10.5">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Time of Day (0-23)</label>
                        <input type="number" id="pred-time" min="0" max="23" class="w-full p-2 rounded bg-gray-800 border border-gray-600 text-white" placeholder="14">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Previous Magnitude</label>
                        <input type="number" id="pred-prev-mag" step="0.1" class="w-full p-2 rounded bg-gray-800 border border-gray-600 text-white" placeholder="3.5">
                    </div>
                    <div>
                        <label class="block text-sm font-medium mb-1">Distance to Fault (km)</label>
                        <input type="number" id="pred-distance" step="0.1" class="w-full p-2 rounded bg-gray-800 border border-gray-600 text-white" placeholder="5.2">
                    </div>
                </div>
                
                <button id="predict-btn" class="w-full mt-4 bg-blue-600 hover:bg-blue-700 text-white py-2 px-4 rounded transition-colors">
                    Predict Magnitude
                </button>
            </div>
            
            <div id="prediction-result" class="prediction-result" style="display: none;">
                <h3 class="font-bold mb-2">Prediction Results</h3>
                <div id="prediction-content"></div>
            </div>
        </div>
    </div>

    <!-- Dashboard content -->
    <div class="flex h-screen overflow-hidden">
        <!-- Left sidebar -->
        <div class="w-72 bg-gray-900/50 border-r border-gray-800 p-4 overflow-y-auto">
            <h2 class="text-lg font-medium mb-4 text-white">Analytics Overview</h2>
            <div class="mb-6">
                <div class="text-sm text-gray-400 mb-1">Total Seismic Events</div>
                <div class="text-4xl font-bold" id="total-incidents">--</div>
                <div class="mt-4">
                    <h3 class="text-sm text-gray-400 mb-2">Magnitude Categories</h3>
                    <div id="category-stats" class="space-y-3">
                        <div class="animate-pulse">
                            <div class="flex justify-between mb-1">
                                <span class="bg-gray-700 h-4 w-24 rounded"></span>
                                <span class="bg-gray-700 h-4 w-8 rounded"></span>
                            </div>
                            <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
                                <div class="h-full bg-gray-700 rounded-full w-3/4"></div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- Depth Analysis Widget -->
            <div class="mb-6">
                <h3 class="text-sm text-gray-400 mb-3">Depth Analysis</h3>
                <div class="bg-gray-900/70 rounded-lg p-3">
                    <div class="flex items-center justify-between mb-2">
                        <div class="text-sm">Average Depth</div>
                        <div id="avg-depth" class="font-medium">-- km</div>
                    </div>
                    <div class="flex items-center justify-between mb-2">
                        <div class="text-sm">Deepest Event</div>
                        <div id="max-depth" class="font-medium">-- km</div>
                    </div>
                    <div class="flex items-center justify-between">
                        <div class="text-sm">Shallowest Event</div>
                        <div id="min-depth" class="font-medium">-- km</div>
                    </div>
                </div>
            </div>
            
            <!-- Seismic Activity Gauge -->
            <div class="mb-6">
                <h3 class="text-sm text-gray-400 mb-3">Latest Event</h3>
                <div class="bg-gray-900/70 rounded-lg p-3">
                    <div class="flex items-center justify-center h-32">
                        <div class="text-center">
                            <div id="current-magnitude" class="text-3xl font-bold text-blue-400">--</div>
                            <div class="text-sm text-gray-400">Magnitude</div>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- API Status -->
            <div class="mb-6">
                <h3 class="text-sm text-gray-400 mb-3">API Status</h3>
                <div class="bg-gray-900/70 rounded-lg p-3">
                    <div class="flex items-center justify-between mb-2">
                        <div class="text-sm">Connection</div>
                        <div id="api-status" class="font-medium text-green-400">Connected</div>
                    </div>
                    <div class="flex items-center justify-between">
                        <div class="text-sm">Last Sync</div>
                        <div id="last-sync" class="font-medium">--</div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Main content area with map -->
        <div class="flex-1 overflow-hidden flex flex-col">
            <div class="flex-1 p-4 overflow-hidden">
                <div class="bg-gray-900/50 rounded-lg overflow-hidden h-full relative border border-gray-800">
                    <div id="map" class="w-full h-full"></div>
                    <!-- Map controls -->
                    <div class="absolute top-4 right-4 flex space-x-2">
                        <button id="add-record-btn" class="bg-gray-200 bg-opacity-20 backdrop-blur-sm rounded-md px-3 py-1 text-sm text-gray-200 flex items-center hover:bg-opacity-30">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
                                <path fill-rule="evenodd" d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" />
                            </svg>
                            Add Record
                        </button>
                        <button id="toggle-fault-lines" class="bg-gray-200 bg-opacity-20 backdrop-blur-sm rounded-md px-3 py-1 text-sm text-gray-200 flex items-center hover:bg-opacity-30">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
                                <path fill-rule="evenodd" d="M12 2a1 1 0 01.967.744L14.146 7.2 17.5 9.134a1 1 0 010 1.732l-3.354 1.935-1.18 4.455a1 1 0 01-1.933 0L9.854 12.8 6.5 10.866a1 1 0 010-1.732l3.354-1.935 1.18-4.455A1 1 0 0112 2z" clip-rule="evenodd" />
                            </svg>
                            Fault Lines
                        </button>
                    </div>
                </div>
            </div>
            
            <!-- Bottom toolbar -->
            <div class="bg-black border-t border-gray-800 py-3 px-6 flex justify-between items-center">
                <div class="flex space-x-6"></div>
                <div class="flex items-center space-x-4">
                    <div class="text-sm text-gray-400">Last updated: <span id="last-updated">--</span></div>
                </div>
            </div>

            <!-- Hidden container for maintaining API data -->
            <div style="display: none;">
                <div id="incident-count">--</div>
                <div id="incidents-table-body"></div>
            </div>
        </div>
    </div>

    <script>
        // Configuration
        import { pythonURI, fetchOptions } from '/pyre_frontend/assets/js/api/config.js';
        const API_ENDPOINTS = {
            predict: `${pythonURI}/api/earthquake/predict`,
            records: `${pythonURI}/api/earthquake/records`,
            record: `${pythonURI}/api/earthquake/record`,
            riskFactors: `${pythonURI}/api/earthquake/calculate-risk-factors`,
            restore: `${pythonURI}/api/earthquake/restore`
        };

        // California boundaries
        const CA_BOUNDS = {
            north: 42.0,
            south: 32.5,
            west: -124.4,
            east: -114.1
        };
        
        // Global variables
        let map;
        let earthquakeData = [];
        let markers = [];
        
        // Initialize the application
        document.addEventListener('DOMContentLoaded', function() {
            initializeMap();
            initializeEventListeners();
            loadEarthquakeData();
            
            // Set up auto-refresh
            setInterval(loadEarthquakeData, 30000); // Refresh every 30 seconds
        });
        
        // Initialize Leaflet map
        function initializeMap() {
            map = L.map('map').setView([40.7128, -74.0060], 8);
            
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '© OpenStreetMap contributors'
            }).addTo(map);
        }
        
        // Initialize event listeners
        function initializeEventListeners() {
            // Help popup
            document.getElementById('help-button').addEventListener('click', () => {
                document.getElementById('help-overlay').style.display = 'flex';
            });
            document.getElementById('close-popup').addEventListener('click', () => {
                document.getElementById('help-overlay').style.display = 'none';
            });
            
            // Prediction popup
            document.getElementById('prediction-button').addEventListener('click', () => {
                document.getElementById('prediction-overlay').style.display = 'flex';
            });
            document.getElementById('close-prediction').addEventListener('click', () => {
                document.getElementById('prediction-overlay').style.display = 'none';
            });
            
            // Prediction form
            document.getElementById('predict-btn').addEventListener('click', handlePrediction);
            
            // Add record button
            document.getElementById('add-record-btn').addEventListener('click', handleAddRecord);
        }
        
        // Load earthquake data from API
        async function loadEarthquakeData() {
            try {
                updateApiStatus('loading');
                
                const response = await fetch(API_ENDPOINTS.records);
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                
                const data = await response.json();
                // Filter for California earthquakes only
                earthquakeData = Array.isArray(data) ? data.filter(quake => 
                    quake.latitude >= CA_BOUNDS.south && 
                    quake.latitude <= CA_BOUNDS.north && 
                    quake.longitude >= CA_BOUNDS.west && 
                    quake.longitude <= CA_BOUNDS.east
                ) : [];
                
                updateDashboard();
                updateMap();
                updateApiStatus('connected');
                
                document.getElementById('last-updated').textContent = new Date().toLocaleTimeString();
                document.getElementById('last-sync').textContent = new Date().toLocaleTimeString();
                
            } catch (error) {
                console.error('Error loading earthquake data:', error);
                updateApiStatus('error');
            }
        }

        // Update dashboard with earthquake data
        function updateDashboard() {
            if (earthquakeData.length > 0) {
                // Update total incidents (keeping this for API data consistency)
                document.getElementById('total-incidents').textContent = earthquakeData.length;
                document.getElementById('incident-count').textContent = earthquakeData.length;

                // Calculate magnitude categories
                const categories = {
                    'Major (7.0+)': earthquakeData.filter(q => q.magnitude >= 7.0).length,
                    'Strong (5.0-6.9)': earthquakeData.filter(q => q.magnitude >= 5.0 && q.magnitude < 7.0).length,
                    'Light (<5.0)': earthquakeData.filter(q => q.magnitude < 5.0).length
                };

                // Update category stats
                const statsContainer = document.getElementById('category-stats');
                statsContainer.innerHTML = '';
                
                Object.entries(categories).forEach(([category, count]) => {
                    const percentage = (count / earthquakeData.length * 100) || 0;
                    const categoryEl = document.createElement('div');
                    categoryEl.innerHTML = `
                        <div class="flex justify-between mb-1">
                            <span class="text-sm">${category}</span>
                            <span class="text-sm">${count}</span>
                        </div>
                        <div class="h-2 bg-gray-800 rounded-full overflow-hidden">
                            <div class="h-full bg-blue-500 rounded-full" style="width: ${percentage}%"></div>
                        </div>
                    `;
                    statsContainer.appendChild(categoryEl);
                });

                // Update depth analysis
                const depths = earthquakeData.map(quake => quake.depth);
                document.getElementById('avg-depth').textContent = 
                    (depths.reduce((a, b) => a + b, 0) / depths.length).toFixed(1) + ' km';
                document.getElementById('max-depth').textContent = 
                    Math.max(...depths).toFixed(1) + ' km';
                document.getElementById('min-depth').textContent = 
                    Math.min(...depths).toFixed(1) + ' km';
                
                // Update current magnitude (latest event)
                document.getElementById('current-magnitude').textContent = 
                    earthquakeData[0].magnitude.toFixed(1);

                // Keep updating the hidden table body for API data consistency
                const tableBody = document.getElementById('incidents-table-body');
                tableBody.innerHTML = earthquakeData.map(quake => `
                    <tr>
                        <td class="${getMagnitudeClass(quake.magnitude)}">${quake.magnitude.toFixed(1)}</td>
                        <td>${quake.depth.toFixed(1)} km</td>
                        <td>${quake.latitude.toFixed(2)}, ${quake.longitude.toFixed(2)}</td>
                    </tr>
                `).join('');
            }
        }

        function getMagnitudeClass(magnitude) {
            if (magnitude >= 7.0) return 'magnitude-high';
            if (magnitude >= 5.0) return 'magnitude-medium';
            return 'magnitude-low';
        }

        function updateApiStatus(status) {
            const statusEl = document.getElementById('api-status');
            switch (status) {
                case 'loading':
                    statusEl.textContent = 'Loading...';
                    statusEl.className = 'font-medium text-yellow-400';
                    break;
                case 'connected':
                    statusEl.textContent = 'Connected';
                    statusEl.className = 'font-medium text-green-400';
                    break;
                case 'error':
                    statusEl.textContent = 'Error';
                    statusEl.className = 'font-medium text-red-400';
                    break;
            }
        }

        // Handle prediction form submission
        async function handlePrediction() {
            const predictionData = {
                latitude: parseFloat(document.getElementById('pred-latitude').value),
                longitude: parseFloat(document.getElementById('pred-longitude').value),
                depth: parseFloat(document.getElementById('pred-depth').value),
                time_of_day: parseInt(document.getElementById('pred-time').value),
                previous_magnitude: parseFloat(document.getElementById('pred-prev-mag').value),
                distance_to_fault: parseFloat(document.getElementById('pred-distance').value)
            };

            try {
                const response = await fetch(API_ENDPOINTS.predict, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(predictionData)
                });

                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }

                const result = await response.json();
                
                // Show prediction result
                const predictionResult = document.getElementById('prediction-result');
                const predictionContent = document.getElementById('prediction-content');
                predictionContent.innerHTML = `
                    <p class="text-lg font-bold text-blue-400 mb-2">Predicted Magnitude: ${result.predicted_magnitude.toFixed(2)}</p>
                `;
                predictionResult.style.display = 'block';

            } catch (error) {
                console.error('Error making prediction:', error);
                alert('Failed to make prediction. Please try again.');
            }
        }

        // Handle adding a new record
        async function handleAddRecord() {
            const newRecord = {
                latitude: parseFloat(prompt('Enter latitude:', '40.7128')),
                longitude: parseFloat(prompt('Enter longitude:', '-74.0060')),
                depth: parseFloat(prompt('Enter depth (km):', '10')),
                time_of_day: parseInt(prompt('Enter time of day (0-23):', '12')),
                previous_magnitude: parseFloat(prompt('Enter previous magnitude:', '0')),
                distance_to_fault: parseFloat(prompt('Enter distance to fault (km):', '5')),
                plate_boundary_type: prompt('Enter plate boundary type:', 'Transform'),
                soil_type: prompt('Enter soil type:', 'Clay'),
                magnitude: parseFloat(prompt('Enter magnitude:', '3.5'))
            };

            try {
                const response = await fetch(API_ENDPOINTS.record, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(newRecord)
                });

                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }

                await loadEarthquakeData(); // Refresh the data
                alert('New earthquake record added successfully!');

            } catch (error) {
                console.error('Error adding new record:', error);
                alert('Failed to add new record. Please try again.');
            }
        }

        // Update map with markers
        function updateMap() {
            // Clear existing markers
            markers.forEach(marker => marker.remove());
            markers = [];

            // Add new markers
            earthquakeData.forEach(quake => {
                const magnitude = parseFloat(quake.magnitude);
                const markerSize = Math.max(20, magnitude * 8);
                const markerColor = magnitude >= 7.0 ? 'red' : magnitude >= 5.0 ? 'orange' : 'blue';

                const marker = L.divIcon({
                    className: 'map-marker',
                    html: `<div style="width: ${markerSize}px; height: ${markerSize}px; background: ${markerColor}; display: flex; align-items: center; justify-content: center; border-radius: 50%; border: 2px solid white;">${magnitude.toFixed(1)}</div>`,
                    iconSize: [markerSize, markerSize]
                });

                const newMarker = L.marker([quake.latitude, quake.longitude], { icon: marker })
                    .bindPopup(`
                        <div class="earthquake-popup">
                            <h3 class="font-bold mb-2">Earthquake Details</h3>
                            <p>Magnitude: <span class="${getMagnitudeClass(magnitude)}">${magnitude.toFixed(1)}</span></p>
                            <p>Depth: <span>${quake.depth} km</span></p>
                            <p>Location: <span>${quake.latitude.toFixed(4)}, ${quake.longitude.toFixed(4)}</span></p>
                            <p>Time of Day: <span>${quake.time_of_day}:00</span></p>
                            <p>Soil Type: <span>${quake.soil_type}</span></p>
                            <p>Plate Boundary: <span>${quake.plate_boundary_type}</span></p>
                        </div>
                    `);

                newMarker.addTo(map);
                markers.push(newMarker);
            });

            // Update map view if there are earthquakes
            if (earthquakeData.length > 0) {
                const bounds = L.latLngBounds(earthquakeData.map(quake => [quake.latitude, quake.longitude]));
                map.fitBounds(bounds, { padding: [50, 50] });
            }
        }
    </script>
    <a href="/pyre_frontend/help/" class="fixed bottom-4 right-4 bg-green-600 text-white rounded-full p-3 shadow-lg hover:bg-green-700 transition duration-200 flex items-center justify-center" title="Help Center" style="font-size:1.05em;">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9 5.25h.008v.008H12v-.008z"/>
      </svg>
      <span class="ml-1 font-medium">Help</span>
    </a>
</body>
</html>