---
name: create-component
description: "Create a new React component with TypeScript, CSS module, and tests."
---
# Command: /create-component

## When to use
Use when you need to scaffold a new React component in this repo.

## Inputs
- Component name (PascalCase)
- Target package/app path
- Should include CSS module: yes | no (default: yes)
- Should include test: yes | no (default: yes)

## Steps
1. Locate the target folder and confirm naming and export conventions.
2. Create `ComponentName.tsx` as a named export with `{ComponentName}Props`.
3. Implement the component with a typed props destructure, a single root element, and no side effects in render.
4. If hooks are needed, place them at the top level, use stable dependencies, and avoid conditional usage.
5. Add a `ComponentName.module.css` when CSS modules are enabled.
6. Wire styles with `import styles from "./ComponentName.module.css"` and apply `className={styles.root}` on the root element.
7. Add a colocated `ComponentName.test.tsx` when tests are enabled.
8. Ensure props are typed, use optional chaining (`?.`) and nullish coalescing (`??`) where appropriate, and keep the component small and focused.

## Component template
```tsx
import React from "react";

import styles from "./ComponentName.module.css";

type ComponentNameProps = {
  readonly title?: string;
  readonly children?: React.ReactNode;
  readonly className?: string;
};

const ComponentName = ({
  title,
  children,
  className,
}: ComponentNameProps) => {
  const resolvedTitle = title ?? "";

  return (
    <section className={`${styles.root} ${className ?? ""}`.trim()}>
      {resolvedTitle.length > 0 && <h2 className={styles.title}>{resolvedTitle}</h2>}
      <div className={styles.content}>{children}</div>
    </section>
  );
};

export { ComponentName };
```

## CSS module template
```css
.root {
  display: grid;
  gap: 8px;
}

.title {
  margin: 0;
  font-size: 14px;
  font-weight: 600;
}

.content {
  display: block;
}
```

## Test template
```tsx
import React from "react";
import { render, screen } from "@testing-library/react";

import { ComponentName } from "./ComponentName";

describe("ComponentName", () => {
  it("renders a title and children", () => {
    render(
      <ComponentName title="Example">
        <span>Body</span>
      </ComponentName>
    );

    expect(screen.getByText("Example")).toBeInTheDocument();
    expect(screen.getByText("Body")).toBeInTheDocument();
  });
});
```

## Expected output
- `ComponentName.tsx`
- `ComponentName.module.css` (optional)
- `ComponentName.test.tsx` (optional)

## Notes
- Keep components small and focused.
- Prefer immutable data and avoid unnecessary allocations.

