# 📋 Specifica dei Requisiti Funzionali (Funzionalità Core)

Questo documento formalizza i requisiti funzionali per il *Minimum Viable Product* (MVP) di **KarateFlow**. La struttura segue rigorosamente la gerarchia Agile (**Epic → Story**) e applica la sintassi BDD (**Given-When-Then**) per i criteri di accettazione, garantendo la piena tracciabilità bidirezionale tra obiettivi di business e artefatti software.

---

## 👥 Attori del Sistema (Personas)

L'accesso e l'interazione con la piattaforma KarateFlow sono definiti dai seguenti ruoli operativi:

### 1. Coach (Istruttore/Amministratore)
*   **Profilo**: Responsabile della gestione tecnica della squadra.
*   **Responsabilità**: Registrazione atleti, inserimento test, monitoraggio performance.
*   **Visibilità**: Accesso completo ai dati della propria squadra.

### 2. Atleta (Utente Finale)
*   **Profilo**: Membro della squadra sportiva.
*   **Responsabilità**: Consultazione profilo e storico personale.
*   **Visibilità**: Sola lettura dei propri dati.

---

# [KF-EPIC-1] Gestione Anagrafica Atleti

## 🎯 Obiettivo
Implementare le funzionalità essenziali per l'amministrazione dei membri della squadra, consentendo l'archiviazione e la gestione dei dati personali e sanitari.

## 📋 Metadati
- **Stato:** ⏳ To Do
- **Priorità:** 🔴 Alta
- **Assegnatario:** 👤 Da assegnare

## ✅ Criteri di Accettazione Generali
- [ ] Persistenza dei dati su **MongoDB**.
- [ ] Operazioni CRUD esposte tramite **API REST**.
- [ ] Validazione univocità dei profili atleti.

## 🔗 User Stories collegate (Tasklist)
- [ ] **KF-1**: Inserimento Nuovo Atleta
- [ ] **KF-2**: Visualizzazione Elenco Atleti
- [ ] **KF-3**: Modifica Dati Atleta

---

# [KF-STORY-1] Inserimento Nuovo Atleta

## 📖 Descrizione
**COME** Coach  
**VOGLIO** inserire un nuovo atleta nel sistema (Nome, Cognome, Data Nascita, Note Mediche)  
**AFFINCHÉ** possa essere registrato ufficialmente e diventi disponibile per i test.

## 🛠️ Criteri di Accettazione (BDD)

**Scenario 1: Inserimento con dati validi**
- **GIVEN** che il Coach si trova nella schermata di creazione
- **WHEN** compila tutti i campi obbligatori con informazioni valide e salva
- **THEN** il sistema salva l'atleta su MongoDB, mostra successo e reindirizza alla lista.

**Scenario 2: Mancanza di campi obbligatori**
- **GIVEN** che il Coach si trova nella schermata di creazione
- **WHEN** tenta di salvare senza compilare i campi obbligatori
- **THEN** il sistema blocca l'invio e evidenzia gli errori nel form.

## 🔗 Riferimenti
- **Epic:** `KF-EPIC-1`
- **Documentazione:** [Modello Dati](domain-model.md)

---

# [KF-STORY-2] Visualizzazione Elenco Atleti

## 📖 Descrizione
**COME** Utente della piattaforma (Coach o Atleta)  
**VOGLIO** visualizzare una tabella con la lista di tutti gli atleti registrati  
**AFFINCHÉ** possa avere una panoramica della squadra e selezionare un profilo.

## 🛠️ Criteri di Accettazione (BDD)

**Scenario 1: Elenco popolato**
- **GIVEN** che vi sono atleti registrati nel database
- **WHEN** l'utente accede alla sezione "Atleti"
- **THEN** il sistema mostra una tabella con Nome, Cognome e Data di Nascita.

**Scenario 2: Navigazione al dettaglio**
- **GIVEN** che l'utente visualizza l'elenco
- **WHEN** seleziona un atleta specifico
- **THEN** il sistema reindirizza alla schermata di dettaglio del profilo.

## 🔗 Riferimenti
- **Epic:** `KF-EPIC-1`

---

# [KF-STORY-3] Modifica Dati Atleta

## 📖 Descrizione
**COME** Coach  
**VOGLIO** modificare le informazioni di un atleta esistente  
**AFFINCHÉ** possa tenere aggiornati i dati di contatto o le note mediche.

## 🛠️ Criteri di Accettazione (BDD)

**Scenario 1: Salvataggio modifiche**
- **GIVEN** che il Coach è in modalità modifica per un atleta
- **WHEN** aggiorna uno o più campi e clicca su "Aggiorna"
- **THEN** il backend sovrascrive il record esistente e mostra successo.

## 🔗 Riferimenti
- **Epic:** `KF-EPIC-1`

---

# [KF-EPIC-2] Modulo Test Fisici e Performance

## 🎯 Obiettivo
Gestire la registrazione e la strutturazione delle metriche prestazionali degli atleti per consentire il tracciamento dei parametri fisici nel tempo.

## 📋 Metadati
- **Stato:** ⏳ To Do
- **Priorità:** 🔴 Alta
- **Assegnatario:** 👤 Da assegnare

## ✅ Criteri di Accettazione Generali
- [ ] Record dei test collegati all'ID Atleta.
- [ ] Supporto per sessioni multiple cronologiche.

## 🔗 User Stories collegate (Tasklist)
- [ ] **KF-4**: Registrazione Sessione di Test
- [ ] **KF-5**: Visualizzazione Storico Performance Atleta

---

# [KF-STORY-4] Registrazione Sessione di Test

## 📖 Descrizione
**COME** Coach  
**VOGLIO** inserire una sessione di test contenente uno o più esercizi svolti (Titolo, Risultato, Unità, Tendenza miglioramento) associandoli a un atleta  
**AFFINCHÉ** le metriche prestazionali siano memorizzate in modo strutturato e granulare.

## 🛠️ Criteri di Accettazione (BDD)

**Scenario 1: Registrazione valida con più esercizi**
- **GIVEN** che il Coach ha selezionato un atleta
- **WHEN** inserisce i dati della sessione e aggiunge due esercizi (es. Salto Verticale e Corsa 30m) con i rispettivi risultati e salva
- **THEN** il sistema persiste un singolo documento di sessione su MongoDB contenente l'array degli esercizi, collegato all'atleta, e mostra successo.

**Scenario 2: Supporto ripetizioni dello stesso esercizio**
- **GIVEN** che il Coach sta inserendo una sessione
- **WHEN** aggiunge due volte lo stesso esercizio (es. due tentativi di "Salto Verticale") con risultati differenti
- **THEN** il sistema deve permettere il salvataggio di entrambi i record all'interno della stessa sessione.

**Scenario 3: Validazione formato**
- **GIVEN** che il Coach compila i dati di un esercizio
- **WHEN** inserisce un valore non numerico nel risultato
- **THEN** il sistema blocca il salvataggio della sessione e evidenzia l'errore nell'esercizio specifico.

## 🔗 Riferimenti
- **Epic:** `KF-EPIC-2`
- **Documentazione:** [Modello Dati](domain-model.md)

---

# [KF-STORY-5] Visualizzazione Storico Performance Atleta

## 📖 Descrizione
**COME** Utente (Coach o Atleta)  
**VOGLIO** visualizzare l'elenco cronologico di tutte le sessioni e dei singoli esercizi effettuati da un atleta  
**AFFINCHÉ** possa analizzare l'andamento delle prestazioni nel tempo.

## 🛠️ Criteri di Accettazione (BDD)

**Scenario 1: Visualizzazione storico sessioni**
- **GIVEN** che un atleta ha diverse sessioni registrate
- **WHEN** accedo alla sezione performance del suo profilo
- **THEN** il sistema mostra le sessioni in ordine decrescente; espandendo una sessione, devo poter vedere il dettaglio di tutti gli esercizi (PerformedExercise) svolti.

## 🔗 Riferimenti
- **Epic:** `KF-EPIC-2`
