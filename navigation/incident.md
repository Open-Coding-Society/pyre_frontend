---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /incident/
title: Traffic Incident 
---

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Traffic Incident Finder</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #1a1a1a 0%, #2d1b1b 100%);
            color: #ffffff;
            min-height: 100vh;
            padding: 40px 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
        }

        .header {
            text-align: center;
            margin-bottom: 50px;
        }

        .header h1 {
            font-size: 3em;
            color: #ff3333;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
            margin-bottom: 10px;
        }

        .header p {
            font-size: 1.2em;
            color: #cccccc;
            margin-bottom: 30px;
        }

        .location-section {
            background: rgba(255, 51, 51, 0.1);
            border: 2px solid #ff3333;
            border-radius: 15px;
            padding: 40px;
            margin-bottom: 40px;
            backdrop-filter: blur(10px);
        }

        .location-controls {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            align-items: center;
            justify-content: center;
            margin-bottom: 30px;
        }

        .input-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .input-group label {
            color: #ff3333;
            font-weight: bold;
            font-size: 1.1em;
        }

        .input-group input {
            padding: 15px 20px;
            border: 2px solid #333;
            border-radius: 8px;
            background: #1a1a1a;
            color: #ffffff;
            font-size: 1em;
            transition: all 0.3s ease;
            min-width: 150px;
        }

        .input-group input:focus {
            outline: none;
            border-color: #ff3333;
            box-shadow: 0 0 10px rgba(255, 51, 51, 0.3);
        }

        .btn {
            padding: 15px 30px;
            border: none;
            border-radius: 8px;
            font-size: 1.1em;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .btn-primary {
            background: linear-gradient(135deg, #ff3333, #cc0000);
            color: white;
            box-shadow: 0 4px 15px rgba(255, 51, 51, 0.3);
        }

        .btn-primary:hover {
            background: linear-gradient(135deg, #ff5555, #ff3333);
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(255, 51, 51, 0.4);
        }

        .btn-secondary {
            background: transparent;
            color: #ff3333;
            border: 2px solid #ff3333;
        }

        .btn-secondary:hover {
            background: #ff3333;
            color: white;
            transform: translateY(-2px);
        }

        .status {
            text-align: center;
            margin: 30px 0;
            padding: 20px;
            border-radius: 10px;
            font-size: 1.1em;
        }

        .status.loading {
            background: rgba(255, 165, 0, 0.2);
            border: 2px solid #ffa500;
            color: #ffa500;
        }

        .status.error {
            background: rgba(255, 51, 51, 0.2);
            border: 2px solid #ff3333;
            color: #ff3333;
        }

        .status.success {
            background: rgba(0, 255, 0, 0.2);
            border: 2px solid #00ff00;
            color: #00ff00;
        }

        .incidents-container {
            background: rgba(0, 0, 0, 0.3);
            border: 2px solid #333;
            border-radius: 15px;
            padding: 40px;
            margin-top: 40px;
        }

        .incidents-header {
            text-align: center;
            margin-bottom: 30px;
        }

        .incidents-header h2 {
            color: #ff3333;
            font-size: 2em;
            margin-bottom: 10px;
        }

        .incident-card {
            background: linear-gradient(135deg, #2a2a2a, #1a1a1a);
            border: 1px solid #ff3333;
            border-radius: 10px;
            padding: 25px;
            margin: 20px 0;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .incident-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, #ff3333, #cc0000);
        }

        .incident-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 30px rgba(255, 51, 51, 0.2);
            border-color: #ff5555;
        }

        .incident-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            flex-wrap: wrap;
            gap: 10px;
        }

        .incident-type {
            background: #ff3333;
            color: white;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 0.9em;
            font-weight: bold;
            text-transform: uppercase;
        }

        .incident-severity {
            padding: 6px 12px;
            border-radius: 15px;
            font-size: 0.8em;
            font-weight: bold;
            text-transform: uppercase;
        }

        .severity-low { background: #28a745; color: white; }
        .severity-medium { background: #ffc107; color: black; }
        .severity-high { background: #dc3545; color: white; }

        .incident-details {
            line-height: 1.6;
        }

        .incident-details p {
            margin: 10px 0;
        }

        .incident-location {
            color: #ff3333;
            font-weight: bold;
        }

        .incident-description {
            color: #cccccc;
            margin-top: 15px;
            padding-top: 15px;
            border-top: 1px solid #444;
        }

        .no-incidents {
            text-align: center;
            padding: 60px 20px;
            color: #888;
            font-size: 1.2em;
        }

        .loading-spinner {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid #333;
            border-radius: 50%;
            border-top-color: #ff3333;
            animation: spin 1s ease-in-out infinite;
            margin-right: 10px;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        @media (max-width: 768px) {
            .header h1 {
                font-size: 2em;
            }
            
            .location-controls {
                flex-direction: column;
                align-items: stretch;
            }
            
            .input-group input {
                min-width: auto;
            }
            
            .incident-header {
                flex-direction: column;
                align-items: flex-start;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🚨 Traffic Incident Finder</h1>
            <p>Get real-time traffic incidents near your location</p>
        </div>

        <div class="location-section">
            <div class="location-controls">
                <div class="input-group">
                    <label for="latitude">Latitude</label>
                    <input type="number" id="latitude" step="any" placeholder="e.g., 40.7128">
                </div>
                <div class="input-group">
                    <label for="longitude">Longitude</label>
                    <input type="number" id="longitude" step="any" placeholder="e.g., -74.0060">
                </div>
                <div class="input-group">
                    <label for="radius">Radius (km)</label>
                    <input type="number" id="radius" min="1" max="50" value="10" placeholder="10">
                </div>
            </div>
            
            <div class="location-controls">
                <button class="btn btn-secondary" onclick="getCurrentLocation()">📍 Use My Location</button>
                <button class="btn btn-primary" onclick="getTrafficIncidents()">🔍 Find Incidents</button>
            </div>
        </div>

        <div id="status" class="status" style="display: none;"></div>

        <div id="incidents-container" class="incidents-container" style="display: none;">
            <div class="incidents-header">
                <h2>Traffic Incidents</h2>
                <p id="location-info"></p>
            </div>
            <div id="incidents-list"></div>
        </div>
    </div>

    <script>
        const API_BASE_URL = 'http://localhost:8505/api';

        function showStatus(message, type) {
            const statusEl = document.getElementById('status');
            statusEl.textContent = message;
            statusEl.className = `status ${type}`;
            statusEl.style.display = 'block';
        }

        function hideStatus() {
            document.getElementById('status').style.display = 'none';
        }

        function getCurrentLocation() {
            if (!navigator.geolocation) {
                showStatus('Geolocation is not supported by this browser.', 'error');
                return;
            }

            showStatus('Getting your location...', 'loading');
            
            navigator.geolocation.getCurrentPosition(
                (position) => {
                    document.getElementById('latitude').value = position.coords.latitude.toFixed(6);
                    document.getElementById('longitude').value = position.coords.longitude.toFixed(6);
                    showStatus('Location obtained successfully!', 'success');
                    setTimeout(hideStatus, 2000);
                },
                (error) => {
                    let errorMessage = 'Unable to get your location. ';
                    switch(error.code) {
                        case error.PERMISSION_DENIED:
                            errorMessage += 'Location access denied by user.';
                            break;
                        case error.POSITION_UNAVAILABLE:
                            errorMessage += 'Location information unavailable.';
                            break;
                        case error.TIMEOUT:
                            errorMessage += 'Location request timed out.';
                            break;
                        default:
                            errorMessage += 'Unknown error occurred.';
                            break;
                    }
                    showStatus(errorMessage, 'error');
                }
            );
        }

        async function getTrafficIncidents() {
            const latitude = document.getElementById('latitude').value;
            const longitude = document.getElementById('longitude').value;
            const radius = document.getElementById('radius').value || 10;

            if (!latitude || !longitude) {
                showStatus('Please provide both latitude and longitude.', 'error');
                return;
            }

            showStatus('Fetching traffic incidents...', 'loading');

            try {
                const response = await fetch(`${API_BASE_URL}/incidents?lat=${latitude}&lon=${longitude}&radius=${radius}`);
                
                if (!response.ok) {
                    throw new Error(`HTTP error! status: ${response.status}`);
                }

                const data = await response.json();
                hideStatus();
                displayIncidents(data.incidents, latitude, longitude, radius);
            } catch (error) {
                console.error('Error fetching incidents:', error);
                showStatus(`Error: ${error.message}. Make sure the backend server is running on port 8505.`, 'error');
            }
        }

        function displayIncidents(incidents, lat, lon, radius) {
            const container = document.getElementById('incidents-container');
            const locationInfo = document.getElementById('location-info');
            const incidentsList = document.getElementById('incidents-list');

            locationInfo.textContent = `Showing incidents within ${radius}km of ${parseFloat(lat).toFixed(4)}, ${parseFloat(lon).toFixed(4)}`;

            if (!incidents || incidents.length === 0) {
                incidentsList.innerHTML = '<div class="no-incidents">🎉 No traffic incidents found in this area!</div>';
            } else {
                incidentsList.innerHTML = incidents.map(incident => `
                    <div class="incident-card">
                        <div class="incident-header">
                            <span class="incident-type">${incident.type || 'Traffic Incident'}</span>
                            <span class="incident-severity severity-${(incident.severity || 'low').toLowerCase()}">
                                ${incident.severity || 'Low'} Impact
                            </span>
                        </div>
                        <div class="incident-details">
                            <p class="incident-location">📍 ${incident.location || 'Location not specified'}</p>
                            ${incident.description ? `<p class="incident-description">${incident.description}</p>` : ''}
                            ${incident.startTime ? `<p><strong>Started:</strong> ${new Date(incident.startTime).toLocaleString()}</p>` : ''}
                            ${incident.endTime ? `<p><strong>Expected End:</strong> ${new Date(incident.endTime).toLocaleString()}</p>` : ''}
                            ${incident.delay ? `<p><strong>Delay:</strong> ${incident.delay} minutes</p>` : ''}
                        </div>
                    </div>
                `).join('');
            }

            container.style.display = 'block';
        }

        // Set default location (New York City) for demo purposes
        window.addEventListener('load', () => {
            document.getElementById('latitude').value = '40.7128';
            document.getElementById('longitude').value = '-74.0060';
        });
    </script>
</body>
</html>

<a href="/QcommVNE_Frontend/help/" class="fixed bottom-4 right-4 bg-green-600 text-white rounded-full p-3 shadow-lg hover:bg-green-700 transition duration-200 flex items-center justify-center" title="Help Center" style="font-size:1.05em;">
    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9 5.25h.008v.008H12v-.008z"/>
    </svg>
    <span class="ml-1 font-medium">Help</span>
  </a>