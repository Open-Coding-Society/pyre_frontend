---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /notification/
title: Notification
---

<div class="flex flex-col items-center justify-center mt-10 space-y-4">
  <h2 class="text-2xl font-semibold text-gray-100">Test a Phone Call for Fire Notifications</h2>

  <div class="flex flex-col sm:flex-row items-center space-y-4 sm:space-y-0 sm:space-x-4">
    <input
      type="tel"
      id="phoneNumber"
      placeholder="+15551234567"
      class="px-4 py-2 rounded-md bg-gray-800 text-gray-100 placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-red-500"
    />
    <button
      id="callButton"
      class="px-6 py-2 rounded-md bg-red-600 hover:bg-red-700 text-white font-medium transition"
      onclick="phoneButton()"
    >
      Make Call
    </button>
  </div>

  <div id="status" class="text-sm text-gray-400"></div>
</div>

<script type="module">
    import { pythonURI, fetchOptions } from '/QcommVNE_Frontend/assets/js/api/config.js';

    const callButton = document.getElementById('callButton');
    const phoneNumberInput = document.getElementById('phoneNumber');
    const statusElement = document.getElementById('status');

    callButton.addEventListener('click', function() {
        console.log('hi');
        const phoneNumber = phoneNumberInput.value;

        if (!phoneNumber) {
            statusElement.textContent = "Please enter a phone number";
            return;
        }

        statusElement.textContent = "Initiating call...";

        fetch(`${pythonURI}/make-call`, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            credentials: 'include',
            body: JSON.stringify({ to_number: phoneNumber })
        })
        .then(response => {
            if (!response.ok) {
                return response.json().then(data => {
                    throw new Error(data.error || 'Something went wrong');
                });
            }
            return response.json();
        })
        .then(data => {
            statusElement.textContent = "Call initiated! Status: " + data.status;
        })
        .catch(error => {
            statusElement.textContent = "Error: " + error.message;
        });
    });
</script>

<a href="/QcommVNE_Frontend/help/" class="fixed bottom-4 right-4 bg-green-600 text-white rounded-full p-3 shadow-lg hover:bg-green-700 transition duration-200 flex items-center justify-center" title="Help Center" style="font-size:1.05em;">
    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.879 7.519c1.171-1.025 3.071-1.025 4.242 0 1.172 1.025 1.172 2.687 0 3.712-.203.179-.43.326-.67.442-.745.361-1.45.999-1.45 1.827v.75M21 12a9 9 0 11-18 0 9 9 0 0118 0zm-9 5.25h.008v.008H12v-.008z"/>
    </svg>
    <span class="ml-1 font-medium">Help</span>
  </a>