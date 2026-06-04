# 📔 Diario di Bordo - Fase 1: Skeleton & Automazione

## 📅 Dettagli della Fase
* **Periodo**: 31 Maggio / 04 Giugno 2026
* **Stato**: 🟢 COMPLETATA
* **Obiettivo**: Inizializzazione degli scheletri applicativi (Backend & Frontend), setup dei sistemi di analisi statica del codice e configurazione della pipeline CI/CD locale.

---

## 🎯 Obiettivi Raggiunti & Corrispondenza WBS

I Work Packages previsti per la Fase 1 sono stati completati e integrati nei rispettivi repository remoti:

1. **Skeleton Backend (WBS 1.2.1)**: Generato lo scheletro Spring Boot. Configurato l'albero dei package strutturato (`controller`, `service`, `repository`, `model`, `config`) e integrato il linter **PMD** nel `pom.xml` per il blocco automatico della build in caso di violazioni della qualità del codice.
2. **Skeleton Frontend (WBS 1.2.2)**: Configurato il workspace Angular tramite Angular CLI. Organizzate le directory secondo il pattern modulare Enterprise (`core`, `shared`, `features`), configurato **ESLint** per l'analisi statica di TypeScript e isolati i file di configurazione ambientale (`environment.ts`) per la mappatura dell'URL del backend.
3. **Automazione CI/CD (WBS 1.2.3)**: Istanziato un server Jenkins locale tramite Docker Compose. Scritti i rispettivi `Jenkinsfile` dichiarativi per l'automazione dei processi di build, testing e controllo qualità dei repository di Backend e Frontend.

---

## 🧠 Archivio delle scelte

Durante lo sviluppo della Fase 1 sono emersi vincoli tecnici reali che hanno richiesto un riallineamento delle strategie operative:

* **Rimozione del vincolo di "Review Approvals" per Sviluppatore Singolo**: L'attivazione iniziale delle regole di protezione rigide sul branch `main` impediva il merge delle Pull Request, in quanto GitHub non consente l'auto-approvazione delle proprie review. Al fine di mantenere il flusso formale delle PR la regola è stata rimodulata disattivando l'obbligo di approval ma mantenendo tassativo il passaggio da PR per l'unione del codice sul `main`.
* **Adozione del pattern "Poll SCM" in alternativa ai Webhook**: La configurazione standard prevedeva l'invio di Webhook push-driven da GitHub verso Jenkins. Tuttavia, trovandosi l'istanza Jenkins all'interno dell'ambiente locale (`localhost`) dietro NAT/Firewall domestico, i server cloud di GitHub non avrebbero potuto raggiungere il ricevitore. Per garantire il funzionamento di Jenkins si è optato per un approccio tramite **Poll SCM** programmato ogni 5 minuti.

---

## ⚠️ Note
* **Setup funzionante**: Gli scheletri compilano correttamente, i linter non rilevano anomalie e le pipeline su Jenkins completano la build con successo.
* **Prossimo passo**: Avvio della **Fase 2 (Analisi Funzionale)**. Spostamento delle macro-attività della WBS sulla bacheca e stesura dei requisiti formali.