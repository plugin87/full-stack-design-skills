# CSS-in-JS Patterns

Using styled-components, emotion, and other CSS-in-JS libraries.

---

## styled-components Fundamentals

### Basic Styling

```jsx
import styled from 'styled-components';

const Button = styled.button`
  padding: 12px 16px;
  border-radius: 4px;
  border: none;
  cursor: pointer;
  transition: background-color 200ms;
  
  background-color: #2563eb;
  color: white;
  
  &:hover {
    background-color: #1d4ed8;
  }
  
  &:focus {
    outline: 2px solid #0052cc;
    outline-offset: 2px;
  }
`;

export default function App() {
  return <Button>Click me</Button>;
}
```

### Props-Based Styling

```jsx
const Button = styled.button`
  padding: ${props => props.size === 'lg' ? '16px 24px' : '12px 16px'};
  background-color: ${props => props.primary ? '#2563eb' : '#e5e7eb'};
  color: ${props => props.primary ? 'white' : '#1e293b'};
  border: none;
  border-radius: 4px;
  cursor: pointer;
  
  &:hover {
    background-color: ${props => props.primary ? '#1d4ed8' : '#cbd5e1'};
  }
`;

<Button primary>Primary</Button>
<Button size="lg">Large</Button>
```

### Extending Styles

```jsx
const Button = styled.button`
  padding: 12px 16px;
  border-radius: 4px;
  border: none;
  cursor: pointer;
`;

const PrimaryButton = styled(Button)`
  background-color: #2563eb;
  color: white;
  
  &:hover {
    background-color: #1d4ed8;
  }
`;

const DangerButton = styled(Button)`
  background-color: #ef4444;
  color: white;
  
  &:hover {
    background-color: #dc2626;
  }
`;
```

---

## Component Patterns

### Wrapper Pattern

```jsx
const CardWrapper = styled.div`
  background: white;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  overflow: hidden;
`;

const CardHeader = styled.div`
  padding: 16px;
  border-bottom: 1px solid #e5e7eb;
`;

const CardBody = styled.div`
  padding: 16px;
`;

const CardFooter = styled.div`
  padding: 16px;
  background: #f9fafb;
`;

// Usage
<CardWrapper>
  <CardHeader>Header</CardHeader>
  <CardBody>Body</CardBody>
  <CardFooter>Footer</CardFooter>
</CardWrapper>
```

### Responsive with Mixins

```jsx
const mobile = (...args) => css`
  @media (max-width: 640px) {
    ${css(...args)}
  }
`;

const Box = styled.div`
  padding: 16px;
  font-size: 16px;
  
  ${mobile`
    padding: 8px;
    font-size: 14px;
  `}
`;
```

---

## Global Styles

```jsx
import { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto;
    color: #1e293b;
    background: white;
  }
  
  @media (prefers-color-scheme: dark) {
    body {
      background: #0f172a;
      color: #f1f5f9;
    }
  }
`;

export default function App() {
  return (
    <>
      <GlobalStyle />
      <YourApp />
    </>
  );
}
```

---

## Theming

### Theme Provider

```jsx
import { ThemeProvider } from 'styled-components';

const theme = {
  colors: {
    primary: '#2563eb',
    secondary: '#64748b',
    success: '#10b981',
    error: '#ef4444',
  },
  spacing: {
    xs: '4px',
    sm: '8px',
    md: '16px',
    lg: '24px',
  },
};

export default function App() {
  return (
    <ThemeProvider theme={theme}>
      <YourApp />
    </ThemeProvider>
  );
}
```

### Using Theme in Components

```jsx
const Button = styled.button`
  padding: ${props => props.theme.spacing.md};
  background-color: ${props => props.theme.colors.primary};
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  
  &:hover {
    opacity: 0.9;
  }
`;
```

---

## Dynamic Styling

```jsx
const StyledDiv = styled.div`
  background-color: ${props => {
    if (props.error) return '#fecaca';
    if (props.warning) return '#fef3c7';
    if (props.success) return '#dcfce7';
    return '#f3f4f6';
  }};
`;

<StyledDiv error>Error state</StyledDiv>
<StyledDiv warning>Warning state</StyledDiv>
<StyledDiv success>Success state</StyledDiv>
```

---

## Emotion (Alternative to styled-components)

### Basic Setup

```jsx
import { css } from '@emotion/react';
import styled from '@emotion/styled';

const Button = styled.button`
  padding: 12px 16px;
  background-color: #2563eb;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  
  &:hover {
    background-color: #1d4ed8;
  }
`;

// Or use css
const buttonStyles = css`
  padding: 12px 16px;
  background-color: #2563eb;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
`;

<button css={buttonStyles}>Click me</button>
```

---

## CSS-in-JS Comparison

| Aspect | styled-components | Emotion | Vanilla CSS |
|--------|-------------------|---------|-------------|
| Performance | Good | Very Good | Best |
| Bundle Size | +15KB | +7KB | 0KB |
| Learning Curve | Moderate | Easy | Easy |
| Styling | Tagged Templates | Tagged + css() | Traditional |
| Theming | Built-in | Manual | CSS Variables |
| Debugging | CSS injected | CSS injected | DevTools |

---

## Checklist

- [ ] CSS-in-JS library chosen (styled-components/emotion)
- [ ] Global styles set up
- [ ] Theme provider configured
- [ ] Props-based styling working
- [ ] Responsive design handled
- [ ] Performance optimized
- [ ] Dark mode support
- [ ] Accessibility included
- [ ] Documented
