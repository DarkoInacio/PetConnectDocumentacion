# API REST — resumen

Base URL en desarrollo: **`http://localhost:3000/api`**

Health check (sin prefijo `/api`): **`GET /health`**

Autenticación: header **`Authorization: Bearer <JWT>`** en rutas protegidas.

---

## Módulos montados

Prefijos definidos en `PetConnectBackend/src/routes/index.js`:

| Prefijo | Dominio |
|---------|---------|
| `/auth` | Registro, login, forgot/reset password |
| `/profile` | Perfil del usuario autenticado |
| `/pets` | CRUD mascotas, ficha médica, PDF, encounters (vista dueño) |
| `/proveedores` | Búsqueda, mapa, perfil público, solicitud walker/cuidador |
| `/appointments` | Citas, slots disponibles, confirmación, cancelación, reseñas por cita |
| `/bookings` | Vista unificada de reservas (dueño / proveedor) |
| `/provider/agenda` | Generación y gestión de slots |
| `/provider/clinic-services` | Servicios de clínica (duración, precio) |
| `/provider/reviews` | Panel de reseñas del proveedor |
| `/vet` | Pacientes vet, encounters clínicos, retracciones |
| `/reviews` | Edición y reporte de reseñas |
| `/chat` | Chatbot Vetto (sesión en memoria) |
| `/admin` | Proveedores pending/active, audit, review reports |
| `/admin/jobs` | Jobs manuales (p. ej. recordatorios 24h) |

> Rutas legacy (`chatbot.routes`, `vet.routes` antiguo) existen en el código pero **no** están montadas en el router principal.

---

## Roles y autorización

Middleware típico:

- `auth` — JWT válido.
- `authorizeRoles('dueno' | 'proveedor' | 'admin')` — rol permitido.
- `requireVeterinarian` / `ensureVeterinariaProvider` — acciones clínicas.

El payload JWT incluye `id`, `role` y datos mínimos del usuario; el frontend guarda el token en `localStorage` (clave configurable con `VITE_AUTH_TOKEN_KEY`).

---

## Modelos de datos principales

| Modelo | Uso |
|--------|-----|
| `User` | Dueños, proveedores, admins; perfil y estado de aprobación |
| `Pet` | Mascotas; estado `active` / `deceased` |
| `Appointment` | Citas (pending, confirmed, completed, cancelled…) |
| `AvailabilitySlot` | Bloques de agenda consumibles |
| `ClinicalEncounter` | Atención veterinaria ligada a cita |
| `Review` | Reseña por cita completada |
| `ClinicService` | Tipos de servicio en clínica |

Diagrama entidad-relación formal: carpeta `Documentación/MER/` de este repositorio.

---

## Colección Postman

- **Fuente de verdad:** `PetConnect/postman/generate-postman.mjs`
- **Copia en backend:** `PetConnectBackend/postman/PetConnect.postman_collection.json`
- **Environments:** Local, Staging, Production, QA TCP-001, CI

Para ejecutar pruebas automatizadas ver [pruebas-y-calidad.md](./pruebas-y-calidad.md).

---

## Alcance de API (`PETCONNECT_API_SCOPE`)

Variable en backend:

- `full` — todas las rutas (desarrollo, Postman, QA).
- `spa` — subconjunto usado por la PWA en producción (oculta algunos endpoints de jobs admin).

Definido en `src/config/apiScope.js`.
