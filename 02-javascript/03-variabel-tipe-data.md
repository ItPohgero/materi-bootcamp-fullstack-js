# 3. Variabel dan Tipe Data

## Variabel

Variabel adalah container untuk menyimpan data. Di JavaScript, ada 3 cara declare variabel:

### var (Old Way - Avoid)

```javascript
var name = 'John';
var age = 25;
```

**Masalah dengan var:**
- Function-scoped (bukan block-scoped)
- Dapat di-redeclare
- Hoisting behavior yang confusing

### let (Modern - Recommended)

```javascript
let name = 'John';
let age = 25;

// Dapat diubah
name = 'Jane';
age = 30;
```

**Karakteristik let:**
- Block-scoped
- Tidak dapat di-redeclare
- Harus declare sebelum use

### const (Modern - Recommended)

```javascript
const PI = 3.14159;
const API_KEY = 'abc123';

// Error! Tidak bisa diubah
// PI = 3.14;  // TypeError
```

**Karakteristik const:**
- Block-scoped
- Tidak dapat diubah (immutable)
- Harus di-initialize saat declare

**Catatan:** Object dan Array yang const masih bisa diubah isinya:

```javascript
const person = {name: 'John'};
person.name = 'Jane';  // OK

const arr = [1, 2, 3];
arr.push(4);  // OK
```

---

## Naming Rules

### Rules (Harus Diikuti)

1. Huruf, angka, underscore, dollar sign
2. Harus dimulai dengan huruf, underscore, atau dollar sign
3. Case-sensitive
4. Tidak boleh reserved keywords

```javascript
// Valid
let firstName = 'John';
let first_name = 'John';
let $price = 100;
let _private = 'secret';

// Invalid
// let 1name = 'John';     // dimulai dengan angka
// let first-name = 'John'; // menggunakan dash
// let let = 'test';        // reserved keyword
```

### Conventions (Best Practice)

```javascript
// camelCase untuk variabel dan functions
let userName = 'John';
let totalPrice = 100;

// PascalCase untuk classes
class UserProfile {
    // ...
}

// UPPER_SNAKE_CASE untuk constants
const MAX_SIZE = 100;
const API_URL = 'https://api.example.com';

// underscore untuk "private" (convention)
let _privateVar = 'secret';
```

---

## Tipe Data Primitif

JavaScript memiliki 7 tipe data primitif:

### 1. String

Text data.

```javascript
let firstName = 'John';
let lastName = "Doe";
let message = `Hello ${firstName}`;  // Template literal

// String methods
let text = 'Hello World';
console.log(text.length);        // 11
console.log(text.toUpperCase()); // 'HELLO WORLD'
console.log(text.toLowerCase()); // 'hello world'
console.log(text.indexOf('o'));  // 4
console.log(text.slice(0, 5));   // 'Hello'
console.log(text.split(' '));    // ['Hello', 'World']
```

**String dengan quotes:**

```javascript
let single = 'Single quotes';
let double = "Double quotes";
let backtick = `Backtick for ${variable}`;

// Escape characters
let quote = 'It\'s a nice day';
let path = "C:\\Users\\John";
let multiline = `Line 1
Line 2
Line 3`;
```

### 2. Number

Integer dan floating-point.

```javascript
let age = 25;
let price = 99.99;
let negative = -10;
let big = 1e6;  // 1000000

// Special numbers
let infinity = Infinity;
let negInfinity = -Infinity;
let notANumber = NaN;

// Number methods
let num = 123.456;
console.log(num.toFixed(2));        // '123.46'
console.log(parseInt('123'));       // 123
console.log(parseFloat('123.45'));  // 123.45
console.log(Number('123'));         // 123
```

**Math operations:**

```javascript
let sum = 10 + 5;      // 15
let diff = 10 - 5;     // 5
let product = 10 * 5;  // 50
let quotient = 10 / 5; // 2
let remainder = 10 % 3; // 1
let power = 2 ** 3;    // 8

// Math object
console.log(Math.round(4.7));    // 5
console.log(Math.ceil(4.1));     // 5
console.log(Math.floor(4.9));    // 4
console.log(Math.max(1, 5, 3));  // 5
console.log(Math.min(1, 5, 3));  // 1
console.log(Math.random());      // random 0-1
```

### 3. Boolean

True atau false.

```javascript
let isActive = true;
let isLoggedIn = false;

// Boolean operations
let result = 10 > 5;   // true
let check = 5 === '5'; // false (strict equality)

// Truthy and Falsy values
// Falsy: false, 0, '', null, undefined, NaN
// Truthy: semua selain falsy

if ('hello') {
    console.log('String is truthy');
}

if (0) {
    console.log('This won\'t run');
}
```

### 4. Undefined

Variabel yang belum di-assign value.

```javascript
let x;
console.log(x);  // undefined

let obj = {name: 'John'};
console.log(obj.age);  // undefined
```

### 5. Null

Intentional absence of value.

```javascript
let user = null;  // no user

// Perbedaan null vs undefined
let a;           // undefined (belum di-set)
let b = null;    // null (sengaja di-set kosong)
```

### 6. Symbol (ES6)

Unique identifier.

```javascript
let id1 = Symbol('id');
let id2 = Symbol('id');

console.log(id1 === id2);  // false (selalu unique)
```

### 7. BigInt (ES2020)

Integer yang sangat besar.

```javascript
let bigNumber = 1234567890123456789012345678901234567890n;
let another = BigInt('1234567890123456789012345678901234567890');

console.log(bigNumber + 1n);  // Harus operasi dengan BigInt juga
```

---

## Tipe Data Non-Primitif

### Object

Collection of key-value pairs.

```javascript
let person = {
    name: 'John',
    age: 30,
    city: 'Jakarta'
};

// Access properties
console.log(person.name);      // 'John'
console.log(person['age']);    // 30

// Modify
person.age = 31;
person.email = 'john@example.com';

// Delete
delete person.city;
```

### Array

Ordered list of values.

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];
let numbers = [1, 2, 3, 4, 5];
let mixed = [1, 'text', true, null, {name: 'John'}];

// Access elements
console.log(fruits[0]);  // 'Apple'
console.log(fruits[1]);  // 'Banana'

// Modify
fruits[0] = 'Mango';
fruits.push('Grape');    // Add to end
fruits.pop();            // Remove from end
fruits.unshift('Kiwi'); // Add to start
fruits.shift();          // Remove from start

// Length
console.log(fruits.length);  // 3
```

### Function

Reusable block of code.

```javascript
function greet(name) {
    return `Hello ${name}`;
}

// Function expression
let sayHi = function(name) {
    return `Hi ${name}`;
};

// Arrow function
let sayHello = (name) => `Hello ${name}`;
```

---

## Type Checking

### typeof Operator

```javascript
console.log(typeof 'Hello');      // 'string'
console.log(typeof 123);          // 'number'
console.log(typeof true);         // 'boolean'
console.log(typeof undefined);    // 'undefined'
console.log(typeof null);         // 'object' (bug di JavaScript!)
console.log(typeof {});           // 'object'
console.log(typeof []);           // 'object'
console.log(typeof function(){}); // 'function'
console.log(typeof Symbol());     // 'symbol'
console.log(typeof 10n);          // 'bigint'
```

### instanceof Operator

```javascript
let arr = [1, 2, 3];
let obj = {name: 'John'};

console.log(arr instanceof Array);   // true
console.log(obj instanceof Object);  // true
console.log(arr instanceof Object);  // true (Array adalah Object)
```

### Array.isArray()

```javascript
console.log(Array.isArray([1, 2, 3]));  // true
console.log(Array.isArray('text'));     // false
```

---

## Type Conversion

### Explicit Conversion

**To String:**

```javascript
let num = 123;
let str1 = String(num);
let str2 = num.toString();
let str3 = '' + num;

console.log(typeof str1);  // 'string'
```

**To Number:**

```javascript
let str = '123';
let num1 = Number(str);
let num2 = parseInt(str);
let num3 = parseFloat('123.45');
let num4 = +str;

console.log(typeof num1);  // 'number'

// NaN jika tidak bisa convert
console.log(Number('hello'));  // NaN
```

**To Boolean:**

```javascript
let bool1 = Boolean(1);        // true
let bool2 = Boolean(0);        // false
let bool3 = Boolean('hello');  // true
let bool4 = Boolean('');       // false

// Double negation trick
let bool5 = !!'hello';  // true
```

### Implicit Conversion (Type Coercion)

```javascript
// String coercion
console.log('5' + 3);    // '53' (string)
console.log('5' + true); // '5true' (string)

// Number coercion
console.log('5' - 3);    // 2 (number)
console.log('5' * '2');  // 10 (number)
console.log('5' / '2');  // 2.5 (number)

// Boolean coercion
console.log(1 + true);   // 2 (true = 1)
console.log(1 + false);  // 1 (false = 0)

// Comparison coercion
console.log('5' == 5);   // true (loose equality)
console.log('5' === 5);  // false (strict equality)
```

---

## Comparison Operators

### Equality

```javascript
// Loose equality (==) - dengan type coercion
console.log(5 == '5');      // true
console.log(0 == false);    // true
console.log(null == undefined);  // true

// Strict equality (===) - tanpa type coercion
console.log(5 === '5');     // false
console.log(0 === false);   // false
console.log(null === undefined);  // false
```

**Always use === (strict equality) unless you have specific reason!**

### Comparison

```javascript
console.log(5 > 3);    // true
console.log(5 < 3);    // false
console.log(5 >= 5);   // true
console.log(5 <= 4);   // false
console.log(5 != 3);   // true
console.log(5 !== '5'); // true
```

---

## Template Literals (ES6)

```javascript
let name = 'John';
let age = 30;

// Old way
let message1 = 'My name is ' + name + ' and I am ' + age + ' years old';

// Template literal
let message2 = `My name is ${name} and I am ${age} years old`;

// Expressions
let total = `Total: ${10 + 5}`;  // 'Total: 15'

// Multi-line
let html = `
    <div>
        <h1>${name}</h1>
        <p>Age: ${age}</p>
    </div>
`;
```

---

## Destructuring

### Array Destructuring

```javascript
let colors = ['red', 'green', 'blue'];

// Old way
let color1 = colors[0];
let color2 = colors[1];

// Destructuring
let [first, second, third] = colors;
console.log(first);   // 'red'
console.log(second);  // 'green'

// Skip elements
let [, , third] = colors;

// Rest operator
let [first, ...rest] = colors;
console.log(rest);  // ['green', 'blue']
```

### Object Destructuring

```javascript
let person = {
    name: 'John',
    age: 30,
    city: 'Jakarta'
};

// Destructuring
let {name, age, city} = person;
console.log(name);  // 'John'

// Different variable names
let {name: userName, age: userAge} = person;
console.log(userName);  // 'John'

// Default values
let {name, country = 'Indonesia'} = person;
console.log(country);  // 'Indonesia'
```

---

## Best Practices

### DO (Lakukan)

1. **Gunakan const by default, let jika perlu**
   ```javascript
   const PI = 3.14159;
   let counter = 0;
   ```

2. **Use strict equality (===)**
   ```javascript
   if (value === 10) {
       // ...
   }
   ```

3. **Descriptive variable names**
   ```javascript
   // Good
   let userAge = 25;
   let isLoggedIn = true;
   
   // Bad
   let a = 25;
   let x = true;
   ```

4. **Initialize variables**
   ```javascript
   let count = 0;
   let name = '';
   let items = [];
   ```

### DON'T (Jangan)

1. Jangan gunakan var
2. Jangan gunakan loose equality (==)
3. Jangan gunakan implicit global variables
4. Jangan gunakan single letter names (kecuali loop counter)

---

## Rangkuman

- Gunakan `const` untuk values yang tidak berubah
- Gunakan `let` untuk values yang berubah
- Hindari `var`
- 7 tipe primitif: String, Number, Boolean, Undefined, Null, Symbol, BigInt
- Tipe non-primitif: Object, Array, Function
- Use `typeof` untuk check type
- Use `===` untuk strict equality
- Template literals untuk string interpolation
- Destructuring untuk extract values dari array/object

---

**Sebelumnya:** [02. Setup Environment](02-setup-environment.md)  
**Selanjutnya:** [04. Operators](04-operators.md)

