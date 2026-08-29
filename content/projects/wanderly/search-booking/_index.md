---
title: "Search & Booking"
description: "Destination search, filtering, traveller details and booking confirmation flow."
weight: 60
toc: true
---

Wanderly contains a complete **booking-style frontend flow**, from destination discovery to a confirmation screen.

This is currently a UI/application flow rather than a live payment or reservation integration.

## Search Flow

```text
Search & Booking
      ↓
All Destinations
      ↓
Category-style filters
      ↓
Destination Cards
      ↓
Book Now
```

The current search page loads destinations from the backend and presents filter chips for:

```text
All
Beach
Mountain
City
Heritage
```

## Destination Card

Each card displays:

- Destination number.
- Destination name.
- Description.
- Price per person.
- Book Now action.

Prices are formatted in Indian Rupees. 

## Booking Flow

After selecting a destination:

```text
Destination
     ↓
Traveller Form
     ↓
Booking Summary
     ↓
Total Calculation
     ↓
Confirm Booking
     ↓
Confirmation
```

The current form stores:

```text
name
email
phone
travellers
date
```

## Price Calculation

The current frontend calculates:

```text
Total
=
Destination Price
×
Number of Travellers
```

The UI also displays taxes and fees as **included** and shows the total in Indian Rupees. 

## Confirmation

The current flow switches to a confirmation state when the user selects **Confirm Booking**.

The interface explicitly notes:

> No payment required now


This is important for describing the project accurately: the current repository implements the **booking experience**, but not a real payment gateway or external booking transaction.

## Destination Detail → Booking

The destination detail page also contains a "Plan This Trip" CTA. The current implementation routes the user toward the search/booking experience. 

## Why This Design?

The booking workflow is intentionally lightweight:

```text
Discovery
   ↓
Decision
   ↓
Traveller Details
   ↓
Cost Preview
   ↓
Confirmation
```

This creates a realistic travel-product interaction without requiring a payment backend.

## Future Booking Architecture

The repository README lists a real booking system as a future improvement. 

A production architecture could extend the current flow:

```text
React Booking Form
        ↓
POST /bookings
        ↓
Express Validation
        ↓
Booking Database
        ↓
Payment Gateway
        ↓
Reservation Provider
        ↓
Confirmation / Email
```

The current implementation should therefore be presented as a **booking-ready UI prototype**, not as a live travel reservation platform.
