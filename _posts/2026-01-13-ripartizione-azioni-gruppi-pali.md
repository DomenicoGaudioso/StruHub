---
layout: post
title: "Ripartizione delle azioni verticali nei gruppi di pali"
description: "Riferimento tecnico StruHub con schema di calcolo, benchmark applicativi e riferimenti normativi tracciabili."
date: 2026-01-13
order: 13
permalink: /posts/ripartizione-azioni-gruppi-pali.html
meta: "Quaderno tecnico - 1760 parole circa"
---

![Schema della ripartizione delle reazioni nei gruppi di pali regolari e irregolari]({{ site.baseurl }}/assets/images/ripartizione-azioni-gruppi-pali-schema.svg)

**Il palo critico non si trova per media, ma per equilibrio**

Quando un plinto su pali riceve solo uno sforzo normale centrato, la ripartizione e banale: ogni palo porta circa $N/n$. Il quadro cambia subito appena compaiono eccentricita, momento biassiale, layout irregolari o pali con rigidezze diverse. In quel momento la domanda utile non e piu "quanto vale il carico medio?", ma "quale palo governa davvero la verifica e con quale margine?".

Per chi progetta opere reali il punto non e soltanto ottenere una tabella di reazioni. Bisogna lasciare una traccia tecnica rifacibile: coordinate dei pali, convenzione dei segni, sistema di equilibrio, caso di carico, collegamento con la portata del singolo palo e, se serve, controllo della trazione.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme tecniche per le costruzioni*, con richiamo al Capitolo 6 sulle opere geotecniche e alle verifiche delle fondazioni profonde.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, istruzioni applicative delle NTC 2018 per modellazione, combinazioni e lettura geotecnica delle fondazioni su pali.
- UNI EN 1997-1:2013, Eurocodice 7 - Progettazione geotecnica - Parte 1: Regole generali.
- UNI EN 1998-5:2005, Eurocodice 8 - Progettazione per la resistenza sismica - Fondazioni, strutture di contenimento ed aspetti geotecnici.
- H. G. Poulos, E. H. Davis, *Pile Foundation Analysis and Design*, John Wiley & Sons, 1980.

Nota StruHub.

Nel lavoro quotidiano conviene tenere distinti tre livelli: ripartizione cinematica delle azioni verticali sui pali, verifica geotecnica del singolo palo e verifica strutturale del plinto. E' la stessa logica adottata nel repository `PlintoPali`, dove benchmark JSON, solver rigido e reportistica servono soprattutto a rendere tracciabile il passaggio da input a margine di sicurezza.

**Il modello minimo: plinto rigido e pali assiali**

Se il plinto e assunto infinitamente rigido nel proprio piano e tutti i pali hanno uguale rigidezza assiale, la reazione verticale del palo $i$ puo essere scritta in forma affine:

$$
R_i=a+b\,x_i+c\,y_i
$$

dove $x_i$ e $y_i$ sono le coordinate del palo rispetto al baricentro del gruppo. I coefficienti $a$, $b$ e $c$ si ricavano imponendo l'equilibrio globale:

$$
\sum_i R_i=N
$$

$$
\sum_i R_i x_i=M_y
$$

$$
\sum_i R_i y_i=M_x
$$

In forma matriciale:

$$
\mathbf{R}=\mathbf{A}\left(\mathbf{A}^{T}\mathbf{A}\right)^{-1}\mathbf{F}
$$

con:

$$
\mathbf{A}=
\begin{bmatrix}
1 & x_1 & y_1 \\
\vdots & \vdots & \vdots \\
1 & x_n & y_n
\end{bmatrix},
\qquad
\mathbf{F}=
\begin{bmatrix}
N \\
M_y \\
M_x
\end{bmatrix}
$$

Questa scrittura e molto piu utile della sola formula scolastica per due motivi:

- funziona anche per layout non a griglia;
- rende esplicito che la ripartizione deriva da un problema di equilibrio, non da un coefficiente tabellare astratto.

Per gruppi simmetrici rispetto agli assi principali, la stessa relazione si riduce alla forma nota:

$$
R_i=\frac{N}{n}+\frac{M_y x_i}{\sum_j x_j^2}+\frac{M_x y_i}{\sum_j y_j^2}
$$

Il pregio operativo di questa formula e enorme: consente un controllo a mano in pochi minuti e permette di capire subito quali pali si avvicinano al limite o entrano in scarico.

**Quando il modello semplice e sufficiente**

La ripartizione rigida e un buon modello di primo livello se, contemporaneamente:

- il plinto e abbastanza rigido rispetto agli interassi e alle rigidezze dei pali;
- i pali hanno risposta assiale comparabile;
- il punto di applicazione delle azioni non introduce concentrazioni locali troppo spinte;
- il layout e regolare o quasi regolare;
- la verifica geotecnica del palo singolo non e gia prossima al limite.

Se una di queste ipotesi si indebolisce, il modello resta utile come benchmark manuale ma non dovrebbe essere l'unica base della decisione progettuale.

Un primo controllo sintetico e il rapporto domanda/capacita sul singolo palo:

$$
\eta_i=\frac{N_{i,Ed}}{R_{i,d}} \le 1
$$

dove $N_{i,Ed}$ e la reazione di progetto sul palo $i$ e $R_{i,d}$ la resistenza verticale di progetto o ammissibile assunta nel workflow adottato. Se compaiono reazioni negative, la verifica va sdoppiata: compressione da una parte, trazione o sfilamento dall'altra.

**Caso numerico 1: benchmark reale 2x2 del repository PlintoPali**

Un caso molto utile per fissare gli ordini di grandezza e il benchmark `test/caso_base.json` del repository pubblico `DomenicoGaudioso/PlintoPali`, generato il 12 maggio 2026. Il file rappresenta un plinto quadrato su quattro pali con carico eccentrico moderato e costituisce un buon riferimento per controlli manuali e regressioni del solver.

Dati principali del caso:

- plinto $5.0 \times 5.0\,\mathrm{m}$ con spessore $t=1.0\,\mathrm{m}$;
- quattro pali di diametro $D=0.60\,\mathrm{m}$ e lunghezza $L=12.0\,\mathrm{m}$;
- interassi $s_x=s_y=3.0\,\mathrm{m}$;
- modulo elastico del plinto $E_c=30000\,\mathrm{MPa}$;
- azioni globali: $N=1200\,\mathrm{kN}$, $M_x=120\,\mathrm{kNm}$, $M_y=80\,\mathrm{kNm}$;
- stratigrafia di input: sabbia media nei primi $2\,\mathrm{m}$, sabbia piu addensata tra $2$ e $5\,\mathrm{m}$, strato coesivo fino alla punta del palo.

Con interassi di $3.0\,\mathrm{m}$, le coordinate dei pali rispetto al baricentro sono:

$$
(x_i,y_i)=(\pm 1.5,\pm 1.5)\,\mathrm{m}
$$

Il contributo uniforme vale:

$$
\frac{N}{n}=\frac{1200}{4}=300\,\mathrm{kN}
$$

Le somme quadratiche sono:

$$
\sum_j x_j^2=\sum_j y_j^2=4 \cdot 1.5^2=9.0\,\mathrm{m^2}
$$

Quindi i contributi flessionali massimi sono:

$$
\frac{M_y x_i}{\sum_j x_j^2}=\frac{80 \cdot (\pm 1.5)}{9}=\pm 13.3\,\mathrm{kN}
$$

$$
\frac{M_x y_i}{\sum_j y_j^2}=\frac{120 \cdot (\pm 1.5)}{9}=\pm 20.0\,\mathrm{kN}
$$

Le reazioni del modello rigido risultano:

| Palo | Coordinate $(x_i,y_i)$ [m] | $R_i$ [kN] |
| --- | --- | ---: |
| P1 | $(-1.5,-1.5)$ | 266.7 |
| P2 | $(+1.5,-1.5)$ | 293.3 |
| P3 | $(-1.5,+1.5)$ | 306.7 |
| P4 | $(+1.5,+1.5)$ | 333.3 |

Il valore massimo coincide con l'output atteso del benchmark:

$$
R_{max}=333.3\,\mathrm{kN}
$$

Lo stesso file riporta inoltre:

- capacita ultima del palo singolo $Q_{ult}=1248.45\,\mathrm{kN}$;
- portata ammissibile effettiva del palo $Q_{amm,eff}=499.38\,\mathrm{kN}$;
- fattore di sicurezza minimo in combinazione statica $FS_{min}=1.498$;
- cedimento elastico semplificato del gruppo pari a $1.89\,\mathrm{mm}$.

La lettura progettuale utile e immediata. Sul palo piu caricato si ha:

$$
\eta_{max}=\frac{333.3}{499.4}=0.67
$$

Il gruppo risulta quindi verificato nel benchmark di base, ma non con margini tali da rendere inutile il controllo del modello. In pratica il caso e abbastanza pulito da essere ricostruito a mano, ma abbastanza vicino alla soglia decisionale da intercettare errori di segno, coordinate scambiate o variazioni involontarie del solver.

**Che cosa insegna davvero il caso base**

Il valore del benchmark non sta solo nei numeri finali. Sta nel fatto che costringe a controllare una catena completa:

1. coordinate dei pali riferite a un baricentro unico;
2. convenzione coerente tra $M_x$, $M_y$, $x_i$ e $y_i$;
3. coerenza tra ripartizione delle azioni e portata del palo singolo;
4. differenza tra resistenza ultima, portata ammissibile ed esito di verifica;
5. tracciabilita del file di input usato per la relazione.

Questo e esattamente il tipo di controllo che un professionista puo rifare in cinque minuti prima di firmare una relazione o validare l'output di un software.

**Caso numerico 2: layout irregolare e palo in trazione**

Il secondo benchmark utile, `test/benchmark/gruppo_pali_irregolare_trazione.json`, mostra una situazione molto piu severa: cinque pali con layout irregolare, momento dominante elevato e un palo che entra in trazione. Le coordinate di input sono:

$$
(-1.8,-1.2),\ (1.7,-1.0),\ (-1.4,1.25),\ (1.35,1.1),\ (0.15,0.05)\,\mathrm{m}
$$

con azioni globali:

$$
N=900\,\mathrm{kN}, \qquad M_x=1450\,\mathrm{kNm}, \qquad M_y=100\,\mathrm{kNm}
$$

Il benchmark restituisce:

- reazione minima $R_{min}=-164.67\,\mathrm{kN}$;
- reazione massima $R_{max}=502.02\,\mathrm{kN}$;
- portata ammissibile effettiva del palo $Q_{amm,eff}=384.08\,\mathrm{kN}$;
- efficienza di gruppo equivalente $\eta_g=0.769$;
- esito atteso: `NON VERIFICATO`.

Questo secondo caso serve a ricordare tre aspetti molto pratici.

Primo: la ripartizione non e piu intuitiva a occhio. Senza la forma matriciale o un solver affidabile, il rischio di sbagliare il palo critico e concreto.

Secondo: una reazione negativa non e un dettaglio numerico. Cambia la natura della verifica. Occorre controllare se il palo puo lavorare a trazione, se il collegamento palo-plinto e adeguato e se la combinazione considerata ha ancora senso rispetto al modello strutturale assunto.

Terzo: il problema non e solo del palo in trazione. Qui anche il palo piu compresso supera la portata ammissibile effettiva. Quindi non basta "aggiustare" il dettaglio di ancoraggio: il gruppo va ripensato in termini di layout, interassi, spessore del plinto, numero di pali o combinazioni agenti.

**Dove la ripartizione rigida smette di bastare**

La formula rigida e un benchmark eccellente, ma non dice tutto. In diversi casi serve un passo in piu, per esempio un graticcio FEM o un modello che introduca rigidezze verticali differenziate dei pali, se:

- il plinto e relativamente snello rispetto agli interassi;
- le rigidezze dei pali cambiano per diametro, lunghezza o stratigrafia;
- il carico e molto eccentrico o localizzato;
- il layout non e simmetrico;
- le verifiche sono gia prossime al limite.

Il punto tecnico da chiarire in relazione e semplice: la ripartizione rigida risponde alla domanda "come si chiude l'equilibrio globale?". Un modello deformabile risponde invece alla domanda "come si redistribuisce davvero la domanda quando il plinto flette e i pali hanno cedevolezze finite?".

Non sempre serve arrivare al secondo modello. Ma quando serve, bisogna dichiararlo senza ambiguita.

**Che cosa dovrebbe comparire in una relazione seria**

Per un post tecnico o per una relazione di progetto non basta scrivere la formula finale. Conviene rendere espliciti almeno questi punti:

- tabella delle coordinate $x_i$, $y_i$ dei pali e definizione degli assi;
- azioni globali considerate per ogni combinazione: $N$, $M_x$, $M_y$;
- reazioni verticali per ogni palo, con evidenza di $R_{max}$ e $R_{min}$;
- criterio di confronto con la portata del singolo palo o con la resistenza di progetto;
- eventuale controllo della trazione o dello sfilamento;
- ipotesi sulla rigidezza del plinto e motivo della scelta del modello rigido o deformabile;
- provenienza dei parametri geotecnici usati per $Q_b$, $Q_s$, efficienza di gruppo e cedimenti.

Questa e la differenza tra un foglio che "fornisce un numero" e un elaborato difendibile davanti a verifica interna, collaudo o contraddittorio.

**Errori ricorrenti da evitare**

Nella pratica di ufficio i problemi si ripetono sempre sugli stessi punti:

- usare $N/n$ anche quando il momento governa il gruppo;
- scambiare il ruolo di $M_x$ e $M_y$ rispetto agli assi geometrici;
- ignorare i pali in trazione perche "tanto il carico medio e basso";
- confrontare $R_i$ con una portata del palo non coerente con la stessa combinazione e con la stessa efficienza di gruppo;
- usare un layout irregolare nel disegno ma uno schema regolare nel calcolo;
- non archiviare il caso numerico di riferimento che ha generato la tabella finale.

Un benchmark come quelli di `PlintoPali` e utile proprio per questo: non per sostituire il giudizio del progettista, ma per evitare che un errore banale di input venga scambiato per risultato raffinato.

**Dal numero alla decisione**

La ripartizione delle azioni verticali nei gruppi di pali e uno di quei problemi dove il livello di semplicita del modello deve restare leggibile, non ingenuo. Il plinto rigido fornisce un controllo manuale molto potente e, in molti casi ordinari, anche sufficiente. Ma il professionista dovrebbe sempre chiedersi quale palo governa, se esistono trazioni, quanto pesa l'irregolarita del layout e quale parte del risultato dipende da ipotesi meccaniche non dichiarate.

Se il workflow resta tracciabile, anche un modello semplice e affidabile. Se invece manca la catena tra coordinate, equilibrio, benchmark e verifica geotecnica, il numero finale puo sembrare pulito ma resta debole dal punto di vista ingegneristico. E' qui che l'approccio StruHub e gli strumenti CivilBox ben costruiti risultano utili: non per fare scena, ma per rendere ogni passaggio rifacibile e discutibile tecnicamente.

Riferimenti normativi e bibliografici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle "Norme tecniche per le costruzioni"*, Ministero delle Infrastrutture e dei Trasporti, G.U. n. 42 del 20 febbraio 2018, S.O. n. 8.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle "Norme tecniche per le costruzioni" di cui al decreto ministeriale 17 gennaio 2018*, G.U. n. 35 del 11 febbraio 2019, S.O. n. 5.
- UNI EN 1997-1:2013, Eurocodice 7 - Progettazione geotecnica - Parte 1: Regole generali.
- UNI EN 1998-5:2005, Eurocodice 8 - Progettazione per la resistenza sismica - Parte 5: Fondazioni, strutture di contenimento ed aspetti geotecnici.
- Poulos, H. G.; Davis, E. H., *Pile Foundation Analysis and Design*, John Wiley & Sons, 1980.
