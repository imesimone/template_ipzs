# Repository Template e-Service IT-Wallet

Questa repository contiene i template OpenAPI (OAS3) per la creazione di e-Service PDND destinati all'erogazione di dati per il Wallet IT. I template sono progettati per essere utilizzati dagli Enti erogatori per standardizzare l'esposizione dei dati necessari alla generazione di credenziali digitali.

## Descrizione dei Template

Il progetto fornisce due modelli di riferimento per servizi sincroni, entrambi redatti integralmente in **lingua inglese** per coerenza con gli standard internazionali di sviluppo API:

1.  **Template Badge Dipendente (`template badge.yaml`)**:
    Destinato agli enti che devono erogare le informazioni necessarie alla generazione di un Badge Dipendente Digitale. Il servizio espone claim relativi all'identità aziendale e professionale in conformità agli standard Schema.org (Organization/Person).

2.  **Template Professioni Sanitarie (`template professioni sanitarie.yaml`)**:
    Modello generico per gli Ordini delle Professioni Sanitarie. Gestisce il recupero dei dati certificati per la tessera professionale digitale, inclusi i dati anagrafici, i dettagli dell'iscrizione all'albo e i loghi istituzionali. La struttura è conforme allo schema dati standard per le professioni sanitarie.

## Interventi di Adeguamento Tecnico

Tutti i template sono stati sottoposti a un processo di revisione e allineamento alla specifica di riferimento `OAS3-PDND-AS.yaml` (IT Wallet API - AS web services). Di seguito si riportano le principali caratteristiche:

### Sicurezza e Autenticazione
- **Autenticazione**: I servizi sono configurati per accettare Voucher di tipo `Bearer`.
- **Integrità del Messaggio**: Gli header `Agid-JWT-Signature`, `Digest` e `Agid-JWT-TrackingEvidence` sono stati allineati formalmente alle definizioni della specifica IT-Wallet, includendo le descrizioni tecniche e i relativi esempi JWT.

### Gestione degli Errori e Rate Limiting
- **Endpoint `/status`**: Verifica dello stato operativo con gestione dei codici di disponibilità (es. `503 Service Unavailable`).
- **Endpoint `/attribute-claims`**: Gestione completa degli errori inclusi i codici HTTP `400`, `401`, `404`, `429`, `500` e `503`.
- **Policy di Rate Limit**:
    - Utilizzo dell'header `Retry-After` per le risposte `429` (Too Many Requests).
    - Inclusione degli header standard `X-RateLimit-Limit`, `X-RateLimit-Remaining` e `X-RateLimit-Reset` per il monitoraggio delle quote di utilizzo.

### Standardizzazione dei Dati
- Il modello per le professioni sanitarie adotta una nomenclatura dei campi coerente con gli standard di interoperabilità (es. `issuing_organization_name`, `registration_number`, `professional_albo`), garantendo la compatibilità tra diversi ordini professionali.

## Struttura della Repository
- `/backup`: Contiene le versioni originali dei file YAML (inclusi i file FNOMCeO specifici) e i file di specifica di riferimento (`OAS3-PDND-AS.yaml`, `schema_dati_professioni_sanitarie_v01.yaml`).
- Root: Contiene i template correnti, aggiornati e validati.
