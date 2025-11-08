# 8. Arrays

## Apa itu Array?

Array adalah struktur data yang menyimpan multiple values dalam satu variable.

## Membuat Array

### Array Literal

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];
let numbers = [1, 2, 3, 4, 5];
let mixed = [1, 'text', true, null, {name: 'John'}];
let empty = [];
```

### Array Constructor

```javascript
let arr1 = new Array();  // Empty array
let arr2 = new Array(5);  // Array dengan 5 empty slots
let arr3 = new Array(1, 2, 3);  // [1, 2, 3]
```

### Array.of()

```javascript
let arr1 = Array.of(5);        // [5]
let arr2 = Array.of(1, 2, 3);  // [1, 2, 3]
```

---

## Accessing Elements

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];

// Access by index (0-based)
console.log(fruits[0]);  // 'Apple'
console.log(fruits[1]);  // 'Banana'
console.log(fruits[2]);  // 'Orange'

// Last element
console.log(fruits[fruits.length - 1]);  // 'Orange'

// Negative index (tidak didukung, use at())
// fruits[-1]  // undefined

// at() method (ES2022)
console.log(fruits.at(0));   // 'Apple'
console.log(fruits.at(-1));  // 'Orange' (last)
console.log(fruits.at(-2));  // 'Banana' (second last)
```

---

## Array Properties

### length

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];

console.log(fruits.length);  // 3

// Set length (truncate array)
fruits.length = 2;
console.log(fruits);  // ['Apple', 'Banana']

// Clear array
fruits.length = 0;
console.log(fruits);  // []
```

---

## Adding Elements

### push()

Tambah element di akhir.

```javascript
let fruits = ['Apple', 'Banana'];

fruits.push('Orange');
console.log(fruits);  // ['Apple', 'Banana', 'Orange']

fruits.push('Mango', 'Grape');
console.log(fruits);  // ['Apple', 'Banana', 'Orange', 'Mango', 'Grape']

// Return new length
let newLength = fruits.push('Kiwi');
console.log(newLength);  // 6
```

### unshift()

Tambah element di awal.

```javascript
let fruits = ['Banana', 'Orange'];

fruits.unshift('Apple');
console.log(fruits);  // ['Apple', 'Banana', 'Orange']

fruits.unshift('Mango', 'Grape');
console.log(fruits);  // ['Mango', 'Grape', 'Apple', 'Banana', 'Orange']
```

### splice()

Insert elements di posisi tertentu.

```javascript
let fruits = ['Apple', 'Orange'];

// splice(index, deleteCount, item1, item2, ...)
fruits.splice(1, 0, 'Banana');
console.log(fruits);  // ['Apple', 'Banana', 'Orange']
```

---

## Removing Elements

### pop()

Hapus element terakhir.

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];

let removed = fruits.pop();
console.log(removed);  // 'Orange'
console.log(fruits);   // ['Apple', 'Banana']
```

### shift()

Hapus element pertama.

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];

let removed = fruits.shift();
console.log(removed);  // 'Apple'
console.log(fruits);   // ['Banana', 'Orange']
```

### splice()

Hapus element di posisi tertentu.

```javascript
let fruits = ['Apple', 'Banana', 'Orange', 'Mango'];

// splice(index, deleteCount)
fruits.splice(1, 2);  // Hapus 2 elements mulai dari index 1
console.log(fruits);  // ['Apple', 'Mango']
```

---

## Array Methods

### slice()

Extract bagian array (tidak mengubah original).

```javascript
let fruits = ['Apple', 'Banana', 'Orange', 'Mango', 'Grape'];

let slice1 = fruits.slice(1, 3);  // Index 1 to 2 (not including 3)
console.log(slice1);  // ['Banana', 'Orange']

let slice2 = fruits.slice(2);  // From index 2 to end
console.log(slice2);  // ['Orange', 'Mango', 'Grape']

let slice3 = fruits.slice(-2);  // Last 2 elements
console.log(slice3);  // ['Mango', 'Grape']

// Original tidak berubah
console.log(fruits);  // ['Apple', 'Banana', 'Orange', 'Mango', 'Grape']
```

### concat()

Gabung arrays.

```javascript
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

let combined = arr1.concat(arr2);
console.log(combined);  // [1, 2, 3, 4, 5, 6]

// Multiple arrays
let arr3 = [7, 8];
let all = arr1.concat(arr2, arr3);
console.log(all);  // [1, 2, 3, 4, 5, 6, 7, 8]

// Modern: spread operator
let combined2 = [...arr1, ...arr2];
```

### join()

Gabung elements jadi string.

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];

let str1 = fruits.join();
console.log(str1);  // 'Apple,Banana,Orange'

let str2 = fruits.join(' - ');
console.log(str2);  // 'Apple - Banana - Orange'

let str3 = fruits.join('');
console.log(str3);  // 'AppleBananaOrange'
```

### reverse()

Reverse array (mengubah original).

```javascript
let numbers = [1, 2, 3, 4, 5];

numbers.reverse();
console.log(numbers);  // [5, 4, 3, 2, 1]
```

### sort()

Sort array (mengubah original).

```javascript
// Sort strings
let fruits = ['Orange', 'Apple', 'Banana'];
fruits.sort();
console.log(fruits);  // ['Apple', 'Banana', 'Orange']

// Sort numbers (need compare function)
let numbers = [3, 1, 4, 1, 5, 9, 2, 6];

// Wrong: converts to string first
numbers.sort();
console.log(numbers);  // [1, 1, 2, 3, 4, 5, 6, 9] (works for this case)

let numbers2 = [10, 5, 40, 25, 1000, 1];
numbers2.sort();
console.log(numbers2);  // [1, 10, 1000, 25, 40, 5] (WRONG!)

// Correct: use compare function
numbers2.sort((a, b) => a - b);  // Ascending
console.log(numbers2);  // [1, 5, 10, 25, 40, 1000]

numbers2.sort((a, b) => b - a);  // Descending
console.log(numbers2);  // [1000, 40, 25, 10, 5, 1]
```

### indexOf() dan lastIndexOf()

```javascript
let fruits = ['Apple', 'Banana', 'Orange', 'Banana'];

console.log(fruits.indexOf('Banana'));      // 1 (first occurrence)
console.log(fruits.lastIndexOf('Banana'));  // 3 (last occurrence)
console.log(fruits.indexOf('Mango'));       // -1 (not found)
```

### includes()

Check jika element ada.

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];

console.log(fruits.includes('Banana'));  // true
console.log(fruits.includes('Mango'));   // false

// Use case
let allowedColors = ['red', 'green', 'blue'];
let userColor = 'red';

if (allowedColors.includes(userColor)) {
    console.log('Valid color');
}
```

---

## Iteration Methods

### forEach()

Execute function untuk setiap element.

```javascript
let numbers = [1, 2, 3, 4, 5];

numbers.forEach(function(num) {
    console.log(num);
});

// Arrow function
numbers.forEach(num => console.log(num));

// With index dan array
numbers.forEach((num, index, arr) => {
    console.log(`Index ${index}: ${num}`);
});
```

### map()

Transform setiap element (return new array).

```javascript
let numbers = [1, 2, 3, 4, 5];

let doubled = numbers.map(num => num * 2);
console.log(doubled);  // [2, 4, 6, 8, 10]

let squared = numbers.map(num => num ** 2);
console.log(squared);  // [1, 4, 9, 16, 25]

// Transform objects
let users = [
    { firstName: 'John', lastName: 'Doe' },
    { firstName: 'Jane', lastName: 'Smith' }
];

let fullNames = users.map(user => `${user.firstName} ${user.lastName}`);
console.log(fullNames);  // ['John Doe', 'Jane Smith']
```

### filter()

Filter elements berdasarkan condition.

```javascript
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

let evens = numbers.filter(num => num % 2 === 0);
console.log(evens);  // [2, 4, 6, 8, 10]

let greaterThan5 = numbers.filter(num => num > 5);
console.log(greaterThan5);  // [6, 7, 8, 9, 10]

// Filter objects
let users = [
    { name: 'John', age: 25, active: true },
    { name: 'Jane', age: 30, active: false },
    { name: 'Bob', age: 22, active: true }
];

let activeUsers = users.filter(user => user.active);
console.log(activeUsers);
// [{ name: 'John', ... }, { name: 'Bob', ... }]
```

### reduce()

Reduce array to single value.

```javascript
let numbers = [1, 2, 3, 4, 5];

// Sum
let sum = numbers.reduce((total, num) => total + num, 0);
console.log(sum);  // 15

// Product
let product = numbers.reduce((total, num) => total * num, 1);
console.log(product);  // 120

// Max
let max = numbers.reduce((max, num) => num > max ? num : max);
console.log(max);  // 5

// Object from array
let fruits = ['apple', 'banana', 'orange'];
let obj = fruits.reduce((acc, fruit, index) => {
    acc[fruit] = index;
    return acc;
}, {});
console.log(obj);  // { apple: 0, banana: 1, orange: 2 }
```

### find() dan findIndex()

```javascript
let users = [
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' },
    { id: 3, name: 'Bob' }
];

// find: return first matching element
let user = users.find(u => u.id === 2);
console.log(user);  // { id: 2, name: 'Jane' }

// findIndex: return index of first match
let index = users.findIndex(u => u.id === 2);
console.log(index);  // 1
```

### some() dan every()

```javascript
let numbers = [1, 2, 3, 4, 5];

// some: at least one matches
let hasEven = numbers.some(num => num % 2 === 0);
console.log(hasEven);  // true

// every: all must match
let allPositive = numbers.every(num => num > 0);
console.log(allPositive);  // true

let allEven = numbers.every(num => num % 2 === 0);
console.log(allEven);  // false
```

---

## Spread Operator

```javascript
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];

// Copy array
let copy = [...arr1];

// Combine arrays
let combined = [...arr1, ...arr2];
console.log(combined);  // [1, 2, 3, 4, 5, 6]

// Add elements
let extended = [0, ...arr1, 4, ...arr2];
console.log(extended);  // [0, 1, 2, 3, 4, 4, 5, 6]

// Function arguments
function sum(a, b, c) {
    return a + b + c;
}
console.log(sum(...arr1));  // 6
```

---

## Destructuring

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];

// Extract elements
let [first, second, third] = fruits;
console.log(first);   // 'Apple'
console.log(second);  // 'Banana'

// Skip elements
let [, , third] = fruits;
console.log(third);  // 'Orange'

// Rest operator
let [first, ...rest] = fruits;
console.log(first);  // 'Apple'
console.log(rest);   // ['Banana', 'Orange']

// Default values
let [a, b, c, d = 'Default'] = fruits;
console.log(d);  // 'Default'
```

---

## Multi-Dimensional Arrays

```javascript
// 2D Array
let matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

console.log(matrix[0]);     // [1, 2, 3]
console.log(matrix[0][0]);  // 1
console.log(matrix[1][2]);  // 6

// Iterate
for (let row of matrix) {
    for (let num of row) {
        console.log(num);
    }
}

// 3D Array
let cube = [
    [[1, 2], [3, 4]],
    [[5, 6], [7, 8]]
];

console.log(cube[0][1][0]);  // 3
```

---

## Common Patterns

### Remove Duplicates

```javascript
let numbers = [1, 2, 2, 3, 3, 3, 4, 5, 5];

// Using Set
let unique = [...new Set(numbers)];
console.log(unique);  // [1, 2, 3, 4, 5]

// Manual
let unique2 = numbers.filter((num, index) => {
    return numbers.indexOf(num) === index;
});
```

### Flatten Array

```javascript
let nested = [1, [2, 3], [4, [5, 6]]];

// flat() - one level
let flat1 = nested.flat();
console.log(flat1);  // [1, 2, 3, 4, [5, 6]]

// flat(depth) - specific depth
let flat2 = nested.flat(2);
console.log(flat2);  // [1, 2, 3, 4, 5, 6]

// flat(Infinity) - all levels
let flat3 = nested.flat(Infinity);
console.log(flat3);  // [1, 2, 3, 4, 5, 6]
```

### Chunk Array

```javascript
function chunkArray(array, size) {
    let result = [];
    for (let i = 0; i < array.length; i += size) {
        result.push(array.slice(i, i + size));
    }
    return result;
}

let numbers = [1, 2, 3, 4, 5, 6, 7, 8];
let chunked = chunkArray(numbers, 3);
console.log(chunked);  // [[1, 2, 3], [4, 5, 6], [7, 8]]
```

### Shuffle Array

```javascript
function shuffleArray(array) {
    let shuffled = [...array];
    for (let i = shuffled.length - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
    }
    return shuffled;
}

let numbers = [1, 2, 3, 4, 5];
let shuffled = shuffleArray(numbers);
console.log(shuffled);  // Random order
```

### Get Random Element

```javascript
function getRandomElement(array) {
    return array[Math.floor(Math.random() * array.length)];
}

let fruits = ['Apple', 'Banana', 'Orange'];
console.log(getRandomElement(fruits));
```

---

## Array Conversion

### String to Array

```javascript
// split()
let str = 'Apple,Banana,Orange';
let fruits = str.split(',');
console.log(fruits);  // ['Apple', 'Banana', 'Orange']

let text = 'Hello World';
let chars = text.split('');
console.log(chars);  // ['H', 'e', 'l', 'l', 'o', ' ', 'W', ...]

// Array.from()
let str2 = 'Hello';
let chars2 = Array.from(str2);
console.log(chars2);  // ['H', 'e', 'l', 'l', 'o']
```

### Array to String

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];

// join()
let str1 = fruits.join(', ');
console.log(str1);  // 'Apple, Banana, Orange'

// toString()
let str2 = fruits.toString();
console.log(str2);  // 'Apple,Banana,Orange'
```

---

## Chaining Methods

```javascript
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

let result = numbers
    .filter(num => num % 2 === 0)  // [2, 4, 6, 8, 10]
    .map(num => num * 2)            // [4, 8, 12, 16, 20]
    .reduce((sum, num) => sum + num, 0);  // 60

console.log(result);  // 60
```

**Example: Process User Data**

```javascript
let users = [
    { name: 'John', age: 25, active: true },
    { name: 'Jane', age: 30, active: false },
    { name: 'Bob', age: 22, active: true },
    { name: 'Alice', age: 35, active: true }
];

let activeUserNames = users
    .filter(user => user.active)
    .filter(user => user.age >= 25)
    .map(user => user.name)
    .sort();

console.log(activeUserNames);  // ['Alice', 'John']
```

---

## Best Practices

### DO (Lakukan)

1. **Use array methods daripada loops**
   ```javascript
   // Good
   let doubled = numbers.map(n => n * 2);
   
   // Avoid
   let doubled = [];
   for (let n of numbers) {
       doubled.push(n * 2);
   }
   ```

2. **Use const untuk arrays yang tidak reassign**
   ```javascript
   const fruits = ['Apple', 'Banana'];
   fruits.push('Orange');  // OK
   ```

3. **Use spread untuk copy/merge**
   ```javascript
   let copy = [...original];
   let merged = [...arr1, ...arr2];
   ```

4. **Chain methods untuk transformations**
   ```javascript
   let result = data
       .filter(condition)
       .map(transform)
       .sort();
   ```

### DON'T (Jangan)

1. **Jangan modify array during iteration**
   ```javascript
   // Bad
   arr.forEach((item, index) => {
       arr.splice(index, 1);
   });
   ```

2. **Jangan gunakan for...in untuk arrays**
   ```javascript
   // Bad
   for (let index in array) {
       console.log(array[index]);
   }
   
   // Good
   for (let item of array) {
       console.log(item);
   }
   ```

3. **Jangan lupa return di map**
   ```javascript
   // Bad
   arr.map(item => {
       item * 2;  // No return!
   });
   
   // Good
   arr.map(item => item * 2);
   ```

---

## Rangkuman

- Array menyimpan multiple values
- Access dengan index (0-based)
- Adding: push(), unshift(), splice()
- Removing: pop(), shift(), splice()
- slice() untuk extract, concat() untuk merge
- Iteration: forEach(), map(), filter(), reduce()
- find(), some(), every() untuk searching
- sort() untuk sorting (use compare function untuk numbers)
- includes() untuk check membership
- Spread operator untuk copy/merge
- Destructuring untuk extract values
- Chain methods untuk data transformation
- Use array methods daripada manual loops

---

**Sebelumnya:** [07. Functions](07-functions.md)  
**Selanjutnya:** [09. Objects](09-objects.md)

