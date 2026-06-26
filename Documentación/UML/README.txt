UML
===

Diagramas de modelado unificado del proyecto PetConnect.

Archivos en esta carpeta
------------------------
Diagramas de actividad (PNG):

  ACTUsuario.png
    Registro de usuario. Swimlanes USUARIO / SISTEMA: ingreso de credenciales,
    validación de correo, creación de usuario, envío de datos y redirección
    a pantalla principal.

  ACTECrearEvento.png
    Crear evento médico. Flujo: inicio de sesión, menú principal, verificación
    de mascota registrada, agregar evento médico y creación de ficha médica.

  ACTEditar.png
    Editar datos de mascota. Flujo: inicio de sesión, menú principal; si hay
    mascota registrada edita datos, si no la registra e ingresa datos;
    confirmación de datos.

  ACTRecordatorio.png
    Gestión de recordatorios. Flujo: inicio de sesión, menú principal,
    mascota registrada; según exista recordatorio, agregar o editar;
    confirmación de datos.

Documentación técnica relacionada
---------------------------------
  Documentación/Desarrollo/arquitectura-general.md
  Documentación/Desarrollo/api-rest-resumen.md
