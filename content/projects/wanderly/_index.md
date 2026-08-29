---
title: "Wanderly  🌍"
description: "Full-stack smart travel planning application built with React, Node.js, Express and MongoDB."
weight: 10
toc: true
---
 
**Wanderly** is a full-stack travel planning web application that lets users discover destinations, search for places, view destination guides, create a three-day trip plan, and move through a simple booking flow.

The project combines a **React frontend**, **Node.js/Express backend**, and **MongoDB Atlas database**. The current repository is a public MSc educational project. 

## What the application does

```text
                    WANDERLY
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
   Discover       Search         Trip Planner
        │              │              │
        └──────────────┼──────────────┘
                       ↓
               Destination Guide
                       ↓
                 Booking Flow
                       ↓
                Confirmation
```

### Main capabilities

- Dynamic destination discovery.
- Destination search.
- Individual destination pages.
- Three-day itinerary generation.
- Destination pricing.
- Booking-style traveller form and confirmation.
- Full-stack React + Express + MongoDB architecture.
- Responsive dark, editorial-style travel UI.

The repository README describes the application as a "Smart Travel Planner" and lists dynamic destinations, real-time search, destination details and itinerary generation as core features. 

## Technology Stack

| Layer | Technology |
|---|---|
| Frontend | React 18 |
| Routing | React Router DOM 6 |
| Styling | Custom CSS / component-level styles |
| Backend | Node.js |
| API | Express.js |
| Database | MongoDB Atlas |
| ODM | Mongoose |
| Configuration | dotenv |
| Deployment target | Frontend + backend + MongoDB Atlas |

The client package uses React 18.3.1, React Router DOM 6.22.0 and Create React App/react-scripts 5.0.1. 

## Project Structure

```text
wanderly/
├── client/
│   ├── public/
│   └── src/
│       └── pages/
│           ├── WanderlyHome.jsx
│           ├── DestinationDetail.jsx
│           ├── TripPlanner.jsx
│           └── SearchAndBooking.jsx
├── server/
│   ├── models/
│   │   └── Destination.js
│   ├── routes/
│   ├── index.js
│   └── .env
├── src/
└── README.md
```

The current GitHub repository contains separate `client` and `server` directories, with the client pages implementing the major user-facing workflows. 

## Architecture

Wanderly follows a straightforward full-stack pattern:

```text
React UI
   ↓ HTTP / JSON
Express API
   ↓ Mongoose
MongoDB Atlas
```

The backend starts Express, enables CORS and JSON parsing, mounts the destination routes, then connects to MongoDB before starting the server. 

## Documentation

- [Architecture](architecture/) — full-stack design and request flow
- [Frontend](frontend/) — React pages, routing and UI
- [Backend & API](backend/) — Express routes, MongoDB and Mongoose
- [Trip Planning](trip-planning/) — destination selection and three-day itinerary
- [Search & Booking](search-booking/) — search, destination selection and booking flow
- [Deployment & Testing](deployment/) — setup, deployment and current project status
- [Roadmap](roadmap/) — planned enhancements

## Project Status

This documentation is based on the **current public GitHub repository and prior Wanderly development discussions**.

The current implementation contains working frontend flows and a backend destination API. Some README items are explicitly described as future improvements, including authentication, real booking integration, AI trip planning, reviews/ratings and map integration. 
