---
layout: tailwind 
title: Statistics Documentation
search_exclude: true
permalink: /stats_doc/
---

# Wildfire Statistics API Documentation

This documentation explains how to use the backend API for retrieving live weather data, how to integrate it with the frontend, and how to test it using Postman.

---

![Stats Flowchart Screenshot]({{site.baseurl}}images/Screenshot 2025-06-03 at 12.58.35 PM.png)

## 1. API Endpoint Overview

The backend exposes a RESTful endpoint to fetch current weather data for San Diego, using the OpenWeatherMap API.

**Endpoint:**  
`GET /api/current_api`

**Returns:**  
A JSON object with temperature, humidity, wind speed, and weather description.

---

## 2. Example Backend Code

Below is a snippet from the backend API implementation (`current_api.py`):

```python
from flask import Blueprint, request, jsonify
from flask_restful import Api, Resource
import requests

current_api = Blueprint('current_api', __name__, url_prefix='/api')
api = Api(current_api)
api_key = "YOUR_API_KEY"
q = "San Diego"

class CurrentApi(Resource):
    def get(self):
        try:
            weather_url = "https://api.openweathermap.org/data/2.5/weather"
            params = {
                'q': q,
                'appid': api_key,
            }
            response = requests.get(weather_url, params=params)
            if response.status_code == 200:
                weather_data = response.json()
                fire_relevant_data = {
                    "weather": {
                        "temperature": weather_data['main']['temp'],
                        "humidity": weather_data['main']['humidity'],
                        "wind_speed": weather_data['wind']['speed'],
                        "description": weather_data['weather'][0]['description']
                    }
                }
                return fire_relevant_data
            else:
                return {"error": "Weather API error", "message": response.text}, response.status_code
        except Exception as e:
            return {"error": "Server error", "message": str(e)}, 500

api.add_resource(CurrentApi, '/current_api')
```

---

## 3. How to Recreate the API

1. **Install dependencies:**
    ```sh
    pip install flask flask-restful python-dotenv requests
    ```

2. **Set up your Flask app and register the blueprint:**
    ```python
    from flask import Flask
    from api.current_api import current_api

    app = Flask(__name__)
    app.register_blueprint(current_api)
    ```

3. **Set your OpenWeatherMap API key:**
    - Replace `"YOUR_API_KEY"` in the code with your actual API key.
    - Or, use environment variables and `python-dotenv` for better security.

4. **Run your Flask app:**
    ```sh
    flask run
    ```

---

## 4. Frontend Integration

The frontend fetches weather data from this API and displays it in the dashboard.  
See the relevant code in [`stats.md`](./stats.md):

```js
// Fetch San Diego weather and update the card
async function getSanDiegoWeather() {
  try {
    const response = await fetch(`${pythonURI}/api/current_api`);
    if (!response.ok) throw new Error('Weather API response was not ok');
    const data = await response.json();
    if (data.weather) {
      // Convert Kelvin to Fahrenheit for display
      const tempF = ((data.weather.temperature - 273.15) * 9/5 + 32).toFixed(1);
      document.getElementById('current-temperature').textContent = `${tempF}°`;
      document.getElementById('weather-conditions').textContent = data.weather.description;
      document.getElementById('current-feels-like').textContent = `${tempF}°F`;
      document.getElementById('current-humidity').textContent = `${data.weather.humidity}%`;
      document.getElementById('current-wind-speed').textContent = `${data.weather.wind_speed} m/s`;
      document.getElementById('weather-location').textContent = "San Diego";
    }
  } catch (error) {
    console.error('Error fetching San Diego weather:', error);
    document.getElementById('weather-conditions').textContent = "Unavailable";
  }
}
```

- The frontend calls `/api/current_api` and updates the weather card with the returned data.

---

## 5. Testing with Postman

1. **Open Postman** and create a new GET request.
2. **Set the URL** to:  
   `http://localhost:5000/api/current_api`
3. **Send the request.**
4. **Expected response:**
    ```json
    {
      "weather": {
        "temperature": 293.15,
        "humidity": 60,
        "wind_speed": 4.1,
        "description": "clear sky"
      }
    }
    ```
5. **Troubleshooting:**
    - If you get an error, check your API key and network connection.
    - Make sure your Flask server is running.

---

## 6. Notes

- The endpoint currently only supports San Diego. To support other cities, modify the backend to accept a `q` parameter from the request.
- For production, secure your API key and handle errors gracefully.

---

**For more details, see the code in `stats.md` and your backend API file.**

