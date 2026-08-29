---
title: "Architecture"
description: "Full-stack architecture and data flow of Wanderly."
weight: 20
toc: true
---

Wanderly is structured as a classic **MERN-style application**:

```text
┌─────────────────────────────┐
│       React Frontend        │
│                             │
│ Home / Search / Detail      │
│ Trip Planner / Booking      │
└──────────────┬──────────────┘
               │ HTTP + JSON
               ↓
┌─────────────────────────────┐
│      Express Backend        │
│                             │
│ CORS + JSON Middleware      │
│ Destination Routes          │
│ Mongoose Models             │
└──────────────┬──────────────┘
               │
               ↓
┌─────────────────────────────┐
│       MongoDB Atlas         │
│                             │
│       Destinations          │
└─────────────────────────────┘
```

## Frontend

The client is a React application using React Router.

The package configuration currently specifies:

- React 18.3.1
- React DOM 18.3.1
- React Router DOM 6.22.0
- react-scripts 5.0.1



The main page modules are:

```text
WanderlyHome
DestinationDetail
TripPlanner
SearchAndBooking
```



## Backend

The Express server:

1. Loads environment variables with `dotenv`.
2. Creates an Express application.
3. Enables CORS.
4. Enables JSON request parsing.
5. Mounts destination routes.
6. Connects to MongoDB using Mongoose.
7. Starts listening on the configured port.

The current implementation uses `PORT=5000` in the documented setup. 

## Database

The core database entity is `Destination`.

The current Mongoose schema contains:

| Field | Type |
|---|---|
| name | String, required, unique |
| description | String |
| price | Number |
| image | String |
| country | String |
| highlights | String array |
| bestTime | String |
| currency | String |
| language | String |
| searchCount | Number |
| lastFetched | Date |
| createdAt | Date |
| updatedAt | Date |


This is a useful design because destination information can grow without changing the React UI architecture.

## Request Flow

### Load destinations

```text
WanderlyHome
     ↓
GET /destinations
     ↓
Express destination route
     ↓
Mongoose
     ↓
MongoDB
     ↓
JSON response
     ↓
React state
     ↓
Destination cards
```

The home page currently fetches `http://localhost:5000/destinations` when it mounts. 

### Search

```text
User enters destination
        ↓
GET /destinations/search?q=...
        ↓
Backend search
        ↓
Destination result
        ↓
React updates results
```

The home page explicitly performs this backend search and then merges the returned destination into its local destination list. 

### Destination details

```text
Destination card
      ↓
Route with destination ID
      ↓
GET /destinations/:id
      ↓
MongoDB
      ↓
DestinationDetail
```

The detail page fetches the destination by ID and renders its image, description, three-day itinerary and booking card. 

## Architectural Strength

A major strength of the project is the clear separation between:

```text
Presentation
    ↓
API
    ↓
Persistence
```

This means the React UI does not need to know how MongoDB is queried, while the backend does not need to know how destination cards are rendered.
