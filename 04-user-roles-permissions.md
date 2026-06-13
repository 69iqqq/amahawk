
# Amazon Loop - User Roles & Permissions Specification

## 1. Overview

Amazon Loop uses a Role-Based Access Control (RBAC) system.

Users may have one or more roles assigned to their account.

Examples:

* A user may be both OWNER and STUDENT.
* A user may be both OWNER and NGO representative.
* A user may switch between active personas without logging out.

The currently selected persona determines:

* Visible navigation items
* Accessible pages
* Available actions
* Dashboard content
* API permissions

---

# 2. System Roles

The platform supports the following roles:

* OWNER
* STUDENT
* HUB_OPERATOR
* NGO
* RECYCLER
* ADMIN

---

# 3. Permission Categories

Permissions are grouped into the following domains:

### Asset Management

* Create Asset
* Edit Asset
* Delete Asset
* View Asset
* Stage Asset
* Donate Asset
* Scrap Asset

### Rental Management

* Create Rental
* Accept Rental
* Return Rental
* Cancel Rental

### Fulfillment Hub Management

* View Hub
* Manage Hub
* Verify Asset
* Update Capacity

### Donation Management

* Request Donation
* Approve Donation
* Receive Donation

### Recycling Management

* Receive Scrap
* Create Bundles
* Report Recovery Value

### Administration

* Manage Users
* Manage Roles
* Manage Hubs
* View Analytics
* Configure Platform

---

# 4. OWNER Role

## Description

Owners are individuals who register and monetize assets.

---

## Allowed Actions

### Assets

✓ Create Assets

✓ Edit Own Assets

✓ Delete Own Assets

✓ View Own Assets

✓ Upload Asset Images

✓ Stage Assets At Hubs

✓ Donate Assets

✓ Submit Assets For Recycling

---

### Rentals

✓ View Rental Requests

✓ Approve Rental Requests

✓ Reject Rental Requests

✓ View Rental History

✓ View Earnings

---

### Analytics

✓ View Personal Analytics

✓ View Revenue

✓ View Asset Performance

---

## Restricted Actions

✗ Manage Hubs

✗ Manage Other Users

✗ Access Admin Panel

✗ Modify Platform Settings

---

# 5. STUDENT Role

## Description

Students can rent products instead of purchasing them.

---

## Allowed Actions

### Assets

✓ Browse Available Assets

✓ Search Assets

✓ Filter Assets

✓ View Asset Details

---

### Rentals

✓ Request Rental

✓ Cancel Rental Request

✓ Return Rented Assets

✓ View Rental History

✓ View Active Rentals

---

### Notifications

✓ Receive Rental Updates

✓ Receive Return Reminders

---

## Restricted Actions

✗ Create Assets

✗ Delete Assets

✗ Manage Hubs

✗ Access Admin Dashboard

---

# 6. HUB_OPERATOR Role

## Description

Hub Operators manage physical fulfillment locations.

Supported Hub Types:

* Kirana Stores
* Dark Stores
* Campus Centers
* NGO Centers
* Recycling Centers

---

## Allowed Actions

### Hub Operations

✓ View Assigned Hub

✓ Manage Assigned Hub

✓ View Capacity

✓ Update Capacity

✓ Mark Hub Availability

---

### Asset Operations

✓ Receive Assets

✓ Verify Asset Condition

✓ Approve Asset Intake

✓ Process Asset Transfers

✓ Process Returns

✓ Assign Storage Locations

---

### Logistics

✓ View Incoming Assets

✓ View Outgoing Assets

✓ Confirm Handoffs

✓ Track Asset Movement

---

## Restricted Actions

✗ Modify User Accounts

✗ Access Platform Settings

✗ View Global Analytics

---

# 7. NGO Role

## Description

NGOs receive donated products.

---

## Allowed Actions

### Donations

✓ Browse Donation Inventory

✓ Request Donations

✓ Accept Donations

✓ Confirm Donation Receipt

✓ View Donation History

---

### Reporting

✓ Generate Donation Reports

✓ View Donation Metrics

---

## Restricted Actions

✗ Manage Platform Users

✗ Manage Hubs

✗ Modify Assets Owned By Others

✗ Access Admin Panel

---

# 8. RECYCLER Role

## Description

Recyclers recover value from damaged assets.

---

## Allowed Actions

### Recycling

✓ Receive Scrap Assets

✓ Inspect Components

✓ Create Component Bundles

✓ Record Recovery Value

✓ Track Recycled Inventory

---

### Reporting

✓ View Recovery Reports

✓ Submit Recovery Reports

---

## Restricted Actions

✗ Manage Rentals

✗ Manage Users

✗ Manage Hubs

✗ Access Admin Features

---

# 9. ADMIN Role

## Description

Administrators manage the entire platform.

---

## Allowed Actions

### User Management

✓ View All Users

✓ Create Users

✓ Update Users

✓ Delete Users

✓ Assign Roles

✓ Revoke Roles

---

### Hub Management

✓ Create Hubs

✓ Update Hubs

✓ Delete Hubs

✓ Assign Operators

✓ View Hub Metrics

---

### Asset Management

✓ View All Assets

✓ Override Asset Status

✓ Resolve Asset Disputes

✓ Remove Assets

---

### Rental Management

✓ View All Rentals

✓ Resolve Rental Disputes

✓ Cancel Rentals

---

### Donations

✓ View All Donations

✓ Approve Donations

✓ Resolve Donation Issues

---

### Recycling

✓ View Recycling Activity

✓ Manage Recyclers

✓ View Recovery Metrics

---

### Analytics

✓ Global Analytics Access

✓ Revenue Analytics

✓ Hub Utilization Analytics

✓ Asset Lifecycle Analytics

---

### System Administration

✓ Platform Configuration

✓ Feature Flags

✓ Notification Templates

✓ Audit Logs

✓ Monitoring Dashboards

---

# 10. Persona Switching Rules

Users may switch personas without logging out.

Example:

Available Roles:

* OWNER
* STUDENT

Current Persona:

OWNER

User Switches To:

STUDENT

Result:

* Navigation changes
* Dashboard changes
* Permissions change
* API authorization updates

---

# 11. Permission Matrix

| Permission       | Owner    | Student  | Hub Operator | NGO           | Recycler      | Admin  |
| ---------------- | -------- | -------- | ------------ | ------------- | ------------- | ------ |
| Create Asset     | ✓        | ✗        | ✗            | ✗             | ✗             | ✓      |
| Edit Asset       | Own      | ✗        | ✗            | ✗             | ✗             | ✓      |
| Delete Asset     | Own      | ✗        | ✗            | ✗             | ✗             | ✓      |
| Request Rental   | ✗        | ✓        | ✗            | ✗             | ✗             | ✓      |
| Approve Rental   | ✓        | ✗        | ✗            | ✗             | ✗             | ✓      |
| Manage Hub       | ✗        | ✗        | Assigned     | ✗             | ✗             | ✓      |
| Request Donation | ✗        | ✗        | ✗            | ✓             | ✗             | ✓      |
| Receive Scrap    | ✗        | ✗        | ✗            | ✗             | ✓             | ✓      |
| View Analytics   | Personal | Personal | Hub Only     | Donation Only | Recovery Only | Global |
| Manage Users     | ✗        | ✗        | ✗            | ✗             | ✗             | ✓      |

---

# 12. Security Rules

Every API endpoint must:

1. Verify authentication.
2. Verify active role.
3. Verify resource ownership.
4. Log access attempts.
5. Create audit records for sensitive actions.

Unauthorized requests must return:

```json
{
  "success": false,
  "error": {
    "code": "FORBIDDEN",
    "message": "Insufficient permissions"
  }
}
```

---

# 13. Future Roles

Reserved for future expansion:

* DELIVERY_PARTNER
* ENTERPRISE_PARTNER
* GOVERNMENT_AGENCY
* UNIVERSITY_ADMIN
* MODERATOR

Future roles must integrate into the same RBAC framework.
