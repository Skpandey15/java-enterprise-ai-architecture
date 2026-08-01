# 10. Portfolio Project and Interview Preparation

Build an **Online Interview Portal**:

- OIDC login and logout
- candidate dashboard
- timed MCQ and text questions
- answer autosave and recovery
- submission confirmation
- result and reviewer screens
- role-based administration
- accessible responsive UI
- typed API client and runtime validation
- component, integration and E2E tests
- Docker image, CI pipeline and observable deployment

## Suggested milestones

1. Static accessible screens with TypeScript models.
2. Router, forms and mocked APIs.
3. Real backend integration and server-state cache.
4. Authentication and role-aware navigation.
5. Tests, performance budget, error tracking and CI/CD.

## Experience-calibrated interview answers

A 0–1 year candidate should explain components, props, hooks and one feature built. A 1–2 year engineer should describe API errors, reusable hooks, forms and tests. A 2–3 year engineer should explain architectural boundaries, security, cache invalidation, performance evidence and a production incident.

## Two-minute answer

“I build React applications using TypeScript and feature-based structure. I keep transient UI state local, URL state in routing, form state in a form library and remote data in a server-state cache. API access is centralized and validated, with explicit loading and error states. Authentication uses OIDC; authorization remains enforced by the backend. I test user behavior with Testing Library and MSW, keep a small E2E suite for critical journeys, and measure accessibility and performance. The CI pipeline type-checks, tests, scans and creates an immutable artifact for deployment.”

## Interview question bank

- Virtual DOM and reconciliation
- props versus state; controlled versus uncontrolled
- effect dependencies and cleanup
- context versus Redux versus server-state cache
- routing and protected-route limitations
- API cancellation and race conditions
- authentication, XSS and CSRF
- testing strategy and flaky tests
- accessibility and semantic HTML
- performance profiling and bundle splitting
- production debugging and rollback

## Readiness checklist

You are job-ready when you can build the project without a tutorial, explain every dependency, test failure paths, diagnose it in browser tools, and discuss trade-offs honestly.
