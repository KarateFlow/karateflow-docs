# 🧬 Modellazione del Dominio e Schema Dati (WP 2.1.2)

In questa fase definiamo la struttura dei dati per l'MVP di **KarateFlow**, ottimizzata per la persistenza su **MongoDB**. Il modello segue una logica documentale, separando l'anagrafica degli atleti dalle sessioni di test per consentire una crescita scalabile dello storico prestazionale.

---

## 1. Entità: Atleta (Collection: `athletes`)

L'entità Atleta rappresenta il perno centrale del sistema. Ogni documento contiene le informazioni personali e cliniche necessarie per la gestione della squadra.

### 📋 Dizionario dei Dati
| Campo | Tipo | Descrizione | Vincoli |
| :--- | :--- | :--- | :--- |
| `_id` | ObjectId | Identificativo univoco generato da MongoDB | Primary Key |
| `nome` | String | Nome di battesimo dell'atleta | Obbligatorio |
| `cognome` | String | Cognome dell'atleta | Obbligatorio |
| `dataNascita` | Date | Data di nascita (formato ISO 8601) | Obbligatorio |
| `contattoRiferimento` | String | Email o numero di telefono del genitore/atleta | Opzionale |
| `noteMediche` | String | Annotazioni su infortuni, allergie o idoneità | Opzionale |
| `createdAt` | Timestamp | Data di creazione del profilo | Automatico |

### 📄 Esempio Documento JSON (BSON)
```json
{
  "_id": "64f1a2b3c4d5e6f7a8b9c0d1",
  "nome": "Mario",
  "cognome": "Rossi",
  "dataNascita": "2010-05-15T00:00:00Z",
  "contattoRiferimento": "+39 333 1234567",
  "noteMediche": "Leggera tendinite al polso sinistro. Nessuna allergia nota.",
  "createdAt": "2023-09-01T10:30:00Z"
}
```

---

## 2. Entità: Test Prestazionale (Collection: `test_execution`)

Questa entità registra una sessione di test fisici. Rispetto alla versione iniziale, il modello è stato reso granulare per supportare più esercizi (o ripetizioni dello stesso esercizio) all'interno di una singola sessione.

### 📋 Dizionario dei Dati (Test)
| Campo | Tipo | Descrizione | Vincoli |
| :--- | :--- | :--- | :--- |
| `_id` | ObjectId | Identificativo univoco della sessione | Primary Key |
| `athleteId` | ObjectId | Riferimento all'atleta (Foreign Key) | Obbligatorio, Indicizzato |
| `dataEsecuzione` | Date | Data in cui è stato eseguito il test | Obbligatorio |
| `tipologia` | String | Nome/Template della sessione (es. "Screening Iniziale") | Opzionale |
| `noteCoach` | String | Commenti tecnici generali sulla sessione | Opzionale |
| `performedExercises` | Array[Obj] | Lista degli esercizi svolti nella sessione | Almeno 1 elemento |
| `createdAt` | Timestamp | Data di registrazione a sistema | Automatico |

### 📋 Struttura Oggetto `PerformedExercise`
| Campo | Tipo | Descrizione |
| :--- | :--- | :--- |
| `exerciseTitle` | String | Nome dell'esercizio (es. "Salto Verticale", "Piegamenti") |
| `result` | Double | Valore numerico della performance |
| `unit` | String | Unità di misura (es. "cm", "sec", "n°") |
| `greaterIsBetter` | Boolean | Se `true`, un valore più alto indica un miglioramento (es. Salto). Se `false`, un valore più basso è migliore (es. Tempo Corsa). |

### 📄 Esempio Documento JSON (BSON)
```json
{
  "_id": "64f1a2b3c4d5e6f7a8b9c0d9",
  "athleteId": "64f1a2b3c4d5e6f7a8b9c0d1",
  "dataEsecuzione": "2023-09-10T15:00:00Z",
  "tipologia": "Valutazione Forza Esplosiva",
  "noteCoach": "Atleta molto motivato. Leggera asimmetria nella spinta.",
  "performedExercises": [
    {
      "exerciseTitle": "Salto Verticale",
      "result": 45.5,
      "unit": "cm",
      "greaterIsBetter": true
    },
    {
      "exerciseTitle": "Salto Verticale",
      "result": 47.2,
      "unit": "cm",
      "greaterIsBetter": true
    },
    {
      "exerciseTitle": "Tempo Reazione",
      "result": 0.25,
      "unit": "sec",
      "greaterIsBetter": false
    }
  ],
  "createdAt": "2023-09-10T15:05:00Z"
}
```

---

## 3. Relazioni e Strategia di Accesso

1.  **Relazione Atleta-Test**: È implementata tramite **Reference (1:N)**. Abbiamo scelto il riferimento rispetto all'embedding per evitare che il documento `Atleta` cresca indefinitamente con il passare degli anni, superando il limite di 16MB di MongoDB.
2.  **Indicizzazione**: Verrà creato un indice composto su `{athleteId: 1, dataEsecuzione: -1}` per velocizzare il recupero dello storico prestazioni di un singolo atleta ordinato cronologicamente.
