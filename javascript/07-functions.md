# 7. Functions

## Apa itu Function?

Function adalah block of code yang dapat digunakan ulang untuk melakukan task tertentu.

## Function Declaration

### Basic Function

```javascript
function greet() {
    console.log('Hello!');
}

// Call function
greet();  // Output: Hello!
```

### Function dengan Parameters

```javascript
function greet(name) {
    console.log(`Hello ${name}!`);
}

greet('John');  // Hello John!
greet('Jane');  // Hello Jane!

// Multiple parameters
function add(a, b) {
    return a + b;
}

let result = add(5, 3);
console.log(result);  // 8
```

### Return Statement

```javascript
function multiply(a, b) {
    return a * b;
}

let result = multiply(5, 4);
console.log(result);  // 20

// Function tanpa return → undefined
function sayHi() {
    console.log('Hi');
}

let x = sayHi();
console.log(x);  // undefined
```

---

## Function Expression

Function yang di-assign ke variable.

```javascript
const greet = function(name) {
    return `Hello ${name}`;
};

console.log(greet('John'));  // Hello John
```

### Anonymous Function

```javascript
// Function tanpa nama
const square = function(x) {
    return x * x;
};

// Sering digunakan sebagai callback
setTimeout(function() {
    console.log('Executed after 1 second');
}, 1000);
```

---

## Arrow Functions (ES6)

Shorthand syntax untuk function.

### Basic Arrow Function

```javascript
// Function expression
const greet = function(name) {
    return `Hello ${name}`;
};

// Arrow function
const greet2 = (name) => {
    return `Hello ${name}`;
};

// Concise arrow function (implicit return)
const greet3 = name => `Hello ${name}`;
```

### Examples

```javascript
// No parameters
const sayHi = () => console.log('Hi');

// One parameter (parentheses optional)
const square = x => x * x;
const cube = (x) => x ** 3;

// Multiple parameters
const add = (a, b) => a + b;
const multiply = (a, b) => {
    let result = a * b;
    return result;
};

// Returning object (wrap in parentheses)
const createPerson = (name, age) => ({
    name: name,
    age: age
});
```

---

## Default Parameters

```javascript
function greet(name = 'Guest') {
    return `Hello ${name}`;
}

console.log(greet());        // Hello Guest
console.log(greet('John'));  // Hello John

// Multiple defaults
function createUser(name, role = 'user', active = true) {
    return { name, role, active };
}

console.log(createUser('John'));
// { name: 'John', role: 'user', active: true }
```

---

## Rest Parameters

Collect unlimited arguments into array.

```javascript
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3));        // 6
console.log(sum(1, 2, 3, 4, 5));  // 15

// Mix dengan regular parameters
function greetAll(greeting, ...names) {
    names.forEach(name => {
        console.log(`${greeting} ${name}`);
    });
}

greetAll('Hello', 'John', 'Jane', 'Bob');
// Hello John
// Hello Jane
// Hello Bob
```

---

## Function Scope

### Local Variables

```javascript
function myFunction() {
    let localVar = 'local';
    console.log(localVar);  // OK
}

myFunction();
// console.log(localVar);  // Error: localVar not defined
```

### Global Variables

```javascript
let globalVar = 'global';

function myFunction() {
    console.log(globalVar);  // OK: can access global
}

myFunction();
console.log(globalVar);  // OK
```

### Block Scope (let/const)

```javascript
function test() {
    if (true) {
        let blockVar = 'block';
        var functionVar = 'function';
    }
    
    // console.log(blockVar);  // Error
    console.log(functionVar);  // OK
}
```

---

## Closures

Function yang "remember" its scope.

```javascript
function outer() {
    let count = 0;
    
    function inner() {
        count++;
        console.log(count);
    }
    
    return inner;
}

let counter = outer();
counter();  // 1
counter();  // 2
counter();  // 3
```

### Practical Example: Counter

```javascript
function createCounter() {
    let count = 0;
    
    return {
        increment() {
            return ++count;
        },
        decrement() {
            return --count;
        },
        getCount() {
            return count;
        }
    };
}

let counter = createCounter();
console.log(counter.increment());  // 1
console.log(counter.increment());  // 2
console.log(counter.getCount());   // 2
console.log(counter.decrement());  // 1
```

---

## Callback Functions

Function yang dipassing sebagai argument.

```javascript
function processUser(name, callback) {
    console.log(`Processing ${name}...`);
    callback(name);
}

processUser('John', function(name) {
    console.log(`Finished processing ${name}`);
});

// With arrow function
processUser('Jane', name => {
    console.log(`Finished processing ${name}`);
});
```

### Array Methods dengan Callbacks

```javascript
let numbers = [1, 2, 3, 4, 5];

// forEach
numbers.forEach(num => console.log(num));

// map
let doubled = numbers.map(num => num * 2);

// filter
let evens = numbers.filter(num => num % 2 === 0);

// find
let firstEven = numbers.find(num => num % 2 === 0);
```

---

## Higher-Order Functions

Function yang menerima atau return function lain.

```javascript
// Function returns function
function multiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

let double = multiplier(2);
let triple = multiplier(3);

console.log(double(5));  // 10
console.log(triple(5));  // 15

// Function menerima function
function repeat(n, action) {
    for (let i = 0; i < n; i++) {
        action(i);
    }
}

repeat(3, i => console.log(`Iteration ${i}`));
```

---

## Immediately Invoked Function Expression (IIFE)

Function yang langsung dijalankan.

```javascript
(function() {
    console.log('This runs immediately');
})();

// With parameters
(function(name) {
    console.log(`Hello ${name}`);
})('John');

// Arrow IIFE
(() => {
    console.log('Arrow IIFE');
})();

// Use case: private scope
const counter = (function() {
    let count = 0;
    return {
        increment: () => ++count,
        getCount: () => count
    };
})();
```

---

## Recursive Functions

Function yang memanggil dirinya sendiri.

### Factorial

```javascript
function factorial(n) {
    // Base case
    if (n === 0 || n === 1) {
        return 1;
    }
    
    // Recursive case
    return n * factorial(n - 1);
}

console.log(factorial(5));  // 120
// 5 * 4 * 3 * 2 * 1
```

### Fibonacci

```javascript
function fibonacci(n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

console.log(fibonacci(7));  // 13
```

### Countdown

```javascript
function countdown(n) {
    if (n < 0) return;
    console.log(n);
    countdown(n - 1);
}

countdown(5);  // 5, 4, 3, 2, 1, 0
```

---

## Best Practices

### DO (Lakukan)

1. **Use arrow functions untuk callbacks**
   ```javascript
   array.map(item => item * 2);
   ```

2. **One purpose per function**
   ```javascript
   function calculateTotal(items) {
       return items.reduce((sum, item) => sum + item.price, 0);
   }
   ```

3. **Use descriptive names**
   ```javascript
   function calculateUserAge(birthDate) {
       // ...
   }
   ```

4. **Return early**
   ```javascript
   function divide(a, b) {
       if (b === 0) return null;
       return a / b;
   }
   ```

5. **Use default parameters**
   ```javascript
   function greet(name = 'Guest') {
       return `Hello ${name}`;
   }
   ```

### DON'T (Jangan)

1. **Jangan function terlalu panjang (max ~20 lines)**
2. **Jangan terlalu banyak parameters (max 3-4)**
3. **Jangan modify global variables dari function**
4. **Jangan recursion tanpa base case**

---

## Function Patterns

### Factory Function

```javascript
function createUser(name, email) {
    return {
        name: name,
        email: email,
        greet() {
            return `Hello, I'm ${this.name}`;
        }
    };
}

let user1 = createUser('John', 'john@example.com');
let user2 = createUser('Jane', 'jane@example.com');
```

### Module Pattern

```javascript
const calculator = (function() {
    // Private variables
    let result = 0;
    
    // Public API
    return {
        add(n) {
            result += n;
            return this;
        },
        subtract(n) {
            result -= n;
            return this;
        },
        getResult() {
            return result;
        },
        reset() {
            result = 0;
            return this;
        }
    };
})();

calculator.add(5).add(3).subtract(2);
console.log(calculator.getResult());  // 6
```

---

## Rangkuman

- Function declaration: `function name() {}`
- Function expression: `const name = function() {}`
- Arrow function: `const name = () => {}`
- Parameters dan return values
- Default parameters
- Rest parameters untuk unlimited arguments
- Scope: local, global, block
- Closures: function remember scope
- Callbacks: function sebagai argument
- Higher-order functions
- IIFE untuk immediate execution
- Recursive functions
- Use arrow functions untuk callbacks
- One function, one purpose
- Return early untuk clean code

---

**Sebelumnya:** [06. Loops](06-loops.md)  
**Selanjutnya:** [08. Arrays](08-arrays.md)

