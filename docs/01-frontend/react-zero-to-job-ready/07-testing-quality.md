# 7. Testing and Code Quality

Test behavior visible to users, not implementation details.

| Layer | Purpose | Tools |
|---|---|---|
| Unit | pure functions, reducers | Vitest/Jest |
| Component | rendered behavior | Testing Library |
| Integration | UI with mocked network | Testing Library + MSW |
| End-to-end | critical browser journeys | Playwright/Cypress |

```tsx
it("submits the selected answer", async () => {
  render(<QuestionCard question={question} onSelect={onSelect} />);
  await userEvent.click(screen.getByRole("radio", { name: /option b/i }));
  expect(onSelect).toHaveBeenCalledWith("b");
});
```

Prefer semantic queries such as role and accessible name. MSW intercepts at the network boundary and keeps API behavior realistic. E2E tests should cover a small set of high-value journeys: authentication, interview completion, timeout recovery and results.

Quality gates can include TypeScript, ESLint, tests, coverage trends, dependency scanning and production build. Coverage is evidence, not proof.

Avoid brittle snapshots, arbitrary sleeps, testing private functions and mocking everything.

## Interview checks

Explain the testing pyramid, flaky-test diagnosis, mocking boundaries and what belongs in E2E.
