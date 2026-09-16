**DairyFlow**
**A .NET 8 microservices platform for dairy milk procurement — built to model a real-world dairy collection workflow from farmer onboarding through milk collection, rate calculation, and reporting.**

**Overview**
DairyFlow is composed of four independently deployable services sitting behind an API Gateway, each following clean architecture with its own database, migrations, and bounded context.

**Service	Responsibility**
Auth Service	User registration, login, JWT issuance, forgot/reset password
Farmer Service	Farmer onboarding, profile & address management, dashboard summary
Collection Service	Milk collection entries, rate calculation, daily/summary reports, dashboard
Rate Service	Milk rate slabs (fat/SNF based), rate matrix, Excel rate-chart export
API Gateway	Single entry point routing requests to downstream services (Ocelot)

**Architecture**
**<img width="1408" height="768" alt="Arch digram" src="https://github.com/user-attachments/assets/540bcc85-bb95-44f5-b4d0-ca680b211e59" />**



**Each service:**

Followed Clean Architecture — Domain / Application / Infrastructure layers
Owns its own SQL Server database with EF Core code-first migrations
Uses the repository pattern for data access
Validates requests with FluentValidation
Shares cross-cutting concerns (global exception middleware, standard error/paged-response contracts) via a common BuildingBlocks library
Logs with Serilog
Documents its API with Swagger / OpenAPI
Collection Service talks to Farmer Service and Rate Service over typed HttpClients wrapped with Polly retry/timeout policies, so a slow or unavailable downstream service degrades gracefully instead of failing the whole request.

**Tech Stack**
Backend: ASP.NET Core 8, C#, Entity Framework Core, Ocelot API Gateway
Auth: JWT Bearer authentication
Validation: FluentValidation
Resilience: Polly (retry, timeout)
Logging: Serilog
Reporting: ClosedXML (Excel export)
Database: SQL Server
API Docs: Swagger / Swashbuckle
Services & Ports
Service	Route Prefix	
API Gateway
Auth Service	/api/v1/Auth	
Farmer Service	/api/v1/farmers	
Collection Service	/api/v1/milk-collections	
Rate Service	/api/V1/milk-rates	


**Key Features**
**User registration/login with JWT issuance and forgot/reset-password flow
Farmer CRUD with address management and dashboard summary
Milk collection entry with automatic rate calculation (fat/SNF based)
Milk rate slab management with a rate matrix and Excel export
Paginated collection listings, daily reports, and summary dashboards
Centralized routing and cross-cutting error handling via the API Gateway
Getting Started
Prerequisites
.NET 8 SDK
SQL Server (LocalDB or full instance)
**

All requests can then be made through the gateway at https://localhost:7186.

**Project Structure**
**DairyFlow/
├── DairyFlow.ApiGateway/       # Ocelot API Gateway
├── DairyFlow/                  # Auth Service
├── DairyFlow.FarmerService/    # Farmer management
├── DairyFlow.CollectionService/# Milk collection & rate calculation
├── DairyFlow.RateService/      # Milk rate slabs & Excel export
└── DairyFlow.BuildingBlocks/   # Shared middleware, models, exceptions**

Roadmap
[ ] Angular front-end client
[ ] Docker Compose for local orchestration
[ ] Unit test coverage (xUnit)
[ ] Centralized authentication via API Gateway (currently per-service)
**Author
Ajinath Sawant**
