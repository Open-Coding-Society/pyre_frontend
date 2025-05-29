---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
---
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>California Is Burning</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.9.1/gsap.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.9.1/ScrollTrigger.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body, html {
            font-family: Arial, sans-serif;
            height: 100%;
            overflow-x: hidden;
            background-color: #000;
            color: #fff;
        }
        
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
        
        .hero-section {
            position: relative;
            height: 100vh;
            width: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            background-image: url('https://images.unsplash.com/photo-1497098478417-d823ef2eed8e?q=80&w=2940&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            z-index: 1;
        }
        
        .overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.6);
            z-index: 2;
        }
        
        .warning-icon {
            position: absolute;
            top: 6rem;
            left: 4rem;
            height: 4rem;
            width: 4rem;
            border: 2px solid #f97316;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #f97316;
            z-index: 10;
        }
        
        .warning-icon span {
            font-size: 2.5rem;
            font-weight: bold;
        }
        
        .secondary-nav {
            position: absolute;
            top: 6rem;
            right: 1.5rem;
            z-index: 10;
            display: flex;
            gap: 2rem;
        }
        
        .nav-link {
            color: white;
            font-size: 0.875rem;
            font-weight: 500;
            text-decoration: none;
            transition: color 0.3s;
        }
        
        .nav-link:hover {
            color: #ef4444;
        }
        
        .embers-container {
            position: absolute;
            inset: 0;
            overflow: hidden;
            pointer-events: none;
            z-index: 3;
        }
        
        .toast {
            position: fixed;
            top: 5rem;
            right: 1.5rem;
            z-index: 50;
            background-color: #111827;
            color: white;
            padding: 0.75rem 1rem;
            border-radius: 0.25rem;
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
            border-left: 4px solid #dc2626;
            transform: translateY(10px);
            opacity: 0;
            transition: all 0.3s;
            pointer-events: none;
            display: flex;
            align-items: center;
        }
        
        .toast.show {
            transform: translateY(0);
            opacity: 1;
        }
        
        .toast-icon {
            margin-right: 0.75rem;
            color: #ef4444;
        }
        
        .toast-content p:first-child {
            font-weight: 500;
        }
        
        .toast-content p:last-child {
            font-size: 0.75rem;
            color: #d1d5db;
        }
        
        .hero-content {
            position: relative;
            z-index: 10;
            max-width: 64rem;
            margin: 0 auto;
            text-align: center;
            padding: 0 1rem;
        }
        
        .hero-title {
            font-size: 4.5rem;
            font-weight: 700;
            margin-bottom: 1.5rem;
            letter-spacing: 0.05em;
            color: white;
            line-height: 1.1;
        }
        
        .hero-title span {
            color: #dc2626;
            display: block;
        }
        
        .cta-button {
            display: inline-block;
            margin-top: 4rem;
            border: 2px solid white;
            color: white;
            padding: 0.75rem 2rem;
            text-transform: uppercase;
            letter-spacing: 0.05em;
            font-weight: 500;
            text-decoration: none;
            transition: all 0.3s;
        }
        
        .cta-button:hover {
            background-color: white;
            color: black;
        }
        
        /* Stats Section */
        .stats-section {
            position: relative;
            padding: 8rem 1.5rem;
            background-color: #000;
            overflow: hidden;
        }
        
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 3rem;
            max-width: 1200px;
            margin: 0 auto;
        }
        
        .stat-card {
            text-align: center;
            opacity: 0;
            transform: translateY(30px);
            transition: all 0.8s ease;
        }
        
        .stat-card.visible {
            opacity: 1;
            transform: translateY(0);
        }
        
        .stat-number {
            font-size: 3.5rem;
            font-weight: 700;
            color: #ef4444;
            margin-bottom: 1rem;
        }
        
        .stat-label {
            font-size: 1.125rem;
            color: white;
        }
        
        /* Impact Section */
        .impact-section {
            position: relative;
            min-height: 100vh;
            display: flex;
            align-items: center;
            background-color: #111;
            overflow: hidden;
        }
        
        .impact-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 4rem 1.5rem;
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 4rem;
            align-items: center;
        }
        
        .impact-image {
            position: relative;
            height: 500px;
            border-radius: 0.5rem;
            overflow: hidden;
            transform: translateX(-100px);
            opacity: 0;
            transition: all 1s ease;
        }
        
        .impact-image.visible {
            transform: translateX(0);
            opacity: 1;
        }
        
        .impact-image img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }
        
        .impact-content {
            transform: translateX(100px);
            opacity: 0;
            transition: all 1s ease;
        }
        
        .impact-content.visible {
            transform: translateX(0);
            opacity: 1;
        }
        
        .impact-title {
            font-size: 2.5rem;
            font-weight: 700;
            color: white;
            margin-bottom: 1.5rem;
        }
        
        .impact-text {
            font-size: 1.125rem;
            color: #d1d5db;
            line-height: 1.7;
            margin-bottom: 2rem;
        }
        
        /* Earthquake Section */
        .earthquake-section {
            position: relative;
            height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #000;
            overflow: hidden;
        }
        
        .earthquake-container {
            position: relative;
            z-index: 10;
            text-align: center;
            max-width: 800px;
            padding: 0 1.5rem;
            opacity: 0;
            transition: opacity 1s ease;
        }
        
        .earthquake-container.visible {
            opacity: 1;
        }
        
        .earthquake-title {
            font-size: 3.5rem;
            font-weight: 700;
            color: white;
            margin-bottom: 2rem;
        }
        
        .earthquake-meter {
            width: 100%;
            height: 2rem;
            background-color: rgba(255, 255, 255, 0.1);
            position: relative;
            border-radius: 1rem;
            overflow: hidden;
            margin-bottom: 4rem;
        }
        
        .earthquake-progress {
            position: absolute;
            left: 0;
            top: 0;
            height: 100%;
            background: linear-gradient(90deg, #3b82f6, #ef4444);
            width: 0;
            transition: width 1.5s ease;
            border-radius: 1rem;
        }
        
        .earthquake-text {
            font-size: 1.125rem;
            color: #d1d5db;
            line-height: 1.7;
            max-width: 600px;
            margin: 0 auto;
        }

        /* Call to Action Section */
        .cta-section {
            position: relative;
            padding: 8rem 1.5rem;
            background-color: #dc2626;
            text-align: center;
        }
        
        .cta-content {
            max-width: 800px;
            margin: 0 auto;
        }
        
        .cta-heading {
            font-size: 3rem;
            font-weight: 700;
            color: white;
            margin-bottom: 1.5rem;
            transform: scale(0.8);
            opacity: 0;
            transition: all 0.8s ease;
        }
        
        .cta-heading.visible {
            transform: scale(1);
            opacity: 1;
        }
        
        .cta-text {
            font-size: 1.25rem;
            color: rgba(255, 255, 255, 0.9);
            margin-bottom: 3rem;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
            transform: translateY(30px);
            opacity: 0;
            transition: all 0.8s ease 0.2s;
        }
        
        .cta-text.visible {
            transform: translateY(0);
            opacity: 1;
        }
        
        .help-button {
            display: inline-block;
            background-color: white;
            color: #dc2626;
            padding: 1rem 2.5rem;
            font-size: 1.125rem;
            font-weight: 600;
            border-radius: 0.25rem;
            text-decoration: none;
            transition: all 0.3s ease;
            transform: translateY(30px);
            opacity: 0;
            transition-property: background-color, color, transform, opacity;
            transition-duration: 0.3s, 0.3s, 0.8s, 0.8s;
            transition-delay: 0s, 0s, 0.4s, 0.4s;
        }
        
        .help-button:hover {
            background-color: #f8fafc;
            transform: translateY(0) scale(1.05);
        }
        
        .help-button.visible {
            transform: translateY(0);
            opacity: 1;
        }
        
        /* Footer */
        .footer {
            background-color: #000;
            padding: 4rem 1.5rem;
            text-align: center;
        }
        
        .footer-text {
            color: #d1d5db;
            font-size: 0.875rem;
        }
        
        /* Media Queries */
        @media (min-width: 768px) {
            .hero-title {
                font-size: 6rem;
            }
        }
        
        @media (max-width: 768px) {
            .impact-container {
                grid-template-columns: 1fr;
            }
            
            .impact-image {
                height: 300px;
            }
        }
    </style>
</head>
<body>
    <!-- Hero Section -->
    <section class="hero-section">
        <div class="overlay"></div>
        
        <!-- Warning Icon -->
        <div class="warning-icon">
            <span>!</span>
        </div>
        
        <!-- Secondary navigation -->
        <div class="secondary-nav">
            <a href="{{ site.baseurl }}/media/" class="nav-link custom-nav-link">MEDIA</a>
            <a href="{{ site.baseurl }}/howtohelp/" class="nav-link custom-nav-link">HOW TO HELP</a>
            <button id="shareButton" class="nav-link custom-nav-link">SHARE</button>
        </div>
        <style>
            .custom-nav-link {
                color: #fff !important;
                transition: color 0.3s;
            }
            .custom-nav-link:hover, .custom-nav-link:focus {
                color: #f97316 !important;
            }
        </style>    
        <!-- Embers animation -->
        <div id="embers-container" class="embers-container"></div>
        
        <!-- Toast notification for clipboard -->
        <div id="toast" class="toast">
            <div class="toast-icon">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
                    <path d="M13 5H9.5C7.01 5 5 7.01 5 9.5C5 11.99 7.01 14 9.5 14H18C20.49 14 22.5 16.01 22.5 18.5C22.5 20.99 20.49 23 18 23H14.5"></path>
                    <path d="M8 14H18C20.5 14 22.5 12 22.5 9.5S20.5 5 18 5H8"></path>
                </svg>
            </div>
            <div class="toast-content">
                <p>Link copied to clipboard</p>
                <p>Share this wildfire awareness page</p>
            </div>
        </div>
        
        <!-- Content -->
        <div class="hero-content">
            <h1 class="hero-title">
                CALIFORNIA
                <span>IS BURNING</span>
            </h1>
            <a href="#stats" class="cta-button scroll-btn custom-cta-button">WHAT HAPPENED?</a>
            <style>
            .custom-cta-button {
                color: #fff !important;
                border-color: #fff !important;
                background: transparent !important;
                transition: color 0.3s, border-color 0.3s, background 0.3s;
            }
            .custom-cta-button:hover, .custom-cta-button:focus {
                color: #fff !important;
                background: #f97316 !important;
                border-color: #f97316 !important;
            }
            </style>    </div>
    </section>

    <!-- Stats Section -->
    <section id="stats" class="stats-section">
        <div class="stats-grid">
            <div class="stat-card">
                <h2 class="stat-number" id="acres-counter">0</h2>
                <p class="stat-label">ACRES BURNED THIS YEAR</p>
            </div>
            <div class="stat-card">
                <h2 class="stat-number" id="fires-counter">0</h2>
                <p class="stat-label">ACTIVE WILDFIRES</p>
            </div>
            <div class="stat-card">
                <h2 class="stat-number" id="homes-counter">0</h2>
                <p class="stat-label">HOMES DESTROYED</p>
            </div>
            <div class="stat-card">
                <h2 class="stat-number" id="displaced-counter">0</h2>
                <p class="stat-label">PEOPLE DISPLACED</p>
            </div>
        </div>
    </section>
    
    <!-- Impact Section -->
    <section class="impact-section">
        <div class="impact-container">
            <div class="impact-image">
                <img src="images/ABCNews.avif" alt="Wildfire Impact" />
            </div>
            <div class="impact-content">
                <h2 class="impact-title">DEVASTATING IMPACT</h2>
                <p class="impact-text">
                    The wildfires sweeping across California are causing unprecedented destruction to communities, ecosystems, and wildlife habitats. As climate change intensifies, these fires are becoming more frequent and severe, threatening the lives and livelihoods of millions.
                </p>
                <p class="impact-text">
                    Beyond the immediate danger, wildfires also release massive amounts of carbon dioxide into the atmosphere, creating a dangerous feedback loop that further accelerates climate change.
                </p>
                <p class="impact-text" style="font-size:0.85rem; color:#d1d5db; margin-top:1.5rem;">
                    Image source: <a href="https://abcnews.go.com/US/debunking-5-claims-california-wildfires/story?id=117549503" target="_blank" rel="noopener" style="color:#3b82f6;">ABC News</a>
                </p>
            </div>
        </div>
    </section>
    <!-- Earthquake Section -->
    <section class="earthquake-section">
        <div class="earthquake-container">
            <h2 class="earthquake-title">DOUBLE THREAT: EARTHQUAKES</h2>
            <div class="earthquake-meter">
                <div class="earthquake-progress"></div>
            </div>
            <p class="earthquake-text">
                California faces not only the threat of wildfires but also seismic activity. The combination of these natural disasters can compound damage, complicate evacuations, and stretch emergency resources to their limits. Being prepared for both threats is essential for all California residents.
            </p>
        </div>
    </section>
    
    <!-- Call to Action Section -->
    <section class="cta-section">
        <div class="cta-content">
            <h2 class="cta-heading">TAKE ACTION NOW</h2>
            <p class="cta-text">
                The time to act is now. Whether it's supporting firefighters, helping affected communities, preparing your own evacuation plan, or advocating for climate policy change, every action makes a difference in our fight against wildfires.
            </p>
            <a href="{{ site.baseurl }}/howtohelp/" class="help-button">HOW TO HELP</a>
        </div>
    </section>
    
    <!-- Footer -->
    <footer class="footer">
        <p class="footer-text">© 2025 California Wildfire & Earthquake Awareness</p>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', function() {
            // Initialize GSAP ScrollTrigger
            gsap.registerPlugin(ScrollTrigger);
            
            // Create floating embers effect
            const embersContainer = document.getElementById('embers-container');
            const shareButton = document.getElementById('shareButton');
            const toast = document.getElementById('toast');
            
            // Copy to clipboard function
            shareButton.addEventListener('click', function() {
                const linkToCopy = window.location.href;
                
                // Copy to clipboard
                navigator.clipboard.writeText(linkToCopy).then(function() {
                    // Show toast notification
                    toast.classList.add('show');
                    
                    // Hide toast after 3 seconds
                    setTimeout(function() {
                        toast.classList.remove('show');
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
            
            // Parallax effect for hero section
            gsap.to('.hero-section', {
                backgroundPosition: `50% ${window.innerHeight * 0.5}px`,
                ease: "none",
                scrollTrigger: {
                    trigger: '.hero-section',
                    start: "top top",
                    end: "bottom top",
                    scrub: true
                }
            });
            
            // Animation for stats counter
            function animateValue(id, start, end, duration) {
                let obj = document.getElementById(id);
                let startTimestamp = null;
                
                function step(timestamp) {
                    if (!startTimestamp) startTimestamp = timestamp;
                    const progress = Math.min((timestamp - startTimestamp) / duration, 1);
                    obj.innerHTML = Math.floor(progress * (end - start) + start).toLocaleString();
                    if (progress < 1) {
                        window.requestAnimationFrame(step);
                    }
                }
                
                window.requestAnimationFrame(step);
            }
            
            // Animation for stat cards
            const statCards = document.querySelectorAll('.stat-card');
            statCards.forEach((card) => {
                ScrollTrigger.create({
                    trigger: card,
                    start: "top 80%",
                    onEnter: () => {
                        card.classList.add('visible');
                        if (card.querySelector('.stat-number').id === 'acres-counter') {
                            animateValue('acres-counter', 0, 1850000, 2000);
                        } else if (card.querySelector('.stat-number').id === 'fires-counter') {
                            animateValue('fires-counter', 0, 24, 2000);
                        } else if (card.querySelector('.stat-number').id === 'homes-counter') {
                            animateValue('homes-counter', 0, 3200, 2000);
                        } else if (card.querySelector('.stat-number').id === 'displaced-counter') {
                            animateValue('displaced-counter', 0, 75000, 2000);
                        }
                    },
                    once: true
                });
            });
            
            // Animation for impact section
            ScrollTrigger.create({
                trigger: '.impact-section',
                start: "top 60%",
                onEnter: () => {
                    document.querySelector('.impact-image').classList.add('visible');
                    document.querySelector('.impact-content').classList.add('visible');
                },
                once: true
            });
            
            // Animation for earthquake section
            ScrollTrigger.create({
                trigger: '.earthquake-section',
                start: "top 60%",
                onEnter: () => {
                    document.querySelector('.earthquake-container').classList.add('visible');
                    setTimeout(() => {
                        document.querySelector('.earthquake-progress').style.width = '75%';
                    }, 500);
                },
                once: true
            });
            
            // Animation for CTA section
            ScrollTrigger.create({
                trigger: '.cta-section',
                start: "top 70%",
                onEnter: () => {
                    document.querySelector('.cta-heading').classList.add('visible');
                    document.querySelector('.cta-text').classList.add('visible');
                    document.querySelector('.help-button').classList.add('visible');
                },
                once: true
            });
            
            // Shake effect for earthquake section
            function shakeElement(element, magnitude = 16, duration = 1000) {
                const startTime = Date.now();
                const endTime = startTime + duration;
                
                function shake() {
                    const elapsed = Date.now() - startTime;
                    const remaining = Math.max(0, endTime - Date.now());
                    const magnitude_factor = magnitude * (remaining / duration);
                    
                    const x = Math.random() * magnitude_factor - magnitude_factor / 2;
                    const y = Math.random() * magnitude_factor - magnitude_factor / 2;
                    
                    element.style.transform = `translate(${x}px, ${y}px)`;
                    
                    if (remaining > 0) {
                        requestAnimationFrame(shake);
                    } else {
                        element.style.transform = '';
                    }
                }
                
                shake();
            }
            
            // Add earthquake shake effect when scrolling to that section
            ScrollTrigger.create({
                trigger: '.earthquake-section',
                start: "top 50%",
                onEnter: () => {
                    setTimeout(() => {
                        shakeElement(document.querySelector('.earthquake-container'), 8, 3000);
                    }, 1000);
                },
                once: true
            });
            
            // Smooth scroll for navigation
            document.querySelectorAll('.scroll-btn').forEach(anchor => {
                anchor.addEventListener('click', function(e) {
                    e.preventDefault();
                    const targetId = this.getAttribute('href');
                    const targetElement = document.querySelector(targetId);
                    
                    window.scrollTo({
                        top: targetElement.offsetTop,
                        behavior: 'smooth'
                    });
                });
            });
            
            // Add scroll-triggered ember intensity
            window.addEventListener('scroll', () => {
                const scrollPosition = window.scrollY;
                const windowHeight = window.innerHeight;
                const documentHeight = document.body.scrollHeight;
                
                // Increase ember intensity as user scrolls down
                const scrollPercentage = scrollPosition / (documentHeight - windowHeight);
                const emberRate = 300 - Math.min(250, scrollPercentage * 500); // Decrease interval (increase rate) as scroll percentage increases
                
                // Clear any existing intervals and set new one based on scroll position
                if (window.emberInterval) {
                    clearInterval(window.emberInterval);
                }
                
                window.emberInterval = setInterval(() => {
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
                }, emberRate);
            });
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