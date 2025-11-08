# 2. Setup Environment Bun

## Tools yang Diperlukan

### 1. Bun Runtime

Install Bun sebagai JavaScript runtime.

### 2. Code Editor

**Visual Studio Code (Recommended)**
- Download: https://code.visualstudio.com/
- Gratis dan powerful
- Banyak extensions

**Alternatif:**
- Sublime Text
- Atom
- WebStorm (IDE berbayar)
- Notepad++

---

## Instalasi Bun

### macOS dan Linux

Menggunakan curl:

```bash
curl -fsSL https://bun.sh/install | bash
```

Atau dengan Homebrew (macOS):

```bash
brew tap oven-sh/bun
brew install bun
```

### Windows

Menggunakan PowerShell:

```powershell
powershell -c "irm bun.sh/install.ps1 | iex"
```

Atau download installer dari https://bun.sh/

### Verifikasi Instalasi

```bash
bun --version
# Output: 1.x.x
```

### Upgrade Bun

```bash
bun upgrade
```

### Uninstall Bun

```bash
# macOS/Linux
rm -rf ~/.bun

# Windows
# Hapus folder C:\Users\YourName\.bun
```

---

## Setup Visual Studio Code

### Install VS Code

1. Download dari https://code.visualstudio.com/
2. Install sesuai sistem operasi
3. Buka VS Code

### Extensions yang Berguna

Install extensions melalui Extensions panel (`Ctrl + Shift + X`):

**1. Prettier**
- Author: Prettier
- Fungsi: Code formatter

**2. ESLint**
- Author: Microsoft
- Fungsi: Linter untuk JavaScript

**3. JavaScript (ES6) code snippets**
- Author: charalampos karypidis
- Fungsi: Code snippets

**4. Path Intellisense**
- Author: Christian Kohler
- Fungsi: Autocomplete file paths

**5. Bun for Visual Studio Code**
- Author: Oven
- Fungsi: Bun language support

**6. Thunder Client** (Optional)
- Author: Ranga Vadhineni
- Fungsi: API testing tool

### VS Code Settings

Buka settings (`Ctrl + ,`) dan set:

```json
{
    "editor.fontSize": 14,
    "editor.tabSize": 2,
    "editor.formatOnSave": true,
    "editor.wordWrap": "on",
    "files.autoSave": "afterDelay",
    "emmet.includeLanguages": {
        "javascript": "javascriptreact"
    }
}
```

### Keyboard Shortcuts Penting

| Shortcut | Fungsi |
|----------|--------|
| `Ctrl + /` | Toggle comment |
| `Alt + Up/Down` | Move line up/down |
| `Shift + Alt + Down` | Copy line down |
| `Ctrl + D` | Select next occurrence |
| `Ctrl + Shift + K` | Delete line |
| `Ctrl + Shift + L` | Select all occurrences |
| `F2` | Rename symbol |
| `Ctrl + Space` | Trigger suggestions |

---

## Project Structure untuk Bun

### Basic Bun Project

```
my-project/
├── package.json
├── src/
│   ├── index.js
│   ├── utils/
│   │   └── helpers.js
│   └── config/
│       └── config.js
├── tests/
│   └── index.test.js
├── .env
└── README.md
```

### Setup Bun Project

**1. Buat folder project:**

```bash
mkdir my-first-bun-project
cd my-first-bun-project
```

**2. Initialize Bun project:**

```bash
bun init

# Atau manual
bun init -y  # Skip questions
```

Ini akan membuat:
- `package.json`
- `tsconfig.json` (if TypeScript)
- `README.md`
- `.gitignore`

**3. package.json - Basic:**

```json
{
  "name": "my-first-bun-project",
  "version": "1.0.0",
  "module": "index.js",
  "type": "module",
  "scripts": {
    "start": "bun run index.js",
    "dev": "bun --watch index.js",
    "test": "bun test"
  },
  "dependencies": {},
  "devDependencies": {
    "bun-types": "latest"
  }
}
```

**4. index.js - Basic Script:**

```javascript
// index.js
console.log('Hello from Bun!');
console.log('Bun version:', Bun.version);

function greet(name) {
    return `Welcome to Bun, ${name}!`;
}

console.log(greet('Developer'));
```

**5. Run project:**

```bash
bun index.js
# atau
bun run start
```

---

## Running JavaScript dengan Bun

### Method 1: Direct Run

```bash
# Run file
bun index.js

# Atau dengan explicit run command
bun run index.js
```

### Method 2: Watch Mode (Auto-reload)

```bash
# Auto-restart saat file berubah
bun --watch index.js

# Atau define di package.json
# "dev": "bun --watch index.js"
bun run dev
```

### Method 3: Run dengan Scripts

Di `package.json`:

```json
{
  "scripts": {
    "start": "bun run index.js",
    "dev": "bun --watch index.js",
    "test": "bun test"
  }
}
```

Run:

```bash
bun start
bun run dev
bun test
```

### Method 4: Run TypeScript (Native)

```bash
# Bun support TypeScript tanpa setup
bun index.ts
```

---

## Bun CLI Commands

### Basic Commands

```bash
# Run file
bun index.js

# Run with watch mode
bun --watch index.js

# Run script dari package.json
bun run start

# Install dependencies
bun install

# Add package
bun add express
bun add --dev @types/express

# Remove package
bun remove express

# Update dependencies
bun update

# Run tests
bun test
```

### Package Management

```bash
# Initialize project
bun init

# Install all dependencies
bun install

# Install specific package
bun add package-name

# Install dev dependency
bun add --dev package-name

# Install global package
bun add --global package-name

# Remove package
bun remove package-name

# Update all packages
bun update
```

### Bun Commands

```bash
# Check version
bun --version

# Upgrade Bun
bun upgrade

# Create executable
bun build ./index.ts --compile --outfile myapp

# Run TypeScript
bun index.ts

# Run tests
bun test

# Run with environment variables
DATABASE_URL=postgres://... bun index.js
```

---

## Bun Package Manager

Bun memiliki package manager built-in yang sangat cepat.

### Install Packages

```bash
# Install all dependencies dari package.json
bun install

# Install specific package
bun add express
bun add prisma zod

# Install dev dependency
bun add --dev @types/express
bun add -d eslint prettier

# Install exact version
bun add express@4.18.0

# Install global
bun add --global nodemon
```

### Remove Packages

```bash
# Remove package
bun remove express

# Remove multiple packages
bun remove package1 package2
```

### Update Packages

```bash
# Update all packages
bun update

# Update specific package
bun update express
```

### Lock File

Bun generates `bun.lockb` (binary lockfile) untuk faster install.

```bash
# Install dari lockfile
bun install --frozen-lockfile
```

---

## Git untuk Version Control

### Setup Git

**Install Git:**
- Download: https://git-scm.com/
- Verify: `git --version`

**Configure Git:**

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Initialize Repository

```bash
cd my-project
git init
```

### .gitignore File

Buat `.gitignore`:

```
# Dependencies
node_modules/
npm-debug.log*

# Environment variables
.env

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db

# Build
dist/
build/
*.min.js
```

### Basic Git Workflow

```bash
# Check status
git status

# Add files
git add .

# Commit
git commit -m "Initial commit"

# Create branch
git branch feature-name

# Switch branch
git checkout feature-name
# atau
git switch feature-name

# Push to remote
git push origin main
```

---

## Testing dengan Bun

Bun memiliki test runner built-in yang sangat cepat.

### Basic Test

```javascript
// math.test.js
import { expect, test } from "bun:test";

function add(a, b) {
    return a + b;
}

test("add function", () => {
    expect(add(2, 3)).toBe(5);
    expect(add(-1, 1)).toBe(0);
});

test("add with strings (coercion)", () => {
    expect(add('2', '3')).toBe('23');
});
```

### Run Tests

```bash
# Run all tests
bun test

# Watch mode
bun test --watch

# Specific file
bun test math.test.js
```

### Test Structure

```javascript
// calculator.test.js
import { describe, test, expect, beforeEach } from "bun:test";

describe("Calculator", () => {
    let calculator;
    
    beforeEach(() => {
        calculator = { result: 0 };
    });
    
    test("should add numbers", () => {
        calculator.result = 5 + 3;
        expect(calculator.result).toBe(8);
    });
    
    test("should multiply numbers", () => {
        calculator.result = 5 * 3;
        expect(calculator.result).toBe(15);
    });
});
```

---

## Environment Variables

Bun support `.env` files secara native.

### Create .env File

```bash
# .env
DATABASE_URL=postgres://localhost/mydb
API_KEY=your_api_key_here
PORT=3000
NODE_ENV=development
```

### Access di Code

```javascript
// index.js
console.log('Database URL:', process.env.DATABASE_URL);
console.log('API Key:', process.env.API_KEY);
console.log('Port:', process.env.PORT);

// Atau dengan Bun.env
console.log('Port:', Bun.env.PORT);
```

### Multiple .env Files

```bash
# .env (default)
PORT=3000

# .env.development
PORT=3001

# .env.production
PORT=8080
```

Load specific env file:

```bash
bun --env-file=.env.production index.js
```

---

## Troubleshooting

### Problem: "bun: command not found"

**Solution:**
1. Restart terminal after install
2. Add to PATH:
   ```bash
   # Add ke ~/.zshrc atau ~/.bashrc
   export BUN_INSTALL="$HOME/.bun"
   export PATH="$BUN_INSTALL/bin:$PATH"
   ```
3. Source file: `source ~/.zshrc`
4. Reinstall Bun jika perlu

### Problem: Import/Export errors

**Solution:**
1. Pastikan menggunakan `.js` extension di import
   ```javascript
   // Good
   import { func } from './utils.js';
   
   // Bad
   import { func } from './utils';
   ```

2. Set `"type": "module"` di package.json

### Problem: Package install gagal

**Solution:**
1. Clear cache: `bun pm cache rm`
2. Delete `node_modules` dan `bun.lockb`
3. Run `bun install` lagi

### Problem: Permission denied

**Solution (Linux/macOS):**
```bash
sudo chown -R $USER ~/.bun
```

---

## Best Practices

### Project Organization

- Use src/ folder untuk source code
- Use tests/ folder untuk test files
- Gunakan clear folder structure
- Naming convention konsisten
- Comment code yang complex

### Development Workflow dengan Bun

1. Initialize dengan `bun init`
2. Install dependencies dengan `bun add`
3. Write code dengan watch mode: `bun --watch index.js`
4. Test dengan `bun test`
5. Commit changes dengan Git
6. Deploy atau build

### Code Quality

- Use ES6 modules (default di Bun)
- Write tests untuk functions penting
- Use TypeScript untuk type safety (Bun support native)
- Follow naming conventions
- Keep functions small
- Avoid global variables

### Performance

- Bun sudah sangat cepat by default
- Use built-in APIs (Bun.file, Bun.serve)
- Leverage Bun's native performance

---

## Rangkuman

- Install Bun dari https://bun.sh
- Bun adalah all-in-one tool: runtime, bundler, transpiler, package manager
- Initialize project dengan `bun init`
- Run script dengan `bun file.js` atau `bun --watch file.js`
- Install packages dengan `bun add package-name`
- Bun support TypeScript dan JSX tanpa konfigurasi
- Built-in test runner dengan `bun test`
- Native support untuk .env files
- ES6 modules adalah default
- 3-4x lebih cepat dari Node.js

---

**Sebelumnya:** [01. Pengenalan JavaScript](01-pengenalan-javascript.md)  
**Selanjutnya:** [03. Variabel dan Tipe Data](03-variabel-tipe-data.md)

