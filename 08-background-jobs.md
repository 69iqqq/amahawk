
# Amazon Loop - Background Jobs & Workflow Engine Specification

## 1. Overview

Amazon Loop uses asynchronous background jobs to process time-consuming operations outside the request-response cycle.

Background jobs improve:

* Performance
* Scalability
* Reliability
* User Experience

The system must use:

```text
Trigger.dev
```

as the primary workflow and job orchestration platform.

---

# 2. Design Principles

## Non-Blocking Operations

Long-running tasks must never block API requests.

Example:

Bad:

```text
User Requests Rental
↓
Wait 20 Seconds
↓
Notifications Sent
↓
Analytics Updated
↓
Response Returned
```

Good:

```text
User Requests Rental
↓
Database Updated
↓
Job Queued
↓
Response Returned Immediately
```

---

## Retry Support

All jobs must support:

* Automatic retries
* Failure recovery
* Dead-letter handling

Retry Policy:

```text
Attempt 1
Attempt 2
Attempt 3
```

After 3 failures:

```text
Mark Failed
Notify Admin
```

---

## Idempotency

Jobs must be safe to run multiple times.

Running the same job twice must not create duplicate records.

---

# 3. Job Categories

The platform uses the following categories:

### Asset Jobs

* Asset Staging
* Asset Verification
* Asset Routing

### Rental Jobs

* Rental Expiration
* Rental Reminders
* Rental Settlement

### Donation Jobs

* Donation Matching
* Donation Notifications

### Recycling Jobs

* Scrap Processing
* Bundle Generation

### Analytics Jobs

* Metrics Aggregation
* Dashboard Updates

### Notification Jobs

* Email Delivery
* Push Notifications
* In-App Notifications

---

# 4. Asset Staging Workflow

Trigger:

```text
Asset Staged At Hub
```

Actions:

1. Create Event
2. Update Inventory
3. Update Hub Capacity
4. Notify Hub Operator
5. Update Analytics

Job Name:

```text
asset.staged
```

---

# 5. Asset Routing Workflow

Trigger:

```text
New Asset Created
```

Purpose:

Determine the best fulfillment hub.

Routing Factors:

* Distance
* Capacity
* Hub Type
* Asset Category
* Availability

Job Name:

```text
asset.route
```

Outputs:

```text
Recommended Hub
Distance
Estimated Cost
```

---

# 6. Rental Request Workflow

Trigger:

```text
Rental Requested
```

Actions:

1. Notify Owner
2. Create Audit Log
3. Update Metrics

Job Name:

```text
rental.requested
```

---

# 7. Rental Approval Workflow

Trigger:

```text
Rental Approved
```

Actions:

1. Notify Student
2. Notify Hub
3. Create Event
4. Schedule Rental Start

Job Name:

```text
rental.approved
```

---

# 8. Rental Reminder Workflow

Purpose:

Remind users before rental expiration.

Schedule:

```text
7 Days Before
3 Days Before
1 Day Before
```

Recipients:

* Student
* Owner

Job Name:

```text
rental.reminder
```

---

# 9. Rental Expiration Workflow

Trigger:

```text
Rental End Date Reached
```

Actions:

1. Mark Rental Expired
2. Notify Student
3. Notify Owner
4. Notify Hub

Job Name:

```text
rental.expired
```

Runs:

```text
Daily
```

---

# 10. Asset Return Workflow

Trigger:

```text
Rental Returned
```

Actions:

1. Update Status

```text
CAMPUS_RENTAL
↓
STAGED_AT_HUB
```

2. Create Event
3. Update Hub Inventory
4. Trigger Inspection

Job Name:

```text
rental.returned
```

---

# 11. Donation Matching Workflow

Trigger:

```text
Asset Available For Donation
```

Purpose:

Find suitable NGOs.

Matching Criteria:

* Location
* Category
* NGO Preferences

Job Name:

```text
donation.match
```

Output:

```text
Top Matching NGOs
```

---

# 12. Donation Completion Workflow

Trigger:

```text
Donation Accepted
```

Actions:

1. Create Donation Record
2. Create Event
3. Notify Owner
4. Update Analytics

Job Name:

```text
donation.completed
```

---

# 13. Scrap Processing Workflow

Trigger:

```text
Asset Marked As Scrap
```

Actions:

1. Assign Recycler
2. Create Event
3. Generate Recovery Task

Job Name:

```text
recycling.process
```

---

# 14. Component Bundle Workflow

Trigger:

```text
New Components Added
```

Purpose:

Create high-value material bundles.

Examples:

```text
Copper Bundle
Battery Bundle
Plastic Bundle
```

Job Name:

```text
recycling.bundle
```

---

# 15. Notification Delivery Workflow

Channels:

* In-App
* Email

Future:

* SMS
* WhatsApp

Job Name:

```text
notification.send
```

Priority Levels:

```text
LOW
NORMAL
HIGH
CRITICAL
```

---

# 16. Analytics Aggregation Workflow

Purpose:

Generate dashboard metrics.

Metrics:

* Assets
* Rentals
* Revenue
* Donations
* Recycling Value
* Hub Utilization

Job Name:

```text
analytics.aggregate
```

Schedule:

```text
Hourly
```

---

# 17. Daily Metrics Workflow

Runs:

```text
Every Day
```

Generates:

* Daily Revenue
* Daily Rentals
* Daily Donations
* Daily Asset Movement

Job Name:

```text
analytics.daily
```

---

# 18. Weekly Reports Workflow

Runs:

```text
Every Monday
```

Generates:

* Weekly Revenue Report
* Hub Utilization Report
* Donation Report
* Recycling Report

Recipients:

* Admins

Job Name:

```text
reports.weekly
```

---

# 19. Capacity Monitoring Workflow

Runs:

```text
Every 15 Minutes
```

Checks:

```text
current_capacity
```

If:

```text
current_capacity > 90%
```

Actions:

1. Notify Hub Operator
2. Notify Admin

Job Name:

```text
hub.capacity.monitor
```

---

# 20. Audit Verification Workflow

Runs:

```text
Daily
```

Checks:

* Missing Audit Logs
* Invalid Events
* Failed Jobs

Job Name:

```text
audit.verify
```

---

# 21. Failed Job Recovery

Failed jobs must:

1. Retry automatically
2. Record failure reason
3. Notify admins after max retries

Failure Record:

```json
{
  "job": "rental.expired",
  "attempts": 3,
  "status": "failed"
}
```

---

# 22. Scheduling Requirements

| Job                  | Schedule         |
| -------------------- | ---------------- |
| rental.expired       | Daily            |
| rental.reminder      | Daily            |
| analytics.aggregate  | Hourly           |
| analytics.daily      | Daily            |
| reports.weekly       | Weekly           |
| hub.capacity.monitor | Every 15 Minutes |
| audit.verify         | Daily            |

---

# 23. Observability

Every job must log:

* Job ID
* Start Time
* End Time
* Duration
* Status
* Error Message

Metrics:

* Success Rate
* Failure Rate
* Average Duration
* Retry Count

---

# 24. Future Jobs

Reserved for future releases:

```text
AI Asset Pricing
Demand Forecasting
Carbon Impact Calculation
Inventory Optimization
Smart Hub Assignment
Fraud Detection
Recommendation Engine
```

All future workflows must integrate through the same Trigger.dev infrastructure.
