---
title: "Cómo solucionar el error de impresora compartida 0x00000006 en Windows 10 y 11"
date: 2026-05-29
description: "¿Recibes el error 0x00000006 (Identificador no válido) al conectarte a una impresora compartida en Windows 10 o 11? Esta guía cubre todas las causas y soluciones — servicio Cola de impresión, reinstalación de controladores y reparación de archivos del sistema."
tags: ["windows", "error 0x00000006", "impresora compartida", "identificador no válido", "cola de impresión", "controlador de impresora", "sfc scannow", "windows 10", "windows 11", "solución de problemas"]
categories: ["Solución de problemas"]
cover:
  image: "/images/printer-error-0x00000006/step1.png"
  alt: "Cuadro de diálogo de error de Windows mostrando el error 0x00000006 — El identificador no es válido"
  caption: "El error 0x00000006 aparece cuando Windows no puede generar un identificador válido para la conexión a la impresora compartida"
  relative: false
---

# Cómo solucionar el error de impresora compartida 0x00000006 en Windows 10 y 11

El error 0x00000006 — «El identificador no es válido» — es un error frecuente de impresora compartida en Windows 10 y Windows 11. Aparece cuando Windows no puede generar ni reconocer un identificador de conexión válido para la impresora compartida. A diferencia de los errores de ruta de red, el error 0x00000006 está causado por problemas con el servicio Cola de impresión (Print Spooler), los controladores de impresora o los componentes del sistema — no por la red en sí. Esta guía recorre cada causa y su solución en un orden lógico.

---

## ¿Qué es el error 0x00000006?

Cuando intentas conectarte a una impresora compartida en tu red local, Windows muestra el siguiente mensaje:

> **La operación no pudo completarse (Error 0x00000006). El identificador no es válido.**

![Cuadro de diálogo de error de Windows mostrando el error 0x00000006 — El identificador no es válido](/images/printer-error-0x00000006/step1.png)

**Síntomas típicos:**
- Al hacer clic en «Conectar» al agregar una impresora compartida, el error se muestra inmediatamente
- La impresora se agrega correctamente, pero al enviar un trabajo de impresión se desencadena el error
- La conexión a la impresora se interrumpe y el error 0x00000006 aparece al reconectarse
- Otros dispositivos en la misma red imprimen correctamente — solo ciertos PCs desencadenan el error

---

## ¿Por qué ocurre el error 0x00000006?

Existen cinco causas comunes:

1. **Fallo del servicio Cola de impresión** — La causa más frecuente. Si el servicio Spooler falla, se bloquea o tiene archivos de caché corruptos, Windows no puede generar un identificador de conexión válido.
2. **Problemas con el controlador de impresora** — El PC cliente no tiene el controlador correcto, tiene una versión de controlador incompatible, o hay archivos residuales de un controlador antiguo que entran en conflicto con una nueva instalación.
3. **Archivos del sistema o componentes de impresión corruptos** — Actualizaciones de Windows fallidas, herramientas agresivas de limpieza del sistema, o software antivirus que elimina archivos del sistema relacionados con la impresión pueden causar este error.
4. **Problemas con el uso compartido de red o la ruta de acceso** — El nombre compartido de la impresora contiene caracteres especiales, espacios o símbolos, o la dirección IP del host no puede ser resuelta por el cliente.
5. **Bloqueos de permisos o directivas de seguridad** — El cliente no tiene permiso para acceder a los recursos compartidos en el PC host, o el Firewall de Windows / software de seguridad de terceros está bloqueando la comunicación de uso compartido de impresora.

---

## Solución

Sigue los pasos a continuación en orden — la mayoría de los casos se resuelven en el Paso 1 o el Paso 2.

### Paso 1 — Reiniciar la Cola de impresión y limpiar la cola de trabajos

**1.** Presiona **Win + R**, escribe `services.msc` y presiona Enter para abrir la ventana Servicios.

![Ventana Servicios — apertura de services.msc mediante el cuadro de diálogo Ejecutar](/images/printer-error-0x00000006/step2.png)

**2.** Busca **Cola de impresión (Print Spooler)** en la lista, haz clic derecho sobre él y selecciona **Detener**. Espera 5 a 10 segundos hasta que el servicio se detenga por completo.

![Ventana Servicios — detención del servicio Cola de impresión](/images/printer-error-0x00000006/step3.png)

**3.** Presiona **Win + R**, escribe `C:\Windows\System32\spool\PRINTERS` y presiona Enter. Elimina **todos los archivos** dentro de esta carpeta (no elimines la carpeta en sí). Esto borra el caché de impresión corrupto.

![Explorador de archivos — vaciado de la carpeta de spool PRINTERS](/images/printer-error-0x00000006/step4.png)

**4.** Vuelve a la ventana Servicios, haz clic derecho en **Cola de impresión (Print Spooler)**, selecciona **Iniciar** y establece el **Tipo de inicio** en **Automático** para que el servicio se inicie automáticamente tras cada reinicio.

![Ventana Servicios — inicio de la Cola de impresión y configuración en Automático](/images/printer-error-0x00000006/step5.png)

**5.** Reinicia el PC cliente e intenta conectarte de nuevo a la impresora compartida.

---

### Paso 2 — Eliminar el controlador antiguo y reinstalar un controlador limpio

**1.** Abre **Panel de control → Dispositivos e impresoras**. Haz clic derecho en cada entrada de impresora fallida o duplicada y selecciona **Quitar dispositivo** para borrar todas las conexiones no válidas.

![Panel de control — eliminación de entradas de impresora fallidas en Dispositivos e impresoras](/images/printer-error-0x00000006/step6.png)

**2.** Presiona **Win + R**, escribe `printui /s /t2` y presiona Enter para abrir **Propiedades del servidor de impresión**. Ve a la pestaña **Controladores**.

**3.** Busca el controlador de tu impresora compartida de destino, selecciónalo, haz clic en **Quitar** y en el cuadro de diálogo marca **Quitar el controlador y el paquete de controlador** para eliminar todos los archivos del controlador por completo.

![Propiedades del servidor de impresión — eliminación del paquete de controlador de impresora](/images/printer-error-0x00000006/step7.png)

**4.** Reinicia el equipo. A continuación, descarga el controlador completo y correcto para tu modelo de impresora desde el sitio web del fabricante e instálalo manualmente.

**5.** Una vez instalado el controlador, agrega de nuevo la impresora compartida — usa la dirección IP del PC host para una conexión más confiable (por ejemplo `\\192.168.1.100`).

---

### Paso 3 — Reparar archivos del sistema

**1.** Busca **cmd**, haz clic derecho sobre él y selecciona **Ejecutar como administrador**. Haz clic en **Sí** si aparece un aviso de Control de cuentas de usuario.

![Símbolo del sistema — apertura como administrador](/images/printer-error-0x00000006/step8.png)

**2.** Escribe el siguiente comando y presiona Enter. No cierres la ventana mientras se ejecuta:

```
sfc /scannow
```

![Símbolo del sistema — ejecución de sfc /scannow para reparar archivos del sistema](/images/printer-error-0x00000006/step9.png)

**3.** Una vez finalizado el análisis, reinicia el equipo e intenta conectarte a la impresora compartida.

---

## Preguntas frecuentes

**P: ¿Qué significa exactamente «El identificador no es válido» en el error 0x00000006?**
En Windows, un «identificador» (handle) es un identificador único que el sistema utiliza para rastrear un recurso — en este caso, la conexión a la impresora. «Identificador no válido» significa que Windows intentó crear o hacer referencia a un identificador de conexión pero falló, normalmente porque el servicio Cola de impresión o el controlador de impresora está en un estado defectuoso.

**P: Otros PCs se conectan bien pero solo el mío muestra el error 0x00000006. ¿Qué debo hacer?**
El problema está íntegramente en tu PC cliente — el PC host y la impresora funcionan correctamente. Ejecuta el Paso 1 (reiniciar la Cola de impresión) y el Paso 2 (reinstalar el controlador) solo en tu PC cliente. No es necesario hacer cambios en el host.

**P: El ping funciona y la red está bien, pero sigo obteniendo el error 0x00000006. ¿Por qué?**
Un ping exitoso solo confirma la conectividad IP básica. El error 0x00000006 está causado por el servicio Cola de impresión o el controlador — no por la ruta de red. Concéntrate en el Paso 1 y el Paso 2 en lugar de en la configuración de red.

**P: ¿Eliminar controladores o modificar servicios afectará el uso normal de mi equipo?**
No. Todos los pasos de esta guía son operaciones estándar de solución de problemas de impresora en Windows. Solo afectan a los servicios, controladores y configuraciones relacionados con la impresión — no a la configuración del núcleo del sistema, el acceso a internet ni otro software. Son seguros tanto para entornos domésticos como empresariales.

**P: Tras resolver el error, ¿cómo evito que vuelva a aparecer?**
Asegúrate de que el servicio **Cola de impresión (Print Spooler)** esté configurado para iniciarse en modo **Automático**. Mantén también actualizado el controlador de tu impresora y evita caracteres especiales o espacios en el nombre compartido de la impresora en el PC host.

---

## Resumen

El error 0x00000006 al conectarse a una impresora compartida en Windows 10 o Windows 11 significa que Windows no puede generar un identificador válido para la conexión a la impresora. No está causado por el hardware de la impresora y no requiere reinstalar Windows. Resuélvelo en este orden:

1. **Reiniciar la Cola de impresión y limpiar el caché de impresión** — la solución más habitual
2. **Eliminar completamente el controlador antiguo y reinstalar un controlador limpio**
3. **Ejecutar `sfc /scannow`** para reparar archivos del sistema corruptos

---

## Artículos relacionados

- [Cómo solucionar el error de impresora compartida 0x00000035 en Windows](/es/posts/printer-sharing-error-0x00000035/)
- [Cómo solucionar el error de impresora compartida 0x000006d9 en Windows](/es/posts/printer-sharing-error-0x000006d9/)
- [Cómo solucionar el error de impresora compartida 0x00000709 en Windows](/es/posts/printer-sharing-error-0x00000709/)
