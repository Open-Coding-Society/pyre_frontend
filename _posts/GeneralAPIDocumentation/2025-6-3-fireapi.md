---
layout: post 
search_exclude: false
show_reading_time: false
permalink: /FireAPI
title: Fire API Documentation
categories: [GeneralAPIDocumentation]
---

# Forest Fire API Documentation

## Overview

The Forest Fire API is a RESTful web service built with Flask and Flask-RESTful that provides comprehensive forest fire data management and prediction capabilities. It combines machine learning predictions with full CRUD operations for forest fire records.

## Technical Architecture

### System Design Overview

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Flask API     │    │   Database      │
│   Application   │────│   Server        │────│   (SQLAlchemy)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │
                                │
                       ┌─────────────────┐
                       │   ML Model      │
                       │ (ForestFireModel)│
                       └─────────────────┘
```

### Component Architecture

#### 1. API Layer (Flask-RESTful)
- **Framework**: Flask with Flask-RESTful
- **Blueprint**: `forest_fire_api` with URL prefix `/api/forest-fire`
- **Authentication**: None (add as needed)
- **Content Type**: JSON

#### 2. Model Layer
- **ORM**: SQLAlchemy via Flask-SQLAlchemy
- **Model**: `ForestFire` entity
- **ML Model**: `ForestFireModel` (Singleton pattern)

#### 3. Data Flow Diagram

```
┌─────────────┐
│   Client    │
│ Application │
└──────┬──────┘
       │ HTTP Request (JSON)
       ▼
┌─────────────┐
│ Flask API   │
│ Endpoints   │
└──────┬──────┘
       │
       ├─────────────────┐
       │                 │
       ▼                 ▼
┌─────────────┐   ┌─────────────┐
│  Database   │   │  ML Model   │
│ Operations  │   │ Predictions │
└─────────────┘   └─────────────┘
```

## API Endpoints

### Base URL
```
/api/forest-fire
```

### 1. Predict Forest Fire Area

**Endpoint**: `POST /predict`

**Description**: Predicts the burned area based on weather and environmental conditions using the ML model.

**Required Fields**:
- `X` (float): X-axis spatial coordinate
- `Y` (float): Y-axis spatial coordinate  
- `month` (string): Month of the year
- `day` (string): Day of the week
- `temp` (float): Temperature in Celsius
- `RH` (float): Relative humidity percentage
- `wind` (float): Wind speed in km/h
- `rain` (float): Outside rain in mm/m²

**Request Example**:
```json
{
    "X": 7.0,
    "Y": 5.0,
    "month": "aug",
    "day": "fri",
    "temp": 18.0,
    "RH": 33.0,
    "wind": 0.9,
    "rain": 0.0
}
```

**Response Example**:
```json
{
    "predicted_area": 2.47,
    "confidence": 0.85,
    "risk_level": "moderate"
}
```

**Error Responses**:
- `400`: Missing required field
- `500`: Model prediction error

### 2. Create Forest Fire Record

**Endpoint**: `POST /record`

**Description**: Creates a new forest fire record in the database.

**Required Fields**:
- `X`, `Y`, `month`, `day`, `temp`, `RH`, `wind`, `rain` (same as predict)
- `FFMC` (float): Fine Fuel Moisture Code
- `DMC` (float): Duff Moisture Code
- `DC` (float): Drought Code
- `ISI` (float): Initial Spread Index
- `area` (float): Burned area in hectares

**Request Example**:
```json
{
    "X": 7.0,
    "Y": 5.0,
    "month": "aug",
    "day": "fri",
    "FFMC": 86.2,
    "DMC": 26.2,
    "DC": 94.3,
    "ISI": 5.1,
    "temp": 18.0,
    "RH": 33.0,
    "wind": 0.9,
    "rain": 0.0,
    "area": 0.36
}
```

**Response Example**:
```json
{
    "message": "Forest fire record created successfully",
    "forest_fire": {
        "id": 123,
        "X": 7.0,
        "Y": 5.0,
        "month": "aug",
        "day": "fri",
        "FFMC": 86.2,
        "DMC": 26.2,
        "DC": 94.3,
        "ISI": 5.1,
        "temp": 18.0,
        "RH": 33.0,
        "wind": 0.9,
        "rain": 0.0,
        "area": 0.36
    }
}
```

### 3. Read Forest Fire Records

**Endpoints**: 
- `GET /records` - Get all records
- `GET /record/<int:record_id>` - Get specific record

**Description**: Retrieves forest fire records from the database.

**Response Example (Single Record)**:
```json
{
    "id": 123,
    "X": 7.0,
    "Y": 5.0,
    "month": "aug",
    "day": "fri",
    "FFMC": 86.2,
    "DMC": 26.2,
    "DC": 94.3,
    "ISI": 5.1,
    "temp": 18.0,
    "RH": 33.0,
    "wind": 0.9,
    "rain": 0.0,
    "area": 0.36
}
```

**Response Example (All Records)**:
```json
[
    {
        "id": 123,
        "X": 7.0,
        "Y": 5.0,
        // ... full record
    },
    {
        "id": 124,
        "X": 8.0,
        "Y": 6.0,
        // ... full record
    }
]
```

### 4. Update Forest Fire Record

**Endpoint**: `PUT /record/<int:record_id>`

**Description**: Updates an existing forest fire record.

**Request Example**:
```json
{
    "temp": 20.0,
    "RH": 35.0,
    "area": 0.45
}
```

**Response Example**:
```json
{
    "message": "Forest fire record updated successfully",
    "forest_fire": {
        "id": 123,
        // ... updated record
    }
}
```

### 5. Delete Forest Fire Record

**Endpoint**: `DELETE /record/<int:record_id>`

**Description**: Deletes a forest fire record from the database.

**Response Example**:
```json
{
    "message": "Forest fire record 123 deleted successfully"
}
```

### 6. Calculate Fire Indices

**Endpoint**: `POST /calculate-indices`

**Description**: Calculates fire weather indices (FFMC, DMC, DC, ISI) based on weather data.

**Required Fields**:
- `temp` (float): Temperature in Celsius
- `RH` (float): Relative humidity percentage
- `wind` (float): Wind speed in km/h
- `rain` (float): Outside rain in mm/m²

**Request Example**:
```json
{
    "temp": 18.0,
    "RH": 33.0,
    "wind": 0.9,
    "rain": 0.0
}
```

**Response Example**:
```json
{
    "FFMC": 86.2,
    "DMC": 26.2,
    "DC": 94.3,
    "ISI": 5.1
}
```

### 7. Restore Records

**Endpoint**: `POST /restore`

**Description**: Bulk restore forest fire records from a list.

**Request Example**:
```json
[
    {
        "X": 7.0,
        "Y": 5.0,
        "month": "aug",
        // ... complete record 1
    },
    {
        "X": 8.0,
        "Y": 6.0,
        "month": "sep",
        // ... complete record 2
    }
]
```

**Response Example**:
```json
{
    "message": "Restored 2 forest fire records successfully"
}
```

## ML Model Integration

### ForestFireModel Class
- **Pattern**: Singleton
- **Purpose**: Provides fire prediction and index calculation
- **Methods**:
  - `predict()`: Main prediction method
  - `estimate_FFMC()`: Fine Fuel Moisture Code calculation
  - `estimate_DMC()`: Duff Moisture Code calculation
  - `estimate_DC()`: Drought Code calculation
  - `estimate_ISI()`: Initial Spread Index calculation

### Fire Weather Indices

#### FFMC (Fine Fuel Moisture Code)
- Represents moisture content of litter and fine fuels
- Range: 0-101
- Affects ignition potential

#### DMC (Duff Moisture Code)  
- Represents moisture content of loosely compacted organic layers
- Affects fire intensity and difficulty of control

#### DC (Drought Code)
- Represents moisture content of deep, compact organic layers
- Seasonal drought effects

#### ISI (Initial Spread Index)
- Combines wind effects with FFMC
- Indicates expected rate of fire spread

## Database Schema

### ForestFire Table
```sql
CREATE TABLE forest_fire (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    X FLOAT NOT NULL,
    Y FLOAT NOT NULL, 
    month VARCHAR(10) NOT NULL,
    day VARCHAR(10) NOT NULL,
    FFMC FLOAT NOT NULL,
    DMC FLOAT NOT NULL,
    DC FLOAT NOT NULL,
    ISI FLOAT NOT NULL,
    temp FLOAT NOT NULL,
    RH FLOAT NOT NULL,
    wind FLOAT NOT NULL,
    rain FLOAT NOT NULL,
    area FLOAT NOT NULL
);
```

## Error Handling

### Standard HTTP Status Codes
- `200`: Success
- `201`: Created
- `400`: Bad Request (validation errors)
- `404`: Not Found
- `500`: Internal Server Error

### Error Response Format
```json
{
    "error": "Description of the error"
}
```

## Frontend Integration

### JavaScript Example
```javascript
// Predict forest fire area
async function predictFireArea(weatherData) {
    const response = await fetch('/api/forest-fire/predict', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify(weatherData)
    });
    
    if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    return await response.json();
}

// Get all forest fire records
async function getFireRecords() {
    const response = await fetch('/api/forest-fire/records');
    return await response.json();
}

// Create new forest fire record
async function createFireRecord(fireData) {
    const response = await fetch('/api/forest-fire/record', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify(fireData)
    });
    
    return await response.json();
}
```

## Testing

### Postman Collection Example
```json
{
    "info": {
        "name": "Forest Fire API",
        "schema": "https://schema.getpostman.com/json/collection/v2.1.0/collection.json"
    },
    "item": [
        {
            "name": "Predict Fire Area",
            "request": {
                "method": "POST",
                "header": [
                    {
                        "key": "Content-Type",
                        "value": "application/json"
                    }
                ],
                "body": {
                    "mode": "raw",
                    "raw": "{\n    \"X\": 7.0,\n    \"Y\": 5.0,\n    \"month\": \"aug\",\n    \"day\": \"fri\",\n    \"temp\": 18.0,\n    \"RH\": 33.0,\n    \"wind\": 0.9,\n    \"rain\": 0.0\n}"
                },
                "url": {
                    "raw": "{{base_url}}/api/forest-fire/predict",
                    "host": ["{{base_url}}"],
                    "path": ["api", "forest-fire", "predict"]
                }
            }
        }
    ]
}
```

## Security Considerations

### Recommended Enhancements
1. **Authentication**: Implement API key or JWT token authentication
2. **Rate Limiting**: Add request rate limiting to prevent abuse
3. **Input Validation**: Enhanced validation for all input parameters
4. **CORS**: Configure Cross-Origin Resource Sharing for web applications
5. **HTTPS**: Ensure all communications are encrypted
6. **SQL Injection**: Use parameterized queries (SQLAlchemy handles this)

### Example Security Middleware
```python
from flask_limiter import Limiter
from flask_limiter.util import get_remote_address

limiter = Limiter(
    app,
    key_func=get_remote_address,
    default_limits=["200 per day", "50 per hour"]
)

@forest_fire_api.before_request
def require_api_key():
    api_key = request.headers.get('X-API-Key')
    if not api_key or not validate_api_key(api_key):
        return jsonify({"error": "Invalid API key"}), 401
```

## Deployment

### Environment Variables
```bash
FLASK_ENV=production
DATABASE_URL=postgresql://user:password@localhost/forestfire
API_SECRET_KEY=your-secret-key
ML_MODEL_PATH=/path/to/model.pkl
```

### Docker Configuration
```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .
EXPOSE 5000

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
```
