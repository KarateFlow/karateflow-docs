# 🥋 KarateFlow - Hub della Documentazione

Benvenuto nel repository centrale della documentazione di **KarateFlow**, una piattaforma web progettata per la gestione e il tracciamento periodico delle performance atletiche per il mio team di Karate.

Si tratta di una soluzione su misura per l'associazione sportiva in cui pratico karate. Eventualmente, in futuro, se il prodotto dovesse risultare valido e attraente per potenziali clienti, potrei estendere la piattaforma, passare ad un ambiente cloud e introdurre piani di pagamento generando profitto. 

Questo spazio raccoglie tutto il materiale accademico, l'analisi dei requisiti e il diario metodologico legati all'evoluzione del progetto.

---

## 🗺️ Mappa dei Repository del Progetto
La piattaforma è strutturata secondo un'architettura **Multi Repository**. Abbiamo:

* [karateflow-be](https://github.com/KarateFlow/karateflow-be) - Microservizio REST API (Spring Boot & MongoDB)
* [karateflow-fe](https://github.com/KarateFlow/karateflow-fe) - Interfaccia Utente SPA (Angular)
* [karateflow-infra](https://github.com/KarateFlow/karateflow-infra) - Infrastructure as Code (Terraform & K3s)
* **karateflow-docs** - *Questo repository (Documentazione e Slide)*

---

## 📂 Indice della Documentazione

Per navigare tra i contenuti del progetto, utilizza i link diretti ai documenti d'analisi:

### 📋 1. [Analisi dei Requisiti e Architettura](./requisiti-architettura.md)
Il documento ufficiale che descrive:
* Le motivazioni del progetto.
* Lo stack tecnologico scelto.
* La strategia DevOps (Jenkins locale e deploy su raspberry PI 5 con K3s).
* La roadmap evolutiva.

### 📝 2. Diario di Bordo (Tracciamento delle varie fasi di sviluppo)
La cronologia delle sessioni di sviluppo, i problemi riscontrati e il registro delle ottimizzazioni incrementali:
* 📁 [Fase 0: Setup & Bootstrapping](./diario-di-bordo/01-setup.md)
* 📁 [Fase 1: Skeleton & Automazione](./diario-di-bordo/02-skeleton-automazione.md)
* 📁 [Fase 2: Analisi Funzionale e Raccolta dei Requisiti](./diario-di-bordo/03-analisi-funzionale-e-governance.md)
* 📁 [Fase 3: Sviluppo Funzionalità Core (MVP)](./diario-di-bordo/04-sviluppo-funzionalità-core.md)
* 📁 [Fase 4: Provisioning Infrastruttura](./diario-di-bordo/05-provisioning-infrastruttura.md)
* 📁 [Fase 5: Disaccoppiamento DevOps e Setup Quality Gate](./diario-di-bordo/06-qualita-codice-e-sicurezza.md)

---

### 📝 3. Definizione di una Branching Strategy
Ho voluto definire formalmente la branching strategy che adotterò durante lo sviluppo del progetto all'interno del file [branching-strategy.md](./branching-strategy.md)

### 📊 4. WBS
Ho impostato una semplice WBS nel file [wbs.md](wbs.md) per tenere traccia dei task, che andrò a censire anche sulla bacheca di GitHub. Per ora non tengo conto dei tempi per non introdurre un overhead poco utile.

### 📢 5. Presentazione del Progetto
È disponibile la presentazione ufficiale del progetto KarateFlow:
* 📄 [Presentazione (PDF)](./presentazione/presentazione_progetto_SPME.pdf) - Slide della presentazione del progetto.
* 🛠️ [Sorgente LaTeX](./presentazione/presentazione.tex) - Codice sorgente della presentazione.

---

## 🚀 Come Consultare questo Progetto durante la Valutazione
1. Parti dal documento di **[Requisiti e Architettura](./requisiti-architettura.md)** per comprendere le scelte tecnologiche e i vincoli del server Raspberry Pi 5.
2. Consulta il **Diario di Bordo** per verificare come il software è cambiato nel tempo e come sono stati risolti i problemi infrastrutturali.
3. Esamina la bacheca attiva nella sezione **Projects** dell'Organizzazione GitHub per consultare la pianificazione dei compiti.