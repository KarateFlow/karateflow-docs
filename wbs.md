# 📊 KarateFlow - Work Breakdown Structure (WBS) Semplificata

Adottiamo l'approccio *Rolling Wave Planning*: le fasi imminenti sono dettagliate e scomposte nei vari Work Package, mentre le fasi successive sono mantenute a livello macro e verranno espanse successivamente.

---

## 1. Scomposizione Gerarchica

### 1. KarateFlow Platform

#### 1.1 Fase 0: Setup & Bootstrapping
* **1.1.1 Configurazione dell'Ambiente Git** 
  * 1.1.1.1 Creazione dell'Organizzazione GitHub.
  * 1.1.1.2 Inizializzazione dei quattro repository (`karateflow-be`, `karateflow-fe`, `karateflow-infra`, `karateflow-docs`). 
  * 1.1.1.3 Configurazione delle *Branch Protection Rules* sul branch `main` dei repository di codice. 
* **1.1.2 Stesura della Documentazione Iniziale**
  * 1.1.2.1 Stesura della WBS nel file `wbs.md`. 
  * 1.1.2.2 Redazione dei file `README.md` e `requisiti-architettura.md` nel repository docs. 
  * 1.1.2.3 Redazione del file `branching-strategy.md` (Trunk-Based Development). 
* **1.1.3 Setup Development Board [Totale: 40m]**
  * 1.1.3.1 Creazione della bacheca `KarateFlow - Development Board` su GitHub Projects. 
  * 1.1.3.2 Caricamento dei Work Packages della Fase 0 e Fase 1 come *Issues* nella colonna Todo. 

#### 1.2 Fase 1: Skeletons & Setup Pipeline CI/CD
* **1.2.1 Inizializzazione dell'Architettura Backend**
  * 1.2.1.1 Generazione del progetto Spring Boot tramite Spring Initializr.
  * 1.2.1.2 Configurazione della struttura delle directory. 
  * 1.2.1.3 Integrazione e configurazione del plugin di analisi statica PMD.

* **1.2.2 Inizializzazione dell'Architettura Frontend**
  * 1.2.2.1 Generazione del workspace Angular tramite Angular CLI.
  * 1.2.2.2 Configurazione della struttura delle directory.
  * 1.2.2.3 Installazione e configurazione di ESLint per l'analisi statica di TypeScript.
  * 1.2.2.4 Configurazione dei file di ambiente per mappare l'URL base delle future API del backend.

* **1.2.3 Configurazione pipeline CI/CD di prova**
  * 1.2.3.1 Installazione e avvio dell'ambiente Jenkins sulla macchina di sviluppo locale. 
  * 1.2.3.2 Scrittura del `Jenkinsfile` minimale per il backend (fasi: Checkout, Maven Compile). 
  * 1.2.3.3 Scrittura del `Jenkinsfile` minimale per il frontend (fasi: Checkout, npm install, Build statica).
  * 1.2.3.4 Collegamento Jenkinsfiles alla repository su github.

#### 1.3 Fase 2: Analisi Funzionale e Raccolta dei Requisiti
*(Verrà dettagliata alla fine della Fase 1.)*
#### 1.4 Fase 3: Sviluppo Core Backend
*(Verrà dettagliata alla fine della Fase 2.)*

#### 1.5 Fase 4: Provisioning Infrastruttura
*(Verrà dettagliata alla fine della Fase 3.)* 

#### 1.6 Fase 5: Sviluppo Core Frontend
*(Verrà dettagliata alla fine della Fase 4.)*
