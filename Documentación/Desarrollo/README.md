# Desarrollo — PetConnect

Guía técnica general del ecosistema PetConnect: arquitectura, tecnologías, instalación local, estructura de repositorios y pruebas.

Esta carpeta complementa la documentación académica (UML, MER, Gantt, Informes) y los entregables en `Gestión/Definicion-del-Proyecto/`.

## Índice

| Documento | Contenido |
|-----------|-----------|
| [arquitectura-general.md](./arquitectura-general.md) | Visión del sistema, roles, flujo cliente–servidor |
| [stack-tecnologico.md](./stack-tecnologico.md) | Tecnologías frontend, backend, infraestructura |
| [instalacion-local.md](./instalacion-local.md) | Requisitos, variables de entorno, cómo levantar ambos proyectos |
| [estructura-repositorios.md](./estructura-repositorios.md) | Carpetas de **PetConnect** y **PetConnectBackend** |
| [api-rest-resumen.md](./api-rest-resumen.md) | Prefijos de API, módulos y autenticación |
| [pruebas-y-calidad.md](./pruebas-y-calidad.md) | Jest, Newman, Vitest, Playwright, QA TCP-001 |

## Repositorios de código

| Repositorio | Descripción |
|-------------|-------------|
| **PetConnect** | PWA frontend (React + Vite) |
| **PetConnectBackend** | API REST (Node.js + Express + MongoDB) |
| **PetConnectDocumentacion** | Este repositorio (documentación, gestión, entregables) |

La documentación detallada de Postman/Newman QA vive en el backend: `PetConnectBackend/postman/README.md`.  
El plan de pruebas funcional: `PetConnect/TEST_PLAN.md`.
