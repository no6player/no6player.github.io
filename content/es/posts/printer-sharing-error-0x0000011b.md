---
title: "Cómo solucionar el error 0x0000011b al conectar impresora compartida en Windows"
date: 2026-04-27
description: "¿Aparece el mensaje 'Windows no puede conectarse a la impresora, error 0x0000011b'? Esta guía te muestra la solución probada con el registro para restaurar la conexión de inmediato."
tags: ["windows", "error 0x0000011b", "impresora compartida", "registro", "solución de problemas"]
categories: ["Solución de Problemas"]
cover:
  image: "/images/printer-error-0x0000011b/step1.png"
  alt: "Cuadro de diálogo de error de Windows 0x0000011b"
  caption: "El error 0x0000011b aparece cuando Windows no puede conectarse a la impresora compartida"
  relative: false
---

Las impresoras compartidas son fundamentales en entornos de oficina y redes domésticas. Sin embargo, un error frustrante impide a muchos usuarios conectarse:

> **«Windows no puede conectarse a la impresora. Error en la operación: 0x0000011b.»**

![Cuadro de diálogo de error 0x0000011b en Windows](/images/printer-error-0x0000011b/step1.png)

Este error apareció de forma masiva tras una actualización de seguridad de Microsoft (KB5005565, finales de 2021). La solución lleva menos de dos minutos.

---

## ¿Por qué ocurre el error 0x0000011b?

La causa es un cambio en la **privacidad de autenticación RPC**. Tras la actualización, Windows exige un nivel de autenticación más estricto que los sistemas más antiguos no siempre pueden negociar, provocando el error `0x0000011b`.

---

## Solución — Editar el Registro de Windows

> ⚠️ **Antes de empezar:** Haga una copia de seguridad del registro: **Archivo → Exportar**.

### Paso 1 — Abrir el Editor del Registro

Presione **Win + R**, escriba `regedit` y presione **Enter**.

![Búsqueda de regedit en el menú Inicio de Windows](/images/printer-error-0x0000011b/step2.png)

### Paso 2 — Navegar a la clave Print

Pegue la siguiente ruta en la barra de direcciones y presione **Enter**:

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print
```

![Editor del Registro con la ruta Control\Print](/images/printer-error-0x0000011b/step3.png)

### Paso 3 — Crear un nuevo valor DWORD

1. Haga clic derecho en un área vacía del panel derecho.
2. Seleccione **Nuevo → Valor DWORD (32 bits)**.

![Menú contextual para crear un nuevo valor DWORD](/images/printer-error-0x0000011b/step4.png)

3. Nombre el valor exactamente: `RpcAuthnLevelPrivacyEnabled`

### Paso 4 — Establecer el valor en 0

1. Haga doble clic en `RpcAuthnLevelPrivacyEnabled`.
2. Ingrese `0` en el campo **Información del valor**.
3. Haga clic en **Aceptar**.

![Configuración de RpcAuthnLevelPrivacyEnabled en 0](/images/printer-error-0x0000011b/step5.png)

### Paso 5 — Reiniciar y verificar

Reinicie el equipo y conéctese de nuevo a la impresora compartida.

![Cola de impresión mostrando conexión exitosa](/images/printer-error-0x0000011b/step6.png)

---

## ¿El error persiste?

- **Aplique la solución en el equipo anfitrión** — misma modificación en el PC que comparte la impresora.
- **Verifique el Firewall** — permitir compartir archivos e impresoras en ambos equipos.
- **Misma red** — ambos equipos deben estar en la misma subred.
- **Reinicie la Cola de impresión** — `services.msc` → **Cola de impresión** → **Reiniciar**.

---

## Resumen

| Paso | Acción |
|---|---|
| 1 | Abrir el Editor del Registro (`Win + R` → `regedit`) |
| 2 | Navegar a `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print` |
| 3 | Crear un nuevo valor **DWORD (32 bits)** |
| 4 | Nombrarlo `RpcAuthnLevelPrivacyEnabled` y establecerlo en `0` |
| 5 | Reiniciar el equipo |
