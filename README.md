# PetConnect — Documentación del proyecto

PetConnect es una Progressive Web App diseñada para centralizar la gestión de salud animal y facilitar la conexión entre dueños de mascotas, centros veterinarios y cuidadores en Chile. La solución resuelve la dispersión de información médica y las dificultades de agendamiento mediante un ecosistema digital integrado.

## Documento oficial de definición e identificación del proyecto

El alcance, objetivos, equipo, metodología (Scrum), plan de trabajo por sprints, stack tecnológico y evidencias acordadas con la asignatura están consignados en:

**`Gestión/Definicion-del-Proyecto/1.1.2-Documento-registro-definicion-identificacion-proyecto.pdf`**

*(Título original del entregable: «1.1.2 Documento de registro de definición e identificación del proyecto», TPY 1101 — Taller Aplicado de Programación.)*

---

## Guía técnica para desarrolladores

Documentación general de arquitectura, tecnologías, instalación local, estructura de carpetas y pruebas:

**→ [`Documentación/Desarrollo/README.md`](Documentación/Desarrollo/README.md)**

| Tema | Enlace |
|------|--------|
| Arquitectura | [arquitectura-general.md](Documentación/Desarrollo/arquitectura-general.md) |
| Stack | [stack-tecnologico.md](Documentación/Desarrollo/stack-tecnologico.md) |
| Instalación local | [instalacion-local.md](Documentación/Desarrollo/instalacion-local.md) |
| Estructura de repos | [estructura-repositorios.md](Documentación/Desarrollo/estructura-repositorios.md) |
| API REST | [api-rest-resumen.md](Documentación/Desarrollo/api-rest-resumen.md) |
| Pruebas | [pruebas-y-calidad.md](Documentación/Desarrollo/pruebas-y-calidad.md) |

Repositorios de código: **PetConnect** (frontend PWA) y **PetConnectBackend** (API REST).

---

## Estructura de carpetas

| Carpeta | Propósito |
|---------|-----------|
| **Documentación** | Informes, UML, wireframes, MER, Gantt, **Desarrollo** (guías técnicas) |
| **Producto** | Scripts de datos, referencia al código fuente y librerías |
| **Gestión** | Definición del proyecto, lista de integrantes |

Las subcarpetas usan **`README.txt`** como guía breve; la documentación técnica extensa está en **Markdown** bajo `Documentación/Desarrollo/`.

---

## Progreso actual del proyecto

*Estado alineado al plan del documento 1.1.2 y al avance técnico del ecosistema PetConnect + PetConnectBackend.*

### Organización y metodología

- Equipo de **cuatro integrantes** con roles Scrum (Scrum Master, Product Owner, desarrollo frontend/backend) según el registro del documento.
- Metodología **Scrum**: Product Backlog, sprints, revisiones y retrospectivas planificadas en el documento oficial.
- Infraestructura: **MongoDB Atlas**, frontend en **Vercel**, backend en **Render**, repositorios en **GitHub**.

### Por sprint (según planificación del documento)

| Fase | Contenido principal | Estado referencial |
|------|----------------------|---------------------|
| **Sprint 0** | Constitución del equipo, setup de repos, Atlas, levantamiento de épicas/HU, modelado de datos | Completado en lo esencial |
| **Sprint 1 — Autenticación y perfil** | HU-01 a HU-05, HU-29 | **Finalizado** (JWT, registro, recuperación de clave, perfil) |
| **Sprint 2 — Búsqueda, mapa y reservas** | HU-06 a HU-15 | **Avanzado**: mapa, citas, agenda por slots, bookings, recordatorios |
| **Sprint 3 — Ficha médica y reseñas** | HU-16 a HU-22 | **Avanzado**: mascotas, encounters, PDF, reseñas por cita |
| **Sprint 4 — Chatbot** | HU-23 a HU-25 | **Implementado** en backend (`/api/chat`) + widget frontend |
| **Sprint 5 — Panel admin y PWA** | HU-26 a HU-28, HU-30, HU-31 | **Parcial**: admin proveedores/reportes; PWA con offline banner |
| **Sprint 6 — Pruebas y despliegue** | Pruebas, bugs, UX, producción | En curso — Jest, Newman QA TCP-001, Vitest, Playwright |
| **Sprint 7 — Cierre** | Review y retrospectiva | Pendiente |

### QA automatizado (estado reciente)

- **`npm run test:qa:full`** en PetConnectBackend ejecuta seed + Newman sobre **14 carpetas** (Auth … Smoke + Pets cleanup).
- Documentación operativa: `PetConnectBackend/postman/README.md`.

### Alcance y fuera de alcance (recordatorio del documento)

- **Incluye:** dueños, veterinarias, paseadores/cuidadores, ficha médica, mapa, agendamiento, chatbot, reseñas, panel admin.
- **Fuera de alcance:** app nativa, pasarela de pago, integración con sistemas clínicos externos, telemedicina estricta.

### Próximos pasos sugeridos

1. Mantener el PDF de definición versionado en `Gestión/Definicion-del-Proyecto/`.
2. Completar entregables visuales en **Documentación** (UML, MER, Gantt).
3. Mantener alineados frontend y backend (variables de entorno, `TEST_PLAN.md`).
4. Archivar evidencias de CI y Newman en `Documentación/Informes/`.

---

*Última actualización: junio 2026 — incluye guías en `Documentación/Desarrollo/` y estado QA Newman TCP-001.*
