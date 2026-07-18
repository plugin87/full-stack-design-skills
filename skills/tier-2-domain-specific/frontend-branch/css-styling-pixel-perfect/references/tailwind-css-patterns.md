# Tailwind CSS Patterns & Best Practices

Practical patterns for Tailwind CSS configuration and component building.

---

## Configuration Essentials

### Custom Design Tokens

```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        brand: {
          50: '#f0f9ff',
          500: '#2563eb',
          900: '#1e3a8a',
        },
      },
      spacing: {
        128: '32rem',
        144: '36rem',
      },
      borderRadius: {
        '4xl': '2rem',
      },
      fontSize: {
        'xs': ['12px', '16px'],
        'sm': ['14px', '20px'],
      },
    },
  },
};
```

### Dark Mode

```javascript
module.exports = {
  darkMode: 'class', // or 'media'
  theme: {
    colors: {
      white: '#ffffff',
      black: '#000000',
    },
  },
};
```

**Usage:**
```html
<div class="bg-white dark:bg-black">Content</div>
```

---

## Component Patterns

### Using @apply

```css
@layer components {
  .btn {
    @apply px-4 py-2 rounded-lg font-medium transition-colors focus:outline-none;
  }

  .btn-primary {
    @apply bg-blue-500 text-white hover:bg-blue-600;
  }

  .btn-secondary {
    @apply bg-gray-200 text-gray-900 hover:bg-gray-300;
  }
}
```

### Responsive Components

```html
<!-- Mobile first, adapts to larger screens -->
<div class="flex flex-col md:flex-row gap-4">
  <div class="w-full md:w-1/3">Sidebar</div>
  <div class="w-full md:w-2/3">Content</div>
</div>
```

### Dynamic Classnames

```jsx
function Button({ variant = 'primary', size = 'md', disabled }) {
  const baseClasses = 'rounded-lg font-medium transition-colors focus:outline-none';
  
  const variantClasses = {
    'primary': 'bg-blue-500 text-white hover:bg-blue-600',
    'secondary': 'bg-gray-200 text-gray-900 hover:bg-gray-300',
    'ghost': 'text-blue-500 hover:bg-blue-50',
  };

  const sizeClasses = {
    'sm': 'px-2 py-1 text-sm',
    'md': 'px-4 py-2 text-base',
    'lg': 'px-6 py-3 text-lg',
  };

  const disabledClasses = disabled ? 'opacity-50 cursor-not-allowed' : '';

  const classes = `${baseClasses} ${variantClasses[variant]} ${sizeClasses[size]} ${disabledClasses}`;

  return <button className={classes}>Click me</button>;
}
```

Or use `clsx` library:

```jsx
import clsx from 'clsx';

function Button({ variant, size, disabled }) {
  return (
    <button className={clsx(
      'rounded-lg font-medium transition-colors focus:outline-none',
      {
        'bg-blue-500 text-white hover:bg-blue-600': variant === 'primary',
        'bg-gray-200 text-gray-900 hover:bg-gray-300': variant === 'secondary',
        'opacity-50 cursor-not-allowed': disabled,
      },
      size === 'sm' && 'px-2 py-1 text-sm',
      size === 'md' && 'px-4 py-2 text-base',
      size === 'lg' && 'px-6 py-3 text-lg',
    )}>
      Click me
    </button>
  );
}
```

---

## Spacing & Layout

### Design System with CSS Variables

```css
:root {
  --spacing-xs: 0.25rem;
  --spacing-sm: 0.5rem;
  --spacing-md: 1rem;
  --spacing-lg: 1.5rem;
  --spacing-xl: 2rem;
}
```

### Grid Layouts

```html
<!-- Responsive grid -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
  <div>Card 1</div>
  <div>Card 2</div>
  <div>Card 3</div>
</div>

<!-- Auto-fit grid -->
<div class="grid auto-fit gap-4" style="grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));">
  <div>Card 1</div>
  <div>Card 2</div>
  <div>Card 3</div>
</div>
```

---

## Typography

### Text Utilities

```html
<!-- Font sizes (from config) -->
<p class="text-sm">Small text</p>
<p class="text-base">Base text</p>
<p class="text-lg">Large text</p>

<!-- Font weight -->
<p class="font-normal">Normal</p>
<p class="font-semibold">Semibold</p>
<p class="font-bold">Bold</p>

<!-- Line height -->
<p class="leading-tight">Tight line height</p>
<p class="leading-relaxed">Relaxed line height</p>

<!-- Letter spacing -->
<p class="tracking-tight">Tight tracking</p>
<p class="tracking-wide">Wide tracking</p>
```

---

## Common Patterns

### Navbar

```html
<nav class="bg-white shadow-md">
  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <div class="flex justify-between h-16">
      <div class="flex items-center">
        <span class="text-xl font-bold">Logo</span>
      </div>
      <div class="flex items-center gap-4">
        <a href="#" class="hover:text-blue-500">Home</a>
        <a href="#" class="hover:text-blue-500">About</a>
      </div>
    </div>
  </div>
</nav>
```

### Card

```html
<div class="bg-white rounded-lg shadow-md p-6">
  <h2 class="text-xl font-bold mb-2">Card Title</h2>
  <p class="text-gray-600 mb-4">Card description</p>
  <button class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">
    Action
  </button>
</div>
```

### Form

```html
<form class="space-y-4">
  <div>
    <label class="block text-sm font-medium mb-1">Email</label>
    <input type="email" class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent">
  </div>
  
  <div>
    <label class="block text-sm font-medium mb-1">Message</label>
    <textarea class="w-full px-3 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"></textarea>
  </div>

  <button class="w-full bg-blue-500 text-white py-2 rounded-lg hover:bg-blue-600">
    Submit
  </button>
</form>
```

---

## Tailwind Best Practices

- [ ] Extend theme for custom brand colors
- [ ] Use @apply for repeated patterns (not for single use)
- [ ] Keep Tailwind config organized
- [ ] Use clsx for dynamic classes
- [ ] Mobile-first (sm:, md:, lg: prefixes)
- [ ] Consistent spacing using spacing scale
- [ ] Dark mode support (@media or class)
- [ ] Accessibility (focus states, colors)
- [ ] Performance (purge unused CSS)
