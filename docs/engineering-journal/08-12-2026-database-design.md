1. ### What was the biggest thing you learned while designing the database?

The biggest thing I learned while designing the database was how relationships work between entities. I also developed a better understanding of the overall database design process and how you can work from business requirements toward an actual data model.

2. ### Why did we separate Room from RoomType?

We separated Room from RoomType because each physical room needs its own identity and operational status. At the same time, RoomType stores the information shared by rooms of the same type, such as the description, maximum occupancy, bed configuration, and base rate.

3. ### If an interviewer asked, “How did you approach designing the database for Whispering Pines?”, how would you explain your process?

I started by identifying the important nouns in the business domain, such as users, reservations, rooms, room types, and amenities. From there, I determined which concepts needed their own entities and identified the relationships between them. I then worked through primary keys, foreign keys, uniqueness constraints, required and optional fields, and how deletion should affect related data. Once I had those decisions documented, I used them to construct an ERD showing the Version 1 database model.

