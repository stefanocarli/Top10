# A1:2017 Injection

| Threat agents/Attack vectors | Problematica di sicurezza       | Impatti              |
| -- | -- | -- |
| Access Lvl : Sfruttabilità 3 | Diffusione 2 : Individuazione 3 | Tecnici 3 : Business |
| Quasi tutte le sorgenti di dati possono essere un vettore di injection: variabili d'ambiente, parametri, servizi web interni ed esterni e tutte le tipologie di utenti. [Una injection](https://www.owasp.org/index.php/Injection_Flaws) si verifica quando un attaccante invia dei dati non fidati ad un interprete. | Queste problematiche sono molto diffuse, in particolare in codice legacy. Le vulnerabilità di injection si trovano spesso in SQL, LDAP, XPath, o query NoSQL, nei comandi OS, nei parser XML, negli header SMTP, negli expression language, e nelle query ORM. Le injection sono semplici da identificare quando si esamina il codice sorgente. Gli scanner e i fuzzer possono essere d'aiuto per trovarle. |Le injection possono comportare perdita o corruzione di dati, disclosure verso parti non autorizzate, perdita della tracciabilità o negazione d'accesso. In alcuni casi possono portare anche al controllo completo dell'host. L'impatto sul business dipende dal valore dell'applicazione e dei dati.|


## Sono vulnerabile?

Un'applicazione è vulnerabile se:

* I dati forniti dagli utenti non vengono validati, filtrati, o ripuliti dall'applicazione.
* L'interprete utilizza direttamente query dinamiche o chiamate non parametrizzate senza un "escaping" contestuale.  
* Dati non fidati vengono utilizzati all'interno dei parametri di ricerca di un object-relational mapping (ORM) per ottenere ulteriori dati confidenziali.
* Dati non fidati vengono utilizzati direttamente, o concatenati, in modo tale che la query SQL o il comando contenga sia la struttura che i dati non fidati in query dinamiche, comandi o stored procedures.
* Alcune delle forme di injection più comuni sono SQL, NoSQL, comandi di OS, Object Relational Mapping (ORM), LDAP, Expression Language (EL) o Object Graph Navigation Library (OGNL). Il concetto è lo stesso tra tutti gli interpreti. Il miglior modo per identificare se le applicazioni sono vulnerabili ad una forma di injection è la revisione del codice, seguita da uno scrupoloso testing automatizzato per tutti gli input quali parametri, header, URL, cookies, JSON, SOAP e XML. Le organizzazioni possono includere nelle loro pipeline di CI/CD strumenti di analisi statica ([SAST](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)) o dinamica ([DAST](https://www.owasp.org/index.php/Category:Vulnerability_Scanning_Tools)) del codice, per rilevare prontamente eventuali falle di injection prima della messa in produzione.

## Come prevenire?

Per prevenire le falle di injection è necessario separare i dati non fidati dai comandi e dalle query.

* La soluzione migliore è utilizzare delle API sicure, che evitano l'utilizzo di un interprete o forniscono un'interfaccia parametrizzata, o utilizzare direttamente un Object Relational Mapping (ORM). **Nota bene**: Le stored procedures, anche se parametrizzate, possono comunque introdurre SQL injection se le istruzioni PL/SQL o T-SQL concatenano query e dati, o eseguono dati non fidati con EXECUTE IMMEDIATE o exec(). 
* Usare una validazione server side di tipo "allowlist". Questa non è comunque una difesa completa in quando molte applicazioni richiedono caratteri speciali, come le aree di testo o le API per le applicazioni mobili.
* Per eventuali query dinamiche rimaste, svolgere un "escaping" dei caratteri speciali utilizzando una sintassi specifica per l'interprete in questione. **Nota bene**: Non si può svolgere un "escaping" per i nomi delle tabelle e delle colonne in SQL, quindi è pericoloso ricevere dati non fidati per questi. Questo è un problema comune per i software che producono reportistica.
* Usare LIMIT o altri controlli SQL all'interno delle query per evitare l'estrazione massiva di dati in caso di SQL injection.

## Esempi di Scenari di Attacco

**Scenario #1**: Un'applicazione usa dati non fidati per la costruzione della seguente chiamata SQL vulnerabile: 

`String query = "SELECT * FROM accounts WHERE custID='" + request.getParameter("id") + "'";`

**Scenario #2**: Analogamente, un'applicazione si fida ciecamente dei propri framework con il risultato che si possono comunque creare query vulnerabili (es. Hibernate Query Language (HQL)):

`Query HQLQuery = session.createQuery("FROM accounts WHERE custID='" + request.getParameter("id") + "'");`

In entrambi i casi, l'attaccante modifica il parametro ‘id’ nel proprio browser per inviare il valore:  ' or '1'='1. Ad esempio:

`http://example.com/app/accountView?id=' or '1'='1`

Questo cambia il significato di entrambe le query per ottenere tutti i record della tabella "account". Attacchi più pericolosi possono portare alla modifica dei dati o all'invocazione di stored procedure.

## Riferimenti

### OWASP

* [OWASP Proactive Controls: Parameterize Queries](https://www.owasp.org/index.php/OWASP_Proactive_Controls#2:_Parameterize_Queries)
* [OWASP ASVS: V5 Input Validation and Encoding](https://www.owasp.org/index.php/ASVS_V5_Input_validation_and_output_encoding)
* [OWASP Testing Guide: SQL Injection](https://www.owasp.org/index.php/Testing_for_SQL_Injection_(OTG-INPVAL-005)), [Command Injection](https://www.owasp.org/index.php/Testing_for_Command_Injection_(OTG-INPVAL-013)), [ORM injection](https://www.owasp.org/index.php/Testing_for_ORM_Injection_(OTG-INPVAL-007))
* [OWASP Cheat Sheet: Injection Prevention](https://www.owasp.org/index.php/Injection_Prevention_Cheat_Sheet)
* [OWASP Cheat Sheet: SQL Injection Prevention](https://www.owasp.org/index.php/SQL_Injection_Prevention_Cheat_Sheet)
* [OWASP Cheat Sheet: Injection Prevention in Java](https://www.owasp.org/index.php/Injection_Prevention_Cheat_Sheet_in_Java)
* [OWASP Cheat Sheet: Query Parameterization](https://www.owasp.org/index.php/Query_Parameterization_Cheat_Sheet)
* [OWASP Automated Threats to Web Applications – OAT-014](https://www.owasp.org/index.php/OWASP_Automated_Threats_to_Web_Applications)

### External

* [CWE-77: Command Injection](https://cwe.mitre.org/data/definitions/77.html)
* [CWE-89: SQL Injection](https://cwe.mitre.org/data/definitions/89.html)
* [CWE-564: Hibernate Injection](https://cwe.mitre.org/data/definitions/564.html)
* [CWE-917: Expression Language Injection](https://cwe.mitre.org/data/definitions/917.html)
* [PortSwigger: Server-side template injection](https://portswigger.net/kb/issues/00101080_serversidetemplateinjection)
