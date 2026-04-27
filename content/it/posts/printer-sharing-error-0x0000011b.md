---
title: "Come correggere l'errore 0x0000011b nella condivisione stampante su Windows"
date: 2026-04-27
description: "Ricevi il messaggio 'Windows non riesce a connettersi alla stampante, errore 0x0000011b'? Questa guida mostra la soluzione tramite registro per ripristinare la connessione immediatamente."
tags: ["windows", "errore 0x0000011b", "stampante condivisa", "registro", "risoluzione problemi"]
categories: ["Risoluzione Problemi"]
cover:
  image: "/images/printer-error-0x0000011b/step1.png"
  alt: "Finestra di dialogo di errore Windows 0x0000011b"
  caption: "L'errore 0x0000011b appare quando Windows non riesce a connettersi alla stampante condivisa"
  relative: false
---

Le stampanti condivise sono indispensabili negli ambienti di ufficio e nelle reti domestiche. Tuttavia, un errore frustrante impedisce a molti utenti di connettersi:

> **«Windows non riesce a connettersi alla stampante. Operazione non riuscita con errore 0x0000011b.»**

![Finestra di dialogo di errore 0x0000011b su Windows](/images/printer-error-0x0000011b/step1.png)

Questo errore è diventato diffuso dopo un aggiornamento di sicurezza Microsoft (KB5005565, fine 2021). La soluzione richiede meno di due minuti.

---

## Perché si verifica l'errore 0x0000011b?

La causa è una modifica alla **privacy dell'autenticazione RPC**. Dopo l'aggiornamento, Windows richiede un livello di autenticazione più rigido che i sistemi più vecchi non sempre riescono a negoziare, causando l'errore `0x0000011b`.

---

## Soluzione — Modificare il Registro di Windows

> ⚠️ **Prima di iniziare:** Creare un backup del registro: **File → Esporta**.

### Passaggio 1 — Aprire l'Editor del Registro

Premere **Win + R**, digitare `regedit` e premere **Invio**.

![Ricerca di regedit nel menu Start di Windows](/images/printer-error-0x0000011b/step2.png)

### Passaggio 2 — Navigare alla chiave Print

Incollare il seguente percorso nella barra degli indirizzi e premere **Invio**:

```
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print
```

![Editor del Registro con il percorso Control\Print](/images/printer-error-0x0000011b/step3.png)

### Passaggio 3 — Creare un nuovo valore DWORD

1. Clic destro su un'area vuota nel pannello di destra.
2. Selezionare **Nuovo → Valore DWORD (32 bit)**.

![Menu contestuale per creare un nuovo valore DWORD](/images/printer-error-0x0000011b/step4.png)

3. Denominare il valore esattamente: `RpcAuthnLevelPrivacyEnabled`

### Passaggio 4 — Impostare il valore su 0

1. Doppio clic su `RpcAuthnLevelPrivacyEnabled`.
2. Inserire `0` nel campo **Dati valore**.
3. Fare clic su **OK**.

![Impostazione di RpcAuthnLevelPrivacyEnabled su 0](/images/printer-error-0x0000011b/step5.png)

### Passaggio 5 — Riavviare e verificare

Riavviare il PC e provare a connettersi nuovamente alla stampante condivisa.

![Coda di stampa che mostra la connessione riuscita](/images/printer-error-0x0000011b/step6.png)

---

## L'errore persiste?

- **Applicare la soluzione anche sul computer host** — stessa modifica al registro sul PC che condivide la stampante.
- **Verificare il Firewall** — consentire la condivisione file e stampanti su entrambi i computer.
- **Stessa rete** — entrambi i dispositivi devono essere sulla stessa sottorete.
- **Riavviare lo Spooler di stampa** — `services.msc` → **Spooler di stampa** → **Riavvia**.

---

## Riepilogo

| Passaggio | Azione |
|---|---|
| 1 | Aprire l'Editor del Registro (`Win + R` → `regedit`) |
| 2 | Navigare a `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print` |
| 3 | Creare un nuovo valore **DWORD (32 bit)** |
| 4 | Denominare `RpcAuthnLevelPrivacyEnabled` e impostare su `0` |
| 5 | Riavviare il computer |
