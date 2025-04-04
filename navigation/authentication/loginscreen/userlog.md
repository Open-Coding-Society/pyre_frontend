---
layout: post
permalink: /userlog
search_exclude: true
show_reading_time: false
---

<!DOCTYPE html>
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

        .cutscene-container {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            width: 100vw;
            text-align: center;
            position: fixed;
            top: 0;
            left: 0;
            z-index: 9999;
            background: linear-gradient(135deg, #0a0a0a 0%, #1a1a1a 100%);
            box-shadow: 0 0 30px rgba(255, 0, 0, 0.3);
        }

        .cutscene {
            width: 100%;
            height: 100%;
            position: relative;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: space-between;
            padding: 10% 0;
            box-sizing: border-box;
        }

        .welcome-message {
            position: relative;
            width: 100%;
            text-align: center;
            opacity: 0;
            animation: fadeIn 1s ease-out forwards 0.5s;
            margin-bottom: 5vh;
        }

        #welcome-text {
            font-size: 4rem;
            color: #ffffff;
            text-shadow: 0 0 15px rgba(255, 0, 0, 0.7);
            font-weight: 700;
            letter-spacing: 2px;
            margin: 0;
        }

        .subtitle {
            font-size: 1.2rem;
            color: #aaaaaa;
            margin-top: 10px;
            opacity: 0;
            animation: fadeIn 1s ease-out forwards 2s;
        }

        .animation-elements {
            position: relative;
            display: flex;
            gap: 40px;
            justify-content: center;
            margin: 5vh 0;
        }

        .circle {
            width: 50px;
            height: 50px;
            background-color: rgba(255, 0, 0, 0.1);
            border: 3px solid #ff3333;
            border-radius: 50%;
            animation: rotate 8s linear infinite, pulse 3s ease-in-out infinite;
            box-shadow: 0 0 15px rgba(255, 0, 0, 0.5);
        }

        .triangle {
            width: 50px;
            height: 43.3px;
            background-color: rgba(255, 255, 255, 0.1);
            border: 3px solid #ffffff;
            clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
            animation: rotate 8s linear infinite reverse, pulse 3s ease-in-out infinite 0.5s;
            box-shadow: 0 0 15px rgba(255, 255, 255, 0.5);
        }

        .square {
            width: 50px;
            height: 50px;
            background-color: rgba(255, 0, 0, 0.1);
            border: 3px solid #ff3333;
            animation: rotate 8s linear infinite, pulse 3s ease-in-out infinite 1s;
            box-shadow: 0 0 15px rgba(255, 0, 0, 0.5);
        }

        .loading-section {
            position: relative;
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            margin-top: 5vh;
        }

        .loading-bar-container {
            width: 300px;
            height: 6px;
            background-color: #333333;
            border-radius: 3px;
            overflow: hidden;
            margin-bottom: 15px;
        }

        .loading-bar {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, #ff3333, #ff6666);
            border-radius: 3px;
            box-shadow: 0 0 10px rgba(255, 0, 0, 0.7);
            transition: width 3s cubic-bezier(0.165, 0.84, 0.44, 1);
        }

        .loading-text {
            color: #aaaaaa;
            font-size: 0.8rem;
            opacity: 0.8;
        }

        .percentage {
            font-weight: bold;
            color: #ffffff;
        }

        @keyframes fadeIn {
            0% { opacity: 0; transform: translateY(20px); }
            100% { opacity: 1; transform: translateY(0); }
        }

        @keyframes rotate {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @keyframes pulse {
            0% { opacity: 0.7; transform: scale(0.9); filter: brightness(0.8); }
            50% { opacity: 1; transform: scale(1.1); filter: brightness(1.2); }
            100% { opacity: 0.7; transform: scale(0.9); filter: brightness(0.8); }
        }

        /* Background particles effect */
        .particles-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            z-index: -1;
        }

        .particle {
            position: absolute;
            width: 2px;
            height: 2px;
            background-color: rgba(255, 51, 51, 0.3);
            border-radius: 50%;
        }

        /* Responsive adjustments */
        @media (max-height: 600px) {
            .cutscene {
                padding: 5% 0;
            }
            #welcome-text {
                font-size: 3rem;
            }
            .circle, .square {
                width: 40px;
                height: 40px;
            }
            .triangle {
                width: 40px;
                height: 34.6px;
            }
            .animation-elements {
                gap: 30px;
                margin: 3vh 0;
            }
        }

        @media (max-width: 500px) {
            #welcome-text {
                font-size: 2.5rem;
            }
            .circle, .square {
                width: 30px;
                height: 30px;
            }
            .triangle {
                width: 30px;
                height: 26px;
            }
            .animation-elements {
                gap: 20px;
            }
            .loading-bar-container {
                width: 80%;
                max-width: 300px;
            }
        }
    </style>
</head>
<body>
    <div class="cutscene-container">
        <div class="particles-container" id="particles"></div>
        <div class="cutscene">
            <!-- Welcome message animation -->
            <div class="welcome-message">
                <h1 id="welcome-text"></h1>
                <div class="subtitle">Ignite your experience</div>
            </div>
            
            <!-- Animation elements -->
            <div class="animation-elements">
                <div class="circle"></div>
                <div class="triangle"></div>
                <div class="square"></div>
            </div>
            
            <!-- Loading section -->
            <div class="loading-section">
                <div class="loading-bar-container">
                    <div class="loading-bar" id="loading-progress"></div>
                </div>
                <div class="loading-text">
                    Loading... <span class="percentage" id="loading-percentage">0%</span>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Create background particles
        function createParticles() {
            const container = document.getElementById('particles');
            const particleCount = 50;
            
            for (let i = 0; i < particleCount; i++) {
                const particle = document.createElement('div');
                particle.classList.add('particle');
                
                // Random position
                const posX = Math.floor(Math.random() * window.innerWidth);
                const posY = Math.floor(Math.random() * window.innerHeight);
                
                // Random size
                const size = Math.random() * 3 + 1;
                
                // Random opacity
                const opacity = Math.random() * 0.5 + 0.3;
                
                // Set styles
                particle.style.left = posX + 'px';
                particle.style.top = posY + 'px';
                particle.style.width = size + 'px';
                particle.style.height = size + 'px';
                particle.style.opacity = opacity;
                
                // Random animation
                const animDuration = Math.random() * 20 + 10;
                particle.style.animation = `pulse ${animDuration}s infinite ease-in-out`;
                particle.style.animationDelay = (Math.random() * 5) + 's';
                
                container.appendChild(particle);
            }
        }

        // Typing animation for the welcome message
        const text = "Welcome to Pyre";
        let index = 0;
        const speed = 100; 

        function typeWriter() {
            if (index < text.length) {
                document.getElementById("welcome-text").innerHTML += text.charAt(index);
                index++;
                setTimeout(typeWriter, speed);
            }
        }

        // Loading progress animation
        function animateLoading() {
            const loadingBar = document.getElementById("loading-progress");
            const percentageText = document.getElementById("loading-percentage");
            let progress = 0;
            
            const interval = setInterval(() => {
                if (progress >= 100) {
                    clearInterval(interval);
                    setTimeout(() => {
                        // Redirect to index.md after completion
                        window.location.href = "{{site.baseurl}}/index";
                    }, 500);
                } else {
                    // Calculate a realistic loading curve
                    const increment = (progress < 70) ? 
                        Math.random() * 3 + 1 : 
                        Math.random() * 1 + 0.3;
                    
                    progress = Math.min(progress + increment, 100);
                    loadingBar.style.width = progress + "%";
                    percentageText.textContent = Math.floor(progress) + "%";
                }
            }, 100);
        }

        // Initialize everything when page loads
        window.onload = function() {
            createParticles();
            typeWriter();
            setTimeout(animateLoading, 500);
        };
    </script>
</body>
</html>
