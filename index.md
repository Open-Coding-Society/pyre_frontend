---
layout: post
search_exclude: true
hide: true
show_reading_time: false 
---

<header class="hero">
  <div class="hero-content">
    <h1>NaviQ: Smart Streets</h1>
    <p>Enhancing autonomous vehicle navigation using Poway's transport data</p>
    <button class="cta-button" onclick="scrollToExplore()">Explore Our Solution</button>
  </div>
</header>

<section id="about" class="section">
  <div class="container">
    <h2>Smarter Navigation for a Faster Future</h2>
    <div class="feature-grid">
      <div class="feature-card">
        <div class="icon">🚗</div>
        <h3>Predictive Traffic Flow</h3>
        <p>Machine learning models that anticipate congestion before it happens</p>
      </div>
      <div class="feature-card">
        <div class="icon">⏱️</div>
        <h3>20% Reduced Travel Time</h3>
        <p>Our algorithms optimize routes for Poway commuters in real-time</p>
      </div>
      <div class="feature-card">
        <div class="icon">📊</div>
        <h3>Data-Driven Decisions</h3>
        <p>Leveraging Poway's Open Data Portal for smarter transportation</p>
      </div>
    </div>
  </div>
</section>

<section id="map" class="section map-section">
  <div class="container">
    <h2>Poway Smart Navigation Map</h2>
    <div class="map-placeholder">
      <p>Interactive map visualization coming soon</p>
      <div class="map-overlay">
        <div class="pulse-point" style="top: 45%; left: 30%;"></div>
        <div class="pulse-point" style="top: 25%; left: 60%;"></div>
        <div class="pulse-point" style="top: 70%; left: 50%;"></div>
      </div>
    </div>
  </div>
</section>

<section id="stats" class="section stats-section">
  <div class="container">
    <h2>Our Impact</h2>
    <div class="stats-grid">
      <div class="stat-card">
        <h3>20%</h3>
        <p>Reduction in travel time</p>
      </div>
      <div class="stat-card">
        <h3>15%</h3>
        <p>Decrease in traffic congestion</p>
      </div>
      <div class="stat-card">
        <h3>30%</h3>
        <p>Lower carbon emissions</p>
      </div>
    </div>
  </div>
</section>

<section id="contact" class="section">
    <div class="container">
        <h2>Get Involved</h2>
        <p>Interested in learning more about NaviQ or partnering with us?</p>
        <a href="mailto:placeholder@gmail.com" class="contact-button">Contact Us</a>
    </div>
</section>

<script>
function scrollToExplore() {
  document.getElementById('about').scrollIntoView({ behavior: 'smooth' });
}
</script>

<style>
  /* Color Palette */
  :root {
    --primary: #1a5b92;
    --secondary: #12b5c8;
    --background: #f7f9fb;
    --text: #2d3748;
    --accent: #ff8a2c;
    --light: #ffffff;
    --dark: #0a2e4a;
    --gray: #e2e8f0;
  }
  
  /* Base Styles */
  body {
    font-family: 'Segoe UI', Arial, sans-serif;
    line-height: 1.6;
    color: var(--text);
    background-color: var(--background);
    margin: 0;
    padding: 0;
  }
  
  .container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
  }
  
  .section {
    padding: 80px 0;
  }
  
  h1, h2, h3 {
    color: var(--primary);
    margin-top: 0;
  }
  
  /* Hero Section */
  .hero {
    background: linear-gradient(135deg, var(--primary), var(--secondary));
    color: var(--light);
    padding: 100px 0;
    text-align: center;
  }
  
  .hero h1 {
    font-size: 3.5rem;
    margin-bottom: 20px;
    color: var(--light);
  }
  
  .hero p {
    font-size: 1.2rem;
    margin-bottom: 30px;
    max-width: 600px;
    margin-left: auto;
    margin-right: auto;
  }
  
  .cta-button {
    background-color: var(--accent);
    color: var(--light);
    border: none;
    padding: 12px 30px;
    font-size: 1.1rem;
    border-radius: 50px;
    cursor: pointer;
    transition: all 0.3s ease;
  }
  
  .cta-button:hover {
    background-color: #e67a20;
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  }
  
  /* Feature Grid */
  .feature-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 30px;
    margin-top: 40px;
  }
  
  .feature-card {
    background-color: var(--light);
    border-radius: 8px;
    padding: 30px;
    text-align: center;
    box-shadow: 0 4px 6px rgba(0,0,0,0.05);
    transition: all 0.3s ease;
  }
  
  .feature-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 10px 20px rgba(0,0,0,0.1);
  }
  
  .icon {
    font-size: 2.5rem;
    margin-bottom: 20px;
  }
  
  /* Map Section */
  .map-placeholder {
    background-color: var(--light);
    border-radius: 8px;
    height: 400px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    overflow: hidden;
    box-shadow: 0 4px 6px rgba(0,0,0,0.05);
    background-image: url("data:image/svg+xml,%3Csvg width='100' height='100' viewBox='0 0 100 100' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M11 18c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm48 25c3.866 0 7-3.134 7-7s-3.134-7-7-7-7 3.134-7 7 3.134 7 7 7zm-43-7c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm63 31c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM34 90c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zm56-76c1.657 0 3-1.343 3-3s-1.343-3-3-3-3 1.343-3 3 1.343 3 3 3zM12 86c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm28-65c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm23-11c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-6 60c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm29 22c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zM32 63c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm57-13c2.76 0 5-2.24 5-5s-2.24-5-5-5-5 2.24-5 5 2.24 5 5 5zm-9-21c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM60 91c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM35 41c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2zM12 60c1.105 0 2-.895 2-2s-.895-2-2-2-2 .895-2 2 .895 2 2 2z' fill='%23e2e8f0' fill-opacity='0.4' fill-rule='evenodd'/%3E%3C/svg%3E");
  }
  
  .map-placeholder p {
    background-color: rgba(255,255,255,0.8);
    padding: 10px 20px;
    border-radius: 4px;
  }
  
  .map-overlay {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }
  
  .pulse-point {
    width: 20px;
    height: 20px;
    background-color: var(--secondary);
    border-radius: 50%;
    position: absolute;
    transform: translate(-50%, -50%);
  }
  
  .pulse-point::after {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: var(--secondary);
    border-radius: 50%;
    animation: pulse 2s infinite;
    opacity: 0.8;
  }
  
  @keyframes pulse {
    0% {
      transform: scale(1);
      opacity: 0.8;
    }
    70% {
      transform: scale(3);
      opacity: 0;
    }
    100% {
      transform: scale(1);
      opacity: 0;
    }
  }
  
  /* Stats Section */
  .stats-section {
    background-color: var(--dark);
    color: var(--light);
    text-align: center;
  }
  
  .stats-section h2 {
    color: var(--light);
  }
  
  .stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 30px;
    margin-top: 40px;
  }
  
  .stat-card {
    background-color: rgba(255,255,255,0.1);
    border-radius: 8px;
    padding: 30px;
  }
  
  .stat-card h3 {
    font-size: 2.5rem;
    margin: 0 0 10px 0;
    color: var(--accent);
  }
  
  /* Contact Section */
  #contact {
    text-align: center;
  }
  
  .contact-button {
    background-color: var(--primary);
    color: var(--light);
    border: none;
    padding: 12px 30px;
    font-size: 1.1rem;
    border-radius: 50px;
    cursor: pointer;
    transition: all 0.3s ease;
    margin-top: 20px;
  }
  
  .contact-button:hover {
    background-color: var(--dark);
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
  }
  
  /* Responsive Adjustments */
  @media (max-width: 768px) {
    .hero h1 {
      font-size: 2.5rem;
    }
    
    .section {
      padding: 60px 0;
    }
    
    .map-placeholder {
      height: 300px;
    }
  }
</style>