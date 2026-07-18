# Component Architecture Patterns

Patterns for scalable, maintainable component structures.

---

## Pattern 1: Presentational vs. Container Components

**Container component:** Handles logic, fetching, state management
**Presentational component:** Pure rendering, receives props

```jsx
// Container: Handles logic
function UserListContainer() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch('/api/users')
      .then(res => res.json())
      .then(data => {
        setUsers(data);
        setLoading(false);
      });
  }, []);

  return <UserList users={users} loading={loading} />;
}

// Presentational: Pure rendering
function UserList({ users, loading }) {
  if (loading) return <div>Loading...</div>;
  return (
    <ul>
      {users.map((user) => (
        <UserItem key={user.id} user={user} />
      ))}
    </ul>
  );
}
```

**Benefits:**
- Reusability (presentational component works with any data)
- Testability (mock props, no API calls)
- Clarity (container = logic, presentation = UI)

---

## Pattern 2: Compound Components

Components that work together to form a complete UI:

```jsx
// Compound component pattern
const Tabs = ({ children }) => {
  const [activeTab, setActiveTab] = useState(0);

  return (
    <div>
      {children.map((child, i) =>
        React.cloneElement(child, { activeTab, setActiveTab, index: i })
      )}
    </div>
  );
};

const TabButton = ({ activeTab, setActiveTab, index, label }) => (
  <button
    onClick={() => setActiveTab(index)}
    style={{ fontWeight: activeTab === index ? 'bold' : 'normal' }}
  >
    {label}
  </button>
);

const TabContent = ({ activeTab, index, children }) => (
  activeTab === index ? <div>{children}</div> : null
);

// Usage
function App() {
  return (
    <Tabs>
      <TabButton label="Tab 1" />
      <TabButton label="Tab 2" />
      <TabContent>Content 1</TabContent>
      <TabContent>Content 2</TabContent>
    </Tabs>
  );
}
```

**Benefits:**
- Flexible composition
- Implicit state sharing
- Natural API

---

## Pattern 3: Render Props

Component accepts a function as a prop to render content:

```jsx
// Component with render prop
function Mouse({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  const handleMouseMove = (event) => {
    setPosition({
      x: event.clientX,
      y: event.clientY,
    });
  };

  return (
    <div onMouseMove={handleMouseMove}>
      {render(position)}
    </div>
  );
}

// Usage
<Mouse
  render={({ x, y }) => (
    <h1>The mouse is at ({x}, {y})</h1>
  )}
/>
```

---

## Pattern 4: Higher-Order Components (HOC)

Function that returns a component with additional props:

```jsx
// HOC that adds theme
function withTheme(Component) {
  return function ThemedComponent(props) {
    const [theme, setTheme] = useState('light');

    const toggleTheme = () => {
      setTheme(theme === 'light' ? 'dark' : 'light');
    };

    return (
      <Component
        theme={theme}
        toggleTheme={toggleTheme}
        {...props}
      />
    );
  };
}

// Usage
function Button({ theme, toggleTheme }) {
  return (
    <button
      onClick={toggleTheme}
      style={{
        background: theme === 'light' ? '#fff' : '#000',
        color: theme === 'light' ? '#000' : '#fff',
      }}
    >
      Toggle Theme
    </button>
  );
}

const ThemedButton = withTheme(Button);
```

---

## Pattern 5: Custom Hooks (Modern)

Preferred over render props and HOCs:

```jsx
// Custom hook (cleaner than HOC/render props)
function useTheme() {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(theme === 'light' ? 'dark' : 'light');
  };

  return { theme, toggleTheme };
}

// Usage
function Button() {
  const { theme, toggleTheme } = useTheme();

  return (
    <button
      onClick={toggleTheme}
      style={{
        background: theme === 'light' ? '#fff' : '#000',
        color: theme === 'light' ? '#000' : '#fff',
      }}
    >
      Toggle Theme
    </button>
  );
}
```

---

## Pattern 6: Composition Over Props Drilling

**Bad: Props drilling (passing through many levels)**

```jsx
function App() {
  const [user, setUser] = useState();
  return (
    <Header user={user} />
    <Content user={user} />
    <Footer user={user} />
  );
}

function Header({ user }) {
  return <Navigation user={user} />;
}

function Navigation({ user }) {
  return <UserMenu user={user} />;
}
```

**Better: Composition with slots**

```jsx
function App() {
  const [user, setUser] = useState();
  
  return (
    <Header>
      <UserMenu user={user} />
    </Header>
  );
}

function Header({ children }) {
  return (
    <header>
      <Navigation>{children}</Navigation>
    </header>
  );
}

function Navigation({ children }) {
  return <nav>{children}</nav>;
}
```

---

## Pattern 7: Controlled vs. Uncontrolled Components

**Controlled: React controls component state**

```jsx
function ControlledInput() {
  const [value, setValue] = useState('');

  return (
    <input
      value={value}
      onChange={(e) => setValue(e.target.value)}
    />
  );
}
```

**Uncontrolled: Component controls its own state**

```jsx
function UncontrolledInput() {
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

**Use controlled for forms.** Uncontrolled only for file inputs and third-party integration.

---

## Pattern 8: Component Folder Structure

**Organized, scalable folder structure:**

```
src/
├── components/
│   ├── Button/
│   │   ├── Button.jsx
│   │   ├── Button.css
│   │   ├── Button.test.jsx
│   │   └── index.js
│   ├── Card/
│   │   ├── Card.jsx
│   │   ├── Card.css
│   │   └── index.js
│   └── index.js (barrel export)
├── hooks/
│   ├── useFormValidation.js
│   ├── useFetch.js
│   └── index.js
├── pages/
│   ├── HomePage.jsx
│   ├── ProfilePage.jsx
│   └── index.js
├── utils/
│   ├── formatDate.js
│   ├── api.js
│   └── index.js
└── App.jsx
```

**Barrel export (components/index.js):**

```jsx
export { default as Button } from './Button';
export { default as Card } from './Card';
```

**Usage:**

```jsx
import { Button, Card } from './components';
```

---

## Pattern 9: Performance-Optimized Component

```jsx
const ExpensiveComponent = React.memo(
  function ExpensiveComponent({ data, onUpdate }) {
    console.log('Component rendered');

    return (
      <div>
        <h2>{data.title}</h2>
        <p>{data.description}</p>
        <button onClick={onUpdate}>Update</button>
      </div>
    );
  },
  (prevProps, nextProps) => {
    // Custom comparison: return true if props are equal (skip re-render)
    return (
      prevProps.data.id === nextProps.data.id &&
      prevProps.onUpdate === nextProps.onUpdate
    );
  }
);
```

---

## Pattern 10: Error Boundary

Catch errors in component tree:

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    console.log('Error caught:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <div>Something went wrong: {this.state.error.message}</div>;
    }

    return this.props.children;
  }
}

// Usage
<ErrorBoundary>
  <ProblematicComponent />
</ErrorBoundary>
```

---

## Component Design Checklist

- [ ] Single responsibility (component does one thing)
- [ ] Reusable (works in different contexts)
- [ ] Testable (easy to test with props)
- [ ] Documented (clear prop types, examples)
- [ ] Accessible (semantic HTML, ARIA)
- [ ] Performant (React.memo if needed, no unnecessary renders)
- [ ] Styled (CSS modules or styled-components)
- [ ] Typed (TypeScript propTypes or interfaces)
- [ ] Composable (works with other components)
- [ ] Error handling (Error Boundary or try-catch)

---

## Comparison: Old vs. New Patterns

| Pattern | Old Way | New Way |
|---------|---------|---------|
| Logic/UI separation | Container/Presentational | Custom Hooks + Functional Components |
| Composition | Render Props | Hooks + Component Composition |
| Reusing Logic | HOC | Custom Hooks |
| State | Class setState | useState Hook |
| Effects | componentDidMount | useEffect Hook |

**Modern React uses hooks as primary pattern.**
