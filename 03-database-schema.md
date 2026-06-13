
# Amazon Loop - Database Schema Specification

## 1. Database Overview

Database Engine:

* PostgreSQL 16+

Required Extensions:

```sql
CREATE EXTENSION IF NOT EXISTS postgis;
CREATE EXTENSION IF NOT EXISTS pgcrypto;
```

Purpose:

* User Management
* Authentication
* Asset Tracking
* Event Sourcing
* Rental Management
* Donation Management
* Recycling Operations
* Fulfillment Hub Network
* Notifications
* Audit Logging
* Geospatial Search

---

## 2. Enumerations

### User Roles

```sql
CREATE TYPE user_role AS ENUM (
    'OWNER',
    'STUDENT',
    'HUB_OPERATOR',
    'NGO',
    'RECYCLER',
    'ADMIN'
);
```

### Hub Types

```sql
CREATE TYPE hub_type AS ENUM (
    'KIRANA',
    'DARK_STORE',
    'CAMPUS_CENTER',
    'NGO_CENTER',
    'RECYCLING_CENTER'
);
```

### Asset Status

```sql
CREATE TYPE asset_status AS ENUM (
    'OWNED',
    'STAGED_AT_HUB',
    'CAMPUS_RENTAL',
    'DONATED',
    'SCRAPPED_FOR_PARTS'
);
```

### Asset Condition

```sql
CREATE TYPE asset_condition AS ENUM (
    'NEW',
    'EXCELLENT',
    'GOOD',
    'FAIR',
    'POOR',
    'BROKEN'
);
```

### Notification Type

```sql
CREATE TYPE notification_type AS ENUM (
    'INFO',
    'SUCCESS',
    'WARNING',
    'ERROR'
);
```

---

## 3. Users

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    email VARCHAR(255) UNIQUE NOT NULL,
    full_name VARCHAR(255) NOT NULL,
    avatar_url TEXT,
    phone_number VARCHAR(30),

    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

---

## 4. User Roles

```sql
CREATE TABLE user_roles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    user_id UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,

    role user_role NOT NULL,

    created_at TIMESTAMP DEFAULT NOW()
);
```

```sql
CREATE INDEX idx_user_roles_user
ON user_roles(user_id);
```

---

## 5. Fulfillment Hubs

```sql
CREATE TABLE fulfillment_hubs (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    name VARCHAR(255) NOT NULL,

    hub_type hub_type NOT NULL,

    address TEXT NOT NULL,

    operator_id UUID REFERENCES users(id),

    location GEOMETRY(Point, 4326) NOT NULL,

    max_capacity INT DEFAULT 50,

    current_capacity INT DEFAULT 0,

    is_active BOOLEAN DEFAULT TRUE,

    created_at TIMESTAMP DEFAULT NOW(),

    updated_at TIMESTAMP DEFAULT NOW()
);
```

```sql
CREATE INDEX idx_fulfillment_hubs_location
ON fulfillment_hubs
USING GIST(location);

CREATE INDEX idx_fulfillment_hubs_type
ON fulfillment_hubs(hub_type);
```

---

## 6. Assets

```sql
CREATE TABLE inventory_items (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    owner_id UUID NOT NULL REFERENCES users(id),

    assigned_hub_id UUID REFERENCES fulfillment_hubs(id),

    product_name VARCHAR(255) NOT NULL,

    category VARCHAR(100) NOT NULL,

    description TEXT,

    original_price NUMERIC(10,2) NOT NULL,

    liquidation_price NUMERIC(10,2),

    condition asset_condition DEFAULT 'GOOD',

    status asset_status DEFAULT 'OWNED',

    purchase_date TIMESTAMP,

    created_at TIMESTAMP DEFAULT NOW(),

    updated_at TIMESTAMP DEFAULT NOW()
);
```

```sql
CREATE INDEX idx_assets_owner
ON inventory_items(owner_id);

CREATE INDEX idx_assets_status
ON inventory_items(status);

CREATE INDEX idx_assets_category
ON inventory_items(category);
```

---

## 7. Asset Images

```sql
CREATE TABLE asset_images (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    asset_id UUID NOT NULL REFERENCES inventory_items(id) ON DELETE CASCADE,

    image_url TEXT NOT NULL,

    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 8. Asset Events

```sql
CREATE TABLE asset_events (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    asset_id UUID NOT NULL REFERENCES inventory_items(id),

    actor_id UUID REFERENCES users(id),

    event_type VARCHAR(100) NOT NULL,

    previous_status asset_status,

    new_status asset_status,

    metadata JSONB,

    created_at TIMESTAMP DEFAULT NOW()
);
```

```sql
CREATE INDEX idx_asset_events_asset
ON asset_events(asset_id);

CREATE INDEX idx_asset_events_created
ON asset_events(created_at DESC);
```

---

## 9. Campus Rentals

```sql
CREATE TABLE campus_rental_ledger (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    item_id UUID NOT NULL REFERENCES inventory_items(id),

    owner_id UUID NOT NULL REFERENCES users(id),

    subscriber_id UUID NOT NULL REFERENCES users(id),

    start_date TIMESTAMP NOT NULL,

    end_date TIMESTAMP NOT NULL,

    term_price NUMERIC(10,2) NOT NULL,

    transit_status VARCHAR(50),

    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 10. Donations

```sql
CREATE TABLE donations (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    asset_id UUID NOT NULL REFERENCES inventory_items(id),

    donor_id UUID NOT NULL REFERENCES users(id),

    ngo_id UUID REFERENCES users(id),

    donation_date TIMESTAMP DEFAULT NOW(),

    status VARCHAR(50) DEFAULT 'PENDING'
);
```

---

## 11. Recyclers

```sql
CREATE TABLE recyclers (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    company_name VARCHAR(255) NOT NULL,

    contact_person VARCHAR(255),

    phone_number VARCHAR(30),

    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 12. Broken Components

```sql
CREATE TABLE broken_components (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    item_id UUID NOT NULL REFERENCES inventory_items(id),

    component_name VARCHAR(100) NOT NULL,

    estimated_value NUMERIC(10,2),

    is_bundled BOOLEAN DEFAULT FALSE,

    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 13. Component Bundles

```sql
CREATE TABLE component_bundles (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    recycler_id UUID REFERENCES recyclers(id),

    bundle_name VARCHAR(255),

    estimated_value NUMERIC(10,2),

    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 14. Notifications

```sql
CREATE TABLE notifications (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    user_id UUID NOT NULL REFERENCES users(id),

    title VARCHAR(255) NOT NULL,

    message TEXT NOT NULL,

    type notification_type NOT NULL,

    is_read BOOLEAN DEFAULT FALSE,

    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 15. Audit Logs

```sql
CREATE TABLE audit_logs (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    actor_id UUID REFERENCES users(id),

    action VARCHAR(255) NOT NULL,

    resource_type VARCHAR(100),

    resource_id UUID,

    metadata JSONB,

    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 16. Revenue Records

```sql
CREATE TABLE revenue_records (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    asset_id UUID REFERENCES inventory_items(id),

    owner_id UUID REFERENCES users(id),

    amount NUMERIC(10,2),

    revenue_type VARCHAR(100),

    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 17. Saved Searches

```sql
CREATE TABLE saved_searches (

    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),

    user_id UUID REFERENCES users(id),

    search_filters JSONB,

    created_at TIMESTAMP DEFAULT NOW()
);
```

---

## 18. Future Tables

Reserved for future releases:

* payments
* subscriptions
* logistics_shipments
* disputes
* reviews
* recommendations
* ai_pricing_models
* carbon_impact_metrics

---

## 19. Database Standards

Every write operation must:

1. Update domain tables.
2. Create audit logs.
3. Create lifecycle events.
4. Trigger notifications.
5. Execute within a transaction.

All primary keys use UUIDs.

All lifecycle transitions must be tracked through asset_events.
