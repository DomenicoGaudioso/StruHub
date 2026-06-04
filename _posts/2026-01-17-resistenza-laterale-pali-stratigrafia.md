---
layout: post
title: "Valutazione della resistenza laterale di pali in terreni stratificati"
description: "Riferimento tecnico StruHub con formule, ipotesi di modello e riferimenti normativi."
date: 2026-01-17
order: 17
permalink: /posts/resistenza-laterale-pali-stratigrafia.html
meta: "Quaderno tecnico · 1536 parole circa"
---

**Palo come trave nel terreno**

Un palo soggetto ad azioni orizzontali si deforma interagendo con il terreno lungo la profondità.

\[p(z)=k_h(z)y(z)\]

\[EI,y''''(z)+k_h(z)y(z)=q(z)\]

Stratigrafia.

\[k_h(z)=k_{h,i}quad z_i<z<z_{i+1}\]

Il cambio di rigidezza tra strati modifica spostamenti e momenti.

Esempio.

Uno strato superficiale con (k_h=8000,mathrm{kN/m^3}) sopra uno strato con (k_h=35000,mathrm{kN/m^3}) produce una risposta governata dagli spostamenti nella parte alta e dalla chiusura della deformata in profondità.

Il controllo SLE può essere scritto come (y_{testa}le y_{lim}).

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, NTC 2018, Capitolo 6.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7.
- UNI EN 1997-1, Eurocodice 7: Progettazione geotecnica.

Nota StruHub.

StruHub confronta differenze finite e FEM beam per leggere la risposta laterale del palo come problema di interazione terreno-struttura.

**Momento massimo non sempre in testa**

Nel palo caricato lateralmente il massimo momento può trovarsi sotto il piano campagna, dove la reazione del terreno cambia il segno della curvatura. Per questo i diagrammi lungo profondità sono più informativi del solo spostamento in testa.

Le grandezze da leggere sono:

\[y(z), \qquad M(z)=-EIy''(z), \qquad V(z)=\frac{dM}{dz}\]

Curve p-y. Il modello lineare \(p=k_hy\) è una prima approssimazione. In terreni reali la reazione può essere non lineare:

\[p=p(y,z)\]

Le curve \(p-y\) distinguono sabbie, argille e condizioni drenate o non drenate. Anche quando si usa un modello lineare, è utile sapere quale comportamento non lineare si sta semplificando.

Nel tema di la risposta laterale del palo, il dato geotecnico non è un parametro accessorio. Stratigrafia, falda, tensioni efficaci e rigidezze del terreno determinano la domanda e la capacità tanto quanto la geometria dell'opera.

**Tensioni efficaci e stratigrafia**

La tensione efficace è la grandezza che rende leggibile la risposta del terreno:

\[\sigma'_v(z)=\sigma_v(z)-u(z)\]

\[\gamma'=\gamma_{sat}-\gamma_w\]

Quando la falda cambia quota, non cambia solo un valore idraulico: cambiano resistenze, spinte, rigidezze e talvolta il meccanismo critico. Per questo ogni calcolo geotecnico deve mostrare come si costruisce (\sigma'_v(z)).

Domanda, capacità e margine. La forma finale della verifica dovrebbe essere un rapporto domanda-capacità:

\[\eta=\frac{E_d}{R_d}\]

**ma il rapporto va accompagnato dalla scomposizione dei contributi**

Nel caso di palo in terreno stratificato, il valore massimo o minimo non è sufficiente: servono diagrammi, stratigrafia e posizione del punto critico.

**Esempio di stratigrafia**

Si consideri uno strato superficiale di 3 m con (\gamma=18\,\mathrm{kN/m^3}), seguito da uno strato saturo con (\gamma_{sat}=20\,\mathrm{kN/m^3}). Con falda a 3 m, a 6 m di profondità:

\[\sigma_v=18\cdot3+20\cdot3=114\,\mathrm{kPa}\]

\[u=9.81\cdot3=29.4\,\mathrm{kPa}\]

\[\sigma'_v=84.6\,\mathrm{kPa}\]

Questo valore entra poi nelle formule di capacità, spinta o rigidezza. Se la falda fosse più alta, il risultato cambierebbe in modo diretto.

**Lettura da progettista**

Nel carico laterale il massimo momento può essere sotto il piano campagna. Guardare solo lo spostamento in testa significa perdere metà del problema. Un riferimento tecnico deve quindi dichiarare le ipotesi geotecniche con la stessa cura delle formule strutturali. Il terreno non è lo sfondo del modello: è parte del modello.

Costruzione del modello geotecnico. Nei problemi geotecnici la parte più importante non è la formula finale, ma la costruzione del profilo di calcolo. Stratigrafia, falda, parametri drenati o non drenati e rigidezze non possono essere trattati come valori intercambiabili. Ogni strato contribuisce in modo diverso alla domanda e alla capacità.

Un profilo minimo dovrebbe riportare quota superiore e inferiore, peso di volume, condizioni di falda, angolo di attrito, coesione o resistenza non drenata, modulo o coefficiente di reazione. Solo dopo questa tabella ha senso applicare formule di capacità, spinta o deformabilità.

Tensioni efficaci come filo conduttore. Molte grandezze geotecniche dipendono dalla tensione efficace. Per questo è utile costruire esplicitamente il diagramma:

\[\sigma'_v(z)=\sigma_v(z)-u(z)\]

e non limitarsi a un valore alla quota di interesse. Nel caso dei pali, la tensione efficace alla punta influenza (Q_b), mentre quella lungo il fusto influenza (Q_s). Nel caso delle opere di sostegno, la tensione efficace entra nella spinta del terreno.

Domanda, capacità e deformabilità. Un calcolo geotecnico completo dovrebbe distinguere tra capacità ultima e risposta in esercizio. Un palo può avere capacità assiale sufficiente ma spostamento laterale eccessivo; un gruppo di pali può rispettare la portanza media ma avere un palo in trazione; un plinto può essere verificato come rigido ma mostrare una distribuzione FEM più gravosa.

Il rapporto domanda-capacità è utile:

\[\eta=\frac{E_d}{R_d}\]

ma deve essere affiancato alla grandezza deformativa, ad esempio spostamento, rotazione o cedimento.

Esempio di sensitività. Se la falda passa da 5 m a 2 m dal piano campagna, la tensione efficace a 8 m può ridursi sensibilmente. Una riduzione di (\sigma'_v) del 20% in un terreno granulare può ridurre direttamente la quota di resistenza di punta calcolata con (N_q\sigma'_vA_b). Questo mostra perché la falda non è un dettaglio di input, ma una variabile meccanica.

Scrittura da riferimento tecnico. Un articolo geotecnico solido deve sempre mostrare il percorso: dati di terreno, tensioni efficaci, formula, risultato, margine. Se manca uno di questi passaggi, il lettore non può ricostruire il calcolo.

Palo caricato lateralmente. La resistenza laterale di un palo in terreno stratificato dipende dall'interazione palo-terreno lungo la profondita. Il modello piu semplice tratta il palo come una trave su suolo elastico, con reazione proporzionale allo spostamento:

\[p(z)=k_h(z)y(z)\]

dove \(p(z)\) e la reazione del terreno per unita di lunghezza, \(k_h(z)\) il modulo di reazione orizzontale e \(y(z)\) lo spostamento laterale. L'equazione della trave diventa:

\[E I \frac{d^4 y}{dz^4}+k_h(z)y=0\]

Se il terreno e stratificato, \(k_h\) non e costante. Strati soffici superficiali possono governare gli spostamenti anche quando strati profondi offrono resistenza elevata. Per questo un palo laterale non va giudicato solo dalla capacita ultima globale: la deformabilita laterale puo essere il criterio dominante.

Nei modelli piu avanzati si usano curve \(p-y\), che introducono non linearita e limite di reazione del terreno. La relazione non e piu una retta, ma una curva che rappresenta la mobilitazione progressiva della resistenza. In termini concettuali:

\[p=f(y,z)\]

Questo e importante nei ponti, dove frenatura, sisma, urti o azioni idrauliche possono generare forze orizzontali significative sulle fondazioni profonde.

Stratigrafia e lunghezza efficace. La profondita realmente coinvolta dalla risposta laterale non coincide sempre con la lunghezza totale del palo. Una parte superiore del terreno puo governare rotazioni e momenti massimi; la parte profonda puo lavorare piu come vincolo elastico. Il momento nel palo e legato alla curvatura:

\[M(z)=E I \frac{d^2 y}{dz^2}\]

Il massimo momento si trova spesso nei primi diametri sotto il piano campagna, ma la posizione dipende da testa libera o incastrata, rigidezza del palo e profilo geotecnico. Una verifica tecnica dovrebbe quindi riportare spostamento di testa, rotazione, momento massimo, taglio e reazioni per strato.

Quando si cita la progettazione geotecnica dei pali, i riferimenti sono D.M. 17 gennaio 2018, Capitolo 6, Circolare C.S.LL.PP. 21 gennaio 2019 n. 7 e UNI EN 1997-1:2013. Per azioni sismiche su fondazioni e ponti occorre richiamare anche il Capitolo 7 delle NTC 2018 e UNI EN 1998-2:2011 quando pertinente.

Controllo dimensionale. Un riferimento tecnico deve permettere al lettore di controllare gli ordini di grandezza. Ogni risultato numerico dovrebbe essere accompagnato da un controllo dimensionale, da una interpretazione fisica e da una indicazione del parametro dominante. Se una formula restituisce un valore in \(\mathrm{kN}\), \(\mathrm{kNm}\), \(\mathrm{kPa}\) o \(\mathrm{mm}\), l'unita deve essere esplicita e coerente con le grandezze inserite.

La forma generale di una verifica puo essere letta come:

\[\eta=\frac{E_d}{R_d} \le 1\]

dove \(E_d\) e l'effetto dell'azione di progetto e \(R_d\) la resistenza di progetto. Questa scrittura, comune a molti problemi strutturali e geotecnici, aiuta a separare domanda, capacita e margine.

Dal calcolo alla decisione. Il valore finale non basta. Bisogna chiedersi da quale ipotesi dipende, quanto e sensibile ai parametri e quale meccanismo fisico rappresenta. Un margine ottenuto con parametri poco tracciabili ha meno valore di un margine piu modesto ma ben documentato. Per questo, quando si cita una norma o un criterio di combinazione, il riferimento deve essere scritto in modo esplicito e verificabile.

Controllo dimensionale. Un riferimento tecnico deve permettere al lettore di controllare gli ordini di grandezza. Ogni risultato numerico dovrebbe essere accompagnato da un controllo dimensionale, da una interpretazione fisica e da una indicazione del parametro dominante. Se una formula restituisce un valore in \(\mathrm{kN}\), \(\mathrm{kNm}\), \(\mathrm{kPa}\) o \(\mathrm{mm}\), l'unita deve essere esplicita e coerente con le grandezze inserite.

La forma generale di una verifica puo essere letta come:

\[\eta=\frac{E_d}{R_d} \le 1\]

dove \(E_d\) e l'effetto dell'azione di progetto e \(R_d\) la resistenza di progetto. Questa scrittura, comune a molti problemi strutturali e geotecnici, aiuta a separare domanda, capacita e margine.

Dal calcolo alla decisione. Il valore finale non basta. Bisogna chiedersi da quale ipotesi dipende, quanto e sensibile ai parametri e quale meccanismo fisico rappresenta. Un margine ottenuto con parametri poco tracciabili ha meno valore di un margine piu modesto ma ben documentato. Per questo, quando si cita una norma o un criterio di combinazione, il riferimento deve essere scritto in modo esplicito e verificabile.

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
