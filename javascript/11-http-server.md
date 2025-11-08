# 11. HTTP Server dengan Bun

## Bun.serve()

Bun menyediakan `Bun.serve()` untuk membuat HTTP server yang sangat cepat.

### Basic Server

```javascript
// server.js
Bun.serve({
    port: 3000,
    fetch(req) {
        return new Response("Hello from Bun!");
    },
});

console.log("Server running on http://localhost:3000");

// Run: bun server.js
```

### Server dengan Options

```javascript
const server = Bun.serve({
    port: 3000,
    hostname: "localhost",
    
    fetch(req) {
        return new Response("Hello Bun!");
    },
    
    error(error) {
        return new Response(`Error: ${error.message}`, { status: 500 });
    },
});

console.log(`Server running at http://${server.hostname}:${server.port}`);
```

---

## Handling Requests

### Request Object

```javascript
Bun.serve({
    port: 3000,
    fetch(req) {
        console.log("Method:", req.method);
        console.log("URL:", req.url);
        console.log("Headers:", req.headers);
        
        return new Response("Request logged");
    },
});
```

### URL Parsing

```javascript
Bun.serve({
    port: 3000,
    fetch(req) {
        const url = new URL(req.url);
        
        console.log("Pathname:", url.pathname);
        console.log("Search params:", url.searchParams);
        
        const name = url.searchParams.get("name");
        
        return new Response(`Hello ${name || 'Guest'}!`);
    },
});

// Access: http://localhost:3000?name=John
```

---

## Response Types

### Text Response

```javascript
Bun.serve({
    fetch(req) {
        return new Response("Plain text response", {
            headers: {
                "Content-Type": "text/plain",
            },
        });
    },
});
```

### JSON Response

```javascript
Bun.serve({
    fetch(req) {
        const data = {
            message: "Hello from API",
            timestamp: new Date().toISOString(),
            version: Bun.version
        };
        
        return Response.json(data);
        
        // Atau
        return new Response(JSON.stringify(data), {
            headers: {
                "Content-Type": "application/json",
            },
        });
    },
});
```

### HTML Response

```javascript
Bun.serve({
    fetch(req) {
        const html = `
            <!DOCTYPE html>
            <html>
            <head>
                <title>Bun Server</title>
            </head>
            <body>
                <h1>Hello from Bun!</h1>
                <p>Current time: ${new Date().toLocaleString()}</p>
            </body>
            </html>
        `;
        
        return new Response(html, {
            headers: {
                "Content-Type": "text/html",
            },
        });
    },
});
```

### File Response

```javascript
Bun.serve({
    async fetch(req) {
        // Serve static file
        return new Response(Bun.file("index.html"));
    },
});
```

---

## Routing

### Simple Routing

```javascript
Bun.serve({
    port: 3000,
    fetch(req) {
        const url = new URL(req.url);
        const path = url.pathname;
        
        if (path === "/") {
            return new Response("Home Page");
        }
        
        if (path === "/about") {
            return new Response("About Page");
        }
        
        if (path === "/api/users") {
            return Response.json({ users: ["John", "Jane"] });
        }
        
        return new Response("404 Not Found", { status: 404 });
    },
});
```

### Method-based Routing

```javascript
Bun.serve({
    fetch(req) {
        const url = new URL(req.url);
        
        // GET /api/users
        if (req.method === "GET" && url.pathname === "/api/users") {
            return Response.json([
                { id: 1, name: "John" },
                { id: 2, name: "Jane" }
            ]);
        }
        
        // POST /api/users
        if (req.method === "POST" && url.pathname === "/api/users") {
            return Response.json({
                message: "User created",
                status: 201
            }, { status: 201 });
        }
        
        // PUT /api/users/:id
        if (req.method === "PUT" && url.pathname.startsWith("/api/users/")) {
            const id = url.pathname.split("/").pop();
            return Response.json({
                message: `User ${id} updated`
            });
        }
        
        // DELETE /api/users/:id
        if (req.method === "DELETE" && url.pathname.startsWith("/api/users/")) {
            const id = url.pathname.split("/").pop();
            return Response.json({
                message: `User ${id} deleted`
            });
        }
        
        return new Response("Not Found", { status: 404 });
    },
});
```

---

## Request Body

### JSON Body

```javascript
Bun.serve({
    async fetch(req) {
        if (req.method === "POST") {
            const body = await req.json();
            
            console.log("Received:", body);
            
            return Response.json({
                message: "Data received",
                data: body
            });
        }
        
        return new Response("Method not allowed", { status: 405 });
    },
});

// Test dengan:
// curl -X POST http://localhost:3000 -H "Content-Type: application/json" -d '{"name":"John"}'
```

### Form Data

```javascript
Bun.serve({
    async fetch(req) {
        if (req.method === "POST") {
            const formData = await req.formData();
            
            const name = formData.get("name");
            const email = formData.get("email");
            
            return Response.json({
                name,
                email
            });
        }
    },
});
```

### Text Body

```javascript
Bun.serve({
    async fetch(req) {
        if (req.method === "POST") {
            const text = await req.text();
            
            console.log("Received text:", text);
            
            return new Response("Text received");
        }
    },
});
```

---

## Headers

### Reading Headers

```javascript
Bun.serve({
    fetch(req) {
        const userAgent = req.headers.get("User-Agent");
        const auth = req.headers.get("Authorization");
        
        console.log("User-Agent:", userAgent);
        console.log("Authorization:", auth);
        
        return new Response("Headers logged");
    },
});
```

### Setting Response Headers

```javascript
Bun.serve({
    fetch(req) {
        return new Response("Success", {
            headers: {
                "Content-Type": "text/plain",
                "X-Custom-Header": "My Value",
                "Access-Control-Allow-Origin": "*",
            },
        });
    },
});
```

### CORS Headers

```javascript
Bun.serve({
    fetch(req) {
        // Handle preflight
        if (req.method === "OPTIONS") {
            return new Response(null, {
                headers: {
                    "Access-Control-Allow-Origin": "*",
                    "Access-Control-Allow-Methods": "GET, POST, PUT, DELETE",
                    "Access-Control-Allow-Headers": "Content-Type",
                },
            });
        }
        
        // Regular response
        return new Response("Data", {
            headers: {
                "Access-Control-Allow-Origin": "*",
            },
        });
    },
});
```

---

## Static File Server

```javascript
Bun.serve({
    port: 3000,
    async fetch(req) {
        const url = new URL(req.url);
        const path = url.pathname;
        
        // Remove leading slash
        const filePath = path === "/" ? "index.html" : path.slice(1);
        
        const file = Bun.file(filePath);
        
        // Check if file exists
        if (await file.exists()) {
            return new Response(file);
        }
        
        // 404 if not found
        return new Response("404 Not Found", { status: 404 });
    },
});

// Directory structure:
// ├── index.html
// ├── about.html
// ├── styles.css
// └── script.js

// Access:
// http://localhost:3000/        → index.html
// http://localhost:3000/about.html → about.html
// http://localhost:3000/styles.css → styles.css
```

---

## REST API Example

### Simple CRUD API

```javascript
// server.js
let users = [
    { id: 1, name: "John", email: "john@example.com" },
    { id: 2, name: "Jane", email: "jane@example.com" }
];

let nextId = 3;

Bun.serve({
    port: 3000,
    async fetch(req) {
        const url = new URL(req.url);
        const path = url.pathname;
        const method = req.method;
        
        // GET /api/users - Get all users
        if (method === "GET" && path === "/api/users") {
            return Response.json(users);
        }
        
        // GET /api/users/:id - Get user by ID
        if (method === "GET" && path.startsWith("/api/users/")) {
            const id = parseInt(path.split("/").pop());
            const user = users.find(u => u.id === id);
            
            if (user) {
                return Response.json(user);
            }
            return Response.json({ error: "User not found" }, { status: 404 });
        }
        
        // POST /api/users - Create user
        if (method === "POST" && path === "/api/users") {
            const body = await req.json();
            const newUser = {
                id: nextId++,
                name: body.name,
                email: body.email
            };
            users.push(newUser);
            
            return Response.json(newUser, { status: 201 });
        }
        
        // PUT /api/users/:id - Update user
        if (method === "PUT" && path.startsWith("/api/users/")) {
            const id = parseInt(path.split("/").pop());
            const userIndex = users.findIndex(u => u.id === id);
            
            if (userIndex !== -1) {
                const body = await req.json();
                users[userIndex] = { ...users[userIndex], ...body };
                return Response.json(users[userIndex]);
            }
            
            return Response.json({ error: "User not found" }, { status: 404 });
        }
        
        // DELETE /api/users/:id - Delete user
        if (method === "DELETE" && path.startsWith("/api/users/")) {
            const id = parseInt(path.split("/").pop());
            const userIndex = users.findIndex(u => u.id === id);
            
            if (userIndex !== -1) {
                const deleted = users.splice(userIndex, 1)[0];
                return Response.json({ message: "User deleted", user: deleted });
            }
            
            return Response.json({ error: "User not found" }, { status: 404 });
        }
        
        return Response.json({ error: "Not found" }, { status: 404 });
    },
});

console.log("API Server running on http://localhost:3000");
```

---

## WebSocket Server

```javascript
Bun.serve({
    port: 3000,
    
    fetch(req, server) {
        // Upgrade to WebSocket
        if (server.upgrade(req)) {
            return; // Don't return response, WebSocket takes over
        }
        
        return new Response("Expected WebSocket connection", { status: 400 });
    },
    
    websocket: {
        open(ws) {
            console.log("Client connected");
            ws.send("Welcome!");
        },
        
        message(ws, message) {
            console.log("Received:", message);
            ws.send(`Echo: ${message}`);
        },
        
        close(ws) {
            console.log("Client disconnected");
        },
    },
});
```

---

## Environment-based Configuration

```javascript
// .env
// PORT=3000
// HOST=localhost
// NODE_ENV=development

const PORT = process.env.PORT || 3000;
const HOST = process.env.HOST || "localhost";

const server = Bun.serve({
    port: PORT,
    hostname: HOST,
    
    fetch(req) {
        return Response.json({
            environment: process.env.NODE_ENV,
            message: "API is running"
        });
    },
});

console.log(`Server running on http://${server.hostname}:${server.port}`);

// Run: bun server.js
```

---

## Best Practices

### DO (Lakukan)

1. **Use Response.json() untuk JSON**
   ```javascript
   return Response.json({ data: "value" });
   ```

2. **Set appropriate status codes**
   ```javascript
   return new Response("Created", { status: 201 });
   return new Response("Not Found", { status: 404 });
   return new Response("Error", { status: 500 });
   ```

3. **Handle errors**
   ```javascript
   Bun.serve({
       fetch(req) {
           try {
               // ... logic
           } catch (error) {
               return new Response("Internal Error", { status: 500 });
           }
       },
       error(error) {
           return new Response(error.message, { status: 500 });
       }
   });
   ```

4. **Use environment variables**
   ```javascript
   const PORT = process.env.PORT || 3000;
   ```

5. **Validate request data**
   ```javascript
   const body = await req.json();
   if (!body.email) {
       return Response.json({ error: "Email required" }, { status: 400 });
   }
   ```

### DON'T (Jangan)

1. **Jangan hardcode sensitive data**
   ```javascript
   // Bad
   const API_KEY = "secret123";
   
   // Good
   const API_KEY = process.env.API_KEY;
   ```

2. **Jangan lupa error handling**

3. **Jangan return without Response object**
   ```javascript
   // Bad
   return "text";  // Error!
   
   // Good
   return new Response("text");
   ```

---

## Complete API Example

```javascript
// api-server.js
const users = [];
let nextId = 1;

Bun.serve({
    port: process.env.PORT || 3000,
    
    async fetch(req) {
        const url = new URL(req.url);
        const path = url.pathname;
        const method = req.method;
        
        // CORS
        const headers = {
            "Access-Control-Allow-Origin": "*",
            "Content-Type": "application/json",
        };
        
        if (method === "OPTIONS") {
            return new Response(null, {
                headers: {
                    ...headers,
                    "Access-Control-Allow-Methods": "GET, POST, PUT, DELETE",
                    "Access-Control-Allow-Headers": "Content-Type",
                },
            });
        }
        
        try {
            // Routes
            if (path === "/api/health") {
                return Response.json({ status: "OK", timestamp: Date.now() });
            }
            
            if (path === "/api/users") {
                if (method === "GET") {
                    return Response.json(users);
                }
                
                if (method === "POST") {
                    const body = await req.json();
                    
                    if (!body.name || !body.email) {
                        return Response.json(
                            { error: "Name and email required" },
                            { status: 400 }
                        );
                    }
                    
                    const newUser = {
                        id: nextId++,
                        ...body,
                        createdAt: new Date().toISOString()
                    };
                    
                    users.push(newUser);
                    return Response.json(newUser, { status: 201 });
                }
            }
            
            if (path.startsWith("/api/users/") && path.split("/").length === 4) {
                const id = parseInt(path.split("/")[3]);
                const userIndex = users.findIndex(u => u.id === id);
                
                if (method === "GET") {
                    if (userIndex !== -1) {
                        return Response.json(users[userIndex]);
                    }
                    return Response.json({ error: "User not found" }, { status: 404 });
                }
                
                if (method === "PUT") {
                    if (userIndex !== -1) {
                        const body = await req.json();
                        users[userIndex] = { ...users[userIndex], ...body };
                        return Response.json(users[userIndex]);
                    }
                    return Response.json({ error: "User not found" }, { status: 404 });
                }
                
                if (method === "DELETE") {
                    if (userIndex !== -1) {
                        const deleted = users.splice(userIndex, 1)[0];
                        return Response.json({ message: "Deleted", user: deleted });
                    }
                    return Response.json({ error: "User not found" }, { status: 404 });
                }
            }
            
            return Response.json({ error: "Not found" }, { status: 404 });
            
        } catch (error) {
            console.error("Error:", error);
            return Response.json(
                { error: "Internal server error" },
                { status: 500 }
            );
        }
    },
    
    error(error) {
        console.error("Server error:", error);
        return new Response("Internal Server Error", { status: 500 });
    },
});

console.log("API Server running on http://localhost:3000");
```

---

## Middleware Pattern

```javascript
// middleware.js
export function logger(req) {
    const timestamp = new Date().toISOString();
    console.log(`[${timestamp}] ${req.method} ${req.url}`);
}

export function authenticate(req) {
    const auth = req.headers.get("Authorization");
    
    if (!auth || !auth.startsWith("Bearer ")) {
        return Response.json({ error: "Unauthorized" }, { status: 401 });
    }
    
    // Verify token here
    return null; // null means OK
}
```

```javascript
// server.js
import { logger, authenticate } from './middleware.js';

Bun.serve({
    async fetch(req) {
        // Logger middleware
        logger(req);
        
        const url = new URL(req.url);
        
        // Public routes
        if (url.pathname === "/login") {
            return Response.json({ token: "abc123" });
        }
        
        // Protected routes
        const authError = authenticate(req);
        if (authError) return authError;
        
        // Your protected logic here
        return Response.json({ message: "Authenticated!" });
    },
});
```

---

## Hot Reload Development

```javascript
// server.js dengan auto-reload
Bun.serve({
    port: 3000,
    fetch(req) {
        return new Response("Server with hot reload!");
    },
});

// Run dengan watch mode
// bun --watch server.js
```

---

## Rangkuman

- Bun.serve() untuk create HTTP server (sangat cepat)
- fetch() function untuk handle requests
- Return Response object
- Response.json() untuk JSON responses
- Parse request body dengan req.json(), req.formData(), req.text()
- URL parsing dengan new URL()
- Simple routing dengan if statements
- CORS dengan headers
- Environment variables dengan process.env
- WebSocket support built-in
- Hot reload dengan --watch flag
- Bun server 4x lebih cepat dari Node.js

---

**Sebelumnya:** [10. File System](10-file-system.md)  
**Selanjutnya:** [README - Daftar Lengkap Materi](README.md)

