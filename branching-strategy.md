# 🌿 Strategia di Branching: Trunk-Based Development & GitHub Releases

Questo documento definisce in modo formale la strategia di gestione del codice sorgente per l'ecosistema **KarateFlow**. Al fine di massimizzare l'efficienza dello sviluppatore solitario (io) e garantire un flusso di Continuous Integration (CI) fluido, il progetto adotta l'approccio **Trunk-Based Development (TBD)** accoppiato al meccanismo delle **GitHub Releases**.

---

## 🎯 1. Motivazioni della Scelta (Perché non GitFlow?)

I modelli tradizionali come *GitFlow* nascono per gestire team numerosi con cicli di rilascio lenti e strutturati. Per un progetto sviluppato da un singolo programmatore, GitFlow introduce un sovraccarico inutile (in questo contesto). 

L'adozione del Trunk-Based Development offre i seguenti vantaggi metodologici:
* **Frictionless Workflow**: Riduce al minimo i passaggi burocratici su Git, permettendo allo sviluppatore di concentrarsi sulla scrittura del codice.
* **Linea Storica Lineare**: Lo storico dei commit su Git si presenta come una linea retta e pulita, facilitando la tracciabilità delle modifiche e la stesura del Diario di Bordo.
* **DevOps-Centric**: Sposta la logica di separazione degli ambienti (Staging e Produzione) dai branch Git all'automazione della pipeline di CI/CD tramite l'uso di **Tag di versione**, in linea con le moderne pratiche industriali.

---

## 🌲 2. La Struttura dei Branch e dei Tag

Il ciclo di vita del codice ruota attorno a un unico branch persistente e all'uso di marcatori temporali (Tag).



### 🦾 Il "Trunk": branch `main`
* **Ambiente collegato**: **Staging** (Namespace Kubernetes: `karateflow-staging`)
* **Responsabilità**: È l'unica sorgente di verità del progetto. Tutto il codice stabile converge qui. Ogni singola modifica integrata in `main` innesca la pipeline Jenkins, che aggiorna automaticamente l'ambiente di Staging sul Raspberry Pi 5 per consentire i test.
* **Restrizioni**: Protetto tramite le impostazioni di GitHub. Non è consentito il push diretto dal terminale locale; l'integrazione avviene solo tramite Pull Request (PR).

### 🛠️ I Branch Temporanei: `feature/*` (es. `feature/setup-be`)
* **Ambiente collegato**: Locale (PC di sviluppo)
* **Responsabilità**: Branch cortissimi, dedicati all'implementazione di una singola funzionalità. Avranno una durata ideale di poche ore.
* **Ciclo di vita**: Nascono da `main` e muoiono non appena viene eseguito il merge su `main`. 

### 🏷️ I Marcatori di Release: I Tag Git (`vX.Y.Z`)
* **Ambiente collegato**: **Produzione** (Namespace Kubernetes: `karateflow-prod`)
* **Responsabilità**: Rappresentano i "fermo immagine" del codice ritenuto totalmente stabile in Staging. Quando viene creata una **Release** su GitHub (es. `v1.0.0`), Jenkins intercetta la creazione del Tag corrispondente e avvia il deploy nel perimetro di Produzione sul Raspberry Pi 5.

---

## 🔄 3. Reminder per lo sviluppo (Workflow Passo-Passo)

Seguirò rigorosamente questa sequenza di comandi per ogni task. 

### Passo 1: Preparazione del Terreno
Prima di iniziare a scrivere codice, allineo l'ambiente locale all'ultima versione stabile del server:
```bash
git checkout main
git pull origin main
```

### Passo 2: Apertura del branch di lavoro
```bash
git checkout -b feature/nome-del-task
```

### Passo 3: Sviluppo e Commit
Lavoro al codice, inserisco commit il più atomici possibili e con messaggi chiari:
```bash
git add .
git commit -m "feat(be): [descrizione...]"
```

### Passo 4: Pubblicazione e Apertura della Pull Request
```bash
git push origin feature/nome-del-task
```

Ora, su GitHub, apro la pagina del repository e clicco su "Compare & Pull Request". Seleziono come target il branch ```main```. 

### Passo 5: Validazione in Staging (Automatico)

1. Jenkins intercetta la Pull Request ed avvia la pipeline. 

Effettua il Merge su GitHub dopo il superamento dei controlli.

La pipeline compila il codice unito, genera l'immagine Docker e aggiorna lo Staging sul Raspberry Pi 5.

Verifico che la funzionalità risponda correttamente nell'ambiente di Staging.

### Passo 6: Rilascio in Produzione

Quando ho completato tutti i task di una determinata fase e l'ambiente di Staging è solido:

1. Vado su GitHub nella sezione Releases. 
2. Clicco su "Draft a new release". 
3. Scelgo un tag di versione (es. `1.0.0`), scrivo un breve riepilogo della feature e clicco su "Publish Release". 
4. Jenkins intercetta il tag e sposta automaticamente la build in produzione. 

--- 

## 🔒 4. Configurazione della Branch Protection Rule su GitHub

Per maggiore sicurezza ho applicato una regola di protezione sul branch `main` per le tre repository (be, fe, infra). 

---

## 📝 5. Convenzione sui Messaggi di Commit e Task (Conventional Commits)

Al fine di garantire la massima leggibilità dello storico di Git e standardizzare la nomenclatura dei task sulla bacheca il progetto adotta rigorosamente la specifica internazionale [**Conventional Commits 1.0.0**](https://www.conventionalcommits.org/en/v1.0.0/).

Ogni commit e ogni titolo di task nella bacheca deve seguire la struttura: `<tipo>(<ambito>): <descrizione breve>`

### 📌 Tipologie di costrutti utilizzati (Types)
* **`feat`** (Feature): Introduzione di una nuova funzionalità applicativa o infrastrutturale che porta valore diretto all'utente o al sistema (es. generazione di uno scheletro applicativo, implementazione di un endpoint).
* **`chore`** (Routine): Attività di manutenzione, pulizia del codice o configurazione di strumenti che non modificano la logica di business e non alterano il comportamento del software a runtime (es. setup di un linter, configurazione di plugin Maven/npm).
* **`docs`** (Documentazione): Modifiche esclusive ai file di documentazione in formato Markdown, senza alcun impatto sul codice sorgente.
* **`test`** (Testing): Scrittura, refactoring o integrazione di suite di test unitari o d'integrazione (JUnit, Mockito, ecc..).
* **`devops`** (Infrastruttura/CI): Modifiche relative alle pipeline di Continuous Integration, script di automazione (Jenkinsfile) o configurazioni di provisioning (Terraform).

### 🔍 Ambiti di applicazione (Scopes)
Lo scope inserito tra parentesi indica il perimetro logico del repository oggetto della modifica:
* `(be)`: Operazione limitata al backend (`karateflow-be`).
* `(fe)`: Operazione limitata al frontend (`karateflow-fe`).
* `(infra)`: Operazione legata all'infrastruttura o all'automazione (`karateflow-infra`).

### 🎓 Giustificazione Metodologica
L'adozione di questo standard risponde ai criteri di ingegneria del software raccomandati dalla letteratura DevOps industriale. La separazione netta tra modifiche evolutive (`feat`) e modifiche correttive o di routine (`chore`, `docs`) riduce drasticamente il carico cognitivo durante le fasi di revisione del codice (Pull Request). Inoltre, costringe lo sviluppatore a mantenere l'atomicità dei task: la necessità di inserire più prefissi in un unico messaggio è un indicatore automatico (code smell sintattico) di un task troppo ampio che richiede di essere frammentato in sotto-attività più gestibili.