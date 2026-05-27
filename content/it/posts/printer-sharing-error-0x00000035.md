---
title: "Come risolvere l'errore 0x00000035 della stampante condivisa in Windows 10 e 11"
date: 2026-05-28
description: "Ricevi l'errore 0x00000035 quando ti connetti a una stampante condivisa in Windows 10 o 11? Questa guida tratta tutte le cause e soluzioni: connettività di rete, impostazioni di condivisione, firewall, protocollo SMB e pulizia delle credenziali."
tags: ["windows", "errore 0x00000035", "errore 0x80070035", "stampante condivisa", "percorso di rete non trovato", "SMB", "firewall windows", "windows 10", "windows 11", "risoluzione dei problemi"]
categories: ["Risoluzione dei problemi"]
cover:
  image: "/images/printer-error-0x00000035/step1.png"
  alt: "Finestra di dialogo di errore di Windows con l'errore 0x00000035 — Impossibile trovare il percorso di rete"
  caption: "L'errore 0x00000035 appare quando Windows non riesce a trovare il percorso condiviso della stampante nella rete locale"
  relative: false
---

# Come risolvere l'errore 0x00000035 della stampante condivisa in Windows 10 e 11

L'errore 0x00000035 — «Impossibile trovare il percorso di rete» — è uno degli errori più comuni delle stampanti condivise in Windows 10 e Windows 11. A differenza di altri errori di stampante causati da un'unica impostazione, l'errore 0x00000035 può avere più cause. Questa guida esamina ogni causa e la relativa soluzione in ordine logico.

---

## Che cos'è l'errore 0x00000035?

Quando si tenta di connettersi a una stampante condivisa sulla rete locale, Windows visualizza:

> **Impossibile completare l'operazione (Errore 0x00000035). Impossibile trovare il percorso di rete.**

![Errore 0x00000035 in Windows — Impossibile trovare il percorso di rete](/images/printer-error-0x00000035/step1.png)

**Sintomi tipici:**
- L'inserimento dell'IP host o del nome del computer nella casella Esegui restituisce immediatamente l'errore
- Alcuni dispositivi riescono a eseguire il ping dell'IP host ma non possono accedere alle risorse condivise
- Il riavvio del PC o la riconfigurazione della condivisione della stampante non risolve il problema
- Gli altri dispositivi nella stessa rete si connettono normalmente — solo alcuni PC specifici generano l'errore

L'errore 0x00000035 viene segnalato anche come **0x80070035** su alcuni sistemi; entrambi i codici significano «percorso di rete non trovato» e si risolvono allo stesso modo.

---

## Perché si verifica l'errore 0x00000035?

Esistono sei cause comuni:

1. **Problema di ambiente di rete** — L'host e il client non si trovano nella stessa rete locale o subnet.
2. **Individuazione rete e condivisione file disabilitati** — L'host o il client non ha attivato l'individuazione rete o la condivisione di file e stampanti.
3. **I servizi principali di condivisione non sono in esecuzione** — I servizi Server, Workstation o Helper NetBIOS su TCP/IP sono arrestati o disabilitati.
4. **Il firewall o il software di sicurezza blocca il traffico** — Windows Firewall o un antivirus di terze parti blocca la porta SMB 445.
5. **Errore di protocollo di rete o risoluzione dei nomi** — NetBIOS è disabilitato, SMB 1.0/CIFS non è installato oppure la risoluzione del nome host non riesce.
6. **Credenziali di rete obsolete** — Il client ha credenziali errate memorizzate nella cache per il PC host.

---

## Soluzione

### Passaggio 1 — Verificare la connettività di rete

Sul **PC host**, apri il prompt dei comandi ed esegui `ipconfig`. Annota l'indirizzo IPv4.

Sul **PC client**, apri il prompt dei comandi ed esegui:
```
ping <indirizzo IP dell'host>
```

![Prompt dei comandi — ipconfig e ping per verificare la connettività](/images/printer-error-0x00000035/step2.png)

Se il ping non riesce, controlla il router e i cavi di rete e assicurati che entrambi i dispositivi siano connessi alla stessa rete e subnet.

---

### Passaggio 2 — Attivare l'individuazione rete e la condivisione file

Sull'host e sul client, vai in **Pannello di controllo → Centro connessioni di rete e condivisione → Modifica impostazioni di condivisione avanzate**. In **Rete privata**, attiva:
- **Attiva individuazione rete**
- **Attiva condivisione file e stampanti**

Salva le impostazioni e riprova a connetterti alla stampante condivisa.

![Impostazioni di condivisione avanzate — attivazione dell'individuazione rete e della condivisione](/images/printer-error-0x00000035/step3.png)

---

### Passaggio 3 — Consentire la condivisione stampanti attraverso il firewall

Apri **Windows Defender Firewall → Consenti app o funzionalità attraverso Windows Defender Firewall**. Trova **Condivisione file e stampanti** e seleziona le caselle **Privato** e **Pubblico**.

Se utilizzi un software antivirus di terze parti, disabilitalo temporaneamente per verificare se sta bloccando la connessione.

![Windows Firewall — consentire la condivisione di file e stampanti](/images/printer-error-0x00000035/step4.png)

---

### Passaggio 4 — Correggere i protocolli di rete e le impostazioni di accesso

Apri le **proprietà della scheda di rete** e assicurati che **Client per reti Microsoft** e **Condivisione file e stampanti per reti Microsoft** siano selezionati.

Apri anche **Attivazione o disattivazione delle funzionalità di Windows** e abilita **Supporto per la condivisione di file SMB 1.0/CIFS**, quindi riavvia.

![Proprietà della scheda di rete — abilitazione di SMB 1.0/CIFS](/images/printer-error-0x00000035/step5.png)

---

### Passaggio 5 — Eliminare le credenziali di rete obsolete

Premi **Win + R**, digita `control keymgr.dll` e apri **Gestione credenziali**. In **Credenziali Windows**, elimina tutte le voci correlate all'indirizzo IP o al nome del computer del PC host.

![Gestione credenziali — rimozione delle credenziali obsolete](/images/printer-error-0x00000035/step6.png)

---

## Domande frequenti

**D: Il ping funziona ma l'errore 0x00000035 persiste. Perché?**
Un ping riuscito conferma solo la connettività IP di base. L'errore 0x00000035 è causato da servizi di condivisione, regole del firewall o problemi di protocollo. Controlla il servizio Server, le regole del firewall e il protocollo SMB.

**D: L'errore 0x00000035 è uguale all'errore 0x80070035?**
Sì. Entrambi significano «Impossibile trovare il percorso di rete». L'unica differenza è il formato del codice di errore. Le cause e le soluzioni sono identiche.

**D: Solo un PC client presenta l'errore — tutti gli altri funzionano normalmente. Cosa devo fare?**
Il problema è specifico di quel PC client. Elimina le sue credenziali obsolete, riavvia i suoi servizi di condivisione locali e abilita il protocollo SMB. Non sono necessarie modifiche sul PC host.

**D: Come evitare che l'errore si ripresenti?**
Configura i servizi **Server**, **Workstation** e **Helper NetBIOS su TCP/IP** per l'avvio automatico. Evita anche i caratteri speciali nel nome di condivisione della stampante.

---

## Riepilogo

L'errore 0x00000035 durante la connessione a una stampante condivisa in Windows 10 o Windows 11 significa che Windows non riesce a trovare il percorso di rete condiviso. Non è correlato all'hardware o al driver della stampante. Risolvilo in questo ordine:

1. **Verificare la connettività di rete** — eseguire il ping dell'IP host; assicurarsi che entrambi i dispositivi siano nella stessa subnet
2. **Attivare l'individuazione rete e la condivisione di file e stampanti**
3. **Consentire la condivisione di file e stampanti in Windows Firewall**
4. **Abilitare SMB 1.0/CIFS** e verificare le impostazioni del protocollo della scheda di rete
5. **Eliminare le credenziali obsolete** in Gestione credenziali

---

## Articoli correlati

- [Risolvere l'errore della stampante condivisa 0x000006d9 in Windows](/it/posts/printer-sharing-error-0x000006d9/)
- [Risolvere l'errore della stampante condivisa 0x00000bcb in Windows](/it/posts/printer-sharing-error-0x00000bcb/)
- [Risolvere l'errore della stampante condivisa 0x00000709 in Windows](/it/posts/printer-sharing-error-0x00000709/)
