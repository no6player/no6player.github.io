---
title: "Cómo solucionar el error 0x00000040 de impresora compartida en Windows"
date: 2026-04-29
description: "¿Los clientes Windows 7 reciben el error 0x00000040 al conectarse a una impresora compartida desde Windows 11? Esta guía explica cómo habilitar SMB 1.0 y modificar el registro para resolver el problema."
tags: ["windows", "error 0x00000040", "impresora compartida", "windows 7", "windows 11", "registro", "SMB", "solución de problemas"]
categories: ["Solución de problemas"]
cover:
  image: "/images/printer-error-0x00000040/step1.png"
  alt: "Cuadro de diálogo de error de Windows con el error 0x00000040 — El nombre de red especificado ya no está disponible"
  caption: "El error 0x00000040 aparece cuando un cliente Windows 7 no puede conectarse a una impresora compartida desde Windows 11"
  relative: false
---

## Descripción del problema

Cuando un PC con Windows 11 se usa como host de impresora compartida, los clientes Windows 7 que intentan conectarse reciben el siguiente mensaje de error:

> **La operación no se pudo completar (Error 0x00000040). El nombre de red especificado ya no está disponible.**

![Error 0x00000040 en Windows](/images/printer-error-0x00000040/step1.png)

Este error suele estar causado por un servicio SMB deshabilitado, un nombre de recurso compartido caducado, una conexión de red interrumpida o una incompatibilidad de protocolo RPC entre los dos sistemas operativos. La solución consiste en habilitar SMB 1.0 y modificar dos valores del registro en el host Windows 11.

---

## Solución

**1.** Abra **Panel de control → Programas → Activar o desactivar las características de Windows** y habilite **Compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS**.

![Habilitar SMB 1.0/CIFS en características de Windows](/images/printer-error-0x00000040/step2.png)

**2.** Presione **Win + R**, escriba `regedit` y presione Enter para abrir el **Editor del Registro**.

![Cuadro de diálogo Ejecutar con regedit](/images/printer-error-0x00000040/step3.png)

**3.** Navegue hasta la siguiente clave (créela si no existe):

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Haga clic derecho en el panel derecho y cree un nuevo **Valor DWORD (32 bits)**.

![Editor del Registro — Crear nuevo valor DWORD bajo la clave RPC](/images/printer-error-0x00000040/step4.png)

**4.** Cambie el nombre del nuevo valor a `RpcProtocols` y establezca sus **datos de valor** en `7`.

![Editor del Registro — RpcProtocols establecido en 7](/images/printer-error-0x00000040/step5.png)

**5.** Haga clic derecho nuevamente en el panel derecho y cree otro **Valor DWORD (32 bits)**. Renómbrelo como `ForceKerberosForRpc` y establezca sus **datos de valor** en `1`.

![Editor del Registro — ForceKerberosForRpc establecido en 1](/images/printer-error-0x00000040/step6.png)

**6.** Una vez completados todos los pasos, **reinicie el equipo** e intente conectarse de nuevo a la impresora compartida.

---

## Resumen

El error 0x00000040 al conectarse a una impresora compartida es un problema frecuente en entornos mixtos Windows 7/Windows 11. Generalmente está causado por un servicio SMB deshabilitado o una incompatibilidad de protocolo RPC. La solución incluye dos pasos:

- Habilitar la **compatibilidad con SMB 1.0/CIFS** en las características de Windows
- Agregar los valores de registro **RpcProtocols** (`7`) y **ForceKerberosForRpc** (`1`) en `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

Tras el reinicio, la conexión a la impresora compartida debería funcionar con normalidad.
