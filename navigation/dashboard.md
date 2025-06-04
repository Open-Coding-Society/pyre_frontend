---
layout: base_chat
search_exclude: true
hide: true
show_reading_time: false
permalink: /dashboardmain/
title: Dashboard
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg,rgb(0, 0, 0) 0%,rgb(0, 0, 0) 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .dashboard-container {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 60px 40px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            text-align: center;
            max-width: 600px;
            width: 100%;
        }

        .dashboard-title {
            color: white;
            font-size: 2.5rem;
            font-weight: 700;
            margin-bottom: 20px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
        }

        .dashboard-subtitle {
            color: rgba(255, 255, 255, 0.9);
            font-size: 1.1rem;
            margin-bottom: 50px;
            font-weight: 300;
        }

        .buttons-container {
            display: flex;
            gap: 30px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .dashboard-button {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            width: 200px;
            height: 160px;
            background: rgba(255, 255, 255, 0.15);
            backdrop-filter: blur(5px);
            border: 2px solid rgba(255, 255, 255, 0.2);
            border-radius: 15px;
            text-decoration: none;
            color: white;
            font-weight: 600;
            font-size: 1.1rem;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }

        .dashboard-button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: left 0.5s;
        }

        .dashboard-button:hover::before {
            left: 100%;
        }

        .dashboard-button:hover {
            transform: translateY(-5px);
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.2);
            border-color: rgba(255, 255, 255, 0.4);
            background: rgba(255, 255, 255, 0.25);
        }

        .button-icon {
            font-size: 3rem;
            margin-bottom: 15px;
            opacity: 0.9;
        }

        .earthquake-button {
            background: linear-gradient(135deg, rgba(255, 87, 34, 0.2), rgba(244, 67, 54, 0.2));
        }

        .earthquake-button:hover {
            background: linear-gradient(135deg, rgba(255, 87, 34, 0.3), rgba(244, 67, 54, 0.3));
        }

        .fire-button {
            background: linear-gradient(135deg, rgba(255, 152, 0, 0.2), rgba(255, 193, 7, 0.2));
        }

        .fire-button:hover {
            background: linear-gradient(135deg, rgba(255, 152, 0, 0.3), rgba(255, 193, 7, 0.3));
        }

        @media (max-width: 600px) {
            .dashboard-container {
                padding: 40px 20px;
            }

            .dashboard-title {
                font-size: 2rem;
            }

            .buttons-container {
                flex-direction: column;
                align-items: center;
            }

            .dashboard-button {
                width: 250px;
            }
        }

        .pulse {
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.05);
            }
            100% {
                transform: scale(1);
            }
        }
    </style>
</head>
<body>
    <div class="dashboard-container">
        <h1 class="dashboard-title">Emergency Dashboard</h1>
        <p class="dashboard-subtitle">Monitor critical events and stay informed</p>
        
        <div class="buttons-container">
            <a href="{{site.baseurl}}/earthquakesdashboard/" class="dashboard-button earthquake-button">
                <div class="button-icon">🌍</div>
                <span>Earthquake Dashboard</span>
            </a>
            
            <a href="{{site.baseurl}}/firesdashboard/" class="dashboard-button fire-button">
                <div class="button-icon">🔥</div>
                <span>Fire Dashboard</span>
            </a>
        </div>
    </div>

    <script>
        // Add subtle animations on page load
        document.addEventListener('DOMContentLoaded', function() {
            const buttons = document.querySelectorAll('.dashboard-button');
            buttons.forEach((button, index) => {
                setTimeout(() => {
                    button.style.opacity = '0';
                    button.style.transform = 'translateY(20px)';
                    button.style.transition = 'opacity 0.6s ease, transform 0.6s ease';
                    
                    setTimeout(() => {
                        button.style.opacity = '1';
                        button.style.transform = 'translateY(0)';
                    }, 100);
                }, index * 200);
            });
        });

        // Add click animation
        document.querySelectorAll('.dashboard-button').forEach(button => {
            button.addEventListener('click', function(e) {
                // Create ripple effect
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
                ripple.style.background = 'rgba(255, 255, 255, 0.3)';
                ripple.style.transform = 'scale(0)';
                ripple.style.animation = 'ripple 0.6s linear';
                ripple.style.pointerEvents = 'none';
                
                this.appendChild(ripple);
                
                setTimeout(() => {
                    ripple.remove();
                }, 600);
            });
        });
    </script>

    <style>
        @keyframes ripple {
            to {
                transform: scale(4);
                opacity: 0;
            }
        }
    </style>
</body>
</html>

<a href="/pyre_frontend/help/" class="fixed bottom-4 right-4 bg-green-600 text-white rounded-full p-3 shadow-lg hover:bg-green-700 transition duration-200 flex items-center justify-center" title="Help Center" style="font-size:1.05em;">
    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9 5.25h.008v.008H12v-.008z"/>
    </svg>
    <span class="ml-1 font-medium">Help</span>
  </a>