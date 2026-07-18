# Atomic Design to Code: Practical Application

Translating Atomic Design principles into folder structure, component organization, and code.

## The Five Levels in Practice

### Atoms (Foundation)

**Definition:** Smallest, non-breakable components. Represent primitive UI elements.

**Examples:**
- Button
- Input
- Label
- Icon
- Badge

**File structure:**
```
atoms/
├── Button/
│   ├── Button.tsx
│   ├── Button.stories.tsx
│   └── README.md
├── Input/
├── Label/
├── Icon/
└── Badge/
```

**Characteristics:**
- Single prop (usually just content or state)
- No child components
- Pure presentation
- Fully accessible by default

**Code example:**
```tsx
// Button.tsx
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
}) => {
  return (
    <button
      className={`btn btn-${variant} btn-${size} ${disabled ? 'disabled' : ''}`}
      onClick={onClick}
      disabled={disabled}
    >
      {children}
    </button>
  );
};
```

---

### Molecules (Simple Combinations)

**Definition:** Combination of atoms that form a distinct UI unit with a single responsibility.

**Examples:**
- FormField (label + input + error message)
- SearchBox (input + button + icon)
- Card Header (icon + title + subtitle)
- Breadcrumb (multiple links + separators)

**File structure:**
```
molecules/
├── FormField/
│   ├── FormField.tsx
│   ├── FormField.stories.tsx
│   └── README.md
├── SearchBox/
├── CardHeader/
└── Breadcrumb/
```

**Characteristics:**
- Composes atoms (no other molecules)
- Single responsibility (one purpose)
- Moderate complexity
- Reusable across contexts

**Code example:**
```tsx
// FormField.tsx
interface FormFieldProps {
  label: string;
  inputId: string;
  error?: string;
  required?: boolean;
  children: ReactNode; // Usually <input />
}

export const FormField: React.FC<FormFieldProps> = ({
  label,
  inputId,
  error,
  required,
  children,
}) => {
  return (
    <div className="form-field">
      <label htmlFor={inputId}>
        {label}
        {required && <span aria-label="required">*</span>}
      </label>
      {children}
      {error && <div className="error">{error}</div>}
    </div>
  );
};

// Usage:
// <FormField label="Email" inputId="email" error="Invalid email">
//   <Input type="email" id="email" />
// </FormField>
```

---

### Organisms (Complex Combinations)

**Definition:** Combination of molecules and/or atoms that form a section of an interface.

**Examples:**
- ContactForm (multiple FormFields + Button)
- ProductCard (Image + Title + Description + Button)
- Header (Logo + Navigation + SearchBox)
- Footer (Links + Social Icons + Copyright)

**File structure:**
```
organisms/
├── ContactForm/
│   ├── ContactForm.tsx
│   ├── ContactForm.stories.tsx
│   └── README.md
├── ProductCard/
├── Header/
└── Footer/
```

**Characteristics:**
- Composes molecules and atoms
- Complex logic (state, validation, API calls)
- Handles interactions and side effects
- Often stateful

**Code example:**
```tsx
// ContactForm.tsx
interface ContactFormProps {
  onSubmit?: (data: FormData) => void;
}

export const ContactForm: React.FC<ContactFormProps> = ({ onSubmit }) => {
  const [formData, setFormData] = useState({ name: '', email: '', message: '' });
  const [errors, setErrors] = useState<Record<string, string>>({});

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    // Validation logic...
    onSubmit?.(formData);
  };

  return (
    <form onSubmit={handleSubmit} className="contact-form">
      <FormField label="Name" inputId="name" error={errors.name} required>
        <Input
          id="name"
          value={formData.name}
          onChange={(e) => setFormData({ ...formData, name: e.target.value })}
        />
      </FormField>
      
      <FormField label="Email" inputId="email" error={errors.email} required>
        <Input
          id="email"
          type="email"
          value={formData.email}
          onChange={(e) => setFormData({ ...formData, email: e.target.value })}
        />
      </FormField>

      <FormField label="Message" inputId="message" required>
        <textarea
          id="message"
          value={formData.message}
          onChange={(e) => setFormData({ ...formData, message: e.target.value })}
        />
      </FormField>

      <Button type="submit" variant="primary">Send Message</Button>
    </form>
  );
};
```

---

### Templates (Layout & Structure)

**Definition:** Wireframe-level layout structure with organisms and molecules positioned. No real content yet.

**Examples:**
- BlogPostTemplate (Header + Sidebar + Content Area + Footer)
- ProductListingTemplate (Header + Filters + Grid + Pagination)
- DashboardTemplate (Sidebar Nav + Content Area)

**File structure:**
```
templates/
├── BlogPostTemplate/
│   ├── BlogPostTemplate.tsx
│   ├── BlogPostTemplate.stories.tsx
│   └── README.md
├── ProductListingTemplate/
└── DashboardTemplate/
```

**Characteristics:**
- Defines layout and structure
- Contains organisms/molecules but no real data
- Accepts content as props/slots
- Reusable across multiple pages

**Code example:**
```tsx
// BlogPostTemplate.tsx
interface BlogPostTemplateProps {
  header: ReactNode;
  sidebar: ReactNode;
  content: ReactNode;
  footer: ReactNode;
}

export const BlogPostTemplate: React.FC<BlogPostTemplateProps> = ({
  header,
  sidebar,
  content,
  footer,
}) => {
  return (
    <div className="blog-template">
      <header>{header}</header>
      <div className="blog-layout">
        <aside>{sidebar}</aside>
        <main>{content}</main>
      </div>
      <footer>{footer}</footer>
    </div>
  );
};

// Usage (no real content yet):
// <BlogPostTemplate
//   header={<Header />}
//   sidebar={<Sidebar />}
//   content={<article>Placeholder</article>}
//   footer={<Footer />}
// />
```

---

### Pages (Real Content)

**Definition:** Template with actual data/content filled in. Real, functional page users see.

**Examples:**
- Blog post page with actual article text
- Product listing page with actual products
- User profile page with actual user data

**File structure:**
```
pages/
├── BlogPostPage/
│   ├── BlogPostPage.tsx
│   ├── BlogPostPage.stories.tsx
│   └── README.md
├── ProductListingPage/
└── UserProfilePage/
```

**Characteristics:**
- Combines template + real data
- Fetches data, handles loading/errors
- Fully functional, ready for production
- Not part of component library; lives in app code

**Code example:**
```tsx
// BlogPostPage.tsx (in your app, not the design system)
export const BlogPostPage: React.FC<{ postId: string }> = ({ postId }) => {
  const [post, setPost] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(`/api/posts/${postId}`)
      .then(r => r.json())
      .then(data => { setPost(data); setLoading(false); });
  }, [postId]);

  if (loading) return <div>Loading...</div>;
  if (!post) return <div>Not found</div>;

  return (
    <BlogPostTemplate
      header={<Header />}
      sidebar={<Sidebar categories={post.categories} />}
      content={<article>{post.content}</article>}
      footer={<Footer />}
    />
  );
};
```

---

## Practical Organization

### Option 1: Strict Atomic Hierarchy (Recommended for Starting Out)

```
components/
├── atoms/
│   ├── Button/
│   ├── Input/
│   └── ...
├── molecules/
│   ├── FormField/
│   ├── SearchBox/
│   └── ...
├── organisms/
│   ├── ContactForm/
│   ├── Header/
│   └── ...
└── templates/
    ├── BlogPostTemplate/
    └── ...
```

**Pros:**
- Clear mental model
- Easy for new contributors to find things
- Obvious where to add new components

**Cons:**
- Can feel rigid
- Debates about which level a component belongs to
- Deep nesting makes imports verbose

### Option 2: Flat with Complexity Indicators (Common in Practice)

```
components/
├── Button/              # Atom
├── Input/               # Atom
├── FormField/           # Molecule (atom composition)
├── ContactForm/         # Organism (molecule + atom composition)
├── Header/              # Organism
├── BlogPostTemplate/    # Template
└── index.ts             # Barrel export
```

**Pros:**
- Simpler folder structure
- Faster to navigate
- Reduced import nesting

**Cons:**
- Less obvious which components are atoms vs. organisms
- Requires discipline (no one-off components)

### Option 3: By Feature (For Monorepos)

```
design-system/
├── core/           # Core atoms (Button, Input, etc.)
├── forms/          # Form-specific (FormField, FormGroup, etc.)
├── navigation/     # Nav-specific (Nav, Breadcrumb, etc.)
├── layout/         # Layout-specific (Container, Grid, etc.)
└── data-display/   # Table, List, Card, etc.
```

**Pros:**
- Organized by use case/domain
- Easy to discover related components
- Natural for multi-brand systems

**Cons:**
- Components across features may duplicate
- Requires thoughtful organization upfront

---

## When to Stop Using Atomic Design

Atomic Design is useful for thinking through hierarchy but isn't sacred. When to break the hierarchy:

- **Shared patterns** — Some molecules appear everywhere; make them atoms
- **Domain-specific needs** — Medical UI has different "atoms" than e-commerce
- **Organizational reality** — If your team works by feature (auth, checkout, dashboard), organize by feature

The point isn't to be "correct" by Atomic Design; it's to have a clear, consistent structure your team understands.

---

## Validation Checklist

- [ ] Every component has a single responsibility
- [ ] Components compose cleanly (atoms → molecules → organisms)
- [ ] No circular dependencies (A depends on B, B doesn't depend on A)
- [ ] Each level has clear examples (stories, documentation)
- [ ] New contributors can find where to add components
- [ ] No "misc" or "utility" folders hiding unclear components
