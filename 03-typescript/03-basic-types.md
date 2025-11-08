# 3. Basic Types

## Primitive Types

### string

Text data.

```typescript
let firstName: string = "John";
let lastName: string = 'Doe';
let message: string = `Hello ${firstName}`;

// Type inference
let city = "Jakarta";  // TypeScript infers string
```

### number

Integer dan floating-point.

```typescript
let age: number = 30;
let price: number = 99.99;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;

// Type inference
let count = 5;  // infers number
```

### boolean

True atau false.

```typescript
let isActive: boolean = true;
let isCompleted: boolean = false;

// Type inference
let hasPermission = true;  // infers boolean
```

### null dan undefined

```typescript
let nothing: null = null;
let notDefined: undefined = undefined;

// Dengan strictNullChecks
let name: string = null;  // Error
let age: number = undefined;  // Error

// Harus explicitly allow
let name: string | null = null;  // OK
let age: number | undefined = undefined;  // OK
```

---

## Type Inference

TypeScript dapat infer types otomatis.

```typescript
// Explicit type
let message: string = "Hello";

// Type inference (preferred ketika obvious)
let message = "Hello";  // TypeScript infers string

// Type inference dengan functions
function add(a: number, b: number) {
    return a + b;  // Return type inferred sebagai number
}

// Type inference dengan objects
let person = {
    name: "John",
    age: 30
};
// TypeScript infers: { name: string; age: number }
```

---

## any Type

Opt-out dari type checking.

```typescript
let value: any = "Hello";
value = 123;     // OK
value = true;    // OK
value = {};      // OK

// Avoid 'any' jika memungkinkan!
```

**Problem dengan any:**
```typescript
let data: any = "123";
console.log(data.toFixed(2));  // Runtime error! (no compile-time check)
```

---

## unknown Type

Safer alternative untuk any.

```typescript
let value: unknown = "Hello";

// Error: tidak bisa langsung use
console.log(value.length);

// Harus type check dulu
if (typeof value === "string") {
    console.log(value.length);  // OK, TypeScript knows it's string
}
```

**Use unknown instead of any:**

```typescript
function processValue(value: unknown) {
    // Type guard
    if (typeof value === "string") {
        return value.toUpperCase();
    }
    
    if (typeof value === "number") {
        return value * 2;
    }
    
    throw new Error("Unsupported type");
}
```

---

## void

Function yang tidak return value.

```typescript
function logMessage(message: string): void {
    console.log(message);
    // No return statement
}

function processData(data: string[]): void {
    data.forEach(item => console.log(item));
}

// Variable dengan type void (jarang digunakan)
let unusable: void = undefined;
```

---

## never

Function yang never returns (throw error atau infinite loop).

```typescript
function throwError(message: string): never {
    throw new Error(message);
}

function infiniteLoop(): never {
    while (true) {
        // ...
    }
}

// Exhaustive checks
type Shape = "circle" | "square";

function getArea(shape: Shape): number {
    switch (shape) {
        case "circle":
            return Math.PI * 10 * 10;
        case "square":
            return 10 * 10;
        default:
            const exhaustive: never = shape;
            throw new Error(`Unhandled shape: ${exhaustive}`);
    }
}
```

---

## Array Types

### Array Syntax

```typescript
// Method 1: Type[]
let numbers: number[] = [1, 2, 3, 4, 5];
let names: string[] = ["John", "Jane", "Bob"];

// Method 2: Array<Type>
let numbers2: Array<number> = [1, 2, 3];
let names2: Array<string> = ["John", "Jane"];

// Mixed types (union)
let mixed: (string | number)[] = [1, "two", 3, "four"];

// Array of objects
let users: { name: string; age: number }[] = [
    { name: "John", age: 30 },
    { name: "Jane", age: 25 }
];
```

### Readonly Arrays

```typescript
let numbers: readonly number[] = [1, 2, 3];
numbers.push(4);  // Error: push doesn't exist on readonly array
numbers[0] = 10;  // Error: cannot assign to read-only index

// Alternative syntax
let names: ReadonlyArray<string> = ["John", "Jane"];
```

---

## Tuple Types

Array dengan fixed length dan types.

```typescript
// Basic tuple
let person: [string, number] = ["John", 30];

// Access
console.log(person[0]);  // "John"
console.log(person[1]);  // 30

// Error
person = [30, "John"];  // Error: wrong order
person = ["John", 30, "Extra"];  // Error: too many elements

// With optional elements
let point: [number, number, number?] = [10, 20];  // z is optional

// With rest elements
let tuple: [string, ...number[]] = ["Hello", 1, 2, 3, 4];

// Readonly tuple
let readonlyTuple: readonly [string, number] = ["John", 30];
```

### Tuple Destructuring

```typescript
let person: [string, number, string] = ["John", 30, "Jakarta"];

let [name, age, city] = person;
console.log(name);  // "John" (type: string)
console.log(age);   // 30 (type: number)
```

---

## Object Types

### Inline Object Type

```typescript
let person: { name: string; age: number } = {
    name: "John",
    age: 30
};

// Optional properties
let user: { name: string; age?: number } = {
    name: "Jane"
    // age is optional
};

// Readonly properties
let config: { readonly apiKey: string } = {
    apiKey: "abc123"
};
config.apiKey = "new";  // Error: readonly
```

### Type Alias

```typescript
type Person = {
    name: string;
    age: number;
    email?: string;
};

let person1: Person = {
    name: "John",
    age: 30
};

let person2: Person = {
    name: "Jane",
    age: 25,
    email: "jane@example.com"
};
```

---

## Union Types

Value dapat be one of multiple types.

```typescript
let id: number | string;
id = 123;      // OK
id = "ABC123"; // OK
id = true;     // Error

// Function parameters
function printId(id: number | string) {
    console.log(`ID: ${id}`);
}

printId(101);       // OK
printId("ABC123");  // OK

// Type narrowing
function processId(id: number | string) {
    if (typeof id === "string") {
        console.log(id.toUpperCase());  // TypeScript knows it's string
    } else {
        console.log(id.toFixed(2));  // TypeScript knows it's number
    }
}
```

---

## Literal Types

Exact values sebagai types.

```typescript
// String literal
let direction: "left" | "right" | "up" | "down";
direction = "left";   // OK
direction = "forward"; // Error

// Number literal
let diceRoll: 1 | 2 | 3 | 4 | 5 | 6;
diceRoll = 3;  // OK
diceRoll = 7;  // Error

// Boolean literal
let success: true = true;
success = false;  // Error

// Combined with union
type Status = "pending" | "approved" | "rejected";
type YesNo = "yes" | "no";

let orderStatus: Status = "pending";
let answer: YesNo = "yes";
```

### Practical Example

```typescript
type HttpMethod = "GET" | "POST" | "PUT" | "DELETE";
type HttpStatus = 200 | 201 | 400 | 404 | 500;

function makeRequest(
    method: HttpMethod,
    url: string
): HttpStatus {
    console.log(`${method} ${url}`);
    return 200;
}

makeRequest("GET", "/api/users");     // OK
makeRequest("PATCH", "/api/users");   // Error: invalid method
```

---

## Type Assertion

Tell TypeScript the type explicitly.

```typescript
// Syntax 1: as
let someValue: unknown = "Hello World";
let strLength: number = (someValue as string).length;

// Syntax 2: <Type> (tidak bisa di JSX/TSX)
let strLength2: number = (<string>someValue).length;

// Practical example
const input = document.getElementById("username") as HTMLInputElement;
input.value = "John";

// Non-null assertion
let name: string | null = getName();
console.log(name!.toUpperCase());  // ! says "I know this is not null"
```

**Caution:** Type assertion tidak runtime check! Hanya untuk TypeScript compiler.

---

## Type Guards

Runtime checks untuk narrow types.

### typeof

```typescript
function process(value: string | number) {
    if (typeof value === "string") {
        console.log(value.toUpperCase());  // string methods
    } else {
        console.log(value.toFixed(2));  // number methods
    }
}
```

### instanceof

```typescript
class Dog {
    bark() { console.log("Woof!"); }
}

class Cat {
    meow() { console.log("Meow!"); }
}

function makeSound(animal: Dog | Cat) {
    if (animal instanceof Dog) {
        animal.bark();
    } else {
        animal.meow();
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

### Custom Type Guards

```typescript
function isString(value: unknown): value is string {
    return typeof value === "string";
}

function process(value: unknown) {
    if (isString(value)) {
        console.log(value.toUpperCase());  // TypeScript knows it's string
    }
}
```

---

## Best Practices

### DO (Lakukan)

1. **Use type inference ketika obvious**
   ```typescript
   // Good
   let count = 5;
   let message = "Hello";
   
   // Redundant
   let count: number = 5;
   ```

2. **Use union types untuk multiple possibilities**
   ```typescript
   function printId(id: number | string) {
       console.log(id);
   }
   ```

3. **Use unknown instead of any**
   ```typescript
   function process(value: unknown) {
       if (typeof value === "string") {
           // safe to use
       }
   }
   ```

4. **Use literal types untuk constants**
   ```typescript
   type Status = "active" | "inactive" | "pending";
   ```

### DON'T (Jangan)

1. **Jangan gunakan any**
   ```typescript
   // Bad
   let data: any = getData();
   
   // Good
   let data: unknown = getData();
   ```

2. **Jangan ignore type errors**
   ```typescript
   // Bad
   // @ts-ignore
   problematicCode();
   ```

3. **Jangan over-specify types**
   ```typescript
   // Bad (redundant)
   let message: string = "Hello";
   
   // Good (inference)
   let message = "Hello";
   ```

---

## Rangkuman

- Primitive types: string, number, boolean, null, undefined
- Special types: any (avoid), unknown (safe), void, never
- Array types: Type[] atau Array<Type>
- Tuple types untuk fixed-length arrays
- Object types: inline atau type alias
- Union types: Type1 | Type2
- Literal types untuk exact values
- Type assertions dengan 'as'
- Type guards untuk runtime checks
- Type inference mengurangi verbosity
- Use unknown instead of any
- Enable strict mode untuk safety

---

**Sebelumnya:** [02. Setup TypeScript](02-setup-typescript.md)  
**Selanjutnya:** [04. Interfaces dan Type Aliases](04-interfaces-type-aliases.md)

