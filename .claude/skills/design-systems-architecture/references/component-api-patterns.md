# Component API Patterns

Designing component interfaces (props, slots, children) for composability and clarity.

## Principle: Props vs. Children

**Props** — Configuration inputs (`variant="primary"`, `size="lg"`, `disabled`)

**Children** — Content/slots that go inside the component

**Best practice:** Separate them clearly. Components with unclear boundaries are hard to maintain.

### Pattern 1: Props for Configuration

```tsx
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  onClick?: () => void;
  children: ReactNode;
}

export const Button: React.FC<ButtonProps> = ({
  variant = 'primary',
  size = 'md',
  disabled = false,
  onClick,
  children,
}) => (
  <button
    className={`btn btn-${variant} btn-${size}`}
    onClick={onClick}
    disabled={disabled}
  >
    {children}
  </button>
);

// Usage
<Button variant="primary" size="lg">Click me</Button>
```

✅ **Pros:** Clear what's being configured. Easy to test variants.

### Pattern 2: Children for Content Slots

```tsx
interface CardProps {
  children: ReactNode;
}

export const Card: React.FC<CardProps> = ({ children }) => (
  <div className="card">{children}</div>
);

// Usage
<Card>
  <img src="..." alt="..." />
  <h3>Title</h3>
  <p>Description</p>
</Card>
```

✅ **Pros:** Flexible; doesn't prescribe structure. Easy to compose.

### Pattern 3: Explicit Slots (Multiple Children)

For components with multiple distinct content areas:

```tsx
interface CardProps {
  header?: ReactNode;
  footer?: ReactNode;
  children: ReactNode;
}

export const Card: React.FC<CardProps> = ({ header, footer, children }) => (
  <div className="card">
    {header && <header>{header}</header>}
    <main>{children}</main>
    {footer && <footer>{footer}</footer>}
  </div>
);

// Usage
<Card
  header={<h3>Title</h3>}
  footer={<button>Action</button>}
>
  <p>Content goes here</p>
</Card>
```

✅ **Pros:** Clear structure; each slot has a purpose. Explicit > implicit.

---

## Variant Pattern

**Problem:** Same component, different looks/behaviors.

**Solution:** `variant` prop.

```tsx
interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'ghost' | 'danger';
  children: ReactNode;
}

export const Button: React.FC<ButtonProps> = ({
  variant = 'primary',
  children,
}) => (
  <button className={`btn btn-${variant}`}>{children}</button>
);

// Usage
<Button variant="primary">Save</Button>
<Button variant="secondary">Cancel</Button>
<Button variant="danger">Delete</Button>
```

**When to use variants:**
- Different visual styles (primary, secondary, ghost)
- Different semantic meanings (success, warning, error)
- Different layouts (vertical, horizontal)

**When NOT to use variants:**
- More than 4-5 variants (split into separate components)
- Mutually exclusive features (split into separate components)

---

## Composition Pattern: Headless Components

**Problem:** Component assumes structure; hard to customize without forking.

**Solution:** Render function prop (headless component).

```tsx
interface SelectProps<T> {
  items: T[];
  renderItem: (item: T) => ReactNode;
  onSelect: (item: T) => void;
}

export const Select = <T,>({
  items,
  renderItem,
  onSelect,
}: SelectProps<T>) => (
  <ul>
    {items.map((item) => (
      <li key={id(item)} onClick={() => onSelect(item)}>
        {renderItem(item)}
      </li>
    ))}
  </ul>
);

// Usage
<Select
  items={users}
  renderItem={(user) => <div>{user.name} - {user.email}</div>}
  onSelect={(user) => console.log(user)}
/>
```

✅ **Pros:** Maximum flexibility; component handles structure, you handle content.

---

## Compound Components Pattern

**Problem:** Parent/child components need to communicate; hard to get right.

**Solution:** Context + compound component API.

```tsx
const SelectContext = React.createContext<any>(null);

export const Select: React.FC<{ children: ReactNode }> = ({ children }) => {
  const [open, setOpen] = useState(false);
  const [selected, setSelected] = useState<any>(null);

  return (
    <SelectContext.Provider value={{ open, setOpen, selected, setSelected }}>
      <div className="select">{children}</div>
    </SelectContext.Provider>
  );
};

export const SelectTrigger: React.FC<{ children: ReactNode }> = ({ children }) => {
  const { open, setOpen } = React.useContext(SelectContext);

  return (
    <button onClick={() => setOpen(!open)}>
      {children}
      <span>{open ? '▼' : '▶'}</span>
    </button>
  );
};

export const SelectOptions: React.FC<{ children: ReactNode }> = ({ children }) => {
  const { open } = React.useContext(SelectContext);

  return open ? <ul>{children}</ul> : null;
};

export const SelectOption: React.FC<{
  value: any;
  children: ReactNode;
}> = ({ value, children }) => {
  const { setSelected, setOpen } = React.useContext(SelectContext);

  return (
    <li onClick={() => { setSelected(value); setOpen(false); }}>
      {children}
    </li>
  );
};

// Usage (super clear!)
<Select>
  <SelectTrigger>Choose an option</SelectTrigger>
  <SelectOptions>
    <SelectOption value="opt1">Option 1</SelectOption>
    <SelectOption value="opt2">Option 2</SelectOption>
  </SelectOptions>
</Select>
```

✅ **Pros:** Explicit structure; parent/child relationship is clear.
⚠️ **Cons:** More code; requires understanding Context.

---

## Control vs. Uncontrolled Pattern

### Uncontrolled (Simple Cases)

Component manages its own state.

```tsx
interface TabsProps {
  children: ReactNode;
}

export const Tabs: React.FC<TabsProps> = ({ children }) => {
  const [activeTab, setActiveTab] = useState(0);

  return (
    <div>
      {/* Tab management is internal */}
      <div className="tabs">
        {/* ... */}
      </div>
    </div>
  );
};
```

✅ Simpler API, less code in consuming component.

### Controlled (Complex Cases)

Parent component manages state; component is "dumb."

```tsx
interface TabsProps {
  activeTab: number;
  onTabChange: (index: number) => void;
  children: ReactNode;
}

export const Tabs: React.FC<TabsProps> = ({
  activeTab,
  onTabChange,
  children,
}) => (
  <div>
    {/* Parent controls tab state */}
  </div>
);

// Usage
const [activeTab, setActiveTab] = useState(0);
<Tabs activeTab={activeTab} onTabChange={setActiveTab}>
  {/* ... */}
</Tabs>
```

✅ Parent has full control; great for complex interactions.

---

## HTML Pass-Through (Spread Props)

**Problem:** Component doesn't expose all HTML attributes (aria-*, data-*, etc.).

**Solution:** `...rest` spread props.

```tsx
interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary';
  children: ReactNode;
}

export const Button = React.forwardRef<
  HTMLButtonElement,
  ButtonProps
>(({ variant = 'primary', children, ...rest }, ref) => (
  <button
    ref={ref}
    className={`btn btn-${variant}`}
    {...rest}
  >
    {children}
  </button>
));

// Usage - now supports all button attributes!
<Button
  variant="primary"
  aria-label="Close modal"
  data-testid="close-btn"
  disabled
>
  Close
</Button>
```

✅ Pros: Full control over HTML; components don't need to expose every possible prop.

---

## API Design Checklist

- [ ] **Single responsibility** — Component does one thing
- [ ] **Clear naming** — Prop names are obvious (`variant`, not `type` or `mode`)
- [ ] **Sensible defaults** — Common case works without props
- [ ] **Flexibility** — Can be extended or composed without forking
- [ ] **Type safety** — TypeScript or PropTypes for prop validation
- [ ] **Accessibility** — Spreads HTML props, exposes a11y attributes
- [ ] **Documented examples** — Show common use cases in Storybook

---

## Common API Anti-Patterns

### ❌ Too Many Props

```tsx
<Button
  variant="primary"
  size="lg"
  color="blue"
  backgroundColor="white"
  textColor="black"
  borderRadius="8px"
  borderWidth="2px"
  shadow="md"
  hoverOpacity="0.8"
  disabledOpacity="0.5"
  padding="12px"
  margin="8px"
/>
```

**Problem:** Every client has to pass tons of props. Hard to maintain.

**Fix:** Use variants. Let design tokens handle the details.

```tsx
<Button variant="primary" size="lg">Click</Button>
```

### ❌ Boolean Props for Variants

```tsx
<Button primary secondary ghost danger large small />
```

**Problem:** Unclear which are active. Can pass conflicting props.

**Fix:** Use enum variant prop.

```tsx
<Button variant="primary" size="lg" />
```

### ❌ Event Handlers With No Args

```tsx
<Button onCustomThing={() => {}} />
```

**Problem:** Hard to pass data through. Naming is unclear.

**Fix:** Use standard event handlers with args.

```tsx
<Button onClick={(e: React.MouseEvent) => handleClick(e)} />
```

---

## Real-World Examples

### Badge Component

```tsx
interface BadgeProps {
  variant?: 'default' | 'success' | 'warning' | 'error';
  size?: 'sm' | 'md';
  children: ReactNode;
}

export const Badge: React.FC<BadgeProps> = ({
  variant = 'default',
  size = 'md',
  children,
}) => (
  <span className={`badge badge-${variant} badge-${size}`}>
    {children}
  </span>
);
```

### Modal Component

```tsx
interface ModalProps {
  isOpen: boolean;
  title: string;
  onClose: () => void;
  children: ReactNode;
  footer?: ReactNode;
}

export const Modal: React.FC<ModalProps> = ({
  isOpen,
  title,
  onClose,
  children,
  footer,
}) => (
  isOpen && (
    <div className="modal-overlay" onClick={onClose}>
      <div className="modal" onClick={(e) => e.stopPropagation()}>
        <h2>{title}</h2>
        {children}
        {footer && <div className="modal-footer">{footer}</div>}
      </div>
    </div>
  )
);
```
