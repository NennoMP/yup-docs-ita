# Specifiche tecniche

## Panoramica
L'ecosistema dell'app Yup è costituito dall'implementazione dell'omonimo protocollo e dai servizi web di supporto. Questi semplificano per gli utenti processi quali la cura di contenuti, la ricezione di ricompense e il guadagno di influenza sociale. Gli utenti interagiscono con il protocollo principalmente tramite l'estensione e l'applicazione web di Yup, entrambe possono operare grazie ai [contratti intelligenti di EOS](https://developers.eos.io/eosio-cpp/docs/introduction-to-smart-contracts). Di seguito un'introduzione dell'attuale stack tecnologico utilizzato per il protocollo e le applicazioni di supporto.

## Estensione

L'[estensione di Yup](https://chrome.google.com/webstore/detail/yup-opinions-social-capit/nhmeoaahigiljjdkoagafdccikgojjoi) include le seguenti funzionalità principali:

* Portafoglio EOS completo di tutte le funzionalità, per memorizzare e trasferisce token YUP e EOS, e firmare transazioni per web DApps
* Registrazione rapida e semplice \(verifica email → username → password\)
* Overlay per siti specifici \(Twitter, Reddit, Google, Google Maps, Youtube, siti di registrazione ai corsi della Columbia University\)
* Valutazione ponderata dell'influenza sociale su qualsiasi sito su Internet
* Colori che identificano il livello sociale in base a come altri utenti Yup hanno valutato un determinato contenuto
* Possibilità di inviare voti in varie categorie \(e.g., divertente, intelligente\)
* Possibilità di convetire gli YUP guadagnati in crediti Amazon \(con un rapport di 20 centesimi per token\)
* Sistema di referenza per incentivare gli utenti a invitare amici e parenti
* Backup dell'account EOS basato su cloud (opzionale), in modo che gli utenti possano effettuare il login su molteplici browser e dispositivi senza la necessità di dover memorizzare e trasferire la propria chiave EOS
* Username abbreviati collegati ad account EOS reali
* Sistema delle ricompense per incentivare gli utenti a valutare contenuti utilizzando il protocollo Yup
* Whitelisting di transazioni Yup di tipo non finanziario, pertanto nessun fastidioso pop-up ogni volta che un utente vota un contenuto
* Notifiche per ricompense recenti / attività sulla rete
* Compatibilità del portafoglio con qualsiasi dApp EOS integrata con [Scatter](https://get-scatter.com/)

L'estensione è implementata in JavaScript e utilizza il front-end web frame Vue.


## Architettura

### EOS
Gli smart contract di EOS eseguono la business logic principale del protocollo Yup. Espongono un'interfaccia per processare i voti, i commenti, i post e altre interazioni degli utenti. Questi smart contract sono basati sulle azioni, le quali sono funzioni che eseguono la business logic e che possono modificare lo stato dello smart contract.

EOS fornisce una piattaforma per smart contract ad alte prestazioni che permette lo sviluppo di applicazioni scalabili e in grado di operare in maniera continua. La piattaforma ci consente di sviluppare applicazioni fornendo una esperienza utente adatta a utenti non esperti. Inoltre, fornisce astrazioni integrate che rendono più semplice lo sviluppo e il mantenimento di applicazioni complesse.

Ecco alcune delle funzionalità più importanti:

* Nomi degli account leggibili dall'uomo
* Supporto integrato per autorizzazione e recupero dell'account
* Transazioni “gratuite” per gli utenti, nessun micropagamento per ogni transazione
* Bassa latenza, elevata velocità di transazione
* Astrazioni di database integrate per memorizzazione persistente, stato indicizzato all'interno dei contratti (ad esempio, tabelle multi-indice)
* Ampio, e in crescita, ecosistema di sviluppatori e robusti strumenti di sviluppo
* Linguaggio dello smart contract familiare e ampiamente popolare \(C++, WASM\)
* Semplici aggiornamento dello smart contract
* Proxy staking per le risorse di account EOS

Una osservazioni sull'utilizzo delle risorse EOS e sui costi delle transazioni:

Mentre EOS non presenta le tradizionali tasse sulle transazioni, l'invio di transazioni nella rete di EOS necessita lo staking di “risorse”. Le risorse EOS rappresentano i costi, in hardware, per processare una transazione. Ci sono 3 tipi di risorse principali: RAM \(i costi di memorizzazione associati a una transazione\), CPU \(il tempo richiesto per l'esecuzione di una transazione\), e NET \(larghezza di banda della rete consumata per una transazione\). Nuovi utenti Yup non possono aspettarsi di acquisire e gestire risorse EOS per il loro account EOS recentemente creato. Pertanto ci occupiamo noi questo aspetto, utilizzando ciò che è noto come “proxy staking”, il quale ci permette di riservare risorse EOS per conto dei nostri utenti.

#### Azioni

I contratti intelligenti di EOS sono sviluppati in C++, e sono basati sulle azioni, funzioni che i contratti sono in grado di eseguire. Il contratto intelligente contiene le seguenti azioni di EOS:

* Crea/Modifica/Elimina Post
* Crea/Modifica/Elimina Voto
* Crea/Modifica/Elimina Commento
* Segui/Smetti di seguire Account

Le azioni dei contratti intelligenti sono testate utilizzando [EOS Factory](https://eosfactory.io/), un framework di testing EOS in Python.

#### Tabelle multi-indice

I contratti intelligenti di EOS includono [tabelle multi-indice](https://developers.eos.io/eosio-cpp/docs/db-api), le quali forniscono un servizio quasi-database per i contratti. **Tali tabelle sono memorizzate nella RAM** e forniscono astrazioni per l'indicizzazione e l'ordinamento dei dati del contratto. Utilizziamo tali tabelle per memorizzare voti, commenti, informazioni relative ai post, le liste di seguaci, ed altri dati leggeri. Dati più pesanti, come immagini e video, sono memorizzati al di fuori della chain utilizzando IPFS.

Implementazioni dei Smart Contract

**Contratti rete principale**

YUP\_CONTRACT\_ACCOUNT=yupyupyupyup

TOKEN\_CONTRACT\_ACCOUNT=yupyupxtoken

**Contratti rete di prova jungle**

YUP\_CONTRACT\_ACCOUNT=yupcorjungle

TOKEN\_CONTRACT\_ACCOUNT=yupxxxjungle

Se desideri visualizzare i dati delle azioni puoi utilizzare il block explorer [EOSX](https://www.eosx.io/)