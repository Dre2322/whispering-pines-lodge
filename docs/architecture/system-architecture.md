## Architecture Style --- Modular Monolith

### Authentication

-   Authenticate users securely.
-   Maintain user sessions.
-   Distinguish guests, staff, and administrators through authorization
    rules.

### Accommodation / Rooms

-   Provide room-type information, descriptions, amenities, images,
    occupancy, and pricing.
-   Determine whether physical rooms are available for requested dates.
-   Track operational room status separately from reservation
    availability.

### Stay Planner

-   Collect trip details and guest preferences.
-   Evaluate guest count, adults/children, trip type, dates, and
    preferences.
-   Rank suitable available room types.
-   Return a primary recommendation plus suitable alternatives.

### Reservations

-   Create reservations for authenticated guests.
-   Validate requested dates, occupancy, room availability, and pricing.
-   Show a reservation summary before confirmation.
-   Create the booking as an all-or-nothing transaction.
-   Generate a unique confirmation number.
-   Allow guests to view and cancel eligible reservations.

### Staff / Administration

-   Restrict staff functionality to authorized users.
-   Allow staff to view rooms and reservations.
-   Allow staff to update room operational status.
-   Allow administrators to manage room types, rooms, amenities, and
    pricing.

### Data Persistence

-   Store users, rooms, room types, amenities, and reservations.
-   Preserve relationships and historical reservation data.
-   Enforce critical integrity constraints.

### Presentation / Web Interface

-   Render guest-facing and staff-facing pages.
-   Collect and validate user input.
-   Display errors, recommendations, reservation details, and
    confirmation information.
-   Support responsive and accessible interaction.

A modular monolith makes sense for Version 1 because Whispering Pines is
still a relatively small application, and splitting it into
microservices would add unnecessary complexity in deployment,
networking, monitoring, and data consistency. A modular monolith still
lets us cleanly separate concerns within Django. If Whispering Pines
Hospitality later expands into larger independent products or services,
we can reevaluate whether certain modules should be offered as separate
services.

## Major Components and Responsibilities

### Accounts

-   Handles authentication and user sessions.
-   Provides roles and permissions for guests, staff, and
    administrators.

### Rooms

-   Manages RoomType, Room, and Amenity information.
-   Provides availability information.
-   Tracks operational room status.

### Planner

-   Collects guest trip information and preferences.
-   Evaluates RoomTypes against guest needs.
-   Returns a primary recommendation and suitable alternatives.
-   Does not create reservations.

### Reservations

-   Validates booking requests.
-   Identifies an available physical Room for the selected RoomType.
-   Creates reservations using an all-or-nothing transaction.
-   Generates confirmation numbers.
-   Handles reservation history and eligible cancellations.

### Core / Presentation

-   Handles shared pages and common web presentation.
-   Renders templates and displays forms, errors, recommendations, and
    reservation information.

### PostgreSQL

-   Stores persistent application data.
-   Preserves relationships and historical records.
-   Supports data-integrity requirements.

## Dependency Direction

``` text
core/presentation → accounts
core/presentation → rooms
core/presentation → planner
core/presentation → reservations

planner → rooms
reservations → rooms
reservations → accounts
```

Dependencies should remain directional where possible. In particular,
`rooms` should not depend on `planner`, and `planner` should not create
or manage reservations.

## Authentication Flow

``` text
Guest enters email and password
        ↓
Authentication View receives credentials
        ↓
Django authentication checks the submitted credentials
        ↓
If credentials are valid:
- user identity is confirmed
- an authenticated session is created
- user role/permissions are available for authorization
        ↓
Guest is redirected to the appropriate page

If credentials are invalid:
- login fails
- no authenticated session is created
- guest sees an error message
```

## Stay Planner Flow

``` text
Guest answers Stay Planner questions
        ↓
Planner View receives and validates the input
        ↓
Planner Recommendation Logic
        ↓
Reads RoomType + Amenity data
        ↓
Checks RoomType availability for requested dates
        ↓
Applies recommendation rules:
- party size
- adults/children
- trip type
- preferences
- requested dates
        ↓
Ranks suitable RoomTypes
        ↓
Returns:
- best-fit recommendation
- suitable alternatives
        ↓
Guest chooses a RoomType
        ↓
Reservation flow takes over
```

## Reservation Booking Flow

``` text
Guest clicks Confirm
        ↓
Reservation View
        ↓
Validate:
- authenticated user
- check-in/check-out
- occupancy
- physical room availability
- operational status
- pricing
        ↓
Reservation Service
        ↓
Begin database transaction
        ↓
Create Reservation
Generate confirmation number
        ↓
Commit transaction
        ↓
PostgreSQL
        ↓
Confirmation data returned
        ↓
Confirmation Template
        ↓
Guest sees confirmation page
```

## Database / Persistence Layer

PostgreSQL will serve as the primary persistent data store for Version
1.

Django models will represent the application's domain entities, and the
Django ORM will provide the primary interface between application code
and PostgreSQL.

Persistent entities include: 
- User 
- RoomType 
- Room 
- Amenity 
- RoomTypeAmenity 
- Reservation

The persistence layer must enforce critical data-integrity rules,
including: 
- unique user email addresses 
- unique room identifiers 
- unique reservation confirmation numbers 
- valid entity relationships 
- valid check-in/check-out dates 
- prevention of overlapping active
reservations for the same physical room

Application business logic will remain outside the database wherever
appropriate. The database is responsible for storing and protecting
valid data, while Django services and application logic determine
business behavior.

## Deferred Architecture Decisions

The following architecture decisions are intentionally deferred beyond
Version 1: 
- Splitting the modular monolith into independently deployed
microservices. 
- Building a separate React or other single-page
application frontend. 
- Exposing a public REST or GraphQL API. 
-Introducing Redis or another caching layer. 
- Asynchronous processing
for email notifications and background jobs. 
- Saved Stay Planner
recommendations and personalization. 
- Multiple Whispering Pines
Hospitality properties. 
- External payment processing. 
- Advanced
observability and distributed tracing.
