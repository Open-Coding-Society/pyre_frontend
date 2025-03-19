---
layout: post
title: Beneficial and Harmful Game
search_exclude: true
permalink: /thisorthat/
---

<title>This or That: Machine Learning</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background-color:rgb(1, 1, 2);
        color: #333;
        margin: 20px;
        padding: 0;
        text-align: center;
    }
    h1 {
        color: #4CAF50;
    }
    .question {
        margin-bottom: 20px;
        font-size: 18px;
        padding: 10px;
        background-color: #000;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .buttons-container {
        display: flex;
        justify-content: center;
        gap: 20px;
        margin-top: 20px;
    }
    button {
        background-color: #4CAF50;
        color: white;
        padding: 12px 24px;
        margin: 10px;
        border: none;
        cursor: pointer;
        border-radius: 5px;
        font-size: 16px;
        transition: all 0.3s ease;
        box-shadow: 0 2px 4px rgba(0,0,0,0.2);
    }
    button:hover {
        background-color: #45a049;
        transform: translateY(-2px);
    }
    button:active {
        transform: translateY(0);
    }
    .result {
        font-size: 20px;
        margin-top: 30px;
        font-weight: bold;
        padding: 15px;
        background-color: #000;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        display: inline-block;
    }
    .progress {
        margin: 20px auto;
        width: 80%;
        max-width: 500px;
        background-color:rgb(0, 0, 0);
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
        margin-top: 10px;
        font-size: 16px;
    }
</style>

<h1>This or That: Machine Learning</h1>

<div class="progress">
    <div class="progress-bar" id="progress-bar"></div>
</div>

<div class="score-display" id="score-display">Score: 0</div>

<div id="game-container">
    <div class="question" id="question">
        <!-- Question will be inserted here -->
    </div>
    <div class="buttons-container" id="buttons-container">
        <button id="button1" onclick="choose(this.getAttribute('data-choice'))">Benefit</button>
        <button id="button2" onclick="choose(this.getAttribute('data-choice'))">Harm</button>
    </div>
</div>

<div class="result" id="result" style="display: none;"></div>

<script>
    const questions = [
        { text: "Machine learning improves medical diagnostics.", answer: "benefit" },
        { text: "Machine learning can reinforce biased decision-making.", answer: "harm" },
        { text: "Machine learning helps with fraud detection.", answer: "benefit" },
        { text: "Machine learning can lead to job loss due to automation.", answer: "harm" },
        { text: "Machine learning improves customer service through chatbots.", answer: "benefit" },
        { text: "Machine learning can lead to privacy invasion.", answer: "harm" },
        { text: "Machine learning helps autonomous vehicles navigate.", answer: "benefit" },
        { text: "Machine learning can lead to misinformation and deepfakes.", answer: "harm" },
        { text: "Machine learning enables better weather prediction models.", answer: "benefit" },
        { text: "Machine learning can cause unfair treatment in loan approvals.", answer: "harm" },
        { text: "Machine learning allows personalized recommendations on streaming platforms.", answer: "benefit" },
        { text: "Machine learning can lead to over-reliance on automated decision-making.", answer: "harm" }
    ];

    let currentQuestionIndex = 0;
    let score = 0;
    let shuffledQuestions = [];

    // Fisher-Yates shuffle algorithm
    function shuffleArray(array) {
        let currentIndex = array.length, randomIndex;
        
        // While there remain elements to shuffle
        while (currentIndex != 0) {
            // Pick a remaining element
            randomIndex = Math.floor(Math.random() * currentIndex);
            currentIndex--;
            
            // And swap it with the current element
            [array[currentIndex], array[randomIndex]] = [array[randomIndex], array[currentIndex]];
        }
        
        return array;
    }

    function initGame() {
        // Shuffle questions at the start of the game
        shuffledQuestions = shuffleArray([...questions]);
        currentQuestionIndex = 0;
        score = 0;
        updateScoreDisplay();
        loadQuestion();
    }

    function updateScoreDisplay() {
        document.getElementById('score-display').textContent = `Score: ${score}`;
        
        // Update progress bar
        const progressPercentage = (currentQuestionIndex / shuffledQuestions.length) * 100;
        document.getElementById('progress-bar').style.width = `${progressPercentage}%`;
    }

    function loadQuestion() {
        if (currentQuestionIndex < shuffledQuestions.length) {
            const currentQuestion = shuffledQuestions[currentQuestionIndex];
            document.getElementById('question').textContent = currentQuestion.text;
            
            // Randomize button order
            const buttons = document.getElementById('buttons-container').children;
            const button1 = document.getElementById('button1');
            const button2 = document.getElementById('button2');
            
            // Randomly decide which button is which
            if (Math.random() > 0.5) {
                button1.textContent = "Benefit";
                button1.setAttribute('data-choice', 'benefit');
                button2.textContent = "Harm";
                button2.setAttribute('data-choice', 'harm');
            } else {
                button1.textContent = "Harm";
                button1.setAttribute('data-choice', 'harm');
                button2.textContent = "Benefit";
                button2.setAttribute('data-choice', 'benefit');
            }
            
            // Show the question
            document.getElementById('game-container').style.display = 'block';
            document.getElementById('result').style.display = 'none';
            
        } else {
            endGame();
        }
    }

    function choose(choice) {
        if (currentQuestionIndex < shuffledQuestions.length) {
            if (choice === shuffledQuestions[currentQuestionIndex].answer) {
                score++;
                updateScoreDisplay();
            }
            currentQuestionIndex++;
            updateScoreDisplay();
            loadQuestion();
        }
    }

    function endGame() {
        document.getElementById('game-container').style.display = 'none';
        document.getElementById('result').style.display = 'block';

        if (score >= 9) {
            document.getElementById('result').innerHTML = `
                Game Over! Your score: ${score}/${shuffledQuestions.length} ✅ You passed!<br>
                <button onclick="restartGame()" style="margin-top: 20px;">Play Again</button>
            `;
        } else {
            document.getElementById('result').innerHTML = `
                Game Over! Your score: ${score}/${shuffledQuestions.length} ❌ You must score at least 9/12 to pass.<br>
                <button onclick="restartGame()" style="margin-top: 20px;">Try Again</button>
            `;
        }
    }

    function restartGame() {
        initGame();
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
    
    // Initialize game
    initGame();
</script>