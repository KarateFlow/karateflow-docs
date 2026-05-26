# 📔 Diario di Bordo - Fase 0: Setup & Bootstrapping

## 📅 Dettagli della Fase
* **Periodo**: 26 Maggio 2026
* **Stato**: 🟢 COMPLETATA
* **Obiettivo**: Setup del progetto, dei repository e della documentazione.

---

## 🎯 Obiettivi Raggiunti & Corrispondenza WBS

Tutti i Work Packages pianificati per la Fase 0 sono stati eseguiti con successo e tracciati sulla bacheca di progetto:

1. **Infrastruttura Git (WBS 1.1.1)**: Creata l'Organizzazione GitHub ufficiale per segregare i progetti. Inizializzati i 4 repository indipendenti (`karateflow-be`, `karateflow-fe`, `karateflow-infra`, `karateflow-docs`). Attivate le *Branch Protection Rules* sul branch `main` per inibire i push diretti accidentali.
2. **Documentazione di Baseline (WBS 1.1.2)**: Redatti i documenti architetturali e operativi fondamentali, inclusi il `README.md` descrittivo, la specifica dei requisiti e la strategia di branching.
3. **Controllo Visivo (WBS 1.1.3)**: Configurato il GitHub Project (`KarateFlow - Development Board`) e popolata la colonna *Todo* con i task atomici della Fase 1.

---

## 🧠 Archivio delle Scelte

Durante questa fase di startup, sono state prese delle decisioni metodologiche che mi accompagneranno durante lo sviluppo del progetto:

* **Trunk-Based Development + GitHub Releases**: Considerando il vincolo di essere uno sviluppatore solo, ho optato per il Trunk-Based Development: lo sviluppo converge su `main` (collegato allo Staging), mentre i rilasci in Produzione sul Raspberry Pi 5 saranno governati dai **Git Tag** tramite le GitHub Releases.
* **Conventional Commits**: È stato adottato lo standard internazionale per la nomenclatura dei task e dei futuri commit su Git (usando i prefissi `feat`, `chore`, `docs`, `devops`).
* **Rolling Wave Planning per la WBS**: Ho espando la WBS segnando i work packages delle Fasi 0 e 1. Le fasi successive sono mantenute a livello macro e verranno espanse successivamente.
* **Rimozione delle Stime Temporali**: Ho scelto di non inserire scadenze orarie rigide sui singoli task, focalizzando l'attenzione sull'atomicità delle card (tutte dimensionate tra le 2 e le 4 ore di lavoro ideale).

---

## ⚠️ Note
* **Progetto cominciato**: L'ecosistema è pronto, i repository sono vuoti e protetti, la bacheca è allineata al 100% con il file `wbs.md`.
* **Prossimo passo**: Spostare il task `WBS 1.2.1.1` in *In Progress* e avviare la generazione dello skeleton Spring Boot.