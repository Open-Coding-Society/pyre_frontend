---
layout: post 
search_exclude: false
show_reading_time: false
permalink: /RiskAPI
title: Risk Predictor API Documentation
categories: [GeneralAPIDocumentation]
---

# Risk Predictor API Documentation

## Overview

The Risk Predictor API is a Flask-based REST service that provides wildfire and earthquake risk assessments for US ZIP codes. The service integrates with external APIs to gather geological and geographical data, applying risk calculation algorithms to provide actionable risk intelligence.

## Technical Architecture

### System Components

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Flask API     │    │  External APIs  │
│   (Client)      │◄──►│   Server        │◄──►│                 │
│                 │    │                 │    │ • USGS          │
│ • Web App       │    │ • Risk Engine   │    │ • Zippopotam    │
│ • Mobile App    │    │ • Data Layer    │    │ • NASA FIRMS   │
│ • CLI Tool      │    │ • API Endpoints │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Technical Stack

- **Backend Framework**: Flask (Python)
- **CORS Support**: Flask-CORS
- **HTTP Client**: Requests library
- **Data Processing**: NumPy-style calculations
- **External APIs**:
  - USGS Earthquake API
  - Zippopotam US ZIP Code API
  - NASA FIRMS (Fire Information for Resource Management System)

## API Endpoints

### Base URL
```
http://localhost:8505/api
```

### 1. Wildfire Risk Assessment

**Endpoint**: `GET /api/risk/wildfire`

**Description**: Calculates wildfire risk for a given US ZIP code based on recent fire activity, vegetation conditions, and weather patterns.

**Parameters**:
- `zip` (required): 5-digit US ZIP code

**Request Example**:
```bash
curl "http://localhost:8505/api/risk/wildfire?zip=92127"
```

**Response Schema**:
```json
{
  "zip_code": "string",
  "coordinates": {
    "latitude": "number",
    "longitude": "number"
  },
  "risk_level": "string", // "Low", "Medium", "High"
  "risk_score": "number", // 0-100
  "recent_fires": "number",
  "vegetation_risk": "string", // "Low", "Medium", "High"
  "weather_risk": "string", // "Low", "Medium", "High"
  "last_updated": "string" // ISO timestamp
}
```

**Sample Response**:
```json
{
  "zip_code": "92127",
  "coordinates": {
    "latitude": 33.0231,
    "longitude": -117.0962
  },
  "risk_level": "High",
  "risk_score": 75,
  "recent_fires": 18,
  "vegetation_risk": "High",
  "weather_risk": "High",
  "last_updated": "2025-06-03T10:30:00.123456"
}
```

### 2. Earthquake Risk Assessment

**Endpoint**: `GET /api/risk/earthquake`

**Description**: Calculates earthquake risk for a given US ZIP code based on recent seismic activity, proximity to fault lines, and historical data.

**Parameters**:
- `zip` (required): 5-digit US ZIP code

**Request Example**:
```bash
curl "http://localhost:8505/api/risk/earthquake?zip=92127"
```

**Response Schema**:
```json
{
  "zip_code": "string",
  "coordinates": {
    "latitude": "number",
    "longitude": "number"
  },
  "risk_level": "string", // "Low", "Medium", "High"
  "risk_score": "number", // 0-100
  "recent_earthquakes": "number",
  "max_magnitude": "number",
  "fault_distance": "string", // Distance in km
  "last_updated": "string" // ISO timestamp
}
```

**Sample Response**:
```json
{
  "zip_code": "92127",
  "coordinates": {
    "latitude": 33.0231,
    "longitude": -117.0962
  },
  "risk_level": "Medium",
  "risk_score": 65,
  "recent_earthquakes": 12,
  "max_magnitude": 4.2,
  "fault_distance": "45.3 km",
  "last_updated": "2025-06-03T10:30:00.123456"
}
```

### 3. Health Check

**Endpoint**: `GET /api/health`

**Description**: Service health and status endpoint for monitoring.

**Request Example**:
```bash
curl "http://localhost:8505/api/health"
```

**Response**:
```json
{
  "status": "healthy",
  "timestamp": "2025-06-03T10:30:00.123456",
  "version": "1.0.0"
}
```

## Error Responses

### Standard Error Format
```json
{
  "error": "string" // Error description
}
```

### HTTP Status Codes
- `200`: Success
- `400`: Bad Request (invalid parameters)
- `404`: Not Found (invalid ZIP code or endpoint)
- `500`: Internal Server Error

### Error Examples

**Invalid ZIP Code**:
```json
{
  "error": "Invalid ZIP code format"
}
```

**Missing Parameter**:
```json
{
  "error": "ZIP code parameter is required"
}
```

**ZIP Code Not Found**:
```json
{
  "error": "Invalid ZIP code"
}
```

## Risk Calculation Models

### Wildfire Risk Algorithm

The wildfire risk model combines multiple factors to generate a comprehensive risk score (0-100):

**Formula Components**:
1. **Recent Fire Activity** (0-40 points)
   - Based on fire incidents in surrounding area
   - Seasonal adjustments applied
   - Data source: NASA FIRMS API

2. **Vegetation Risk** (0-30 points)
   - Geographic vegetation analysis
   - Western US: High risk
   - Central US: Medium risk  
   - Eastern US: Low risk

3. **Weather Risk** (0-30 points)
   - Seasonal fire weather patterns
   - Monthly risk adjustments
   - Fire season (June-September) increases risk

**Risk Level Classification**:
- **Low**: 0-29 points
- **Medium**: 30-69 points
- **High**: 70-100 points

### Earthquake Risk Algorithm

The earthquake risk model evaluates seismic hazard based on:

**Formula Components**:
1. **Recent Seismic Activity** (0-30 points)
   - Earthquake count in past 365 days
   - 1-degree radius search area
   - Minimum magnitude: 1.0

2. **Maximum Magnitude** (0-40 points)
   - ≥6.0 magnitude: 40 points
   - ≥4.0 magnitude: 25 points
   - ≥2.0 magnitude: 10 points

3. **Fault Proximity** (0-30 points)
   - Distance to nearest major fault line
   - <50km: 30 points
   - <100km: 20 points
   - <200km: 10 points
   - ≥200km: 5 points

**Major Fault Lines Monitored**:
- San Andreas Fault (Northern & Southern CA)
- New Madrid Seismic Zone (Missouri)
- Seattle Fault (Washington)
- Wasatch Fault (Utah)

## Data Flow Diagram

```
┌─────────────┐
│   Client    │
│   Request   │
└──────┬──────┘
       │ HTTP GET /api/risk/{type}?zip=XXXXX
       ▼
┌─────────────┐
│   Flask     │
│   Router    │
└──────┬──────┘
       │ Route to handler
       ▼
┌─────────────┐     ┌─────────────┐
│ Validation  │────►│ RiskPredictor│
│ Layer       │     │ Class        │
└─────────────┘     └──────┬──────┘
                           │ get_coordinates_from_zip()
                           ▼
                    ┌─────────────┐
                    │Zippopotam   │
                    │API Call     │
                    └──────┬──────┘
                           │ lat, lon
                           ▼
       ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
       │   USGS      │ │   NASA      │ │Risk Calc    │
       │Earthquake   │ │   FIRMS     │ │ Engine      │
       │   API       │ │     API     │ │             │
       └──────┬──────┘ └──────┬──────┘ └──────┬──────┘
              │               │               │
              └───────────────┼───────────────┘
                              ▼
                       ┌─────────────┐
                       │  Risk Score │
                       │ Calculation │
                       └──────┬──────┘
                              │ JSON Response
                              ▼
                       ┌─────────────┐
                       │   Client    │
                       │  Response   │
                       └─────────────┘
```

## Frontend Integration

### JavaScript Example

```javascript
class RiskPredictorClient {
    constructor(baseUrl = 'http://localhost:8505/api') {
        this.baseUrl = baseUrl;
    }
    
    async getWildfireRisk(zipCode) {
        try {
            const response = await fetch(`${this.baseUrl}/risk/wildfire?zip=${zipCode}`);
            if (!response.ok) {
                throw new Error(`HTTP ${response.status}: ${response.statusText}`);
            }
            return await response.json();
        } catch (error) {
            console.error('Wildfire risk fetch error:', error);
            throw error;
        }
    }
    
    async getEarthquakeRisk(zipCode) {
        try {
            const response = await fetch(`${this.baseUrl}/risk/earthquake?zip=${zipCode}`);
            if (!response.ok) {
                throw new Error(`HTTP ${response.status}: ${response.statusText}`);
            }
            return await response.json();
        } catch (error) {
            console.error('Earthquake risk fetch error:', error);
            throw error;
        }
    }
    
    async checkHealth() {
        try {
            const response = await fetch(`${this.baseUrl}/health`);
            return await response.json();
        } catch (error) {
            console.error('Health check error:', error);
            throw error;
        }
    }
}

// Usage Example
const riskClient = new RiskPredictorClient();

async function displayRisk(zipCode) {
    try {
        const [wildfireRisk, earthquakeRisk] = await Promise.all([
            riskClient.getWildfireRisk(zipCode),
            riskClient.getEarthquakeRisk(zipCode)
        ]);
        
        console.log('Wildfire Risk:', wildfireRisk);
        console.log('Earthquake Risk:', earthquakeRisk);
        
        // Update UI with risk data
        updateRiskDisplay(wildfireRisk, earthquakeRisk);
    } catch (error) {
        console.error('Error fetching risk data:', error);
        showErrorMessage(error.message);
    }
}
```

### React Component Example

```jsx
import React, { useState, useEffect } from 'react';

const RiskDashboard = () => {
    const [zipCode, setZipCode] = useState('');
    const [wildfireRisk, setWildfireRisk] = useState(null);
    const [earthquakeRisk, setEarthquakeRisk] = useState(null);
    const [loading, setLoading] = useState(false);
    const [error, setError] = useState(null);
    
    const fetchRiskData = async () => {
        if (!zipCode || zipCode.length !== 5) return;
        
        setLoading(true);
        setError(null);
        
        try {
            const [wildfireResponse, earthquakeResponse] = await Promise.all([
                fetch(`/api/risk/wildfire?zip=${zipCode}`),
                fetch(`/api/risk/earthquake?zip=${zipCode}`)
            ]);
            
            if (!wildfireResponse.ok || !earthquakeResponse.ok) {
                throw new Error('Failed to fetch risk data');
            }
            
            const wildfireData = await wildfireResponse.json();
            const earthquakeData = await earthquakeResponse.json();
            
            setWildfireRisk(wildfireData);
            setEarthquakeRisk(earthquakeData);
        } catch (err) {
            setError(err.message);
        } finally {
            setLoading(false);
        }
    };
    
    return (
        <div className="risk-dashboard">
            <div className="input-section">
                <input
                    type="text"
                    placeholder="Enter ZIP Code"
                    value={zipCode}
                    onChange={(e) => setZipCode(e.target.value)}
                    maxLength={5}
                />
                <button onClick={fetchRiskData} disabled={loading}>
                    {loading ? 'Loading...' : 'Get Risk Assessment'}
                </button>
            </div>
            
            {error && (
                <div className="error-message">
                    Error: {error}
                </div>
            )}
            
            {wildfireRisk && (
                <div className="risk-card wildfire">
                    <h3>Wildfire Risk</h3>
                    <div className={`risk-level ${wildfireRisk.risk_level.toLowerCase()}`}>
                        {wildfireRisk.risk_level}
                    </div>
                    <div className="risk-score">
                        Score: {wildfireRisk.risk_score}/100
                    </div>
                    <div className="risk-details">
                        <p>Recent Fires: {wildfireRisk.recent_fires}</p>
                        <p>Vegetation Risk: {wildfireRisk.vegetation_risk}</p>
                        <p>Weather Risk: {wildfireRisk.weather_risk}</p>
                    </div>
                </div>
            )}
            
            {earthquakeRisk && (
                <div className="risk-card earthquake">
                    <h3>Earthquake Risk</h3>
                    <div className={`risk-level ${earthquakeRisk.risk_level.toLowerCase()}`}>
                        {earthquakeRisk.risk_level}
                    </div>
                    <div className="risk-score">
                        Score: {earthquakeRisk.risk_score}/100
                    </div>
                    <div className="risk-details">
                        <p>Recent Earthquakes: {earthquakeRisk.recent_earthquakes}</p>
                        <p>Max Magnitude: {earthquakeRisk.max_magnitude}</p>
                        <p>Fault Distance: {earthquakeRisk.fault_distance}</p>
                    </div>
                </div>
            )}
        </div>
    );
};

export default RiskDashboard;
```

## Configuration

### Environment Variables

```bash
# ZIP Code API Key (Zippopotam is free, but rate limited)
ZIPCODE_API_KEY=your_zipcode_api_key

# NASA Earthdata Token for FIRMS API
NASA_EARTHDATA_TOKEN=your_nasa_token
```

### API Rate Limits

- **Zippopotam API**: No official limits, but recommend <1000 requests/hour
- **USGS Earthquake API**: No rate limits specified
- **NASA FIRMS**: Requires authentication, 1000 requests/day limit

## Deployment

### Local Development

```bash
# Install dependencies
pip install flask flask-cors requests

# Set environment variables
export ZIPCODE_API_KEY=your_key
export NASA_EARTHDATA_TOKEN=your_token

# Run the server
python app.py
```

### Production Deployment

```bash
# Use production WSGI server
pip install gunicorn

# Run with Gunicorn
gunicorn -w 4 -b 0.0.0.0:8505 app:app
```

### Docker Deployment

```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 8505
CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:8505", "app:app"]
```

## Future Enhancements

### Planned Features
1. **Real-time Data Integration**: Direct NASA FIRMS API integration
2. **Historical Trend Analysis**: Multi-year risk trend calculations
3. **Weather API Integration**: Real-time weather conditions
4. **Caching Layer**: Redis caching for improved performance
5. **Authentication**: API key-based access control
6. **WebSocket Support**: Real-time risk updates
7. **ML Models**: Machine learning-based risk prediction

### API Versioning
Future versions will include API versioning:
```
/api/v1/risk/wildfire
/api/v2/risk/wildfire
```

## Support and Troubleshooting

### Common Issues
1. **Invalid ZIP Code**: Ensure 5-digit US ZIP codes only
2. **API Timeouts**: External API dependencies may cause delays
3. **CORS Issues**: CORS is enabled for all origins in development

### Monitoring
- Health check endpoint: `/api/health`
- Application logs include error details
- External API response codes are logged

### Contact
For technical support or feature requests, please contact the development team.