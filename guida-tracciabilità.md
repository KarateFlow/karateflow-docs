# 📘 Linee Guida per lo Sviluppo: Gestione dei Requisiti e Tracciabilità Bidirezionale in GitHub

Questo documento stabilisce le linee guida per la gestione dei requisiti funzionali e la configurazione della tracciabilità bidirezionale all'interno dell'ecosistema GitHub. 

L'obiettivo è garantire la totale tracciabilità tra l'obiettivo di business (Epic/Story) e l'artefatto software (Codice/Pull Request).

---

## 1. Struttura dei Requisiti (Gerarchia Agile & BDD)

Ogni funzionalità del progetto deve seguire la scomposizione: **Epic → User Story → Scenario BDD**.

### 1.1 Configurazione delle Epic (Livello Macro)
Un'Epic rappresenta un blocco di funzionalità significativo. Deve contenere:
* **Identificativo Univoco:** Es. `KF-EPIC-X`
* **Tabella dei Metadati:** Stato, Priorità (Alta/Media/Bassa), Assegnatario.
* **Descrizione:** Contesto di alto livello e obiettivi di business.
* **Criteri di Accettazione Generali:** Vincoli tecnologici o architetturali.

### 1.2 Struttura delle Stories (Livello Atomico)
Ogni Story deve essere focalizzata sull'utente finale e seguire il formato standard descrittivo:
* **COME [Ruolo/Utente]** (es. *Coach, Atleta, Amministratore*)
* **VOGLIO [Azione/Funzionalità]** (es. *inserire un nuovo atleta, visualizzare un grafico*)
* **AFFINCHÉ [Beneficio/Valore di Business]** (es. *possa essere registrato ufficialmente, possa analizzare i trend*)

### 1.3 Criteri di Accettazione in Sintassi BDD
Per eliminare l'ambiguità nei test e nello sviluppo, i criteri di accettazione *devono* essere scritti in formato **Gherkin / BDD (Behavior-Driven Development)**:
* **GIVEN (Dato che):** Il contesto o lo stato iniziale del sistema.
* **WHEN (Quando):** L'azione o l'evento scatenato dall'utente.
* **THEN (Allora):** Il comportamento atteso del sistema o il risultato visibile.

### 1.4 Template per Epic (GitHub Issue)

Da utilizzare nel corpo (descrizione) delle Issue di tipo Epic su GitHub.

```markdown
# [KF-EPIC-X] Titolo dell'Epic

## 🎯 Obiettivo
[Descrizione ad alto livello del contesto e dell'obiettivo di business]

## 📋 Metadati
- **Stato:** [To Do / In Progress / Done]
- **Priorità:** [Alta / Media / Bassa]
- **Assegnatario:** [@username]

## ✅ Criteri di Accettazione Generali
- [ ] [Vincolo o criterio architetturale/tecnologico 1]
- [ ] [Vincolo o criterio architetturale/tecnologico 2]

## 🔗 User Stories collegate (Tasklist)
- [ ] #ID_STORY_1 [Titolo Story 1]
- [ ] #ID_STORY_2 [Titolo Story 2]
```

### 1.5 Template per User Story (GitHub Issue)

Da utilizzare nel corpo (descrizione) delle Issue di tipo User Story su GitHub.

```markdown
# [KF-STORY-X] Titolo della Story

## 📖 Descrizione
**COME** [Ruolo/Utente]  
**VOGLIO** [Azione/Funzionalità]  
**AFFINCHÉ** [Beneficio/Valore di Business]

## 🛠️ Criteri di Accettazione (BDD)

**Scenario 1: [Titolo dello Scenario]**
- **GIVEN** [Contesto o stato iniziale del sistema]
- **WHEN** [Azione o evento scatenato dall'utente]
- **THEN** [Comportamento atteso del sistema]

**Scenario 2: [Titolo dello Scenario]**
- **GIVEN** [Contesto o stato iniziale del sistema]
- **WHEN** [Azione o evento scatenato dall'utente]
- **THEN** [Comportamento atteso del sistema]

## 🔗 Riferimenti
- **Epic:** #ID_EPIC
- **Documentazione:** [Link al file dei requisiti]
```

---


## 2. Flusso di Tracciabilità Bidirezionale su GitHub
GitHub offre strumenti nativi per creare una catena di tracciabilità che va dal requisito al codice sorgente, e viceversa. 

### Fase 1: Mappatura della Gerarchia su GitHub Issues
Issue Madre (Epic): Viene creata una Issue per l'Epic (es. KF-EPIC-1).

Tasklist Nativa: All'interno del corpo dell'Epic, si utilizza la sintassi markdown delle Tasklist di GitHub per elencare e creare le Issue "Figlie" (Stories):

```
- [ ] #12 Inserimento Nuovo Atleta
- [ ] #13 Visualizzazione Elenco Atleti
```

### Fase 2: Collegamento Documentazione-Issue
All'interno dei requisiti, l'ID della Story deve essere un link diretto alla Issue di GitHub corrispondente: 

```
### 📄 [KF-1](https://github.com/owner/repo/issues/12): Inserimento Nuovo Atleta.
```

Quando questo link viene salvato nel repository, GitHub inserisce automaticamente una notifica (un "Timeline Event") all'interno della Issue stessa, segnalando che l'elemento è stato menzionato nel file di specifica. Questo garantisce il collegamento permanente Documento ↔ Ticket.

---

## Fase 3: Sviluppo e Gestione dei Branch

All'interno dell'Issue della User Story su GitHub, si può utilizzare il pannello laterale destro sotto la voce Development e cliccare su "Create a branch".

Questo comando genera un branch pre-collegato all'Issue (es. `12-inserimento-nuovo-atleta`), stabilendo la tracciabilità Ticket ↔ Branch di Sviluppo.

---

## Fase 4: Chiusura Automatica e Revisione tramite Pull Request
Al completamento dello sviluppo, viene aperta una Pull Request (PR) verso il branch principale.

Nella descrizione della PR è obbligatorio inserire una Closing Keyword seguita dal numero dell'Issue della Story:

```
Closes #12
```

La PR mostrerà visivamente quale requisito sta implementando. Al momento del merge approvato della PR nel branch principale, GitHub chiuderà automaticamente l'Issue associata, aggiornando lo stato del progetto.