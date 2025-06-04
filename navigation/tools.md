---
layout: base_chat
search_exclude: true
hide: true
show_reading_time: false
permalink: /tools/
title: Tools
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tools</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg,rgb(0, 0, 0) 0%,rgb(0, 0, 0) 50%,rgb(0, 0, 0) 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .tools-container {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(15px);
            border-radius: 25px;
            padding: 50px 40px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            text-align: center;
            max-width: 900px;
            width: 100%;
        }

        .tools-title {
            color: white;
            font-size: 2.8rem;
            font-weight: 700;
            margin-bottom: 15px;
            text-shadow: 0 3px 6px rgba(0, 0, 0, 0.3);
        }

        .tools-subtitle {
            color: rgba(255, 255, 255, 0.9);
            font-size: 1.2rem;
            margin-bottom: 50px;
            font-weight: 300;
            line-height: 1.5;
        }

        .tools-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 25px;
            margin-bottom: 30px;
        }

        .tool-button {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 180px;
            background: rgba(255, 255, 255, 0.12);
            backdrop-filter: blur(8px);
            border: 2px solid rgba(255, 255, 255, 0.15);
            border-radius: 20px;
            text-decoration: none;
            color: white;
            font-weight: 600;
            font-size: 1.1rem;
            transition: all 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }

        .tool-button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: left 0.6s;
        }

        .tool-button:hover::before {
            left: 100%;
        }

        .tool-button:hover {
            transform: translateY(-8px) scale(1.02);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.2);
            border-color: rgba(255, 255, 255, 0.4);
            background: rgba(255, 255, 255, 0.2);
        }

        .tool-icon {
            font-size: 3.5rem;
            margin-bottom: 15px;
            filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.3));
        }

        .tool-title {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 5px;
        }

        .tool-description {
            font-size: 0.9rem;
            opacity: 0.8;
            font-weight: 300;
            text-align: center;
            line-height: 1.3;
        }

        /* Unique colors for each tool */
        .earthquake-tool {
            background: linear-gradient(135deg, rgba(231, 76, 60, 0.15), rgba(192, 57, 43, 0.15));
        }

        .earthquake-tool:hover {
            background: linear-gradient(135deg, rgba(231, 76, 60, 0.25), rgba(192, 57, 43, 0.25));
        }

        .fire-tool {
            background: linear-gradient(135deg, rgba(230, 126, 34, 0.15), rgba(211, 84, 0, 0.15));
        }

        .fire-tool:hover {
            background: linear-gradient(135deg, rgba(230, 126, 34, 0.25), rgba(211, 84, 0, 0.25));
        }

        .notification-tool {
            background: linear-gradient(135deg, rgba(52, 152, 219, 0.15), rgba(41, 128, 185, 0.15));
        }

        .notification-tool:hover {
            background: linear-gradient(135deg, rgba(52, 152, 219, 0.25), rgba(41, 128, 185, 0.25));
        }

        .risk-tool {
            background: linear-gradient(135deg, rgba(155, 89, 182, 0.15), rgba(142, 68, 173, 0.15));
        }

        .risk-tool:hover {
            background: linear-gradient(135deg, rgba(155, 89, 182, 0.25), rgba(142, 68, 173, 0.25));
        }

        .evac-tool {
            background: linear-gradient(135deg, rgba(46, 204, 113, 0.15), rgba(39, 174, 96, 0.15));
        }

        .evac-tool:hover {
            background: linear-gradient(135deg, rgba(46, 204, 113, 0.25), rgba(39, 174, 96, 0.25));
        }

        .dashboard-tool {
            background: linear-gradient(135deg, rgba(52, 73, 94, 0.15), rgba(44, 62, 80, 0.15));
        }

        .dashboard-tool:hover {
            background: linear-gradient(135deg, rgba(52, 73, 94, 0.25), rgba(44, 62, 80, 0.25));
        }

        @media (max-width: 768px) {
            .tools-container {
                padding: 30px 20px;
            }

            .tools-title {
                font-size: 2.2rem;
            }

            .tools-grid {
                grid-template-columns: 1fr;
                gap: 20px;
            }

            .tool-button {
                height: 160px;
            }

            .tool-icon {
                font-size: 3rem;
            }
        }

        @media (max-width: 480px) {
            .tools-grid {
                grid-template-columns: 1fr;
            }
        }

        .loading-animation {
            opacity: 0;
            transform: translateY(30px);
            animation: fadeInUp 0.6s ease forwards;
        }

        @keyframes fadeInUp {
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @keyframes ripple {
            to {
                transform: scale(4);
                opacity: 0;
            }
        }
    </style>
</head>
<body>
    <div class="tools-container">
        <h1 class="tools-title">Emergency Tools</h1>
        <p class="tools-subtitle">Comprehensive emergency management and monitoring tools</p>
        
        <div class="tools-grid">
            <a href="{{ site.baseurl }}/notification/" class="tool-button notification-tool loading-animation">
                <div class="tool-icon">🔔</div>
                <div class="tool-title">Notifications</div>
                <div class="tool-description">Alert system and updates</div>
            </a>
            
            <a href="{{ site.baseurl }}/risk-predictor/" class="tool-button risk-tool loading-animation">
                <div class="tool-icon">📊</div>
                <div class="tool-title">Risk Predictor</div>
                <div class="tool-description">AI-powered risk assessment</div>
            </a>
            
            <a href="{{ site.baseurl }}/evac/" class="tool-button evac-tool loading-animation">
                <div class="tool-icon">🚨</div>
                <div class="tool-title">Evacuation</div>
                <div class="tool-description">Emergency evacuation planning</div>
            </a>
        </div>
    </div>

    <script>
        // Staggered loading animation
        document.addEventListener('DOMContentLoaded', function() {
            const tools = document.querySelectorAll('.loading-animation');
            tools.forEach((tool, index) => {
                tool.style.animationDelay = `${index * 0.1}s`;
            });
        });

        // Enhanced click animation with ripple effect
        document.querySelectorAll('.tool-button').forEach(button => {
            button.addEventListener('click', function(e) {
                const ripple = document.createElement('span');
                const rect = this.getBoundingClientRect();
                const size = Math.max(rect.height, rect.width);
                const x = e.clientX - rect.left - size / 2;
                const y = e.clientY - rect.top - size / 2;
                
                ripple.style.width = ripple.style.height = size + 'px';
                ripple.style.left = x + 'px';
                ripple.style.top = y + 'px';
                ripple.style.position = 'absolute';
                ripple.style.borderRadius = '50%';
                ripple.style.background = 'rgba(255, 255, 255, 0.4)';
                ripple.style.transform = 'scale(0)';
                ripple.style.animation = 'ripple 0.6s linear';
                ripple.style.pointerEvents = 'none';
                ripple.style.zIndex = '1';
                
                this.appendChild(ripple);
                
                setTimeout(() => {
                    ripple.remove();
                }, 600);
            });
        });

        // Add subtle parallax effect on mouse move
        document.addEventListener('mousemove', function(e) {
            const tools = document.querySelectorAll('.tool-button');
            const mouseX = e.clientX / window.innerWidth;
            const mouseY = e.clientY / window.innerHeight;
            
            tools.forEach(tool => {
                const speed = 2;
                const x = (mouseX - 0.5) * speed;
                const y = (mouseY - 0.5) * speed;
                tool.style.transform += ` translate(${x}px, ${y}px)`;
            });
        });
    </script>
</body>
</html>