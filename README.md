# Repository Template e-Service IT-Wallet

Questa repository contiene i template OpenApi (OAS3) per la creazione di e-Service PDND destinati all'erogazione di dati per il Wallet IT. I template sono progettati per essere utilizzati dagli Enti erogatori per standardizzare l'esposizione dei dati necessari alla generazione di credenziali digitali.

## Descrizione dei Template

Il progetto fornisce due modelli di riferimento per servizi sincroni:

1.  **Template Badge Dipendente (`template badge.yaml`)**:
    Destinato agli enti che devono erogare le informazioni necessarie alla generazione di un Badge Dipendente Digitale. Il servizio espone claim relativi all'identità aziendale e professionale in conformità agli standard Schema.org (Organization/Person).

2.  **Template Professioni Sanitarie (`template fnomceo.yaml`)**:
    Modello specifico per la Federazione Nazionale dei Medici Chirurghi e degli Odontoiatri (e potenzialmente estendibile ad altri ordini professionali). Gestisce il recupero dei dati certificati per la tessera professionale digitale, inclusi i dati anagrafici, i dettagli dell'iscrizione all'albo e le immagini (logo e foto) in formato Base64.

## Interventi di Adeguamento Tecnico

Tutti i template sono stati sottoposti a un processo di revisione e allineamento alla specifica di riferimento `OAS3-PDND-AS.yaml` (IT Wallet API - AS web services). Di seguito si riportano le modifiche effettuate:

### Sicurezza e Autenticazione
- **Rimozione DPoP**: È stato rimosso il supporto allo schema `DPoPAuth` in quanto i servizi sono configurati per accettare esclusivamente Voucher di tipo `Bearer`.
- **Integrità del Messaggio**: Gli header `Agid-JWT-Signature`, `Digest` e `Agid-JWT-TrackingEvidence` sono stati allineati formalmente alle definizioni della specifica IT-Wallet, includendo le descrizioni tecniche originali in lingua inglese e i relativi esempi JWT.

### Gestione degli Errori e Rate Limiting
- **Endpoint `/status`**: È stata inserita la gestione del codice HTTP `503` (Service Unavailable).
- **Endpoint `/attribute-claims`**: La gestione degli errori è stata estesa includendo i codici HTTP `400`, `401`, `404`, `500` e `503`.
- **Policy di Rate Limit**:
    - Nelle risposte di errore `429` (Too Many Requests), viene utilizzato esclusivamente l'header `Retry-After`, rimuovendo gli header `X-RateLimit-*` come previsto dalla RFC 6585.
    - Per le risposte di successo (`200`) e gli errori client (`400`, `401`, `404`), sono stati inclusi gli header standard `X-RateLimit-Limit`, `X-RateLimit-Remaining` e `X-RateLimit-Reset` per garantire la visibilità delle quote di utilizzo.

### Standardizzazione dei Dati (FNOMCeO)
- Il modello per le professioni sanitarie è stato raffinato utilizzando come riferimento lo `schema_dati_professioni_sanitarie_v01.yaml`, adottando una nomenclatura dei campi più coerente con gli standard di interoperabilità (es. `issuing_organization_name`, `registration_number`, `professional_albo`).

## Struttura della Repository
- `/backup`: Contiene le versioni originali dei file YAML prima degli interventi di adeguamento.
- `OAS3-PDND-AS.yaml`: Specifica di riferimento IT-Wallet.
- `schema_dati_professioni_sanitarie_v01.yaml`: Schema dati di riferimento per le professioni sanitarie.
