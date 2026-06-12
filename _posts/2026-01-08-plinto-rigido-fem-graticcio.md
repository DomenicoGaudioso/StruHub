---
layout: post
title: "Confronto tra modello di plinto rigido e modello FEM a graticcio"
description: "Riferimento tecnico StruHub con schema di calcolo, benchmark applicativi e riferimenti NTC/EC tracciabili."
date: 2026-01-08
order: 8
permalink: /posts/plinto-rigido-fem-graticcio.html
meta: "Quaderno tecnico - 1750 parole circa"
---

![Schema del confronto tra plinto rigido e plinto FEM a graticcio]({{ site.baseurl }}/assets/images/plinto-rigido-fem-graticcio-schema.svg)

**Quando il plinto smette di essere un vincolo perfetto**

Nel progetto di una fondazione su pali, il primo modello che quasi tutti impostano e il plinto rigido. E' rapido, leggibile e spesso sufficiente per capire come si ripartiscono $N$, $M_x$ e $M_y$ tra i pali. Il problema nasce quando questa ipotesi, comoda in predimensionamento, viene trattata come una verita generale: se il plinto e relativamente sottile, se i pali hanno rigidezze diverse oppure se il layout e irregolare, la distribuzione reale delle reazioni puo scostarsi in modo non trascurabile da quella del modello rigido.

Un confronto serio tra plinto rigido e modello FEM a graticcio non serve a "complicare il conto". Serve a rispondere a una domanda molto concreta: la semplificazione adottata e ancora conservativa rispetto ai pali piu caricati e ai momenti nel plinto?

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle "Norme tecniche per le costruzioni"*, con particolare attenzione ai Capitoli 4, 6 e 7.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle "Norme tecniche per le costruzioni"*.
- UNI EN 1992-1-1:2015, Eurocodice 2 - Progettazione delle strutture di calcestruzzo - Regole generali e regole per gli edifici.
- UNI EN 1997-1:2013, Eurocodice 7 - Progettazione geotecnica - Parte 1: Regole generali.
- H. G. Poulos, E. H. Davis, *Pile Foundation Analysis and Design*, John Wiley & Sons, 1980.

Nota StruHub.

Nel lavoro quotidiano conviene tenere separati tre livelli: ripartizione cinematica delle reazioni, verifica geotecnica dei singoli pali, verifica strutturale del plinto. E' anche la logica adottata in tool come `PlintoPali`, dove il confronto tra solver rigido e graticcio FEM e affiancato da benchmark di regressione e da una reportistica che rende espliciti input, margini e casi critici.

**Il modello rigido: rapido, ma con ipotesi forti**

Assumendo il plinto infinitamente rigido, le teste dei pali restano complanari. Se tutti i pali hanno uguale rigidezza assiale e si trascura la deformabilita locale del plinto, la reazione del palo $i$ puo essere scritta come:

$$
R_i=\frac{N}{n}+\frac{M_x y_i}{\sum_j y_j^2}+\frac{M_y x_i}{\sum_j x_j^2}
$$

dove:

- $N$ e il carico verticale risultante;
- $M_x$ e $M_y$ sono i momenti rispetto agli assi baricentrici del gruppo;
- $x_i$, $y_i$ sono le coordinate del palo rispetto al baricentro del gruppo.

Questa formula ha un pregio enorme: consente un controllo a mano in pochi minuti. Se i pali sono disposti in modo regolare, il progettista capisce subito chi e il palo piu caricato e quanto pesa l'eccentricita del carico.

Ma l'ipotesi regge solo se:

- il plinto e abbastanza rigido rispetto ai pali;
- i punti di applicazione delle azioni non inducono concentrazioni locali rilevanti;
- il layout dei pali e regolare o quasi regolare;
- la verifica geotecnica non e gia vicina al limite.

Appena uno di questi punti si indebolisce, il modello rigido diventa un ottimo controllo preliminare, non piu il modello definitivo.

**Che cosa aggiunge il graticcio FEM**

Nel modello FEM a graticcio il plinto viene discretizzato con aste o elementi piastra; i pali sono rappresentati da molle verticali, oppure da elementi equivalenti con rigidezza assiale:

$$
k_p \approx \frac{E_p A_p}{L_{eq}}
$$

Il solver non restituisce soltanto le reazioni sui pali, ma anche:

- abbassamenti differenziali alle teste dei pali;
- concentrazioni di reazione vicino alla zona di carico;
- momenti e tagli interni nel plinto;
- eventuali pali scaricati o in trazione.

Il passaggio dal modello rigido al FEM ha quindi senso quando la decisione di progetto dipende non dalla media dei carichi, ma dal massimo locale.

Come indicatore di buon senso, non normativo, conviene confrontare la rigidezza flessionale del plinto con quella dei pali su un interasse caratteristico $s$:

$$
\chi=\frac{k_p s^3}{E_c I_{eq}}
$$

dove $I_{eq}$ e l'inerzia della striscia equivalente di plinto che collega due pali. Se $\chi$ e molto piccolo, il plinto tende a comportarsi come corpo rigido. Quando $\chi$ si avvicina all'unita o la supera, il confronto con un modello deformabile diventa difficilmente evitabile. Non e una soglia di norma, ma una lettura meccanica utile per capire se vale la pena fermarsi al Navier oppure no.

**Caso numerico 1: benchmark 2x2 quasi al limite**

Un caso molto utile per capire l'ordine di grandezza e quello salvato nel benchmark `test/caso_base.json` del repository pubblico `PlintoPali`. Il caso rappresenta un plinto quadrato su quattro pali con carico eccentrico ma ancora in campo ordinario di progetto.

Dati principali del caso:

- plinto $5.0 \times 5.0\,\mathrm{m}$, spessore $t=1.0\,\mathrm{m}$;
- quattro pali da diametro $D=0.60\,\mathrm{m}$ e lunghezza $L=12.0\,\mathrm{m}$;
- interassi $s_x=s_y=3.0\,\mathrm{m}$;
- calcestruzzo del plinto con $E_c=30000\,\mathrm{MPa}$;
- azioni di progetto: $N=1200\,\mathrm{kN}$, $M_x=120\,\mathrm{kNm}$, $M_y=80\,\mathrm{kNm}$;
- stratigrafia di input: sabbia media nei primi $2\,\mathrm{m}$, sabbia addensata da $2$ a $5\,\mathrm{m}$, strato coesivo da $5$ a $10\,\mathrm{m}$.

Con interassi di $3.0\,\mathrm{m}$ le coordinate dei pali rispetto al baricentro sono $(\pm 1.5,\pm 1.5)\,\mathrm{m}$. La quota uniforme vale:

$$
\frac{N}{n}=\frac{1200}{4}=300\,\mathrm{kN}
$$

e i contributi flessionali sono:

$$
\frac{M_x y_i}{\sum y_j^2}=\frac{120 \cdot y_i}{4 \cdot 1.5^2}
=\frac{120 \cdot y_i}{9}
$$

$$
\frac{M_y x_i}{\sum x_j^2}=\frac{80 \cdot x_i}{4 \cdot 1.5^2}
=\frac{80 \cdot x_i}{9}
$$

Le reazioni del modello rigido risultano quindi:

| Palo | Coordinate $(x_i,y_i)$ [m] | $R_i$ [kN] |
| --- | --- | ---: |
| P1 | $(-1.5,-1.5)$ | 266.7 |
| P2 | $(+1.5,-1.5)$ | 293.3 |
| P3 | $(-1.5,+1.5)$ | 306.7 |
| P4 | $(+1.5,+1.5)$ | 333.3 |

Il valore massimo coincide con quello del benchmark:

$$
R_{max}=333.3\,\mathrm{kN}
$$

Lo stesso caso riporta:

- capacita ultima del palo singolo $Q_{ult}=1248.5\,\mathrm{kN}$;
- portata ammissibile effettiva del palo $Q_{amm}=499.4\,\mathrm{kN}$;
- fattore di sicurezza minimo sul gruppo, in termini semplificati, pari a circa $FS_{min}=1.50$;
- cedimento elastico di gruppo dell'ordine di $1.89\,\mathrm{mm}$.

Lettura tecnica. Questo e un caso molto istruttivo perche non esplode numericamente, ma e gia vicino a una soglia decisionale: il plinto rigido non segnala trazione nei pali e la massima reazione resta inferiore a $Q_{amm}$, pero il margine non e ampio. In un progetto reale, con combinazioni piu gravose o con un plinto piu snello, il FEM non e un raffinamento estetico: e il modo per capire se il palo P4 resta davvero nell'intorno dei $333\,\mathrm{kN}$ oppure se la deformabilita del plinto ne aumenta la domanda locale.

**Dove il plinto rigido inizia a perdere affidabilita**

Ci sono almeno quattro segnali operativi che suggeriscono di non fermarsi al modello rigido:

- spessore del plinto modesto rispetto agli interassi;
- carichi applicati in posizione localizzata o lontani dal baricentro del gruppo;
- layout irregolare dei pali;
- verifica geotecnica gia vicina al limite.

Nel caso appena visto il rapporto tra spessore e interasse e:

$$
\frac{t}{s}=\frac{1.0}{3.0}=0.33
$$

Un rapporto cosi non dimostra da solo che il modello rigido sia errato, ma basta per dire che il confronto con un graticcio non e tempo perso. Se inoltre il carico verticale fosse portato da un setto o da un fusto fuori asse, la concentrazione sulle teste dei pali piu vicine alla risultante potrebbe salire anche a parita di azioni globali.

**Caso numerico 2: layout irregolare con palo in trazione**

Il benchmark `test/benchmark/gruppo_pali_irregolare_trazione.json` mostra bene quando il modello rigido, pur corretto come equilibrio, diventa insufficiente per chiudere la progettazione. Il gruppo e composto da cinque pali con coordinate irregolari, carico totale $N=900\,\mathrm{kN}$ e momento dominante $M_x=1450\,\mathrm{kNm}$.

I risultati attesi del benchmark sono:

- reazione minima $R_{min}=-164.7\,\mathrm{kN}$;
- reazione massima $R_{max}=502.0\,\mathrm{kN}$;
- portata ammissibile effettiva $Q_{amm}=384.1\,\mathrm{kN}$;
- efficienza di gruppo $\eta_g=0.769$;
- fattore di sicurezza minimo $FS_{min}=0.995$.

Questo secondo caso e molto piu severo del primo e fa emergere due problemi diversi:

1. un palo entra in trazione, quindi il controllo non e piu solo di compressione geotecnica ma anche di sfilamento, collegamento palo-plinto e dettaglio di armatura;
2. il palo piu caricato supera la portata ammissibile del singolo elemento, quindi la riprogettazione non puo limitarsi a "spalmare meglio" il carico nel foglio Excel.

Qui il FEM a graticcio non sostituisce la verifica geotecnica, ma aiuta a vedere se la concentrazione nasce soprattutto dal layout, dalla posizione della risultante oppure dalla deformabilita del plinto. E' il tipo di confronto che cambia le scelte: aumentare lo spessore del plinto, modificare il passo dei pali, aggiungere un palo o ruotare il layout per allinearlo meglio al momento dominante.

**Che cosa deve uscire da un confronto serio**

Se si esegue il doppio calcolo rigido/FEM, la tabella finale non dovrebbe limitarsi a mostrare un solo "OK". Conviene riportare almeno:

- $R_{max}$ e $R_{min}$ per entrambi i modelli;
- differenza percentuale sul palo piu caricato:

$$
\Delta_R=\frac{R_{max,FEM}-R_{max,rig}}{R_{max,rig}}
$$

- abbassamento massimo e differenziale tra le teste dei pali;
- momento flettente di progetto nel plinto in corrispondenza delle fasce critiche;
- rapporto domanda/capacita per i pali in compressione e, se presenti, per i pali in trazione.

Una regola pratica utile e questa: se il FEM sposta poco il palo critico e non cambia il segno delle reazioni, il modello rigido puo restare il riferimento principale di predimensionamento. Se invece aumenta sensibilmente $R_{max}$, introduce reazioni negative oppure fa emergere momenti locali elevati nel plinto, allora il modello rigido va declassato a controllo preliminare.

**Errori ricorrenti da evitare**

Nel confronto tra i due modelli compaiono spesso gli stessi errori:

- usare solo $N/n$ e dimenticare il contributo dei momenti;
- controllare la massima reazione ma non la minima, perdendo l'informazione sulla trazione;
- confrontare i modelli strutturali senza aggiornare la portata ammissibile con l'efficienza di gruppo;
- citare il FEM senza documentare rigidezze delle molle, geometria della mesh e convenzioni di segno;
- chiudere il report senza un benchmark o un caso di controllo rifacibile a mano.

E' proprio qui che i repository ben costruiti fanno la differenza: non tanto perche "automatizzano", ma perche costringono a lasciare una traccia tecnica verificabile. Nel caso di `PlintoPali`, la presenza di benchmark JSON per casi regolari e irregolari e piu utile di molte interfacce eleganti: permette di capire se una modifica del solver cambia davvero il comportamento del caso base.

**Dal numero alla decisione progettuale**

Il punto non e scegliere in astratto tra plinto rigido e FEM a graticcio. Il punto e capire quando il secondo diventa necessario per difendere il progetto. Se il plinto e massiccio, il layout e regolare e i margini geotecnici sono larghi, il modello rigido resta uno strumento eccellente. Se invece il carico si concentra, i pali non sono simmetrici o la verifica e gia prossima al limite, il FEM non aggiunge "precisione accademica": aggiunge informazione sul palo che governa davvero e sulla fascia di plinto da armare.

Per un professionista, il valore di un software o di un foglio di calcolo non sta nel numero finale, ma nella sua tracciabilita. Un buon workflow, nello spirito StruHub e CivilBox, e quello che consente di passare dal benchmark al modello, dal modello alla tabella delle reazioni e dalla tabella alla decisione: ispessire il plinto, spostare i pali, modificare le combinazioni o giustificare perche l'ipotesi rigida resta adeguata.

Riferimenti normativi e bibliografici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle "Norme tecniche per le costruzioni"*, Ministero delle Infrastrutture e dei Trasporti, G.U. n. 42 del 20 febbraio 2018, S.O. n. 8.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle "Norme tecniche per le costruzioni" di cui al decreto ministeriale 17 gennaio 2018*, G.U. n. 35 del 11 febbraio 2019, S.O. n. 5.
- UNI EN 1992-1-1:2015, Eurocodice 2 - Progettazione delle strutture di calcestruzzo - Parte 1-1: Regole generali e regole per gli edifici.
- UNI EN 1997-1:2013, Eurocodice 7 - Progettazione geotecnica - Parte 1: Regole generali.
- Poulos, H. G.; Davis, E. H., *Pile Foundation Analysis and Design*, John Wiley & Sons, 1980.
