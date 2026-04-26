---
title: "Cómo instalar un controlador de impresora en Windows 10/11"
date: 2026-04-26
description: "Guía completa paso a paso para instalar controladores de impresora en Windows 10 y Windows 11 — USB, red e instalación manual con consejos para solucionar problemas."
tags: ["windows", "controlador", "instalación", "USB", "impresora de red"]
categories: ["Instalación"]
cover:
  image: "/images/printer-driver/connection-types.svg"
  alt: "Resumen de métodos de conexión de impresora"
  caption: "Dos formas principales de conectar una impresora: USB y red/Wi-Fi"
  relative: false
---

Instalar correctamente un controlador de impresora es la base de una configuración de impresión que funcione. Esta guía le explica todos los métodos — desde el sencillo plug-and-play hasta la instalación completamente manual — y le muestra cómo solucionar los problemas más comunes.

---

## Antes de empezar

Reúna la siguiente información antes de comenzar:

| Lo que necesita | Dónde encontrarlo |
|---|---|
| Número de modelo de la impresora | Etiqueta en la parte frontal o inferior de la impresora |
| Versión de Windows (10 u 11) | Configuración → Sistema → Acerca de |
| Tipo de sistema (32 bits / 64 bits) | Configuración → Sistema → Acerca de → Tipo de sistema |
| Tipo de conexión (USB o Wi-Fi) | Decídalo antes de empezar |

---

## Método 1 — USB Plug-and-Play (El más sencillo)

Este método funciona para la mayoría de las impresoras modernas.

**Pasos:**

1. Asegúrese de que la impresora está **apagada**.
2. Conecte el cable USB de la impresora a un puerto USB del ordenador.
3. Encienda la impresora.
4. Windows detecta automáticamente la impresora y descarga el controlador.
5. Espere la notificación: *«El dispositivo está listo»* en la esquina inferior derecha.

> **Consejo:** Use un cable USB de menos de 3 metros. Los cables más largos pueden causar errores de conexión.

---

## Método 2 — Configuración de Windows (USB o red)

Use este método si el plug-and-play no funcionó, o para agregar una impresora de red.

![Configuración de Windows — Impresoras y escáneres](/images/printer-driver/windows-settings.svg)

**Pasos:**

1. Abra **Inicio** → **Configuración** (icono ⚙).
2. Haga clic en **Bluetooth y dispositivos** → **Impresoras y escáneres**.
3. Haga clic en **Agregar una impresora o escáner** (botón azul en la parte superior).
4. Windows busca impresoras cercanas. Espere 20–30 segundos.

![Asistente para agregar impresora — seleccione su impresora de la lista](/images/printer-driver/add-printer-wizard.svg)

5. Cuando aparezca su impresora en la lista, haga clic en ella.
6. Haga clic en **Agregar dispositivo**.
7. Windows instala el controlador automáticamente.

> **¿No aparece la impresora de red?** Asegúrese de que la impresora y el ordenador estén conectados a la **misma red Wi-Fi**.

---

## Método 3 — Descarga manual del controlador

Use este método cuando Windows no encuentra el controlador automáticamente.

![Descarga manual del controlador — cuatro pasos](/images/printer-driver/manual-driver-download.svg)

**Pasos:**

1. **Encuentre el modelo de su impresora** — revise la etiqueta en la impresora.
2. **Vaya al sitio web del fabricante:**
   - HP: `support.hp.com`
   - Canon: `canon.com/support`
   - Epson: `epson.com/support`
   - Brother: `support.brother.com`
3. **Busque** su número de modelo exacto.
4. En **Software y controladores**, seleccione su sistema operativo.
5. Descargue el **controlador completo** (recomendado) o el controlador básico.
6. Abra el archivo `.exe` descargado y siga el asistente de instalación.

---

## Método 4 — Impresora de red mediante dirección IP

Use este método cuando su impresora de red no se detecta automáticamente.

**Pasos:**

1. Imprima una **página de configuración** desde el panel de control de la impresora. Muestra la dirección IP.
2. Abra **Configuración** → **Bluetooth y dispositivos** → **Impresoras y escáneres** → **Agregar una impresora o escáner**.
3. Haga clic en **«La impresora que deseo no está en la lista»**.
4. Seleccione **«Agregar una impresora usando una dirección TCP/IP o nombre de host»** → **Siguiente**.
5. Introduzca la dirección IP de la impresora (p. ej. `192.168.1.105`).
6. Haga clic en **Siguiente** — Windows se conecta e instala el controlador.

---

## Solución de problemas

![Problemas comunes y soluciones](/images/printer-driver/troubleshooting.svg)

### Impresora no encontrada durante la instalación

- Compruebe que la impresora está **encendida** (no en modo de ahorro de energía).
- Para USB: pruebe un puerto USB diferente; evite los concentradores USB.
- Para red: confirme que ambos dispositivos están en la misma red Wi-Fi.
- Desactive temporalmente el cortafuegos y vuelva a intentarlo.

### La impresora aparece como «Sin conexión»

1. Pulse `Win + R`, escriba `services.msc` y pulse **Enter**.
2. Desplácese hasta **Cola de impresión**.
3. Haga clic derecho → **Reiniciar**.
4. Abra **Dispositivos e impresoras**, haga clic derecho en su impresora → **Ver qué se está imprimiendo**.
5. En el menú, haga clic en **Impresora** y desmarque **Usar impresora sin conexión**.

### La instalación del controlador falla

- Haga clic derecho en el instalador → **Ejecutar como administrador**.
- Desactive temporalmente el antivirus y vuelva a intentarlo.
- Compruebe que descargó el controlador correcto (64 bits o 32 bits).

---

## Resumen

| Método | Ideal para | Fuente del controlador |
|---|---|---|
| USB Plug-and-Play | Impresoras nuevas, instalación rápida | Windows Update |
| Configuración de Windows | Impresora USB o de red | Windows Update |
| Descarga manual | Funciones completas, fallo en auto-instalación | Sitio web del fabricante |
| Dirección IP | Impresoras de red no detectadas automáticamente | Windows / Fabricante |

Después de la instalación, imprima siempre una **página de prueba**: haga clic derecho en la impresora → **Propiedades de impresora** → **Imprimir página de prueba**.
