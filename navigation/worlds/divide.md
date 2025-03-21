---
layout: base
title: Digital Divide Map Activity
search_exclude: true
permalink: /digitalDivideActivity/
---

<style>
  :root {
    --primary-color: #3b82f6;
    --primary-dark: #2563eb;
    --secondary-color: #10b981;
    --background: #f9fafb;
    --card-bg: #ffffff;
    --text-primary: #1f2937;
    --text-secondary: #6b7280;
    --border-color: #e5e7eb;
    --shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
  }

  body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
    background-color: var(--background);
    color: var(--text-primary);
    line-height: 1.5;
    margin: 0;
    padding: 0;
  }

  .container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem 1rem;
  }

  h1, h2, h3, h4 {
    margin-top: 0;
    color: var(--text-primary);
    font-weight: 700;
  }

  h1 {
    font-size: 2rem;
    margin-bottom: 1.5rem;
    text-align: center;
    color: var(--primary-dark);
    position: relative;
    padding-bottom: 0.5rem;
  }

  h1::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 80px;
    height: 3px;
    background-color: var(--primary-color);
    border-radius: 3px;
  }

  p {
    color: var(--text-secondary);
  }

  .card {
    background-color: var(--card-bg);
    border-radius: 8px;
    box-shadow: var(--shadow);
    overflow: hidden;
    transition: all 0.3s ease;
  }

  .card:hover {
    box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
  }

  .map-container {
    height: 500px;
    border-radius: 8px;
    overflow: hidden;
    background-color: #f1f5f9;
  }

  .info-panel {
    height: 500px;
    overflow-y: auto;
    scrollbar-width: thin;
    scrollbar-color: var(--primary-color) var(--border-color);
  }

  .info-panel::-webkit-scrollbar {
    width: 6px;
  }

  .info-panel::-webkit-scrollbar-track {
    background: var(--border-color);
    border-radius: 10px;
  }

  .info-panel::-webkit-scrollbar-thumb {
    background-color: var(--primary-color);
    border-radius: 10px;
  }

  .panel-content {
    padding: 1.5rem;
  }

  .stat-item {
    margin-bottom: 1.25rem;
  }

  .stat-label {
    display: flex;
    justify-content: space-between;
    margin-bottom: 0.5rem;
    font-weight: 500;
    color: var(--text-secondary);
  }

  .stat-value {
    font-size: 0.875rem;
    color: var(--primary-dark);
    font-weight: 600;
  }

  .progress-bar {
    width: 100%;
    height: 8px;
    background-color: #e5e7eb;
    border-radius: 4px;
    overflow: hidden;
  }

  .progress-fill {
    height: 100%;
    border-radius: 4px;
    transition: width 0.5s ease-out;
  }

  .internet-fill {
    background-color: var(--primary-color);
  }

  .mobile-fill {
    background-color: var(--secondary-color);
  }

  .speed-fill {
    background-color: #8b5cf6;
  }

  .literacy-fill {
    background-color: #f59e0b;
  }

  .barriers-section {
    margin-top: 1.5rem;
    border-top: 1px solid var(--border-color);
    padding-top: 1rem;
  }

  .barriers-title {
    font-weight: 600;
    margin-bottom: 0.5rem;
    color: var(--text-primary);
  }

  .barriers-list {
    padding-left: 1.5rem;
    margin: 0.5rem 0;
  }

  .barriers-list li {
    margin-bottom: 0.375rem;
    font-size: 0.875rem;
    color: var(--text-secondary);
  }

  .about-section {
    margin-top: 2rem;
    padding: 1.5rem;
    background-color: var(--card-bg);
    border-radius: 8px;
    box-shadow: var(--shadow);
  }

  .flex-container {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
  }

  .map-col {
    flex: 1 1 65%;
    min-width: 300px;
  }

  .info-col {
    flex: 1 1 30%;
    min-width: 250px;
  }

  .placeholder-info {
    color: var(--text-secondary);
    font-style: italic;
    text-align: center;
    margin-top: 3rem;
  }

  .country-header {
    display: flex;
    align-items: center;
    margin-bottom: 1rem;
    padding-bottom: 0.75rem;
    border-bottom: 1px solid var(--border-color);
  }

  .country-name {
    font-size: 1.25rem;
    font-weight: 700;
    color: var(--primary-dark);
    margin: 0;
  }

  /* Legend styling */
  .legend-container {
    font-size: 0.75rem;
  }

  .legend-title {
    font-size: 0.8rem;
    font-weight: 600;
  }

  /* Tooltip styling */
  .tooltip {
    position: absolute;
    padding: 0.5rem;
    background: rgba(0, 0, 0, 0.8);
    color: #fff;
    border-radius: 4px;
    pointer-events: none;
    font-size: 0.75rem;
    z-index: 1000;
    max-width: 200px;
  }

  /* Footer styling */
  .footer {
    text-align: center;
    margin-top: 2rem;
    padding-top: 1rem;
    border-top: 1px solid var(--border-color);
    color: var(--text-secondary);
    font-size: 0.875rem;
  }
</style>

<div class="container">
  <h1>Digital Divide Map Explorer</h1>
  
  <div class="flex-container">
    <div class="map-col">
      <div id="map-container" class="card map-container"></div>
    </div>
    <div class="info-col">
      <div id="info-panel" class="card info-panel">
        <div class="panel-content">
          <div id="default-info">
            <h2>Digital Divide Statistics</h2>
            <p class="placeholder-info">Click on a country to view detailed information about internet access, device ownership, and digital literacy.</p>
          </div>
          <div id="country-details" class="hidden">
            <div class="country-header">
              <h3 id="country-name" class="country-name"></h3>
            </div>
            <div class="stat-item">
              <div class="stat-label">
                <span>Internet Penetration</span>
                <span id="internet-value" class="stat-value"></span>
              </div>
              <div class="progress-bar">
                <div id="internet-bar" class="progress-fill internet-fill" style="width: 0%"></div>
              </div>
            </div>
            <div class="stat-item">
              <div class="stat-label">
                <span>Mobile Device Access</span>
                <span id="mobile-value" class="stat-value"></span>
              </div>
              <div class="progress-bar">
                <div id="mobile-bar" class="progress-fill mobile-fill" style="width: 0%"></div>
              </div>
            </div>
            <div class="stat-item">
              <div class="stat-label">
                <span>Average Internet Speed</span>
                <span id="speed-value" class="stat-value"></span>
              </div>
              <div class="progress-bar">
                <div id="speed-bar" class="progress-fill speed-fill" style="width: 0%"></div>
              </div>
            </div>
            <div class="stat-item">
              <div class="stat-label">
                <span>Digital Literacy</span>
                <span id="literacy-value" class="stat-value"></span>
              </div>
              <div class="progress-bar">
                <div id="literacy-bar" class="progress-fill literacy-fill" style="width: 0%"></div>
              </div>
            </div>
            <div class="barriers-section">
              <h4 class="barriers-title">Main Barriers:</h4>
              <ul id="barriers-list" class="barriers-list"></ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  
  <div class="about-section">
    <h3>About the Digital Divide</h3>
    <br>
    <p>The digital divide refers to the gap between demographics and regions that have access to modern information and communications technology (ICT) and those that don't or have restricted access.</p>
    <p>This explorer visualizes global inequalities in internet access, showing disparities in connectivity, device ownership, internet speeds, and digital literacy across different countries and regions.</p>
  </div>
  
  <div class="footer">
    <p>Data sources: ITU, World Bank, GSMA, and various national telecommunications authorities (2023-2024)</p>
  </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.8.5/d3.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/topojson/3.0.2/topojson.min.js"></script>

<script>
  // Digital divide dataset
  const digitalDivideData = {
    "United States": {
        internetPenetration: 92,
        mobileAccess: 96,
        averageSpeed: 224.5,
        digitalLiteracy: 86,
        barriers: ["Affordability in rural areas", "Infrastructure gaps", "Senior digital literacy"]
    },
    "Canada": {
        internetPenetration: 94,
        mobileAccess: 95,
        averageSpeed: 167.4,
        digitalLiteracy: 83,
        barriers: ["Rural connectivity", "Indigenous community access", "Affordability"]
    },
    "United Kingdom": {
        internetPenetration: 95,
        mobileAccess: 97,
        averageSpeed: 95.5,
        digitalLiteracy: 87,
        barriers: ["Rural broadband quality", "Age-related digital exclusion", "Income inequality"]
    },
    "Germany": {
        internetPenetration: 92,
        mobileAccess: 95,
        averageSpeed: 107.8,
        digitalLiteracy: 80,
        barriers: ["Rural internet quality", "Digital skills gap among seniors", "Infrastructure development"]
    },
    "France": {
        internetPenetration: 91,
        mobileAccess: 94,
        averageSpeed: 183.7,
        digitalLiteracy: 78,
        barriers: ["Rural-urban divide", "Age-related digital gaps", "Economic barriers"]
    },
    "Japan": {
        internetPenetration: 93,
        mobileAccess: 98,
        averageSpeed: 205.6,
        digitalLiteracy: 85,
        barriers: ["Aging population adoption", "Rural connectivity", "Traditional business practices"]
    },
    "South Korea": {
        internetPenetration: 97,
        mobileAccess: 99,
        averageSpeed: 254.7,
        digitalLiteracy: 90,
        barriers: ["Elderly digital exclusion", "Digital addiction concerns", "Rural-urban gaps"]
    },
    "China": {
        internetPenetration: 70,
        mobileAccess: 83,
        averageSpeed: 169.3,
        digitalLiteracy: 64,
        barriers: ["Rural-urban divide", "Content restrictions", "Language barriers", "Income inequality"]
    },
    "India": {
        internetPenetration: 47,
        mobileAccess: 78,
        averageSpeed: 76.6,
        digitalLiteracy: 43,
        barriers: ["Infrastructure limitations", "Affordability", "Language barriers", "Digital literacy", "Gender gap"]
    },
    "Brazil": {
        internetPenetration: 75,
        mobileAccess: 85,
        averageSpeed: 91.8,
        digitalLiteracy: 56,
        barriers: ["Income inequality", "Rural connectivity", "Infrastructure costs", "Education barriers"]
    },
    "Mexico": {
        internetPenetration: 72,
        mobileAccess: 79,
        averageSpeed: 52.3,
        digitalLiteracy: 52,
        barriers: ["Rural access", "Income disparity", "Infrastructure limitations", "Education gaps"]
    },
    "South Africa": {
        internetPenetration: 68,
        mobileAccess: 82,
        averageSpeed: 44.5,
        digitalLiteracy: 48,
        barriers: ["Cost of data", "Infrastructure gaps", "Income inequality", "Urban-rural divide"]
    },
    "Nigeria": {
        internetPenetration: 42,
        mobileAccess: 73,
        averageSpeed: 22.2,
        digitalLiteracy: 32,
        barriers: ["Electricity access", "Cost barriers", "Infrastructure limitations", "Digital skills gap"]
    },
    "Kenya": {
        internetPenetration: 40,
        mobileAccess: 82,
        averageSpeed: 19.2,
        digitalLiteracy: 35,
        barriers: ["Infrastructure limitations", "Cost of internet", "Digital literacy", "Electricity access"]
    },
    "Egypt": {
        internetPenetration: 57,
        mobileAccess: 76,
        averageSpeed: 35.7,
        digitalLiteracy: 45,
        barriers: ["Infrastructure quality", "Cost barriers", "Digital literacy", "Gender gap"]
    },
    "Australia": {
        internetPenetration: 90,
        mobileAccess: 93,
        averageSpeed: 95.4,
        digitalLiteracy: 82,
        barriers: ["Remote area connectivity", "Indigenous community access", "Affordability in outback"]
    },
    "Russia": {
        internetPenetration: 85,
        mobileAccess: 89,
        averageSpeed: 73.9,
        digitalLiteracy: 68,
        barriers: ["Remote region connectivity", "Infrastructure limitations", "Digital skills gap"]
    },
    "Saudi Arabia": {
        internetPenetration: 98,
        mobileAccess: 97,
        averageSpeed: 109.2,
        digitalLiteracy: 72,
        barriers: ["Gender gap", "Digital skills disparity", "Content restrictions"]
    },
    "Pakistan": {
        internetPenetration: 36,
        mobileAccess: 65,
        averageSpeed: 18.5,
        digitalLiteracy: 30,
        barriers: ["Literacy barriers", "Infrastructure gaps", "Affordability", "Gender disparity", "Electricity access"]
    },
    "Indonesia": {
        internetPenetration: 62,
        mobileAccess: 81,
        averageSpeed: 41.3,
        digitalLiteracy: 47,
        barriers: ["Geographic isolation", "Infrastructure challenges", "Affordability", "Digital literacy"]
    },
    "Argentina": {
        internetPenetration: 81,
        mobileAccess: 89,
        averageSpeed: 56.7,
        digitalLiteracy: 62,
        barriers: ["Economic volatility", "Rural connectivity", "Infrastructure maintenance", "Affordability"]
    },
    "Chile": {
        internetPenetration: 87,
        mobileAccess: 92,
        averageSpeed: 171.2,
        digitalLiteracy: 67,
        barriers: ["Geographic disparities", "Indigenous access", "Income inequality", "Education gaps"]
    },
    "Vietnam": {
        internetPenetration: 73,
        mobileAccess: 92,
        averageSpeed: 78.4,
        digitalLiteracy: 53,
        barriers: ["Rural-urban divide", "Language barriers", "Digital skills", "Content restrictions"]
    },
    "Thailand": {
        internetPenetration: 78,
        mobileAccess: 93,
        averageSpeed: 225.2,
        digitalLiteracy: 58,
        barriers: ["Rural connectivity", "Language barriers", "Senior adoption", "Income disparity"]
    },
    "Malaysia": {
        internetPenetration: 89,
        mobileAccess: 94,
        averageSpeed: 112.7,
        digitalLiteracy: 65,
        barriers: ["Rural infrastructure", "Indigenous community access", "Digital skills gap"]
    },
    "Philippines": {
        internetPenetration: 67,
        mobileAccess: 89,
        averageSpeed: 58.7,
        digitalLiteracy: 49,
        barriers: ["Geographic challenges", "Infrastructure limitations", "Affordability", "Natural disasters"]
    },
    "Bangladesh": {
        internetPenetration: 33,
        mobileAccess: 63,
        averageSpeed: 39.6,
        digitalLiteracy: 29,
        barriers: ["Literacy barriers", "Gender gap", "Infrastructure limitations", "Affordability", "Electricity access"]
    },
    "Ethiopia": {
        internetPenetration: 24,
        mobileAccess: 45,
        averageSpeed: 11.9,
        digitalLiteracy: 20,
        barriers: ["Infrastructure deficits", "Electricity access", "Affordability", "Literacy barriers", "Language barriers"]
    },
    "Ghana": {
        internetPenetration: 53,
        mobileAccess: 81,
        averageSpeed: 31.7,
        digitalLiteracy: 39,
        barriers: ["Infrastructure limitations", "Cost barriers", "Electricity access", "Digital skills gap"]
    },
    "Morocco": {
        internetPenetration: 74,
        mobileAccess: 87,
        averageSpeed: 27.8,
        digitalLiteracy: 51,
        barriers: ["Rural connectivity", "Affordability", "Digital skills gap", "Gender disparity"]
    },
    "Israel": {
        internetPenetration: 91,
        mobileAccess: 94,
        averageSpeed: 122.3,
        digitalLiteracy: 85,
        barriers: ["Ultra-Orthodox community access", "Minority groups gap", "Affordability"]
    },
    "Poland": {
        internetPenetration: 87,
        mobileAccess: 93,
        averageSpeed: 90.5,
        digitalLiteracy: 73,
        barriers: ["Rural-urban divide", "Age-related gaps", "Digital skills among seniors"]
    },
    "Sweden": {
        internetPenetration: 98,
        mobileAccess: 99,
        averageSpeed: 166.9,
        digitalLiteracy: 93,
        barriers: ["Senior technology adoption", "Remote northern regions", "Refugee integration"]
    },
    "Turkey": {
        internetPenetration: 78,
        mobileAccess: 91,
        averageSpeed: 68.2,
        digitalLiteracy: 59,
        barriers: ["Rural connectivity", "Content restrictions", "Income disparity", "Education gaps"]
    },
    "Colombia": {
        internetPenetration: 68,
        mobileAccess: 83,
        averageSpeed: 63.4,
        digitalLiteracy: 54,
        barriers: ["Geography challenges", "Infrastructure limitations", "Armed conflict areas", "Economic barriers"]
    },
    "Peru": {
        internetPenetration: 65,
        mobileAccess: 79,
        averageSpeed: 46.8,
        digitalLiteracy: 48,
        barriers: ["Geographic isolation", "Indigenous access", "Infrastructure challenges", "Altitude challenges"]
    },
    "New Zealand": {
        internetPenetration: 93,
        mobileAccess: 96,
        averageSpeed: 144.2,
        digitalLiteracy: 84,
        barriers: ["Rural connectivity", "Māori community access", "Senior adoption"]
    },
    "Singapore": {
        internetPenetration: 99,
        mobileAccess: 99,
        averageSpeed: 269.3,
        digitalLiteracy: 92,
        barriers: ["Elderly adoption", "Income inequality", "Digital skills among seniors"]
    },
    "Spain": {
        internetPenetration: 89,
        mobileAccess: 94,
        averageSpeed: 183.9,
        digitalLiteracy: 75,
        barriers: ["Rural connectivity", "Age-related gaps", "Economic barriers", "Language barriers"]
    },
    "Rwanda": {
        internetPenetration: 26,
        mobileAccess: 54,
        averageSpeed: 11.4,
        digitalLiteracy: 28,
        barriers: ["Infrastructure limitations", "Affordability", "Literacy barriers", "Electricity access"]
    },
    "Italy": {
        internetPenetration: 84,
        mobileAccess: 92,
        averageSpeed: 93.6,
        digitalLiteracy: 67,
        barriers: ["Aging population", "Rural connectivity", "Digital skills gap", "Bureaucratic barriers"]
    },
    "Netherlands": {
        internetPenetration: 96,
        mobileAccess: 98,
        averageSpeed: 156.8,
        digitalLiteracy: 90,
        barriers: ["Senior digital literacy", "Immigration integration", "Socioeconomic divide"]
    },
    "Norway": {
        internetPenetration: 99,
        mobileAccess: 99,
        averageSpeed: 194.7,
        digitalLiteracy: 95,
        barriers: ["Remote Arctic communities", "Senior adoption", "Integration of new immigrants"]
    },
    "Denmark": {
        internetPenetration: 98,
        mobileAccess: 99,
        averageSpeed: 221.3,
        digitalLiteracy: 94,
        barriers: ["Age-related digital gaps", "Integration of immigrants", "Rural islands connectivity"]
    },
    "Finland": {
        internetPenetration: 97,
        mobileAccess: 99,
        averageSpeed: 173.5,
        digitalLiteracy: 93,
        barriers: ["Rural Arctic connectivity", "Senior adoption", "Language barriers for immigrants"]
    },
    "Switzerland": {
        internetPenetration: 97,
        mobileAccess: 98,
        averageSpeed: 192.3,
        digitalLiteracy: 91,
        barriers: ["Mountain region connectivity", "Senior digital literacy", "High service costs"]
    },
    "Portugal": {
        internetPenetration: 85,
        mobileAccess: 91,
        averageSpeed: 121.6,
        digitalLiteracy: 69,
        barriers: ["Rural access", "Senior digital literacy", "Income inequality", "Island connectivity"]
    },
    "Greece": {
        internetPenetration: 76,
        mobileAccess: 87,
        averageSpeed: 63.2,
        digitalLiteracy: 63,
        barriers: ["Island connectivity", "Economic constraints", "Senior adoption", "Rural infrastructure"]
    },
    "Austria": {
        internetPenetration: 90,
        mobileAccess: 95,
        averageSpeed: 111.9,
        digitalLiteracy: 82,
        barriers: ["Alpine region connectivity", "Senior digital literacy", "Rural-urban divide"]
    },
    "Czech Republic": {
        internetPenetration: 89,
        mobileAccess: 94,
        averageSpeed: 87.2,
        digitalLiteracy: 74,
        barriers: ["Rural connectivity", "Senior digital literacy", "Income disparity"]
    },
    "Hungary": {
        internetPenetration: 83,
        mobileAccess: 91,
        averageSpeed: 175.2,
        digitalLiteracy: 65,
        barriers: ["Rural-urban divide", "Roma community access", "Socioeconomic barriers", "Senior adoption"]
    },
    "Belgium": {
        internetPenetration: 92,
        mobileAccess: 96,
        averageSpeed: 109.5,
        digitalLiteracy: 81,
        barriers: ["Regional disparities", "Senior digital literacy", "Socioeconomic divide"]
    },
    "Ireland": {
        internetPenetration: 91,
        mobileAccess: 96,
        averageSpeed: 123.7,
        digitalLiteracy: 84,
        barriers: ["Rural broadband quality", "Island connectivity", "Senior adoption"]
    },
    "United Arab Emirates": {
        internetPenetration: 99,
        mobileAccess: 99,
        averageSpeed: 233.5,
        digitalLiteracy: 87,
        barriers: ["Expatriate disparities", "Regulatory restrictions", "Cost barriers for lower income workers"]
    },
    "Qatar": {
        internetPenetration: 100,
        mobileAccess: 100,
        averageSpeed: 186.6,
        digitalLiteracy: 84,
        barriers: ["Migrant worker access", "Content restrictions", "Digital skills gaps for certain populations"]
    },
    "Kuwait": {
        internetPenetration: 99,
        mobileAccess: 98,
        averageSpeed: 141.8,
        digitalLiteracy: 78,
        barriers: ["Migrant worker disparities", "Content restrictions", "Digital skills gaps"]
    },
    "Ukraine": {
        internetPenetration: 75,
        mobileAccess: 86,
        averageSpeed: 81.3,
        digitalLiteracy: 58,
        barriers: ["War-affected infrastructure", "Rural connectivity", "Economic challenges", "Aging population"]
    },
    "Belarus": {
        internetPenetration: 86,
        mobileAccess: 92,
        averageSpeed: 65.4,
        digitalLiteracy: 63,
        barriers: ["Content restrictions", "Rural connectivity", "Government restrictions", "Economic factors"]
    },
    "Kazakhstan": {
        internetPenetration: 81,
        mobileAccess: 89,
        averageSpeed: 54.8,
        digitalLiteracy: 59,
        barriers: ["Rural connectivity", "Language barriers", "Infrastructure limitations", "Content restrictions"]
    },
    "Algeria": {
        internetPenetration: 65,
        mobileAccess: 83,
        averageSpeed: 25.7,
        digitalLiteracy: 44,
        barriers: ["Rural access", "Infrastructure quality", "Regulatory constraints", "Digital skills gap"]
    },
    "Tunisia": {
        internetPenetration: 67,
        mobileAccess: 84,
        averageSpeed: 33.5,
        digitalLiteracy: 48,
        barriers: ["Rural connectivity", "Infrastructure quality", "Affordability", "Gender gap"]
    },
    "Lebanon": {
        internetPenetration: 79,
        mobileAccess: 89,
        averageSpeed: 21.3,
        digitalLiteracy: 54,
        barriers: ["Infrastructure instability", "Electricity shortages", "Economic crisis", "Affordability"]
    },
    "Jordan": {
        internetPenetration: 86,
        mobileAccess: 92,
        averageSpeed: 62.8,
        digitalLiteracy: 60,
        barriers: ["Refugee access", "Rural connectivity", "Gender gap", "Economic barriers"]
    },
    "Oman": {
        internetPenetration: 92,
        mobileAccess: 95,
        averageSpeed: 89.6,
        digitalLiteracy: 69,
        barriers: ["Rural mountain access", "Gender gap in certain regions", "Migrant worker access"]
    },
    "Bolivia": {
        internetPenetration: 54,
        mobileAccess: 76,
        averageSpeed: 27.3,
        digitalLiteracy: 41,
        barriers: ["Geographic challenges", "Indigenous access", "Altitude connectivity issues", "Affordability"]
    },
    "Ecuador": {
        internetPenetration: 61,
        mobileAccess: 79,
        averageSpeed: 39.8,
        digitalLiteracy: 46,
        barriers: ["Geographic isolation", "Indigenous barriers", "Infrastructure costs", "Rural access"]
    },
    "Venezuela": {
        internetPenetration: 53,
        mobileAccess: 71,
        averageSpeed: 9.7,
        digitalLiteracy: 43,
        barriers: ["Economic crisis", "Infrastructure deterioration", "Electricity shortages", "Affordability"]
    },
    "Paraguay": {
        internetPenetration: 68,
        mobileAccess: 83,
        averageSpeed: 34.2,
        digitalLiteracy: 44,
        barriers: ["Rural connectivity", "Indigenous access", "Language barriers", "Economic constraints"]
    },
    "Uruguay": {
        internetPenetration: 83,
        mobileAccess: 92,
        averageSpeed: 120.4,
        digitalLiteracy: 65,
        barriers: ["Rural-urban divide", "Economic barriers", "Age-related digital gaps"]
    },
    "Panama": {
        internetPenetration: 72,
        mobileAccess: 87,
        averageSpeed: 136.3,
        digitalLiteracy: 53,
        barriers: ["Indigenous community access", "Geographic barriers", "Urban-rural divide", "Affordability"]
    },
    "Costa Rica": {
        internetPenetration: 81,
        mobileAccess: 90,
        averageSpeed: 73.2,
        digitalLiteracy: 61,
        barriers: ["Rural connectivity", "Indigenous access", "Mountain infrastructure challenges"]
    },
    "Jamaica": {
        internetPenetration: 64,
        mobileAccess: 85,
        averageSpeed: 48.9,
        digitalLiteracy: 51,
        barriers: ["Rural access", "Affordability", "Infrastructure limitations", "Natural disaster vulnerability"]
    },
    "Trinidad and Tobago": {
        internetPenetration: 82,
        mobileAccess: 90,
        averageSpeed: 56.3,
        digitalLiteracy: 63,
        barriers: ["Rural connectivity", "Socioeconomic divide", "Digital skills gap", "Tobago island access"]
    },
    "Dominican Republic": {
        internetPenetration: 68,
        mobileAccess: 83,
        averageSpeed: 41.2,
        digitalLiteracy: 48,
        barriers: ["Rural connectivity", "Infrastructure quality", "Affordability", "Natural disaster vulnerability"]
    },
    "Haiti": {
        internetPenetration: 25,
        mobileAccess: 54,
        averageSpeed: 14.7,
        digitalLiteracy: 22,
        barriers: ["Infrastructure damage", "Electricity access", "Extreme poverty", "Natural disasters"]
    },
    "Tanzania": {
        internetPenetration: 31,
        mobileAccess: 59,
        averageSpeed: 17.3,
        digitalLiteracy: 26,
        barriers: ["Rural connectivity", "Infrastructure limitations", "Affordability", "Digital literacy"]
    },
    "Uganda": {
        internetPenetration: 27,
        mobileAccess: 63,
        averageSpeed: 15.8,
        digitalLiteracy: 29,
        barriers: ["Infrastructure gaps", "Electricity access", "Affordability", "Digital skills gap"]
    },
    "Mozambique": {
        internetPenetration: 21,
        mobileAccess: 50,
        averageSpeed: 11.2,
        digitalLiteracy: 18,
        barriers: ["Infrastructure limitations", "Electricity access", "Affordability", "Literacy barriers"]
    },
    "Senegal": {
        internetPenetration: 46,
        mobileAccess: 74,
        averageSpeed: 23.5,
        digitalLiteracy: 33,
        barriers: ["Rural connectivity", "Affordability", "Digital literacy", "Infrastructure quality"]
    },
    "Ivory Coast": {
        internetPenetration: 36,
        mobileAccess: 71,
        averageSpeed: 19.4,
        digitalLiteracy: 31,
        barriers: ["Infrastructure gaps", "Affordability", "Literacy barriers", "Rural connectivity"]
    },
    "Botswana": {
        internetPenetration: 53,
        mobileAccess: 78,
        averageSpeed: 38.6,
        digitalLiteracy: 47,
        barriers: ["Rural desert connectivity", "Cost barriers", "Infrastructure limitations", "Digital skills gap"]
    },
    "Namibia": {
        internetPenetration: 51,
        mobileAccess: 74,
        averageSpeed: 25.3,
        digitalLiteracy: 43,
        barriers: ["Geography challenges", "Population dispersion", "Infrastructure costs", "Digital literacy"]
    },
    "Cameroon": {
        internetPenetration: 28,
        mobileAccess: 55,
        averageSpeed: 16.5,
        digitalLiteracy: 27,
        barriers: ["Infrastructure limitations", "Affordability", "Rural access", "Literacy barriers"]
    },
    "Zambia": {
        internetPenetration: 30,
        mobileAccess: 58,
        averageSpeed: 14.9,
        digitalLiteracy: 25,
        barriers: ["Rural connectivity", "Electricity access", "Affordability", "Digital skills gap"]
    },
    "Nepal": {
        internetPenetration: 34,
        mobileAccess: 62,
        averageSpeed: 25.7,
        digitalLiteracy: 28,
        barriers: ["Mountain terrain", "Electricity access", "Affordability", "Digital literacy"]
    },
    "Sri Lanka": {
        internetPenetration: 47,
        mobileAccess: 76,
        averageSpeed: 54.3,
        digitalLiteracy: 43,
        barriers: ["Rural connectivity", "Affordability", "Language barriers", "Digital skills gap"]
    },
    "Myanmar": {
        internetPenetration: 41,
        mobileAccess: 70,
        averageSpeed: 32.7,
        digitalLiteracy: 34,
        barriers: ["Political restrictions", "Infrastructure gaps", "Affordability", "Rural access"]
    },
    "Cambodia": {
        internetPenetration: 52,
        mobileAccess: 85,
        averageSpeed: 31.2,
        digitalLiteracy: 38,
        barriers: ["Rural connectivity", "Digital literacy", "Language barriers", "Infrastructure quality"]
    },
    "Laos": {
        internetPenetration: 43,
        mobileAccess: 65,
        averageSpeed: 28.3,
        digitalLiteracy: 34,
        barriers: ["Mountainous terrain", "Infrastructure limitations", "Affordability", "Digital literacy"]
    },
    "Mongolia": {
        internetPenetration: 62,
        mobileAccess: 78,
        averageSpeed: 42.3,
        digitalLiteracy: 54,
        barriers: ["Nomadic populations", "Extreme climate", "Geographic vastness", "Infrastructure costs"]
    },
    "Fiji": {
        internetPenetration: 59,
        mobileAccess: 74,
        averageSpeed: 34.8,
        digitalLiteracy: 48,
        barriers: ["Island isolation", "Natural disaster vulnerability", "Infrastructure costs", "Rural access"]
    },
    "Papua New Guinea": {
        internetPenetration: 15,
        mobileAccess: 38,
        averageSpeed: 13.2,
        digitalLiteracy: 19,
        barriers: ["Geography challenges", "Tribal isolation", "Infrastructure limitations", "Literacy barriers"]
    },
    "Iceland": {
        internetPenetration: 99,
        mobileAccess: 99,
        averageSpeed: 203.4,
        digitalLiteracy: 94,
        barriers: ["Remote area connectivity", "Extreme weather challenges", "Volcanic effects on infrastructure"]
    },
    "Malta": {
        internetPenetration: 91,
        mobileAccess: 95,
        averageSpeed: 138.5,
        digitalLiteracy: 80,
        barriers: ["Small market challenges", "Infrastructure maintenance", "Senior digital literacy"]
    },
    "Cyprus": {
        internetPenetration: 87,
        mobileAccess: 93,
        averageSpeed: 104.6,
        digitalLiteracy: 77,
        barriers: ["Political division", "Rural connectivity", "Age-related digital gaps"]
    },
    "Luxembourg": {
        internetPenetration: 99,
        mobileAccess: 99,
        averageSpeed: 187.3,
        digitalLiteracy: 92,
        barriers: ["Cross-border worker integration", "Multilingual barriers", "Digital skills training"]
    },
    "Estonia": {
        internetPenetration: 93,
        mobileAccess: 96,
        averageSpeed: 103.7,
        digitalLiteracy: 87,
        barriers: ["Rural connectivity", "Aging population", "Russian-speaking community integration"]
    },
    "Serbia": {
        internetPenetration: 79,
        mobileAccess: 90,
        averageSpeed: 74.8,
        digitalLiteracy: 61,
        barriers: ["Rural-urban divide", "Economic barriers", "Infrastructure quality", "Roma community access"]
    },
    "Croatia": {
        internetPenetration: 81,
        mobileAccess: 91,
        averageSpeed: 80.5,
        digitalLiteracy: 64,
        barriers: ["Island connectivity", "Rural access", "Seasonal infrastructure load", "Senior digital literacy"]
    },
    "Slovenia": {
        internetPenetration: 87,
        mobileAccess: 93,
        averageSpeed: 122.6,
        digitalLiteracy: 73,
        barriers: ["Mountain region connectivity", "Rural access", "Senior digital literacy"]
    },
    "Lithuania": {
        internetPenetration: 85,
        mobileAccess: 92,
        averageSpeed: 142.9,
        digitalLiteracy: 77,
        barriers: ["Rural connectivity", "Aging population", "Socioeconomic divide"]
    },
    "Latvia": {
        internetPenetration: 88,
        mobileAccess: 94,
        averageSpeed: 128.3,
        digitalLiteracy: 75,
        barriers: ["Rural-urban divide", "Aging population", "Economic barriers"]
    },
    "Afghanistan": {
        internetPenetration: 18,
        mobileAccess: 43,
        averageSpeed: 7.5,
        digitalLiteracy: 15,
        barriers: ["Political instability", "Infrastructure destruction", "Extreme poverty", "Gender restrictions"]
    },
    "Yemen": {
        internetPenetration: 29,
        mobileAccess: 55,
        averageSpeed: 11.7,
        digitalLiteracy: 22,
        barriers: ["Conflict damage", "Infrastructure destruction", "Economic crisis", "Electricity shortages"]
    },
    "Sudan": {
        internetPenetration: 31,
        mobileAccess: 57,
        averageSpeed: 15.4,
        digitalLiteracy: 26,
        barriers: ["Political instability", "Infrastructure limitations", "Affordability", "Literacy barriers"]
    },
    "Cuba": {
        internetPenetration: 58,
        mobileAccess: 65,
        averageSpeed: 16.8,
        digitalLiteracy: 49,
        barriers: ["Government restrictions", "Infrastructure limitations", "Affordability", "International connectivity"]
    },
    "North Korea": {
        internetPenetration: 0.1,
        mobileAccess: 10,
        averageSpeed: 2.5,
        digitalLiteracy: 15,
        barriers: ["Government restrictions", "International isolation", "Limited infrastructure", "Access control"]
    },
    "Turkmenistan": {
        internetPenetration: 25,
        mobileAccess: 58,
        averageSpeed: 13.2,
        digitalLiteracy: 32,
        barriers: ["Government restrictions", "Limited infrastructure", "Content filtering", "Affordability"]
    },
    "Uzbekistan": {
        internetPenetration: 55,
        mobileAccess: 73,
        averageSpeed: 30.6,
        digitalLiteracy: 41,
        barriers: ["Content restrictions", "Infrastructure limitations", "Rural connectivity", "Digital literacy"]
    },
    "Kyrgyzstan": {
        internetPenetration: 51,
        mobileAccess: 72,
        averageSpeed: 34.2,
        digitalLiteracy: 44,
        barriers: ["Mountain connectivity", "Rural access", "Affordability", "Infrastructure limitations"]
    },
    "Tajikistan": {
        internetPenetration: 34,
        mobileAccess: 62,
        averageSpeed: 18.7,
        digitalLiteracy: 32,
        barriers: ["Mountain terrain", "Infrastructure limitations", "Affordability", "Digital literacy"]
    },
    "Bhutan": {
        internetPenetration: 53,
        mobileAccess: 78,
        averageSpeed: 34.2,
        digitalLiteracy: 42,
        barriers: ["Mountain terrain", "Rural isolation", "Infrastructure costs", "Digital literacy"]
    },
    "Maldives": {
        internetPenetration: 76,
        mobileAccess: 89,
        averageSpeed: 81.3,
        digitalLiteracy: 58,
        barriers: ["Island distribution", "Climate vulnerability", "Infrastructure costs", "Digital skills gap"]
    }
  };

  // Create tooltip
  const tooltip = d3.select("body")
    .append("div")
    .attr("class", "tooltip")
    .style("opacity", 0);

  // Initialize the map
  function initMap() {
    const width = document.getElementById('map-container').clientWidth;
    const height = document.getElementById('map-container').clientHeight;
    
    // Create SVG element
    const svg = d3.select("#map-container")
      .append("svg")
      .attr("width", width)
      .attr("height", height);
    
    // Define map projection
    const projection = d3.geoNaturalEarth1()
      .scale(width / 5.5)
      .translate([width / 2, height / 2]);
    
    // Define path generator
    const path = d3.geoPath()
      .projection(projection);
    
    // Load world map data
    d3.json("https://cdn.jsdelivr.net/npm/world-atlas@2/countries-110m.json").then(function(data) {
      // Convert TopoJSON to GeoJSON
      const countries = topojson.feature(data, data.objects.countries).features;
      
      // Color scale based on internet penetration
      const colorScale = d3.scaleSequential(d3.interpolateBlues)
        .domain([0, 100]);
      
      // Add a background
      svg.append("rect")
        .attr("width", width)
        .attr("height", height)
        .attr("fill", "#f1f5f9");
      
      // Create a group for the map
      const mapGroup = svg.append("g");
      
      // Draw the map
      mapGroup.selectAll("path")
        .data(countries)
        .enter()
        .append("path")
        .attr("d", path)
        .attr("fill", function(d) {
          // Get the country name
          const countryName = getCountryName(d.id);
          // Get the data for this country if available
          const countryData = digitalDivideData[countryName];
          // Return color based on internet penetration or a default color
          return countryData ? colorScale(countryData.internetPenetration) : "#e0e0e0";
        })
        .attr("stroke", "#fff")
        .attr("stroke-width", 0.5)
        .attr("class", "country")
        .on("mouseover", function(event, d) {
          // Highlight the country
          d3.select(this)
            .attr("stroke-width", 1.5)
            .attr("stroke", "#333");
          
          // Get country name
          const countryName = getCountryName(d.id);
          const countryData = digitalDivideData[countryName];
          
          // Show tooltip if data exists
          if (countryData) {
            tooltip.transition()
              .duration(200)
              .style("opacity", 0.9);
            
            tooltip.html(`
              <strong>${countryName}</strong><br/>
              Internet: ${countryData.internetPenetration}%
            `)
              .style("left", (event.pageX + 10) + "px")
              .style("top", (event.pageY - 28) + "px");
          }
        })
        .on("mouseout", function() {
          // Restore the country appearance
          d3.select(this)
            .attr("stroke-width", 0.5)
            .attr("stroke", "#fff");
          
          // Hide tooltip
          tooltip.transition()
            .duration(500)
            .style("opacity", 0);
        })
        .on("click", function(event, d) {
          const countryName = getCountryName(d.id);
          showCountryDetails(countryName);
          
          // Highlight the selected country
          mapGroup.selectAll(".country")
            .attr("stroke-width", 0.5)
            .attr("stroke", "#fff");
          
          d3.select(this)
            .attr("stroke-width", 2)
            .attr("stroke", "#ff6b6b");
        });
      
      // Add a legend
      const legendWidth = 200;
      const legendHeight = 12;
      const legendX = width - legendWidth - 20;
      const legendY = height - 40;
      
      const legendScale = d3.scaleLinear()
        .domain([0, 100])
        .range([0, legendWidth]);
      
      const legendAxis = d3.axisBottom(legendScale)
        .tickValues([0, 25, 50, 75, 100])
        .tickFormat(d => d + "%")
        .tickSize(4);
      
      const legendGroup = svg.append("g")
        .attr("class", "legend-container");
      
      const defs = legendGroup.append("defs");
      const linearGradient = defs.append("linearGradient")
        .attr("id", "legend-gradient")
        .attr("x1", "0%")
        .attr("y1", "0%")
        .attr("x2", "100%")
        .attr("y2", "0%");
      
      linearGradient.append("stop")
        .attr("offset", "0%")
        .attr("stop-color", colorScale(0));
      
      linearGradient.append("stop")
        .attr("offset", "100%")
        .attr("stop-color", colorScale(100));
      
      legendGroup.append("rect")
        .attr("x", legendX)
        .attr("y", legendY)
        .attr("width", legendWidth)
        .attr("height", legendHeight)
        .attr("rx", 4)
        .style("fill", "url(#legend-gradient)");
      
      legendGroup.append("g")
        .attr("transform", `translate(${legendX},${legendY + legendHeight})`)
        .call(legendAxis)
        .selectAll("text")
        .style("font-size", "10px");
      
      legendGroup.append("text")
        .attr("x", legendX)
        .attr("y", legendY - 8)
        .attr("class", "legend-title")
        .text("Internet Penetration Rate");
    });
  }

  // Function to show country details
  function showCountryDetails(countryName) {
    const countryData = digitalDivideData[countryName];
    
    if (countryData) {
      // Hide the default info
      document.getElementById('default-info').style.display = 'none';
      
      // Show the details section
      document.getElementById('country-details').classList.remove('hidden');
      
      // Update country name
      document.getElementById('country-name').textContent = countryName;
      
      // Update progress bars with animation
      updateProgressBar('internet', countryData.internetPenetration, '%');
      updateProgressBar('mobile', countryData.mobileAccess, '%');
      updateProgressBar('speed', countryData.averageSpeed, ' Mbps', 300);
      updateProgressBar('literacy', countryData.digitalLiteracy, '%');
      
      // Update barriers list
      const barriersList = document.getElementById('barriers-list');
      barriersList.innerHTML = '';
      countryData.barriers.forEach(barrier => {
        const li = document.createElement('li');
        li.textContent = barrier;
        barriersList.appendChild(li);
      });
    } else {
      // If no data available for this country
      document.getElementById('default-info').style.display = 'block';
      document.getElementById('country-details').classList.add('hidden');
    }
  }

  // Helper function to update progress bars with animation
  function updateProgressBar(id, value, unit, max = 100) {
    const percentage = Math.min(value / max * 100, 100);
    const bar = document.getElementById(`${id}-bar`);
    const valueDisplay = document.getElementById(`${id}-value`);
    
    // Reset bar width
    bar.style.width = '0%';
    
    // Set text value
    valueDisplay.textContent = `${value}${unit}`;
    
    // Animate the bar
    setTimeout(() => {
      bar.style.width = `${percentage}%`;
    }, 50);
  }

  // Helper function to get country name from country ID
  function getCountryName(countryId) {
    // This is a simplified mapping of country IDs to names
    const countryMap = {
        840: "United States",
        124: "Canada",
        826: "United Kingdom",
        276: "Germany",
        250: "France",
        392: "Japan",
        410: "South Korea",
        156: "China",
        356: "India",
        76: "Brazil",
        484: "Mexico",
        710: "South Africa",
        566: "Nigeria",
        404: "Kenya",
        818: "Egypt",
        36: "Australia",
        643: "Russia",
        682: "Saudi Arabia",
        586: "Pakistan",
        360: "Indonesia",
        32: "Argentina",
        152: "Chile",
        704: "Vietnam",
        764: "Thailand",
        458: "Malaysia",
        608: "Philippines",
        50: "Bangladesh",
        231: "Ethiopia",
        288: "Ghana",
        504: "Morocco",
        376: "Israel",
        616: "Poland",
        752: "Sweden",
        792: "Turkey",
        170: "Colombia",
        604: "Peru",
        554: "New Zealand",
        702: "Singapore",
        724: "Spain",
        646: "Rwanda",
        380: "Italy",
        528: "Netherlands",
        578: "Norway",
        208: "Denmark",
        246: "Finland",
        756: "Switzerland",
        620: "Portugal",
        300: "Greece",
        40: "Austria",
        203: "Czech Republic",
        348: "Hungary",
        56: "Belgium",
        372: "Ireland",
        784: "United Arab Emirates",
        634: "Qatar",
        414: "Kuwait",
        804: "Ukraine",
        112: "Belarus",
        398: "Kazakhstan",
        12: "Algeria",
        788: "Tunisia",
        422: "Lebanon",
        400: "Jordan",
        512: "Oman",
        68: "Bolivia",
        218: "Ecuador",
        862: "Venezuela",
        600: "Paraguay",
        858: "Uruguay",
        591: "Panama",
        188: "Costa Rica",
        388: "Jamaica",
        780: "Trinidad and Tobago",
        214: "Dominican Republic",
        332: "Haiti",
        834: "Tanzania",
        800: "Uganda",
        508: "Mozambique",
        686: "Senegal",
        384: "Ivory Coast",
        72: "Botswana",
        516: "Namibia",
        120: "Cameroon",
        894: "Zambia",
        524: "Nepal",
        144: "Sri Lanka",
        104: "Myanmar",
        116: "Cambodia",
        418: "Laos",
        496: "Mongolia",
        242: "Fiji",
        598: "Papua New Guinea",
        352: "Iceland",
        470: "Malta",
        196: "Cyprus",
        442: "Luxembourg",
        233: "Estonia",
        688: "Serbia",
        191: "Croatia",
        705: "Slovenia",
        440: "Lithuania",
        428: "Latvia",
        4: "Afghanistan",
        887: "Yemen",
        729: "Sudan",
        192: "Cuba",
        408: "North Korea",
        795: "Turkmenistan",
        860: "Uzbekistan",
        417: "Kyrgyzstan",
        762: "Tajikistan",
        64: "Bhutan",
        462: "Maldives"
    };
    
    return countryMap[countryId] || "Unknown";
  }

  // Initialize the map when the page loads
  window.onload = function() {
    initMap();
  };

  // Handle window resize
  window.addEventListener('resize', function() {
    // Clear the map container
    document.getElementById('map-container').innerHTML = '';
    // Reinitialize the map
    initMap();
  });
</script>