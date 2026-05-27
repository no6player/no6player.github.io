---
title: "Cómo solucionar el error 0x00000035 de impresora compartida en Windows 10 y 11"
date: 2026-05-28
description: "¿Aparece el error 0x00000035 al conectar una impresora compartida en Windows 10 o 11? Esta guía cubre todas las causas y soluciones: conectividad de red, configuración de uso compartido, firewall, protocolo SMB y limpieza de credenciales."
tags: ["windows", "error 0x00000035", "error 0x80070035", "impresora compartida", "ruta de red no encontrada", "SMB", "firewall windows", "windows 10", "windows 11", "solución de problemas"]
categories: ["Solución de problemas"]
cover:
  image: "/images/printer-error-0x00000035/step1.png"
  alt: "Cuadro de diálogo de error de Windows con el error 0x00000035 — No se encontró la ruta de acceso de red"
  caption: "El error 0x00000035 aparece cuando Windows no puede localizar la ruta compartida de la impresora en la red local"
  relative: false
---

# Cómo solucionar el error 0x00000035 de impresora compartida en Windows 10 y 11

El error 0x00000035 — «No se encontró la ruta de acceso de red» — es uno de los errores de impresora compartida más comunes en Windows 10 y Windows 11. A diferencia de otros errores de impresora causados por un único ajuste, el error 0x00000035 puede tener múltiples causas. Esta guía recorre cada causa y su solución en orden lógico.

---

## ¿Qué es el error 0x00000035?

Al intentar conectarse a una impresora compartida en la red local, Windows muestra:

> **La operación no se pudo completar (Error 0x00000035). No se encontró la ruta de acceso de red.**

![Error 0x00000035 en Windows — No se encontró la ruta de acceso de red](/images/printer-error-0x00000035/step1.png)

**Síntomas típicos:**
- Introducir la IP del host o el nombre del equipo en el cuadro Ejecutar devuelve el error inmediatamente
- Algunos dispositivos pueden hacer ping a la IP del host pero no pueden acceder a los recursos compartidos
- Reiniciar el PC o reconfigurar el uso compartido de la impresora no resuelve el problema
- Otros dispositivos de la misma red se conectan bien — solo algunos PCs desencadenan el error

El error 0x00000035 también se reporta como **0x80070035** en algunos sistemas; ambos códigos significan «ruta de red no encontrada» y se corrigen de la misma manera.

---

## ¿Por qué ocurre el error 0x00000035?

Existen seis causas comunes:

1. **Problema de entorno de red** — El host y el cliente no están en la misma red local o subred.
2. **Detección de red y uso compartido de archivos desactivados** — El host o el cliente no ha activado la detección de redes o el uso compartido de archivos e impresoras.
3. **Los servicios principales de uso compartido no están en ejecución** — Los servicios Server, Workstation o Ayuda de NetBIOS sobre TCP/IP están detenidos o deshabilitados.
4. **El firewall o el software de seguridad bloquea el tráfico** — El Firewall de Windows o un antivirus de terceros bloquea el puerto SMB 445.
5. **Error de protocolo de red o resolución de nombres** — NetBIOS está deshabilitado, SMB 1.0/CIFS no está instalado o la resolución del nombre de host falla.
6. **Credenciales de red obsoletas** — El cliente tiene credenciales incorrectas almacenadas para el PC host.

---

## Solución

### Paso 1 — Verificar la conectividad de red

En el **PC host**, abra el símbolo del sistema y ejecute `ipconfig`. Anote la dirección IPv4.

En el **PC cliente**, abra el símbolo del sistema y ejecute:
```
ping <dirección IP del host>
```

![Símbolo del sistema — ipconfig y ping para verificar la conectividad](/images/printer-error-0x00000035/step2.png)

Si el ping falla, compruebe el router y los cables de red, y asegúrese de que ambos dispositivos estén conectados a la misma red y subred.

---

### Paso 2 — Activar la detección de redes y el uso compartido de archivos

En el host y el cliente, vaya a **Panel de control → Centro de redes y recursos compartidos → Cambiar configuración de uso compartido avanzado**. En **Red privada**, active:
- **Activar la detección de redes**
- **Activar el uso compartido de archivos e impresoras**

Guarde la configuración y vuelva a intentar conectarse a la impresora compartida.

![Configuración avanzada de uso compartido — activando la detección de redes y el uso compartido](/images/printer-error-0x00000035/step3.png)

---

### Paso 3 — Permitir el uso compartido de impresoras en el firewall

Abra **Firewall de Windows Defender → Permitir una aplicación o una característica a través del Firewall de Windows Defender**. Busque **Uso compartido de archivos e impresoras** y marque las casillas **Privado** y **Público**.

Si usa software antivirus de terceros, desactívelo temporalmente para comprobar si está bloqueando la conexión.

![Firewall de Windows — permitiendo el uso compartido de archivos e impresoras](/images/printer-error-0x00000035/step4.png)

---

### Paso 4 — Corregir protocolos de red y configuración de acceso

Abra las **propiedades del adaptador de red** y asegúrese de que **Cliente para redes Microsoft** y **Compartir archivos e impresoras para redes Microsoft** estén marcados.

También abra **Activar o desactivar las características de Windows** y habilite **Compatibilidad con el protocolo para compartir archivos SMB 1.0/CIFS**, luego reinicie.

![Propiedades del adaptador de red — habilitando SMB 1.0/CIFS](/images/printer-error-0x00000035/step5.png)

---

### Paso 5 — Eliminar credenciales de red obsoletas

Presione **Win + R**, escriba `control keymgr.dll` y abra el **Administrador de credenciales**. En **Credenciales de Windows**, elimine todas las entradas relacionadas con la dirección IP o el nombre de equipo del PC host.

![Administrador de credenciales — eliminando credenciales obsoletas](/images/printer-error-0x00000035/step6.png)

---

## Preguntas frecuentes

**P: El ping funciona pero el error 0x00000035 sigue apareciendo. ¿Por qué?**
Un ping exitoso solo confirma la conectividad IP básica. El error 0x00000035 está causado por servicios de uso compartido, reglas de firewall o problemas de protocolo. Compruebe el servicio Server, las reglas del firewall y el protocolo SMB.

**P: ¿El error 0x00000035 es el mismo que el error 0x80070035?**
Sí. Ambos significan «No se encontró la ruta de acceso de red». La única diferencia es el formato del código de error. Las causas y soluciones son idénticas.

**P: Solo un PC cliente tiene el error — los demás funcionan bien. ¿Qué debo hacer?**
El problema es específico de ese PC cliente. Elimine sus credenciales obsoletas, reinicie sus servicios de uso compartido locales y active el protocolo SMB. No es necesario realizar cambios en el PC host.

**P: ¿Cómo evito que el error vuelva a aparecer?**
Configure los servicios **Server**, **Workstation** y **Ayuda de NetBIOS sobre TCP/IP** para que se inicien automáticamente. Evite también caracteres especiales en el nombre compartido de la impresora.

---

## Resumen

El error 0x00000035 al conectar una impresora compartida en Windows 10 o Windows 11 significa que Windows no puede localizar la ruta de red compartida. No está relacionado con el hardware o el controlador de la impresora. Corrríjalo en este orden:

1. **Verificar la conectividad de red** — hacer ping a la IP del host; asegurarse de que ambos dispositivos estén en la misma subred
2. **Activar la detección de redes y el uso compartido de archivos e impresoras**
3. **Permitir el uso compartido de archivos e impresoras en el Firewall de Windows**
4. **Habilitar SMB 1.0/CIFS** y verificar la configuración de protocolos del adaptador de red
5. **Eliminar credenciales obsoletas** en el Administrador de credenciales

---

## Artículos relacionados

- [Solucionar el error de impresora compartida 0x000006d9 en Windows](/es/posts/printer-sharing-error-0x000006d9/)
- [Solucionar el error de impresora compartida 0x00000bcb en Windows](/es/posts/printer-sharing-error-0x00000bcb/)
- [Solucionar el error de impresora compartida 0x00000709 en Windows](/es/posts/printer-sharing-error-0x00000709/)
