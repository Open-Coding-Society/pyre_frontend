---
layout: post
title: Binary + Logic Gates Game
search_exclude: true
permalink: /BinaryGame/
---

# Simple Binary Game

<div id="binary-game" class="game-container">
  <h1 class="game-title">Binary Conversion Game</h1>
  
  <div class="number-display">
    <p class="instruction">Convert to binary:</p>
    <p id="current-number" class="number">0</p>
  </div>
  
  <div class="input-area">
    <input type="text" id="user-guess" placeholder="Enter binary number">
  </div>
  
  <button id="submit-button">Submit</button>
  
  <div id="feedback" class="feedback hidden"></div>
  
  <div class="score-display">
    <p>Score: <span id="score">0</span> / <span id="attempts">0</span></p>
  </div>
  
  <div class="instructions">
    <p>How to play: Convert the decimal number to binary (base-2) and enter your answer.</p>
    <p>Example: 5 in binary is 101</p>
  </div>
</div>

<style>
  .game-container {
    max-width: 500px;
    margin: 0 auto;
    padding: 20px;
    background-color:rgb(0, 0, 0);
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    font-family: Arial, sans-serif;
  }
  
  .game-title {
    text-align: center;
    margin-bottom: 20px;
    font-size: 24px;
  }
  
  .number-display {
    text-align: center;
    margin-bottom: 20px;
  }
  
  .instruction {
    font-size: 18px;
    margin-bottom: 10px;
    font-weight: bold;
  }
  
  .number {
    font-size: 48px;
    font-weight: bold;
    margin-bottom: 20px;
  }
  
  .input-area {
    margin-bottom: 15px;
  }
  
  input[type="text"] {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
    box-sizing: border-box;
  }
  
  button {
    width: 100%;
    padding: 10px;
    background-color:rgb(0, 0, 0);
    color: black;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    font-weight: bold;
    cursor: pointer;
    margin-bottom: 15px;
  }
  
  button:hover {
    background-color:rgb(0, 0, 0);
  }
  
  .feedback {
    padding: 10px;
    background-color:rgb(0, 0, 0);
    border-radius: 5px;
    margin-bottom: 15px;
  }
  
  .hidden {
    display: none;
  }
  
  .score-display {
    text-align: center;
    margin-bottom: 20px;
    font-weight: bold;
  }
  
  .instructions {
    font-size: 14px;
    color: #666;
    margin-top: 20px;
  }
</style>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    // Game variables
    let currentNumber = 0;
    let score = 0;
    let attempts = 0;
    
    // DOM elements
    const currentNumberEl = document.getElementById('current-number');
    const userGuessEl = document.getElementById('user-guess');
    const submitButton = document.getElementById('submit-button');
    const feedbackEl = document.getElementById('feedback');
    const scoreEl = document.getElementById('score');
    const attemptsEl = document.getElementById('attempts');
    
    // Generate a new random number
    function generateNewNumber() {
      currentNumber = Math.floor(Math.random() * 100);
      currentNumberEl.textContent = currentNumber;
    }
    
    // Check the user's guess
    function checkGuess() {
      const userGuess = userGuessEl.value.trim();
      const correctBinary = currentNumber.toString(2);
      
      attempts++;
      attemptsEl.textContent = attempts;
      
      if (userGuess === correctBinary) {
        feedbackEl.textContent = `Correct! ${currentNumber} in binary is ${correctBinary}`;
        feedbackEl.style.backgroundColor = 'green';    
        score++;    scoreEl.textContent = score;
        
        // Generate new number
        generateNewNumber();
        userGuessEl.value = '';
      } else {
        feedbackEl.textContent = `Incorrect. Try again! The binary representation of ${currentNumber} is ${correctBinary}`;
        feedbackEl.style.backgroundColor = 'red';    }
      
      feedbackEl.classList.remove('hidden');
    }
    
    // Event listeners
    submitButton.addEventListener('click', checkGuess);
    userGuessEl.addEventListener('keypress', function(event) {
      if (event.key === 'Enter') {
        checkGuess();
      }
    });
    
    // Initialize the game
    generateNewNumber();
  });
</script>