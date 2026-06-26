# Calcoli di Confronto e Miglioramento Complessivo

Questo documento descrive in dettaglio la logica matematica e di business utilizzata per calcolare le metriche di confronto, i trend temporali e il **miglioramento complessivo** all'interno dell'applicazione KarateFlow.

---

## 1. Calcoli per Singoli Esercizi (Punto-a-Punto)

Quando si confrontano due test (Test baseline $A$ e Test di confronto $B$), per ogni esercizio confrontabile vengono calcolati il **Delta** e la **Variazione Percentuale**.

### Formule Fondamentali

*   **Valore di Baseline ($A$):** Il risultato ottenuto nel test meno recente (antecedente).
*   **Valore di Confronto ($B$):** Il risultato ottenuto nel test più recente (posteriore).
*   **Delta ($\Delta$):** Rappresenta la differenza assoluta tra i due risultati.
    $$\Delta = B - A$$
*   **Variazione Percentuale ($\%_{\text{variazione}}$):** Rappresenta il cambiamento in rapporto alla situazione di partenza. Se $A \neq 0$:
    $$\%_{\text{variazione}} = \frac{B - A}{A} \times 100 = \frac{\Delta}{A} \times 100$$
    *Se $A = 0$, la variazione percentuale viene impostata convenzionalmente a $0.0\%$.*

---

## 2. Classificazione del Risultato (Migliorato / Peggiorato / Stabile)

La valutazione del progresso atletico non si basa solo sul segno del Delta, ma dipende strettamente dal tipo di esercizio e dal suo indicatore di performance (`greaterIsBetter`).

| Metrica dell'Esercizio (`greaterIsBetter`) | Delta ($\Delta$) | Classificazione | Esempio |
| :--- | :--- | :--- | :--- |
| **Alto è meglio (`true`)** | $> 0$ | **Migliorato** | Elevazione nel salto in alto (cm): $40 \rightarrow 45$ |
| **Alto è meglio (`true`)** | $< 0$ | **Peggiorato** | Forza massimale di Squat (kg): $120 \rightarrow 115$ |
| **Basso è meglio (`false`)** | $< 0$ | **Migliorato** | Tempo sui 100 metri sprint (secondi): $12.5 \rightarrow 12.0$ |
| **Basso è meglio (`false`)** | $> 0$ | **Peggiorato** | Tempo di reazione (ms): $180 \rightarrow 210$ |
| **Qualsiasi** | $= 0$ | **Rimasti Uguali (Stabile)** | Nessuna variazione nei valori di test |

### Calcolo del Miglioramento Firmato ($\%_{\text{miglioramento}}$)
Per rendere coerenti le variazioni nel calcolo aggregato, la variazione percentuale viene convertita in un valore di **miglioramento effettivo** firmato:
*   Se l'esercizio è *Alto è meglio* (`greaterIsBetter = true`):
    $$\%_{\text{miglioramento}} = \%_{\text{variazione}}$$
*   Se l'esercizio è *Basso è meglio* (`greaterIsBetter = false`):
    $$\%_{\text{miglioramento}} = -\%_{\text{variazione}}$$

Questo assicura che una riduzione del tempo nei $100\text{m}$ (variazione negativa) corrisponda a un miglioramento positivo nell'efficacia complessiva del piano di allenamento.

---

## 3. Il Problema della Media Aritmetica Semplice

Utilizzare la media aritmetica semplice per sintetizzare le performance complessive introduce una grave distorsione legata a **esercizi con valori assoluti molto piccoli**.

### Esempio Pratico di Distorsione:
Immaginiamo un atleta che esegue due soli test:
1.  **Stretching (cm)** (Alto è meglio): Passa da $3\text{ cm}$ a $1\text{ cm}$.
    *   $\Delta = -2\text{ cm}$
    *   $\%_{\text{variazione}} = -66.67\%$ (Peggioramento)
2.  **Panca Piana (kg)** (Alto è meglio): Passa da $100\text{ kg}$ a $102\text{ kg}$.
    *   $\Delta = +2\text{ kg}$
    *   $\%_{\text{variazione}} = +2.00\%$ (Miglioramento)

Se calcoliamo la **Media Aritmetica Semplice**:
$$\text{Media Aritmetica} = \frac{-66.67\% + 2.00\%}{2} = -32.33\%$$

**Il report indicherebbe un peggioramento complessivo del -32.33%.**
Questo risultato è fuorviante: l'atleta ha ottenuto una progressione atletica significativa nella panca piana (+2 kg su un carico elevato), mentre ha perso 2 cm nello stretching (valore fisicamente minimo, ma percentuale altissima a causa della baseline molto bassa).

---

## 4. La Soluzione: Media Ponderata sul Valore di Baseline

Per azzerare l'impatto distorsivo dei piccoli numeri, KarateFlow implementa una **Media Ponderata**. Ciascuna variazione percentuale viene moltiplicata per il valore iniziale assoluto ottenuto in quell'esercizio (usato come peso).

### Formula del Miglioramento Complessivo
$$\text{Miglioramento Complessivo} = \frac{\sum_{i=1}^{n} (\text{Baseline}_i \times \%_{\text{miglioramento}, i})}{\sum_{i=1}^{n} \text{Baseline}_i}$$

Dove:
*   $\text{Baseline}_i$ è il valore di partenza assoluto dell'esercizio $i$ (corrispondente al peso $w_i$).
*   $\%_{\text{miglioramento}, i}$ è il miglioramento percentuale firmato dell'esercizio $i$.

### Semplificazione Matematica
Espandendo la formula del miglioramento percentuale:
$$\text{Miglioramento Complessivo} = \frac{\sum_{i=1}^{n} \left(\text{Baseline}_i \times \pm \frac{\Delta_i}{\text{Baseline}_i} \times 100\right)}{\sum_{i=1}^{n} \text{Baseline}_i} = \frac{\sum_{i=1}^{n} (\pm \Delta_i)}{\sum_{i=1}^{n} \text{Baseline}_i} \times 100$$
*(Il segno $\pm$ è positivo per esercizi "Alto è meglio" e negativo per esercizi "Basso è meglio")*

### Calcolo dell'Esempio Precedente con la Media Ponderata:
*   **Stretching**: Peso $w_1 = 3$, Miglioramento $= -66.67\%$
*   **Panca Piana**: Peso $w_2 = 100$, Miglioramento $= +2.00\%$

$$\text{Miglioramento Complessivo} = \frac{3 \times (-66.67\%) + 100 \times (+2.00\%)}{3 + 100} = \frac{-200 + 200}{103} = 0\%$$

Il valore aggregato mostra ora un **$0.0\%$**, bilanciando equamente la perdita assoluta di 2 cm nello stretching con il guadagno assoluto di 2 kg nel sollevamento pesi. Se la panca fosse aumentata a $105\text{ kg}$ (con stretching sempre a 1 cm), il miglioramento complessivo pesato sarebbe stato del $+2.91\%$, riflettendo l'andamento reale dell'atleta.

---

## 5. Calcolo del Trend Temporale Storico

Nell'analisi dei trend temporali (andamento su più di due test nel tempo), il calcolo del riepilogo in alto segue una logica analoga a quella del confronto puntuale per misurare l'evoluzione dal primo all'ultimo giorno disponibile:

1.  **Analisi per Singolo Esercizio:**
    *   Vengono presi i punti di rilevazione ordinati cronologicamente.
    *   La baseline iniziale è il primo punto cronologico: $P_{\text{inizio}} = \text{result}_0$.
    *   La rilevazione più recente è l'ultimo punto: $P_{\text{fine}} = \text{result}_{n-1}$.
    *   La variazione è calcolata come:
        $$\Delta = P_{\text{fine}} - P_{\text{inizio}}$$
2.  **Esclusione dei Dati Insufficienti:**
    *   Gli esercizi che hanno meno di 2 punti di rilevazione registrati nel periodo temporale non possono determinare una variazione e vengono classificati come **Non Confrontabili (N/A)**.
3.  **Aggregazione Complessiva:**
    *   Tutti gli esercizi aventi almeno 2 punti partecipano alla media ponderata utilizzando il rispettivo valore di baseline iniziale $P_{\text{inizio}}$ come peso:
        $$\text{Miglioramento Complessivo Trend} = \frac{\sum (\text{Valore Iniziale}_i \times \%_{\text{miglioramento}, i})}{\sum \text{Valore Iniziale}_i}$$
