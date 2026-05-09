---
title: "Cómo solucionar el error 0x00000709 de impresora compartida en Windows"
date: 2026-04-29
description: "¿Aparece el error 0x00000709 al conectar una impresora compartida en Windows? Esta guía explica cómo resolverlo añadiendo credenciales de Windows y modificando un valor del registro."
tags: ["windows", "error 0x00000709", "impresora compartida", "administrador de credenciales", "registro", "solución de problemas"]
categories: ["Solución de problemas"]
cover:
  image: "/images/printer-error-0x00000709/step1.png"
  alt: "Cuadro de diálogo de error de Windows con el error 0x00000709 — La operación no se pudo completar"
  caption: "El error 0x00000709 aparece cuando Windows no puede conectarse a una impresora compartida en la red"
  relative: false
---

## Descripción del problema

Al intentar conectarse a una impresora compartida en la red, la conexión falla con el siguiente mensaje de error:

> **La operación no se pudo completar (Error 0x00000709). Compruebe el nombre de la impresora y asegúrese de que está conectada a la red.**

![Error 0x00000709 en Windows](/images/printer-error-0x00000709/step1.png)

El error 0x00000709 es un error frecuente de conexión a impresoras compartidas en Windows. Generalmente está causado por una actualización del sistema, una configuración incorrecta del protocolo RPC o un problema de autenticación de credenciales. La solución consiste en añadir credenciales de Windows y modificar un valor del registro.

---

## Solución

**1.** Abra el **Panel de control**, configure la vista en **Iconos pequeños** y haga clic en **Administrador de credenciales**.

![Panel de control — Administrador de credenciales](/images/printer-error-0x00000709/step2.png)

**2.** Seleccione **Credenciales de Windows** y haga clic en **Agregar una credencial de Windows**.

![Administrador de credenciales — Agregar una credencial de Windows](/images/printer-error-0x00000709/step3.png)

**3.** En el campo **Dirección de Internet o de red**, introduzca la dirección IP del PC host. Establezca el **Nombre de usuario** como `guest` y deje el campo **Contraseña** vacío. Haga clic en **Aceptar**.

![Agregar credencial de Windows — introducir IP del host y nombre de usuario invitado](/images/printer-error-0x00000709/step4.png)

**4.** Presione **Win + R**, escriba `regedit` y presione Enter para abrir el **Editor del Registro**.

![Cuadro de diálogo Ejecutar con regedit](/images/printer-error-0x00000709/step5.png)

**5.** Navegue hasta la siguiente clave (créela si no existe):

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Haga clic derecho en el panel derecho y cree un nuevo **Valor DWORD (32 bits)**.

![Editor del Registro — Crear nuevo valor DWORD bajo la clave RPC](/images/printer-error-0x00000709/step6.png)

**6.** Cambie el nombre del nuevo valor a `RpcUseNamedPipeProtocol` y establezca sus **datos de valor** en `1`.

![Editor del Registro — RpcUseNamedPipeProtocol establecido en 1](/images/printer-error-0x00000709/step7.png)

**7.** Una vez completados todos los pasos, **reinicie el equipo** e intente conectarse de nuevo a la impresora compartida.

---

## Resumen

El error 0x00000709 al conectarse a una impresora compartida es un problema habitual en Windows, normalmente causado por una actualización del sistema, una configuración incorrecta del protocolo RPC o un fallo de autenticación. La solución incluye dos pasos:

- Añadir la dirección IP del PC host y la cuenta **guest** en el **Administrador de credenciales de Windows**
- Crear el valor de registro **RpcUseNamedPipeProtocol** (`1`) en `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

Tras reiniciar, la conexión a la impresora compartida debería funcionar con normalidad.
