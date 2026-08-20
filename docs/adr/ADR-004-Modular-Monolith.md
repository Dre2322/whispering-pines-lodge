# ADR-004: Use a Modular Monolith for Version 1

A modular monolith makes sense for Version 1 because Whispering Pines is still a relatively small application, and splitting it into microservices would add unnecessary deployment, networking, monitoring, and data-consistency complexity. A modular monolith still lets us separate concerns cleanly inside Django. If Whispering Pines Hospitality later expands into larger independent products or services, we can reevaluate whether some modules should become separate services.
