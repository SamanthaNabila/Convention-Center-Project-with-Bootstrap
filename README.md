https://convention-center-project-with-boot-chi.vercel.app/
<img width="1905" height="910" alt="image" src="https://github.com/user-attachments/assets/52d5122f-f4ff-4cc3-ae17-a185167e74aa" />


Below is the proposed complete `AGENT.md` content for UniDesk. It is written from your `spec.md` and adds agent workflow guidance without duplicating the entire specification.

```markdown
# UniDesk Agent Instructions

## Purpose

UniDesk is an internal IT support ticketing system built with:

- FastAPI and Python for the backend.
- PostgreSQL with SQLAlchemy and Alembic.
- React and Vite for the frontend.
- JWT authentication and Bcrypt password hashing.
- Pytest for backend testing.

The complete product specification is documented in `spec.md`. Read it before implementing features or changing behavior.

## Repository Structure

### Backend

The backend is located in `backend/`.

- `app/main.py`: FastAPI application and router registration.
- `app/database.py`: Database engine, sessions, and database dependency.
- `app/dependencies.py`: Authentication and current-user dependencies.
- `app/core/config.py`: Application configuration.
- `app/core/security.py`: Password hashing and JWT functions.
- `app/core/whitelist.py`: Approved registration records.
- `app/models/`: SQLAlchemy database models.
- `app/schemas/`: Pydantic request and response schemas.
- `app/routers/`: Authentication, ticket, and comment endpoints.
- `alembic/`: Database migrations.
- `tests/`: Backend API, validation, and RBAC tests.

### Frontend

The frontend is located in `frontend/`.

- `src/App.jsx`: React routes.
- `src/main.jsx`: Application bootstrap.
- `src/context/AuthContext.jsx`: Authentication state and session handling.
- `src/services/api.js`: Axios client and JWT request handling.
- `src/views/`: Application pages.
- `src/components/common/`: Shared UI components.
- `src/components/dashboard/`: Dashboard components.
- `src/components/tickets/`: Ticket detail, comments, and agent controls.
- `src/index.css`: Global styling.

## Required Working Process

1. Read the relevant section of `spec.md`.
2. Inspect the existing implementation and nearby tests.
3. Identify the smallest set of files responsible for the requested behavior.
4. State the intended change and any assumption before editing.
5. Make a focused change that follows existing patterns.
6. Do not modify unrelated files.
7. Run focused validation immediately after the change.
8. Run broader tests or builds when the change affects shared behavior.
9. Report changed files, validation results, and assumptions.

## General Coding Rules

- Preserve existing public APIs unless the task explicitly changes them.
- Follow the existing project structure and naming conventions.
- Avoid unnecessary new abstractions and dependencies.
- Do not perform unrelated refactoring.
- Do not overwrite or revert user changes.
- Do not commit or push changes unless explicitly requested.
- Do not modify secrets, credentials, or `.env` files.
- Do not add passwords, JWTs, or secret keys to source code or logs.
- Keep comments brief and only add them when the code is not self-explanatory.

## Backend Rules

- Use FastAPI routers and dependencies already established in the project.
- Validate request data through Pydantic schemas.
- Use SQLAlchemy models and the existing database session dependency.
- Enforce authorization on the backend, not only in the frontend.
- Return consistent HTTP status codes and error response formats.
- Use `401` for missing or invalid authentication.
- Use `403` for authenticated users without permission.
- Use `404` for missing resources.
- Use `409` for business-rule conflicts.
- Preserve existing ownership checks and role restrictions.
- Use database transactions consistently.
- Add or update pytest tests for backend behavior changes.

## Role Permissions

### Employee

Employees can:

- Register through the approved whitelist.
- Log in and view the dashboard.
- View all tickets.
- Create tickets.
- Edit their own tickets.
- Delete their own tickets.
- Comment on their own tickets.

Employees cannot:

- Create tickets for other users.
- Change ticket status.
- Change ticket priority through agent controls.
- Edit or delete another user's tickets.
- Comment on another user's tickets.

### Support Agent

Support agents can:

- Register through the approved whitelist.
- View all tickets and dashboard statistics.
- Change ticket status.
- Change ticket priority.
- Comment on any ticket.

Support agents cannot:

- Create tickets.
- Edit or delete ticket title and description as ticket owners.
- Modify other users' comments unless explicitly required.

The backend must remain the final authority for these permissions.

## Ticket Rules

Valid statuses are:

```text
open
in_progress
resolved
closed
```

Valid priorities are:

```text
low
medium
high
```

The specification describes this lifecycle:

```text
open -> in_progress -> resolved -> closed
```

Do not add, remove, or enforce additional transitions without confirming the intended business rule. In particular, clarify whether:

- Agents may jump directly between statuses.
- `open -> closed` is allowed.
- Resolved tickets may be reopened.
- Closed tickets may be reopened.
- Status transitions require an audit history.

Priority-only updates must remain separate from status-transition validation.

## Frontend Rules

- Follow the existing React component structure.
- Use the existing Axios service for API calls.
- Keep authentication state in `AuthContext`.
- Keep role-based UI behavior consistent with backend permissions.
- Preserve loading, error, and empty states.
- Do not change backend APIs for frontend-only tasks.
- Use accessible labels and meaningful button text.
- Do not mutate API response arrays directly.
- Derive filtered or displayed data from existing state.
- Reuse existing Tailwind styling conventions.
- Avoid adding dependencies when existing project tools are sufficient.

## Authentication and Security

- Registration must validate users against `MOCK_EMPLOYEE_WHITELIST`.
- Whitelist matching is case-insensitive and ignores surrounding whitespace.
- Passwords must be hashed before storage.
- JWTs must be validated before protected operations.
- The database user record is authoritative for current permissions.
- Authentication failures must not expose sensitive details.
- Never log passwords, tokens, secrets, or authorization headers.
- Preserve protected routes and logout behavior unless security changes are requested.

## Testing and Validation

### Backend

From `backend/`:

```powershell
.\venv\Scripts\activate
pytest -q
pytest --cov=app --cov-report=term-missing --cov-fail-under=70
```

Run the most focused test first, then run the complete suite for shared backend changes.

Tests should cover:

- Successful behavior.
- Invalid input.
- Missing resources.
- Authentication failures.
- Role restrictions.
- Ownership restrictions.
- Edge cases introduced by the change.

### Frontend

From `frontend/`:

```powershell
npm install
npm run build
npm run lint
```

For frontend changes, verify loading, success, error, empty, and role-specific states when applicable.

## Handling Ambiguous Requirements

Do not silently invent important business rules.

When requirements conflict or are incomplete:

1. Compare `spec.md` with the current implementation.
2. Check nearby tests and API behavior.
3. Identify the ambiguity clearly.
4. Ask the project owner if the decision affects security, permissions, data structure, or workflow.
5. If the decision is low-risk, choose the smallest implementation and document the assumption.

## Final Report

After completing a task, report:

- The behavior that changed.
- Files modified or created.
- Why each file was changed.
- Tests, builds, or checks that were run.
- Validation results.
- Any assumptions, limitations, or remaining risks.

Do not claim a test or command passed unless it was actually executed.
```

This file should be placed at the repository root:

```text
UniDesk/AGENT.md
```

Your `spec.md` remains the source of truth for product requirements. `AGENT.md` adds instructions about how an AI coding agent should interpret those requirements, make changes, validate them, and handle ambiguity.

