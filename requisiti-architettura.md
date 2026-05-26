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

Il progetto seguirà un approccio evolutivo e incrementale, strutturato per validare l'infrastruttura di build e automazione prima di stratificare la complessità del dominio applicativo.

### Fase 0: Setup Ecosistema e Bootstrapping (In corso)
* **Attività**: Creazione dell'Organizzazione GitHub e inizializzazione dei quattro repository (`karateflow-be`, `-fe`, `-infra`, `-docs`).
* **Documentazione**: Configurazione del file dei requisiti iniziali e apertura del Diario di Bordo (`01-setup.md`). Configurazione delle *Branch Protection Rules* sul branch `main`.

### Fase 1: Skeletons & setup pipeline CI/CD
* **Attività**: Generazione dello scheletro backend con Spring Boot Initializer e dell'applicazione frontend con Angular (CLI). 
* **Automazione**: Configurazione iniziale di Jenkins sulla macchina di sviluppo. Scrittura di un `Jenkinsfile` minimale per BE e FE mirato a validare esclusivamente i processi di clonazione del codice, installazione delle dipendenze e compilazione ad ogni commit.

### Fase 2: Analisi Funzionale e Modellazione del Dominio
* **Attività**: Raccolta dettagliata dei requisiti funzionali della piattaforma KarateFlow.
* **Progettazione**: Definizione formale delle entità di business (Atleta, Test, Esercizio) e stesura del contratto delle API REST (endpoint, metodi HTTP, payloads e risposte).

### Fase 3: Sviluppo Core BE
* **Infrastruttura (Terraform)**: Scrittura dei primi script Terraform nel repository `karateflow-infra` per configurare il cluster K3s sul Raspberry Pi 5, isolando il namespace logico dedicato allo Staging (`karateflow-staging`) e definendo le quote di risorse hardware.
* **Sviluppo**: Implementazione delle prime funzionalità CRUD del backend (gestione atleti e inserimento test) e scrittura dei test unitari isolati con JUnit 5, Mockito e Hamcrest.

### Fase 4: Provisioning Infrastruttura
* **Pipeline**: Estensione del `Jenkinsfile` per includere i test automatizzati, la containerizzazione dell'app (Docker multi-stage) e il primo deploy automatico nel cluster K3s di Staging.

### Fase 5: Sviluppo Frontend e Integrazione
* **Sviluppo**: Implementazione dell'interfaccia utente in Angular (creazione delle viste per l'inserimento dati e dashboard atleti).
* **Integrazione**: Connessione dei componenti Angular agli endpoint REST del backend sviluppati nella fase precedente. Esecuzione dei test di integrazione frontend.
* **Pipeline**: Automazione del rilascio del frontend statico all'interno dell'ambiente di Staging.

### Fase 6: Quality Management, Security & Production Rollout
* **Qualità**: Integrazione bloccante nella pipeline Jenkins dei controlli di qualità centralizzati su **SonarQube** (identificazione del debito tecnico, code smells, ESLint ecc..). 
* **Sicurezza**: Integrazione e configurazione di **Keycloak** per proteggere l'accesso agli ambienti (probabilmente verrà utilizzato proprio Kuberneetes come API Gateway, piuttosto che introdurre un microservizio apposito e mantenere leggero l'ambiente di produzione).
* **Produzione**: Utilizzo di Terraform per il provisioning del namespace di Produzione (`karateflow-prod`). Apertura della prima Pull Request formale su branch `main` per innescare il deploy stabile definitivo e configurazione dello **Stack PLG** per il monitoraggio dei log.


### Fase 6: Todo...