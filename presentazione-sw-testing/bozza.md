# Presentazione Progetto KarateFlow (Per Software Testing)

Struttura finale delle slide implementata in `presentazione.tex` per l'avvio del progetto:

1. **Titolo & Introduzione**: Dettagli del corso, studente (Simone Ingenito), data (27 Luglio 2026).
2. **Cos'è KarateFlow?**: Presentazione della piattaforma, value proposition per i Coach, digitalizzazione dei test fisici.
3. **Contesto & Obiettivi del Corso di Software Testing**: Inquadramento del progetto e i tre obiettivi principali (testing avanzato, coverage/debito, GitHub Actions vs Jenkins).
4. **Architettura Tecnologica**: Rappresentazione grafica (diagramma TikZ) dell'infrastruttura (Raspberry Pi 5 host, K3s cluster, Angular, Spring Boot, MongoDB, Traefik, Cloudflare Tunnel).
5. **Stato di Partenza: Strategia di Testing**: Tabella di sintesi dello stack iniziale (JUnit 5, Mockito, MockMvc, PMD, JaCoCo, Vitest, jsdom, ESLint).
6. **Stato di Partenza: Qualità Backend (JaCoCo)**: Metriche iniziali di coverage (85% linee / 55% rami) con screenshot `sonar-be-status.png` integrato.
7. **Stato di Partenza: Qualità Frontend (Vitest)**: Metriche iniziali di coverage (82% linee / 75% rami) con screenshot `sonar-fe-status.png` integrato.
8. **Obiettivo 1: Integrazione di Testing Avanzato (Pianificazione)**: Attività pianificate (Testcontainers MongoDB, Singleton pattern, UI Testing Angular) con blocco TODO per gli esiti a fine progetto.
9. **Obiettivo 2: Aumento Coverage e Riduzione Debito (Pianificazione)**: Attività pianificate (test parametrici, risoluzione anomalie PMD/SonarQube, Quality Gate) con blocco TODO per esiti di fine progetto.
10. **Obiettivo 3: CI/CD e Integrazione GitHub Actions (Pianificazione)**: Stato iniziale con Jenkins e Poll SCM per NAT locale, pianificazione dei workflow YAML in cloud con blocco TODO per setup del confronto.
11. **Confronto Tecnologico: Jenkins vs GitHub Actions (TODO)**: Tabella comparativa post-implementazione con parametri da compilare al termine del progetto.
12. **Integrità del Codice e Quality Gate Finale (TODO)**: Placeholders grafici per screenshot di PR bloccata su GitHub e pipeline finale completata.
13. **Sviluppi Futuri del Testing**: Playwright/Cypress E2E, Pact Contract Testing, Dependency scanning (CVE).
14. **Conclusioni**: Sintesi dello stato di partenza e degli obiettivi del corso tracciati.