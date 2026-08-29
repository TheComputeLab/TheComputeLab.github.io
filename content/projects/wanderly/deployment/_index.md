---
title: "Deployment & Testing"
description: "Local setup, deployment architecture and current implementation status of Wanderly."
weight: 70
toc: true
---

## Local Development

### Backend

From the `server` directory:

```bash
npm install
node index.js
```

The documented server configuration uses:

```env
MONGO_URI=your_mongodb_connection_string
PORT=5000
```

### Frontend

From the `client` directory:

```bash
npm install
npm start
```

The client uses Create React App tooling through `react-scripts`. 

## Runtime Architecture

During local development:

```text
React Development Server
        │
        │ HTTP
        ↓
Express :5000
        │
        ↓
MongoDB Atlas
```

The frontend components currently reference the backend at:

```text
http://localhost:5000
```

for destination requests. 

## Deployment

The repository README lists:

```text
Frontend  → Netlify
Backend   → Render
Database  → MongoDB Atlas
```

However, the current GitHub repository's About section also exposes a deployed Wanderly application at a **Vercel** URL. Therefore, the README deployment note and current repository link should be treated as different project-state references rather than silently merging them. 

urlOpen the current Wanderly deploymenthttps://wanderly-five.vercel.app/

## Testing Strategy

The project can be tested at several levels.

### Frontend

Verify:

- Home page renders.
- Destination list loads.
- Search returns results.
- Destination detail opens.
- Trip Planner selects a destination.
- Three-day itinerary renders.
- Booking form accepts data.
- Traveller count changes total.
- Confirmation state appears.
- Mobile layout remains usable.

### Backend

Verify:

```text
GET /
GET /destinations
GET /destinations/:id
GET /destinations/search?q=...
POST /add-destination
```

The repository README documents the primary destination endpoints, while the frontend confirms use of the search endpoint. 

### Database

Verify:

- MongoDB connection succeeds.
- Destination documents are returned.
- Unique destination names are respected.
- Destination updates preserve timestamps.

The Mongoose model enables `timestamps: true`. 

## Error Handling

The current frontend includes basic error states such as:

```text
Connection error — is your server running?
Destination not found
Loading destination…
```

The backend currently logs MongoDB connection errors. 

## Current Status

The current repository contains:

**Implemented**

- React frontend.
- Destination browsing.
- Backend destination API.
- MongoDB/Mongoose model.
- Search flow.
- Destination detail page.
- Three-day trip planner.
- Booking-style frontend flow.
- Responsive styling.

**Future / not yet implemented as production services**

- User authentication.
- Real booking system.
- AI-based trip planner.
- Reviews and ratings.
- Map integration.
- Real payment processing.

These future items are explicitly listed in the repository README. 

## Portfolio Positioning

The strongest way to present Wanderly is:

> **A full-stack travel-planning web application demonstrating React UI development, REST API design, MongoDB data modelling, destination search, itinerary generation and a booking-oriented user journey.**

This accurately reflects the current code while leaving a clear path toward AI-powered travel planning.
