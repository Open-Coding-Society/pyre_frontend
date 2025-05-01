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
