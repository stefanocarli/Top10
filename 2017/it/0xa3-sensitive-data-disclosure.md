# A3:2017 Sensitive Data Exposure

| Threat agents/Attack vectors | Problematica di sicurezza | Impatti |
| -- | -- | -- |
| Access Lvl : Sfruttabilità 2 | Diffusione 3 : Individuazione 2 | Tecnici 3 : Business |
| Anzichè attaccare direttamente la crittografia, gli attaccanti rubano le chiavi, svolgono attacchi man-in-the-middle o rubano i dati in chiaro dai server mentre sono in transito o dai client degli utenti, es. browser. Generalmente è richiesto un attacco manuale. Sui database di password ottenuti dagli attaccanti si possono effettuare attacchi di brute force con l'ausilio di Graphics Processing Units (GPUs). | Negli ultimi anni, questo è stato l'attacco più impattante. L'errore più comune è non cifrare le informazioni confidenziali. Quando si utilizza la crittografia invece è comune la generazione di chiavi deboli o la gestione non corretta delle stesse, l'utilizzo di algoritmi, cifrari e protocolli poco robusti, in particolare per la memorizzazione di password con hash. Per i dati in transito, le debolezze lato server sono semplici da individuare, più difficili per i dati a riposo. | Spesso le vulnerabilità compromettono i dati che avrebbero dovuto essere protetti. Di solito questi dati comprendono informazioni personali confidenziali (PII) come dati sanitari, credenziali, carte di credito, che spesso richiedono una protezione adeguata, come definito da leggi o norme quali il GDPR in EU o altre leggi locali sulla privacy. |

## Sono vulnerabile?

La prima cosa da fare è determinare quali sono le informazioni confidenziali che dovrebbero essere maggiormente protette, sia in transito che a riposo. Ad esempio password, carte di credito, dati sanitari ecc, in particolare se i dati sono tutelati da leggi sulla privacy come la General Data Protection Regulation (GDPR) in EU, es. protezione dei dati finanziari con la PCI Data Security Standard (PCI DSS). Per questi dati:

* Il dato viene trasmesso in chiaro? Questo riguarda i protocolli come HTTP, SMTP e FTP. Il traffico esterno è particolarmente pericoloso. Verifica tutto il traffico interno, ad esempio tra i load balancer, i web server e i sistemi di back-end.
* Vengono utilizzati di default o su codice legacy algoritmi di crittografia deboli o vecchi?
* Vengono utilizzate o riutilizzate chiavi crittografiche di default o deboli, o manca un apposito meccanismo di gestione e rotazione delle stesse?
* Non viene imposta la cifratura, es. ci sono degli user agent (browser) dove mancano header o direttive di sicurezza?
* Lo user agent (es. app, client di posta) non verifica se il certificato ricevuto dal server è valido?

Vedi ASVS [Crypto (V7)](https://www.owasp.org/index.php/ASVS_V7_Cryptography), [Data Protection (V9)](https://www.owasp.org/index.php/ASVS_V9_Data_Protection) e [SSL/TLS (V10)](https://www.owasp.org/index.php/ASVS_V10_Communications).

## Come prevenire?

Consultare i riferimenti e fare almeno le seguenti: 

* Catalogare i dati elaborati, memorizzati o trasmessi dall'applicazione. Identificare quali sono i dati sensibili in base alle leggi sulla privacy, requisiti normativi o necessità aziendali.
* Applicare i controlli in base alla classificazione svolta.
* Memorizzare solo i dati necessari. Eliminarli il prima possibile o utilizzare un offuscamento conforme al PCI DSS o anche un troncamento. I dati non presenti non possono essere rubati.
* Crittografare i dati sensibili una volta a riposo.
* Utilizzare gli ultimi standard per gli algoritmi, i protocolli e le chiavi; utilizzare un meccanismo per la gestione di queste.
* Cifrare tutti i dati in transito con protocolli sicuri come il TLS utilizzando cifrari con perfect forward secrecy (PFS), una priorità dei cifrari da parte del server e parametri sicuri. Forzare la cifratura con direttive come HTTP Strict Transport Security (HSTS).
* Disabilitare la cache per risposte che contengono dati confidenziali.
* Memorizzare le password con funzioni hash adattive e forti e con "salt" con un work factor (delay factor), come [Argon2](https://www.cryptolux.org/index.php/Argon2), [scrypt](https://wikipedia.org/wiki/Scrypt), [bcrypt](https://wikipedia.org/wiki/Bcrypt) o [PBKDF2](https://wikipedia.org/wiki/PBKDF2).
* Verificare indipendentemente l'efficacia di impostazioni e configurazioni.

## Esempi di Scenari di Attacco

**Scenario #1**: Un'applicazione cifra i numeri delle carte di credito in un database utilizzando la cifratura automatica fornita da esso. Questi dati però vengono automaticamente decifrati una volta letti, permettendo ad una vulnerabilità di SQL injection di recuperare i numeri delle carte di credito in chiaro. 

**Scenario #2**: Un sito non utilizza e non forza l'uso di TLS o supporta una cifratura debole. Un attaccante che monitora il traffico di rete (es. su una rete wireless insicura), declassa la connessione da HTTPS ad HTTP, intercetta le richieste e ruba il cookie di sessione dell'utente. L'attaccante può quindi riutilizzare il cookie e ottenere la sessione già autenticata dell'utente, e leggere o modificare così i dati privati della vittima. Un attaccante potrebbe inoltre alterare i dati in transito e magari modificare il destinatario di un bonifico bancario.

**Scenario #3**: Le password vengono memorizzate nell'analogo database con funzioni hash deboli o senza "salt". Una falla sul meccanismo di upload dei file permette ad un attaccante di ottenere l'intero database delle password. Dagli hash senza "salt" si possono recuperare le password utilizzando le rainbow table o hash precalcolati. Hash generati da funzioni semplici o troppo veloci si possono craccare con GPU, anche in presenza di "salt".

## Riferimenti

* [OWASP Proactive Controls: Protect Data](https://www.owasp.org/index.php/OWASP_Proactive_Controls#7:_Protect_Data)
* [OWASP Application Security Verification Standard]((https://www.owasp.org/index.php/Category:OWASP_Application_Security_Verification_Standard_Project)): [V7](https://www.owasp.org/index.php/ASVS_V7_Cryptography), [9](https://www.owasp.org/index.php/ASVS_V9_Data_Protection), [10](https://www.owasp.org/index.php/ASVS_V10_Communications)
* [OWASP Cheat Sheet: Transport Layer Protection](https://www.owasp.org/index.php/Transport_Layer_Protection_Cheat_Sheet)
* [OWASP Cheat Sheet: User Privacy Protection](https://www.owasp.org/index.php/User_Privacy_Protection_Cheat_Sheet)
* [OWASP Cheat Sheet: Password](https://www.owasp.org/index.php/Password_Storage_Cheat_Sheet) and [Cryptographic Storage](https://www.owasp.org/index.php/Cryptographic_Storage_Cheat_Sheet)
* [OWASP Security Headers Project](https://www.owasp.org/index.php/OWASP_Secure_Headers_Project); [Cheat Sheet: HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet)
* [OWASP Testing Guide: Testing for weak cryptography](https://www.owasp.org/index.php/Testing_for_weak_Cryptography)

### Esterni

* [CWE-220: Exposure of sens. information through data queries](https://cwe.mitre.org/data/definitions/220.html)
* [CWE-310: Cryptographic Issues](https://cwe.mitre.org/data/definitions/310.html); [CWE-311: Missing Encryption](https://cwe.mitre.org/data/definitions/311.html)
* [CWE-312: Cleartext Storage of Sensitive Information](https://cwe.mitre.org/data/definitions/312.html)
* [CWE-319: Cleartext Transmission of Sensitive Information](https://cwe.mitre.org/data/definitions/319.html)
* [CWE-326: Weak Encryption](https://cwe.mitre.org/data/definitions/326.html); [CWE-327: Broken/Risky Crypto](https://cwe.mitre.org/data/definitions/327.html)
* [CWE-359: Exposure of Private Information - Privacy Violation](https://cwe.mitre.org/data/definitions/359.html)
