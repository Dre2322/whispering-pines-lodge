## Performance: 

At least 95% of normal page requests shall complete within 2 seconds while the application is serving up to 50 concurrent users under expected Version 1 operating conditions.

## Security:

- User passwords shall never be stored as plaintext and shall use Django's password-hashing mechanisms.
- Staff and administrative functionality shall only be accessible to authorized users.
- Production traffic containing authentication or guest information shall use HTTPS.

## Reliability: 

Booking should behave as an all-or-nothing operation. Either the reservation succeeds completely, or it fails without leaving an incomplete reservation behind. 

## Accessibility:

- Core guest workflows shall be operable using keyboard navigation.
- Forms shall provide appropriate labels and accessible validation feedback.
- Images conveying meaningful content shall include appropriate alternative text.
- The application shall target WCAG 2.2 Level AA where applicable.

## Usability: 

A guest should be able to complete the Stay Planner without requiring instructions outside the application. 

## Responsive Design: 

Core guest workflows shall remain usable on supported mobile, tablet, and desktop viewport sizes. 

## Maintainability: 

The application shall follow established Django project conventions, organize functionality into clearly separated components, include automated tests for critical business logic, and maintain documentation for significant architecture and business-rule decisions.

## Data Integrity: 

- A reservation's check-out date must occur after its check-in date.
- The number of guests on a reservation must not exceed the maximum occupancy of the selected accommodation.
- The system shall prevent overlapping active reservations for the same physical room.
- Every confirmed reservation shall reference a valid guest and physical room.
- Every confirmed reservation shall have a unique confirmation identifier.

