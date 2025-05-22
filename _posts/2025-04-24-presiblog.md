---
layout: tailwind
search_exclude: false
show_reading_time: false
permalink: /PyreInfo
title: Pyre Overview
categories: [Documentation]
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pyre: Transforming Fire Management</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            background-color: rgb(0, 0, 0);
            color: #333;
        }
        p {
            color: #eee;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        .logo {
            width: 80px;
            height: 80px;
            background-color: #000;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 15px;
        }
        .logo svg {
            width: 50px;
            height: 50px;
            fill: #ff5500;
        }
        h1 {
            margin: 0;
            font-size: 36px;
            font-weight: 800;
        }
        .subtitle {
            margin-top: 5px;
            font-size: 18px;
            opacity: 0.9;
        }
        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-top: 20px;
        }
        .card {
            background: black;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            padding: 20px;
            transition: transform 0.3s, box-shadow 0.3s;
        }
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 16px rgba(0,0,0,0.15);
        }
        .card-header {
            display: flex;
            align-items: center;
            margin-bottom: 15px;
            color: #ff5500;
        }
        .card-header h2 {
            margin: 0 0 0 10px;
            font-size: 20px;
            color: #eee;
        }
        .stats-container {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
        }
        .stat-box {
            background: #fff4ee;
            border-radius: 8px;
            padding: 15px;
            text-align: center;
            border-left: 4px solid #ff5500;
        }
        .stat-number {
            font-size: 32px;
            font-weight: 800;
            color: #ff5500;
            margin: 0;
        }
        .stat-label {
            font-size: 14px;
            color: #666;
            margin: 5px 0 0;
        }
        .chart-container {
            height: 200px;
            margin: 20px 0;
            position: relative;
        }
        .bar-chart {
            display: flex;
            align-items: flex-end;
            height: 100%;
            padding-top: 20px;
        }
        .bar {
            flex: 1;
            margin: 0 5px;
            background: linear-gradient(to top, #ff5500, #ff8c00);
            border-radius: 5px 5px 0 0;
            position: relative;
            transition: height 0.5s;
        }
        .bar:hover {
            background: linear-gradient(to top, #ff3300, #ff7300);
        }
        .bar-label {
            position: absolute;
            bottom: -25px;
            left: 0;
            right: 0;
            text-align: center;
            font-size: 12px;
        }
        .bar-value {
            position: absolute;
            top: -25px;
            left: 0;
            right: 0;
            text-align: center;
            font-size: 12px;
            font-weight: bold;
        }
        .list {
            list-style-type: none;
            padding: 0;
            margin: 0;
        }
        .list li {
            padding: 10px 0;
            border-bottom: 1px solid #eee;
            display: flex;
            align-items: flex-start;
        }
        .list li:last-child {
            border-bottom: none;
        }
        .list li:before {
            content: "•";
            color: #ff5500;
            font-weight: bold;
            margin-right: 10px;
        }
        .highlight {
            font-weight: 600;
        }
        .footer {
            text-align: center;
            margin-top: 30px;
            padding: 20px;
            font-size: 12px;
            color: #777;
            border-top: 1px solid #eee;
        }
        .tooltip {
            position: relative;
            display: inline-block;
            cursor: pointer;
        }
        .tooltip .tooltip-text {
            visibility: hidden;
            width: 200px;
            background-color: rgba(0,0,0,0.8);
            color: #fff;
            text-align: center;
            border-radius: 6px;
            padding: 5px;
            position: absolute;
            z-index: 1;
            bottom: 125%;
            left: 50%;
            margin-left: -100px;
            opacity: 0;
            transition: opacity 0.3s;
            font-size: 12px;
        }
        .tooltip:hover .tooltip-text {
            visibility: visible;
            opacity: 1;
        }
        @media (max-width: 768px) {
            .grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <!-- <div class="container">
        <header>
        </header>
        
        <div class="grid">
            <div class="card">
                <div class="card-header">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M12 2L4 8.5V20H20V8.5L12 2Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                        <path d="M12 15C13.6569 15 15 13.6569 15 12C15 10.3431 13.6569 9 12 9C10.3431 9 9 10.3431 9 12C9 13.6569 10.3431 15 12 15Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                    <h2>About Pyre</h2>
                </div>
                <p>Pyre is a comprehensive fire management platform that integrates advanced technology, data analytics, and machine learning to revolutionize how we predict, prevent, and respond to wildfires. Our solution bridges critical gaps in current fire management systems, providing actionable intelligence to decision-makers.</p>
            </div>
            
            <div class="card">
                <div class="card-header">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2C6.47715 2 2 6.47715 2 12C2 17.5228 6.47715 22 12 22Z" stroke="currentColor" stroke-width="2"/>
                        <path d="M12 16V12" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
                        <path d="M12 8H12.01" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
                    </svg>
                    <h2>Current Challenges</h2>
                </div>
                <ul class="list">
                    <li class="tooltip">Resource limitations: <span class="highlight"> 40% shortage </span> in firefighting personnel
                        <span class="tooltip-text">Based on National Fire Protection Association data</span>
                    </li>
                    <li class="tooltip">Climate change extending fire seasons by <span class="highlight">78 days</span> since 1970
                        <span class="tooltip-text">Source: U.S. Forest Service Research</span>
                    </li>
                    <li class="tooltip"><span class="highlight">46% increase</span> in populated areas at high risk
                        <span class="tooltip-text">Growth in wildland-urban interface zones</span>
                    </li>
                    <li class="tooltip"><span class="highlight">65% of detection systems</span> are over 15 years old
                        <span class="tooltip-text">Based on infrastructure assessment reports</span>
                    </li>
                </ul>
            </div>
        </div>
        
        <div class="grid">
            <div class="card">
                <div class="card-header">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M18 6L6 18" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                        <path d="M6 6L18 18" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                    <h2>Why Current Approaches Fail</h2>
                </div>
                <ul class="list">
                    <li class="tooltip"><span class="highlight">75% of funding</span> goes to suppression, only 25% to prevention
                        <span class="tooltip-text">Budget allocation shows reactionary approach</span>
                    </li>
                    <li class="tooltip"><span class="highlight">58% of agencies</span> report difficulty sharing critical information
                        <span class="tooltip-text">Data silos prevent effective coordination</span>
                    </li>
                    <li class="tooltip">Current predictive models have <span class="highlight">42% accuracy limitations</span>
                        <span class="tooltip-text">Outdated modeling technology hinders prevention</span>
                    </li>
                    <li class="tooltip">Only <span class="highlight">23% of recommended burns</span> are completed annually
                        <span class="tooltip-text">Prescribed burning implementation gap</span>
                    </li>
                </ul>
            </div>
            
            <div class="card">
                <div class="card-header">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M16 6L12 2L8 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                        <path d="M12 2V15" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                        <path d="M22 14L18 18L14 14" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                        <path d="M18 18V12" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                        <path d="M10 14L6 18L2 14" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                        <path d="M6 18V12" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                    <h2>Key Statistics</h2>
                </div>
                <div class="stats-container">
                    <div class="stat-box">
                        <p class="stat-number">58K+</p>
                        <p class="stat-label">Annual Wildfires</p>
                    </div>
                    <div class="stat-box">
                        <p class="stat-number">7.1M</p>
                        <p class="stat-label">Acres Burned Annually</p>
                    </div>
                    <div class="stat-box">
                        <p class="stat-number">$4.3B</p>
                        <p class="stat-label">Annual Suppression Cost</p>
                    </div>
                    <div class="stat-box">
                        <p class="stat-number">71%</p>
                        <p class="stat-label">Human-Caused Fires</p>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="grid">
            <div class="card">
                <div class="card-header">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M8.5 14.5A2.5 2.5 0 0011 12c0-1.38-.5-2-1-3-1.072-2.143-.224-4.054 2-6 .5 2.5 2 4.9 4 6.5 2 1.6 3 3.5 3 5.5a7 7 0 11-14 0c0-1.153.433-2.294 1-3a2.5 2.5 0 002.5 2.5z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                    <h2>Annual Wildfires Trend</h2>
                </div>
                <div class="chart-container">
                    <div class="bar-chart">
                        <div class="bar" style="height: 80%;">
                            <span class="bar-value">58,083</span>
                            <span class="bar-label">2018</span>
                        </div>
                        <div class="bar" style="height: 70%;">
                            <span class="bar-value">50,477</span>
                            <span class="bar-label">2019</span>
                        </div>
                        <div class="bar" style="height: 81%;">
                            <span class="bar-value">58,950</span>
                            <span class="bar-label">2020</span>
                        </div>
                        <div class="bar" style="height: 80%;">
                            <span class="bar-value">58,733</span>
                            <span class="bar-label">2021</span>
                        </div>
                        <div class="bar" style="height: 95%;">
                            <span class="bar-value">68,988</span>
                            <span class="bar-label">2022</span>
                        </div>
                        <div class="bar" style="height: 78%;">
                            <span class="bar-value">56,580</span>
                            <span class="bar-label">2023</span>
                        </div>
                    </div>
                </div>
            </div>
            
            <div class="card">
                <div class="card-header">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2C6.47715 2 2 6.47715 2 12C2 17.5228 6.47715 22 12 22Z" stroke="currentColor" stroke-width="2"/>
                        <path d="M12 6V12L16 14" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                    <h2>Acres Burned (in millions)</h2>
                </div>
                <div class="chart-container">
                    <div class="bar-chart">
                        <div class="bar" style="height: 87%;">
                            <span class="bar-value">8.8M</span>
                            <span class="bar-label">2018</span>
                        </div>
                        <div class="bar" style="height: 46%;">
                            <span class="bar-value">4.7M</span>
                            <span class="bar-label">2019</span>
                        </div>
                        <div class="bar" style="height: 100%;">
                            <span class="bar-value">10.1M</span>
                            <span class="bar-label">2020</span>
                        </div>
                        <div class="bar" style="height: 70%;">
                            <span class="bar-value">7.1M</span>
                            <span class="bar-label">2021</span>
                        </div>
                        <div class="bar" style="height: 74%;">
                            <span class="bar-value">7.5M</span>
                            <span class="bar-label">2022</span>
                        </div>
                        <div class="bar" style="height: 27%;">
                            <span class="bar-value">2.7M</span>
                            <span class="bar-label">2023</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="grid">
            <div class="card">
                <div class="card-header">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M5 3v4M3 5h4M6 17v4M4 19h4M13 3l4 4M17 3h-4v4M15 21l-2-2-2 2-2-2" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                    <h2>Our Solution</h2>
                </div>
                <ul class="list">
                    <li class="tooltip"><span class="highlight">Real-time analytics:</span> Processes data <span class="highlight">85% faster</span> than traditional systems
                        <span class="tooltip-text">Using edge computing and optimized algorithms</span>
                    </li>
                    <li class="tooltip"><span class="highlight">Unified data platform:</span> Connects <span class="highlight">7 different data sources</span>
                        <span class="tooltip-text">Integration of satellite, sensor, weather, and historical data</span>
                    </li>
                    <li class="tooltip"><span class="highlight">AI-driven prediction:</span> Increases detection accuracy by <span class="highlight">67%</span>
                        <span class="tooltip-text">Using custom machine learning models</span>
                    </li>
                    <li class="tooltip"><span class="highlight">Resource optimization:</span> Reduces response time by <span class="highlight">53%</span>
                        <span class="tooltip-text">Through intelligent deployment algorithms</span>
                    </li>
                </ul>
            </div>
            
            <div class="card">
                <div class="card-header">
                    <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path d="M22 12H16L14 15H10L8 12H2" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                        <path d="M5.45 5.11L2 12V18C2 18.5304 2.21071 19.0391 2.58579 19.4142C2.96086 19.7893 3.46957 20 4 20H20C20.5304 20 21.0391 19.7893 21.4142 19.4142C21.7893 19.0391 22 18.5304 22 18V12L18.55 5.11C18.3844 4.77679 18.1292 4.49637 17.813 4.30028C17.4967 4.10419 17.1321 4.0002 16.76 4H7.24C6.86792 4.0002 6.50326 4.10419 6.18704 4.30028C5.87083 4.49637 5.61558 4.77679 5.45 5.11Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    </svg>
                    <h2>Technical Innovations</h2>
                </div>
                <ul class="list">
                    <li>Multi-source data integration framework</li>
                    <li>Edge computing implementation for remote sensor networks</li>
                    <li>Custom machine learning models trained on historical fire behavior</li>
                    <li>Scalable cloud infrastructure for peak fire season demands</li>
                    <li>Real-time notification system with geo-targeting capabilities</li>
                </ul>
            </div>
        </div>
        
        <div class="footer">
            Data sources: National Interagency Fire Center, U.S. Forest Service, and Congressional Research Service (2023)
        </div>
    </div> -->

<div class="container">
    <header>
    </header>
    
    <div class="grid">
        <div class="card">
            <div class="card-header">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M12 2L4 8.5V20H20V8.5L12 2Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M12 15C13.6569 15 15 13.6569 15 12C15 10.3431 13.6569 9 12 9C10.3431 9 9 10.3431 9 12C9 13.6569 10.3431 15 12 15Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
                <h2>About Pyre</h2>
            </div>
            <p>Pyre is a comprehensive fire management platform that integrates advanced technology, data analytics, and machine learning to revolutionize how we predict, prevent, and respond to wildfires. Our solution addresses critical challenges in wildfire management including climate change, human-caused ignitions, resource limitations, and urban expansion into fire-prone regions.</p>
        </div>
        
        <div class="card">
            <div class="card-header">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2C6.47715 2 2 6.47715 2 12C2 17.5228 6.47715 22 12 22Z" stroke="currentColor" stroke-width="2"/>
                    <path d="M12 16V12" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
                    <path d="M12 8H12.01" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
                </svg>
                <h2>Current Challenges</h2>
            </div>
            <ul class="list">
                <li class="tooltip">Resource limitations: <span class="highlight">50% drop</span> in firefighter applications
                    <span class="tooltip-text">Based on 2022 data with regions hitting only half their staffing goals</span>
                </li>
                <li class="tooltip">Climate change shifting peak fire season from <span class="highlight">August to July</span>
                    <span class="tooltip-text">Source: US EPA data on earlier, hotter springs</span>
                </li>
                <li class="tooltip"><span class="highlight">90% of wildfires</span> are human-induced
                    <span class="tooltip-text">Sources: World Wildlife Fund, Axios</span>
                </li>
                <li class="tooltip"><span class="highlight">Urban expansion</span> into fire-prone regions
                    <span class="tooltip-text">Increasing risk to lives and infrastructure</span>
                </li>
            </ul>
        </div>
    </div>
    
    <div class="grid">
        <div class="card">
            <div class="card-header">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M18 6L6 18" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M6 6L18 18" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
                <h2>Why Current Approaches Fail</h2>
            </div>
            <ul class="list">
                <li class="tooltip">Suppression-focused approach causing <span class="highlight">fuel buildup</span>
                    <span class="tooltip-text">Decades of prioritizing suppression over ecosystem management</span>
                </li>
                <li class="tooltip"><span class="highlight">Underutilization</span> of prescribed burns
                    <span class="tooltip-text">Effective but rarely implemented due to regulations and resources</span>
                </li>
                <li class="tooltip">Inconsistent policies across <span class="highlight">jurisdictions</span>
                    <span class="tooltip-text">Creates fragmented, inefficient responses</span>
                </li>
                <li class="tooltip"><span class="highlight">72.6% of 2023 fires</span> were human-caused
                    <span class="tooltip-text">Peaked at 77.2% in 2020, indicating worsening trend</span>
                </li>
            </ul>
        </div>
        
        <div class="card">
            <div class="card-header">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M16 6L12 2L8 6" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M12 2V15" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M22 14L18 18L14 14" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M18 18V12" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M10 14L6 18L2 14" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M6 18V12" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
                <h2>Key Statistics</h2>
            </div>
            <div class="stats-container">
                <div class="stat-box">
                    <p class="stat-number">70K</p>
                    <p class="stat-label">Avg. Annual Fires</p>
                </div>
                <div class="stat-box">
                    <p class="stat-number">8.9M</p>
                    <p class="stat-label">Acres Burned 2024</p>
                </div>
                <div class="stat-box">
                    <p class="stat-number">4.3M</p>
                    <p class="stat-label">CA Acres Burned 2020</p>
                </div>
                <div class="stat-box">
                    <p class="stat-number">72.6%</p>
                    <p class="stat-label">Human-Caused Fires</p>
                </div>
            </div>
        </div>
    </div>
    
    <div class="grid">
        <div class="card">
            <div class="card-header">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M8.5 14.5A2.5 2.5 0 0011 12c0-1.38-.5-2-1-3-1.072-2.143-.224-4.054 2-6 .5 2.5 2 4.9 4 6.5 2 1.6 3 3.5 3 5.5a7 7 0 11-14 0c0-1.153.433-2.294 1-3a2.5 2.5 0 002.5 2.5z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
                <h2>Annual Wildfires Trend</h2>
            </div>
            <div class="chart-container">
                <div class="bar-chart">
                    <div class="bar" style="height: 80%;">
                        <span class="bar-value">58,083</span>
                        <span class="bar-label">2018</span>
                    </div>
                    <div class="bar" style="height: 70%;">
                        <span class="bar-value">50,477</span>
                        <span class="bar-label">2019</span>
                    </div>
                    <div class="bar" style="height: 81%;">
                        <span class="bar-value">58,950</span>
                        <span class="bar-label">2020</span>
                    </div>
                    <div class="bar" style="height: 80%;">
                        <span class="bar-value">58,733</span>
                        <span class="bar-label">2021</span>
                    </div>
                    <div class="bar" style="height: 95%;">
                        <span class="bar-value">68,988</span>
                        <span class="bar-label">2022</span>
                    </div>
                    <div class="bar" style="height: 78%;">
                        <span class="bar-value">56,580</span>
                        <span class="bar-label">2023</span>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="card">
            <div class="card-header">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M12 22C17.5228 22 22 17.5228 22 12C22 6.47715 17.5228 2 12 2C6.47715 2 2 6.47715 2 12C2 17.5228 6.47715 22 12 22Z" stroke="currentColor" stroke-width="2"/>
                    <path d="M12 6V12L16 14" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
                <h2>Acres Burned (in millions)</h2>
            </div>
            <div class="chart-container">
                <div class="bar-chart">
                    <div class="bar" style="height: 87%;">
                        <span class="bar-value">8.8M</span>
                        <span class="bar-label">2018</span>
                    </div>
                    <div class="bar" style="height: 46%;">
                        <span class="bar-value">4.7M</span>
                        <span class="bar-label">2019</span>
                    </div>
                    <div class="bar" style="height: 100%;">
                        <span class="bar-value">10.1M</span>
                        <span class="bar-label">2020</span>
                    </div>
                    <div class="bar" style="height: 70%;">
                        <span class="bar-value">7.1M</span>
                        <span class="bar-label">2021</span>
                    </div>
                    <div class="bar" style="height: 74%;">
                        <span class="bar-value">7.5M</span>
                        <span class="bar-label">2022</span>
                    </div>
                    <div class="bar" style="height: 27%;">
                        <span class="bar-value">2.7M</span>
                        <span class="bar-label">2023</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
<!--     
    <div class="grid">
        <div class="card">
            <div class="card-header">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M5 3v4M3 5h4M6 17v4M4 19h4M13 3l4 4M17 3h-4v4M15 21l-2-2-2 2-2-2" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
                <h2>Our Solution</h2>
            </div>
            <ul class="list">
                <li class="tooltip"><span class="highlight">Climate analysis:</span> Tracking shifting fire seasons and <span class="highlight">temperature impacts</span>
                    <span class="tooltip-text">Addressing the earlier July peak fire season</span>
                </li>
                <li class="tooltip"><span class="highlight">Cross-jurisdiction platform:</span> Connects <span class="highlight">fragmented response systems</span>
                    <span class="tooltip-text">Integration of data across different management authorities</span>
                </li>
                <li class="tooltip"><span class="highlight">Prescribed burn planning:</span> Increases implementation by <span class="highlight">40%</span>
                    <span class="tooltip-text">Using ecosystem-based management approaches</span>
                </li>
                <li class="tooltip"><span class="highlight">Human ignition prevention:</span> Targets the <span class="highlight">72.6%</span> human-caused fires
                    <span class="tooltip-text">Through education and monitoring of high-risk activities</span>
                </li>
            </ul>
        </div>
        
        <div class="card">
            <div class="card-header">
                <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M22 12H16L14 15H10L8 12H2" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                    <path d="M5.45 5.11L2 12V18C2 18.5304 2.21071 19.0391 2.58579 19.4142C2.96086 19.7893 3.46957 20 4 20H20C20.5304 20 21.0391 19.7893 21.4142 19.4142C21.7893 19.0391 22 18.5304 22 18V12L18.55 5.11C18.3844 4.77679 18.1292 4.49637 17.813 4.30028C17.4967 4.10419 17.1321 4.0002 16.76 4H7.24C6.86792 4.0002 6.50326 4.10419 6.18704 4.30028C5.87083 4.49637 5.61558 4.77679 5.45 5.11Z" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
                <h2>Technical Innovations</h2>
            </div>
            <ul class="list">
                <li>Predictive modeling tools for climate-driven fire escalation</li>
                <li>Resource allocation algorithms to address staffing shortages</li>
                <li>Inter-jurisdictional coordination system for consistent policies</li>
                <li>Urban-wildland interface risk assessment mapping</li>
                <li>Real-time notification system for early fire detection</li>
            </ul>
        </div>
    </div>
    
    <div class="footer">
        Data sources: National Interagency Fire Center, U.S. Forest Service, EPA, CalFire, and Congressional Research Service (2023-2024)
    </div> -->
</div>



    <script>
        // Simple hover interaction for bars
        document.querySelectorAll('.bar').forEach(bar => {
            bar.addEventListener('mouseenter', () => {
                bar.style.height = `${parseInt(bar.style.height) + 5}%`;
            });
            
            bar.addEventListener('mouseleave', () => {
                bar.style.height = `${parseInt(bar.style.height) - 5}%`;
            });
        });
        
        // Card interaction
        document.querySelectorAll('.card').forEach(card => {
            card.addEventListener('mouseenter', () => {
                card.style.transform = 'translateY(-5px)';
                card.style.boxShadow = '0 8px 16px rgba(0,0,0,0.15)';
            });
            
            card.addEventListener('mouseleave', () => {
                card.style.transform = 'translateY(0)';
                card.style.boxShadow = '0 4px 8px rgba(0,0,0,0.1)';
            });
        });
    </script>
</body>
</html>