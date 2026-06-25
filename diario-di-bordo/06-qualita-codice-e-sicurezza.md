# 📔 Diario di Bordo Fase 5: Qualità del Codice, Analisi Statica & Sicurezza

## 📅 Dettagli della Fase
* **Periodo**: Quarta settimana di Giugno 2026
* **Stato**: ✅ Completata
* **Obiettivo**: Provisioning di SonarQube su K3s, scomposizione delle pipeline CI/CD in flussi dedicati di integrazione e rilascio, tracciamento sistemico della code coverage (JaCoCo per Spring Boot e Vitest per Angular) e configurazione del Quality Gate per inibire build instabili.

---

## 🎯 Obiettivi Raggiunti & Governance

In questa fase abbiamo integrato gli strumenti di qualità statica del codice e il tracciamento della code coverage all'interno del flusso automatizzato delle pipeline di Jenkins. L'obiettivo era garantire che ogni modifica al codice del backend o del frontend venisse preventivamente analizzata contro code smells, bug latenti e vulnerabilità di sicurezza, bloccando il rilascio in caso di mancato superamento del Quality Gate.

---

## 🚀 Stato Avanzamento: Completamento KF-EPIC-5

Tutti i compiti di integrazione della qualità, suddivisi in tre storie principali (`KF-E5-S1`, `KF-E5-S2`, `KF-E5-S3`), sono stati completati con successo.

### Dettaglio dei Risultati Raggiunti:

#### 1. Provisioning di SonarQube su K3s (`KF-E5-S1`)
*   **Database Dedicato**: Eseguito il deploy di PostgreSQL (`postgres-sonar`) nel namespace `kube-system` per garantire la persistenza dello storico dei dati delle analisi di SonarQube.
*   **Server SonarQube**: Configurato ed avviato il pod `sonarqube:lts-community` con connessione JDBC al database Postgres.
*   **Routing Ingress**: Mappate le regole di routing su Traefik per consentire l'accesso interno alla dashboard tramite l'URL **`http://sonarqube.local`**.
*   **Risoluzione Crash OOMKilled**: A causa dell'elevato sforzo computazionale dei sensori JavaScript/TypeScript durante l'analisi, il pod di SonarQube andava frequentemente in crash per esaurimento memoria (OOMKilled, exit code 137). Abbiamo riconfigurato i limiti di memoria del container portandoli da `2252Mi` a **`3Gi`**, ripristinando la stabilità del servizio.

#### 2. Pipeline Backend & Code Coverage JaCoCo (`KF-E5-S2`)
*   **Separazione CI/CD**: Scomposta la pipeline originale del backend in due pipeline separate: `Jenkinsfile.backend.ci` (Integrazione Continua, test e analisi qualità) e `Jenkinsfile.backend.cd` (Rilascio Continuo e deploy).
*   **Tracciamento Coverage**: Configurato il plugin Maven `jacoco-maven-plugin` nel file `pom.xml` del backend (`karateflow-be`). L'esecuzione dei test genera ora un report XML standard di coverage in `target/site/jacoco/jacoco.xml`.
*   **Integrazione SonarQube**: Aggiornato lo script della pipeline CI per eseguire il plugin `sonar-maven-plugin` che trasmette in automatico i risultati di compilazione e i dati di coverage a SonarQube tramite il token protetto `SONAR_TOKEN`.

#### 3. Pipeline Frontend, Compatibilità TypeScript & Vitest Coverage (`KF-E5-S3`)
*   **Separazione CI/CD**: Suddiviso il flusso del frontend in `Jenkinsfile.frontend.ci` e `Jenkinsfile.frontend.cd`.
*   **Coverage Vitest**: Installata la dipendenza `@vitest/coverage-v8` in `karateflow-fe` e aggiornata la sezione `test` del file `angular.json` per abilitare la coverage in formato `lcov` (generando il file `coverage/karateflow-fe/lcov.info`).
*   **Compatibilità Modulo TypeScript**: La versione di SonarQube in uso (9.9.8) non riconosce l'opzione `"module": "preserve"` definita nel `tsconfig.json` di Angular 21, causando il fallimento dei sensori TS. Abbiamo risolto creando un file dedicato [tsconfig.sonar.json](file:///Users/simoneingenito/Projects/KarateFlow/karateflow-fe/tsconfig.sonar.json) che forza temporaneamente `"module": "esnext"` e `"moduleResolution": "node"`, indicandone il percorso a SonarScanner.
*   **Risoluzione Errore Rosetta su ARM64 (Bug: `KF-E5-S3-B1`)**: Su agent Jenkins in esecuzione su architettura ARM64 (es. Apple Silicon), il wrapper NPM `@sonar/scan` falliva scaricando ed eseguendo un binario x86_64 del SonarScanner CLI (mancando il linker `/lib64/ld-linux-x86-64.so.2`). Abbiamo risolto riscrivendo la logica nel Jenkinsfile per rilevare l'architettura tramite `uname -m`, scaricare il pacchetto nativo appropriato (`linux-aarch64` o `linux-x64`) da `binaries.sonarsource.com`, memorizzarlo in cache ed eseguirlo direttamente.
*   **Rinvio Dinamico della Coverage**: Modificato `Jenkinsfile.frontend.ci` per raccogliere la coverage ed inoltrarla condizionatamente solo se il file `lcov.info` viene generato, prevenendo crash della pipeline su branch non ancora configurati.

---

## 🗺️ Dettaglio di Epiche e Stories

### 📊 [KF-EPIC-5] Code Quality & Security Pipeline Integration (Stato: ✅ Completata)
Questa epica definisce l'automazione del controllo qualità sul codice sorgente. Tramite l'introduzione di strumenti di analisi statica ed il tracciamento della code coverage su backend e frontend, si garantisce che nessuna regressione tecnica o buco di copertura possa finire nei branch stabili.

*   **[KF-E5-S1] Provisioning di SonarQube su K3s** (Stato: ✅ Completata)
    *   *Obiettivo*: Deployare l'ambiente di analisi centralizzato nel cluster locale.
    *   *Task completati*:
        *   [x] `KF-E5-S1-T1`: `feat(infra): deploy SonarQube server and Postgres database on K3s`
*   **[KF-E5-S2] Integrazione SonarQube su Backend** (Stato: ✅ Completata)
    *   *Obiettivo*: Separare le pipeline ed inserire il controllo di coverage del codice Java.
    *   *Task completati*:
        *   [x] `KF-E5-S2-T1`: `feat(infra): split backend pipeline into CI and CD and integrate SonarQube`
        *   [x] `KF-E5-S2-T3`: `feat(be): integrate JaCoCo for code coverage tracking`
*   **[KF-E5-S3] Integrazione SonarQube su Frontend** (Stato: ✅ Completata)
    *   *Obiettivo*: Separare le pipeline ed inserire il controllo di coverage del codice Angular/Vitest, risolvendo le incompatibilità su architetture miste.
    *   *Task/Bug completati*:
        *   [x] `KF-E5-S3-T1`: `feat(infra): split frontend pipeline into CI and CD and integrate SonarQube`
        *   [x] `KF-E5-S3-T3`: `feat(test): configure Vitest code coverage collection`
        *   [x] `KF-E5-S3-T4`: `feat(ci): add code coverage reporting to frontend pipeline`
        *   [x] `KF-E5-S3-B1`: `fix(ci): solve SonarQube scan failure on ARM64 agents`

---

## 🧠 Archivio delle Scelte

*   **Sdoppiamento delle pipeline in CI e CD**: Mantenere compilazione, unit test e analisi statica (CI) separati dal build dell'immagine Docker e deploy (CD) velocizza le verifiche sui pull request e riduce il carico computazionale del cluster K3s, evitando build di immagini non necessarie per codice non testato o non conforme al Quality Gate.
*   **Gestione dinamica di SonarScanner CLI**: Per gestire lo sviluppo multipiattaforma (computer Intel dei programmatori vs server Jenkins su ARM64/Apple Silicon), si è scelto di bypassare l'auto-download del modulo NPM e gestire programmaticamente tramite shell lo scaricamento del pacchetto nativo, assicurando prestazioni ottimali ed azzerando gli errori di emulazione Rosetta.
*   **Uso di tsconfig.sonar.json**: Anziché degradare lo standard del compilatore del frontend per accomodare i limiti di compatibilità della versione preinstallata di SonarQube, è stata scelta una configurazione di estensione ad-hoc utile solo per il processo di analisi statica.
*   **Ridimensionamento della memoria a 3Gi**: Per evitare fallimenti casuali durante le scansioni su progetti via via più complessi, si è preferito sovradimensionare la memoria per permettere ai motori di calcolo di lavorare in totale stabilità.

---

## ⏭️ Prossimi Passi
**Fase 6: Gestione Avanzata Test**.

