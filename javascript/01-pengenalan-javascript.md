# 1. Pengenalan JavaScript

## Apa itu JavaScript?

JavaScript adalah bahasa pemrograman yang digunakan untuk membuat website interaktif dan dinamis. Dibuat oleh Brendan Eich di Netscape tahun 1995, JavaScript kini menjadi salah satu bahasa pemrograman paling populer di dunia.

## Karakteristik JavaScript

### High-Level Language
JavaScript adalah bahasa tingkat tinggi yang abstrak dari detail hardware, membuat lebih mudah dipelajari.

### Interpreted Language
JavaScript dijalankan line-by-line oleh JavaScript engine (seperti V8 di Chrome), tidak perlu kompilasi.

### Dynamically Typed
Tipe data variabel ditentukan saat runtime, bukan saat deklarasi.

```javascript
let x = 5;        // x adalah number
x = "hello";      // sekarang x adalah string (valid!)
```

### Multi-Paradigm
Mendukung berbagai paradigma pemrograman:
- Procedural
- Object-Oriented (OOP)
- Functional Programming

### Single-Threaded
JavaScript menjalankan kode satu operasi pada satu waktu, tapi punya mekanisme asynchronous.

## Kegunaan JavaScript dengan Node.js

### 1. Back-End Development
Build server-side applications:
- REST API dan GraphQL
- Microservices
- Authentication dan Authorization
- Server-side rendering

### 2. Database Operations
Interact dengan berbagai database:
- MongoDB dengan Mongoose
- PostgreSQL dengan pg
- MySQL dengan mysql2
- Redis untuk caching

### 3. File System Operations
Manipulasi files dan directories:
- Read/write files
- File uploads
- Directory management
- Stream processing

### 4. Real-Time Applications
Build real-time features:
- Chat applications
- Live notifications
- WebSocket servers
- Collaboration tools

### 5. CLI Tools
Create command-line tools:
- Build tools
- Automation scripts
- Development utilities

### 6. Desktop Applications
Dengan Electron:
- VS Code
- Slack
- Discord
- Postman

## JavaScript vs Java

Meskipun namanya mirip, JavaScript dan Java adalah bahasa yang berbeda:

| Aspek | JavaScript | Java |
|-------|-----------|------|
| **Tipe** | Interpreted | Compiled |
| **Typing** | Dynamic | Static |
| **Platform** | Browser & Server | JVM |
| **Sintaks** | Lebih fleksibel | Lebih strict |
| **Use Case** | Web development | Enterprise apps, Android |

## JavaScript Engine

Browser modern memiliki JavaScript engine:

- **V8** - Chrome, Edge, Node.js
- **SpiderMonkey** - Firefox
- **JavaScriptCore** - Safari

## ECMAScript (ES)

ECMAScript adalah standar spesifikasi JavaScript. Versi penting:

- **ES5 (2009)** - Strict mode, JSON support
- **ES6/ES2015** - Arrow functions, Classes, Promises, let/const
- **ES2016+** - Async/await, Spread operator, dan fitur modern lainnya

## Dimana JavaScript Dijalankan?

### Browser (Client-Side)
```html
<!DOCTYPE html>
<html>
<head>
    <title>JavaScript Example</title>
</head>
<body>
    <h1>Hello World</h1>
    
    <!-- Inline JavaScript -->
    <button onclick="alert('Hello!')">Click Me</button>
    
    <!-- Internal JavaScript -->
    <script>
        console.log('JavaScript running in browser');
    </script>
    
    <!-- External JavaScript -->
    <script src="script.js"></script>
</body>
</html>
```

### Node.js (Server-Side)
```javascript
// server.js
const http = require('http');

const server = http.createServer((req, res) => {
    res.end('Hello from Node.js');
});

server.listen(3000);
```

## Console di Bun

Bun menyediakan console object yang sama untuk logging dan debugging.

### Console Methods

```javascript
// Output ke console
console.log('Hello World');

// Warning
console.warn('This is a warning');

// Error
console.error('This is an error');

// Info
console.info('Information message');

// Table
console.table({name: 'John', age: 30});

// Clear console
console.clear();
```

## Hello World Program di Bun

### Basic Script

```javascript
// hello.js
console.log('Hello World from Bun!');

// Run with: bun hello.js
// atau: bun run hello.js
```

### Multiple Outputs

```javascript
// app.js
console.log('Hello World');
console.log('Bun version:', Bun.version);
console.log('Current file:', import.meta.url);

// Run with: bun app.js
```

### Simple Calculation

```javascript
// calc.js
const num1 = 10;
const num2 = 5;

console.log(`Sum: ${num1 + num2}`);
console.log(`Product: ${num1 * num2}`);
console.log(`Division: ${num1 / num2}`);

// Run with: bun calc.js
```

## Cara Menulis Kode JavaScript untuk Bun

### Single File

```javascript
// app.js
function greet(name) {
    return `Hello ${name}!`;
}

const message = greet('World');
console.log(message);

// Run: bun app.js
```

### Multiple Files dengan ES6 Modules (Default di Bun)

Bun support ES6 modules secara native tanpa konfigurasi.

```javascript
// utils.js
export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}

export default function calculate(a, b, operation) {
    if (operation === 'add') return add(a, b);
    if (operation === 'multiply') return multiply(a, b);
}
```

```javascript
// app.js
import { add, multiply } from './utils.js';
import calculate from './utils.js';

console.log(add(5, 3));           // 8
console.log(multiply(5, 3));      // 15
console.log(calculate(5, 3, 'add')); // 8

// Run: bun app.js
```

### CommonJS (Also Supported)

```javascript
// utils.js
function add(a, b) {
    return a + b;
}

module.exports = { add };
```

```javascript
// app.js
const { add } = require('./utils.js');

console.log(add(5, 3));

// Run: bun app.js
```

## Sintaks Dasar

### Statements
Instruksi yang dieksekusi oleh browser/Node.js:

```javascript
let x = 5;
console.log(x);
```

### Semicolons
Opsional tapi recommended:

```javascript
// Dengan semicolon (recommended)
let name = 'John';
console.log(name);

// Tanpa semicolon (valid tapi tidak recommended)
let name = 'John'
console.log(name)
```

### Case Sensitivity
JavaScript case-sensitive:

```javascript
let name = 'John';
let Name = 'Jane';  // Variabel berbeda!

console.log(name);  // 'John'
console.log(Name);  // 'Jane'
```

### Comments

```javascript
// Single line comment

/*
Multi-line
comment
*/

/**
 * Documentation comment
 * @param {string} name
 */
function greet(name) {
    return `Hello ${name}`;
}
```

### Whitespace
JavaScript mengabaikan extra whitespace:

```javascript
// Sama saja
let x=5+10;
let x = 5 + 10;
let x  =  5  +  10;
```

## Naming Conventions

### Variables dan Functions
camelCase:

```javascript
let firstName = 'John';
let userAge = 25;

function calculateTotal() {
    // ...
}
```

### Constants
UPPER_SNAKE_CASE:

```javascript
const MAX_SIZE = 100;
const API_KEY = 'abc123';
```

### Classes
PascalCase:

```javascript
class UserProfile {
    // ...
}

class ShoppingCart {
    // ...
}
```

### Private Properties (Convention)
Dimulai dengan underscore:

```javascript
let _privateVar = 10;

function _privateFunction() {
    // ...
}
```

## Best Practices

### DO (Lakukan)

1. **Gunakan 'use strict'**
   ```javascript
   'use strict';
   ```

2. **Declare variabel sebelum digunakan**
   ```javascript
   let name = 'John';
   console.log(name);
   ```

3. **Gunakan const untuk nilai yang tidak berubah**
   ```javascript
   const PI = 3.14159;
   ```

4. **Indentasi konsisten (2 atau 4 spaces)**
   ```javascript
   function example() {
       if (true) {
           console.log('Indented');
       }
   }
   ```

5. **Comment kode yang kompleks**
   ```javascript
   // Calculate total with tax
   const total = price * quantity * (1 + taxRate);
   ```

### DON'T (Jangan)

1. Jangan gunakan var (gunakan let/const)
2. Jangan lupa semicolon
3. Jangan gunakan global variables sembarangan
4. Jangan buat kode nested terlalu dalam
5. Jangan hardcode values (gunakan constants)

## Fitur Modern JavaScript (ES6+)

### Let dan Const
```javascript
let variable = 'can change';
const constant = 'cannot change';
```

### Arrow Functions
```javascript
const greet = (name) => `Hello ${name}`;
```

### Template Literals
```javascript
let name = 'John';
let message = `Hello ${name}!`;
```

### Destructuring
```javascript
const person = {name: 'John', age: 30};
const {name, age} = person;
```

### Spread Operator
```javascript
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5];
```

### Promises dan Async/Await
```javascript
async function fetchData() {
    const response = await fetch(url);
    const data = await response.json();
    return data;
}
```

## Tools untuk Bun Development

### Code Editors
- Visual Studio Code (recommended)
- Sublime Text
- WebStorm
- Vim/Neovim

### Bun Tools
- Bun runtime (all-in-one)
- Bun package manager (built-in)
- Bun test runner (built-in)
- Bun bundler (built-in)

### Development Tools
- ESLint (linter)
- Prettier (formatter)
- Bun built-in test runner
- PM2 atau bun untuk process management

### Debugging
- Bun built-in debugger
- VS Code debugger
- console.log debugging

## Resources untuk Belajar

### Documentation
- MDN Web Docs (mozilla.org)
- JavaScript.info
- W3Schools

### Practice Platforms
- FreeCodeCamp
- Codecademy
- LeetCode
- HackerRank

## Rangkuman

- JavaScript adalah bahasa pemrograman yang versatile
- Bun adalah runtime modern yang sangat cepat (3-4x Node.js)
- Bun all-in-one: runtime, bundler, transpiler, package manager, test runner
- Dynamically typed dan multi-paradigm
- Gunakan console untuk testing dan debugging
- Bun support ES6 modules native tanpa konfigurasi
- Bun support TypeScript dan JSX out-of-the-box
- Follow naming conventions dan best practices
- Practice adalah kunci untuk menguasai JavaScript

---

**Selanjutnya:** [02. Setup Environment Bun](02-setup-environment.md)

