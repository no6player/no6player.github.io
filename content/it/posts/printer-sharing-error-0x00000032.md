---
title: "Come Risolvere l'Errore Stampante 0x00000032 su Windows 10 e 11"
date: 2026-05-30
description: "Ricevi l'errore 0x00000032 (La richiesta non è supportata) quando installi o colleghi una stampante su Windows 10 o 11? Questa guida copre tutte le cause e le soluzioni — pulizia del driver, ripristino dello Spooler di stampa, Criteri di gruppo e riparazione della stampante condivisa."
tags: ["windows", "error 0x00000032", "errore stampante", "la richiesta non è supportata", "spooler di stampa", "driver stampante", "criteri di gruppo", "windows 10", "windows 11", "risoluzione dei problemi"]
categories: ["Risoluzione dei problemi"]
cover:
  image: "/images/printer-error-0x00000032/step1.png"
  alt: "Finestra di dialogo di errore di Windows con l'errore 0x00000032 — La richiesta non è supportata"
  caption: "L'errore 0x00000032 appare quando Windows si rifiuta di caricare o installare il driver della stampante a causa di incompatibilità o di un vecchio driver bloccato"
  relative: false
---

# Come Risolvere l'Errore Stampante 0x00000032 su Windows 10 e 11

L'errore 0x00000032 — "La richiesta non è supportata" — è uno degli errori del driver di stampa più comuni su Windows 10 e Windows 11. Si verifica quando si installa una stampante USB locale, ci si connette a una stampante di rete condivisa o si aggiorna un driver di stampa. A differenza degli errori di rete, l'errore 0x00000032 è un errore di classe del driver: Windows si rifiuta di caricare, sostituire o installare il driver della stampante corrente. Questa guida spiega ogni causa e illustra ogni soluzione in ordine logico.

---

## Cos'è l'Errore 0x00000032?

Quando si installa una stampante o ci si connette a essa, Windows visualizza:

> **Impossibile completare l'operazione (Errore 0x00000032). La richiesta non è supportata.**

![Finestra di dialogo di errore di Windows con l'errore 0x00000032 — La richiesta non è supportata](/images/printer-error-0x00000032/step1.png)

**I sintomi tipici includono:**
- L'aggiunta di una stampante USB locale fallisce immediatamente con questo errore
- La connessione a una stampante condivisa su LAN fallisce su un PC client ma funziona sugli altri
- L'aggiornamento del driver della stampante genera l'errore anche dopo un download aggiornato
- Il driver generico/universale si installa correttamente, ma il driver ufficiale restituisce 0x00000032

---

## Perché si Verifica l'Errore 0x00000032?

Esistono sei cause comuni:

1. **Incompatibilità del driver (più comune)** — La versione del driver non corrisponde al driver del PC host, un driver a 32 bit è mescolato con un sistema a 64 bit (o viceversa), oppure il driver non supporta la versione di Windows. Windows giudica non valida la richiesta di installazione e la blocca.
2. **File del driver residui che bloccano l'installazione** — File e voci del registro di un driver installato in precedenza non sono stati completamente rimossi. Il driver è trattenuto dal sistema e non può essere sovrascritto, causando l'errore 0x00000032.
3. **Guasto del servizio Spooler di stampa** — Il processo Spooler è bloccato o mantiene aperti i file del driver, impedendo la scrittura o il caricamento di un nuovo driver.
4. **Blocco dei criteri di installazione del driver di sistema** — I criteri di sicurezza di Windows bloccano l'installazione di driver non firmati o di terze parti.
5. **Errore di sincronizzazione del driver condiviso** — Quando ci si connette a una stampante condivisa su LAN, il PC host non dispone di un driver corrispondente per la versione di Windows del client (soprattutto in caso di mancata corrispondenza tra 32 bit e 64 bit). Windows rifiuta la richiesta di download del driver.
6. **Lieve danneggiamento dei file di sistema** — Componenti di Windows relativi alla stampa mancanti o danneggiati causano il blocco dell'interfaccia di installazione del driver.

---

## Soluzione

Seguire i passaggi seguenti in ordine. La maggior parte dei casi viene risolta con i passaggi 1 e 2.

### Passaggio 1 — Riavviare lo Spooler di stampa e cancellare la cache di stampa

**1.** Premere **Win + R**, digitare `services.msc` e premere Invio.

**2.** Trovare **Spooler di stampa** nell'elenco, fare clic destro e selezionare **Arresta**.

![Finestra Servizi — arresto del servizio Spooler di stampa](/images/printer-error-0x00000032/step2.png)

**3.** Premere **Win + R**, digitare `C:\Windows\System32\spool\PRINTERS` e premere Invio. Eliminare **tutti i file** all'interno della cartella (non eliminare la cartella stessa).

![Esplora file — eliminazione di tutti i file nella cartella spool PRINTERS](/images/printer-error-0x00000032/step3.png)

**4.** Tornare a Servizi, fare clic destro su **Spooler di stampa**, selezionare **Avvia** e impostare il **Tipo di avvio** su **Automatico**.

---

### Passaggio 2 — Rimuovere completamente il pacchetto driver residuo

L'errore 0x00000032 è causato più spesso da un vecchio driver bloccato. È necessario rimuovere il pacchetto driver completo, non solo il dispositivo stampante:

**1.** Aprire **Pannello di controllo → Dispositivi e stampanti**. Fare clic destro su ogni stampante difettosa o duplicata e selezionare **Rimuovi dispositivo**.

**2.** Premere **Win + R**, digitare `printui /s /t2` e premere Invio per aprire **Proprietà server di stampa**. Passare alla scheda **Driver**. Selezionare il driver della stampante problematica, fare clic su **Rimuovi**, quindi scegliere **Rimuovi driver e pacchetto driver** per eliminare tutti i file del driver.

![Proprietà server di stampa — rimozione completa del driver e del pacchetto driver](/images/printer-error-0x00000032/step4.png)

**3.** Riavviare il computer. Quindi scaricare il driver completo corretto dal sito web del produttore e installarlo.

---

### Passaggio 3 — Modificare i criteri di sistema per consentire l'installazione del driver

Nelle edizioni Professional ed Enterprise di Windows, i Criteri di gruppo potrebbero bloccare l'installazione del driver:

**1.** Premere **Win + R**, digitare `gpedit.msc` e premere Invio. Navigare fino a **Configurazione computer → Modelli amministrativi → Stampanti**.

**2.** Disabilitare **Non consentire l'installazione di stampanti con driver in modalità kernel** (se abilitato).

**3.** Abilitare **Consenti allo Spooler di stampa di accettare connessioni client**.

![Editor Criteri di gruppo — configurazione dei criteri di installazione del driver della stampante](/images/printer-error-0x00000032/step5.png)

---

### Passaggio 4 — Risolvere i problemi con la stampante condivisa (connessione LAN)

Se l'errore 0x00000032 appare solo quando ci si connette a una stampante condivisa sulla rete locale:

**1.** Sul **PC host**, aggiungere il pacchetto driver aggiuntivo corrispondente alla **versione di Windows del PC client** (32 bit o 64 bit). Farlo in **Proprietà server di stampa → Driver → Aggiungi**.

**2.** Sul **PC client**, abilitare **Abilita accesso guest non sicuro**: aprire **Criteri di gruppo locali → Configurazione computer → Modelli amministrativi → Rete → Workstation Lanman** e impostare **Abilita accessi guest non sicuri** su **Abilitato**.

**3.** Cancellare le credenziali di rete obsolete sul PC client: premere **Win + R**, digitare `control keymgr.dll`, aprire **Gestione credenziali** ed eliminare tutte le voci relative all'indirizzo IP o al nome del computer del PC host.

![Gestione credenziali — eliminazione delle credenziali di rete obsolete sul PC client](/images/printer-error-0x00000032/step6.png)

**4.** Riconnettersi utilizzando direttamente l'indirizzo IP del PC host: `\\192.168.1.x`.

---

### Passaggio 5 — Riparare i file di sistema

**1.** Cercare **cmd**, fare clic destro e selezionare **Esegui come amministratore**.

**2.** Eseguire il seguente comando e attendere il completamento:

```
sfc /scannow
```

![Prompt dei comandi — esecuzione di sfc /scannow per riparare i file di sistema](/images/printer-error-0x00000032/step7.png)

**3.** Riavviare il computer e riprovare a installare il driver della stampante.

---

## Domande Frequenti

**D: Cosa significa effettivamente l'errore 0x00000032 "La richiesta non è supportata"?**
Significa che Windows sta rifiutando la richiesta di installazione del driver. Il sistema ha determinato che la richiesta non è valida — il più delle volte perché il driver non corrisponde alla versione del sistema, un vecchio driver in conflitto è bloccato in posizione, oppure un criterio di sicurezza sta bloccando l'installazione.

**D: Il driver generico si installa correttamente, ma il driver ufficiale restituisce l'errore 0x00000032. Perché?**
Questo è quasi sempre causato da un pacchetto driver residuo bloccato da una precedente installazione. Il driver generico utilizza un percorso di installazione diverso, motivo per cui funziona. Eliminare il pacchetto driver completo tramite `printui /s /t2`, riavviare, quindi installare il driver ufficiale.

**D: Gli altri computer riescono a connettersi alla stampante condivisa — solo il mio riceve l'errore 0x00000032. Cosa devo fare?**
Il problema è specifico dell'ambiente driver del tuo PC client. Gli altri PC hanno driver compatibili; il tuo ha un conflitto con un vecchio driver o un'incompatibilità di versione. Pulire il driver solo sul proprio PC (passaggi 1 e 2) — non è necessario apportare modifiche al PC host o alla stampante.

**D: Ho reinstallato il driver più volte ma l'errore persiste. Cosa mi sfugge?**
Molto probabilmente hai eliminato il dispositivo stampante ma non il **pacchetto driver**. La semplice rimozione del dispositivo da Dispositivi e stampanti lascia i file del driver in posizione. Aprire `printui /s /t2`, andare alla scheda Driver ed eliminare completamente il pacchetto driver prima di reinstallare.

**D: Un'incompatibilità 32 bit/64 bit tra host e client può causare l'errore 0x00000032?**
Sì. Se il PC host è a 64 bit e il client è a 32 bit, e l'host non ha aggiunto un pacchetto driver a 32 bit, il client non può scaricare un driver corrispondente e Windows restituisce "La richiesta non è supportata." Aggiungere il driver dell'architettura mancante sul PC host nelle Proprietà server di stampa.

---

## Riepilogo

L'errore 0x00000032 durante l'installazione o la connessione di una stampante su Windows 10 o Windows 11 significa che Windows sta rifiutando la richiesta di installazione del driver. Non è correlato alla rete, all'hardware della stampante o a danni gravi al sistema. Risolvilo in questo ordine:

1. **Riavviare lo Spooler di stampa e cancellare la cache di stampa**
2. **Rimuovere completamente il vecchio pacchetto driver** tramite `printui /s /t2` — questa è la soluzione più comune
3. **Modificare i Criteri di gruppo** per consentire l'installazione del driver (edizioni Professional/Enterprise)
4. **Per le stampanti condivise**: aggiungere driver dell'architettura corrispondente sull'host, abilitare l'accesso guest sul client e cancellare le credenziali obsolete
5. **Eseguire `sfc /scannow`** per riparare eventuali file di sistema danneggiati

---

## Articoli Correlati

- [Come Risolvere l'Errore della Stampante Condivisa 0x00000006 su Windows](/it/posts/printer-sharing-error-0x00000006/)
- [Come Risolvere l'Errore della Stampante Condivisa 0x00000035 su Windows](/it/posts/printer-sharing-error-0x00000035/)
- [Come Risolvere l'Errore della Stampante Condivisa 0x00000709 su Windows](/it/posts/printer-sharing-error-0x00000709/)
