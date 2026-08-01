# 5. State Management and Server State

Classify state before selecting a library.

| State | Examples | Typical home |
|---|---|---|
| Local UI | modal, selected tab | component |
| Shared client | theme, draft wizard | lifted state, context or store |
| Server state | interviews, results | TanStack Query |
| URL state | filters, page, search | router/search params |
| Form state | values, errors | form library |

Server state has caching, staleness, retries, invalidation and concurrent-request concerns. A client store alone should not reinvent them.

```ts
const query = useQuery({
  queryKey: ["interview", interviewId],
  queryFn: () => api.getInterview(interviewId),
  staleTime: 30_000
});

const submit = useMutation({
  mutationFn: api.submitInterview,
  onSuccess: () => queryClient.invalidateQueries({ queryKey: ["interviews"] })
});
```

Context is useful for low-frequency global dependencies, but a frequently changing large context may re-render many consumers. Redux Toolkit or Zustand can help complex client workflows; choose based on measured complexity.

Never store access tokens or business-authoritative data in a global store merely for convenience. Prefer secure cookies/BFF where architecture permits.

## Interview checks

Explain cache invalidation, optimistic updates, rollback, normalized state and when Redux is unnecessary.
