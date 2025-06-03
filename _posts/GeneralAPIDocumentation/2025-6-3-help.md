---
layout: post 
search_exclude: false
show_reading_time: false
permalink: /HelpAPI
title: Help API Documentation
categories: [GeneralAPIDocumentation]
---

# Help/FAQ System API Documentation

## Overview

The Help/FAQ System is a comprehensive customer support platform built with Flask and SQLAlchemy. It provides ticket management, FAQ administration, and user support capabilities with role-based access control.

## Technical Architecture

### Technology Stack
- **Backend**: Flask (Python)
- **Database**: SQLAlchemy ORM
- **Authentication**: JWT tokens
- **API**: RESTful endpoints with Flask-RESTful
- **Authorization**: Role-based access control (User/Admin)

### System Components

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   API Layer     │    │   Database      │
│   (Client)      │◄──►│   (Flask)       │◄──►│   (SQLAlchemy)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                               │
                       ┌───────▼───────┐
                       │  Auth System  │
                       │  (JWT)        │
                       └───────────────┘
```

## Data Models

### Core Entities

#### HelpRequest
```python
class HelpRequest:
    id: int (Primary Key)
    title: str (Required)
    description: str (Required)
    category: str (Enum: technical, account, general, bug_report, feature_request, billing, other)
    priority: str (Enum: low, medium, high, urgent)
    status: str (Enum: open, in_progress, resolved, closed, on_hold)
    user_id: int (Foreign Key to User)
    assigned_to_id: int (Foreign Key to Admin User)
    contact_email: str
    admin_notes: str
    created_date: datetime
    updated_date: datetime
```

#### HelpResponse
```python
class HelpResponse:
    id: int (Primary Key)
    help_request_id: int (Foreign Key)
    admin_id: int (Foreign Key to Admin User)
    response_text: str (Required)
    is_public: bool (Default: True)
    created_date: datetime
```

#### FAQ
```python
class FAQ:
    id: int (Primary Key)
    question: str (Required)
    answer: str (Required)
    category: str (Required)
    tags: str
    display_order: int (Default: 0)
    is_active: bool (Default: True)
    created_by_id: int (Foreign Key to Admin User)
    created_date: datetime
    updated_date: datetime
```

#### HelpHistory
```python
class HelpHistory:
    id: int (Primary Key)
    help_request_id: int (Foreign Key)
    action: str (created, updated, response_added)
    details: str
    user_id: int (Foreign Key)
    created_date: datetime
```

## API Endpoints

### Authentication
All endpoints except FAQ retrieval and search require JWT authentication via the `@token_required()` decorator.

**Header Format:**
```
Authorization: Bearer <jwt_token>
```

### Help Request Management

#### Create Help Request
```http
POST /api/help/requests
```

**Request Body:**
```json
{
    "title": "Unable to access dashboard",
    "description": "I'm getting a 403 error when trying to access my dashboard",
    "category": "technical",
    "priority": "medium",
    "contact_email": "user@example.com"
}
```

**Response (201):**
```json
{
    "message": "Help request created successfully",
    "request_id": 123,
    "data": {
        "id": 123,
        "title": "Unable to access dashboard",
        "description": "I'm getting a 403 error when trying to access my dashboard",
        "category": "technical",
        "priority": "medium",
        "status": "open",
        "user_id": 456,
        "contact_email": "user@example.com",
        "created_date": "2025-06-03T10:30:00Z",
        "updated_date": "2025-06-03T10:30:00Z"
    }
}
```

#### Get Help Requests
```http
GET /api/help/requests?status=open&category=technical&page=1&per_page=10&sort_by=created_date&sort_order=desc
```

**Query Parameters:**
- `status`: Filter by status (open, in_progress, resolved, closed, on_hold, all)
- `category`: Filter by category (technical, account, general, etc.)
- `priority`: Filter by priority (low, medium, high, urgent, all)
- `page`: Page number (default: 1)
- `per_page`: Items per page (max: 100, default: 10)
- `sort_by`: Sort field (created_date, updated_date, priority, status)
- `sort_order`: Sort direction (asc, desc)

**Response (200):**
```json
{
    "requests": [
        {
            "id": 123,
            "title": "Unable to access dashboard",
            "status": "open",
            "priority": "medium",
            "category": "technical",
            "created_date": "2025-06-03T10:30:00Z"
        }
    ],
    "pagination": {
        "page": 1,
        "pages": 5,
        "per_page": 10,
        "total": 47,
        "has_next": true,
        "has_prev": false
    }
}
```

#### Get Help Request Details
```http
GET /api/help/requests/{request_id}
```

**Response (200):**
```json
{
    "id": 123,
    "title": "Unable to access dashboard",
    "description": "I'm getting a 403 error when trying to access my dashboard",
    "category": "technical",
    "priority": "medium",
    "status": "open",
    "user_id": 456,
    "assigned_to_id": null,
    "contact_email": "user@example.com",
    "admin_notes": null,
    "created_date": "2025-06-03T10:30:00Z",
    "updated_date": "2025-06-03T10:30:00Z",
    "history": [
        {
            "id": 1,
            "action": "created",
            "details": "Help request created by John Doe",
            "user_id": 456,
            "created_date": "2025-06-03T10:30:00Z"
        }
    ]
}
```

#### Update Help Request
```http
PUT /api/help/requests/{request_id}
```

**Admin Update Request:**
```json
{
    "status": "in_progress",
    "priority": "high",
    "assigned_to_id": 789,
    "admin_notes": "Escalated to technical team"
}
```

**User Update Request:**
```json
{
    "title": "Updated title",
    "description": "Updated description with more details",
    "contact_email": "newemail@example.com"
}
```

**Response (200):**
```json
{
    "message": "Help request updated successfully",
    "changes": [
        "Status changed from open to in_progress",
        "Priority changed from medium to high",
        "Assigned to Jane Smith"
    ],
    "data": {
        "id": 123,
        "status": "in_progress",
        "priority": "high",
        "assigned_to_id": 789
    }
}
```

### Admin Response Management

#### Add Admin Response
```http
POST /api/help/requests/{request_id}/response
```

**Request Body:**
```json
{
    "response": "Thank you for reporting this issue. I've escalated it to our technical team and they will investigate shortly.",
    "is_public": true,
    "new_status": "in_progress"
}
```

**Response (201):**
```json
{
    "message": "Response added successfully",
    "response_id": 456,
    "data": {
        "id": 456,
        "help_request_id": 123,
        "admin_id": 789,
        "response_text": "Thank you for reporting this issue...",
        "is_public": true,
        "created_date": "2025-06-03T11:00:00Z"
    }
}
```

### FAQ Management

#### Get FAQs (Public)
```http
GET /api/help/faqs?category=technical&search=login
```

**Query Parameters:**
- `category`: Filter by category (all by default)
- `search`: Search in questions, answers, and tags

**Response (200):**
```json
{
    "faqs": {
        "technical": [
            {
                "id": 1,
                "question": "How do I reset my password?",
                "answer": "You can reset your password by clicking the 'Forgot Password' link on the login page.",
                "category": "technical",
                "tags": "password, login, reset",
                "display_order": 1,
                "created_date": "2025-06-01T09:00:00Z"
            }
        ],
        "account": [
            {
                "id": 2,
                "question": "How do I update my profile?",
                "answer": "Navigate to Settings > Profile to update your information.",
                "category": "account",
                "tags": "profile, settings, update",
                "display_order": 1,
                "created_date": "2025-06-01T09:15:00Z"
            }
        ]
    },
    "total_count": 2
}
```

#### Create FAQ (Admin Only)
```http
POST /api/help/faqs
```

**Request Body:**
```json
{
    "question": "How do I contact support?",
    "answer": "You can contact support by creating a help request through the system or emailing support@company.com",
    "category": "general",
    "tags": "support, contact, help",
    "display_order": 5
}
```

**Response (201):**
```json
{
    "message": "FAQ created successfully",
    "faq_id": 3,
    "data": {
        "id": 3,
        "question": "How do I contact support?",
        "answer": "You can contact support by creating a help request...",
        "category": "general",
        "tags": "support, contact, help",
        "display_order": 5,
        "is_active": true,
        "created_by_id": 789,
        "created_date": "2025-06-03T11:30:00Z"
    }
}
```

#### Update FAQ (Admin Only)
```http
PUT /api/help/faqs/{faq_id}
```

**Request Body:**
```json
{
    "question": "Updated question",
    "answer": "Updated answer",
    "category": "general",
    "tags": "updated, tags",
    "display_order": 10,
    "is_active": true
}
```

#### Delete FAQ (Admin Only)
```http
DELETE /api/help/faqs/{faq_id}
```

**Response (200):**
```json
{
    "message": "FAQ deleted successfully"
}
```

### Search

#### Search Help System
```http
GET /api/help/search?q=password
```

**Response (200):**
```json
{
    "faqs": [
        {
            "id": 1,
            "question": "How do I reset my password?",
            "answer": "You can reset your password by clicking...",
            "category": "technical"
        }
    ],
    "help_requests": [
        {
            "id": 123,
            "title": "Password reset not working",
            "description": "I clicked the reset link but...",
            "status": "open"
        }
    ]
}
```

### Statistics (Admin Only)

#### Get System Statistics
```http
GET /api/help/statistics
```

**Response (200):**
```json
{
    "requests": {
        "total": 150,
        "open": 25,
        "in_progress": 10,
        "resolved": 115,
        "by_category": {
            "technical": 75,
            "account": 30,
            "general": 25,
            "bug_report": 15,
            "billing": 5
        },
        "by_priority": {
            "low": 60,
            "medium": 70,
            "high": 15,
            "urgent": 5
        }
    },
    "responses": {
        "total": 200,
        "avg_response_time_days": 1.5
    },
    "faqs": {
        "total": 25
    }
}
```

## Error Handling

### Standard Error Responses

**400 Bad Request:**
```json
{
    "message": "Help request title is required"
}
```

**401 Unauthorized:**
```json
{
    "message": "Valid token is required"
}
```

**403 Forbidden:**
```json
{
    "message": "Admin access required"
}
```

**404 Not Found:**
```json
{
    "message": "Help request not found"
}
```

**500 Internal Server Error:**
```json
{
    "message": "Error creating help request: Database connection failed"
}
```

## Frontend Integration

### Recommended Frontend Architecture

```
┌─────────────────────────────┐
│        Frontend App         │
├─────────────────────────────┤
│  ┌─────────────────────────┐│
│  │    User Dashboard       ││
│  │  - My Help Requests     ││
│  │  - Create New Request   ││
│  │  - View Responses       ││
│  └─────────────────────────┘│
│  ┌─────────────────────────┐│
│  │    Admin Dashboard      ││
│  │  - All Help Requests    ││
│  │  - Response Management  ││
│  │  - FAQ Management       ││
│  │  - Statistics           ││
│  └─────────────────────────┘│
│  ┌─────────────────────────┐│
│  │    Public FAQ Page      ││
│  │  - Browse FAQs          ││
│  │  - Search FAQs          ││
│  └─────────────────────────┘│
└─────────────────────────────┘
```

### Key Frontend Features

1. **User Interface**
   - Help request creation form with category/priority selection
   - Request tracking with status updates
   - Response viewing and notifications
   - FAQ browsing and search

2. **Admin Interface**
   - Request queue management with filtering/sorting
   - Response composition with rich text editor
   - FAQ management with drag-and-drop ordering
   - Analytics dashboard with charts

3. **Public Interface**
   - FAQ browsing with categorization
   - Search functionality with highlighting
   - Contact form integration

## Security Considerations

### Authentication & Authorization
- JWT tokens with expiration
- Role-based access control (User/Admin)
- Request ownership validation
- Admin-only endpoints protection

### Data Validation
- Input sanitization and validation
- SQL injection prevention via ORM
- XSS protection through proper encoding
- File upload restrictions (if implemented)

### Privacy & Data Protection
- Personal data encryption
- Audit logging for sensitive operations
- Data retention policies
- GDPR compliance considerations

## Deployment & Operations

### Database Setup
```sql
-- Example table creation
CREATE TABLE help_requests (
    id SERIAL PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    category VARCHAR(50) NOT NULL,
    priority VARCHAR(20) NOT NULL,
    status VARCHAR(20) DEFAULT 'open',
    user_id INTEGER REFERENCES users(id),
    assigned_to_id INTEGER REFERENCES users(id),
    contact_email VARCHAR(255),
    admin_notes TEXT,
    created_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Environment Configuration
```python
# Required environment variables
JWT_SECRET_KEY = "your-secret-key"
DATABASE_URL = "postgresql://user:pass@localhost/helpdb"
MAIL_SERVER = "smtp.company.com"  # For notifications
MAIL_PORT = 587
MAIL_USERNAME = "support@company.com"
MAIL_PASSWORD = "mail-password"
```

### Monitoring & Logging
- Request/response logging
- Error tracking and alerting
- Performance monitoring
- Usage analytics
- Database query optimization

## Future Enhancements

### Potential Features
1. **Email Notifications**
   - New request alerts for admins
   - Status update notifications for users
   - Scheduled digest emails

2. **File Attachments**
   - Screenshot uploads for bug reports
   - Document attachments for requests
   - File size and type restrictions

3. **Advanced Workflow**
   - Automated routing based on category
   - SLA tracking and alerts
   - Escalation rules

4. **Integration Capabilities**
   - Third-party ticketing systems
   - Chat widget integration
   - Knowledge base synchronization

5. **Analytics & Reporting**
   - Custom dashboard creation
   - Export capabilities
   - Trend analysis
   - Customer satisfaction surveys
