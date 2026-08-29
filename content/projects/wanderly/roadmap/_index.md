---
title: "Roadmap"
description: "Future evolution of Wanderly into an AI-powered travel planning platform."
weight: 80
toc: true
---

Wanderly already establishes the core travel-product workflow:

```text
Discover
   ↓
Search
   ↓
Destination Guide
   ↓
Trip Plan
   ↓
Booking Flow
```

The next evolution is to turn the current application into a more intelligent and data-driven travel platform.

## Phase 1 — Strengthen Current Platform

- Centralise frontend API configuration.
- Add stronger API validation.
- Add loading and error states consistently.
- Add automated frontend/backend tests.
- Add production environment configuration.
- Improve accessibility.
- Add persistent booking records.

## Phase 2 — Authentication

The repository README lists authentication as a future improvement. 

Target architecture:

```text
User
 ↓
Register / Login
 ↓
JWT / Session
 ↓
Protected APIs
 ↓
Saved Trips
Bookings
Preferences
```

## Phase 3 — Real Booking System

Replace the current confirmation-only flow with:

```text
Booking Form
    ↓
Backend Validation
    ↓
Booking Record
    ↓
Payment
    ↓
Reservation
    ↓
Confirmation
```

The current UI already provides traveller information and total-price calculation, so it provides a natural starting point.

## Phase 4 — Maps

The repository README identifies map integration as a future improvement. 

A map layer could connect:

```text
Destination
   ↓
Coordinates
   ↓
Map
   ↓
Hotels / Attractions
   ↓
Daily Route
```

## Phase 5 — Reviews & Ratings

Add:

```text
User
 ↓
Visited Destination
 ↓
Rating + Review
 ↓
MongoDB
 ↓
Destination Score
```

This would turn destination pages into community-driven travel guides.

## Phase 6 — AI Trip Planner

The most significant future feature is the AI-based planner listed in the repository roadmap. 

The current template-based planner can evolve into:

```text
Destination
+
Dates
+
Budget
+
Interests
+
Traveller Type
+
Travel Pace
+
Preferences
        ↓
AI Trip Planner
        ↓
Personalised Itinerary
        ↓
Map Route
        ↓
Hotels / Activities
        ↓
Booking Options
```

## Phase 7 — Intelligent Destination Search

The current backend search endpoint can become semantic search:

```text
User:
"quiet beach destination for a family"

             ↓

Embedding / Semantic Search

             ↓

Destination Ranking

             ↓

Personalised Results
```

This would be a meaningful AI upgrade over simple text matching.

## Phase 8 — Personalisation

With authentication and saved trips:

```text
Past Trips
+
Saved Destinations
+
Ratings
+
Budget
+
Interests
        ↓
Traveller Profile
        ↓
Personalised Recommendations
```

## Phase 9 — Production Architecture

Long-term:

```text
                  React / Mobile
                        ↓
                 API Gateway
                        ↓
          ┌─────────────┼─────────────┐
          ↓             ↓             ↓
      Auth API      Travel API     AI Planner
          │             │             │
          └─────────────┼─────────────┘
                        ↓
                  MongoDB Atlas
                        +
                  Cache / Search
                        +
                 External Travel APIs
```

## Final Vision

Wanderly can evolve from a travel website into:

> **An AI-powered personal travel companion that discovers destinations, builds personalised itineraries, maps the journey and connects the traveller to booking services.**

The current repository already provides the important foundation: React-based discovery, Express APIs, MongoDB destination data, destination details, itinerary presentation and a booking-oriented user journey. 
