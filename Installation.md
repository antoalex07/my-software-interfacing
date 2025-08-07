How to Start
Open Visual Studio 2022 or 2025 (latest).

Ensure you’ve installed:

✅ .NET 8 SDK

✅ “.NET desktop development” and/or “.NET and web development” workloads

Create a new project and pick:

Console App → for headless backend

Worker Service → for background tasks or Windows services

Minimal WinForms app → if you still want a UI window

Choose .NET 8 as the target framework.

Create two projects:

A Windows Service (e.g., a Worker Service project using .NET 8)

A UI app (e.g., WinForms or WPF)

The service does all the background work.

The UI app communicates with the service (via file, database, named pipes, TCP, etc.)

Benefits:

Clean separation of responsibilities.

You can update the UI without touching the service.

Service runs without user login; UI runs only when needed.

1. Open Visual Studio
Click Create a new project

Search for and choose Worker Service

Name it something like MyBackgroundService
