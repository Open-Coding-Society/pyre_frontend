---
layout: tailwind
title: Home Page
search_exclude: true
permalink: /home/
---


<!-- NOTE: I removed the below part from the class part of the overall div, when switching to the base layout -->
<!-- style="background-image: url('https://cdnjs.cloudflare.com/ajax/libs/particles.js/2.0.0/particles.min.js'); background-size: cover;" -->
<div class="min-h-screen bg-black text-gray-200">
  <!-- Background pattern -->
  <!-- <div class="absolute inset-0 bg-black bg-opacity-90 z-0" style="background-image: radial-gradient(circle, rgba(255, 100, 50, 0.1) 1px, transparent 1px); background-size: 30px 30px;"></div> -->
  <br>
  <br>

  <!-- Main Content -->
  <div class="relative z-10 container mx-auto px-4 py-12">
    <!-- Header -->
    <div class="text-center mb-12">
      <h1 class="text-4xl font-bold text-white mb-2">Welcome to the Pilot City Project</h1>
      <p class="text-xl text-gray-400">Pyre AI - Advanced wildfire monitoring and prediction system</p>
    </div>

    <!-- Menu Table Section -->
    <div class="mb-12">
      <div class="bg-black bg-opacity-60 p-6 rounded-lg mb-6">
        <p class="text-orange-500 mb-4">The table below is temporary. The website is still underdevelopment.</p>
        
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
          <a href="{{site.baseurl}}/TitanicSimulator" class="block bg-gray-900 border border-gray-800 p-6 text-center hover:bg-gray-800 transition-colors rounded">
            <span class="text-orange-500 font-medium text-lg">Titanic</span>
          </a>
          
          <a href="{{site.baseurl}}/thisorthat" class="block bg-gray-900 border border-gray-800 p-6 text-center hover:bg-gray-800 transition-colors rounded">
            <span class="text-orange-500 font-medium text-lg">Beneficial/Harmful Game</span>
          </a>
          
          <a href="{{site.baseurl}}/BinaryGame" class="block bg-gray-900 border border-gray-800 p-6 text-center hover:bg-gray-800 transition-colors rounded">
            <span class="text-orange-500 font-medium text-lg">Binary Logic Gates Game</span>
          </a>
          
          <a href="{{site.baseurl}}/digitalDivideActivity" class="block bg-gray-900 border border-gray-800 p-6 text-center hover:bg-gray-800 transition-colors rounded">
            <span class="text-orange-500 font-medium text-lg">Digital Divide Activity</span>
          </a>
        </div>
      </div>
    </div>

    <!-- Additional Content Section (Optional) -->
    <div class="bg-gradient-to-r from-orange-900 to-red-900 bg-opacity-40 p-8 rounded-lg mb-12">
      <div class="flex items-center mb-4">
        <span class="text-orange-400 text-2xl mr-3">🔥</span>
        <h2 class="text-2xl font-bold text-white">About Pyre</h2>
      </div>
      <p class="text-gray-200 leading-relaxed">
        Pyre combines cutting-edge technology with environmental science to protect communities and natural resources from the devastating impact of wildfires. Our platform empowers users with timely information and predictive insights to make informed decisions and take preventive actions.
      </p>
    </div>

    <!-- Feature Highlights (Optional) -->
    <div class="grid md:grid-cols-2 gap-6 mb-12">
      <div class="bg-gray-900 bg-opacity-80 backdrop-blur-sm p-6 rounded-lg border border-gray-800 hover:border-orange-900 transition duration-300">
        <div class="flex items-center mb-4">
          <span class="text-orange-500 text-2xl mr-3">🛰️</span>
          <h2 class="text-xl font-semibold text-white">Satellite Monitoring</h2>
        </div>
        <p class="text-gray-400">
          Our system utilizes real-time satellite data to track and monitor potential wildfire hotspots across various regions.
        </p>
      </div>
      
      <div class="bg-gray-900 bg-opacity-80 backdrop-blur-sm p-6 rounded-lg border border-gray-800 hover:border-orange-900 transition duration-300">
        <div class="flex items-center mb-4">
          <span class="text-orange-500 text-2xl mr-3">🤖</span>
          <h2 class="text-xl font-semibold text-white">AI Prediction</h2>
        </div>
        <p class="text-gray-400">
          Advanced AI algorithms analyze weather patterns, vegetation data, and historical fire incidents to predict high-risk areas.
        </p>
      </div>
    </div>
  </div>

  <!-- Footer -->
  <footer class="relative z-10 bg-black bg-opacity-80 py-8 border-t border-gray-800">
    <div class="container mx-auto px-4">
      <div class="flex flex-col md:flex-row justify-between items-center">
        <div class="mb-4 md:mb-0">
          <p class="text-gray-500">© 2025 Pyre. All rights reserved.</p>
        </div>
        <div class="flex space-x-4">
          <a href="#" class="text-gray-500 hover:text-orange-500 transition duration-200">Terms of Service</a>
          <a href="#" class="text-gray-500 hover:text-orange-500 transition duration-200">Privacy Policy</a>
          <a href="#" class="text-gray-500 hover:text-orange-500 transition duration-200">Contact</a>
        </div>
      </div>
    </div>
  </footer>
</div>