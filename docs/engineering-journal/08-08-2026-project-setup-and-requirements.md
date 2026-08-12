# August 8, 2026 — Project Setup and Requirements

## What I worked on
Today I:

- established the product vision
- made Version 1 scope decisions
- structured the repository
- initialized Git
- configured .gitattributes
- created the GitHub repository
- created the Kanban workflow
- started formal requirements analysis
- learned about authentication vs. authorization
- discovered the distinction between booking availability and operational room status

## Decisions I made
- Set the Version 1 cancellation cutoff to 48 hours before check-in.
- Decided Version 1 reservations can be viewed or canceled, but not modified after booking.
- Deferred housekeeping employee assignments to a later version while keeping room operational-status management in Version 1.
- Decided the Stay Planner will recommend room types rather than specific physical rooms. 

## What I Learned

### Authentication vs. Authorization

Authentication verifies **who you are**.

Authorization determines **what you are allowed to do**.

For Whispering Pines Lodge, a guest, staff member, and administrator may all authenticate through the application, but authorization determines which features and data each role is allowed to access.

### Reservation Availability vs. Operational Room Status

Reservation availability tracks whether a room can be booked for a requested future date range.

Operational room status tracks the current physical state of the room, such as whether it is ready, occupied, being cleaned, under maintenance, or out of service.

These need to be treated separately because a room can be unavailable operationally today while still being available for a future reservation.

## What confused me
Everything has been straightforward for me today.

## Bugs / blockers
- Initially received `src refspec main does not match any` when pushing to GitHub because the repository did not yet contain a commit. Creating the initial commit before pushing resolved the issue.

## What I would explain in an interview
I started Whispering Pines Lodge by defining the users and the product vision. From there, I decided what functionality belonged in Version 1 versus what should be saved for later versions. I documented our architectural decisions, established Git for version control, and began formalizing the application's requirements before writing any code.

## Next step
Complete and review the Version 1 functional requirements, then begin defining the application's non-functional requirements.