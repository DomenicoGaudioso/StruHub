---
layout: post
title: "Monitorare lo spostamento di una trave tramite inclinometri"
description: "Riferimento tecnico StruHub con schema di misura, caso numerico e riferimenti normativi tracciabili."
date: 2026-01-25
order: 25
permalink: /posts/monitorare-lo-spostamento-di-una.html
meta: "Quaderno tecnico - 1620 parole circa"
---

![Campata da ponte monitorata con sensori di rotazione e confronto con la deformata]({{ site.baseurl }}/assets/images/monitorare-lo-spostamento-di-una-01.jpg)

**L'inclinometro non misura la freccia: misura una derivata**

Quando si installano inclinometri su una trave o su una trave da ponte il dato utile non e la rotazione in se, ma la catena che porta da quel segnale alla deformata e quindi alla decisione tecnica: collaudo riuscito, anomalia locale, deriva termica, perdita di simmetria tra travi affiancate oppure variazione di rigidezza nel tempo. Il passaggio critico e tutto qui: una serie di angoli sparsi lungo la campata deve diventare uno spostamento verticale ricostruibile, controllabile e confrontabile con un modello teorico o FEM.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme tecniche per le costruzioni*, con richiamo ai Capitoli 5, 8, 9 e 10.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, istruzioni applicative delle NTC 2018.
- D.M. 1 luglio 2022, n. 204, *Adozione delle Linee Guida per la classificazione e gestione del rischio, la valutazione della sicurezza ed il monitoraggio dei ponti esistenti*.
- Consiglio Superiore dei Lavori Pubblici, Decreto del Presidente n. 326 del 21 settembre 2022, *Istruzioni operative per l'applicazione delle Linee Guida ponti esistenti*.

Nota StruHub.

In un workflow serio il dato inclinometrico non dovrebbe mai rimanere isolato. Conviene tenere separati: misura grezza, correzione di zero e temperatura, ricostruzione della deformata, confronto con il modello di riferimento e giudizio finale. E' anche la logica utile in una piccola routine nello spirito StruHub o in un benchmark rapido con `beamfeapy`: non serve un grafico elegante se non si riesce a risalire a catene, segni, unita di misura e ipotesi elastiche adottate.

![Schema del workflow per ricostruire la deformata di una trave da misure inclinometriche]({{ site.baseurl }}/assets/images/monitorare-lo-spostamento-di-una-schema.svg)

**Dalla rotazione alla linea elastica**

Se si assume che la deformata verticale della campata sia sufficientemente regolare, una ricostruzione pratica consiste nell'approssimare lo spostamento con un polinomio:

$$
v(x)=c_0+c_1x+c_2x^2+\dots+c_nx^n
$$

L'inclinometro fornisce la rotazione locale, che nel modello di piccole deformazioni coincide con:

$$
\theta(x)=\frac{dv(x)}{dx}
$$

Con cinque inclinometri e due condizioni di spostamento nullo agli appoggi si hanno sette vincoli indipendenti. Una scelta naturale e quindi un polinomio di sesto grado:

$$
v(x)=c_0+c_1x+c_2x^2+c_3x^3+c_4x^4+c_5x^5+c_6x^6
$$

Per evitare coefficienti numericamente poco leggibili conviene introdurre la coordinata normalizzata:

$$
\xi=\frac{x}{L}
$$

e scrivere:

$$
v(\xi)=a_0+a_1\xi+a_2\xi^2+a_3\xi^3+a_4\xi^4+a_5\xi^5+a_6\xi^6
$$

La rotazione diventa:

$$
\theta(\xi)=\frac{1}{L}\left(a_1+2a_2\xi+3a_3\xi^2+4a_4\xi^3+5a_5\xi^4+6a_6\xi^5\right)
$$

Per una campata semplicemente appoggiata con sensori alle ascisse $\xi=\{0,0.25,0.50,0.75,1.00\}$, il sistema da risolvere e:

$$
v(0)=0,\qquad v(1)=0
$$

$$
\theta(\xi_i)=\theta_i \qquad i=1,\dots,5
$$

In forma matriciale:

$$
\mathbf{A}\mathbf{a}=\mathbf{b}
$$

dove $\mathbf{a}=[a_0,\dots,a_6]^T$ e il vettore dei termini noti contiene due spostamenti nulli e cinque rotazioni misurate. Il vantaggio operativo e semplice: se il set di misure e coerente, l'intera deformata si ricostruisce con un solo sistema lineare; se il set non e coerente, l'incompatibilita emerge subito da residui, oscillazioni spurie o perdita di simmetria.

**Prima del calcolo: i controlli che evitano errori grossolani**

Nella pratica di cantiere e di monitoraggio, gli errori piu comuni non nascono dall'algebra ma dal pre-processing. Prima di integrare le rotazioni conviene fissare almeno queste regole:

- lavorare sempre in radianti o milliradianti e dichiararlo nel report;
- assegnare una convenzione di segno unica tra sensori, modello numerico e grafici;
- correggere offset iniziale e deriva termica prima della ricostruzione della deformata;
- associare ogni misura alla sua ascissa reale, non a una posizione nominale approssimata;
- verificare che gli inclinometri di estremita siano davvero solidali con la trave e non con dettagli locali deformabili.

Se mancano i sensori in prossimita degli appoggi, la ricostruzione resta possibile ma cambia il problema matematico: bisogna sostituire una o piu condizioni con spostamenti noti, quote topografiche, vincoli di simmetria oppure una stima ai minimi quadrati. Il punto professionale non e "far tornare il polinomio", ma dichiarare quali informazioni esterne lo chiudono.

**Caso numerico: collaudo statico di una campata prefabbricata da ponte**

Si consideri una trave principale di un impalcato semplicemente appoggiato in c.a.p., luce netta:

$$
L=31.80\,\mathrm{m}=31800\,\mathrm{mm}
$$

Durante una fase di collaudo statico si assume, come benchmark manuale, una distribuzione equivalente del carico di prova:

$$
q=58\,\mathrm{kN/m}=58\,\mathrm{N/mm}
$$

Per il confronto elastico si adottano:

- modulo elastico efficace del conglomerato: $E_{c,eff}=38000\,\mathrm{MPa}$;
- inerzia equivalente della trave: $I=1.35 \cdot 10^{12}\,\mathrm{mm^4}$;
- cinque inclinometri alle sezioni $x=\{0,\ L/4,\ L/2,\ 3L/4,\ L\}$.

La soluzione teorica di Eulero-Bernoulli per trave appoggiata sotto carico uniforme, assunta con freccia verso il basso negativa, e:

$$
v_{ref}(x)= -\frac{q\,x}{24EI}\left(L^3-2Lx^2+x^3\right)
$$

La corrispondente rotazione e:

$$
\theta_{ref}(x)= -\frac{q}{24EI}\left(L^3-6Lx^2+4x^3\right)
$$

Valutando la formula nelle cinque sezioni strumentate si ottiene il set di rotazioni riportato in tabella.

| Sensore | Posizione $x$ [m] | $\theta_i$ [mrad] |
| --- | ---: | ---: |
| S1 | 0.00 | -1.515 |
| S2 | 7.95 | -1.041 |
| S3 | 15.90 | 0.000 |
| S4 | 23.85 | +1.041 |
| S5 | 31.80 | +1.515 |

Risolto il sistema nelle coordinate normalizzate $\xi=x/L$, la deformata interpolata risulta:

$$
v(\xi)=
-48.17\,\xi
+5.97\,\xi^2
+63.19\,\xi^3
+21.46\,\xi^4
-63.67\,\xi^5
+21.22\,\xi^6
\quad [\mathrm{mm}]
$$

I valori ricostruiti nelle sezioni principali diventano:

| Sezione | $\xi$ | $v_{ric}$ [mm] | $v_{ref}$ [mm] | Errore relativo |
| --- | ---: | ---: | ---: | ---: |
| Appoggio sinistro | 0.00 | 0.00 | 0.00 | 0.0% |
| Quarto di luce | 0.25 | -10.66 | -10.73 | 0.7% |
| Mezzeria | 0.50 | -15.01 | -15.05 | 0.3% |
| Tre quarti di luce | 0.75 | -10.66 | -10.73 | 0.7% |
| Appoggio destro | 1.00 | 0.00 | 0.00 | 0.0% |

La freccia massima teorica di riferimento e:

$$
f_{max,ref}=\frac{5qL^4}{384EI}=15.05\,\mathrm{mm}
$$

mentre la ricostruzione da sole rotazioni porta a:

$$
f_{max,ric}\approx 15.01\,\mathrm{mm}
$$

La differenza e inferiore allo 0.3%. In un collaudo reale non ci si aspetta una coincidenza cosi pulita, ma il caso mostra un fatto utile: con sensori ben posizionati e catena di segno coerente, il metodo inclinometrico puo ricostruire la deformata con accuratezza pienamente sufficiente per controlli di esercizio, prove di carico e monitoraggi comparativi.

![Confronto tra deformata teorica e deformata ricostruita da misure inclinometriche]({{ site.baseurl }}/assets/images/monitorare-lo-spostamento-di-una-02.jpg)

**Che cosa controlla davvero il progettista o il collaudatore**

Il numero di freccia a mezzeria non basta. In un uso professionale conviene leggere almeno cinque aspetti:

- coerenza della simmetria della deformata rispetto allo schema di carico applicato;
- congruenza tra rotazioni di estremita e reazioni attese agli appoggi;
- differenza tra deformata misurata e deformata del modello elastico di riferimento;
- eventuale deformata residua dopo scarico;
- confronto tra travi adiacenti, soprattutto nei ponti a piu travi principali.

Se una trave mostra la stessa freccia massima di progetto ma una curvatura locale anomala in un quarto di luce, il problema non e intercettato dal solo valore in mezzeria. E' qui che l'inclinometro diventa interessante: non restituisce un solo spostamento puntuale, ma consente di leggere la forma della linea elastica.

Per un confronto rapido con la teoria, lo scarto punto per punto puo essere scritto come:

$$
\Delta v(x)=v_{mis}(x)-v_{ref}(x)
$$

e l'errore normalizzato massimo come:

$$
\varepsilon_{max}=\frac{\max|\Delta v(x)|}{\max|v_{ref}(x)|}
$$

Se $\varepsilon_{max}$ cresce in modo sistematico tra campagne successive, la misura non va archiviata come semplice "rumore". Potrebbe indicare variazione di rigidezza, modifica dei vincoli, danneggiamento locale, degradazione dell'appoggio oppure un problema di installazione dei sensori.

**Quando il polinomio inganna**

La ricostruzione polinomiale e utile ma non va scambiata per una verita fisica automatica. Ci sono almeno quattro casi in cui puo diventare fuorviante:

- pochi sensori rispetto alla complessita reale della deformata;
- presenza di discontinuita locali, ad esempio giunti, diaframmi rigidi o appoggi ammalorati;
- rumore o deriva termica non corretti;
- ricostruzione forzata di un comportamento tridimensionale con un modello monodimensionale.

Un campanello d'allarme classico e la comparsa di oscillazioni artificiali tra due sensori. Se il polinomio produce flessi o controfrecce che il modello strutturale non giustifica, non bisogna "fidarsi del software": bisogna cambiare base di approssimazione, aumentare il numero di sensori, introdurre un fit regolarizzato oppure passare a una ricostruzione coerente con le shape function FEM.

Qui un benchmark numerico e molto utile. Un modello rapido di trave in `beamfeapy`, anche elementare, permette di confrontare rotazioni, freccia e curvatura con lo stesso schema di carico. Se il dato di campo diverge, il valore non e dimostrare che il sensore "sbaglia", ma capire se l'anomalia e meccanica o strumentale. In alcune campagne torna utile anche affiancare un controllo di controfreccia iniziale, nello spirito di una utility come `ControMonta`, per separare la deformata sotto carico dalla geometria di partenza dell'impalcato.

**Dal monitoraggio al report tecnico**

Perche il risultato sia davvero verificabile, nel report conviene rendere espliciti almeno questi elementi:

1. posizione dei sensori, sistema di riferimento e convenzione dei segni;
2. unita di misura, frequenza di acquisizione e criteri di filtraggio;
3. condizioni al contorno assunte per chiudere la ricostruzione;
4. equazioni o algoritmo usato per passare da $\theta(x)$ a $v(x)$;
5. confronto con il modello teorico o numerico adottato come benchmark;
6. giudizio finale sulla campagna: freccia, residuo, simmetria, scarti anomali.

Questo e coerente sia con la logica del collaudo statico richiamata dalle NTC 2018, sia con la logica di gestione, sorveglianza e monitoraggio esplicitata nelle Linee Guida ponti esistenti. Un buon elaborato non deve limitarsi a dire che "gli inclinometri sono compatibili": deve mostrare in quale punto, rispetto a quale modello e con quale margine.

**Dal dato alla decisione**

Monitorare lo spostamento di una trave tramite inclinometri ha senso quando la misura porta a una decisione tecnica: confermare il comportamento elastico, individuare una differenza tra travi gemelle, intercettare deriva dei vincoli, organizzare una soglia di attenzione nel tempo oppure chiudere un collaudo con una traccia ripetibile. Il vero valore non sta nell'algoritmo di interpolazione preso da solo, ma nella trasparenza dell'intera catena: misura, correzione, ricostruzione, benchmark e interpretazione.

Se questa catena resta leggibile, anche un sistema semplice di inclinometri diventa uno strumento robusto. Se invece il report si limita a un grafico finale senza quote, segni, filtri e riferimento normativo, il risultato puo apparire convincente ma resta poco difendibile davanti a un controllo tecnico serio.

Riferimenti normativi e bibliografici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle "Norme tecniche per le costruzioni"*, Ministero delle Infrastrutture e dei Trasporti, G.U. n. 42 del 20 febbraio 2018, S.O. n. 8.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle "Norme tecniche per le costruzioni" di cui al decreto ministeriale 17 gennaio 2018*, G.U. n. 35 del 11 febbraio 2019, S.O. n. 5.
- D.M. 1 luglio 2022, n. 204, *Adozione delle Linee Guida per la classificazione e gestione del rischio, la valutazione della sicurezza ed il monitoraggio dei ponti esistenti*.
- Consiglio Superiore dei Lavori Pubblici, Decreto del Presidente n. 326 del 21 settembre 2022, *Istruzioni operative per l'applicazione delle Linee Guida ponti esistenti*.
- Timoshenko, S. P., Young, D. H., Weaver, W., *Vibration Problems in Engineering*, John Wiley & Sons.
- Gere, J. M., Timoshenko, S. P., *Mechanics of Materials*, PWS Publishing Company.
