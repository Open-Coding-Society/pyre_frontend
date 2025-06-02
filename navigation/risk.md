---
layout: base_chat
search_exclude: true
hide: true
show_reading_time: false
permalink: /risk-predictor/
title: Risk Predictor
---

<style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        .risk-container {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #000000, #1a1a1a);
            min-height: 100vh;
            color: #ffffff;
            padding: 20px 0;
        }

        .risk-wrapper {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .risk-header {
            text-align: center;
            margin-bottom: 40px;
            color: #ff3333;
        }

        .risk-header h1 {
            font-size: 3rem;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(255, 51, 51, 0.3);
            background: linear-gradient(90deg, #ff3333, #ff6600, #ff3300);
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .risk-header p {
            font-size: 1.2rem;
            opacity: 0.9;
            color: #ff6600;
        }

        .search-section {
            background: #1a1a1a;
            border-radius: 15px;
            padding: 30px;
            margin-bottom: 30px;
            box-shadow: 0 10px 30px rgba(255, 51, 51, 0.1);
            border: 1px solid #333;
        }

        .search-form {
            display: flex;
            gap: 15px;
            align-items: center;
            justify-content: center;
            flex-wrap: wrap;
        }

        .input-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .input-group label {
            font-weight: 600;
            color: #ff6600;
            font-size: 0.9rem;
        }

        .zip-input {
            padding: 15px 20px;
            border: 2px solid #333;
            border-radius: 10px;
            font-size: 1.1rem;
            width: 200px;
            transition: all 0.3s ease;
            background: #1a1a1a;
            color: #ffffff;
        }

        .zip-input:focus {
            outline: none;
            border-color: #ff3333;
            box-shadow: 0 0 10px rgba(255, 51, 51, 0.3);
        }

        .search-btn {
            background: linear-gradient(135deg, #ff3333, #cc0000);
            color: white;
            border: none;
            padding: 15px 30px;
            border-radius: 10px;
            font-size: 1.1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-top: 24px;
            box-shadow: 0 4px 15px rgba(255, 51, 51, 0.3);
        }

        .search-btn:hover {
            background: linear-gradient(135deg, #ff5555, #ff3333);
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(255, 51, 51, 0.4);
        }

        .search-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            transform: none;
        }

        .results-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 30px;
            margin-top: 30px;
        }

        @media (max-width: 768px) {
            .results-container {
                grid-template-columns: 1fr;
            }
        }

        .risk-card {
            background: linear-gradient(135deg, #2a2a2a, #1a1a1a);
            border: 1px solid #ff3333;
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 10px 30px rgba(255, 51, 51, 0.1);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }

        .risk-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, #ff3333, #cc0000);
        }

        .risk-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 40px rgba(255, 51, 51, 0.2);
            border-color: #ff5555;
        }

        .risk-header {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 25px;
        }

        .risk-icon {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
        }

        .fire-icon {
            background: linear-gradient(135deg, #ff3333, #ff6600);
            color: white;
        }

        .earthquake-icon {
            background: linear-gradient(135deg, #ff6600, #ff3333);
            color: white;
        }

        .risk-title {
            font-size: 1.5rem;
            font-weight: 700;
            color: #ffffff;
        }

        .risk-level {
            display: inline-block;
            padding: 8px 16px;
            border-radius: 20px;
            font-weight: 600;
            font-size: 0.9rem;
            margin-bottom: 20px;
        }

        .risk-low {
            background: rgba(255, 102, 0, 0.2);
            color: #ff6600;
            border: 1px solid #ff6600;
        }

        .risk-medium {
            background: rgba(255, 51, 51, 0.2);
            color: #ff3333;
            border: 1px solid #ff3333;
        }

        .risk-high {
            background: rgba(204, 0, 0, 0.2);
            color: #cc0000;
            border: 1px solid #cc0000;
        }

        .risk-details {
            margin-top: 20px;
        }

        .detail-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 0;
            border-bottom: 1px solid #333;
        }

        .detail-item:last-child {
            border-bottom: none;
        }

        .detail-label {
            font-weight: 600;
            color: #ff6600;
        }

        .detail-value {
            font-weight: 700;
            color: #ffffff;
        }

        .loading {
            text-align: center;
            padding: 40px;
            color: #ff6600;
        }

        .loading-spinner {
            width: 40px;
            height: 40px;
            border: 4px solid rgba(255, 51, 51, 0.1);
            border-top: 4px solid #ff3333;
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 20px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .error {
            background: rgba(255, 51, 51, 0.2);
            color: #ff3333;
            border: 1px solid #ff3333;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
        }

        .info-section {
            background: white;
            border-radius: 15px;
            padding: 30px;
            margin-top: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.1);
        }

        .info-section h3 {
            color: #333;
            margin-bottom: 15px;
            font-size: 1.3rem;
        }

        .info-section p {
            color: #666;
            line-height: 1.6;
            margin-bottom: 15px;
        }

        .hidden {
            display: none;
    </style>

<div class="risk-container">
    <div class="risk-wrapper">
        <div class="risk-header">
            <h1>🔥 Risk Predictor</h1>
            <p>Assess wildfire and earthquake risks for any location</p>
        </div>

 <div class="search-section">
            <form class="search-form" id="riskForm">
                <div class="input-group">
                    <label for="zipCode">ZIP Code</label>
                    <input 
                        type="text" 
                        id="zipCode" 
                        class="zip-input" 
                        placeholder="e.g. 92127" 
                        pattern="[0-9]{5}"
                        maxlength="5"
                        required
                    >
                </div>
                <button type="submit" class="search-btn" id="searchBtn">
                    🔍 Analyze Risk
                </button>
            </form>
        </div>
    <div id="loadingSection" class="loading hidden">
            <div class="loading-spinner"></div>
            <p>Analyzing risk data for your location...</p>
        </div>

<div id="errorSection" class="error hidden"></div>

 <div id="resultsSection" class="results-container hidden">
            <div class="risk-card" id="wildfireCard">
                <div class="risk-header">
                    <div class="risk-icon fire-icon">🔥</div>
                    <div class="risk-title">Wildfire Risk</div>
                </div>
                <div id="wildfireRiskLevel" class="risk-level">Calculating...</div>
                <div class="risk-details" id="wildfireDetails">
                    <!-- Dynamic content will be inserted here -->
                </div>
            </div>

   <div class="risk-card" id="earthquakeCard">
                <div class="risk-header">
                    <div class="risk-icon earthquake-icon">🌍</div>
                    <div class="risk-title">Earthquake Risk</div>
                </div>
                <div id="earthquakeRiskLevel" class="risk-level">Calculating...</div>
                <div class="risk-details" id="earthquakeDetails">
                    <!-- Dynamic content will be inserted here -->
                </div>
            </div>
        </div>
 <div class="info-section">
            <h3>Understanding Risk Levels</h3>
            <p><strong>Low Risk:</strong> Minimal historical activity and favorable geographic conditions. Stay informed about general preparedness.</p>
            <p><strong>Medium Risk:</strong> Some historical activity or moderate risk factors present. Consider basic preparedness measures.</p>
            <p><strong>High Risk:</strong> Significant historical activity or multiple risk factors. Implement comprehensive preparedness plans.</p>
            
   <h3>How We Calculate Risk</h3>
            <p>Our risk assessment combines historical data, geographic factors, and current conditions to provide you with an accurate risk profile for your location. Wildfire risk considers vegetation, climate patterns, and recent fire activity. Earthquake risk is based on seismic history, fault proximity, and geological conditions.</p>
        </div>
    </div>
</div>

 <script>
        const API_BASE_URL = 'http://127.0.0.1:8505/api/risk'; // Adjust this to match your backend URL
        
        document.getElementById('riskForm').addEventListener('submit', async function(e) {
            e.preventDefault();
            
            const zipCode = document.getElementById('zipCode').value;
            const searchBtn = document.getElementById('searchBtn');
            
            if (!zipCode || zipCode.length !== 5) {
                showError('Please enter a valid 5-digit ZIP code');
                return;
            }
            
            // Show loading state
            showLoading();
            searchBtn.disabled = true;
            searchBtn.textContent = '🔄 Analyzing...';
            
            try {
                // Fetch both risks simultaneously
                const [wildfireResponse, earthquakeResponse] = await Promise.all([
                    fetch(`${API_BASE_URL}/wildfire?zip=${zipCode}`),
                    fetch(`${API_BASE_URL}/earthquake?zip=${zipCode}`)
                ]);
                
                if (!wildfireResponse.ok || !earthquakeResponse.ok) {
                    throw new Error('Failed to fetch risk data');
                }
                
                const wildfireData = await wildfireResponse.json();
                const earthquakeData = await earthquakeResponse.json();
                
                displayResults(wildfireData, earthquakeData);
                
            } catch (error) {
                console.error('Error fetching risk data:', error);
                showError('Unable to fetch risk data. Please try again later.');
            } finally {
                hideLoading();
                searchBtn.disabled = false;
                searchBtn.textContent = '🔍 Analyze Risk';
            }
        });
        
        function showLoading() {
            document.getElementById('loadingSection').classList.remove('hidden');
            document.getElementById('resultsSection').classList.add('hidden');
            document.getElementById('errorSection').classList.add('hidden');
        }
        
        function hideLoading() {
            document.getElementById('loadingSection').classList.add('hidden');
        }
        
        function showError(message) {
            const errorSection = document.getElementById('errorSection');
            errorSection.textContent = message;
            errorSection.classList.remove('hidden');
            document.getElementById('resultsSection').classList.add('hidden');
            hideLoading();
        }
        
        function displayResults(wildfireData, earthquakeData) {
            // Display wildfire results
            const wildfireLevel = document.getElementById('wildfireRiskLevel');
            const wildfireDetails = document.getElementById('wildfireDetails');
            
            wildfireLevel.textContent = wildfireData.risk_level;
            wildfireLevel.className = `risk-level risk-${wildfireData.risk_level.toLowerCase()}`;
            
            wildfireDetails.innerHTML = `
                <div class="detail-item">
                    <span class="detail-label">Recent Fires (30 days)</span>
                    <span class="detail-value">${wildfireData.recent_fires || 0}</span>
                </div>
                <div class="detail-item">
                    <span class="detail-label">Vegetation Risk</span>
                    <span class="detail-value">${wildfireData.vegetation_risk || 'N/A'}</span>
                </div>
                <div class="detail-item">
                    <span class="detail-label">Weather Conditions</span>
                    <span class="detail-value">${wildfireData.weather_risk || 'N/A'}</span>
                </div>
                <div class="detail-item">
                    <span class="detail-label">Risk Score</span>
                    <span class="detail-value">${wildfireData.risk_score || 'N/A'}/100</span>
                </div>
            `;
            
            // Display earthquake results
            const earthquakeLevel = document.getElementById('earthquakeRiskLevel');
            const earthquakeDetails = document.getElementById('earthquakeDetails');
            
            earthquakeLevel.textContent = earthquakeData.risk_level;
            earthquakeLevel.className = `risk-level risk-${earthquakeData.risk_level.toLowerCase()}`;
            
            earthquakeDetails.innerHTML = `
                <div class="detail-item">
                    <span class="detail-label">Recent Earthquakes (1 year)</span>
                    <span class="detail-value">${earthquakeData.recent_earthquakes || 0}</span>
                </div>
                <div class="detail-item">
                    <span class="detail-label">Largest Magnitude</span>
                    <span class="detail-value">${earthquakeData.max_magnitude || 'N/A'}</span>
                </div>
                <div class="detail-item">
                    <span class="detail-label">Fault Proximity</span>
                    <span class="detail-value">${earthquakeData.fault_distance || 'N/A'}</span>
                </div>
                <div class="detail-item">
                    <span class="detail-label">Risk Score</span>
                    <span class="detail-value">${earthquakeData.risk_score || 'N/A'}/100</span>
                </div>
            `;
            
            // Show results
            document.getElementById('resultsSection').classList.remove('hidden');
            document.getElementById('errorSection').classList.add('hidden');
        }
        
        // Input validation for ZIP code
        document.getElementById('zipCode').addEventListener('input', function(e) {
            e.target.value = e.target.value.replace(/[^0-9]/g, '');
        });
    </script>
<a href="/pyre_frontend/help/" class="fixed bottom-4 right-4 bg-green-600 text-white rounded-full p-3 shadow-lg hover:bg-green-700 transition duration-200 flex items-center justify-center" title="Help Center" style="font-size:1.05em;">
    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9 5.25h.008v.008H12v-.008z"/>
    </svg>
    <span class="ml-1 font-medium">Help</span>
</a>