---
title: "Frontend"
description: "React frontend architecture, pages, navigation and user experience."
weight: 30
toc: true
---

The Wanderly frontend is built with **React 18** and **React Router DOM**. The UI uses custom styling rather than a large component framework. 

## Main Pages

```text
client/src/pages/

WanderlyHome.jsx
DestinationDetail.jsx
TripPlanner.jsx
SearchAndBooking.jsx
```



## 1. Wanderly Home

The home page is designed as a cinematic travel landing experience.

The implementation uses:

- Dark background.
- Cormorant Garamond for editorial headings.
- DM Sans for interface text.
- Sticky navigation.
- Large hero section.
- Destination search.
- Destination cards.

The actual component loads the fonts from Google Fonts and presents the hero message **"Discover the world, on your terms"** with a destination search box. 

### Data behaviour

On mount:

```text
GET /destinations
      ↓
destinations state
      ↓
render cards
```

When a user searches:

```text
Search text
    ↓
GET /destinations/search?q=...
    ↓
searchResult
    ↓
update destination list
```

## 2. Destination Detail

The destination page is a dedicated guide view.

```text
Hero Image
    ↓
Destination Name
    ↓
Description
    ↓
3-Day Suggested Itinerary
    ↓
Booking Summary
    ↓
Plan This Trip
```

The page retrieves the destination using its ID and displays a hero image when available. 

The booking card currently presents:

- Starting price.
- Per-person pricing.
- Flight + hotel included.
- Guided three-day itinerary.
- 24/7 travel support.
- "Plan This Trip" CTA.


## 3. Trip Planner

The Trip Planner begins by loading destinations and asking:

> **Where do you want to go?**

The user selects a destination and receives a **personalised 3-day itinerary**.

The current implementation presents:

```text
Day 1
Arrival & hotel check-in
Local neighbourhood exploration
Welcome dinner

Day 2
Main landmark visits
Guided food walk
Sunset viewpoint

Day 3
Leisure morning
Shopping & souvenirs
Return journey
```

## 4. Search & Booking

The Search & Booking page provides:

```text
All Destinations
      ↓
Filter-style chips
      ↓
Destination cards
      ↓
Book Now
      ↓
Traveller form
      ↓
Booking summary
      ↓
Confirmation
```

The current UI exposes category-style chips for **All, Beach, Mountain, City and Heritage**.

The booking form currently collects:

- Name
- Email
- Phone
- Number of travellers
- Date

The total is calculated from destination price × traveller count. 
## UI Design Language

The current UI consistently uses:

```text
Dark / near-black background
        +
Warm off-white typography
        +
Green accent
        +
Editorial serif headings
        +
Minimal card borders
```

The frontend code uses `#0a0c0b` as the primary home background and `#4ecb8e` as a key accent colour. 

## Responsive Design

The pages include mobile breakpoints.

For example, the destination detail layout changes from a two-column layout to a single-column layout at smaller widths. 

This makes the project more than a desktop-only demo and demonstrates practical responsive UI implementation.
