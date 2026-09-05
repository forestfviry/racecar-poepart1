# racecar-poepart1
RaceDay is a road-race event management system. Organisers can create events (e.g. the Joburg City Run or the Soweto Marathon) with multiple distance categories, participants can enrol in those categories, and results are recorded once a race is completed.

This repository covers Part 1 of the project: the planning and database design phase. Application code is out of scope until Part 2 — the goal here is a solid, well-normalised data foundation and a clear API contract to build against.
Enrolments is the junction table resolving the many-to-many relationship between Users and Categories, with a unique constraint on (UserID, CategoryID) so a participant can't double-enrol in the same category. Results carries a unique constraint on EnrolmentID to enforce the 1:1 link.

Tech Stack
Database: SQL Server (developed and tested in SSMS)
Planned application layer: see the API plan document
Setup Instructions
Open racecar-schema-poepart1.sql in SQL Server Management Studio.
Execute the script — it creates the RaceDayDB database, all six tables (Roles, Users, Events, Categories, Enrolments, Results), their constraints, and seed data.
Re-running the script is safe — it drops and recreates the tables first (dev convenience, not intended for production use).
Verifying the Data

A few quick checks after running the script:

sql
SELECT * FROM dbo.Users;
SELECT * FROM dbo.Events;

-- Confirm the join chain works end-to-end
SELECT u.FullName, e.EventName, c.CategoryName, r.FinishTime, r.Position
FROM dbo.Enrolments en
JOIN dbo.Users u ON u.UserID = en.UserID
JOIN dbo.Categories c ON c.CategoryID = en.CategoryID
JOIN dbo.Events e ON e.EventID = c.EventID
LEFT JOIN dbo.Results r ON r.EnrolmentID = en.EnrolmentID;
YOUTUBE
https://youtu.be/dCiUI-qNyXs
