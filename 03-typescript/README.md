# Materi TypeScript untuk Bun

Selamat datang di materi pembelajaran TypeScript untuk Bun! Materi ini disusun secara berurutan untuk memudahkan Anda mempelajari TypeScript dengan Bun dari nol hingga mahir.

---

## Daftar Materi

### 1. [Pengenalan TypeScript](01-pengenalan-typescript.md)
- Apa itu TypeScript?
- JavaScript vs TypeScript
- Mengapa menggunakan TypeScript?
- Type safety dan benefits
- ECMAScript dan TypeScript
- Compilation process (dan Bun's native support)
- When to use TypeScript
- Learning path

### 2. [Setup TypeScript dengan Bun](02-setup-typescript.md)
- Install Bun
- Initialize TypeScript project
- tsconfig.json configuration
- Project structure
- Package.json setup
- Running TypeScript dengan Bun (no compilation!)
- Install type definitions
- VSCode setup
- ESLint dan Prettier
- Type checking

### 3. [Basic Types](03-basic-types.md)
- Primitive types (string, number, boolean, null, undefined)
- Type inference
- any type (dan kenapa avoid)
- unknown type (safe alternative)
- void type
- never type
- Array types
- Tuple types
- Object types
- Union types
- Literal types
- Type assertions
- Type guards

### 4. [Interfaces dan Type Aliases](04-interfaces-type-aliases.md)
- Interface basics
- Optional properties
- Readonly properties
- Interface methods
- Extending interfaces
- Type aliases
- Union dan intersection types
- Interface vs Type
- Index signatures
- Function types
- Generic interfaces
- Utility types (Partial, Required, Readonly, Pick, Omit, Record)

### 5. [Functions](05-functions.md)
- Function type annotations
- Parameter types
- Return types
- Optional parameters
- Default parameters
- Rest parameters
- Function type expressions
- Arrow functions
- Function overloads
- Generic functions
- Async functions dengan types
- Callback types
- this parameter

### 6. [Classes](06-classes.md)
- Basic classes
- Access modifiers (public, private, protected)
- Readonly properties
- Parameter properties
- Getters dan setters
- Static members
- Inheritance (extends)
- Abstract classes
- Implementing interfaces
- Generic classes
- Practical patterns

### 7. [Generics](07-generics.md)
- Basic generic functions
- Generic constraints
- Generic interfaces
- Generic classes
- Multiple type parameters
- Generic utility types
- Practical examples (Repository pattern)
- Best practices

### 8. [Advanced Types](08-advanced-types.md)
- Union types
- Intersection types
- Conditional types
- Mapped types
- Template literal types
- keyof operator
- typeof operator
- Utility types (ReturnType, Parameters, Awaited)
- Discriminated unions
- Type narrowing
- Type predicates
- Index accessed types

### 9. [Modules](09-modules.md)
- ES6 modules
- Export dan import
- Default exports
- Named exports
- Type-only imports/exports
- Re-exporting
- Path aliases
- Module patterns (barrel exports)
- CommonJS compatibility
- Project structure
- Best practices

### 10. [TypeScript dengan Bun](10-typescript-dengan-bun.md)
- Native TypeScript support di Bun
- Bun-specific types
- Bun APIs dengan TypeScript (file, serve)
- Type-safe request handlers
- Environment variables
- Database integration (Prisma)
- Validation libraries (Zod)
- Testing dengan TypeScript
- Complete REST API example
- Best practices dengan Bun

---

## Quick Start

### Install Bun

```bash
# macOS/Linux
curl -fsSL https://bun.sh/install | bash

# Windows
powershell -c "irm bun.sh/install.ps1 | iex"

# Verify
bun --version
```

### Create TypeScript Project

```bash
# Initialize project
mkdir my-ts-project
cd my-ts-project
bun init

# Run TypeScript
bun index.ts

# Watch mode
bun --watch index.ts
```

---

## Keunggulan TypeScript dengan Bun

**No Compilation Step**
- Run .ts files directly
- Instant feedback
- No build process

**Fast Execution**
- Bun is 3-4x faster than Node.js
- Native TypeScript parsing

**All-in-One**
- Runtime
- Bundler
- Test runner
- Package manager

**Great DX**
- Hot reload dengan --watch
- Built-in testing
- .env support

---

## TypeScript Fundamentals

```typescript
// Types
let name: string = "John";
let age: number = 30;
let isActive: boolean = true;

// Interface
interface User {
    id: number;
    name: string;
    email: string;
}

// Type Alias
type ID = number | string;
type Status = "active" | "inactive";

// Function
function greet(name: string): string {
    return `Hello, ${name}!`;
}

// Generic
function identity<T>(arg: T): T {
    return arg;
}

// Class
class Person {
    constructor(
        public name: string,
        private age: number
    ) {}
    
    greet(): string {
        return `Hello, I'm ${this.name}`;
    }
}

// Union type
function process(value: string | number) {
    if (typeof value === "string") {
        return value.toUpperCase();
    }
    return value * 2;
}

// Array
const numbers: number[] = [1, 2, 3];
const users: User[] = [];

// Object
const config: { port: number; host: string } = {
    port: 3000,
    host: "localhost"
};

// Async
async function fetchData(): Promise<User[]> {
    const response = await fetch('/api/users');
    return response.json();
}
```

---

## Resources Tambahan

### TypeScript Documentation
- [Official TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/)
- [TypeScript Cheat Sheet](https://www.typescriptlang.org/cheatsheets)

### Bun Documentation
- [Bun Official Docs](https://bun.sh/docs)
- [Bun TypeScript Guide](https://bun.sh/docs/typescript)
- [Bun GitHub](https://github.com/oven-sh/bun)

### Practice
- [TypeScript Exercises](https://typescript-exercises.github.io/)
- [Type Challenges](https://github.com/type-challenges/type-challenges)
- Build real projects

---

## Common Patterns

### Repository Pattern

```typescript
interface Repository<T> {
    findAll(): Promise<T[]>;
    findById(id: number): Promise<T | null>;
    create(data: Omit<T, 'id'>): Promise<T>;
    update(id: number, data: Partial<T>): Promise<T | null>;
    delete(id: number): Promise<boolean>;
}
```

### Result Type

```typescript
type Result<T, E = Error> = 
    | { success: true; data: T }
    | { success: false; error: E };
```

### Builder Pattern

```typescript
class QueryBuilder<T> {
    private query = '';
    
    where(condition: string): this {
        this.query += ` WHERE ${condition}`;
        return this;
    }
    
    orderBy(column: keyof T): this {
        this.query += ` ORDER BY ${String(column)}`;
        return this;
    }
}
```

---

## Best Practices Summary

### Type Definitions

- Use interfaces untuk object shapes
- Use type aliases untuk unions/intersections
- Use enums untuk fixed set of values
- Use literal types untuk exact values

### Functions

- Annotate parameters
- Let TypeScript infer simple returns
- Use generics untuk reusable code
- Use function overloads when needed

### Classes

- Use access modifiers (public, private, protected)
- Use parameter properties
- Implement interfaces
- Use abstract classes untuk base classes

### Code Organization

- One type per file untuk complex types
- Barrel exports (index.ts)
- Path aliases untuk clean imports
- Separate types dari implementation

### Bun-Specific

- Run .ts files directly
- No build step needed
- Use built-in test runner
- Leverage Bun's performance

---

## Next Steps

Setelah menguasai materi ini, lanjut ke:

### Advanced TypeScript
- Advanced generics
- Conditional types
- Template literal types
- Declaration merging
- Decorators

### Ecosystem
- Prisma (ORM)
- Zod (validation)
- Elysia (Bun framework)
- tRPC (type-safe APIs)

### Production
- Error handling
- Logging
- Testing strategies
- Deployment
- Performance optimization

---

## Project Ideas

### Beginner
1. **Calculator CLI**
   - Type-safe operations
   - Input validation

2. **Todo API**
   - CRUD operations
   - JSON file storage

### Intermediate
1. **REST API dengan Database**
   - Prisma ORM
   - CRUD endpoints
   - Validation dengan Zod

2. **Authentication API**
   - Register/Login
   - JWT tokens
   - Type-safe middleware

### Advanced
1. **E-Commerce API**
   - Products, orders, users
   - Relationships
   - Complex queries

2. **Real-time Chat**
   - WebSocket
   - Type-safe events
   - Multiple rooms

---

## Progress Tracking

- [ ] 1. Pengenalan TypeScript
- [ ] 2. Setup TypeScript dengan Bun
- [ ] 3. Basic Types
- [ ] 4. Interfaces dan Type Aliases
- [ ] 5. Functions
- [ ] 6. Classes
- [ ] 7. Generics
- [ ] 8. Advanced Types
- [ ] 9. Modules
- [ ] 10. TypeScript dengan Bun

---

## Butuh Bantuan?

1. **Read TypeScript errors carefully** - Mereka sangat informatif
2. **Use VSCode IntelliSense** - Hover untuk see types
3. **Check Bun docs** - https://bun.sh/docs
4. **TypeScript Handbook** - https://www.typescriptlang.org/docs/
5. **Bun Discord** - Active community

---

## Selamat Belajar!

TypeScript dengan Bun adalah kombinasi yang powerful. Anda mendapatkan:

- Type safety dari TypeScript
- Speed dari Bun
- Great developer experience
- Modern tooling
- Production-ready

**Happy Coding!**

---

*Materi ini dibuat untuk membantu Anda belajar TypeScript dengan Bun dari dasar hingga mahir.*

