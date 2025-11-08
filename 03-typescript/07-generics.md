# 7. Generics

## Apa itu Generics?

Generics memungkinkan kita membuat reusable components yang dapat work dengan berbagai types sambil maintaining type safety.

## Basic Generic Function

```typescript
// Without generics (specific type)
function identityString(arg: string): string {
    return arg;
}

function identityNumber(arg: number): number {
    return arg;
}

// With generics (reusable)
function identity<T>(arg: T): T {
    return arg;
}

// Usage
let output1 = identity<string>("Hello");
let output2 = identity<number>(123);
let output3 = identity<boolean>(true);

// Type inference
let output4 = identity("Hello");  // T inferred as string
```

---

## Generic Constraints

Limit types yang bisa digunakan.

```typescript
// Constrain to objects dengan length property
interface HasLength {
    length: number;
}

function logLength<T extends HasLength>(arg: T): void {
    console.log(arg.length);
}

logLength("Hello");       // OK: string has length
logLength([1, 2, 3]);     // OK: array has length
logLength({ length: 10 }); // OK: object has length
logLength(123);           // Error: number doesn't have length

// Constrain to specific types
function merge<T extends object, U extends object>(obj1: T, obj2: U): T & U {
    return { ...obj1, ...obj2 };
}

const merged = merge({ name: "John" }, { age: 30 });
console.log(merged.name);  // OK
console.log(merged.age);   // OK
```

---

## Generic Interfaces

```typescript
interface Box<T> {
    value: T;
}

const stringBox: Box<string> = { value: "Hello" };
const numberBox: Box<number> = { value: 123 };

// Generic interface dengan methods
interface Repository<T> {
    items: T[];
    add(item: T): void;
    findById(id: number): T | undefined;
}

class UserRepository implements Repository<User> {
    items: User[] = [];
    
    add(user: User): void {
        this.items.push(user);
    }
    
    findById(id: number): User | undefined {
        return this.items.find(u => u.id === id);
    }
}
```

---

## Generic Classes

```typescript
class DataStore<T> {
    private data: T[] = [];
    
    add(item: T): void {
        this.data.push(item);
    }
    
    remove(item: T): void {
        const index = this.data.indexOf(item);
        if (index > -1) {
            this.data.splice(index, 1);
        }
    }
    
    find(predicate: (item: T) => boolean): T | undefined {
        return this.data.find(predicate);
    }
    
    getAll(): T[] {
        return [...this.data];
    }
}

// Usage
const numberStore = new DataStore<number>();
numberStore.add(1);
numberStore.add(2);

const userStore = new DataStore<User>();
userStore.add({ id: 1, name: "John" });
```

---

## Multiple Type Parameters

```typescript
function pair<T, U>(first: T, second: U): [T, U] {
    return [first, second];
}

const pair1 = pair<string, number>("age", 30);
const pair2 = pair("name", "John");  // Inferred

// With constraints
function merge<T extends object, U extends object>(obj1: T, obj2: U): T & U {
    return { ...obj1, ...obj2 };
}

const result = merge(
    { name: "John" },
    { age: 30, email: "john@example.com" }
);
```

---

## Generic Utility Types

### Partial

```typescript
interface User {
    name: string;
    age: number;
    email: string;
}

function updateUser(id: number, updates: Partial<User>): void {
    // updates can have any/all properties of User
}

updateUser(1, { age: 31 });
updateUser(2, { name: "Jane", email: "jane@example.com" });
```

### Required

```typescript
interface User {
    name?: string;
    age?: number;
}

type CompleteUser = Required<User>;

const user: CompleteUser = {
    name: "John",
    age: 30  // Both required now
};
```

### Pick dan Omit

```typescript
interface User {
    id: number;
    name: string;
    email: string;
    password: string;
}

// Pick specific properties
type UserPreview = Pick<User, 'id' | 'name'>;

const preview: UserPreview = {
    id: 1,
    name: "John"
};

// Omit specific properties
type PublicUser = Omit<User, 'password'>;

const publicUser: PublicUser = {
    id: 1,
    name: "John",
    email: "john@example.com"
};
```

### Record

```typescript
type UserRoles = "admin" | "user" | "guest";

type RolePermissions = Record<UserRoles, string[]>;

const permissions: RolePermissions = {
    admin: ["read", "write", "delete"],
    user: ["read", "write"],
    guest: ["read"]
};
```

---

## Practical Examples

### API Response Wrapper

```typescript
interface ApiResponse<T> {
    success: boolean;
    data?: T;
    error?: string;
    message?: string;
}

async function fetchUser(id: number): Promise<ApiResponse<User>> {
    try {
        const response = await fetch(`/api/users/${id}`);
        const data = await response.json();
        
        return {
            success: true,
            data: data
        };
    } catch (error) {
        return {
            success: false,
            error: error.message
        };
    }
}

// Usage
const response = await fetchUser(1);
if (response.success && response.data) {
    console.log(response.data.name);
}
```

### Generic Repository

```typescript
interface BaseEntity {
    id: number;
}

class GenericRepository<T extends BaseEntity> {
    private items: T[] = [];
    private nextId = 1;
    
    create(item: Omit<T, 'id'>): T {
        const newItem = {
            ...item,
            id: this.nextId++
        } as T;
        
        this.items.push(newItem);
        return newItem;
    }
    
    findById(id: number): T | undefined {
        return this.items.find(item => item.id === id);
    }
    
    findAll(): T[] {
        return [...this.items];
    }
    
    update(id: number, updates: Partial<T>): T | undefined {
        const item = this.findById(id);
        if (item) {
            Object.assign(item, updates);
            return item;
        }
        return undefined;
    }
    
    delete(id: number): boolean {
        const index = this.items.findIndex(item => item.id === id);
        if (index > -1) {
            this.items.splice(index, 1);
            return true;
        }
        return false;
    }
}

// Usage
interface Product extends BaseEntity {
    name: string;
    price: number;
}

const productRepo = new GenericRepository<Product>();

const product = productRepo.create({
    name: "Laptop",
    price: 1000
});

console.log(product.id);  // Auto-generated
```

---

## Best Practices

### DO (Lakukan)

1. **Use generics untuk reusable code**
   ```typescript
   function toArray<T>(value: T): T[] {
       return [value];
   }
   ```

2. **Use constraints when needed**
   ```typescript
   function process<T extends object>(obj: T) {
       // ...
   }
   ```

3. **Use descriptive type parameter names untuk complex generics**
   ```typescript
   // Good
   interface KeyValuePair<TKey, TValue> {
       key: TKey;
       value: TValue;
   }
   ```

4. **Let TypeScript infer when obvious**
   ```typescript
   const result = identity("Hello");  // T inferred
   ```

### DON'T (Jangan)

1. **Jangan over-use generics**
   ```typescript
   // Bad: unnecessary
   function add<T extends number>(a: T, b: T): T {
       return (a + b) as T;
   }
   
   // Good: simple
   function add(a: number, b: number): number {
       return a + b;
   }
   ```

2. **Jangan create generic tanpa constraints jika perlu specific features**

---

## Rangkuman

- Generics untuk create reusable, type-safe code
- Syntax: `<T>` untuk type parameters
- Constraints dengan `<T extends Type>`
- Generic functions, interfaces, dan classes
- Multiple type parameters: `<T, U>`
- Generic utility types: Partial, Required, Pick, Omit, Record
- Type inference works dengan generics
- Use constraints untuk ensure capabilities
- Common pattern: Repository, API wrapper, Container

---

**Sebelumnya:** [06. Classes](06-classes.md)  
**Selanjutnya:** [08. Advanced Types](08-advanced-types.md)

