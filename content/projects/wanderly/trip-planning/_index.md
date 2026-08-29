---
title: "Trip Planning"
description: "How Wanderly turns a selected destination into a structured three-day travel plan."
weight: 50
toc: true
---

Trip planning is one of Wanderly's main user-facing features.

The current implementation is intentionally simple: the user selects a destination, and the application presents a structured **three-day itinerary**.

## User Flow

```text
Trip Planner
     ↓
Load destinations
     ↓
Choose destination
     ↓
Selected destination
     ↓
Generate / display 3-day plan
     ↓
Day 1 → Day 2 → Day 3
```

The current Trip Planner loads destinations from `GET /destinations`, displays them with price information and allows the user to select one. 

## Destination Selection

The selection page displays:

- Destination number.
- Destination name.
- Description.
- Price.
- "Plan trip" action.

The implementation formats prices in Indian Rupees using the `en-IN` locale. 

## Three-Day Itinerary

The current itinerary structure is:

### Day 1 — Arrival

- Arrival & hotel check-in.
- Local neighbourhood exploration.
- Welcome dinner.

### Day 2 — Exploration

- Main landmark visits.
- Guided food walk.
- Sunset viewpoint.

### Day 3 — Leisure & Departure

- Leisure morning.
- Shopping & souvenirs.
- Return journey.

The same baseline itinerary is also embedded in the destination-detail experience. 

## Data Model vs Itinerary

An important architectural observation is that the current `Destination` MongoDB model stores destination metadata such as name, description, price, image, country, highlights, best time, currency and language, but the three-day activity template is currently defined in the React page rather than stored as destination-specific database data. 

That means the current planner is best described as a **template-based itinerary generator**, not yet a fully personalised AI travel planner.

## Future AI Direction

The repository README lists **AI-based trip planner** as a future improvement. 

A future version could evolve the current flow into:

```text
Destination
+
Travel dates
+
Budget
+
Interests
+
Traveller profile
+
Preferred pace
        ↓
AI Planner
        ↓
Personalised itinerary
        ↓
Day-by-day activities
        ↓
Booking / map integration
```

That would preserve the existing frontend concept while making the itinerary engine genuinely data-driven.

## Engineering Value

The current implementation demonstrates:

- React state management.
- Data retrieval from an API.
- Destination selection.
- Conditional rendering.
- Reusable itinerary presentation.
- Separation between destination data and presentation logic.

It is therefore a good foundation for a later AI-powered travel-planning layer.
