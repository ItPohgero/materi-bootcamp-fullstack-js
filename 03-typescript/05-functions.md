# 5. Functions

## Function Types

### Function dengan Type Annotations

```typescript
// Parameter dan return types
function greet(name: string): string {
    return `Hello, ${name}!`;
}

function add(a: number, b: number): number {
    return a + b;
}

// Void return type
function logMessage(message: string): void {
    console.log(message);
}
```

### Type Inference untuk Returns

```typescript
// TypeScript infers return type
function multiply(a: number, b: number) {
    return a * b;  // Inferred as number
}

// Explicit lebih baik untuk public APIs
function divide(a: number, b: number): number {
    return a / b;
}
```

---

## Optional Parameters

```typescript
function greet(name: string, greeting?: string): string {
    return `${greeting || "Hello"}, ${name}!`;
}

console.log(greet("John"));              // "Hello, John!"
console.log(greet("Jane", "Hi"));        // "Hi, Jane!"
```

---

## Default Parameters

```typescript
function greet(name: string, greeting: string = "Hello"): string {
    return `${greeting}, ${name}!`;
}

console.log(greet("John"));        // "Hello, John!"
console.log(greet("Jane", "Hi"));  // "Hi, Jane!"

// Type inferred from default value
function createUser(name: string, role = "user") {
    // role inferred as string
    return { name, role };
}
```

---

## Rest Parameters

```typescript
function sum(...numbers: number[]): number {
    return numbers.reduce((total, n) => total + n, 0);
}

console.log(sum(1, 2, 3));        // 6
console.log(sum(1, 2, 3, 4, 5));  // 15

// Mixed parameters
function logMessages(prefix: string, ...messages: string[]): void {
    messages.forEach(msg => console.log(`${prefix}: ${msg}`));
}

logMessages("INFO", "Server started", "Database connected");
```

---

## Function Type Expressions

```typescript
// Type untuk function
type GreetFunction = (name: string) => string;

const greet: GreetFunction = (name) => {
    return `Hello, ${name}!`;
};

// Dengan multiple parameters
type MathOperation = (a: number, b: number) => number;

const add: MathOperation = (a, b) => a + b;
const subtract: MathOperation = (a, b) => a - b;
```

---

## Arrow Functions

```typescript
// Concise
const add = (a: number, b: number): number => a + b;

// Block body
const greet = (name: string): string => {
    const message = `Hello, ${name}!`;
    return message;
};

// No parameters
const getTimestamp = (): number => Date.now();

// Void return
const log = (message: string): void => {
    console.log(message);
};
```

---

## Function Overloads

Multiple function signatures.

```typescript
// Overload signatures
function getValue(id: number): string;
function getValue(name: string): number;

// Implementation signature
function getValue(param: number | string): string | number {
    if (typeof param === "number") {
        return `ID-${param}`;
    } else {
        return param.length;
    }
}

console.log(getValue(123));      // "ID-123"
console.log(getValue("John"));   // 4
```

### Practical Example

```typescript
function makeDate(timestamp: number): Date;
function makeDate(year: number, month: number, day: number): Date;

function makeDate(yearOrTimestamp: number, month?: number, day?: number): Date {
    if (month !== undefined && day !== undefined) {
        return new Date(yearOrTimestamp, month - 1, day);
    } else {
        return new Date(yearOrTimestamp);
    }
}

const date1 = makeDate(1643760000000);
const date2 = makeDate(2025, 1, 15);
```

---

## Generic Functions

```typescript
// Generic function
function identity<T>(arg: T): T {
    return arg;
}

let output1 = identity<string>("Hello");
let output2 = identity<number>(123);

// Type inference
let output3 = identity("Hello");  // T inferred as string

// Array generic
function getFirstElement<T>(arr: T[]): T | undefined {
    return arr[0];
}

let first = getFirstElement([1, 2, 3]);      // number | undefined
let firstStr = getFirstElement(["a", "b"]);  // string | undefined
```

### Multiple Type Parameters

```typescript
function pair<T, U>(first: T, second: U): [T, U] {
    return [first, second];
}

let result = pair("age", 30);  // [string, number]
let result2 = pair(1, "one");  // [number, string]
```

### Generic Constraints

```typescript
interface HasLength {
    length: number;
}

function logLength<T extends HasLength>(arg: T): void {
    console.log(arg.length);
}

logLength("Hello");       // OK
logLength([1, 2, 3]);     // OK
logLength({ length: 10 }); // OK
logLength(123);           // Error: number doesn't have length
```

---

## Async Functions

```typescript
// Async function returns Promise
async function fetchUser(id: number): Promise<User> {
    const response = await fetch(`/api/users/${id}`);
    const data = await response.json();
    return data;
}

// Usage
const user = await fetchUser(1);
console.log(user.name);

// Error handling
async function safeF

fetchUser(id: number): Promise<User | null> {
    try {
        const response = await fetch(`/api/users/${id}`);
        if (!response.ok) {
            return null;
        }
        return await response.json();
    } catch (error) {
        console.error(error);
        return null;
    }
}
```

---

## Callback Types

```typescript
// Callback type
type Callback = (error: Error | null, result?: string) => void;

function processData(data: string, callback: Callback): void {
    try {
        const result = data.toUpperCase();
        callback(null, result);
    } catch (error) {
        callback(error as Error);
    }
}

// Usage
processData("hello", (error, result) => {
    if (error) {
        console.error(error);
    } else {
        console.log(result);
    }
});
```

---

## this Parameter

```typescript
interface User {
    name: string;
    greet(this: User): void;
}

const user: User = {
    name: "John",
    greet() {
        console.log(`Hello, I'm ${this.name}`);
    }
};

user.greet();  // OK

const greetFn = user.greet;
greetFn();  // Error: 'this' context required
```

---

## Best Practices

### DO (Lakukan)

1. **Annotate parameters, infer returns**
   ```typescript
   function add(a: number, b: number) {
       return a + b;  // Return type inferred
   }
   ```

2. **Use explicit return types untuk public APIs**
   ```typescript
   export function calculateTotal(items: Item[]): number {
       return items.reduce((sum, item) => sum + item.price, 0);
   }
   ```

3. **Use generics untuk reusable functions**
   ```typescript
   function wrapInArray<T>(value: T): T[] {
       return [value];
   }
   ```

4. **Use async/await dengan proper types**
   ```typescript
   async function fetchData(): Promise<Data> {
       const response = await fetch(url);
       return response.json();
   }
   ```

### DON'T (Jangan)

1. **Jangan use any untuk parameters**
   ```typescript
   // Bad
   function process(data: any) { }
   
   // Good
   function process(data: unknown) { }
   ```

2. **Jangan skip return type untuk complex functions**

3. **Jangan overload tanpa perlu**

---

## Rangkuman

- Annotate function parameters dengan types
- Specify return types untuk clarity
- Optional parameters dengan ?
- Default parameters
- Rest parameters dengan ...Type[]
- Function type expressions
- Function overloads untuk multiple signatures
- Generic functions untuk type reusability
- Async functions return Promise<Type>
- Use 'this' parameter untuk context
- Infer simple returns, explicit complex returns

---

**Sebelumnya:** [04. Interfaces dan Type Aliases](04-interfaces-type-aliases.md)  
**Selanjutnya:** [06. Classes](06-classes.md)

