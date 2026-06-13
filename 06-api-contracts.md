
# Amazon Loop - API Contracts Specification

## 1. Overview

This document defines all API contracts for Amazon Loop.

Architecture:

* REST API
* JSON Request/Response
* JWT Authentication
* Role-Based Authorization
* Versioned APIs

Base URL:

```text
/api/v1
```

Content Type:

```http
Content-Type: application/json
```

Authentication:

```http
Authorization: Bearer <token>
```

---

# 2. Standard Response Format

## Success Response

```json
{
  "success": true,
  "data": {}
}
```

---

## Error Response

```json
{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "Resource not found"
  }
}
```

---

# 3. Authentication APIs

## Register User

```http
POST /api/v1/auth/register
```

Request:

```json
{
  "fullName": "John Doe",
  "email": "john@example.com",
  "password": "StrongPassword123"
}
```

Response:

```json
{
  "success": true,
  "data": {
    "userId": "uuid"
  }
}
```

---

## Login

```http
POST /api/v1/auth/login
```

Request:

```json
{
  "email": "john@example.com",
  "password": "password"
}
```

Response:

```json
{
  "success": true,
  "data": {
    "token": "jwt_token",
    "user": {}
  }
}
```

---

## Logout

```http
POST /api/v1/auth/logout
```

Authentication Required:

Yes

---

## Current User

```http
GET /api/v1/auth/me
```

Returns:

* User
* Roles
* Active Persona

---

# 4. User APIs

## Get User Profile

```http
GET /api/v1/users/:id
```

---

## Update User Profile

```http
PATCH /api/v1/users/:id
```

Request:

```json
{
  "fullName": "Updated Name"
}
```

---

## Switch Persona

```http
POST /api/v1/users/persona
```

Request:

```json
{
  "role": "OWNER"
}
```

Response:

```json
{
  "success": true
}
```

---

# 5. Asset APIs

## Create Asset

```http
POST /api/v1/assets
```

Permission:

OWNER

Request:

```json
{
  "productName": "Laptop",
  "category": "Electronics",
  "description": "MacBook Air",
  "originalPrice": 85000,
  "condition": "GOOD"
}
```

Response:

```json
{
  "success": true,
  "data": {
    "assetId": "uuid"
  }
}
```

---

## Get Assets

```http
GET /api/v1/assets
```

Filters:

```http
?page=1
&limit=20
&category=Electronics
&status=OWNED
&condition=GOOD
```

---

## Get Asset Details

```http
GET /api/v1/assets/:id
```

Returns:

* Asset Details
* Images
* Event Timeline
* Rental History

---

## Update Asset

```http
PATCH /api/v1/assets/:id
```

Permission:

Owner or Admin

---

## Delete Asset

```http
DELETE /api/v1/assets/:id
```

Permission:

Owner or Admin

---

# 6. Asset Image APIs

## Upload Asset Image

```http
POST /api/v1/assets/:id/images
```

Multipart Upload

Response:

```json
{
  "success": true,
  "data": {
    "imageUrl": "..."
  }
}
```

---

## Delete Asset Image

```http
DELETE /api/v1/assets/:id/images/:imageId
```

---

# 7. Fulfillment Hub APIs

## Get Hubs

```http
GET /api/v1/hubs
```

Filters:

```http
?hubType=KIRANA
?hubType=DARK_STORE
?hubType=CAMPUS_CENTER
```

---

## Get Hub

```http
GET /api/v1/hubs/:id
```

---

## Create Hub

```http
POST /api/v1/hubs
```

Permission:

ADMIN

Request:

```json
{
  "name": "Downtown Hub",
  "hubType": "KIRANA",
  "address": "Address"
}
```

---

## Update Hub

```http
PATCH /api/v1/hubs/:id
```

Permission:

ADMIN

---

## Delete Hub

```http
DELETE /api/v1/hubs/:id
```

Permission:

ADMIN

---

## Nearby Hubs

```http
GET /api/v1/hubs/nearby
```

Query:

```http
?lat=23.8315
&lng=91.2868
&radius=10000
```

Returns:

* Distance
* Capacity
* Hub Type

---

# 8. Asset Lifecycle APIs

## Stage Asset

```http
POST /api/v1/assets/:id/stage
```

Request:

```json
{
  "hubId": "uuid"
}
```

Transition:

```text
OWNED
↓
STAGED_AT_HUB
```

---

## Donate Asset

```http
POST /api/v1/assets/:id/donate
```

Transition:

```text
OWNED/STAGED_AT_HUB
↓
DONATED
```

---

## Scrap Asset

```http
POST /api/v1/assets/:id/scrap
```

Transition:

```text
OWNED/STAGED_AT_HUB
↓
SCRAPPED_FOR_PARTS
```

---

# 9. Rental APIs

## Request Rental

```http
POST /api/v1/rentals/request
```

Permission:

STUDENT

Request:

```json
{
  "assetId": "uuid"
}
```

---

## Approve Rental

```http
POST /api/v1/rentals/:id/approve
```

Permission:

OWNER

---

## Start Rental

```http
POST /api/v1/rentals/:id/start
```

Transition:

```text
STAGED_AT_HUB
↓
CAMPUS_RENTAL
```

---

## Return Rental

```http
POST /api/v1/rentals/:id/return
```

Transition:

```text
CAMPUS_RENTAL
↓
STAGED_AT_HUB
```

---

## Get Rentals

```http
GET /api/v1/rentals
```

Filters:

```http
?status=ACTIVE
```

---

# 10. Donation APIs

## Request Donation

```http
POST /api/v1/donations/request
```

Permission:

NGO

---

## Accept Donation

```http
POST /api/v1/donations/:id/accept
```

Permission:

NGO

---

## Get Donations

```http
GET /api/v1/donations
```

---

# 11. Recycling APIs

## Assign Scrap

```http
POST /api/v1/recycling/assign
```

Permission:

ADMIN

---

## Create Component Bundle

```http
POST /api/v1/recycling/bundles
```

Permission:

RECYCLER

---

## Recovery Report

```http
POST /api/v1/recycling/reports
```

Permission:

RECYCLER

---

# 12. Event APIs

## Asset Timeline

```http
GET /api/v1/assets/:id/events
```

Returns:

```json
[
  {
    "eventType": "AssetCreated",
    "timestamp": "..."
  }
]
```

---

# 13. Notification APIs

## Get Notifications

```http
GET /api/v1/notifications
```

---

## Mark As Read

```http
PATCH /api/v1/notifications/:id/read
```

---

## Mark All Read

```http
PATCH /api/v1/notifications/read-all
```

---

# 14. Analytics APIs

## Dashboard Analytics

```http
GET /api/v1/analytics/dashboard
```

Returns:

* Asset Count
* Revenue
* Donations
* Rentals
* Hub Utilization

---

## Revenue Analytics

```http
GET /api/v1/analytics/revenue
```

---

## Hub Analytics

```http
GET /api/v1/analytics/hubs
```

---

## Asset Analytics

```http
GET /api/v1/analytics/assets
```

---

# 15. Admin APIs

## Get Users

```http
GET /api/v1/admin/users
```

Permission:

ADMIN

---

## Assign Role

```http
POST /api/v1/admin/users/:id/roles
```

---

## Remove Role

```http
DELETE /api/v1/admin/users/:id/roles/:role
```

---

## Audit Logs

```http
GET /api/v1/admin/audit-logs
```

Permission:

ADMIN

---

# 16. Pagination Standard

All list endpoints support:

```http
?page=1
&limit=20
```

Response:

```json
{
  "success": true,
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 150,
    "totalPages": 8
  }
}
```

---

# 17. Error Codes

Standard Error Codes:

```text
UNAUTHORIZED
FORBIDDEN
VALIDATION_ERROR
RESOURCE_NOT_FOUND
ASSET_NOT_FOUND
HUB_NOT_FOUND
RENTAL_NOT_FOUND
DONATION_NOT_FOUND
INVALID_STATE_TRANSITION
FILE_UPLOAD_FAILED
INTERNAL_SERVER_ERROR
```

---

# 18. Audit Requirements

The following actions must generate audit logs:

* Asset Creation
* Asset Updates
* Asset Status Changes
* Role Changes
* Hub Updates
* Rental Approvals
* Donation Acceptance
* Scrap Assignments
* Admin Actions

Every audit log must contain:

* Actor ID
* Action
* Resource Type
* Resource ID
* Timestamp
* Metadata

---

# 19. Future APIs

Reserved for future releases:

* Payments API
* Subscription API
* Logistics API
* Carbon Impact API
* AI Pricing API
* Recommendation API
* Mobile API
