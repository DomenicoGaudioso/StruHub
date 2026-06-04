---
layout: post
title: "Ripartizione trasversale dei carichi negli impalcati da ponte"
description: "Riferimento tecnico StruHub con formule, ipotesi di modello e riferimenti normativi."
date: 2026-01-14
order: 14
permalink: /posts/ripartizione-trasversale-carichi-impalcati.html
meta: "Quaderno tecnico · 1349 parole circa"
---

**Distribuzione trasversale**

Un carico applicato su una corsia non viene assorbito da una sola trave. Soletta, traversi e torsione trasferiscono quota della domanda alle travi vicine.

\[E_j=eta_jE_{tot}, qquad sum_jeta_japprox1\]

Rigidezze equivalenti.

\[D_x=\frac{EI_x}{b}, qquad D_y=\frac{EI_y}{l}\]

Esempio.

Con (M_{tot}=1200,mathrm{kNm}) e coefficienti (0.25,0.50,0.25):

\[M_1=300,quad M_2=600,quad M_3=300,mathrm{kNm}\]

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, NTC 2018, Capitolo 5.
- UNI EN 1991-2, Eurocodice 1: Azioni da traffico sui ponti.

Nota StruHub.

La ripartizione trasversale è il ponte tra modello globale e verifica locale delle travi principali.

**Compatibilità oltre l'equilibrio**

La somma dei coefficienti di ripartizione garantisce l'equilibrio globale, ma non basta a descrivere il comportamento dell'impalcato. La ripartizione nasce anche dalla compatibilità degli spostamenti trasversali: una trave caricata trascina le altre attraverso la soletta e gli elementi trasversali.

In termini concettuali, la distribuzione non è solo:

\[\sum_j V_j=V_{tot}\]

ma anche coerenza tra deformate:

\[w_j(x)\approx w_{j+1}(x) \quad \text{se la soletta è molto rigida trasversalmente}\]

**Effetto della torsione**

La rigidezza torsionale modifica la capacità dell'impalcato di distribuire un carico eccentrico. Un sistema con bassa torsione tende a concentrare la domanda vicino alla linea di carico; un sistema più torsionalmente rigido la spalma in modo più uniforme.

Nel tema di la ripartizione trasversale dei carichi, il punto non è ottenere un coefficiente isolato, ma costruire una catena coerente tra modello globale e verifica locale. Il carico applicato al impalcato a travi e soletta attraversa una gerarchia resistente: soletta, traversi, travi principali, appoggi e infine sottostrutture.

Il primo controllo è l'equilibrio. Se gli effetti ripartiti non ricostruiscono l'azione globale, il modello non è accettabile. Il secondo controllo è la compatibilità: gli elementi connessi non possono deformarsi come sistemi indipendenti se la soletta o i collegamenti impongono continuità.

\[\sum_j E_j \simeq E_{tot}\]

**Rigidezze equivalenti e sensibilità**

La ripartizione dipende dai rapporti di rigidezza, non dai soli interassi geometrici. Un elemento molto rigido attira più domanda, mentre un elemento flessibile tende a scaricare verso gli elementi vicini. Questo vale per travi, piastre equivalenti e modelli a graticcio.

\[D_x \propto EI_x, \qquad D_y \propto EI_y, \qquad D_{xy} \propto GJ\]

Una variazione del 20% nella rigidezza trasversale può modificare in modo significativo il coefficiente di ripartizione. Per questo l'articolo tecnico non deve limitarsi a presentare il risultato: deve spiegare quali parametri lo muovono.

Esempio di confronto. Si consideri un effetto globale (E_{tot}=1000\,\mathrm{kNm}). Un primo modello fornisce coefficienti (0.20,0.35,0.35,0.10); un secondo modello, con maggiore rigidezza trasversale, fornisce (0.24,0.29,0.29,0.18). I due risultati sono entrambi equilibrati, ma raccontano impalcati diversi.

\[E_{j}=\eta_j E_{tot}\]

Nel primo caso la domanda è concentrata sulle travi interne; nel secondo la distribuzione è più uniforme. La scelta progettuale deve leggere questa differenza, non solo copiare il massimo valore.

**Uso nella relazione tecnica**

Una relazione ben costruita dovrebbe riportare schema dell'impalcato, rigidezze equivalenti, posizione dei carichi, coefficienti ottenuti e controllo di equilibrio. Se il metodo è semianalitico, è utile affiancare un confronto con un modello graticcio o FEM semplificato.

La ripartizione trasversale nasce dall’equilibrio ma anche dalla compatibilità. Una soletta rigida obbliga travi vicine a deformarsi insieme; una soletta più deformabile lascia il carico più concentrato. In questo modo il post diventa un riferimento tecnico: non propone soltanto un numero, ma insegna a giudicarlo.

**Dal modello globale alla verifica locale**

Nei temi di impalcato, sezioni composte e azioni di progetto, il passaggio più importante è collegare una grandezza globale a un controllo locale. Un momento globale lungo l'impalcato non è ancora una verifica di fibra; un carico da traffico non è ancora una tensione; un coefficiente di ripartizione non è ancora una domanda resistente. Il calcolo diventa affidabile quando ogni trasformazione è esplicita.

La sequenza tipica è: definizione dell'azione, scelta dello schema resistente, calcolo dell'effetto globale, ripartizione o trasformazione sezionale, verifica della fibra o dell'elemento. Saltare uno di questi passaggi produce risultati difficili da revisionare.

Tracciabilità delle combinazioni. Ogni valore dovrebbe conservare l'origine. Se una sezione ha (M_{max}=950\,\mathrm{kNm}), la relazione deve indicare se deriva da traffico, temperatura, fase costruttiva, ritiro o combinazione sismica. In assenza di questa informazione il dato è numericamente utile ma tecnicamente debole.

Una forma pratica di archiviazione è associare a ogni effetto tre campi: valore, combinazione governante e posizione. Per gli inviluppi:

\[E_{max}(x_j)=E_{d,k}(x_j) \quad con \quad k=k_{gov}(x_j)\]

Questa scrittura chiarisce che il massimo non è astratto: è prodotto da una combinazione precisa.

Controllo delle rigidezze. Quando si lavora con ripartizioni, sezioni composte o fasi costruttive, la rigidezza è il parametro più influente. Una rigidezza sbagliata può produrre una distribuzione apparentemente equilibrata ma fisicamente non corretta. Per questo, prima della verifica, conviene riportare le proprietà principali: area, inerzia, modulo elastico, coefficiente di omogeneizzazione e fase resistente.

Per una sezione composta, ad esempio, una stessa azione può produrre tensioni diverse se applicata prima o dopo la maturazione della soletta. Il valore finale è la somma di contributi nati in tempi diversi:

\[\sigma_{finale}=\sum_i \sigma_i(A_i,I_i,E_i,t_i)\]

La notazione evidenzia che ogni contributo ha proprietà resistenti proprie.

Esempio di lettura critica. Si consideri un impalcato con quattro travi. Un modello semplificato fornisce coefficienti (0.18,0.32,0.32,0.18), mentre un modello più rigido trasversalmente fornisce (0.22,0.28,0.28,0.22). Entrambi rispettano l'equilibrio, ma il secondo distribuisce maggiormente verso le travi laterali. Se la verifica locale di una trave laterale è vicina al limite, questa differenza non è accademica.

Scrittura da riferimento tecnico. Il testo deve accompagnare il lettore dalla causa all'effetto: quale azione entra, come viene distribuita, quale proprietà resistente la trasforma in tensione o sollecitazione, quale limite viene controllato. Questa catena rende l'articolo consultabile anche a distanza di tempo.

Il significato della ripartizione trasversale. La ripartizione trasversale dei carichi nasce da una domanda pratica: quanto del carico applicato su una corsia viene assorbito da ciascuna trave longitudinale? In un impalcato reale la risposta non e quella di travi isolate. Soletta, traversi, diaframmi e rigidezza torsionale trasferiscono parte dell'azione alle travi vicine. La quota assegnata a una trave puo essere espressa tramite un coefficiente di ripartizione \(\alpha_i\):

\[Q_i=\alpha_i Q \qquad \sum_i \alpha_i \approx 1\]

La difficolta e stimare \(\alpha_i\) in modo coerente con geometria, rigidezze e posizione del carico. Nei ponti a piu travi, una stima troppo semplificata puo sovraccaricare o sottovalutare le travi di bordo, soprattutto quando la carreggiata consente eccentricita elevate del carico mobile.

Un modello a graticcio rappresenta le travi longitudinali e gli elementi trasversali come aste, con rigidezze flessionali e torsionali equivalenti. La soletta non viene descritta punto per punto, ma trasferisce rigidezza tramite elementi trasversali. La scelta delle rigidezze equivalenti e il punto sensibile: se i traversi sono troppo rigidi, il modello distribuisce eccessivamente; se sono troppo deformabili, il carico resta confinato nella trave caricata.

Rigidezza relativa e posizione del carico. Per capire la ripartizione e utile ragionare in termini di rigidezza relativa. Se \(k_i\) e la rigidezza verticale equivalente della trave \(i\), una prima approssimazione elastica puo essere:

\[\alpha_i \simeq \frac{k_i}{\sum_j k_j}\]

Questa formula e solo orientativa, perche ignora l'eccentricita del carico e la torsione dell'impalcato, ma chiarisce un punto: la ripartizione non e una percentuale geometrica, e una conseguenza della compatibilita degli spostamenti. Quando il carico e eccentrico, alla ripartizione verticale si somma un effetto torsionale. Per una risultante \(Q\) applicata con eccentricita \(e_q\), il momento torcente globale e:

\[T = Q e_q\]

Le travi di bordo diventano allora decisive. Un controllo tecnico dovrebbe confrontare almeno tre scenari: carico centrato, carico eccentrico verso un bordo, carico distribuito su piu corsie. Quando si parla di carichi da traffico sui ponti, la citazione normativa corretta include D.M. 17 gennaio 2018, Capitolo 5, e UNI EN 1991-2:2005.

La lettura finale non e il singolo coefficiente, ma l'inviluppo delle sollecitazioni nelle travi. Se il modello e ben impostato, il progettista riesce a distinguere l'effetto locale del carico mobile dall'effetto globale di torsione dell'impalcato.

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
