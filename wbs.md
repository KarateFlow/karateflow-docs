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
* **1.3.1 Modellazione del Dominio e Identificazione degli Attori (WP 2.1)**
  * 1.3.1.1 Identificazione e Profilazione delle Personas (Attori).
    * *Descrizione*: Definizione formale dei ruoli operativi (Coach e Atleta), mappando confini di accesso e visibilità.
    * *Rilascio*: Sezione "Attori del Sistema" nella documentazione.
  * 1.3.1.2 Definizione del Modello dei Dati Spaziale (Schema Entità MVP).
    * *Descrizione*: Mappatura analitica dei campi, tipi di dato e relazioni logiche (es. Atleta-Sessioni) per MongoDB.
    * *Rilascio*: Dizionario dati e schemi JSON/BSON nel diario di bordo.
* **1.3.2 Formalizzazione delle Specifiche ed Elaborazione dei Ticket (WP 2.2)**
  * 1.3.2.1 Scrittura del documento dei Requisiti Funzionali (`core-requirements.md`).
    * *Descrizione*: Stesura specifiche in `karateflow-docs` con Epiche e Storie (ID, Type, Title, Description, Status, Assignee, Priority).
    * *Rilascio*: File `core-requirements.md` fuso in `main`.
  * 1.3.2.2 Definizione dei Criteri di Accettazione in Formato BDD.
    * *Descrizione*: Elaborazione scenari *Given-When-Then* per ogni User Story.
    * *Rilascio*: Scenari BDD integrati in `core-requirements.md`.
* **1.3.3 Governance della Bacheca e Configurazione della Tracciabilità (WP 2.3)**
  * 1.3.3.1 Configurazione del Workflow di Triage su GitHub Projects.
    * *Descrizione*: Implementazione flusso di classificazione e creazione card (da `KF-1` a `KF-5`) con metadati.
    * *Rilascio*: Kanban Board popolata in colonna "To Do".

#### 1.4 Fase 3: Sviluppo, Testing e consegna dell'MVP
* **1.4.1 Sviluppo e Integrazione Feature: Inserimento Nuovo Atleta (WP 3.1)**
  * 1.4.1.1 Implementazione Struttura Dati e Persistenza Atleta (Task: KF-1-BE-1).
    * *Descrizione*: Definizione della classe di dominio `Athlete` con annotazioni Spring Data MongoDB (campi: nome, cognome, dataNascita, noteMediche, ecc.). Strutturazione del repository estendendo `MongoRepository`.
    * *Rilascio*: Entità e Repository integrati nel codice sorgente di `karateflow-be`.
  * 1.4.1.2 Esposizione delle API REST di Scrittura e Validazione (Task: KF-1-BE-2).
    * *Descrizione*: Sviluppo del service layer e del REST Controller per l'endpoint `POST /api/v1/athletes`. Configurazione delle validazioni JSR-303 per campi obbligatori e test unitari con MockMvc.
    * *Rilascio*: Controller REST e Service funzionanti e testati in `karateflow-be`.
  * 1.4.1.3 Sviluppo Integrazione Client HTTP Frontend (Task: KF-1-FE-1).
    * *Descrizione*: Implementazione dell'interfaccia TypeScript `Athlete` e scrittura del servizio Angular per la gestione delle chiamate HttpClient verso l'endpoint POST.
    * *Rilascio*: Servizio HTTP Angular codificato nel modulo core di `karateflow-fe`.
  * 1.4.1.4 Implementazione Interfaccia di Registrazione Atleta (Task: KF-1-FE-2).
    * *Descrizione*: Sviluppo del componente UI per l'acquisizione dati (Reactive Forms) includendo validazioni client-side e gestione `noteMediche`.
    * *Rilascio*: Schermata Form di registrazione accessibile in `karateflow-fe`.

* **1.4.2 Sviluppo e Integrazione Modulo Consultazione e Aggiornamento (WP 3.2)**
  * 1.4.2.1 Implementazione API REST per Lettura ed Elenco (Task: KF-2-BE-1).
    * *Descrizione*: Estensione di controller e service per l'endpoint `GET /api/v1/athletes`. Recupero dei record dalla collection e mappatura nei DTO.
    * *Rilascio*: Endpoint GET integrato e verificato in `karateflow-be`.
  * 1.4.2.2 Sviluppo Componente UI Tabella Squadra (Task: KF-2-FE-1).
    * *Descrizione*: Sviluppo del componente di visualizzazione tabellare degli atleti con caricamento dati in `ngOnInit`.
    * *Rilascio*: Tabella riassuntiva elenco squadra in `karateflow-fe`.
  * 1.4.2.3 Configurazione Routing e Dettaglio Profilo (Task: KF-2-FE-2).
    * *Descrizione*: Configurazione rotte parametrizzate nell'Angular Router per la navigazione verso la scheda del singolo atleta.
    * *Rilascio*: Sistema di navigazione implementato in `karateflow-fe`.
  * 1.4.2.4 API REST di Aggiornamento Dati Atleta (Task: KF-3-BE-1).
    * *Descrizione*: Sviluppo dell'endpoint `PUT /api/v1/athletes/{id}` per la gestione delle modifiche anagrafiche e sanitarie.
    * *Rilascio*: Logica di update integrata in `karateflow-be`.
  * 1.4.2.5 Schermata e Form di Modifica Dati (Task: KF-3-FE-1).
    * *Descrizione*: Vista di dettaglio profilo in Angular con caricamento dati pregressi nel form e sottomissione richiesta PUT.
    * *Rilascio*: Interfaccia utente di editing completata in `karateflow-fe`.

* **1.4.3 Sviluppo e Integrazione Modulo Test Prestazionali (WP 3.3)**
  * 1.4.3.1 Modellazione e API di Registrazione Test Execution (Task: KF-4-BE-1).
    * *Descrizione*: Creazione della classe documentale `TestExecution` (collection `test_execution`) con array di `performedExercises`. Sviluppo endpoint `POST /api/v1/tests` con validazione dei parametri.
    * *Rilascio*: Componenti logici e API per la memorizzazione dei test in `karateflow-be`.
  * 1.4.3.2 Sviluppo Interfaccia Form Acquisizione Sessioni (Task: KF-4-FE-1).
    * *Descrizione*: Codifica del form reattivo Angular per l'inserimento di sessioni di test (tipologia, noteCoach) e multipli esercizi (`performedExercises`).
    * *Rilascio*: Schermata form di inserimento sessioni in `karateflow-fe`.
  * 1.4.3.3 Validazione Client-Side e Gestione Errori Formato (Task: KF-4-FE-2).
    * *Descrizione*: Sviluppo validatori per bloccare l'invio in caso di dati non numerici o non validi negli esercizi della sessione.
    * *Rilascio*: Logica di validazione e feedback di errore in `karateflow-fe`.
  * 1.4.3.4 API REST di Recupero Storico Performance (Task: KF-5-BE-1).
    * *Descrizione*: Implementazione dell'endpoint `GET /api/v1/tests?athleteId={id}` per estrazione cronologica decrescente delle sessioni dell'atleta.
    * *Rilascio*: Logica di query e sorting (athleteId + dataEsecuzione) in `karateflow-be`.
  * 1.4.3.5 Componente UI Elenco Cronologico Performance (Task: KF-5-FE-1).
    * *Descrizione*: Sviluppo modulo visivo per mostrare l'andamento dei test con possibilità di espandere le sessioni per vedere i dettagli degli esercizi svolti.
    * *Rilascio*: Storico prestazioni atleta visibile in `karateflow-fe`.

#### 1.5 Fase 4: Provisioning Infrastruttura
* **1.5.1 IaC Setup & Cluster Topology (WP 4.1)**
  * **1.5.1.1 Governance del Cluster e IaC (Story: KF-EPIC-3-STORY-1)**
    * 1.5.1.1.1 Bootstrap Raspberry Pi 5 OS e installazione cluster K3s (Task: KF-EPIC-3-STORY-1-TASK-1).
    * 1.5.1.1.2 Setup del progetto e autenticazione del provider Kubernetes in Terraform (Task: KF-EPIC-3-STORY-1-TASK-2).
    * 1.5.1.1.3 Provisioning dei namespace di staging e produzione tramite IaC (Task: KF-EPIC-3-STORY-1-TASK-3).
    * 1.5.1.1.4 Definizione di quote per le risorse CPU e RAM (Task: KF-EPIC-3-STORY-1-TASK-4).
  * **1.5.1.2 Manifesti Kubernetes e Topologia dello Storage Persistente (Story: KF-EPIC-3-STORY-2)**
    * 1.5.1.2.1 Configurazione di un volume SSD isolato da 10GB per il local-path provider di K3s (Task: KF-EPIC-3-STORY-2-TASK-1).
    * 1.5.1.2.2 Scrittura del manifesto di deployment stateful per MongoDB e PVC da 3GB (Task: KF-EPIC-3-STORY-2-TASK-2).
    * 1.5.1.2.3 Scrittura del deployment e del servizio ClusterIP del backend (Task: KF-EPIC-3-STORY-2-TASK-3).
    * 1.5.1.2.4 Scrittura del deployment e del servizio ClusterIP del frontend (Task: KF-EPIC-3-STORY-2-TASK-4).
    * 1.5.1.2.5 Configurazione delle regole di Ingress Traefik per la risoluzione interna (Task: KF-EPIC-3-STORY-2-TASK-5).
    * 1.5.1.2.6 Deploy del daemon Cloudflare Tunnel per accesso sicuro esterno in HTTPS (Task: KF-EPIC-3-STORY-2-TASK-6).

* **1.5.2 Containerizzazione e Continuous Delivery (WP 4.2)**
  * **1.5.2.1 Containerizzazione Immutabile e Profili Ambientali (Story: KF-EPIC-4-STORY-1)**
  * **1.5.2.2 Continuous Delivery Automatica via Jenkins (Story: KF-EPIC-4-STORY-2)**

#### 1.6 Fase 5: Incremento funzionale e Analisi avanzata performance
*(Verrà dettagliata alla fine della Fase 4.)*

