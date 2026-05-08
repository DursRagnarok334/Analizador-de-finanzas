# Finanzas Personales

App web de gestión financiera personal. Una sola página HTML, sin frameworks, con Firebase Firestore para persistencia en la nube y análisis de extractos bancarios mediante IA.

## Funcionalidades

- Ingresos: salarios, comisiones, ingresos pasivos, freelance
- Gastos: por categoría (fijo/variable), con historial filtrable
- Presupuesto mensual: límites por categoría con barra de progreso
- Ahorro: meta configurable, aportes por destino, progreso visual
- Deudas: registro con cuota, tasa, meses restantes estimados
- Fondo de emergencias: cobertura en meses de gastos fijos
- Análisis de extractos PDF: categorización automática con IA (Claude)
- Autosave en Firebase: sincronización 2 segundos después de cada cambio
- Modo local: funciona sin cuenta, datos en localStorage

## Configuración rápida

Ver [GUIA_CONFIGURACION.md](./GUIA_CONFIGURACION.md) para instrucciones completas paso a paso.

Los valores que debes configurar en `index.html`:

```javascript
// Bloque Firebase (Paso 5 de la guía)
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  ...
};

// API key Anthropic para análisis de PDF (Paso 6)
const ANTHROPIC_KEY = 'sk-ant-...';
```

## Tecnologías

- HTML + CSS + JavaScript vanilla
- Chart.js (gráficas de gastos e ingresos)
- Firebase Authentication (login con Google)
- Firebase Firestore (base de datos en la nube, plan gratuito)
- Anthropic API / Claude (análisis de extractos bancarios en PDF)
- GitHub Pages (hosting gratuito)

## Seguridad

- El repositorio debe mantenerse **privado** (contiene la API key de Anthropic)
- Los datos en Firestore están protegidos por reglas que solo permiten acceso al usuario autenticado
- Sin cuenta, los datos se guardan localmente en el navegador (localStorage)
