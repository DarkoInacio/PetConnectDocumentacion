# Informe de Estado de QA y Plan de Pruebas Real — PetConnect Sprint 6

**Fecha de auditoría:** 17 de junio de 2026  
**Auditor:** Análisis automatizado sobre código fuente (Backend + Frontend)  
**Alcance:** `PetConnect/` y `PetConnectBackend/` — excluye documentación desactualizada como fuente de verdad  
**Referencia nomenclatura:** `PC-{MÓDULO}-{NNN}` según `PetConnect/TEST_PLAN.md` (105 casos teóricos Sprint 6)

---

## Resumen ejecutivo

| Métrica | Valor real (código) |
|---------|---------------------|
| Casos teóricos Sprint 6 (`TEST_PLAN.md`) | **105** |
| Tests automatizados Backend (Jest) | **150** — **150 Pass** |
| Tests automatizados Frontend (Vitest) | **38** — **38 Pass** |
| Tests E2E Frontend (Playwright) | **6** — **No ejecutados localmente** (requieren build+preview) |
| Smoke Newman (carpeta `Smoke`) | **13 requests**, **17 assertions** `pm.test` |
| Smoke Jest (`smoke.flow.test.js`) | **1 test**, **~17 expect()** |
| Casos PC-* con cobertura automatizada directa | **~62 de 105 (~59 %)** |
| Integración Jira Test / Xray | **No implementada en código** |

---

## 2. Auditoría y conteo de cobertura automatizada (métricas reales)

Escaneo realizado sobre `*.test.js`, `*.test.jsx`, `*.spec.js` y configuración CI.

| Capa Técnica | Framework / Herramienta | Archivos escaneados | Total tests (Nº real) | Estado / Resultado |
| :--- | :--- | :--- | :--- | :--- |
| **Backend (Integración/Lógica)** | Jest 29.7 + Supertest + MongoMemoryServer | **17** (`src/**/*.test.js`) | **150** | **Pass** (ejecutado 17-jun-2026, 37.3 s) |
| **Frontend (Unitario/Componentes)** | Vitest 4.1.8 + Testing Library + jsdom | **14** (`src/**/*.test.{js,jsx}`) | **38** | **Pass** (ejecutado 17-jun-2026, 19.4 s) |
| **Frontend E2E** | Playwright 1.52 | **5** (`e2e/*.spec.js`) | **6** | **Pendiente verificación local** (CI: `npm run test:e2e` tras build) |
| **Smoke Tests (Pre-deploy)** | Newman (carpeta Smoke) + Jest smoke flow | Postman collection + `smoke.flow.test.js` | **1 test Jest** + **13 requests Newman** | Jest: **Pass**. Newman: **Pendiente** (requiere API levantada + `seed:smoke`; corre en CI) |

### Desglose Backend Jest (150 tests)

| Archivo | Tests |
|---------|-------|
| `auth.routes.test.js` | 22 |
| `pets.routes.test.js` | 28 |
| `appointments.routes.test.js` | 18 |
| `providers.routes.test.js` | 11 |
| `vetClinical.routes.test.js` | 11 |
| `admin.routes.test.js` | 11 |
| `clinicServices.routes.test.js` | 8 |
| `medicalPdf.service.test.js` | 8 |
| `reviews.routes.test.js` | 5 |
| `petAccess.service.test.js` | 6 |
| `jwt.test.js` | 5 |
| `profile.routes.test.js` | 4 |
| `providerAgenda.routes.test.js` | 4 |
| `bookings.routes.test.js` | 4 |
| `providerReviewsPanel.routes.test.js` | 2 |
| `chat.routes.test.js` | 2 |
| `smoke.flow.test.js` | 1 |
| **Total** | **150** |

### Desglose Frontend Vitest (38 tests)

| Archivo | Tests |
|---------|-------|
| `userRoles.test.js` | 6 |
| `api.test.js` | 5 |
| `chileTime.test.js` | 4 |
| `AuthProvider.test.jsx` | 3 |
| `authForms.test.js` | 3 |
| `pets.test.js` | 3 |
| `ChatWidget.test.jsx` | 2 |
| `ForgotPasswordPage.test.jsx` | 2 |
| `LoginPage.test.jsx` | 2 |
| `OfflineBanner.test.jsx` | 2 |
| `chat.test.js` | 2 |
| `utils.test.js` | 2 |
| `useAuth.test.jsx` | 1 |
| `useOnlineStatus.test.jsx` | 1 |
| **Total** | **38** |

### CI/CD verificado en código

| Repo | Workflow | Jobs |
|------|----------|------|
| `PetConnectBackend` | `.github/workflows/backend-tests.yml` | Jest + Newman Smoke (Mongo Docker + seed) |
| `PetConnect` | `.github/workflows/frontend-tests.yml` | Vitest + Playwright E2E |

**Nota:** `test:qa:full` (Newman 14 carpetas, ~100 assertions totales en colección) **no** está en CI; solo Smoke.

---

## 3. Verificación del flujo Smoke (SMK-001 → SMK-015)

### 3.1 Cobertura SMK teórico vs automatizado

| ID Smoke | Descripción (smoke-tests.md) | Newman `Smoke` | Jest `smoke.flow.test.js` | Playwright E2E |
|----------|-------------------------------|----------------|---------------------------|----------------|
| SMK-001 | Health check | ✅ `GET {{healthUrl}}` | ✅ `GET /health` | — |
| SMK-002 | Login dueño API | ✅ | ✅ `POST /api/auth/login` | ✅ `e2e/login.spec.js` (mock) |
| SMK-003 | Login dueño UI | — | — | ✅ parcial (login.spec) |
| SMK-004 | Listar mascotas | ✅ | ✅ `GET /api/pets` | ✅ `e2e/mascotas.spec.js` |
| SMK-005 | Mapa proveedores | ✅ | ✅ `GET /api/proveedores/mapa` | ✅ parcial (navigation.spec, mock) |
| SMK-006 | Buscar veterinarias | ✅ | ✅ `GET /api/proveedores/buscar` | — |
| SMK-007 | Slots + agendar cita | ✅ SMK-007a + 007b | ✅ slots + `POST /api/appointments` 201 | — |
| SMK-008 | Bookings dueño | ✅ | ✅ `GET /api/bookings/mine` | — |
| SMK-009 | Pacientes vet | ✅ (login vet + patients) | ✅ | — |
| SMK-010 | Ficha médica resumen | ✅ | ✅ `GET .../medical-summary` | — |
| SMK-011 | Chat Vetto | ✅ | ✅ `POST /api/chat` | ✅ parcial (ChatWidget unit) |
| SMK-012 | PWA service worker | — | — | — |
| SMK-013 | Banner offline | — | — | ✅ `e2e/offline.spec.js` |
| SMK-014 | Logout limpia sesión | — | — | — |
| SMK-015 | Forgot password | ✅ | ✅ `POST /api/auth/forgot-password` 200 | ✅ parcial (forgot-password.spec, solo formulario) |

### 3.2 Orden cronológico Happy Path — Newman carpeta `Smoke`

Fuente: `PetConnectBackend/postman/PetConnect.postman_collection.json` (carpeta `Smoke`).

| Paso | Request Newman | Endpoint |
|------|----------------|----------|
| 1 | SMK-001 Health check | `GET /health` |
| 2 | SMK-002 Login dueño | `POST /api/auth/login` |
| 3 | SMK-004 Listar mascotas | `GET /api/pets` |
| 4 | SMK-005 Mapa proveedores | `GET /api/proveedores/mapa?lat&lng&radioKm` |
| 5 | SMK-006 Buscar veterinarias | `GET /api/proveedores/buscar?tipo=veterinaria` |
| 6 | SMK-007a Slots disponibles | `GET /api/appointments/providers/:id/available-slots` |
| 7 | SMK-007b Crear cita | `POST /api/appointments` |
| 8 | SMK-008 Bookings dueño | `GET /api/bookings/mine` |
| 9 | SMK-009a Login veterinario | `POST /api/auth/login` |
| 10 | SMK-009 Pacientes vet | `GET /api/vet/patients` |
| 11 | SMK-010 Ficha médica resumen | `GET /api/pets/:petId/medical-summary` |
| 12 | SMK-011 Chat Vetto | `POST /api/chat` |
| 13 | SMK-015 Forgot password | `POST /api/auth/forgot-password` |

**SMK-003, SMK-012, SMK-013, SMK-014:** no tienen request en Newman; están marcados como Manual en `test-cases/smoke-tests.md`.

### 3.3 Assertions Newman (carpeta Smoke únicamente)

| Request | Assertions `pm.test` |
|---------|------------------------|
| SMK-001 | 2 (status 200, body.status `"ok"`) |
| SMK-002 | 2 (status 200, token string) |
| SMK-004 | 1 |
| SMK-005 | 1 |
| SMK-006 | 1 |
| SMK-007a | 2 (status + slots array no vacío) |
| SMK-007b | 2 (slotId env + status 201) |
| SMK-008 | 1 |
| SMK-009a | 1 |
| SMK-009 | 1 |
| SMK-010 | 1 |
| SMK-011 | 1 |
| SMK-015 | 1 |
| **Total** | **17 assertions** |

### 3.4 Assertions Jest — `smoke.flow.test.js`

Un solo `it()` con **17 validaciones** `expect()`:

1. `GET /health` → 200, `{ status: 'ok' }`
2. `POST /api/auth/login` (dueño) → 200
3. `GET /api/pets` → 200 o 201
4. `GET /api/proveedores/mapa` → 200 o 201
5. `GET /api/proveedores/buscar` → 200 o 201
6. `GET .../available-slots` → 200 o 201 + `slots.length > 0`
7. `POST /api/appointments` → 201
8. `GET /api/bookings/mine` → 200 o 201
9. `POST /api/auth/login` (vet) → 200
10. `GET /api/vet/patients` → 200 o 201
11. `GET .../medical-summary` → 200 o 201
12. `POST /api/chat` → 200 o 201
13. `POST /api/auth/forgot-password` → 200

---

## 1. Mapeo y trazabilidad real de casos de prueba (PC-*)

Convención: cada fila corresponde a un **`it()` / `test()`** existente en código, mapeado al caso teórico `PC-*` más cercano. Los sufijos `-NEG` indican variantes negativas no listadas explícitamente en el plan pero implementadas en código.

### 1.1 Módulo AUTH — Autenticación

| ID del Caso | Módulo/Ruta afectado | Archivo de test | Descripción | Resultado esperado (código) | Prioridad |
|-------------|---------------------|-----------------|-------------|----------------------------|-----------|
| **PC-AUTH-001** | `POST /api/auth/register` | `auth.routes.test.js` | Registro dueño exitoso | 201, token, user role `dueno`, password hasheada en BD | Alta |
| **PC-AUTH-001-NEG** | `POST /api/auth/register` | `auth.routes.test.js` | Campos obligatorios faltantes | 400 | Alta |
| **PC-AUTH-001-NEG** | `POST /api/auth/register` | `auth.routes.test.js` | Rol inválido `superadmin` | 400 | Alta |
| **PC-AUTH-001-NEG** | `POST /api/auth/register` | `auth.routes.test.js` | Intento registrar proveedor por `/register` | 400, mensaje register-provider | Alta |
| **PC-AUTH-002** | `POST /api/auth/register` | `auth.routes.test.js` | Email duplicado | 409 | Alta |
| **PC-AUTH-014** | `POST /api/auth/register` | `auth.routes.test.js` | Bloqueo creación admin | 403 | Media |
| **PC-AUTH-003** | `POST /api/auth/login` | `auth.routes.test.js` | Login credenciales válidas | 200, token, user | Alta |
| **PC-AUTH-004** | `POST /api/auth/login` | `auth.routes.test.js` | Usuario inexistente / password incorrecta | **400** (plan dice 401) | Alta |
| **PC-AUTH-004-NEG** | `POST /api/auth/login` | `auth.routes.test.js` | Faltan email/password | 400 | Alta |
| **PC-AUTH-009** | `POST /api/auth/forgot-password` | `auth.routes.test.js` | Email inexistente (anti-enumeración) | 200 genérico | Alta |
| **PC-AUTH-009** | `POST /api/auth/forgot-password` | `auth.routes.test.js` | Usuario existente genera reset token | 200, resetUrl en test | Alta |
| **PC-AUTH-009-NEG** | `POST /api/auth/forgot-password` | `auth.routes.test.js` | Falta email | 400 | Alta |
| **PC-AUTH-010** | `POST /api/auth/reset-password` | `auth.routes.test.js` | Reset con token válido | 200, nueva password funciona | Alta |
| **PC-AUTH-010-NEG** | `POST /api/auth/reset-password` | `auth.routes.test.js` | Token inválido/expirado | 400 | Alta |
| **PC-AUTH-012** | `GET /api/profile/me` | `auth.routes.test.js` | Sin token / token inválido | 401 | Alta |
| **PC-AUTH-013-NEG** | `POST /api/auth/upgrade-to-provider` | `auth.routes.test.js` | Proveedor intenta upgrade | 403 | Media |
| **PC-AUTH-013-NEG** | `POST /api/auth/upgrade-to-provider` | `auth.routes.test.js` | Sin token | 401 | Media |
| **PC-PROF-001** | `GET /api/profile/me` | `auth.routes.test.js` | Perfil con token válido | 200 | Media |
| **PC-AUTH-005** | Componente `LoginPage` | `LoginPage.test.jsx` | Login exitoso navega a `/` | `navigate('/', { replace: true })` | Alta |
| **PC-AUTH-005-NEG** | Componente `LoginPage` | `LoginPage.test.jsx` | Login fallido muestra alert | role=alert con mensaje API | Alta |
| **PC-AUTH-005** | `AuthProvider` | `AuthProvider.test.jsx` | Login guarda token y carga usuario | setStoredAuthToken, POST /auth/login | Alta |
| **PC-AUTH-005** | `AuthProvider` | `AuthProvider.test.jsx` | Restaura sesión desde token | fetchMyProfile → "Hola Ana" | Alta |
| **PC-AUTH-012** | `AuthProvider` | `AuthProvider.test.jsx` | Token inválido limpia sesión | setStoredAuthToken(null), "Sin sesión" | Alta |
| **PC-AUTH-011** | `ForgotPasswordPage` | `ForgotPasswordPage.test.jsx` | Éxito recuperación | role=status con mensaje | Alta |
| **PC-AUTH-011-NEG** | `ForgotPasswordPage` | `ForgotPasswordPage.test.jsx` | Error API | role=alert | Alta |
| **PC-AUTH-005** | Servicio auth | `authForms.test.js` | registerOwner payload correcto | POST /auth/register | Alta |
| **PC-AUTH-009** | Servicio auth | `authForms.test.js` | forgotPassword | POST /auth/forgot-password | Alta |
| **PC-AUTH-010** | Servicio auth | `authForms.test.js` | resetPassword | POST /auth/reset-password | Alta |
| **PC-AUTH-005** | Hook `useAuth` | `useAuth.test.jsx` | Error fuera de AuthProvider | throw /AuthProvider/ | Alta |
| **PC-AUTH-012** | Utilidad JWT | `jwt.test.js` | sign/verify token válido | JWT decodable | Alta |
| **PC-AUTH-012-NEG** | Utilidad JWT | `jwt.test.js` | Token manipulado | throw | Alta |
| **PC-AUTH-005** | `api.js` helpers | `api.test.js` | Persistencia token localStorage | get/set/remove token | Alta |
| **PC-AUTH-005** | E2E login | `e2e/login.spec.js` | Login dueño → mapa | URL `/`, menú cuenta visible | Alta |
| **PC-AUTH-011** | E2E | `e2e/forgot-password.spec.js` | Formulario recuperación visible | heading + input email | Alta |

**Sin cobertura automatizada en código:** PC-AUTH-006 (logout UI), PC-AUTH-007/008 (registro proveedor API/UI), PC-AUTH-013 happy path (upgrade dueño→proveedor).

---

### 1.2 Módulo PROF — Perfil

| ID del Caso | Módulo/Ruta | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|-------------|---------|-------------|-------------------|-----------|
| **PC-PROF-003** | `GET /api/profile/me` | `profile.routes.test.js` | Sin token | 401 | Media |
| **PC-PROF-003** | `PUT /api/profile/me` | `profile.routes.test.js` | Sin token | 401 | Media |
| **PC-PROF-002** | `PUT /api/profile/me` | `profile.routes.test.js` | Actualiza nombre y teléfono | 200 | Media |
| **PC-PROF-002-NEG** | `PUT /api/profile/me` | `profile.routes.test.js` | Intento cambiar email | 400 | Media |

---

### 1.3 Módulo PETS — Mascotas

| ID del Caso | Módulo/Ruta | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|-------------|---------|-------------|-------------------|-----------|
| **PC-PETS-010** | `POST /api/pets` | `pets.routes.test.js` | Sin autenticación | 401 | Alta |
| **PC-PETS-010-NEG** | `POST /api/pets` | `pets.routes.test.js` | Rol no dueño | 403 | Alta |
| **PC-PETS-001-NEG** | `POST /api/pets` | `pets.routes.test.js` | Campos obligatorios / species inválida | 400 | Alta |
| **PC-PETS-001** | `POST /api/pets` | `pets.routes.test.js` | Crear mascota activa | 201, status active | Alta |
| **PC-PETS-002** | `GET /api/pets` | `pets.routes.test.js` | Lista mascotas del dueño | 200, solo propias | Alta |
| **PC-PETS-002** | `GET /api/pets?forAgenda=1` | `pets.routes.test.js` | Filtro activas para agenda | 200, excluye deceased | Alta |
| **PC-PETS-003** | `GET /api/pets/:id` | `pets.routes.test.js` | Dueño ve su mascota | 200 | Alta |
| **PC-PETS-009** | `GET /api/pets/:id` | `pets.routes.test.js` | Otro dueño / vet sin relación | 403/404 | Alta |
| **PC-PETS-003** | `GET /api/pets/:id` | `pets.routes.test.js` | Vet con cita confirmada | 200 | Alta |
| **PC-PETS-004** | `PATCH /api/pets/:id` | `pets.routes.test.js` | Actualización parcial | 200 | Alta |
| **PC-PETS-004-NEG** | `PATCH /api/pets/:id` | `pets.routes.test.js` | Mascota fallecida | 400 | Alta |
| **PC-PETS-008** | `PATCH .../mark-deceased` | `pets.routes.test.js` | Marcar fallecida | 200, status deceased | Alta |
| **PC-PETS-008-NEG** | `PATCH .../mark-deceased` | `pets.routes.test.js` | Ya fallecida | 400 | Alta |
| **PC-PETS-002** | Servicio `pets.js` | `pets.test.js` | listPets GET /pets | mock api.get | Alta |
| **PC-PETS-001** | Servicio `pets.js` | `pets.test.js` | createPet FormData | POST /pets campos | Alta |
| **PC-PETS-008** | Servicio `pets.js` | `pets.test.js` | markPetDeceased | PATCH mark-deceased | Alta |
| **PC-PETS-005** | E2E | `e2e/mascotas.spec.js` | Lista mascotas post-login | texto "Firulais" visible | Alta |

**Sin cobertura:** PC-PETS-005/006 UI completa (crear/editar), PC-PETS-007 foto (`GET .../photo`).

---

### 1.4 Módulo MED — Ficha médica

| ID del Caso | Módulo/Ruta | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|-------------|---------|-------------|-------------------|-----------|
| **PC-MED-001** | `GET .../medical-summary` | `pets.routes.test.js` | Resumen al dueño | 200 | Alta |
| **PC-MED-003** | `GET .../clinical-encounters` | `pets.routes.test.js` | Lista encounters dueño | 200 | Alta |
| **PC-MED-003-NEG** | `GET .../clinical-encounters` | `pets.routes.test.js` | Filtro from inválido | 400 | Alta |
| **PC-MED-004** | `GET .../clinical-encounters/:id` | `pets.routes.test.js` | Detalle encounter | 200 | Alta |
| **PC-MED-004-NEG** | `GET .../clinical-encounters/:id` | `pets.routes.test.js` | Tercero sin acceso | 403 | Alta |
| **PC-MED-001** | `GET .../medical-summary` | `pets.routes.test.js` | Total atenciones en resumen | 200, count coherente | Alta |
| **PC-MED-009** | `GET .../medical-record/export.pdf` | `pets.routes.test.js` | Export PDF dueño | 200 | Alta |
| **PC-MED-009-NEG** | `GET .../export.pdf` | `pets.routes.test.js` | Sin token / otro dueño | 401/403 | Alta |
| **PC-MED-005** | `POST /api/vet/pets/:id/clinical-encounters` | `vetClinical.routes.test.js` | Crear encounter vinculado a cita | 201 | Alta |
| **PC-MED-005-NEG** | idem | `vetClinical.routes.test.js` | Sin auth / no vet / sin appointmentId | 401/403/400 | Alta |
| **PC-MED-005-NEG** | idem | `vetClinical.routes.test.js` | Encounter duplicado misma cita | 409 | Alta |
| **PC-MED-012** | `GET /api/vet/pets/:id/clinical-encounters` | `vetClinical.routes.test.js` | Lista vet con acceso | 200 | Alta |
| **PC-MED-007** | `PATCH /api/vet/clinical-encounters/:id` | `vetClinical.routes.test.js` | Actualizar en ventana edición | 200 | Alta |
| **PC-MED-007-NEG** | idem | `vetClinical.routes.test.js` | Otro veterinario | 403 | Alta |
| **PC-MED-009** | `medicalPdf.service` | `medicalPdf.service.test.js` | assertPdfAccess dueño/vet | pet o throw 403/404 | Alta |
| **PC-MED-009** | `medicalPdf.service` | `medicalPdf.service.test.js` | streamMedicalRecordPdf | Buffer PDF, content-type | Alta |
| **PC-MED-011** | Smoke flow | `smoke.flow.test.js` | `GET /api/vet/patients` | 200/201 | Alta |
| **PC-MED-001** | Smoke flow | `smoke.flow.test.js` | medical-summary | 200/201 | Alta |
| **PC-MED-012** | `petAccess.service` | `petAccess.service.test.js` | vetHasAccessToPet / assertVetAppointment | true/false/null | Alta |

**Sin cobertura:** PC-MED-002/006 UI, PC-MED-008 retracciones, PC-MED-010 adjuntos, PC-MED-012 vía ruta vet sin relación (parcial en service).

---

### 1.5 Módulo SEARCH — Búsqueda y geo

| ID del Caso | Módulo/Ruta | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|-------------|---------|-------------|-------------------|-----------|
| **PC-SEARCH-001** | `GET /api/proveedores/buscar` | `providers.routes.test.js` | Buscar veterinarias | 200 paginado | Media |
| **PC-SEARCH-001-NEG** | `GET /api/proveedores` | `providers.routes.test.js` | Tipo inválido | 400 | Media |
| **PC-SEARCH-003** | `GET /api/proveedores/mapa` | `providers.routes.test.js` | Markers mapa | 200 | Media |
| **PC-SEARCH-008** | `GET /api/proveedores/:id/perfil` | `providers.routes.test.js` | Perfil público por id | 200 | Media |
| **PC-SEARCH-008** | `GET /api/proveedores/perfil/veterinaria/:slug` | `providers.routes.test.js` | Perfil por slug | 200 | Media |
| **PC-SEARCH-004** | E2E `/` | `e2e/navigation.spec.js` | Mapa carga + menú cuenta | botón visible | Media |
| **PC-SEARCH-004** | E2E | `e2e/navigation.spec.js` | Navegar a login desde menú | URL /login | Media |

**Sin cobertura:** PC-SEARCH-002 (filtro ciudad), PC-SEARCH-005/006 geo, PC-SEARCH-007 explorar UI, PC-SEARCH-009 perfil UI.

---

### 1.6 Módulo APPT — Citas / agendamiento

| ID del Caso | Módulo/Ruta | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|-------------|---------|-------------|-------------------|-----------|
| **PC-APPT-001** | `GET .../available-slots` | `appointments.routes.test.js` | Listar slots | 200, array slots | Alta |
| **PC-APPT-001-NEG** | idem | `appointments.routes.test.js` | 401/403 | Alta |
| **PC-APPT-002** | `POST /api/appointments` | `appointments.routes.test.js` | Agendar consumiendo slot | 201 | Alta |
| **PC-APPT-004** | `POST /api/appointments` | `appointments.routes.test.js` | Slot no disponible | 409 | Alta |
| **PC-APPT-002-NEG** | idem | `appointments.routes.test.js` | Campos faltantes | 400 | Alta |
| **PC-APPT-005** | `GET /api/appointments/mine` | `appointments.routes.test.js` | Citas del dueño | 200 | Alta |
| **PC-APPT-007** | `PATCH .../provider/confirm` | `appointments.routes.test.js` | Proveedor confirma | 200 | Alta |
| **PC-APPT-007-NEG** | idem | `appointments.routes.test.js` | Dueño no puede confirmar | 403 | Alta |
| **PC-APPT-009** | `PATCH .../complete-vet` | `appointments.routes.test.js` | Completar cita vet | 200 | Alta |
| **PC-APPT-006** | `PATCH .../cancel` | `appointments.routes.test.js` | Dueño cancela (≥2h) | 200 | Alta |
| **PC-APPT-006-NEG** | idem | `appointments.routes.test.js` | Sin cancellationReason cuando aplica | 400 | Alta |
| **PC-APPT-008** | `PATCH .../provider/cancel` | `appointments.routes.test.js` | Proveedor cancela | 200 | Alta |
| **PC-APPT-010** | `PATCH .../complete-walker` | `appointments.routes.test.js` | Completar walker | 200 | Alta |
| **PC-REV-001** | `GET .../review-eligibility` | `appointments.routes.test.js` | Elegibilidad reseña | 200, canReview | Media |
| **PC-REV-002** | `POST .../reviews` | `appointments.routes.test.js` | Crear reseña + duplicado | 201 / 409 | Media |

**Sin cobertura:** PC-APPT-003 UI agendar, PC-APPT-011 complete-visit cuidador, PC-APPT-012 notas internas, PC-APPT-013 mascota fallecida, PC-APPT-014/015 UI vet.

---

### 1.7 Módulo AGENDA — Agenda proveedor

| ID del Caso | Módulo/Ruta | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|-------------|---------|-------------|-------------------|-----------|
| **PC-AGENDA-002** | `GET /api/provider/agenda/slots` | `providerAgenda.routes.test.js` | Lista slots vet | 200 | Media |
| **PC-AGENDA-002-NEG** | idem | `providerAgenda.routes.test.js` | No proveedor | 403 | Media |
| **PC-AGENDA-003** | `PATCH .../slots/:id/block` | `providerAgenda.routes.test.js` | Bloquear slot propio | 200 | Media |
| **PC-AGENDA-003-NEG** | idem | `providerAgenda.routes.test.js` | Otro proveedor | 404 | Media |

**Sin cobertura:** PC-AGENDA-001 generate, PC-AGENDA-004 unblock, PC-AGENDA-005 delete, PC-AGENDA-006 omits.

---

### 1.8 Módulo BOOK — Reservas unificadas

| ID del Caso | Módulo/Ruta | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|-------------|---------|-------------|-------------------|-----------|
| **PC-BOOK-001** | `GET /api/bookings/mine` | `bookings.routes.test.js` | Reservas dueño | 200, items appointment | Alta |
| **PC-BOOK-001-NEG** | idem | `bookings.routes.test.js` | No dueño | 403 | Alta |
| **PC-BOOK-003** | `GET /api/bookings/provider/mine` | `bookings.routes.test.js` | Reservas proveedor | 200 | Alta |
| **PC-BOOK-004** | `POST /api/proveedores/solicitar-servicio` | `providers.routes.test.js` | Solicitud paseador | 201 | Media |
| **PC-BOOK-004-NEG** | idem | `providers.routes.test.js` | Proveedor no walker | 400 | Media |

**Sin cobertura:** PC-BOOK-002 UI reservas.

---

### 1.9 Módulo REV — Reseñas

| ID del Caso | Módulo/Ruta | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|-------------|---------|-------------|-------------------|-----------|
| **PC-REV-002** | `POST /api/appointments/:id/reviews` | `reviews.routes.test.js` | Publicar tras cita completada | 201 | Media |
| **PC-REV-002-NEG** | idem | `reviews.routes.test.js` | Usuario no elegible | 403 | Media |
| **PC-REV-005** | `GET /api/proveedores/:id/reviews` | `reviews.routes.test.js` | Listado público | 200 | Media |
| **PC-REV-003** | `PATCH /api/reviews/:id` | `reviews.routes.test.js` | Edición dueño 24h | 200 | Media |
| **PC-REV-004** | `POST /api/reviews/:id/report` | `reviews.routes.test.js` | Reportar reseña | 201 | Media |
| **PC-REV-006** | `GET /api/provider/reviews` | `providerReviewsPanel.routes.test.js` | Panel proveedor | 200 | Media |

**Sin cobertura:** PC-REV-006 reply (`PUT .../reply`) — solo listado implementado.

---

### 1.10 Módulo CHAT — Vetto

| ID del Caso | Módulo/Ruta | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|-------------|---------|-------------|-------------------|-----------|
| **PC-CHAT-001** | `POST /api/chat` | `chat.routes.test.js` | Visitante sin token | 200, reply mock | Media |
| **PC-CHAT-001-NEG** | idem | `chat.routes.test.js` | Sin mensaje | 400 | Media |
| **PC-CHAT-001** | Servicio `chat.js` | `chat.test.js` | sendChatMessage | POST /chat + history | Media |
| **PC-CHAT-003** | Servicio `chat.js` | `chat.test.js` | resetChatSession | POST reset:true | Media |
| **PC-CHAT-004** | `ChatWidget` | `ChatWidget.test.jsx` | Envío y respuesta UI | texto asistente visible | Media |
| **PC-CHAT-005** | `ChatWidget` | `ChatWidget.test.jsx` | Offline deshabilita input | alert sin conexión | Media |

**Sin cobertura:** PC-CHAT-002 continuar sesión con sessionId (backend).

---

### 1.11 Módulo PROV — Proveedor / servicios clínica

| ID del Caso | Módulo/Ruta | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|-------------|---------|-------------|-------------------|-----------|
| **PC-PROV-003** | `GET /api/provider/clinic-services` | `clinicServices.routes.test.js` | Lista servicios vet | 200 | Media |
| **PC-PROV-003-NEG** | idem | `clinicServices.routes.test.js` | No aprobado / dueño | 403 | Media |
| **PC-PROV-004** | `POST /api/provider/clinic-services` | `clinicServices.routes.test.js` | Crear servicio | 201 | Media |
| **PC-PROV-004-NEG** | idem | `clinicServices.routes.test.js` | Sin displayName / paseador sin price | 400 | Media |
| **PC-PROV-005** | `PATCH /api/provider/clinic-services/:id` | `clinicServices.routes.test.js` | Actualizar servicio | 200 | Media |
| **PC-PROV-001-NEG** | `PUT /api/proveedores/mi-perfil` | `providers.routes.test.js` | 401/403 | Media |

**Sin cobertura:** PC-PROV-001 happy path, PC-PROV-002 UI, PC-PROV-006 UI solicitar servicio.

---

### 1.12 Módulo ADMIN — Administración

| ID del Caso | Módulo/Ruta | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|-------------|---------|-------------|-------------------|-----------|
| **PC-ADMIN-001** | `GET /api/admin/providers/pending` | `admin.routes.test.js` | Lista pendientes | 200 | Media |
| **PC-ADMIN-001-NEG** | idem | `admin.routes.test.js` | 401/403 | Media |
| **PC-ADMIN-002** | `PATCH .../approve` | `admin.routes.test.js` | Aprobar proveedor | 200 | Media |
| **PC-ADMIN-002-NEG** | idem | `admin.routes.test.js` | No en revisión | 400 | Media |
| **PC-ADMIN-003** | `PATCH .../reject` | `admin.routes.test.js` | Rechazar con motivo | 200 | Media |
| **PC-ADMIN-003-NEG** | idem | `admin.routes.test.js` | Sin reason | 400 | Media |
| **PC-ADMIN-004** | `PATCH .../suspend` | `admin.routes.test.js` | Suspender aprobado | 200 | Media |
| **PC-ADMIN-004-NEG** | idem | `admin.routes.test.js` | Suspender pendiente | 400 | Media |

**Sin cobertura:** PC-ADMIN-005 UI, PC-ADMIN-006 review-reports, PC-ADMIN-007 job recordatorios.

---

### 1.13 Módulo PWA — Progressive Web App

| ID del Caso | Componente | Archivo | Descripción | Resultado esperado | Prioridad |
|-------------|------------|---------|-------------|-------------------|-----------|
| **PC-PWA-004** | `OfflineBanner` | `OfflineBanner.test.jsx` | Sin conexión muestra alert | role=alert | Media |
| **PC-PWA-004** | `OfflineBanner` | `OfflineBanner.test.jsx` | Con conexión no renderiza | container vacío | Media |
| **PC-PWA-004** | `useOnlineStatus` | `useOnlineStatus.test.jsx` | Refleja navigator.onLine | true/false | Media |
| **PC-PWA-004** | E2E | `e2e/offline.spec.js` | Banner en contexto offline | alert visible | Media |
| **PC-CHAT-005** | `ChatWidget` | `ChatWidget.test.jsx` | Chat offline | input disabled | Media |

**Sin cobertura:** PC-PWA-001 SW registrado, PC-PWA-002 manifest Lighthouse, PC-PWA-003 instalación, PC-PWA-005 shell offline.

---

### 1.14 Módulos auxiliares (sin ID PC directo en plan)

| ID asignado | Archivo | Descripción | Prioridad |
|-------------|---------|-------------|-----------|
| **PC-UTIL-001** | `utils.test.js` | Helper `cn()` tailwind-merge | Media |
| **PC-UTIL-002** | `userRoles.test.js` | hasRole / isAdministrator | Media |
| **PC-UTIL-003** | `chileTime.test.js` | Fechas zona Chile / UTC | Media |

---

### 1.15 Matriz resumen: 105 casos teóricos vs código

| Módulo | Casos plan (105) | Automatizado (directo o parcial) | Solo manual / sin test |
|--------|------------------|----------------------------------|------------------------|
| AUTH | 14 | 10 | 4 (006, 007, 008, 013 HP) |
| PROF | 3 | 3 | 0 |
| PETS | 10 | 7 | 3 (005, 006, 007) |
| MED | 12 | 9 | 3 (002, 006, 008, 010) |
| SEARCH | 9 | 3 | 6 |
| APPT | 15 | 11 | 4 |
| AGENDA | 6 | 2 | 4 |
| BOOK | 4 | 3 | 1 |
| REV | 6 | 5 | 1 |
| CHAT | 5 | 4 | 1 |
| PROV | 6 | 4 | 2 |
| ADMIN | 7 | 4 | 3 |
| PWA | 5 | 1 | 4 |
| NOTIF | 3 | 0 | 3 |
| **Total** | **105** | **~66 parciales / ~62 únicos** | **~43 sin automatizar** |

---

## 4. Identificación de brechas honestas (Presentación vs. código real)

### 4.1 Meta Sprint 6: 105 casos teóricos

- Los **105 casos `PC-*`** existen en `PetConnect/TEST_PLAN.md` (secciones 5.1–5.14).
- **No hay 105 tests automatizados** ni mapeo 1:1 en código; hay **194 tests técnicos** (150+38+6) que cubren subconjuntos y variantes negativas.
- **~41 % de casos PC-* no tienen ningún test automatizado** (principalmente UI manual, geo, PWA avanzada, notificaciones email, registro proveedor).

### 4.2 Prioridad Alta — brechas críticas

| Caso | Estado en código |
|------|------------------|
| PC-AUTH-006 Logout | **Sin cobertura** |
| PC-AUTH-007/008 Registro proveedor | **Sin cobertura** |
| PC-PETS-005/006 CRUD UI mascotas | Solo E2E listado parcial |
| PC-MED-002 Ficha médica UI | **Sin cobertura** |
| PC-APPT-003 Agendar UI | **Sin cobertura** |
| PC-BOOK-002 Reservas UI | **Sin cobertura** |
| PC-APPT-013 Mascota fallecida al agendar | **Sin test dedicado** (lógica puede existir en controller) |

### 4.3 Prioridad Media — brechas

- **Geo (PC-SEARCH-005/006):** sin tests automatizados.
- **PWA completa (PC-PWA-001/002/003/005):** solo banner offline cubierto.
- **Admin reportes reseñas (PC-ADMIN-006):** sin tests.
- **Respuesta proveedor a reseña (PC-REV-006 PUT reply):** sin test.

### 4.4 Discrepancias plan vs implementación de tests

| Tema | Plan (`TEST_PLAN.md`) | Código real |
|------|----------------------|-------------|
| Login inválido | PC-AUTH-004 → **401** | Tests esperan **400** |
| Roadmap automatización | Decía "futuro Jest/Playwright" | **Ya implementado** (doc desactualizado en §4 y §9) |
| Newman QA completo | 14 carpetas TCP-001 | Script existe (`test:qa:full`) pero **no en CI** |
| E2E integración real | — | Playwright **mockea API** (`e2e/helpers/apiMocks.js`), no golpea backend |

### 4.5 Jira Test / reportes automatizados

- **No existe** integración con Jira, Xray, TestRail ni exportación automática de resultados en ningún repo.
- `TEST_PLAN.md` §11 menciona "Reporte de bugs | Jira / Notion / GitHub Issues" como **proceso manual**, no como código.
- No hay scripts, webhooks, plugins ni configuración CI hacia Jira.

### 4.6 Módulos backend sin archivo `*.test.js`

Sin cobertura de pruebas en código para rutas/controladores no listados arriba, incluyendo:

- `adminJobs.routes.js` (PC-ADMIN-007)
- Rutas de adjuntos clínicos dedicadas
- Geocodificación Nominatim
- Jobs cron (`appointmentReminders.job.js`) — PC-NOTIF-002
- Envío email real — PC-NOTIF-* (mockeado globalmente en `test/setup/mocks.js`)

### 4.7 Frontend sin tests

Páginas/componentes **sin** `*.test.jsx` (muestra representativa):

- `ProvidersMapPage`, `ProvidersExplorePage`, `BookAppointmentPage`, `MyBookingsPage`
- `PetFormPage`, `PetMedicalPage`, `VetClinicalPage`, `AdminProvidersPage`
- `RegisterProviderPage`, `ResetPasswordPage`
- Flujos proveedor/vet completos

---

## Apéndice A — Comandos de verificación reproducible

```bash
# Backend (150 tests)
cd PetConnectBackend
npm ci
npm test

# Frontend (38 tests)
cd PetConnect
npm ci
npm test

# Smoke Newman (requiere API + seed)
cd PetConnectBackend
npm run seed:smoke
npm run dev   # otra terminal
npm run test:smoke

# E2E Frontend
cd PetConnect
npm run test:e2e
```

---

## Apéndice B — Archivos de prueba inventariados

**Backend (17):**  
`auth.routes.test.js`, `pets.routes.test.js`, `appointments.routes.test.js`, `providers.routes.test.js`, `vetClinical.routes.test.js`, `admin.routes.test.js`, `clinicServices.routes.test.js`, `bookings.routes.test.js`, `reviews.routes.test.js`, `profile.routes.test.js`, `providerAgenda.routes.test.js`, `providerReviewsPanel.routes.test.js`, `chat.routes.test.js`, `smoke.flow.test.js`, `jwt.test.js`, `petAccess.service.test.js`, `medicalPdf.service.test.js`

**Frontend Vitest (14):**  
`AuthProvider.test.jsx`, `LoginPage.test.jsx`, `ForgotPasswordPage.test.jsx`, `ChatWidget.test.jsx`, `OfflineBanner.test.jsx`, `useAuth.test.jsx`, `useOnlineStatus.test.jsx`, `api.test.js`, `authForms.test.js`, `pets.test.js`, `chat.test.js`, `utils.test.js`, `userRoles.test.js`, `chileTime.test.js`

**Frontend E2E (5):**  
`login.spec.js`, `mascotas.spec.js`, `navigation.spec.js`, `offline.spec.js`, `forgot-password.spec.js`

---
