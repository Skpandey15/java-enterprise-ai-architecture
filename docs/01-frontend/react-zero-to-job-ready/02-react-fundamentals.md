# 2. React Fundamentals and Component Thinking

React renders UI from state. Components should express one responsibility and remain predictable.

```tsx
type Props = {
  question: Question;
  selected?: string;
  onSelect: (answer: string) => void;
};

export function QuestionCard({ question, selected, onSelect }: Props) {
  return (
    <fieldset>
      <legend>{question.title}</legend>
      {question.options.map(option => (
        <label key={option.id}>
          <input type="radio" checked={selected === option.id}
            onChange={() => onSelect(option.id)} />
          {option.label}
        </label>
      ))}
    </fieldset>
  );
}
```

## Core rules

- Props flow down; events flow up.
- State is a snapshot, not a mutable variable.
- Keys identify sibling elements across renders; stable domain IDs beat array indexes.
- Controlled inputs make React state authoritative.
- Composition is usually more flexible than inheritance.
- Derived values should normally be calculated during render, not duplicated in state.

Rendering and committing are separate. Rendering must stay pure because React may invoke it more than once during development or concurrent work.

## Common mistakes

- Calling a setter during render.
- Mutating arrays or objects in state.
- Using `useEffect` to calculate ordinary derived state.
- Creating one giant page component.
- Hiding invalid HTML behind styled components.

## Interview checks

Explain reconciliation, keys, controlled inputs, fragments, conditional rendering and why a component re-renders.
