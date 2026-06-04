---
layout: post
title: "Confronto tra modello di plinto rigido e modello FEM a graticcio"
description: "Riferimento tecnico StruHub con formule, ipotesi di modello e riferimenti normativi."
date: 2026-01-08
order: 8
permalink: /posts/plinto-rigido-fem-graticcio.html
meta: "Quaderno tecnico · 1361 parole circa"
---

**Due ipotesi sulla fondazione**

Il plinto rigido mantiene piana la distribuzione degli spostamenti; il graticcio FEM consente deformabilità.

Nel modello rigido:

\[R_i=a+bx_i+cy_i\]

Nel modello flessibile:

\[R_i=k_{v,i}w_i, qquad Ku=F\]

Esempio.

Se il modello rigido dà (R_{max}=1250,mathrm{kN}) e il FEM (R_{max}=1380,mathrm{kN}):

\[Delta=\frac{1380-1250}{1250}=0.104\]

La differenza è circa 10%.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, NTC 2018, Capitolo 6.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7.
- UNI EN 1997-1, Eurocodice 7: Progettazione geotecnica.

Nota StruHub.

Il confronto non serve a complicare il calcolo: serve a capire se l'ipotesi semplificata è adeguata.

Quando il plinto non è abbastanza rigido.

L'ipotesi di plinto rigido è tanto più plausibile quanto maggiore è la rigidezza flessionale del plinto rispetto alla rigidezza verticale dei pali. Se il plinto è sottile o i pali hanno rigidezze molto diverse, le reazioni possono allontanarsi dalla distribuzione lineare.

**Rigidezza relativa**

Un indicatore qualitativo è il confronto tra rigidezza flessionale del graticcio e rigidezza delle molle:

\[\lambda=\frac{EI_{plinto}}{k_v s^4}\]

dove \(s\) è un interasse caratteristico. Valori bassi suggeriscono maggiore deformabilità relativa.

**Lettura del confronto**

Se il FEM aumenta molto la reazione su un palo rispetto al modello rigido, il progetto dovrebbe usare il valore FEM o almeno giustificare perché il modello rigido resta rappresentativo.

Nel tema di il confronto plinto rigido e FEM a graticcio, il dato geotecnico non è un parametro accessorio. Stratigrafia, falda, tensioni efficaci e rigidezze del terreno determinano la domanda e la capacità tanto quanto la geometria dell'opera.

**Tensioni efficaci e stratigrafia**

La tensione efficace è la grandezza che rende leggibile la risposta del terreno:

\[\sigma'_v(z)=\sigma_v(z)-u(z)\]

\[\gamma'=\gamma_{sat}-\gamma_w\]

Quando la falda cambia quota, non cambia solo un valore idraulico: cambiano resistenze, spinte, rigidezze e talvolta il meccanismo critico. Per questo ogni calcolo geotecnico deve mostrare come si costruisce (\sigma'_v(z)).

Domanda, capacità e margine. La forma finale della verifica dovrebbe essere un rapporto domanda-capacità:

\[\eta=\frac{E_d}{R_d}\]

**ma il rapporto va accompagnato dalla scomposizione dei contributi**

Nel caso di fondazione su pali, il valore massimo o minimo non è sufficiente: servono diagrammi, stratigrafia e posizione del punto critico.

**Esempio di stratigrafia**

Si consideri uno strato superficiale di 3 m con (\gamma=18\,\mathrm{kN/m^3}), seguito da uno strato saturo con (\gamma_{sat}=20\,\mathrm{kN/m^3}). Con falda a 3 m, a 6 m di profondità:

\[\sigma_v=18\cdot3+20\cdot3=114\,\mathrm{kPa}\]

\[u=9.81\cdot3=29.4\,\mathrm{kPa}\]

\[\sigma'_v=84.6\,\mathrm{kPa}\]

Questo valore entra poi nelle formule di capacità, spinta o rigidezza. Se la falda fosse più alta, il risultato cambierebbe in modo diretto.

Lettura da progettista. Il modello rigido è un’ipotesi, non una verità. Confrontarlo con un graticcio FEM consente di capire se quella ipotesi è ancora sostenibile. Un riferimento tecnico deve quindi dichiarare le ipotesi geotecniche con la stessa cura delle formule strutturali. Il terreno non è lo sfondo del modello: è parte del modello.

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

Plinto rigido: ipotesi e limiti. Il modello di plinto rigido assume che la testa dei pali resti su un piano. In questa ipotesi, gli spostamenti verticali dei pali sono compatibili con una traslazione e due rotazioni del plinto. La forza nel palo dipende dalla posizione nel gruppo e dalla rigidezza assiale:

\[N_i=k_i\left(w+\theta_x y_i+\theta_y x_i\right)\]

Se tutti i pali hanno rigidezza simile, si ottiene la classica ripartizione lineare. Il vantaggio e la trasparenza: il progettista vede subito quali pali sono caricati di piu e come l'eccentricita influenza il gruppo. Il limite e che il plinto reale puo flettersi, soprattutto se ha grandi dimensioni, spessori ridotti, carichi concentrati o disposizione irregolare dei pali.

Un plinto rigido produce spesso una distribuzione piu ordinata di quella reale. Se la platea si deforma, le reazioni possono concentrarsi vicino alle zone caricate e ridursi altrove. Questa differenza puo influenzare sia la verifica dei pali sia l'armatura del plinto.

Modello FEM a graticcio. Un modello FEM a graticcio o piastra consente di rappresentare la deformabilita del plinto. I pali possono essere inseriti come molle verticali e, se necessario, molle orizzontali o vincoli elastici. La rigidezza assiale equivalente del palo puo essere stimata come:

\[k_p=\frac{E_p A_p}{L_{eq}}\]

Il valore \(L_{eq}\) non e sempre la lunghezza geometrica: dipende dalla mobilitazione del terreno e dal modello di interazione. La scelta della rigidezza delle molle e quindi uno dei punti piu delicati.

Il confronto tra modello rigido e FEM non deve servire a scegliere il risultato piu favorevole, ma a capire la sensibilita. Se le reazioni cambiano poco, l'ipotesi rigida e robusta. Se cambiano molto, il plinto ha un ruolo strutturale attivo e va verificato con un modello deformabile. Per fondazioni profonde e plinti, i riferimenti sono D.M. 17 gennaio 2018, Capitoli 4 e 6, Circolare C.S.LL.PP. 21 gennaio 2019 n. 7, UNI EN 1992-1-1:2015 e UNI EN 1997-1:2013.

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
