# 🏠 Inmobiliaria Web — Guía de configuración

Este sitio usa **Firebase** como base de datos en la nube y **GitHub Pages** para el hosting gratuito.

---

## PASO 1 — Crear proyecto en Firebase

1. Andá a **https://console.firebase.google.com**
2. Hacé clic en **"Agregar proyecto"**
3. Poné un nombre (ej: `mi-inmobiliaria`)
4. Desactivá Google Analytics si querés (no es necesario) → **Crear proyecto**

---

## PASO 2 — Activar Realtime Database

1. En el menú izquierdo → **Compilación → Realtime Database**
2. Clic en **"Crear base de datos"**
3. Elegí ubicación → **Estados Unidos (us-central1)**
4. Seleccioná **"Empezar en modo de prueba"** → Habilitar

> ⚠️ El modo de prueba dura 30 días. Después tenés que configurar las reglas de seguridad (ver PASO 6).

---

## PASO 3 — Activar Storage (para las fotos)

1. En el menú izquierdo → **Compilación → Storage**
2. Clic en **"Comenzar"**
3. Modo de prueba → **Siguiente** → **Listo**

---

## PASO 4 — Obtener las credenciales del proyecto

1. En Firebase, hacé clic en el **ícono de engranaje ⚙** → **Configuración del proyecto**
2. Bajá hasta **"Tus apps"** → clic en **`</>`** (agregar app web)
3. Poné un nombre (ej: `web`) → **Registrar app**
4. Vas a ver un bloque de código con `firebaseConfig`. **Copiá esos valores.**

---

## PASO 5 — Pegar las credenciales en el código

Abrí el archivo `js/firebase-config.js` y reemplazá cada valor:

```js
const firebaseConfig = {
  apiKey:            "AIzaSy...",         // ← tu valor
  authDomain:        "mi-inmo.firebaseapp.com",
  databaseURL:       "https://mi-inmo-default-rtdb.firebaseio.com",
  projectId:         "mi-inmo",
  storageBucket:     "mi-inmo.appspot.com",
  messagingSenderId: "123456789",
  appId:             "1:123456789:web:abc123"
};
```

---

## PASO 6 — Reglas de seguridad (importante antes de publicar)

### Realtime Database
En Firebase Console → Realtime Database → **Reglas**, pegá esto:

```json
{
  "rules": {
    "propiedades": {
      ".read": true,
      ".write": false
    },
    "config": {
      ".read": true,
      ".write": false
    },
    "meta": {
      ".read": false,
      ".write": false
    }
  }
}
```

> Esto permite que cualquier visitante **lea** las propiedades, pero **nadie puede escribir** directamente desde el navegador (solo el panel admin usa las credenciales del proyecto, que están en tu código).
>
> ⚠️ Para mayor seguridad en producción, considerá agregar Firebase Authentication para el panel admin.

### Storage
En Firebase Console → Storage → **Reglas**:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read: if true;
      allow write: if true;  // Cambiar a false después de cargar las fotos iniciales
    }
  }
}
```

---

## PASO 7 — Subir a GitHub

1. Creá un repositorio en **https://github.com/new**
   - Nombre: `inmobiliaria` (o el que prefieras)
   - Visibilidad: **Public**
2. En VS Code, abrí la carpeta del proyecto
3. Abrí la terminal integrada (**Ctrl + `**) y ejecutá:

```bash
git init
git add .
git commit -m "primer commit"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/inmobiliaria.git
git push -u origin main
```

---

## PASO 8 — Activar GitHub Pages

1. En tu repositorio de GitHub → **Settings** → **Pages**
2. En "Source" → seleccioná **"Deploy from a branch"**
3. Branch: **main** / Folder: **/ (root)** → **Save**
4. En unos minutos tu sitio va a estar en:
   `https://TU_USUARIO.github.io/inmobiliaria/`

---

## PASO 9 — Para actualizar el sitio

Cada vez que modificás algo en VS Code:

```bash
git add .
git commit -m "descripción del cambio"
git push
```

GitHub Pages se actualiza automáticamente en 1-2 minutos.

---

## Estructura del proyecto

```
inmobiliaria/
├── index.html          ← Sitio principal
├── js/
│   ├── firebase-config.js  ← ⚙ Tus credenciales Firebase (editar este)
│   └── db.js               ← Lógica de base de datos
└── README.md
```

---

## Contraseña del panel admin

La contraseña inicial es: **`admin123`**

Podés cambiarla desde el panel de administración (botón "Admin" en el header → pestaña 🔑 Contraseña).

---

## Extensiones recomendadas para VS Code

- **Live Server** — para previsualizar en local
- **GitHub Copilot** — ayuda con el código (opcional)
- **GitLens** — historial de cambios

Para previsualizar en local, instalá Live Server, abrí `index.html` y hacé clic en **"Go Live"** en la barra inferior de VS Code.
