---
layout: post
permalink: /userlog
search_exclude: true
show_reading_time: false
---

<html>
<head>
    <title>Pyre - Loading</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            overflow: hidden;
            height: 100%;
            width: 100%;
            background-color: #0a0a0a;
            color: #ffffff;
            font-family: 'Montserrat', 'Arial', sans-serif;
        }

        /* Main loading container */
        .loading-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: radial-gradient(circle at center, #1a0000 0%, #000000 70%);
            z-index: 10000;
        }

        /* Crack system */
        .crack-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
        }

        .crack {
            position: absolute;
            height: 2px;
            background: linear-gradient(90deg, transparent, #ff6600, transparent);
            transform-origin: left center;
            opacity: 0;
            box-shadow: 0 0 20px #ff6600;
        }

        /* Pre-defined crack paths */
        .crack1 {
            width: 40%;
            top: 20%;
            left: 30%;
            transform: rotate(15deg);
        }

        .crack2 {
            width: 50%;
            top: 50%;
            left: 25%;
            transform: rotate(-30deg);
        }

        .crack3 {
            width: 35%;
            top: 70%;
            left: 35%;
            transform: rotate(45deg);
        }

        .crack4 {
            width: 60%;
            top: 35%;
            left: 20%;
            transform: rotate(-10deg);
        }

        .crack5 {
            width: 45%;
            top: 60%;
            left: 10%;
            transform: rotate(25deg);
        }

        /* Secondary cracks (smaller) */
        .crack-secondary {
            position: absolute;
            height: 1px;
            background: linear-gradient(90deg, transparent, #ff3300, transparent);
            transform-origin: left center;
            opacity: 0;
        }

        /* Fire container */
        .fire-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            opacity: 0;
            pointer-events: none;
        }

        .fire-particle {
            position: absolute;
            width: 60px;
            height: 60px;
            background: radial-gradient(circle, #ffff00 0%, #ff6600 30%, #ff0000 60%, transparent 100%);
            border-radius: 50%;
            filter: blur(2px);
            animation: fireRise 2s ease-out forwards;
        }

        @keyframes fireRise {
            0% {
                transform: translateY(100vh) scale(0);
                opacity: 0;
            }
            50% {
                opacity: 1;
            }
            100% {
                transform: translateY(-100px) scale(1.5);
                opacity: 0;
            }
        }

        /* Shatter effect */
        .shatter-piece {
            position: absolute;
            background: radial-gradient(circle at center, #1a0000 0%, #000000 70%);
            border: 1px solid rgba(255, 102, 0, 0.5);
            transform-origin: center;
            transition: none;
        }

        /* Welcome text */
        .welcome-text {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            text-align: center;
            opacity: 0;
            z-index: 100;
        }

        .welcome-text h1 {
            font-size: 5rem;
            font-weight: 900;
            letter-spacing: 5px;
            margin: 0;
            background: linear-gradient(90deg, #ff6600, #ffcc00, #ff3300);
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 40px rgba(255, 102, 0, 0.5);
        }

        .welcome-text p {
            font-size: 1.5rem;
            color: #ff6600;
            margin-top: 20px;
            letter-spacing: 2px;
            text-shadow: 0 0 20px rgba(255, 102, 0, 0.7);
        }

        /* Loading progress */
        .loading-progress {
            position: absolute;
            bottom: 10%;
            left: 50%;
            transform: translateX(-50%);
            text-align: center;
            z-index: 101;
        }

        .loading-bar-container {
            width: 300px;
            height: 4px;
            background: rgba(255, 255, 255, 0.1);
            border-radius: 2px;
            overflow: hidden;
            margin-bottom: 10px;
        }

        .loading-bar {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, #ff3300, #ff6600, #ffcc00);
            border-radius: 2px;
            transition: width 0.3s ease;
            box-shadow: 0 0 10px rgba(255, 102, 0, 0.7);
        }

        .loading-text {
            color: #ff6600;
            font-size: 0.9rem;
            text-shadow: 0 0 10px rgba(255, 102, 0, 0.5);
        }

        /* Animations */
        @keyframes crackAppear {
            0% {
                opacity: 0;
                transform: scaleX(0);
            }
            50% {
                opacity: 1;
                transform: scaleX(1);
            }
            100% {
                opacity: 1;
                transform: scaleX(1);
            }
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            10%, 30%, 50%, 70%, 90% { transform: translateX(-2px); }
            20%, 40%, 60%, 80% { transform: translateX(2px); }
        }

        @keyframes fadeIn {
            0% {
                opacity: 0;
                transform: translate(-50%, -50%) scale(0.5);
            }
            100% {
                opacity: 1;
                transform: translate(-50%, -50%) scale(1);
            }
        }

        @keyframes explode {
            0% {
                transform: scale(1) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: scale(0) rotate(720deg) translateY(500px);
                opacity: 0;
            }
        }

        /* Glow effect */
        .glow-orb {
            position: absolute;
            width: 200px;
            height: 200px;
            background: radial-gradient(circle, rgba(255, 102, 0, 0.8) 0%, transparent 60%);
            border-radius: 50%;
            filter: blur(40px);
            opacity: 0;
        }

        /* Start animations */
        .animate-crack {
            animation: crackAppear 0.5s ease-out forwards;
        }

        .shake-screen {
            animation: shake 0.5s;
        }

        .fade-in {
            animation: fadeIn 1s ease-out forwards;
        }

        .shatter-animate {
            animation: explode 1.5s ease-out forwards;
        }

        /* Responsive adjustments */
        @media (max-width: 768px) {
            .welcome-text h1 {
                font-size: 3rem;
            }
            .welcome-text p {
                font-size: 1rem;
            }
            .crack {
                height: 1px;
            }
        }
    </style>
</head>
<body>
    <div class="loading-container" id="loadingContainer">
        <!-- Cracks container -->
        <div class="crack-container" id="crackContainer">
            <div class="crack crack1"></div>
            <div class="crack crack2"></div>
            <div class="crack crack3"></div>
            <div class="crack crack4"></div>
            <div class="crack crack5"></div>
        </div>

        <!-- Fire container -->
        <div class="fire-container" id="fireContainer"></div>

        <!-- Glow effects -->
        <div class="glow-orb" id="glowOrb1" style="top: 30%; left: 20%;"></div>
        <div class="glow-orb" id="glowOrb2" style="top: 60%; left: 70%;"></div>
        <div class="glow-orb" id="glowOrb3" style="top: 50%; left: 50%;"></div>

        <!-- Welcome text -->
        <div class="welcome-text" id="welcomeText">
            <h1>PYRE</h1>
            <p>Prepare to be ignited</p>
        </div>

        <!-- Loading progress -->
        <div class="loading-progress">
            <div class="loading-bar-container">
                <div class="loading-bar" id="loadingBar"></div>
            </div>
            <div class="loading-text">Loading... <span id="loadingPercent">0%</span></div>
        </div>
    </div>

    <script>
        let loadingProgress = 0;
        const loadingBar = document.getElementById('loadingBar');
        const loadingPercent = document.getElementById('loadingPercent');
        const container = document.getElementById('loadingContainer');
        const crackContainer = document.getElementById('crackContainer');
        const fireContainer = document.getElementById('fireContainer');
        const welcomeText = document.getElementById('welcomeText');

        // Create secondary cracks
        function createSecondaryCracks() {
            for (let i = 0; i < 20; i++) {
                const crack = document.createElement('div');
                crack.className = 'crack-secondary';
                crack.style.width = Math.random() * 200 + 50 + 'px';
                crack.style.top = Math.random() * 100 + '%';
                crack.style.left = Math.random() * 100 + '%';
                crack.style.transform = `rotate(${Math.random() * 360}deg)`;
                crackContainer.appendChild(crack);
            }
        }

        // Create fire particles
        function createFireParticle(x, y) {
            const particle = document.createElement('div');
            particle.className = 'fire-particle';
            particle.style.left = x + 'px';
            particle.style.top = y + 'px';
            particle.style.width = Math.random() * 40 + 20 + 'px';
            particle.style.height = particle.style.width;
            particle.style.animationDelay = Math.random() * 0.5 + 's';
            fireContainer.appendChild(particle);

            setTimeout(() => particle.remove(), 2000);
        }

        // Create shatter pieces
        function createShatterPieces() {
            const pieces = 50;
            const width = window.innerWidth;
            const height = window.innerHeight;

            for (let i = 0; i < pieces; i++) {
                const piece = document.createElement('div');
                piece.className = 'shatter-piece';
                
                const size = Math.random() * 150 + 50;
                const x = Math.random() * width;
                const y = Math.random() * height;
                
                piece.style.width = size + 'px';
                piece.style.height = size + 'px';
                piece.style.left = x + 'px';
                piece.style.top = y + 'px';
                piece.style.clipPath = `polygon(${Math.random() * 100}% 0%, 100% ${Math.random() * 100}%, ${Math.random() * 100}% 100%, 0% ${Math.random() * 100}%)`;
                
                container.appendChild(piece);
                
                setTimeout(() => {
                    piece.classList.add('shatter-animate');
                }, 100 + i * 20);
            }
        }

        // Main sequence
        function startLoadingSequence() {
            createSecondaryCracks();

            // Loading progress
            const loadingInterval = setInterval(() => {
                if (loadingProgress < 100) {
                    loadingProgress += Math.random() * 3 + 1;
                    loadingProgress = Math.min(loadingProgress, 100);
                    loadingBar.style.width = loadingProgress + '%';
                    loadingPercent.textContent = Math.floor(loadingProgress) + '%';
                }

                // Start crack sequence at 40%
                if (loadingProgress >= 40 && !container.classList.contains('cracking')) {
                    container.classList.add('cracking');
                    startCrackSequence();
                }

                // Complete loading at 100%
                if (loadingProgress >= 100) {
                    clearInterval(loadingInterval);
                    setTimeout(finalExplosion, 500);
                }
            }, 50);
        }

        function startCrackSequence() {
            // Primary cracks appear
            const cracks = document.querySelectorAll('.crack');
            cracks.forEach((crack, index) => {
                setTimeout(() => {
                    crack.classList.add('animate-crack');
                    container.classList.add('shake-screen');
                    
                    // Add secondary cracks near primary
                    const secondaryCracks = document.querySelectorAll('.crack-secondary');
                    if (index < 5) {
                        for (let i = index * 4; i < (index + 1) * 4 && i < secondaryCracks.length; i++) {
                            setTimeout(() => {
                                secondaryCracks[i].classList.add('animate-crack');
                            }, Math.random() * 200);
                        }
                    }
                }, index * 300);
            });

            // Start glowing
            setTimeout(() => {
                document.getElementById('glowOrb1').style.opacity = '0.6';
                document.getElementById('glowOrb2').style.opacity = '0.6';
                document.getElementById('glowOrb3').style.opacity = '0.8';
            }, 1000);
        }

        function finalExplosion() {
            // Hide loading progress
            document.querySelector('.loading-progress').style.display = 'none';

            // Fire eruption
            fireContainer.style.opacity = '1';
            
            // Create fire particles from cracks
            const cracks = document.querySelectorAll('.crack');
            cracks.forEach((crack) => {
                const rect = crack.getBoundingClientRect();
                for (let i = 0; i < 5; i++) {
                    setTimeout(() => {
                        createFireParticle(
                            rect.left + Math.random() * rect.width,
                            rect.top + Math.random() * rect.height
                        );
                    }, i * 100);
                }
            });

            // Show welcome text
            setTimeout(() => {
                welcomeText.classList.add('fade-in');
            }, 500);

            // Shatter effect
            setTimeout(() => {
                createShatterPieces();
                container.style.background = 'transparent';
            }, 1500);

            // Final redirect
            setTimeout(() => {
                container.style.transition = 'opacity 1s';
                container.style.opacity = '0';
                setTimeout(() => {
                    window.location.href = "{{site.baseurl}}/";
                }, 1000);
            }, 3000);
        }

        // Start everything
        window.addEventListener('load', () => {
            startLoadingSequence();
        });

        // Add screen shake on mouse move (optional effect)
        let mouseTimeout;
        window.addEventListener('mousemove', (e) => {
            if (container.classList.contains('cracking')) {
                clearTimeout(mouseTimeout);
                container.style.transform = `translate(${(e.clientX - window.innerWidth/2) * 0.01}px, ${(e.clientY - window.innerHeight/2) * 0.01}px)`;
                mouseTimeout = setTimeout(() => {
                    container.style.transform = 'translate(0, 0)';
                }, 100);
            }
        });
    </script>
</body>
</html>