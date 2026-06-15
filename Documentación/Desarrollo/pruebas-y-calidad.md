# Pruebas y calidad

Estrategia híbrida: **automáticas en API**, **unitarias en ambos repos**, **e2e en frontend**, **manual** para PWA y UX.

Plan detallado: **`PetConnect/TEST_PLAN.md`**.

---

## Backend — PetConnectBackend

### Jest + Supertest

```bash
npm test              # Suite completa
npm run test:watch    # Modo watch
npm run test:coverage # Cobertura
```

- Tests junto a rutas: `src/routes/*.routes.test.js`
- Tests de servicios: `src/services/*.test.js`
- MongoDB en memoria vía `mongodb-memory-server` en CI/local

### Newman (Postman CLI)

| Comando | Descripción |
|---------|-------------|
| `npm run test:smoke:full` | Seed smoke + health + carpeta **Smoke** |
| `npm run test:qa:full` | Seed QA + health + **14 carpetas** Newman |
| `npm run test:qa` | Solo Newman QA (requiere seed previo) |

Documentación operativa: **`PetConnectBackend/postman/README.md`**.

#### Orden QA TCP-001 (Newman)

Auth → Profile → Pets → Providers → Clinic Services → Vet Clinical → Bookings → Chat → Appointments → Agenda → Reviews → Admin → Smoke → **Pets cleanup**

#### Datos semilla QA

```bash
npm run seed:qa
```

Genera `postman/PetConnect-QA.postman_environment.json` con usuarios `*@petconnect.test`, contraseña `QaTest2026!`, IDs de cita, encounter, reseña, etc.

### CI (GitHub Actions)

Workflow `backend-tests.yml`: Jest + Newman Smoke en cada push/PR a `main`.

---

## Frontend — PetConnect

### Vitest + Testing Library

```bash
npm test           # Una pasada
npm run test:watch
```

Tests en `src/**/*.test.jsx` y servicios `*.test.js`.

### Playwright (e2e)

```bash
npm run test:e2e      # Headless
npm run test:e2e:ui   # UI mode
```

Especificaciones en `e2e/` (login, navegación, mascotas, offline, forgot password).

### CI

Workflow `frontend-tests.yml`: lint + unit tests (+ e2e según configuración del workflow).

---

## Casos smoke documentados

| Archivo | Contenido |
|---------|-----------|
| `PetConnectBackend/test-cases/smoke-tests.md` | SMK-001 … SMK-015 |
| `PetConnect/test-cases/smoke-tests.md` | Copia / referencia frontend |

IDs smoke en colección Postman (carpeta **Smoke**).

---

## Entornos Postman

| Environment | Generado por |
|-------------|--------------|
| `PetConnect-CI.postman_environment.json` | `npm run seed:smoke` |
| `PetConnect-QA.postman_environment.json` | `npm run seed:qa` |
| Local / Staging / Production | Plantillas en `postman/` (valores manuales) |

---

## Criterios de aceptación generales

- **Smoke Newman** en verde antes de deploy backend.
- **QA TCP-001** (`test:qa:full`) para regresión amplia de API antes de entregas o demos.
- Respuestas **409/400** documentadas como idempotentes en re-runs (encounters, reseñas, servicios duplicados).
- Frontend: flujos críticos cubiertos en TEST_PLAN + e2e donde aplique.

---

## Evidencias para asignatura

Además de logs de Newman/Jest, el equipo puede adjuntar en `Documentación/Informes/`:

- Capturas de CI en GitHub Actions.
- Export de corrida Newman (`postman/newman-results.xml` si se configura).
- Checklist TCP-001 (`PetConnect/docs/qa/`).
