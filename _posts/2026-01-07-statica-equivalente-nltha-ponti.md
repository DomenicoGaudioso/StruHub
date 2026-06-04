---
layout: post
title: "Confronto tra analisi statica equivalente e analisi dinamica non lineare per ponti"
description: "Riferimento tecnico StruHub con formule, ipotesi di modello e riferimenti normativi."
date: 2026-01-07
order: 7
permalink: /posts/statica-equivalente-nltha-ponti.html
meta: "Quaderno tecnico · 1635 parole circa"
---

**Due livelli di analisi**

La statica equivalente rappresenta il sisma con forze orizzontali; la NLTHA integra la risposta nel tempo includendo eventuali non linearità.

\[F=mS_a(T)\]

Nel caso non lineare:

\[Mddot{u}+Cdot{u}+R(u,dot{u})=-Mra_g(t)\]

Confronto degli effetti.

\[Delta_V=\frac{V_{NLTHA,max}-V_{stat}}{V_{stat}}\]

Esempio.

Se (V_{stat}=3.2,mathrm{MN}) e (V_{NLTHA,max}=3.8,mathrm{MN}):

\[Delta_V=0.1875\]

La differenza è circa 19%.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme Tecniche per le Costruzioni* (NTC 2018).
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle Norme Tecniche per le Costruzioni*.

Riferimento aggiuntivo.

- UNI EN 1998-2, Eurocodice 8: Ponti.

Nota StruHub.

StruHub usa la statica equivalente come controllo e la dinamica non lineare come approfondimento.

Quando il confronto è significativo. Il confronto tra statica equivalente e NLTHA ha senso solo se le grandezze sono omogenee. Non bisogna confrontare un taglio elastico statico con un taglio massimo ottenuto dopo plasticizzazione senza leggere anche lo spostamento e lo stato del dispositivo o dell'elemento.

Un confronto minimo dovrebbe includere:

- taglio alla base;
- spostamento massimo;
- momento nelle sezioni critiche;
- eventuali deformazioni plastiche o cicli isteretici.

**Rapporto domanda-capacità nel tempo**

Per grandezze verificabili, si può costruire una storia del rapporto:

\[\eta(t)=\frac{E_d(t)}{R_d}\]

Il massimo \(\eta_{max}\) dice se la capacità viene superata; la durata e la ripetizione dei picchi aiutano a leggere la severità della domanda.

Per trasformare il confronto tra statica equivalente e NLTHA in un riferimento tecnico è utile separare tre livelli: il modello fisico, il modello numerico e la lettura dei risultati. Il modello fisico identifica masse, rigidezze, smorzamento e azioni. Il modello numerico stabilisce come queste grandezze entrano nell'equazione del moto. La lettura dei risultati decide quali effetti sono davvero utili al progetto.

\[M\ddot{u}(t)+C\dot{u}(t)+Ku(t)=F(t)\]

Nel caso di ponte soggetto a sisma, la matrice di massa non è un dettaglio secondario. Una massa assegnata al nodo sbagliato modifica frequenze proprie e partecipazione modale; una rigidezza non coerente con la sezione resistente sposta il periodo e altera la domanda dinamica. Prima di discutere il risultato, il modello deve produrre modi plausibili.

**Passaggio modale e significato dei modi**

Il problema agli autovalori consente di estrarre frequenze e deformate. La forma modale non è solo un disegno: indica quali parti della struttura partecipano a un certo tipo di moto. Se la deformata è locale, il modo può essere poco rilevante per la risposta globale ma importante per una sollecitazione locale.

\[K\phi_n=\omega_n^2M\phi_n\]

\[T_n=\frac{2\pi}{\omega_n}\]

La massa partecipante permette di passare da una lista di frequenze a una gerarchia ingegneristica. Un modo con alta partecipazione nella direzione dell'azione è un modo che può governare spostamenti e tagli globali. Modi con partecipazione inferiore possono comunque influenzare accelerazioni o curvature.

**Procedura di calcolo consigliata**

Una procedura robusta per il confronto tra statica equivalente e NLTHA parte da un controllo statico o quasi-statico, prosegue con l'analisi modale, quindi entra nel dominio del tempo solo dopo aver verificato che frequenze e deformate siano coerenti. In questo ordine, l'analisi dinamica diventa un approfondimento controllato, non una scatola nera.

- definizione di geometria, masse e rigidezze;
- estrazione di periodi e forme modali;
- scelta dello smorzamento e del passo temporale;
- applicazione della storia di carico o del segnale;
- estrazione di tagli, spostamenti e stati non lineari;
- confronto con un controllo semplificato.

**Esempio di controllo numerico**

Supponiamo che il primo controllo restituisca un periodo teorico di (0.55\,\mathrm{s}) e il modello FEM un periodo di (0.92\,\mathrm{s}). La differenza relativa è:

\[\Delta_T=\frac{0.92-0.55}{0.55}=0.67\]

**Uno scarto del 67% non va ignorato**

Può derivare da vincoli troppo flessibili, massa eccessiva, inerzia ridotta o unità non coerenti. Questo tipo di controllo è ciò che distingue un calcolo numerico da un uso passivo del software.

Lettura progettuale. Il confronto ha senso solo se si confrontano grandezze omogenee. Un taglio massimo non lineare deve essere letto insieme allo spostamento e allo stato dei dispositivi o delle sezioni. Un riferimento tecnico deve quindi mostrare non solo la formula, ma il motivo per cui quella formula è stata usata e il modo in cui il risultato viene interpretato. La parte finale del calcolo dovrebbe sempre riportare domanda, capacità, margine e ipotesi principali.

Impostazione del modello. Per usare questo tema come riferimento tecnico, conviene separare la modellazione in fasi verificabili. La prima fase è la definizione dello schema resistente: geometria, vincoli, masse, rigidezze e grandezze da estrarre. La seconda fase è la scelta del tipo di analisi: controllo statico, analisi modale, analisi nel tempo o confronto tra più livelli di modello. La terza fase è la lettura critica dei risultati, che non deve limitarsi al valore massimo ma deve includere posizione, segno, combinazione o istante di occorrenza.

Un controllo utile consiste nel confrontare sempre un risultato numerico con una stima semplificata. Per esempio, un periodo ottenuto con FEM dovrebbe essere compatibile con una valutazione manuale basata su massa e rigidezza. Se l'ordine di grandezza non torna, il problema non è nel solutore ma nella rappresentazione fisica del sistema.

La catena minima di controllo può essere scritta così: definizione del modello, verifica delle unità, estrazione delle frequenze, scelta dello smorzamento, applicazione dell'azione, inviluppo degli effetti, interpretazione del margine. Ogni passaggio deve lasciare una traccia riproducibile.

Unità, segni e grandezze da archiviare. Le analisi dinamiche sono molto sensibili alla coerenza delle unità. Massa in chilogrammi, forze in Newton, rigidezze in Newton per metro e accelerazioni in metri al secondo quadrato devono essere trattate nello stesso sistema. Un errore tra massa e peso può spostare il periodo di un fattore significativo.

Per ogni analisi è utile archiviare almeno: periodo fondamentale, masse partecipanti, smorzamento, passo temporale, massimo spostamento, massimo taglio, massimo momento e istante di occorrenza. Se il modello lavora per inviluppi, va salvata anche la sezione in cui ciascun effetto governa.

La grandezza di progetto non è sempre il massimo assoluto. Nelle verifiche accoppiate può essere necessario leggere valori concomitanti. Se il massimo momento si verifica a un certo istante, lo sforzo normale da usare nel dominio di verifica deve essere quello dello stesso istante, non il massimo normale indipendente.

Esempio di controllo incrociato. Supponiamo che un modello dinamico restituisca un taglio massimo alla base pari a (2.80\,\mathrm{MN}), mentre il controllo statico equivalente fornisce (2.35\,\mathrm{MN}). Il rapporto è:

\[r_V=\frac{2.80}{2.35}=1.19\]

Il dato indica una amplificazione del 19%. Questo non significa automaticamente che il modello dinamico sia più corretto, ma segnala che l'effetto dinamico non è trascurabile. A questo punto il tecnico deve controllare se la velocità, il contenuto in frequenza o il segnale sismico sono coerenti con la condizione più gravosa.

Se invece il rapporto fosse inferiore a 1, non si dovrebbe concludere in modo automatico che la dinamica è meno gravosa: potrebbe essere cambiata la grandezza controllata, oppure la massima risposta dinamica potrebbe verificarsi in un'altra sezione.

Come trasformare il risultato in testo tecnico. Un post di riferimento deve chiudere il cerchio: non basta presentare formule e grafici. Bisogna spiegare quale grandezza governa, quale ipotesi è più sensibile e quale controllo manuale consente di validare il risultato. Questa impostazione rende il contenuto utile anche a chi non usa lo stesso software, perché il metodo rimane trasferibile.

Quando l'equivalente statico smette di bastare. L'analisi statica equivalente e utile quando la risposta puo essere rappresentata da una distribuzione di forze coerente con il primo modo e con un comportamento sostanzialmente elastico. Nei ponti, pero, la massa dell'impalcato, la deformabilita delle pile, la presenza di appoggi, ritegni, isolatori o vincoli dissimmetrici possono rendere il comportamento fortemente dipendente dalla dinamica. La forza statica equivalente puo essere scritta, in forma sintetica, come:

\[F_b = M^* S_a(T_1)\]

dove \(M^*\) e la massa partecipante e \(S_a(T_1)\) l'ordinata spettrale associata al periodo principale. Questa forma e chiara, ma nasconde una ipotesi forte: la domanda e concentrata in una forma modale dominante. Se piu modi partecipano, se la torsione e importante o se la risposta entra in campo non lineare, il modello statico diventa una semplificazione da giustificare, non una verita strutturale.

L'analisi dinamica non lineare descrive invece l'evoluzione temporale:

\[M\ddot u(t)+C\dot u(t)+f_s(u,\dot u)= -M r a_g(t)\]

Il termine \(f_s\) non e necessariamente lineare: puo includere plasticizzazioni, isteresi degli isolatori, degrado di rigidezza, attrito, gap o dispositivi di ritegno. La differenza concettuale e netta: nel metodo statico si assegna una domanda; nella time-history si lascia che la domanda emerga dall'interazione tra input, massa, smorzamento e legame resistente.

Uso comparativo dei due livelli di analisi. Un buon articolo tecnico non deve presentare i due metodi come alternativi ideologici. La statica equivalente serve spesso per orientare la progettazione, controllare ordini di grandezza e individuare le sottostrutture piu sollecitate. La dinamica non lineare serve per verificare meccanismi, spostamenti, contemporaneita delle azioni e margini locali. Il confronto tra i due risultati e prezioso: se la time-history produce tagli molto minori ma spostamenti molto maggiori, il messaggio progettuale non e che il ponte e "piu sicuro"; e che il problema si e spostato verso compatibilita e dettagli.

Per valutare la coerenza, si puo confrontare un indicatore di domanda:

\[\rho = \frac{E_{NLTHA}}{E_{stat}}\]

Se \(\rho\) e molto diverso da 1, occorre spiegare il motivo: partecipazione modale, isolamento, non linearita, smorzamento, scaling degli accelerogrammi o distribuzione delle masse. Quando si richiamano analisi sismiche di ponti, i riferimenti principali sono D.M. 17 gennaio 2018, Capitoli 3 e 7, Circolare C.S.LL.PP. 21 gennaio 2019 n. 7 e UNI EN 1998-2:2011.

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
