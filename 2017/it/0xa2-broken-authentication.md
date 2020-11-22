# A2:2017 Broken Authentication

| Threat agents/Attack vectors | Problematica di sicurezza       | Impatti               |
| -- | -- | -- |
| Access Lvl : Sfruttabilità 3 | Diffusione 2 : Individuazione 2 | Tecnici 3 : Business |
| Gli attaccanti hanno a disposizione centinaia di milioni di combinazioni di username e password e account amministrativi di default per svolgere attacchi di credentials stuffing, strumenti per attacchi brute force e a dizionario. Gli attacchi sulla gestione delle sessioni sono ben noti, in particolare sulla perduranza delle stesse.  | La prevalenza di broken authentication è diffusa a causa della progettazione e implementazione della maggior parte dei meccanismi di controllo degli accessi e identità. Il meccanismo di gestione delle sessioni è fondamentale per i controlli di questo tipo, ed è presente in tutte le applicazioni stateful. Gli attaccanti possono identificare vulnerabilità di questo tipo con approcci manuali e sfruttarle con l'ausilio di strumenti automatizzati con liste di password e attacchi a dizionario. | Gli attaccanti per compromettere il sistema devono semplicemente riuscire ad accedere a qualche account, oppure a quello amministrativo. In base al dominio dell'applicazione, questo può portare al riciclaggio di denaro, frodi e furto di identità, o ottenere informazioni altamente riservate. |

## Sono vulnerabile?

La conferma dell'identità degli utenti, l'autenticazione e la gestione della sessione sono fondamentali per la protezione contro attacchi legati all'autenticazione.

Ci potrebbero essere debolezze sui meccanismi di autenticazione se l'applicazione:

* Permette attacchi automatizzati come [credential stuffing](https://www.owasp.org/index.php/Credential_stuffing), dove l'attaccante possiede una lista di username e password valide.
* Permette attacchi brute force o altri attacchi automatizzati.
* Permette password deboli, di default o ben note, come "Password1" o "admin/admin“.
* Utilizza funzionalità inefficaci di recupero delle credenziali o password, come risposte "knowledge-based", che non possono essere rese sicure.
* Utilizza password in chiaro, cifrate o con un hash debole (vedi **A3:2017-Sensitive Data Exposure**).
* Non ha un meccanismo di autenticazione a due fattori o è inefficace.
* Espone gli ID della sessione nell'URL (es., URL rewriting).
* Non ruota gli ID delle sessioni dopo un login avvenuto con successo.
* Non invalida correttamente gli ID delle sessioni. Le sessioni o i token di autenticazione (in particolare i token di single sign-on (SSO)) non vengono invalidati al logout o dopo un periodo di inattività.

## Come prevenire?

* Dove possibile, implementare l'autenticazione multi-fattore per evitare attacchi automatizzati di credential stuffing, brute force, e riutilizzo di credenziali rubate.
* Non pubblicare applicazioni con credenziali di default, in particolare per gli utenti amministrativi.
* Implementare controlli di password deboli, come i test con la lista delle [top 10000 peggiori passwords](https://github.com/danielmiessler/SecLists/tree/master/Passwords) per quelle nuove o cambiate recentemente.
* Rendere conformi la lunghezza delle password, la complessità e le politiche di rotazione con le [linee guida NIST 800-63 nella sezione 5.1.1 per "Memorized Secrets"](https://pages.nist.gov/800-63-3/sp800-63b.html#memsecret) o altre politiche moderne equivalenti.
* Assicurarsi che la registrazione, il recupero delle credenziali e le API siano robuste contro gli attacchi di enumerazione degli account utilizzando un messaggio generico per tutte le casistiche.
* Limitare o limitare temporalmente i tentativi di login falliti. Scrivere sul log tutti gli errori e avvisare gli amministratori quando vengono rilevati attacchi di credential stuffing, brute force ecc.
* Utilizzare un session manager lato server che sia sicuro, e che dopo il login generi un nuovo ID di sessione con alta entropia. Gli ID di sessione non dovrebbero essere nell'URL ma essere memorizzati in modo sicuro e invalidati dopo il logout, inattività o timeout.

## Esempi di Scenari di Attacco

**Scenario #1**: [Credential stuffing](https://www.owasp.org/index.php/Credential_stuffing), l'uso di [liste di password conosciute](https://github.com/danielmiessler/SecLists), è un attacco comune. Se un'applicazione non implementa protezioni automatiche contro le minacce o contro il credential stuffing, l'applicazione potrebbe essere utilizzata per stabilire se le credenziali inserite sono valide.

**Scenario #2**: La maggior parte degli attacchi sui meccanismi di autenticazione avvengono perchè vengono utilizzate solo le password come fattore di autenticazione. Le best practice, i requisiti di complessità e rotazione delle password sono viste dagli utenti come un incoraggiamento all'utilizzo di password deboli. Per le organizzazioni è consigliato non seguire le direttive NIST 800-63 e utilizzare l'autenticazione multi-fattore.

**Scenario #3**: I timeout delle sessioni delle applicazioni sono vengono impostati correttamente. Un utente utilizza un computer pubblico per accedere a un'applicazoine. Anzichè selezionare "logout" l'utente si limita a chiudere la finestra del browser e va via. Un attaccante un'ora dopo utilizza lo stesso browser, e l'utente è ancora autenticato.

## Riferimenti

### OWASP

* [OWASP Proactive Controls: Implement Identity and Authentication Controls](https://www.owasp.org/index.php/OWASP_Proactive_Controls#5:_Implement_Identity_and_Authentication_Controls)
* [OWASP Application Security Verification Standard: V2 Authentication](https://www.owasp.org/index.php/Category:OWASP_Application_Security_Verification_Standard_Project#tab=Home)
* [OWASP Application Security Verification Standard: V3 Session Management](https://www.owasp.org/index.php/Category:OWASP_Application_Security_Verification_Standard_Project#tab=Home)
* [OWASP Testing Guide: Identity](https://www.owasp.org/index.php/Testing_Identity_Management)
 and [Authentication](https://www.owasp.org/index.php/Testing_for_authentication)
* [OWASP Cheat Sheet: Authentication](https://www.owasp.org/index.php/Authentication_Cheat_Sheet)
* [OWASP Cheat Sheet: Credential Stuffing](https://www.owasp.org/index.php/Credential_Stuffing_Prevention_Cheat_Sheet)
* [OWASP Cheat Sheet: Forgot Password](https://www.owasp.org/index.php/Forgot_Password_Cheat_Sheet)
* [OWASP Cheat Sheet: Session Management](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet)
* [OWASP Automated Threats Handbook](https://www.owasp.org/index.php/OWASP_Automated_Threats_to_Web_Applications)

### External

* [NIST 800-63b: 5.1.1 Memorized Secrets](https://pages.nist.gov/800-63-3/sp800-63b.html#memsecret) - per consigli sull'autenticazione accurati, moderni e basati su prove. 
* [CWE-287: Improper Authentication](https://cwe.mitre.org/data/definitions/287.html)
* [CWE-384: Session Fixation](https://cwe.mitre.org/data/definitions/384.html)
