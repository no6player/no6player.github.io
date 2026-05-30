---
title: "Cómo solucionar el error de impresora compartida 0x0000007c en Windows 10 y 11"
date: 2026-05-30
description: "¿Aparece el error 0x0000007c al conectarse a una impresora compartida en Windows 10 o 11? Esta guía cubre todas las causas y soluciones: limpieza de controladores, reinicio del spooler de impresión, protocolo SMB 1.0 y adición de controladores bidireccionales en el host."
tags: ["windows", "error 0x0000007c", "impresora compartida", "controlador de impresora", "SMB", "spooler de impresión", "windows 10", "windows 11", "solución de problemas"]
categories: ["Solución de problemas"]
cover:
  image: "/images/printer-error-0x0000007c/step1.png"
  alt: "Cuadro de diálogo de error de Windows mostrando el error 0x0000007c — La operación no se pudo completar"
  caption: "El error 0x0000007c aparece cuando el cliente no puede cargar el controlador de impresora remoto debido a una incompatibilidad de controlador o protocolo SMB"
  relative: false
---

# Cómo solucionar el error de impresora compartida 0x0000007c en Windows 10 y 11

El error 0x0000007c — «La operación no se pudo completar» — es un error común en impresoras compartidas en Windows 10 y Windows 11. Ocurre cuando el PC cliente se conecta a una impresora compartida en la red LAN y Windows no puede cargar el controlador de impresora remoto. Se trata de un problema de **compatibilidad de controladores y protocolo SMB**, no de un fallo de hardware o una interrupción de la red. Esta guía explica todas las causas y recorre cada solución en orden.

---

## ¿Qué es el error 0x0000007c?

Al conectarse a una impresora compartida en la red local, Windows muestra:

> **La operación no se pudo completar (Error 0x0000007c).**

![Cuadro de diálogo de error de Windows mostrando el error 0x0000007c — La operación no se pudo completar](/images/printer-error-0x0000007c/step1.png)

**Los síntomas típicos incluyen:**
- El error aparece inmediatamente al intentar agregar una impresora compartida en la red LAN
- Los recursos compartidos de archivos de red en el mismo PC host son accesibles — solo la impresora falla
- Los PC más antiguos se conectan sin problemas a la impresora compartida; solo los PC más nuevos con Windows 10/11 reciben el error
- Eliminar y volver a agregar la impresora no resuelve el problema

---

## ¿Por qué ocurre el error 0x0000007c?

Hay seis causas comunes:

1. **Discrepancia del controlador remoto** — La versión o arquitectura del controlador de impresora (32 bits frente a 64 bits) en el PC host no coincide con lo que requiere el PC cliente. El cliente rechaza el controlador enviado como no válido.
2. **Incompatibilidad de la versión del protocolo SMB** — Windows 10/11 usa SMB 3.0 de forma predeterminada, mientras que los PC host más antiguos o los controladores heredados dependen de SMB 1.0. La incompatibilidad de protocolos hace que falle la validación de la transferencia del controlador.
3. **Bloqueo por verificación de firma del controlador** — Las versiones más recientes de Windows aplican estrictamente los requisitos de firma digital del controlador. Los controladores de impresora compartida más antiguos sin una firma válida quedan bloqueados al cargarse.
4. **Conflicto de controlador residual en el cliente** — Un controlador instalado anteriormente para el mismo modelo de impresora dejó archivos residuales que impiden que se cargue el nuevo controlador remoto.
5. **Corrupción de la caché del spooler de impresión** — Los datos corruptos de la caché del spooler impiden que el servicio reciba e instale el controlador compartido remoto.
6. **Controladores bidireccionales faltantes en el PC host** — El host solo tiene instalado un controlador para su propia versión de Windows y no ha agregado un paquete de controladores correspondiente para la arquitectura del PC cliente.

---

## Solución

Siga los pasos a continuación en orden. La mayoría de los casos se resuelven con los pasos 1 y 2.

### Paso 1 — Borrar la caché de impresión y reiniciar el spooler de impresión

**1.** Presione **Win + R**, escriba `services.msc` y presione Entrar. Busque **Cola de impresión** en la lista, haga clic derecho sobre ella y seleccione **Detener**.

![Ventana de Servicios — detener el servicio Cola de impresión](/images/printer-error-0x0000007c/step2.png)

**2.** Presione **Win + R**, escriba `C:\Windows\System32\spool\PRINTERS` y presione Entrar. Elimine **todos los archivos** dentro de la carpeta (no elimine la carpeta en sí). Luego regrese a Servicios y vuelva a iniciar la **Cola de impresión**, estableciendo el **Tipo de inicio** en **Automático**.

![Explorador de archivos — eliminar todos los archivos en la carpeta del spooler PRINTERS](/images/printer-error-0x0000007c/step3.png)

---

### Paso 2 — Eliminar completamente el controlador antiguo del PC cliente

**1.** Abra **Panel de control → Dispositivos e impresoras**. Haga clic derecho en cada impresora con error y seleccione **Quitar dispositivo**.

**2.** Presione **Win + R**, escriba `printui /s /t2` y presione Entrar para abrir las **Propiedades del servidor de impresión**. Vaya a la pestaña **Controladores**, seleccione el controlador de impresora correspondiente, haga clic en **Quitar** y elija **Quitar el controlador y el paquete de controladores** para eliminar completamente todos los archivos del controlador.

![Propiedades del servidor de impresión — eliminar completamente el controlador y el paquete de controladores](/images/printer-error-0x0000007c/step4.png)

**3.** Reinicie el equipo. Descargue el controlador completo y correcto desde el sitio web del fabricante e instálelo manualmente antes de volver a conectarse a la impresora compartida.

---

### Paso 3 — Habilitar el protocolo SMB 1.0/CIFS

Si el PC host ejecuta una versión anterior de Windows o utiliza paquetes de controladores más antiguos que dependen de SMB 1.0:

**1.** Abra **Panel de control → Programas → Activar o desactivar las características de Windows**.

**2.** Desplácese hacia abajo y marque **Compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS**. Haga clic en **Aceptar** y reinicie el equipo.

![Características de Windows — habilitar la compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS](/images/printer-error-0x0000007c/step5.png)

> **Nota de seguridad:** SMB 1.0 tiene vulnerabilidades conocidas. Actívelo solo como medida temporal y desactívelo una vez que el problema de la impresora se haya resuelto, o actualice el controlador del PC host a una versión compatible con SMB 2.0/3.0.

---

### Paso 4 — Agregar controladores bidireccionales en el PC host

Si el PC cliente tiene una arquitectura de Windows diferente (32 bits frente a 64 bits) a la del host:

**1.** En el **PC host**, abra **Dispositivos e impresoras**, haga clic derecho en la impresora compartida y seleccione **Propiedades de la impresora → Pestaña Compartir → Controladores adicionales**.

**2.** Marque la arquitectura que corresponde al **PC cliente** (por ejemplo, marque **x86** si el cliente es de 32 bits y el host es de 64 bits), luego haga clic en **Aceptar** e instale el paquete de controladores correspondiente.

Una vez que el host tenga el controlador correspondiente, vuelva a conectarse a la impresora compartida desde el PC cliente.

---

## Preguntas frecuentes

**P: ¿Qué significa el error 0x0000007c?**
El significado oficial es «el controlador de impresora no es válido o el controlador de impresora remoto no se puede cargar». En términos simples, el sistema Windows del PC cliente no acepta el controlador enviado por el host. Esto es causado por una incompatibilidad de protocolo o una discrepancia de versión de controlador, no por un fallo de hardware.

**P: Puedo hacer ping al host y acceder a carpetas compartidas, pero la impresora da el error 0x0000007c. ¿Por qué?**
El acceso a carpetas compartidas solo requiere conectividad de red SMB. Las conexiones a impresoras compartidas requieren tanto **compatibilidad de protocolo como compatibilidad de controladores**. Una red que funciona no garantiza la compatibilidad de controladores. Habilite SMB 1.0 e instale manualmente el controlador de impresora local para resolver esto.

**P: Los PC antiguos se conectan sin problemas a la impresora compartida. ¿Por qué mi nuevo PC con Windows 10/11 recibe el error 0x0000007c?**
Windows 10/11 deshabilita SMB 1.0 de forma predeterminada y aplica una validación de firma de controlador más estricta que las versiones anteriores de Windows. Los PC más antiguos funcionan porque utilizan configuraciones de compatibilidad más permisivas. El nuevo sistema bloquea el controlador compartido heredado.

**P: Sigo eliminando y volviendo a agregar la impresora pero el error persiste. ¿Qué me falta?**
Eliminar el dispositivo de impresora por sí solo no elimina los archivos del controlador. Debe eliminar el **paquete de controladores** usando `printui /s /t2`. Reinicie el equipo y luego preinstale manualmente el controlador correcto antes de volver a conectarse a la impresora compartida.

**P: ¿Necesito editar el registro para corregir el error 0x0000007c?**
No, en la gran mayoría de los casos. El error 0x0000007c se resuelve habilitando SMB 1.0, limpiando el paquete de controladores antiguo e instalando manualmente el controlador correcto — no se requiere edición del registro.

---

## Resumen

El error 0x0000007c al conectarse a una impresora compartida en Windows 10 o Windows 11 significa que el cliente no puede cargar el controlador de impresora remoto debido a una incompatibilidad de controlador o protocolo SMB. No está relacionado con el hardware de la impresora ni con daños en el sistema. Corríjalo en este orden:

1. **Borrar la caché de impresión y reiniciar el spooler de impresión**
2. **Eliminar completamente el paquete de controladores antiguo** mediante `printui /s /t2` y reinstalar manualmente
3. **Habilitar SMB 1.0/CIFS** si el host usa paquetes de controladores heredados
4. **Agregar controladores bidireccionales en el host** si el cliente y el host tienen arquitecturas de Windows diferentes

---

## Artículos relacionados

- [Cómo solucionar el error de impresora compartida 0x00000035 en Windows](/es/posts/printer-sharing-error-0x00000035/)
- [Cómo solucionar el error de impresora compartida 0x00000032 en Windows](/es/posts/printer-sharing-error-0x00000032/)
- [Cómo solucionar el error de impresora compartida 0x00000bcb en Windows](/es/posts/printer-sharing-error-0x00000bcb/)
