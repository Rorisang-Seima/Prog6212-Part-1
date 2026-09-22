# RaceDay API Endpoint Plan - Part 1

## Purpose and design rationale

RaceDay is planned as a resource-oriented REST API. The HTTP methods in this plan follow standard HTTP semantics for retrieving, creating, updating and deleting resources (IETF, 2022). The resource-based route structure is consistent with the REST architectural style described by Fielding (2000).

The assessment requires two roles: Organiser and Participant. The endpoint plan therefore states the role required for each protected operation. Authentication identifies a user, while authorization determines which protected operations that identity is permitted to perform; ASP.NET Core supports role-based authorization for protected application operations (Microsoft, 2026a).

The endpoint set covers authentication, user profile, events, categories, event enrolments and results. EventCategories is included because event-category associations require their own data, including event-specific entry fee and participant limits.

## Endpoint specification

| HTTP Method | Route | Description | Role Required | Request Body | Expected Response |
|---|---|---|---|---|---|
| POST | /api/auth/register | Creates a new Participant account. Public registration does not allow a caller to assign an Organiser role. | Public | firstName, lastName, email, password, phoneNumber (optional) | 201 Created; 400 Bad Request; 409 Conflict |
| POST | /api/auth/login | Authenticates a registered user and returns an access token for protected requests. | Public | email, password | 200 OK; 400 Bad Request; 401 Unauthorized |
| GET | /api/users/me | Returns the authenticated user's own profile. | Authenticated | None | 200 OK; 401 Unauthorized |
| PUT | /api/users/me | Updates the authenticated user's own profile. | Authenticated | firstName, lastName, phoneNumber | 200 OK; 400 Bad Request; 401 Unauthorized |
| GET | /api/events | Returns available RaceDay events for browsing. | Public | None | 200 OK |
| GET | /api/events/{id} | Returns details for one RaceDay event. | Public | None | 200 OK; 404 Not Found |
| POST | /api/events | Creates an event owned by the authenticated Organiser. | Organiser | name, description, eventDate, location, registrationDeadline, imageUrl (optional), routeInfoUrl (optional) | 201 Created; 400 Bad Request; 401 Unauthorized; 403 Forbidden |
| PUT | /api/events/{id} | Updates an event managed by the authenticated Organiser. | Organiser | name, description, eventDate, location, registrationDeadline, imageUrl (optional), routeInfoUrl (optional) | 200 OK; 400 Bad Request; 403 Forbidden; 404 Not Found |
| DELETE | /api/events/{id} | Deletes an event managed by the authenticated Organiser when related enrolments do not prevent deletion. | Organiser | None | 204 No Content; 403 Forbidden; 404 Not Found; 409 Conflict |
| GET | /api/categories | Returns the available event categories. | Public | None | 200 OK |
| GET | /api/categories/{id} | Returns one event category. | Public | None | 200 OK; 404 Not Found |
| POST | /api/categories | Creates a reusable event category. | Organiser | name, description (optional) | 201 Created; 400 Bad Request; 403 Forbidden; 409 Conflict |
| PUT | /api/categories/{id} | Updates an existing event category. | Organiser | name, description (optional) | 200 OK; 400 Bad Request; 403 Forbidden; 404 Not Found; 409 Conflict |
| DELETE | /api/categories/{id} | Deletes a category that is not currently associated with an event. | Organiser | None | 204 No Content; 403 Forbidden; 404 Not Found; 409 Conflict |
| GET | /api/events/{eventId}/categories | Returns categories configured for an event. | Public | None | 200 OK; 404 Not Found |
| POST | /api/events/{eventId}/categories | Associates a category with an event and defines entry rules. | Organiser | categoryId, entryFee, maximumParticipants (optional) | 201 Created; 400 Bad Request; 403 Forbidden; 404 Not Found; 409 Conflict |
| PUT | /api/events/{eventId}/categories/{id} | Updates the event-specific category configuration. | Organiser | entryFee, maximumParticipants (optional) | 200 OK; 400 Bad Request; 403 Forbidden; 404 Not Found |
| DELETE | /api/events/{eventId}/categories/{id} | Removes a category association when no enrolments depend on it. | Organiser | None | 204 No Content; 403 Forbidden; 404 Not Found; 409 Conflict |
| POST | /api/enrolments | Enrols the authenticated Participant in an event category. | Participant | eventCategoryId | 201 Created; 400 Bad Request; 401 Unauthorized; 403 Forbidden; 404 Not Found; 409 Conflict |
| GET | /api/enrolments/my | Returns only the authenticated Participant's own enrolments. | Participant | None | 200 OK; 401 Unauthorized; 403 Forbidden |
| GET | /api/events/{eventId}/enrolments | Returns all enrolments for an event managed by the authenticated Organiser. | Organiser | None | 200 OK; 403 Forbidden; 404 Not Found |
| POST | /api/results | Records a result against a participant enrolment. | Organiser | enrolmentId, finishPosition (optional), finishTime (optional), resultStatus | 201 Created; 400 Bad Request; 403 Forbidden; 404 Not Found; 409 Conflict |
| GET | /api/results/my | Returns the authenticated Participant's personal results. | Participant | None | 200 OK; 401 Unauthorized; 403 Forbidden |
| PUT | /api/results/{id} | Updates a previously recorded result. | Organiser | finishPosition (optional), finishTime (optional), resultStatus | 200 OK; 400 Bad Request; 403 Forbidden; 404 Not Found |

## Database-design consistency

The ERD contains Users, Events, Categories, EventCategories, Enrolments and Results. Primary and foreign keys maintain the relationships between related entities. UNIQUE and CHECK constraints strengthen integrity by preventing duplicate values or restricting invalid values (Microsoft, 2026b; Microsoft, 2026c).

## References

Fielding, R.T. (2000) Architectural Styles and the Design of Network-based Software Architectures. PhD dissertation. University of California, Irvine. Available at: https://ics.uci.edu/~fielding/pubs/dissertation/ (Accessed: 22 September 2026).

IETF (2022) RFC 9110: HTTP Semantics. Available at: https://www.rfc-editor.org/rfc/rfc9110.html (Accessed: 22 September 2026).

Microsoft (2026a) Introduction to authorization in ASP.NET Core. Microsoft Learn. Available at: https://learn.microsoft.com/en-us/aspnet/core/security/authorization/introduction (Accessed: 22 September 2026).

Microsoft (2026b) Primary and foreign key constraints - SQL Server. Microsoft Learn. Available at: https://learn.microsoft.com/en-us/sql/relational-databases/tables/primary-and-foreign-key-constraints (Accessed: 22 September 2026).

Microsoft (2026c) Unique constraints and check constraints - SQL Server. Microsoft Learn. Available at: https://learn.microsoft.com/en-us/sql/relational-databases/tables/unique-constraints-and-check-constraints (Accessed: 22 September 2026).
