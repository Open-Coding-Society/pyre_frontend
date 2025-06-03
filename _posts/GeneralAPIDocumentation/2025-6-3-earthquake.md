---
layout: post 
search_exclude: false
show_reading_time: false
permalink: /EarthquakeAPI
title: Earthquake API Documentation
categories: [GeneralAPIDocumentation]
---

# Earthquake Monitoring System API Documentation

## Table of Contents
1. [System Overview](#system-overview)
2. [Technical Architecture](#technical-architecture)
3. [System Design Diagrams](#system-design-diagrams)
4. [API Endpoints](#api-endpoints)
5. [Data Models](#data-models)
6. [Machine Learning Integration](#machine-learning-integration)
7. [Error Handling](#error-handling)
8. [Usage Examples](#usage-examples)

## System Overview

The Earthquake Monitoring System is a Flask-based REST API that provides comprehensive earthquake data management, real-time analysis, and predictive capabilities. The system integrates machine learning models for earthquake magnitude prediction and risk assessment.

### Key Features
- Real-time earthquake data processing
- Magnitude-based categorization
- Predictive modeling for earthquake risk
- Comprehensive CRUD operations
- Advanced search and filtering
- Risk factor calculations
- Data backup and restoration

## Technical Architecture

### Technology Stack
- **Backend Framework**: Flask with Flask-RESTful
- **Database**: SQLAlchemy ORM with relational database
- **Data Processing**: Pandas for CSV data handling
- **Machine Learning**: Custom EarthquakeModel for predictions
- **API Design**: RESTful architecture with JSON responses

### System Components

#### 1. Data Layer
- **Earthquake Model**: SQLAlchemy model for database operations
- **CSV Data Source**: Primary data storage in `earthquakes.csv`
- **Dual Data Access**: Both database and CSV file support

#### 2. Business Logic Layer
- **EarthquakeModel**: Singleton ML model for predictions
- **Risk Assessment**: Geological risk factor calculations
- **Data Validation**: Input validation and error handling

#### 3. API Layer
- **Flask Blueprint**: Modular API organization
- **RESTful Endpoints**: Standard HTTP methods (GET, POST, PUT, DELETE)
- **JSON Serialization**: Structured data exchange

## System Design Diagrams

### High-Level Architecture
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Flask API     │    │   Data Layer    │
│   Application   │◄──►│   (Blueprint)   │◄──►│                 │
│                 │    │                 │    │  ┌──────────┐   │
│  - Dashboard    │    │  ┌───────────┐  │    │  │Database  │   │
│  - Monitoring   │    │  │Controllers│  │    │  │(SQLAlch) │   │
│  - Analytics    │    │  │           │  │    │  └──────────┘   │
│                 │    │  └───────────┘  │    │                 │
└─────────────────┘    │                 │    │  ┌──────────┐   │
                       │  ┌───────────┐  │    │  │   CSV    │   │
┌─────────────────┐    │  │ML Models  │  │    │  │   Data   │   │
│  External APIs  │◄──►│  │           │  │    │  └──────────┘   │
│                 │    │  └───────────┘  │    │                 │
│ - USGS Feed     │    └─────────────────┘    └─────────────────┘
│ - Seismic Data  │
└─────────────────┘
```

### Data Flow Diagram
```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   Client    │    │    API      │    │   Model     │    │    Data     │
│ Application │    │ Controller  │    │   Layer     │    │   Storage   │
└──────┬──────┘    └──────┬──────┘    └──────┬──────┘    └──────┬──────┘
       │                  │                  │                  │
       │ HTTP Request     │                  │                  │
       ├─────────────────►│                  │                  │
       │                  │ Validate Input   │                  │
       │                  ├─────────────────►│                  │
       │                  │                  │ Query/Update     │
       │                  │                  ├─────────────────►│
       │                  │                  │ Data Response    │
       │                  │                  ◄─────────────────┤
       │                  │ Process Data     │                  │
       │                  ◄─────────────────┤                  │
       │ JSON Response    │                  │                  │
       ◄─────────────────┤                  │                  │
```

### ML Model Integration Flow
```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│  Input Data │    │ Earthquake  │    │ Prediction  │
│             │    │   Model     │    │   Output    │
│ ┌─────────┐ │    │             │    │             │
│ │Latitude │ ├───►│ ┌─────────┐ ├───►│ ┌─────────┐ │
│ │Longitude│ │    │ │Feature  │ │    │ │Magnitude│ │
│ │Depth    │ │    │ │Engineer.│ │    │ │Estimate │ │
│ │Previous │ │    │ │         │ │    │ │         │ │
│ │Magnitude│ │    │ └─────────┘ │    │ └─────────┘ │
│ └─────────┘ │    │             │    │             │
└─────────────┘    │ ┌─────────┐ │    │ ┌─────────┐ │
                   │ │ML Model │ │    │ │Risk     │ │
                   │ │Predict  │ ├───►│ │Factors  │ │
                   │ └─────────┘ │    │ └─────────┘ │
                   └─────────────┘    └─────────────┘
```

## API Endpoints

### Base URL
```
/api/earthquake
```

### 1. Get Recent Earthquakes
**Endpoint**: `GET /`

**Description**: Retrieves earthquake data from the last 24 hours with magnitude categorization.

**Response Format**:
```json
{
  "recent_earthquakes": [...],
  "category_counts": {
    "Major (7.0+)": 0,
    "Strong (5.0-6.9)": 2,
    "Moderate (3.0-4.9)": 15,
    "Minor (<3.0)": 8
  },
  "last_update": "2025-06-03 14:30:00"
}
```

### 2. Predict Earthquake Magnitude
**Endpoint**: `POST /predict`

**Description**: Predicts earthquake magnitude based on location and geological data.

**Request Body**:
```json
{
  "latitude": 34.0522,
  "longitude": -118.2437,
  "depth": 10.5
}
```

**Response**:
```json
{
  "predicted_magnitude": 4.2,
  "confidence": 0.85,
  "risk_level": "moderate"
}
```

### 3. Create Earthquake Record
**Endpoint**: `POST /record`

**Description**: Creates a new earthquake record in the system.

**Request Body**:
```json
{
  "time": "2025-06-03T14:30:00Z",
  "latitude": 34.0522,
  "longitude": -118.2437,
  "depth": 10.5,
  "mag": 4.2,
  "magType": "ml",
  "place": "Los Angeles, CA",
  "type": "earthquake"
}
```

### 4. Read Earthquake Records
**Endpoints**: 
- `GET /records` - Get all records
- `GET /record/<int:record_id>` - Get specific record

**Single Record Response**:
```json
{
  "id": 123,
  "time": "2025-06-03T14:30:00",
  "latitude": 34.0522,
  "longitude": -118.2437,
  "depth": 10.5,
  "magnitude": 4.2,
  "magType": "ml",
  "place": "Los Angeles, CA",
  "type": "earthquake",
  "time_of_day": 14,
  "soil_type": "Unknown",
  "plate_boundary_type": "Transform",
  "previous_magnitude": 0.0,
  "distance_to_fault": 0.0
}
```

### 5. Update Earthquake Record
**Endpoint**: `PUT /record/<int:record_id>`

**Description**: Updates an existing earthquake record.

### 6. Delete Earthquake Record
**Endpoint**: `DELETE /record/<int:record_id>`

**Description**: Deletes an earthquake record from the system.

### 7. Calculate Risk Factors
**Endpoint**: `POST /risk-factors`

**Description**: Calculates seismic risk factors for a given location.

**Request Body**:
```json
{
  "latitude": 34.0522,
  "longitude": -118.2437,
  "depth": 10.5,
  "previous_magnitude": 4.0,
  "distance_to_fault": 2.5
}
```

**Response**:
```json
{
  "seismic_intensity": 6.8,
  "ground_acceleration": 0.25,
  "liquefaction_potential": 0.15
}
```

### 8. Search Earthquakes
**Endpoint**: `GET /search`

**Description**: Advanced search with multiple filter criteria.

**Query Parameters**:
- `min_mag`: Minimum magnitude
- `max_mag`: Maximum magnitude
- `start_time`: Start time (ISO format)
- `end_time`: End time (ISO format)
- `min_lat`, `max_lat`: Latitude range
- `min_lon`, `max_lon`: Longitude range

**Example**: `/search?min_mag=4.0&max_mag=6.0&start_time=2025-01-01T00:00:00Z`

### 9. Restore Data
**Endpoint**: `POST /restore`

**Description**: Bulk restore earthquake records from backup data.

**Request Body**:
```json
[
  {
    "time": "2025-06-03T14:30:00Z",
    "latitude": 34.0522,
    "longitude": -118.2437,
    "depth": 10.5,
    "mag": 4.2,
    "magType": "ml",
    "place": "Los Angeles, CA",
    "type": "earthquake"
  }
]
```

## Data Models

### Earthquake Model Schema
```python
class Earthquake:
    id: Integer (Primary Key)
    time: DateTime (Required)
    latitude: Float (Required)
    longitude: Float (Required)
    depth: Float (Required)
    mag: Float (Required)
    magType: String (Optional)
    place: String (Optional)
    type: String (Default: 'earthquake')
```

### Magnitude Categories
- **Major**: 7.0+ magnitude
- **Strong**: 5.0-6.9 magnitude  
- **Moderate**: 3.0-4.9 magnitude
- **Minor**: <3.0 magnitude

## Machine Learning Integration

### EarthquakeModel Features
The system integrates a singleton ML model with the following capabilities:

#### 1. Magnitude Prediction
- **Input Features**: Latitude, longitude, depth
- **Output**: Predicted magnitude with confidence score
- **Method**: `predict(earthquake_data)`

#### 2. Risk Assessment Functions
- **Seismic Intensity**: `estimate_seismic_intensity(magnitude, depth, distance)`
- **Ground Acceleration**: `estimate_ground_acceleration(intensity, distance)`
- **Liquefaction Potential**: `estimate_liquefaction_potential(lat, lon)`

#### 3. Model Architecture
```python
class EarthquakeModel:
    @staticmethod
    def get_instance():
        # Singleton pattern implementation
        
    def predict(self, data):
        # Feature preprocessing
        # Model inference
        # Post-processing
        
    def estimate_seismic_intensity(self, mag, depth, distance):
        # Physics-based calculations
        
    def estimate_ground_acceleration(self, intensity, distance):
        # Ground motion prediction
        
    def estimate_liquefaction_potential(self, lat, lon):
        # Geological risk assessment
```

## Error Handling

### Standard Error Responses
```json
{
  "error": "Error description",
  "status_code": 400/404/500
}
```

### Common Error Codes
- **400**: Bad Request (Missing required fields)
- **404**: Not Found (Record doesn't exist)
- **500**: Internal Server Error (System/database errors)

### Input Validation
All endpoints perform comprehensive input validation:
- Required field checking
- Data type validation
- Range checking for numerical values
- Date format validation

## Usage Examples

### Python Client Example
```python
import requests
import json

# Base URL
base_url = "http://localhost:5000/api/earthquake"

# Get recent earthquakes
response = requests.get(f"{base_url}/")
recent_data = response.json()
print(f"Recent earthquakes: {len(recent_data['recent_earthquakes'])}")

# Predict earthquake magnitude
prediction_data = {
    "latitude": 34.0522,
    "longitude": -118.2437,
    "depth": 10.5
}
response = requests.post(f"{base_url}/predict", json=prediction_data)
prediction = response.json()
print(f"Predicted magnitude: {prediction['predicted_magnitude']}")

# Search for earthquakes
search_params = {
    "min_mag": 4.0,
    "max_mag": 6.0,
    "start_time": "2025-01-01T00:00:00Z"
}
response = requests.get(f"{base_url}/search", params=search_params)
search_results = response.json()
print(f"Found {len(search_results)} earthquakes")
```

### JavaScript/Fetch Example
```javascript
// Get recent earthquakes
const getRecentEarthquakes = async () => {
  try {
    const response = await fetch('/api/earthquake/');
    const data = await response.json();
    console.log('Recent earthquakes:', data.recent_earthquakes.length);
    return data;
  } catch (error) {
    console.error('Error fetching earthquakes:', error);
  }
};

// Create new earthquake record
const createEarthquake = async (earthquakeData) => {
  try {
    const response = await fetch('/api/earthquake/record', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(earthquakeData)
    });
    const result = await response.json();
    return result;
  } catch (error) {
    console.error('Error creating earthquake:', error);
  }
};
```

### cURL Examples
```bash
# Get recent earthquakes
curl -X GET http://localhost:5000/api/earthquake/

# Predict magnitude
curl -X POST http://localhost:5000/api/earthquake/predict \
  -H "Content-Type: application/json" \
  -d '{"latitude": 34.0522, "longitude": -118.2437, "depth": 10.5}'

# Search earthquakes
curl -X GET "http://localhost:5000/api/earthquake/search?min_mag=4.0&max_mag=6.0"

# Create earthquake record
curl -X POST http://localhost:5000/api/earthquake/record \
  -H "Content-Type: application/json" \
  -d '{
    "time": "2025-06-03T14:30:00Z",
    "latitude": 34.0522,
    "longitude": -118.2437,
    "depth": 10.5,
    "mag": 4.2,
    "magType": "ml",
    "place": "Los Angeles, CA"
  }'
```

## Performance Considerations

### Optimization Strategies
1. **Database Indexing**: Index on time, magnitude, and location fields
2. **Caching**: Implement Redis for frequently accessed data
3. **Pagination**: Add pagination for large result sets
4. **Async Processing**: Use Celery for heavy ML computations
5. **Data Compression**: Compress CSV files for faster I/O

### Scalability Recommendations
1. **Load Balancing**: Deploy multiple API instances
2. **Database Clustering**: Use read replicas for queries
3. **CDN Integration**: Cache static earthquake data
4. **Microservices**: Split ML prediction into separate service
5. **Message Queues**: Implement async processing for batch operations

## Security Considerations

### Authentication & Authorization
- Implement JWT tokens for API access
- Role-based access control (RBAC)
- Rate limiting per user/IP
- Input sanitization and validation

### Data Protection
- HTTPS encryption for all endpoints
- Database connection encryption
- Sensitive data masking in logs
- Regular security audits
