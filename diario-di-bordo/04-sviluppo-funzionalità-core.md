# 📔 Diario di Bordo Fase 4: Sviluppo Funzionalità Core

## 📅 Dettagli della Fase
- **Periodo**: Seconda settimana di Giugno 2026
- **Stato**: In Corso
- **Obiettivo**: Sviluppo delle funzionalità core dell'anagrafica e stabilizzazione dell'ecosistema CI/CD.

---

## 🚀 Riepilogo Avanzamento Epic 1: Gestione Anagrafica Atleti

L'attività della seconda settimana di giugno è stata dedicata al completamento delle prime tre User Story, segnando la chiusura della prima Epic di progetto. Il percorso è stato caratterizzato da una forte attività di risoluzione bug e miglioramento infrastrutturale.

### 1. Story 1 (KF-1): Inserimento Nuovo Atleta e Blocker Infrastrutturali
La prima storia ha richiesto un impegno significativamente superiore alle stime iniziali (espandendosi da 4 a 14 task). La causa principale è stata l'instabilità della pipeline Jenkins e dei test d'integrazione.
*   **Correzioni**: È stato necessario isolare i conflitti di configurazione MongoDB, adeguare il backend a **Spring Boot 4.0.6** e risolvere i timeout dei container tramite la centralizzazione del context caching.
*   **Standardizzazione**: Per prevenire regressioni e gestire la complessità crescente, sono stati introdotti **GitHub Issue & PR Templates** in tutti i repository, forzando checklist di validazione (test, lint, PMD) prima di ogni merge.

### 2. Story 2 (KF-2): Visualizzazione Elenco e Dettaglio Profilo
Implementata la navigazione completa e la visualizzazione granulare dei dati.
*   **Innovazione**: Utilizzo delle nuove API **Resource di Angular 19** per la gestione del caricamento asincrono.
*   **CI/CD**: Creata e stabilizzata la **Jenkins Pipeline per il Frontend**, integrata con le API di GitHub per la notifica automatica dello stato del commit direttamente nelle Pull Request.

### 3. Story 3 (KF-3): Modifica Dati Atleta
Chiusura del ciclo CRUD con la funzionalità di aggiornamento parziale.
*   **Integrità**: Il sistema garantisce ora l'immutabilità di Nome, Cognome e Data di Nascita, permettendo la modifica solo di contatti e note mediche.
*   **UX Personalizzata**: Sostituiti i dialoghi nativi del browser con un componente **ConfirmDialog custom**, garantendo un'esperienza utente moderna e accessibile.

---

## 🏁 Chiusura Epic 1
Con la validazione finale dei test, dichiaro **chiusa la Epic [KF-EPIC-1]**. L'anagrafica atleti è ora solida, testata e pronta per l'uso operativo.

---

## ⏭️ Prossimi Passi
*   Apertura della **Epic [KF-EPIC-2]: Modulo Test Fisici e Performance**.
*   Sviluppo della Story **KF-4**: Modellazione e registrazione delle sessioni di test (Test Execution).
