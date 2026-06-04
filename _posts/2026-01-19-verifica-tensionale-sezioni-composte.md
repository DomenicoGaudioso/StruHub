---
layout: post
title: "Verifica tensionale di sezioni composte acciaio-calcestruzzo"
description: "Riferimento tecnico StruHub con formule, ipotesi di modello e riferimenti normativi."
date: 2026-01-19
order: 19
permalink: /posts/verifica-tensionale-sezioni-composte.html
meta: "Quaderno tecnico · 1402 parole circa"
---

**Due materiali nella stessa sezione**

La verifica tensionale richiede di considerare moduli elastici diversi e collaborazione tra acciaio e calcestruzzo.

\[n=\frac{E_s}{E_c}\]

Una possibile trasformazione è:

\[A_{c,eq}=\frac{A_c}{n}\]

Asse neutro e tensioni.

\[y_G=\frac{sum A_iy_i}{sum A_i}, qquad I=sum(I_i+A_i(y_i-y_G)^2)\]

\[sigma(y)=\frac{N}{A}pm\frac{M}{I}y\]

Esempio.

Con (E_s=210000,mathrm{MPa}), (E_c=35000,mathrm{MPa}): (n=6). Un'area (A_c=0.90,mathrm{m^2}) diventa (A_{c,eq}=0.15,mathrm{m^2}).

Riferimenti tecnici utilizzati:

- UNI EN 1994-2, Eurocodice 4: Ponti composti acciaio-calcestruzzo.
- D.M. 17 gennaio 2018, NTC 2018.

Nota StruHub.

StruHub organizza il calcolo fibra per fibra e caso per caso.

**Fibre critiche e segno delle tensioni**

Nelle sezioni composte è essenziale conservare il segno delle tensioni. La fibra superiore della soletta, la fibra inferiore dell'acciaio e l'interfaccia possono governare combinazioni diverse. Una tabella tensionale dovrebbe quindi riportare almeno:

- quota della fibra;
- materiale;
- tensione da sforzo normale;
- tensione da momento;
- tensione totale.

**Effetto della connessione**

La teoria della sezione composta presuppone una collaborazione efficace tra acciaio e calcestruzzo. In termini pratici, questo richiede che la connessione trasferisca lo scorrimento longitudinale. Se la connessione è parziale, la rigidezza equivalente e le tensioni non coincidono con quelle della piena collaborazione.

Lo scorrimento all'interfaccia può essere concettualmente legato al flusso di taglio:

\[q=\frac{VQ}{I}\]

Per la verifica tensionale composta, la sezione non può essere letta come un oggetto unico e immutabile. Materiali diversi, fasi costruttive, deformazioni imposte e limiti tensionali obbligano a ragionare per fibre, per tempo e per caso di carico.

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

Impostazione da riferimento. La verifica tensionale è una storia di fibre. La fibra superiore della soletta, l’intradosso dell’acciaio e l’interfaccia non sono equivalenti e possono governare casi di carico diversi. Un post tecnico dovrebbe quindi fornire metodo, non solo teoria: definizione delle fibre, tabella dei contributi, limiti, segno delle tensioni e interpretazione finale.

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

Omogeneizzazione e asse neutro. La verifica tensionale di una sezione composta acciaio-calcestruzzo richiede prima di tutto una scelta di modello: sezione interamente reagente, calcestruzzo fessurato, collaborazione parziale o completa, fase di getto e fase di esercizio. Nel caso elastico lineare, il metodo classico consiste nell'omogeneizzare i materiali tramite il coefficiente modulare:

\[n=\frac{E_s}{E_c}\]

Le aree di calcestruzzo possono essere trasformate in aree equivalenti di acciaio, o viceversa. L'asse neutro si ottiene imponendo l'annullamento del momento statico della sezione omogeneizzata:

\[\bar y=\frac{\sum_i A_i^* y_i}{\sum_i A_i^*}\]

Il momento d'inerzia omogeneizzato e poi:

\[I^*=\sum_i \left(I_i^*+A_i^*(y_i-\bar y)^2\right)\]

Le tensioni elastiche si leggono con la formula di Navier, ma tenendo conto del materiale a cui appartiene il punto considerato. Per l'acciaio:

\[\sigma_s(y)=\frac{N}{A^*}+\frac{M(y-\bar y)}{I^*}\]

Per il calcestruzzo, la tensione equivalente va ritrasformata secondo il coefficiente modulare. Questa distinzione e essenziale: una sezione composta non e una sezione monomateriale mascherata.

Fasi e tensioni residue. La vera complessita delle sezioni composte e che la sezione efficace cambia nel tempo. Prima del getto collaborante, il peso della soletta fresca puo gravare sulla sola carpenteria metallica; dopo la maturazione, i sovraccarichi agiscono sulla sezione composta. La tensione finale e quindi somma di contributi calcolati su sezioni diverse:

\[\sigma_{tot}=\sigma_{fase1}+\sigma_{fase2}+\sigma_{ritiro}+\sigma_{termica}\]

Un controllo tecnico deve dichiarare quale sezione resiste a quale azione. In caso contrario, la verifica puo risultare numericamente ordinata ma fisicamente sbagliata. Per le sezioni composte da ponte i riferimenti principali sono D.M. 17 gennaio 2018, Capitoli 4 e 5, UNI EN 1994-2:2006, UNI EN 1993-1-1:2014 e UNI EN 1992-1-1:2015.

La conclusione operativa e semplice: la verifica tensionale non e una tabella di limiti, ma una ricostruzione della storia resistente della sezione.

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
