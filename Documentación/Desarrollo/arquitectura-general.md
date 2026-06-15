# Arquitectura general

## Qué es PetConnect

PetConnect es una **Progressive Web App (PWA)** orientada al mercado chileno que centraliza:

- **Ficha médica** de mascotas (historial, atenciones clínicas, export PDF).
- **Agendamiento** de citas en clínicas veterinarias (slots de agenda).
- **Búsqueda y mapa** de proveedores (veterinarias, paseadores, cuidadores).
- **Reseñas** ligadas a citas completadas.
- **Chatbot Vetto** (triaje orientativo vía IA, proxy en backend).
- **Panel de administración** (aprobación de proveedores, reportes de reseñas).

## Modelo cliente–servidor

```
┌─────────────────────┐         HTTPS / JSON          ┌──────────────────────────┐
│  PetConnect (PWA)   │  ◄──────────────────────────► │  PetConnectBackend (API) │
│  React + Vite       │         JWT en header         │  Express + Mongoose      │
│  Puerto 5173 (dev)  │                               │  Puerto 3000 (dev)       │
└─────────────────────┘                               └────────────┬─────────────┘
                                                                   │
                    ┌──────────────────────────────────────────────┼──────────────┐
                    │                                              ▼              │
                    │  MongoDB Atlas          OpenAI API          Nodemailer     │
                    │  (datos)                (chat Vetto)        (correos)      │
                    └─────────────────────────────────────────────────────────────┘
```

- El **frontend nunca** expone claves de OpenAI ni credenciales de base de datos.
- La autenticación usa **JWT** (`Authorization: Bearer <token>`).
- CORS restringe orígenes según `CLIENT_URL` (backend) y despliegue en Vercel (frontend).

## Roles de usuario

| Rol | Descripción | Ejemplos en la app |
|-----|-------------|-------------------|
| **Dueño** | Registra mascotas, agenda citas, ve ficha médica, deja reseñas | `/cuenta/*`, `/mascotas/*`, `/agendar` |
| **Proveedor** | Veterinaria, paseador o cuidador; agenda, atenciones, perfil público | `/proveedor/*` |
| **Administrador** | Aprueba/rechaza proveedores, gestiona reportes de reseñas | `/admin/*` |

Un mismo usuario puede tener rol `proveedor` además de `dueno` según el modelo `User` en backend.

## Despliegue previsto

| Componente | Plataforma típica | Notas |
|------------|-------------------|--------|
| Frontend PWA | **Vercel** | SPA con rewrite a `index.html` |
| Backend API | **Render** (u otro Node host) | Health check en `/health` |
| Base de datos | **MongoDB Atlas** | URI en `MONGODB_URI` |

En desarrollo local ambos servicios corren en la máquina del desarrollador apuntando a la misma base Atlas o a una instancia local de MongoDB.

## Zona horaria y locale

La agenda clínica y los bloques horarios usan **America/Santiago** (Luxon en backend). El frontend incluye utilidades de fecha/hora para Chile (`src/constants/chileTime.js`).
