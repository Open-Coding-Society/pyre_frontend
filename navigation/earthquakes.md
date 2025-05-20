---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /earthquakes/
title: Earthquakes
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
    .earthquake-popup .leaflet-popup-content-wrapper {
      background-color: transparent;
      color: white;
    }
    .earthquake-popup .leaflet-popup-tip {
      background-color: #374151;
    }
    .magnitude-high {
      color: #ef4444;
    }
    .magnitude-medium {
      color: #f97316;
    }
    .magnitude-low {
      color: #3b82f6;
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
    .epicenter {
      animation: ripple 3s ease-out infinite;
    }
    @keyframes ripple {
      0% {
        transform: scale(0.1);
        opacity: 1;
      }
      70% {
        transform: scale(3);
        opacity: 0.3;
      }
      100% {
        transform: scale(4);
        opacity: 0;
      }
    }
    
    /* Help popup styles */
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
    .close-popup:hover {
      color: #f9fafb;
    }
    .help-feature {
      border-left: 2px solid #3b82f6;
      padding-left: 1rem;
      margin-bottom: 1rem;
    }
    /* Dashboard title styles */
    .dashboard-title {
      background: linear-gradient(to right, #3b82f6, #2563eb);
      padding: 0.75rem 1.5rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    /* Help button style */
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
</style>

<div class="min-h-screen bg-gray-950 text-gray-200">
  <!-- Dashboard title bar -->
  <div class="dashboard-title">
    <div class="flex items-center">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z" />
      </svg>
      <h1 class="text-xl font-bold">Seismic Monitor Dashboard</h1>
    </div>
    <button id="help-button" class="help-button" type="button">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.228 9c.549-1.165 2.03-2 3.772-2 2.21 0 4 1.343 4 3 0 1.4-1.278 2.575-3.006 2.907-.542.104-.994.54-.994 1.093m0 3h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
      </svg>
      How to Use
    </button>
  </div>
  
  <!-- Help overlay and popup -->
  <div id="help-overlay" class="help-overlay" style="display: none;">
    <div class="help-popup" style="color: #fff;">
      <button id="close-popup" class="close-popup" type="button" style="color: #fff;">&times;</button>
      <h2 class="text-xl font-bold mb-4" style="color: #fff;">How to Use the Seismic Monitor Dashboard</h2>
      <p class="mb-4" style="color: #fff;">Welcome to the Seismic Monitor Dashboard! This tool helps you track and analyze earthquake data in real-time. Here's how to use the main features:</p>
      
      <div class="space-y-4">
        <div class="help-feature" style="color: #fff;">
          <h3 class="text-lg font-medium mb-2" style="color: #fff;">Interactive Map</h3>
          <p style="color: #fff;">The central map displays earthquake locations. Click on any marker to view detailed information about that seismic event.</p>
        </div>
        
        <div class="help-feature" style="color: #fff;">
          <h3 class="text-lg font-medium mb-2" style="color: #fff;">Analytics Panel</h3>
          <p style="color: #fff;">The left sidebar shows statistics and analytics including:</p>
          <ul class="list-disc ml-5 mt-2 space-y-1" style="color: #fff;">
            <li>Total seismic events in the past 24 hours</li>
            <li>Breakdown by magnitude categories</li>
            <li>Depth analysis of recorded earthquakes</li>
            <li>Current seismic activity gauge</li>
            <li>Fault zone analysis</li>
          </ul>
        </div>
        
        <div class="help-feature" style="color: #fff;">
          <h3 class="text-lg font-medium mb-2" style="color: #fff;">Earthquake Events Table</h3>
          <p style="color: #fff;">The right sidebar lists all recent seismic events with magnitude, time, and location data. Click any row to center the map on that event.</p>
        </div>
        
        <div class="help-feature" style="color: #fff;">
          <h3 class="text-lg font-medium mb-2" style="color: #fff;">Map Controls</h3>
          <ul class="list-disc ml-5 mt-2 space-y-1" style="color: #fff;">
            <li><strong>Layers</strong>: Toggle different map overlays</li>
            <li><strong>Toggle Fault Lines</strong>: Show or hide known fault lines</li>
            <li><strong>Zoom</strong>: Use the controls in the top-right of the map</li>
          </ul>
        </div>
        
        <div class="help-feature" style="color: #fff;">
          <h3 class="text-lg font-medium mb-2" style="color: #fff;">Bottom Toolbar</h3>
          <ul class="list-disc ml-5 mt-2 space-y-1" style="color: #fff;">
            <li><strong>Seismic Forecast</strong>: View predictions for future activity</li>
            <li><strong>Alerts</strong>: Check important notifications</li>
            <li><strong>Refresh Data</strong>: Update the dashboard with latest information</li>
            <li><strong>Export Report</strong>: Download current data as a report</li>
          </ul>
        </div>
      </div>
      
      <div class="mt-6 p-3 bg-blue-900/30 border border-blue-800 rounded" style="color: white;">
        <p class="text-sm"><strong>Tip:</strong> This dashboard updates automatically every few minutes, but you can use the "Refresh Data" button to get the most current information at any time.</p>
      </div>
    </div>
  </div>

  <script>
    // Help popup logic
    document.addEventListener('DOMContentLoaded', function() {
      const helpButton = document.getElementById('help-button');
      const helpOverlay = document.getElementById('help-overlay');
      const closePopup = document.getElementById('close-popup');

      if (helpButton && helpOverlay && closePopup) {
        helpButton.addEventListener('click', function() {
          helpOverlay.style.display = 'flex';
        });

        closePopup.addEventListener('click', function() {
          helpOverlay.style.display = 'none';
        });

        helpOverlay.addEventListener('click', function(event) {
          if (event.target === helpOverlay) {
            helpOverlay.style.display = 'none';
          }
        });

        document.addEventListener('keydown', function(event) {
          if (event.key === 'Escape' && helpOverlay.style.display === 'flex') {
            helpOverlay.style.display = 'none';
          }
        });
      }
    });
  </script>

  <!-- Dashboard content -->
  <div class="flex h-screen overflow-hidden">
    <!-- Left sidebar -->
    <div class="w-72 bg-gray-900/50 border-r border-gray-800 p-4 overflow-y-auto">
      <h2 class="text-lg font-medium mb-4">Analytics Overview</h2>
      <div class="mb-6">
        <div class="text-sm text-gray-400 mb-1">Total Seismic Events (Past day)</div>
        <div class="text-4xl font-bold" id="total-incidents">--</div>
        <div class="mt-4">
          <h3 class="text-sm text-gray-400 mb-2">Magnitude Categories</h3>
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
      </div>    <!-- Depth Analysis Widget -->
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
      <!-- Ground Motion Widget -->
      <div class="mb-6">
        <h3 class="text-sm text-gray-400 mb-3">Seismic Activity Gauge</h3>
        <div class="flex justify-between text-xs text-gray-400 mb-1">
          <div id="activity-location">--</div>
          <div id="activity-status">--</div>
        </div>
        <div class="bg-gray-900/70 rounded-lg p-3 h-40">
          <!-- Seismic Gauge -->
          <div class="h-full w-full flex items-center justify-center">
            <div class="w-32 h-32 relative">
              <div class="absolute inset-0 flex items-center justify-center">
                <div id="current-magnitude" class="text-3xl font-bold">--</div>
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
                  class="stroke-blue-500"
                  stroke-width="6"
                  stroke-dasharray="295.31" 
                  stroke-dashoffset="220"
                />
              </svg>
            </div>
          </div>
        </div>
      </div>
      <!-- Fault Zone Analysis Widget -->
      <div class="mb-6">
        <h3 class="text-sm text-gray-400 mb-3">Fault Zone Analysis</h3>
        <div class="bg-gray-900/70 rounded-lg p-3">
          <div class="flex items-center justify-between mb-2">
            <div class="text-sm">Active Fault Zones</div>
            <div id="active-faults" class="font-medium">--</div>
          </div>
          <div class="flex items-center justify-between mb-2">
            <div class="text-sm">Recent Activity</div>
            <div id="recent-activity" class="font-medium pulse">--</div>
          </div>
          <div class="flex items-center justify-between">
            <div class="text-sm">Risk Level</div>
            <div id="risk-level" class="font-medium">--</div>
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
          <div id="map" class="w-full h-full"></div>
          <!-- Map controls -->
          <div class="absolute top-4 right-4 flex space-x-2">
            <button class="bg-gray-200 bg-opacity-20 backdrop-blur-sm rounded-md px-3 py-1 text-sm text-gray-200 flex items-center hover:bg-opacity-30">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M3 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm0 4a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z" clip-rule="evenodd"/>
              </svg>
              Layers
            </button>
            <button id="toggle-fault-lines" class="bg-gray-200 bg-opacity-20 backdrop-blur-sm rounded-md px-3 py-1 text-sm text-gray-200 flex items-center hover:bg-opacity-30">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M5 2a1 1 0 011 1v1h1a1 1 0 010 2H6v1a1 1 0 01-2 0V6H3a1 1 0 010-2h1V3a1 1 0 011-1zm0 10a1 1 0 011 1v1h1a1 1 0 110 2H6v1a1 1 0 11-2 0v-1H3a1 1 0 110-2h1v-1a1 1 0 011-1zM12 2a1 1 0 01.967.744L14.146 7.2 17.5 9.134a1 1 0 010 1.732l-3.354 1.935-1.18 4.455a1 1 0 01-1.933 0L9.854 12.8 6.5 10.866a1 1 0 010-1.732l3.354-1.935 1.18-4.455A1 1 0 0112 2z" clip-rule="evenodd" />
              </svg>
              Toggle Fault Lines
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
            Seismic Forecast
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
          <div class="text-sm text-gray-400">Last updated: <span id="last-updated">--</span></div>
          <button class="bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-500 hover:to-indigo-500 text-white px-3 py-1 rounded flex items-center text-sm" id="refresh-data">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M4 2a1 1 0 011 1v2.101a7.002 7.002 0 0111.601 2.566 1 1 0 11-1.885.666A5.002 5.002 0 005.999 7H9a1 1 0 010 2H4a1 1 0 01-1-1V3a1 1 0 011-1zm.008 9.057a1 1 0 011.276.61A5.002 5.002 0 0014.001 13H11a1 1 0 110-2h5a1 1 0 011 1v5a1 1 0 11-2 0v-2.101a7.002 7.002 0 01-11.601-2.566 1 1 0 01.61-1.276z" clip-rule="evenodd" />
            </svg>
            Refresh Data
          </button>
          <button class="bg-gradient-to-r from-blue-600 to-indigo-600 hover:from-blue-500 hover:to-indigo-500 text-white px-3 py-1 rounded flex items-center text-sm">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 mr-1" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M3 17a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zm3.293-7.707a1 1 0 011.414 0L9 10.586V3a1 1 0 112 0v7.586l1.293-1.293a1 1 0 111.414 1.414l-3 3a1 1 0 01-1.414 0l-3-3a1 1 0 010-1.414z" clip-rule="evenodd" />
            </svg>
            Export Report
          </button>
        </div>
      </div>
    </div>
    <!-- Right sidebar - Earthquake Events Table -->
    <div class="w-72 bg-gray-900/50 border-l border-gray-800 p-4 overflow-y-auto">
      <div class="mb-4 flex justify-between items-center">
        <h2 class="text-lg font-medium">Earthquake Events</h2>
        <div class="text-xs text-gray-400 flex items-center">
          <span id="incident-count" class="mr-1">--</span> events
        </div>
      </div>
      <!-- Earthquakes Table -->
      <div class="overflow-y-auto max-h-full">
        <table class="incidents-table text-sm">
          <thead>
            <tr>
              <th class="sticky top-0 z-10">Magnitude</th>
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
  // Main Earthquake Dashboard Script with ML Integration
  document.addEventListener('DOMContentLoaded', function() {
    
    // Initialize the help popup functionality
    const helpButton = document.getElementById('help-button');
    const helpOverlay = document.getElementById('help-overlay');
    const closePopup = document.getElementById('close-popup');
    
    // Show help popup on page load for first-time users
    // You can use localStorage to show this only on first visit
    helpOverlay.style.display = 'flex';
    
    helpButton.addEventListener('click', function() {
      helpOverlay.style.display = 'flex';
    });
    
    closePopup.addEventListener('click', function() {
      helpOverlay.style.display = 'none';
    });
    
    // Close popup when clicking outside the popup content
    helpOverlay.addEventListener('click', function(event) {
      if (event.target === helpOverlay) {
        helpOverlay.style.display = 'none';
      }
    });
    
    // Close popup with escape key
    document.addEventListener('keydown', function(event) {
      if (event.key === 'Escape' && helpOverlay.style.display === 'flex') {
        helpOverlay.style.display = 'none';
      }
    });
    
    // ============ EARTHQUAKE DATA FUNCTIONS ============
    
    // Fetch earthquake data from API
    async function fetchEarthquakeData() {
      try {
        // In a real implementation, this would fetch from a real earthquake API
        // For demo purposes, we'll generate sample data
        
        // Sample earthquake magnitude categories
        const category_counts = {
          'Minor (< 4.0)': 18,
          'Light (4.0-4.9)': 8,
          'Moderate (5.0-5.9)': 3,
          'Strong (≥ 6.0)': 1
        };
        
        // Calculate total incidents
        let total_incidents = Object.values(category_counts).reduce((sum, count) => sum + count, 0);
        
        // Sample earthquake events for today
        const today_incidents = [
          {
            magnitude: '6.2',
            time: '04:32:15',
            location: 'Offshore, 120km SW',
            depth: '25.4',
            felt_reports: '347'
          },
          {
            magnitude: '4.5',
            time: '07:15:22',
            location: 'Valley Region, North',
            depth: '8.7',
            felt_reports: '213'
          },
          {
            magnitude: '3.8',
            time: '10:44:09',
            location: 'Mountain Range, East',
            depth: '12.5',
            felt_reports: '76'
          },
          {
            magnitude: '5.1',
            time: '13:20:51',
            location: 'Coastal Area, West',
            depth: '18.3',
            felt_reports: '189'
          },
          {
            magnitude: '3.5',
            time: '15:09:33',
            location: 'Downtown District',
            depth: '5.2',
            felt_reports: '122'
          },
          {
            magnitude: '2.8',
            time: '17:45:12',
            location: 'Highland Area',
            depth: '9.6',
            felt_reports: '18'
          }
        ];
        
        // Update the earthquake table
        updateEarthquakeTable(today_incidents);
        
        // Update counters and stats
        document.getElementById('total-incidents').textContent = total_incidents;
        document.getElementById('incident-count').textContent = total_incidents;
        document.getElementById('last-updated').textContent = new Date().toLocaleTimeString();
        
        // Update category stats
        updateCategoryStats(category_counts, total_incidents);
        
        // Update depth analysis
        updateDepthAnalysis(today_incidents);
        
        // Update gauge
        updateSeismicGauge(today_incidents);
        
        // Update fault zone analysis
        updateFaultZoneAnalysis