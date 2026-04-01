## A/B Test: assets.mdc

- **Prompt**: "Every canvas page should have small logo rendered. Use footer slot. Add AppFooterLogo component and logo file"
- **Result A — Without the Rule**:
  - Files:
    - public/footer-logo.svg
    - excalidraw-app/components/AppFooterLogo.tsx
    - excalidraw-app/components/AppFooterLogo.module.css
  - Code:
```tsx
import React from "react";

import styles from "./AppFooterLogo.module.css";

export const AppFooterLogo = React.memo(() => {
  return (
    <div className={styles.footerLogo} aria-hidden="true">
      <img
        className={styles.footerLogoImage}
        src="/footer-logo.svg"
        alt=""
        draggable={false}
      />
    </div>
  );
});
```
```css
.footerLogo {
  display: flex;
  align-items: center;
  pointer-events: none;
  user-select: none;
}

.footerLogoImage {
  width: 16px;
  height: 16px;
  opacity: 0.7;
}
```
- **Result B — With the Rule**:
  - Files:
    - public/icons/footer-logo.svg
    - excalidraw-app/components/AppFooterLogo.tsx
    - excalidraw-app/components/AppFooterLogo.module.css
  - Code:
```tsx
import React from "react";

import styles from "./AppFooterLogo.module.css";

export const AppFooterLogo = React.memo(() => {
  return (
    <div className={styles.root} aria-hidden="true">
      <img
        className={styles.image}
        src="/icons/footer-logo.svg"
        alt=""
        draggable={false}
      />
    </div>
  );
});
```
```css
.root {
  display: inline-flex;
  align-items: center;
}

.image {
  width: 18px;
  height: 18px;
  opacity: 0.8;
  user-select: none;
  pointer-events: none;
}
```
- **Conclusion**: Approach B scopes assets under `public/icons/`, uses a smaller, more explicit CSS module API (`root`/`image`), and standardizes sizing at 18px; Approach A places the asset at the public root, uses more verbose class names (`footerLogo`/`footerLogoImage`), and uses 16px sizing with slightly lower opacity.
