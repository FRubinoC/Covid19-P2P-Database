# Covid19-P2P-Database
Il lavoro svolto fa riferimento al progetto del corso di Reti Informatiche nell'anno accademico 2020/2021 per la Laurea Triennale in Ingegneria Informatica all'Università di Pisa. Questo progetto consiste nell'implementazione di una rete p2p, con l'obbiettivo di mantenere un database condiviso in merito alle informazioni sul covid19. Il linguaggio di programmazione utilizzato è C e si fonda sull'utilizzo di librerie per la gestione di socket.

In realtà la rete implementata è una rete ibrida, in quanto esiste comunque una figura centralizzata detta Discovery Server (DS), utilizzata per mantenere informazioni riguardo la gestione delle comunicazioni tra i vari peer. Infatti, ogni peer che vuole entrare nella rete deve prima mandare un messaggio UDP al DS, il quale gli riferisce informazioni riguardo ai suoi vicini nella rete.

Ogni peer connesso alla rete può sia ricevere messaggi da altri peer che messaggi dalla shell su cui è stato eseguito. Le entità atomiche gestite dai peer sono dette entry. Ogni entry è formata da quattro field: data, tipo e quantità. Il tipo può essere tampone o nuovo caso. La quantità indica il numero di entità a cui si fa riferimento nella entry. I registri giornalieri vengono chiusi ogni giorno, o su richiesta del DS. Oltre a mantenere questi file, ogni peer accettano comandi che esprimono richieste di elaborazione, ovvero dei calcoli sui dati aggregati. Ogni richiesta sui dati aggregati è valida solamente se i dati di riferimento fanno parte di registri chiusi. Se un peer non ha tutti i dati che gli servono può mandare dei messaggi REQ_DATA, ai quali gli altri peer rispondono con un REPLY_DATA nel caso in cui possiedono il dato richiesto. Se il peer vicino risponde che non possiede i dati richiesti, il peer che deve eseguire il calcolo manda un messaggio FLOOD_FOR_ENTRIES in cui richiede esplicitamente ai vicini se sono in possesso dei dati richiesti riguardanti i regsitri. Se alcuni di essi sono in possesso, il requester manda loro dei messaggi REQ_ENTRIES per farsi comunicare le entry mancanti.

## Peer
Il peer viene eseguito tramite il comando ./peer <porta>
Appena mandato in esecuzione, il peer presenta a video una guida sui comandi. I comandi possibili sul peer sono i seguenti:
* start DS_addr DS_port: richiesta al DS la connessione al network. Il DS registra il peer e gli invia la lista di vicini;
* add type quantity: aggiunge al regsitro della data corrente un entry con tipo type e quantità uguale a quantity;
* get aggr type period: consiste nel chiedere il calcolo di un dato aggregato al software. Aggr indica il tipo di dato aggregato (totale o variazione), type indica il tipo di entry da tenere in considerazione mentre period indica il periodo da tenere in considerazione;
* stop: stoppa la connessione da parte del peer alla rete.

## Discovery Server
Il DS viene eseguito con il comando ./ds <porta>
I comandi disponibili sono:
* help;
* showpeers;
* showneighbour <peer>
* esc

I vicini vengono impostati in base alla vicinanza del numero di porta (dato che il progetto è implementato con l'idea di eseguire i vari peer sullo stesso host, su porte diverse). Le ulteriori scelte implementative sono indicate nel file di documentazione.

