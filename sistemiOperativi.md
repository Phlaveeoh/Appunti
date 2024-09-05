# Sistemi Operativi

## Indice
- [Il sistema di elaborazione](#il-sistema-di-elaborazione)
- [Struttura del SO](#struttura-del-so)
- [Processi](#processi)
- [Threads](#threads)
- [Mutua Esclusione](#mutua-esclusione)
- [Stallo](#stallo)
- [Gestione Memoria](#gestione-della-memoria)
- [Memoria Virtuale](#memoria-virtuale)
- [Politiche di Gestione della Memoria Virtuale](#politiche-di-gestione-della-memoria-virtuale)
- [Scheduling](#scheduling)
- [Gestione I/O](#gestione-io)
- [Gestione File](#gestione-file)

---
---

## Il Sistema di Elaborazione

### Componenti del sistema
* Microporcessore
  * Su un singolo chip
  * Anche multiprocessore: più **cores** su un **socket**
  * Dotato di **cache**
* Processore Grafico (**GPU**)
  * Computa su array di dati
  * Non solo per applicazioni grafiche
* Processore di Segnale Digitale (**DSP**)
  * Codifica/decodifica segnali audio e video
  * Supporto crittografico
* SoC
  * Sistema su un chip per smartphone e palmari

### Ciclo Fetch-Execute
![Ciclo Fetch-Execute](/resource/fetch-execute.png)

### Interruzioni
Permettono di interrompere dil ciclo fetch-execute

#### Fetch-Execute con Interruzioni
![Fetch-Execute con Interruzioni](/resource/fetch-execute-interruzioni.png)

#### Vantaggi
* evitano di lasciare la cpu in attesa
* possono sospendere l'esecuzione di un programma
 
#### Classi di interruzioni 
* Programma
* Timer
* I/O
* Errore HW

#### Esistono due approcci per le interruzioni multiple:
* Interruzioni disabilitate durante la gestione di un’interruzione
* Schema a priorità

#### Aspetti principali nello sviluppo dei sistemi operativi:
* Processi
* Gestione della memoria
* Protezione e sicurezza dell’informazione
* Schedulazione e gestione delle risorse
* Struttura del sistema
  
Approcci e elementi progettuali dei sistemi operativi:
* Architettura microkernel
* Multithread
* Multiprocessing simmetrico
* Sistemi operativi distribuiti
* Architettura orientata agli oggetti
  
### Macchine Virtuali
Il sistema operativo crea delle macchine virtuali ciascuna con la propria memoria e sistema operativo.  
Il VMM gestisce lo scambio di informazioni tra macchine virtuali e processore.  

---
###### [Torna all'indice](#indice)
---

## Struttura del SO
Il sistema operativo si pone da intermediario tra l'utente e l'hardware ed offre vari servizi tra i quali:
* Interfaccia Utente
* Esecuzione Programmi
* Operazioni I/O
* File System
* Comunicazione tra processi
* Rilevazione Errori
* Allocazione Risorse
* Accounting
* Protezione e Sicurezza

### CLI o GUI?
* **CLI**
  * Esegue comandi in sequenza
  * Implementato nel nucleo del sistema
  * Comandi implementati nell'interprete o programmi esterni
* **GUI**
  * Amichevole
  * Metafora della scrivania
  * Icone per file, cartelle e programmi
  * Menù
  * Touch-screen o comandi vocali
  
### System Call
Fungono da interfaccia per programmatore ai servizi del sistema operativo.  
Vi si accede attraverso API che nascondono informazioni al programmatore.

#### Categorie delle System Call
* Controllo Processi
* Gestione File
* Gestione I/O
* Gestione Informazioni
* Comunicazione
* Protezione

### Programmi di Sistema 
Realizzano l'ambiente utile per lo sviluppo e l'esecuzione dei programmi ad esempio attraverso editor, servizi in background, programmi applicativi.

### Struttura del SO
- Monolitico (MS-DOS, Unix)
- Stratificato
  - Ogni strato è l'implementazione di un oggetto
  - Ogni **layer** nasconde i dettagli agli strati superiori ed inferiori
  - Vantaggi di implementazione
  - Svantaggi per via dell'Overhead
- Microkernel
  - Il microkernel contiene solo le funzionalità essenziali 
  - Tutto il resto viene eseguito a livello dell'utente

### Avvio del Sistema
L'avvio dell'esecuzione del sistema avviene in uno specifico indirizzo della ROM detto **Bootstrap Loader**.  
Esso carica il Bootstrap Program che a sua volta carica il kernel che poi avvia il sistema

---
###### [Torna all'indice](#indice)
---

## Processi
Un **processo** (o **job**) è un programma in esecuzione

Possiede:
* un Program Cunter (PC)
* Stack (per dati temporanei)
* Programma
* Variabili Globali
* Heap (allocazione dinamica)

>[!IMPORTANT]
>Il programma è un'**entità passiva** (file eseguibile).  
>Il processo l'**entità attiva** con PC e risorse assegnate!  
>Un programma può avere quindi più processi

### Gli Stati del Processo
![Stati del Processo](/resource/stadi-processo.png "Stati del Processo")
* Nuovo
* Esecuzione
* Attesa
* Pronto
* Terminato

### Descrittore di Processo (PCB)
A ogni processo è associata una struttura dati detta descrittore di processo
(**process control block**), contenente tutte le informazioni relative al
processo.  
Un processo può avere multiple sequenze di istruzioni (**threads**) che possono
essere eseguite in parallelo (**anche su core differenti**) per realizzare task
differenti.
In tal caso, il PCB include informazioni su ciascun thread.

### Schedulatori
Sono di tre tipi:
* Breve termine
  * CPU Scheduler
  * Assegna la CPU ad uno dei processi pronti
  * Eseguito con grande frequenza
* Lungo termine
  * Job Scheduler
  * Seleziona i processi da inserire in coda pronti
  * Non eseguito spesso
* Medio termine
  * Assegna e revoca memoria ai processi (**Swapping**)

Le politiche per l'assegnazione sono divise tra I/O-bound e CPU-bound

### Creazione e Terminazione Processi
Il sistema operativo fornisce meccanismi per la creazione e la terminazione dei processi

* Creazione
  * Un processo genitore crea un processo figlio che a sua volta può creare altri processi
  * Ogni processo ha un identificatore (**PID**)
* Assegnazione risorse
  * Il processo figlio ottiene le risorse dal genitore o dal SO
* Opzione di esecuzione
  * Può essere eseguito concorrentemente al genitore
* Terminazione
  * Il processo termina con la chiamata di exit() o abort() (da parte del genitore)
  * Il pid viene deallocato solo dopo il wait() del genitore

---
###### [Torna all'indice](#indice)
---

## Threads

Si dice multithreading la capacità di un SO di permettere più tracce di
esecuzione concorrenti in un singolo processo.  
Quattro casi:  
|| Single-Threading | Multi-Threading|
|--- |--- |--- |
|**Single-Process**| Un processo, un thread | Un processo, più thread |
|**Multi-Process**| Più processi, un thread | Più processi, più thread |

In un ambiente multi-thread, il processo è l’unità di allocazione delle risorse
e un’unità di protezione.  
Associati al processo:
* Spazio di indirizzamento virtuale
* Accesso protetto al processore
* Uno o più thread

Associati al thread:
* stato
* contesto del thread (immagine del processore)
* stack di esecuzione
* spazio di memoria statico per le variabili locali
* accesso a memoria e risorse del processo, condivise con gli altri thread
del processo

Benefici dei thread:
* Velocità di creazione e terminazione
* Rapidità di switch tra thread dello stesso programma
* Comunicazione Efficiente tra programmi

>[!NOTE]
>La schedulazione è gestita principalmente a livello di thread.  
>Ma non la sospensione (memory swap) e la terminazione.

I thread hanno bisogno di essere sincronizzati tra di loro.  
Esistono tre approcci:
* Thread a livello utente
  * Vantaggi
    * Immediati
    * Schedulazione specifica
    * Portabili
  * Svantaggi
    * System call bloccanti per tutto il processo
    * Nessun vantaggio dalla multiprogrammazione
* Thread a livello kernel
  * Vantaggi
    * thread diversi su differenti processori
    * blocco thread non bloccante per il processo
    * kernel routine gestite in multithreading
  * Svantaggi
    * Cambi di contesto producono Overhead
* Approcci misti

### Thread su UNIX
Il kernel non distingue tra processi e thread.  
Per ogni **task** (processo o thread) c'è un **Task Control Block**.
* fork() crea una nuova task con una copia del **Task Control Block**
* clone() crea un nuova task con un puntatore al **Task Control Block** del genitore

---
###### [Torna all'indice](#indice)
---

## Mutua Esclusione

La concorrenza appare in 3 differenti contesti:

* Applicazioni multiple
* Applicazioni strutturate
* Struttura del SO

La velocità relativa dei processi **NON** è prevedibile.  
È quindi necessario proteggere le variabili condivise e controllare il codice che accede ad una variabile.

### Race Condition
Si verifica quando più processi o thread leggono e scrivono dati in modo che il risultato dipenda dall'ordine di esecuzione delle istruzioni dei processi.

### Gradi di Consapevolezza
![Gradi di Consapevolezza](/resource/gradi-consapevolezza.png "Gradi di Consapevolezza")

#### Competizione per le risorse
Processi in conflitto tra loro per l'accesso alle risorse.

#### Cooperazione per Condivisione
Processi che interagiscono senza conoscersi usano e modificano dati condivisi.  
Va assicurata l'integrità dei dati e l'accesso controllato alle risorse.

>[!NOTE]
> Le operazioni di lettura non richiedono Mutua Esclusione

#### Cooperazione per Scambio di Messaggi
I processi collaborano per un obiettivo comune scambiandosi messaggi per ottenere la sincronizzazione, le primitive per lo scambio die messaggi sono fornite dal kernel o linguaggio di programmazione.  
Non richiede mutua esclusione ma può comunque avvenire deadlock o starvation.

### Requisiti per la Mutua Esclusione
* Solo un processo alla volta ammesso alla sezione critica
* Un processo che si ferma fuori dalla propria sezione critica non deve interferire con gli altri processi
* Si deve evitare deadlock e starvation
* Se nessun processo è nella sezione critica deve essere concesso ad ogni processo di entrare senza ritardo
* Nessuna ipotesi sulla velocità
* Un processo rimane per un tempo finito nella sezione critica

### Mutua Esclusione con supporto HW
#### Disabilitazione delle interruzioni
Assicura che la sezione critica venga eseguita senza interruzioni.  
Rallenta il sistema

>[!WARNING]
> Inapplicabile Con multiprocessore

* Istruzioni speciali
  * Vantaggi
    * Applicabili a qualsiasi numero di processi
    * Semplicità
    * Supporto multiple sezioni critiche
  * Svantaggi
    * Attesa attiva
    * Possibilità di stallo e starvation
* Compare and Swap

### Algoritmo di Dekker
![Algoritmo di Dekker](/resource/algoritmo-dekker.png "Algoritmo di Dekker")
### Algoritmo di Peterson
![Algoritmo di Peterson](/resource/algoritmo-peterson.png "Algoritmo di Peterson")

### Semafori

#### Semaforo Generico
Una variabile con valore intero, una coda e tre operazioni:
* Inizializzazione a valore non negativo
* **semWait()** decrementa il valore del semaforo
  * Se il valore diventa negativo, il processo è sospeso e inserito nella coda
* **semSignal()** incrementa il valore del semaforo.
  * Se il valore non diventa positivo, uno dei processi della coda è
riattivato

#### Semaforo Binario
Una variabile con valore binario (0 o 1), una coda e tre operazioni
* inizializzazione a zero o uno
* **semWaitB()** verifica il valore del semaforo
  * Se il valore è 1 allora è portato a 0, nel caso contrario, il processo è
  sospeso e inserito nella coda,
* **semSignalB()** verifica se ci sono processi in attesa nella coda.
  * In caso affermativo, uno dei processi della coda è riattivato, nel caso
  opposto, il valore del semaforo è portato a 1.

>[!NOTE]
>semWait() e semSignal() devono essere primitive atomiche

### Messaggi
Nell'interazione tra processi sono necessarie
* Sincronizzazione
* Comunicazione
  
Lo scambio di messaggi permette di realizzare entrambi tramite due primitive:
* send()
* receive()

#### Caratteristiche dei Sistemi di Messaggistica
* Sincronizzazione
* Indirizzamento
* Formato
* Organizzazione delle code

#### Sincronizzazione
Il destinatario non può ricevere il messaggio prima che sia stato inviato.  
Quando esegue receive():  
* Se è presente un messaggio lo riceve
* Se non è presente o si sospende o continua l'esecuzione

Tre casi:
* Send bloccante, Receive bloccante
* Send non bloccante, Receive Bloccante
* Send non bloccante, Receive non Bloccante

#### Indirizzamento
* Diretto
  * Send include il destinatario nel messaggio
* Indiretto
  * Messaggi inviati ad una **mailbox**
  * Destinatari possono prendere i messaggi se presenti

#### Gestione delle code
* FIFO
* Priorità

### Monitor
Costrutto di alcuni linguaggi di programmazione con funzionalità analoghe
ai semafori.  
Disponibile in numerosi linguaggi (Concurrent Pascal, Modula-2, Modula-3,
Java) e in moduli di libreria.

#### Variabili di Condizione
Variabili accessibili solo all’interno del monitor attraverso due funzioni:
* **cwait(c)** sospende l’esecuzione del processo che la ha eseguita sulla
condizione c
* **csignal(c)** riattiva uno dei processi in attesa sulla condizione c; se non ce ne sono non fa nulla.

---
###### [Torna all'indice](#indice)
---

## Stallo
Un insieme di processi è in stallo (**deadlock**) se ogni processo è in attesa di un evento che può essere generato solo da un altro processo dell’insieme  

### Categorie di risorse
* Riutilizzabili
  * Utilizzabili solo da un processo alla volta, non vengono ditrutte dopo l'utilizzo
* Consumabili
  * Possono essere prodotte e consumate

### Condizioni per lo stallo
  1. Mutua Esclusione
  2. Possesso e Attesa
  3. Assenza di Prerilascio
  4. Attesa Circolare
  
1, 2, 3 => Stallo **possibile**  
1, 2, 3, 4 => Stallo **esistente**

### Azioni contro lo stallo
* prevenzione
* esclusione
* rilevamento

#### Prevenzione
Progettare il sistema in modo da escludere lo stallo
* Metodi Indiretti
  * Prevengono il presentarsi di:
    * Mutua esclusione
    * Possesso e attesa
    * Assenza di prerilascio
* Metodi Diretti
  * prevengono l'attesa circolare

#### Esclusione
Due approcci:
* **Rifiuto di esecuzione** per i processi le cui richieste potrebbero determinare stallo
  * Inefficiente
* **Rifiuto di allocazione** per le richieste di risorse che potrebbero determinare stallo
  * Algoritmo del Banchiere

Vantaggi dell'evitare lo stallo:
* meno restrittivo della prevenzione
Limitazioni:
* risorse allocabili in numero fisso
* i processi devono stabilire in anticipo le risorse necessarie
* nessun processo può terminare senza aver rilasciato le risorse

#### Rilevamento
* Risorse assegnate senza restrizioni
* periodiche verifiche dell'assenza di attesa circolare

Vantaggi:
* algoritmo semplice
* la rilevazione dell'attesa circolare permette un ripristino precoce

Svantaggi:
* Overhead

### In Sintesi
![Approcci Stallo](/resource/approcci-stallo.png "Approcci Stallo")

### Strategia Integrata
Raggruppare le risorse in classi ed usare l'assegnazione ordinata per prevenire attesa circolare tra i processi.  
Ogni classe utilizza il metodo più appropriato:
* Spazio Swap (assegnazione statica)
* Risorse dei processi (algoritmo del banchiere)
* Memoria principale (prevenzione tramite prerilascio)
* Risorse interne (assegnazione ordinata)

---
###### [Torna all'indice](#indice)
---

## Gestione della memoria
Compiti della gestione della memoria:
* Rilocazione
  * Il processo può essere **rilocato** in una differente area di memoria
* Protezione
  * I processi devono avere la possibilità di fare riferimenti alle locazioni di memoria per leggere e scrivere dati
* Condivisione
  * È utile permettere ai processi che eseguono lo stesso programma di accedere entrambi alla stessa copia del codice
* Organizzazione Logica
  * La memoria principale è organizzata linearmente, i programmi sono scritti in moduli
* Organizzazione Fisica
  * La memoria disponibile per programma e dati potrebbe essere insufficiente

### Partizionamento della memoria
Il gestore della memoria carica i processi nella memoria principale per essere eseguiti
* realizza la memoria virtuale
* si basa su:
  * **Partizionamento**
  * **Paginazione**
  * **Segmentazione**

### Partizionamento
Divide la memoria in partizioni che possono essere assegnate ai processi.  
Obsoleto, non prevede la memoria virtuale e le partizioni possono essere:
* Statiche
  * Necessario **overlay**
  * Utilizzo inefficiente della memoria
  * **Frammentazione Interna**
* Dinamiche
  * **Frammentazione Esterna** (necessita compattazione)

#### Algoritmi di Allocazione
* Best-Fit
* First-Fit
* Next-Fit

#### Buddy System
Funge da compromesso tra le partizioni fisse e dinamiche

![Buddy System](/resource/buddy-system.png "Buddy System")

#### Indirizzi
* Indirizzo logico
  * Riferimento a una locazione di memoria indipendente dall’assegnazione della memoria principale al processo
* Indirizzo fisico
  * Effettiva posizione del dato nella memoria principale
* Indirizzo relativo
  * Indirizzo logico espresso come posizione relativa a un altro indirizzo noto

#### Rilocazione
* Statica
  * Se ad un processo sarà assegnata sempre la stessa partizione
* Dinamica
  * Se il processo può occupare diverse partizioni durante il suo ciclo di vita

### Paginazione
La memoria principale è divisa in blocchi di dimensioni fisse detti **frame**.  
I programmi sono divisi in blocchi (**pagine**) della medesima dimensione dei frame e saranno poi caricati all'interno di quest'ultimi non necessariamente in maniera contigua.

#### Tabella delle pagine
Diventa necessaria una tabella delle pagine:
* Una per processo, gestita dal SO
* Contiene la posizione del frame per ogni pagina del processo
* Il processore accede alla tabella delle pagine per produrre l'indirizzo fisico del processo attualmente in esecuzione

### Segmentazione
* I programmi possono essere divisi in segmenti dalla lunghezza differenza (la lunghezza massima è limitata).  
* L'indirizzo logico è diviso in indice di segmento ed offset.  
  * L'indirizzo fisico è dato dalla somma dell'indirizzo all'indice di segmento e l'offset.  
* Il programmatore deve tenere conto della lunghezza dei segmenti.

---
###### [Torna all'indice](#indice)
---

## Memoria Virtuale
Alcune caratteristiche fondamentali per la gestione dinamica della memoria sono:
* Rilocazione **run-time**
* Possibilità di spezzare il processo in parti che non necessitano di essere caricati in spazi contigui della memoria durante l’esecuzione

>[!NOTE]
>Non è necessario che tutte le pagine, o segmenti, siano in memoria principale durante l’esecuzione.

### Esecuzione
1. Il SO carica in memoria solo poche pagine (o segmenti) del programma (**Resident set**)
2. Quando è richiesto un indirizzo non presente in memoria principale si genera un’interruzione
3. Il SO sospende il processo e il processore è assegnato a un altro processo
4. La pagina (o segmento) contenente l’indirizzo logico richiesto è caricata in memoria
   1. DMA
5. Al termine del caricamento, è generata un’interruzione in seguito alla quale il SO riattiva il processo.

Vantaggi:
* Incremento della multiprogrammazione
* Un processo può esserepiù lungo della memoria principale

### Principio di Località
Si basa su due principi:
* **Località Temporale**
* **Località Spaziale**

Serve per evitare il **trashing**

#### Trashing
Stato in cui il sistema spende la maggior parte del suo tempo nel caricamento e scaricamento dalla memoria, anziché nell’esecuzione dei processi.

### Supporto HW e SW per la memoria virtuale
* L’hardware deve supportare la paginazione e la segmentazione
* Il SO deve includere il software per gestire il movimento delle pagine o dei segmenti fra memoria secondaria e memoria principale

### Paginazione
* Nel linguaggio comune, il termine memoria virtuale si riferisce alla paginazione dinamica.
* Ogni processo ha una tabella delle pagine

![Paginazione dinamica con supporto HW](/resource/paginazione-dinamica-supporto-hw.png "Paginazione dinamica con supporto HW")

#### Tabella delle pagine invertita
![Tabella delle pagine invertita](/resource/tabella-pagine-invertita.png "Tabella delle pagine invertita")

#### Translation Lookaside Buffer
* Evita un doppio accesso in memoria a ogni riferimento
* La cache contiene le PTE usate di recente
* Mappa associativa

#### Dimensione delle pagine
* Pagine piccole riducono la frammentazione interna ma aumentano la dimensione della tabella delle pagine
* Le caratteristiche fisiche della memoria secondaria influiscono sulla scelta
* La dimensione delle pagine influisce sulla frequenza di errori di pagina

### Segmentazione
Con la segmentazione il programmatore vede la memoria composta da multipli spazi di indirizzamento (**segmenti**).  
* Semplifica la gestione di strutture dati di dimensione variabile
* Permette modifica e ricompilazione di segmenti di programma
* Facilita la condivisione di dati
* Facilita la protezione

#### Tabella dei Segmenti
Ogni record della tabella contiene:
* L'indirizzo iniziale del corrispondente segmento in memoria
* La lunghezza del segmento
* Flags "Caricato" e "Modificato"

![Rilocazione Segmentata](/resource/rilocazione-segmentata.png "Rilocazione Segmentata")

### Segmentazione Paginata
Ibrido tra le due tecniche:
* Lo spazio logico è diviso in segmenti
* Ogni segmento è diviso in pagine di dimensione fissa uguale ai frame in memoria principale

![Segmentazione Paginata](/resource/segmentazione-paginata.png "Segmentazione Paginata")

#### Protezione e Condivisione
La segmentazione facilita l’implementazione della protezione e della condivisione

---
###### [Torna all'indice](#indice)
---

## Politiche di Gestione della Memoria Virtuale
La progettazione del gestore della memoria dipende da tre decisioni fondamentali:

* Usare o meno tecniche di memoria virtuale,
* Usare la paginazione, la segmentazione o la segmentazione paginata
* Gli algoritmi utilizzati per i vari aspetti della gestione della memoria

### Strategia di Caricamento
Determina quando una pagina deve essere caricata in memoria.

#### Paginazione a richiesta
Carica una pagina in memoria solo quando c’è un riferimento a un indirizzo della pagina.

#### Prepaginazione
Sono caricate altre pagine oltre a quelle che hanno determinato l’errore di .pagina.

### Strategia di Posizionamento
Determina in quale parte della memoria principale la pagina o il segmento deve essere caricato.  
Importante solo per la segmentazione non paginata.

### Strategia di Sostituzione
Seleziona la pagina in memoria principale da sostituire quando è necessario caricare una nuova pagina.

#### Algoritmo Ottimo
Sostituisce la pagina che in futuro sarà riferita per ultima.

>[!WARNING]
> **IRREALIZZABILE**

#### Last Recently Used (LRU)
Sostituisce la pagina che non è stata riferita per il tempo massimo.
* Difficile da implementare
* Grande Overhead

#### First-in-First-out (LRU)
* Pagine in un buffer circolare (coda **FIFO**)
* Rimosse in ordine di caricamento
* Facile da implementare
* Sostituisce la pagina da più tempo in memoria

#### Strategia dell’orologio
Associa un flag a ogni frame (bit di utilizzo) che viene settato ad 1 quando la pagina viene caricata o riferita.  
L'algoritmo scandisce le pagine caricate e se il loro flag è ad 1 lo imposta a 0, se è a 0 le scarica.

#### Buffer per Pagine
Migliora le prestazioni della paginazione e permette l’uso di politiche di sostituzione più semplici
* Le pagine scaricate sono inserite in due liste:
  * Lista delle pagine libere
  * Lista delle pagine modificate
* Le pagine restano disponibili prima della sovrascrittura
* Le pagine modificate sono salvate a gruppi

#### Resident Set
Il SO decide quante pagine di ogni processo tenere in memoria principale

* Allocazione Fissa
  * Processo ha un numero fisso di frame in cui essere eseguito
  * A ogni errore di pagina si scarica una pagina del processo che lo ha determinato
* Allocazione Variabile
  * Il numero dei frame allocati a un processo può variare nel tempo

#### Ambito della Strategia di Sostituzione
* Locale
  * Sceglie la pagina da sostituire tra le pagine residenti del processo che ha generato l'errore
* Globale
  * Considera tutte le pagine non bloccate in memoria principale

#### Working Set
Il working set è l’insieme delle pagine riferite in un intervallo di tempo virtuale

### Politica di Cleaning
#### Cleaning su Richiesta
La pagina è salvata in memoria secondaria solo quando deve essere sostituita
#### Precleaning
Permette il salvataggio di pagine a lotti

### Politica di caricamento
Determina il numero dei processi residenti in memoria principale

---
###### [Torna all'indice](#indice)
---

## Scheduling
Lo scopo dello scheduling è assegnare al processore i processi da eseguire in modo da realizzare gli obiettivi del sistema:
* Tempo di risposta
* Throughput
* Efficienza del processore

Sono divisi tra:
* Lungo termine
  * Inserimento nella lista processi da eseguire
* Medio termine
  * Aggiunta ai processi caricati in memoria principale
* Breve termine
  * Selezione del processo da eseguire
* Scheduling I/O
  * Seleziona il processo in attesa di I/O da assegnare ad un dispositivo disponibile

### Schedulatore a lungo termine
Determina quali programmi sono ammessi nel sistema per l’esecuzione e controlla il grado di multiprogrammazione.  
Per creare un processo dalla coda dei nuovi job, lo schedulatore deve decidere:
* Quando il SO può accettare uno o più nuovi processi
* Quali job trasformare in processi

### Schedulatore a medio termine
Parte della funzione di Swapping

### Schedulatore a breve termine (Dispatcher)
Viene eseguito con grande frequenza e decide quale tra i processi pronti sarà eseguito.  
Viene attivato da: 
* Interruzioni di clock
* Interruzioni I/O
* System Call
* Segnali

L'obbiettivo è allocare la CPU in modo da ottimizzare alcuni aspetti del comportamento del sistema

* Criteri orientati all'utente
* Criteri orientati al sistema
* Criteri relativi alle prestazioni
* Criteri non relativi alle prestazioni

#### Criteri nel dettaglio
* Tempo di risposta
* Tempo di turnaround
* Scadenze
* Prevedibilità
* Throughput
* Utilizzazione del processore
* Fairness
* Priorità
* Bilanciamento delle risorse

#### Funzione di Selezione
Determina quale processo, tra quelli pronti, è selezionato per l’esecuzione

#### Modo di Decisione
Specifica il momento in cui è eseguita la decisione:
* Senza prerilascio
  * Il processo in esecuzione continua fino al termine o fino a un’interruzione per I/O
* Con prerilascio
  * Il processo in esecuzione può essere interrotto dal SO e posto in stato di pronto

#### FIFO
* Metodo più semplice
* Ogni processo nella coda pronti
* Quando il processo termina o viene sospeso viene selezionato il processo da più tempo in coda

#### Round Robin
* Prerilascio basato su un **clock**
* Bisogna progettare il **quanto** del tempo con attenzione

#### Shortest Process Next (SPN)
Seleziona il processo col minimo tempo di esecuzione da eseguire senza prerilascio

#### Shortest Remaining Time (SRT)
È un SPN con prerilascio. Seleziona il processo con il minor tempo di esecuzione rimanente stimato.  
Rischio di **starvation** per processi lunghi e grande **overhead** per la stima del tempo rimanente

#### Highest Response Ratio Next (HRRN)
Seleziona il processo con il più alto rapporto di risposta (tempo di turnaround normalizzato)

#### In Sintesi
![Tecniche di Schedulazione](/resource/tecniche-schedulazione.png "Tecniche di Schedulazione")

### Schedulazione UNIX tradizionale
* Ottimizzato per sistemi time-sharing
* Buon tempo di risposta per i processi interattivi, evitando starvation dei processi in background
* Feedback multilivello con round-robin in ogni coda di priorità
* Prerilascio dopo 1 sec
* Priorità basata su tipo di processo e storia dell’esecuzione

---
###### [Torna all'indice](#indice)
---

## Gestione I/O

### Categorie di Dispositivi I/O
* Leggibili all'uomo
* Leggibili dalla macchina
* Dispositivi di comunicazione

### Caratteristiche dei Dispositivi I/O
* Velocità di Trasferimento
* Applicazioni
* Complessità di Controllo
* Unità di Trasferimento
* Rappresentazione dei Dati
* Condizioni di Errore

### Funzioni I/O
* I/O programmato
  * Il processore genera un comando di I/O per conto di un processo che rimane in attesa attiva durante il completamento dell’operazione
* I/O guidato dalle interruzioni
  * Il processore genera un comando di I/O per conto di un processo:
    * Non bloccante
    * Bloccante

Un modulo di DMA controlla lo scambio di dati tra memoria principale e
dispositivo I/O

### Obiettivi di Progettazione
* Efficienza
  * Maggiore obiettivo della progettazione
  * Soprattutto per I/O dal disco
* Generalità
  * Ricerca uniformità nella gestione dei dispositivi
  * Difficile da realizzare

### Struttura Logica

* I/O Logico
  * Tratta il dispositivo come risorsa logica
* I/O sul Dispositivo
  * Converte le operazioni richieste in sequenze di istruzioni
* Scheduling e Controllo
  * Accodamento, scheduling, controllo, interruzioni
* Gestione Directory e File e Organizzazione Fisica
  * Realizzazione dell’astrazione dei file

### Buffering
Per evitare inefficienze e overhead, può essere utile eseguire trasferimenti di input in anticipo sulla richiesta e trasferimenti di output in ritardo rispetto alla richiesta.

* Dispositivi a Blocchi
* Dispositivi a Flusso

#### Buffer Singolo
* Dispositivi a blocchi
  * Sistema di supporto più semplice
  * Nelle operazioni di input i dati sono inseriti nel buffer
  * Input anticipato
  * Velocizza il sistema
* Dispositivi a Flusso
  * Una linea alla volta
  * Un byte alla volta

![Buffer Singolo](/resource/buffer-singolo.png)

#### Doppio Buffer
* Due buffer assegnati all’operazione di I/O
* Un processo trasferisce dati da/a un buffer mentre il SO svuota o riempie l’altro (buffer swapping)

![Doppio Buffer](/resource/doppio-buffer.png)

#### Buffer Circolare
Più di due buffer assegnati all’operazione di I/O

![Buffer Circolare](/resource/buffer-circolare.png)

### Parametri di Prestazioni del Disco
* Tempo di accesso
  * Tempo di ricerca
    * Tempo necessario a muovere il braccio del disco sulla traccia richiesta
  * Ritardo rotazionale
    * Il tempo necessario al settore per raggiungere la testina
  * Tempo di trasferimento

### Politiche di Schedulazione del Disco
L'obiettivo è quello di ridurre il tempo medio di ricerca
* FIFO
  * Richieste processate per ordine di arrivo
* Priorità
  * La politica è gestita al di fuori del software di controllo del disco
* SSTF
  * Precedenza alla richiesta con tempo di servizio minore
* SCAN
  * Il braccio si muove in una direzione e soddisfa tutte le richieste che trova, poi inverte la direzione
* C-SCAN
  * Restringe la scansione a un’unica direzione
* N-Step-SCAN
  * Le richieste sono inserite in sotto-code di lunghezza N e processate con SCAN
* F-SCAN
  * Due sottocode, inizialmente una è vuota, le richieste vengono eseguite su una coda mentre l'altra viene riempita

---
###### [Torna all'indice](#indice)
---

## Gestione File
Un file è una collezione di dati creata dall'utilizzatore.

#### Proprietà
* Persistenza
* Condivisione
* Struttura

### File System
Fornisce servizi agli utenti e alle applicazioni che permettono l’utilizzo dei file
* Uno strumento per memorizzare dati organizzati in file
* Collezione di funzioni per operare sui file
* Gestione degli attributi associati ai file

Fornisce varie operazioni:
* Creazione
* Cancellazione
* Apertura
* Chiusura
* Lettura
* Scrittura

### Architettura del Sistema di Gestione dei File
![Architettura File System](/resource/architettura-file-system.png)

#### Driver dei Dispositivi
* Livello più basso
* Comunica direttamente con i dispositivi periferici
* Responsabile dell’avvio delle operazioni I/O su un dispositivo
* Gestisce il completamento delle richieste di I/O

#### Base del File System
* Interfaccia primaria con l’ambiente esterno (livello dell’I/O fisico)
* Si occupa dei blocchi di dati scambiati con il disco (o i nastri)
* Controlla il posizionamento dei blocchi nella memoria secondaria
* Organizza i buffer in memoria principale
* Non conosce né il contenuto dei dati né la struttura del file

#### Supervisore dell’I/O base
* Responsabile di inizio e terminazione di tutte le operazioni di I/O su file
* Gestisce le strutture di controllo per i dispositivi I/O, la schedulazione dei dispositivi e lo stato dei file.
* Seleziona il dispositivo su cui eseguire I/O
* Si occupa della schedulazione del disco e degli altri dispositivi per ottimizzare le prestazioni
* Assegna i buffer I/O e alloca la memoria secondaria

#### I/O logico
* Abilita utenti e applicazioni ad accedere ai record
* Fornisce le funzioni di I/O ai record di tipo generale
* Realizza le basi di dati relative ai file

#### Metodi di accesso
* Il livello più prossimo all’utilizzatore
* Fornisce un’interfaccia standard tra le applicazioni e il file system e i dispositivi
* Differenti metodi di accesso riflettono differenti strutture dei file e modi differenti di accedere ai dati

### Directory
Operazioni Richieste:
* Ricerca
* Crea file
* Cancella file
* Listare il contenuto della directory
* Aggiornare la directory

#### Schema a due Livelli
* Una directory principale e una directory per ogni utente
* La directory principale contiene una entry per ogni directory utente con l’indirizzo e i diritti di accesso
* Ogni directory utente è una lista di files dell’utente
* Nomi unici solo all’interno di ciascuna directory
* Il SO impone le restrizioni di accesso sulle directory

#### Schema ad Albero

### Allocazione dei File
* In memoria secondaria, un file consiste di una collezione di blocchi
* Il SO gestisce l’allocazione dei blocchi ai file
* L’approccio scelto per l’allocazione dei file influenza la gestione dello spazio libero
* Lo spazio è allocato in una o più porzioni (insiemi di blocchi contigui)

#### FAT
Struttura dati che tiene traccia dei blocchi allocati al file

#### Preallocazione o allocazione dinamica ?
* La preallocazione richiede che la dimensione del file sia dichiarata al momento della creazione
* L’allocazione dinamica alloca lo spazio al file in base alle necessità

#### Dimensioni delle Porzioni
* Porzioni contigue grandi e variabili
  * Prestazioni efficienti
  * Evita sprechi di spazio
  * tabelle di allocazione piccole
* Blocchi
  * Flessibili
  * tabelle di allocazione grandi
  * non contigui

#### Gestione degli Spazi Liberi
È necessario conoscere i blocchi disponibili: **tabella di allocazione del disco**

* Bitmap
* Porzioni Libere Concatenate
* Indicizzazione

### In UNIX
* I file UNIX sono amministrati mediante gli i-node
* L’allocazione è gestita sulla base dei blocchi
* Per tenere traccia dei blocchi assegnati a ogni file si usa un metodo a indice con:
  * alcuni indici dell'i-node
  * tre livelli di indicizzazione diretta

---
###### [Torna all'indice](#indice)
---
