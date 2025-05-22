---
layout: post 
search_exclude: false
show_reading_time: false
permalink: /APIDoc
title: Help/FAQ API Documentation
author: Pranav S
categories: [HelpDocumentation]
---

# Help/FAQ API Documentation

## Authentication
All endpoints except FAQ listing and search require JWT token authentication. Include the token in the Authorization header:
```
Authorization: Bearer <your-jwt-token>
```

## Base URL
```
/api/help
```

---

## Help Requests

### Create Help Request
**POST** `/requests`

Create a new help request (authenticated users only).

**Request Body:**
```json
{
  "title": "Issue title",
  "description": "Detailed description", 
  "category": "technical|account|general|bug_report|feature_request|billing|other",
  "priority": "low|medium|high|urgent",
  "contact_email": "user@example.com"
}
```

**Response:** `201 Created`
```json
{
  "message": "Help request created successfully",
  "request_id": 123,
  "data": { ... }
}
```

### List Help Requests
**GET** `/requests`

Get help requests (users see own, admins see all).

**Query Parameters:**
- `status` - Filter by status (open, in_progress, resolved, closed, on_hold)
- `category` - Filter by category
- `priority` - Filter by priority
- `page` - Page number (default: 1)
- `per_page` - Items per page (max: 100, default: 10)
- `sort_by` - Sort field (default: created_date)
- `sort_order` - asc/desc (default: desc)

**Response:** `200 OK`
```json
{
  "requests": [...],
  "pagination": {
    "page": 1,
    "pages": 5,
    "total": 42
  }
}
```

### Get Help Request Details
**GET** `/requests/{id}`

Get specific help request with history.

**Response:** `200 OK`
```json
{
  "id": 123,
  "title": "...",
  "status": "open",
  "history": [...]
}
```

### Update Help Request
**PUT** `/requests/{id}`

Update help request (admins can update status/assignment, users can update content).

**Admin Request Body:**
```json
{
  "status": "in_progress",
  "priority": "high",
  "assigned_to_id": 5,
  "admin_notes": "Working on this"
}
```

**User Request Body:**
```json
{
  "title": "Updated title",
  "description": "Updated description"
}
```

### Add Admin Response
**POST** `/requests/{id}/response`

Add admin response to help request (admin only).

**Request Body:**
```json
{
  "response": "Response text",
  "new_status": "resolved",
  "is_public": true
}
```

---

## FAQs

### List FAQs
**GET** `/faqs` (Public)

Get all active FAQs grouped by category.

**Query Parameters:**
- `category` - Filter by category
- `search` - Search in question/answer/tags

**Response:** `200 OK`
```json
{
  "faqs": {
    "technical": [...],
    "account": [...]
  },
  "total_count": 25
}
```

### Create FAQ
**POST** `/faqs` (Admin only)

**Request Body:**
```json
{
  "question": "How do I...?",
  "answer": "You can...",
  "category": "account",
  "tags": "password, login",
  "display_order": 1
}
```

### Update FAQ
**PUT** `/faqs/{id}` (Admin only)

**Request Body:**
```json
{
  "question": "Updated question?",
  "answer": "Updated answer",
  "is_active": true
}
```

### Delete FAQ
**DELETE** `/faqs/{id}` (Admin only)

---

## Utilities

### Search
**GET** `/search`

Search FAQs and help requests.

**Query Parameters:**
- `q` - Search query (required)

**Response:** `200 OK`
```json
{
  "faqs": [...],
  "help_requests": [...]
}
```

### Statistics
**GET** `/statistics` (Admin only)

Get help system statistics.

**Response:** `200 OK`
```json
{
  "requests": {
    "total": 150,
    "open": 12,
    "by_category": {...}
  },
  "responses": {
    "avg_response_time_days": 1.5
  }
}
```

---

## Error Responses

**400 Bad Request**
```json
{
  "message": "Validation error description"
}
```

**401 Unauthorized**
```json
{
  "message": "Authentication required"
}
```

**403 Forbidden**
```json
{
  "message": "Admin access required"
}
```

**404 Not Found**
```json
{
  "message": "Resource not found"
}
```

**500 Internal Server Error**
```json
{
  "message": "Error description"
}
```

---
