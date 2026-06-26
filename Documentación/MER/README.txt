MER — Modelo Entidad-Relación
==============================

Modelo de datos de PetConnect. El backend implementado usa MongoDB Atlas (NoSQL)
con colecciones Mongoose; los diagramas incluyen modelos conceptuales relacionales
y vistas de arquitectura de alto nivel.

Diagramas en esta carpeta
-------------------------
  Diagrama clases.png
    Modelo entidad-relación de clases: Usuario, Dueño, Mascota, Veterinaria,
    FichaMedica, Cita, Servicio, Calificacion, Notificacion, Mensaje, etc.

  Diagrama Alto Nivel V1.png
    Arquitectura cliente-servidor: actores, frontend React, backend Node.js,
    MongoDB Atlas, módulos Auth, Mapa, Agendamiento, Ficha médica, Perfiles/Reseñas.

  Diagrama de Alto Nivel V2.png
    Arquitectura detallada: stack por capa (PWA/Vercel, API/Render, Atlas),
    integraciones OpenStreetMap, JWT, OpenAI, Nodemailer.

Implementación en código (MongoDB)
----------------------------------
Colecciones Mongoose en PetConnectBackend/src/models/:

  User, Pet, Appointment, AvailabilitySlot, ClinicalEncounter, Review,
  ClinicService, AgendaSlotOmit, ReviewReport, AuditLog

Relaciones conceptuales (resumen)
---------------------------------
  User 1 — N Pet
  User (proveedor) 1 — N AvailabilitySlot, ClinicService
  Pet 1 — N Appointment, ClinicalEncounter
  Appointment N — 1 AvailabilitySlot (slot reservado)
  Appointment 1 — 0..1 Review, 0..1 ClinicalEncounter

Referencia técnica
------------------
  Documentación/Desarrollo/api-rest-resumen.md
