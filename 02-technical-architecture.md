# Amazon Loop - Technical Architecture

## 1. Architecture Overview

Amazon Loop follows a layered architecture designed for scalability, maintainability, testability, and future expansion.

### Architecture Style

* Modular Monolith (Initial Release)
* Domain-Driven Design (DDD)
* Event-Driven Asset Lifecycle
* REST API Architecture
* Repository Pattern
* Service Layer Pattern

### System Flow

Client UI
→ API Routes
→ Service Layer
→ Repository Layer
→ PostgreSQL + PostGIS

Background Workers
→ Event Processing
→ Notification Delivery
→ Analytics Updates

---

# 2. Technology Stack

## Frontend

### Framework

* Next.js 15 (App Router)

### Language

* TypeScript

### Styling

* Tailwind CSS

### Component Library

* shadcn/ui

### Animations

* Framer Motion

### Forms

* React Hook Form

### Validation

* Zod

### Data Fetching

* TanStack Query

### Global State

* Zustand

### Maps

* Mapbox GL

### Charts

* Recharts

### Icons

* Lucide React

---

## Backend

### Framework

* Next.js Route Handlers

### Language

* TypeScript

### Validation

* Zod

### Authentication

* Better Auth

### Authorization

* Role-Based Access Control (RBAC)

### File Storage

* UploadThing
* AWS S3

### Background Jobs

* Trigger.dev

### Email Service

* Resend

---

## Database

### Database Engine

* PostgreSQL 16+

### Extensions

* PostGIS
* pgcrypto

### Connection Library

* pg

### Connection Pooling

* Shared Singleton Pool

---

## Monitoring & Observability

### Error Tracking

* Sentry

### Product Analytics

* PostHog

### Logging

* Structured JSON Logs

---

# 3. Project Structure

```text
src/
├── app/
├── components/
├── features/
├── lib/
├── hooks/
├── store/
├── types/
└── utils/
```

---

# 4. Detailed Folder Structure

```text
src/

app/
├── dashboard/
├── assets/
├── hubs/
├── rentals/
├── analytics/
├── admin/
└── api/

components/
├── ui/
├── layout/
├── charts/
└── maps/

features/
├── assets/
├── rentals/
├── donations/
├── recycling/
├── analytics/
└── notifications/

lib/
├── db/
├── auth/
├── repositories/
├── services/
├── validators/
└── events/

hooks/

store/

types/

utils/
```

---

# 5. Layer Responsibilities

## API Layer

Responsibilities:

* Parse requests
* Validate input
* Authenticate users
* Authorize access
* Call service methods
* Return standardized responses

Rules:

* No business logic
* No direct database access

---

## Service Layer

Responsibilities:

* Business rules
* Asset lifecycle validation
* Routing decisions
* Permission enforcement
* Event generation
* Transaction orchestration

Services coordinate repositories.

---

## Repository Layer

Responsibilities:

* Database operations
* Query execution
* Transactions
* Persistence

Repositories must not contain business logic.

---

## Database Layer

Responsibilities:

* Data persistence
* Constraints
* Relationships
* Indexing
* Geospatial queries

---

# 6. Authentication Architecture

### Provider

* Better Auth

### Authentication Methods

* Email & Password
* Google OAuth

### Session Type

* JWT-Based Sessions

### Route Protection

Middleware-based authorization

### Access Flow

Anonymous User
→ Login

Authenticated User
→ Dashboard

Administrator
→ Admin Dashboard

---

# 7. Authorization Architecture

Role-Based Access Control (RBAC)

### Roles

* OWNER
* STUDENT
* HUB_OPERATOR
* NGO
* RECYCLER
* ADMIN

A user may have multiple roles simultaneously.

Example:

User

* OWNER
* STUDENT

The currently selected role determines UI visibility and permissions.

---

# 8. Persona Switching

Users may dynamically switch between personas.

Example:

Current Persona:

OWNER

Switch To:

STUDENT

Requirements:

* Navigation updates immediately
* Dashboard updates immediately
* Permission checks update immediately
* Active persona stored in Zustand
* Active persona persisted in the database

---

# 9. Fulfillment Hub Architecture

Amazon Loop operates through a Fulfillment Hub Network.

A Fulfillment Hub is any approved location capable of receiving, storing, verifying, transferring, donating, renting, or recycling assets.

### Supported Hub Types

* KIRANA
* DARK_STORE
* CAMPUS_CENTER
* NGO_CENTER
* RECYCLING_CENTER

### Common Capabilities

* Asset Intake
* Condition Verification
* Temporary Storage
* Capacity Management
* Asset Handoff
* Asset Transfer

---

# 10. Fulfillment Routing Engine

Purpose:

Automatically determine the optimal hub for an asset.

### Routing Factors

* Distance
* Available Capacity
* Hub Type Compatibility
* Asset Category Compatibility
* Operating Hours
* Estimated Logistics Cost

### Routing Priority

1. Nearest Compatible Hub
2. Lowest Logistics Cost
3. Highest Available Capacity

The routing engine must support dynamic reassignment when hubs become unavailable or reach capacity.

---

# 11. State Management

Global State Library:

* Zustand

### Auth Store

Contains:

* User
* Roles
* Active Persona

### Notification Store

Contains:

* Notifications
* Unread Count

### UI Store

Contains:

* Theme
* Sidebar State
* Modal State

---

# 12. Data Fetching Strategy

Library:

* TanStack Query

Requirements:

* Request Caching
* Background Refetching
* Optimistic Updates
* Pagination Support
* Error Recovery

All API interactions must use typed query hooks.

---

# 13. Validation Strategy

Library:

* Zod

Requirements:

* Every API request validated
* Every API response typed
* Every form validated
* Shared schemas between frontend and backend

---

# 14. Event-Driven Architecture

Every lifecycle change generates an immutable event.

### Event Examples

* AssetCreated
* AssetStaged
* RentalStarted
* RentalEnded
* DonationCompleted
* AssetScrapped

Rules:

* Events are immutable
* Events cannot be updated
* Events can only be appended

---

# 15. Asset Lifecycle Engine

Every state transition must be validated.

Illegal transitions must be rejected.

Example:

Invalid:

OWNED
→ CAMPUS_RENTAL

Required:

OWNED
→ STAGED_AT_HUB
→ CAMPUS_RENTAL

The lifecycle engine is the single source of truth for asset state transitions.

---

# 16. Geospatial Architecture

PostGIS is required.

### Functions

* ST_DWithin()
* ST_Distance()
* ST_MakePoint()

### Supported Queries

* Nearby Fulfillment Hubs
* Radius Searches
* Distance Sorting
* Capacity-Based Searches
* Hub-Type Filtering

Coordinate System:

* SRID 4326 (WGS84)

---

# 17. File Storage Architecture

### Storage Provider

* AWS S3

### Upload Service

* UploadThing

### Supported Files

* Asset Images
* Delivery Proof Images
* Condition Reports
* PDFs

### Constraints

Maximum Size:

* 10 MB

Allowed Types:

* JPG
* PNG
* WEBP
* PDF

---

# 18. Caching Strategy

Future-Ready Architecture

### Cache Layer

* Redis

### Use Cases

* Session Caching
* Analytics Caching
* Hub Lookup Caching
* Notification Queues
* Rate Limiting

The MVP may launch without Redis.

---

# 19. Logging Architecture

Every significant action must generate structured logs.

### Log Levels

* INFO
* WARN
* ERROR
* CRITICAL

### Format

JSON

### Required Fields

* timestamp
* requestId
* userId
* action
* resource
* metadata

---

# 20. Error Handling

### Success Response

```json
{
  "success": true,
  "data": {}
}
```

### Failure Response

```json
{
  "success": false,
  "error": {
    "code": "ASSET_NOT_FOUND",
    "message": "Asset not found"
  }
}
```

Rules:

* Never expose stack traces
* Use standardized error codes
* Log all server-side failures

---

# 21. Security Requirements

### Password Hashing

* Argon2

### Security Controls

* HTTPS Only
* Rate Limiting
* CSRF Protection
* Input Validation
* Output Sanitization
* Audit Logging
* RBAC Enforcement
* Secure File Upload Validation

---

# 22. Performance Requirements

### Dashboard Load Time

* Under 2 seconds

### API Response Time

* Under 300ms average

### Database Query Time

* Under 100ms average

### Scalability Targets

* 100,000+ Assets
* 10,000+ Users
* 1,000+ Fulfillment Hubs

---

# 23. Scalability Roadmap

### Phase 1

* Modular Monolith
* PostgreSQL
* Background Jobs

### Phase 2

* Redis
* Event Queue
* Dedicated Worker Services

### Phase 3

Microservices

* Asset Service
* Rental Service
* Analytics Service
* Notification Service
* Fulfillment Service

---

# 24. Development Standards

### Code Quality

* TypeScript Strict Mode
* No Any Types
* ESLint Required
* Prettier Required

### Architecture Rules

* Repository Pattern Mandatory
* Service Layer Mandatory
* Reusable Components Only
* No Business Logic in UI Components

### Every Feature Must Include

* Validation
* Error Handling
* Audit Logging
* Unit Tests
* Integration Tests
* Documentation
