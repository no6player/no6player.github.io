---
title: "Cómo solucionar el error 0x00000bcb de impresora compartida en Windows 10 y 11"
date: 2026-05-11
description: "¿Aparece el error 0x00000bcb al conectar una impresora compartida en Windows 10 o 11? Esta guía cubre todas las causas y soluciones: ajuste del registro, limpieza de controladores y restablecimiento del spooler."
tags: ["windows", "error 0x00000bcb", "impresora compartida", "registro", "point and print", "controlador impresora", "windows 10", "windows 11", "solución de problemas"]
categories: ["Solución de problemas"]
cover:
  image: "/images/printer-error-0x00000bcb/step1.png"
  alt: "Cuadro de diálogo de error de Windows con el error 0x00000bcb — El controlador de impresora especificado no se encontró en este sistema"
  caption: "El error 0x00000bcb aparece cuando Windows no puede instalar el controlador de la impresora compartida debido a restricciones de la política RPC"
  relative: false
---

# Cómo solucionar el error 0x00000bcb de impresora compartida en Windows 10 y 11

El error 0x00000bcb es un fallo de conexión frecuente en impresoras compartidas en Windows 10 y Windows 11. Windows muestra un mensaje indicando que el controlador de impresora especificado no se encontró y debe descargarse — pero la descarga falla. Este artículo explica exactamente por qué ocurre el error 0x00000bcb y cómo solucionarlo paso a paso.

---

## ¿Qué es el error 0x00000bcb?

Al intentar conectarse a una impresora compartida en la red local, Windows muestra:

> **La operación no se pudo completar (Error 0x00000bcb). El controlador de impresora especificado no se encontró en este sistema y debe descargarse.**

![Error 0x00000bcb en Windows](/images/printer-error-0x00000bcb/step1.png)

Windows intenta instalar automáticamente el controlador de impresora desde el PC host al conectarse a una impresora compartida. El error 0x00000bcb significa que esta instalación automática fue bloqueada — la mayoría de las veces por una política de seguridad introducida en actualizaciones recientes de Windows.

---

## ¿Por qué ocurre el error 0x00000bcb?

Existen cinco causas comunes:

1. **La actualización de Windows reforzó la seguridad RPC** — Los parches de seguridad de Microsoft restringieron la instalación de controladores Point and Print solo a administradores, bloqueando la descarga automática desde impresoras compartidas.
2. **Controlador de impresora ausente o incompatible** — El PC cliente no tiene el controlador correcto, o la versión instalada no coincide con la del host.
3. **Error del servicio Spooler de impresión o caché de impresión dañada** — Un trabajo de impresión atascado puede impedir la instalación de nuevos controladores.
4. **Permisos LAN insuficientes** — El PC cliente no tiene los derechos necesarios para obtener e instalar controladores desde el host.
5. **Firewall o software de seguridad bloqueando la comunicación compartida** — Reglas de antivirus de terceros o del Firewall de Windows pueden bloquear el tráfico de uso compartido de impresoras.

---

## Solución

### Paso 1 — Modificar el registro (restricción Point and Print)

**1.** Presione **Win + R**, escriba `regedit` y presione Enter para abrir el **Editor del Registro**.

![Cuadro de diálogo Ejecutar con regedit](/images/printer-error-0x00000bcb/step2.png)

**2.** Navegue hasta la siguiente ruta. Si alguna parte de la ruta no existe, cree las subclaves faltantes manualmente: clic derecho en la clave padre → **Nuevo → Clave**.

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint
```

![Editor del Registro — navegando a la clave PointAndPrint](/images/printer-error-0x00000bcb/step3.png)

**3.** En el panel derecho, haga clic derecho y seleccione **Nuevo → Valor DWORD (32 bits)**. Nómbrelo `RestrictDriverInstallationToAdministrators`.

![Editor del Registro — crear DWORD RestrictDriverInstallationToAdministrators](/images/printer-error-0x00000bcb/step4.png)

**4.** Haga doble clic en el nuevo valor y establezca sus **datos de valor** en `0`. Haga clic en **Aceptar**.

![Editor del Registro — RestrictDriverInstallationToAdministrators establecido en 0](/images/printer-error-0x00000bcb/step5.png)

### Paso 2 — Eliminar el controlador de impresora antiguo

**5.** Abra **Panel de control → Hardware y sonido → Dispositivos e impresoras**. Haga clic en **Propiedades del servidor de impresión** en la barra de menú superior, luego vaya a la pestaña **Controladores**. Seleccione el controlador antiguo de la impresora compartida y haga clic en **Quitar**.

![Propiedades del servidor de impresión — pestaña Controladores](/images/printer-error-0x00000bcb/step6.png)

### Paso 3 — Reiniciar y reconectar

**6.** Reinicie el equipo. Tras reiniciar, intente agregar de nuevo la impresora compartida. Windows debería poder descargar e instalar el controlador sin mostrar el error 0x00000bcb.

---

## Preguntas frecuentes

**P: ¿El error 0x00000bcb afecta solo a Windows 10 o también a Windows 11?**
A ambos. El error 0x00000bcb aparece en Windows 10 y Windows 11, ya que la causa subyacente — una política de seguridad Point and Print reforzada — se aplicó a ambos sistemas mediante Windows Update.

**P: ¿Es seguro establecer RestrictDriverInstallationToAdministrators en 0?**
Sí, en un entorno LAN doméstico o de oficina. Este valor del registro solo relaja el permiso de instalación de controladores para impresoras compartidas en la red local. No deshabilita ninguna otra función de seguridad.

**P: Seguí todos los pasos pero el error 0x00000bcb persiste. ¿Qué más puedo intentar?**
Instale manualmente el controlador de impresora en el PC cliente (descárguelo del sitio web del fabricante) y luego conecte la impresora compartida. Esto omite completamente la descarga automática del controlador.

**P: ¿El error 0x00000bcb es el mismo que el error 0x00000709?**
No son el mismo código, pero comparten causas similares — ambos involucran restricciones de política RPC y problemas de permisos de controladores. Si los pasos anteriores no resuelven 0x00000bcb, consulte la [guía de corrección de 0x00000709](/es/posts/printer-sharing-error-0x00000709/).

---

## Resumen

El error 0x00000bcb al conectar una impresora compartida en Windows 10 o Windows 11 está causado principalmente por una política de seguridad de Windows Update que restringe la instalación automática de controladores de impresora. La corrección en tres pasos:

1. Establecer `RestrictDriverInstallationToAdministrators` = `0` en el registro bajo `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint`
2. Eliminar el controlador antiguo en **Propiedades del servidor de impresión → Controladores**
3. Reiniciar el equipo y reconectar la impresora compartida

---

## Artículos relacionados

- [Solucionar el error de impresora compartida 0x00000709 en Windows](/es/posts/printer-sharing-error-0x00000709/)
- [Solucionar el error de impresora compartida 0x0000011b en Windows](/es/posts/printer-sharing-error-0x0000011b/)
- [Solucionar el error de impresora compartida 0x000003e3 en Windows 10](/es/posts/printer-sharing-error-0x000003e3/)
