Librerías
=========

Dependencias del software PetConnect. No incluir node_modules en este repositorio;
las versiones exactas están en package.json y lockfile de cada repo de código.

Requisitos de entorno
---------------------
  Node.js >= 18
  MongoDB (Atlas en producción; local o Atlas en desarrollo)

Frontend — PetConnect (package.json)
------------------------------------
  Runtime: React 19, Vite 8, ES modules
  UI: Tailwind CSS 4, Radix UI, Lucide, class-variance-authority
  Routing: react-router-dom 7
  HTTP: axios
  Mapas: leaflet, react-leaflet, react-leaflet-cluster
  PWA: vite-plugin-pwa

  Desarrollo: Vitest, Testing Library, Playwright, ESLint 9

Backend — PetConnectBackend (package.json)
------------------------------------------
  Runtime: Node.js, Express 4, CommonJS
  Datos: mongoose 8 (MongoDB)
  Seguridad: jsonwebtoken, bcryptjs, helmet, cors, express-rate-limit
  Utilidades: luxon, multer, sharp, pdfkit, nodemailer, node-cron

  Desarrollo: jest, supertest, newman, mongodb-memory-server, nodemon

Documentación ampliada
----------------------
  Documentación/Desarrollo/stack-tecnologico.md
  Documentación/Desarrollo/instalacion-local.md

Licencia del backend: MIT (ver package.json en PetConnectBackend).
