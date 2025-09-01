# Authorization Server (Tutorial - Ticket App like Jira)

This repository is the OAuth2 Authorization Server for a tutorial series building a Jira-like ticket application using Spring Boot microservices and Angular. The goal is to document each step as the project evolves.

Current local date/time: 2025-09-01 14:37

## Overview
- Service name: authorizationserver
- Purpose: Will act as the central OAuth2/OIDC Authorization Server for the system (issuing access/refresh tokens, managing clients and users, and supporting flows suitable for an Angular frontend and other microservices).
- Current status: Project scaffolded with dependencies; no custom security configuration yet. App starts but does not expose configured OAuth2 Authorization Server endpoints until we add configuration.

## Tech Stack
- Java 21
- Spring Boot 3.4.4
- Spring Authorization Server (via spring-boot-starter-oauth2-authorization-server)
- Spring Web (MVC)
- Spring Data JPA
- Bean Validation (Jakarta Validation)
- Thymeleaf (for potential login/consent UIs)
- Spring Cloud Netflix Eureka Client (service discovery)
- PostgreSQL driver (runtime)
- Lombok (compile-time only)
- TOTP (dev.samstevens.totp) for MFA later
- YAUAA (User-Agent parser) for device/agent insights later

## Project Structure
- src/main/java/io/glebtanaka/authorizationserver/Application.java: Spring Boot entrypoint.
- src/main/resources/application.properties: Currently only sets `spring.application.name=authorizationserver`.
- src/test/java/.../AuthorizationserverApplicationTests.java: Basic context load test.
- HELP.md: Auto-generated references.
- pom.xml: Declares the dependencies listed above.

## Current Capabilities (as of now)
- Application boots as a Spring Boot app.
- No explicit Authorization Server configuration beans yet (no registered client, keys, login, or persistence). Default endpoints are not wired until we add configuration (as per Spring Authorization Server docs).
- No database schema or entities yet; JPA is unused for the moment.
- Eureka Client dependency is present, but discovery properties are not configured. Registration will be added later when the Discovery Server is available.

## Prerequisites
- Java 21 (ensure `java -version` reports a 21.x runtime)
- Maven Wrapper included (no need to install Maven separately)
- Optional (future steps): PostgreSQL instance

## How to Run Locally (Windows / PowerShell)
1. From the project root, run:
   - `./mvnw.cmd spring-boot:run` (PowerShell or CMD)
   - Or build a jar: `./mvnw.cmd clean package` then run: `java -jar target/authorizationserver-0.0.1-SNAPSHOT.jar`
2. The app will start on the default port 8080 unless overridden later.
3. Since no endpoints are configured yet, visiting http://localhost:8080 will return a basic response or 404 for missing routes. We will add endpoints and UIs in upcoming steps.

## Configuration
- File: `src/main/resources/application.properties`
- Currently:
  - `spring.application.name=authorizationserver`
- As we progress, we will add:
  - Server port, logging levels
  - Authorization Server settings (issuer, endpoints)
  - Registered clients
  - Database connection (PostgreSQL) and JPA properties
  - Eureka client configuration

## Database
- Driver dependency is present, but no schema or connection properties are set yet.
- Future steps will introduce:
  - `spring.datasource.*` properties
  - Flyway/Liquibase (optional) for schema migration
  - Entities for users, consents, authorization codes/tokens, etc.

## Discovery (Eureka)
- Dependency added but not configured.
- Later we will set:
  - `eureka.client.serviceUrl.defaultZone`
  - `eureka.instance.*` metadata if needed

## Security & OAuth2 Roadmap
Planned features to be added step-by-step:
1. Basic Authorization Server bootstrap
   - Minimal configuration beans for Spring Authorization Server
   - In-memory registered client for initial testing
   - Generate/define signing keys (JWK)
2. Login & Consent
   - Simple login page (Thymeleaf) and consent screen
   - Wire default security filters
3. Persistence
   - Switch registered clients and authorizations to database (JPA)
   - Add schema migrations
4. User management
   - User entity, password hashing, role/authority model
   - Admin endpoints for managing users/clients (secured)
5. MFA (TOTP)
   - Integrate `dev.samstevens.totp` for 2FA
6. Device/User-Agent insights
   - Use YAUAA to log/track user agent details
7. Integration with other microservices
   - Resource servers validate JWTs
   - Eureka registration and service-to-service auth
8. Production hardening
   - Externalized configuration
   - CORS, CSRF strategies for SPA
   - HTTPS, cookie security, headers

## Testing
- Current test: context loads only.
- We will add:
  - Unit tests for security config and services
  - Integration tests for auth flows (authorization code, client credentials)

## Known Gaps (to be addressed next)
- No Authorization Server beans (no endpoints yet)
- No JWK source or key material
- No registered clients
- No database configuration
- No Eureka client configuration
- No login/consent UI

## How to Follow Along
This README will be updated at each tutorial step. Use the checklist below to track progress.

### Tutorial Progress Checklist
- [ ] Initialize Spring Boot project and dependencies (this repo)
- [ ] Add minimal Spring Authorization Server configuration (issuer, JWK)
- [ ] Add in-memory registered client for initial auth-code flow
- [ ] Create login and consent pages (Thymeleaf)
- [ ] Issue and validate JWT access tokens
- [ ] Configure PostgreSQL, JPA, and migrations
- [ ] Persist clients, authorizations, and consents
- [ ] Add user entity and password-based login
- [ ] Add TOTP MFA for high-privilege operations
- [ ] Integrate YAUAA for user-agent/device insights
- [ ] Register with Eureka and test service discovery
- [ ] Secure other services as resource servers
- [ ] Add admin endpoints and UI for managing users/clients
- [ ] Add end-to-end tests for OAuth2 flows
- [ ] Production hardening (HTTPS, headers, CORS/CSRF)

## References
- Spring Authorization Server (Boot 3.4 docs): https://docs.spring.io/spring-boot/3.4.4/reference/web/spring-security.html#web.security.oauth2.authorization-server
- Spring Authorization Server project: https://spring.io/projects/spring-authorization-server
- Eureka Client docs: https://docs.spring.io/spring-cloud-netflix/reference/spring-cloud-netflix.html#_service_discovery_eureka_clients
- Spring Boot Maven Plugin: https://docs.spring.io/spring-boot/3.4.4/maven-plugin
