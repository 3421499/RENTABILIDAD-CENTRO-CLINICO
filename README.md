# Mi Centro — Utilidad

App PWA para control de ingresos y gastos mensuales.
Login con Google, datos sincronizados en Firestore, instalable en celular y PC.

---

## Setup en 5 pasos

### 1. Crear proyecto en Firebase

1. Ve a [console.firebase.google.com](https://console.firebase.google.com)
2. **Crear proyecto** → ponle nombre (ej. `mi-centro-utilidad`)
3. Activa **Google Analytics** si quieres (opcional)

### 2. Habilitar servicios

En la consola de Firebase:

- **Authentication** → Sign-in method → habilitar **Google**
- **Firestore Database** → Crear base de datos → modo **producción**
- **Hosting** → Comenzar (solo para activarlo)

### 3. Obtener configuración

En la consola → ⚙️ Configuración del proyecto → Tus apps → Agregar app **Web**:

Copia el objeto `firebaseConfig` que aparece y reemplázalo en `public/index.html`:

```js
const firebaseConfig = {
  apiKey:            "...",
  authDomain:        "...",
  projectId:         "...",
  storageBucket:     "...",
  messagingSenderId: "...",
  appId:             "..."
};
```

También actualiza `.firebaserc`:
```json
{ "projects": { "default": "TU_PROJECT_ID" } }
```

### 4. Subir a GitHub

```bash
git init
git add .
git commit -m "init: Mi Centro app"
git remote add origin https://github.com/TU_USUARIO/mi-centro.git
git push -u origin main
```

### 5. Deploy a Firebase Hosting

**Opción A — desde terminal (una vez):**
```bash
npm install -g firebase-tools
firebase login
firebase deploy
```
Tu app quedará en `https://TU_PROJECT_ID.web.app`

**Opción B — deploy automático con GitHub Actions (recomendado):**

1. En la consola Firebase → Hosting → GitHub Actions → conecta tu repo
2. Firebase te genera automáticamente el secret `FIREBASE_SERVICE_ACCOUNT` en tu repo de GitHub
3. Agrega `FIREBASE_PROJECT_ID` en GitHub → Settings → Secrets → `TU_PROJECT_ID`
4. Cada `git push` a `main` despliega automáticamente ✓

---

## Agregar la app al celular

**iPhone (Safari):**
1. Abre `https://TU_PROJECT_ID.web.app` en Safari
2. Botón compartir → "Agregar a pantalla de inicio"

**Android (Chrome):**
1. Abre la URL en Chrome
2. Chrome pregunta automáticamente "¿Instalar app?"

---

## Estructura del proyecto

```
mi-centro/
├── public/
│   ├── index.html      ← App completa
│   ├── sw.js           ← Service worker (offline)
│   └── manifest.json   ← PWA manifest
├── .github/
│   └── workflows/
│       └── deploy.yml  ← Auto-deploy en push
├── firebase.json       ← Config hosting
├── firestore.rules     ← Seguridad Firestore
├── firestore.indexes.json
└── .firebaserc         ← Proyecto Firebase
```

## Estructura de datos en Firestore

```
users/
  {uid}/
    config/
      categories → { ingresos: [...], gastos: [...] }
    months/
      2025_06 → { ingresos: { Efectivo: [...] }, gastos: { Sueldos: [...] } }
      2025_07 → { ... }
```
