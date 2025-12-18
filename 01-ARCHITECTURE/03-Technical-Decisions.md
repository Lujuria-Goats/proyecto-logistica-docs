---
title: Technical Decisions
description: Justification of technologies and design patterns.
---

# Technical Decisions

Record of architectural decisions and technology stack selection.

# Backend: C# .NET 8

## Justification:

### Outstanding Performance
.NET 8 introduces significant improvements in JIT and AOT compilation, positioning itself as one of the fastest frameworks for web APIs, surpassing many alternatives in TechEmpower benchmarks. This is crucial for handling high concurrency in logistics.

### Mature and Productive Ecosystem
Entity Framework Core greatly simplifies data access and migrations (PostgreSQL). Additionally, ASP.NET Core includes out-of-the-box Dependency Injection, Logging (Serilog), and Authentication (Identity), reducing configuration time ("batteries included").

### Robustness and Maintainability
C#'s static type system, along with Nullable Reference Types, prevents entire categories of bugs at compile time, facilitating long-term maintenance in a critical delivery system.

### Asynchronous Model (Async/Await)
Efficient handling of I/O bound operations (calls to Azure Vision, Cloudinary, RabbitMQ) without blocking server threads.
