L’idea è che IPZS (in realtà noi) crei dei template e-service per gli enti che devono erogare dei dati in modo che si crei poi la credenziale sul wallet.
I primi due servizi che devono diventare template sono per l’esposizione di un badge (template badge) e per i tesserini delle professioni sanitarie (template fnomceo). Per questi ultimi abbiamo pensato ad uno schema dati (schema_dati_professioni_sanitarie_v01) da cui prendere spunto.

Prima di passarli a IPZS, i template andrebbero sistemati. Vanno allineati a come già fatto nella specifica OAS3-PDND-AS.yaml perché ad esempio alcuni header non ci sono o sono usati male. Qui il branch master https://github.com/italia/eid-wallet-it-docs/blob/versione-corrente/docs/en/oas3/OAS3-PDND-AS.yaml, nel caso ci fosse qualche cambiamento impattante magari parliamone.
 
Ogni miglioramento è ben accetto, manca ad esempio il campo contact.email.
I servizi sono sincroni.

Nelle risposte di errore con HTTP Code 429 è previsto solo l’header Retry-After pertanto l’X-RateLimit-Limit dovremmo rimuoverlo, in accordo al commento presente nell’OAS3: “# RFC 6585 defines Retry-After. X-RateLimit-Limit, X-RateLimit-Remaining and X-RateLimit-Reset are not required because redundant along with Retry-After.”

Tra gli errori previsti per l’endpoint /status mancherebbe la gestione del codice HTTP 503.

Tra gli errori previsti per l’endpoint /attribute-claims mancherebbero i codici HTTP 400, 401, 404, 500 e 503.

Nel template non è previsto l’header DPOP pertanto tra i securityScheme dovremmo rimuovere il DPoPAuth. L’e-service è pensato per accettare solo un voucher di tipo Bearer.