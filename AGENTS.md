# Identity Service Guidelines

## Project Overview
Identity service owns user identities, profile preferences, entitlements, roles, and account access relationships for the Shiya platform. It also hosts JWT configuration for validating access across the API.

## Project Structure & Module Organization
- Use DDD layering: `Domain/`, `Application/`, `Infrastructure/`, `Presentation/` (align with `custody-service`).
- Keep controllers thin; move business flows into managers/services and storage into repositories.

## Build, Test, and Development Commands
- `npm run start` (dotnet watch on http://localhost:5203)
- `npm run build`
- `npm run test`
- `npm run format`

## Coding Style & Naming Conventions
- Follow `.editorconfig` and `dotnet format`.
- Use clear, domain-aligned names; avoid abbreviations.
- Order methods alphabetically within classes and interfaces.
- Prefer explicit types over `var`.
- Sort public members alphabetically, then private fields alphabetically (private fields must be prefixed with `_`).
- Entitlement naming: `{ProjectName}{AppName}{Feature}{Subfeature if applicable}{Viewer/Editor or custom action name}`.
- Centralize DTO mapping in mappers/factories if DTOs are introduced.

## Configuration & Data
- Database: `appsettings.json` -> `ConnectionStrings:Default`.
- JWT: `Jwt:Issuer`, `Jwt:Audience`, `Jwt:Key` (minimum 32 chars).
- Seed data runs on startup via `Infrastructure/Persistence/Seed/DatabaseInitializer.cs` when no users exist.

## Docs
- `README.md` provides overview and runbook.
- `docs/` contains API, data model, and seed references.
- Maintain `docs/api-flows.md` with mermaid controller/data-source flow diagrams; update when controllers, routes, or data sources change.

## Codex Implementation Guide
### Frontend (TypeScript & Angular)
- Keep import blocks alpha-sorted and grouped by source (Angular, third-party, workspace aliases, relative).
- Use workspace path aliases for intra-app code; avoid deep relative paths.
- Build new UI using Angular standalone components and declare dependencies in the `imports` array.
- Scope state management to the feature folder (signals/stores/controllers) and expose read-only accessors to components.

### State & Data Flow
- Feature routes lazy-load a standalone component that wires feature-level services, stores, and controllers.
- Controllers orchestrate async work and update the feature store; components subscribe via computed signals or observable bridges.
- Mocked data and fallbacks belong in service layers so components stay declarative.

### Backend (API & Domain)
- Layer features using DDD: controller -> manager/service -> domain model -> repository.
- Keep controllers thin and return DTOs defined in the feature `Dtos/` namespace.
- Managers encapsulate business flow and compose repository calls or external integrations.
- Domain entities live under `Domain/`; repositories live under `Infrastructure/Persistence`.
- Feature configuration belongs in strongly typed `Options/` registered via DI.
- Cross-service calls should use typed resource clients in `Infrastructure/Services` (via `HttpClientFactory`).
- Centralize DTO mapping in mappers or factory methods.
- Tests should target managers and repositories using domain language.

### UI Conventions
- Use `FeatureShellComponent` (user app) and `FeatureHeaderComponent` (admin app) for consistent headers.
- For dashboards, track `lastSynced` in stores and surface it in subtitles.
- Keep navigation metadata in each app's `shared/navigation/nav-links.ts`.

### UI Theming
- Use shared tokens in `styles/theme/tokens.css` for palette, spacing, typography, motion, and utilities.
- Prefer utility classes like `surface-card`, `glass-panel`, `grid-responsive`, and `motion-fade-in`.
- Respect reduced-motion; use token-based animation durations.
- Wrap page bodies with `layout-container` and use `grid-responsive` for responsive cards.
