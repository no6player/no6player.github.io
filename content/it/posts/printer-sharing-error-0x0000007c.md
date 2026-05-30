---
title: "Come Risolvere l'Errore della Stampante Condivisa 0x0000007c su Windows 10 e 11"
date: 2026-05-30
description: "Ricevi l'errore 0x0000007c quando ti connetti a una stampante condivisa su Windows 10 o 11? Questa guida copre tutte le cause e le soluzioni — pulizia dei driver, ripristino dello spooler di stampa, protocollo SMB 1.0 e aggiunta di driver bidirezionali sull'host."
tags: ["windows", "error 0x0000007c", "stampante condivisa", "driver stampante", "SMB", "spooler di stampa", "windows 10", "windows 11", "risoluzione dei problemi"]
categories: ["Risoluzione dei problemi"]
cover:
  image: "/images/printer-error-0x0000007c/step1.png"
  alt: "Finestra di dialogo di errore di Windows che mostra l'errore 0x0000007c — Impossibile completare l'operazione"
  caption: "L'errore 0x0000007c appare quando il client non riesce a caricare il driver della stampante remota a causa di incompatibilità del driver o del protocollo SMB"
  relative: false
---

# Come Risolvere l'Errore della Stampante Condivisa 0x0000007c su Windows 10 e 11

L'errore 0x0000007c — "Impossibile completare l'operazione" — è un errore comune della stampante condivisa su Windows 10 e Windows 11. Si verifica quando il PC client si connette a una stampante condivisa sulla LAN e Windows non riesce a caricare il driver della stampante remota. Si tratta di un problema di **compatibilità del driver e del protocollo SMB**, non di un guasto hardware o di un'interruzione della rete. Questa guida spiega ogni causa e descrive ogni soluzione nell'ordine corretto.

---

## Cos'è l'Errore 0x0000007c?

Quando ci si connette a una stampante condivisa sulla rete locale, Windows visualizza:

> **Impossibile completare l'operazione (Errore 0x0000007c).**

![Finestra di dialogo di errore di Windows che mostra l'errore 0x0000007c — Impossibile completare l'operazione](/images/printer-error-0x0000007c/step1.png)

**I sintomi tipici includono:**
- L'errore appare immediatamente quando si tenta di aggiungere una stampante condivisa sulla LAN
- Le condivisioni di file di rete sullo stesso PC host sono accessibili — solo la stampante non funziona
- I PC più vecchi si connettono alla stampante condivisa senza problemi; solo i PC più nuovi con Windows 10/11 ricevono l'errore
- Eliminare e aggiungere nuovamente la stampante non risolve il problema

---

## Perché si Verifica l'Errore 0x0000007c?

Ci sono sei cause comuni:

1. **Mancata corrispondenza del driver remoto** — La versione del driver della stampante o l'architettura (32 bit contro 64 bit) sul PC host non corrisponde a ciò che richiede il PC client. Il client rifiuta il driver inviato come non valido.
2. **Incompatibilità della versione del protocollo SMB** — Windows 10/11 utilizza SMB 3.0 per impostazione predefinita, mentre i PC host più vecchi o i driver legacy dipendono da SMB 1.0. L'incompatibilità del protocollo causa il fallimento della convalida del trasferimento del driver.
3. **Blocco della verifica della firma del driver** — Le versioni più recenti di Windows applicano rigorosamente i requisiti di firma digitale del driver. I driver delle stampanti condivise più vecchi privi di una firma valida vengono bloccati dal caricamento.
4. **Conflitto di driver residuo sul client** — Un driver precedentemente installato per lo stesso modello di stampante ha lasciato file residui che impediscono il caricamento del nuovo driver remoto.
5. **Danneggiamento della cache dello spooler di stampa** — I dati della cache dello spooler corrotti impediscono al servizio di ricevere e installare il driver condiviso remoto.
6. **PC host privo di driver bidirezionali** — L'host ha solo un driver installato per la propria versione di Windows e non ha aggiunto un pacchetto driver corrispondente per l'architettura del PC client.

---

## Soluzione

Seguire i passaggi seguenti nell'ordine indicato. La maggior parte dei casi viene risolta con i Passaggi 1 e 2.

### Passaggio 1 — Cancellare la Cache di Stampa e Riavviare lo Spooler di Stampa

**1.** Premere **Win + R**, digitare `services.msc` e premere Invio. Trovare **Spooler di stampa** nell'elenco, fare clic con il pulsante destro del mouse e selezionare **Arresta**.

![Finestra Servizi — arresto del servizio Spooler di stampa](/images/printer-error-0x0000007c/step2.png)

**2.** Premere **Win + R**, digitare `C:\Windows\System32\spool\PRINTERS` e premere Invio. Eliminare **tutti i file** all'interno della cartella (non eliminare la cartella stessa). Quindi tornare a Servizi e avviare nuovamente lo **Spooler di stampa**, impostando il **Tipo di avvio** su **Automatico**.

![Esplora file — cancellazione di tutti i file nella cartella di spool PRINTERS](/images/printer-error-0x0000007c/step3.png)

---

### Passaggio 2 — Rimuovere Completamente il Vecchio Driver dal PC Client

**1.** Aprire **Pannello di controllo → Dispositivi e stampanti**. Fare clic con il pulsante destro del mouse su ogni stampante non funzionante e selezionare **Rimuovi dispositivo**.

**2.** Premere **Win + R**, digitare `printui /s /t2` e premere Invio per aprire **Proprietà server di stampa**. Andare alla scheda **Driver**, selezionare il driver della stampante corrispondente, fare clic su **Rimuovi** e scegliere **Rimuovi driver e pacchetto driver** per eliminare completamente tutti i file del driver.

![Proprietà server di stampa — rimozione completa del driver e del pacchetto driver](/images/printer-error-0x0000007c/step4.png)

**3.** Riavviare il computer. Scaricare il driver completo corretto dal sito Web del produttore e installarlo manualmente prima di riconnettersi alla stampante condivisa.

---

### Passaggio 3 — Abilitare il Protocollo SMB 1.0/CIFS

Se il PC host esegue una versione precedente di Windows o utilizza pacchetti driver meno recenti che dipendono da SMB 1.0:

**1.** Aprire **Pannello di controllo → Programmi → Attivazione o disattivazione delle funzionalità Windows**.

**2.** Scorrere verso il basso e selezionare **Supporto per la condivisione di file SMB 1.0/CIFS**. Fare clic su **OK** e riavviare il computer.

![Funzionalità Windows — abilitazione del supporto per la condivisione di file SMB 1.0/CIFS](/images/printer-error-0x0000007c/step5.png)

> **Nota sulla sicurezza:** SMB 1.0 presenta vulnerabilità note. Abilitarlo solo come misura temporanea e disabilitarlo una volta risolto il problema della stampante oppure aggiornare il driver del PC host a una versione compatibile con SMB 2.0/3.0.

---

### Passaggio 4 — Aggiungere Driver Bidirezionali sul PC Host

Se il PC client ha un'architettura Windows diversa (32 bit contro 64 bit) rispetto all'host:

**1.** Sul **PC host**, aprire **Dispositivi e stampanti**, fare clic con il pulsante destro del mouse sulla stampante condivisa e selezionare **Proprietà stampante → scheda Condivisione → Driver aggiuntivi**.

**2.** Selezionare l'architettura che corrisponde al **PC client** (ad esempio, selezionare **x86** se il client è a 32 bit e l'host è a 64 bit), fare clic su **OK** e installare il pacchetto driver corrispondente.

Dopo che l'host dispone del driver corrispondente, riconnettersi alla stampante condivisa dal PC client.

---

## Domande Frequenti

**D: Cosa significa l'errore 0x0000007c?**
Il significato ufficiale è "il driver della stampante non è valido o il driver della stampante remota non può essere caricato." In parole semplici, il sistema Windows del PC client non accetta il driver inviato dall'host. Ciò è causato da incompatibilità del protocollo o mancata corrispondenza della versione del driver, non da un guasto hardware.

**D: Posso eseguire il ping dell'host e accedere alle cartelle condivise, ma la stampante dà l'errore 0x0000007c. Perché?**
L'accesso alle cartelle condivise richiede solo la connettività di rete SMB. Le connessioni alle stampanti condivise richiedono sia la **compatibilità del protocollo che la compatibilità del driver**. Una rete funzionante non garantisce la compatibilità del driver. Abilitare SMB 1.0 e installare manualmente il driver della stampante locale per risolvere il problema.

**D: I PC vecchi si connettono alla stampante condivisa senza problemi. Perché il mio nuovo PC con Windows 10/11 riceve l'errore 0x0000007c?**
Windows 10/11 disabilita SMB 1.0 per impostazione predefinita e applica una convalida della firma del driver più severa rispetto alle versioni precedenti di Windows. I PC più vecchi funzionano perché utilizzano impostazioni di compatibilità più permissive. Il nuovo sistema blocca il driver condiviso legacy.

**D: Continuo a eliminare e aggiungere nuovamente la stampante, ma l'errore persiste. Cosa mi sfugge?**
Eliminare il dispositivo stampante da solo non rimuove i file del driver. È necessario eliminare il **pacchetto driver** utilizzando `printui /s /t2`. Riavviare il computer, quindi preinstallare manualmente il driver corretto prima di riconnettersi alla stampante condivisa.

**D: È necessario modificare il registro per correggere l'errore 0x0000007c?**
No, nella grande maggioranza dei casi. L'errore 0x0000007c viene risolto abilitando SMB 1.0, pulendo il vecchio pacchetto driver e installando manualmente il driver corretto — non è richiesta alcuna modifica al registro.

---

## Riepilogo

L'errore 0x0000007c durante la connessione a una stampante condivisa su Windows 10 o Windows 11 significa che il client non riesce a caricare il driver della stampante remota a causa di incompatibilità del driver o del protocollo SMB. Non è correlato all'hardware della stampante o a danni al sistema. Correggerlo in questo ordine:

1. **Cancellare la cache di stampa e riavviare lo Spooler di stampa**
2. **Rimuovere completamente il vecchio pacchetto driver** tramite `printui /s /t2` e reinstallare manualmente
3. **Abilitare SMB 1.0/CIFS** se l'host utilizza pacchetti driver legacy
4. **Aggiungere driver bidirezionali sull'host** se il client e l'host hanno architetture Windows diverse

---

## Articoli Correlati

- [Come Risolvere l'Errore della Stampante Condivisa 0x00000035 su Windows](/it/posts/printer-sharing-error-0x00000035/)
- [Come Risolvere l'Errore della Stampante Condivisa 0x00000032 su Windows](/it/posts/printer-sharing-error-0x00000032/)
- [Come Risolvere l'Errore della Stampante Condivisa 0x00000bcb su Windows](/it/posts/printer-sharing-error-0x00000bcb/)
