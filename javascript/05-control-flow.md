# 5. Control Flow

## if Statement

Menjalankan code jika condition true.

### Basic if

```javascript
let age = 18;

if (age >= 18) {
    console.log('You are an adult');
}
```

### if...else

```javascript
let age = 16;

if (age >= 18) {
    console.log('You are an adult');
} else {
    console.log('You are a minor');
}
```

### if...else if...else

```javascript
let score = 85;

if (score >= 90) {
    console.log('Grade: A');
} else if (score >= 80) {
    console.log('Grade: B');
} else if (score >= 70) {
    console.log('Grade: C');
} else if (score >= 60) {
    console.log('Grade: D');
} else {
    console.log('Grade: F');
}
```

### Nested if

```javascript
let age = 25;
let hasLicense = true;

if (age >= 18) {
    if (hasLicense) {
        console.log('You can drive');
    } else {
        console.log('You need a license');
    }
} else {
    console.log('You are too young to drive');
}
```

---

## Truthy dan Falsy Values

### Falsy Values

Values yang dianggap `false` dalam boolean context:

```javascript
if (false)     { /* tidak dijalankan */ }
if (0)         { /* tidak dijalankan */ }
if ('')        { /* tidak dijalankan */ }
if (null)      { /* tidak dijalankan */ }
if (undefined) { /* tidak dijalankan */ }
if (NaN)       { /* tidak dijalankan */ }
```

### Truthy Values

Semua values selain falsy values:

```javascript
if (true)      { console.log('Truthy'); }
if (1)         { console.log('Truthy'); }
if ('hello')   { console.log('Truthy'); }
if ([])        { console.log('Truthy'); }  // Empty array
if ({})        { console.log('Truthy'); }  // Empty object
if (function(){}) { console.log('Truthy'); }
```

### Practical Examples

```javascript
// Check if variable has value
let userName = '';

if (userName) {
    console.log(`Hello ${userName}`);
} else {
    console.log('Hello Guest');
}

// Check if array has items
let items = [];

if (items.length) {
    console.log('Cart has items');
} else {
    console.log('Cart is empty');
}
```

---

## switch Statement

Alternative untuk multiple if-else conditions.

### Basic switch

```javascript
let day = 'Monday';

switch (day) {
    case 'Monday':
        console.log('Start of work week');
        break;
    case 'Friday':
        console.log('End of work week');
        break;
    case 'Saturday':
    case 'Sunday':
        console.log('Weekend!');
        break;
    default:
        console.log('Midweek day');
}
```

### switch dengan Numbers

```javascript
let grade = 'B';

switch (grade) {
    case 'A':
        console.log('Excellent!');
        break;
    case 'B':
        console.log('Good job!');
        break;
    case 'C':
        console.log('You passed');
        break;
    case 'D':
    case 'F':
        console.log('You failed');
        break;
    default:
        console.log('Invalid grade');
}
```

### switch Tanpa break (Fall-through)

```javascript
let month = 2;
let quarter;

switch (month) {
    case 1:
    case 2:
    case 3:
        quarter = 'Q1';
        break;
    case 4:
    case 5:
    case 6:
        quarter = 'Q2';
        break;
    case 7:
    case 8:
    case 9:
        quarter = 'Q3';
        break;
    case 10:
    case 11:
    case 12:
        quarter = 'Q4';
        break;
    default:
        quarter = 'Invalid month';
}

console.log(quarter);  // 'Q1'
```

---

## Ternary Operator

Shorthand untuk simple if-else.

### Basic Ternary

```javascript
let age = 20;
let status = age >= 18 ? 'Adult' : 'Minor';
console.log(status);  // 'Adult'
```

### Multiple Ternaries

```javascript
let score = 85;
let grade = score >= 90 ? 'A' :
            score >= 80 ? 'B' :
            score >= 70 ? 'C' :
            score >= 60 ? 'D' : 'F';

console.log(grade);  // 'B'
```

### Ternary dengan Function Calls

```javascript
let isLoggedIn = true;
let message = isLoggedIn ? getWelcomeMessage() : getLoginPrompt();

function getWelcomeMessage() {
    return 'Welcome back!';
}

function getLoginPrompt() {
    return 'Please log in';
}
```

---

## Logical Operators untuk Control Flow

### AND (&&) untuk Conditional Execution

```javascript
let isLoggedIn = true;
let userName = 'John';

// Only execute if condition true
isLoggedIn && console.log(`Welcome ${userName}`);

// Multiple conditions
let hasPermission = true;
let isActive = true;
hasPermission && isActive && console.log('Access granted');
```

### OR (||) untuk Default Values

```javascript
let userName = '';
let displayName = userName || 'Guest';
console.log(displayName);  // 'Guest'

// Function parameters
function greet(name) {
    name = name || 'Guest';
    console.log(`Hello ${name}`);
}

greet();          // 'Hello Guest'
greet('John');    // 'Hello John'
```

### Nullish Coalescing (??) untuk Precise Defaults

```javascript
let count = 0;

// Dengan ||, 0 dianggap falsy
let display1 = count || 10;
console.log(display1);  // 10 (salah!)

// Dengan ??, hanya check null/undefined
let display2 = count ?? 10;
console.log(display2);  // 0 (benar!)

// Use case
function setDefaults(options) {
    return {
        timeout: options.timeout ?? 5000,
        retry: options.retry ?? 3,
        cache: options.cache ?? true
    };
}
```

---

## Guard Clauses

Early return pattern untuk cleaner code.

### Without Guard Clauses

```javascript
function processOrder(order) {
    if (order) {
        if (order.items.length > 0) {
            if (order.total > 0) {
                // Process order
                console.log('Processing order');
                return true;
            } else {
                console.log('Invalid total');
                return false;
            }
        } else {
            console.log('No items in order');
            return false;
        }
    } else {
        console.log('No order');
        return false;
    }
}
```

### With Guard Clauses (Better)

```javascript
function processOrder(order) {
    // Guard clauses
    if (!order) {
        console.log('No order');
        return false;
    }
    
    if (order.items.length === 0) {
        console.log('No items in order');
        return false;
    }
    
    if (order.total <= 0) {
        console.log('Invalid total');
        return false;
    }
    
    // Main logic
    console.log('Processing order');
    return true;
}
```

---

## Short-Circuit Evaluation

### AND (&&) Short-Circuit

```javascript
// Right side tidak di-evaluate jika left false
let result = false && expensiveOperation();
// expensiveOperation() tidak dijalankan

// Practical example
let user = getUser();
let userName = user && user.name;  // Safe access

// Modern alternative: optional chaining
let userName2 = user?.name;
```

### OR (||) Short-Circuit

```javascript
// Right side tidak di-evaluate jika left true
let result = true || expensiveOperation();
// expensiveOperation() tidak dijalankan

// Practical example: cache result
let cachedData = cache || fetchFromAPI();
```

---

## Common Patterns

### Range Checking

```javascript
let age = 25;

// Check if in range
if (age >= 18 && age <= 65) {
    console.log('Working age');
}

// Alternative
if (age >= 18) {
    if (age <= 65) {
        console.log('Working age');
    }
}
```

### Multiple Conditions for Same Action

```javascript
// Using OR
let day = 'Saturday';
if (day === 'Saturday' || day === 'Sunday') {
    console.log('Weekend');
}

// Using array (better for many values)
if (['Saturday', 'Sunday'].includes(day)) {
    console.log('Weekend');
}

// Using switch
switch (day) {
    case 'Saturday':
    case 'Sunday':
        console.log('Weekend');
        break;
}
```

### Type Checking before Operation

```javascript
function divide(a, b) {
    // Type check
    if (typeof a !== 'number' || typeof b !== 'number') {
        return 'Error: Both arguments must be numbers';
    }
    
    // Zero check
    if (b === 0) {
        return 'Error: Cannot divide by zero';
    }
    
    return a / b;
}
```

### Default Object Properties

```javascript
function createUser(options) {
    // Set defaults
    const defaults = {
        role: 'user',
        active: true,
        notifications: true
    };
    
    // Merge with defaults
    return { ...defaults, ...options };
}

let user1 = createUser({ name: 'John' });
// { name: 'John', role: 'user', active: true, notifications: true }

let user2 = createUser({ name: 'Jane', role: 'admin' });
// { name: 'Jane', role: 'admin', active: true, notifications: true }
```

---

## Best Practices

### DO (Lakukan)

1. **Use === for comparisons**
   ```javascript
   if (value === 10) {
       // ...
   }
   ```

2. **Use guard clauses untuk early returns**
   ```javascript
   if (!isValid) return;
   // main logic here
   ```

3. **Use ternary untuk simple conditions**
   ```javascript
   let message = isLoggedIn ? 'Welcome' : 'Login';
   ```

4. **Use switch untuk multiple specific values**
   ```javascript
   switch (status) {
       case 'pending': // ...
       case 'approved': // ...
   }
   ```

5. **Always include break in switch**
   ```javascript
   switch (value) {
       case 1:
           console.log('One');
           break;  // Don't forget!
   }
   ```

### DON'T (Jangan)

1. **Jangan nested if terlalu dalam (max 2-3 levels)**
   ```javascript
   // Bad
   if (a) {
       if (b) {
           if (c) {
               if (d) {
                   // too nested!
               }
           }
       }
   }
   
   // Better: use guard clauses or functions
   ```

2. **Jangan compare boolean dengan true/false**
   ```javascript
   // Bad
   if (isActive === true) {
       // ...
   }
   
   // Good
   if (isActive) {
       // ...
   }
   ```

3. **Jangan nested ternary terlalu complex**
   ```javascript
   // Bad
   let result = a ? b ? c ? d : e : f : g;
   
   // Better: use if-else
   ```

4. **Jangan lupa default case di switch**
   ```javascript
   switch (value) {
       case 'a': // ...
       case 'b': // ...
       default:  // Always include
           console.log('Unknown value');
   }
   ```

---

## Code Examples

### Login Validation

```javascript
function validateLogin(username, password) {
    // Guard clauses
    if (!username) {
        return { success: false, message: 'Username required' };
    }
    
    if (!password) {
        return { success: false, message: 'Password required' };
    }
    
    if (username.length < 3) {
        return { success: false, message: 'Username too short' };
    }
    
    if (password.length < 8) {
        return { success: false, message: 'Password too short' };
    }
    
    // Main logic
    return { success: true, message: 'Login successful' };
}
```

### Grade Calculator

```javascript
function calculateGrade(score) {
    // Input validation
    if (typeof score !== 'number') {
        return 'Error: Score must be a number';
    }
    
    if (score < 0 || score > 100) {
        return 'Error: Score must be between 0 and 100';
    }
    
    // Calculate grade
    if (score >= 90) return 'A';
    if (score >= 80) return 'B';
    if (score >= 70) return 'C';
    if (score >= 60) return 'D';
    return 'F';
}
```

### Access Control

```javascript
function checkAccess(user, resource) {
    // Check if user exists
    if (!user) {
        return { granted: false, reason: 'Not logged in' };
    }
    
    // Check if user is active
    if (!user.isActive) {
        return { granted: false, reason: 'Account inactive' };
    }
    
    // Check permissions
    if (user.role === 'admin') {
        return { granted: true };
    }
    
    if (user.permissions.includes(resource)) {
        return { granted: true };
    }
    
    return { granted: false, reason: 'Insufficient permissions' };
}
```

---

## Rangkuman

- `if` untuk conditional execution
- `else` untuk alternative execution
- `else if` untuk multiple conditions
- `switch` untuk multiple specific values
- Ternary operator untuk simple conditions
- Guard clauses untuk clean code
- Short-circuit evaluation dengan && dan ||
- Nullish coalescing (??) untuk precise defaults
- Always use === for comparisons
- Avoid deeply nested conditions
- Use early returns untuk better readability

---

**Sebelumnya:** [04. Operators](04-operators.md)  
**Selanjutnya:** [06. Loops](06-loops.md)

