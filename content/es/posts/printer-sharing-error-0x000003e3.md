---
title: "Cómo solucionar el error 0x000003e3 de impresora compartida en Windows 10"
date: 2026-04-28
description: "¿Aparece el error 0x000003e3 al conectar una impresora compartida en Windows 10? Esta guía le explica cómo ajustar la directiva de seguridad local y la directiva de grupo para restablecer la conexión."
tags: ["windows 10", "error 0x000003e3", "impresora compartida", "directiva de seguridad", "directiva de grupo", "solución de problemas"]
categories: ["Solución de problemas"]
cover:
  image: "/images/printer-error-0x000003e3/step1.png"
  alt: "Cuadro de diálogo de error de Windows con el error 0x000003e3 al conectar una impresora compartida"
  caption: "El error 0x000003e3 aparece cuando Windows 10 no puede conectarse a una impresora compartida en la red local"
  relative: false
---

## Descripción del problema

En Windows 10, al intentar conectarse a una impresora compartida en la red local, la conexión falla con el siguiente código de error:

> **Error 0x000003e3**

![Error 0x000003e3 en Windows 10](/images/printer-error-0x000003e3/step1.png)

Este es un error común de permisos o autenticación al conectarse a impresoras compartidas. Generalmente es causado por acceso denegado, una anomalía en el servicio o un problema de protocolo de red. Siga los pasos a continuación para resolver el problema.

---

## Solución

### Paso 1 — Modificar la directiva de seguridad local

**1.** Abra **Panel de control → Programas → Activar o desactivar las características de Windows** y habilite **Compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS**.

![Habilitar SMB 1.0/CIFS en características de Windows](/images/printer-error-0x000003e3/step2.png)

**2.** Presione **Win + R**, escriba `secpol.msc` y presione Enter para abrir el editor de **Directiva de seguridad local**.

![Cuadro de diálogo Ejecutar con secpol.msc](/images/printer-error-0x000003e3/step3.png)

**3.** Vaya a **Directivas locales → Asignación de derechos de usuario → Tener acceso a este equipo desde la red**. Agregue la cuenta **Invitado** local.  
Luego vaya a **Denegar el acceso desde la red a este equipo** y **elimine** la cuenta Invitado de esa lista.

![Directiva de seguridad local — Asignación de derechos de usuario](/images/printer-error-0x000003e3/step4.png)

**4.** Vaya a **Directivas locales → Opciones de seguridad → Acceso de red: modelo de seguridad y uso compartido para cuentas locales** y establézcalo en **Solo invitado: los usuarios locales se autentican como invitados**.

![Directiva de seguridad local — Modelo de uso compartido establecido en Solo invitado](/images/printer-error-0x000003e3/step5.png)

---

### Paso 2 — Modificar la directiva de grupo local

**1.** Presione **Win + R**, escriba `gpedit.msc` y presione Enter para abrir el **Editor de directiva de grupo local**.

![Cuadro de diálogo Ejecutar con gpedit.msc](/images/printer-error-0x000003e3/step6.png)

**2.** Vaya a **Configuración del equipo → Configuración de Windows → Configuración de seguridad → Directivas locales → Opciones de seguridad**. Busque la directiva **Cuentas: limitar el uso de cuentas locales con contraseñas en blanco solo para inicios de sesión de consola**, haga doble clic en ella, seleccione **Deshabilitado** y haga clic en **Aceptar**.

![Directiva de grupo — Deshabilitar restricción de contraseña en blanco](/images/printer-error-0x000003e3/step7.png)

**3.** Abra **Administración de equipos**, luego vaya a **Herramientas del sistema → Usuarios y grupos locales → Usuarios**. Haga doble clic en **Invitado**, haga clic en la pestaña **Miembro de**, luego en **Agregar**. En el cuadro de diálogo, escriba `users` y haga clic en **Aceptar**.

![Administración de equipos — Agregar Invitado al grupo Usuarios](/images/printer-error-0x000003e3/step8.png)

**4.** De vuelta en la ventana Propiedades de invitado, en **Miembro de**, seleccione **Usuarios**, haga clic en **Agregar**, luego en **Aplicar** y **Aceptar**.

![Propiedades de invitado — Pestaña Miembro de mostrando el grupo Usuarios](/images/printer-error-0x000003e3/step9.png)

**5.** Una vez completados todos los pasos, **reinicie el equipo** e intente agregar la impresora compartida de nuevo.

---

## Resumen

El error 0x000003e3 al conectar una impresora compartida en Windows 10 generalmente está causado por configuraciones incorrectas de directiva de seguridad o de grupo. La solución incluye:

- Habilitar la compatibilidad con SMB 1.0/CIFS
- Conceder al Invitado derechos de acceso a la red
- Establecer el modelo de seguridad de uso compartido en "Solo invitado"
- Deshabilitar la restricción de inicio de sesión de consola para contraseñas en blanco
- Agregar la cuenta Invitado al grupo local Usuarios

Si el error persiste después de seguir todos los pasos, compruebe también:

- Que la conexión LAN funciona correctamente (el host y el cliente deben estar en la misma subred y poder hacerse ping mutuamente)
- Que el equipo host de la impresora compartida tiene la configuración correspondiente
- Desinstalar y reinstalar el controlador de la impresora e intentarlo de nuevo
