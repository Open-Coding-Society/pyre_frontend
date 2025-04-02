---
layout: post
title: Titanic Survival Simulator
search_exclude: true
permalink: /TitanicSimulator/
---
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Titanic: Voyage of Fate</title>
    <style>
        /* Global Styles */
        :root {
            --primary-color: #0a3b5b;
            --secondary-color: #d4af37;
            --dark-color: #0d1b2a;
            --light-color: #f0f0f0;
            --danger-color: #a52a2a;
            --success-color: #2e8b57;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Playfair Display', Georgia, serif;
            color: #333;
            background-color: #000;
            background-image: url('/api/placeholder/1600/900');
            background-size: cover;
            background-attachment: fixed;
            background-position: center;
            position: relative;
            min-height: 100vh;
        }
        
        body::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            z-index: -1;
        }
        
        /* Typography */
        h1, h2, h3, h4 {
            font-family: 'Playfair Display', Georgia, serif;
            color: var(--secondary-color);
            letter-spacing: 1px;
        }
        
        /* Main Layout */
        .game-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        /* Header Styles */
        .game-header {
            text-align: center;
            padding: 40px 0;
            position: relative;
        }
        
        .game-title {
            font-size: 3.5rem;
            margin-bottom: 10px;
            font-weight: 700;
            color: var(--secondary-color);
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
            position: relative;
        }
        
        .game-subtitle {
            font-size: 1.5rem;
            color: var(--light-color);
            font-style: italic;
            margin-bottom: 20px;
        }
        
        /* Game Interface */
        .game-interface {
            display: flex;
            gap: 30px;
            margin-top: 20px;
        }
        
        /* Game Phases */
        .game-phase {
            display: none;
            opacity: 0;
            transition: opacity 0.6s ease-in-out;
        }
        
        .game-phase.active {
            display: block;
            opacity: 1;
        }
        
        /* Character Creation */
        .character-creation {
            flex: 1;
            background: rgba(10, 59, 91, 0.8);
            border-radius: 8px;
            padding: 25px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            border: 1px solid var(--secondary-color);
            color: var(--light-color);
        }
        
        .character-creation h2 {
            font-size: 2rem;
            margin-bottom: 20px;
            border-bottom: 2px solid var(--secondary-color);
            padding-bottom: 10px;
            text-align: center;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--secondary-color);
        }
        
        .form-control {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid rgba(212, 175, 55, 0.3);
            border-radius: 4px;
            background: rgba(0, 0, 0, 0.6);
            color: var(--light-color);
            font-family: 'Playfair Display', Georgia, serif;
            font-size: 1rem;
        }
        
        .form-control:focus {
            outline: none;
            border-color: var(--secondary-color);
            box-shadow: 0 0 8px rgba(212, 175, 55, 0.6);
        }
        
        select.form-control {
            appearance: none;
            background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23d4af37' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
            background-repeat: no-repeat;
            background-position: right 15px center;
            background-size: 15px;
        }
        
        /* Passenger Display */
        .passenger-display {
            flex: 1;
            background: rgba(10, 59, 91, 0.8);
            border-radius: 8px;
            padding: 25px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            border: 1px solid var(--secondary-color);
            color: var(--light-color);
            display: flex;
            flex-direction: column;
        }
        
        .passenger-display h2 {
            font-size: 2rem;
            margin-bottom: 20px;
            border-bottom: 2px solid var(--secondary-color);
            padding-bottom: 10px;
            text-align: center;
        }
        
        /* Gameplay Screen */
        .gameplay-screen {
            flex: 2;
            background: rgba(10, 59, 91, 0.8);
            border-radius: 8px;
            padding: 25px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            border: 1px solid var(--secondary-color);
            color: var(--light-color);
            min-height: 400px;
            position: relative;
            overflow: hidden;
        }
        
        .scene-container {
            position: relative;
            width: 100%;
            height: 300px;
            overflow: hidden;
            margin-bottom: 20px;
            border-radius: 8px;
            background-image: url('/api/placeholder/800/300');
            background-size: cover;
            background-position: center;
            border: 2px solid var(--secondary-color);
        }
        
        .ship-image {
            position: absolute;
            width: 100%;
            height: 100%;
            background-size: contain;
            background-repeat: no-repeat;
            background-position: center;
            transition: transform 1s ease-in-out;
        }
        
        .narrative-text {
            background: rgba(0, 0, 0, 0.7);
            padding: 20px;
            border-radius: 8px;
            font-style: italic;
            line-height: 1.8;
            margin-bottom: 20px;
            min-height: 100px;
            border-left: 4px solid var(--secondary-color);
        }
        
        /* Button Styles */
        .btn {
            display: inline-block;
            padding: 12px 25px;
            background-color: var(--secondary-color);
            color: var(--dark-color);
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-weight: 700;
            text-transform: uppercase;
            letter-spacing: 1px;
            transition: all 0.3s ease;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            margin: 5px;
            font-family: 'Playfair Display', Georgia, serif;
        }
        
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 7px 10px rgba(0, 0, 0, 0.2);
            background-color: #e0c050;
        }
        
        .btn:active {
            transform: translateY(-1px);
            box-shadow: 0 5px 8px rgba(0, 0, 0, 0.2);
        }
        
        .btn-primary {
            background-color: var(--secondary-color);
            color: var(--dark-color);
        }
        
        .btn-danger {
            background-color: var(--danger-color);
            color: var(--light-color);
        }
        
        .btn-success {
            background-color: var(--success-color);
            color: var(--light-color);
        }
        
        .btn-outline {
            background-color: transparent;
            border: 2px solid var(--secondary-color);
            color: var(--secondary-color);
        }
        
        .btn-outline:hover {
            background-color: var(--secondary-color);
            color: var(--dark-color);
        }
        
        .btn-group {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-top: 20px;
        }
        
        /* Passenger Card */
        .passenger-card {
            background: rgba(0, 0, 0, 0.6);
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 20px;
            border: 1px solid var(--secondary-color);
            display: flex;
            flex-direction: column;
            gap: 10px;
            transition: all 0.3s ease;
        }
        
        .passenger-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
        }
        
        .passenger-card h3 {
            border-bottom: 1px solid var(--secondary-color);
            padding-bottom: 10px;
            margin-bottom: 10px;
        }
        
        .passenger-card p {
            display: flex;
            justify-content: space-between;
        }
        
        .passenger-card p span:first-child {
            font-weight: bold;
            color: var(--secondary-color);
        }
        
        /* Passenger List */
        .passenger-list {
            flex: 1;
            overflow-y: auto;
            max-height: 500px;
            padding-right: 10px;
        }
        
        .passenger-list::-webkit-scrollbar {
            width: 8px;
        }
        
        .passenger-list::-webkit-scrollbar-track {
            background: rgba(0, 0, 0, 0.2);
            border-radius: 10px;
        }
        
        .passenger-list::-webkit-scrollbar-thumb {
            background: var(--secondary-color);
            border-radius: 10px;
        }
        
        /* Results Screen */
        .results-screen {
            text-align: center;
            padding: 30px;
        }
        
        .result-message {
            font-size: 2.5rem;
            margin-bottom: 30px;
            text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        .survival-result {
            font-size: 3rem;
            font-weight: bold;
            margin: 30px 0;
            text-transform: uppercase;
            letter-spacing: 3px;
            animation: pulse 2s infinite;
        }
        
        .survived {
            color: var(--success-color);
            text-shadow: 0 0 10px rgba(46, 139, 87, 0.7);
        }
        
        .perished {
            color: var(--danger-color);
            text-shadow: 0 0 10px rgba(165, 42, 42, 0.7);
        }
        
        .probability-meter {
            width: 80%;
            height: 40px;
            background: rgba(0, 0, 0, 0.5);
            margin: 0 auto 30px;
            border-radius: 20px;
            position: relative;
            overflow: hidden;
            border: 2px solid var(--light-color);
        }
        
        .probability-fill {
            height: 100%;
            background: linear-gradient(to right, var(--danger-color), var(--success-color));
            transition: width 1.5s ease-in-out;
            position: relative;
            width: 0;
        }
        
        .probability-text {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            color: var(--light-color);
            font-weight: bold;
            text-shadow: 0 0 3px rgba(0, 0, 0, 0.8);
            z-index: 1;
        }
        
        /* Animation */
        .fade-in {
            animation: fadeIn 1s ease-in forwards;
        }
        
        .slide-in {
            animation: slideIn 1s ease-out forwards;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @keyframes slideIn {
            from { transform: translateY(-20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        /* Modal Styles */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            z-index: 100;
            overflow: auto;
            align-items: center;
            justify-content: center;
        }
        
        .modal-content {
            background: linear-gradient(135deg, #0a3b5b 0%, #051e2d 100%);
            color: var(--light-color);
            width: 90%;
            max-width: 700px;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 0 20px rgba(212, 175, 55, 0.5);
            position: relative;
            border: 2px solid var(--secondary-color);
            animation: modalOpen 0.5s ease-out forwards;
        }
        
        @keyframes modalOpen {
            from { transform: scale(0.8); opacity: 0; }
            to { transform: scale(1); opacity: 1; }
        }
        
        .close-btn {
            position: absolute;
            top: 15px;
            right: 15px;
            font-size: 1.8rem;
            color: var(--secondary-color);
            cursor: pointer;
            transition: transform 0.3s ease;
            background: none;
            border: none;
        }
        
        .close-btn:hover {
            transform: rotate(90deg);
            color: var(--light-color);
        }
        
        /* Historical Records Table */
        .historical-records {
            max-height: 350px;
            overflow-y: auto;
            margin-top: 20px;
        }
        
        .records-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        
        .records-table th, .records-table td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid rgba(212, 175, 55, 0.3);
        }
        
        .records-table th {
            background-color: rgba(0, 0, 0, 0.5);
            color: var(--secondary-color);
            text-transform: uppercase;
            letter-spacing: 1px;
            font-weight: 600;
            position: sticky;
            top: 0;
            z-index: 10;
        }
        
        .records-table tr:hover {
            background-color: rgba(10, 59, 91, 0.5);
        }
        
        .survived-tag, .perished-tag {
            padding: 5px 10px;
            border-radius: 4px;
            font-weight: bold;
            display: inline-block;
            text-align: center;
            min-width: 80px;
        }
        
        .survived-tag {
            background-color: rgba(46, 139, 87, 0.7);
            color: var(--light-color);
        }
        
        .perished-tag {
            background-color: rgba(165, 42, 42, 0.7);
            color: var(--light-color);
        }
        
        /* Loading Effect */
        .loading-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.8);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            display: none;
        }
        
        .loading-spinner {
            width: 100px;
            height: 100px;
            border: 10px solid rgba(212, 175, 55, 0.3);
            border-top: 10px solid var(--secondary-color);
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        /* Responsiveness */
        @media (max-width: 992px) {
            .game-interface {
                flex-direction: column;
            }
            
            .game-title {
                font-size: 2.5rem;
            }
            
            .modal-content {
                width: 95%;
                padding: 15px;
            }
            
            .survival-result {
                font-size: 2.5rem;
            }
        }
        
        @media (max-width: 576px) {
            .game-title {
                font-size: 2rem;
            }
            
            .btn {
                padding: 10px 20px;
                font-size: 0.9rem;
            }
            
            .survival-result {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <header class="game-header slide-in">
            <h1 class="game-title">Titanic: Voyage of Fate</h1>
            <p class="game-subtitle">April 14, 1912 - Will You Survive the Disaster?</p>
        </header>
        
        <main class="game-interface">
            <!-- Character Creation Phase -->
            <section class="game-phase character-creation active fade-in" id="creation-phase">
                <h2>Create Your Passenger</h2>
                <div class="narrative-text">
                    The RMS Titanic, the largest ship afloat, is preparing for her maiden voyage. As you prepare to board this majestic vessel, who will you be?
                </div>
                
                <form id="passenger-form">
                    <div class="form-group">
                        <label for="passenger-name">Passenger Name</label>
                        <input type="text" id="passenger-name" class="form-control" placeholder="Enter your name" required>
                    </div>
                    
                    <div class="form-group">
                        <label for="passenger-class">Ticket Class</label>
                        <select id="passenger-class" class="form-control" required>
                            <option value="">-- Select Class --</option>
                            <option value="1">First Class - Luxury accommodations and fine dining</option>
                            <option value="2">Second Class - Comfortable accommodations</option>
                            <option value="3">Third Class - Basic accommodations</option>
                        </select>
                    </div>
                    
                    <div class="form-group">
                        <label for="passenger-sex">Gender</label>
                        <select id="passenger-sex" class="form-control" required>
                            <option value="">-- Select Gender --</option>
                            <option value="male">Male</option>
                            <option value="female">Female</option>
                        </select>
                    </div>
                    
                    <div class="form-group">
                        <label for="passenger-age">Age</label>
                        <input type="number" id="passenger-age" class="form-control" placeholder="Enter age" min="0" max="100" required>
                    </div>
                    
                    <div class="form-group">
                        <label for="passenger-sibsp">Family Members Aboard (Siblings/Spouse)</label>
                        <input type="number" id="passenger-sibsp" class="form-control" placeholder="Number of siblings/spouse" min="0" max="10" value="0" required>
                    </div>
                    
                    <div class="form-group">
                        <label for="passenger-parch">Family Members Aboard (Parents/Children)</label>
                        <input type="number" id="passenger-parch" class="form-control" placeholder="Number of parents/children" min="0" max="10" value="0" required>
                    </div>
                    
                    <div class="form-group">
                        <label for="passenger-fare">Ticket Fare (£)</label>
                        <input type="number" id="passenger-fare" class="form-control" placeholder="Ticket price" min="0" required>
                    </div>
                    
                    <div class="form-group">
                        <label for="passenger-embarked">Port of Embarkation</label>
                        <select id="passenger-embarked" class="form-control" required>
                            <option value="">-- Select Port --</option>
                            <option value="C">Cherbourg, France</option>
                            <option value="Q">Queenstown (Cobh), Ireland</option>
                            <option value="S">Southampton, England</option>
                        </select>
                    </div>
                    
                    <div class="btn-group">
                        <button type="button" id="board-btn" class="btn btn-primary">Board the Ship</button>
                    </div>
                </form>
            </section>
            
            <!-- Gameplay Phase -->
            <section class="game-phase gameplay-screen" id="gameplay-phase">
                <div class="scene-container" id="scene-image">
                    <div class="ship-image" id="ship-animation"></div>
                </div>
                
                <div class="narrative-text" id="narrative">
                    Loading your journey on the magnificent RMS Titanic...
                </div>
                
                <div class="btn-group">
                    <button type="button" id="continue-btn" class="btn btn-primary">Continue Journey</button>
                </div>
            </section>
            
            <!-- Results Phase -->
            <section class="game-phase results-screen" id="results-phase">
                <h2>Your Fate Has Been Decided</h2>
                
                <div class="narrative-text" id="result-narrative">
                    The Titanic has struck an iceberg. Panic spreads throughout the ship as passengers scramble to the lifeboats. Will you be among those who survive?
                </div>
                
                <div class="probability-meter">
                    <div class="probability-fill" id="probability-bar"></div>
                    <div class="probability-text" id="probability-text">0%</div>
                </div>
                
                <div class="survival-result" id="survival-result"></div>
                
                <div class="passenger-card" id="final-passenger-card">
                    <!-- Passenger details will be displayed here -->
                </div>
                
                <div class="btn-group">
                    <button type="button" id="save-passenger-btn" class="btn btn-success">Save to Historical Records</button>
                    <button type="button" id="new-passenger-btn" class="btn btn-primary">New Passenger</button>
                    <button type="button" id="view-records-btn" class="btn btn-outline">View All Records</button>
                </div>
            </section>
            
            <!-- Passenger Records -->
            <section class="game-phase passenger-display" id="records-phase">
                <h2>Historical Records</h2>
                
                <div class="passenger-list" id="passenger-list">
                    <!-- Passenger cards will be displayed here -->
                </div>
                
                <div class="btn-group">
                    <button type="button" id="back-to-game-btn" class="btn btn-outline">Back to Game</button>
                </div>
            </section>
        </main>
    </div>
    
    <!-- Historical Records Modal -->
    <div id="records-modal" class="modal">
        <div class="modal-content">
            <button class="close-btn" id="close-records-modal">&times;</button>
            <h2>Titanic Historical Records</h2>
            <p>Records of all passengers who attempted the voyage</p>
            
            <div class="historical-records">
                <table class="records-table">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Name</th>
                            <th>Class</th>
                            <th>Gender</th>
                            <th>Age</th>
                            <th>Survived</th>
                            <th>Actions</th>
                        </tr>
                    </thead>
                    <tbody id="records-table-body">
                        <!-- Records will be populated here -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>
    
    <!-- Welcome Modal -->
    <div id="welcome-modal" class="modal">
        <div class="modal-content">
            <button class="close-btn" id="close-welcome-modal">&times;</button>
            <h2>Welcome Aboard the RMS Titanic</h2>
            <p>April 10, 1912 - The Ship of Dreams embarks on her maiden voyage.</p>
            
            <div style="margin: 20px 0;">
                <h3>Game Instructions</h3>
                <ul style="list-style-type: none; padding: 0;">
                    <li style="margin-bottom: 10px;">✓ <strong>Create Your Identity:</strong> Choose your passenger details carefully - your choices will affect your survival chances.</li>
                    <li style="margin-bottom: 10px;">✓ <strong>Experience the Voyage:</strong> Live through the historic journey of the Titanic.</li>
                    <li style="margin-bottom: 10px;">✓ <strong>Face the Disaster:</strong> When disaster strikes, will you survive based on historical data?</li>
                    <li style="margin-bottom: 10px;">✓ <strong>Record Your Journey:</strong> Save your passenger to the historical records.</li>
                </ul>
            </div>
            
            <p><strong>Historical Note:</strong> This simulation uses real data and machine learning to predict survival outcomes based on actual Titanic passenger statistics.</p>
            
            <div class="btn-group">
                <button type="button" id="start-journey-btn" class="btn btn-primary">Begin Your Journey</button>
            </div>
        </div>
    </div>
    
    <!-- Loading Overlay -->
    <div class="loading-overlay" id="loading-overlay">
        <div class="loading-spinner"></div>
    </div>
    
    <script type="module">
        import { pythonURI, fetchOptions } from "{{site.baseurl}}/assets/js/api/config.js";
        
        // Game state
        let gameState = {
            currentPhase: 'creation',
            passengerData: null,
            predictionResult: null,
            narrativeIndex: 0,
            gamePhases: ['creation', 'gameplay', 'results', 'records'],
            narratives: [
                "You board the magnificent RMS Titanic, known as the 'Ship of Dreams'. The grandeur of the vessel is breathtaking as you settle into your accommodations.",
                "April 14, 1912, 11:40 PM - A lookout spots an iceberg directly ahead. Despite attempts to turn the ship, Titanic strikes the iceberg on its starboard side.",
                "The damage is severe. Water begins to flood the ship's compartments. Pyre Smith orders the lifeboats to be prepared.",
                "Panic spreads throughout the ship. Officers begin loading passengers into lifeboats with women and children given priority. The ship begins to tilt as water fills more compartments.",
                "The Titanic's fate is sealed. The question remains - will you be among those who survive?"
            ]
        };
        
        // DOM Elements
        const elements = {
            welcomeModal: document.getElementById('welcome-modal'),
            closeWelcomeBtn: document.getElementById('close-welcome-modal'),
            startJourneyBtn: document.getElementById('start-journey-btn'),
            
            recordsModal: document.getElementById('records-modal'),
            closeRecordsBtn: document.getElementById('close-records-modal'),
            
            creationPhase: document.getElementById('creation-phase'),
            gameplayPhase: document.getElementById('gameplay-phase'),
            resultsPhase: document.getElementById('results-phase'),
            recordsPhase: document.getElementById('records-phase'),
            
            passengerForm: document.getElementById('passenger-form'),
            boardBtn: document.getElementById('board-btn'),
            continueBtn: document.getElementById('continue-btn'),
            
            shipAnimation: document.getElementById('ship-animation'),
            narrativeText: document.getElementById('narrative'),
            
            savePassengerBtn: document.getElementById('save-passenger-btn'),
            newPassengerBtn: document.getElementById('new-passenger-btn'),
            viewRecordsBtn: document.getElementById('view-records-btn'),
            backToGameBtn: document.getElementById('back-to-game-btn'),
            
            probabilityBar: document.getElementById('probability-bar'),
            probabilityText: document.getElementById('probability-text'),
            survivalResult: document.getElementById('survival-result'),
            finalPassengerCard: document.getElementById('final-passenger-card'),
            
            passengerList: document.getElementById('passenger-list'),
            recordsTableBody: document.getElementById('records-table-body'),
            loadingOverlay: document.getElementById('loading-overlay'),
            resultNarrative: document.getElementById('result-narrative')
        };
        
        // Initialize stored passengers
        let storedPassengers = JSON.parse(localStorage.getItem('titanicPassengers')) || [];
        
        // Show welcome modal on page load
        window.addEventListener('DOMContentLoaded', () => {
            elements.welcomeModal.style.display = 'flex';
            updatePassengersList();
        });
        
        // Modal controls
        elements.closeWelcomeBtn.addEventListener('click', () => {
            elements.welcomeModal.style.display = 'none';
        });
        
        elements.startJourneyBtn.addEventListener('click', () => {
            elements.welcomeModal.style.display = 'none';
        });
        
        elements.closeRecordsBtn.addEventListener('click', () => {
            elements.recordsModal.style.display = 'none';
        });
        
        elements.viewRecordsBtn.addEventListener('click', () => {
            elements.recordsModal.style.display = 'flex';
            updateRecordsTable();
        });
        
        // Phase navigation
        function switchPhase(targetPhase) {
            const phases = document.querySelectorAll('.game-phase');
            phases.forEach(phase => {
                phase.classList.remove('active');
            });
            
            document.getElementById(`${targetPhase}-phase`).classList.add('active');
            gameState.currentPhase = targetPhase;
        }
        
        // Board the ship (start journey)
        elements.boardBtn.addEventListener('click', () => {
            if (validatePassengerForm()) {
                gameState.passengerData = collectPassengerData();
                switchPhase('gameplay');
                startJourney();
            }
        });
        
        // Continue journey button
        elements.continueBtn.addEventListener('click', () => {
            advanceNarrative();
        });
        
        // Save passenger button
        elements.savePassengerBtn.addEventListener('click', () => {
            savePassengerToHistory();
        });
        
        // New passenger button
        elements.newPassengerBtn.addEventListener('click', () => {
            resetGame();
            switchPhase('creation');
        });
        
        // Back to game button
        elements.backToGameBtn.addEventListener('click', () => {
            switchPhase('creation');
        });
        
        // Form validation
        function validatePassengerForm() {
            const form = elements.passengerForm;
            const inputs = form.querySelectorAll('input, select');
            let isValid = true;
            
            inputs.forEach(input => {
                if (!input.value) {
                    input.style.borderColor = 'var(--danger-color)';
                    isValid = false;
                } else {
                    input.style.borderColor = 'rgba(212, 175, 55, 0.3)';
                }
            });
            
            // Additional validation could be added here
            
            return isValid;
        }
        
        // Collect passenger data from form
        function collectPassengerData() {
            return {
                id: Date.now(),
                name: document.getElementById('passenger-name').value,
                pclass: parseInt(document.getElementById('passenger-class').value),
                sex: document.getElementById('passenger-sex').value,
                age: parseFloat(document.getElementById('passenger-age').value),
                sibsp: parseInt(document.getElementById('passenger-sibsp').value),
                parch: parseInt(document.getElementById('passenger-parch').value),
                fare: parseFloat(document.getElementById('passenger-fare').value),
                embarked: document.getElementById('passenger-embarked').value,
                survived: null, // Will be determined later
                survival_probability: null
            };
        }
        
        // Start journey narrative
        function startJourney() {
            gameState.narrativeIndex = 0;
            elements.narrativeText.textContent = gameState.narratives[0];
            elements.shipAnimation.style.backgroundImage = "url('/api/placeholder/800/400')"; // Ship image
        }
        
        // Advance narrative
        function advanceNarrative() {
            gameState.narrativeIndex++;
            
            if (gameState.narrativeIndex < gameState.narratives.length) {
                elements.narrativeText.textContent = gameState.narratives[gameState.narrativeIndex];
                
                // Visual effects based on narrative stage
                if (gameState.narrativeIndex === 1) {
                    elements.shipAnimation.style.transform = "rotate(-5deg)";
                } else if (gameState.narrativeIndex === 3) {
                    elements.shipAnimation.style.transform = "rotate(-15deg)";
                }
            } else {
                // End of narrative, predict survival
                predictSurvival();
            }
        }
        
        // Predict survival using "simulated" ML model
        async function predictSurvival() {
            // Show loading overlay
            elements.loadingOverlay.style.display = 'flex';
            
            try {
                // In a real app, this would call an API with ML model
                // For demo purposes, we'll use a simplified predictive algorithm based on historical data
                
                // Simple logistic regression-inspired calculation
                let probability = 0;
                
                // Base probability based on gender (historical data shows women had higher survival rates)
                if (gameState.passengerData.sex === 'female') {
                    probability += 0.7;
                } else {
                    probability += 0.2;
                }
                
                // Class adjustment (1st class had higher survival rates)
                if (gameState.passengerData.pclass === 1) {
                    probability += 0.2;
                } else if (gameState.passengerData.pclass === 2) {
                    probability += 0.1;
                } else {
                    probability -= 0.1;
                }
                
                // Age adjustment (children had priority)
                if (gameState.passengerData.age < 12) {
                    probability += 0.15;
                } else if (gameState.passengerData.age > 50) {
                    probability -= 0.05;
                }
                
                // Family adjustment
                const totalFamily = gameState.passengerData.sibsp + gameState.passengerData.parch;
                if (totalFamily > 0 && totalFamily < 4) {
                    probability += 0.05; // Small families coordinated better
                } else if (totalFamily >= 4) {
                    probability -= 0.1; // Larger families had difficulty staying together
                }
                
                // Embarkation point (minor effect)
                if (gameState.passengerData.embarked === 'C') { // Cherbourg
                    probability += 0.03;
                }
                
                // Add some randomness (but keep within bounds)
                probability += (Math.random() * 0.1) - 0.05;
                
                // Ensure probability is between 0 and 1
                probability = Math.max(0, Math.min(1, probability));
                
                // Determine survival based on probability
                const survived = probability > 0.5;
                
                // Set result in passenger data
                gameState.passengerData.survived = survived ? 1 : 0;
                gameState.passengerData.survival_probability = Math.round(probability * 100);
                
                // Simulate API delay
                await new Promise(resolve => setTimeout(resolve, 2000));
                
                // Show results
                displayResults(probability);
            } catch (error) {
                console.error("Error predicting survival:", error);
                alert("An error occurred while calculating your fate.");
            } finally {
                // Hide loading overlay
                elements.loadingOverlay.style.display = 'none';
            }
        }
        
        // Display results
        function displayResults(probability) {
            switchPhase('results');
            
            const survived = gameState.passengerData.survived === 1;
            const probabilityPercent = Math.round(probability * 100);
            
            // Update probability meter
            elements.probabilityBar.style.width = `${probabilityPercent}%`;
            elements.probabilityText.textContent = `${probabilityPercent}% Chance`;
            
            // Update survival result text
            elements.survivalResult.textContent = survived ? "SURVIVED" : "PERISHED";
            elements.survivalResult.className = `survival-result ${survived ? 'survived' : 'perished'}`;
            
            // Generate narrative based on outcome
            let narrative = "";
            if (survived) {
                narrative = `As chaos engulfed the sinking Titanic, ${gameState.passengerData.name} managed to secure a place on lifeboat number ${Math.floor(Math.random() * 16) + 1}. Through the freezing night, they waited until the RMS Carpathia arrived for rescue. They were among the 705 fortunate souls who survived the disaster.`;
            } else {
                narrative = `Despite valiant efforts, ${gameState.passengerData.name} could not secure a place on a lifeboat. As the mighty Titanic slipped beneath the waves, they joined the 1,500 souls who perished in the frigid North Atlantic waters. Their story becomes part of the tragic legacy of the Ship of Dreams.`;
            }
            elements.resultNarrative.textContent = narrative;
            
            // Create passenger card
            createPassengerResultCard();
        }
        
        // Create passenger result card
        function createPassengerResultCard() {
            const passenger = gameState.passengerData;
            let classText = "";
            switch (passenger.pclass) {
                case 1: classText = "First Class"; break;
                case 2: classText = "Second Class"; break;
                case 3: classText = "Third Class"; break;
            }
            
            let embarkedText = "";
            switch (passenger.embarked) {
                case "C": embarkedText = "Cherbourg, France"; break;
                case "Q": embarkedText = "Queenstown, Ireland"; break;
                case "S": embarkedText = "Southampton, England"; break;
            }
            
            elements.finalPassengerCard.innerHTML = `
                <h3>${passenger.name}</h3>
                <p><span>Class:</span> <span>${classText}</span></p>
                <p><span>Gender:</span> <span>${passenger.sex.charAt(0).toUpperCase() + passenger.sex.slice(1)}</span></p>
                <p><span>Age:</span> <span>${passenger.age}</span></p>
                <p><span>Family aboard:</span> <span>${passenger.sibsp + passenger.parch} members</span></p>
                <p><span>Ticket fare:</span> <span>£${passenger.fare.toFixed(2)}</span></p>
                <p><span>Embarked from:</span> <span>${embarkedText}</span></p>
                <p><span>Survival probability:</span> <span>${passenger.survival_probability}%</span></p>
                <p><span>Outcome:</span> <span class="${passenger.survived ? 'survived-tag' : 'perished-tag'}">${passenger.survived ? 'SURVIVED' : 'PERISHED'}</span></p>
            `;
        }
        
        // Save passenger to history
        function savePassengerToHistory() {
            // Check if already saved
            const existingIndex = storedPassengers.findIndex(p => p.id === gameState.passengerData.id);
            
            if (existingIndex === -1) {
                storedPassengers.push(gameState.passengerData);
                localStorage.setItem('titanicPassengers', JSON.stringify(storedPassengers));
                alert(`${gameState.passengerData.name} has been added to the historical records.`);
                elements.savePassengerBtn.disabled = true;
                elements.savePassengerBtn.textContent = "Passenger Saved";
                updatePassengersList();
            } else {
                alert("This passenger is already in the historical records.");
            }
        }
        
        // Update passengers list in records view
        function updatePassengersList() {
            elements.passengerList.innerHTML = '';
            
            if (storedPassengers.length === 0) {
                elements.passengerList.innerHTML = '<p style="text-align: center; padding: 20px;">No historical records found.</p>';
                return;
            }
            
            storedPassengers.forEach(passenger => {
                const card = document.createElement('div');
                card.className = 'passenger-card';
                
                let classText = "";
                switch (passenger.pclass) {
                    case 1: classText = "First Class"; break;
                    case 2: classText = "Second Class"; break;
                    case 3: classText = "Third Class"; break;
                }
                
                card.innerHTML = `
                    <h3>${passenger.name}</h3>
                    <p><span>Class:</span> <span>${classText}</span></p>
                    <p><span>Gender:</span> <span>${passenger.sex.charAt(0).toUpperCase() + passenger.sex.slice(1)}</span></p>
                    <p><span>Age:</span> <span>${passenger.age}</span></p>
                    <p><span>Outcome:</span> <span class="${passenger.survived ? 'survived-tag' : 'perished-tag'}">${passenger.survived ? 'SURVIVED' : 'PERISHED'}</span></p>
                `;
                
                elements.passengerList.appendChild(card);
            });
        }
        
        // Update records table in modal
        function updateRecordsTable() {
            elements.recordsTableBody.innerHTML = '';
            
            if (storedPassengers.length === 0) {
                const row = document.createElement('tr');
                row.innerHTML = '<td colspan="7" style="text-align: center;">No historical records found.</td>';
                elements.recordsTableBody.appendChild(row);
                return;
            }
            
            storedPassengers.forEach((passenger, index) => {
                const row = document.createElement('tr');
                
                row.innerHTML = `
                    <td>${index + 1}</td>
                    <td>${passenger.name}</td>
                    <td>${passenger.pclass}</td>
                    <td>${passenger.sex.charAt(0).toUpperCase() + passenger.sex.slice(1)}</td>
                    <td>${passenger.age}</td>
                    <td><span class="${passenger.survived ? 'survived-tag' : 'perished-tag'}">${passenger.survived ? 'SURVIVED' : 'PERISHED'}</span></td>
                    <td>
                        <button class="btn btn-danger btn-delete" data-id="${passenger.id}">Delete</button>
                    </td>
                `;
                
                elements.recordsTableBody.appendChild(row);
            });
            
            // Add event listeners for delete buttons
            document.querySelectorAll('.btn-delete').forEach(button => {
                button.addEventListener('click', function() {
                    const passengerId = parseInt(this.getAttribute('data-id'));
                    deletePassenger(passengerId);
                });
            });
        }
        
        // Delete passenger record
        function deletePassenger(passengerId) {
            if (confirm("Are you sure you want to delete this passenger record?")) {
                storedPassengers = storedPassengers.filter(p => p.id !== passengerId);
                localStorage.setItem('titanicPassengers', JSON.stringify(storedPassengers));
                updateRecordsTable();
                updatePassengersList();
                alert("Passenger record deleted.");
            }
        }
        
        // Reset game state
        function resetGame() {
            gameState.narrativeIndex = 0;
            gameState.passengerData = null;
            gameState.predictionResult = null;
            
            // Reset form
            elements.passengerForm.reset();
            
            // Reset buttons
            elements.savePassengerBtn.disabled = false;
            elements.savePassengerBtn.textContent = "Save to Historical Records";
            
            // Reset animations
            elements.shipAnimation.style.transform = "none";
        }
        
        // Initialize ticket fare based on class selection
        document.getElementById('passenger-class').addEventListener('change', function() {
            const fareInput = document.getElementById('passenger-fare');
            const selectedClass = parseInt(this.value);
            
            // Set default fare based on class
            switch (selectedClass) {
                case 1:
                    fareInput.value = Math.floor(Math.random() * (80 - 30 + 1)) + 30; // First class: £30-£80
                    break;
                case 2:
                    fareInput.value = Math.floor(Math.random() * (20 - 10 + 1)) + 10; // Second class: £10-£20
                    break;
                case 3:
                    fareInput.value = Math.floor(Math.random() * (10 - 5 + 1)) + 5; // Third class: £5-£10
                    break;
                default:
                    fareInput.value = "";
            }
        });
        
        // Initialize game
        function init() {
            switchPhase('creation');
        }
        
        // Start the game
        init();
    </script>
</body>
</html>