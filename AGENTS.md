AGENT / OPERATIONAL — Not architecture, excluded from platform spec

# Repository Guidelines

## Project Structure & Module Organization
This repo is a multi-service Angular + .NET + Solidity workspace (frontends/services/contracts are git submodules).
- `frontends/` contains Angular apps: `user-app`, `admin-app`, `broker-app`.
- `services/` hosts ASP.NET Core APIs: `custody-service`, `control-plane-service`, `payments-service`, `identity-service`.
- `contracts/` is the Hardhat workspace (Solidity sources, scripts, tests).
- `infrastructure/` holds dev-stack scripts, AI proxy, and local data helpers.
- `docker/`, `docker-compose*.yml`, `docs/`, `styles/`, and `tools/` support ops and docs.
- Front-end unit tests live beside code (e.g. `frontends/**/src/**/*.spec.ts`); Solidity tests are in `contracts/test/`.

## Build, Test, and Development Commands
- `npm start`: run the full Docker-based dev stack (`docker-compose.dev.yml`).
- `npm run frontend:serve` / `admin:serve` / `broker:serve`: run Angular apps locally.
- `npm run custody-service:start` / `control-plane-service:start`: run the custody/control-plane services with `dotnet watch`.
- `npm run stack:up` / `stack:down`: start/stop the Docker stack explicitly.
- `npm run build`: Angular production build.
- `npm test` / `npm run test:ci`: Angular unit tests (Karma/Jasmine).
- `npm run test:sol`: Hardhat test suite.

## Coding Style & Naming Conventions
- TypeScript is formatted with `dprint` (`npm run format:ts`): 100-col width, single quotes, semicolons.
- CSS/SCSS uses Prettier (`npm run format:css`); APIs use `dotnet format` (`npm run format:api`).
- Follow Angular conventions: kebab-case filenames, PascalCase classes.
- Style guide: Angular imports first, then alphabetized; public members before private; observables and query functions end with `$`.

## Testing Guidelines
- Angular tests are `*.spec.ts` in `frontends/**/src/` and run via `ng test`.
- Solidity tests use Mocha via Hardhat in `contracts/test/`.
- Add or update tests for behavior changes, especially UI flows and contract logic.

## Commit & Pull Request Guidelines
- Recent commits use short, sentence-case summaries (e.g. “Add Payments”); keep messages concise and action-oriented.
- PRs should include a clear description, relevant commands run, and screenshots for UI changes. Link related issues when available.

## Configuration & Secrets
- Copy `.env.example` to `.env` for RPC keys, JWTs, and deployer secrets.
- Front-end environment presets are in `frontends/user-app/src/environments/`.
- Client-side tokens (e.g. Pinata) load from `frontends/user-app/src/assets/env.js`.

---
### Typing Standards (Mandatory)
- `var` is not allowed anywhere in the codebase.
- All values must use explicit, hard types.
- If a type does not exist, engineers must create:
  - a DTO or domain model in backend services
  - an interface or type in frontend model files
- Anonymous, dynamic, inferred, or loosely typed objects are prohibited.
- Frontend must use strict TypeScript settings.
- Enums must use explicit numeric values for every member.
- C# classes must be organized with public members first and use #region sections.
- Repositories must support optional Include loading via explicit query option types (lite/full patterns).

## Agent Skills (Opt-In)

### Clarification-Discipline Skill (Explicit Invocation Only)
- Activation: apply this skill only when the user explicitly invokes "clarification-discipline" by name; it must not auto-trigger.
- Scope: applies to all agent actions across frontend, backend, contracts, infrastructure, and docs.
- Underspecified definition: treat a request as underspecified when required inputs are missing or ambiguous (target files/modules, desired behavior/output, constraints, acceptance criteria, interfaces/dependencies, or conflicting interpretations).
- Pause-before-action: when underspecified, pause implementation; only low-risk discovery is allowed (read files/configs). Do not edit or run state-changing commands.
- Clarify first: ask concise, decision-focused questions and wait for answers before implementing.
- Proceeding without answers: list explicit assumptions and require user confirmation to proceed; do not start work until confirmed.
- Restate before work: restate confirmed requirements and confirmed assumptions, then obtain an explicit go-ahead before starting changes.
- Anti-patterns: do not over-ask; avoid open-ended questions when targeted options exist; do not ask what can be inspected—inspect allowed files/configs directly.

## Requirements Clarification & Execution Discipline (Mandatory)

When a request is underspecified, the assistant must clarify requirements before implementing changes.

### When a request is considered underspecified
A request is underspecified if, after a brief mental walkthrough of the work, any of the following are unclear:

- Objective: what should change vs. what must remain unchanged
- Definition of done: acceptance criteria, examples, edge cases
- Scope: which apps, services, contracts, or users are in/out
- Constraints: compatibility, performance, style, dependencies, timelines
- Environment: language/runtime versions, OS, build or test runners
- Safety & reversibility: migrations, rollouts, rollback expectations

If multiple reasonable interpretations exist, treat the request as underspecified.

### Clarification workflow
1. Ask only must-have questions first
   - Ask 1–5 questions max in the first pass.
   - Prefer questions that eliminate entire branches of work.
   - Optimize for fast answers:
     - Numbered questions
     - Multiple-choice options when possible
     - Clear recommended defaults (marked explicitly)
     - A fast-path response (e.g. defaults)
     - Compact reply format (e.g. 1b 2a 3c)
     - Include “Not sure — use default” when helpful

2. Pause before acting
   Until must-have answers are received:
   - Do not modify files, run commands, or produce plans that depend on unknowns
   - Low-risk discovery (reading configs, existing patterns, repo structure) is allowed

   If the user explicitly asks to proceed without answers:
   - State assumptions as a short numbered list
   - Ask for confirmation
   - Proceed only after confirmation or correction

3. Confirm interpretation, then proceed
   Once clarified:
   - Restate requirements in 1–3 sentences, including constraints and success criteria
   - Begin implementation only after confirmation

### Anti-patterns
- Do not ask questions answerable by quick inspection of existing code or config
- Do not ask open-ended questions where a multiple-choice or yes/no would suffice
- Do not over-clarify when the request is already precise

This clarification discipline applies across frontends, services, contracts, infrastructure, and docs.
