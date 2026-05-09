---
title: "Cómo solucionar el error 0x00000709 de impresora compartida en Windows"
date: 2026-04-29
description: "¿Aparece el error 0x00000709 al conectar una impresora compartida en Windows 10 o Windows 11? Esta guía paso a paso lo resuelve añadiendo credenciales de Windows y modificando un valor del registro."
tags: ["windows", "error 0x00000709", "impresora compartida", "administrador de credenciales", "registro", "windows 10", "windows 11", "solución de problemas"]
categories: ["Solución de problemas"]
cover:
  image: "/images/printer-error-0x00000709/step1.png"
  alt: "Cuadro de diálogo de error de Windows con el error 0x00000709 — La operación no se pudo completar"
  caption: "El error 0x00000709 aparece cuando Windows no puede conectarse a una impresora compartida en la red"
  relative: false
---

# Cómo solucionar el error 0x00000709 de impresora compartida en Windows

El error 0x00000709 es uno de los errores de impresora compartida más frecuentes en Windows 10 y Windows 11. Impide conectarse a una impresora compartida en la red y puede aparecer de repente, a menudo tras una actualización de Windows. La buena noticia es que este error se puede solucionar en pocos minutos sin software de terceros.

---

## ¿Qué es el error 0x00000709?

Al intentar conectarse a una impresora compartida en la red local, Windows puede mostrar:

> **La operación no se pudo completar (Error 0x00000709). Compruebe el nombre de la impresora y asegúrese de que está conectada a la red.**

![Error 0x00000709 en Windows](/images/printer-error-0x00000709/step1.png)

El error 0x00000709 es un error de permisos o autenticación. Windows intenta comunicarse con el equipo host de la impresora a través de la red, pero es bloqueado por la falta de credenciales o una restricción del protocolo RPC.

---

## ¿Por qué ocurre el error 0x00000709?

Existen tres causas principales:

- **Credenciales de red faltantes** — Windows no tiene guardado un nombre de usuario/contraseña para el equipo host, por lo que se deniega el acceso.
- **Restricción del protocolo RPC** — Tras ciertas actualizaciones de seguridad de Windows, el método de comunicación RPC predeterminado entre equipos cambió, interrumpiendo las conexiones a impresoras compartidas.
- **Cuenta de invitado no reconocida** — La cuenta de invitado del host no es considerada de confianza por el equipo cliente, lo que provoca un fallo de autenticación silencioso.

Este error aparece con frecuencia en clientes **Windows 10** y **Windows 11** que se conectan a una impresora compartida desde otro PC.

---

## Solución

La corrección tiene dos partes: primero añadir las credenciales del host al Administrador de credenciales de Windows y luego ajustar un valor del registro RPC en el equipo cliente.

### Parte 1 — Añadir credenciales de Windows

**1.** Abra el **Panel de control**, configure la vista en **Iconos pequeños** y haga clic en **Administrador de credenciales**.

![Panel de control — Administrador de credenciales](/images/printer-error-0x00000709/step2.png)

**2.** Seleccione **Credenciales de Windows** y haga clic en **Agregar una credencial de Windows**.

![Administrador de credenciales — Agregar una credencial de Windows](/images/printer-error-0x00000709/step3.png)

**3.** En el campo **Dirección de Internet o de red**, introduzca la dirección IP del PC host (ej. `192.168.1.100`). Establezca el **Nombre de usuario** como `guest` y deje el campo **Contraseña** vacío. Haga clic en **Aceptar**.

![Agregar credencial de Windows — introducir IP del host y nombre de usuario invitado](/images/printer-error-0x00000709/step4.png)

> **Consejo:** Puede encontrar la dirección IP del PC host ejecutando `ipconfig` en un símbolo del sistema en ese equipo.

### Parte 2 — Modificar el registro

**4.** Presione **Win + R**, escriba `regedit` y presione Enter para abrir el **Editor del Registro**.

![Cuadro de diálogo Ejecutar con regedit](/images/printer-error-0x00000709/step5.png)

**5.** Navegue hasta la siguiente clave. Si la subclave `RPC` no existe, créela manualmente bajo `Printers`:

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Haga clic derecho en el panel derecho y seleccione **Nuevo → Valor DWORD (32 bits)**.

![Editor del Registro — Crear nuevo valor DWORD bajo la clave RPC](/images/printer-error-0x00000709/step6.png)

**6.** Cambie el nombre del nuevo valor a `RpcUseNamedPipeProtocol` y establezca sus **datos de valor** en `1`. Haga clic en **Aceptar**.

![Editor del Registro — RpcUseNamedPipeProtocol establecido en 1](/images/printer-error-0x00000709/step7.png)

**7.** Una vez completados todos los pasos, **reinicie el equipo** e intente conectarse de nuevo a la impresora compartida. El error 0x00000709 ya no debería aparecer.

---

## Preguntas frecuentes

**P: ¿Esta solución funciona en Windows 10 y Windows 11?**
Sí. El error 0x00000709 ocurre en Windows 10 y Windows 11. Los pasos descritos se aplican a ambas versiones.

**P: ¿Necesito hacer cambios en el PC host de la impresora?**
Generalmente no. La corrección se aplica en el PC **cliente**. Asegúrese, no obstante, de que la impresora está correctamente compartida en el host y de que la cuenta de invitado está habilitada allí.

**P: El error persiste después de seguir todos los pasos. ¿Qué más puedo intentar?**
- Verifique que la dirección IP del PC host en el Administrador de credenciales sea correcta.
- Asegúrese de que el firewall del PC host no bloquee el uso compartido de archivos e impresoras.
- Intente conectarse usando el nombre del PC host en lugar de su dirección IP.
- Desinstale y reinstale el controlador de la impresora en el equipo cliente.

---

## Resumen

El error 0x00000709 al conectar una impresora compartida en Windows 10 o Windows 11 es causado generalmente por credenciales de red faltantes o una restricción del protocolo RPC. La corrección en dos pasos:

1. Añadir la IP del host y la cuenta **guest** al **Administrador de credenciales de Windows**
2. Crear el valor de registro **RpcUseNamedPipeProtocol** = `1` en `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

---

## Artículos relacionados

- [Solucionar el error de impresora compartida 0x0000011b en Windows](/es/posts/printer-sharing-error-0x0000011b/)
- [Solucionar el error de impresora compartida 0x000003e3 en Windows 10](/es/posts/printer-sharing-error-0x000003e3/)
- [Solucionar el error de impresora compartida 0x00000040 en Windows](/es/posts/printer-sharing-error-0x00000040/)
