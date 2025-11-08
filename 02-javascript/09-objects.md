# 9. Objects

## Apa itu Object?

Object adalah collection of key-value pairs. Object digunakan untuk menyimpan data yang complex dan related.

## Membuat Object

### Object Literal

```javascript
let person = {
    name: 'John',
    age: 30,
    city: 'Jakarta'
};
```

### Object Constructor

```javascript
let person = new Object();
person.name = 'John';
person.age = 30;
person.city = 'Jakarta';
```

### Object.create()

```javascript
let personPrototype = {
    greet() {
        return `Hello, I'm ${this.name}`;
    }
};

let person = Object.create(personPrototype);
person.name = 'John';
person.age = 30;
```

---

## Accessing Properties

### Dot Notation

```javascript
let person = {
    name: 'John',
    age: 30
};

console.log(person.name);  // 'John'
console.log(person.age);   // 30
```

### Bracket Notation

```javascript
let person = {
    name: 'John',
    age: 30,
    'home address': 'Jakarta'
};

console.log(person['name']);  // 'John'
console.log(person['home address']);  // 'Jakarta'

// Dynamic property access
let prop = 'age';
console.log(person[prop]);  // 30
```

---

## Adding dan Modifying Properties

```javascript
let person = {
    name: 'John'
};

// Add property
person.age = 30;
person['city'] = 'Jakarta';

// Modify property
person.name = 'Jane';

console.log(person);
// { name: 'Jane', age: 30, city: 'Jakarta' }
```

---

## Deleting Properties

```javascript
let person = {
    name: 'John',
    age: 30,
    city: 'Jakarta'
};

delete person.age;

console.log(person);
// { name: 'John', city: 'Jakarta' }
```

---

## Methods

Function dalam object disebut method.

```javascript
let person = {
    name: 'John',
    age: 30,
    
    // Method
    greet() {
        return `Hello, I'm ${this.name}`;
    },
    
    // Method dengan parameter
    celebrateBirthday() {
        this.age++;
        return `Happy birthday! Now ${this.age} years old`;
    }
};

console.log(person.greet());  // Hello, I'm John
console.log(person.celebrateBirthday());  // Happy birthday! Now 31 years old
```

---

## this Keyword

`this` refers to object yang memanggil method.

```javascript
let person = {
    name: 'John',
    greet() {
        console.log(`Hello, I'm ${this.name}`);
    }
};

person.greet();  // Hello, I'm John

// Arrow functions TIDAK punya this
let person2 = {
    name: 'Jane',
    greet: () => {
        console.log(this.name);  // undefined (this bukan person2)
    }
};
```

---

## Checking Properties

### in Operator

```javascript
let person = {
    name: 'John',
    age: 30
};

console.log('name' in person);    // true
console.log('email' in person);   // false
```

### hasOwnProperty()

```javascript
console.log(person.hasOwnProperty('name'));   // true
console.log(person.hasOwnProperty('email'));  // false
```

### Optional Chaining

```javascript
let user = {
    name: 'John',
    address: {
        city: 'Jakarta'
    }
};

// Safe access
console.log(user?.address?.city);     // 'Jakarta'
console.log(user?.contact?.phone);    // undefined (no error)
```

---

## Iterating Objects

### for...in

```javascript
let person = {
    name: 'John',
    age: 30,
    city: 'Jakarta'
};

for (let key in person) {
    console.log(`${key}: ${person[key]}`);
}
// Output:
// name: John
// age: 30
// city: Jakarta
```

### Object.keys()

```javascript
let keys = Object.keys(person);
console.log(keys);  // ['name', 'age', 'city']

keys.forEach(key => {
    console.log(`${key}: ${person[key]}`);
});
```

### Object.values()

```javascript
let values = Object.values(person);
console.log(values);  // ['John', 30, 'Jakarta']
```

### Object.entries()

```javascript
let entries = Object.entries(person);
console.log(entries);
// [['name', 'John'], ['age', 30], ['city', 'Jakarta']]

// Iteration
for (let [key, value] of Object.entries(person)) {
    console.log(`${key}: ${value}`);
}
```

---

## Object Methods

### Object.assign()

Copy properties dari source ke target.

```javascript
let target = { a: 1, b: 2 };
let source = { b: 3, c: 4 };

Object.assign(target, source);
console.log(target);  // { a: 1, b: 3, c: 4 }

// Clone object
let person = { name: 'John', age: 30 };
let clone = Object.assign({}, person);

// Merge objects
let obj1 = { a: 1 };
let obj2 = { b: 2 };
let merged = Object.assign({}, obj1, obj2);
```

### Spread Operator (Preferred)

```javascript
let obj1 = { a: 1, b: 2 };
let obj2 = { c: 3, d: 4 };

// Copy
let copy = { ...obj1 };

// Merge
let merged = { ...obj1, ...obj2 };
console.log(merged);  // { a: 1, b: 2, c: 3, d: 4 }

// Override properties
let updated = { ...obj1, b: 10 };
console.log(updated);  // { a: 1, b: 10 }
```

### Object.freeze()

Make object immutable.

```javascript
let person = {
    name: 'John',
    age: 30
};

Object.freeze(person);

person.age = 31;  // Tidak berpengaruh
person.city = 'Jakarta';  // Tidak ditambahkan

console.log(person);  // { name: 'John', age: 30 }
```

### Object.seal()

Prevent adding/removing properties, tapi bisa modify.

```javascript
let person = {
    name: 'John',
    age: 30
};

Object.seal(person);

person.age = 31;         // OK
person.city = 'Jakarta'; // Tidak ditambahkan
delete person.name;      // Tidak dihapus

console.log(person);  // { name: 'John', age: 31 }
```

---

## Destructuring

```javascript
let person = {
    name: 'John',
    age: 30,
    city: 'Jakarta'
};

// Extract properties
let { name, age } = person;
console.log(name);  // 'John'
console.log(age);   // 30

// Different variable names
let { name: userName, age: userAge } = person;

// Default values
let { name, country = 'Indonesia' } = person;
console.log(country);  // 'Indonesia'

// Nested destructuring
let user = {
    name: 'John',
    address: {
        city: 'Jakarta',
        country: 'Indonesia'
    }
};

let { address: { city, country } } = user;
console.log(city);  // 'Jakarta'
```

---

## Shorthand Property Names

```javascript
let name = 'John';
let age = 30;

// Old way
let person1 = {
    name: name,
    age: age
};

// Shorthand (ES6)
let person2 = {
    name,
    age
};

console.log(person2);  // { name: 'John', age: 30 }
```

---

## Computed Property Names

```javascript
let propName = 'age';

let person = {
    name: 'John',
    [propName]: 30,
    ['is' + 'Active']: true
};

console.log(person);
// { name: 'John', age: 30, isActive: true }
```

---

## Getters dan Setters

```javascript
let person = {
    firstName: 'John',
    lastName: 'Doe',
    
    // Getter
    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },
    
    // Setter
    set fullName(value) {
        let parts = value.split(' ');
        this.firstName = parts[0];
        this.lastName = parts[1];
    }
};

console.log(person.fullName);  // 'John Doe'

person.fullName = 'Jane Smith';
console.log(person.firstName);  // 'Jane'
console.log(person.lastName);   // 'Smith'
```

---

## Object Composition

### Nested Objects

```javascript
let user = {
    name: 'John',
    age: 30,
    address: {
        street: 'Jl. Sudirman',
        city: 'Jakarta',
        country: 'Indonesia'
    },
    contacts: {
        email: 'john@example.com',
        phone: '081234567890'
    }
};

console.log(user.address.city);     // 'Jakarta'
console.log(user.contacts.email);   // 'john@example.com'
```

### Arrays of Objects

```javascript
let users = [
    { id: 1, name: 'John', age: 30 },
    { id: 2, name: 'Jane', age: 25 },
    { id: 3, name: 'Bob', age: 35 }
];

// Access
console.log(users[0].name);  // 'John'

// Iterate
users.forEach(user => {
    console.log(`${user.name} is ${user.age} years old`);
});

// Filter
let adults = users.filter(user => user.age >= 30);
```

---

## JSON (JavaScript Object Notation)

### JSON.stringify()

Convert object to JSON string.

```javascript
let person = {
    name: 'John',
    age: 30,
    city: 'Jakarta'
};

let json = JSON.stringify(person);
console.log(json);
// '{"name":"John","age":30,"city":"Jakarta"}'

// Dengan formatting
let prettyJson = JSON.stringify(person, null, 2);
console.log(prettyJson);
// {
//   "name": "John",
//   "age": 30,
//   "city": "Jakarta"
// }
```

### JSON.parse()

Convert JSON string to object.

```javascript
let jsonString = '{"name":"John","age":30,"city":"Jakarta"}';

let person = JSON.parse(jsonString);
console.log(person.name);  // 'John'
console.log(person.age);   // 30
```

---

## Common Patterns

### Merge Objects

```javascript
let defaults = {
    theme: 'light',
    language: 'en',
    notifications: true
};

let userSettings = {
    theme: 'dark',
    fontSize: 14
};

// Merge (userSettings override defaults)
let settings = { ...defaults, ...userSettings };
console.log(settings);
// {
//   theme: 'dark',
//   language: 'en',
//   notifications: true,
//   fontSize: 14
// }
```

### Clone Deep Object

```javascript
// Shallow clone
let original = { a: 1, b: { c: 2 } };
let shallow = { ...original };

shallow.b.c = 3;
console.log(original.b.c);  // 3 (modified!)

// Deep clone
let deep = JSON.parse(JSON.stringify(original));

deep.b.c = 4;
console.log(original.b.c);  // 3 (not modified)
```

### Check Empty Object

```javascript
function isEmpty(obj) {
    return Object.keys(obj).length === 0;
}

console.log(isEmpty({}));            // true
console.log(isEmpty({name: 'John'})); // false
```

### Pick Properties

```javascript
function pick(object, keys) {
    return keys.reduce((result, key) => {
        if (key in object) {
            result[key] = object[key];
        }
        return result;
    }, {});
}

let user = {
    id: 1,
    name: 'John',
    email: 'john@example.com',
    password: 'secret',
    role: 'admin'
};

let publicData = pick(user, ['id', 'name', 'email']);
console.log(publicData);
// { id: 1, name: 'John', email: 'john@example.com' }
```

### Omit Properties

```javascript
function omit(object, keys) {
    let result = { ...object };
    keys.forEach(key => delete result[key]);
    return result;
}

let user = {
    id: 1,
    name: 'John',
    password: 'secret',
    role: 'admin'
};

let safeUser = omit(user, ['password']);
console.log(safeUser);
// { id: 1, name: 'John', role: 'admin' }
```

---

## Object Comparison

Objects dibandingkan by reference, bukan by value.

```javascript
let obj1 = { name: 'John' };
let obj2 = { name: 'John' };
let obj3 = obj1;

console.log(obj1 === obj2);  // false (different objects)
console.log(obj1 === obj3);  // true (same reference)

// Compare values
function isEqual(obj1, obj2) {
    return JSON.stringify(obj1) === JSON.stringify(obj2);
}

console.log(isEqual(obj1, obj2));  // true
```

---

## Object Destructuring

```javascript
let person = {
    name: 'John',
    age: 30,
    city: 'Jakarta'
};

// Basic
let { name, age } = person;

// Rename
let { name: userName, age: userAge } = person;

// Default values
let { name, country = 'Indonesia' } = person;

// Nested
let user = {
    name: 'John',
    address: {
        city: 'Jakarta',
        zip: '12345'
    }
};

let { address: { city, zip } } = user;

// Function parameters
function printUser({ name, age }) {
    console.log(`${name} is ${age} years old`);
}

printUser(person);
```

---

## Object Methods

### Object.keys()

```javascript
let person = { name: 'John', age: 30, city: 'Jakarta' };

let keys = Object.keys(person);
console.log(keys);  // ['name', 'age', 'city']
```

### Object.values()

```javascript
let values = Object.values(person);
console.log(values);  // ['John', 30, 'Jakarta']
```

### Object.entries()

```javascript
let entries = Object.entries(person);
console.log(entries);
// [['name', 'John'], ['age', 30], ['city', 'Jakarta']]

// Convert to object
let obj = Object.fromEntries(entries);
```

### Object.assign()

```javascript
let target = { a: 1 };
let source1 = { b: 2 };
let source2 = { c: 3 };

Object.assign(target, source1, source2);
console.log(target);  // { a: 1, b: 2, c: 3 }
```

---

## Object Patterns

### Factory Function

```javascript
function createUser(name, email) {
    return {
        name: name,
        email: email,
        greet() {
            return `Hello, I'm ${this.name}`;
        },
        getEmail() {
            return this.email;
        }
    };
}

let user1 = createUser('John', 'john@example.com');
let user2 = createUser('Jane', 'jane@example.com');

console.log(user1.greet());  // Hello, I'm John
```

### Object with Methods

```javascript
const calculator = {
    result: 0,
    
    add(n) {
        this.result += n;
        return this;  // For chaining
    },
    
    subtract(n) {
        this.result -= n;
        return this;
    },
    
    multiply(n) {
        this.result *= n;
        return this;
    },
    
    getResult() {
        return this.result;
    },
    
    reset() {
        this.result = 0;
        return this;
    }
};

// Method chaining
calculator.add(5).add(3).multiply(2).subtract(4);
console.log(calculator.getResult());  // 12
```

### Private Properties (Using Closures)

```javascript
function createBankAccount(initialBalance) {
    let balance = initialBalance;  // Private
    
    return {
        deposit(amount) {
            if (amount > 0) {
                balance += amount;
                return `Deposited: ${amount}. New balance: ${balance}`;
            }
        },
        
        withdraw(amount) {
            if (amount > 0 && amount <= balance) {
                balance -= amount;
                return `Withdrawn: ${amount}. New balance: ${balance}`;
            }
            return 'Insufficient funds';
        },
        
        getBalance() {
            return balance;
        }
    };
}

let account = createBankAccount(1000);
console.log(account.deposit(500));    // Deposited: 500. New balance: 1500
console.log(account.withdraw(200));   // Withdrawn: 200. New balance: 1300
console.log(account.getBalance());    // 1300
// console.log(account.balance);      // undefined (private!)
```

---

## this Binding

### Method Invocation

```javascript
let person = {
    name: 'John',
    greet() {
        console.log(`Hello, I'm ${this.name}`);
    }
};

person.greet();  // this = person
```

### Lost this Context

```javascript
let person = {
    name: 'John',
    greet() {
        console.log(`Hello, I'm ${this.name}`);
    }
};

let greetFunction = person.greet;
greetFunction();  // this = undefined (atau window di browser)
```

### Preserving this

```javascript
// Option 1: bind()
let boundGreet = person.greet.bind(person);
boundGreet();  // this = person

// Option 2: arrow function wrapper
setTimeout(() => person.greet(), 1000);

// Option 3: arrow function method (loses this!)
let person2 = {
    name: 'Jane',
    greet: () => {
        console.log(this.name);  // undefined
    }
};
```

---

## Object-Oriented Programming (OOP)

### Constructor Function (Old Way)

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};

let person1 = new Person('John', 30);
let person2 = new Person('Jane', 25);

console.log(person1.greet());  // Hello, I'm John
```

### Classes (Modern)

```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    greet() {
        return `Hello, I'm ${this.name}`;
    }
    
    celebrateBirthday() {
        this.age++;
    }
}

let person = new Person('John', 30);
console.log(person.greet());  // Hello, I'm John
person.celebrateBirthday();
console.log(person.age);  // 31
```

### Inheritance

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        return `${this.name} makes a sound`;
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name);  // Call parent constructor
        this.breed = breed;
    }
    
    speak() {
        return `${this.name} barks`;
    }
}

let dog = new Dog('Buddy', 'Golden Retriever');
console.log(dog.speak());  // Buddy barks
```

---

## Best Practices

### DO (Lakukan)

1. **Use object shorthand**
   ```javascript
   let name = 'John';
   let person = { name };  // instead of { name: name }
   ```

2. **Use const untuk objects**
   ```javascript
   const person = { name: 'John' };
   person.age = 30;  // OK
   ```

3. **Use methods daripada functions di object**
   ```javascript
   // Good
   const obj = {
       method() {
           return this.value;
       }
   };
   ```

4. **Destructure untuk cleaner code**
   ```javascript
   function printUser({ name, age }) {
       console.log(name, age);
   }
   ```

5. **Use spread untuk merge**
   ```javascript
   let merged = { ...obj1, ...obj2 };
   ```

### DON'T (Jangan)

1. **Jangan gunakan arrow functions untuk methods**
   ```javascript
   // Bad
   const obj = {
       name: 'John',
       greet: () => console.log(this.name)  // this tidak work!
   };
   ```

2. **Jangan modify object yang di-freeze**

3. **Jangan compare objects dengan ==**
   ```javascript
   // Bad
   if (obj1 === obj2) {  // compares reference
   ```

---

## Rangkuman

- Object adalah collection of key-value pairs
- Access properties dengan dot atau bracket notation
- Methods adalah functions dalam object
- this refers to object yang memanggil method
- Object.keys(), values(), entries() untuk iteration
- Spread operator untuk copy/merge
- Destructuring untuk extract properties
- Object.freeze() dan seal() untuk immutability
- Factory functions untuk create objects
- Classes untuk OOP
- Use const untuk objects
- Use object methods daripada manual loops
- Understand this context

---

**Sebelumnya:** [08. Arrays](08-arrays.md)  
**Selanjutnya:** [10. DOM Manipulation](10-dom-manipulation.md)

