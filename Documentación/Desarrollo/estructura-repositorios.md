# Estructura de repositorios

Descripción general de carpetas en **PetConnect** (frontend) y **PetConnectBackend** (API). No lista cada archivo; sirve como mapa para orientarse.

---

## PetConnectBackend

```
PetConnectBackend/
├── src/
│   ├── server.js          # Entrada: conecta DB, jobs, listen
│   ├── app.js             # Express: middleware, rutas, /health
│   ├── config/            # DB, uploads, multer, mailer, apiScope
│   ├── routes/            # Routers por dominio (+ tests *.routes.test.js)
│   ├── controllers/       # Lógica HTTP por recurso
│   ├── models/            # Esquemas Mongoose
│   ├── services/          # Reglas de negocio reutilizables
│   ├── middlewares/       # auth, roles, errorHandler
│   ├── jobs/              # Cron (recordatorios citas)
│   ├── utils/             # JWT, email, geo, notificaciones
│   └── validators/        # Validación de payloads
├── scripts/               # Seeds, Newman runners, migraciones
├── postman/               # Colección + environments + README QA
├── test/                  # Helpers Jest (factories, setup)
├── test-cases/            # Casos smoke documentados
├── docs/                  # Notas técnicas puntuales del backend
├── .github/workflows/     # CI (Jest + Newman)
├── package.json
├── nodemon.json           # Ignora postman/ en dev
└── render.yaml            # Despliegue Render
```

### Capas en `src/`

| Capa | Responsabilidad |
|------|-----------------|
| **routes** | Define endpoints y middlewares por ruta |
| **controllers** | Parseo request, respuesta HTTP, delegación a services |
| **services** | Reglas de negocio, acceso a datos complejo |
| **models** | Persistencia MongoDB |
| **middlewares** | Autenticación JWT, autorización por rol |

### Scripts npm relevantes

Ver [instalacion-local.md](./instalacion-local.md) y [pruebas-y-calidad.md](./pruebas-y-calidad.md).

---

## PetConnect (frontend)

```
PetConnect/
├── src/
│   ├── main.jsx           # Entrada React
│   ├── App.jsx            # Rutas principales
│   ├── index.css          # Tailwind v4
│   ├── pages/             # Una página por ruta/feature
│   ├── components/        # UI reutilizable (layout, mapa, chat)
│   │   └── ui/            # Primitivos (button, input, card…)
│   ├── services/          # Cliente API (axios) por dominio
│   ├── context/           # AuthProvider, ThemeProvider
│   ├── hooks/             # useAuth, useOnlineStatus
│   ├── lib/               # Utilidades (cn, roles)
│   └── constants/         # Especies, timezone Chile, etc.
├── public/                # Assets estáticos, iconos PWA
├── e2e/                   # Playwright
├── postman/               # Generador colección (generate-postman.mjs)
├── docs/                  # QA TCP-001, notas internas
├── scripts/               # Iconos PWA
├── .github/workflows/     # CI frontend
├── TEST_PLAN.md           # Plan de pruebas maestro
├── vite.config.js
├── vercel.json
└── package.json
```

### Organización frontend

| Carpeta | Contenido típico |
|---------|------------------|
| **pages/** | Pantallas completas ligadas a rutas en `App.jsx` |
| **services/** | Funciones `async` que llaman a `/api/...` |
| **components/** | Piezas compartidas (header, mapa, chat flotante) |
| **context/** | Estado global de sesión (token, usuario) |

### Rutas principales (referencia)

| Ruta | Página / propósito |
|------|---------------------|
| `/` | Mapa de proveedores |
| `/explorar` | Búsqueda listada |
| `/login`, `/registro*` | Autenticación |
| `/cuenta/*` | Área dueño (reservas, perfil, mascotas) |
| `/mascotas/*` | CRUD y ficha médica |
| `/agendar` | Reservar cita clínica |
| `/proveedor/*` | Panel proveedor / vet |
| `/admin/*` | Panel administrador |

---

## PetConnectDocumentacion (este repo)

```
PetConnectDocumentacion/
├── Documentación/
│   ├── Desarrollo/        # ← Guías técnicas (Markdown)
│   ├── Informes/
│   ├── UML/
│   ├── MER/
│   ├── Wireframes/
│   └── Diagramas-Gantt/
├── Producto/
│   ├── Codigo-Fuente/     # Referencia a repos GitHub
│   ├── Librerias/
│   └── Scripts-Base-de-Datos/
├── Gestión/
│   ├── Definicion-del-Proyecto/   # PDF académico 1.1.2
│   └── Lista-de-Integrantes/
└── README.md
```

---

## Documentación viva en repos de código

| Tema | Ubicación |
|------|-----------|
| QA Newman TCP-001 | `PetConnectBackend/postman/README.md` |
| Plan de pruebas | `PetConnect/TEST_PLAN.md` |
| Integración frontend | `PetConnectBackend/docs/FRONTEND-INTEGRATION.md` |
| Agenda clínica | `PetConnectBackend/docs/AGENDA_PERSONAL_CLINICA.md` |
| Generador Postman | `PetConnect/postman/generate-postman.mjs` |

Estos archivos evolucionan con el código; esta carpeta **Desarrollo** resume lo esencial de forma estable para el equipo y evaluadores.
