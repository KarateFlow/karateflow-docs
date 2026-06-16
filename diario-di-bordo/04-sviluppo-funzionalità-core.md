# 📔 Diario di Bordo Fase 4: Sviluppo Funzionalità Core

## 📅 Dettagli della Fase
**Periodo**: Seconda e Terza settimana di Giugno 2026
**Stato**: ✅ Completata (MVP Release)
**Obiettivo**: Sviluppo delle funzionalità core dell'anagrafica e delle performance atletiche.

---

## 🚀 Sintesi delle Attività e Traguardi

In questa fase abbiamo completato lo sviluppo dell'MVP (Minimum Viable Product), chiudendo le prime due Epiche di progetto: **Gestione Anagrafica Atleti** (Stories 1-3) e **Modulo Test Fisici** (Stories 4-5).

### Evoluzione dello Scope e Governance
Il percorso è iniziato con una forte discontinuità rispetto alle stime iniziali. La **Story 1** è passata da 4 a **14 task** a causa di gravi blocker emersi nell'ambiente di integrazione (Jenkins) e conflitti nelle configurazioni del database MongoDB.

Per gestire questa complessità e prevenire regressioni, abbiamo adottato una strategia di **standardizzazione rigorosa**:
*   **Template GitHub**: Introdotti modelli formali per Issue, Bug Report e Pull Request in tutti i repository.
*   **Pipeline CI/CD**: Modificati i `Jenkinsfile` per supportare il modulo frontend e integrata la notifica automatica dello stato dei commit (Commit Status) per un feedback immediato sulla qualità del codice.

### Progressi Funzionali
*   **Anagrafica (Epic 1)**: Implementato il ciclo completo di registrazione, elenco e modifica parziale, garantendo l'immutabilità dei dati sensibili (Nome/Cognome) e la validazione dei criteri di unicità.
*   **Performance (Epic 2)**: Sviluppato un sistema dinamico per la registrazione di sessioni multi-esercizio, con capacità di duplicazione rapida dei dati e visualizzazione storica cronologica ad accordion.

---

## 🛠️ Sfide Tecniche e Troubleshooting

La fase ha richiesto interventi mirati su diversi fronti per garantire la stabilità del sistema:

1.  **Blocker Infrastrutturali**: Risolti problemi di autorizzazione (403) del token Jenkins e timeout dei container Mongo nei test d'integrazione tramite ottimizzazione del context caching.
2.  **Qualità e Analisi Statica**: Superate molteplici violazioni PMD (BE) ed ESLint (FE), rifattorizzando la gestione delle eccezioni e la tipizzazione dei mock nei test unitari (Vitest).
3.  **Bug Logici**: Identificato e corretto un errore critico di "409 Conflict" nell'API di update che impediva il corretto salvataggio dei dati non anagrafici.

---

## 📋 Nota sullo Scope e Posticipi

Per focalizzare le risorse sulla stabilità dell'ambiente core, abbiamo scelto di **posticipare alcune funzionalità** alle fasi successive:
*   La gestione completa del ciclo di vita dei test (modifica e cancellazione fisica) è stata esclusa dall'MVP attuale per essere validata in un ambiente pre-produzione più fedele alla realtà operativa.

---

## 🏁 Chiusura Fase 3: Consegna MVP
Con la validazione finale dei test (BE/FE) e la stabilizzazione della pipeline, dichiariamo **chiusa la Fase 3**. Il sistema è pronto per configurare l'infrastruttura e fare il setup dell'ambiente di staging e produzione sul raspberry pi 5. 

---

## ⏭️ Prossimi Passi
*   **Fase 4: Provisioning Infrastruttura**.
*   Automazione del deploy e configurazione degli ambienti di staging/produzione.
