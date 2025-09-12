Berikut struktur project **ES Modules (ESM)** yang jelas, termasuk penjelasan file utama dan cara mengimpor library:

---

## 📂 **Struktur Project (ES Modules)**
```markdown
my-project/
├── node_modules/          # Dependencies (jika ada)
├── src/
│   ├── lib/               # Kode library internal
│   │   ├── math.js        # Modul matematika
│   │   └── greet.js       # Modul sapaan
│   └── main.js           # File utama (entry point)
├── index.html            # Untuk browser (opsional)
├── package.json          # Konfigurasi project
└── vite.config.js        # Bundler (opsional, contoh pakai Vite)
```

---

## 📜 **1. File Library (`src/lib/math.js`)**
```javascript
// Named exports
export function add(a, b) {
  return a + b;
}

export const PI = 3.14;

// Default export
export default function multiply(a, b) {
  return a * b;
}
```

---

## 📜 **2. File Library (`src/lib/greet.js`)**
```javascript
// Named export
export function hello(name) {
  return `Hello, ${name}!`;
}

// Default export
export default function welcome() {
  return "Welcome to my app!";
}
```

---

## 📜 **3. File Utama (`src/main.js`)**
File ini adalah **entry point** yang mengimpor dan menggunakan library.  
```javascript
// Impor dari library internal
import multiply, { add, PI } from './lib/math.js';
import welcome, { hello } from './lib/greet.js';

// Gunakan fungsi library
console.log(add(2, 3));        // 5
console.log(multiply(2, 3));    // 6
console.log(hello("Alice"));    // "Hello, Alice!"
console.log(PI);                // 3.14

// Ekspor jika perlu (untuk dipakai di file lain)
export { add, multiply, hello };
```

---

## 🌐 **4. Contoh Penggunaan di Browser (`index.html`)**
Jika ingin dipakai di browser, gunakan `<script type="module">`:  
```html
<!DOCTYPE html>
<html>
<head>
  <title>ES Modules Demo</title>
</head>
<body>
  <script type="module" src="src/main.js"></script>
</body>
</html>
```

---

## ⚙️ **5. Konfigurasi (`package.json`)**
Pastikan project menggunakan ES Modules:  
```json
{
  "name": "my-project",
  "type": "module",  // Penting! Aktifkan ES Modules
  "scripts": {
    "dev": "vite",   // Contoh: Dev server dengan Vite
    "build": "vite build"
  },
  "dependencies": {
    "vite": "^4.0.0" // Opsional: Bundler modern
  }
}
```

---

## 🔧 **6. Setup Bundler (Opsional)**
Jika menggunakan **Vite** (untuk optimasi):  
1. Install Vite:  
   ```bash
   npm install vite --save-dev
   ```
2. Jalankan dev server:  
   ```bash
   npm run dev
   ```
   Akses di `http://localhost:5173`.

---

## 📌 **Poin Penting**
1. **File Utama**:  
   - Biasanya `main.js`/`index.js` di folder `src/`.  
   - Bertugas mengimpor modul lain dan menjalankan aplikasi.  

2. **Aturan ES Modules**:  
   - Ekstensi file **harus jelas** (`.js` atau `.mjs`).  
   - Path import **harus lengkap** (`./lib/math.js`, bukan `./lib/math`).  

3. **Untuk Browser**:  
   - Gunakan `<script type="module">`.  
   - Jika ada relative path (e.g., `./lib/math.js`), pastikan struktur folder sesuai.  

4. **Untuk Node.js**:  
   - Tambahkan `"type": "module"` di `package.json`.  
   - Atau gunakan ekstensi `.mjs`.  

---

### 💡 **Contoh Import dari `node_modules`**
Jika mengimpor library eksternal (e.g., `lodash`):  
```javascript
import _ from 'lodash'; // Auto-resolve dari node_modules

console.log(_.capitalize("hello")); // "Hello"
```

---

## 🎯 **Bagan Alir Impor**
```
src/main.js
  │
  ├── import { add } from './lib/math.js' → src/lib/math.js
  │
  └── import welcome from './lib/greet.js' → src/lib/greet.js
```

Semoga membantu! 😊