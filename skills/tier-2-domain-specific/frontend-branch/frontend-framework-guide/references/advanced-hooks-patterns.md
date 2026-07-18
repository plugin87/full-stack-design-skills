# Advanced React Hooks Patterns

Practical patterns for complex hooks scenarios.

---

## Pattern 1: Custom Hook for Form Validation

```jsx
function useFormValidation(initialState, onSubmit) {
  const [values, setValues] = useState(initialState);
  const [errors, setErrors] = useState({});
  const [touched, setTouched] = useState({});

  const validate = (name, value) => {
    const newErrors = { ...errors };

    if (name === 'email') {
      const isValid = /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value);
      if (!isValid) {
        newErrors.email = 'Invalid email';
      } else {
        delete newErrors.email;
      }
    }

    if (name === 'password') {
      if (value.length < 8) {
        newErrors.password = 'Min 8 characters';
      } else {
        delete newErrors.password;
      }
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleChange = (e) => {
    const { name, value } = e.target;
    setValues({ ...values, [name]: value });
    validate(name, value);
  };

  const handleBlur = (e) => {
    const { name } = e.target;
    setTouched({ ...touched, [name]: true });
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    const isValid = Object.keys(values).every((key) =>
      validate(key, values[key])
    );
    if (isValid) {
      onSubmit(values);
    }
  };

  return { values, errors, touched, handleChange, handleBlur, handleSubmit };
}
```

---

## Pattern 2: useFetch Hook for Data Loading

```jsx
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    let isMounted = true;

    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        if (!response.ok) throw new Error('Fetch failed');
        const json = await response.json();
        if (isMounted) {
          setData(json);
          setError(null);
        }
      } catch (err) {
        if (isMounted) {
          setError(err.message);
          setData(null);
        }
      } finally {
        if (isMounted) {
          setLoading(false);
        }
      }
    };

    fetchData();

    return () => {
      isMounted = false;
    };
  }, [url]);

  return { data, loading, error };
}
```

---

## Pattern 3: useLocalStorage Hook

```jsx
function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.log(error);
      return initialValue;
    }
  });

  const setValue = (value) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.log(error);
    }
  };

  return [storedValue, setValue];
}
```

---

## Pattern 4: useAsync Hook

```jsx
function useAsync(asyncFunction, immediate = true) {
  const [status, setStatus] = useState('idle');
  const [data, setData] = useState(null);
  const [error, setError] = useState(null);

  const execute = useCallback(async (...args) => {
    setStatus('pending');
    setData(null);
    setError(null);

    try {
      const response = await asyncFunction(...args);
      setData(response);
      setStatus('success');
      return response;
    } catch (error) {
      setError(error);
      setStatus('error');
      throw error;
    }
  }, [asyncFunction]);

  useEffect(() => {
    if (immediate) {
      execute();
    }
  }, [execute, immediate]);

  return { execute, status, data, error };
}
```

---

## Pattern 5: useReducer for Complex State

```jsx
const initialState = { count: 0, loading: false, error: null };

function reducer(state, action) {
  switch (action.type) {
    case 'INCREMENT':
      return { ...state, count: state.count + 1 };
    case 'LOAD_START':
      return { ...state, loading: true };
    case 'LOAD_SUCCESS':
      return { ...state, loading: false, count: action.payload };
    case 'LOAD_ERROR':
      return { ...state, loading: false, error: action.payload };
    default:
      return state;
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  const handleIncrement = () => dispatch({ type: 'INCREMENT' });
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={handleIncrement}>+</button>
    </div>
  );
}
```

---

## Pattern 6: useDebounce Hook

```jsx
function useDebounce(value, delay) {
  const [debouncedValue, setDebouncedValue] = useState(value);

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value);
    }, delay);

    return () => clearTimeout(handler);
  }, [value, delay]);

  return debouncedValue;
}

// Usage: Search with debouncing
function SearchUsers() {
  const [searchTerm, setSearchTerm] = useState('');
  const debouncedSearchTerm = useDebounce(searchTerm, 500);

  useEffect(() => {
    if (debouncedSearchTerm) {
      fetch(`/api/users?q=${debouncedSearchTerm}`)
        .then(res => res.json())
        .then(data => console.log(data));
    }
  }, [debouncedSearchTerm]);

  return (
    <input
      value={searchTerm}
      onChange={(e) => setSearchTerm(e.target.value)}
      placeholder="Search..."
    />
  );
}
```

---

## Pattern 7: useThrottle Hook

```jsx
function useThrottle(value, interval) {
  const [throttledValue, setThrottledValue] = useState(value);
  const lastUpdated = useRef(Date.now());

  useEffect(() => {
    const now = Date.now();
    if (now >= lastUpdated.current + interval) {
      lastUpdated.current = now;
      setThrottledValue(value);
    }
  }, [value, interval]);

  return throttledValue;
}
```

---

## Common Hook Mistakes

### ❌ Missing Dependencies

```jsx
// ❌ Wrong: Will use stale value
useEffect(() => {
  console.log(value);
}, []);

// ✅ Right: Include all dependencies
useEffect(() => {
  console.log(value);
}, [value]);
```

### ❌ Not Cleaning Up

```jsx
// ❌ Wrong: Event listener leaks memory
useEffect(() => {
  window.addEventListener('resize', handleResize);
}, []);

// ✅ Right: Clean up in return
useEffect(() => {
  window.addEventListener('resize', handleResize);
  return () => window.removeEventListener('resize', handleResize);
}, []);
```

### ❌ Infinite Loops

```jsx
// ❌ Wrong: Will loop forever
useEffect(() => {
  setData(newData);
}, [data]);

// ✅ Right: Run once on mount
useEffect(() => {
  setData(newData);
}, []);
```
