# 📋 Test Design Specification & Lifecycle Management

**Progetto**: KarateFlow  
**Fase**: Software Testing & DevOps Analysis  

---

## 1. Gestione e Ciclo di Vita del Test (Test Lifecycle)

Per garantire la massima tracciabilità e manutenibilità, ogni test all'interno del progetto KarateFlow (sia Backend che Frontend) seguirà uno stretto ciclo di vita a stati:

1. **Design (Draft)**: Per il backend, la progettazione dei test segue i gap di copertura evidenziati da SonarQube. Per il frontend, le funzionalità verrannò descritte utilizzando scenari Gherkin. 
2. **Implementation**: Scrittura effettiva del codice di test.
   - *Per l'E2E*: Traduzione degli scenari Gherkin in Step Definitions mappate sui Page Objects.
   - *Per Unit/Integration*: Scrittura delle classi di test sfruttando i pattern Arrange-Act-Assert.
3. **Automated (Active)**: Il test è integrato nel repository e agganciato alle pipeline CI/CD (Jenkins / GitHub Actions). Diventa parte del Quality Gate.
4. **Quarantine (Muted)**: Se un test genera "Flaky Tests" (falsi positivi/negativi intermittenti), viene temporaneamente disabilitato (es. `@Disabled` in JUnit o `.skip` in Vitest/Playwright) e viene aperto un task per la sua risoluzione.
5. **Deprecated**: Il test viene eliminato quando la funzionalità associata viene rimossa o modificata strutturalmente.

---

## 2. Mapping della Test Pyramid e Strumenti

### 🟢 Livello 1: Unit & Component Testing
Verifica atomica di singole classi o funzioni in totale isolamento.
- **Backend (Java)**: JUnit 5 + Mockito. I test isolano la logica del Core Domain (Use Cases) mockando i Persistence Adapters.
- **Frontend (Angular)**: Vitest + jsdom + Angular TestBed.
  - *Component Testing*: Verifica che i componenti UI renderizzino correttamente l'HTML e gestiscano gli input/output senza dipendere dal server reale.

### 🟡 Livello 2: Integration & Functional Testing
Verifica l'interazione tra i moduli dell'applicazione e le dipendenze esterne.
- **Backend Integration**: Spring Boot Test + Testcontainers. Verifica che le query ai repository JPA/Mongo funzionino su un database reale (sollevato come Singleton Container per abbattere i tempi di esecuzione).
- **Backend Functional**: Invocazione degli endpoint REST isolati. Utilizzo di Hamcrest per le asserzioni.
  - *Perché Hamcrest?* Garantisce asserzioni dichiarative e semanticamente vicine al linguaggio umano (es. `assertThat(response.getStatusCode(), is(equalTo(HttpStatus.OK)));`).

### 🔴 Livello 3: System & End-to-End (E2E) Testing
Verifica il sistema nella sua interezza simulando l'utente finale.
- **Strumenti**: Playwright + Cucumber (Gherkin).
- **Architettura E2E**:
  - *Feature Files*: Documenti `.feature` scritti in linguaggio Gherkin.
  - *Playwright*: Simula il browser (Chromium, Firefox, WebKit) navigando l'interfaccia Angular e innescando vere chiamate HTTP verso il Backend (che a sua volta scriverà sul DB Testcontainers).
  - *Page Object Model (POM)*: I locatori CSS/XPath del frontend verranno incapsulati in classi dedicate per ridurre la fragilità dei test al variare della UI.

### 🟣 Livelli Trasversali (Advanced Testing)
- **Mutation Testing (Backend)**: Utilizzo di PIT (Pitest). PIT altera il bytecode Java (es. cambia `==` in `!=`) e verifica se la suite di test fallisce. Misura la reale robustezza dei test, puntando a una Mutation Score > 80%.
- **Performance Testing (Backend)**: Utilizzo di Apache JMeter. Simulazione di carico (Load Testing) inviando richieste concorrenti all'endpoint di "Confronto Test" (il task computazionalmente più pesante) per valutarne latenza e throughput.

---

## 3. Esempi di Test Design Formale

### 3.1 Scenario E2E (System Testing con Gherkin)
- **Feature**: `Gestione Autocompilazione Template`
- **File**: `template_autofill.feature`

```gherkin
Feature: Autocompilazione dei Test fisici da Template
  Come allenatore
  Voglio selezionare un template predefinito
  Affinché gli esercizi si compilino automaticamente nel form

  Scenario: Compilazione corretta del template "Forza Base"
    Given l'allenatore è autenticato nella dashboard
    And naviga nella pagina "Registra Nuovo Test" per l'atleta "Mario Rossi"
    When seleziona il template "Forza Base" dal menu a tendina
    Then la griglia degli esercizi deve popolarsi istantaneamente con "Squat", "Panca Piana" e "Stacco"
    And il pulsante "Salva" deve rimanere disabilitato finché non vengono inseriti i risultati numerici
```

### 3.2 Scenario Functional/Integration (Backend con Hamcrest)
- **Target**: `TestTemplateController`
- **Tecnica**: Invio JSON e validazione struttura di risposta.

```java
@Test
@DisplayName("Dato un payload template valido, Quando viene inviato in POST, Allora ritorna 201 Created con i dati salvati")
void createTemplate_ReturnsCreated() throws Exception {
    // Arrange: Setup del payload JSON
    
    // Act: Chiamata tramite MockMvc o REST Assured
    ResultActions response = mockMvc.perform(post("/api/v1/templates").content(payload));
    
    // Assert: Asserzioni funzionali con Hamcrest
    response.andExpect(status().isCreated())
            .andExpect(jsonPath("$.name", is(equalTo("Forza Base"))))
            .andExpect(jsonPath("$.exercises", hasSize(3)))
            .andExpect(jsonPath("$.exercises", hasItems("Squat", "Panca Piana")));
}
```

### 3.3 Gestione dei Difetti nei Test (Mutation Testing)
- **Obiettivo**: Aumentare la Branch Coverage.
- **Dettaglio**: Se PIT segnala che un mutante è "sopravvissuto" (*Survived*) all'interno del metodo `calculateDelta()` (es. avendo mutato `if (value > 0)` in `if (value >= 0)`), lo sviluppatore deve creare un nuovo Unit Test specifico per il valore limite (`value == 0`) per "uccidere" il mutante, garantendo la copertura della *Boundary Value Analysis*.
