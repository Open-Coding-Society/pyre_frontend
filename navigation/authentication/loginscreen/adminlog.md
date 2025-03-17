---
layout: post
title: Admin Login
permalink: /adminlog
search_exclude: true
show_reading_time: false
---
<!DOCTYPE html>
<html>
<head>
    <title>NaviQ - Loading</title>
    <style>
        body, html {
            margin: 0;
            padding: 0;
            overflow: hidden;
            height: 100%;
            width: 100%;
            background-color: #000000;
            color: #ffffff;
            font-family: 'Arial', sans-serif;
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
            background-color: #000000;
            border: 2px solid #ff8a2c;
            box-shadow: 0 0 20px rgba(255, 138, 44, 0.5);
        }

        .cutscene {
            width: 100%;
            height: 100%;
            position: relative;
        }

        .welcome-message {
            position: absolute;
            top: 40%;
            width: 100%;
            text-align: center;
            font-size: 4rem;
            color: #12b5c8;
            text-shadow: 0 0 10px rgba(18, 181, 200, 0.7);
            font-weight: bold;
        }

        #welcome-text::after {
            content: '|';
            animation: blink 1s infinite;
        }

        .animation-elements {
            position: absolute;
            top: 60%;
            left: 50%;
            transform: translate(-50%, -50%);
            display: flex;
            gap: 40px;
            justify-content: center;
        }

        .circle {
            width: 50px;
            height: 50px;
            background-color: transparent;
            border: 3px solid #12b5c8;
            border-radius: 50%;
            animation: rotate 4s linear infinite, pulse 2s ease-in-out infinite;
        }

        .triangle {
            width: 50px;
            height: 43.3px;
            background-color: transparent;
            border: 3px solid #ff8a2c;
            clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
            animation: rotate 4s linear infinite reverse, pulse 2s ease-in-out infinite 0.5s;
        }

        .square {
            width: 50px;
            height: 50px;
            background-color: transparent;
            border: 3px solid #ffffff;
            animation: rotate 4s linear infinite, pulse 2s ease-in-out infinite 1s;
        }

        .loading-bar-container {
            position: absolute;
            bottom: 20%;
            width: 300px;
            height: 4px;
            background-color: #333333;
            left: 50%;
            transform: translateX(-50%);
            border-radius: 2px;
        }

        .loading-bar {
            height: 100%;
            width: 0%;
            background-color: #12b5c8;
            border-radius: 2px;
            transition: width 3s linear;
        }

        @keyframes fadeIn {
            0% { opacity: 0; }
            100% { opacity: 1; }
        }

        @keyframes rotate {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        @keyframes pulse {
            0% { opacity: 0.6; transform: scale(0.9); }
            50% { opacity: 1; transform: scale(1.1); }
            100% { opacity: 0.6; transform: scale(0.9); }
        }

        @keyframes blink {
            0%, 100% { opacity: 1; }
            50% { opacity: 0; }
        }
    </style>
</head>
<body>
    <div class="cutscene-container">
        <div class="cutscene">
            <!-- Welcome message animation -->
            <div class="welcome-message">
                <h1 id="welcome-text"></h1>
            </div>
            <!-- Animation elements -->
            <div class="animation-elements">
                <div class="circle"></div>
                <div class="triangle"></div>
                <div class="square"></div>
            </div>
            <!-- Loading bar -->
            <div class="loading-bar-container">
                <div class="loading-bar" id="loading-progress"></div>
            </div>
        </div>
    </div>

    <script>
        // Typing animation for the welcome message
        const text = "Welcome to NaviQ";
        let index = 0;
        const speed = 80; 

        function typeWriter() {
            if (index < text.length) {
                document.getElementById("welcome-text").innerHTML += text.charAt(index);
                index++;
                setTimeout(typeWriter, speed);
            }
        }

        // Start typing animation
        typeWriter();

        // Animate loading bar
        setTimeout(() => {
            document.getElementById("loading-progress").style.width = "100%";
        }, 100);

        // Redirect to index.md after the cutscene plays
        setTimeout(() => {
            window.location.href = "{{site.baseurl}}/index";
        }, 3000); // 3 second cutscene duration
    </script>
</body>
</html>