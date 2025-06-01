---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /historical/
title: Historical
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
            <h1 class="text-4xl font-bold text-white mb-2">Historical Fire Data Visualization Dashboard</h1>
            <p class="text-slate-600">Explore fire incidents from 2015-2022</p>
        </div>
        <!-- Controls -->
        <div class="bg-white rounded-lg shadow-md p-6 mb-8">
            <div class="flex flex-wrap items-center gap-4">
                <div class="flex-1 min-w-200">
                    <label for="yearSelect" class="block text-sm font-medium text-gray-700 mb-2">Year</label>
                    <select id="yearSelect" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500">
                        <option value="2015">2015</option>
                        <option value="2016">2016</option>
                        <option value="2017">2017</option>
                        <option value="2018">2018</option>
                        <option value="2019">2019</option>
                        <option value="2020">2020</option>
                        <option value="2021">2021</option>
                        <option value="2022">2022</option>
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
                        <option value="markers">Fire Locations</option>
                        <option value="heatmap">Heat Map</option>
                        <option value="frp">FRP Intensity</option>
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
                        <h3 class="text-sm font-medium text-gray-500">Total Fires</h3>
                        <p id="totalFires" class="text-2xl font-semibold text-gray-900">-</p>
                    </div>
                </div>
            </div>
            <div class="bg-white rounded-lg shadow-sm p-6">
                <div class="flex items-center">
                    <div class="p-2 bg-orange-100 rounded-lg">
                        <div class="w-6 h-6 bg-orange-600 rounded"></div>
                    </div>
                    <div class="ml-4">
                        <h3 class="text-sm font-medium text-gray-500">Avg Brightness</h3>
                        <p id="avgBrightness" class="text-2xl font-semibold text-gray-900">-</p>
                    </div>
                </div>
            </div>
            <div class="bg-white rounded-lg shadow-sm p-6">
                <div class="flex items-center">
                    <div class="p-2 bg-yellow-100 rounded-lg">
                        <div class="w-6 h-6 bg-yellow-600 rounded"></div>
                    </div>
                    <div class="ml-4">
                        <h3 class="text-sm font-medium text-gray-500">Avg FRP</h3>
                        <p id="avgFRP" class="text-2xl font-semibold text-gray-900">-</p>
                    </div>
                </div>
            </div>
            <div class="bg-white rounded-lg shadow-sm p-6">
                <div class="flex items-center">
                    <div class="p-2 bg-green-100 rounded-lg">
                        <div class="w-6 h-6 bg-green-600 rounded"></div>
                    </div>
                    <div class="ml-4">
                        <h3 class="text-sm font-medium text-gray-500">High Confidence</h3>
                        <p id="highConfidence" class="text-2xl font-semibold text-gray-900">-</p>
                    </div>
                </div>
            </div>
        </div>
        <!-- Loading Indicator -->
        <div id="loadingIndicator" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
            <div class="bg-white rounded-lg p-6 flex items-center space-x-3">
                <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-blue-600"></div>
                <span class="text-gray-700">Loading fire data...</span>
            </div>
        </div>
        <!-- Main Content -->
        <div class="grid grid-cols-1 gap-8">
            <!-- Map -->
            <div class="bg-white rounded-lg shadow-md p-6">
                <h2 class="text-xl font-semibold text-gray-800 mb-4">Fire Locations</h2>
                <div id="map" class="h-96 rounded-lg border"></div>
            </div>
            <!-- Fire Count by Day -->
            <div class="bg-white rounded-lg shadow-md p-6">
                <h2 class="text-xl font-semibold text-gray-800 mb-4">Daily Fire Count</h2>
                <canvas id="fireCountChart" class="w-full h-64"></canvas>
            </div>
            <!-- Brightness Distribution -->
            <div class="bg-white rounded-lg shadow-md p-6">
                <h2 class="text-xl font-semibold text-gray-800 mb-4">Brightness Distribution</h2>
                <canvas id="brightnessChart" class="w-full h-64"></canvas>
            </div>
            <!-- FRP vs Brightness Correlation -->
            <div class="bg-white rounded-lg shadow-md p-6">
                <h2 class="text-xl font-semibold text-gray-800 mb-4">FRP vs Brightness Correlation</h2>
                <canvas id="correlationChart" class="w-full h-64"></canvas>
            </div>
        </div>
        <!-- Advanced Regression Analysis -->
        <div class="mt-12">
            <h2 class="text-3xl font-bold text-white mb-6 text-center">Advanced Regression Analysis</h2>
            <!-- Advanced Controls -->
            <div class="bg-white rounded-lg shadow-md p-6 mb-8">
                <div class="flex flex-wrap items-center gap-4">
                    <div class="flex-1 min-w-200">
                        <label for="analysisType" class="block text-sm font-medium text-gray-700 mb-2">Analysis Type</label>
                        <select id="analysisType" class="w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:outline-none focus:ring-2 focus:ring-purple-500 focus:border-purple-500">
                            <option value="comprehensive">Comprehensive Analysis</option>
                            <option value="time_series">Time Series Forecasting</option>
                            <option value="spatial">Spatial Clustering</option>
                            <option value="statistics">Statistical Summary</option>
                            <option value="animation">Animated Visualization</option>
                        </select>
                    </div>
                    <div class="flex-1 min-w-200">
                        <label class="block text-sm font-medium text-gray-700 mb-2">&nbsp;</label>
                        <button id="loadAdvancedData" class="w-full bg-purple-600 text-white px-6 py-2 rounded-md shadow-sm hover:bg-purple-700 focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-offset-2 transition duration-200">
                            Run Advanced Analysis
                        </button>
                    </div>
                </div>
            </div>
            <!-- Advanced Stats Cards -->
            <div id="advancedStatsSection" class="hidden grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
                <!-- Stats will be populated dynamically -->
            </div>
            <!-- Advanced Visualizations -->
            <div id="advancedVisualizationsSection" class="hidden grid grid-cols-1 gap-8">
                <!-- Time Series Forecast Chart -->
                <div id="forecastChartContainer" class="bg-white rounded-lg shadow-md p-6">
                    <h3 class="text-xl font-semibold text-gray-800 mb-4">Time Series Forecast</h3>
                    <div id="forecastChart" class="w-full h-96 border rounded-lg"></div>
                </div>
                <!-- Spatial Analysis -->
                <div id="spatialAnalysisContainer" class="bg-white rounded-lg shadow-md p-6">
                    <h3 class="text-xl font-semibold text-gray-800 mb-4">Spatial Clustering Analysis</h3>
                    <div id="spatialChart" class="w-full h-96 border rounded-lg"></div>
                </div>
                <!-- Statistical Insights -->
                <div id="statisticalInsightsContainer" class="bg-white rounded-lg shadow-md p-6">
                    <h3 class="text-xl font-semibold text-gray-800 mb-4">Statistical Insights</h3>
                    <div id="statisticalContent" class="space-y-4"></div>
                </div>
                <!-- Animation Container -->
                <div id="animationContainer" class="bg-white rounded-lg shadow-md p-6">
                    <h3 class="text-xl font-semibold text-gray-800 mb-4">Dynamic Visualization</h3>
                    <div id="animationContent" class="w-full h-96 border rounded-lg flex items-center justify-center">
                        <p class="text-gray-500">Select animation analysis to view dynamic visualizations</p>
                    </div>
                </div>
            </div>
            <!-- Advanced Loading Indicator -->
            <div id="advancedLoadingIndicator" class="hidden fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50">
                <div class="bg-white rounded-lg p-6 flex items-center space-x-3">
                    <div class="animate-spin rounded-full h-8 w-8 border-b-2 border-purple-600"></div>
                    <span class="text-gray-700">Running advanced analysis...</span>
                </div>
            </div>
        </div>
        <!-- Error Message -->
        <div id="errorMessage" class="hidden mt-4 p-4 bg-red-50 border border-red-200 rounded-lg">
            <div class="flex">
                <div class="ml-3">
                    <h3 class="text-sm font-medium text-red-800">Error loading data</h3>
                    <p id="errorText" class="text-sm text-red-700 mt-1"></p>
                </div>
            </div>
        </div>
        <!-- Lesson Button -->
        <a href="/QcommVNE_Frontend/datascience/" class="fixed bottom-6 right-6 bg-blue-600 text-white rounded-full p-4 shadow-lg hover:bg-blue-700 transition duration-200 flex items-center justify-center" title="Learn about Data Science & ML">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"/>
          </svg>
          <span class="ml-2 font-medium">Learning Guide</span>
        </a>
</div>

<script type="module">
        import { pythonURI, fetchOptions } from '/QcommVNE_Frontend/assets/js/api/config.js';

        // ----------------------------------------------------------------
        // INITIAL MAPS, METRICS & DISPLAY W/ LISTENER SETUP
        // ----------------------------------------------------------------

        // Global variables
        let map;
        let currentData = [];
        let charts = {};
        let markers = [];
        let heatLayer;

        // Initialize the dashboard
        document.addEventListener('DOMContentLoaded', function() {
            initializeMap();
            initializeCharts();
            setupEventListeners();
            
            // Load initial data
            loadData();
        });

        // Initialize the map
        function initializeMap() {
            map = L.map('map').setView([39.8283, -98.5795], 4); // Center on USA
            
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '© OpenStreetMap contributors'
            }).addTo(map);
        }

        // Initialize charts
        function initializeCharts() {
            // Fire Count Chart
            const fireCountCtx = document.getElementById('fireCountChart').getContext('2d');
            charts.fireCount = new Chart(fireCountCtx, {
                type: 'line',
                data: {
                    labels: [],
                    datasets: [{
                        label: 'Daily Fire Count',
                        data: [],
                        borderColor: 'rgb(239, 68, 68)',
                        backgroundColor: 'rgba(239, 68, 68, 0.1)',
                        tension: 0.1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });

            // Brightness Chart
            const brightnessCtx = document.getElementById('brightnessChart').getContext('2d');
            charts.brightness = new Chart(brightnessCtx, {
                type: 'bar',
                data: {
                    labels: [],
                    datasets: [{
                        label: 'Fire Count',
                        data: [],
                        backgroundColor: 'rgba(251, 146, 60, 0.8)',
                        borderColor: 'rgb(251, 146, 60)',
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            display: false
                        }
                    },
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });

            // Correlation Chart
            const correlationCtx = document.getElementById('correlationChart').getContext('2d');
            charts.correlation = new Chart(correlationCtx, {
                type: 'scatter',
                data: {
                    datasets: [{
                        label: 'FRP vs Brightness',
                        data: [],
                        backgroundColor: 'rgba(59, 130, 246, 0.6)',
                        borderColor: 'rgb(59, 130, 246)',
                        pointRadius: 3
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: {
                            title: {
                                display: true,
                                text: 'Brightness'
                            },
                            min: 250,
                            max: 500
                        },
                        y: {
                            title: {
                                display: true,
                                text: 'FRP'
                            },
                            min: 0,
                            max: 100
                        }
                    }
                }
            });
        }

        // Setup event listeners
        function setupEventListeners() {
            document.getElementById('loadData').addEventListener('click', loadData);
            document.getElementById('mapType').addEventListener('change', updateMapVisualization);

            // advanced regression (prophet) ML model listener
            setupAdvancedEventListeners();
        }

        // Load data from API
        async function loadData() {
            const year = document.getElementById('yearSelect').value;
            const month = document.getElementById('monthSelect').value;
            
            showLoading(true);
            hideError();

            try {
                const response = await fetch(`${pythonURI}/get-historical-data?year=${year}&month=${month}`);
                
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                
                const data = await response.json();
                currentData = data;
                
                updateStats(data);
                updateCharts(data);
                updateMapVisualization();
                
            } catch (error) {
                console.error('Error loading data:', error);
                showError(`Failed to load data: ${error.message}`);
            } finally {
                showLoading(false);
            }
        }

        // Update statistics cards
        function updateStats(data) {
            document.getElementById('totalFires').textContent = data.length.toLocaleString();
            
            if (data.length > 0) {
                const avgBrightness = data.reduce((sum, item) => sum + (item.brightness || 0), 0) / data.length;
                const avgFRP = data.reduce((sum, item) => sum + (item.frp || 0), 0) / data.length;
                const highConf = data.filter(item => item.confidence >= 80).length;
                
                document.getElementById('avgBrightness').textContent = Math.round(avgBrightness);
                document.getElementById('avgFRP').textContent = avgFRP.toFixed(1);
                document.getElementById('highConfidence').textContent = `${((highConf / data.length) * 100).toFixed(1)}%`;
            }
        }

        // Update charts with new data
        function updateCharts(data) {
            // Daily fire count
            const dailyCounts = {};
            data.forEach(item => {
                const date = new Date(item.acq_date).getDate();
                dailyCounts[date] = (dailyCounts[date] || 0) + 1;
            });
            
            const days = Object.keys(dailyCounts).sort((a, b) => a - b);
            charts.fireCount.data.labels = days;
            charts.fireCount.data.datasets[0].data = days.map(day => dailyCounts[day]);
            charts.fireCount.update();

            // Brightness distribution
            const brightnessRanges = {
                '250-300': 0, '300-350': 0, '350-400': 0, '400-450': 0, '450+': 0
            };
            
            data.forEach(item => {
                const brightness = item.brightness || 0;
                if (brightness < 300) brightnessRanges['250-300']++;
                else if (brightness < 350) brightnessRanges['300-350']++;
                else if (brightness < 400) brightnessRanges['350-400']++;
                else if (brightness < 450) brightnessRanges['400-450']++;
                else brightnessRanges['450+']++;
            });

            charts.brightness.data.labels = Object.keys(brightnessRanges);
            charts.brightness.data.datasets[0].data = Object.values(brightnessRanges);
            charts.brightness.update();

            // FRP vs Brightness correlation
            const correlationData = data
                .filter(item => item.frp && item.brightness)
                .map(item => ({
                    x: item.brightness,
                    y: item.frp
                }));
            
            charts.correlation.data.datasets[0].data = correlationData.slice(0, 1000); // Limit for performance
            charts.correlation.update();
        }

        // Update map visualization
        function updateMapVisualization() {
            const mapType = document.getElementById('mapType').value;
            
            // Clear existing layers
            clearMapLayers();
            
            if (currentData.length === 0) return;

            switch (mapType) {
                case 'markers':
                    showFireMarkers();
                    break;
                case 'heatmap':
                    showHeatMap();
                    break;
                case 'frp':
                    showFRPMarkers();
                    break;
            }
        }

        // Clear all map layers
        function clearMapLayers() {
            markers.forEach(marker => map.removeLayer(marker));
            markers = [];
            
            if (heatLayer) {
                map.removeLayer(heatLayer);
                heatLayer = null;
            }
        }

        // Show fire markers
        function showFireMarkers() {
            currentData.forEach(item => {
                if (item.latitude && item.longitude) {
                    const marker = L.circleMarker([item.latitude, item.longitude], {
                        radius: 5,
                        fillColor: getFireColor(item.confidence),
                        color: '#000',
                        weight: 1,
                        opacity: 1,
                        fillOpacity: 0.8
                    }).addTo(map);
                    
                    marker.bindPopup(`
                        <strong>Fire Detection</strong><br>
                        Date: ${new Date(item.acq_date).toLocaleDateString()}<br>
                        Brightness: ${item.brightness}<br>
                        FRP: ${item.frp}<br>
                        Confidence: ${item.confidence}%
                    `);
                    
                    markers.push(marker);
                }
            });
        }

        // Show heat map
        function showHeatMap() {
            const heatData = currentData
                .filter(item => item.latitude && item.longitude)
                .map(item => [item.latitude, item.longitude, item.frp || 1]);
            
            heatLayer = L.heatLayer(heatData, {
                radius: 20,
                blur: 15,
                maxZoom: 17
            }).addTo(map);
        }

        // Show FRP intensity markers
        function showFRPMarkers() {
            currentData.forEach(item => {
                if (item.latitude && item.longitude && item.frp) {
                    const radius = Math.max(3, Math.min(20, item.frp / 5));
                    const marker = L.circleMarker([item.latitude, item.longitude], {
                        radius: radius,
                        fillColor: getFRPColor(item.frp),
                        color: '#000',
                        weight: 1,
                        opacity: 1,
                        fillOpacity: 0.7
                    }).addTo(map);
                    
                    marker.bindPopup(`
                        <strong>Fire Detection</strong><br>
                        Date: ${new Date(item.acq_date).toLocaleDateString()}<br>
                        FRP: ${item.frp} MW<br>
                        Brightness: ${item.brightness}<br>
                        Confidence: ${item.confidence}%
                    `);
                    
                    markers.push(marker);
                }
            });
        }

        // Get color based on fire confidence
        function getFireColor(confidence) {
            if (confidence >= 80) return '#dc2626'; // red-600
            if (confidence >= 60) return '#ea580c'; // orange-600
            if (confidence >= 40) return '#facc15'; // yellow-400
            return '#84cc16'; // lime-500
        }

        // Get color based on FRP value
        function getFRPColor(frp) {
            if (frp >= 100) return '#7f1d1d'; // red-900
            if (frp >= 50) return '#dc2626'; // red-600
            if (frp >= 25) return '#ea580c'; // orange-600
            if (frp >= 10) return '#f97316'; // orange-500
            return '#fbbf24'; // amber-400
        }

        // Show/hide loading indicator
        function showLoading(show) {
            const indicator = document.getElementById('loadingIndicator');
            indicator.classList.toggle('hidden', !show);
        }

        // Show error message
        function showError(message) {
            document.getElementById('errorText').textContent = message;
            document.getElementById('errorMessage').classList.remove('hidden');
        }

        // Hide error message
        function hideError() {
            document.getElementById('errorMessage').classList.add('hidden');
        }
        
        // ----------------------------------------------------------------
        // ADVANCED ML MODEL CALL & DISPLAY
        // ----------------------------------------------------------------
        let advancedData = null;

        function getMonthName(monthNum) {
            if (!monthNum) return 'N/A';
            const months = [
                'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
                'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'
            ];
            return months[monthNum - 1] || 'N/A';
        }

        // Setup advanced analysis event listeners
        function setupAdvancedEventListeners() {
            document.getElementById('loadAdvancedData').addEventListener('click', loadAdvancedAnalysis);
        }

        // Load advanced analysis data
        async function loadAdvancedAnalysis() {
            const year = document.getElementById('yearSelect').value;
            const month = document.getElementById('monthSelect').value;
            const analysisType = document.getElementById('analysisType').value;
            
            showAdvancedLoading(true);
            hideAdvancedError();

            try {
                const requestBody = {
                    year: parseInt(year),
                    month: parseInt(month),
                    analysis_type: analysisType
                };

                const response = await fetch(`${pythonURI}/api/historical-fire/advanced/analyze`, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(requestBody)
                });
                
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }
                
                const data = await response.json();
                console.log(data)
                advancedData = data;
                
                displayAdvancedResults(data, analysisType);
                
            } catch (error) {
                console.error('Error loading advanced analysis:', error);
                showAdvancedError(`Failed to load advanced analysis: ${error.message}`);
            } finally {
                showAdvancedLoading(false);
            }
        }

        // Display advanced analysis results
        function displayAdvancedResults(data, analysisType) {
            // Show the sections
            document.getElementById('advancedStatsSection').classList.remove('hidden');
            document.getElementById('advancedVisualizationsSection').classList.remove('hidden');
            
            // Update advanced stats
            updateAdvancedStats(data, analysisType);
            
            // Display visualizations based on analysis type
            switch(analysisType) {
                case 'comprehensive':
                    displayComprehensiveAnalysis(data);
                    break;
                case 'time_series':
                    displayTimeSeriesAnalysis(data);
                    break;
                case 'spatial':
                    displaySpatialAnalysis(data);
                    break;
                case 'statistics':
                    displayStatisticalAnalysis(data);
                    break;
                case 'animation':
                    displayAnimationAnalysis(data);
                    break;
            }
        }

        // Update advanced statistics cards
        function updateAdvancedStats(data, analysisType) {
            const statsSection = document.getElementById('advancedStatsSection');
            
            let statsHtml = '';
            
            if (analysisType === 'comprehensive') {
                // For comprehensive analysis, extract stats from different sections
                const timeSeriesStats = data.time_series?.summary || {};
                const spatialStats = data.spatial?.summary || {};
                const animationStats = data.animation?.summary || {};
                
                statsHtml = `
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-purple-100 rounded-lg">
                                <div class="w-6 h-6 bg-purple-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Max Fires</h3>
                                <p class="text-2xl font-semibold text-gray-900">${animationStats.max_fires || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-indigo-100 rounded-lg">
                                <div class="w-6 h-6 bg-indigo-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Min Fires</h3>
                                <p class="text-2xl font-semibold text-gray-900">${animationStats.min_fires || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-blue-100 rounded-lg">
                                <div class="w-6 h-6 bg-blue-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Clusters Found</h3>
                                <p class="text-2xl font-semibold text-gray-900">${spatialStats.n_clusters || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-teal-100 rounded-lg">
                                <div class="w-6 h-6 bg-teal-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Total Frames</h3>
                                <p class="text-2xl font-semibold text-gray-900">${animationStats.total_frames || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                `;
            } else if (analysisType === 'time_series') {
                const stats = data.time_series?.summary || {};
                statsHtml = `
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-purple-100 rounded-lg">
                                <div class="w-6 h-6 bg-purple-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Status</h3>
                                <p class="text-2xl font-semibold text-gray-900">${data.time_series?.status || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                `;
            } else if (analysisType === 'spatial') {
                const stats = data.spatial?.summary || {};
                statsHtml = `
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-blue-100 rounded-lg">
                                <div class="w-6 h-6 bg-blue-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Target Cluster</h3>
                                <p class="text-2xl font-semibold text-gray-900">${stats.target_cluster || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-indigo-100 rounded-lg">
                                <div class="w-6 h-6 bg-indigo-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Clusters</h3>
                                <p class="text-2xl font-semibold text-gray-900">${stats.n_clusters || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-green-100 rounded-lg">
                                <div class="w-6 h-6 bg-green-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Total Fires</h3>
                                <p class="text-2xl font-semibold text-gray-900">${stats.total_fires || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                `;
            } else if (analysisType === 'statistics') {
                // For statistics, we don't have specific stats in the provided data
                statsHtml = `
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-purple-100 rounded-lg">
                                <div class="w-6 h-6 bg-purple-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Analysis Status</h3>
                                <p class="text-2xl font-semibold text-gray-900">${data.statistics?.status || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                `;
            } else if (analysisType === 'animation') {
                const stats = data.animation?.summary || {};
                statsHtml = `
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-purple-100 rounded-lg">
                                <div class="w-6 h-6 bg-purple-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Max Fires</h3>
                                <p class="text-2xl font-semibold text-gray-900">${stats.max_fires || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-indigo-100 rounded-lg">
                                <div class="w-6 h-6 bg-indigo-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Min Fires</h3>
                                <p class="text-2xl font-semibold text-gray-900">${stats.min_fires || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                    <div class="bg-white rounded-lg shadow-sm p-6">
                        <div class="flex items-center">
                            <div class="p-2 bg-blue-100 rounded-lg">
                                <div class="w-6 h-6 bg-blue-600 rounded"></div>
                            </div>
                            <div class="ml-4">
                                <h3 class="text-sm font-medium text-gray-500">Total Frames</h3>
                                <p class="text-2xl font-semibold text-gray-900">${stats.total_frames || 'N/A'}</p>
                            </div>
                        </div>
                    </div>
                `;
            }
            
            statsSection.innerHTML = statsHtml;
        }

        // Display comprehensive analysis - FIXED VERSION
        function displayComprehensiveAnalysis(data) {
            // Clear existing content first
            clearAdvancedVisualizationContainers();
            
            // Show all containers
            showAllAdvancedContainers();
            
            // Display time series plots
            if (data.time_series?.plots) {
                console.log('Displaying time series plots:', data.time_series.plots);
                displayTimeSeriesPlots(data.time_series.plots);
            } else {
                // Show fallback message for time series
                const container = document.getElementById('forecastChart');
                container.innerHTML = '<p class="text-gray-500 text-center py-8">No time series data available for this selection</p>';
            }
            
            // Display spatial clustering plots
            if (data.spatial?.plots?.clusters) {
                console.log('Displaying spatial plots:', data.spatial.plots.clusters);
                displaySpatialPlots(data.spatial.plots.clusters);
            } else {
                // Show fallback message for spatial
                const container = document.getElementById('spatialChart');
                container.innerHTML = '<p class="text-gray-500 text-center py-8">No spatial clustering data available for this selection</p>';
            }
            
            // Display animation data
            if (data.animation?.animation_data) {
                displayAnimationFromData(data.animation.animation_data);
            }
            
            // Display statistical analysis
            if (data.statistics) {
                displayStatisticalAnalysis(data);
            }
        }

        // Helper function to clear visualization containers
        function clearAdvancedVisualizationContainers() {
            const containers = ['forecastChart', 'spatialChart', 'statisticalContent', 'animationContent'];
            containers.forEach(containerId => {
                const container = document.getElementById(containerId);
                if (container) {
                    container.innerHTML = '';
                }
            });
        }

        // Helper function to show all advanced containers
        function showAllAdvancedContainers() {
            const containers = ['forecastChartContainer', 'spatialAnalysisContainer', 'statisticalInsightsContainer', 'animationContainer'];
            containers.forEach(containerId => {
                const container = document.getElementById(containerId);
                if (container) {
                    container.style.display = 'block';
                }
            });
        }

        // Display time series analysis
        function displayTimeSeriesAnalysis(data) {
            clearAdvancedVisualizationContainers();
            if (data.time_series?.plots) {
                displayTimeSeriesPlots(data.time_series.plots);
            }
        }

        // Display spatial analysis
        function displaySpatialAnalysis(data) {
            clearAdvancedVisualizationContainers();
            if (data.spatial?.plots?.clusters) {
                displaySpatialPlots(data.spatial.plots.clusters);
            }
            
            if (data.spatial?.cluster_data) {
                displayClusteringResults(data.spatial.cluster_data);
            }
        }

        // Display statistical analysis
        function displayStatisticalAnalysis(data) {
            const container = document.getElementById('statisticalContent');
            
            if (data.statistics) {
                // Check if there are CSV data or plots available
                const hasCSVData = data.statistics.csv_data && data.statistics.csv_data.length > 0;
                const hasPlots = data.statistics.plots && Object.keys(data.statistics.plots).length > 0;
                
                let content = `
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-2">Analysis Status</h4>
                            <ul class="space-y-1 text-sm text-gray-600">
                                <li>Status: ${data.statistics.status || 'N/A'}</li>
                                <li>CSV Data Available: ${hasCSVData ? 'Yes' : 'No'}</li>
                                <li>Plots Available: ${hasPlots ? 'Yes' : 'No'}</li>
                            </ul>
                        </div>
                `;
                
                if (data.statistics.summary) {
                    const summary = data.statistics.summary;
                    content += `
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-2">Summary Statistics</h4>
                            <ul class="space-y-1 text-sm text-gray-600">
                                ${Object.entries(summary).map(([key, value]) => 
                                    `<li>${key.replace(/_/g, ' ').replace(/\b\w/g, l => l.toUpperCase())}: ${value}</li>`
                                ).join('')}
                            </ul>
                        </div>
                    `;
                } else {
                    content += `
                        <div class="bg-gray-50 p-4 rounded-lg">
                            <h4 class="font-semibold text-gray-800 mb-2">Data Insights</h4>
                            <p class="text-sm text-gray-600">Statistical analysis completed successfully.</p>
                        </div>
                    `;
                }
                
                content += '</div>';
                container.innerHTML = content;
                
                // Display any plots if available
                if (hasPlots) {
                    displayStatisticalPlots(data.statistics.plots);
                }
            }
        }

        // Display animation analysis
        function displayAnimationAnalysis(data) {
            if (data.animation?.animation_data) {
                displayAnimationFromData(data.animation.animation_data);
            }
        }

        // NEW: Specific function to display time series plots
        function displayTimeSeriesPlots(plots) {
            const container = document.getElementById('forecastChart');
            
            // Handle different plot formats
            if (typeof plots === 'string') {
                // Single base64 image
                const imgSrc = plots.startsWith('data:image') ? plots : `data:image/png;base64,${plots}`;
                container.innerHTML = `
                    <div class="w-full h-full flex items-center justify-center">
                        <img src="${imgSrc}" alt="Time Series Forecast" class="max-w-full max-h-full object-contain rounded-lg" />
                    </div>
                `;
            } else if (typeof plots === 'object' && plots !== null) {
                // Multiple plots object
                const plotKeys = Object.keys(plots);
                if (plotKeys.length > 0) {
                    let plotsHtml = '<div class="space-y-4">';
                    
                    plotKeys.forEach(plotKey => {
                        const plotData = plots[plotKey];
                        const imgSrc = typeof plotData === 'string' 
                            ? (plotData.startsWith('data:image') ? plotData : `data:image/png;base64,${plotData}`)
                            : null;
                        
                        if (imgSrc) {
                            plotsHtml += `
                                <div class="text-center">
                                    <h4 class="text-sm font-medium text-gray-700 mb-2">${plotKey.replace(/_/g, ' ').replace(/\b\w/g, l => l.toUpperCase())}</h4>
                                    <img src="${imgSrc}" alt="${plotKey}" class="max-w-full h-auto rounded-lg mx-auto" style="max-height: 400px;" />
                                </div>
                            `;
                        }
                    });
                    
                    plotsHtml += '</div>';
                    container.innerHTML = plotsHtml;
                } else {
                    container.innerHTML = '<p class="text-gray-500 text-center py-8">No time series plots available</p>';
                }
            } else {
                container.innerHTML = '<p class="text-gray-500 text-center py-8">Invalid time series plot data format</p>';
            }
        }

        // NEW: Specific function to display spatial plots with larger container
        function displaySpatialPlots(plots) {
            const container = document.getElementById('spatialChart');
            
            // Make the container larger for spatial plots
            container.parentElement.style.minHeight = '600px';
            
            // Handle different plot formats
            if (typeof plots === 'string') {
                // Single base64 image
                const imgSrc = plots.startsWith('data:image') ? plots : `data:image/png;base64,${plots}`;
                container.innerHTML = `
                    <div class="w-full h-full flex items-center justify-center" style="min-height: 500px;">
                        <img src="${imgSrc}" alt="Spatial Clustering" class="max-w-full max-h-full object-contain rounded-lg" style="min-height: 400px;" />
                    </div>
                `;
            } else if (typeof plots === 'object' && plots !== null) {
                // Multiple plots object
                const plotKeys = Object.keys(plots);
                if (plotKeys.length > 0) {
                    let plotsHtml = '<div class="space-y-6">';
                    
                    plotKeys.forEach(plotKey => {
                        const plotData = plots[plotKey];
                        const imgSrc = typeof plotData === 'string' 
                            ? (plotData.startsWith('data:image') ? plotData : `data:image/png;base64,${plotData}`)
                            : null;
                        
                        if (imgSrc) {
                            plotsHtml += `
                                <div class="text-center">
                                    <h4 class="text-sm font-medium text-gray-700 mb-3">${plotKey.replace(/_/g, ' ').replace(/\b\w/g, l => l.toUpperCase())}</h4>
                                    <img src="${imgSrc}" alt="${plotKey}" class="max-w-full h-auto rounded-lg mx-auto" style="min-height: 300px; max-height: 500px;" />
                                </div>
                            `;
                        }
                    });
                    
                    plotsHtml += '</div>';
                    container.innerHTML = plotsHtml;
                } else {
                    container.innerHTML = '<p class="text-gray-500 text-center py-8">No spatial clustering plots available</p>';
                }
            } else {
                container.innerHTML = '<p class="text-gray-500 text-center py-8">Invalid spatial plot data format</p>';
            }
        }

        // NEW: Specific function to display statistical plots
        function displayStatisticalPlots(plots) {
            const container = document.getElementById('statisticalContent');
            
            if (typeof plots === 'object' && plots !== null) {
                const plotKeys = Object.keys(plots);
                if (plotKeys.length > 0) {
                    let plotsHtml = '<div class="mt-6 space-y-4">';
                    
                    plotKeys.forEach(plotKey => {
                        const plotData = plots[plotKey];
                        const imgSrc = typeof plotData === 'string' 
                            ? (plotData.startsWith('data:image') ? plotData : `data:image/png;base64,${plotData}`)
                            : null;
                        
                        if (imgSrc) {
                            plotsHtml += `
                                <div class="text-center">
                                    <h4 class="text-sm font-medium text-gray-700 mb-2">${plotKey.replace(/_/g, ' ').replace(/\b\w/g, l => l.toUpperCase())}</h4>
                                    <img src="${imgSrc}" alt="${plotKey}" class="max-w-full h-auto rounded-lg mx-auto" style="max-height: 400px;" />
                                </div>
                            `;
                        }
                    });
                    
                    plotsHtml += '</div>';
                    container.innerHTML += plotsHtml;
                }
            }
        }

        // Helper function to display animation data
        function displayAnimationFromData(animationData) {
            const container = document.getElementById('animationContent');
            
            if (Array.isArray(animationData) && animationData.length > 0) {
                // Create a simple frame-by-frame animation viewer
                let currentFrame = 0;
                
                container.innerHTML = `
                    <div class="animation-viewer">
                        <div class="mb-4 flex items-center justify-between">
                            <button id="prevFrame" class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">Previous</button>
                            <span class="text-gray-600">Frame ${currentFrame + 1} of ${animationData.length}</span>
                            <button id="nextFrame" class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">Next</button>
                        </div>
                        <div id="frameDisplay" class="text-center">
                            <p class="text-gray-600">Animation frames loaded (${animationData.length} frames)</p>
                            <p class="text-sm text-gray-500">Use the controls above to navigate through frames</p>
                        </div>
                    </div>
                `;
                
                // Add event listeners for frame navigation
                document.getElementById('prevFrame').addEventListener('click', () => {
                    currentFrame = Math.max(0, currentFrame - 1);
                    updateFrameDisplay();
                });
                
                document.getElementById('nextFrame').addEventListener('click', () => {
                    currentFrame = Math.min(animationData.length - 1, currentFrame + 1);
                    updateFrameDisplay();
                });
                
                function updateFrameDisplay() {
                    const frameData = animationData[currentFrame];
                    document.querySelector('.animation-viewer span').textContent = `Frame ${currentFrame + 1} of ${animationData.length}`;
                    
                    // Display frame information
                    const frameDisplay = document.getElementById('frameDisplay');
                    frameDisplay.innerHTML = `
                        <div class="bg-gray-100 p-4 rounded-lg">
                            <h5 class="font-semibold mb-2">Frame ${currentFrame + 1} Data</h5>
                            <pre class="text-xs text-left overflow-auto max-h-64">${JSON.stringify(frameData, null, 2)}</pre>
                        </div>
                    `;
                }
            } else {
                container.innerHTML = '<p class="text-gray-500">No animation data available</p>';
            }
        }

        // Helper function to display clustering results
        function displayClusteringResults(clusterData) {
            const container = document.getElementById('spatialChart');
            
            if (Array.isArray(clusterData) && clusterData.length > 0) {
                container.innerHTML = `
                    <div class="space-y-4">
                        <h4 class="font-semibold text-gray-800">Cluster Data (${clusterData.length} points)</h4>
                        <div class="max-h-64 overflow-auto">
                            <table class="min-w-full text-sm">
                                <thead>
                                    <tr class="bg-gray-100">
                                        <th class="px-2 py-1 text-left">Index</th>
                                        <th class="px-2 py-1 text-left">Cluster Info</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    ${clusterData.slice(0, 10).map((cluster, index) => `
                                        <tr class="border-t">
                                            <td class="px-2 py-1">${index}</td>
                                            <td class="px-2 py-1">${JSON.stringify(cluster)}</td>
                                        </tr>
                                    `).join('')}
                                    ${clusterData.length > 10 ? `
                                        <tr class="border-t">
                                            <td colspan="2" class="px-2 py-1 text-center text-gray-500">
                                                ... and ${clusterData.length - 10} more clusters
                                            </td>
                                        </tr>
                                    ` : ''}
                                </tbody>
                            </table>
                        </div>
                    </div>
                `;
            } else {
                container.innerHTML = '<p class="text-gray-500">No cluster data available</p>';
            }
        }

        // Show/hide advanced loading indicator
        function showAdvancedLoading(show) {
            const indicator = document.getElementById('advancedLoadingIndicator');
            indicator.classList.toggle('hidden', !show);
        }
        // Show advanced error message
        function showAdvancedError(message) {
            // You can reuse the existing error display or create a new one
            showError(message);
        }

        // Hide advanced error message
        function hideAdvancedError() {
            hideError();
        }
</script>
