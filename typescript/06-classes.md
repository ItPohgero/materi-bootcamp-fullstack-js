# 6. Classes

## Basic Class

```typescript
class Person {
    name: string;
    age: number;
    
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
    
    greet(): string {
        return `Hello, I'm ${this.name}`;
    }
}

const person = new Person("John", 30);
console.log(person.greet());  // "Hello, I'm John"
```

---

## Access Modifiers

### public (Default)

Accessible dari anywhere.

```typescript
class User {
    public name: string;  // public keyword optional
    
    constructor(name: string) {
        this.name = name;
    }
}

const user = new User("John");
console.log(user.name);  // OK
```

### private

Hanya accessible dalam class.

```typescript
class BankAccount {
    private balance: number;
    
    constructor(initialBalance: number) {
        this.balance = initialBalance;
    }
    
    getBalance(): number {
        return this.balance;
    }
    
    deposit(amount: number): void {
        this.balance += amount;
    }
}

const account = new BankAccount(1000);
console.log(account.balance);  // Error: private
console.log(account.getBalance());  // OK: 1000
```

### protected

Accessible dalam class dan subclasses.

```typescript
class Animal {
    protected name: string;
    
    constructor(name: string) {
        this.name = name;
    }
}

class Dog extends Animal {
    bark(): string {
        return `${this.name} says Woof!`;  // OK: can access protected
    }
}

const dog = new Dog("Buddy");
console.log(dog.bark());    // OK
console.log(dog.name);      // Error: protected
```

---

## Readonly Properties

```typescript
class User {
    readonly id: number;
    name: string;
    
    constructor(id: number, name: string) {
        this.id = id;
        this.name = name;
    }
    
    updateName(newName: string): void {
        this.name = newName;  // OK
        // this.id = 123;     // Error: readonly
    }
}
```

---

## Parameter Properties

Shorthand untuk declare dan initialize properties.

```typescript
// Long way
class User {
    name: string;
    age: number;
    
    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }
}

// Shorthand (parameter properties)
class User2 {
    constructor(
        public name: string,
        public age: number,
        private password: string
    ) {
        // Properties automatically created and assigned
    }
}

const user = new User2("John", 30, "secret");
console.log(user.name);      // OK
console.log(user.password);  // Error: private
```

---

## Getters dan Setters

```typescript
class User {
    private _age: number = 0;
    
    get age(): number {
        return this._age;
    }
    
    set age(value: number) {
        if (value < 0) {
            throw new Error("Age cannot be negative");
        }
        this._age = value;
    }
}

const user = new User();
user.age = 30;     // Calls setter
console.log(user.age);  // Calls getter: 30
user.age = -5;     // Error thrown
```

---

## Static Members

```typescript
class MathUtils {
    static PI: number = 3.14159;
    
    static add(a: number, b: number): number {
        return a + b;
    }
    
    static calculateCircleArea(radius: number): number {
        return this.PI * radius * radius;
    }
}

// Access without instantiation
console.log(MathUtils.PI);
console.log(MathUtils.add(5, 3));
console.log(MathUtils.calculateCircleArea(10));

// No need to create instance
// const math = new MathUtils();  // Not needed
```

---

## Inheritance

### Extends

```typescript
class Animal {
    constructor(public name: string) {}
    
    move(distance: number): void {
        console.log(`${this.name} moved ${distance}m`);
    }
}

class Dog extends Animal {
    bark(): void {
        console.log(`${this.name} barks!`);
    }
}

const dog = new Dog("Buddy");
dog.move(10);  // Inherited from Animal
dog.bark();    // Own method
```

### super Keyword

```typescript
class Animal {
    constructor(public name: string) {
        console.log(`Animal created: ${name}`);
    }
    
    move(): void {
        console.log(`${this.name} is moving`);
    }
}

class Dog extends Animal {
    constructor(name: string, public breed: string) {
        super(name);  // Call parent constructor
        console.log(`Dog breed: ${breed}`);
    }
    
    move(): void {
        console.log(`${this.name} is running`);
        super.move();  // Call parent method
    }
}
```

---

## Abstract Classes

Cannot be instantiated, only extended.

```typescript
abstract class Shape {
    constructor(public color: string) {}
    
    // Abstract method (must be implemented by subclasses)
    abstract getArea(): number;
    
    // Concrete method
    describe(): string {
        return `A ${this.color} shape with area ${this.getArea()}`;
    }
}

class Circle extends Shape {
    constructor(color: string, public radius: number) {
        super(color);
    }
    
    getArea(): number {
        return Math.PI * this.radius * this.radius;
    }
}

class Rectangle extends Shape {
    constructor(
        color: string,
        public width: number,
        public height: number
    ) {
        super(color);
    }
    
    getArea(): number {
        return this.width * this.height;
    }
}

// const shape = new Shape("red");  // Error: cannot instantiate abstract class
const circle = new Circle("red", 10);
console.log(circle.describe());
```

---

## Implementing Interfaces

```typescript
interface Printable {
    print(): void;
}

interface Saveable {
    save(): void;
}

class Document implements Printable, Saveable {
    constructor(public content: string) {}
    
    print(): void {
        console.log(this.content);
    }
    
    save(): void {
        console.log("Saving document...");
    }
}

const doc = new Document("Hello World");
doc.print();
doc.save();
```

---

## Generic Classes

```typescript
class Box<T> {
    private value: T;
    
    constructor(value: T) {
        this.value = value;
    }
    
    getValue(): T {
        return this.value;
    }
    
    setValue(value: T): void {
        this.value = value;
    }
}

const stringBox = new Box<string>("Hello");
const numberBox = new Box<number>(123);

console.log(stringBox.getValue());  // "Hello"
console.log(numberBox.getValue());  // 123

// Type inference
const autoBox = new Box("Auto");  // Box<string> inferred
```

---

## Class Expression

```typescript
const Person = class {
    constructor(public name: string) {}
    
    greet(): string {
        return `Hello, I'm ${this.name}`;
    }
};

const person = new Person("John");
console.log(person.greet());
```

---

## Practical Examples

### User Management

```typescript
interface IUser {
    id: number;
    username: string;
    email: string;
}

class User implements IUser {
    constructor(
        public id: number,
        public username: string,
        public email: string,
        private password: string
    ) {}
    
    validatePassword(password: string): boolean {
        return this.password === password;
    }
    
    changePassword(oldPassword: string, newPassword: string): boolean {
        if (this.validatePassword(oldPassword)) {
            this.password = newPassword;
            return true;
        }
        return false;
    }
}

class UserManager {
    private users: User[] = [];
    private nextId: number = 1;
    
    createUser(username: string, email: string, password: string): User {
        const user = new User(this.nextId++, username, email, password);
        this.users.push(user);
        return user;
    }
    
    findById(id: number): User | undefined {
        return this.users.find(u => u.id === id);
    }
    
    findByUsername(username: string): User | undefined {
        return this.users.find(u => u.username === username);
    }
}
```

### Data Repository Pattern

```typescript
abstract class Repository<T> {
    protected items: T[] = [];
    
    add(item: T): void {
        this.items.push(item);
    }
    
    findAll(): T[] {
        return [...this.items];
    }
    
    abstract findById(id: number): T | undefined;
}

interface Product {
    id: number;
    name: string;
    price: number;
}

class ProductRepository extends Repository<Product> {
    findById(id: number): Product | undefined {
        return this.items.find(p => p.id === id);
    }
    
    findByPriceRange(min: number, max: number): Product[] {
        return this.items.filter(p => p.price >= min && p.price <= max);
    }
}
```

---

## Best Practices

### DO (Lakukan)

1. **Use parameter properties**
   ```typescript
   class User {
       constructor(
           public name: string,
           private password: string
       ) {}
   }
   ```

2. **Use access modifiers**
   ```typescript
   class BankAccount {
       private balance: number;
       public getBalance() { return this.balance; }
   }
   ```

3. **Implement interfaces**
   ```typescript
   interface Saveable {
       save(): void;
   }
   
   class Document implements Saveable {
       save() { /* ... */ }
   }
   ```

4. **Use abstract classes untuk base classes**
   ```typescript
   abstract class BaseRepository {
       abstract save(): void;
   }
   ```

### DON'T (Jangan)

1. **Jangan make everything public**
2. **Jangan create classes untuk simple objects (use interface)**
3. **Jangan skip access modifiers**

---

## Rangkuman

- Classes dengan typed properties dan methods
- Access modifiers: public, private, protected
- Parameter properties untuk concise code
- Getters dan setters untuk encapsulation
- Static members untuk class-level functionality
- Inheritance dengan extends
- Abstract classes untuk base classes
- Implement interfaces
- Generic classes untuk type reusability
- Use classes untuk complex behavior
- Use interfaces untuk simple data structures

---

**Sebelumnya:** [05. Functions](05-functions.md)  
**Selanjutnya:** [07. Generics](07-generics.md)

