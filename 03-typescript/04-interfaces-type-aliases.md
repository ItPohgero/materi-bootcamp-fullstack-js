# 4. Interfaces dan Type Aliases

## Interface

Interface mendefinisikan struktur object.

### Basic Interface

```typescript
interface User {
    name: string;
    age: number;
    email: string;
}

const user: User = {
    name: "John",
    age: 30,
    email: "john@example.com"
};

// Error: missing property
const user2: User = {
    name: "Jane",
    age: 25
    // Error: email is required
};
```

### Optional Properties

```typescript
interface User {
    name: string;
    age: number;
    email?: string;  // Optional
    phone?: string;  // Optional
}

const user1: User = {
    name: "John",
    age: 30
    // email optional
};

const user2: User = {
    name: "Jane",
    age: 25,
    email: "jane@example.com"
};
```

### Readonly Properties

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}

const point: Point = { x: 10, y: 20 };
point.x = 30;  // Error: readonly property
```

---

## Interface Methods

```typescript
interface Calculator {
    add(a: number, b: number): number;
    subtract(a: number, b: number): number;
}

const calc: Calculator = {
    add(a, b) {
        return a + b;
    },
    subtract(a, b) {
        return a - b;
    }
};

// Alternative method syntax
interface Logger {
    log: (message: string) => void;
    error: (message: string) => void;
}
```

---

## Extending Interfaces

```typescript
interface Person {
    name: string;
    age: number;
}

interface Employee extends Person {
    employeeId: number;
    department: string;
}

const employee: Employee = {
    name: "John",
    age: 30,
    employeeId: 12345,
    department: "IT"
};

// Multiple extends
interface Manager extends Employee, Person {
    teamSize: number;
}
```

---

## Type Aliases

Alternative untuk define custom types.

### Basic Type Alias

```typescript
type UserID = number | string;

let id1: UserID = 123;
let id2: UserID = "ABC123";

// Object type
type Person = {
    name: string;
    age: number;
};

const person: Person = {
    name: "John",
    age: 30
};
```

### Union Types

```typescript
type Status = "pending" | "approved" | "rejected";
type ID = number | string;

let orderStatus: Status = "pending";
let userId: ID = 123;
```

### Intersection Types

Combine multiple types.

```typescript
type Person = {
    name: string;
    age: number;
};

type Employee = {
    employeeId: number;
    department: string;
};

type Staff = Person & Employee;

const staff: Staff = {
    name: "John",
    age: 30,
    employeeId: 12345,
    department: "IT"
};
```

---

## Interface vs Type Alias

### Similarities

Keduanya bisa define object shapes:

```typescript
// Interface
interface Point {
    x: number;
    y: number;
}

// Type alias
type Point2 = {
    x: number;
    y: number;
};
```

### Differences

**Interface can be extended:**

```typescript
interface Animal {
    name: string;
}

interface Dog extends Animal {
    breed: string;
}
```

**Type alias can use unions:**

```typescript
type ID = number | string;
type Status = "active" | "inactive";
```

**Interface can be reopened (declaration merging):**

```typescript
interface User {
    name: string;
}

interface User {
    age: number;
}

// Merged: { name: string; age: number }
```

**Type alias cannot be reopened:**

```typescript
type User = {
    name: string;
};

type User = {
    age: number;
};
// Error: Duplicate identifier 'User'
```

### When to Use What?

**Use Interface when:**
- Defining object shapes
- Need to extend/implement
- Building libraries (can be augmented)

**Use Type when:**
- Union types
- Intersection types
- Primitives, tuples
- Complex type manipulations

---

## Index Signatures

Define properties dengan dynamic keys.

```typescript
interface StringMap {
    [key: string]: string;
}

const translations: StringMap = {
    hello: "Halo",
    goodbye: "Selamat tinggal",
    thanks: "Terima kasih"
};

// Numeric index
interface NumberArray {
    [index: number]: string;
}

const fruits: NumberArray = ["Apple", "Banana", "Orange"];
```

### With Known Properties

```typescript
interface Config {
    name: string;
    version: number;
    [key: string]: any;  // Additional properties
}

const config: Config = {
    name: "MyApp",
    version: 1,
    author: "John",      // OK (index signature)
    license: "MIT"       // OK
};
```

---

## Function Types

### Function Type Expression

```typescript
type GreetFunction = (name: string) => string;

const greet: GreetFunction = (name) => {
    return `Hello ${name}`;
};

// With interface
interface Calculator {
    (a: number, b: number): number;
}

const add: Calculator = (a, b) => a + b;
```

### Call Signatures

```typescript
interface DescribableFunction {
    description: string;
    (someArg: number): boolean;
}

function doSomething(fn: DescribableFunction) {
    console.log(fn.description);
    console.log(fn(6));
}
```

---

## Generics dengan Interfaces

```typescript
interface Box<T> {
    value: T;
}

const stringBox: Box<string> = { value: "Hello" };
const numberBox: Box<number> = { value: 123 };

// Multiple generic types
interface Pair<T, U> {
    first: T;
    second: U;
}

const pair: Pair<string, number> = {
    first: "One",
    second: 1
};
```

---

## Practical Examples

### User System

```typescript
interface User {
    id: number;
    username: string;
    email: string;
    password: string;
    createdAt: Date;
    updatedAt?: Date;
}

interface UserProfile extends User {
    bio?: string;
    avatar?: string;
    socialLinks?: {
        twitter?: string;
        github?: string;
    };
}

interface UserCredentials {
    username: string;
    password: string;
}

type PublicUser = Omit<User, 'password'>;  // Exclude password
type UserUpdate = Partial<User>;  // All properties optional
```

### API Response Types

```typescript
interface ApiResponse<T> {
    success: boolean;
    data?: T;
    error?: string;
    message?: string;
}

interface UserData {
    id: number;
    name: string;
    email: string;
}

// Usage
const response: ApiResponse<UserData> = {
    success: true,
    data: {
        id: 1,
        name: "John",
        email: "john@example.com"
    }
};

const errorResponse: ApiResponse<null> = {
    success: false,
    error: "User not found"
};
```

### Database Models

```typescript
interface BaseModel {
    id: number;
    createdAt: Date;
    updatedAt: Date;
}

interface Product extends BaseModel {
    name: string;
    description: string;
    price: number;
    stock: number;
    categoryId: number;
}

interface Category extends BaseModel {
    name: string;
    description?: string;
}

interface Order extends BaseModel {
    userId: number;
    total: number;
    status: "pending" | "processing" | "shipped" | "delivered";
}
```

---

## Utility Types

TypeScript menyediakan utility types built-in.

### Partial<Type>

Semua properties jadi optional.

```typescript
interface User {
    name: string;
    age: number;
    email: string;
}

type UserUpdate = Partial<User>;

const update: UserUpdate = {
    age: 31  // name dan email optional
};
```

### Required<Type>

Semua properties jadi required.

```typescript
interface User {
    name: string;
    age?: number;
    email?: string;
}

type CompleteUser = Required<User>;

const user: CompleteUser = {
    name: "John",
    age: 30,      // Required!
    email: "john@example.com"  // Required!
};
```

### Readonly<Type>

Semua properties jadi readonly.

```typescript
interface User {
    name: string;
    age: number;
}

type ReadonlyUser = Readonly<User>;

const user: ReadonlyUser = {
    name: "John",
    age: 30
};

user.age = 31;  // Error: readonly
```

### Pick<Type, Keys>

Pick specific properties.

```typescript
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
}

type UserPreview = Pick<User, 'id' | 'name'>;

const preview: UserPreview = {
    id: 1,
    name: "John"
    // email dan password tidak included
};
```

### Omit<Type, Keys>

Exclude specific properties.

```typescript
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
}

type PublicUser = Omit<User, 'password'>;

const publicUser: PublicUser = {
    id: 1,
    name: "John",
    email: "john@example.com"
    // password excluded
};
```

### Record<Keys, Type>

Create object type dengan specific keys.

```typescript
type Roles = "admin" | "user" | "guest";

type Permissions = Record<Roles, string[]>;

const permissions: Permissions = {
    admin: ["read", "write", "delete"],
    user: ["read", "write"],
    guest: ["read"]
};
```

---

## Best Practices

### DO (Lakukan)

1. **Use interfaces untuk object shapes**
   ```typescript
   interface User {
       name: string;
       age: number;
   }
   ```

2. **Use type aliases untuk unions**
   ```typescript
   type ID = number | string;
   type Status = "active" | "inactive";
   ```

3. **Use utility types**
   ```typescript
   type UserUpdate = Partial<User>;
   type PublicUser = Omit<User, 'password'>;
   ```

4. **Extend interfaces untuk reusability**
   ```typescript
   interface Employee extends Person {
       employeeId: number;
   }
   ```

### DON'T (Jangan)

1. **Jangan mix interface dan type untuk same purpose**
2. **Jangan create types yang terlalu complex**
3. **Jangan duplicate type definitions**

---

## Rangkuman

- Interface untuk define object structure
- Type alias untuk unions, primitives, complex types
- Optional properties dengan ?
- Readonly properties
- Extending interfaces
- Intersection types dengan &
- Index signatures untuk dynamic keys
- Utility types: Partial, Required, Readonly, Pick, Omit, Record
- Interface vs Type: choose based on use case
- Use generics untuk reusable types

---

**Sebelumnya:** [03. Basic Types](03-basic-types.md)  
**Selanjutnya:** [05. Functions](05-functions.md)

