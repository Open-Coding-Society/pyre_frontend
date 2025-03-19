---
layout: post
title: Binary + Logic Gates Game
search_exclude: true
permalink: /BinaryGame/
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Logic Gate Challenge</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: rgb(1, 1, 2);
            color: #f0f0f0;
            margin: 20px;
            padding: 0;
            text-align: center;
        }
        h1 {
            color: #4CAF50;
            margin-bottom: 20px;
        }
        .game-container {
            max-width: 600px;
            margin: 0 auto;
            padding: 20px;
            background-color: #111;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
        }
        .progress {
            margin: 20px auto;
            width: 80%;
            background-color: #000;
            border-radius: 10px;
            overflow: hidden;
        }
        .progress-bar {
            height: 10px;
            background-color: #4CAF50;
            width: 0%;
            transition: width 0.5s ease;
        }
        .score-display {
            margin: 15px 0;
            font-size: 16px;
            font-weight: bold;
        }
        .question-container {
            margin-bottom: 20px;
            padding: 15px;
            background-color: #222;
            border-radius: 8px;
        }
        .truth-table {
            margin: 20px auto;
            border-collapse: collapse;
            width: 80%;
            max-width: 300px;
        }
        .truth-table th, .truth-table td {
            border: 1px solid #444;
            padding: 8px;
            text-align: center;
        }
        .truth-table th {
            background-color: #333;
        }
        .options-container {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
            flex-wrap: wrap;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 12px 24px;
            border: none;
            cursor: pointer;
            border-radius: 5px;
            font-size: 16px;
            transition: all 0.3s ease;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        }
        button:hover:not([disabled]) {
            background-color: #45a049;
            transform: translateY(-2px);
        }
        button:active:not([disabled]) {
            transform: translateY(0);
        }
        button:disabled {
            background-color: #555;
            color: #999;
            cursor: not-allowed;
            transform: none;
        }
        .selected {
            background-color: #2196F3;
        }
        .correct {
            background-color: #4CAF50;
        }
        .incorrect {
            background-color: #f44336;
        }
        .result {
            font-size: 20px;
            margin-top: 30px;
            font-weight: bold;
            padding: 15px;
            background-color: #222;
            border-radius: 8px;
            display: none;
        }
        .instructions {
            margin-top: 20px;
            font-size: 14px;
            color: #aaa;
            text-align: left;
        }
        .hidden {
            display: none;
        }
        .feedback {
            margin: 15px 0;
            padding: 10px;
            border-radius: 5px;
            font-weight: bold;
        }
        .feedback.correct {
            background-color: rgba(0, 128, 0, 0.3);
            color: #4CAF50;
        }
        .feedback.incorrect {
            background-color: rgba(128, 0, 0, 0.3);
            color: #ff6b6b;
        }
        .next-button {
            margin-top: 15px;
            display: none;
        }
    </style>
</head>
<body>
    <div class="game-container">
        <h1>Logic Gate Challenge</h1>
        
        <div class="progress">
            <div class="progress-bar" id="progress-bar"></div>
        </div>
        
        <div class="score-display" id="score-display">Score: 0/0</div>
        
        <div id="game-content">
            <div class="question-container" id="question-container">
                <p id="question-text">Loading question...</p>
                
                <div id="truth-table-container" class="hidden">
                    <table class="truth-table" id="truth-table">
                        <thead>
                            <tr>
                                <th>A</th>
                                <th>B</th>
                                <th>?</th>
                            </tr>
                        </thead>
                        <tbody id="table-body">
                            <!-- Table rows will be inserted here -->
                        </tbody>
                    </table>
                </div>
            </div>
            
            <div class="options-container" id="options-container">
                <!-- Buttons will be inserted here -->
            </div>
            
            <div class="feedback hidden" id="feedback"></div>
            
            <button id="next-button" class="next-button">Next Question</button>
        </div>
        
        <div class="result" id="result"></div>
        
        <div class="instructions">
            <h3>How to play:</h3>
            <p>1. For each question, you'll be shown a truth table or a logic gate problem.</p>
            <p>2. Select the correct logical operator (AND, OR, XOR, NOT) or output value.</p>
            <p>3. You need to score at least 7/10 to pass the challenge.</p>
            <p>4. Remember the basic logic gates:</p>
            <ul>
                <li>AND: Returns true only if both inputs are true</li>
                <li>OR: Returns true if at least one input is true</li>
                <li>XOR: Returns true if exactly one input is true</li>
                <li>NOT: Inverts the input (true becomes false, false becomes true)</li>
                <li>NAND: Opposite of AND, returns false only if both inputs are true</li>
                <li>NOR: Opposite of OR, returns true only if both inputs are false</li>
            </ul>
        </div>
    </div>

    <script>
        // Game variables
        let currentQuestionIndex = 0;
        let score = 0;
        let questions = [];
        let answerSubmitted = false;
        let selectedOption = null;
        
        // Question types
        const questionTypes = [
            { type: 'identify-gate', generateQuestion: generateIdentifyGateQuestion },
            { type: 'fill-truth-table', generateQuestion: generateFillTruthTableQuestion },
            { type: 'predict-output', generateQuestion: generatePredictOutputQuestion }
        ];
        
        // Gates and their truth tables
        const gates = {
            'AND': {
                truthTable: [
                    { a: 0, b: 0, output: 0 },
                    { a: 0, b: 1, output: 0 },
                    { a: 1, b: 0, output: 0 },
                    { a: 1, b: 1, output: 1 }
                ],
                description: "Returns true only if both inputs are true"
            },
            'OR': {
                truthTable: [
                    { a: 0, b: 0, output: 0 },
                    { a: 0, b: 1, output: 1 },
                    { a: 1, b: 0, output: 1 },
                    { a: 1, b: 1, output: 1 }
                ],
                description: "Returns true if at least one input is true"
            },
            'XOR': {
                truthTable: [
                    { a: 0, b: 0, output: 0 },
                    { a: 0, b: 1, output: 1 },
                    { a: 1, b: 0, output: 1 },
                    { a: 1, b: 1, output: 0 }
                ],
                description: "Returns true if exactly one input is true"
            },
            'NOT': {
                truthTable: [
                    { a: 0, output: 1 },
                    { a: 1, output: 0 }
                ],
                description: "Inverts the input (true becomes false, false becomes true)"
            },
            'NAND': {
                truthTable: [
                    { a: 0, b: 0, output: 1 },
                    { a: 0, b: 1, output: 1 },
                    { a: 1, b: 0, output: 1 },
                    { a: 1, b: 1, output: 0 }
                ],
                description: "Returns false only if both inputs are true"
            },
            'NOR': {
                truthTable: [
                    { a: 0, b: 0, output: 1 },
                    { a: 0, b: 1, output: 0 },
                    { a: 1, b: 0, output: 0 },
                    { a: 1, b: 1, output: 0 }
                ],
                description: "Returns true only if both inputs are false"
            }
        };
        
        // Generate a question to identify a gate from its truth table
        function generateIdentifyGateQuestion() {
            const gateNames = Object.keys(gates);
            const gateName = gateNames[Math.floor(Math.random() * gateNames.length)];
            const gate = gates[gateName];
            
            const options = ['AND', 'OR', 'XOR', 'NOT', 'NAND', 'NOR'].filter(option => 
                (option !== gateName) && (option === 'NOT' ? gate.truthTable.length === 2 : gate.truthTable.length === 4)
            );
            
            // Shuffle options and include the correct answer
            const shuffledOptions = shuffleArray([gateName, ...options.slice(0, 3)]);
            
            return {
                type: 'identify-gate',
                text: "Which logic gate does this truth table represent?",
                gate: gateName,
                truthTable: gate.truthTable,
                options: shuffledOptions,
                correctAnswer: gateName
            };
        }
        
        // Generate a question to fill in a truth table value
        function generateFillTruthTableQuestion() {
            const gateNames = Object.keys(gates).filter(name => name !== 'NOT'); // Exclude NOT gate for this question type
            const gateName = gateNames[Math.floor(Math.random() * gateNames.length)];
            const gate = gates[gateName];
            
            // Select a random row from the truth table
            const rowIndex = Math.floor(Math.random() * gate.truthTable.length);
            const row = gate.truthTable[rowIndex];
            
            return {
                type: 'fill-truth-table',
                text: `For the ${gateName} gate, what is the output when A = ${row.a} and B = ${row.b}?`,
                gate: gateName,
                rowIndex: rowIndex,
                options: [0, 1],
                correctAnswer: row.output
            };
        }
        
        // Generate a question to predict the output of a compound gate
        function generatePredictOutputQuestion() {
            const gateNames = Object.keys(gates).filter(name => name !== 'NOT'); // Exclude NOT for simplicity
            const gateName = gateNames[Math.floor(Math.random() * gateNames.length)];
            
            // Generate two random inputs
            const a = Math.round(Math.random());
            const b = Math.round(Math.random());
            
            // Calculate the correct output
            let output;
            switch(gateName) {
                case 'AND':
                    output = a && b ? 1 : 0;
                    break;
                case 'OR':
                    output = a || b ? 1 : 0;
                    break;
                case 'XOR':
                    output = (a && !b) || (!a && b) ? 1 : 0;
                    break;
                case 'NAND':
                    output = !(a && b) ? 1 : 0;
                    break;
                case 'NOR':
                    output = !(a || b) ? 1 : 0;
                    break;
                default:
                    output = 0;
            }
            
            return {
                type: 'predict-output',
                text: `What is the output of a ${gateName} gate with inputs A = ${a} and B = ${b}?`,
                gate: gateName,
                inputs: { a, b },
                options: [0, 1],
                correctAnswer: output
            };
        }
        
        // Initialize the game
        function initGame() {
            currentQuestionIndex = 0;
            score = 0;
            questions = [];
            answerSubmitted = false;
            selectedOption = null;
            
            // Generate 10 questions
            for (let i = 0; i < 10; i++) {
                const questionType = questionTypes[Math.floor(Math.random() * questionTypes.length)];
                questions.push(questionType.generateQuestion());
            }
            
            updateScoreDisplay();
            loadQuestion();
            
            // Hide the result and show the game content
            document.getElementById('result').style.display = 'none';
            document.getElementById('game-content').style.display = 'block';
            document.getElementById('next-button').style.display = 'none';
            
            // Set up next button event listener
            document.getElementById('next-button').addEventListener('click', nextQuestion);
        }
        
        // Load the current question
        function loadQuestion() {
            const questionContainer = document.getElementById('question-container');
            const optionsContainer = document.getElementById('options-container');
            const truthTableContainer = document.getElementById('truth-table-container');
            const feedbackElement = document.getElementById('feedback');
            const nextButton = document.getElementById('next-button');
            
            // Reset state for new question
            answerSubmitted = false;
            selectedOption = null;
            
            // Hide feedback and next button
            feedbackElement.classList.add('hidden');
            feedbackElement.classList.remove('correct', 'incorrect');
            nextButton.style.display = 'none';
            
            if (currentQuestionIndex < questions.length) {
                const question = questions[currentQuestionIndex];
                document.getElementById('question-text').textContent = question.text;
                
                // Clear previous options
                optionsContainer.innerHTML = '';
                
                // Handle different question types
                if (question.type === 'identify-gate') {
                    // Show truth table
                    truthTableContainer.classList.remove('hidden');
                    const tableBody = document.getElementById('table-body');
                    tableBody.innerHTML = '';
                    
                    if (question.gate === 'NOT') {
                        // Update table header for NOT gate
                        document.querySelector('#truth-table thead tr').innerHTML = '<th>A</th><th>NOT A</th>';
                        
                        question.truthTable.forEach(row => {
                            const tr = document.createElement('tr');
                            tr.innerHTML = `
                                <td>${row.a}</td>
                                <td>${row.output}</td>
                            `;
                            tableBody.appendChild(tr);
                        });
                    } else {
                        // Update table header for two-input gates
                        document.querySelector('#truth-table thead tr').innerHTML = '<th>A</th><th>B</th><th>Output</th>';
                        
                        question.truthTable.forEach(row => {
                            const tr = document.createElement('tr');
                            tr.innerHTML = `
                                <td>${row.a}</td>
                                <td>${row.b}</td>
                                <td>${row.output}</td>
                            `;
                            tableBody.appendChild(tr);
                        });
                    }
                    
                    // Create option buttons
                    question.options.forEach(option => {
                        const button = document.createElement('button');
                        button.textContent = option;
                        button.setAttribute('data-value', option);
                        button.onclick = () => selectAnswer(button, option);
                        optionsContainer.appendChild(button);
                    });
                } else if (question.type === 'fill-truth-table' || question.type === 'predict-output') {
                    // Hide truth table
                    truthTableContainer.classList.add('hidden');
                    
                    // Create option buttons
                    question.options.forEach(option => {
                        const button = document.createElement('button');
                        button.textContent = option;
                        button.setAttribute('data-value', option);
                        button.onclick = () => selectAnswer(button, option);
                        optionsContainer.appendChild(button);
                    });
                }
            } else {
                endGame();
            }
        }
        
        // Handle answer selection
        function selectAnswer(button, answer) {
            // Prevent selecting an answer if one is already submitted
            if (answerSubmitted) return;
            
            const optionsContainer = document.getElementById('options-container');
            const buttons = optionsContainer.querySelectorAll('button');
            const question = questions[currentQuestionIndex];
            const feedbackElement = document.getElementById('feedback');
            const nextButton = document.getElementById('next-button');
            
            // Remove selected class from all buttons
            buttons.forEach(btn => {
                btn.classList.remove('selected', 'correct', 'incorrect');
            });
            
            // Add selected class to clicked button
            button.classList.add('selected');
            selectedOption = answer;
            
            // Check answer
            if (answer === question.correctAnswer) {
                score++;
                feedbackElement.textContent = "Correct!";
                feedbackElement.classList.add('correct');
                feedbackElement.classList.remove('incorrect');
                button.classList.add('correct');
            } else {
                feedbackElement.textContent = `Incorrect. The correct answer is ${question.correctAnswer}.`;
                feedbackElement.classList.add('incorrect');
                feedbackElement.classList.remove('correct');
                button.classList.add('incorrect');
                
                // Highlight the correct answer
                buttons.forEach(btn => {
                    if (btn.getAttribute('data-value') == question.correctAnswer) {
                        btn.classList.add('correct');
                    }
                });
            }
            
            // Mark answer as submitted
            answerSubmitted = true;
            
            // Disable all buttons
            buttons.forEach(btn => {
                btn.disabled = true;
            });
            
            // Show feedback and next button
            feedbackElement.classList.remove('hidden');
            nextButton.style.display = 'inline-block';
            
            // Update score display
            updateScoreDisplay();
        }
        
        // Move to the next question
        function nextQuestion() {
            currentQuestionIndex++;
            loadQuestion();
        }
        
        // Update the score display and progress bar
        function updateScoreDisplay() {
            document.getElementById('score-display').textContent = `Score: ${score}/${currentQuestionIndex} (${questions.length} total)`;
            
            const progressPercentage = (currentQuestionIndex / questions.length) * 100;
            document.getElementById('progress-bar').style.width = `${progressPercentage}%`;
        }
        
        // End the game and show the result
        function endGame() {
            document.getElementById('game-content').style.display = 'none';
            const resultElement = document.getElementById('result');
            resultElement.style.display = 'block';
            
            const passed = score >= 7;
            resultElement.innerHTML = `
                <p>Game Over!</p>
                <p>Your score: ${score}/${questions.length} ${passed ? '✅ You passed!' : '❌ You need 7/10 to pass.'}</p>
                <button onclick="restartGame()">Play Again</button>
            `;
        }
        
        // Restart the game
        function restartGame() {
            initGame();
        }
        
        // Shuffle an array using Fisher-Yates algorithm
        function shuffleArray(array) {
            const newArray = [...array];
            for (let i = newArray.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
            }
            return newArray;
        }
        
        // Anti-inspection measures
        document.addEventListener('contextmenu', function(e) {
            e.preventDefault();
            return false;
        });
        
        document.addEventListener('keydown', function(e) {
            if (e.key === 'F12' || 
                (e.ctrlKey && e.shiftKey && e.keyCode === 73) ||
                (e.metaKey && e.altKey && e.keyCode === 73) ||
                (e.ctrlKey && e.shiftKey && e.keyCode === 74) ||
                (e.metaKey && e.altKey && e.keyCode === 74) ||
                (e.ctrlKey && e.shiftKey && e.keyCode === 67) ||
                (e.metaKey && e.altKey && e.keyCode === 67) ||
                (e.ctrlKey && e.keyCode === 85) ||
                (e.metaKey && e.keyCode === 85)) {
                e.preventDefault();
                return false;
            }
        });
        
        // Detect DevTools
        let devToolsDetector = function() {
            const widthThreshold = window.outerWidth - window.innerWidth > 160;
            const heightThreshold = window.outerHeight - window.innerHeight > 160;
            
            if (widthThreshold || heightThreshold) {
                document.body.innerHTML = '<div style="text-align:center;padding:50px;"><h1>Developer Tools Detected</h1><p>Please close the developer tools to continue.</p></div>';
            }
        };
        
        setInterval(devToolsDetector, 1000);
        
        // Console warning
        console.clear();
        console.log("%cWarning!", "color: red; font-size: 30px; font-weight: bold;");
        console.log("%cThis is a secure page. Attempting to inspect elements or use developer tools is not allowed.", "font-size: 16px;");
        
        // Initialize the game
        initGame();
    </script>
</body>
</html>