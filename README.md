# RaceDay - Part 1

## Project overview

RaceDay is a planned full-stack web-based event management system for the South African road-running, walking and cycling community. Part 1 focuses on system planning and the SQL Server data model.

The API planning follows a resource-oriented REST approach and standard HTTP semantics (Fielding, 2000; IETF, 2022). Role requirements are stated explicitly because authentication and authorization are separate concerns, and ASP.NET Core supports role-based authorization (Microsoft, 2026a).

## User roles

**Organiser:** creates, edits and deletes events; manages event categories; captures participant results; and views event enrolments.

**Participant:** creates an account; browses events; enters an event by selecting a category; views own enrolments; and tracks personal results.

## Part 1 deliverables

The `docs` folder contains the required ERD, API endpoint plan and SQL Server script. It also includes a supporting design-rationale document.

- `RaceDay-ERD.png`
- `RaceDay-API-Endpoint-Plan.pdf`
- `RaceDay-API-Endpoint-Plan.md`
- `RaceDay-Database.sql`
- `RaceDay-Part1-Design-Rationale.pdf`

## ERD and database consistency

The six entities are Users, Events, Categories, EventCategories, Enrolments and Results. Primary and foreign keys maintain relationships, while UNIQUE and CHECK constraints add integrity rules (Microsoft, 2026b; Microsoft, 2026c). Event records also include an optional route-information URL. Live weather is treated as dynamic external information rather than a static database entity.

## SQL setup

Open `docs/RaceDay-Database.sql` in SQL Server Management Studio (SSMS) and execute it on a clean SQL Server instance. Confirm that all six tables are created and that the verification SELECT statements return the seeded records.

## CI/CD

The GitHub Actions workflow is stored in `.github/workflows/part1-validation.yml`. It validates the required Part 1 files and checks the ERD image, database entities/seed requirements, API coverage and PDF file headers.

Before submission, add the required screenshot of a successful green build: `docs/github-actions-green-build.png`.

## Video presentation

The assessment requires an unlisted YouTube video using the student's own voice. Replace `YOUTUBE_LINK_TO_ADD` with the actual Part 1 video URL. The video should walk through the planning documents, ERD decisions, endpoint plan and the live SQL script execution in SSMS.

## GitHub commit requirement

The assessment requires a minimum of 20 meaningful Part 1 commits using the student's own GitHub account. Commit messages must correspond to genuine work and should be created progressively.

## AI-use disclosure

The assessment brief requires disclosure when AI tools are used for planning, proofreading or coding. Any AI assistance used must therefore be disclosed accurately in the manner required by the institution.

## References

Fielding, R.T. (2000) *Architectural Styles and the Design of Network-based Software Architectures*. PhD dissertation. University of California, Irvine. Available at: https://ics.uci.edu/~fielding/pubs/dissertation/ (Accessed: 22 September 2026).

IETF (2022) *RFC 9110: HTTP Semantics*. Available at: https://www.rfc-editor.org/rfc/rfc9110.html (Accessed: 22 September 2026).

Microsoft (2026a) *Introduction to authorization in ASP.NET Core*. Microsoft Learn. Available at: https://learn.microsoft.com/en-us/aspnet/core/security/authorization/introduction (Accessed: 22 September 2026).

Microsoft (2026b) *Primary and foreign key constraints - SQL Server*. Microsoft Learn. Available at: https://learn.microsoft.com/en-us/sql/relational-databases/tables/primary-and-foreign-key-constraints (Accessed: 22 September 2026).

Microsoft (2026c) *Unique constraints and check constraints - SQL Server*. Microsoft Learn. Available at: https://learn.microsoft.com/en-us/sql/relational-databases/tables/unique-constraints-and-check-constraints (Accessed: 22 September 2026).
