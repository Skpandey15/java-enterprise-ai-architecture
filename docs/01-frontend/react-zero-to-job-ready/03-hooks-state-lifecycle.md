# 3. Hooks, State and Lifecycle

Use `useState` for local state and `useReducer` when transitions form a small state machine. Use refs for mutable values that must not trigger rendering.

```ts
type State = { index: number; answers: Record<string, string> };
type Action =
  | { type: "answer"; questionId: string; value: string }
  | { type: "next" };

function reducer(state: State, action: Action): State {
  if (action.type === "answer") {
    return { ...state, answers: { ...state.answers, [action.questionId]: action.value } };
  }
  return { ...state, index: state.index + 1 };
}
```

## Effects

An effect synchronizes React with an external system: network connection, browser API, timer or subscription. It is not a general data-transformation mechanism.

```tsx
useEffect(() => {
  const controller = new AbortController();
  loadInterview(id, controller.signal).then(setInterview).catch(handleAbort);
  return () => controller.abort();
}, [id]);
```

Dependencies describe values used by the effect. Cleanup prevents leaks and stale work. Custom hooks reuse behavior, not shared state.

## Choosing a hook

| Need | Choice |
|---|---|
| Local UI value | `useState` |
| Related state transitions | `useReducer` |
| DOM/mutable instance | `useRef` |
| External synchronization | `useEffect` |
| Expensive derived calculation | `useMemo`, after measuring |
| Stable callback for a proven need | `useCallback` |

Do not sprinkle memoization everywhere. It adds comparison and cognitive cost.

## Interview checks

Explain stale closures, functional state updates, effect cleanup, hook ordering and state batching.
