# Guía de Configuración — Finanzas Personales
## Stack: Firebase Firestore + GitHub Pages

---

## Paso 1: Crear proyecto en Firebase (gratis, sin tarjeta)

1. Ve a https://console.firebase.google.com
2. Haz clic en **"Agregar proyecto"**
3. Nombre del proyecto: `finanzas-personales`
4. Desactiva Google Analytics (no es necesario) → **Crear proyecto**
5. Espera ~30 segundos a que se cree

---

## Paso 2: Activar autenticación con Google

1. En el menú izquierdo → **Build → Authentication**
2. Clic en **"Comenzar"**
3. Pestaña **"Sign-in method"** → clic en **Google**
4. Activa el interruptor → ingresa un email de soporte → **Guardar**

---

## Paso 3: Crear la base de datos Firestore

1. En el menú izquierdo → **Build → Firestore Database**
2. Clic en **"Crear base de datos"**
3. Selecciona **"Iniciar en modo de producción"** → Siguiente
4. Elige la región más cercana (ej: `us-east1`) → **Listo**

### Configurar reglas de seguridad

Una vez creada la base de datos, ve a la pestaña **"Reglas"** y reemplaza el contenido con:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /usuarios/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

Clic en **"Publicar"**. Esto garantiza que solo tú puedes leer y escribir tus datos.

---

## Paso 4: Obtener las credenciales de tu app

1. Firebase Console → ícono de engranaje (⚙) → **Configuración del proyecto**
2. Baja hasta **"Tus apps"** → clic en el ícono **`</>`** (Web)
3. Nombre de la app: `finanzas-web` → clic en **"Registrar app"**
4. Verás un bloque con tus credenciales. Cópialas.

---

## Paso 5: Pegar las credenciales en index.html

Abre `index.html` y busca esta sección al inicio:

```javascript
const firebaseConfig = {
  apiKey:            "TU_API_KEY",
  authDomain:        "TU_PROJECT_ID.firebaseapp.com",
  projectId:         "TU_PROJECT_ID",
  storageBucket:     "TU_PROJECT_ID.appspot.com",
  messagingSenderId: "TU_MESSAGING_SENDER_ID",
  appId:             "TU_APP_ID"
};
```

Reemplaza cada valor con el que copiaste de Firebase.

---

## Paso 6: API key de Anthropic (análisis de extractos PDF)

1. Ve a https://console.anthropic.com
2. Menú → **"API Keys"** → **"Create Key"**
3. Copia la key (empieza con `sk-ant-...`)
4. En `index.html` busca:

```javascript
const ANTHROPIC_KEY = 'TU_API_KEY_ANTHROPIC';
```

Reemplaza con tu key real.

El repositorio debe ser **privado** en GitHub porque contiene esta key.

---

## Paso 7: Agregar dominio autorizado en Firebase

Para que el login funcione desde GitHub Pages:

1. Firebase Console → **Authentication → Settings → Authorized domains**
2. Clic en **"Agregar dominio"**
3. Agrega: `TU_USUARIO.github.io`
4. Guardar

---

## Paso 8: Subir a GitHub y activar GitHub Pages

```bash
git init
git add .
git commit -m "Primer commit: app de finanzas personales"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/finanzas-personales.git
git push -u origin main
```

Luego en tu repo → **Settings → Pages** → Source: `main` / `root` → Save.

La app quedará en: `https://TU_USUARIO.github.io/finanzas-personales`

---

## Paso 9: Probar localmente

Si tienes VS Code, instala **Live Server**:
1. Clic derecho en `index.html` → "Open with Live Server"
2. Se abre en `http://127.0.0.1:5500`
3. Agrega `127.0.0.1` a los dominios autorizados en Firebase Auth

---

## Estructura del proyecto

```
finanzas-personales/
├── index.html
├── GUIA_CONFIGURACION.md
└── README.md
```

---

## Dónde se guardan tus datos

Los datos se guardan en Firestore con esta estructura:

```
usuarios/
  {tu_uid}/
    datos/
      finanzas → { ingresos, gastos, ahorros, deudas, ... }
```

Solo tú, autenticado con tu cuenta Google, puedes acceder a ellos.

---

## Actualizar la app después de cambios

```bash
git add index.html
git commit -m "Descripción del cambio"
git push
```

---

## Problemas comunes

| Problema | Solución |
|---|---|
| "auth/unauthorized-domain" | Agrega el dominio en Firebase Auth → Authorized domains |
| "permission-denied" en Firestore | Revisa las reglas de seguridad del Paso 3 |
| El popup de Google se bloquea | Permite popups para tu dominio en el navegador |
| Los datos no se cargan | Abre F12 → Console y verifica el error exacto |
| PDF no se analiza | Verifica que la API key de Anthropic sea válida |
