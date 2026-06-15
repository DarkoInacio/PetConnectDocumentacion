# Stack tecnológico

## Resumen

| Capa | Tecnologías principales |
|------|-------------------------|
| Frontend | React 19, Vite 8, React Router 7, Tailwind CSS 4, Axios |
| Backend | Node.js ≥18, Express 4, Mongoose 8 |
| Base de datos | MongoDB (Atlas en producción) |
| Auth | JWT (`jsonwebtoken`), bcrypt |
| PWA | `vite-plugin-pwa`, service worker |
| Mapas | Leaflet + react-leaflet |
| UI | Radix UI, Lucide icons, CVA |
| IA (chat) | OpenAI API (solo backend) |
| Email | Nodemailer |
| PDF ficha médica | PDFKit |
| Imágenes | Sharp, Multer |
| Tareas programadas | node-cron (recordatorios 24h) |
| Pruebas backend | Jest, Supertest, Newman, mongodb-memory-server |
| Pruebas frontend | Vitest, Testing Library, Playwright |

---

## Frontend — PetConnect

| Área | Detalle |
|------|---------|
| **Runtime** | ES modules (`"type": "module"`) |
| **Bundler** | Vite 8 con `@vitejs/plugin-react` |
| **Estilos** | Tailwind CSS 4 (`@tailwindcss/vite`) |
| **Routing** | `react-router-dom` v7 |
| **HTTP** | Axios (`src/services/api.js`) — base URL desde `VITE_API_BASE_URL` |
| **Estado auth** | React Context (`AuthProvider`, `useAuth`) |
| **PWA** | Manifest, iconos generados en build (`scripts/generate-pwa-icons.mjs`) |
| **Accesibilidad** | Skip link, roles ARIA en componentes clave |
| **CI** | GitHub Actions — `.github/workflows/frontend-tests.yml` |

---

## Backend — PetConnectBackend

| Área | Detalle |
|------|---------|
| **Runtime** | CommonJS (`"type": "commonjs"`) |
| **Servidor** | Express + `helmet`, `cors`, `morgan` |
| **Rate limit** | `express-rate-limit` — 100 req/15 min en producción; desactivado en desarrollo |
| **ODM** | Mongoose (modelos en `src/models/`) |
| **Validación** | Controladores + validators puntuales (`src/validators/`) |
| **Uploads** | Multer + almacenamiento en `src/uploads/` (efímero en Render free tier) |
| **Jobs** | `src/jobs/appointmentReminders.job.js` |
| **Geocodificación** | Nominatim (OpenStreetMap) para direcciones de proveedores |
| **CI** | GitHub Actions — Jest + Newman Smoke — `.github/workflows/backend-tests.yml` |

---

## Infraestructura y herramientas

| Herramienta | Uso |
|-------------|-----|
| **Git / GitHub** | Control de versiones; flujo tipo GitFlow mencionado en documento académico |
| **Postman / Newman** | Colección API en ambos repos (`postman/`); QA automatizado en backend |
| **MongoDB Atlas** | Base de datos cloud |
| **Vercel** | Hosting frontend |
| **Render** | Hosting backend (`render.yaml`) |
| **Mailtrap / SMTP** | Pruebas de correo en desarrollo |

---

## Dependencias de desarrollo destacadas

- **Backend:** nodemon (hot reload), newman (CLI Postman).
- **Frontend:** ESLint 9, jsdom (Vitest), Playwright (e2e).

Para versiones exactas, consultar `package.json` de cada repositorio.
