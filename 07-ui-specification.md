
# Amazon Loop - UI & Design Specification

## 1. Design Philosophy

Amazon Loop should feel like a natural extension of the Amazon ecosystem.

The experience should communicate:

* Trust
* Reliability
* Simplicity
* Operational Efficiency
* Sustainability
* Professionalism

The visual design should resemble a modern Amazon internal product while maintaining its own identity.

The user should feel:

"This looks like an Amazon service."

---

# 2. Design Principles

### Functional First

Every screen should prioritize usability over visual complexity.

---

### Data Driven

Users should immediately understand:

* Asset status
* Rental activity
* Revenue
* Hub activity
* Notifications

---

### Minimal Cognitive Load

Reduce unnecessary clicks.

Prefer:

* Tables
* Cards
* Drawers
* Contextual actions

Avoid:

* Deep navigation trees
* Excessive modal usage

---

# 3. Brand Positioning

Amazon Loop is presented as:

```text id="w7v6fr"
Amazon Loop

Circular Economy Platform
```

Tagline:

```text id="j9v1r8"
Reuse. Rent. Donate. Recycle.
```

---

# 4. Visual Style

### Style Direction

* Amazon-inspired
* Enterprise-grade
* Modern SaaS
* Clean
* High readability
* Operational dashboard focused

---

### Design Keywords

```text id="1r7z6h"
Professional
Trustworthy
Efficient
Scalable
Sustainable
Minimal
```

---

# 5. Color System

## Primary Colors

Amazon Orange

```css id="wjr18w"
#FF9900
```

Amazon Dark Blue

```css id="ixxlt5"
#131A22
```

---

## Secondary Colors

Dark Gray

```css id="srl8c6"
#232F3E
```

Light Gray

```css id="5krj5r"
#F3F4F6
```

White

```css id="h2mx0r"
#FFFFFF
```

---

## Semantic Colors

Success

```css id="2cth6e"
#16A34A
```

Warning

```css id="9c5aqk"
#F59E0B
```

Danger

```css id="xktd4m"
#DC2626
```

Info

```css id="aq3uxm"
#2563EB
```

---

# 6. Typography

Primary Font:

```text id="u9p31m"
Amazon Ember (if licensed)
```

Fallback:

```text id="98c83o"
Inter
```

Final Font Stack:

```css id="uy7sr6"
font-family:
"Amazon Ember",
"Inter",
sans-serif;
```

---

# 7. Border Radius

Cards

```css id="tct5ta"
12px
```

Buttons

```css id="rt5owd"
8px
```

Inputs

```css id="dxr9dn"
8px
```

Modals

```css id="2gs9hk"
16px
```

---

# 8. Shadows

Cards

```css id="ozxj53"
0 1px 3px rgba(0,0,0,.08)
```

Hover

```css id="o9j9r3"
0 4px 12px rgba(0,0,0,.12)
```

---

# 9. Layout System

### Maximum Width

```css id="6jlwmg"
1440px
```

---

### Grid

```css id="afm5em"
12 Column Grid
```

---

### Spacing Scale

```text id="nzhclm"
4
8
12
16
24
32
48
64
```

---

# 10. Navigation Layout

Desktop:

```text id="wqf4j0"
┌──────── Top Navigation ────────┐
│ Logo                           │
│ Search                         │
│ Notifications                  │
│ User Menu                      │
└────────────────────────────────┘

┌ Sidebar ─────┬─────────────────┐
│ Dashboard    │                 │
│ Assets       │                 │
│ Rentals      │ Main Content    │
│ Hubs         │                 │
│ Analytics    │                 │
│ Admin        │                 │
└──────────────┴─────────────────┘
```

---

# 11. Persona Switcher

Top-right user menu:

```text id="v4v1mg"
Current Persona

OWNER ▼
```

Options:

```text id="xbsmn5"
OWNER
STUDENT
HUB_OPERATOR
NGO
RECYCLER
ADMIN
```

Switching personas updates:

* Navigation
* Dashboard
* Permissions
* Available actions

Without page reload.

---

# 12. Dashboard Design

Dashboard Cards:

```text id="r3y1ql"
Total Assets

Active Rentals

Revenue

Donations

Hub Utilization
```

Display:

* KPI Cards
* Trend Charts
* Recent Activity
* Notifications

---

# 13. Asset Pages

Asset List:

Features:

* Search
* Filters
* Sorting
* Bulk Actions

View Modes:

```text id="g7u0wu"
Table View
Grid View
```

---

Asset Detail:

Sections:

```text id="8o17te"
Overview

Images

Rental History

Lifecycle Timeline

Revenue

Events
```

---

# 14. Fulfillment Hub Pages

Display:

```text id="td3m7u"
Hub Information

Capacity

Asset Count

Location

Hub Type
```

Map View:

Required.

---

# 15. Analytics Design

Charts:

* Line Charts
* Bar Charts
* Pie Charts
* Area Charts

Library:

```text id="x1h4ot"
Recharts
```

Metrics:

* Revenue
* Rentals
* Donations
* Recycling Value
* Hub Utilization

---

# 16. Maps

Library:

```text id="9x8p3f"
Mapbox GL
```

Features:

* Nearby Hubs
* Hub Clustering
* Capacity Visualization
* Radius Search

---

# 17. Tables

Library:

```text id="7q0vwh"
TanStack Table
```

Required Features:

* Pagination
* Sorting
* Filtering
* Column Visibility
* Row Selection

---

# 18. Forms

Library:

```text id="ddmx0w"
React Hook Form
```

Validation:

```text id="78wctw"
Zod
```

Requirements:

* Inline Validation
* Error Messages
* Loading States
* Success Feedback

---

# 19. Loading States

Use:

* Skeleton Screens
* Loading Spinners
* Progress Indicators

Never show blank pages.

---

# 20. Empty States

Every page must have meaningful empty states.

Example:

```text id="o6ljht"
No Assets Found

Create your first asset to begin
participating in Amazon Loop.
```

---

# 21. Notifications

Display as:

* Toasts
* Notification Center
* Email Alerts

Priority Levels:

* Info
* Success
* Warning
* Critical

---

# 22. Accessibility

Requirements:

* WCAG AA Compliance
* Keyboard Navigation
* Screen Reader Support
* Visible Focus States

---

# 23. Dark Mode

Required.

Themes:

```text id="vav4rk"
Light

Dark
```

Default:

```text id="xvs0ql"
System Preference
```

---

# 24. Component Library Requirements

Use:

```text id="7s9u7z"
shadcn/ui
```

Required Components:

* Cards
* Tables
* Dialogs
* Drawers
* Dropdowns
* Tooltips
* Tabs
* Forms
* Toasts

---

# 25. Animation Guidelines

Library:

```text id="cfbgo7"
Framer Motion
```

Use animations for:

* Page transitions
* Sidebar transitions
* Modal opening
* Toast notifications

Avoid excessive animation.

Animations should feel fast and professional.

---

# 26. Mobile Responsiveness

Breakpoints:

```text id="knyjfe"
Mobile
Tablet
Desktop
Wide Screen
```

Every page must be fully responsive.

Mobile-first implementation required.

---

# 27. Overall User Experience Goal

The application should feel like:

```text id="s5s0l9"
Amazon Seller Central
+
Amazon Logistics
+
Modern SaaS Dashboard
```

while maintaining a unique identity centered around:

```text id="zcmctc"
Reuse
Rent
Donate
Recycle
```
