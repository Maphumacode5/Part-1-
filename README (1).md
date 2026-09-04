# RaceDay - Part 1: System Planning and Database

**Module:** PROG6212/w - Programming 2B
**Part:** 1 of 3 (System Planning and Database) - 100 Marks

## System Description

RaceDay is a full-stack event management platform built for the South African road running, walking, and cycling community. It replaces the paper-based registration and spreadsheet-driven workflows that many local events (park runs, charity walks, community races) still rely on.

The platform lets **Event Organisers** create and manage events and categories, and capture participant results, while **Participants** can browse upcoming events, enter events by selecting a category, and track their personal race history.

This Part 1 submission covers the planning phase only: no application code is written yet. The goal is to design the data model and API surface before any implementation begins, so that Part 2 (the REST API) and Part 3 (the MVC front end) can be built directly against an approved plan.

## Roles

- **Organiser** - creates, edits, and deletes events; manages event categories; captures participant results; views all enrolments for their own events.
- **Participant** - registers an account, browses events, enrols in an event under a chosen category, views their own enrolments, and tracks their personal results.

Role-based access will be enforced at the API level in Part 2 and reflected in the MVC interface in Part 3.

## Contents of /docs

| File | Description |
|---|---|
| `RaceDay_ERD.png` | Entity Relationship Diagram covering all 6 entities (Users, EventTypes, Events, Categories, Enrolments, Results), with primary keys, foreign keys, and cardinality. |
| `RaceDay_API_Endpoint_Plan.md` | Full endpoint plan covering Authentication, User Profile, Events, Categories, Event Enrolments, and Results. |
| `RaceDay_Database_Script.sql` | SQL Server script that creates the full schema (matching the ERD exactly) and seeds it with realistic sample data - 2 Organisers, 2 Participants, 3 Events, categories per event, and sample enrolments/results. |

## Design Notes

- **EventTypes** was added as its own lookup table (rather than a free-text column on Events) to normalise the Run/Walk/Cycle classification and keep the schema clean for filtering.
- **Enrolments** has a `UNIQUE (ParticipantId, EventId)` constraint so a Participant cannot enrol in the same event twice.
- **Results** has a one-to-one relationship with **Enrolments** (`UNIQUE` foreign key), since a result only exists once a Participant has enrolled and the event has concluded.
- Passwords are stored as hashes (`PasswordHash` column); actual hashing logic will be implemented with the authentication code in Part 2.

## How the SQL Script was Tested

The script was written to run cleanly on a fresh SQL Server instance in SSMS: it creates the `RaceDayDB` database if it doesn't exist, drops any existing tables in dependency order (so it is safely re-runnable during testing), recreates all six tables with their constraints, and then seeds sample data. Verification `SELECT` statements are included at the bottom (commented out) and were run manually to confirm the seed data loaded correctly.

## Next Steps

- **Part 2** will implement the REST API in ASP.NET Core against this plan, connect it to the database via EF Core, add authentication/session management and role enforcement, and cover the endpoints with unit tests.
- **Part 3** will build the MVC front end that consumes the Part 2 API, add Azure Blob Storage for event banners and profile pictures, and containerise the application with Docker.

## AI Disclosure

AI tools were used to assist with planning structure, drafting the SQL script, and formatting this documentation. All design decisions (entity structure, relationships, endpoint design) were reviewed and understood before submission.

## CI/CD Screenshot

*(Insert screenshot of the green GitHub Actions build here before submission.)*

## Video Link

*(Insert unlisted YouTube video link here before submission.)*
