# PIT Mutation Testing - Baseline Report (Issue #60)

Questo documento traccia lo stato iniziale della copertura dei test (baseline) subito dopo l'introduzione di PIT (Pitest) nel progetto backend. L'analisi rivela i punti deboli della nostra test suite e fa da guida per gli interventi di irrobustimento previsti nella Issue #61 (Killing Mutants).

## 1. Fotografia Generale
- **Totale Mutanti Generati**: 283
- **Mutanti Uccisi (Killed)**: 186
- **Mutanti Sopravvissuti (Survived / No Coverage)**: 97
- **Mutation Coverage**: ~66%

*Significato: Circa 1 volta su 3, un'alterazione (potenziale bug) introdotta nel codice di produzione non fa fallire la test suite.*

---

## 2. Le Classi "Hotspot" (Target per la Issue #61)
Le classi in cui sopravvivono più mutanti indicano le aree del sistema meno coperte (o con asserzioni troppo deboli):

1. **`ReportMapper`** (28 sopravvissuti)
   Essendo ricco di logiche di mappatura e controlli (`if (obj == null)`), molti rami condizionali non vengono esplorati nei test. Se PIT forza la restituzione di `null` per una proprietà opzionale, nessun test attualmente se ne accorge.
2. **`GlobalExceptionHandler`** (18 sopravvissuti)
   L'handler globale gestisce i fallback delle eccezioni (Spring). Attualmente mancano test unitari mirati per verificare l'esatto payload e lo status HTTP di risposta per i singoli scenari d'errore.
3. **`ReportCalculator`** (7 sopravvissuti)
   Core domain critico. La sopravvivenza dei mutanti qui indica che le nostre asserzioni (es. sui calcoli delle medie o delle percentuali) non sono sufficientemente restrittive per fallire se la logica aritmetica o l'arrotondamento vengono alterati.
4. **Vari Adapters & Config** (`AthleteRepositoryAdapter`, `MongoConfig`, `AthleteMapper`, ecc.)
   I restanti mutanti (tra 4 e 7 per classe) sono sparsi tra i livelli periferici del sistema, prevalentemente per l'assenza di casi d'uso limite nei test di integrazione.

---

## 3. Analisi dei Mutatori Sopravvissuti
Analizzare i *tipi* di mutazione sopravvissuta ci permette di comprendere i pattern errati o le mancanze nella stesura dei test:

- **`RemoveConditionalMutator` (41 sopravvissuti)**
  PIT ha rimosso (o forzato a `true`/`false`) i check logici di tipo `if`/`else`, e i test sono passati ugualmente. 
  *Conclusione:* Stiamo testando prevalentemente gli "Happy Path". I casi limite (es. gestione di collection vuote, parametri nulli) vengono ignorati.

- **`VoidMethodCallMutator` (24 sopravvissuti)**
  PIT ha rimosso chiamate interne a metodi `void` (come loggers o l'impostazione di proprietà accessorie), ma i test hanno dato esito verde.
  *Conclusione:* Mancano asserzioni sui *side-effect*. Se un metodo deve invocare un altro componente in stato di fire-and-forget, non lo stiamo validando tramite Mock (`verify()`).

- **`NullReturnValsMutator` (22 sopravvissuti)**
  PIT ha alterato l'output di metodi facendogli ritornare `null`, e nessun test ha fallito.
  *Conclusione:* Le asserzioni a valle sono assenti o troppo flessibili nei confronti di valori `null`.

## 4. Next Steps (Strategia per Issue #61)
Per azzerare il debito sui mutanti ed innalzare la resilienza del software, interverremo in tre direzioni:
1. **Happy Path vs Sad Path**: Aggiungere unit test specifici per coprire i rami `if/else` finora ignorati nei Mapper e negli UseCase.
2. **Global Exception Handling**: Scrivere una test suite dedicata per `GlobalExceptionHandler` e i vari custom exceptions.
3. **Asserzioni rigorose nel Core Domain**: Raffinare gli unit test su `ReportCalculator` con matchers Hamcrest precisi al millimetro, in modo da stroncare sul nascere qualsiasi alterazione matematica prodotta da PIT.
