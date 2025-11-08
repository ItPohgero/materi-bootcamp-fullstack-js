# Learning Path - Panduan Pembelajaran Lengkap

Dokumen ini menjelaskan urutan pembelajaran yang optimal dan mapping materi untuk memastikan tidak ada tumpang tindih.

---

## Overview Struktur Materi

```
01-git/        → 10 topik → Version Control
02-javascript/ → 11 topik → Programming Fundamentals (Bun)
03-typescript/ → 10 topik → Type-Safe Programming (Bun)
04-sql/        → 10 topik → Database Management (PostgreSQL)
```

**Total: 41 topik pembelajaran**

---

## Materi 01: Git (Version Control)

### Fokus
Version control, code management, collaboration

### Topik (10)
1. Pengenalan Git - Konsep VCS
2. Instalasi dan Konfigurasi - Setup Git
3. Membuat Repository - Init & Clone
4. Basic Workflow - Add, Commit, Status
5. Melihat History - Log, Show, Blame
6. Bekerja dengan Branch - Branch management
7. Merging Branches - Merge & conflicts
8. Remote Repository - Push, Pull, GitHub
9. Membatalkan Perubahan - Reset, Revert
10. .gitignore - File management

### Dependencies
- Tidak ada (start here)

### Time Estimate
- Beginner: 1 minggu
- With practice: 2-3 hari

### Practical Output
- Setup Git dan GitHub
- Manage code repositories
- Branching strategy
- Collaboration workflow

---

## Materi 02: JavaScript (Programming dengan Bun)

### Fokus
JavaScript programming fundamentals, backend development dengan Bun.js

### Topik (11)
1. Pengenalan JavaScript - Overview, Bun vs Node.js
2. Setup Environment - Install Bun, project setup
3. Variabel dan Tipe Data - let, const, types
4. Operators - Arithmetic, logical, comparison
5. Control Flow - if, switch, ternary
6. Loops - for, while, array methods
7. Functions - Declaration, arrow, callbacks
8. Arrays - Methods, iteration, transformation
9. Objects - Properties, methods, classes
10. File System - Bun.file() API
11. HTTP Server - Bun.serve() API

### Dependencies
- Git basics (untuk save progress)

### Time Estimate
- Beginner: 3-4 minggu
- With programming background: 2 minggu

### Practical Output
- CLI tools
- File operations
- HTTP servers
- REST APIs
- Async programming

### No Overlap dengan TypeScript
- Fokus JavaScript syntax
- No type annotations
- Runtime behavior
- Dynamic typing

### No Overlap dengan SQL
- Tidak ada database operations
- File-based storage examples
- Preparation untuk database integration

---

## Materi 03: TypeScript (Type-Safe JavaScript)

### Fokus
Type system, type safety, better tooling dengan Bun native support

### Topik (10)
1. Pengenalan TypeScript - TS vs JS, benefits
2. Setup TypeScript - Bun native support
3. Basic Types - string, number, boolean, etc
4. Interfaces & Type Aliases - Custom types
5. Functions - Type annotations
6. Classes - OOP dengan types
7. Generics - Reusable type-safe code
8. Advanced Types - Union, mapped, conditional
9. Modules - Import/export dengan types
10. TypeScript dengan Bun - Practical integration

### Dependencies
- JavaScript fundamentals (materi 02)
- Understanding of JS syntax

### Time Estimate
- After JavaScript: 1-2 minggu
- Without JS background: 3-4 minggu (learn JS first!)

### Practical Output
- Type-safe APIs
- Better code quality
- Reduced runtime errors
- Better IDE support

### Build on JavaScript, Not Duplicate
- Same syntax, add types
- Enhanced JavaScript, not replacement
- Focus on type system
- No re-teaching JavaScript basics

### Integration Points
- Convert JS code to TS
- Add types to existing projects
- Type-safe database operations (preparation for SQL)

---

## Materi 04: SQL (Database dengan PostgreSQL)

### Fokus
Relational database, data modeling, querying

### Topik (10)
1. Pengenalan SQL & PostgreSQL - RDBMS concepts
2. Instalasi dan Konfigurasi - Setup PostgreSQL
3. Database dan Table - DDL operations
4. Tipe Data PostgreSQL - Data types
5. Basic Query SELECT - Reading data
6. Filtering WHERE - Conditions
7. Sorting dan Limiting - ORDER BY, LIMIT
8. Insert, Update, Delete - Data manipulation
9. JOIN Tables - Relationships
10. Aggregate Functions - GROUP BY, analytics

### Dependencies
- JavaScript/TypeScript (untuk integration examples)
- Basic programming concepts

### Time Estimate
- Beginner: 2-3 minggu
- With DB experience: 1 minggu

### Practical Output
- Database design
- Complex queries
- Data relationships
- Integration dengan TypeScript

### No Overlap dengan JavaScript/TypeScript
- Different domain (data vs logic)
- SQL syntax vs JS syntax
- Declarative vs Imperative
- But: integration examples menggunakan JS/TS

### Integration with Programming
- Connect dari TypeScript
- Type-safe queries
- ORM concepts (Prisma mention)

---

## Mapping Materi - Tidak Ada Tumpang Tindih

### Git - Standalone
- Pure version control
- No programming concepts
- Tool for managing code
- Used across ALL projects

### JavaScript - Programming Fundamentals
- Syntax dan semantics
- Logic dan algorithms
- Bun runtime features
- File dan HTTP operations
- No types (that's TypeScript)
- No database (that's SQL)

### TypeScript - Type Layer
- Build on JavaScript
- Add type system
- Same runtime (Bun)
- No new runtime features
- Focus on compile-time safety

### SQL - Data Layer
- Different language entirely
- Data modeling
- Querying data
- No application logic
- Integrates with JS/TS

---

## Progressive Complexity

### Week 1-2: Git
**Complexity:** Low  
**Hands-on:** High  
**Concepts:** Version control, branching

### Week 3-5: JavaScript
**Complexity:** Medium  
**Hands-on:** Very High  
**Concepts:** Programming fundamentals, algorithms

### Week 6-7: TypeScript
**Complexity:** Medium-High  
**Hands-on:** High  
**Concepts:** Type system, generics

### Week 8-10: SQL
**Complexity:** Medium  
**Hands-on:** High  
**Concepts:** Data modeling, relational algebra

---

## Integration Points

### Git + JavaScript
```bash
git init
# Create JavaScript project
git add .
git commit -m "Initial JavaScript project"
```

### JavaScript + TypeScript
```typescript
// Convert JavaScript to TypeScript
// Add type annotations
// Enhanced JavaScript, same runtime (Bun)
```

### TypeScript + SQL
```typescript
// Type-safe database queries
interface User {
    id: number;
    name: string;
    email: string;
}

// Query results are typed
const users: User[] = await db.query('SELECT * FROM users');
```

### All Together
```typescript
// Git: Version control
// TypeScript: Type-safe code
// Bun: Fast runtime
// PostgreSQL: Data storage

import { Pool } from 'pg';

interface User {
    id: number;
    name: string;
    email: string;
}

const pool = new Pool({
    connectionString: process.env.DATABASE_URL
});

async function getUsers(): Promise<User[]> {
    const result = await pool.query('SELECT * FROM users');
    return result.rows;
}

// Commit to Git
// Run with Bun: bun index.ts
```

---

## Quality Assurance

### Materi Sudah Di-Review Untuk:

**Consistency**
- Naming conventions sama
- Code style sama
- Format markdown sama
- Tanpa emoji/icon

**Completeness**
- Setiap topik lengkap
- Examples praktis
- Best practices
- Troubleshooting

**Progression**
- Simple ke complex
- Building on previous knowledge
- Clear prerequisites

**No Redundancy**
- Each topic focused
- No unnecessary repetition
- Clear boundaries

**Professional Quality**
- Production-ready examples
- Industry best practices
- Real-world patterns

---

## Recommended Daily Schedule

### Beginner (2-3 jam/hari)

**Hour 1: Theory**
- Read materi
- Understand concepts
- Take notes

**Hour 2: Practice**
- Type all code examples
- Modify examples
- Experiment

**Hour 3: Projects**
- Build small projects
- Apply what learned
- Commit to Git

### Intermediate (1-2 jam/hari)

**Hour 1: Learn**
- Skim materi
- Focus on new concepts
- Practice advanced topics

**Hour 2: Build**
- Work on project
- Apply concepts
- Refine skills

---

## Assessment Checkpoints

### After Git (Week 2)
Test yourself:
- [ ] Dapat create repository
- [ ] Dapat branching dan merging
- [ ] Dapat resolve conflicts
- [ ] Dapat push/pull dari GitHub
- [ ] Comfortable dengan Git workflow

### After JavaScript (Week 6)
Test yourself:
- [ ] Dapat write functions dan classes
- [ ] Understand async/await
- [ ] Dapat manipulate arrays dan objects
- [ ] Dapat build HTTP server dengan Bun
- [ ] Dapat read/write files

### After TypeScript (Week 8)
Test yourself:
- [ ] Dapat define interfaces
- [ ] Understand generic types
- [ ] Dapat convert JS to TS
- [ ] Dapat build type-safe API
- [ ] Comfortable dengan type system

### After SQL (Week 11)
Test yourself:
- [ ] Dapat design database schema
- [ ] Dapat write complex queries
- [ ] Understand JOINs
- [ ] Dapat optimize queries
- [ ] Dapat integrate dengan TypeScript

---

## Final Project Ideas

Combine semua skills:

### 1. Task Management API
**Tech Stack:**
- Git: Version control
- TypeScript: Backend logic
- Bun: Runtime
- PostgreSQL: Data storage

**Features:**
- User authentication
- CRUD operations
- Database relationships
- RESTful API

### 2. Blog Platform Backend
**Tech Stack:** Same as above

**Features:**
- Users, posts, comments
- Tags dan categories
- Search functionality
- Pagination

### 3. E-Commerce API
**Tech Stack:** Same as above

**Features:**
- Products, orders, customers
- Cart management
- Order processing
- Analytics queries

---

## Success Path

```
Git Basics
    ↓
JavaScript Fundamentals
    ↓
Build Simple Projects
    ↓
TypeScript Basics
    ↓
Type-Safe Projects
    ↓
SQL Fundamentals
    ↓
Database Integration
    ↓
Full-Stack Projects
    ↓
Portfolio Ready
    ↓
Job Applications
```

---

## Selamat Belajar!

Materi ini dirancang untuk memberikan foundation yang solid untuk career sebagai developer. Consistency dan practice adalah kunci sukses.

**Start today. Code every day. Build your future.**

---

*Learning Path ini di-design untuk maksimal efficiency dan minimal overlap.*

