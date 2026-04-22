# PetConnect — Documentación del proyecto

PetConnect es una Progressive Web App diseñada para centralizar la gestión de salud animal y facilitar la conexión entre dueños de mascotas, centros veterinarios y cuidadores en Chile. La solución resuelve la dispersión de información médica y las dificultades de agendamiento mediante un ecosistema digital integrado.

## Documento oficial de definición e identificación del proyecto

El alcance, objetivos, equipo, metodología (Scrum), plan de trabajo por sprints, stack tecnológico y evidencias acordadas con la asignatura están consignados en:

**`Gestión/Definicion-del-Proyecto/1.1.2-Documento-registro-definicion-identificacion-proyecto.pdf`**

*(Título original del entregable: «1.1.2 Documento de registro de definición e identificación del proyecto», TPY 1101 — Taller Aplicado de Programación.)*

Las subcarpetas de este repositorio de documentación usan archivos **`README.txt`** (texto plano) como guía; el único archivo **Markdown** en la raíz es este **`README.md`**.

---

## Estructura de carpetas

| Carpeta | Propósito |
|---------|-----------|
| **Documentación** | Informes, UML, wireframes, MER, diagramas Gantt |
| **Producto** | Scripts de datos, referencia al código fuente y librerías |
| **Gestión** | Definición del proyecto, lista de integrantes |

---

## Progreso actual del proyecto

*Estado alineado al plan del documento 1.1.2 y al avance técnico conocido del repositorio (backend / API). Las fechas de sprint deben ajustarse al calendario real del semestre.*

### Organización y metodología

- Equipo de **cuatro integrantes** con roles Scrum (Scrum Master, Product Owner, desarrollo frontend/backend) según el registro del documento.
- Metodología **Scrum**: Product Backlog, sprints, revisiones y retrospectivas planificadas en el documento oficial.
- Infraestructura prevista: **MongoDB Atlas**, despliegue frontend (p. ej. Vercel) y backend (p. ej. Railway/Render), repositorio **GitHub** con flujo tipo GitFlow.

### Por sprint (según planificación del documento)

| Fase | Contenido principal | Estado referencial |
|------|----------------------|---------------------|
| **Sprint 0** | Constitución del equipo, setup de repos, Atlas, levantamiento de épicas/HU, modelado de datos | Completado en lo esencial (documentación y arranque técnico) |
| **Sprint 1 — Autenticación y perfil** | HU-01 a HU-05, HU-29 (registro dueño/proveedor, login JWT, recuperación de contraseña, edición de perfil, cierre de sesión) | **Finalizado** según el documento (entregables de autenticación en backend) |
| **Sprint 2 — Búsqueda, mapa y reservas** | HU-06 a HU-15 (perfiles públicos, búsqueda, mapa, citas, agenda, historial, cancelación, recordatorios) | **En curso / parcial**: backend con agenda por slots, citas, proveedores, reseñas; **HU-06** marcada en el documento como «en curso» en un momento dado del plan |
| **Sprint 3 — Ficha médica y reseñas** | HU-20 a HU-22 (mascota, atención veterinaria, historial), HU-16 a HU-19 (reseñas) | **Avanzado en backend**: modelos y API de mascotas, encuentros clínicos, historial y exportación PDF; integración con citas confirmadas |
| **Sprint 4 — Chatbot** | HU-23 a HU-25 | Pendiente / no cubierto en el alcance del backend descrito |
| **Sprint 5 — Panel admin y PWA** | HU-26 a HU-28, HU-30, HU-31 | Parcial: aprobación de proveedores (admin); PWA y paneles completos según frontend |
| **Sprint 6 — Pruebas y despliegue** | Pruebas, bugs, UX, producción | En curso según calendario del equipo |
| **Sprint 7 — Cierre** | Review y retrospectiva | Pendiente |

### Alcance y fuera de alcance (recordatorio del documento)

- **Incluye:** tipos de usuario (dueño, veterinaria, paseador/cuidador), ficha médica por mascota, mapa y georreferenciación, agendamiento, chatbot, reseñas, panel de administración (según diseño).
- **Fuera de alcance:** app nativa, pasarela de pago, integración con sistemas clínicos externos, telemedicina en el sentido estricto del documento.

### Próximos pasos sugeridos

1. Mantener el PDF de definición **versionado** en `Gestión/Definicion-del-Proyecto/` y actualizar este README si cambia el alcance.
2. Completar entregables visuales en **Documentación** (UML, MER, Gantt) según el plan del docente.
3. Alinear el **frontend** con la API (variables de entorno apuntando a MongoDB Atlas y URL del backend).
4. Cerrar evidencias listadas en la sección 7 del PDF (organización, backlog Jira, diagramas, capturas de repositorio, etc.).

---

*Última actualización de esta sección de progreso: coherente con el documento 1.1.2 y el estado del desarrollo; revisar al cierre de cada sprint.*
