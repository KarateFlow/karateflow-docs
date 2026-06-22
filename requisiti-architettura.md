# KarateFlow - Documento dei Requisiti e Architettura Iniziale

Questo documento definisce i requisiti iniziali, lo stack tecnologico scelto, le motivazioni e la strategia di evoluzione incrementale per la piattaforma **KarateFlow**, sviluppata nell'ambito del corso di *Software Project Management and Evolution*. 

## 1. Introduzione e Motivazioni

**KarateFlow** nasce dall'esigenza reale di gestire una squadra di karate, automatizzando il tracciamento periodico delle performance atletiche su vari esercizi. La piattaforma permetterà di:
* Semplificare l'inserimento dei dati dei test eseguiti periodicamente.
* Centralizzare la gestione degli atleti.
* Generare metriche storiche, report di andamento e confronti avanzati tra le performance degli atleti.


## 2. Struttura dei Repository (Architettura Multi-Repo)

Ho individuato quattro repository principali che conterranno i vari moduli del progetto:

* **`karateflow-be`**: Repository contenente la logica di business e le API REST (Spring Boot).
* **`karateflow-fe`**: Repository dedicato all'interfaccia utente (Angular).
* **`karateflow-infra`**: Repository contenente le configurazioni per la virtualizzazione, l'orchestrazione, i file Terraform e gli asset DevOps di supporto.
* **`karateflow-docs`**: Ospita la documentazione accademica, il diario di bordo e il materiale per la presentazione finale.



## 3. Architettura del Sistema e Stack Tecnologico

Il perimetro applicativo sarà isolato all'interno di un server domestico Raspberry Pi 5, ottimizzando l'allocazione delle risorse per garantire la coesistenza con altri servizi. Questa scelta ha una precisa motivazione: **ridurre i costi di gestione il più bassi possibili**, trattandosi di una realtà piccola.  

### 3.1 Componenti Software
* **Frontend**: **Angular**. Compilato in modalità statica in fase di build per essere servito in modo leggero, riducendo al minimo l'impatto sulla memoria RAM in esecuzione.
* **Backend**: **Java / Spring Boot**. L'ecosistema Spring è estremamente conveniente grazie ad una suite vasta di componenti (Spring JPA, Annotations ecc..) ed inoltre implementa naturalmente il meccanismo di *Dependency Injection* che sarà molto utile in fase di definizione dei test.  
* **Database Principale**: **MongoDB** (NoSQL), per la gestione flessibile e schemaless delle metriche dei test atletici. L'adozione di un db documentale permetterà facilmente di creare varie tipologie di test ed estenderli a piacimento.  
**Database Accessorio (Opzionale/Fase avanzata)**: **PostgreSQL**, mirato specificamente alla gestione strutturata e relazionale dell'anagrafica utenti e dei ruoli.
* **Access Management**: **Keycloak**, per centralizzare l'autenticazione e la sicurezza (RBAC).

### 3.2 Strumenti di Testing e Qualità
**Testing Backend**: **JUnit 5**, **Mockito** e **Hamcrest**.
**Testing Frontend**: Suite nativa di Angular per test unitari e di integrazione riproducibili (ulteriori strumenti da stabilire successivamente).



## 4. Strategia DevOps: CI/CD e Infrastructure as Code (IaC)

L'adozione di un flusso automatizzato riduce al minimo l'intervento manuale, accelera il rilascio di nuove feature e garantisce controlli di qualità continui prima del rilascio in produzione. 

### 4.1 Orchestrazione delle Pipeline (Jenkins)
Per non stressare l'hardware del Raspberry Pi 5, l'orchestratore **Jenkins sarà ospitato sulla macchina di sviluppo locale**. Quest'ultima interagirà da remoto con il Raspberry Pi 5 (target di produzione).

### 4.2 Gestione a Doppio Ambiente: Staging e Produzione
In un'ottica di gestione professionale del rilascio del software, il progetto implementa una strategia a doppio ambiente (**Staging** e **Produzione**) interamente ospitata sul singolo Raspberry Pi 5. Questo approccio consente di testare le nuove release in un ambiente controllato ed equivalente a quello produttivo prima del rilascio ufficiale, mitigando drasticamente il rischio di crash o regressioni nell'applicazione reale.

### 4.3 Motivazioni sull'utilizzo di Terraform (IaC)
Sebbene la configurazione del Raspberry Pi 5 sia attuabile manualmente, l'introduzione di **Terraform** nel repository `karateflow-infra` è metodologicamente necessaria per soddisfare i seguenti requisiti di progetto:
* **Isolamento dell'Ambiente**: Definizione programmatica di un **Namespace Kubernetes** (`karateflow-prod`) segregato, imponendo dei *Resource Quotas* (limiti rigidi di CPU e RAM) per non impattare sugli altri progetti ospitati sul microcomputer.
* **Documentazione dell'Infrastruttura**: L'architettura sistemistica viene trattata come codice sorgente, tracciata su Git e storicizzata in linea con l'evoluzione applicativa.
* **Disaster Recovery**: Protezione contro l'eventuale corruzione dei supporti di memorizzazione del Raspberry Pi 5. Il ripristino dell'intero ecosistema (configurazioni cluster, database, volumi persistenti e network) può essere rigenerato tramite automazione in pochi minuti.


## 5. Tabella di Marcia per lo Sviluppo Incrementale

Il progetto segue un approccio *Rolling Wave Planning*, dove le fasi imminenti sono dettagliate e le successive sono mantenute a livello macro, permettendo flessibilità e adattamento.

### Fase 0: Setup Ecosistema e Bootstrapping (Completata)
* **Attività**: Creazione dell'Organizzazione GitHub, inizializzazione dei repository (`be`, `fe`, `infra`, `docs`) e configurazione delle *Branch Protection Rules*.
* **Documentazione**: Stesura della WBS, dei requisiti iniziali e della strategia di branching (Trunk-Based Development).
* **Governance**: Configurazione della *Development Board* su GitHub Projects.

### Fase 1: Skeletons & Setup Pipeline CI/CD (Completata)
* **Backend**: Inizializzazione progetto Spring Boot con analisi statica PMD.
* **Frontend**: Inizializzazione workspace Angular con ESLint.
* **Automazione**: Installazione Jenkins locale e scrittura dei `Jenkinsfile` minimali per validare compilazione e build statica.

### Fase 2: Analisi Funzionale e Raccolta dei Requisiti (Completata)
* **Modellazione**: Identificazione degli attori (Coach, Atleta) e definizione del modello dati spaziale per MongoDB.
* **Specifiche**: Redazione di `core-requirements.md` con Epiche, User Stories e criteri di accettazione in formato BDD (*Given-When-Then*).
* **Governance**: Configurazione del workflow di triage e popolamento della board con i Work Packages della Fase 3.

### Fase 3: Sviluppo, Testing e Consegna dell'MVP (In corso)
L'implementazione segue un approccio orientato alle feature (vertical slices), garantendo che ogni incremento porti valore funzionale immediato.
* **WP 3.1: Inserimento Nuovo Atleta**: Implementazione persistenza MongoDB, API REST di scrittura e form di registrazione reattivo in Angular.
* **WP 3.2: Consultazione e Aggiornamento**: API di lettura/update e componenti UI per la visualizzazione tabellare e la modifica del profilo atleta.
* **WP 3.3: Modulo Test Prestazionali**: Modellazione delle sessioni di test, API di registrazione performance e interfaccia di acquisizione dati multi-esercizio.

### Fase 4: Provisioning Infrastruttura (Completata)
* **IaC**: Utilizzo di Terraform per il provisioning del cluster K3s su Raspberry Pi 5.
* **CI/CD**: Estensione pipeline per containerizzazione (Docker) e deploy automatico in ambiente di Staging (https://staging.karate-flow.com).
* **Ambienti**: Configurazione dei namespace di Staging e Produzione con isolamento delle risorse e quote hardware.

### Fase 5: Incremento Funzionale e Miglioramento interfaccia web