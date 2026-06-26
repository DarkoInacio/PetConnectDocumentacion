Scripts de base de datos
==========================

PetConnect usa MongoDB Atlas (NoSQL). Los scripts operativos viven en el
repositorio PetConnectBackend/scripts/ — no se duplican en este repositorio.

Scripts disponibles (PetConnectBackend)
---------------------------------------
  seed-smoke.js              Datos idempotentes para smoke tests Newman/Postman
  seed-qa.js                 Datos para suite QA TCP-001
  create-admin.js            Creación de usuario administrador
  migrate-clinic-services.js Migración de servicios de clínica
  run-smoke.js               Orquesta seed + Newman (carpeta Smoke)
  run-qa.js                  Orquesta seed:qa + Newman (14 carpetas QA)
  run-qa-newman.js           Ejecutor Newman para QA
  wait-for-health.js         Espera /health antes de pruebas
  start-with-dns.cjs         Arranque con resolución DNS (entornos CI)

Comandos npm relacionados (en PetConnectBackend)
------------------------------------------------
  npm run seed:smoke
  npm run seed:qa
  npm run seed:admin
  npm run migrate:clinic-services
  npm run test:smoke:full
  npm run test:qa:full

Modelo de datos
---------------
  PetConnectBackend/src/models/   Esquemas Mongoose (User, Pet, Appointment…)
  Documentación/MER/              Diagramas entidad-relación y arquitectura

Nota: la persistencia es documental (MongoDB), no relacional SQL. El modelo
conceptual relacional de los diagramas MER se implementa como colecciones
Mongoose en el backend.
