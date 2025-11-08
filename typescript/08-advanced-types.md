# 8. Advanced Types

## Union Types

Type yang bisa be one of several types.

```typescript
type StringOrNumber = string | number;

let value: StringOrNumber;
value = "Hello";  // OK
value = 123;      // OK
value = true;     // Error

// Function dengan union
function printId(id: string | number): void {
    console.log(`ID: ${id}`);
}

// Type narrowing
function process(value: string | number) {
    if (typeof value === "string") {
        console.log(value.toUpperCase());
    } else {
        console.log(value.toFixed(2));
    }
}
```

---

## Intersection Types

Combine multiple types.

```typescript
interface Person {
    name: string;
    age: number;
}

interface Employee {
    employeeId: number;
    department: string;
}

type Staff = Person & Employee;

const staff: Staff = {
    name: "John",
    age: 30,
    employeeId: 12345,
    department: "IT"
};
```

---

## Type Aliases vs Interfaces

```typescript
// Type alias (can do unions)
type ID = number | string;
type Point = { x: number; y: number };

// Interface (can extend)
interface Point3D extends Point {
    z: number;
}

// Intersection
type PersonWithEmail = Person & { email: string };
```

---

## Conditional Types

Types yang depend on condition.

```typescript
type IsString<T> = T extends string ? true : false;

type A = IsString<string>;  // true
type B = IsString<number>;  // false

// Practical example
type NonNullable<T> = T extends null | undefined ? never : T;

type A = NonNullable<string | null>;  // string
type B = NonNullable<number | undefined>;  // number
```

---

## Mapped Types

Transform properties of type.

```typescript
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

type Optional<T> = {
    [P in keyof T]?: T[P];
};

interface User {
    name: string;
    age: number;
}

type ReadonlyUser = Readonly<User>;
type OptionalUser = Optional<User>;

// With modifiers
type Mutable<T> = {
    -readonly [P in keyof T]: T[P];
};

type Required<T> = {
    [P in keyof T]-?: T[P];  // Remove optional
};
```

---

## Template Literal Types

```typescript
type Greeting = "hello" | "hi";
type Name = "world" | "bun";

type GreetingMessage = `${Greeting} ${Name}`;
// "hello world" | "hello bun" | "hi world" | "hi bun"

// Practical example
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";
type Endpoint = `/api/${string}`;

type ApiRoute = `${HttpMethod} ${Endpoint}`;
// "GET /api/users" | "POST /api/users" | etc
```

---

## keyof Operator

Get keys of type sebagai union.

```typescript
interface User {
    name: string;
    age: number;
    email: string;
}

type UserKeys = keyof User;  // "name" | "age" | "email"

function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const user: User = {
    name: "John",
    age: 30,
    email: "john@example.com"
};

const name = getProperty(user, "name");    // string
const age = getProperty(user, "age");      // number
const invalid = getProperty(user, "invalid");  // Error
```

---

## typeof Operator

Get type dari value.

```typescript
const person = {
    name: "John",
    age: 30
};

type Person = typeof person;
// { name: string; age: number }

const person2: Person = {
    name: "Jane",
    age: 25
};

// With functions
function greet() {
    return { message: "Hello", timestamp: Date.now() };
}

type GreetReturn = ReturnType<typeof greet>;
// { message: string; timestamp: number }
```

---

## Utility Types

### ReturnType<Type>

Extract return type dari function.

```typescript
function createUser() {
    return {
        id: 1,
        name: "John",
        email: "john@example.com"
    };
}

type User = ReturnType<typeof createUser>;
// { id: number; name: string; email: string }
```

### Parameters<Type>

Extract parameter types.

```typescript
function greet(name: string, age: number) {
    return `${name} is ${age} years old`;
}

type GreetParams = Parameters<typeof greet>;
// [name: string, age: number]

const params: GreetParams = ["John", 30];
greet(...params);
```

### Awaited<Type>

Unwrap Promise type.

```typescript
type A = Awaited<Promise<string>>;  // string
type B = Awaited<Promise<Promise<number>>>;  // number

// Practical
async function fetchUser(): Promise<User> {
    const response = await fetch("/api/user");
    return response.json();
}

type UserType = Awaited<ReturnType<typeof fetchUser>>;  // User
```

---

## Discriminated Unions

Union dengan common property untuk type narrowing.

```typescript
interface Circle {
    kind: "circle";
    radius: number;
}

interface Square {
    kind: "square";
    size: number;
}

interface Rectangle {
    kind: "rectangle";
    width: number;
    height: number;
}

type Shape = Circle | Square | Rectangle;

function getArea(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "square":
            return shape.size ** 2;
        case "rectangle":
            return shape.width * shape.height;
    }
}

// Usage
const circle: Circle = { kind: "circle", radius: 10 };
const square: Square = { kind: "square", size: 20 };

console.log(getArea(circle));
console.log(getArea(square));
```

---

## Type Narrowing

### typeof

```typescript
function padLeft(value: string, padding: string | number): string {
    if (typeof padding === "number") {
        return " ".repeat(padding) + value;
    }
    return padding + value;
}
```

### instanceof

```typescript
class Fish {
    swim() { console.log("swimming"); }
}

class Bird {
    fly() { console.log("flying"); }
}

function move(animal: Fish | Bird) {
    if (animal instanceof Fish) {
        animal.swim();
    } else {
        animal.fly();
    }
}
```

### in operator

```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
    if ("swim" in animal) {
        animal.swim();
    } else {
        animal.fly();
    }
}
```

### Truthiness narrowing

```typescript
function printName(name: string | null | undefined) {
    if (name) {
        console.log(name.toUpperCase());
    } else {
        console.log("No name provided");
    }
}
```

---

## Type Predicates

Custom type guards.

```typescript
function isString(value: unknown): value is string {
    return typeof value === "string";
}

function isNumber(value: unknown): value is number {
    return typeof value === "number";
}

function process(value: unknown) {
    if (isString(value)) {
        console.log(value.toUpperCase());  // string
    } else if (isNumber(value)) {
        console.log(value.toFixed(2));  // number
    }
}

// Complex type guard
interface User {
    type: "user";
    name: string;
}

interface Admin {
    type: "admin";
    name: string;
    permissions: string[];
}

function isAdmin(user: User | Admin): user is Admin {
    return user.type === "admin";
}

function processUser(user: User | Admin) {
    if (isAdmin(user)) {
        console.log(user.permissions);  // Admin
    } else {
        console.log(user.name);  // User
    }
}
```

---

## Index Accessed Types

```typescript
interface User {
    name: string;
    age: number;
    email: string;
}

type NameType = User["name"];  // string
type AgeType = User["age"];    // number

// Multiple keys
type NameOrAge = User["name" | "age"];  // string | number

// All values
type UserValue = User[keyof User];  // string | number
```

---

## Practical Patterns

### Result Type (Error Handling)

```typescript
type Success<T> = {
    success: true;
    data: T;
};

type Failure = {
    success: false;
    error: string;
};

type Result<T> = Success<T> | Failure;

function divide(a: number, b: number): Result<number> {
    if (b === 0) {
        return {
            success: false,
            error: "Cannot divide by zero"
        };
    }
    
    return {
        success: true,
        data: a / b
    };
}

// Usage
const result = divide(10, 2);
if (result.success) {
    console.log(result.data);
} else {
    console.error(result.error);
}
```

### Builder Pattern

```typescript
class QueryBuilder<T> {
    private conditions: string[] = [];
    
    where(condition: string): this {
        this.conditions.push(condition);
        return this;
    }
    
    and(condition: string): this {
        this.conditions.push(`AND ${condition}`);
        return this;
    }
    
    build(): string {
        return this.conditions.join(" ");
    }
}

const query = new QueryBuilder<User>()
    .where("age > 18")
    .and("status = 'active'")
    .build();
```

---

## Best Practices

### DO (Lakukan)

1. **Use discriminated unions**
   ```typescript
   type Shape = 
       | { kind: "circle"; radius: number }
       | { kind: "square"; size: number };
   ```

2. **Use type predicates untuk custom guards**
   ```typescript
   function isUser(obj: any): obj is User {
       return obj && typeof obj.name === "string";
   }
   ```

3. **Use utility types**
   ```typescript
   type UserUpdate = Partial<User>;
   type PublicUser = Omit<User, 'password'>;
   ```

### DON'T (Jangan)

1. **Jangan create overly complex types**
2. **Jangan skip type narrowing**
3. **Jangan mix too many type features sekaligus**

---

## Rangkuman

- Union types untuk multiple possibilities
- Intersection types untuk combine types
- Conditional types untuk type logic
- Mapped types untuk transform properties
- Template literal types untuk string patterns
- keyof untuk get object keys
- typeof untuk get value type
- Utility types: ReturnType, Parameters, Awaited
- Discriminated unions untuk type-safe switches
- Type narrowing dengan typeof, instanceof, in
- Type predicates untuk custom guards
- Advanced patterns: Result type, Builder pattern

---

**Sebelumnya:** [07. Generics](07-generics.md)  
**Selanjutnya:** [09. Modules](09-modules.md)

