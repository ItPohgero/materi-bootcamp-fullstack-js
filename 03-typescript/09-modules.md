# 9. Modules

## ES6 Modules

Bun support ES6 modules secara native.

### Export

```typescript
// user.ts
export interface User {
    id: number;
    name: string;
    email: string;
}

export function createUser(name: string, email: string): User {
    return {
        id: Date.now(),
        name,
        email
    };
}

export const MAX_USERS = 1000;
```

### Import

```typescript
// app.ts
import { User, createUser, MAX_USERS } from './user.js';

const user = createUser("John", "john@example.com");
console.log(user);
console.log(MAX_USERS);
```

---

## Default Exports

```typescript
// calculator.ts
export default class Calculator {
    add(a: number, b: number): number {
        return a + b;
    }
}

// Or
class Calculator {
    add(a: number, b: number): number {
        return a + b;
    }
}

export default Calculator;
```

```typescript
// app.ts
import Calculator from './calculator.js';

const calc = new Calculator();
console.log(calc.add(5, 3));
```

### Named + Default Export

```typescript
// math.ts
export default function add(a: number, b: number): number {
    return a + b;
}

export function subtract(a: number, b: number): number {
    return a - b;
}

export const PI = 3.14159;
```

```typescript
// app.ts
import add, { subtract, PI } from './math.js';

console.log(add(5, 3));
console.log(subtract(10, 4));
console.log(PI);
```

---

## Export All

```typescript
// utils.ts
export function add(a: number, b: number): number {
    return a + b;
}

export function multiply(a: number, b: number): number {
    return a * b;
}
```

```typescript
// app.ts
import * as MathUtils from './utils.js';

console.log(MathUtils.add(5, 3));
console.log(MathUtils.multiply(5, 3));
```

---

## Re-exporting

```typescript
// types/user.ts
export interface User {
    id: number;
    name: string;
}

// types/product.ts
export interface Product {
    id: number;
    name: string;
    price: number;
}

// types/index.ts
export { User } from './user.js';
export { Product } from './product.js';
export type { User as UserType } from './user.js';
```

```typescript
// app.ts
import { User, Product } from './types/index.js';
```

---

## Type-Only Imports/Exports

```typescript
// types.ts
export interface User {
    name: string;
}

export function createUser(name: string): User {
    return { name };
}
```

```typescript
// app.ts
// Import only type (no runtime code)
import type { User } from './types.js';

// Import function normally
import { createUser } from './types.js';

const user: User = createUser("John");
```

### Why Type-Only Imports?

- Smaller bundle size
- Clear intention
- Faster compilation

```typescript
// Export type only
export type { User, Product };

// Import type only
import type { User } from './types.js';
```

---

## Module Patterns

### Barrel Exports

```typescript
// src/models/user.ts
export interface User {
    id: number;
    name: string;
}

// src/models/product.ts
export interface Product {
    id: number;
    name: string;
}

// src/models/index.ts (barrel file)
export * from './user.js';
export * from './product.js';
```

```typescript
// app.ts
import { User, Product } from './models/index.js';
```

---

## Path Aliases

Configure di `tsconfig.json`:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"],
      "@models/*": ["src/models/*"],
      "@utils/*": ["src/utils/*"]
    }
  }
}
```

```typescript
// Instead of
import { User } from '../../../models/user.js';

// Use
import { User } from '@/models/user.js';
import { helpers } from '@utils/helpers.js';
```

---

## CommonJS Compatibility

Bun support CommonJS juga.

```typescript
// math.ts
export function add(a: number, b: number): number {
    return a + b;
}

// Can also use CommonJS
module.exports = {
    add(a: number, b: number): number {
        return a + b;
    }
};
```

```typescript
// app.ts
// ES6
import { add } from './math.js';

// CommonJS
const { add } = require('./math.js');
```

---

## Namespace (Legacy)

Tidak recommended untuk modern TypeScript. Use modules.

```typescript
// Not recommended
namespace MathUtils {
    export function add(a: number, b: number): number {
        return a + b;
    }
}

// Use ES6 modules instead
export function add(a: number, b: number): number {
    return a + b;
}
```

---

## Practical Examples

### Project Structure

```
src/
├── index.ts
├── types/
│   ├── index.ts
│   ├── user.ts
│   └── product.ts
├── services/
│   ├── userService.ts
│   └── productService.ts
├── utils/
│   ├── helpers.ts
│   └── validators.ts
└── config/
    └── database.ts
```

### Types Module

```typescript
// src/types/user.ts
export interface User {
    id: number;
    username: string;
    email: string;
}

export interface UserCredentials {
    username: string;
    password: string;
}

export type UserRole = "admin" | "user" | "guest";
```

```typescript
// src/types/index.ts
export * from './user.js';
export * from './product.js';
```

### Service Module

```typescript
// src/services/userService.ts
import type { User, UserCredentials } from '@/types/user.js';

export class UserService {
    private users: User[] = [];
    
    async login(credentials: UserCredentials): Promise<User | null> {
        // Login logic
        return null;
    }
    
    async getUser(id: number): Promise<User | null> {
        return this.users.find(u => u.id === id) || null;
    }
}
```

### Main Application

```typescript
// src/index.ts
import { UserService } from './services/userService.js';
import type { User } from './types/index.js';

const userService = new UserService();

const user = await userService.getUser(1);
if (user) {
    console.log(user.username);
}
```

---

## Best Practices

### DO (Lakukan)

1. **Use named exports untuk multiple exports**
   ```typescript
   export const config = { };
   export function helper() { }
   ```

2. **Use barrel files (index.ts) untuk group exports**
   ```typescript
   // types/index.ts
   export * from './user.js';
   export * from './product.js';
   ```

3. **Use path aliases untuk clean imports**
   ```typescript
   import { User } from '@/types/user.js';
   ```

4. **Use type-only imports when possible**
   ```typescript
   import type { User } from './types.js';
   ```

5. **Organize by feature/domain**
   ```
   src/
   ├── users/
   │   ├── user.types.ts
   │   ├── user.service.ts
   │   └── user.controller.ts
   └── products/
       └── ...
   ```

### DON'T (Jangan)

1. **Jangan mix default dan named exports tanpa alasan**
2. **Jangan create circular dependencies**
3. **Jangan export everything (be selective)**
4. **Jangan use namespaces (use ES modules)**

---

## Rangkuman

- ES6 modules adalah standard
- Named exports: `export const/function/class`
- Default exports: `export default`
- Import dengan destructuring
- Type-only imports untuk types
- Barrel files untuk group exports
- Path aliases untuk clean imports
- Re-export untuk public API
- Bun support ES6 modules native
- Avoid circular dependencies
- Organize by feature

---

**Sebelumnya:** [08. Advanced Types](08-advanced-types.md)  
**Selanjutnya:** [10. TypeScript dengan Bun](10-typescript-dengan-bun.md)

