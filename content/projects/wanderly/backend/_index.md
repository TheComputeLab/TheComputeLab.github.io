---
title: "Backend & API"
description: "Express API, Mongoose model and MongoDB Atlas integration."
weight: 40
toc: true
---

The Wanderly backend is a Node.js + Express application connected to MongoDB Atlas through Mongoose.

## Server Flow

```text
Node.js
   ↓
dotenv
   ↓
Express
   ↓
CORS + JSON middleware
   ↓
Destination Routes
   ↓
Mongoose
   ↓
MongoDB Atlas
```

The current `server/index.js` implements this startup sequence directly. 

## Environment

The documented backend setup uses:

```env
MONGO_URI=your_mongodb_connection_string
PORT=5000
```

The server reads these through `process.env`. 

## API Endpoints

The repository README documents:

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/destinations` | Get all destinations |
| GET | `/destinations/:id` | Get one destination |
| POST | `/add-destination` | Add a destination |

The frontend also uses:

```text
GET /destinations/search?q=...
```

for live destination search. 

## Destination Model

The Mongoose model is:

```text
Destination
├── name
├── description
├── price
├── image
├── country
├── highlights[]
├── bestTime
├── currency
├── language
├── searchCount
├── lastFetched
├── createdAt
└── updatedAt
```

The schema defines `name` as required and unique and enables Mongoose timestamps. 

## Why MongoDB?

Destination data is naturally document-shaped:

```json
{
  "name": "Destination",
  "description": "...",
  "price": 50000,
  "image": "...",
  "country": "...",
  "highlights": ["...", "..."],
  "bestTime": "...",
  "currency": "...",
  "language": "..."
}
```

MongoDB makes this structure straightforward to store and retrieve through Mongoose.

## Search Architecture

The search experience is deliberately split between frontend filtering and backend search.

### Local filtering

The home page filters already-loaded destinations by:

```text
name
OR
description
```

### Backend search

For a new search, the UI calls:

```text
GET /destinations/search?q=<query>
```

The returned result is inserted or updated in the local React destination state. 

This allows the application to start with cached destinations and request a specific destination when needed.

## Server Startup

The current backend connects to MongoDB first and starts Express only after a successful connection:

```text
mongoose.connect(...)
       ↓
Success
       ↓
app.listen(PORT)
```

If the MongoDB connection fails, the current implementation logs the error. 

## Backend Evolution

The current API is intentionally small. The repository README identifies future opportunities such as authentication, booking integration, AI trip planning, reviews and ratings, and maps. 
