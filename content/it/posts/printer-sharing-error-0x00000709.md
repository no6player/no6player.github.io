---
title: "Come correggere l'errore 0x00000709 della stampante condivisa su Windows"
date: 2026-04-29
description: "Viene visualizzato l'errore 0x00000709 durante la connessione a una stampante condivisa su Windows 10 o Windows 11? Questa guida passo passo risolve il problema aggiungendo credenziali Windows e modificando un valore del registro."
tags: ["windows", "errore 0x00000709", "stampante condivisa", "gestione credenziali", "registro", "windows 10", "windows 11", "risoluzione problemi"]
categories: ["Risoluzione Problemi"]
cover:
  image: "/images/printer-error-0x00000709/step1.png"
  alt: "Finestra di dialogo di errore di Windows con il codice 0x00000709 — Impossibile completare l'operazione"
  caption: "L'errore 0x00000709 appare quando Windows non riesce a connettersi a una stampante condivisa sulla rete"
  relative: false
---

# Come correggere l'errore 0x00000709 della stampante condivisa su Windows

L'errore 0x00000709 è uno degli errori di stampante condivisa più comuni su Windows 10 e Windows 11. Impedisce la connessione a una stampante condivisa in rete e può comparire improvvisamente — spesso dopo un aggiornamento di Windows. La buona notizia è che questo errore si risolve in pochi minuti senza software di terze parti.

---

## Cos'è l'errore 0x00000709?

Quando si tenta di connettersi a una stampante condivisa sulla rete locale, Windows può visualizzare:

> **Impossibile completare l'operazione (Errore 0x00000709). Verificare il nome della stampante e assicurarsi che sia connessa alla rete.**

![Errore 0x00000709 su Windows](/images/printer-error-0x00000709/step1.png)

L'errore 0x00000709 è un errore di autorizzazione o autenticazione. Windows tenta di comunicare con il PC host della stampante tramite la rete, ma viene bloccato a causa di credenziali mancanti o di una restrizione del protocollo RPC.

---

## Perché si verifica l'errore 0x00000709?

Esistono tre cause principali:

- **Credenziali di rete mancanti** — Windows non dispone di un nome utente/password salvato per il PC host, quindi l'accesso viene negato.
- **Restrizione del protocollo RPC** — Dopo alcuni aggiornamenti di sicurezza di Windows, il metodo di comunicazione RPC predefinito tra i computer è cambiato, interrompendo le connessioni alle stampanti condivise.
- **Account Guest non riconosciuto** — L'account Guest dell'host non è considerato attendibile dal PC client, causando un errore di autenticazione silenzioso.

Questo errore compare frequentemente su client **Windows 10** e **Windows 11** che si connettono a una stampante condivisa da un altro PC.

---

## Soluzione

La correzione si articola in due parti: prima aggiungere le credenziali dell'host alla Gestione credenziali di Windows, poi modificare un valore del registro RPC sul PC client.

### Parte 1 — Aggiungere le credenziali Windows

**1.** Aprire il **Pannello di controllo**, impostare la visualizzazione su **Icone piccole** e fare clic su **Gestione credenziali**.

![Pannello di controllo — Gestione credenziali](/images/printer-error-0x00000709/step2.png)

**2.** Selezionare **Credenziali Windows** e fare clic su **Aggiungi credenziali Windows**.

![Gestione credenziali — Aggiungi credenziali Windows](/images/printer-error-0x00000709/step3.png)

**3.** Nel campo **Indirizzo Internet o di rete**, inserire l'indirizzo IP del PC host (es. `192.168.1.100`). Impostare il **Nome utente** su `guest` e lasciare il campo **Password** vuoto. Fare clic su **OK**.

![Aggiungi credenziali Windows — inserire IP host e nome utente guest](/images/printer-error-0x00000709/step4.png)

> **Suggerimento:** È possibile trovare l'indirizzo IP del PC host eseguendo `ipconfig` in un Prompt dei comandi su quel computer.

### Parte 2 — Modificare il registro

**4.** Premere **Win + R**, digitare `regedit` e premere Invio per aprire l'**Editor del Registro di sistema**.

![Finestra di dialogo Esegui con regedit](/images/printer-error-0x00000709/step5.png)

**5.** Passare alla chiave seguente. Se la sottochiave `RPC` non esiste, crearla manualmente sotto `Printers`:

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC
```

Fare clic con il pulsante destro nel riquadro di destra e selezionare **Nuovo → Valore DWORD (32 bit)**.

![Editor del Registro — Creazione di un nuovo valore DWORD sotto la chiave RPC](/images/printer-error-0x00000709/step6.png)

**6.** Rinominare il nuovo valore in `RpcUseNamedPipeProtocol` e impostare i **dati del valore** su `1`. Fare clic su **OK**.

![Editor del Registro — RpcUseNamedPipeProtocol impostato su 1](/images/printer-error-0x00000709/step7.png)

**7.** Dopo aver completato tutti i passaggi, **riavviare il computer** e provare a connettersi nuovamente alla stampante condivisa. L'errore 0x00000709 non dovrebbe più comparire.

---

## Domande frequenti

**D: Questa soluzione funziona su Windows 10 e Windows 11?**
Sì. L'errore 0x00000709 si verifica su Windows 10 e Windows 11. I passaggi descritti si applicano a entrambe le versioni.

**D: È necessario apportare modifiche anche sul PC host della stampante?**
In genere no. La correzione viene applicata sul PC **client**. Assicurarsi tuttavia che la stampante sia correttamente condivisa sull'host e che l'account Guest sia abilitato.

**D: L'errore persiste dopo tutti i passaggi. Cosa altro posso provare?**
- Verificare che l'indirizzo IP del PC host nella Gestione credenziali sia corretto.
- Assicurarsi che il firewall del PC host non stia bloccando la condivisione di file e stampanti.
- Provare a connettersi usando il nome del PC host anziché l'indirizzo IP.
- Disinstallare e reinstallare il driver della stampante sul PC client.

---

## Riepilogo

L'errore 0x00000709 durante la connessione a una stampante condivisa su Windows 10 o Windows 11 è generalmente causato da credenziali di rete mancanti o da una restrizione del protocollo RPC. La correzione in due passaggi:

1. Aggiungere l'IP dell'host e l'account **guest** alla **Gestione credenziali Windows**
2. Creare il valore di registro **RpcUseNamedPipeProtocol** = `1` in `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC`

---

## Articoli correlati

- [Correggere l'errore stampante condivisa 0x0000011b su Windows](/it/posts/printer-sharing-error-0x0000011b/)
- [Correggere l'errore stampante condivisa 0x000003e3 su Windows 10](/it/posts/printer-sharing-error-0x000003e3/)
- [Correggere l'errore stampante condivisa 0x00000040 su Windows](/it/posts/printer-sharing-error-0x00000040/)
