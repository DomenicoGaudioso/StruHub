---
layout: post
title: "Modellazione semplificata di pile da ponte come sistemi a un grado di liberta"
description: "Riferimento tecnico StruHub con schema meccanico, caso numerico realistico e riferimenti normativi tracciabili."
date: 2026-01-12
order: 12
permalink: /posts/pile-sistema-un-grado-liberta.html
meta: "Quaderno tecnico - 1810 parole circa"
---

![Schema di una pila da ponte ricondotta a oscillatore equivalente con massa concentrata in testa]({{ site.baseurl }}/assets/images/pile-sistema-un-grado-liberta-schema.svg)

**Perche un modello SDOF resta utile anche quando il FEM e gia disponibile**

Ridurre una pila da ponte a un sistema a un grado di liberta non significa banalizzare il problema. Significa costruire un controllo indipendente e leggibile di tre grandezze che governano quasi tutto il resto: rigidezza laterale, massa efficace e periodo fondamentale. Se queste tre quantita non hanno ordini di grandezza plausibili, anche un modello FEM molto dettagliato rischia di diventare una scatola nera.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme tecniche per le costruzioni*, con richiamo ai Capitoli 3, 5 e 7.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, istruzioni applicative delle NTC 2018.
- UNI EN 1998-2:2011, *Eurocodice 8 - Progettazione delle strutture per la resistenza sismica - Parte 2: Ponti*.
- UNI EN 1991-2:2005, *Eurocodice 1 - Azioni sulle strutture - Parte 2: Carichi da traffico sui ponti*.

Nota StruHub.

In un workflow serio il modello SDOF non sostituisce analisi modale, spettro di risposta o time-history: serve a controllarle. E la stessa logica che torna utile in una pipeline StruHub o in un benchmark rapido con `beamfeapy`, che gestisce analisi statiche e modali di telai 3D e permette di confrontare in pochi minuti periodo, massa partecipante e forma deformata con il controllo manuale.

**Dal continuo al modello equivalente**

La pila reale e un elemento continuo con massa distribuita, sezione eventualmente fessurata, vincolo al piede non perfettamente incastrato e interazione con impalcato, appoggi e fondazione. Il modello SDOF concentra invece il problema in una coordinata generalizzata $u(t)$:

$$
M^* \ddot{u}(t) + C^* \dot{u}(t) + K^* u(t) = F^*(t)
$$

dove:

- $M^*$ e la massa generalizzata;
- $C^*$ rappresenta lo smorzamento equivalente;
- $K^*$ e la rigidezza laterale equivalente;
- $F^*(t)$ e la forza generalizzata associata alla forma modale assunta.

Se la pila e assimilabile in prima approssimazione a una mensola prevalente nel primo modo, la rigidezza elastica laterale si puo scrivere come:

$$
k = \frac{3EI_{eff}}{H^3}
$$

con $H$ altezza libera della pila ed $EI_{eff}$ rigidezza flessionale efficace nella direzione considerata. Il termine chiave non e $EI$ teorico a sezione integra, ma $EI_{eff}$, cioe la rigidezza che descrive la risposta realmente mobilitata nel livello di analisi che si sta svolgendo.

**La rigidezza efficace non e un dettaglio**

L'errore piu frequente nei controlli preliminari sta qui: usare l'inerzia lorda senza chiedersi se la pila lavorera in campo fessurato gia per livelli modesti di domanda. Per una pila in c.a. il controllo manuale e utile solo se la rigidezza e coerente con:

- direzione di sisma o di spinta considerata;
- geometria reale della sezione;
- presenza di fessurazione o confinamento;
- eventuale cedevolezza di appoggi, fondazione o plinto.

In pratica conviene distinguere tre livelli:

1. $EI_g$, rigidezza geometrica lorda, utile solo per un primissimo screening.
2. $EI_{eff}$, rigidezza efficace di esercizio o di predimensionamento, adatta al confronto con il FEM lineare.
3. $EI_{sec}$, rigidezza secante legata a un certo livello di domanda, utile quando si entra in ambito non lineare.

Se il periodo ottenuto con $EI_g$ e vicino a quello del FEM ma il modello numerico usa una sezione gia fessurata, la coincidenza e solo apparente.

**Massa efficace: non tutta la massa pesa allo stesso modo**

Per una pila con impalcato tributario concentrato in testa e massa distribuita lungo il fusto, una stima pratica della massa equivalente del primo modo e:

$$
m_{eq} \approx m_{testa} + 0.236 \, m_{fusto}
$$

dove il coefficiente $0.236$ deriva dalla partecipazione del primo modo di una mensola uniforme. Questa scrittura e molto piu utile del semplice "metto tutta la massa in testa", perche evita sia sovrastime sia sottostime del periodo.

Il periodo fondamentale diventa quindi:

$$
T_1 = 2 \pi \sqrt{\frac{m_{eq}}{k}}
= 2 \pi \sqrt{\frac{m_{eq} H^3}{3EI_{eff}}}
$$

Se si vuole passare a una forma modale piu rigorosa, il quadro generale e:

$$
M^* = \int_0^H m(z)\phi^2(z)\,dz + \sum_i m_i \phi^2(z_i)
$$

$$
\Gamma = \frac{\int_0^H m(z)\phi(z)\,dz + \sum_i m_i \phi(z_i)}{M^*}
$$

Il tecnico non ha sempre bisogno di calcolare integralmente $\Gamma$, ma deve sapere se sta assumendo una massa nodale, una massa distribuita o una combinazione delle due.

**Caso numerico: pila di viadotto stradale in c.a.**

Si consideri una pila singola di un viadotto con impalcato prefabbricato a travi affiancate. L'obiettivo non e sostituire il modello globale del ponte, ma verificare se il primo ordine di grandezza di periodo e taglio sismico e compatibile con il modello completo.

Dati geometrici e meccanici assunti per la direzione trasversale:

- altezza libera della pila tra plinto e pulvino: $H = 10.50\,\mathrm{m}$;
- rigidezza flessionale efficace della sezione fessurata nella direzione trasversale: $EI_{eff} = 27.5 \cdot 10^9\,\mathrm{N\,m^2}$;
- massa tributaria di impalcato, traverso e apparecchi di appoggio concentrata in testa: $m_{testa} = 970\,\mathrm{t}$;
- massa del solo fusto della pila: $m_{fusto} = 176\,\mathrm{t}$;
- smorzamento viscoso equivalente per controllo lineare: $\xi = 5\%$.

La massa equivalente del primo modo vale:

$$
m_{eq} = 970 + 0.236 \cdot 176 = 1011.5\,\mathrm{t}
$$

cioe:

$$
m_{eq} = 1.0115 \cdot 10^6\,\mathrm{kg}
$$

La rigidezza laterale equivalente della mensola e:

$$
k = \frac{3EI_{eff}}{H^3}
= \frac{3 \cdot 27.5 \cdot 10^9}{10.5^3}
= 71.3 \cdot 10^6\,\mathrm{N/m}
$$

Il periodo fondamentale stimato risulta:

$$
T_1 = 2 \pi \sqrt{\frac{1.0115 \cdot 10^6}{71.3 \cdot 10^6}}
= 0.75\,\mathrm{s}
$$

Supponendo che dall'analisi spettrale preliminare del sito risulti, alla stessa percentuale di smorzamento, un'ordinata di progetto:

$$
S_d(T_1) = 0.32 g
$$

la forza orizzontale equivalente alla testa della pila e:

$$
F_h = m_{eq} \, S_d(T_1)\, g
= 1.0115 \cdot 10^6 \cdot 0.32 \cdot 9.81
= 3.17 \cdot 10^6\,\mathrm{N}
$$

quindi:

$$
F_h = 3.17\,\mathrm{MN}
$$

Il momento al piede associato al modello equivalente e:

$$
M_b = F_h H = 3.17 \cdot 10.50 = 33.3\,\mathrm{MN\,m}
$$

Lo spostamento elastico di testa, coerente con il modello lineare, diventa:

$$
u_{top} = \frac{F_h}{k}
= \frac{3.17 \cdot 10^6}{71.3 \cdot 10^6}
= 0.044\,\mathrm{m}
$$

cioe circa:

$$
u_{top} = 44\,\mathrm{mm}
$$

Gia questa catena fornisce un controllo completo: massa, periodo, taglio, momento e spostamento. Se uno di questi valori fosse incompatibile con la geometria dell'opera, il problema andrebbe cercato prima nel modello e poi nel solutore.

**Confronto con il FEM: un vero uso professionale del controllo SDOF**

Supponiamo ora che un modello FEM della stessa pila, costruito con un'asta equivalente e masse distribuite, restituisca:

- primo periodo trasversale $T_{FEM} = 0.79\,\mathrm{s}$;
- massa partecipante del primo modo nella stessa direzione pari all'88%;
- taglio alla base da analisi spettrale lineare $V_{b,FEM} = 3.34\,\mathrm{MN}$.

Il confronto sul periodo vale:

$$
\Delta_T = \frac{0.79 - 0.75}{0.75} = 0.053
$$

quindi circa il $5.3\%$. Il rapporto tra taglio FEM e controllo semplificato e:

$$
r_V = \frac{3.34}{3.17} = 1.05
$$

Uno scarto del 5% sia su periodo sia su taglio e esattamente il tipo di risultato che rende il modello SDOF utile: non "indovina" il FEM al millimetro, ma conferma che la rappresentazione numerica sta descrivendo lo stesso sistema meccanico.

In questa fase il controllo diventa realmente professionale quando si fanno almeno tre domande:

- il FEM usa la stessa rigidezza efficace o una sezione lorda non fessurata?
- il vincolo al piede e davvero un incastro o include molle di fondazione?
- la massa tributaria dell'impalcato e stata assegnata in modo coerente alla pila considerata?

Se la differenza sul periodo supera il 15-20%, quasi sempre la causa non e "la complessita del ponte", ma una differenza nelle ipotesi meccaniche di base.

**Dove il modello a un grado di liberta inizia a perdere affidabilita**

Il controllo SDOF funziona bene finche il primo modo domina davvero la risposta e la pila si comporta come una mensola prevalente. Ci sono almeno sei casi in cui conviene passare rapidamente a un modello piu ricco:

- pile molto basse e tozze, dove taglio e deformazione di taglio non sono trascurabili;
- pile molto alte o telai di pile, in cui piu modi partecipano alla risposta;
- impalcati continui con forte interazione tra pile adiacenti;
- presenza di isolamento sismico, dissipatori o appoggi fortemente cedevoli;
- fondazioni profonde o plinti flessibili che rendono non trascurabile la rotazione al piede;
- verifiche non lineari in cui rigidezza e smorzamento cambiano con la domanda.

Nel caso di fondazioni cedevoli, la correzione minima consiste nel riconoscere che la deformabilita laterale totale e somma di contributi:

$$
u_{tot} = u_{pile} + u_{fond} + u_{appoggi}
$$

e quindi:

$$
\frac{1}{k_{tot}} = \frac{1}{k_{pile}} + \frac{1}{k_{fond}} + \frac{1}{k_{appoggi}}
$$

Trascurare questi termini porta quasi sempre a sottostimare il periodo.

**Un caso reale di verifica preliminare che evita un errore di modellazione**

In pratica professionale questo controllo viene spesso usato prima di un'analisi piu spinta. Un caso tipico e il seguente: il modello globale del ponte restituisce un periodo trasversale di circa $1.05\,\mathrm{s}$, mentre il controllo manuale della sola pila porta a $0.72$-$0.78\,\mathrm{s}$. La differenza non va archiviata come "effetto del modello 3D".

Prima di accettarla conviene controllare:

- se le masse di piu campate sono state tributate alla stessa pila;
- se i link degli apparecchi di appoggio hanno rigidezza troppo bassa;
- se i vincoli alla base includono rotazioni libere non volute;
- se l'inerzia della sezione e stata assegnata nelle direzioni giuste;
- se il peso proprio e stato trasformato in massa con unita coerenti.

In un caso di questo tipo basta spesso correggere la massa o la cedevolezza di un appoggio per riportare il periodo nella fascia attesa. Il valore del modello SDOF non e produrre il numero finale di progetto, ma intercettare queste incoerenze prima che inquinino tutto il resto: spettro, combinazioni, verifiche allo SLV e valutazioni di duttilita.

**Che cosa conviene riportare in una relazione tecnica**

Per rendere tracciabile il controllo non basta scrivere "la pila e stata assimilata a un oscillatore". In relazione conviene dichiarare almeno:

1. geometria della pila e direzione del controllo;
2. criterio con cui e stato scelto $EI_{eff}$;
3. massa tributaria di impalcato e coefficiente adottato per la massa del fusto;
4. valore di smorzamento e tipo di spettro usato;
5. periodo, taglio, momento e spostamento risultanti;
6. confronto con il FEM o con il modello globale del ponte.

Questa traccia e coerente con l'impostazione delle NTC 2018 per azioni e ponti, con le istruzioni della Circolare 2019 e con la logica dell'Eurocodice 8 Parte 2: il problema non e soltanto calcolare, ma dimostrare che il modello scelto rappresenta correttamente il comportamento della pila nella direzione studiata.

**Dal numero alla decisione**

Una pila modellata come SDOF non e un esercizio accademico. E un filtro di qualita sul modello complessivo. Se il controllo manuale produce $T_1$, $V_b$ e $u_{top}$ compatibili con il FEM, il progettista puo passare con piu fiducia alle analisi successive. Se invece i numeri divergono, il vantaggio non e aver trovato una formula "alternativa", ma aver scoperto che qualcosa nel modello strutturale va chiarito.

Per questo il modello equivalente resta utile anche in ambienti digitali avanzati: costringe a separare massa, rigidezza, vincoli e domanda sismica. Ed e proprio questa separazione che rende il risultato difendibile davanti a un controllo tecnico serio.

Riferimenti normativi e bibliografici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle "Norme tecniche per le costruzioni"*, Ministero delle Infrastrutture e dei Trasporti, G.U. n. 42 del 20 febbraio 2018, S.O. n. 8.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle "Norme tecniche per le costruzioni" di cui al decreto ministeriale 17 gennaio 2018*, G.U. n. 35 dell'11 febbraio 2019, S.O. n. 5.
- UNI EN 1991-2:2005, *Eurocodice 1 - Azioni sulle strutture - Parte 2: Carichi da traffico sui ponti*.
- UNI EN 1998-2:2011, *Eurocodice 8 - Progettazione delle strutture per la resistenza sismica - Parte 2: Ponti*.
- Priestley, M. J. N., Seible, F., Calvi, G. M., *Seismic Design and Retrofit of Bridges*, John Wiley and Sons.
- Chopra, A. K., *Dynamics of Structures: Theory and Applications to Earthquake Engineering*, Pearson.
