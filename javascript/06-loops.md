# 6. Loops

## for Loop

Loop dengan counter.

### Basic for Loop

```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
// Output: 0, 1, 2, 3, 4
```

**Anatomy:**
```javascript
for (initialization; condition; increment) {
    // code to execute
}
```

### Examples

```javascript
// Count up
for (let i = 1; i <= 10; i++) {
    console.log(i);
}

// Count down
for (let i = 10; i >= 1; i--) {
    console.log(i);
}

// Step by 2
for (let i = 0; i < 10; i += 2) {
    console.log(i);  // 0, 2, 4, 6, 8
}

// Multiple variables
for (let i = 0, j = 10; i < 5; i++, j--) {
    console.log(`i: ${i}, j: ${j}`);
}
```

### Loop Array

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];

for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}
```

---

## while Loop

Loop selama condition true.

### Basic while

```javascript
let i = 0;
while (i < 5) {
    console.log(i);
    i++;
}
// Output: 0, 1, 2, 3, 4
```

### Examples

```javascript
// Countdown
let count = 5;
while (count > 0) {
    console.log(count);
    count--;
}

// User input (conceptual)
let password = '';
while (password !== 'secret') {
    password = prompt('Enter password:');
}

// Process items
let items = ['a', 'b', 'c'];
let index = 0;
while (index < items.length) {
    console.log(items[index]);
    index++;
}
```

---

## do...while Loop

Execute at least once, then check condition.

### Basic do...while

```javascript
let i = 0;
do {
    console.log(i);
    i++;
} while (i < 5);
// Output: 0, 1, 2, 3, 4
```

### Perbedaan dengan while

```javascript
// while: might not execute at all
let x = 10;
while (x < 5) {
    console.log(x);  // Tidak dijalankan
}

// do...while: executes at least once
let y = 10;
do {
    console.log(y);  // Dijalankan sekali
} while (y < 5);
```

### Practical Example

```javascript
// Menu selection
let choice;
do {
    console.log('1. Start');
    console.log('2. Settings');
    console.log('3. Exit');
    choice = prompt('Choose option:');
} while (choice !== '3');
```

---

## for...of Loop

Iterate over iterable objects (arrays, strings, etc).

### Array Iteration

```javascript
let fruits = ['Apple', 'Banana', 'Orange'];

for (let fruit of fruits) {
    console.log(fruit);
}
// Output: Apple, Banana, Orange
```

### String Iteration

```javascript
let text = 'Hello';

for (let char of text) {
    console.log(char);
}
// Output: H, e, l, l, o
```

### With Index

```javascript
let colors = ['red', 'green', 'blue'];

for (let [index, color] of colors.entries()) {
    console.log(`${index}: ${color}`);
}
// Output:
// 0: red
// 1: green
// 2: blue
```

---

## for...in Loop

Iterate over object properties.

### Object Properties

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

### Array (Not Recommended)

```javascript
let arr = ['a', 'b', 'c'];

// Works but not recommended
for (let index in arr) {
    console.log(arr[index]);
}

// Better: use for...of
for (let item of arr) {
    console.log(item);
}
```

---

## break Statement

Exit loop immediately.

```javascript
// Find first even number
for (let i = 1; i <= 10; i++) {
    if (i % 2 === 0) {
        console.log(`First even: ${i}`);
        break;
    }
}

// Search in array
let numbers = [1, 3, 5, 7, 8, 9];
let found = false;

for (let num of numbers) {
    if (num % 2 === 0) {
        console.log(`Found even number: ${num}`);
        found = true;
        break;
    }
}
```

---

## continue Statement

Skip current iteration.

```javascript
// Print odd numbers only
for (let i = 1; i <= 10; i++) {
    if (i % 2 === 0) {
        continue;  // Skip even numbers
    }
    console.log(i);
}
// Output: 1, 3, 5, 7, 9

// Skip empty strings
let items = ['apple', '', 'banana', '', 'orange'];

for (let item of items) {
    if (!item) {
        continue;
    }
    console.log(item);
}
// Output: apple, banana, orange
```

---

## Nested Loops

Loop di dalam loop.

### 2D Array / Matrix

```javascript
let matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];

for (let i = 0; i < matrix.length; i++) {
    for (let j = 0; j < matrix[i].length; j++) {
        console.log(matrix[i][j]);
    }
}
```

### Multiplication Table

```javascript
for (let i = 1; i <= 5; i++) {
    let row = '';
    for (let j = 1; j <= 5; j++) {
        row += (i * j) + '\t';
    }
    console.log(row);
}
```

### Pattern Printing

```javascript
// Triangle pattern
for (let i = 1; i <= 5; i++) {
    let stars = '';
    for (let j = 1; j <= i; j++) {
        stars += '*';
    }
    console.log(stars);
}
// Output:
// *
// **
// ***
// ****
// *****
```

---

## Array Methods (Alternative to Loops)

### forEach

```javascript
let numbers = [1, 2, 3, 4, 5];

numbers.forEach(function(num) {
    console.log(num);
});

// Arrow function
numbers.forEach(num => console.log(num));

// With index
numbers.forEach((num, index) => {
    console.log(`${index}: ${num}`);
});
```

### map

Transform array elements.

```javascript
let numbers = [1, 2, 3, 4, 5];

let doubled = numbers.map(num => num * 2);
console.log(doubled);  // [2, 4, 6, 8, 10]

let squared = numbers.map(num => num ** 2);
console.log(squared);  // [1, 4, 9, 16, 25]
```

### filter

Filter array elements.

```javascript
let numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

let evens = numbers.filter(num => num % 2 === 0);
console.log(evens);  // [2, 4, 6, 8, 10]

let greaterThan5 = numbers.filter(num => num > 5);
console.log(greaterThan5);  // [6, 7, 8, 9, 10]
```

### reduce

Reduce array to single value.

```javascript
let numbers = [1, 2, 3, 4, 5];

let sum = numbers.reduce((total, num) => total + num, 0);
console.log(sum);  // 15

let product = numbers.reduce((total, num) => total * num, 1);
console.log(product);  // 120
```

### find

Find first matching element.

```javascript
let users = [
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' },
    { id: 3, name: 'Bob' }
];

let user = users.find(u => u.id === 2);
console.log(user);  // { id: 2, name: 'Jane' }
```

### some dan every

```javascript
let numbers = [1, 2, 3, 4, 5];

// some: at least one matches
let hasEven = numbers.some(num => num % 2 === 0);
console.log(hasEven);  // true

// every: all match
let allPositive = numbers.every(num => num > 0);
console.log(allPositive);  // true
```

---

## Performance Considerations

### Cache Array Length

```javascript
// Slow: length calculated every iteration
for (let i = 0; i < array.length; i++) {
    // ...
}

// Fast: length cached
let len = array.length;
for (let i = 0; i < len; i++) {
    // ...
}
```

### Choose Right Loop

```javascript
// Simple iteration: for...of
for (let item of array) {
    console.log(item);
}

// Need index: forEach
array.forEach((item, index) => {
    console.log(index, item);
});

// Transform: map
let doubled = array.map(n => n * 2);

// Filter: filter
let evens = array.filter(n => n % 2 === 0);
```

---

## Common Patterns

### Sum Array

```javascript
let numbers = [1, 2, 3, 4, 5];
let sum = 0;

for (let num of numbers) {
    sum += num;
}

// Or with reduce
let sum2 = numbers.reduce((total, num) => total + num, 0);
```

### Find Max/Min

```javascript
let numbers = [5, 2, 9, 1, 7];

// Manual
let max = numbers[0];
for (let num of numbers) {
    if (num > max) {
        max = num;
    }
}

// Built-in
let max2 = Math.max(...numbers);
let min = Math.min(...numbers);
```

### Reverse Array

```javascript
let arr = [1, 2, 3, 4, 5];

// Manual
let reversed = [];
for (let i = arr.length - 1; i >= 0; i--) {
    reversed.push(arr[i]);
}

// Built-in
let reversed2 = arr.reverse();
```

### Count Occurrences

```javascript
let fruits = ['apple', 'banana', 'apple', 'orange', 'banana', 'apple'];

let count = {};
for (let fruit of fruits) {
    count[fruit] = (count[fruit] || 0) + 1;
}

console.log(count);
// { apple: 3, banana: 2, orange: 1 }
```

### Remove Duplicates

```javascript
let numbers = [1, 2, 2, 3, 3, 3, 4, 5, 5];

// Using Set
let unique = [...new Set(numbers)];

// Manual
let unique2 = [];
for (let num of numbers) {
    if (!unique2.includes(num)) {
        unique2.push(num);
    }
}
```

---

## Best Practices

### DO (Lakukan)

1. **Use for...of untuk simple iteration**
   ```javascript
   for (let item of array) {
       console.log(item);
   }
   ```

2. **Use array methods ketika appropriate**
   ```javascript
   let doubled = numbers.map(n => n * 2);
   let evens = numbers.filter(n => n % 2 === 0);
   ```

3. **Use descriptive variable names**
   ```javascript
   for (let user of users) {
       console.log(user.name);
   }
   ```

4. **Break early when possible**
   ```javascript
   for (let item of items) {
       if (item.found) {
           break;
       }
   }
   ```

### DON'T (Jangan)

1. **Jangan modify array during iteration**
   ```javascript
   // Bad
   for (let i = 0; i < arr.length; i++) {
       arr.splice(i, 1);  // Problem!
   }
   ```

2. **Jangan nested loops terlalu dalam**
   ```javascript
   // Avoid if possible
   for (...) {
       for (...) {
           for (...) {
               // too nested
           }
       }
   }
   ```

3. **Jangan infinite loops**
   ```javascript
   // Bad: infinite loop
   while (true) {
       // no break condition
   }
   ```

---

## Rangkuman

- `for` loop untuk counted iterations
- `while` loop untuk condition-based iterations
- `do...while` untuk at-least-once execution
- `for...of` untuk iterate arrays/iterables
- `for...in` untuk iterate object properties
- `break` untuk exit loop
- `continue` untuk skip iteration
- Array methods (forEach, map, filter) sering lebih clean
- Choose right loop for the task
- Avoid infinite loops dan nested loops yang terlalu dalam

---

**Sebelumnya:** [05. Control Flow](05-control-flow.md)  
**Selanjutnya:** [07. Functions](07-functions.md)

