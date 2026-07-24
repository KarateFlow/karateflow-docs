# 📋 BACKLOG - EPIC 12: Advanced Software Testing & DevOps Analysis

**Descrizione**:  
Come QA Engineer e DevOps Specialist,  
voglio implementare una suite di test multilivello (Integration, E2E BDD, Mutation) e condurre un'analisi comparativa tra pipeline CI/CD,  
affinché il sistema raggiunga standard di qualità accademici (>80% coverage, alta resilienza ai mutanti) e si ottengano dati empirici per la scelta del miglior tool di automazione.

---

## 📖 STORY 1: Backend Quality Gate & Mutation Testing

- **Obiettivo**: Innalzare la code coverage del Backend, consolidare i test di integrazione con database reali e misurare la reale robustezza della suite tramite l'iniezione di difetti (mutanti).

### Criteri di Accettazione (DoD)
- [ ] La Code Coverage del repository backend supera l'80% nel Quality Gate di SonarQube.
- [ ] Il pattern "Singleton Container" per Testcontainers è implementato e i test girano in meno di 20 secondi.
- [ ] Il framework PIT (Pitest) è integrato nel `pom.xml`.
- [ ] È stato generato il report HTML di PIT con una Mutation Coverage adeguata, dimostrando l'eliminazione dei mutanti critici nel `TestComparisonService`.
- [ ] Sono presenti test funzionali sui Controller REST che utilizzano la sintassi dichiarativa di Hamcrest.

### Task Tecnici Suggeriti
- [ ] **[KF-T-E12-S1-T1]** Implementazione Singleton Pattern Testcontainers per MongoDB.
- [ ] **[KF-T-E12-S1-T2]** Scrittura Unit/Integration Test con Hamcrest sui Controller Core.
- [ ] **[KF-T-E12-S1-T3]** Setup PIT Mutation Testing e risoluzione mutanti sopravvissuti (Boundary Value Analysis).
- [ ] **[KF-T-E12-S1-T4]** Setup Apache JMeter e Load Testing sull'endpoint di "Confronto Test".

---

## 📖 STORY 2: Frontend UI & End-to-End Automation (BDD)

- **Obiettivo**: Automatizzare la navigazione utente sul client Angular e verificare i flussi critici end-to-end (dall'interfaccia grafica fino alla persistenza su database) adottando un approccio Behavior-Driven Development.

### Criteri di Accettazione (DoD)
- [ ] Il framework Playwright è installato nel repository frontend.
- [ ] Il preprocessore Cucumber è configurato per interpretare i file `.feature`.
- [ ] Esiste almeno un file `test_lifecycle.feature` scritto in Gherkin che descrive in linguaggio naturale il flusso di creazione e salvataggio di un Test Fisico.
- [ ] Esistono le Step Definitions mappate agli scenari Gherkin.
- [ ] È stato implementato il Page Object Model (POM) per astrarre i selettori CSS/XPath (es. `DashboardPage`, `AthleteFormPage`).

### Task Tecnici Suggeriti
- [ ] **[KF-T-E12-S2-T1]** Installazione e configurazione base di Playwright + Cucumber.
- [ ] **[KF-T-E12-S2-T2]** Definizione Page Object Model (POM) per le viste Angular principali.
- [ ] **[KF-T-E12-S2-T3]** Scrittura scenari Gherkin e Step Definitions per flusso "Creazione Test da Template".

---

## 📖 STORY 3: Analisi Comparativa CI/CD (Jenkins vs GitHub Actions)

- **Obiettivo**: Configurare un ambiente di Continuous Integration parallelo per raccogliere metriche reali e redigere il confronto finale per l'esame.

### Criteri di Accettazione (DoD)
- [ ] Sono presenti i file YAML in `.github/workflows` all'interno del branch `feature/exam-software-testing`.
- [ ] Le GitHub Actions si innescano parallelamente a Jenkins ad ogni push sul branch di test.
- [ ] Esiste un Self-Hosted Runner di GitHub configurato sul Raspberry Pi 5 (per garantire parità di condizioni hardware nel confronto dei tempi).
- [ ] È stata raccolta una base dati sufficiente (es. 10 esecuzioni) per stilare il documento finale.

### Task Tecnici Suggeriti
- [ ] **[KF-T-E12-S3-T1]** Deploy GitHub Self-Hosted Runner Pod su cluster K3s.
- [ ] **[KF-T-E12-S3-T2]** Traduzione del Jenkinsfile di CI backend in workflow GitHub Actions YAML.
- [ ] **[KF-T-E12-S3-T3]** Traduzione del Jenkinsfile di CI frontend in workflow GitHub Actions YAML.
