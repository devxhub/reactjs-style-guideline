# React.js Style Guide

<div align="center">

![React Version](https://img.shields.io/badge/React-v19-blue?style=for-the-badge&logo=react)
![TypeScript](https://img.shields.io/badge/TypeScript-Latest-blue?style=for-the-badge&logo=typescript)

**A comprehensive guide for building scalable, performant, and maintainable React.js applications**

</div>

---

## 📋 Table of Contents

| Section                                                                 | Description                                                 |
| ----------------------------------------------------------------------- | ----------------------------------------------------------- |
| [Project Setup](#project-setup)                                         | Initial setup instructions and environment configuration    |
| [File Structure](#file-structure)                                       | Recommended file and folder organization for scalability    |
| [Naming Conventions](#naming-conventions)                               | Standard naming rules for files, folders, and components    |
| [Component Structure](#component-structure)                             | Guidelines for organizing and structuring React components  |
| [TypeScript Usage](#typescript-usage)                                   | Best practices for using TypeScript across the codebase     |
| [Hooks](#hooks)                                                         | Usage and conventions for React and custom hooks            |
| [Props and State](#props-and-state)                                     | Managing props, state, and data flow within components      |
| [JSX Guidelines](#jsx-guidelines)                                       | Writing clean, readable, and performant JSX                 |
| [Styling](#styling)                                                     | Styling approaches using Tailwind CSS or CSS Modules        |
| [Error Handling](#error-handling)                                       | Handling runtime errors and providing user feedback         |
| [Performance Optimizations](#performance-optimizations)                 | Techniques for improving performance and reducing bloat     |
| [Testing](#testing)                                                     | Strategies for unit, integration, and end-to-end testing    |
| [Accessibility](#accessibility)                                         | Ensuring inclusive and accessible user interfaces           |
| [Internationalization](#internationalization)                           | Supporting multiple languages and regional formats          |
| [ESLint and Prettier Configuration](#eslint-and-prettier-configuration) | Enforcing consistent code style with linting and formatting |
| [Tooling and Workflow](#tooling-and-workflow)                           | Development tools, scripts, and team collaboration workflow |
| [References](#references)                                               | Useful links, libraries, and additional documentation       |

---

## Project Setup

- **Toolchain**: Use Vite or Create React App (CRA) for new projects. Vite is preferred for faster builds and modern ES modules.
- **TypeScript**: Initialize with TypeScript (`--template typescript` for CRA or `vite create` with TypeScript template).
- **Dependencies**:
  - Core: `react`, `react-dom`, `typescript`, `@types/react`, `@types/react-dom`.
  - Linting/Formatting: `eslint`, `prettier`, `eslint-config-airbnb-typescript`, `eslint-plugin-react`, `eslint-plugin-react-hooks`.
  - Testing: `jest`, `@testing-library/react`, `@testing-library/jest-dom`.
  - Styling: `tailwindcss` (optional) or `css-modules`.
- **tsconfig.json**: Use strict mode and target modern JavaScript (`esnext` or `es2020`).

Example `tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "esnext",
    "module": "esnext",
    "jsx": "react-jsx",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "baseUrl": "src"
  },
  "include": ["src"]
}
```

## File Structure

- **Feature-based organization**: Group files by feature (e.g., `auth/`, `user/`) rather than type (e.g., `components/`, `hooks/`).
- **Co-location**: Place related files (components, hooks, tests, styles) in the same directory.
- **File extensions**: Use `.tsx` for React components and `.ts` for utilities, hooks, or types.
- **Avoid deep nesting**: Keep folder hierarchies shallow (3–4 levels max).
- **Index files**: Use `index.tsx` only for feature entry points, not for exporting multiple files.

Example:

```
src/
  assets/
    images/
    fonts/
  features/
    auth/
      LoginForm.tsx
      useAuth.ts
      AuthContext.tsx
      LoginForm.test.tsx
      styles.module.css
    user/
      UserProfile.tsx
      UserCard.tsx
      useUserData.ts
      UserProfile.test.tsx
      styles.module.css
  components/
    shared/
      Button.tsx
      Input.tsx
  hooks/
    useFetch.ts
  types/
    user.ts
    auth.ts
  utils/
    api.ts
    formatDate.ts
  App.tsx
  main.tsx
```

## Naming Conventions

- **Components**: PascalCase for component names and files (e.g., `UserProfile.tsx`).
- **Instances**: camelCase for component instances (e.g., `const userProfile = <UserProfile />;`).
- **Hooks**: Prefix with `use` and use camelCase (e.g., `useUserFetch`).
- **Functions/Variables**: camelCase (e.g., `fetchUser`, `userData`).
- **Constants**: UPPER_SNAKE_CASE (e.g., `API_BASE_URL`).
- **Types/Interfaces**: PascalCase (e.g., `UserProps`, `AuthState`).
- **Files**: Match file names to exported content (e.g., `useAuth.ts` for `useAuth` hook).
- **Avoid abbreviations**: Use descriptive names (e.g., `userProfile` over `usrProf`).

## Component Structure

- **Functional components**: Use function components with hooks instead of class components.
- **One component per file**: Export one main component per file; allow small stateless components as helpers.
- **Default exports**: Use default exports for components, named exports for utilities or hooks.
- **Props typing**: Always define props with TypeScript interfaces.
- **Folder for complex components**: For components with multiple related files, create a folder (e.g., `UserProfile/UserProfile.tsx`).

Example:

```tsx
// features/user/UserProfile.tsx
import React from "react";
import styles from "./styles.module.css";

interface UserProfileProps {
  name: string;
  email: string;
  avatar?: string;
}

const UserProfile: React.FC<UserProfileProps> = ({ name, email, avatar }) => (
  <div className={styles.profile}>
    {avatar && (
      <img src={avatar} alt={`${name}'s avatar`} className={styles.avatar} />
    )}
    <h2 className={styles.name}>{name}</h2>
    <p className={styles.email}>{email}</p>
  </div>
);

export default UserProfile;
```

## TypeScript Usage

- **Interfaces for props**: Use interfaces for component props (e.g., `interface ButtonProps`).
- **Types for utilities**: Use `type` for complex types or unions (e.g., `type UserStatus = 'active' | 'inactive'`).
- **Avoid `any`**: Use `unknown` or specific types; refactor if `any` is tempting.
- **Generics**: Name generics with `T`, `U`, etc. (e.g., `Array<T>`).
- **Type inference**: Rely on TypeScript’s inference for simple cases (e.g., `useState`).
- **Explicit event types**: Use React’s event types (e.g., `React.MouseEvent<HTMLButtonElement>`).
- **Utility types**: Leverage TypeScript utilities like `Partial`, `Pick`, and `Omit`.

Example:

```tsx
interface FormProps {
  onSubmit: (data: FormData) => void;
  initialValues?: Partial<FormData>;
}

interface FormData {
  name: string;
  email: string;
}

const Form: React.FC<FormProps> = ({ onSubmit, initialValues }) => {
  const [formData, setFormData] = React.useState<FormData>({
    name: initialValues?.name ?? "",
    email: initialValues?.email ?? "",
  });

  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setFormData({ ...formData, [e.target.name]: e.target.value });
  };

  const handleSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    onSubmit(formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="name" value={formData.name} onChange={handleChange} />
      <input name="email" value={formData.email} onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
};

export default Form;
```

## Hooks

- **Custom hooks**: Extract shared logic into custom hooks (e.g., `useFetch`, `useAuth`).
- **Hook naming**: Always prefix with `use` (e.g., `useForm`).
- **Rules of Hooks**: Call hooks only at the top level of components or custom hooks, not in loops or conditionals.
- **Dependency arrays**: Explicitly list all dependencies in `useEffect`, `useCallback`, and `useMemo`.
- **Single responsibility**: Keep hooks focused on one purpose.

Example:

```ts
// hooks/useFetch.ts
import { useState, useEffect } from "react";

interface FetchState<T> {
  data: T | null;
  loading: boolean;
  error: Error | null;
}

const useFetch = <T>(url: string): FetchState<T> => {
  const [state, setState] = useState<FetchState<T>>({
    data: null,
    loading: true,
    error: null,
  });

  useEffect(() => {
    const controller = new AbortController();
    const fetchData = async () => {
      try {
        setState((prev) => ({ ...prev, loading: true }));
        const response = await fetch(url, { signal: controller.signal });
        if (!response.ok) throw new Error("Network error");
        const result = await response.json();
        setState({ data: result, loading: false, error: null });
      } catch (error) {
        if (error instanceof Error && error.name !== "AbortError") {
          setState({ data: null, loading: false, error });
        }
      }
    };
    fetchData();
    return () => controller.abort();
  }, [url]);

  return state;
};

export default useFetch;
```

## Props and State

- **Props naming**: Use descriptive names; avoid DOM prop names for custom props (e.g., don’t use `class` or `style`).
- **Destructuring**: Destructure props in the function signature.
- **Default values**: Use TypeScript’s optional properties or default parameters.
- **State management**:
  - Local: Use `useState` for simple state.
  - Complex: Use `useReducer` for state with multiple transitions.
  - Global: Use Context API or libraries like Zustand for shared state.
- **Immutable updates**: Always return new state objects (e.g., spread operator).

Example:

```tsx
interface CounterProps {
  initialCount?: number;
  onCountChange?: (count: number) => void;
}

const Counter: React.FC<CounterProps> = ({
  initialCount = 0,
  onCountChange,
}) => {
  const [count, setCount] = React.useState(initialCount);

  const increment = () => {
    setCount((prev) => {
      const newCount = prev + 1;
      onCountChange?.(newCount);
      return newCount;
    });
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button type="button" onClick={increment}>
        Increment
      </button>
    </div>
  );
};

export default Counter;
```

## JSX Guidelines

- **Self-closing tags**: Use self-closing tags for elements without children (e.g., `<img />`).
- **Parentheses**: Wrap multiline JSX in parentheses.
- **Fragments**: Use `<></>` for grouping without extra DOM nodes.
- **Quotes**: Double quotes for JSX attributes, single quotes for JavaScript strings.
- **Boolean props**: Omit values for boolean props (e.g., `<input disabled />`).
- **Avoid inline styles**: Use CSS classes instead.
- **Conditional rendering**: Use ternary operators or `&&` for simple conditions; extract complex logic to variables.

Example:

```tsx
const TodoList: React.FC<{
  items: string[];
  onDelete: (index: number) => void;
}> = ({ items, onDelete }) => (
  <>
    {items.length > 0 ? (
      <ul>
        {items.map((item, index) => (
          <li key={index}>
            {item}
            <button type="button" onClick={() => onDelete(index)}>
              Delete
            </button>
          </li>
        ))}
      </ul>
    ) : (
      <p>No items to display</p>
    )}
  </>
);
```

## Styling

- **CSS Modules**: Use CSS Modules for scoped styles (e.g., `styles.module.css`).
- **Tailwind CSS**: Preferred for rapid development; configure via `tailwind.config.js`.
- **Class naming**: Use descriptive, semantic names (e.g., `cardHeader` over `header`).
- **No inline styles**: Reserve inline styles for dynamic values (e.g., `style={{ width: `${progress}%` }}`).
- **CSS organization**: Group related styles in logical sections within CSS files.

Example with CSS Modules:

```tsx
// components/Button.tsx
import styles from "./Button.module.css";

interface ButtonProps {
  label: string;
  variant?: "primary" | "secondary";
  onClick: () => void;
}

const Button: React.FC<ButtonProps> = ({
  label,
  variant = "primary",
  onClick,
}) => (
  <button
    type="button"
    className={`${styles.button} ${
      variant === "primary" ? styles.primary : styles.secondary
    }`}
    onClick={onClick}
  >
    {label}
  </button>
);

export default Button;
```

```css
/* Button.module.css */
.button {
  padding: 8px 16px;
  border-radius: 4px;
  font-size: 16px;
  cursor: pointer;
}

.primary {
  background-color: #007bff;
  color: white;
}

.secondary {
  background-color: #6c757d;
  color: white;
}
```

## Error Handling

- **Error boundaries**: Use error boundaries for component-level error handling.
- **Try-catch in effects**: Handle errors in async operations within `useEffect` or custom hooks.
- **User feedback**: Display user-friendly error messages using state or toast notifications.
- **Type-safe errors**: Define error types for API responses.

Example:

```tsx
// components/ErrorBoundary.tsx
import React from "react";

interface ErrorBoundaryState {
  hasError: boolean;
  error: Error | null;
}

class ErrorBoundary extends React.Component<{}, ErrorBoundaryState> {
  state: ErrorBoundaryState = { hasError: false, error: null };

  static getDerivedStateFromError(error: Error): ErrorBoundaryState {
    return { hasError: true, error };
  }

  render() {
    if (this.state.hasError) {
      return <p>Something went wrong: {this.state.error?.message}</p>;
    }
    return this.props.children;
  }
}

export default ErrorBoundary;
```

## Performance Optimizations

- **Memoization**: Use `React.memo`, `useMemo`, and `useCallback` to avoid unnecessary re-renders.
- **Lazy loading**: Implement `React.lazy` and `Suspense` for code splitting.
- **List keys**: Use unique, stable keys for lists (avoid indices unless stable).
- **Avoid inline functions**: Define event handlers outside JSX.
- **Batching updates**: Leverage React v19’s automatic batching for state updates.

Example:

```tsx
const Item = React.memo(
  ({
    id,
    name,
    onSelect,
  }: {
    id: string;
    name: string;
    onSelect: () => void;
  }) => {
    console.log(`Rendering ${name}`);
    return (
      <li onClick={onSelect} key={id}>
        {name}
      </li>
    );
  }
);

const List: React.FC<{ items: { id: string; name: string }[] }> = ({
  items,
}) => {
  const handleSelect = useCallback(() => {
    console.log("Item selected");
  }, []);

  return (
    <ul>
      {items.map((item) => (
        <Item
          key={item.id}
          id={item.id}
          name={item.name}
          onSelect={handleSelect}
        />
      ))}
    </ul>
  );
};
```

## Testing

- **Tools**: Use Jest and React Testing Library for unit and integration tests.
- **Test behavior**: Test user interactions and rendering, not implementation details.
- **Mocking**: Mock APIs with `msw` or Jest mocks; mock hooks with `@testing-library/react-hooks`.
- **Coverage**: Aim for 80%+ coverage on critical components.
- **Test file naming**: Name tests `[Component].test.tsx` in the same directory.

Example:

```tsx
// features/auth/LoginForm.test.tsx
import { render, screen, fireEvent } from "@testing-library/react";
import LoginForm from "./LoginForm";

test("submits form with valid input", async () => {
  const handleSubmit = jest.fn();
  render(<LoginForm onSubmit={handleSubmit} />);

  fireEvent.change(screen.getByLabelText(/email/i), {
    target: { value: "test@example.com" },
  });
  fireEvent.change(screen.getByLabelText(/password/i), {
    target: { value: "password123" },
  });
  fireEvent.click(screen.getByText(/submit/i));

  expect(handleSubmit).toHaveBeenCalledWith({
    email: "test@example.com",
    password: "password123",
  });
});
```

## Accessibility

- **Semantic HTML**: Use appropriate HTML elements (e.g., `<nav>`, `<main>`).
- **ARIA attributes**: Add ARIA roles and attributes where needed (e.g., `aria-label`).
- **Keyboard navigation**: Ensure all interactive elements are keyboard-accessible.
- **Testing**: Use `axe` or `@testing-library/react` for accessibility checks.
- **Focus management**: Manage focus for modals and dynamic content.

Example:

```tsx
const Modal: React.FC<{ isOpen: boolean; onClose: () => void }> = ({
  isOpen,
  onClose,
  children,
}) => {
  if (!isOpen) return null;

  return (
    <div role="dialog" aria-modal="true" className="modal">
      <div className="modal-content">
        <button type="button" onClick={onClose} aria-label="Close modal">
          &times;
        </button>
        {children}
      </div>
    </div>
  );
};
```

## Internationalization

- **Library**: Use `react-i18next` for internationalization.
- **Translation files**: Store translations in JSON files (e.g., `public/locales/en.json`).
- **Type safety**: Define translation keys with TypeScript.
- **Pluralization**: Handle plural forms and dynamic values.

Example:

```tsx
// components/Greeting.tsx
import { useTranslation } from "react-i18next";

const Greeting: React.FC<{ name: string }> = ({ name }) => {
  const { t } = useTranslation();
  return <p>{t("greeting", { name })}</p>;
};

export default Greeting;
```

```json
// public/locales/en.json
{
  "greeting": "Hello, {{name}}!"
}
```

## ESLint and Prettier Configuration

- **ESLint**: Use `eslint-config-airbnb-typescript` with React and hooks plugins.
- **Prettier**: Enforce consistent formatting.
- **Husky/Lint-staged**: Run linting and formatting on staged files before commits.
- **VS Code**: Enable ESLint and Prettier extensions.

Example `.eslintrc.json`:

```json
{
  "env": {
    "browser": true,
    "es2021": true,
    "jest": true
  },
  "extends": [
    "airbnb",
    "airbnb-typescript",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended",
    "plugin:prettier/recommended"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "./tsconfig.json",
    "ecmaFeatures": { "jsx": true }
  },
  "plugins": ["react", "react-hooks", "prettier"],
  "rules": {
    "react/jsx-filename-extension": [2, { "extensions": [".tsx"] }],
    "react/prop-types": "off",
    "react/function-component-definition": [
      2,
      { "namedComponents": "function-declaration" }
    ],
    "import/prefer-default-export": "off",
    "prettier/prettier": "error"
  }
}
```

Example `.prettierrc`:

```json
{
  "printWidth": 100,
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "arrowParens": "avoid"
}
```

## Tooling and Workflow

- **Version control**: Use Git with meaningful commit messages (e.g., `feat: add login form`).
- **CI/CD**: Configure GitHub Actions or similar for linting, testing, and deployment.
- **Code reviews**: Enforce PR reviews with at least one approver.
- **Documentation**: Maintain a `CONTRIBUTING.md` and update this README.
- **Type checking**: Run `tsc --noEmit` in CI to catch type errors.

Example GitHub Action:

```yaml
name: CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: "18"
      - run: npm install
      - run: npm run lint
      - run: npm run test
      - run: npm run build
```

## References

- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)
- [React Documentation](https://react.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook)
- [React Testing Library](https://testing-library.com/docs/react-testing-library/intro)
- [Tailwind CSS](https://tailwindcss.com)

This guide should be reviewed and updated quarterly to reflect new React.js features, and team learnings. Regular team code reviews should ensure adherence to these standards.
