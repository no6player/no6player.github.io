---
title: "Come correggere l'errore 0x00000040 della stampante condivisa su Windows"
date: 2026-04-29
description: "I client Windows 7 ricevono l'errore 0x00000040 durante la connessione a una stampante condivisa da Windows 11? Questa guida spiega come abilitare SMB 1.0 e modificare il registro per risolvere il problema."
tags: ["windows", "errore 0x00000040", "stampante condivisa", "windows 7", "windows 11", "registro", "SMB", "risoluzione problemi"]
categories: ["Risoluzione Problemi"]
cover:
  image: "/images/printer-error-0x00000040/step1.png"
  alt: "Finestra di dialogo di errore di Windows con il codice 0x00000040 — Il nome di rete specificato non è più disponibile"
  caption: "L'errore 0x00000040 appare quando un client Windows 7 non riesce a connettersi a una stampante condivisa da Windows 11"
  relative: false
---

## Descrizione del problema

Quando un PC Windows 11 viene utilizzato come host per la condivisione della stampante, i client Windows 7 che tentano di connettersi ricevono il seguente messaggio di errore:

> **Impossibile completare l'operazione (Errore 0x00000040). Il nome di rete specificato non è più disponibile.**

![Errore 0x00000040 su Windows](/images/printer-error-0x00000040/step1.png)

Questo errore è generalmente causato da un servizio SMB disabilitato, un nome di condivisione scaduto, una connessione di rete interrotta o un'incompatibilità del protocollo RPC tra i due sistemi operativi. La correzione prevede l'abilitazione di SMB 1.0 e la modifica di due valori del registro sull'host Windows 11.

---

## Soluzione

**1.** Aprire **Pannello di controllo → Programmi → Attiva o disattiva funzionalità di Windows** e abilitare **Supporto per la condivisione di file SMB 1.0/CIFS**.

![Abilitare SMB 1.0/CIFS nelle funzionalità di Windows](/images/printer-error-0x00000040/step2.png)

**2.** Premere **Win + R**, digitare `regedit` e premere Invio per aprire l'**Editor del Registro di sistema**.

![Finestra di dialogo Esegui con regedit](/images/printer-error-0x00000040/step3.png)

**3.** Passare alla chiave seguente (crearla se non esiste):

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Fare clic con il pulsante destro nel riquadro di destra e creare un nuovo **Valore DWORD (32 bit)**.

![Editor del Registro — Creazione di un nuovo valore DWORD sotto la chiave RPC](/images/printer-error-0x00000040/step4.png)

**4.** Rinominare il nuovo valore in `RpcProtocols` e impostare i **dati del valore** su `7`.

![Editor del Registro — RpcProtocols impostato su 7](/images/printer-error-0x00000040/step5.png)

**5.** Fare di nuovo clic con il pulsante destro nel riquadro di destra e creare un altro **Valore DWORD (32 bit)**. Rinominarlo in `ForceKerberosForRpc` e impostare i **dati del valore** su `1`.

![Editor del Registro — ForceKerberosForRpc impostato su 1](/images/printer-error-0x00000040/step6.png)

**6.** Dopo aver completato tutti i passaggi, **riavviare il computer** e provare a connettersi nuovamente alla stampante condivisa.

---

## Riepilogo

L'errore 0x00000040 durante la connessione a una stampante condivisa è un problema comune negli ambienti misti Windows 7/Windows 11. È generalmente causato da un servizio SMB disabilitato o da un'incompatibilità del protocollo RPC. La correzione comprende due passaggi:

- Abilitare il **supporto per la condivisione di file SMB 1.0/CIFS** nelle funzionalità di Windows
- Aggiungere i valori di registro **RpcProtocols** (`7`) e **ForceKerberosForRpc** (`1`) in `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

Dopo il riavvio, la connessione alla stampante condivisa dovrebbe funzionare normalmente.
