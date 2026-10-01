# Project Guidelines

Shared context for every developer and every AI assistant on this repo. Read this before writing code.

## Stack
- **Frontend:** React + TypeScript (Vite) in `client/`
- **Backend:** Node + Express + TypeScript in `server/`
- **Shared types:** `shared/types/`, imported by both client and server
- **Tests:** Vitest for unit/component tests, Supertest for API routes, Playwright for end-to-end
- **Validation:** zod for request params

Don't add a new library without asking the team in the issue first.

## Directory structure
```
client/src/
  api/          # all fetch calls live here, nowhere else
  components/   # presentational components (filters/ for filter controls)
  hooks/        # data and state hooks
  pages/        # route-level components
server/
  db/           # schema, migrations, seed
  routes/       # Express routers, one file per resource
  services/     # business logic and queries (no req/res here)
  validation/   # zod schemas for request input
shared/types/   # contracts shared by client and server
docs/api.md     # API contract: source of truth for endpoints
e2e/            # Playwright specs
```

## Commands
```
npm install            # install everything
npm run dev            # client + server
npm test               # unit + API tests
npm run test:e2e       # Playwright
npm run lint && npm run typecheck
```

## Workflow rules
1. **One issue, one developer, one branch.** Never work on a file another open issue owns. Each issue lists the files it owns.
2. **Interfaces first.** `shared/types/*` and `docs/api.md` are contracts. Don't change them inside a feature branch; open a separate issue and get the team's approval.
3. **Branch naming:** `type/<issue#>-short-description`, e.g. `feature/42-add-login-page`, `fix/57-null-avatar-crash`, `chore/63-update-dependencies`, `test/70-filter-e2e`.
4. **Commits** reference the issue: `Add date range filter (#42)`.
5. **Never commit to `main`.** Push the branch, open a PR, fill in the PR template.
6. **Small PRs.** One responsibility per change. If an issue grows past ~4 hours, split it.
7. **Merge often.** Don't let a branch sit for days; rebase on `main` before opening the PR.

## Coding conventions
- TypeScript strict mode; no `any` without a comment explaining why
- Functional React components with hooks; components receive data via props and don't fetch
- Services are pure business logic and take `userId` explicitly; routes read it from the session, **never** from the query string
- SQL is always parameterized
- Errors from the API use `{ error: { code, message, field? } }`
- Name tests `<file>.test.ts(x)` next to the file they test

## Definition of Done
- [ ] Every acceptance criterion in the issue is checked off
- [ ] Tests from the issue's "Test expectations" are written and passing
- [ ] Lint and typecheck pass
- [ ] `docs/api.md` or README updated if behavior changed
- [ ] PR description has the acceptance criteria and `Closes #<issue>`
- [ ] A teammate has reviewed and approved

## Notes for AI assistants
- Stay inside the files the issue owns. If you need to touch another file, stop and say so.
- Prefer small, focused diffs over multi-file rewrites.
- Write the tests listed in the issue alongside the code.
- Reuse existing helpers in `client/src/api/`, `server/services/`, and `shared/types/` before creating new ones.
