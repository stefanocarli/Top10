# A4:2017 XML External Entities (XXE)

| Threat agents/Attack vectors | Problematiche di sicurezza           | Impatti              |
| -- | -- | -- |
| Access Lvl : Sfruttabilità 2 | Diffusione 2 : Individuazione 3 | Tecnici 3 : Business |
| Gli attaccanti possono sfruttare processori XML vulnerabili se possono caricare documenti XML con contenuto malevolo, sfruttando codice vulnerabile, integrazioni o dipendenze. | Di default, molti processori XML vecchi permettono di speficare entità esterne, un URI che viene dereferenziato e valutato durante l'eleborazione del XML. Strumenti di [SAST](https://www.owasp.org/index.php/Source_Code_Analysis_Tools) possono rilevare queste problematiche ispezinando le dipendenze e la configurazione. Strumenti di [DAST](https://www.owasp.org/index.php/Category:Vulnerability_Scanning_Tools) richiedono un ulteriore sforzo manuale per rilevare e sfruttare questa problematica. I tester devono essere formati per il test di XXE in quanto nel 2017 non viene fatto a dovere. | Queste problematiche possono essere utilizzate per ottenere dati, eseguire delle chiamate dal server, scansionare sistemi interni, svolgere attacchi di denial-of-service, così come tanti altri. |

## Sono vulnerabile?

Le applicazioni e in particolare i servizi web XML-based o integrazioni a valle potrebbero essere vulnerabili se:

* L'applicazione accetta XML o il caricamento di documenti XML, specialmente da fonti non fidate, o inserisce dati non validati all'interno di documenti XML, che vengono elaborati da un parser XML.
* I parser XML dell'applicazione o i servizi web SOAP permettono la definizione di [document type definitions (DTDs)](https://en.wikipedia.org/wiki/Document_type_definition). Dato che la disattivazione dell'elaborazione dei DTD varia in base al parser, è bene consultare delle guide come la [OWASP Cheat Sheet 'XXE Prevention'](https://www.owasp.org/index.php/XML_External_Entity_(XXE)_Prevention_Cheat_Sheet). 
* L'applicazione usa SAML per l'elaborazione delle identità ai fini della sicurezza federata o del single sign on (SSO). SAML utilizza il formato XML per le Assertion e potrebbe essere vulnerabile.
* L'applicazione utilizza SOAP con una versione inferiore alla 1.2, è probabilmente suscettibile ad attacchi XXE se le entità XML vengono passate al framework SOAP.
* Essere vulnerabile ad attacchi XXE molto probabilmente significa che l'applicazione è vulnerabile ad attacchi di Denial of Service, incluso il Billon Laughs.

## Come prevenire?

La formazione degli sviluppatori è essenziale per identificare e mitigare XXE. E per prevenirle occorre inoltre:

* Dove possibile, utilizzare formati di dati più semplici, come il JSON, ed evitare la serializzazione di dati confidenziali.
* Patchare o aggiornare tutti i parser e le librerie XML utilizzate dall'applicazione o sul sistema operativo. Utilizzare sistemi di controllo delle dipendenze. Aggiornare a SOAP 1.2 o superiore.
* Disabilitare le entità esterne XML e l'elaborazione dei DTD in tutti i parser XML nell'applicazione, come illustrato nella [OWASP Cheat Sheet 'XXE Prevention'](https://www.owasp.org/index.php/XML_External_Entity_(XXE)_Prevention_Cheat_Sheet). 
* Implementare la validazione degli input lato server stile "allowlist", filtraggio o sanificazione per evitare dati malevoli all'interno di documenti, headers o nodi XML.
* Verificare che eventuali funzionalità di upload di file XML o XSL validino il XML in ingresso con XSD o simile.
* Strumenti di SAST possono aiutare a rilevare XXE nel codice sorgente, anche se la revisione del codice manuale è la migliore alternativa per applicazioni grandi e complesse con molte integrazioni.

Se tutti questi controlli non sono applicabili, si può utilizzare il patching virtuale, API security gateways, o Web Application Firewalls (WAFs) per rilevare, monitorare e bloccare attacchi XXE.

## Esempi di Scenari di Attacco

Sono numerosi gli esempi di problematiche XXE, inclusi gli attacchi a dispositivi embedded. XXE si presenta in moltissimi posti inaspettati, incluso in dipendenze profondamente annidate. Il modo più semplice è caricare un file XML malevolo, se viene accettato:

**Scenario #1**: L'attaccante prova ad ottenere dei dati presenti nel server:

```
  <?xml version="1.0" encoding="ISO-8859-1"?>
    <!DOCTYPE foo [
    <!ELEMENT foo ANY >
    <!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
    <foo>&xxe;</foo>
```

**Scenario #2**: L'attaccante sonda la rete privata del server cambiando la linea ENTITY precedente con:
```
   <!ENTITY xxe SYSTEM "https://192.168.1.1/private" >]>
```

**Scenario #3**: L'attaccante prova un attacco di denial-of-service includento un file potenzialmente infinito:

```
   <!ENTITY xxe SYSTEM "file:///dev/random" >]>
```

## Riferimenti

### OWASP

* [OWASP Application Security Verification Standard](https://www.owasp.org/index.php/Category:OWASP_Application_Security_Verification_Standard_Project#tab=Home)
* [OWASP Testing Guide: Testing for XML Injection](https://www.owasp.org/index.php/Testing_for_XML_Injection_(OTG-INPVAL-008))
* [OWASP XXE Vulnerability](https://www.owasp.org/index.php/XML_External_Entity_(XXE)_Processing)
* [OWASP Cheat Sheet: XXE Prevention](https://www.owasp.org/index.php/XML_External_Entity_(XXE)_Prevention_Cheat_Sheet)
* [OWASP Cheat Sheet: XML Security](https://www.owasp.org/index.php/XML_Security_Cheat_Sheet)

### Esterni

* [CWE-611: Improper Restriction of XXE](https://cwe.mitre.org/data/definitions/611.html)
* [Billion Laughs Attack](https://en.wikipedia.org/wiki/Billion_laughs_attack)
* [SAML Security XML External Entity Attack](https://secretsofappsecurity.blogspot.tw/2017/01/saml-security-xml-external-entity-attack.html)
* [Detecting and exploiting XXE in SAML Interfaces](https://web-in-security.blogspot.tw/2014/11/detecting-and-exploiting-xxe-in-saml.html)
