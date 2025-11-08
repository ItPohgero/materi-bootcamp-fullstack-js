# 1. Pengenalan TypeScript

## Apa itu TypeScript?

TypeScript adalah superset dari JavaScript yang menambahkan static typing. Dibuat oleh Microsoft dan dirilis tahun 2012, TypeScript memberikan type safety dan better tooling support.

## JavaScript vs TypeScript

```javascript
// JavaScript
let name = "John";
name = 123;  // Valid! (tapi mungkin menyebabkan bug)

function add(a, b) {
    return a + b;
}
console.log(add(5, "3"));  // "53" (unexpected!)
```

```typescript
// TypeScript
let name: string = "John";
name = 123;  // Error: Type 'number' is not assignable to type 'string'

function add(a: number, b: number): number {
    return a + b;
}
console.log(add(5, "3"));  // Error: Argument of type 'string' is not assignable to parameter
```

## Mengapa Menggunakan TypeScript?

### 1. Type Safety
Catch errors saat development, bukan saat runtime.

```typescript
interface User {
    name: string;
    age: number;
}

function greet(user: User) {
    console.log(`Hello ${user.name}, age ${user.age}`);
}

greet({ name: "John", age: 30 });           // OK
greet({ name: "John", age: "30" });         // Error: age harus number
greet({ name: "John" });                    // Error: age required
```

### 2. Better IDE Support
- Autocomplete yang lebih baik
- IntelliSense
- Refactoring tools
- Go to definition
- Find all references

### 3. Self-Documenting Code
Types sebagai dokumentasi.

```typescript
// Jelas dari signature apa yang function butuhkan dan return
function calculateTotal(
    price: number,
    quantity: number,
    taxRate: number
): number {
    return price * quantity * (1 + taxRate);
}
```

### 4. Easier Refactoring
TypeScript akan alert jika ada breaking changes.

### 5. Latest JavaScript Features
TypeScript support ES6+ features dan transpile ke ES5 jika diperlukan.

### 6. Large Codebase Management
Type system membantu maintain large applications.

---

## TypeScript Features

### Static Typing
```typescript
let count: number = 5;
let message: string = "Hello";
let isActive: boolean = true;
```

### Type Inference
```typescript
let count = 5;  // TypeScript infer sebagai number
let message = "Hello";  // infer sebagai string
```

### Interfaces
```typescript
interface Person {
    name: string;
    age: number;
}

const person: Person = {
    name: "John",
    age: 30
};
```

### Classes
```typescript
class Animal {
    name: string;
    
    constructor(name: string) {
        this.name = name;
    }
    
    move(distance: number): void {
        console.log(`${this.name} moved ${distance}m`);
    }
}
```

### Generics
```typescript
function identity<T>(arg: T): T {
    return arg;
}

let output = identity<string>("Hello");
let num = identity<number>(123);
```

### Union Types
```typescript
let value: string | number;
value = "Hello";  // OK
value = 123;      // OK
value = true;     // Error
```

### Enums
```typescript
enum Status {
    Pending,
    Active,
    Inactive
}

let userStatus: Status = Status.Active;
```

---

## TypeScript Compilation

TypeScript code (.ts) harus di-compile ke JavaScript (.js) untuk dijalankan (kecuali menggunakan Bun yang support native).

### Traditional Workflow

```
TypeScript (.ts) → TypeScript Compiler (tsc) → JavaScript (.js) → Node.js/Browser
```

### Bun Workflow (No Compilation!)

```
TypeScript (.ts) → Bun Runtime (Native TypeScript)
```

Bun dapat langsung run TypeScript tanpa compilation step!

---

## TypeScript dengan Bun

Bun memiliki native TypeScript support.

### Run TypeScript Directly

```bash
# No compilation needed!
bun index.ts
```

### TypeScript Features di Bun

```typescript
// user.ts
interface User {
    id: number;
    name: string;
    email: string;
}

const user: User = {
    id: 1,
    name: "John Doe",
    email: "john@example.com"
};

console.log(`User: ${user.name}`);

// Run: bun user.ts
```

---

## Gradual Adoption

TypeScript dapat diadopsi secara gradual:

1. Start dengan JavaScript (.js)
2. Rename ke TypeScript (.ts)
3. Add types gradually
4. Strict mode optional

```typescript
// Bisa start tanpa types
let name = "John";  // TypeScript infer type

// Tambah types gradually
let age: number = 30;

// Full typing
interface Person {
    name: string;
    age: number;
}

const person: Person = { name, age };
```

---

## Common Type Errors (Good!)

Ini adalah contoh error yang TypeScript catch sebelum runtime:

```typescript
// Error 1: Type mismatch
let age: number = "30";  // Error: string not assignable to number

// Error 2: Missing property
interface User {
    name: string;
    email: string;
}
const user: User = { name: "John" };  // Error: email required

// Error 3: Wrong argument type
function greet(name: string) {
    console.log(`Hello ${name}`);
}
greet(123);  // Error: number not assignable to string

// Error 4: Undefined property
const user = { name: "John" };
console.log(user.age);  // Error: Property 'age' does not exist

// Error 5: Null/undefined access
function process(value: string | null) {
    console.log(value.length);  // Error: possibly null
}
```

---

## TypeScript Ecosystem

### Type Definitions

TypeScript memiliki type definitions untuk libraries populer:

```bash
# Install types untuk library
bun add -d @types/node
bun add -d @types/express
bun add -d @types/react
```

### DefinitelyTyped

Repository besar type definitions: https://github.com/DefinitelyTyped/DefinitelyTyped

---

## When to Use TypeScript?

### Use TypeScript When:

- Building large applications
- Working in teams
- Need refactoring often
- Want better IDE support
- Building libraries/packages
- Need documentation through types

### JavaScript Might Be Better When:

- Small scripts
- Prototyping quickly
- Learning JavaScript
- Simple one-off tasks

---

## TypeScript Configuration

TypeScript menggunakan `tsconfig.json` untuk konfigurasi.

### Basic tsconfig.json

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "module": "ESNext",
    "lib": ["ES2020"],
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "bundler",
    "types": ["bun-types"]
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### Generate tsconfig.json

```bash
# Dengan Bun
bun init

# Atau manual
tsc --init
```

---

## Learning Path

### 1. Basic Types
Learn primitive types (string, number, boolean, etc)

### 2. Complex Types
Arrays, tuples, objects

### 3. Functions
Function types, parameters, return types

### 4. Interfaces & Types
Define custom types

### 5. Classes
OOP dengan TypeScript

### 6. Generics
Reusable type-safe components

### 7. Advanced Types
Union, intersection, conditional types

### 8. Modules
Import/export dengan types

---

## Best Practices

### DO (Lakukan)

1. **Enable strict mode**
   ```json
   {
     "compilerOptions": {
       "strict": true
     }
   }
   ```

2. **Use type inference ketika clear**
   ```typescript
   // Good (inference)
   let count = 5;
   
   // Redundant
   let count: number = 5;
   ```

3. **Use interfaces untuk objects**
   ```typescript
   interface User {
       name: string;
       age: number;
   }
   ```

4. **Avoid 'any' type**
   ```typescript
   // Bad
   let data: any;
   
   // Good
   let data: unknown;  // Safer
   ```

### DON'T (Jangan)

1. Jangan gunakan 'any' kecuali terpaksa
2. Jangan disable strict mode tanpa alasan
3. Jangan over-type (redundant types)
4. Jangan ignore TypeScript errors

---

## Rangkuman

- TypeScript adalah JavaScript dengan static typing
- Catch errors sebelum runtime
- Better IDE support dan autocomplete
- Self-documenting code dengan types
- Bun support TypeScript native (no compilation!)
- Run .ts files directly: `bun file.ts`
- Types dapat di-infer atau explicit
- Gradual adoption possible
- Essential untuk large applications
- Learning curve worth it untuk long-term

---

**Selanjutnya:** [02. Setup TypeScript dengan Bun](02-setup-typescript.md)

