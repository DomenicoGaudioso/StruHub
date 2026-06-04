---
layout: post
title: "Valutazione degli effetti dell'isolamento sismico negli impalcati da ponte"
description: "Riferimento tecnico StruHub con formule, ipotesi di modello e riferimenti normativi."
date: 2026-01-15
order: 15
permalink: /posts/isolamento-sismico-impalcati.html
meta: "Quaderno tecnico · 1641 parole circa"
---

**Il ruolo meccanico dell'isolamento**

L'isolamento modifica rigidezza, periodo e spostamenti concentrando parte della domanda nei dispositivi.

\[T=2pisqrt{\frac{m}{k_{eff}}}\]

Forza e dissipazione.

\[F_{max}=k_{eff}d_{max}\]

L'energia dissipata in un ciclo è:

\[E_d=oint F,dd\]

Per un FPS, in forma semplificata:

\[Fapprox \frac{N}{R}d+mu N,mathrm{sign}(dot{d})\]

Esempio.

Con (m=2.0cdot10^6,mathrm{kg}) e (k_{eff}=30cdot10^6,mathrm{N/m}):

\[Tapprox1.62,mathrm{s}\]

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme Tecniche per le Costruzioni* (NTC 2018).
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle Norme Tecniche per le Costruzioni*.

Riferimento aggiuntivo.

- UNI EN 1998-2, Eurocodice 8: Ponti.

Nota StruHub.

StruHub tratta isolamento e dissipazione come leggi meccaniche, non come coefficienti correttivi generici.

Spostamento: il prezzo dell'isolamento. L'isolamento riduce le forze trasmesse solo se si accetta un aumento degli spostamenti. Per questo la verifica non può fermarsi al taglio alla base. Lo spostamento del dispositivo diventa una grandezza primaria:

\[d_{max}\leq d_{amm}\]

dove \(d_{amm}\) dipende dalla capacità del dispositivo, dai dettagli costruttivi e dagli spazi disponibili.

**Rigidezza secante e rigidezza tangente**

Nei dispositivi non lineari, la rigidezza non è unica. La rigidezza secante è:

\[k_{sec}=\frac{F_{max}}{d_{max}}\]

La rigidezza tangente è la derivata locale della curva:

\[k_t=\frac{dF}{dd}\]

Confondere le due grandezze può portare a periodi equivalenti non coerenti con la risposta reale.

Per trasformare l’isolamento sismico degli impalcati in un riferimento tecnico è utile separare tre livelli: il modello fisico, il modello numerico e la lettura dei risultati. Il modello fisico identifica masse, rigidezze, smorzamento e azioni. Il modello numerico stabilisce come queste grandezze entrano nell'equazione del moto. La lettura dei risultati decide quali effetti sono davvero utili al progetto.

\[M\ddot{u}(t)+C\dot{u}(t)+Ku(t)=F(t)\]

Nel caso di sistema isolato, la matrice di massa non è un dettaglio secondario. Una massa assegnata al nodo sbagliato modifica frequenze proprie e partecipazione modale; una rigidezza non coerente con la sezione resistente sposta il periodo e altera la domanda dinamica. Prima di discutere il risultato, il modello deve produrre modi plausibili.

**Passaggio modale e significato dei modi**

Il problema agli autovalori consente di estrarre frequenze e deformate. La forma modale non è solo un disegno: indica quali parti della struttura partecipano a un certo tipo di moto. Se la deformata è locale, il modo può essere poco rilevante per la risposta globale ma importante per una sollecitazione locale.

\[K\phi_n=\omega_n^2M\phi_n\]

\[T_n=\frac{2\pi}{\omega_n}\]

La massa partecipante permette di passare da una lista di frequenze a una gerarchia ingegneristica. Un modo con alta partecipazione nella direzione dell'azione è un modo che può governare spostamenti e tagli globali. Modi con partecipazione inferiore possono comunque influenzare accelerazioni o curvature.

**Procedura di calcolo consigliata**

Una procedura robusta per l’isolamento sismico degli impalcati parte da un controllo statico o quasi-statico, prosegue con l'analisi modale, quindi entra nel dominio del tempo solo dopo aver verificato che frequenze e deformate siano coerenti. In questo ordine, l'analisi dinamica diventa un approfondimento controllato, non una scatola nera.

- definizione di geometria, masse e rigidezze;
- estrazione di periodi e forme modali;
- scelta dello smorzamento e del passo temporale;
- applicazione della storia di carico o del segnale;
- estrazione di spostamenti dei dispositivi e forze trasmesse;
- confronto con un controllo semplificato.

**Esempio di controllo numerico**

Supponiamo che il primo controllo restituisca un periodo teorico di (0.55\,\mathrm{s}) e il modello FEM un periodo di (0.92\,\mathrm{s}). La differenza relativa è:

\[\Delta_T=\frac{0.92-0.55}{0.55}=0.67\]

**Uno scarto del 67% non va ignorato**

Può derivare da vincoli troppo flessibili, massa eccessiva, inerzia ridotta o unità non coerenti. Questo tipo di controllo è ciò che distingue un calcolo numerico da un uso passivo del software.

Lettura progettuale. L’isolamento non elimina la domanda: la sposta. Una riduzione del taglio alla base è accettabile solo se lo spostamento richiesto ai dispositivi e i dettagli costruttivi sono compatibili. Un riferimento tecnico deve quindi mostrare non solo la formula, ma il motivo per cui quella formula è stata usata e il modo in cui il risultato viene interpretato. La parte finale del calcolo dovrebbe sempre riportare domanda, capacità, margine e ipotesi principali.

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

Periodo isolato e domanda di spostamento. Il punto centrale dell'isolamento sismico non e ridurre genericamente le sollecitazioni, ma spostare la struttura in una zona dello spettro in cui l'accelerazione efficace e piu bassa, accettando una domanda di spostamento maggiore e controllata. Per un sistema equivalente a un grado di liberta, la rigidezza orizzontale complessiva degli isolatori puo essere letta come \(K_{iso}\), mentre la massa partecipante e \(M\). Il periodo isolato diventa:

\[T_{iso}=2\pi\sqrt{\frac{M}{K_{iso}}}\]

Questa formula semplice contiene gia il criterio progettuale: se \(K_{iso}\) diminuisce, il periodo cresce; se il periodo cresce, la domanda di taglio alla base tende a ridursi, ma il sistema deve poter accomodare spostamenti orizzontali maggiori. Per questo il progetto dell'isolamento non si chiude con il calcolo del taglio, ma richiede sempre il controllo degli spostamenti, dei giunti, degli appoggi, degli arresti laterali e delle condizioni ultime degli isolatori.

La forza equivalente trasmessa all'impalcato puo essere rappresentata, in prima lettura, come \(F=K_{eff}d\). Se il dispositivo dissipa energia, la rigidezza secante \(K_{eff}\) e lo smorzamento equivalente \(\xi_{eff}\) dipendono dal ciclo isteretico. In termini energetici, lo smorzamento equivalente puo essere stimato come:

\[\xi_{eff}=\frac{1}{4\pi}\frac{E_D}{E_S}\]

dove \(E_D\) e l'energia dissipata in un ciclo e \(E_S\) e l'energia elastica associata allo spostamento massimo. Questa relazione aiuta a capire perche due sistemi con la stessa rigidezza secante possono produrre domande diverse: non conta solo dove si sposta il periodo, ma anche quanta energia viene dissipata durante il ciclo.

Gerarchia dei controlli. In un ponte isolato la verifica dovrebbe essere letta su piu livelli. Il primo livello e globale: periodo, taglio alla base, spostamento massimo e distribuzione degli sforzi tra le sottostrutture. Il secondo e locale: escursione degli isolatori, rotazioni ammissibili, stabilita verticale, eventuale trazione, condizioni di appoggio e compatibilita con il pulvino. Il terzo riguarda gli elementi non strutturali: giunti, barriere, cordoli, dispositivi di ritegno e connessioni di servizio. Un progetto formalmente corretto ma privo di questa gerarchia rischia di trasferire il problema dalla pila al dettaglio costruttivo.

Una stima preliminare dello spostamento puo essere ottenuta dallo spettro di spostamento:

\[S_d(T)=S_a(T)\left(\frac{T}{2\pi}\right)^2\]

Questa grandezza deve poi essere amplificata o combinata secondo il modello adottato e le prescrizioni normative applicabili. Quando si citano i criteri di progettazione sismica per ponti, i riferimenti principali sono D.M. 17 gennaio 2018, Capitoli 3 e 7, Circolare C.S.LL.PP. 21 gennaio 2019 n. 7 e UNI EN 1998-2:2011.

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
