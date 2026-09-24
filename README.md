C# Monolithic Clean Architecture
=============================

This project follows the **Clean Architecture** principles, organizing code into separate layers with clear responsibilities and dependency boundaries. The architecture is monolithic (single deployable unit) while maintaining separation of concerns.

## Architecture Layers

### 1. Domain
The innermost layer containing the core business logic.
- **Purpose**: Business entities, value objects, domain events, and domain interfaces
- **Key Concepts**:
  - Entities: Core business objects with identity
  - Value Objects: Immutable objects defined by their attributes
  - Domain Events: Business events that occurred in the domain
  - Domain Interfaces: Abstractions for external dependencies (e.g., repositories)
- **Dependencies**: None (pure business logic)
- **Project**: `*.Domain`

### 2. Application / Services
The application layer orchestrates business workflows.
- **Purpose**: Application services, command/query handlers, and use case logic
- **Key Concepts**:
  - Application Services: Coordinate use cases and manage transactions
  - Commands & Queries: Define input/output models for operations
  - DTOs: Data transfer objects for layer communication
  - Business Orchestration: Workflow coordination between domain objects
- **Dependencies**: Domain layer
- **Project**: `*.Services` or `*.Application`

### 3. Infrastructure (Infra)
The outer layer handling technical concerns and external integrations.
- **Purpose**: Data access, external service clients, email/SMS, file storage, caching, and technical implementations
- **Key Concepts**:
  - Entity Framework / ORM: Database persistence
  - Repository Implementations: Concrete implementations of domain repository interfaces
  - External Services: HTTP clients, payment gateways, messaging queues
  - Unit of Work: Transaction management
  - Caching: Redis, in-memory cache implementations
- **Dependencies**: Domain and Application layers
- **Project**: `*.Infra` or `*.Infrastructure`

### 4. API (Presentation)
The entry point of the application.
- **Purpose**: REST API controllers, middleware, request/response handling, and HTTP pipeline configuration
- **Key Concepts**:
  - Controllers: Handle HTTP requests and responses
  - Middleware: Authentication, logging, error handling
  - Filters: Validation, authorization, and action filters
  - Swagger/OpenAPI: API documentation
  - Dependency Injection: Wiring up services at startup
- **Dependencies**: Application, Domain, and Infrastructure layers
- **Project**: `*.API` or `*.WebApi`

### 5. Resources
Supporting configuration and shared resources.
- **Purpose**: Configuration files, resource files, constants, enums, and shared utilities
- **Key Concepts**:
  - `appsettings.json`: Environment-specific configuration
  - Logging configuration: Serilog, logging levels
  - Shared Models: Common enums, constants, extension methods
  - Migration scripts: Database schema versioning
  - Static Resources: Images, templates, localization files
- **Dependencies**: Available to all layers
- **Project**: `*.Resources` or as configuration files

## Dependency Flow

```
+-------------------+
|      API          |
+-------------------+
        |
        v
+-------------------+
|    Services       |
+-------------------+
        |
        v
+-------------------+
|     Domain        |
+-------------------+
        ^
        |
+-------------------+
|     Infra         |
+-------------------+
        |
        v
+-------------------+
|   Resources       |
+-------------------+
```

**Key Rule**: Inner layers must never depend on outer layers. Dependencies always point inward.

## Layer Interaction Example

1. **API** receives an HTTP request and maps it to a service command
2. **Services** orchestrates the use case, calling domain entities and domain interfaces
3. **Domain** contains the core business logic and rules
4. **Infra** provides concrete implementations of domain interfaces (e.g., database persistence)
5. **Resources** supplies configuration and shared utilities across all layers

## Project Structure

```
/src
  /ProjectName.Domain
    /Entities
    /ValueObjects
    /Events
    /Interfaces
    /Enums
  /ProjectName.Services
    /Commands
    /Queries
    /DTOs
    /Interfaces
  /ProjectName.Infra
    /Data
    /Repositories
    /ExternalServices
    /UnitOfWork
  /ProjectName.API
    /Controllers
    /Middleware
    /Filters
    /Configurations
  /ProjectName.Resources
    /Configuration
    /Constants
    /Extensions
```

## Setup

### Prerequisites
- .NET 8 SDK or later
- SQL Server or preferred database
- Docker (optional, for containerized deployment)

### Building and Running

```bash
# Restore dependencies
dotnet restore

# Build the solution
dotnet build --configuration Release

# Run migrations
dotnet ef database update --project src/ProjectName.Infra

# Run the API
dotnet run --project src/ProjectName.API
```

### Running Tests

```bash
dotnet test
```

## Technologies

- **C#** - Primary programming language
- **ASP.NET Core** - Web framework for API layer
- **Entity Framework Core** - ORM for data access
- **MediatR** - Mediator pattern for command/query handling
- **FluentValidation** - Input validation
- **AutoMapper** - Object-to-object mapping
- **Swagger/OpenAPI** - API documentation
