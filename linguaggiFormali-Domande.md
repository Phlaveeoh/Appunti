# Domande 
## 1 **Grammatica a struttura di frase, come si definisce la grammatica?**

   1. La e definita tramite una quadrupla di $G= \langle V, \Sigma, P, S \rangle$  
   **$V$** è l'alfabeto finito detto vocabolario totale.  
   **$\Sigma$** è l'alfabeto dei simboli terminali con $\Sigma \subseteq V$           
   **$S$** è il simbolo iniziale o assioma  
   **$P$** è un insieme finito di espressioni della forma  
   2. con $\alpha$ che è detto insieme delle produzioni e le lettere N = V - Sigma si dicono Variabili e si indicano con le lettere maiuscole e le minuscole si dicono terminali.

## 2 **Chiusura di Kleene** 
   1. La chiusura di Kleene in un linguaggio: 
     $L^* = \bigcup_{n≥0}  L^n = (u_1u_2, ··· u_n | n \geq 0, u_1, u_2, ... u_n \in L)$  
    **Spiegato** La chiusura di Kleene di un linguaggio è l'insieme di tutte le possibile concatenazioni dei simboli di un linguaggio.  
    avendo un alfabeto $\Sigma[a, b]$ la chiusura di Kleene sarà:  
    $L^0 = \epsilon \\ 
    L^1 = [a, b] \\ 
    L^2 = [aa, ab, ba, bb] \\
    L^3 = L^1 * L^2$  
    O più in generale: $L^n = L^1 * L^{n-1}$ 

## 3 **Cos'è una espressione regolare? {Fasi di sintesi e analisi (linguaggio accettato da automa) per stabilire se è una espressione regolare}?**
    Una epressione regolare e talse se segue queste 3 regole
   1. Ogni lettera a e una espressione regolare e anche 0 e una espressione regolare
   2. se E e F sono espressioni regolaro, allora (E+F), (EF) e E* sono epressini regolari
   3. Solo le parole che si ottengono applicando un numero finito di volte anche loro sono espressioni regolari.

   **Sintesi** data una espressione regolare ricavo un automa (disegnini)

   **Analisi** dato un automa e possibile ricavare un automa. 

   **Per teorema di Kleene** un linguaggio e regolare se e solo se e riconosciuto da un automa a stati finiti.



## 4 **Parsing top - down (ricerca in ampiezza e ricerca in prfondità)**  
il parsing top down consiste nel creare un albero di derivazione data una grammatica, con vari algoritmi, e abbiamo 2 strategia principali Bottom Up Top Down

**Ricerca Ampiezza** e un metro di parsing che consiste nel verificare tutti i rami possibili contemporaneamente (sviluppo dei rami in ambiezza) 

**Ricerca In Profondità** consiste nel decidere albrirariamente quale strada decidere, in caso ci si accorge che la strada non risulta essere quella corretta si fa backtraking. 

## 5 Che succede se ho un caso di funzione x -> aX? 
E una grammatica in forma normale di greibach, perche tutte le produzioni producono un simbolo terminale seguito da una variabile, e S -> Epsilon solo se S non compare a destra delle produzioni. 

X -> aY con a appartiene a Sigma Y appartiene N*
S -> Epsilon solo se S Non appare ai lati destri delle produzioni.

Per costruire una qualsiesi parola si deve usare N passi dove N e la lungezza esatta della parola.

## 6 Teorema di nerode e 3 condizioni equivalenti
Sia L un linguaggio sull'alfabeto Σ le seguenti proposizioni sono equivalenti:  
   1. L e regolare,
   2. L e Unione Classi di congruenza a destra su Σ* di indice finito
   3. N(L) ha indice finito  
   
**Congrunza a destra**
si tratta di una relazione di equivalenza su un'insieme di stringe o parole che e compatibile con l'operazione di concatenazione dal lato destro, in altre parole se 2 stringhe sono equivalenti sotto una congruenza a destra, allora concatenare la stessa stringa ad entrambe le stringhe iniziali mantiene l'equivalenza.  

ES utilizzo x ~ y allora xz ~ yz
e comodo perche ci permetti di capire se 2 stati di un automa vanno in una stesso stato.

**N(L)** 
e l'insieme delle stringhe o parole che hanno la stessa congruenza a destra. 

**le 3 condizioni equivalenti I = II = III = I ..**  
   1. I = II:   
   sia L linguiaggio regolare, per teorema di kleene (se abbiamo un linguaggio regolare quel l'inguaggio e riconosciuto da un automa a stati finiti e viceversa) dunque possiamo supporre che tutti gli stati dell'automa sono accessibili, dunque per ogni stato q dell'automa consideriamo il linguaggio generato da quello stato (Lq), che sono tutte le parole che sono generate a partire dello stato iniziale fino allo stato q.  
   Adesso dimistriamo che ~ associata a tale partizione e una congruenza a destra  
   Supponiamo u ~ v che sono in una stessa classe Lq,  
   per ogni z appartiene Σ* la funzione di transizione 
   δ(q0, uz) = δ(q0, vz) = δ(q, z) (primi 2 delta sono delta cappuccio)
   *verificata la congruenza a destra!*
   2. II = III  
      supponiamo che L sia una unione di classi di congruenza a destra ~ su un Σ* di indice finito, per dimostrare la proposizione 3 basterà far vedere che ogni classe di NL e un'unione di classi di ~  
      in effetti se u ~ v allora uz ~ vz dato che il linguaggio e unione di classi di ~ o sia uz sia vz stanno in L o nessuna delle 2 sta nel linguaggio.
      uz appartiene L se solo se vz appartiene L dunque u NL v questo prova che ogni classe di equivalenza ~ e interamente contenua in una classe di NL dunque l'indice di NL e necessariemente <= a ~
   3. III = I  
      supponiamo che NL abbia un'indice finito, allora possiamo considerare un automa A = <Q, Σ, δ, q0, Fi> 
         - Q = classi di equivalenza di NL 
         - la funzione di transizione δ è definita nel modo seguente:
         Siano q ∈ Q e a ∈ Σ. Poiché q è una classe dell’equivalenza NL, tutte le parole  
         ua con u ∈ q appartengono a un’unica classe p dell’equivalenza NL. Poniamo  
         allora δ(q, a) = p
         - lo stato iniziale qo e la classe d'equivalenza della parola vuota,
         - gli stati finali sono le classi d'equivalenza inculse in L  
 
      dato che q0 contiene la parola vuota si verifica facilmente che per ogni parola l'insieme dellee funzioni di transizione contioene quella parola 
      
      Ricordando che L è unione di classi dell’equivalenza NL, ne segue che se w ∈ L,  
      allora δb(q0, w) è incluso in L e quindi è uno stato finale, mentre se w ∈/ L, allora δb(q0, w)  
      è incluso nel complemento di L e quindi non è uno stato finale. Se ne conclude che il  
      linguaggio riconosciuto da A è proprio L.  
      

## 7 per passare da automa a espressione regolare? metodo di minimizzazione (formula)
Per utilizzare il metodo della minimizzazione dobbiamo creare 2 stati nuovi, uno per lo stato iniziale e uno per tutti gli stati finali e li colleghi tramite una Epsilon, Una volta creati si devono eliminare gli stati piu piccolo fino a rimanere solo con gli ultimi 2 stati creati pocanzi

Come si eliminano?  
si eliminano rendirizzando tutti gli stati dello stato che si vuole eliminare in uno stato adiacente con il collegamento che rappresenta l'espressione regolare della transizione eliminata.

## 8 automa a pila
L'automa a pila e un particolare automa che riconosce i linguaggi non contestuali, ed e una settupla con

Q = Insieme finito di stati
A = Sigma (Alfabeto)  
Gamma = Alfabeto della pila  
Delta = Funzioni di transizioni   
qo = Stato iniziale
Z0 = Stato iniziale Della Pila
F = Insime di stati finali


come funziona?
ad ogni passo l'automa legge dal nastro di lettura e preleva tramite l'operazione **pop**  l'ultimo elemento della pila, dopo aver preso l'input si calcolo il nuovo stato e pusha i nuovi valori all'iterno della pila in LIFO 



## 9 linguaggi della gerarchia di chomsky accettati dall'automa a pila?
I linguaggi accettati dalla pila sono i linguaggi NON contestuali dunque nella gerarchia di chomsky sono i linguaggi di tipo 2 dunque i linguaggi non contestuali  
Descrizione: Le prooduzioni delle grammatiche non contestuali sono delle forma A -> Gamma dove A e unsingolo non terminale e Y e qualsiesi stringa terminale o non terminale.

## 10 Gerarchia di chomsky

Grammatica tipo 0 Grammatiche non vincolati
Queste grammatiche sono le piu generali e non sono viincolate da nulla

Grammatica tipo 1 Grammatiche Sensibili al contesto
Le produzioni devono avere la forma aAb -> aYb dopve A e un singolo non terminale e Y non e una striga vuota, il non terminale A puo essere sostituito da Y solo se e contornato da a e b 

Grammatica tipo 2 Grammatiche Non contestuali
Descrizione: Le prooduzioni delle grammatiche non contestuali sono delle forma A -> Gamma dove A e unsingolo non terminale e Y e qualsiesi stringa terminale o non terminale.  

Grammatica tipo 3  Grammatiche regolari 
sono grammatiche piu semplici e hanno una forma A -> aB o A -> a e sono definiti dagli automi a stati finiti 

## 11 parsing top-down (con automa a pila)

consiste nel creare un'automa a pila che partendo da S costruisca l'albero di derivazione di una data parola, se alla fine si arriva che la stringa dell'input e vuota e la pila e vuota allora la computazione ha avuto successo, altrimenti se l'input e vuoto ma la pila no, o se hai raggiunto uno stato in cui non ci sono transizioni valide, il parsing fallisce. 

## 12 come si definisce il linguiaggio generato dalla grammatica?
Dipende da che tipo di grammatica viene generata, 

Guardare chomsky

## 13 Definizione di lettera. Conseguenza diretta e conseguenza non diretta
una lettera e un elemento di un'alfabeto, che e un'insieme finito di insieme per comporre delle parole e si indica con SIGMA, e quindi un elemento es Sigma = {a,b,c} dove a, b e c sono lettere dell'alfabeto

**conseguenza diretta**  
una conseguenza diretta e una proposizione "un prezzo di frase o lettera" che puo essere derivata tramite l'utilizzo di una sola produzione es S -> ab  
**conseguenza indiretta**   
una conseguenza indiretta e una proposizione "un prezzo di frase o lettera" che non puo essere derivata tramite l'utilizzi di una sola produzione es S -> Ab A -> a Parola = ab

## 14 forma sentenziale conseguenza di quale variabile?
La forma sentenziale di una grammatica e qualsiesi conseguenza della variabile S ES:  
S -> {aSb | ab | bB} B -> b  
ed e denotato con il simbolo S(g)


## 15 le parole generate dalla grammatica quali forme sentenziali ci sono? che caratteristiche devono avere?
Le parole generate della grammatica devono contenere solo simboli terminali ed essere generate da partire dal simbolo S, dunque l'insieme delle parole generata dalla grammatica sarà denotato come linguaggio della grammatica L(G) = S(G) Intersecato Sigma* 


## 16 lemma di iterazione per linguaggi regolari. Perche in un linguaggio regolare tutte le parole abbastanza lunghe hanno una parte ....? 
il lemma di iterazioane dice che: se L e un linguaggio regolare esiste un intero n : w appartenente L di lunghezza |w| >= n si puo fattorizzare in w = xyz con y != Epsilon & xy*z contenuto = L

puo essere usato per capire se il linguaggio e regolare

**Dimostrazione**
per il teorema di kleene L e riconosciuto da un automa A, poniamo n = Card(Q), sia w una parola di lunghezza k > n e scriviamo w = a1, a2.. ak poniamo poi   
q1 = δ(q0, a1), q2 = δ(q1, a2), . . . , qk = δ(qk−1 , ak ). 

Poiché w ∈ L, si ha qk = δ(q0, w) ∈ F. Dato che k ≥ n, esisteranno degli indici i, j con 0 ≤ i < j ≤ k tali che qi = qj. Poniamo ora  

x = a1a2 · · · ai , y = ai+1ai+2 · · · aj , z = aj+1aj+2 · · · ak.  

Abbiamo quindi w = xyz e y != ε. Per completare la dimostrazione basta quindi far vedere che xy∗ z ⊆ L. Dalla (11) si ottiene facilmente che    

δ(q0, x) = qi, δ(qi, y) = qj = qi, δ(qj, z) = qk.

Ne segue che per ogni m ≥ 0,

δb(q0, xymz) = δb(qi, ymz) = δb(qi, z) = δb(qj, z) = qk ∈ F.  (δb = delta cappuccio che e l'insieme delle transizioni)

Conseguentemente, xymz ∈ L. Questo dimostra che xy∗
z ⊆ L.


## 17 lemma di iterazione per linguaggi non contestuali
Sia L un linguaggio non contestuale. Esiste un intero n tale che
ogni parola w ∈ L di lunghezza |w| > n si può fattorizzare w = xuyvz con uv != ε e xu^k yv^k z ∈ L per ogni k ≥ 0.

Sia G = {V, Σ, P, S} una grammatica non contestuale che genera L.
G e priva di epsilon produzioni e produzioni unarie, e l'albero di derivazione di w avrà 2 nodi etichettati con la stessa variabile A
![alt text](siummando.png)
Avremo quindi

S ∗=⇒ xAz , A ∗=⇒ uAv , A ∗=⇒ y,  
per opportuni x, u, y, v, z ∈ Σ∗ tali che w = xuyvz e uv 6= ε, poiché la grammatica è priva di ε-produzioni e produzioni unarie. Ne segue

e quindi xu^k yv^kz ∈ L

## 18 automa a stati finiti cos'è? come si rappresente con un grafo? parole accettate?
un automa a stati finiti e una quintupla
A = (Q, Σ, δ, q0, F)

Q = insieme degli stati  
Σ = alfabeto  
δ = Funzioni di transizioni  
q0 = stato iniziale  
F = stati finali  

con un grafo si rappresenta tramite lutilizzo di cerchietti che sono gli stati, e con le freccie etichettate che vanno in altri stati

le parole accettate sono tutte quelle che terminano in uno stato finale dell'automa

## 19 in un grafo i cammini molto lunghi (molti vertici) che proprietà hanno? 
non possono essere semplici, ovvero passano due volte per lo stesso vertice. 

**Che cambia se ci passo piu volte?**
(in questo modo ha dimostrato il lemma)  
vedi dimostrazione lemma iterazione

## 20 cos'è un albero di derivazione in una grammatica non contestuale?
Un albero di derivazione (o albero sintattico) è una rappresentazione grafica che mostra come una stringa può essere generata da una grammatica formale. In particolare, per una grammatica non contestuale (o grammatica context-free, GNC), un albero di derivazione visualizza la struttura gerarchica delle applicazioni delle regole di produzione.

## 21 algoritmo di cock-kasami-yunger
Sia G = {V, Σ, P, S} una grammatica non-contestuale. Vogliamo considerare i due
seguenti problemi:
1. Ricognizione: Data una parola w ∈ Σ∗, decidere se w ∈ L(G).
2. Parsing: Data una parola w ∈ L(G) costruire un albero di derivazionedi G associato a w.

o poi fare le tabelle.
![alt text](Immaginona.png)

## 22 grammatica in forma normale di chomsky. se la variabile deriva Produzione X cosa significa? come deve essere per farle diventare produzioni normali di chomsky

una grammatica in forma normale di chomsky e una grammatica senza epsilon produzioni e produzioni unarie, e le qui uniche produzioni sono solamente nella forma X -> AA e A -> a   
S -> epsilon se e solo se S non compare nei lati destri delle produzioni

# 23 e parola e lunga albero di derivazione e molto alto. Cosa succede se l'albero di derivazione e molto alto? 
che ci saranno tante foglie alla sua discesaa troverò una o piu variabili che si ripeteranno piu volte.

