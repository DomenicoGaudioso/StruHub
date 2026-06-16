---
layout: post
title: "La ripartizione trasversale dei carichi negli impalcati da ponte"
description: "Riferimento tecnico StruHub con caso numerico su impalcato a cinque travi, schema di ripartizione e riferimenti normativi tracciabili."
date: 2026-01-23
order: 23
permalink: /posts/la-ripartizione-trasversale-dei-carichi.html
meta: "Quaderno tecnico - 1840 parole circa"
---

![Vista di un impalcato pluritrave dove la posizione della corsia cambia la domanda sulla singola trave]({{ site.baseurl }}/assets/images/la-ripartizione-trasversale-dei-carichi-01.jpg)

**Non si divide per il numero di travi**

Nel progetto di un impalcato pluritrave il passaggio piu delicato non e quasi mai ottenere il momento globale lungo la campata. Quello, con un modello longitudinale pulito e i carichi da traffico correttamente disposti, si ottiene abbastanza rapidamente. Il problema vero e un altro: capire quanta parte di quell'effetto globale entra davvero nella trave di bordo, nella trave interna o nella sezione che poi verra verificata a flessione, taglio, fatica o tensione.

Se si salta questo passaggio e si divide il momento totale per il numero di travi, il risultato puo essere ordinato ma meccanicamente sbagliato. La ripartizione trasversale dipende da eccentricita del carico, rigidezza della soletta, rigidezza dei traversi, torsione delle travi principali, sbalzi laterali e regolarita della sezione trasversale. Per questo il coefficiente di ripartizione non e un numero decorativo di tabella: e il ponte tra il modello globale e la verifica locale.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle "Norme tecniche per le costruzioni"*, come modificato dal D.M. 9 marzo 2023.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, istruzioni applicative delle NTC 2018.
- UNI EN 1990:2006, basi della progettazione strutturale.
- UNI EN 1991-2:2005, azioni da traffico sui ponti.
- E. C. Hambly, *Bridge Deck Behaviour*, 2nd edition.
- JRC European Commission, *Bridge Design to Eurocodes - Worked Examples*.

Nota StruHub.

Nella pratica conviene tenere separati tre livelli: modello longitudinale per l'effetto globale, modello trasversale per la diffusione sulle travi e verifica locale della sezione governante. E' la stessa logica utile quando si imposta un benchmark rapido nello spirito di `beamfeapy`: il valore del tool non e "dare un coefficiente", ma costringere a lasciare traccia di interassi, rigidezze, eccentricita e controlli di equilibrio.

![Schema della ripartizione trasversale su un impalcato a cinque travi con confronto tra media uniforme, Courbon e benchmark graticcio]({{ site.baseurl }}/assets/images/la-ripartizione-trasversale-dei-carichi-schema.svg)

**Che cosa stai ripartendo davvero**

Nel caso piu comune, il modello longitudinale dell'impalcato fornisce un effetto globale lungo la luce, ad esempio il momento flettente in mezzeria o il taglio in prossimita dell'appoggio, gia determinato secondo i carichi di traffico prescritti da NTC 2018 e UNI EN 1991-2. Quel risultato non e ancora la sollecitazione della singola trave. Il primo passaggio corretto e scrivere:

$$
M_{medio}(x)=\frac{M_{tot}(x)}{n}
$$

dove $n$ e il numero delle travi principali. La ripartizione trasversale entra solo dopo:

$$
M_j(x)=\eta_j(x,e)\,M_{medio}(x)
$$

con la condizione di coerenza:

$$
\sum_{j=1}^{n}\eta_j(x,e)=n
$$

Se si preferisce ragionare in quote percentuali, si puo usare:

$$
w_j(x,e)=\frac{\eta_j(x,e)}{n}, \qquad \sum_{j=1}^{n} w_j(x,e)=1
$$

Il punto importante e questo: il coefficiente $\eta_j$ non nasce dal solo interasse tra le travi. Nasce dalla combinazione tra geometria, rigidezze e posizione trasversale del carico. Due impalcati con identica larghezza ma con soletta, traversi o sbalzi diversi possono portare coefficienti molto differenti.

**Le variabili che spostano davvero il risultato**

Per leggere con senso ingegneristico una ripartizione trasversale conviene controllare almeno questi fattori:

- posizione reale della corsia o della linea di carico rispetto all'asse dell'impalcato;
- rigidezza flessionale delle travi longitudinali e differenze tra travi di bordo e interne;
- rigidezza trasversale efficace di soletta, traversi e diaframmi;
- contributo torsionale delle travi, soprattutto per cassoni o travi molto aperte;
- ampiezza e rigidezza degli sbalzi laterali;
- regolarita della sezione trasversale lungo la campata;
- presenza di skew, curvatura o appoggi non allineati.

Qui sta anche il limite dei coefficienti presi da esempi standard: sono utili come confronto, non come sostituto del modello. La normativa ti impone di definire correttamente azioni e schema resistente; non ti consegna un coefficiente universale valido per ogni impalcato.

**Due controlli minimi che evitano errori grossolani**

Prima ancora del caso numerico, ci sono due verifiche che dovrebbero comparire in ogni relazione seria:

1. equilibrio: la somma delle quote ripartite deve ricostruire l'effetto globale;
2. simmetria: con carico centrato e impalcato simmetrico, le travi simmetriche devono restituire lo stesso coefficiente.

Se uno di questi due controlli fallisce, il problema non e raffinabile con un grafico migliore: il modello e da rivedere.

**Caso numerico: impalcato prefabbricato a cinque travi**

Consideriamo un impalcato da ponte stradale semplicemente appoggiato, luce di calcolo:

$$
L=32.0\,\mathrm{m}
$$

La sezione trasversale e composta da cinque travi principali in c.a.p. con interasse costante:

$$
s=2.70\,\mathrm{m}
$$

e sbalzi laterali pari a:

$$
b_{sb}=1.20\,\mathrm{m}
$$

Le ascisse delle travi rispetto all'asse dell'impalcato sono quindi:

$$
y_j=\{-5.40,\,-2.70,\,0,\,+2.70,\,+5.40\}\,\mathrm{m}
$$

Dal modello longitudinale dell'impalcato, gia caricato secondo la corsia governante di UNI EN 1991-2, assumiamo in mezzeria un momento totale di inviluppo:

$$
M_{tot}=5100\,\mathrm{kNm}
$$

La media per trave sarebbe:

$$
M_{medio}=\frac{5100}{5}=1020\,\mathrm{kNm}
$$

Questa media serve solo come base di confronto. Il carico trasversale equivalente, dopo la diffusione locale nella soletta, e assunto con linea di azione a:

$$
e=-2.60\,\mathrm{m}
$$

rispetto all'asse dell'impalcato. Il segno negativo indica una corsia che grava sul lato della trave di bordo sinistra.

**Controllo rapido con la formula di Courbon**

Per un primo check manuale, con traversi sufficientemente efficaci e travi di rigidezza comparabile, si puo usare la classica forma di Courbon:

$$
\eta_j=1+\frac{n\,e\,y_j}{\sum_i y_i^2}
$$

Nel nostro caso:

$$
\sum_i y_i^2=72.9\,\mathrm{m^2}
$$

e quindi si ottiene:

| Trave | $y_j$ [m] | $\eta_j$ Courbon | $M_j$ [kNm] |
| --- | ---: | ---: | ---: |
| G1 bordo sinistro | -5.40 | 1.963 | 2002 |
| G2 | -2.70 | 1.481 | 1511 |
| G3 | 0.00 | 1.000 | 1020 |
| G4 | +2.70 | 0.519 | 529 |
| G5 bordo destro | +5.40 | 0.037 | 38 |

La somma dei coefficienti vale 5.000, quindi il controllo di equilibrio e rispettato. Il risultato dice una cosa molto concreta: se si verificasse la trave G1 con la sola media uniforme, si passerebbe da $1020\,\mathrm{kNm}$ a circa $2000\,\mathrm{kNm}$, cioe si sottostimerebbe quasi della meta la domanda sulla trave di bordo piu caricata.

Ma Courbon, da solo, non basta sempre. Il metodo e utile come controllo veloce, ma tende a irrigidire troppo il comportamento trasversale quando soletta e traversi diffondono il carico in modo piu graduale.

**Benchmark rapido di graticcio**

Per lo stesso impalcato, un benchmark numerico leggero con cinque travi longitudinali equivalenti, sei telai trasversali e una rigidezza distribuita della soletta condensata in elementi trasversali porta a coefficienti piu smussati. E' il tipo di confronto che conviene rifare in una libreria snella come `beamfeapy` oppure in un graticcio FEM essenziale prima di congelare la verifica finale.

Assumiamo per il benchmark i seguenti coefficienti:

$$
\eta_j^{gr}=\{1.72,\ 1.37,\ 1.02,\ 0.63,\ 0.26\}
$$

Anche in questo caso:

$$
\sum_j \eta_j^{gr}=5.00
$$

I momenti di progetto sulle singole travi diventano:

| Trave | $\eta_j$ uniforme | $\eta_j$ Courbon | $\eta_j$ benchmark | $M_j$ benchmark [kNm] |
| --- | ---: | ---: | ---: | ---: |
| G1 bordo sinistro | 1.00 | 1.963 | 1.72 | 1754 |
| G2 | 1.00 | 1.481 | 1.37 | 1397 |
| G3 | 1.00 | 1.000 | 1.02 | 1040 |
| G4 | 1.00 | 0.519 | 0.63 | 643 |
| G5 bordo destro | 1.00 | 0.037 | 0.26 | 265 |

Questo confronto e professionale perche separa i ruoli dei modelli:

- la media uniforme e solo una base contabile, non una verifica affidabile;
- Courbon fornisce un controllo a mano immediato e spesso cautelativo;
- il benchmark di graticcio restituisce una distribuzione piu plausibile quando la soletta e i traversi hanno davvero un ruolo collaborante.

Nel caso G1 la ripartizione uniforme sottostima il momento della trave governante di circa il 72% rispetto al benchmark. Courbon, invece, resta conservativo di circa il 14%. Questo e esattamente il tipo di informazione che serve in una relazione: non un coefficiente astratto, ma un intervallo tecnico credibile entro cui giudicare la domanda della singola trave.

![Impalcato a travi: la corsia eccentrica carica in modo molto diverso bordo e mezzeria]({{ site.baseurl }}/assets/images/la-ripartizione-trasversale-dei-carichi-02.jpg)

**Dove il metodo semplificato smette di bastare**

Una ripartizione tabellare o semianalitica puo diventare debole quando il ponte esce dal caso regolare. I punti che meritano un salto di modello sono almeno questi:

- impalcati in curva o con skew marcato;
- travi con rigidezze molto diverse tra bordo e interno;
- diaframmi irregolari o non efficaci lungo tutta la campata;
- cassoni con forte rigidezza torsionale;
- sezioni variabili lungo la luce;
- contemporanea necessita di leggere momento, taglio, torsione e deformata.

In questi casi il coefficiente non va abolito, ma retrocesso a strumento di controllo. Il modello principale deve diventare un graticcio o un FEM in grado di restituire superfici di influenza e sollecitazioni locali in modo coerente con lo schema resistente reale.

**Checklist minima da relazione tecnica**

Quando la ripartizione trasversale entra in una relazione di progetto o verifica, conviene rendere espliciti almeno questi sei punti:

1. geometria trasversale completa: numero di travi, interassi, sbalzi, traversi, larghezza carrabile;
2. origine del momento o del taglio globale da ripartire, con indicazione della combinazione governante;
3. ipotesi con cui e stata definita l'eccentricita del carico trasversale;
4. metodo usato per la distribuzione: tabella, Courbon, Guyon-Massonnet-Bares, graticcio o FEM;
5. controllo di equilibrio dei coefficienti e, se possibile, confronto con un benchmark indipendente;
6. effetto finale sulla trave o sezione governante, non solo il coefficiente in se.

Se manca uno di questi passaggi, il risultato e difficile da revisionare. Un altro progettista deve poter rifare il conto a mano almeno come ordine di grandezza, capire quale corsia governa e vedere perche la trave G1 e critica piu della G3 o viceversa.

**Dal coefficiente alla verifica vera**

Il coefficiente di ripartizione non e la verifica finale. Una volta determinato il momento nella singola trave, bisogna ancora passare alla sezione resistente, alle combinazioni, all'eventuale comportamento composto, alla verifica di appoggi, traversi e diaframmi. Qui si vede la differenza tra un post divulgativo e un riferimento utile per chi progetta davvero: non fermarsi al numero, ma mostrare la catena completa che porta dal traffico alla fibra.

Per questo, in un flusso di lavoro serio, la ripartizione trasversale conviene usarla in tre modi diversi:

- come stima preliminare per capire subito quale trave potrebbe governare;
- come controllo manuale indipendente del graticcio o del FEM;
- come traccia tecnica da archiviare nel report, con coefficienti, segni ed eccentricita dichiarati.

E' qui che un piccolo benchmark ripetibile, anche costruito con uno script leggero nello spirito StruHub o con una libreria come `beamfeapy`, diventa piu utile di una schermata piena di numeri: obbliga a conservare la logica del modello e non solo il suo output.

**In sintesi**

La ripartizione trasversale dei carichi negli impalcati da ponte non e un accessorio del calcolo, ma il punto in cui il carico da traffico smette di essere "dell'impalcato" e diventa della singola trave da verificare. Se questo passaggio e trattato bene, il progettista sa difendere il risultato e capire quando una trave di bordo governa davvero. Se e trattato male, anche un buon modello longitudinale produce verifiche locali deboli.

La regola pratica e semplice: mai fermarsi alla media per trave, usare sempre almeno un controllo di equilibrio, e portare in relazione un confronto leggibile tra metodo rapido e modello piu rappresentativo. E' questo che trasforma una ripartizione trasversale da formula scollegata a decisione strutturale difendibile.

Riferimenti normativi e bibliografici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle "Norme tecniche per le costruzioni"*, Ministero delle Infrastrutture e dei Trasporti, G.U. n. 42 del 20 febbraio 2018, S.O. n. 8, come modificato dal D.M. 9 marzo 2023, G.U. n. 69 del 22 marzo 2023.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle "Norme tecniche per le costruzioni" di cui al decreto ministeriale 17 gennaio 2018*, G.U. n. 35 del 11 febbraio 2019, S.O. n. 5.
- UNI EN 1990:2006, *Eurocodice - Basi della progettazione strutturale*.
- UNI EN 1991-2:2005, *Eurocodice 1 - Azioni sulle strutture - Parte 2: Carichi da traffico sui ponti*.
- Hambly, E. C., *Bridge Deck Behaviour*, 2nd edition, Chapman and Hall.
- Joint Research Centre, European Commission, *Bridge Design to Eurocodes - Worked Examples*.
