---
layout: tailwind
title: Login
permalink: /login
search_exclude: true
show_reading_time: false
---

<div class="min-h-screen w-full flex flex-col items-center justify-center bg-black bg-opacity-95 bg-[url('/api/placeholder/1920/1080')] bg-cover bg-center bg-blend-overlay py-12 px-4">
  <br>
  <br>
  <div class="flex flex-col md:flex-row gap-8 w-full max-w-4xl">
    <!-- Python Login Form -->
    <div class="w-full md:w-1/2 bg-gray-900 bg-opacity-80 rounded-lg shadow-xl p-8 backdrop-blur">
      <h2 class="text-2xl font-bold text-white mb-6" id="pythonTitle">User Login (Python/Flask)</h2>
      <form class="space-y-4" id="pythonForm" onsubmit="pythonLogin(); return false;">
        <div>
          <label class="block text-gray-400 text-sm mb-2">GitHub ID:</label>
          <input type="text" id="uid" name="uid" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">
        </div>
        <div>
          <label class="block text-gray-400 text-sm mb-2">Password:</label>
          <input type="password" id="password" name="password" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">
        </div>
        <div class="pt-2">
          <button type="submit" class="w-full py-3 px-4 bg-gray-800 hover:bg-gray-700 text-white font-medium rounded shadow transition duration-300">
            Login
          </button>
        </div>
        <div id="message" class="text-red-500 text-sm"></div>
      </form>
    </div>
    <!-- Sign Up Form -->
    <div class="w-full md:w-1/2 bg-gray-900 bg-opacity-80 rounded-lg shadow-xl p-8 backdrop-blur">
      <h2 class="text-2xl font-bold text-white mb-6" id="signupTitle">Sign Up</h2>
      <form class="space-y-4" id="signupForm" onsubmit="signup(); return false;">
        <div>
          <label class="block text-gray-400 text-sm mb-2">Name:</label>
          <input type="text" id="name" name="name" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">
        </div>
        <div>
          <label class="block text-gray-400 text-sm mb-2">GitHub ID:</label>
          <input type="text" id="signupUid" name="signupUid" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">
        </div>
        <div>
          <label class="block text-gray-400 text-sm mb-2">Password:</label>
          <input type="password" id="signupPassword" name="signupPassword" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">
        </div>
        <div>
          <label class="block text-gray-400 text-sm mb-2">Interests:</label>
          <input type="text" id="interests" name="interests" placeholder="e.g., Soccer, Pool, Computer Science" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">
        </div>
        <div class="pt-2 signup-card">
          <button type="submit" class="w-full py-3 px-4 bg-gradient-to-r from-orange-600 to-red-600 hover:from-orange-500 hover:to-red-500 text-white font-medium rounded shadow transition duration-300">
            Sign Up
          </button>
        </div>
        <div id="signupMessage" class="text-green-500 text-sm"></div>
      </form>
    </div>
  </div>
  <footer class="w-full py-6 text-center mt-8">
    <div class="text-gray-500 text-sm">© 2025 Pyre. All rights reserved.</div>
  </footer>
</div>

<!-- <div class="login-container">
    <div class="login-card">
        <h1 id="pythonTitle">User Login (Python/Flask)</h1>
        <form id="pythonForm" onsubmit="pythonLogin(); return false;">
            <p>
                <label>
                    GitHub ID:
                    <input type="text" name="uid" id="uid" required>
                </label>
            </p>
            <p>
                <label>
                    Password:
                    <input type="password" name="password" id="password" required>
                </label>
            </p>
            <p>
                <button type="submit">Login</button>
            </p>
            <p id="message" style="color: red;"></p>
        </form>
    </div>
    <div class="signup-card">
        <h1 id="signupTitle">Sign Up</h1>
        <form id="signupForm" onsubmit="signup(); return false;">
            <p>
                <label>
                    Name:
                    <input type="text" name="name" id="name" required>
                </label>
            </p>
            <p>
                <label>
                    GitHub ID:
                    <input type="text" name="signupUid" id="signupUid" required>
                </label>
            </p>
            <p>
                <label>
                    Password:
                    <input type="password" name="signupPassword" id="signupPassword" required>
                </label>
            </p>
            <p>
                <label>
                    Interests:
                    <input type="text" name="interests" id="interests" placeholder="e.g., Soccer, Pool, Computer Science" required>
                </label>
            </p>
            <p>
                <button type="submit">Sign Up</button>
            </p>
            <p id="signupMessage" style="color: green;"></p>
        </form>
    </div>
</div> -->

<script type="module">
    import { login, pythonURI, fetchOptions } from '{{site.baseurl}}/assets/js/api/config.js';

    // Function to handle Python login
    window.pythonLogin = function() {
        const options = {
            URL: `${pythonURI}/api/authenticate`,
            callback: handleLoginResponse,
            message: "message",
            method: "POST",
            cache: "no-cache",
            body: {
                uid: document.getElementById("uid").value,
                password: document.getElementById("password").value,
            }
        };
        login(options);
    }

    // Function to handle signup
    window.signup = function() {
        const signupButton = document.querySelector(".signup-card button");

        // Disable the button and change its color
        signupButton.disabled = true;
        signupButton.style.backgroundColor = '#d3d3d3'; // Light gray to indicate disabled state

        const signupOptions = {
            URL: `${pythonURI}/api/user`,
            method: "POST",
            cache: "no-cache",
            body: {
                name: document.getElementById("name").value,
                uid: document.getElementById("signupUid").value,
                password: document.getElementById("signupPassword").value,
                interests: document.getElementById("interests").value, // Include interests
            }
        };

        fetch(signupOptions.URL, {
            method: signupOptions.method,
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify(signupOptions.body)
        })
        .then(response => {
            if (!response.ok) {
                throw new Error(`Signup failed: ${response.status}`);
            }
            return response.json();
        })
        .then(data => {
            document.getElementById("signupMessage").textContent = "Signup successful!";
            // Optionally redirect to login page or handle as needed
            // window.location.href = '{{site.baseurl}}/profile';
        })
        .catch(error => {
            console.error("Signup Error:", error);
            document.getElementById("signupMessage").textContent = `Signup Error: ${error.message}`;
            // Re-enable the button if there is an error
            signupButton.disabled = false;
            signupButton.style.backgroundColor = ''; // Reset to default color
        });
    };

    // Function to handle login response
    function handleLoginResponse() {
        const URL = `${pythonURI}/api/id`;

        fetch(URL, fetchOptions)
            .then(response => {
                if (!response.ok) {
                    throw new Error(`Flask server response: ${response.status}`);
                }
                return response.json();
            })
            .then(data => {
                if (data.role === 'admin') {
                    window.location.href = '{{site.baseurl}}/adminlog';
                } else {
                    window.location.href = '{{site.baseurl}}/userlog';
                }
            })
            .catch(error => {
                console.error("Python Database Error:", error);
                const errorMsg = `Python Database Error: ${error.message}`;
                document.getElementById("message").textContent = errorMsg;
            });
    }

    // Call relevant database functions on the page load
    window.onload = function() {
         pythonDatabase();
    };
</script>