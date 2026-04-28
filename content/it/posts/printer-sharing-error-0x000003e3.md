---
title: "Come correggere l'errore 0x000003e3 della stampante condivisa su Windows 10"
date: 2026-04-28
description: "Viene visualizzato l'errore 0x000003e3 durante la connessione a una stampante condivisa su Windows 10? Questa guida spiega come modificare le impostazioni dei criteri di sicurezza locali e dei criteri di gruppo per ripristinare la connessione."
tags: ["windows 10", "errore 0x000003e3", "stampante condivisa", "criteri di sicurezza locali", "criteri di gruppo", "risoluzione problemi"]
categories: ["Risoluzione Problemi"]
cover:
  image: "/images/printer-error-0x000003e3/step1.png"
  alt: "Finestra di dialogo di errore di Windows con il codice 0x000003e3 durante la connessione a una stampante condivisa"
  caption: "L'errore 0x000003e3 appare quando Windows 10 non riesce a connettersi a una stampante condivisa nella rete locale"
  relative: false
---

## Descrizione del problema

Su Windows 10, quando si tenta di connettersi a una stampante condivisa nella rete locale, la connessione non riesce con il seguente codice di errore:

> **Errore 0x000003e3**

![Errore 0x000003e3 su Windows 10](/images/printer-error-0x000003e3/step1.png)

Si tratta di un errore comune di autorizzazione o autenticazione durante la connessione a stampanti condivise. È tipicamente causato da accesso negato, un'anomalia del servizio o un problema di protocollo di rete. Seguire i passaggi seguenti per risolvere il problema.

---

## Soluzione

### Passaggio 1 — Modificare i criteri di sicurezza locali

**1.** Aprire **Pannello di controllo → Programmi → Attiva o disattiva funzionalità di Windows** e abilitare **Supporto per la condivisione di file SMB 1.0/CIFS**.

![Abilitare il supporto SMB 1.0/CIFS nelle funzionalità di Windows](/images/printer-error-0x000003e3/step2.png)

**2.** Premere **Win + R**, digitare `secpol.msc` e premere Invio per aprire l'editor dei **Criteri di sicurezza locali**.

![Finestra di dialogo Esegui con secpol.msc](/images/printer-error-0x000003e3/step3.png)

**3.** Passare a **Criteri locali → Assegnazione diritti utente → Accedi al computer dalla rete**. Aggiungere l'account **Guest** locale.  
Quindi andare a **Nega accesso al computer dalla rete** e **rimuovere** l'account Guest da tale elenco.

![Criteri di sicurezza locali — Assegnazione diritti utente](/images/printer-error-0x000003e3/step4.png)

**4.** Passare a **Criteri locali → Opzioni di sicurezza → Accesso di rete: modello di condivisione e sicurezza per gli account locali** e impostarlo su **Solo guest - gli utenti locali vengono autenticati come Guest**.

![Criteri di sicurezza locali — Modello di condivisione impostato su Solo guest](/images/printer-error-0x000003e3/step5.png)

---

### Passaggio 2 — Modificare i criteri di gruppo locali

**1.** Premere **Win + R**, digitare `gpedit.msc` e premere Invio per aprire l'**Editor criteri di gruppo locali**.

![Finestra di dialogo Esegui con gpedit.msc](/images/printer-error-0x000003e3/step6.png)

**2.** Passare a **Configurazione computer → Impostazioni di Windows → Impostazioni di sicurezza → Criteri locali → Opzioni di sicurezza**. Trovare il criterio **Account: limita l'uso di password vuote agli account locali all'accesso alla console**, fare doppio clic, selezionare **Disabilitato** e fare clic su **OK**.

![Criteri di gruppo — Disabilitare la restrizione della password vuota](/images/printer-error-0x000003e3/step7.png)

**3.** Aprire **Gestione computer**, quindi andare a **Utilità di sistema → Utenti e gruppi locali → Utenti**. Fare doppio clic su **Guest**, fare clic sulla scheda **Membro di**, quindi su **Aggiungi**. Nella finestra di dialogo digitare `users` e fare clic su **OK**.

![Gestione computer — Aggiunta di Guest al gruppo Utenti](/images/printer-error-0x000003e3/step8.png)

**4.** Tornare nella finestra Proprietà guest, sotto **Membro di** selezionare **Utenti**, fare clic su **Aggiungi**, quindi su **Applica** e **OK**.

![Proprietà Guest — Scheda Membro di con il gruppo Utenti](/images/printer-error-0x000003e3/step9.png)

**5.** Una volta completati tutti i passaggi, **riavviare il computer** e provare ad aggiungere nuovamente la stampante condivisa.

---

## Riepilogo

L'errore 0x000003e3 durante la connessione a una stampante condivisa su Windows 10 è generalmente causato da impostazioni errate dei criteri di sicurezza o di gruppo. La soluzione include:

- Abilitare il supporto per la condivisione di file SMB 1.0/CIFS
- Concedere all'account Guest i diritti di accesso alla rete
- Impostare il modello di sicurezza della condivisione su "Solo guest"
- Disabilitare la restrizione di accesso alla console per le password vuote
- Aggiungere l'account Guest al gruppo locale Utenti

Se l'errore persiste dopo aver seguito tutti i passaggi, verificare anche:

- Che la connessione LAN funzioni correttamente (host e client devono trovarsi nella stessa sottorete e riuscire a fare ping reciproco)
- Che il computer host della stampante condivisa disponga delle impostazioni corrispondenti
- Disinstallare e reinstallare il driver della stampante, quindi riprovare
