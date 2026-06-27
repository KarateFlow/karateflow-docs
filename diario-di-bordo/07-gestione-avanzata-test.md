# 📔 Diario di Bordo Fase 6: Gestione Avanzata Test (Template e Confronto)

## 📅 Dettagli della Fase
* **Periodo**: 26-27 Giugno 2026
* **Stato**: ✅ Completata
* **Obiettivo**: Implementazione della gestione dei Template di Test per l'autocompilazione dei form, sviluppo delle logiche di calcolo del confronto (delta assoluti/percentuali e overlap), persistenza dei report ed esportazione PDF client-side per minimizzare il consumo di risorse sul Raspberry Pi 5.

---

## 🎯 Obiettivi Raggiunti & Governance

In questa fase abbiamo arricchito significativamente le funzionalità analitiche di KarateFlow, trasformando la piattaforma da una semplice raccolta di dati ad uno strumento di supporto decisionale per i coach. L'introduzione di template riutilizzabili accelera notevolmente l'inserimento dei dati sul campo, mentre i grafici di andamento e l'esportazione dei report offrono una visualizzazione immediata dell'evoluzione atletica nel tempo.

---

## 🚀 Stato Avanzamento: Completamento KF-EPIC-6

Tutte le storie associate all'epica di gestione e confronto dei test (`KF-EPIC-6-STORY-0`, `KF-EPIC-6-STORY-1`, `KF-EPIC-6-STORY-2`, `KF-EPIC-6-STORY-3`) sono state chiuse con successo.

### Dettaglio dei Risultati Raggiunti:

#### 1. Completamento CRUD Core e Bugfixing (`KF-EPIC-6-STORY-0`)
*   **Endpoint di Gestione**: Implementati nel backend Spring Boot (`karateflow-be`) gli endpoint mancanti `GET` (dettaglio), `PUT` (modifica) e `DELETE` (eliminazione) per i Test, consentendo il controllo totale sui dati storici inseriti.
*   **Vista Dettaglio e Modali**: Sviluppata nel frontend Angular la vista read-only di dettaglio del test e la gestione dell'eliminazione e modifica sicura tramite finestre modali di conferma.
*   **Gestione Errori UI (Bug: `KF-E6-S0-B1`)**: Risolto il problema per cui l'interfaccia utente rimaneva bloccata senza feedback in caso di errori 4xx/5xx restituiti dal backend durante il salvataggio dei test, inserendo corretti toast/alert di notifica dell'errore.

#### 2. Gestione Template di Test (`KF-EPIC-6-STORY-1`)
*   **Modello Dati e REST API**: Creata la nuova collection MongoDB `TestTemplate` e realizzati i relativi endpoint REST per supportare la creazione, modifica, lettura e cancellazione di template predefiniti di esercizi.
*   **Interfaccia di Configurazione**: Sviluppata la schermata amministrativa per la gestione dei template, che consente di comporre liberamente la lista degli esercizi da associare.
*   **Autocompilazione**: Integrato un selettore di template all'interno del form di creazione dei test. Alla selezione del template, la griglia del `FormArray` Angular si popola dinamicamente con gli esercizi previsti dal modello, velocizzando l'inserimento sul campo.

#### 3. Implementazione Confronto tra Test (`KF-EPIC-6-STORY-2`)
*   **Motore Matematico Delta**: Sviluppato l'algoritmo di calcolo nel backend per confrontare due test (punto-a-punto), ricavando la differenza assoluta e percentuale per ciascun esercizio comune.
*   **Flag Overlap Insufficiente**: Implementato il controllo di copertura: se gli esercizi condivisi fra i test coprono meno del 30% del totale, il sistema restituisce il flag `lowOverlap: true` per allertare il coach. Gli esercizi presenti in un solo test mostrano il valore `"N/A"` nei delta.
*   **Trend Storici**: Aggiunta la query temporale per aggregare ed ordinare cronologicamente i risultati di performance dell'atleta.
*   **Dashboard Grafica**: Integrata nel frontend la libreria `ng2-charts` (Chart.js) per mostrare grafici a barre e a linee dell'andamento atletico. Aggiunti colori semantici (verde per miglioramenti, rosso per peggioramenti) e banner di alert per coperture insufficienti.

#### 4. Persistenza dei Report ed Export PDF (`KF-EPIC-6-STORY-3`)
*   **Collezione Reports**: Introdotta la collezione MongoDB `reports` per archiviare snapshot immutabili dei confronti. Questo evita di rieseguire costosi ricalcoli ad ogni visualizzazione.
*   **Re-idratazione Grafici**: Sviluppata la tab "Report Salvati" che elenca le analisi passate dell'atleta e le ricarica istantaneamente sfruttando il payload JSON persistito.
*   **Esportazione PDF Client-Side**: Implementata la generazione del PDF direttamente sul client tramite `html2canvas` e `jspdf`. Il sistema acquisisce il DOM contenente tabelle e grafici, formattando il report in un foglio A4 e scaricando autonomamente il file. Il backend è totalmente sgravato da questo compito.

---

## 🧠 Archivio delle Scelte

*   **Esportazione PDF Lato Client**: Per non sovraccaricare le risorse hardware limitate del server Raspberry Pi 5 (su cui gira il cluster K3s), abbiamo delegato la conversione in PDF interamente al browser dell'utente (tramite `jspdf` e `html2canvas`). Ciò evita l'installazione di librerie pesanti (come Puppeteer o motori Headless Chrome) nel backend.
*   **Endpoint Preview Non Persistente**: L'uso dell'endpoint `POST /api/v1/reports/preview` consente al coach di generare confronti al volo in memoria, salvando sul database solo quando è soddisfatto del risultato.
*   **Soglia lowOverlap al 30%**: Questa metrica previene conclusioni affrettate o confronti fuorvianti tra test con profili atletici e tipologie di esercizi quasi completamente scollegate tra loro.
*   **Media Ponderata vs Media Aritmetica per il Miglioramento Complessivo**: Come dettagliato nel file di analisi [logica-confronto-test.md](file:///Users/simoneingenito/Projects/KarateFlow/karateflow-docs/analisi/logica-confronto-test.md), l'uso della media aritmetica semplice introdurrebbe distorsioni significative in presenza di esercizi con valori iniziali (baseline) molto piccoli (ad esempio, un modesto peggioramento assoluto su piccoli numeri si tradurrebbe in una percentuale elevatissima, oscurando grandi miglioramenti assoluti ottenuti su carichi elevati). KarateFlow risolve questo problema applicando una **Media Ponderata** che utilizza il valore di baseline iniziale di ciascun esercizio come peso, offrendo un indice di miglioramento complessivo equo, realistico e coerente con la progressione dell'atleta.

---

## ⏭️ Prossimi Passi

Le prossime epiche pianificate ed impostate riguarderanno:
1.  **Sicurezza**: Rafforzamento dei meccanismi di autenticazione e autorizzazione (RBAC) e protezione degli endpoint sensibili.
2.  **Ristrutturazione dell'Interfaccia Utente**: Revisione stilistica e usabilità complessiva (dashboard responsive, micro-interazioni).
3.  **Monitoring**: Setup di grafici di telemetria e alert sullo stato dei servizi e del cluster K3s.

Una volta completate queste ultime tre epiche, l'applicazione sarà finalmente rilasciata in produzione.
