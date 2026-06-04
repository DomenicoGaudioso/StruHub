---
layout: post
title: "Effetti delle fasi costruttive nelle sezioni composte da ponte"
description: "Riferimento tecnico StruHub con formule, ipotesi di modello e riferimenti normativi."
date: 2026-01-11
order: 11
permalink: /posts/fasi-costruttive-sezioni-composte.html
meta: "Quaderno tecnico · 1432 parole circa"
---

**La sezione resistente cambia nel tempo**

Il peso della carpenteria può agire sulla sola sezione metallica, mentre i permanenti successivi agiscono sulla sezione composta.

\[sigma_{tot}=sigma_{G1,acciaio}+sigma_{G2,composta}+sigma_Q+sigma_{ritiro}+sigma_T\]

Modulo efficace.

\[E_{c,eff}=\frac{E_c}{1+ arphi(t,t_0)}\]

Esempio.

Con (M_{G1}=1200,mathrm{kNm}), (W_s=0.045,mathrm{m^3}):

\[sigma_{G1}=26.7,mathrm{MPa}\]

Con (M_{G2}=900,mathrm{kNm}), (W_c=0.080,mathrm{m^3}):

\[sigma_{G2}=11.25,mathrm{MPa}\]

Riferimenti tecnici utilizzati:

- UNI EN 1994-2, Eurocodice 4.
- UNI EN 1992-1-1, Eurocodice 2.
- D.M. 17 gennaio 2018, NTC 2018.

Nota StruHub.

La fase costruttiva è una informazione meccanica: StruHub la mantiene separata nella somma degli effetti.

Il punto delicato: non sommare inerzie sbagliate. Il rischio più frequente nelle fasi costruttive è sommare momenti prodotti in fasi diverse usando un unico modulo resistente. Il momento da peso della carpenteria deve essere letto sulla sezione metallica; il momento da carichi successivi sulla sezione composta, se la collaborazione è attiva.

**Ritiro e redistribuzione**

Il ritiro del calcestruzzo introduce una deformazione imposta nella soletta. Se la soletta è connessa all'acciaio, questa deformazione genera tensioni autoequilibrate. Una lettura semplificata considera un'azione assiale equivalente:

\[N_{cs}=E_c A_c \varepsilon_{cs}\]

Il valore effettivo dipende dalla viscosità e dalla rigidezza relativa dei materiali.

**Ordine cronologico del calcolo**

Un buon articolo di calcolo dovrebbe mostrare la cronologia: sezione resistente, azione applicata, tensione prodotta, contributo accumulato. Questo rende verificabile la storia tensionale finale.

Per le fasi costruttive della sezione composta, la sezione non può essere letta come un oggetto unico e immutabile. Materiali diversi, fasi costruttive, deformazioni imposte e limiti tensionali obbligano a ragionare per fibre, per tempo e per caso di carico.

**Dal modello geometrico al modello resistente**

La prima operazione è definire quali parti della sezione partecipano alla resistenza in una certa fase. In una sezione composta, ad esempio, il calcestruzzo può non essere collaborante durante il getto o può collaborare con modulo efficace ridotto a lungo termine.

\[n=\frac{E_s}{E_c}\]

\[\sigma(y)=\frac{N}{A}\pm\frac{M}{I}y\]

Queste formule sono semplici solo se (A), (I) e (y) sono quelli della sezione corretta. Usare proprietà sezionali finali per carichi applicati in una fase precedente è un errore concettuale, non un'approssimazione innocua.

**Fibre di controllo**

La verifica deve scegliere fibre significative: estradosso soletta, intradosso acciaio, interfaccia, lembi estremi e punti in cui cambiano materiale o armatura. Per ogni fibra conviene separare i contributi:

\[\sigma_{tot}=\sigma_N+\sigma_M+\sigma_{imp}+\sigma_{fase}\]

Questa scomposizione rende comprensibile il risultato. Se la tensione finale supera un limite, si può capire se il problema nasce da un permanente, da una fase costruttiva, da temperatura o da ritiro.

**Esempio di lettura**

Si consideri una fibra inferiore con tre contributi: (+32\,\mathrm{MPa}) da peso proprio, (+18\,\mathrm{MPa}) da carichi permanenti successivi e (-6\,\mathrm{MPa}) da effetto imposto. La tensione finale è:

\[\sigma_{tot}=32+18-6=44\,\mathrm{MPa}\]

Il dato finale da solo non spiega nulla; la scomposizione mostra invece che il peso proprio è il contributo dominante. Questo orienta il progettista verso la fase costruttiva corretta.

Impostazione da riferimento. Il punto essenziale è non usare un’unica sezione resistente per carichi applicati in tempi diversi. La storia costruttiva è parte della meccanica. Un post tecnico dovrebbe quindi fornire metodo, non solo teoria: definizione delle fibre, tabella dei contributi, limiti, segno delle tensioni e interpretazione finale.

Dal modello globale alla verifica locale. Nei temi di impalcato, sezioni composte e azioni di progetto, il passaggio più importante è collegare una grandezza globale a un controllo locale. Un momento globale lungo l'impalcato non è ancora una verifica di fibra; un carico da traffico non è ancora una tensione; un coefficiente di ripartizione non è ancora una domanda resistente. Il calcolo diventa affidabile quando ogni trasformazione è esplicita.

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

La sezione non nasce composta. Negli impalcati composti, la sequenza costruttiva determina lo stato tensionale finale. Una trave metallica varata e caricata dal getto della soletta lavora inizialmente da sola; solo dopo maturazione, connessione e solidarizzazione la sezione diventa composta. Per questo una verifica corretta non puo applicare tutte le azioni alla sezione finale. Deve invece scomporre la storia in fasi.

Se \(M_1\) e il momento applicato prima della collaborazione e \(M_2\) quello applicato dopo, la tensione nell'acciaio puo essere letta come:

\[\sigma_s = \frac{M_1 y_s}{I_s}+\frac{M_2 y_s^*}{I_{comp}^*}\]

dove \(I_s\) e l'inerzia della sola carpenteria, mentre \(I_{comp}^*\) e l'inerzia omogeneizzata della sezione composta. La formula e volutamente semplice, ma mette in evidenza il punto essenziale: momenti diversi interrogano sezioni resistenti diverse.

Le fasi influenzano anche la deformata. Una controfreccia impostata sulla trave metallica puo compensare il peso proprio e il getto, ma non elimina gli effetti successivi di ritiro, viscosita, sovraccarichi permanenti e traffico. La freccia finale e una somma di contributi:

\[w_{tot}=w_{acciaio}+w_{comp}+w_{visc}+w_{ritiro}+w_{variabili}\]

Connessione e trasferimento di taglio. La collaborazione richiede trasferimento di taglio all'interfaccia. Il flusso di taglio longitudinale puo essere stimato con:

\[q_l=\frac{V S}{I}\]

Questo valore guida la progettazione dei connettori e la verifica della collaborazione. Se la connessione e parziale, la distribuzione delle tensioni cambia e il modello elastico a piena collaborazione non e piu sufficiente. Nei ponti, inoltre, la fatica dei connettori puo diventare rilevante per effetto dei cicli di traffico.

La lettura editoriale corretta e mostrare la cronologia: varo, getto, maturazione, solidarizzazione, pavimentazione, traffico, effetti lenti. Ogni fase aggiunge azioni e modifica il quadro tensionale. Quando si citano criteri per sezioni composte, i riferimenti sono D.M. 17 gennaio 2018, Capitoli 4 e 5, UNI EN 1994-2:2006, UNI EN 1993-1-1:2014 e UNI EN 1992-1-1:2015.

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
