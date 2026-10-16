# RaceDay

RaceDay is a web platform designed for managing South African running, walking, and cycling events. Event organizers can schedule events, set up categories, and record outcomes, while participants can search for events, register, and review their race results.

## Part 2: RESTful API

Developed using ASP.NET Core Web API (.NET 10), EF Core (Code-First approach), SQL Server, and custom session authentication.

### User Roles

- **Organiser**: Manages owned events, manages event categories, monitors event registrations, and logs participant results.
- **Participant**: Registers an account, explores available events, signs up for race categories, and views personal registrations and performance.

Security policies are enforced directly at the API level (unauthenticated requests return 401, while unauthorized role or ownership access yields 403).

### Setup and Running locally

1. Ensure .NET 10 SDK and Visual Studio 2026 are installed on your machine.
2. Clone this repository.
3. Execute the project using the following commands:

cd RaceDay.Api
dotnet run --launch-profile https


4. The application automatically initializes the `RaceDayApi` database on startup via EF Core migrations.
5. Access interactive API documentation at `https://localhost:7186/swagger`.

### Executing Tests

To run the automated unit tests, navigate to the test folder and execute: 

cd RaceDay.Api.Tests
dotnet test

### Continuous Integration

Automated testing is configured using GitHub Actions (`.github/workflows/ci.yml`), which builds the solution and executes tests upon every code commit.

![CI Status](docs/ci-green-build.png)

### Design Adjustments from Part 1

- Database tables are named using plural form (e.g., Users, Events) to avoid conflicts with reserved T-SQL keywords.
- Part 2 uses the `RaceDayApi` database generated via EF Core migrations, maintaining structural parity with Part 1's `RaceDay` database.
- Executing `DELETE /api/events/{id}` responds with 409 Conflict if active enrolments exist for the target event.
- The `GET /api/events` endpoint includes support for query parameters (`eventTypeId`, `search`, and `upcomingOnly`).
