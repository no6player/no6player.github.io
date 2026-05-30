---
title: "Cómo solucionar el error de impresora 0x00000032 en Windows 10 y 11"
date: 2026-05-30
description: "¿Aparece el error 0x00000032 (La solicitud no es compatible) al instalar o conectar una impresora en Windows 10 o 11? Esta guía cubre todas las causas y soluciones: limpieza de controladores, restablecimiento del Spooler de impresión, directiva de grupo y reparación de impresora compartida."
tags: ["windows", "error 0x00000032", "error de impresora", "la solicitud no es compatible", "spooler de impresión", "controlador de impresora", "directiva de grupo", "windows 10", "windows 11", "solución de problemas"]
categories: ["Solución de problemas"]
cover:
  image: "/images/printer-error-0x00000032/step1.png"
  alt: "Cuadro de diálogo de error de Windows que muestra el error 0x00000032 — La solicitud no es compatible"
  caption: "El error 0x00000032 aparece cuando Windows se niega a cargar o instalar el controlador de impresora debido a una incompatibilidad o un controlador antiguo bloqueado"
  relative: false
---

# Cómo solucionar el error de impresora 0x00000032 en Windows 10 y 11

El error 0x00000032 — «La solicitud no es compatible» — es uno de los errores de controlador de impresora más comunes en Windows 10 y Windows 11. Ocurre al instalar una impresora USB local, conectarse a una impresora de red compartida o actualizar un controlador de impresora. A diferencia de los errores de red, el error 0x00000032 es un fallo de clase de controlador: Windows se niega a cargar, reemplazar o instalar el controlador de impresora actual. Esta guía explica cada causa y detalla cada solución en un orden lógico.

---

## ¿Qué es el error 0x00000032?

Cuando instala o se conecta a una impresora, Windows muestra:

> **La operación no se pudo completar (Error 0x00000032). La solicitud no es compatible.**

![Cuadro de diálogo de error de Windows que muestra el error 0x00000032 — La solicitud no es compatible](/images/printer-error-0x00000032/step1.png)

**Los síntomas típicos incluyen:**
- Agregar una impresora USB local falla inmediatamente con este error
- Conectarse a una impresora compartida en la LAN falla en un PC cliente pero funciona en otros
- Actualizar el controlador de impresora desencadena el error incluso después de una descarga reciente
- El controlador genérico/universal se instala correctamente, pero el controlador oficial devuelve 0x00000032

---

## ¿Por qué ocurre el error 0x00000032?

Hay seis causas comunes:

1. **Incompatibilidad de controladores (causa más común)** — La versión del controlador no coincide con la del PC anfitrión, se mezcla un controlador de 32 bits con un sistema de 64 bits (o viceversa), o el controlador no es compatible con la versión de Windows. Windows considera la solicitud de instalación inválida y la bloquea.
2. **Archivos de controlador residuales que bloquean la instalación** — Los archivos y entradas del registro de un controlador instalado anteriormente no fueron eliminados completamente. El controlador está retenido por el sistema y no puede sobrescribirse, lo que desencadena el error 0x00000032.
3. **Fallo del servicio Spooler de impresión** — El proceso Spooler está congelado o mantiene archivos de controlador abiertos, impidiendo que se escriba o cargue un nuevo controlador.
4. **Bloqueo de la directiva de instalación de controladores del sistema** — La directiva de seguridad de Windows bloquea la instalación de controladores sin firmar o de terceros.
5. **Fallo de sincronización del controlador compartido** — Al conectarse a una impresora compartida en la LAN, el PC anfitrión no tiene un controlador que coincida con la versión de Windows del PC cliente (especialmente incompatibilidades de 32 bits vs. 64 bits). Windows rechaza la solicitud de descarga del controlador.
6. **Corrupción menor de archivos del sistema** — Los componentes de Windows relacionados con la impresión que faltan o están dañados hacen que la interfaz de instalación de controladores deje de funcionar.

---

## Solución

Siga los pasos a continuación en orden. La mayoría de los casos se resuelven con los pasos 1 y 2.

### Paso 1 — Reiniciar el Spooler de impresión y limpiar la caché de impresión

**1.** Presione **Win + R**, escriba `services.msc` y presione Entrar.

**2.** Encuentre **Cola de impresión (Print Spooler)** en la lista, haga clic derecho sobre él y seleccione **Detener**.

![Ventana Servicios — detener el servicio Cola de impresión](/images/printer-error-0x00000032/step2.png)

**3.** Presione **Win + R**, escriba `C:\Windows\System32\spool\PRINTERS` y presione Entrar. Elimine **todos los archivos** dentro de la carpeta (no elimine la carpeta en sí).

![Explorador de archivos — eliminar todos los archivos de la carpeta de spool PRINTERS](/images/printer-error-0x00000032/step3.png)

**4.** Regrese a Servicios, haga clic derecho en **Cola de impresión (Print Spooler)**, seleccione **Iniciar** y establezca el **Tipo de inicio** en **Automático**.

---

### Paso 2 — Eliminar completamente el paquete de controlador residual

El error 0x00000032 es causado más frecuentemente por un controlador antiguo bloqueado. Debe eliminar el paquete de controlador completo — no solo el dispositivo de impresora:

**1.** Abra **Panel de control → Dispositivos e impresoras**. Haga clic derecho en cada impresora fallida o duplicada y seleccione **Quitar dispositivo**.

**2.** Presione **Win + R**, escriba `printui /s /t2` y presione Entrar para abrir **Propiedades del servidor de impresión**. Cambie a la pestaña **Controladores**. Seleccione el controlador de la impresora problemática, haga clic en **Quitar** y luego elija **Quitar controlador y paquete de controladores** para eliminar todos los archivos del controlador.

![Propiedades del servidor de impresión — eliminación completa del controlador y el paquete de controladores](/images/printer-error-0x00000032/step4.png)

**3.** Reinicie el equipo. Luego descargue el controlador completo correcto del sitio web del fabricante e instálelo.

---

### Paso 3 — Ajustar la directiva del sistema para permitir la instalación de controladores

En las ediciones Windows Professional y Enterprise, la directiva de grupo puede estar bloqueando la instalación de controladores:

**1.** Presione **Win + R**, escriba `gpedit.msc` y presione Entrar. Navegue a **Configuración del equipo → Plantillas administrativas → Impresoras**.

**2.** Deshabilite **No permitir la instalación de impresoras con controladores en modo kernel** (si está habilitado).

**3.** Habilite **Permitir que el Spooler de impresión acepte conexiones de clientes**.

![Editor de directivas de grupo — ajuste de las directivas de instalación de controladores de impresora](/images/printer-error-0x00000032/step5.png)

---

### Paso 4 — Solucionar problemas de impresora compartida (conexión LAN)

Si el error 0x00000032 aparece solo al conectarse a una impresora compartida en la red local:

**1.** En el **PC anfitrión**, agregue el paquete de controladores adicional que coincida con la **versión de Windows del PC cliente** (32 bits o 64 bits). Hágalo en **Propiedades del servidor de impresión → Controladores → Agregar**.

**2.** En el **PC cliente**, habilite **Habilitar inicios de sesión de invitado no seguros**: abra **Directiva de grupo local → Configuración del equipo → Plantillas administrativas → Red → Estación de trabajo Lanman** y establezca **Habilitar inicios de sesión de invitado no seguros** en **Habilitado**.

**3.** Borre las credenciales de red obsoletas en el PC cliente: presione **Win + R**, escriba `control keymgr.dll`, abra el **Administrador de credenciales** y elimine todas las entradas relacionadas con la dirección IP o el nombre del equipo del PC anfitrión.

![Administrador de credenciales — eliminación de credenciales de red obsoletas en el PC cliente](/images/printer-error-0x00000032/step6.png)

**4.** Vuelva a conectarse usando directamente la dirección IP del PC anfitrión: `\\192.168.1.x`.

---

### Paso 5 — Reparar archivos del sistema

**1.** Busque **cmd**, haga clic derecho y seleccione **Ejecutar como administrador**.

**2.** Ejecute el siguiente comando y espere a que se complete:

```
sfc /scannow
```

![Símbolo del sistema — ejecución de sfc /scannow para reparar archivos del sistema](/images/printer-error-0x00000032/step7.png)

**3.** Reinicie el equipo e intente instalar el controlador de impresora de nuevo.

---

## Preguntas frecuentes

**P: ¿Qué significa realmente el error 0x00000032 «La solicitud no es compatible»?**
Significa que Windows está rechazando la solicitud de instalación del controlador. El sistema ha determinado que la solicitud no es válida — con mayor frecuencia porque el controlador no coincide con la versión del sistema, un controlador antiguo en conflicto está bloqueado en su lugar, o una directiva de seguridad está bloqueando la instalación.

**P: El controlador genérico se instala correctamente, pero el controlador oficial da el error 0x00000032. ¿Por qué?**
Esto casi siempre es causado por un paquete de controlador residual bloqueado de una instalación anterior. El controlador genérico usa una ruta de instalación diferente, por eso funciona. Elimine el paquete de controlador completo a través de `printui /s /t2`, reinicie y luego instale el controlador oficial.

**P: Otros equipos pueden conectarse a la impresora compartida — solo el mío obtiene el error 0x00000032. ¿Qué debo hacer?**
El problema es específico del entorno de controladores de su PC cliente. Los demás PC tienen controladores compatibles; el suyo tiene un conflicto de controlador antiguo o una incompatibilidad de versión. Limpie el controlador solo en su PC (pasos 1 y 2) — no se necesitan cambios en el PC anfitrión ni en la impresora.

**P: Reinstalé el controlador varias veces pero sigo obteniendo el error. ¿Qué me estoy perdiendo?**
Lo más probable es que eliminó el dispositivo de impresora pero no el **paquete de controladores**. Simplemente quitar el dispositivo de Dispositivos e impresoras deja los archivos del controlador en su lugar. Abra `printui /s /t2`, vaya a la pestaña Controladores y elimine el paquete de controladores completamente antes de reinstalar.

**P: ¿Una incompatibilidad de 32 bits/64 bits entre el anfitrión y el cliente puede causar el error 0x00000032?**
Sí. Si el PC anfitrión es de 64 bits y el cliente es de 32 bits, y el anfitrión no ha agregado un paquete de controladores de 32 bits, el cliente no puede descargar un controlador compatible y Windows devuelve «La solicitud no es compatible». Agregue el controlador de arquitectura faltante en el PC anfitrión en Propiedades del servidor de impresión.

---

## Resumen

El error 0x00000032 al instalar o conectar una impresora en Windows 10 o Windows 11 significa que Windows está rechazando la solicitud de instalación del controlador. No está relacionado con la red, el hardware de la impresora ni daños importantes del sistema. Corríjalo en este orden:

1. **Reiniciar el Spooler de impresión y limpiar la caché de impresión**
2. **Eliminar completamente el paquete de controlador antiguo** a través de `printui /s /t2` — esta es la solución más común
3. **Ajustar la directiva de grupo** para permitir la instalación de controladores (ediciones Professional/Enterprise)
4. **Para impresoras compartidas**: agregar controladores de arquitectura coincidentes en el anfitrión, habilitar el inicio de sesión de invitado en el cliente y borrar las credenciales obsoletas
5. **Ejecutar `sfc /scannow`** para reparar los archivos del sistema dañados

---

## Artículos relacionados

- [Cómo solucionar el error de impresora compartida 0x00000006 en Windows](/es/posts/printer-sharing-error-0x00000006/)
- [Cómo solucionar el error de impresora compartida 0x00000035 en Windows](/es/posts/printer-sharing-error-0x00000035/)
- [Cómo solucionar el error de impresora compartida 0x00000709 en Windows](/es/posts/printer-sharing-error-0x00000709/)
