# PIANO DI TESTING E DEVOPS: Progetto KarateFlow

## 🎯 1. Obiettivo del Progetto

Il presente documento definisce la strategia di validazione e automazione per il progetto "KarateFlow". L'obiettivo è implementare un ciclo di vita del testing rigoroso, coprendo tutti i livelli della Test Pyramid, e condurre un'analisi comparativa tra paradigmi CI/CD (Jenkins vs GitHub Actions) in un ambiente Edge Computing (K3s su Raspberry Pi).

## 📋 2. Gestione e Ciclo di Vita dei Test (Test Lifecycle)

Il testing seguirà un ciclo di vita preciso e tracciabile:
1. **Test Design (BDD)**: I requisiti funzionali vengono tradotti in file `.feature` scritti in linguaggio Gherkin.
2. **Test Implementation**: Scrittura del codice di test associato ai vari livelli (Unit, Integration, E2E) utilizzando i framework designati.
3. **Test Execution**: Automazione totale tramite pipeline CI/CD.
4. **Test Reporting & Quality Gate**: Risultati e coperture inviati a SonarQube. La build viene interrotta se non rispetta i vincoli di qualità (>80% coverage, assenza di vulnerabilità).

---

## ⚙️ 3. Backend Testing Strategy (Java Spring Boot)

### 3.1 Unit Testing & Functional Testing
- **Obiettivo**: Validare la logica isolata dei Core Use Cases e il comportamento funzionale dei Controller REST.
- **Strumenti**: JUnit 5, Mockito e Hamcrest.
- **Dettaglio**: Hamcrest verrà utilizzato per scrivere asserzioni altamente leggibili e dichiarative migliorando la manutenibilità del codice rispetto alle asserzioni standard.

### 3.2 Integration Testing
- **Obiettivo**: Verificare l'interazione tra l'applicazione e il database, senza l'uso di DB in memoria (H2) che mascherano bug specifici del dialetto SQL/NoSQL.
- **Strumenti**: Testcontainers (MongoDB).
- **Dettaglio**: Implementazione del pattern Singleton Container per mantenere il container attivo durante tutto il ciclo JVM, riducendo i tempi di build di Maven.

### 3.3 Mutation Testing
- **Obiettivo**: Valutare la reale robustezza della suite di test unitari utilizzando il Mutation Testing. 
- **Strumento**: PIT (Pitest).
- **Dettaglio**: PIT altererà il bytecode Java creando dei "Mutanti". Se i test JUnit passano ugualmente, il mutante è sopravvissuto (indicando un test debole). L'obiettivo è "uccidere" una buona percentuale di mutanti (es. > 85%).

### 3.4 Performance & Load Testing
- **Obiettivo**: Verificare il comportamento del sistema sotto stress.
- **Strumento**: Apache JMeter.
- **Dettaglio**: Creazione di un Thread Group in JMeter per simulare l'interazione con utenti concorrenti che interrogano il motore matematico di "Confronto Test" (il task computazionalmente più pesante). Analisi di latenza, throughput e colli di bottiglia.

---

## 🎨 4. Frontend Testing Strategy (Angular)

### 4.1 Component Testing
- **Obiettivo**: Validare il rendering del DOM, il binding dei dati e la logica interna dei singoli componenti isolati dal resto dell'app.
- **Strumenti**: Vitest + jsdom.

### 4.2 System Testing & End-to-End (E2E)
- **Obiettivo**: Validare il sistema nella sua interezza, simulando un utente reale che interagisce con il browser, naviga l'interfaccia ed esegue operazioni.
- **Strumenti**: Playwright + Cucumber (Gherkin).
- **Dettaglio**: Playwright garantirà un'automazione cross-browser veloce e resiliente. I test saranno scritti utilizzando la sintassi BDD (Behavior-Driven Development).

Esempio Gherkin:
```gherkin
Scenario: Esecuzione test da Template
  Given l'allenatore è autenticato
  When crea un nuovo test usando il "Template Base"
  Then la griglia si popola e il salvataggio va a buon fine.
```

---

## 🚀 5. DevOps & CI/CD Comparative Analysis

L'infrastruttura farà da sfondo a un'analisi empirica sui sistemi di automazione:
- **GitHub Actions (Cloud/Runner)**: Installazione di un Self-Hosted Runner all'interno del cluster K3s per eseguire workflow YAML.
- **Jenkins (Server-Based)**: Esecuzione di Jenkinsfile dichiarativi tramite Multibranch Pipelines.

**Il Confronto**: Verrà redatta una relazione tecnica analizzando: tempi di esecuzione (a parità di hardware host), complessità di sintassi, curva di apprendimento e integrazione con il sistema di tracciabilità (GitHub Projects).

---

## 📦 6. Deliverables Accademici

Al termine del progetto, il pacchetto di consegna comprenderà:
- Codice sorgente aggiornato.
- Snapshot della SonarQube Dashboard (confronto prima/dopo).
- Report HTML di PIT (Mutation Testing).
- Report HTML di JMeter sui tempi di risposta.
- Registrazione video/log dell'esecuzione dei test E2E BDD guidati da Playwright.
- **Relazione Finale**: Documento riassuntivo con l'analisi comparativa Jenkins vs GitHub Actions.
- **Presentazione Latex**: Presentazione pdf dell'attività svolta e dei risultati raggiunti. 
