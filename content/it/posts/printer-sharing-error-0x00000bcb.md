---
title: "Come correggere l'errore 0x00000bcb della stampante condivisa su Windows 10 e 11"
date: 2026-05-11
description: "Viene visualizzato l'errore 0x00000bcb durante la connessione a una stampante condivisa su Windows 10 o 11? Questa guida copre tutte le cause e soluzioni — modifica del registro, pulizia dei driver e ripristino dello spooler."
tags: ["windows", "errore 0x00000bcb", "stampante condivisa", "registro", "point and print", "driver stampante", "windows 10", "windows 11", "risoluzione problemi"]
categories: ["Risoluzione Problemi"]
cover:
  image: "/images/printer-error-0x00000bcb/step1.png"
  alt: "Finestra di dialogo di errore di Windows con il codice 0x00000bcb — Il driver di stampa specificato non è stato trovato in questo sistema"
  caption: "L'errore 0x00000bcb appare quando Windows non riesce a installare il driver della stampante condivisa a causa delle restrizioni della politica RPC"
  relative: false
---

# Come correggere l'errore 0x00000bcb della stampante condivisa su Windows 10 e 11

L'errore 0x00000bcb è un errore di connessione comune alle stampanti condivise su Windows 10 e Windows 11. Windows visualizza un messaggio che indica che il driver di stampa specificato non è stato trovato e deve essere scaricato — ma il download fallisce. Questo articolo spiega esattamente perché si verifica l'errore 0x00000bcb e come correggerlo passo dopo passo.

---

## Cos'è l'errore 0x00000bcb?

Quando si tenta di connettersi a una stampante condivisa sulla rete locale, Windows visualizza:

> **Impossibile completare l'operazione (Errore 0x00000bcb). Il driver di stampa specificato non è stato trovato in questo sistema e deve essere scaricato.**

![Errore 0x00000bcb su Windows](/images/printer-error-0x00000bcb/step1.png)

Windows tenta automaticamente di installare il driver della stampante dal PC host quando ci si connette a una stampante condivisa. L'errore 0x00000bcb significa che questa installazione automatica è stata bloccata — nella maggior parte dei casi da una politica di sicurezza introdotta nei recenti aggiornamenti di Windows.

---

## Perché si verifica l'errore 0x00000bcb?

Esistono cinque cause comuni:

1. **L'aggiornamento di Windows ha rafforzato la sicurezza RPC** — Le patch di sicurezza di Microsoft hanno limitato l'installazione dei driver Point and Print ai soli amministratori, bloccando il download automatico dalle stampanti condivise.
2. **Driver di stampa mancante o incompatibile** — Il PC client non dispone del driver corretto, o la versione installata non corrisponde a quella dell'host.
3. **Errore del servizio Spooler di stampa o cache di stampa danneggiata** — Un processo di stampa bloccato può impedire l'installazione di nuovi driver.
4. **Autorizzazioni LAN insufficienti** — Il PC client non dispone dei diritti necessari per recuperare e installare i driver dall'host.
5. **Firewall o software di sicurezza che blocca la comunicazione condivisa** — Regole di antivirus di terze parti o del Firewall di Windows possono bloccare il traffico di condivisione delle stampanti.

---

## Soluzione

### Passaggio 1 — Modificare il registro (restrizione Point and Print)

**1.** Premere **Win + R**, digitare `regedit` e premere Invio per aprire l'**Editor del Registro di sistema**.

![Finestra di dialogo Esegui con regedit](/images/printer-error-0x00000bcb/step2.png)

**2.** Passare al percorso seguente. Se una parte del percorso non esiste, creare le sottochiavi mancanti manualmente: clic destro sulla chiave padre → **Nuovo → Chiave**.

```
HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint
```

![Editor del Registro — navigazione verso la chiave PointAndPrint](/images/printer-error-0x00000bcb/step3.png)

**3.** Nel riquadro di destra, fare clic con il pulsante destro e selezionare **Nuovo → Valore DWORD (32 bit)**. Denominarlo `RestrictDriverInstallationToAdministrators`.

![Editor del Registro — creazione DWORD RestrictDriverInstallationToAdministrators](/images/printer-error-0x00000bcb/step4.png)

**4.** Fare doppio clic sul nuovo valore e impostare i **dati del valore** su `0`. Fare clic su **OK**.

![Editor del Registro — RestrictDriverInstallationToAdministrators impostato su 0](/images/printer-error-0x00000bcb/step5.png)

### Passaggio 2 — Rimuovere il vecchio driver della stampante

**5.** Aprire **Pannello di controllo → Hardware e suoni → Dispositivi e stampanti**. Fare clic su **Proprietà server di stampa** nella barra dei menu superiore, quindi accedere alla scheda **Driver**. Selezionare il vecchio driver della stampante condivisa e fare clic su **Rimuovi**.

![Proprietà server di stampa — scheda Driver](/images/printer-error-0x00000bcb/step6.png)

### Passaggio 3 — Riavviare e riconnettere

**6.** Riavviare il computer. Dopo il riavvio, provare ad aggiungere nuovamente la stampante condivisa. Windows dovrebbe ora essere in grado di scaricare e installare il driver senza visualizzare l'errore 0x00000bcb.

---

## Domande frequenti

**D: L'errore 0x00000bcb riguarda solo Windows 10 o anche Windows 11?**
Entrambi. L'errore 0x00000bcb compare su Windows 10 e Windows 11, poiché la causa sottostante — una politica di sicurezza Point and Print rafforzata — è stata applicata a entrambi i sistemi tramite Windows Update.

**D: È sicuro impostare RestrictDriverInstallationToAdministrators su 0?**
Sì, in un ambiente LAN domestico o d'ufficio. Questo valore del registro allenta solo il permesso di installazione dei driver per le stampanti condivise sulla rete locale. Non disabilita nessun'altra funzionalità di sicurezza.

**D: Ho seguito tutti i passaggi ma l'errore 0x00000bcb persiste. Cos'altro posso provare?**
Installare manualmente il driver della stampante sul PC client (scaricarlo dal sito del produttore), quindi connettere la stampante condivisa. Questo bypassa completamente il download automatico del driver.

**D: L'errore 0x00000bcb è lo stesso dell'errore 0x00000709?**
Sono codici diversi, ma le cause sono molto simili — entrambi coinvolgono restrizioni della politica RPC e problemi di autorizzazione dei driver. Se i passaggi sopra non risolvono 0x00000bcb, consultare la [guida per la correzione di 0x00000709](/it/posts/printer-sharing-error-0x00000709/).

---

## Riepilogo

L'errore 0x00000bcb durante la connessione a una stampante condivisa su Windows 10 o Windows 11 è causato principalmente da una politica di sicurezza di Windows Update che limita l'installazione automatica dei driver di stampa. La correzione in tre passaggi:

1. Impostare `RestrictDriverInstallationToAdministrators` = `0` nel registro sotto `HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint`
2. Rimuovere il vecchio driver in **Proprietà server di stampa → Driver**
3. Riavviare il computer e riconnettere la stampante condivisa

---

## Articoli correlati

- [Correggere l'errore stampante condivisa 0x00000709 su Windows](/it/posts/printer-sharing-error-0x00000709/)
- [Correggere l'errore stampante condivisa 0x0000011b su Windows](/it/posts/printer-sharing-error-0x0000011b/)
- [Correggere l'errore stampante condivisa 0x000003e3 su Windows 10](/it/posts/printer-sharing-error-0x000003e3/)
