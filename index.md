---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
---
<!-- Hero Section -->
<div class="relative min-h-screen flex flex-col justify-center items-center" style="background-image: url('https://images.unsplash.com/photo-1497098478417-d823ef2eed8e?q=80&w=2940&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'); background-size: cover; background-position: center;">
    <!-- Dark overlay -->
    <div class="absolute inset-0 bg-black opacity-60"></div>
    <!-- Warning Icon -->
    <div class="absolute top-24 left-16 lg:left-24 text-orange-500 z-10">
        <div class="h-16 w-16 rounded-full border-2 border-orange-500 flex items-center justify-center">
            <span class="text-4xl">!</span>
        </div>
    </div>
    <!-- Secondary navigation -->
    <div class="absolute top-24 right-6 z-10">
        <div class="flex space-x-8">
            <a href="{{ site.baseurl }}/whathappened/" class="text-white hover:text-red-500 transition-colors duration-300 text-sm font-medium">WHAT HAPPENED</a>
            <a href="{{ site.baseurl }}/media/" class="text-white hover:text-red-500 transition-colors duration-300 text-sm font-medium">MEDIA</a>
            <a href="{{ site.baseurl }}/howtohelp/" class="text-white hover:text-red-500 transition-colors duration-300 text-sm font-medium">HOW TO HELP</a>
            <button id="shareButton" class="text-white hover:text-red-500 transition-colors duration-300 text-sm font-medium">SHARE</button>
        </div>
    </div>
    <!-- Embers animation -->
    <div id="embers-container" class="absolute inset-0 overflow-hidden pointer-events-none"></div>
    <!-- Toast notification for clipboard -->
    <div id="toast" class="fixed top-20 right-6 z-50 bg-gray-900 text-white py-3 px-4 rounded shadow-lg border-l-4 border-red-600 transform translate-y-10 opacity-0 transition-all duration-300 pointer-events-none">
        <div class="flex items-center">
            <div class="mr-3">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-red-500">
                    <path d="M13 5H9.5C7.01 5 5 7.01 5 9.5C5 11.99 7.01 14 9.5 14H18C20.49 14 22.5 16.01 22.5 18.5C22.5 20.99 20.49 23 18 23H14.5"></path>
                    <path d="M8 14H18C20.5 14 22.5 12 22.5 9.5S20.5 5 18 5H8"></path>
                </svg>
            </div>
            <div>
                <p class="font-medium">Link copied to clipboard</p>
                <p class="text-xs text-gray-300">Share this wildfire awareness page</p>
            </div>
        </div>
    </div>
    <!-- Content -->
    <div class="relative z-10 max-w-5xl mx-auto text-center px-4">
        <h1 class="text-7xl md:text-9xl font-bold mb-6 tracking-wide text-white">
            CALIFORNIA
            <br/>
            <span class="text-red-600">IS BURNING</span>
        </h1>
        <div class="mt-16">
            <a href="https://www.fire.ca.gov/incidents" class="inline-block border-2 border-white hover:bg-white hover:text-black transition-colors duration-300 px-8 py-3 uppercase tracking-wide font-medium text-white">
                WHAT HAPPENED?
            </a>
        </div>
    </div>
</div>

<script>
    // Create floating embers effect
    document.addEventListener('DOMContentLoaded', function() {
        const embersContainer = document.getElementById('embers-container');
        const shareButton = document.getElementById('shareButton');
        const toast = document.getElementById('toast');
        
        // Copy to clipboard function
        shareButton.addEventListener('click', function() {
            const linkToCopy = 'https://sanpranav.github.io/QcommVNE_Frontend/';
            
            // Copy to clipboard
            navigator.clipboard.writeText(linkToCopy).then(function() {
                // Show toast notification
                toast.classList.remove('translate-y-10', 'opacity-0');
                toast.classList.add('translate-y-0', 'opacity-100');
                
                // Hide toast after 3 seconds
                setTimeout(function() {
                    toast.classList.add('translate-y-10', 'opacity-0');
                    toast.classList.remove('translate-y-0', 'opacity-100');
                }, 3000);
            }).catch(function(err) {
                console.error('Could not copy text: ', err);
            });
        });
        
        function createEmbers(count) {
            for (let i = 0; i < count; i++) {
                const ember = document.createElement('div');
                ember.classList.add('ember');
                
                // Random positions and delays
                const size = Math.random() * 4 + 2;
                const startPositionX = Math.random() * window.innerWidth;
                const delay = Math.random() * 15;
                const duration = 5 + Math.random() * 10;
                
                ember.style.width = `${size}px`;
                ember.style.height = `${size}px`;
                ember.style.left = `${startPositionX}px`;
                ember.style.bottom = `0px`;
                ember.style.animationDuration = `${duration}s`;
                ember.style.animationDelay = `${delay}s`;
                
                embersContainer.appendChild(ember);
            }
        }
        
        // Create initial set of embers
        createEmbers(100);
        
        // Create new embers periodically
        setInterval(() => {
            const newEmbers = document.createElement('div');
            newEmbers.classList.add('ember');
            
            const size = Math.random() * 4 + 2;
            const startPositionX = Math.random() * window.innerWidth;
            const duration = 5 + Math.random() * 10;
            
            newEmbers.style.width = `${size}px`;
            newEmbers.style.height = `${size}px`;
            newEmbers.style.left = `${startPositionX}px`;
            newEmbers.style.bottom = `0px`;
            newEmbers.style.animationDuration = `${duration}s`;
            
            embersContainer.appendChild(newEmbers);
            
            // Remove ember after animation completes
            setTimeout(() => {
                newEmbers.remove();
            }, duration * 1000);
        }, 300);
    });
</script>

<style>
    .ember {
        position: absolute;
        background-color: #ff6600;
        border-radius: 50%;
        box-shadow: 0 0 10px 2px rgba(255, 102, 0, 0.7);
        animation-name: float;
        animation-iteration-count: infinite;
        animation-timing-function: ease-out;
        pointer-events: none;
    }
    
    @keyframes float {
        0% {
            transform: translateY(0) scale(1);
            opacity: 1;
        }
        100% {
            transform: translateY(-100vh) scale(0.3);
            opacity: 0;
        }
    }
</style>