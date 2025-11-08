# Materi JavaScript untuk Bun

Selamat datang di materi pembelajaran JavaScript untuk Bun! Materi ini disusun secara berurutan untuk memudahkan Anda mempelajari JavaScript dan Bun dari nol hingga mahir.

## Apa itu Bun?

Bun adalah JavaScript runtime modern yang sangat cepat, 3-4x lebih cepat dari Node.js. Bun adalah all-in-one tool yang mencakup:

- **Runtime** - Menjalankan JavaScript/TypeScript
- **Package Manager** - Faster than npm/yarn
- **Bundler** - Bundle code untuk production
- **Test Runner** - Built-in testing framework  
- **Transpiler** - TypeScript dan JSX support native

---

## Daftar Materi

### 1. [Pengenalan JavaScript](01-pengenalan-javascript.md)
- Apa itu JavaScript?
- Karakteristik JavaScript
- Kegunaan JavaScript dengan Bun
- Bun vs Node.js
- JavaScript Engine dan ECMAScript
- Console dan debugging
- Hello World dengan Bun
- Sintaks dasar dan naming conventions
- ES6 Modules

### 2. [Setup Environment Bun](02-setup-environment.md)
- Instalasi Bun (Windows, macOS, Linux)
- Setup Visual Studio Code
- Extensions yang berguna
- Project structure untuk Bun
- Running JavaScript dengan Bun
- Bun CLI commands
- Package management dengan Bun
- Testing dengan Bun test runner
- Environment variables
- Git untuk version control

### 3. [Variabel dan Tipe Data](03-variabel-tipe-data.md)
- Variabel (var, let, const)
- Naming rules dan conventions
- Tipe data primitif (String, Number, Boolean, Undefined, Null, Symbol, BigInt)
- Tipe data non-primitif (Object, Array, Function)
- typeof operator
- Type conversion (explicit dan implicit)
- Template literals
- Destructuring (array dan object)

### 4. [Operators](04-operators.md)
- Arithmetic operators (+, -, *, /, %, **)
- Assignment operators (=, +=, -=, dll)
- Comparison operators (===, !==, >, <, >=, <=)
- Logical operators (&&, ||, !)
- Ternary operator (? :)
- Nullish coalescing (??)
- Optional chaining (?.)
- Spread operator (...)
- Rest operator
- Operator precedence

### 5. [Control Flow](05-control-flow.md)
- if statement
- if...else
- if...else if...else
- Nested if
- Truthy dan falsy values
- switch statement
- Ternary operator
- Logical operators untuk control flow
- Guard clauses
- Short-circuit evaluation

### 6. [Loops](06-loops.md)
- for loop
- while loop
- do...while loop
- for...of loop
- for...in loop
- break statement
- continue statement
- Nested loops
- Array methods (forEach, map, filter, reduce)
- find(), some(), every()

### 7. [Functions](07-functions.md)
- Function declaration
- Function expression
- Arrow functions
- Parameters dan return values
- Default parameters
- Rest parameters
- Function scope
- Closures
- Callback functions
- Higher-order functions
- IIFE (Immediately Invoked Function Expression)
- Recursive functions

### 8. [Arrays](08-arrays.md)
- Membuat array
- Accessing elements
- Array properties (length)
- Adding elements (push, unshift, splice)
- Removing elements (pop, shift, splice)
- Array methods (slice, concat, join, reverse, sort)
- Iteration methods (forEach, map, filter, reduce)
- find(), some(), every()
- includes(), indexOf()
- Spread operator
- Destructuring
- Multi-dimensional arrays
- Method chaining

### 9. [Objects](09-objects.md)
- Membuat object
- Accessing properties (dot dan bracket notation)
- Adding dan modifying properties
- Deleting properties
- Methods
- this keyword
- Checking properties
- Iterating objects (for...in, Object.keys(), values(), entries())
- Object methods (assign, freeze, seal)
- Destructuring
- Shorthand properties
- Getters dan setters
- JSON (stringify, parse)
- Object patterns
- Classes dan inheritance

### 10. [File System dengan Bun](10-file-system.md)
- Bun.file() API
- Reading files (.text(), .json(), .arrayBuffer())
- Writing files (Bun.write())
- File properties
- File existence check
- Streaming files
- File paths (import.meta.dir)
- Working with different file types
- Common file operations (copy, append, read CSV)

### 11. [HTTP Server dengan Bun](11-http-server.md)
- Bun.serve() untuk create HTTP server
- Handling requests dan responses
- URL parsing
- Response types (text, JSON, HTML, file)
- Routing (simple routing, method-based)
- Request body (JSON, FormData, text)
- Headers dan CORS
- Static file server
- REST API example (CRUD)
- WebSocket server
- Middleware pattern
- Environment-based configuration

---

## Cara Menggunakan Materi Ini

### Untuk Pemula:
1. **Install Bun** terlebih dahulu
2. **Baca berurutan** dari materi 1 hingga 11
3. **Practice** setiap kode yang diajarkan
4. **Run dengan Bun** untuk test
5. **Buat project kecil** untuk latihan

### Untuk Review:
- Gunakan sebagai **reference** saat coding
- **Bookmark** materi yang sering digunakan
- **Practice** examples yang diberikan

---

## Quick Start dengan Bun

### Install Bun

```bash
# macOS/Linux
curl -fsSL https://bun.sh/install | bash

# Windows
powershell -c "irm bun.sh/install.ps1 | iex"

# Verify
bun --version
```

### Create First Project

```bash
# Initialize project
mkdir my-bun-app
cd my-bun-app
bun init -y

# Create index.js
echo 'console.log("Hello Bun!")' > index.js

# Run
bun index.js
```

### Install Package

```bash
# Add package
bun add express

# Add dev dependency
bun add --dev @types/express
```

---

## Code Snippets Penting

```javascript
// Variables
const name = 'John';
let age = 30;

// Function
function greet(name) {
    return `Hello ${name}`;
}

// Arrow function
const add = (a, b) => a + b;

// Array methods
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);
const evens = numbers.filter(n => n % 2 === 0);
const sum = numbers.reduce((total, n) => total + n, 0);

// Object
const person = {
    name: 'John',
    age: 30,
    greet() {
        return `Hello, I'm ${this.name}`;
    }
};

// Destructuring
const { name, age } = person;
const [first, second] = numbers;

// Spread
const newArray = [...numbers, 6, 7];
const newObject = { ...person, city: 'Jakarta' };

// Async/Await
async function fetchData(url) {
    const response = await fetch(url);
    const data = await response.json();
    return data;
}

// File operations dengan Bun
const file = Bun.file("data.txt");
const content = await file.text();
await Bun.write("output.txt", "Hello Bun!");

// HTTP Server dengan Bun
Bun.serve({
    port: 3000,
    fetch(req) {
        return new Response("Hello from Bun!");
    },
});

// Read environment variables
const PORT = process.env.PORT || 3000;
```

---

## Resources Tambahan

### Bun Documentation
- [Official Bun Docs](https://bun.sh/docs)
- [Bun GitHub](https://github.com/oven-sh/bun)
- [Bun Examples](https://github.com/oven-sh/bun/tree/main/examples)

### JavaScript Documentation
- [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [JavaScript.info](https://javascript.info/)
- [W3Schools](https://www.w3schools.com/js/)

### Practice Platforms
- [FreeCodeCamp](https://www.freecodecamp.org/)
- [LeetCode](https://leetcode.com/)
- [HackerRank](https://www.hackerrank.com/domains/javascript)
- [Codewars](https://www.codewars.com/)

### Communities
- [Bun Discord](https://bun.sh/discord)
- Stack Overflow
- Dev.to
- Reddit r/javascript

---

## Project Ideas untuk Practice

### Beginner Level
1. **Calculator CLI**
   - Command-line calculator
   - Basic operations
   - Input validation

2. **Todo CLI**
   - Add, list, complete, delete tasks
   - Save to JSON file
   - Read from file on startup

3. **File Manager**
   - List files in directory
   - Create files
   - Read file contents

### Intermediate Level
1. **REST API**
   - CRUD operations
   - JSON responses
   - Error handling

2. **File-based Database**
   - Store data in JSON
   - CRUD operations
   - Search and filter

3. **Web Scraper**
   - Fetch HTML from URLs
   - Parse data
   - Save to file

### Advanced Beginner
1. **Real-time Chat Server**
   - WebSocket implementation
   - Multiple clients
   - Message broadcasting

2. **API with Database**
   - Connect to PostgreSQL/MongoDB
   - CRUD endpoints
   - Authentication

3. **CLI Tool**
   - Command-line interface
   - Arguments parsing
   - File operations

---

## Bun-Specific Features

### Why Bun?

**Speed**
- 3-4x faster than Node.js
- Instant startup time
- Fast package installs

**All-in-One**
- No need webpack/babel/jest
- Everything built-in
- Less configuration

**TypeScript Native**
- Run .ts files directly
- No compilation step
- Better DX

**Modern APIs**
- Web-standard APIs (fetch, Response, Request)
- Built-in .env support
- Native SQLite coming soon

---

## Best Practices Summary

### Code Quality
- Use meaningful variable names
- Follow camelCase convention
- Use const by default, let when needed
- Never use var
- Write small, focused functions
- Comment complex logic

### Bun-Specific
- Use ES6 modules (default di Bun)
- Leverage Bun's built-in APIs
- Use TypeScript untuk better type safety
- Write tests dengan bun test
- Use --watch untuk development
- Use .env untuk configuration

### Performance
- Bun is already fast by default
- Use Bun.file() untuk file operations
- Use Bun.serve() untuk HTTP server
- Leverage streaming untuk large files

### Security
- Validate user input
- Use environment variables untuk secrets
- Never commit .env files
- Sanitize data before use

---

## Next Steps

Setelah menguasai materi dasar ini, lanjut ke:

### Advanced JavaScript
- Asynchronous JavaScript (Promises, Async/Await)
- Error handling (try/catch)
- Advanced Modules
- Regular Expressions
- Memory management

### Bun Advanced
- Bun.build() untuk bundling
- Custom transpilers
- Plugins
- Performance optimization
- Deployment strategies

### Frameworks dan Libraries
- Elysia.js (Bun-native framework)
- Hono (lightweight framework)
- Database libraries (Prisma, Drizzle)
- TypeScript dengan Bun

---

## Progress Tracking

- [ ] 1. Pengenalan JavaScript
- [ ] 2. Setup Environment Bun
- [ ] 3. Variabel dan Tipe Data
- [ ] 4. Operators
- [ ] 5. Control Flow
- [ ] 6. Loops
- [ ] 7. Functions
- [ ] 8. Arrays
- [ ] 9. Objects
- [ ] 10. File System dengan Bun
- [ ] 11. HTTP Server dengan Bun

---

## Butuh Bantuan?

Jika menemui kesulitan:

1. **Baca ulang materi** yang relevan
2. **Test di terminal** dengan `bun file.js`
3. **Check Bun docs** di https://bun.sh/docs
4. **Debug dengan console.log()**
5. **Join Bun Discord** untuk community help

---

## Selamat Belajar!

JavaScript dengan Bun adalah kombinasi powerful untuk backend development. Dengan menguasai JavaScript dan Bun, Anda dapat:

- Build REST APIs yang sangat cepat
- Create CLI tools
- Develop microservices
- Build real-time applications
- Full-stack development
- Berkarir sebagai Backend Developer

**Happy Coding!**

---

*Materi ini dibuat untuk membantu Anda belajar JavaScript dengan Bun dari dasar hingga mahir.*
