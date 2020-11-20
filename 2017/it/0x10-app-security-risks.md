# Risk - Application Security Risks

## Quali sono i rischi per la sicurezza delle applicazioni?

Gli attaccanti potenzialmente possono utilizzare diversi percorsi attraverso la vostra applicazione per nuocere alla vostra attività o organizzazione. Ognuno di questi rappresenta un rischio che può, o non può, essere abbastanza grave da attirare l’attenzione.
![App Security Risks](images/0x10-risk-1.png)

Alcune volte, questi percorsi sono facili da trovare e sfruttare, altre volte sono estremamente difficili. Allo stesso modo il danno causato può non avere conseguenze o farvi fallire. Al fine di determinare il rischio per la vostra organizzazione, potete valutare le probabilità associate ad ogni minaccia, vettore di attacco e debolezza di sicurezza e combinarlo con una stima degli impatti tecnici e finanziari per la vostra azienda.

## Quali sono i miei rischi?

La [OWASP Top 10](https://www.owasp.org/index.php/Top10) si focalizza nell'identificare i rischi più gravi per una vasta gamma di organizzazioni. Per ognuno di questi rischi, noi forniamo informazioni generiche su probabilità e impatto tecnico usando i seguenti schemi di valutazione, che sono alla base della OWASP Risk Rating Methodology.

| Agenti di minaccia | Sfruttabilità | Diffusione | Individuazione | Impatti Tecnici | Impatti sul Business |
| -- | -- | -- | -- | -- | -- |
| Specifico   | Facile 3    | Diffuso 3    | Facile 3    | Grave 3    | Specifico |
| per         | Medio 2     | Comune 2     | Medio 2     | Moderato 2 | per il    |
| App         | Difficile 1 | Non Comune 1 | Difficile 1 | Minore 1   | Business  |

In questa edizione, abbiamo aggiornato il sistema di valutazione del rischio, in modo da assisterti nel calcolo della probabilità e dell'impatto di ciascun rischio. Per più dettagli, vedi [Appunti sui rischi](0xc0-note-about-risks.md). 

Ciascuna organizzazione è un caso a parte, così come le minacce per quella organizzazione, i loro obiettivi e l'impatto di una intrusione. Se un'organizzazione di interesse pubblico utilizza un sistema di gestione dei contenuti (CMS) per informazioni pubbliche e un sistema sanitario utilizza lo stesso CMS per dati sanitari sensibili, la minaccia e l'impatto sul business possono essere diversi per lo stesso software. È di importanza critica comprendere il rischio della tua organizzazione in base alle minacce e all'impatto sul business.

Dove possibile, i nomi dei rischi nella Top 10 sono allineati con le debolezze della [Common Weakness Enumeration](https://cwe.mitre.org/) (CWE) per promuovere delle convenzioni sui nomi generalmente riconosciute e ridurne così la confusione.

## Riferimenti

### OWASP

* [OWASP Risk Rating Methodology](https://www.owasp.org/index.php/OWASP_Risk_Rating_Methodology)
* [Article on Threat/Risk Modeling](https://www.owasp.org/index.php/Threat_Risk_Modeling)

### Esterni

* [ISO 31000: Risk Management Std](https://www.iso.org/iso-31000-risk-management.html)
* [ISO 27001: ISMS](https://www.iso.org/isoiec-27001-information-security.html)
* [NIST Cyber Framework (US)](https://www.nist.gov/cyberframework)
* [ASD Strategic Mitigations (AU)](https://www.asd.gov.au/infosec/mitigationstrategies.htm)
* [NIST CVSS 3.0](https://nvd.nist.gov/vuln-metrics/cvss/v3-calculator)
* [Microsoft Threat Modelling Tool](https://www.microsoft.com/en-us/download/details.aspx?id=49168)
