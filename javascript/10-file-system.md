# 10. File System dengan Bun

## Bun.file()

Bun menyediakan API yang sangat cepat untuk file operations.

### Reading Files

```javascript
// Read file as text
const file = Bun.file("data.txt");
const text = await file.text();
console.log(text);

// Read file as JSON
const jsonFile = Bun.file("data.json");
const data = await jsonFile.json();
console.log(data);

// Read file as ArrayBuffer
const buffer = await file.arrayBuffer();

// Read file as Stream
const stream = file.stream();
```

### File Properties

```javascript
const file = Bun.file("data.txt");

console.log(file.name);      // Filename
console.log(file.size);      // Size in bytes
console.log(file.type);      // MIME type
```

---

## Writing Files

### Bun.write()

```javascript
// Write string
await Bun.write("output.txt", "Hello Bun!");

// Write JSON
const data = { name: "John", age: 30 };
await Bun.write("data.json", JSON.stringify(data, null, 2));

// Write from Response
const response = await fetch("https://api.example.com/data");
await Bun.write("api-response.json", response);

// Write to specific path
await Bun.write("./data/output.txt", "Content");
```

### Append to File

```javascript
// Read existing content
const file = Bun.file("log.txt");
const existingContent = await file.text();

// Append new content
const newContent = existingContent + "\nNew log entry";
await Bun.write("log.txt", newContent);
```

---

## File Existence Check

```javascript
const file = Bun.file("data.txt");

if (await file.exists()) {
    console.log("File exists");
    const content = await file.text();
} else {
    console.log("File not found");
    await Bun.write("data.txt", "Default content");
}
```

---

## Reading Different File Types

### Text Files

```javascript
// Read .txt
const textFile = Bun.file("document.txt");
const text = await textFile.text();
console.log(text);

// Read .csv
const csvFile = Bun.file("data.csv");
const csvData = await csvFile.text();
const lines = csvData.split('\n');
```

### JSON Files

```javascript
// Read and parse JSON
const jsonFile = Bun.file("config.json");
const config = await jsonFile.json();

console.log(config.port);
console.log(config.database.host);
```

### Binary Files

```javascript
// Read image/binary
const imageFile = Bun.file("image.png");
const arrayBuffer = await imageFile.arrayBuffer();

// Get as Blob
const blob = await imageFile.blob();
```

---

## File Paths

### Absolute vs Relative

```javascript
// Relative path
const file1 = Bun.file("data.txt");
const file2 = Bun.file("./data.txt");
const file3 = Bun.file("../data.txt");

// Absolute path
const file4 = Bun.file("/Users/username/project/data.txt");

// Using import.meta
const file5 = Bun.file(import.meta.dir + "/data.txt");
```

### Path Helpers

```javascript
// Current directory
console.log(import.meta.dir);

// Current file
console.log(import.meta.path);

// Current file URL
console.log(import.meta.url);
```

---

## Streaming Files

### Read Stream

```javascript
const file = Bun.file("large-file.txt");
const stream = file.stream();

const reader = stream.getReader();

while (true) {
    const { done, value } = await reader.read();
    if (done) break;
    
    // Process chunk
    console.log(new TextDecoder().decode(value));
}
```

### Write Stream

```javascript
const file = Bun.file("output.txt");
const writer = file.writer();

writer.write("Line 1\n");
writer.write("Line 2\n");
writer.write("Line 3\n");

await writer.end();
```

---

## File Operations Examples

### Read Configuration File

```javascript
// config.json
// {
//     "port": 3000,
//     "host": "localhost",
//     "database": {
//         "url": "postgres://localhost/mydb"
//     }
// }

async function loadConfig() {
    const configFile = Bun.file("config.json");
    
    if (!(await configFile.exists())) {
        throw new Error("Config file not found");
    }
    
    const config = await configFile.json();
    return config;
}

const config = await loadConfig();
console.log(`Server will run on ${config.host}:${config.port}`);
```

### Logger Function

```javascript
async function log(message) {
    const timestamp = new Date().toISOString();
    const logEntry = `[${timestamp}] ${message}\n`;
    
    const logFile = Bun.file("app.log");
    const existingLogs = await logFile.exists() 
        ? await logFile.text() 
        : '';
    
    await Bun.write("app.log", existingLogs + logEntry);
}

// Usage
await log("Application started");
await log("User logged in");
await log("Database connected");
```

### Copy File

```javascript
async function copyFile(source, destination) {
    const sourceFile = Bun.file(source);
    
    if (!(await sourceFile.exists())) {
        throw new Error(`Source file not found: ${source}`);
    }
    
    const content = await sourceFile.arrayBuffer();
    await Bun.write(destination, content);
    
    console.log(`Copied ${source} to ${destination}`);
}

// Usage
await copyFile("source.txt", "backup.txt");
```

### Read CSV File

```javascript
async function readCSV(filename) {
    const file = Bun.file(filename);
    const text = await file.text();
    
    const lines = text.trim().split('\n');
    const headers = lines[0].split(',');
    
    const data = lines.slice(1).map(line => {
        const values = line.split(',');
        const obj = {};
        
        headers.forEach((header, index) => {
            obj[header.trim()] = values[index]?.trim();
        });
        
        return obj;
    });
    
    return data;
}

// Usage
const csvData = await readCSV("users.csv");
console.log(csvData);
// [
//   { name: 'John', age: '30', city: 'Jakarta' },
//   { name: 'Jane', age: '25', city: 'Bandung' }
// ]
```

### Write JSON Data

```javascript
async function saveData(filename, data) {
    const jsonString = JSON.stringify(data, null, 2);
    await Bun.write(filename, jsonString);
    console.log(`Data saved to ${filename}`);
}

// Usage
const users = [
    { id: 1, name: "John", email: "john@example.com" },
    { id: 2, name: "Jane", email: "jane@example.com" }
];

await saveData("users.json", users);
```

---

## Working with Directories

Bun tidak memiliki built-in directory operations, gunakan Node.js fs module yang compatible:

```javascript
import { readdir, mkdir, rmdir } from 'node:fs/promises';

// List files di directory
const files = await readdir('./');
console.log(files);

// Create directory
await mkdir('new-folder', { recursive: true });

// Remove directory
await rmdir('folder-to-delete');
```

Atau gunakan `Bun.file` dengan glob pattern (upcoming feature).

---

## File Upload Processing

### Handle Upload

```javascript
// server.js
Bun.serve({
    port: 3000,
    async fetch(req) {
        if (req.method === "POST" && req.url.endsWith("/upload")) {
            const formData = await req.formData();
            const file = formData.get("file");
            
            if (file) {
                // Save file
                await Bun.write(`uploads/${file.name}`, file);
                return new Response("File uploaded successfully");
            }
            
            return new Response("No file provided", { status: 400 });
        }
        
        return new Response("Not found", { status: 404 });
    }
});
```

---

## Best Practices

### DO (Lakukan)

1. **Use async/await untuk file operations**
   ```javascript
   const text = await file.text();
   ```

2. **Check file existence before read**
   ```javascript
   if (await file.exists()) {
       const content = await file.text();
   }
   ```

3. **Handle errors**
   ```javascript
   try {
       const data = await Bun.file("data.json").json();
   } catch (error) {
       console.error("Error reading file:", error);
   }
   ```

4. **Use appropriate file methods**
   ```javascript
   // For text
   await file.text()
   
   // For JSON
   await file.json()
   
   // For binary
   await file.arrayBuffer()
   ```

### DON'T (Jangan)

1. **Jangan read large files tanpa streaming**
   ```javascript
   // Bad untuk file besar
   const largeFile = await Bun.file("large.txt").text();
   
   // Good
   const stream = Bun.file("large.txt").stream();
   ```

2. **Jangan lupa error handling**

3. **Jangan hardcode paths**
   ```javascript
   // Bad
   Bun.file("/home/user/data.txt")
   
   // Good
   Bun.file(import.meta.dir + "/data.txt")
   ```

---

## Rangkuman

- Bun.file() untuk file operations (sangat cepat)
- Read files: .text(), .json(), .arrayBuffer()
- Write files: Bun.write()
- Check existence dengan .exists()
- Stream untuk large files
- Use import.meta.dir untuk paths
- Handle errors dengan try/catch
- Bun's file API lebih cepat dari Node.js fs

---

**Sebelumnya:** [09. Objects](09-objects.md)  
**Selanjutnya:** [11. HTTP Server dengan Bun](11-http-server.md)

