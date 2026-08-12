# Whispering Pines Lodge — Version 1 Data Model

## Purpose

This document defines the initial domain entities, relationships, constraints, and data rules for Version 1 of Whispering Pines Lodge.

The goal is to preserve reservation history, prevent invalid bookings, support the Stay Planner, and keep the data model simple enough to maintain and extend.

---

## Core Entities

### User

Represents an authenticated person using the application.

#### Fields

* `id` — Primary key
* `first_name` — Required
* `last_name` — Required
* `email` — Required, unique
* `password/authentication_data` — Required and managed through Django authentication
* `role` — Required, restricted to approved values

#### Allowed Roles

* `GUEST`
* `STAFF`
* `ADMIN`

#### Rules

* Two users may not share the same email address.
* User accounts with reservation history should be deactivated rather than physically deleted.
* Authorization determines which application features a user may access.

---

### RoomType

Represents a category of accommodation offered to guests.

Examples include:

* Woodland Room
* Summit Suite
* Whispering Pines Cabin

#### Fields

* `id` — Primary key
* `name` — Required
* `description` — Optional
* `maximum_occupancy` — Required
* `bed_configuration` — Required
* `base_rate` — Required

#### Rules

* A RoomType may describe many physical Rooms.
* RoomTypes referenced by physical Rooms should not be deleted.
* A RoomType may instead be marked inactive in a future implementation if it is no longer offered.

---

### Room

Represents a specific physical accommodation.

Examples:

* Room 204
* Cabin 3

#### Fields

* `id` — Primary key
* `room_number_or_name` — Required, unique
* `room_type_id` — Required foreign key to `RoomType`
* `operational_status` — Required, restricted to approved values

#### Allowed Operational Status Values

* `READY`
* `OCCUPIED`
* `CLEANING`
* `MAINTENANCE`
* `OUT_OF_SERVICE`

#### Rules

* Each Room belongs to exactly one RoomType.
* One RoomType may contain many Rooms.
* A Room may have many Reservations over its lifetime.
* A Room with reservation history should not be physically deleted.
* Rooms that are no longer usable should be marked inactive or `OUT_OF_SERVICE`.

---

### Amenity

Represents a feature that may be associated with one or more RoomTypes.

Examples include:

* Fireplace
* Wi-Fi
* Hot Tub
* Kitchen
* Pet Friendly
* Mountain View

#### Fields

* `id` — Primary key
* `name` — Required, unique
* `description` — Optional

#### Rules

* An Amenity may belong to many RoomTypes.
* A RoomType may contain many Amenities.
* Amenities should be deactivated rather than physically deleted when historical relationships should be preserved.

---

### RoomTypeAmenity

Represents the many-to-many relationship between RoomType and Amenity.

#### Fields

* `room_type_id` — Required foreign key to `RoomType`
* `amenity_id` — Required foreign key to `Amenity`

#### Rules

* The pair `(room_type_id, amenity_id)` must be unique.
* Duplicate RoomType/Amenity relationships are not allowed.

Example:

`Summit Suite + Fireplace`

may exist once, but the same relationship may not appear twice.

---

### Reservation

Represents a completed booking associated with one authenticated user and one physical room.

#### Fields

* `id` — Primary key
* `confirmation_number` — Required, unique
* `user_id` — Required foreign key to `User`
* `room_id` — Required foreign key to `Room`
* `check_in_date` — Required
* `check_out_date` — Required
* `adults` — Required
* `children` — Required; zero is a valid value
* `nightly_rate` — Required
* `total` — Required
* `status` — Required, restricted to approved values
* `created_at` — Required
* `cancelled_at` — Optional

#### Allowed Reservation Status Values

* `CONFIRMED`
* `CANCELLED`
* `COMPLETED`

#### Rules

* Each Reservation belongs to exactly one User.
* Each Reservation references exactly one physical Room in Version 1.
* A User may have many Reservations.
* A Room may have many Reservations over time.
* The system shall prevent overlapping active Reservations for the same physical Room.
* `check_out_date` must occur after `check_in_date`.
* The number of guests may not exceed the maximum occupancy of the RoomType associated with the selected Room.
* Every confirmed Reservation must have a unique confirmation number.
* A cancelled Reservation should remain stored with `status = CANCELLED`.
* `cancelled_at` should only contain a value when a Reservation has been cancelled.
* Reservation creation must behave as an all-or-nothing transaction.

---

## Pricing Behavior

`RoomType.base_rate` represents the current standard rate for that accommodation type.

`Reservation.nightly_rate` stores the nightly rate agreed to at the time the Reservation was created.

This intentionally preserves historical pricing.

Example:

If a Woodland Room is booked at `$199.00` per night and the base rate later changes to `$229.00`, the existing Reservation should continue to show the original `$199.00` nightly rate.

A more advanced pricing model, including seasonal rates and refundable/non-refundable rate plans, is deferred to a future version.

---

## Stay Planner Data

Stay Planner recommendations will not be stored in Version 1.

The application will calculate recommendations using guest input, RoomType capacity, available dates, and accommodation preferences.

Recommendation history and personalization may be introduced in a later version.

---

## Deletion and Historical Data Policy

Whispering Pines Lodge should preserve historical business data wherever possible.

### User

Deactivate rather than delete when reservation history exists.

### Room

Protect against deletion when referenced by Reservations.

### RoomType

Protect against deletion when referenced by Rooms.

### Amenity

Prefer deactivation when historical relationships should remain available.

### Reservation

Do not physically delete normal canceled Reservations. Update the status to `CANCELLED` and record `cancelled_at`.

---

## Relationship Summary

* One User may have many Reservations.
* Each Reservation belongs to one User.
* One RoomType may have many Rooms.
* Each Room belongs to one RoomType.
* One Room may have many Reservations over its lifetime.
* Each Reservation references one Room.
* RoomType and Amenity have a many-to-many relationship through RoomTypeAmenity.

---

## Version 1 Entity List

* User
* RoomType
* Room
* Amenity
* RoomTypeAmenity
* Reservation

---

## Deferred Data Features

The following are intentionally outside the Version 1 data model:

* Housekeeping employee assignments
* Reservation modification history
* Saved Stay Planner recommendations
* Seasonal pricing
* Refundable and non-refundable rate plans
* Cancellation reasons and refund records
* Room-status history
* Multiple Whispering Pines properties