# ADR-005: Stay Planner Recommends Room Types but Does Not Create Reservations

The rationale would be that recommendation logic and booking logic serve different responsibilities. Keeping them separate makes each module easier to test, reason about, and change later.