# Generic Prompt — Generate an End-to-End Java Backend Application Structure
 
## Objective
Generate a complete end-to-end backend application structure for a modern Java service. The goal is to produce a production-friendly, maintainable starter codebase that includes all major layers, clear separation of concerns, and enough implementation detail for a team to extend safely.
 
## Primary Goal
Create a backend application that includes:
- Project structure
- Domain model
- API layer
- Service layer
- Persistence layer
- Configuration layer
- Security layer
- Exception handling
- Observability
- Testing setup
- Deployment artifacts
 
The output should be a strong starting point for a real backend application, not just a toy example.
 
## Technology Baseline
Use the following default stack unless otherwise specified:
- Java 17+
- Spring Boot 3+
- Maven
- REST APIs
- Spring Web
- Spring Data JPA or Spring Data JDBC
- PostgreSQL
- Liquibase or Flyway for database migrations
- Bean Validation
- OpenAPI / Swagger
- JUnit 5 + Mockito
- Testcontainers for integration tests
- Docker
 
If reactive architecture is explicitly requested, switch to:
- Spring WebFlux
- R2DBC
- Reactor types (Mono / Flux)
 
## Scope
Generate the full backend structure for a typical business application, including:
 
### 1. Project Structure
Create a clean package structure such as:
- config
- controller
- dto
- entity or domain
- repository
- service
- service.impl
- mapper
- exception
- security
- validation
- util
 
### 2. API Layer
Generate REST controllers with:
- CRUD endpoints where applicable
- Search/filter endpoints where useful
- Pagination support for list endpoints
- Consistent request/response DTOs
- Proper HTTP status codes
- Validation annotations
- OpenAPI annotations
 
### 3. Service Layer
Generate service interfaces and implementations that:
- Contain business logic
- Validate business rules
- Orchestrate repository calls
- Map between DTOs and entities
- Handle transactional boundaries where needed
 
### 4. Persistence Layer
Generate:
- Entities
- Repository interfaces
- Database migration scripts
- Relationships between entities
- Indexes and constraints
- Audit fields where appropriate
 
Use realistic database modeling, including:
- Primary keys
- Foreign keys
- Unique constraints
- Created/updated timestamps
- Soft delete support if useful
 
### 5. Security
Generate a basic but extensible security model:
- Spring Security configuration
- Authentication placeholder or JWT-based setup
- Authorization rules by endpoint or role
- Public vs protected endpoints
- Password encoding if auth is included
 
If authentication details are not provided, generate a clean placeholder structure with TODO comments rather than inventing unsafe logic.
 
### 6. Error Handling
Generate centralized exception handling with:
- Custom business exceptions
- Validation error responses
- Standard API error response format
- Global exception handler
 
Use a consistent error response such as:
json
{
  "timestamp": "2026-04-14T10:00:00Z",
  "status": 400,
  "error": "Bad Request",
  "message": "Validation failed",
  "path": "/api/resource",
  "traceId": "optional-correlation-id"
}

 
### 7. Validation
Include:
- Request validation using Bean Validation
- Custom validators where needed
- Business rule validation in service layer
 
### 8. Mapping
Use MapStruct or a clean manual mapper pattern to map:
- Request DTO -> Entity
- Entity -> Response DTO
- Update DTO -> existing Entity
 
### 9. Observability
Add:
- Health endpoint
- Metrics endpoint
- Structured logging
- Correlation ID support
- Basic actuator configuration
 
### 10. Testing
Generate:
- Unit tests for services
- Controller tests
- Repository integration tests
- Testcontainers-based DB tests
- Example test data builders or fixtures
 
### 11. Configuration
Generate:
- application.yml
- application-local.yml
- application-dev.yml
- application-prod.yml
 
Externalize:
- DB connection properties
- JWT/auth config
- Third-party API configs
- Feature flags
 
Use environment variables for secrets.
 
### 12. Deployment Artifacts
Generate:
- Dockerfile
- .dockerignore
- README with run instructions
- Example .env or environment variable documentation
 
## Design Principles
The generated backend must follow these principles:
- Clean architecture / layered structure
- Separation of concerns
- Readable and maintainable code
- Production-friendly defaults
- Extensible design
- Minimal duplication
- Consistent naming conventions
 
## Rules for Code Generation
- Do not generate placeholder code that is empty unless absolutely necessary
- Prefer complete starter implementations over stubs
- Use interfaces where helpful, but do not over-engineer
- Add TODO comments only where business-specific behavior is unknowable
- Keep naming generic and reusable unless domain terms are provided
- Avoid introducing unnecessary frameworks
- Keep code idiomatic for Spring Boot applications
 
## API Standards
Use the following conventions:
- Base path: /api/v1
- GET /resources for list
- GET /resources/{id} for detail
- POST /resources for create
- PUT /resources/{id} for full update
- PATCH /resources/{id} for partial update
- DELETE /resources/{id} for delete
- Support pagination using query params such as page, size, and sort
 
## Output Format
Generate the following:
1. Project directory structure
2. pom.xml
3. Main application class
4. Configuration classes
5. Domain/entity classes
6. DTO classes
7. Mapper classes
8. Repository interfaces
9. Service interfaces and implementations
10. Controller classes
11. Exception classes and global handler
12. Security configuration
13. Database migration files
14. Sample tests
15. Dockerfile
16. README
 
## Important Behavior
If the application domain is not provided:
- Use a simple example domain such as Product, Customer, Order, or Task Management
- Keep the structure generic enough to be reused
 
If the user provides a domain:
- Adapt entities, endpoints, services, and validation rules to that domain
 
If external integrations are requested:
- Create adapter interfaces and configuration scaffolding
- Do not invent exact third-party payloads unless specified
 
## Quality Expectations
The generated code should:
- Compile with minimal modification
- Follow standard Spring Boot best practices
- Be organized for real-world team development
- Be ready for iterative enhancement
 
## Final Instruction
Generate a complete backend starter structure for the requested application, with production-friendly organization and enough implementation detail that a developer can immediately start building features on top of it.