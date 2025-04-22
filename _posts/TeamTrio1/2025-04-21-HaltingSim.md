---
layout: post
search_exclude: true
show_reading_time: false
permalink: /HSim
title: Halting Simulator
categories: [TeamTeachTrio1]
---

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Halting Machine Simulator</title>
  <style>
    body {
      font-family: 'Courier New', monospace;
      display: flex;
      flex-direction: column;
      align-items: center;
      background-color: #1e1e1e;
      color: #ddd;
      margin: 0;
      padding: 20px;
      height: 100vh;
    }
    .container {
      width: 100%;
      max-width: 800px;
      text-align: center;
    }
    .display {
      background-color: #000;
      border-radius: 5px;
      padding: 20px;
      margin: 20px 0;
      height: 300px;
      overflow: auto;
      display: flex;
      flex-direction: column;
      text-align: left;
    }
    .log {
      margin: 2px 0;
      font-size: 14px;
    }
    .success { color: #4CAF50; }
    .error { color: #F44336; }
    .warning { color: #FFC107; }
    .info { color: #2196F3; }
    .controls {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      justify-content: center;
      margin-bottom: 20px;
    }
    button {
      background-color: #333;
      color: #fff;
      border: 1px solid #555;
      border-radius: 3px;
      padding: 8px 16px;
      cursor: pointer;
      font-family: 'Courier New', monospace;
      transition: background-color 0.3s;
    }
    button:hover {
      background-color: #444;
    }
    button:disabled {
      background-color: #222;
      color: #666;
      cursor: not-allowed;
    }
    textarea {
      background-color: #111;
      color: #ddd;
      border: 1px solid #555;
      border-radius: 3px;
      padding: 10px;
      font-family: 'Courier New', monospace;
      width: 100%;
      resize: vertical;
    }
    h1 {
      margin: 10px 0;
    }
    .status {
      display: flex;
      align-items: center;
      gap: 10px;
      margin: 10px 0;
      justify-content: center;
    }
    .status-indicator {
      width: 15px;
      height: 15px;
      border-radius: 50%;
      background-color: #555;
    }
    .status-indicator.running {
      background-color: #2196F3;
      animation: blink 1s infinite;
    }
    .status-indicator.halted {
      background-color: #F44336;
    }
    .status-indicator.completed {
      background-color: #4CAF50;
    }
    @keyframes blink {
      0% { opacity: 1; }
      50% { opacity: 0.5; }
      100% { opacity: 1; }
    }
    #exampleSelect {
      background-color: #333;
      color: #ddd;
      border: 1px solid #555;
      padding: 8px;
      border-radius: 3px;
    }
    .help-section {
      background-color: #222;
      padding: 15px;
      border-radius: 5px;
      margin-top: 20px;
      text-align: left;
    }
    .help-section h2 {
      margin-top: 0;
    }
    pre {
      background-color: #111;
      padding: 10px;
      border-radius: 3px;
      overflow-x: auto;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Halting Machine Simulator</h1>
    
    <div class="status">
      <div id="statusIndicator" class="status-indicator"></div>
      <span id="statusText">Ready</span>
    </div>
    
    <div class="controls">
      <button id="runBtn">Run</button>
      <button id="stopBtn" disabled>Force Stop</button>
      <button id="clearBtn">Clear Output</button>
      <button id="resetBtn">Reset</button>
      <select id="exampleSelect">
        <option value="">Select Example...</option>
        <option value="simple">Simple Halting Program</option>
        <option value="infinite">Infinite Loop</option>
        <option value="unpredictable">Unpredictable Halting</option>
        <option value="conditional">Conditional Halting</option>
      </select>
    </div>
    
    <div>
      <h3>Program:</h3>
      <textarea id="codeInput" rows="5" placeholder="Enter your program here...">function run(iterations) {
  let i = 0;
  while (i < iterations) {
    log("Processing iteration " + i);
    i++;
  }
  return "Program completed after " + iterations + " iterations";
}</textarea>
    </div>
    
    <div class="display" id="outputDisplay"></div>
    
    <div class="help-section">
      <h2>How to Use</h2>
      <p>This simulator demonstrates the concept of the halting problem. You can write simple programs that may or may not halt.</p>
      <p>Available functions:</p>
      <ul>
        <li><code>log(message)</code> - Print a message to the output</li>
        <li><code>wait(ms)</code> - Wait for specified milliseconds</li>
        <li><code>random()</code> - Generate a random number between 0 and 1</li>
      </ul>
      <p>Example program that may halt:</p>
      <pre>function run() {
  let x = 10;
  while (x > 0) {
    log("Current value: " + x);
    x--;
    wait(500);
  }
  return "Program halted successfully";
}</pre>
    </div>
  </div>
  
  <script>
    const codeInput = document.getElementById('codeInput');
    const outputDisplay = document.getElementById('outputDisplay');
    const runBtn = document.getElementById('runBtn');
    const stopBtn = document.getElementById('stopBtn');
    const clearBtn = document.getElementById('clearBtn');
    const resetBtn = document.getElementById('resetBtn');
    const exampleSelect = document.getElementById('exampleSelect');
    const statusIndicator = document.getElementById('statusIndicator');
    const statusText = document.getElementById('statusText');
    
    let running = false;
    let executionTimeout = null;
    let executionInterval = null;
    let shouldStop = false;
    
    // Example programs
    const examples = {
      simple: `function run() {
  log("Starting program...");
  for (let i = 0; i < 5; i++) {
    log("Processing step " + i);
    wait(800);
  }
  return "Program halted successfully";
}`,
      infinite: `function run() {
  log("Starting infinite loop...");
  let counter = 0;
  while (true) {
    log("Iteration " + counter++);
    // This will never halt without intervention
    wait(500);
  }
}`,
      unpredictable: `function run() {
  log("Starting unpredictable program...");
  let iterations = 0;
  
  while (true) {
    iterations++;
    log("Iteration " + iterations);
    
    // Random chance of halting
    if (random() < 0.15) {
      return "Program decided to halt after " + iterations + " iterations";
    }
    
    wait(500);
    
    // This is just to prevent browser from freezing
    if (iterations > 100) {
      return "Safety limit reached";
    }
  }
}`,
      conditional: `function run() {
  let number = Math.floor(random() * 100);
  log("Searching for factors of " + number);
  
  for (let i = 2; i < number; i++) {
    log("Testing if " + i + " is a factor of " + number);
    wait(500);
    
    if (number % i === 0) {
      return "Halting: Found factor " + i + " of " + number;
    }
  }
  
  return "Halting: " + number + " is prime";
}`
    };
    
    // Helper functions available in the sandbox
    function log(message, type = 'info') {
      const logElement = document.createElement('div');
      logElement.classList.add('log', type);
      logElement.textContent = message;
      outputDisplay.appendChild(logElement);
      outputDisplay.scrollTop = outputDisplay.scrollHeight;
    }
    
    function wait(ms) {
      return new Promise(resolve => {
        if (shouldStop) {
          throw new Error("Program execution was stopped by user");
        }
        setTimeout(resolve, ms);
      });
    }
    
    function random() {
      return Math.random();
    }
    
    function updateStatus(status) {
      statusIndicator.className = 'status-indicator';
      if (status === 'running') {
        statusIndicator.classList.add('running');
        statusText.textContent = 'Running...';
      } else if (status === 'halted') {
        statusIndicator.classList.add('halted');
        statusText.textContent = 'Halted';
      } else if (status === 'completed') {
        statusIndicator.classList.add('completed');
        statusText.textContent = 'Completed';
      } else {
        statusText.textContent = 'Ready';
      }
    }
    
    // Execute the code in a controlled environment
    async function executeCode(code) {
      running = true;
      shouldStop = false;
      updateStatus('running');
      runBtn.disabled = true;
      stopBtn.disabled = false;
      
      try {
        // Create a sandbox function
        const sandboxFunction = new Function('log', 'wait', 'random', `
          return async function() {
            try {
              ${code}
              // Try to find and execute the run function
              if (typeof run === "function") {
                return await run();
              } else {
                throw new Error("No 'run' function defined");
              }
            } catch (error) {
              throw error;
            }
          }
        `);
        
        // Execute the sandbox function
        const runFunction = sandboxFunction(log, wait, random);
        
        // Set a timeout for the execution
        const executionPromise = runFunction();
        
        // Add a timeout to detect infinite loops
        const timeoutPromise = new Promise((_, reject) => {
          executionTimeout = setTimeout(() => {
            reject(new Error("Execution timeout - program may be stuck in an infinite loop"));
          }, 60000); // 60 second timeout
        });
        
        // Race the execution against the timeout
        const result = await Promise.race([executionPromise, timeoutPromise]);
        
        // Clear the timeout if execution completes
        clearTimeout(executionTimeout);
        
        // Log the result
        if (result) {
          log(result, 'success');
        }
        
        updateStatus('completed');
      } catch (error) {
        log(`Error: ${error.message}`, 'error');
        updateStatus('halted');
      } finally {
        running = false;
        stopBtn.disabled = true;
        runBtn.disabled = false;
      }
    }
    
    // Event Listeners
    runBtn.addEventListener('click', () => {
      if (!running) {
        outputDisplay.innerHTML = '';
        log("Starting program execution...", 'info');
        
        // Get the code from the textarea
        const code = codeInput.value;
        
        // Execute the code
        executeCode(code);
      }
    });
    
    stopBtn.addEventListener('click', () => {
      if (running) {
        shouldStop = true;
        log("Stopping program execution...", 'warning');
        clearTimeout(executionTimeout);
        updateStatus('halted');
        stopBtn.disabled = true;
        runBtn.disabled = false;
        running = false;
      }
    });
    
    clearBtn.addEventListener('click', () => {
      outputDisplay.innerHTML = '';
    });
    
    resetBtn.addEventListener('click', () => {
      codeInput.value = 'function run(iterations) {\n  let i = 0;\n  while (i < iterations) {\n    log("Processing iteration " + i);\n    i++;\n  }\n  return "Program completed after " + iterations + " iterations";\n}';
      outputDisplay.innerHTML = '';
      updateStatus('');
      if (running) {
        shouldStop = true;
        clearTimeout(executionTimeout);
        running = false;
      }
      stopBtn.disabled = true;
      runBtn.disabled = false;
    });
    
    exampleSelect.addEventListener('change', () => {
      if (exampleSelect.value && examples[exampleSelect.value]) {
        codeInput.value = examples[exampleSelect.value];
      }
      exampleSelect.value = '';
    });
  </script>
</body>
</html>