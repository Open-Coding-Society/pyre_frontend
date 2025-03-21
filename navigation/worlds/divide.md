---
layout: base
title: Digital Divide Activity
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
      360: "Indonesia"
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