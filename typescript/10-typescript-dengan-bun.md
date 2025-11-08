# 10. TypeScript dengan Bun

## Native TypeScript Support

Bun dapat run TypeScript directly tanpa compilation step!

### Run TypeScript

```bash
# No transpilation needed
bun index.ts

# Watch mode
bun --watch index.ts

# Run dengan environment
DATABASE_URL=postgres://... bun index.ts
```

---

## Bun-Specific Types

### Install Bun Types

```bash
bun add -d bun-types
```

### tsconfig.json

```json
{
  "compilerOptions": {
    "types": ["bun-types"],
    "module": "ESNext",
    "target": "ESNext",
    "moduleResolution": "bundler"
  }
}
```

---

## Bun APIs dengan TypeScript

### Bun.file()

```typescript
// Read file
const file = Bun.file("data.txt");
const content: string = await file.text();

// Type-safe file operations
interface Config {
    port: number;
    host: string;
}

const configFile = Bun.file("config.json");
const config: Config = await configFile.json();

console.log(`Server: ${config.host}:${config.port}`);
```

### Bun.serve()

```typescript
// HTTP Server dengan types
interface User {
    id: number;
    name: string;
    email: string;
}

const users: User[] = [];

Bun.serve({
    port: 3000,
    
    async fetch(req: Request): Promise<Response> {
        const url = new URL(req.url);
        
        if (url.pathname === "/api/users" && req.method === "GET") {
            return Response.json(users);
        }
        
        if (url.pathname === "/api/users" && req.method === "POST") {
            const body: Omit<User, 'id'> = await req.json();
            const newUser: User = {
                id: users.length + 1,
                ...body
            };
            users.push(newUser);
            
            return Response.json(newUser, { status: 201 });
        }
        
        return Response.json(
            { error: "Not found" },
            { status: 404 }
        );
    },
});
```

---

## Request Handler Types

### Type-safe Route Handlers

```typescript
type RouteHandler = (req: Request) => Response | Promise<Response>;

const handleUsers: RouteHandler = async (req) => {
    const users = await getUsers();
    return Response.json(users);
};

const handleCreateUser: RouteHandler = async (req) => {
    const body = await req.json();
    const user = await createUser(body);
    return Response.json(user, { status: 201 });
};
```

### Request Body Types

```typescript
interface CreateUserDTO {
    name: string;
    email: string;
    password: string;
}

interface UpdateUserDTO {
    name?: string;
    email?: string;
}

Bun.serve({
    async fetch(req) {
        if (req.method === "POST") {
            const body: CreateUserDTO = await req.json();
            
            // TypeScript ensures all required fields present
            const user = {
                id: 1,
                ...body,
                createdAt: new Date()
            };
            
            return Response.json(user);
        }
        
        return new Response("Not Found", { status: 404 });
    },
});
```

---

## Environment Variables

### Type-safe Environment

```typescript
// env.ts
interface Environment {
    PORT: number;
    DATABASE_URL: string;
    API_KEY: string;
    NODE_ENV: 'development' | 'production' | 'test';
}

export function getEnv(): Environment {
    return {
        PORT: parseInt(process.env.PORT || '3000'),
        DATABASE_URL: process.env.DATABASE_URL || '',
        API_KEY: process.env.API_KEY || '',
        NODE_ENV: (process.env.NODE_ENV as Environment['NODE_ENV']) || 'development'
    };
}
```

```typescript
// index.ts
import { getEnv } from './env.js';

const env = getEnv();

Bun.serve({
    port: env.PORT,
    fetch(req) {
        return Response.json({
            environment: env.NODE_ENV
        });
    },
});
```

---

## Database dengan TypeScript

### Prisma Example

```bash
# Install Prisma
bun add prisma @prisma/client
bun add -d @types/node

# Initialize
bunx prisma init
```

```typescript
// schema.prisma
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String
  createdAt DateTime @default(now())
}
```

```typescript
// db.ts
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();

export async function getUsers() {
    return await prisma.user.findMany();
}

export async function createUser(email: string, name: string) {
    return await prisma.user.create({
        data: { email, name }
    });
}

// TypeScript knows return types automatically!
const users = await getUsers();  // User[]
```

---

## Validation Libraries

### Zod with TypeScript

```bash
bun add zod
```

```typescript
import { z } from 'zod';

// Define schema
const UserSchema = z.object({
    name: z.string().min(3),
    email: z.string().email(),
    age: z.number().min(18).optional()
});

// Infer type from schema
type User = z.infer<typeof UserSchema>;

// Validate
function createUser(data: unknown): User {
    return UserSchema.parse(data);  // Throws if invalid
}

// Safe parse
const result = UserSchema.safeParse(data);
if (result.success) {
    const user: User = result.data;
} else {
    console.error(result.error);
}
```

---

## API Response Types

### Type-safe API

```typescript
// types/api.ts
export interface ApiResponse<T> {
    success: boolean;
    data?: T;
    error?: string;
}

export interface PaginatedResponse<T> extends ApiResponse<T[]> {
    page: number;
    limit: number;
    total: number;
}

// services/api.ts
export async function fetchUsers(): Promise<ApiResponse<User[]>> {
    try {
        const response = await fetch('/api/users');
        const data = await response.json();
        
        return {
            success: true,
            data
        };
    } catch (error) {
        return {
            success: false,
            error: error.message
        };
    }
}

// Usage
const response = await fetchUsers();
if (response.success && response.data) {
    response.data.forEach(user => {
        console.log(user.name);  // TypeScript knows structure
    });
}
```

---

## Testing dengan TypeScript

### Bun Test Runner

```typescript
// math.ts
export function add(a: number, b: number): number {
    return a + b;
}

export function divide(a: number, b: number): number {
    if (b === 0) {
        throw new Error("Cannot divide by zero");
    }
    return a / b;
}
```

```typescript
// math.test.ts
import { expect, test, describe } from "bun:test";
import { add, divide } from "./math";

describe("Math functions", () => {
    test("add two numbers", () => {
        expect(add(2, 3)).toBe(5);
        expect(add(-1, 1)).toBe(0);
    });
    
    test("divide two numbers", () => {
        expect(divide(10, 2)).toBe(5);
        expect(divide(15, 3)).toBe(5);
    });
    
    test("divide by zero throws error", () => {
        expect(() => divide(10, 0)).toThrow("Cannot divide by zero");
    });
});
```

---

## Complete Example: REST API

```typescript
// types/user.ts
export interface User {
    id: number;
    username: string;
    email: string;
    createdAt: Date;
}

export interface CreateUserDTO {
    username: string;
    email: string;
    password: string;
}

export interface UpdateUserDTO {
    username?: string;
    email?: string;
}
```

```typescript
// services/userService.ts
import type { User, CreateUserDTO, UpdateUserDTO } from '../types/user.js';

export class UserService {
    private users: User[] = [];
    private nextId = 1;
    
    create(dto: CreateUserDTO): User {
        const user: User = {
            id: this.nextId++,
            username: dto.username,
            email: dto.email,
            createdAt: new Date()
        };
        
        this.users.push(user);
        return user;
    }
    
    findAll(): User[] {
        return this.users;
    }
    
    findById(id: number): User | undefined {
        return this.users.find(u => u.id === id);
    }
    
    update(id: number, dto: UpdateUserDTO): User | undefined {
        const user = this.findById(id);
        if (user) {
            Object.assign(user, dto);
            return user;
        }
        return undefined;
    }
    
    delete(id: number): boolean {
        const index = this.users.findIndex(u => u.id === id);
        if (index > -1) {
            this.users.splice(index, 1);
            return true;
        }
        return false;
    }
}
```

```typescript
// index.ts
import { UserService } from './services/userService.js';
import type { CreateUserDTO } from './types/user.js';

const userService = new UserService();

Bun.serve({
    port: 3000,
    
    async fetch(req: Request): Promise<Response> {
        const url = new URL(req.url);
        const path = url.pathname;
        const method = req.method;
        
        try {
            // GET /api/users
            if (path === "/api/users" && method === "GET") {
                const users = userService.findAll();
                return Response.json(users);
            }
            
            // POST /api/users
            if (path === "/api/users" && method === "POST") {
                const dto: CreateUserDTO = await req.json();
                const user = userService.create(dto);
                return Response.json(user, { status: 201 });
            }
            
            // GET /api/users/:id
            if (path.match(/^\/api\/users\/\d+$/) && method === "GET") {
                const id = parseInt(path.split("/").pop()!);
                const user = userService.findById(id);
                
                if (user) {
                    return Response.json(user);
                }
                return Response.json({ error: "Not found" }, { status: 404 });
            }
            
            return Response.json({ error: "Not found" }, { status: 404 });
            
        } catch (error) {
            return Response.json(
                { error: "Internal server error" },
                { status: 500 }
            );
        }
    },
});

console.log("TypeScript API Server running on http://localhost:3000");
```

---

## Best Practices

### DO (Lakukan)

1. **Enable strict mode**
   ```json
   {
     "compilerOptions": {
       "strict": true
     }
   }
   ```

2. **Define clear types untuk DTOs**
   ```typescript
   interface CreateUserDTO {
       username: string;
       email: string;
   }
   ```

3. **Use type guards**
   ```typescript
   if (typeof value === "string") {
       // TypeScript knows it's string
   }
   ```

4. **Type function parameters dan returns**
   ```typescript
   async function fetchData(): Promise<Data> {
       // ...
   }
   ```

5. **Use generics untuk reusable code**
   ```typescript
   class Repository<T> {
       // ...
   }
   ```

### DON'T (Jangan)

1. **Jangan use 'any'**
   ```typescript
   // Bad
   let data: any;
   
   // Good
   let data: unknown;
   ```

2. **Jangan skip types di function parameters**

3. **Jangan ignore TypeScript errors**

---

## Rangkuman

- Bun support TypeScript native (no compilation!)
- Run .ts files directly: `bun file.ts`
- Install bun-types untuk Bun API types
- Type-safe file operations dengan Bun.file()
- Type-safe HTTP server dengan Bun.serve()
- Use DTOs untuk request/response types
- Combine dengan validation libraries (Zod)
- Built-in test runner support TypeScript
- Environment variables dapat di-type
- Bun makes TypeScript development seamless

---

**Sebelumnya:** [09. Modules](09-modules.md)  
**Selanjutnya:** [README - Daftar Lengkap Materi](README.md)

