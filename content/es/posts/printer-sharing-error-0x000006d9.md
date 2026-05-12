---
title: "Cómo solucionar el error 0x000006d9 al compartir una impresora en Windows"
date: 2026-05-12
description: "¿Aparece el error 0x000006d9 al compartir una impresora en Windows 10 o 11? La solución es sencilla: inicie el servicio Firewall de Windows. Esta guía explica las causas y los pasos a seguir."
tags: ["windows", "error 0x000006d9", "compartir impresora", "servicio firewall windows", "services.msc", "windows 10", "windows 11", "solución de problemas"]
categories: ["Solución de problemas"]
cover:
  image: "/images/printer-error-0x000006d9/step1.png"
  alt: "Cuadro de diálogo de error de Windows con el error 0x000006d9 — No se puede guardar la configuración de la impresora"
  caption: "El error 0x000006d9 aparece cuando el servicio Firewall de Windows no está en ejecución, impidiendo activar el uso compartido de la impresora"
  relative: false
---

# Cómo solucionar el error 0x000006d9 al compartir una impresora en Windows

El error 0x000006d9 es un error frecuente de Windows que aparece específicamente cuando se intenta **compartir** una impresora, no al usarla. La impresora se conecta e imprime con normalidad, pero en el momento de abrir la configuración de uso compartido y hacer clic en Aplicar, Windows muestra:

> **No se puede guardar la configuración de la impresora. La operación no se pudo completar (Error 0x000006d9).**

Esta guía explica exactamente por qué ocurre el error 0x000006d9 y proporciona una solución clara paso a paso para Windows 10 y Windows 11.

---

## ¿Qué es el error 0x000006d9?

Cuando se activa el uso compartido de impresora en Windows, el sistema depende del **servicio Firewall de Windows** para registrar y exponer la impresora compartida en la red, incluso si el propio firewall está desactivado. Si el *servicio* Firewall de Windows está detenido o deshabilitado, Windows no puede completar la configuración del uso compartido y devuelve el error 0x000006d9.

![Error 0x000006d9 en Windows — No se puede guardar la configuración de la impresora](/images/printer-error-0x000006d9/step1.png)

El mensaje de error menciona "almacenamiento insuficiente", lo cual es engañoso. En el contexto del uso compartido de impresoras, el error 0x000006d9 no tiene nada que ver con el espacio en disco; siempre indica que el servicio Firewall de Windows no está en ejecución.

---

## ¿Por qué ocurre el error 0x000006d9?

Existen cuatro causas comunes:

1. **El servicio Firewall de Windows está detenido** — El uso compartido de impresoras requiere que este servicio esté *en ejecución*, independientemente de si el firewall está activado o no.
2. **El servicio Firewall de Windows está deshabilitado** — Las herramientas de optimización del sistema, instalaciones de Windows reducidas o ajustes manuales suelen deshabilitar completamente este servicio.
3. **Los servicios dependientes no están en ejecución** — Servicios que dependen del Firewall de Windows pueden también estar detenidos o en estado anómalo.
4. **Archivos del sistema dañados** — En casos raros, archivos del sistema faltantes o dañados impiden que el servicio se inicie.

---

## Solución

### Paso 1 — Abrir el Administrador de servicios

Presione **Win + R**, escriba `services.msc` y presione Enter para abrir la ventana **Servicios**.

![Cuadro de diálogo Ejecutar con services.msc](/images/printer-error-0x000006d9/step2.png)

### Paso 2 — Encontrar el servicio Firewall de Windows

Desplácese por la lista para localizar **Firewall de Windows** (también listado como *MpsSvc* en algunas versiones).

![Ventana Servicios — localizando el servicio Firewall de Windows](/images/printer-error-0x000006d9/step3.png)

### Paso 3 — Habilitar e iniciar el servicio

Haga doble clic en **Firewall de Windows** para abrir sus propiedades. Establezca el **Tipo de inicio** en **Automático** y luego haga clic en **Iniciar**. Haga clic en **Aceptar** para guardar.

![Propiedades del servicio Firewall de Windows — tipo de inicio Automático e inicio del servicio](/images/printer-error-0x000006d9/step4.png)

> **Importante:** Iniciar el *servicio* Firewall de Windows no activa el propio firewall. Puede seguir manteniéndolo desactivado en Seguridad de Windows — el servicio solo necesita estar en ejecución para que el uso compartido de impresoras funcione.

### Paso 4 — Si Firewall de Windows no aparece en la lista

En algunas instalaciones de Windows personalizadas o reducidas, el servicio Firewall de Windows puede no aparecer en la lista. En ese caso, inicie primero estos dos servicios relacionados:

**Windows Update:** Haga doble clic en **Windows Update**, establezca el tipo de inicio en **Automático**, haga clic en **Iniciar** y luego en **Aceptar**.

![Propiedades del servicio Windows Update](/images/printer-error-0x000006d9/step5.png)

**Windows Installer:** Localice **Windows Installer**, haga doble clic, establezca el tipo de inicio en **Automático**, haga clic en **Iniciar** y luego en **Aceptar**.

![Propiedades del servicio Windows Installer](/images/printer-error-0x000006d9/step6.png)

### Paso 5 — Reiniciar y volver a intentarlo

Reinicie el equipo. Tras reiniciar, abra **Servicios** y compruebe si **Firewall de Windows** aparece ahora en la lista. Si es así, establézcalo en **Automático** e inícielo (como en el paso 3). A continuación, vuelva a la configuración de la impresora y active el uso compartido: el error 0x000006d9 debería haber desaparecido.

---

## Preguntas frecuentes

**P: ¿El error 0x000006d9 significa que el disco está lleno?**
No. Aunque el mensaje menciona "almacenamiento insuficiente", en el contexto del uso compartido de impresoras este código de error indica exclusivamente que el servicio Firewall de Windows no está en ejecución. No tiene nada que ver con el espacio en disco.

**P: Ya he desactivado el firewall — ¿por qué sigue apareciendo el error?**
Porque el uso compartido de impresoras no comprueba si el firewall está activado o desactivado, sino solo si el *servicio* Firewall de Windows está en ejecución. El servicio debe estar iniciado aunque el firewall esté desactivado.

**P: ¿Iniciar el servicio Firewall de Windows afectará mi configuración de seguridad?**
No. Puede seguir manteniendo el firewall desactivado en **Seguridad de Windows → Firewall y protección de red**. Iniciar el servicio no activa automáticamente el firewall.

**P: ¿El servicio Firewall de Windows debe permanecer en ejecución permanentemente?**
Sí, para que el uso compartido de impresoras siga funcionando. Al establecer el tipo de inicio en **Automático**, se garantiza que se inicie con Windows cada vez.

---

## Resumen

El error 0x000006d9 al compartir una impresora en Windows tiene una única causa: **el servicio Firewall de Windows no está en ejecución**. La solución:

1. Abrir **Servicios** (`services.msc`)
2. Encontrar **Firewall de Windows**, establecer el tipo de inicio en **Automático** y hacer clic en **Iniciar**
3. Reiniciar el equipo y volver a activar el uso compartido de la impresora

---

## Artículos relacionados

- [Solucionar el error de impresora compartida 0x00000bcb en Windows](/es/posts/printer-sharing-error-0x00000bcb/)
- [Solucionar el error de impresora compartida 0x00000709 en Windows](/es/posts/printer-sharing-error-0x00000709/)
- [Solucionar el error de impresora compartida 0x0000011b en Windows](/es/posts/printer-sharing-error-0x0000011b/)
