# August 20, 2026 --- Separation of Concerns

Stay Planner recommends a RoomType; Reservations handles the actual booking.

Instead of building one giant piece of code that recommends, checks availability, assigns rooms, calculates prices, and creates reservations, we're assigning responsibilities to appropriate modules.

That's exactly the kind of thing an interviewer might ask about:

"How did you decide how to structure your Django application?"