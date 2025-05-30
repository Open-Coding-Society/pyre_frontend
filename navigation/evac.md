---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /evac/
title: Evacuation
---

<div class="w-full max-w-6xl mx-auto p-4">
  <div class="bg-white rounded-lg shadow-lg overflow-hidden">
    <div class="p-4 bg-red-600 text-white">
      <h2 class="text-2xl font-bold text-white">Fire Evacuation Routes</h2>
      <p class="mt-1 text-white">Find the safest evacuation routes from your current location</p>
    </div>
    <div class="p-4 bg-gray-50 flex flex-wrap items-center gap-2">
      <div class="flex-grow">
        <input id="address-input" type="text" placeholder="Enter your location or address"
               class="w-full px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-red-500">
      </div>
      <button id="find-routes-btn"
              class="px-4 py-2 bg-red-600 text-white rounded-md hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-red-500">
        Find Routes
      </button>
    </div>
    <div id="location-error" class="hidden p-3 bg-yellow-100 border-l-4 border-yellow-500 text-yellow-700">
      <p>Could not determine your location. Please enter an address manually.</p>
    </div>
    <div class="flex flex-col md:flex-row">
      <!-- Map Container -->
      <div class="w-full md:w-2/3">
        <div id="evacuation-map" class="h-96 md:h-[600px]"></div>
      </div>
      <div class="w-full md:w-1/3 border-l border-gray-200">
        <div class="p-4">
          <h3 class="font-bold text-lg mb-3 text-gray-800">Evacuation Routes</h3>
          <div id="evacuation-routes" class="overflow-y-auto max-h-60">
            <!-- Route cards will be added here dynamically -->
          </div>
          <div class="mt-6">
            <div id="route-details" class="overflow-y-auto max-h-80">
              <!-- Turn-by-turn directions will be added here -->
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
    API_KEY = "AIzaSyDbp8o8s5zAm-H2MqYNGDSZCQGO9mQhmSY"

    // Fire Evacuation Map with Google Maps Directions API
    // This code should be added to your existing JS file

    // Initialize the map and directions service when the maps API is loaded
    function initEvacuationMap() {
    // Create map centered at default location (can be adjusted based on user's location)
    const map = new google.maps.Map(document.getElementById("evacuation-map"), {
        zoom: 14,
        center: { lat: 34.0522, lng: -118.2437 }, // Default center (Los Angeles)
        mapTypeId: google.maps.MapTypeId.ROADMAP
    });
    
    // Create the directions service and renderer
    const directionsService = new google.maps.DirectionsService();
    const directionsRenderer = new google.maps.DirectionsRenderer({
        map: map,
        polylineOptions: {
        strokeColor: "#4CAF50", // Green route line
        strokeWeight: 6
        }
    });
    
    // Get user's current location if geolocation is available
    if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
        (position) => {
            const userLocation = {
            lat: position.coords.latitude,
            lng: position.coords.longitude
            };
            
            // Center map on user's location
            map.setCenter(userLocation);
            
            // Create marker for current location
            new google.maps.Marker({
            position: userLocation,
            map: map,
            icon: {
                path: google.maps.SymbolPath.CIRCLE,
                scale: 8,
                fillColor: "#4285F4",
                fillOpacity: 1,
                strokeWeight: 2,
                strokeColor: "#FFFFFF"
            },
            title: "Your Location"
            });
            
            // Find evacuation routes
            findEvacuationRoutes(userLocation, directionsService, directionsRenderer, map);
        },
        (error) => {
            console.error("Error getting user location:", error);
            document.getElementById("location-error").classList.remove("hidden");
        }
        );
    } else {
        document.getElementById("location-error").classList.remove("hidden");
    }
    
    // Setup event listeners for manual location entry
    document.getElementById("find-routes-btn").addEventListener("click", () => {
        const address = document.getElementById("address-input").value;
        if (address) {
        const geocoder = new google.maps.Geocoder();
        geocoder.geocode({ address: address }, (results, status) => {
            if (status === "OK" && results[0]) {
            const location = results[0].geometry.location;
            map.setCenter(location);
            findEvacuationRoutes(location, directionsService, directionsRenderer, map);
            } else {
            alert("Could not find location: " + status);
            }
        });
        }
    });
    }

    // Find and display evacuation routes
    function findEvacuationRoutes(startLocation, directionsService, directionsRenderer, map) {
    // In a real implementation, you would get evacuation points from your fire prediction system
    // For this example, we'll use hardcoded safe locations (emergency shelters, etc.)
    const evacuationPoints = [
        { lat: startLocation.lat + 0.02, lng: startLocation.lng + 0.02, name: "Emergency Shelter A" },
        { lat: startLocation.lat - 0.015, lng: startLocation.lng + 0.025, name: "Emergency Shelter B" },
        { lat: startLocation.lat + 0.025, lng: startLocation.lng - 0.02, name: "Emergency Shelter C" }
    ];
    
    // Clear existing routes info
    const routesContainer = document.getElementById("evacuation-routes");
    routesContainer.innerHTML = "";
    
    // Calculate routes to all evacuation points
    const routes = [];
    
    evacuationPoints.forEach((point, index) => {
        // Add markers for evacuation points
        new google.maps.Marker({
        position: point,
        map: map,
        icon: {
            url: "https://maps.google.com/mapfiles/ms/icons/green-dot.png"
        },
        title: point.name
        });
        
        // Request directions
        directionsService.route(
        {
            origin: startLocation,
            destination: point,
            travelMode: google.maps.TravelMode.DRIVING,
            provideRouteAlternatives: true,
            avoidHighways: false,
            avoidTolls: false
        },
        (response, status) => {
            if (status === "OK") {
            const route = response.routes[0];
            routes.push({
                destination: point.name,
                distance: route.legs[0].distance.text,
                duration: route.legs[0].duration.text,
                instructions: route.legs[0].steps,
                route: route
            });
            
            // Create route info card with explicit text colors
            const routeCard = document.createElement("div");
            routeCard.className = "bg-white rounded-lg shadow-md p-4 mb-4 cursor-pointer hover:bg-gray-50";
            routeCard.innerHTML = `
                <h3 class="font-bold text-lg text-gray-800">${point.name}</h3>
                <div class="flex items-center mt-2">
                <svg class="w-5 h-5 text-gray-500 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"></path>
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"></path>
                </svg>
                <span class="text-gray-700">${route.legs[0].distance.text}</span>
                </div>
                <div class="flex items-center mt-1">
                <svg class="w-5 h-5 text-gray-500 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                </svg>
                <span class="text-gray-700">${route.legs[0].duration.text}</span>
                </div>
                <div class="mt-3 pt-3 border-t border-gray-200">
                    <button class="bg-transparent text-blue-600 hover:text-blue-800 font-medium px-2 py-1 rounded">View Directions</button>
                </div>
            `;
            
            // Add event listener to show this route
            routeCard.addEventListener("click", () => {
                directionsRenderer.setDirections(response);
                showRouteDetails(route.legs[0]);
            });
            
            routesContainer.appendChild(routeCard);
            
            // Show first route by default if this is the first one
            if (index === 0) {
                directionsRenderer.setDirections(response);
                showRouteDetails(route.legs[0]);
            }
            } else {
            console.error("Directions request failed due to " + status);
            }
        }
        );
    });
    }

    // Show turn-by-turn directions for the selected route
    function showRouteDetails(routeLeg) {
    const detailsContainer = document.getElementById("route-details");
    detailsContainer.innerHTML = "";
    
    const heading = document.createElement("h3");
    heading.className = "font-bold text-xl mb-3 text-gray-800";
    heading.textContent = "Turn-by-Turn Directions";
    detailsContainer.appendChild(heading);
    
    const stepsList = document.createElement("ol");
    stepsList.className = "list-decimal pl-5";
    
    routeLeg.steps.forEach(step => {
        const stepItem = document.createElement("li");
        stepItem.className = "mb-3 text-gray-800";
        
        // Create a div for the instruction
        const instruction = document.createElement("div");
        instruction.className = "mb-1 text-gray-800";
        instruction.innerHTML = step.instructions;
        
        // Create a div for the distance and duration
        const details = document.createElement("div");
        details.className = "text-sm text-gray-600";
        details.textContent = `${step.distance.text} · ${step.duration.text}`;
        
        stepItem.appendChild(instruction);
        stepItem.appendChild(details);
        stepsList.appendChild(stepItem);
    });
    
    detailsContainer.appendChild(stepsList);
    }

    // Load Google Maps API
    function loadGoogleMapsAPI() {
    const script = document.createElement("script");
    script.src = `https://maps.googleapis.com/maps/api/js?key=AIzaSyDbp8o8s5zAm-H2MqYNGDSZCQGO9mQhmSY&callback=initEvacuationMap`;
    script.defer = true;
    document.body.appendChild(script);
    }

    // Call this function to load the map when your page is ready
    window.addEventListener("DOMContentLoaded", loadGoogleMapsAPI);
</script>

<a href="/pyre_frontend/help/" class="fixed bottom-4 right-4 bg-green-600 text-white rounded-full p-3 shadow-lg hover:bg-green-700 transition duration-200 flex items-center justify-center" title="Help Center" style="font-size:1.05em;">
    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9 5.25h.008v.008H12v-.008z"/>
    </svg>
    <span class="ml-1 font-medium">Help</span>
  </a>