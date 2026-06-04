---
layout: post
title: "Combinazione degli inviluppi dinamici da accelerogrammi multipli"
description: "Riferimento tecnico StruHub con formule, ipotesi di modello e riferimenti normativi."
date: 2026-01-06
order: 6
permalink: /posts/inviluppi-dinamici-accelerogrammi.html
meta: "Quaderno tecnico · 1611 parole circa"
---

**Perché più segnali**

Accelerogrammi diversi, anche se spettro-compatibili, generano massimi in istanti e sezioni differenti.

$$
E_i^+(z)=max_t E_i(z,t), qquad E_i^-(z)=min_t E_i(z,t)
$$

Media degli inviluppi.

$$
ar{E}^+(z)=\frac{1}{n}sum_{i=1}^{n}E_i^+(z)
$$

Si mediano gli effetti massimi, non i segnali nel tempo.

Esempio.

Momenti massimi (12.0,13.5,11.8,14.1,12.9,13.2,12.4,mathrm{MN m}) danno:

$$
ar{M}=12.84,mathrm{MN m}
$$

Il massimo assoluto è (14.1,mathrm{MN m}).

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme Tecniche per le Costruzioni* (NTC 2018).
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle Norme Tecniche per le Costruzioni*.

Riferimento aggiuntivo.

- UNI EN 1998-2, Eurocodice 8: Ponti.

Nota StruHub.

La gestione degli inviluppi è parte del calcolo, non un'operazione grafica finale.

**Massimi non concomitanti**

Negli inviluppi dinamici il massimo momento e il massimo taglio non si verificano necessariamente nello stesso istante. Questo aspetto è importante quando si vogliono costruire combinazioni locali di sollecitazioni. Un vettore concomitante può essere definito leggendo tutte le grandezze all'istante in cui una di esse è massima:

$$
\mathbf{E}(t_{Mmax})=[N(t_{Mmax}),V(t_{Mmax}),M(t_{Mmax})]
$$

Questa lettura è diversa dall'inviluppo indipendente di ciascuna componente.

**Tracciabilità del segnale governante**

Quando si usa un set di accelerogrammi, è utile conservare quale segnale governa ogni sezione:

$$
i_{gov}(z)=\arg\max_i E_i^+(z)
$$

Questo consente di capire se un risultato è governato sempre dallo stesso segnale o se cambia lungo la struttura.

Per trasformare la combinazione degli inviluppi dinamici in un riferimento tecnico è utile separare tre livelli: il modello fisico, il modello numerico e la lettura dei risultati. Il modello fisico identifica masse, rigidezze, smorzamento e azioni. Il modello numerico stabilisce come queste grandezze entrano nell'equazione del moto. La lettura dei risultati decide quali effetti sono davvero utili al progetto.

$$
M\ddot{u}(t)+C\dot{u}(t)+Ku(t)=F(t)
$$

Nel caso di set di accelerogrammi, la matrice di massa non è un dettaglio secondario. Una massa assegnata al nodo sbagliato modifica frequenze proprie e partecipazione modale; una rigidezza non coerente con la sezione resistente sposta il periodo e altera la domanda dinamica. Prima di discutere il risultato, il modello deve produrre modi plausibili.

**Passaggio modale e significato dei modi**

Il problema agli autovalori consente di estrarre frequenze e deformate. La forma modale non è solo un disegno: indica quali parti della struttura partecipano a un certo tipo di moto. Se la deformata è locale, il modo può essere poco rilevante per la risposta globale ma importante per una sollecitazione locale.

$$
K\phi_n=\omega_n^2M\phi_n
$$

$$
T_n=\frac{2\pi}{\omega_n}
$$

La massa partecipante permette di passare da una lista di frequenze a una gerarchia ingegneristica. Un modo con alta partecipazione nella direzione dell'azione è un modo che può governare spostamenti e tagli globali. Modi con partecipazione inferiore possono comunque influenzare accelerazioni o curvature.

**Procedura di calcolo consigliata**

Una procedura robusta per la combinazione degli inviluppi dinamici parte da un controllo statico o quasi-statico, prosegue con l'analisi modale, quindi entra nel dominio del tempo solo dopo aver verificato che frequenze e deformate siano coerenti. In questo ordine, l'analisi dinamica diventa un approfondimento controllato, non una scatola nera.

- definizione di geometria, masse e rigidezze;
- estrazione di periodi e forme modali;
- scelta dello smorzamento e del passo temporale;
- applicazione della storia di carico o del segnale;
- estrazione di massimi, minimi e grandezze concomitanti;
- confronto con un controllo semplificato.

**Esempio di controllo numerico**

Supponiamo che il primo controllo restituisca un periodo teorico di (0.55\,\mathrm{s}) e il modello FEM un periodo di (0.92\,\mathrm{s}). La differenza relativa è:

$$
\Delta_T=\frac{0.92-0.55}{0.55}=0.67
$$

Uno scarto del 67% non va ignorato. Può derivare da vincoli troppo flessibili, massa eccessiva, inerzia ridotta o unità non coerenti. Questo tipo di controllo è ciò che distingue un calcolo numerico da un uso passivo del software.

Lettura progettuale. La grandezza governante può cambiare lungo la struttura e da un accelerogramma all’altro. Per questo il dato importante non è solo il valore, ma anche quale segnale e quale istante lo generano. Un riferimento tecnico deve quindi mostrare non solo la formula, ma il motivo per cui quella formula è stata usata e il modo in cui il risultato viene interpretato. La parte finale del calcolo dovrebbe sempre riportare domanda, capacità, margine e ipotesi principali.

Impostazione del modello. Per usare questo tema come riferimento tecnico, conviene separare la modellazione in fasi verificabili. La prima fase è la definizione dello schema resistente: geometria, vincoli, masse, rigidezze e grandezze da estrarre. La seconda fase è la scelta del tipo di analisi: controllo statico, analisi modale, analisi nel tempo o confronto tra più livelli di modello. La terza fase è la lettura critica dei risultati, che non deve limitarsi al valore massimo ma deve includere posizione, segno, combinazione o istante di occorrenza.

Un controllo utile consiste nel confrontare sempre un risultato numerico con una stima semplificata. Per esempio, un periodo ottenuto con FEM dovrebbe essere compatibile con una valutazione manuale basata su massa e rigidezza. Se l'ordine di grandezza non torna, il problema non è nel solutore ma nella rappresentazione fisica del sistema.

La catena minima di controllo può essere scritta così: definizione del modello, verifica delle unità, estrazione delle frequenze, scelta dello smorzamento, applicazione dell'azione, inviluppo degli effetti, interpretazione del margine. Ogni passaggio deve lasciare una traccia riproducibile.

Unità, segni e grandezze da archiviare. Le analisi dinamiche sono molto sensibili alla coerenza delle unità. Massa in chilogrammi, forze in Newton, rigidezze in Newton per metro e accelerazioni in metri al secondo quadrato devono essere trattate nello stesso sistema. Un errore tra massa e peso può spostare il periodo di un fattore significativo.

Per ogni analisi è utile archiviare almeno: periodo fondamentale, masse partecipanti, smorzamento, passo temporale, massimo spostamento, massimo taglio, massimo momento e istante di occorrenza. Se il modello lavora per inviluppi, va salvata anche la sezione in cui ciascun effetto governa.

La grandezza di progetto non è sempre il massimo assoluto. Nelle verifiche accoppiate può essere necessario leggere valori concomitanti. Se il massimo momento si verifica a un certo istante, lo sforzo normale da usare nel dominio di verifica deve essere quello dello stesso istante, non il massimo normale indipendente.

Esempio di controllo incrociato. Supponiamo che un modello dinamico restituisca un taglio massimo alla base pari a (2.80\,\mathrm{MN}), mentre il controllo statico equivalente fornisce (2.35\,\mathrm{MN}). Il rapporto è:

$$
r_V=\frac{2.80}{2.35}=1.19
$$

Il dato indica una amplificazione del 19%. Questo non significa automaticamente che il modello dinamico sia più corretto, ma segnala che l'effetto dinamico non è trascurabile. A questo punto il tecnico deve controllare se la velocità, il contenuto in frequenza o il segnale sismico sono coerenti con la condizione più gravosa.

Se invece il rapporto fosse inferiore a 1, non si dovrebbe concludere in modo automatico che la dinamica è meno gravosa: potrebbe essere cambiata la grandezza controllata, oppure la massima risposta dinamica potrebbe verificarsi in un'altra sezione.

Come trasformare il risultato in testo tecnico. Un post di riferimento deve chiudere il cerchio: non basta presentare formule e grafici. Bisogna spiegare quale grandezza governa, quale ipotesi è più sensibile e quale controllo manuale consente di validare il risultato. Questa impostazione rende il contenuto utile anche a chi non usa lo stesso software, perché il metodo rimane trasferibile.

Dall'accelerogramma all'inviluppo. L'inviluppo dinamico non e il massimo di un singolo risultato, ma la sintesi controllata di piu storie temporali. Per ogni accelerogramma si ottiene una funzione di risposta, ad esempio momento flettente $M_i(t)$, taglio $V_i(t)$, spostamento $u_i(t)$ o reazione $R_i(t)$. Il massimo assoluto della singola storia e:

$$
E_i=\max_t |r_i(t)|
$$

Il problema tecnico e decidere come passare dall'insieme $\{E_i\}$ al valore di progetto. Una scelta puramente massima, $E_d=\max_i E_i$, e cautelativa ma puo essere governata da un accelerogramma anomalo. Una media sui massimi, se ammessa dal quadro di calcolo adottato, descrive meglio il comportamento centrale del set ma richiede accelerogrammi coerenti, scalati e compatibili con lo spettro di riferimento.

La compatibilita spettrale e un passaggio sostanziale. Se gli accelerogrammi sono scalati male, l'inviluppo risultante non rappresenta la domanda sismica del sito, ma la casualita del set scelto. Per un periodo strutturale $T_1$, il confronto tra spettro medio degli accelerogrammi e spettro elastico di riferimento consente di controllare se la domanda nel campo dinamicamente rilevante e coerente:

$$
\bar S_a(T)=\frac{1}{n}\sum_{i=1}^n S_{a,i}(T)
$$

Il controllo non dovrebbe limitarsi a $T_1$. Nei ponti, modi torsionali, modi longitudinali e modi locali delle sottostrutture possono rendere rilevante un intervallo di periodi. Per questo e utile definire una finestra, ad esempio $[0.2T_1,2T_1]$, adattandola alla struttura reale.

Segno, contemporaneita e componenti. Un errore frequente nella lettura degli inviluppi e perdere il segno delle azioni. Il massimo positivo e il minimo negativo devono restare distinti, soprattutto quando servono per combinare pressoflessione, taglio e reazioni vincolari. Per una sezione di pila, ad esempio, la coppia $N(t),M(t)$ puo essere piu significativa dei massimi separati di $N$ e $M$. Il dominio resistente viene interrogato con stati contemporanei, non con massimi indipendenti:

$$
\eta(t)=\frac{M(t)}{M_{Rd}(N(t))}
$$

L'inviluppo operativo diventa allora il massimo di $\eta(t)$, non il massimo astratto del momento. Questa impostazione e particolarmente utile nei modelli non lineari, dove rigidezza, dissipazione e ridistribuzione cambiano durante la storia temporale. I riferimenti normativi da citare quando si impostano analisi dinamiche e combinazioni sismiche sono D.M. 17 gennaio 2018, Capitoli 3 e 7, Circolare C.S.LL.PP. 21 gennaio 2019 n. 7 e UNI EN 1998-2:2011.

Riferimenti normativi e bibliografici utilizzati:

- D.M. 17 gennaio 2018, "Aggiornamento delle Norme tecniche per le costruzioni", Ministero delle Infrastrutture e dei Trasporti, Supplemento ordinario alla Gazzetta Ufficiale n. 42 del 20 febbraio 2018.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, "Istruzioni per l'applicazione dell'Aggiornamento delle Norme tecniche per le costruzioni di cui al decreto ministeriale 17 gennaio 2018".
- UNI EN 1990:2006, Eurocodice - Criteri generali di progettazione strutturale.
- UNI EN 1991-2:2005, Eurocodice 1 - Azioni sulle strutture - Parte 2: Carichi da traffico sui ponti.
- UNI EN 1992-1-1:2015, Eurocodice 2 - Progettazione delle strutture di calcestruzzo.
- UNI EN 1993-1-1:2014, Eurocodice 3 - Progettazione delle strutture di acciaio.
- UNI EN 1994-2:2006, Eurocodice 4 - Progettazione delle strutture composte acciaio-calcestruzzo - Ponti.
- UNI EN 1997-1:2013, Eurocodice 7 - Progettazione geotecnica - Parte 1: Regole generali.
- UNI EN 1998-2:2011, Eurocodice 8 - Progettazione delle strutture per la resistenza sismica - Ponti.
