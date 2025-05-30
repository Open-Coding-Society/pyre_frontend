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
                <section class="w-full md:w-1/2 bg-gray-900 bg-opacity-80 rounded-lg shadow-xl p-8 backdrop-blur">
                        <h2 class="text-2xl font-bold text-white mb-6" id="pythonTitle">User Login (Python/Flask)</h2>
                        <form class="space-y-4" id="pythonForm" onsubmit="pythonLogin(); return false;">
                                <label class="block text-gray-400 text-sm mb-2" for="uid">GitHub ID:</label>
                                <input type="text" id="uid" name="uid" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">

                                <label class="block text-gray-400 text-sm mb-2" for="password">Password:</label>
                                <input type="password" id="password" name="password" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">

                                <div class="pt-2">
                                        <button type="submit" class="w-full py-3 px-4 bg-gray-800 hover:bg-gray-700 text-white font-medium rounded shadow transition duration-300">
                                                Login
                                        </button>
                                </div>
                                <div id="message" class="text-red-500 text-sm"></div>
                        </form>
                </section>
                <!-- Sign Up Form -->
                <section class="w-full md:w-1/2 bg-gray-900 bg-opacity-80 rounded-lg shadow-xl p-8 backdrop-blur">
                        <h2 class="text-2xl font-bold text-white mb-6" id="signupTitle">Sign Up</h2>
                        <form class="space-y-4" id="signupForm" onsubmit="signup(); return false;">
                                <label class="block text-gray-400 text-sm mb-2" for="name">Name:</label>
                                <input type="text" id="name" name="name" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">

                                <label class="block text-gray-400 text-sm mb-2" for="signupUid">GitHub ID:</label>
                                <input type="text" id="signupUid" name="signupUid" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">

                                <label class="block text-gray-400 text-sm mb-2" for="signupPassword">Password:</label>
                                <input type="password" id="signupPassword" name="signupPassword" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">

                                <label class="block text-white text-sm mb-2" for="zip">ZIP Code</label>
                                <input type="text" id="zip" name="zip" placeholder="e.g. 12345" required class="w-full px-4 py-3 rounded bg-gray-800 text-white border border-gray-700 focus:border-orange-500 focus:outline-none">

                                <div class="pt-2 signup-card">
                                        <button type="submit" class="w-full py-3 px-4 bg-gradient-to-r from-orange-600 to-red-600 hover:from-orange-500 hover:to-red-500 text-white font-medium rounded shadow transition duration-300">
                                                Sign Up
                                        </button>
                                </div>
                                <div id="signupMessage" class="text-green-500 text-sm"></div>
                        </form>
                </section>
        </div>
        <footer class="w-full py-6 text-center mt-8">
                <div class="text-gray-500 text-sm">© 2025 Pyre. All rights reserved.</div>
        </footer>
</div>

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

<a href="/pyre_frontend/help/" class="fixed bottom-4 right-4 bg-green-600 text-white rounded-full p-3 shadow-lg hover:bg-green-700 transition duration-200 flex items-center justify-center" title="Help Center" style="font-size:1.05em;">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9 5.25h.008v.008H12v-.008z"/>
                </svg>
                <span class="ml-1 font-medium">Help</span>
        </a>
