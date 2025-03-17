---
layout: post
title: Titanic Survival Simulator
search_exclude: true
permalink: /TitanicSimulator/
---

<header class="heading">
    <h1>Titanic Survival Simulator</h1>
    <p>Create a passenger and see if you would survive the Titanic disaster!</p>
</header>

<script>
    function showPopup() {
        const popup = document.getElementById('popup');
        const closeBtn = document.querySelector('.popup .close');

        popup.style.display = 'block';

        closeBtn.onclick = function() {
            popup.style.display = 'none';
        };

        window.onclick = function(event) {
            if (event.target == popup) {
                popup.style.display = 'none';
            }
        };
    }

    document.addEventListener("DOMContentLoaded", () => {
        showPopup();
    });
</script>

<script type="module">
    import { pythonURI, fetchOptions } from "{{site.baseurl}}/assets/js/api/config.js";

    async function checkAuthorization() {
        try {
            const response = await fetch(`${pythonURI}/api/id`, fetchOptions);

            if (response.status === 401) {
                window.location.href = "{{site.baseurl}}/login";
            } else if (response.ok) {
                const contentElements = document.querySelectorAll('.content');
                contentElements.forEach(element => {
                    element.style.display = "block";
                });
            }
        } catch (error) {
            console.error("Authorization check failed:", error);
            window.location.href = "{{site.baseurl}}/login";
        }
    }

    checkAuthorization();
</script>

<!-- Main content area with two columns -->
<div class="main">
    <!-- Left Column: Passenger Creation & Simulation -->
    <div class="left-column">
        <div class="container">
            <h2>Create Your Passenger</h2>
            <div class="game-message" id="game-message">
                <p>Fill in your details to create a Titanic passenger and test your survival chances!</p>
            </div>
            <form id="simulation-form">
                <label for="passenger-name">Passenger Name</label>
                <input type="text" id="passenger-name" placeholder="Enter your name" required />

                <label for="passenger-class">Class (1-3)</label>
                <select id="passenger-class" required>
                    <option value="1">First Class</option>
                    <option value="2">Second Class</option>
                    <option value="3">Third Class</option>
                </select>

                <label for="passenger-sex">Sex</label>
                <select id="passenger-sex" required>
                    <option value="male">Male</option>
                    <option value="female">Female</option>
                </select>

                <label for="passenger-age">Age</label>
                <input type="number" id="passenger-age" placeholder="Enter age" min="0" max="100" required />

                <label for="passenger-sibsp">Siblings/Spouses Aboard</label>
                <input type="number" id="passenger-sibsp" placeholder="Number of siblings/spouses" min="0" max="10" value="0" required />

                <label for="passenger-parch">Parents/Children Aboard</label>
                <input type="number" id="passenger-parch" placeholder="Number of parents/children" min="0" max="10" value="0" required />

                <label for="passenger-fare">Fare (£)</label>
                <input type="number" id="passenger-fare" placeholder="Ticket fare" min="0" max="500" required />

                <label for="passenger-embarked">Port of Embarkation</label>
                <select id="passenger-embarked" required>
                    <option value="C">Cherbourg</option>
                    <option value="Q">Queenstown</option>
                    <option value="S">Southampton</option>
                </select>

                <div id="result-container" class="hidden">
                    <h3>Survival Prediction</h3>
                    <div id="prediction-result"></div>
                </div>

                <div class="form-actions">
                    <button type="button" onclick="simulateSurvival()">Test Survival</button>
                    <button type="button" onclick="savePassenger()" id="save-btn" disabled>Save Passenger</button>
                    <button type="button" onclick="resetForm()">New Passenger</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Right Column: Passenger Records -->
    <div class="right-column">
        <div class="table-container">
            <h2>Passenger Records</h2>
            <table id="passengers-table">
                <thead>
                    <tr>
                        <th>ID</th>
                        <th>Name</th>
                        <th>Class</th>
                        <th>Sex</th>
                        <th>Age</th>
                        <th>Survived?</th>
                        <th>Actions</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- Table rows will be populated dynamically -->
                </tbody>
            </table>
        </div>
    </div>
</div>

<script type="module">
    import { pythonURI, fetchOptions } from "{{site.baseurl}}/assets/js/api/config.js";

    // Make functions available globally
    window.currentPrediction = null;

    // Function to simulate survival
    window.simulateSurvival = async function simulateSurvival() {
        const name = document.getElementById('passenger-name').value;
        const pclass = document.getElementById('passenger-class').value;
        const sex = document.getElementById('passenger-sex').value;
        const age = document.getElementById('passenger-age').value;
        const sibsp = document.getElementById('passenger-sibsp').value;
        const parch = document.getElementById('passenger-parch').value;
        const fare = document.getElementById('passenger-fare').value;
        const embarked = document.getElementById('passenger-embarked').value;
        
        // Calculate "alone" status based on sibsp and parch
        const alone = (parseInt(sibsp) === 0 && parseInt(parch) === 0);

        if (!name || !pclass || !sex || !age || !fare || !embarked) {
            alert('Please fill out all fields');
            return;
        }

        const passengerData = {
            pclass: parseInt(pclass),
            sex: sex,
            age: parseFloat(age),
            sibsp: parseInt(sibsp),
            parch: parseInt(parch),
            fare: parseFloat(fare),
            embarked: embarked,
            alone: alone
        };

        try {
            const response = await fetch(`${pythonURI}/api/titanic/predict`, {
                ...fetchOptions,
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(passengerData)
            });

            if (response.ok) {
                const result = await response.json();
                window.currentPrediction = result;
                
                const resultContainer = document.getElementById('result-container');
                const predictionResult = document.getElementById('prediction-result');
                const saveBtn = document.getElementById('save-btn');
                const gameMessage = document.getElementById('game-message');
                
                resultContainer.classList.remove('hidden');
                saveBtn.disabled = false;

                // Create dramatic effect
                gameMessage.innerHTML = "<p>The ship has struck an iceberg! Calculating your chances...</p>";
                
                // Calculate survival chance percentage
                const survivalChance = (result.survival_probability * 100).toFixed(1);
                const survivalOutcome = result.survival_probability >= 0.5;
                
                setTimeout(() => {
                    let message = '';
                    let resultHTML = '';
                    let animationClass = '';

                    if (survivalOutcome) {
                        message = "<p>Congratulations! You have survived the Titanic disaster!</p>";
                        resultHTML = `
                            <div class="prediction survived survive-animation">
                                <h4>SURVIVED</h4>
                                <p>Survival Probability: ${survivalChance}%</p>
                            </div>
                        `;
                        animationClass = 'survive-animation';
                    } else {
                        message = "<p>Unfortunately, you did not survive the Titanic disaster.</p>";
                        resultHTML = `
                            <div class="prediction perished perish-animation">
                                <h4>PERISHED</h4>
                                <p>Survival Probability: ${survivalChance}%</p>
                            </div>
                        `;
                        animationClass = 'perish-animation';
                    }
                    
                    gameMessage.innerHTML = message;
                    predictionResult.innerHTML = resultHTML;
                    predictionResult.classList.add(animationClass);
                }, 1500);
                
            } else {
                alert('Error predicting survival');
            }
        } catch (error) {
            console.error('Error predicting survival:', error);
            alert('Error predicting survival');
        }
    };

    // Function to save passenger
    window.savePassenger = async function savePassenger() {
        if (!window.currentPrediction) {
            alert('Please simulate survival first');
            return;
        }

        const name = document.getElementById('passenger-name').value;
        const pclass = document.getElementById('passenger-class').value;
        const sex = document.getElementById('passenger-sex').value;
        const age = document.getElementById('passenger-age').value;
        const sibsp = document.getElementById('passenger-sibsp').value;
        const parch = document.getElementById('passenger-parch').value;
        const fare = document.getElementById('passenger-fare').value;
        const embarked = document.getElementById('passenger-embarked').value;
        const alone = (parseInt(sibsp) === 0 && parseInt(parch) === 0);
        
        // Survival is determined by the prediction
        const survived = window.currentPrediction.survival_probability >= 0.5 ? 1 : 0;

        const passengerData = {
            name: name,
            pclass: parseInt(pclass),
            sex: sex,
            age: parseFloat(age),
            sibsp: parseInt(sibsp),
            parch: parseInt(parch),
            fare: parseFloat(fare),
            embarked: embarked,
            alone: alone,
            survived: survived
        };

        try {
            const response = await fetch(`${pythonURI}/api/titanic/passenger`, {
                ...fetchOptions,
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(passengerData)
            });

            if (response.ok) {
                alert('Passenger saved to historical records');
                fetchPassengers();
            } else {
                alert('Error saving passenger');
            }
        } catch (error) {
            console.error('Error saving passenger:', error);
            alert('Error saving passenger');
        }
    };

    // Function to delete a passenger
    window.deletePassenger = async function deletePassenger(id) {
        try {
            const response = await fetch(`${pythonURI}/api/titanic/passenger/${id}`, {
                ...fetchOptions,
                method: 'DELETE'
            });

            if (response.ok) {
                alert('Passenger deleted successfully');
                fetchPassengers();
            } else {
                alert('Error deleting passenger');
            }
        } catch (error) {
            console.error('Error deleting passenger:', error);
            alert('Error deleting passenger');
        }
    };

    // Function to fetch and display the list of passengers
    window.fetchPassengers = async function fetchPassengers() {
        try {
            const response = await fetch(`${pythonURI}/api/titanic/passengers`, fetchOptions);

            if (response.ok) {
                const data = await response.json();
                const tableBody = document.getElementById('passengers-table').getElementsByTagName('tbody')[0];
                tableBody.innerHTML = '';

                data.forEach(passenger => {
                    const row = tableBody.insertRow();
                    row.insertCell(0).innerText = passenger.id;
                    row.insertCell(1).innerText = passenger.name;
                    row.insertCell(2).innerText = passenger.pclass;
                    row.insertCell(3).innerText = passenger.sex;
                    row.insertCell(4).innerText = passenger.age;
                    
                    const survivedCell = row.insertCell(5);
                    if (passenger.survived === 1) {
                        survivedCell.innerHTML = '<span class="survived-tag">Yes</span>';
                    } else {
                        survivedCell.innerHTML = '<span class="perished-tag">No</span>';
                    }

                    const actionsCell = row.insertCell(6);
                    const deleteButton = document.createElement('button');
                    deleteButton.innerText = 'Delete';
                    deleteButton.className = 'delete-btn';
                    deleteButton.onclick = () => deletePassenger(passenger.id);
                    actionsCell.appendChild(deleteButton);
                });
            } else {
                console.error('Error fetching passengers');
            }
        } catch (error) {
            console.error('Error fetching passengers:', error);
        }
    };

    // Function to reset form
    window.resetForm = function resetForm() {
        document.getElementById('simulation-form').reset();
        document.getElementById('result-container').classList.add('hidden');
        document.getElementById('save-btn').disabled = true;
        document.getElementById('game-message').innerHTML = 
            "<p>Fill in your details to create a Titanic passenger and test your survival chances!</p>";
        window.currentPrediction = null;
    };

    // Initialize on page load
    window.onload = function() {
        fetchPassengers();
    };
</script>

<!-- Popup for instructions -->
<div id="popup" class="popup">
    <div class="popup-content">
        <span class="close">&times;</span>
        <h2>Welcome to the Titanic Survival Simulator!</h2>
        <p>Step back in time to April 14, 1912. Will you survive the tragic sinking of the RMS Titanic?</p>
        <ul>
            <li><strong>Create a Passenger:</strong> Fill in the passenger details on the left.</li>
            <li><strong>Test Survival:</strong> Click "Test Survival" to see if you would have survived based on historical data.</li>
            <li><strong>Save to History:</strong> Save your passenger to the historical records.</li>
            <li><strong>View Records:</strong> See all saved passengers in the table on the right.</li>
            <li><strong>Historical Facts:</strong> Your survival chances are calculated based on real Titanic survival data and machine learning!</li>
        </ul>
        <p><strong>Good luck, and may your journey be safer than the Titanic's!</strong></p>
    </div>
</div>

<style>
    .popup {
        display: none;
        position: fixed;
        z-index: 1;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        overflow: auto;
        background-color: rgba(0, 0, 0, 0.4);
    }

    .popup-content {
        background-color:rgb(0, 0, 0);
        margin: 15% auto;
        padding: 20px;
        border: 1px solid #888;
        width: 80%;
        max-width: 600px;
        box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        animation: fadeIn 0.3s;
    }

    .close {
        color: #aaa;
        float: right;
        font-size: 28px;
        font-weight: bold;
    }

    .close:hover,
    .close:focus {
        color: black;
        text-decoration: none;
        cursor: pointer;
    }

    @keyframes fadeIn {
        from { opacity: 0; }
        to { opacity: 1; }
    }
</style>

<style>
    /* Base styles */
    body {
        font-family: 'Georgia', serif;
        line-height: 1.6;
        color: #333;
        background-color: #f5f5f5;
        background-image: url('https://www.placecage.com/800/600');
    }

    .heading {
        text-align: center;
        margin-bottom: 30px;
        color: #0d3d56;
    }

    .heading h1 {
        font-size: 2.5em;
        margin-bottom: 0;
    }

    /* Layout */
    .main {
        display: flex;
        flex-wrap: wrap;
        gap: 20px;
        max-width: 1200px;
        margin: 0 auto;
    }

    .left-column, .right-column {
        flex: 1;
    }

    /* Existing styles... */

    @keyframes surviveAnimation {
        from { opacity: 0; transform: scale(0.5); }
        to { opacity: 1; transform: scale(1); }
    }

    @keyframes perishAnimation {
        from { opacity: 0; transform: scale(1.5); }
        to { opacity: 1; transform: scale(1); }
    }

    .survive-animation {
        animation: surviveAnimation 1s ease-out;
    }

    .perish-animation {
        animation: perishAnimation 1s ease-out;
    }
</style>