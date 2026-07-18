# State Management Patterns

Comparison and patterns for different state management approaches.

---

## When to Choose Each Approach

### Local State (useState)
**Use when:** State is only used in one component
**Pros:** Simple, minimal boilerplate, no dependencies
**Cons:** Can't share between components

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  return <div>{count}</div>;
}
```

### Lifting State Up
**Use when:** Siblings share state
**Pros:** Simple, no extra dependencies
**Cons:** Prop drilling if many levels

```jsx
function Parent() {
  const [count, setCount] = useState(0);
  return (
    <>
      <Child1 count={count} />
      <Child2 count={count} setCount={setCount} />
    </>
  );
}
```

### Context API
**Use when:** Global state, moderate complexity, infrequent updates
**Pros:** Simple, no dependencies, good for theming/auth
**Cons:** Performance issues if state updates frequently

```jsx
const CountContext = createContext();

export function CountProvider({ children }) {
  const [count, setCount] = useState(0);
  return (
    <CountContext.Provider value={{ count, setCount }}>
      {children}
    </CountContext.Provider>
  );
}

function useCount() {
  return useContext(CountContext);
}
```

### Zustand
**Use when:** Simple global state, minimal boilerplate preferred
**Pros:** Small, simple API, good performance
**Cons:** Less ecosystem than Redux

```jsx
const useStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));

function Counter() {
  const { count, increment } = useStore();
  return <button onClick={increment}>{count}</button>;
}
```

### Redux
**Use when:** Complex state, many actions, devtools debugging needed
**Pros:** Powerful, predictable, great devtools
**Cons:** Boilerplate heavy, steep learning curve

```jsx
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },
  },
});

const store = configureStore({ reducer: { counter: counterSlice.reducer } });

function Counter() {
  const dispatch = useDispatch();
  const count = useSelector((state) => state.counter.value);
  return <button onClick={() => dispatch(counterSlice.actions.increment())}>{count}</button>;
}
```

---

## Decision Tree

```
State needed in:
├── Single component
│   └── Use useState (local state)
├── Parent + children
│   └── Lift state to parent
├── Multiple branches
│   ├── Not frequently updated
│   │   └── Use Context API (theme, user, auth)
│   ├── Simple global state
│   │   └── Use Zustand
│   └── Complex interconnected state
│       └── Use Redux
```

---

## Context API: Complete Example

```jsx
// Create context
const UserContext = createContext();

// Provider component
export function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Fetch current user on mount
    fetch('/api/user')
      .then(res => res.json())
      .then(data => {
        setUser(data);
        setLoading(false);
      });
  }, []);

  const logout = () => {
    setUser(null);
  };

  const value = { user, loading, logout };

  return (
    <UserContext.Provider value={value}>
      {children}
    </UserContext.Provider>
  );
}

// Custom hook for using context
export function useUser() {
  const context = useContext(UserContext);
  if (!context) {
    throw new Error('useUser must be used within UserProvider');
  }
  return context;
}

// Usage in component
function Profile() {
  const { user, loading, logout } = useUser();

  if (loading) return <div>Loading...</div>;
  return (
    <div>
      <h1>{user.name}</h1>
      <button onClick={logout}>Logout</button>
    </div>
  );
}

// Wrap app with provider
function App() {
  return (
    <UserProvider>
      <Profile />
    </UserProvider>
  );
}
```

---

## Zustand: Complete Example

```jsx
import { create } from 'zustand';
import { devtools, persist } from 'zustand/middleware';

const useStore = create(
  persist(
    devtools((set) => ({
      // State
      count: 0,
      items: [],
      
      // Actions
      increment: () => set((state) => ({ count: state.count + 1 })),
      decrement: () => set((state) => ({ count: state.count - 1 })),
      addItem: (item) => set((state) => ({ items: [...state.items, item] })),
      removeItem: (id) => set((state) => ({
        items: state.items.filter((item) => item.id !== id),
      })),
      
      // Reset
      reset: () => set({ count: 0, items: [] }),
    })),
    { name: 'app-store' } // Persists to localStorage
  )
);

// Usage in component
function App() {
  const { count, increment, decrement, items, addItem } = useStore();

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      
      <ul>
        {items.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
      <button onClick={() => addItem({ id: Date.now(), name: 'New' })}>
        Add
      </button>
    </div>
  );
}
```

---

## Redux: Complete Example

```jsx
import { createSlice, configureStore } from '@reduxjs/toolkit';
import { useDispatch, useSelector } from 'react-redux';

// Define slice
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1; },
    decrement: (state) => { state.value -= 1; },
    incrementByAmount: (state, action) => { state.value += action.payload; },
  },
});

const itemsSlice = createSlice({
  name: 'items',
  initialState: [],
  reducers: {
    addItem: (state, action) => { state.push(action.payload); },
    removeItem: (state, action) => state.filter((item) => item.id !== action.payload),
  },
});

// Create store
const store = configureStore({
  reducer: {
    counter: counterSlice.reducer,
    items: itemsSlice.reducer,
  },
});

// Usage in component
function App() {
  const dispatch = useDispatch();
  const count = useSelector((state) => state.counter.value);
  const items = useSelector((state) => state.items);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => dispatch(counterSlice.actions.increment())}>+</button>
      <button onClick={() => dispatch(counterSlice.actions.decrement())}>-</button>
      
      <ul>
        {items.map((item) => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
      <button onClick={() => dispatch(itemsSlice.actions.addItem({ id: Date.now(), name: 'New' }))}>
        Add
      </button>
    </div>
  );
}

// Wrap app
import { Provider } from 'react-redux';

function Root() {
  return (
    <Provider store={store}>
      <App />
    </Provider>
  );
}
```

---

## Performance Optimization with State

### Context Performance Issue

```jsx
// ❌ Bad: All consumers re-render when any value changes
const CountContext = createContext();

function CountProvider({ children }) {
  const [count, setCount] = useState(0);
  const [color, setColor] = useState('blue');

  // Every change to count or color re-renders all consumers!
  return (
    <CountContext.Provider value={{ count, setCount, color, setColor }}>
      {children}
    </CountContext.Provider>
  );
}
```

**Solution: Split into multiple contexts**

```jsx
// ✅ Good: Separate concerns
const CountContext = createContext();
const ColorContext = createContext();

function CountProvider({ children }) {
  const [count, setCount] = useState(0);
  return (
    <CountContext.Provider value={{ count, setCount }}>
      {children}
    </CountContext.Provider>
  );
}

function ColorProvider({ children }) {
  const [color, setColor] = useState('blue');
  return (
    <ColorContext.Provider value={{ color, setColor }}>
      {children}
    </ColorContext.Provider>
  );
}

// Wrap separately
function App() {
  return (
    <CountProvider>
      <ColorProvider>
        <Content />
      </ColorProvider>
    </CountProvider>
  );
}
```

### Zustand & Redux are Optimized

Both automatically subscribe to only needed slices:

```jsx
// Only re-renders when count changes (not color)
const count = useStore((state) => state.count);

// Only re-renders when counter changes (not items)
const count = useSelector((state) => state.counter.value);
```

---

## Testing State Management

### Testing Zustand Store

```jsx
const useTestStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));

describe('Zustand Store', () => {
  it('increments count', () => {
    const { result } = renderHook(() => useTestStore());
    
    act(() => {
      result.current.increment();
    });

    expect(result.current.count).toBe(1);
  });
});
```

### Testing Redux Store

```jsx
describe('Counter Reducer', () => {
  it('increments', () => {
    const action = { type: 'INCREMENT' };
    const state = { value: 0 };
    const newState = reducer(state, action);
    expect(newState.value).toBe(1);
  });
});
```

### Testing Context

```jsx
const TestProvider = ({ children, value }) => (
  <UserContext.Provider value={value}>
    {children}
  </UserContext.Provider>
);

describe('useUser', () => {
  it('returns user', () => {
    const mockUser = { id: 1, name: 'Test' };
    const { result } = renderHook(() => useUser(), {
      wrapper: ({ children }) => (
        <TestProvider value={{ user: mockUser }}>
          {children}
        </TestProvider>
      ),
    });

    expect(result.current.user).toEqual(mockUser);
  });
});
```

---

## State Management Checklist

- [ ] Chose appropriate state management (local, lift, Context, Zustand, Redux)
- [ ] State is not duplicated in multiple places
- [ ] Complex state uses actions/reducers (not just setters)
- [ ] Performance optimized (not unnecessary re-renders)
- [ ] Tested (store logic, actions/reducers)
- [ ] Documented (what each state/action does)
- [ ] DevTools enabled (Redux, Zustand) for debugging
- [ ] Persisted to localStorage if needed (Zustand middleware)
- [ ] Typed (TypeScript for Redux/Zustand)
