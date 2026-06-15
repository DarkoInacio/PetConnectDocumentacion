# Instalación y ejecución local

Guía para levantar **PetConnectBackend** y **PetConnect** en una máquina de desarrollo (Windows, macOS o Linux).

## Requisitos previos

| Herramienta | Versión mínima |
|-------------|----------------|
| **Node.js** | 18 LTS o superior |
| **npm** | Incluido con Node |
| **Git** | Cualquier versión reciente |
| **MongoDB** | Atlas (recomendado) o instancia local |

Opcional: **Postman** o **Newman** para pruebas API; **Playwright** se instala con el frontend para e2e.

---

## 1. Clonar repositorios

Clonar en carpetas hermanas (o la estructura que prefiera el equipo):

```
Documents/GitHub/
├── PetConnect/
├── PetConnectBackend/
└── PetConnectDocumentacion/
```

---

## 2. Backend (PetConnectBackend)

### Configuración

```bash
cd PetConnectBackend
npm install
cp .env.example .env   # Windows: copy .env.example .env
```

Editar `.env` con valores reales. Variables **obligatorias** para arrancar:

| Variable | Ejemplo | Descripción |
|----------|---------|-------------|
| `MONGODB_URI` | `mongodb+srv://...` | Conexión MongoDB |
| `JWT_SECRET` | cadena larga aleatoria | Firma de tokens |
| `CLIENT_URL` | `http://localhost:5173` | Origen CORS del frontend |

Variables **recomendadas**:

| Variable | Descripción |
|----------|-------------|
| `MAIL_*` | SMTP para forgot password y notificaciones |
| `OPENAI_API_KEY` | Chat Vetto (sin ella el chat puede fallar o usar fallback) |
| `ADMIN_SEED_EMAIL` / `ADMIN_SEED_PASSWORD` | Crear admin local con `npm run seed:admin` |

### Arrancar

```bash
npm run dev
```

- API en **http://localhost:3000**
- Health check: **http://localhost:3000/health** → `{ "status": "ok" }`
- Prefijo REST: **http://localhost:3000/api**

`nodemon` recarga al cambiar código en `src/`; la carpeta `postman/` está ignorada para no reiniciar durante seeds o Newman.

### Seeds útiles

```bash
npm run seed:admin    # Usuario administrador
npm run seed:smoke    # Datos smoke + env Postman CI
npm run seed:qa       # Datos QA TCP-001 + env Postman QA
```

---

## 3. Frontend (PetConnect)

### Configuración

```bash
cd PetConnect
npm install
cp .env.example .env
```

Contenido típico de `.env`:

```env
VITE_API_BASE_URL=http://localhost:3000/api
```

Si Vite usa el puerto **5174** (5173 ocupado), añadir ese origen en `CLIENT_URL` del backend.

### Arrancar

```bash
npm run dev
```

- App en **http://localhost:5173** (o el puerto que indique Vite)
- La PWA y el service worker se comportan mejor en **build + preview** que en dev puro

### Build de producción (local)

```bash
npm run build
npm run preview
```

---

## 4. Orden recomendado al desarrollar

1. Levantar **MongoDB** (Atlas accesible desde tu IP).
2. Levantar **backend** (`npm run dev` en PetConnectBackend).
3. Verificar `/health`.
4. Levantar **frontend** (`npm run dev` en PetConnect).
5. Registrar usuario o usar seeds (`seed:admin`, `seed:qa`).

---

## 5. Pruebas rápidas sin UI

Con el backend en marcha:

```bash
# Smoke API (Newman)
cd PetConnectBackend
npm run test:smoke:full

# QA completo TCP-001 (seed + 14 carpetas Newman)
npm run test:qa:full
```

Ver [pruebas-y-calidad.md](./pruebas-y-calidad.md).

---

## 6. Problemas frecuentes

| Problema | Solución |
|----------|----------|
| CORS bloqueado | Alinear `CLIENT_URL` (backend) con la URL exacta del frontend |
| `ECONNREFUSED` en frontend | Backend no está levantado o `VITE_API_BASE_URL` incorrecta |
| Login admin falla | `npm run seed:admin` o `FORCE_ADMIN_PASSWORD=1` en `.env` |
| Puerto 5173 en uso | Vite elige 5174; actualizar `CLIENT_URL` en backend |
| Newman reinicia API | Usar `nodemon.json` actualizado (ignora `postman/`) |
| Rate limit 429 en QA | Reiniciar backend en modo desarrollo (sin límite en `/api`) |

---

## 7. Producción (referencia)

| Servicio | Comando / config |
|----------|------------------|
| Backend | `npm start` o `npm run start:prod`; variables en panel Render |
| Frontend | `npm run build`; despliegue en Vercel con `VITE_API_BASE_URL` apuntando al backend |

Detalle de variables de producción: `.env.example` del backend y dashboard de Render/Vercel.
