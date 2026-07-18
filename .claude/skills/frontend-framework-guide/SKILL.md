---
name: frontend-framework-guide
description: Master modern frontend frameworks for building production-quality web applications. Use this skill for React patterns and best practices, hooks and state management, component architecture, performance optimization, testing strategies, and framework selection. Triggers include: React component patterns, custom hooks development, state management (Redux, Context, Zustand), component composition, performance with useCallback/useMemo, dependency arrays, testing React components, framework comparisons, and architectural decisions. Essential for frontend developers building scalable, maintainable, and performant React applications.
---

# Frontend Framework Guide

Patterns and best practices for building production applications with modern frameworks, primarily React.

React is the dominant frontend framework for building scalable applications. Mastering React's mental model—component composition, hooks, state management, and performance optimization—is essential for professional development.

---

## Core Concept: Components & Composition

### The React Mental Model

React is built on **declarative component composition**:

```
State → Component → UI

User interaction → State change → Re-render → New UI
```

**Declarative:** Describe what UI should look like, React handles updates.
**Imperative:** Manually update DOM (old way, error-prone).

### Functional Components (Modern)

```jsx
function Button({ label, onClick }) {
  return (
    <button onClick={onClick}>
      {label}
    </button>
  );
}

// Use it
<Button label="Click me" onClick={() => console.log('clicked')} />
```

**Functional components are the standard now.** Class components are legacy.

---

## Hooks: The Foundation

### useState: Managing Component State

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  // count: current state value
  // setCount: function to update state

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

**Key points:**
- `useState` returns `[value, setter]`
- Setter triggers re-render
- Initial value can be a function (useful for expensive calculations)

### useEffect: Side Effects

```jsx
import { useEffect, useState } from 'react';

function DataFetcher() {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Run after render
    fetch('/api/data')
      .then(res => res.json())
      .then(data => {
        setData(data);
        setLoading(false);
      });
  }, []); // Dependency array: run once on mount

  if (loading) return <div>Loading...</div>;
  return <div>{data}</div>;
}
```

**Dependency array patterns:**
- `[]` (empty): Run once on mount
- `[dep1, dep2]`: Run when deps change
- No array: Run after every render (rarely needed, performance killer)

### useCallback: Memoizing Functions

```jsx
import { useCallback, useState } from 'react';

function Parent() {
  const [count, setCount] = useState(0);

  // Without useCallback: new function every render
  const handleClick = () => {
    console.log('clicked', count);
  };

  // With useCallback: same function if deps don't change
  const handleClickMemo = useCallback(() => {
    console.log('clicked', count);
  }, [count]); // Only recreate if count changes

  return <Child onClick={handleClickMemo} />;
}
```

**When to use:**
- Passing function to memoized child
- Using function as dependency in other hooks
- Performance-critical code paths

**When NOT to use:**
- Passing to non-memoized children (wastes memory)
- Debugging (adds complexity)
- Simple inline handlers

### useMemo: Memoizing Values

```jsx
import { useMemo, useState } from 'react';

function ExpensiveCalculation({ items }) {
  const [sort, setSort] = useState('asc');

  // Without useMemo: recalculates every render
  const sorted = items
    .filter(item => item.active)
    .sort((a, b) => sort === 'asc' ? a.value - b.value : b.value - a.value);

  // With useMemo: only recalculate if items/sort change
  const sortedMemo = useMemo(() => {
    return items
      .filter(item => item.active)
      .sort((a, b) => sort === 'asc' ? a.value - b.value : b.value - a.value);
  }, [items, sort]);

  return <div>{sortedMemo.length} items</div>;
}
```

**When to use:**
- Expensive calculations (sorting, filtering large arrays)
- Dependency for other memoized values
- Preventing child re-renders

**When NOT to use:**
- Simple calculations (wastes memory)
- Rarely-changing data

### useRef: Accessing DOM Directly

```jsx
import { useRef } from 'react';

function TextInput() {
  const inputRef = useRef(null);

  const focus = () => {
    inputRef.current.focus();
  };

  return (
    <>
      <input ref={inputRef} />
      <button onClick={focus}>Focus input</button>
    </>
  );
}
```

**When to use:**
- Managing focus, text selection, media playback
- Triggering animations
- Integrating third-party libraries

**When NOT to use:**
- State management (use useState)
- Rendering data (use state)

---

## Component Patterns

### Controlled vs Uncontrolled Inputs

**Controlled (React manages state):**
```jsx
function Form() {
  const [email, setEmail] = useState('');

  return (
    <input
      value={email}
      onChange={(e) => setEmail(e.target.value)}
    />
  );
}
```

**Uncontrolled (DOM manages state):**
```jsx
function Form() {
  const inputRef = useRef(null);

  const handleSubmit = () => {
    console.log(inputRef.current.value);
  };

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleSubmit}>Submit</button>
    </>
  );
}
```

**Use controlled for forms.** Uncontrolled only for file inputs and integrations.

### Lifting State Up

When multiple components need shared state:

```jsx
// ❌ Bad: State in each component (out of sync)
function Parent() {
  return (
    <>
      <FirstChild />
      <SecondChild />
    </>
  );
}

// ✅ Good: State in parent, passed down
function Parent() {
  const [count, setCount] = useState(0);

  return (
    <>
      <FirstChild count={count} />
      <SecondChild count={count} onIncrement={() => setCount(count + 1)} />
    </>
  );
}
```

### Custom Hooks

Encapsulate stateful logic:

```jsx
// Custom hook
function useFormInput(initialValue = '') {
  const [value, setValue] = useState(initialValue);

  return {
    value,
    setValue,
    bind: {
      value,
      onChange: (e) => setValue(e.target.value),
    },
  };
}

// Use it
function Form() {
  const email = useFormInput('');
  const password = useFormInput('');

  return (
    <form>
      <input {...email.bind} placeholder="Email" />
      <input type="password" {...password.bind} placeholder="Password" />
    </form>
  );
}
```

---

## State Management

### Context API

For prop drilling (passing props through many levels):

```jsx
import { createContext, useState } from 'react';

const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Use it
function Button() {
  const { theme, toggleTheme } = useContext(ThemeContext);

  return (
    <button onClick={toggleTheme}>
      Current theme: {theme}
    </button>
  );
}
```

**When to use Context:**
- Theme, language, user authentication
- Moderate-complexity state
- Avoid if state updates frequently (performance hit)

### Zustand (Simple Global State)

```jsx
import { create } from 'zustand';

const useStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
}));

function Counter() {
  const { count, increment, decrement } = useStore();

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
    </div>
  );
}
```

**When to use Zustand:**
- Simple, lightweight global state
- Minimal boilerplate
- Predictable performance

### Redux (Complex State)

```jsx
import { createSlice, configureStore } from '@reduxjs/toolkit';
import { useDispatch, useSelector } from 'react-redux';

// Define state slice
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value += 1;
    },
    decrement: (state) => {
      state.value -= 1;
    },
  },
});

// Create store
const store = configureStore({
  reducer: {
    counter: counterSlice.reducer,
  },
});

// Use in component
function Counter() {
  const dispatch = useDispatch();
  const count = useSelector((state) => state.counter.value);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch(counterSlice.actions.increment())}>+</button>
    </div>
  );
}
```

**When to use Redux:**
- Complex, interconnected state
- Many actions/reducers
- DevTools debugging needed

---

## Performance Optimization

### React.memo: Prevent Unnecessary Re-renders

```jsx
import { memo } from 'react';

const Button = memo(({ label, onClick }) => {
  console.log('Button rendered');
  return <button onClick={onClick}>{label}</button>;
});

function Parent() {
  const [count, setCount] = useState(0);

  // Parent re-renders, but Button doesn't (props unchanged)
  return (
    <div>
      <p>Count: {count}</p>
      <Button label="Increment" onClick={() => setCount(count + 1)} />
    </div>
  );
}
```

**When to use:**
- Heavy components (lists, charts)
- Props rarely change
- Profiler shows it's needed

### Code Splitting with Dynamic Imports

```jsx
import { lazy, Suspense } from 'react';

const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <HeavyComponent />
    </Suspense>
  );
}
```

**Reduces initial bundle size.** Load features on-demand.

### Avoid Inline Objects in Dependencies

```jsx
// ❌ Bad: Object re-created every render (infinite loop)
useEffect(() => {
  // fetch with config
}, [{ retry: true }]); // New object every render!

// ✅ Good: Define outside
const config = { retry: true };
useEffect(() => {
  // fetch with config
}, [config]);

// ✅ Also good: Use useMemo
useEffect(() => {
  // fetch with config
}, [useMemo(() => ({ retry: true }), [])]);
```

---

## Testing React Components

### Testing Patterns

```jsx
import { render, screen, fireEvent } from '@testing-library/react';
import Button from './Button';

describe('Button', () => {
  // Test rendering
  it('renders with label', () => {
    render(<Button label="Click me" />);
    expect(screen.getByRole('button', { name: /click me/i })).toBeInTheDocument();
  });

  // Test interaction
  it('calls onClick when clicked', () => {
    const onClick = jest.fn();
    render(<Button label="Click" onClick={onClick} />);
    fireEvent.click(screen.getByRole('button'));
    expect(onClick).toHaveBeenCalled();
  });

  // Test state
  it('updates count on click', () => {
    render(<Counter />);
    const button = screen.getByRole('button', { name: /increment/i });
    fireEvent.click(button);
    expect(screen.getByText(/count: 1/i)).toBeInTheDocument();
  });
});
```

**Best practices:**
- Test user behavior, not implementation
- Use `screen` queries (more resilient)
- Test accessibility (`getByRole` uses ARIA)
- Mock external APIs

---

## Common Mistakes

### ❌ Modifying State Directly

```jsx
// ❌ Wrong: Direct mutation
const [items, setItems] = useState([]);
items.push(newItem); // Doesn't trigger re-render

// ✅ Right: Create new array
setItems([...items, newItem]);
```

### ❌ Infinite Loops in useEffect

```jsx
// ❌ Wrong: No dependency array (runs every render)
useEffect(() => {
  setData(newData);
});

// ✅ Right: Empty array (run once)
useEffect(() => {
  setData(newData);
}, []);

// ✅ Right: Specific dependencies
useEffect(() => {
  fetchData(id);
}, [id]);
```

### ❌ Using Index as Key in Lists

```jsx
// ❌ Wrong: Index as key (can break state)
{items.map((item, index) => <Item key={index} />)}

// ✅ Right: Unique ID
{items.map((item) => <Item key={item.id} />)}
```

### ❌ Unnecessary Memoization

```jsx
// ❌ Wrong: Wastes memory on simple components
const SimpleText = memo(({ text }) => <p>{text}</p>);

// ✅ Right: Use memo for heavy components only
const DataTable = memo(({ rows }) => <table>{/* expensive render */}</table>);
```

---

## When to Use This Skill

✅ **Do use for:**
- React component patterns and best practices
- Hooks (useState, useEffect, useCallback, useMemo, useRef)
- State management approaches (Context, Zustand, Redux)
- Component composition and lifting state
- Performance optimization (React.memo, code splitting, memoization)
- Custom hooks development
- Testing React components
- Framework selection guidance

❌ **Don't use for:**
- CSS styling (see css-styling-pixel-perfect)
- Design system setup (see design-systems-architecture)
- Responsive design (see responsive-universal-design)
- Build tools and bundlers
- Node.js backend frameworks
- HTML/semantic markup (see web-accessibility-a11y)
