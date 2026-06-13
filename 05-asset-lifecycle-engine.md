
# Amazon Loop - Asset Lifecycle Engine Specification

## 1. Overview

The Asset Lifecycle Engine is the core business logic of Amazon Loop.

Every asset registered on the platform must move through a controlled lifecycle.

The lifecycle engine is responsible for:

* Validating state transitions
* Recording lifecycle events
* Triggering notifications
* Creating audit logs
* Updating analytics
* Enforcing business rules

The lifecycle engine acts as the single source of truth for asset status.

---

# 2. Design Principles

## Event Driven

Every state transition generates an immutable event.

Example:

```text
Asset Created
Asset Staged
Rental Started
Rental Ended
Donation Completed
Asset Scrapped
```

Events cannot be modified.

Events can only be appended.

---

## State Machine

Assets may only move through approved states.

Invalid transitions must be rejected.

Example:

```text
OWNED
  ↓
CAMPUS_RENTAL
```

Invalid

Required path:

```text
OWNED
  ↓
STAGED_AT_HUB
  ↓
CAMPUS_RENTAL
```

---

# 3. Asset States

## OWNED

Description:

Asset belongs to owner and is not currently available through the fulfillment network.

Examples:

* Stored at owner's home
* Recently purchased
* Returned from rental

Allowed Actions:

* Edit asset
* Upload images
* Stage asset
* Donate asset
* Scrap asset

Allowed Transitions:

```text
OWNED
  ↓
STAGED_AT_HUB

OWNED
  ↓
DONATED

OWNED
  ↓
SCRAPPED_FOR_PARTS
```

---

## STAGED_AT_HUB

Description:

Asset has been transferred to a fulfillment hub and is available for circulation.

Examples:

* Stored at Kirana hub
* Stored at Dark Store
* Stored at Campus Center

Allowed Actions:

* Rental listing
* Donation matching
* Inspection
* Inventory verification

Allowed Transitions:

```text
STAGED_AT_HUB
  ↓
CAMPUS_RENTAL

STAGED_AT_HUB
  ↓
DONATED

STAGED_AT_HUB
  ↓
SCRAPPED_FOR_PARTS

STAGED_AT_HUB
  ↓
OWNED
```

---

## CAMPUS_RENTAL

Description:

Asset is currently rented by a subscriber.

Allowed Actions:

* Track rental
* Extend rental
* Return asset

Allowed Transitions:

```text
CAMPUS_RENTAL
  ↓
STAGED_AT_HUB

CAMPUS_RENTAL
  ↓
OWNED
```

Notes:

Returned assets should normally return to the hub first.

---

## DONATED

Description:

Asset ownership has been permanently transferred to an NGO.

Characteristics:

* Final State
* No further rentals
* No further transfers

Allowed Transitions:

None

---

## SCRAPPED_FOR_PARTS

Description:

Asset has reached the end of its useful life.

Components are extracted for recycling.

Characteristics:

* Final State
* No future circulation

Allowed Transitions:

None

---

# 4. State Transition Matrix

| Current State      | Next State         | Allowed |
| ------------------ | ------------------ | ------- |
| OWNED              | STAGED_AT_HUB      | YES     |
| OWNED              | DONATED            | YES     |
| OWNED              | SCRAPPED_FOR_PARTS | YES     |
| OWNED              | CAMPUS_RENTAL      | NO      |
| STAGED_AT_HUB      | CAMPUS_RENTAL      | YES     |
| STAGED_AT_HUB      | DONATED            | YES     |
| STAGED_AT_HUB      | SCRAPPED_FOR_PARTS | YES     |
| STAGED_AT_HUB      | OWNED              | YES     |
| CAMPUS_RENTAL      | STAGED_AT_HUB      | YES     |
| CAMPUS_RENTAL      | OWNED              | YES     |
| CAMPUS_RENTAL      | DONATED            | NO      |
| CAMPUS_RENTAL      | SCRAPPED_FOR_PARTS | NO      |
| DONATED            | Any                | NO      |
| SCRAPPED_FOR_PARTS | Any                | NO      |

---

# 5. Lifecycle Events

Every transition must generate an event.

## AssetCreated

Triggered when:

* Owner registers asset

Creates:

```text
Asset Event
Audit Log
Notification
```

---

## AssetStaged

Triggered when:

```text
OWNED
↓
STAGED_AT_HUB
```

Creates:

```text
Asset Event
Hub Inventory Update
Notification
Audit Log
```

---

## RentalRequested

Triggered when:

* Student requests rental

Creates:

```text
Asset Event
Notification
```

---

## RentalApproved

Triggered when:

* Owner approves rental

Creates:

```text
Asset Event
Notification
```

---

## RentalStarted

Triggered when:

```text
STAGED_AT_HUB
↓
CAMPUS_RENTAL
```

Creates:

```text
Asset Event
Revenue Record
Notification
```

---

## RentalReturned

Triggered when:

```text
CAMPUS_RENTAL
↓
STAGED_AT_HUB
```

Creates:

```text
Asset Event
Condition Inspection Task
Notification
```

---

## DonationCompleted

Triggered when:

```text
OWNED
↓
DONATED
```

or

```text
STAGED_AT_HUB
↓
DONATED
```

Creates:

```text
Asset Event
Donation Record
Audit Log
```

---

## AssetScrapped

Triggered when:

```text
OWNED
↓
SCRAPPED_FOR_PARTS
```

or

```text
STAGED_AT_HUB
↓
SCRAPPED_FOR_PARTS
```

Creates:

```text
Asset Event
Recycling Record
Recovery Workflow
```

---

# 6. Lifecycle Validation Rules

Rule 1:

Assets must have an owner.

Rule 2:

Assets cannot be rented while in OWNED state.

Rule 3:

Assets must pass through a fulfillment hub before rental.

Rule 4:

Final states cannot be changed.

Final states:

```text
DONATED
SCRAPPED_FOR_PARTS
```

Rule 5:

Every status change must create an asset event.

Rule 6:

Every status change must create an audit log.

Rule 7:

All lifecycle operations must run inside a database transaction.

---

# 7. Revenue Lifecycle

Rental Revenue Flow:

```text
Owner Asset
     ↓
Hub Storage
     ↓
Rental
     ↓
Rental Revenue
     ↓
Owner Earnings
```

Revenue Record Created:

```text
Rental Started
Rental Extended
Rental Completed
```

---

# 8. Hub Lifecycle Integration

When an asset enters a hub:

```text
current_capacity + 1
```

When an asset leaves a hub:

```text
current_capacity - 1
```

Capacity must never exceed:

```text
max_capacity
```

---

# 9. Notification Triggers

Notify Owner:

* Rental Request
* Rental Approved
* Rental Returned
* Donation Completed

Notify Student:

* Rental Approved
* Rental Reminder
* Rental Expiry

Notify Hub Operator:

* Incoming Asset
* Outgoing Asset
* Capacity Threshold

Notify NGO:

* Donation Available
* Donation Approved

Notify Recycler:

* Scrap Asset Assigned

---

# 10. Lifecycle Timeline UI

Every asset detail page must display a timeline.

Example:

```text
Asset Created
      ↓
Staged At Hub
      ↓
Rental Started
      ↓
Rental Returned
      ↓
Rental Started
      ↓
Donation Completed
```

Timeline data source:

```text
asset_events
```

Events must be displayed in chronological order.

---

# 11. Future Lifecycle States

Reserved for future releases:

```text
IN_TRANSIT
UNDER_REPAIR
AUCTION_LISTED
ENTERPRISE_REUSE
EXPORT_RECYCLING
```

The lifecycle engine must be extensible to support additional states without redesigning the architecture.
