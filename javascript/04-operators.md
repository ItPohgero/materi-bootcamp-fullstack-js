# 4. Operators

## Arithmetic Operators

Operators untuk operasi matematika.

### Basic Arithmetic

```javascript
let a = 10;
let b = 3;

console.log(a + b);  // 13 (Addition)
console.log(a - b);  // 7  (Subtraction)
console.log(a * b);  // 30 (Multiplication)
console.log(a / b);  // 3.333... (Division)
console.log(a % b);  // 1  (Modulus/Remainder)
console.log(a ** b); // 1000 (Exponentiation)
```

### Increment dan Decrement

```javascript
let count = 5;

// Post-increment (return old value, then increment)
console.log(count++);  // 5
console.log(count);    // 6

// Pre-increment (increment first, then return)
count = 5;
console.log(++count);  // 6
console.log(count);    // 6

// Post-decrement
count = 5;
console.log(count--);  // 5
console.log(count);    // 4

// Pre-decrement
count = 5;
console.log(--count);  // 4
console.log(count);    // 4
```

---

## Assignment Operators

### Basic Assignment

```javascript
let x = 10;  // Assign 10 to x
```

### Compound Assignment

```javascript
let x = 10;

x += 5;   // x = x + 5  → 15
x -= 3;   // x = x - 3  → 12
x *= 2;   // x = x * 2  → 24
x /= 4;   // x = x / 4  → 6
x %= 5;   // x = x % 5  → 1
x **= 3;  // x = x ** 3 → 1
```

---

## Comparison Operators

### Equality Operators

```javascript
// Loose equality (==)
console.log(5 == '5');     // true (type coercion)
console.log(0 == false);   // true
console.log('' == false);  // true

// Strict equality (===)
console.log(5 === '5');    // false (no type coercion)
console.log(0 === false);  // false
console.log('' === false); // false

// Inequality
console.log(5 != '5');     // false
console.log(5 !== '5');    // true (recommended)
```

**Best Practice:** Always use `===` and `!==`

### Relational Operators

```javascript
let a = 10;
let b = 5;

console.log(a > b);   // true  (greater than)
console.log(a < b);   // false (less than)
console.log(a >= 10); // true  (greater than or equal)
console.log(b <= 5);  // true  (less than or equal)
```

---

## Logical Operators

### AND (&&)

Returns true jika SEMUA conditions true.

```javascript
console.log(true && true);    // true
console.log(true && false);   // false
console.log(false && false);  // false

// Dengan values
let age = 25;
let hasLicense = true;

if (age >= 18 && hasLicense) {
    console.log('Can drive');
}

// Short-circuit evaluation
let result = false && expensiveOperation();
// expensiveOperation() tidak dijalankan karena false
```

### OR (||)

Returns true jika SALAH SATU condition true.

```javascript
console.log(true || true);    // true
console.log(true || false);   // true
console.log(false || false);  // false

// Dengan values
let isWeekend = true;
let isHoliday = false;

if (isWeekend || isHoliday) {
    console.log('No work today!');
}

// Default values
let userName = inputName || 'Guest';
```

### NOT (!)

Negasi/membalik boolean value.

```javascript
console.log(!true);   // false
console.log(!false);  // true

let isLoggedIn = false;
if (!isLoggedIn) {
    console.log('Please login');
}

// Double negation (convert to boolean)
console.log(!!'hello');  // true
console.log(!!0);        // false
```

### Combining Logical Operators

```javascript
let age = 25;
let hasLicense = true;
let hasInsurance = false;

// Combination
if (age >= 18 && hasLicense && !hasInsurance) {
    console.log('Need insurance before driving');
}

// Precedence: ! > && > ||
let result = true || false && false;
// Sama dengan: true || (false && false)
console.log(result);  // true
```

---

## Nullish Coalescing Operator (??)

Returns right operand jika left operand adalah `null` atau `undefined`.

```javascript
let value1 = null ?? 'default';
console.log(value1);  // 'default'

let value2 = undefined ?? 'default';
console.log(value2);  // 'default'

let value3 = 0 ?? 'default';
console.log(value3);  // 0 (bukan 'default')

let value4 = '' ?? 'default';
console.log(value4);  // '' (bukan 'default')
```

**Perbedaan dengan ||:**

```javascript
// || treats 0, '', false as falsy
let count1 = 0 || 10;
console.log(count1);  // 10

// ?? only checks null/undefined
let count2 = 0 ?? 10;
console.log(count2);  // 0
```

---

## Ternary Operator (Conditional Operator)

Shorthand untuk if-else statement.

### Syntax

```javascript
condition ? valueIfTrue : valueIfFalse
```

### Examples

```javascript
// Basic
let age = 20;
let status = age >= 18 ? 'Adult' : 'Minor';
console.log(status);  // 'Adult'

// With variables
let price = 100;
let discount = price > 50 ? 10 : 0;
console.log(discount);  // 10

// Nested ternary (avoid if too complex)
let score = 85;
let grade = score >= 90 ? 'A' :
            score >= 80 ? 'B' :
            score >= 70 ? 'C' : 'D';
console.log(grade);  // 'B'

// In function
function getAbsoluteValue(num) {
    return num >= 0 ? num : -num;
}
```

---

## String Operators

### Concatenation (+)

```javascript
let firstName = 'John';
let lastName = 'Doe';

let fullName = firstName + ' ' + lastName;
console.log(fullName);  // 'John Doe'

// With numbers
let result = 'Score: ' + 100;
console.log(result);  // 'Score: 100'
```

### Template Literals (Preferred)

```javascript
let name = 'John';
let age = 30;

let message = `My name is ${name} and I am ${age} years old`;
console.log(message);
```

---

## Type Operators

### typeof

Returns string indicating type.

```javascript
console.log(typeof 'hello');     // 'string'
console.log(typeof 123);         // 'number'
console.log(typeof true);        // 'boolean'
console.log(typeof undefined);   // 'undefined'
console.log(typeof null);        // 'object' (bug)
console.log(typeof {});          // 'object'
console.log(typeof []);          // 'object'
console.log(typeof function(){}); // 'function'
```

### instanceof

Checks if object is instance of class.

```javascript
let arr = [1, 2, 3];
let date = new Date();

console.log(arr instanceof Array);   // true
console.log(date instanceof Date);   // true
console.log(arr instanceof Object);  // true
```

---

## Spread Operator (...)

### Array Spread

```javascript
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

// Combine arrays
let combined = [...arr1, ...arr2];
console.log(combined);  // [1, 2, 3, 4, 5, 6]

// Copy array
let copy = [...arr1];

// Add elements
let withExtra = [0, ...arr1, 4];
console.log(withExtra);  // [0, 1, 2, 3, 4]

// Function arguments
function sum(a, b, c) {
    return a + b + c;
}
let numbers = [1, 2, 3];
console.log(sum(...numbers));  // 6
```

### Object Spread

```javascript
let obj1 = {a: 1, b: 2};
let obj2 = {c: 3, d: 4};

// Combine objects
let combined = {...obj1, ...obj2};
console.log(combined);  // {a: 1, b: 2, c: 3, d: 4}

// Copy object
let copy = {...obj1};

// Override properties
let updated = {...obj1, b: 10};
console.log(updated);  // {a: 1, b: 10}
```

---

## Rest Operator (...)

Collect multiple elements into array.

```javascript
// Function parameters
function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3));        // 6
console.log(sum(1, 2, 3, 4, 5));  // 15

// Array destructuring
let [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first);  // 1
console.log(second); // 2
console.log(rest);   // [3, 4, 5]

// Object destructuring
let {a, b, ...others} = {a: 1, b: 2, c: 3, d: 4};
console.log(a);       // 1
console.log(b);       // 2
console.log(others);  // {c: 3, d: 4}
```

---

## Optional Chaining (?.)

Safely access nested properties.

```javascript
let user = {
    name: 'John',
    address: {
        street: 'Main St',
        city: 'New York'
    }
};

// Without optional chaining
let city1 = user && user.address && user.address.city;

// With optional chaining
let city2 = user?.address?.city;
console.log(city2);  // 'New York'

// If property doesn't exist
let zip = user?.address?.zip;
console.log(zip);  // undefined (no error)

// With functions
let result = user?.getName?.();
// Returns undefined if getName doesn't exist

// With arrays
let firstItem = array?.[0];
```

---

## Operator Precedence

Order of operations (highest to lowest):

1. `()` - Grouping
2. `++`, `--` - Increment/Decrement
3. `!`, `typeof` - Unary
4. `**` - Exponentiation
5. `*`, `/`, `%` - Multiplication, Division, Modulus
6. `+`, `-` - Addition, Subtraction
7. `<`, `>`, `<=`, `>=` - Comparison
8. `==`, `===`, `!=`, `!==` - Equality
9. `&&` - Logical AND
10. `||` - Logical OR
11. `?:` - Ternary
12. `=`, `+=`, `-=`, etc. - Assignment

**Examples:**

```javascript
let result1 = 2 + 3 * 4;
console.log(result1);  // 14 (not 20)

let result2 = (2 + 3) * 4;
console.log(result2);  // 20

let result3 = 10 > 5 && 3 < 4;
console.log(result3);  // true

let result4 = true || false && false;
console.log(result4);  // true (AND executes first)
```

---

## Bitwise Operators (Advanced)

Operations pada binary level.

```javascript
let a = 5;   // 0101
let b = 3;   // 0011

console.log(a & b);   // 1 (0001) - AND
console.log(a | b);   // 7 (0111) - OR
console.log(a ^ b);   // 6 (0110) - XOR
console.log(~a);      // -6      - NOT
console.log(a << 1);  // 10 (1010) - Left shift
console.log(a >> 1);  // 2  (0010) - Right shift
```

**Use Cases:**
- Permissions/flags
- Performance optimization
- Low-level programming

---

## Best Practices

### DO (Lakukan)

1. **Use strict equality**
   ```javascript
   if (value === 10) {
       // ...
   }
   ```

2. **Use ternary untuk simple conditions**
   ```javascript
   let message = isLoggedIn ? 'Welcome' : 'Please login';
   ```

3. **Use optional chaining**
   ```javascript
   let city = user?.address?.city;
   ```

4. **Use nullish coalescing untuk default values**
   ```javascript
   let count = userInput ?? 0;
   ```

5. **Use parentheses untuk clarity**
   ```javascript
   let result = (a + b) * c;
   ```

### DON'T (Jangan)

1. Jangan gunakan `==` (use `===`)
2. Jangan nested ternary terlalu dalam
3. Jangan increment dalam expression yang complex
4. Jangan gunakan bitwise operators tanpa pemahaman

---

## Common Patterns

### Swap Variables

```javascript
// Using temp variable
let a = 1, b = 2;
let temp = a;
a = b;
b = temp;

// Using destructuring (modern)
[a, b] = [b, a];
```

### Toggle Boolean

```javascript
let isOpen = false;
isOpen = !isOpen;  // true
```

### Default Parameters

```javascript
function greet(name = 'Guest') {
    console.log(`Hello ${name}`);
}

// Or with nullish coalescing
function greet2(name) {
    name = name ?? 'Guest';
    console.log(`Hello ${name}`);
}
```

### Check Multiple Conditions

```javascript
// Check if value is one of many
let color = 'red';
if (color === 'red' || color === 'blue' || color === 'green') {
    console.log('Primary color');
}

// Better: use array.includes()
if (['red', 'blue', 'green'].includes(color)) {
    console.log('Primary color');
}
```

---

## Rangkuman

- Arithmetic: `+`, `-`, `*`, `/`, `%`, `**`
- Assignment: `=`, `+=`, `-=`, `*=`, `/=`
- Comparison: `===`, `!==`, `>`, `<`, `>=`, `<=`
- Logical: `&&`, `||`, `!`
- Ternary: `condition ? true : false`
- Nullish coalescing: `??`
- Optional chaining: `?.`
- Spread/Rest: `...`
- Always use `===` instead of `==`
- Use optional chaining untuk safe property access
- Understand operator precedence

---

**Sebelumnya:** [03. Variabel dan Tipe Data](03-variabel-tipe-data.md)  
**Selanjutnya:** [05. Control Flow](05-control-flow.md)

