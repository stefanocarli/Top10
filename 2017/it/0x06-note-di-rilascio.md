# NV Note sulla Versione

## Cosa è cambiato dalla Top 10 2013 alla 2017?

Negli ultimi quattro anni ci sono stati innumerevoli cambiamenti, e la Top 10 OWASP aveva bisogno di cambiare. Abbiamo completamente riorganizzato la Top 10 OWASP, aggioranto la metodologia, utilizzato un nuovo processo per ottenere i dati, lavorato con la community, riorganizzato i nostri rischi, riscritto ciascun rischio da zero, e aggiunto riferimenti ai linguaggi e ai framework più utilizzati.

Durante gli ultimi anni, l'architettura e le tecnologie delle applicazioni sono cambiate in modo significativo:

* Microservizi scritti in node.js e Spring Boot stanno sostituendo le tradizionali applicazioni monolitiche. I microservizi hanno le loro sfide in termini di sicurezza, ad esempio stabilire fiducia tra di loro, i container, la gestione dei segreti e così via. Codice vecchio non pensato per essere accessibile da Internet è ora pubblico, dietro una API o un servizio web RESTful, pronto per essere consumato da Single Page Applications (SPAs) e applicazioni mobili. Ipotesi architetturali sul codice, ad esempio chiamate da client fidati, non sono più valide. 
* Single Page Applications, scritte con framework JavaScript come Angular e React, permettono la creazione di front end modulari e ricchi di funzionalità. Funzionalità lato client che in passato venivano fornite lato server, presentano le loro sfide alla sicurezza.
* JavaScript è il linguaggio principe del web con node.js lato server e framework web moderni come Bootstrap, Electron, Angular e React in esecuzione sul client.

## Nuove problematiche, basate sui dati

* **A4:2017-XML External Entities (XXE)** è una nuova categoria supportata principalmente da dati provenienti da strumenti che svolgono test di sicurezza sui codici sorgenti ([SAST](https://www.owasp.org/index.php/Source_Code_Analysis_Tools)).

## Nuove problematiche, supportate dalla community

Abbiamo chiesto alla community di fornirci intuizioni a lungo termine su due nuove categorie di debolezze. Dopo più di 500 peer submissions, e eliminando problematiche già supportate dai dati (come Sensitive Data Exposure e XXE), le due nuove problematiche sono:

* **A8:2017-Insecure Deserialization**, che permette l'esecuzione di codice da remoto o la manipolazione di oggetti sensibili sulle piattaforme vulnerabili.
* **A10:2017-Insufficient Logging and Monitoring**, la quale mancanza può prevenire o ritardare significativamente la rilevazione di attività malevole o di intrusione, incident response e indagini forensi.

## Accorpate o ritirate, ma mai dimenticate

* **A4-Insecure Direct Object References** e **A7-Missing Function Level Access Control** accorpate in **A5:2017-Broken Access Control**.
* **A8-Cross-Site Request Forgery (CSRF)**, dato che molti framework implementano [difese contro CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), individuate nel 5% delle applicazioni.
* **A10-Unvalidated Redirects and Forwards**, nonostante fossero presenti in circa l'8% delle applicazioni, sono state surclassate da XXE.

![0x06-release-notes-1](images/0x06-release-notes-1.png)
