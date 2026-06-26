# PetConnect — Documentación del proyecto

PetConnect es una Progressive Web App diseñada para centralizar la gestión de salud animal y facilitar la conexión entre dueños de mascotas, centros veterinarios y cuidadores en Chile. La solución resuelve la dispersión de información médica y las dificultades de agendamiento mediante un ecosistema digital integrado.

## Documento oficial de definición e identificación del proyecto

El alcance, objetivos, equipo, metodología (Scrum), plan de trabajo por sprints, stack tecnológico y evidencias acordadas con la asignatura están consignados en:

**`Gestión/Definicion-del-Proyecto/1.1.2-Documento-registro-definicion-identificacion-proyecto.pdf`**

*(Título original del entregable: «1.1.2 Documento de registro de definición e identificación del proyecto», TPY 1101 — Taller Aplicado de Programación.)*

Product Backlog Jira: **`Gestión/Definicion-del-Proyecto/PetConnect_Jira_Backlog.xlsx`**

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
| **Documentación** | Informes, UML, MER, Gantt, presentaciones, **Desarrollo** (guías técnicas) |
| **Producto** | Referencia al código fuente, librerías y scripts de base de datos |
| **Gestión** | Definición del proyecto, backlog Jira, lista de integrantes |

Las subcarpetas usan **`README.txt`** como índice breve; la documentación técnica extensa está en **Markdown** bajo `Documentación/Desarrollo/`.

---

## Progreso del proyecto (cierre junio 2026)

*Estado final al cierre del semestre, alineado al documento 1.1.2 y al ecosistema PetConnect + PetConnectBackend.*

### Organización y metodología

- Equipo de **cuatro integrantes** con roles Scrum (Scrum Master, Product Owner, desarrollo frontend/backend) — ver [`Gestión/Lista-de-Integrantes/`](Gestión/Lista-de-Integrantes/README.txt).
- Metodología **Scrum**: Product Backlog, sprints 0–7, revisiones y retrospectivas.
- Infraestructura: **MongoDB Atlas**, frontend en **Vercel**, backend en **Render**, repositorios en **GitHub**.

### Por sprint

| Fase | Contenido principal | Estado |
|------|----------------------|--------|
| **Sprint 0** | Constitución del equipo, setup de repos, Atlas, levantamiento de épicas/HU, modelado de datos | Completado |
| **Sprint 1 — Autenticación y perfil** | HU-01 a HU-05, HU-29 | Completado |
| **Sprint 2 — Búsqueda, mapa y reservas** | HU-06 a HU-15 | Completado |
| **Sprint 3 — Ficha médica y reseñas** | HU-16 a HU-22 | Completado |
| **Sprint 4 — Chatbot** | HU-23 a HU-25 | Completado |
| **Sprint 5 — Panel admin y PWA** | HU-26 a HU-28, HU-30, HU-31 | Completado |
| **Sprint 6 — Pruebas y despliegue** | Pruebas, bugs, UX, producción | Completado |
| **Sprint 7 — Cierre** | Review y retrospectiva | Completado |

### QA automatizado

- **`npm run test:qa:full`** en PetConnectBackend ejecuta seed + Newman sobre **14 carpetas** (Auth … Smoke + Pets cleanup).
- Plan de pruebas: `Documentación/Informes/Informe 3 Pet Connect.pdf` e `Informe-Estado-QA-Sprint6.md`.
- Documentación operativa: `PetConnectBackend/postman/README.md`.

### Alcance y fuera de alcance

- **Incluye:** dueños, veterinarias, paseadores/cuidadores, ficha médica, mapa, agendamiento, chatbot, reseñas, panel admin.
- **Fuera de alcance:** app nativa, pasarela de pago, integración con sistemas clínicos externos, telemedicina estricta.

*Última actualización: junio 2026 — cierre del repositorio de documentación académica.*
