# 📔 Diario di Bordo Fase 4: Provisioning Infrastruttura

## 📅 Dettagli della Fase
* **Periodo**: Terza e Quarta settimana di Giugno 2026
* **Stato**: 🟡 In Corso
* **Obiettivo**: Provisioning dell'infrastruttura cloud-native (K3s) su Raspberry Pi 5 tramite IaC (Terraform), containerizzazione dei servizi ed estensione delle pipeline di CD su Jenkins.

---

## 🎯 Obiettivi Raggiunti & Governance (Avvio della Fase)

In questa fase iniziale abbiamo impostato la governance e strutturato il backlog delle attività necessarie per il provisioning infrastrutturale. Seguendo la metodologia *Rolling Wave Planning*, abbiamo dettagliato e scomposto i requisiti in Epiche, User Story e Task su GitHub.

### Strutturazione del Backlog su GitHub
Abbiamo formalizzato e caricato sulla bacheca di progetto:
1. **2 Epiche di Progetto** legate a questa fase:
   * **`KF-EPIC-3`**: IaC Setup & Cluster Topology
   * **`KF-EPIC-4`**: Containerization & Continuous Delivery
2. **4 User Story** (2 per ciascuna Epica):
   * **`KF-EPIC-3-STORY-1`**: Cloud-Native Cluster Governance via IaC
   * **`KF-EPIC-3-STORY-2`**: Kubernetes Manifests & Persistent Storage Topology
   * **`KF-EPIC-4-STORY-1`**: Immutable Containerization & Environment Profiles
   * **`KF-EPIC-4-STORY-2`**: Automated Continuous Delivery (CD) via Jenkins
3. **10 Task atomici** per la prima Epica (`KF-EPIC-3`), pronti per essere presi in carico nel repository `karateflow-infra`.

---

## 🗺️ Dettaglio di Epiche e Stories

### 🌐 [KF-EPIC-3] IaC Setup & Cluster Topology
Questa Epica ha l'obiettivo di automatizzare il provisioning dell'infrastruttura. Applicando i principi dell'Infrastructure as Code (IaC) e dell'orchestrazione cloud-native, si definisce un ambiente di esecuzione sicuro, isolato a livello di rete (Kubernetes Namespaces) e limitato a livello hardware (Resource Quotas) per consentire la coesistenza di Staging e Produzione sul Raspberry Pi 5.

*   **[KF-EPIC-3-STORY-1] Cloud-Native Cluster Governance via IaC**
    *   *Obiettivo*: Effettuare il provisioning dei namespace (`karateflow-stg`, `karateflow-prod`) ed impostare quote hardware (CPU/RAM) tramite Terraform.
    *   *Task definiti*:
        *   `KF-EPIC-3-STORY-1-TASK-1`: `chore(host): Bootstrap Raspberry Pi 5 OS and install K3s cluster`
        *   `KF-EPIC-3-STORY-1-TASK-2`: `feat(infra): Setup project and Kubernetes provider authentication`
        *   `KF-EPIC-3-STORY-1-TASK-3`: `feat(infra): provision staging and production namespaces`
        *   `KF-EPIC-3-STORY-1-TASK-4`: `feat(infra): define CPU and RAM resource quotas`
*   **[KF-EPIC-3-STORY-2] Kubernetes Manifests & Persistent Storage Topology**
    *   *Obiettivo*: Definire i manifesti di deployment, configurare la persistenza del database limitando l'uso del disco (SSD da 10GB per K3s) e configurare il proxy di routing e il tunneling HTTPS.
    *   *Task definiti*:
        *   `KF-EPIC-3-STORY-2-TASK-1`: `feat(infra): configure 10GB isolated SSD volume for K3s local-path`
        *   `KF-EPIC-3-STORY-2-TASK-2`: `feat(infra): write MongoDB stateful deployment and 3GB PVC manifest`
        *   `KF-EPIC-3-STORY-2-TASK-3`: `feat(infra): write backend deployment and clusterIP service`
        *   `KF-EPIC-3-STORY-2-TASK-4`: `feat(infra): write frontend deployment and clusterIP service`
        *   `KF-EPIC-3-STORY-2-TASK-5`: `feat(infra): configure Traefik Ingress routing rules for internal resolution`
        *   `KF-EPIC-3-STORY-2-TASK-6`: `feat(infra): deploy Cloudflare Tunnel daemon for secure external access`

---

### 📦 [KF-EPIC-4] Containerization & Continuous Delivery
Questa Epica ha l'obiettivo di colmare il divario tra lo sviluppo locale e automatizzare l'intero ciclo di vita del rilascio software. Attraverso la Containerizzazione, si garantisce che il medesimo artefatto (immagine Docker) testato in locale possa girare senza alterazioni su K3s, governando le differenze ambientali esclusivamente tramite configurazioni di profilo (Staging vs Prod). Inoltre, l'estensione delle pipeline CI/CD introduce la Continuous Delivery, abbattendo il tempo di deploy ed eliminando l'intervento manuale post-merge.

*   **[KF-EPIC-4-STORY-1] Immutable Containerization & Environment Profiles**
    *   *Obiettivo*: Creare Dockerfile multi-stage leggeri per Backend (con JRE minimale) e Frontend (servito da Nginx) eliminando credenziali o IP hardcoded.
*   **[KF-EPIC-4-STORY-2] Automated Continuous Delivery (CD) via Jenkins**
    *   *Obiettivo*: Configurare un Docker Registry locale sul Raspberry Pi 5 e aggiornare le pipeline per compilare le immagini, pusharle sul registry ed eseguire il deploy su K3s (`kubectl apply`).

---

## 🧠 Archivio delle Scelte

*   **Scelta della virtualizzazione leggera (K3s)**: Data la disponibilità limitata di hardware sul Raspberry Pi 5, si è scelto di adottare K3s (una distribuzione Kubernetes leggera) rispetto a Kubernetes standard, riducendo l'overhead di memoria del control plane.
*   **Separazione logica dei Namespace con Resource Quota**: Per impedire che picchi di carico o memory leak nell'ambiente di Staging possano bloccare la produzione o gli altri servizi del Raspberry Pi, abbiamo configurato limiti rigidi di CPU e RAM a livello di Namespace tramite Terraform.
*   **Persistence con local-path provisioner + SSD dedicato**: Per garantire elevate prestazioni di I/O del database MongoDB e preservare i dati in caso di riavvio, viene dedicato un volume SSD da 10GB montato sull'host e cablato tramite un PersistentVolumeClaim (PVC).
*   **Pianificazione incrementale del Backlog (Epic 3 vs Epic 4)**: Ho deciso di scomporre in task di dettaglio (i 10 task pronti su GitHub) solo la prima Epic (`KF-EPIC-3`), lasciando la seconda (`KF-EPIC-4`) al solo livello macro di User Story per applicare i principi del *Rolling Wave Planning*:
    1. *Evitare il rework per instabilità (Hard Dependency)*: Non ha alcun senso perdere ore a definire i micro-task di containerizzazione (Dockerfiles) e automazione del rilascio (Jenkins pipeline CD) prima di avere la certezza che il cluster K3s su Raspberry Pi 5 giri correttamente e che i manifest di storage/network (Terraform/Kubernetes) siano stabili. Qualsiasi imprevisto hardware o sistemistico sul Raspberry durante la prima epica invaliderebbe i dettagli operativi delle build e dei deploy, costringendoci a riscrivere i task.
    2. *Limitazione del sovraccarico cognitivo e Focus*: Dettagliare le cose a tempo debito (just-in-time) mantiene il backlog snello, riduce la decision fatigue sulla board e focalizza lo sforzo sul percorso critico immediato. Le storie di `KF-EPIC-4` verranno espanse in task specifici solo dopo aver validato empiricamente la topologia del cluster.

---
