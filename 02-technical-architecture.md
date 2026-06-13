# BUILD AMAZON LOOP Use dummy database nopw

You are a senior staff engineer, product architect, UI designer, database architect, and AI systems engineer.

Build a production-quality web application called **Amazon Loop**.

Do not generate placeholder code.

Do not simplify architecture.

Generate scalable, maintainable, enterprise-grade code.

Follow all requirements below exactly.

---

# PRODUCT OVERVIEW

Amazon Loop is an AI-powered circular commerce platform that transforms e-commerce returns into an intelligent sustainable ecosystem.

Instead of:

Buy → Return → Discard

Amazon Loop creates:

Buy → Return → Evaluate → Refurbish → Resell → Recycle

The platform uses AI to determine the best next destination for returned or unused products.

The system should feel like a real Amazon product.

Design quality should be extremely high.

---

# CORE PROBLEM

Millions of products are returned every year.

Many are:

* perfectly functional
* underused
* discarded unnecessarily

Customers also struggle to trust refurbished products.

Amazon Loop solves this through:

* AI return prevention
* AI grading
* AI routing
* AI trust reports
* sustainable incentives
* trusted resale marketplace

---

# TECH STACK

Frontend:

* Next.js 15 App Router
* TypeScript
* Tailwind CSS
* shadcn/ui
* Framer Motion
* React Hook Form
* Zod
* TanStack Query
* Zustand

Backend:

* Next.js Route Handlers
* TypeScript
* Better Auth
* PostgreSQL
* Drizzle ORM

Storage:

* AWS S3
* UploadThing

Background Jobs:

* Trigger.dev

Analytics:

* PostHog

Monitoring:

* Sentry

Maps:

* Mapbox

Charts:

* Recharts

---

# DESIGN REQUIREMENTS

Visual Style:

Modern Amazon Internal Product

Must communicate:

* Trust
* Sustainability
* Reliability
* Intelligence
* Professionalism

Color Palette:

Primary:
Amazon Orange

Secondary:
Deep Blue

Accent:
Green

UI Requirements:

* Responsive
* Mobile First
* Dark Mode
* Light Mode
* Beautiful Animations
* Loading States
* Empty States
* Skeleton Loaders
* Toast Notifications

Use premium SaaS quality.

Think Linear + Stripe + Amazon.

---

# USER ROLES

CUSTOMER

REFURB_PARTNER

RECYCLING_PARTNER

NGO_PARTNER

WAREHOUSE_OPERATOR

ADMIN

Role-based permissions required.

---

# CORE FEATURES

## 1. Predictive Return Prevention

Before checkout:

Analyze:

* historical returns
* sizing issues
* compatibility risks

Show AI recommendation:

"Customers like you usually order one size larger."

Display:

* return probability
* confidence score
* recommendation

---

## 2. Smart Product Grading

User uploads:

* images
* videos

AI analyzes:

* scratches
* dents
* wear
* missing accessories
* packaging quality

Generate grade:

* Mint
* Excellent
* Good
* Fair
* Damaged
* Recycle

Create condition report.

---

## 3. AI Routing Engine

Automatically determine destination.

Possible destinations:

* Open Box
* Refurbishment
* Marketplace
* Donation
* Recycling
* Exchange

Routing factors:

* condition
* demand
* distance
* resale value
* repair cost
* environmental impact

Show routing explanation.

---

## 4. Trust Passport

Every product receives:

* inspection report
* images
* damage map
* battery health
* certification status
* repair history

Generate beautiful visual report.

---

## 5. Green Credits

Users earn credits for:

* donating
* recycling
* buying refurbished
* slower shipping

Dashboard should show:

* balance
* history
* rewards
* achievements

---

## 6. Refurbished Marketplace

Marketplace for certified products.

Features:

* search
* filters
* categories
* condition filters
* AI recommendations

Product cards must display:

* savings
* condition
* trust score
* green credits reward

---

## 7. Next Best Owner AI

Core feature.

For every returned product:

Determine:

* likely buyer
* demand score
* location demand
* resale probability

Show AI explanation.

---

## 8. Admin Dashboard

Analytics:

* returns
* refurbished sales
* recycling
* donations
* revenue recovered

Charts required.

---

# DATABASE DESIGN

Create complete Drizzle schema.

Tables:

users

products

returns

return_scans

condition_reports

trust_passports

routing_decisions

marketplace_listings

green_credit_accounts

green_credit_transactions

recommendations

donations

recycling_orders

refurbishment_orders

notifications

audit_logs

asset_events

All tables must include:

* id
* created_at
* updated_at

Use UUIDs.

Create indexes.

Create relationships.

---

# EVENT SYSTEM

Create immutable event architecture.

Events:

ReturnCreated

ScanUploaded

ConditionGraded

RoutingCalculated

RefurbishmentStarted

RefurbishmentCompleted

DonationCompleted

RecyclingCompleted

MarketplaceListed

MarketplaceSold

CreditsAwarded

Every state transition should generate events.

---

# API ENDPOINTS

Generate complete API structure.

Examples:

POST /api/returns

POST /api/scans

POST /api/ai/grade

POST /api/ai/route

GET /api/trust-passport/:id

GET /api/marketplace

POST /api/green-credits/redeem

GET /api/admin/analytics

All endpoints require:

* validation
* authentication
* authorization
* error handling

---

# FOLDER STRUCTURE

Use feature-based architecture.

src/

app/

components/

features/

returns/

ai-grading/

routing-engine/

trust-passport/

marketplace/

green-credits/

recommendations/

recycling/

donations/

analytics/

notifications/

lib/

services/

repositories/

validators/

events/

hooks/

store/

types/

utils/

---

# DASHBOARDS

Customer Dashboard

Show:

* returns
* credits
* purchases
* sustainability impact

Warehouse Dashboard

Show:

* incoming returns
* grading queue
* routing decisions

Admin Dashboard

Show:

* total returns
* recovered revenue
* landfill reduction
* carbon savings
* marketplace performance

---

# AI MOCKING REQUIREMENT

For MVP:

Use mocked AI responses.

Create service abstraction layer.

Future AI providers:

* OpenAI
* Claude
* Gemini

Should be replaceable without changing business logic.

---

# SECURITY

Implement:

* Better Auth
* RBAC
* Rate Limiting
* CSRF Protection
* Audit Logs
* Secure Upload Validation

---

# PERFORMANCE

Target:

* Dashboard < 2s
* API < 300ms
* Lighthouse > 90

---

# DEVELOPMENT RULES

Use TypeScript strict mode.

No any types.

Repository Pattern required.

Service Layer required.

No business logic inside React components.

Reusable components only.

Use clean architecture.

Generate production-ready codebase.

Start by creating:

1. Project Structure
2. Database Schema
3. Authentication
4. Core Layout
5. Feature Modules
6. API Routes
7. Dashboard Pages
8. Mock AI Services
9. Seed Data
10. Deployment Configuration

Then implement the entire application step-by-step.
