---
layout: post
title: "Valutazione degli effetti di ritiro, viscosita e temperatura negli impalcati"
description: "Riferimento tecnico StruHub con formule, ipotesi di modello e riferimenti normativi."
date: 2026-01-16
order: 16
permalink: /posts/ritiro-viscosita-temperatura-impalcati.html
meta: "Quaderno tecnico · 1377 parole circa"
---

**Azioni indirette**

Ritiro, viscosità e temperatura sono deformazioni imposte o modifiche del comportamento nel tempo. In strutture vincolate generano sollecitazioni.

$$
sigma_c=E_c arepsilon_{cs}
$$

$$
E_{c,eff}=\frac{E_c}{1+ arphi(t,t_0)}
$$

$$
arepsilon_T=alpha_TDelta T
$$

Esempio.

Con (alpha_T=10^{-5},1/^circ C), (Delta T=25^circ C):

$$
arepsilon_T=2.5cdot10^{-4}
$$

Se impedita in acciaio con (E=210000,mathrm{MPa}):

$$
sigma=52.5,mathrm{MPa}
$$

Riferimenti tecnici utilizzati:

- UNI EN 1991-1-5, Azioni termiche.
- UNI EN 1992-1-1, Eurocodice 2.
- D.M. 17 gennaio 2018, NTC 2018.

Nota StruHub.

StruHub tratta queste azioni come contributi meccanici tracciabili, non come note laterali.

Deformazioni imposte e vincoli.

Ritiro e temperatura generano sollecitazioni solo nella misura in cui la deformazione libera è impedita. Se un elemento potesse deformarsi liberamente, una variazione termica uniforme produrrebbe spostamento, non tensione. Il vincolo trasforma deformazione in azione interna.

Il grado di vincolo può essere rappresentato con un coefficiente $\lambda$:

$$
\sigma=\lambda E\varepsilon_{imp}
$$

con $\lambda=0$ per deformazione libera e $\lambda=1$ per impedimento completo.

**Gradiente termico. Il gradiente termico produce curvatura**

Se la differenza di temperatura tra estradosso e intradosso è $\Delta T_g$, una stima della curvatura libera è:

$$
\chi_T \approx \frac{\alpha_T\Delta T_g}{h}
$$

dove $h$ è l'altezza della sezione.

Per ritiro, viscosita e temperatura, la sezione non può essere letta come un oggetto unico e immutabile. Materiali diversi, fasi costruttive, deformazioni imposte e limiti tensionali obbligano a ragionare per fibre, per tempo e per caso di carico.

**Dal modello geometrico al modello resistente**

La prima operazione è definire quali parti della sezione partecipano alla resistenza in una certa fase. In una sezione composta, ad esempio, il calcestruzzo può non essere collaborante durante il getto o può collaborare con modulo efficace ridotto a lungo termine.

$$
n=\frac{E_s}{E_c}
$$

$$
\sigma(y)=\frac{N}{A}\pm\frac{M}{I}y
$$

Queste formule sono semplici solo se (A), (I) e (y) sono quelli della sezione corretta. Usare proprietà sezionali finali per carichi applicati in una fase precedente è un errore concettuale, non un'approssimazione innocua.

**Fibre di controllo**

La verifica deve scegliere fibre significative: estradosso soletta, intradosso acciaio, interfaccia, lembi estremi e punti in cui cambiano materiale o armatura. Per ogni fibra conviene separare i contributi:

$$
\sigma_{tot}=\sigma_N+\sigma_M+\sigma_{imp}+\sigma_{fase}
$$

Questa scomposizione rende comprensibile il risultato. Se la tensione finale supera un limite, si può capire se il problema nasce da un permanente, da una fase costruttiva, da temperatura o da ritiro.

**Esempio di lettura**

Si consideri una fibra inferiore con tre contributi: (+32\,\mathrm{MPa}) da peso proprio, (+18\,\mathrm{MPa}) da carichi permanenti successivi e (-6\,\mathrm{MPa}) da effetto imposto. La tensione finale è:

$$
\sigma_{tot}=32+18-6=44\,\mathrm{MPa}
$$

Il dato finale da solo non spiega nulla; la scomposizione mostra invece che il peso proprio è il contributo dominante. Questo orienta il progettista verso la fase costruttiva corretta.

**Impostazione da riferimento**

Le azioni indirette sono spesso meno intuitive dei carichi verticali perché non “spingono” la struttura: provano a deformarla. Sono i vincoli a trasformarle in sollecitazioni. Un post tecnico dovrebbe quindi fornire metodo, non solo teoria: definizione delle fibre, tabella dei contributi, limiti, segno delle tensioni e interpretazione finale.

Dal modello globale alla verifica locale. Nei temi di impalcato, sezioni composte e azioni di progetto, il passaggio più importante è collegare una grandezza globale a un controllo locale. Un momento globale lungo l'impalcato non è ancora una verifica di fibra; un carico da traffico non è ancora una tensione; un coefficiente di ripartizione non è ancora una domanda resistente. Il calcolo diventa affidabile quando ogni trasformazione è esplicita.

La sequenza tipica è: definizione dell'azione, scelta dello schema resistente, calcolo dell'effetto globale, ripartizione o trasformazione sezionale, verifica della fibra o dell'elemento. Saltare uno di questi passaggi produce risultati difficili da revisionare.

Tracciabilità delle combinazioni. Ogni valore dovrebbe conservare l'origine. Se una sezione ha (M_{max}=950\,\mathrm{kNm}), la relazione deve indicare se deriva da traffico, temperatura, fase costruttiva, ritiro o combinazione sismica. In assenza di questa informazione il dato è numericamente utile ma tecnicamente debole.

Una forma pratica di archiviazione è associare a ogni effetto tre campi: valore, combinazione governante e posizione. Per gli inviluppi:

$$
E_{max}(x_j)=E_{d,k}(x_j) \quad con \quad k=k_{gov}(x_j)
$$

Questa scrittura chiarisce che il massimo non è astratto: è prodotto da una combinazione precisa.

Controllo delle rigidezze. Quando si lavora con ripartizioni, sezioni composte o fasi costruttive, la rigidezza è il parametro più influente. Una rigidezza sbagliata può produrre una distribuzione apparentemente equilibrata ma fisicamente non corretta. Per questo, prima della verifica, conviene riportare le proprietà principali: area, inerzia, modulo elastico, coefficiente di omogeneizzazione e fase resistente.

Per una sezione composta, ad esempio, una stessa azione può produrre tensioni diverse se applicata prima o dopo la maturazione della soletta. Il valore finale è la somma di contributi nati in tempi diversi:

$$
\sigma_{finale}=\sum_i \sigma_i(A_i,I_i,E_i,t_i)
$$

La notazione evidenzia che ogni contributo ha proprietà resistenti proprie.

Esempio di lettura critica. Si consideri un impalcato con quattro travi. Un modello semplificato fornisce coefficienti (0.18,0.32,0.32,0.18), mentre un modello più rigido trasversalmente fornisce (0.22,0.28,0.28,0.22). Entrambi rispettano l'equilibrio, ma il secondo distribuisce maggiormente verso le travi laterali. Se la verifica locale di una trave laterale è vicina al limite, questa differenza non è accademica.

Scrittura da riferimento tecnico. Il testo deve accompagnare il lettore dalla causa all'effetto: quale azione entra, come viene distribuita, quale proprietà resistente la trasforma in tensione o sollecitazione, quale limite viene controllato. Questa catena rende l'articolo consultabile anche a distanza di tempo.

Azioni differite e deformazioni impresse. Ritiro, viscosita e temperatura non sono dettagli secondari: negli impalcati possono generare stati tensionali e deformativi comparabili con quelli dei carichi permanenti secondari. Il ritiro e una deformazione imposta del calcestruzzo, spesso rappresentata come $\varepsilon_{cs}$. Se il calcestruzzo fosse libero, si accorcerebbe senza tensioni; nella sezione composta, invece, l'acciaio e i vincoli interni contrastano questa deformazione e generano auto-tensioni.

In forma semplificata, una deformazione imposta produce una forza equivalente:

$$
N_{cs}=E_c A_c \varepsilon_{cs}
$$

La distribuzione reale dipende dalla posizione dell'asse neutro, dalla connessione, dalla sequenza costruttiva e dal grado di vincolo. La viscosita modifica invece la rigidezza efficace nel tempo. Un approccio usuale consiste nell'utilizzare un modulo efficace:

$$
E_{c,eff}=\frac{E_c}{1+\varphi(t,t_0)}
$$

dove $\varphi(t,t_0)$ e il coefficiente di viscosita. Riducendo il modulo efficace del calcestruzzo, cambia il coefficiente di omogeneizzazione e quindi cambiano asse neutro, inerzia e tensioni.

Gradiente termico e curvatura. La temperatura agisce in due modi. Una variazione uniforme produce allungamento o accorciamento globale; un gradiente termico produce curvatura. Per una variazione uniforme $\Delta T$, la deformazione libera e:

$$
\varepsilon_T=\alpha_T \Delta T
$$

Se l'impalcato e vincolato, questa deformazione genera azioni assiali. Se invece la temperatura varia lungo l'altezza, la curvatura termica puo essere stimata come:

$$
\chi_T \simeq \alpha_T\frac{\Delta T}{h}
$$

Il controllo tecnico deve distinguere tra effetti primari e secondari. In una struttura isostatica, una deformazione imposta puo generare spostamenti senza sollecitazioni iperstatiche; in una struttura continua, la stessa deformazione produce momenti e tagli secondari. Per ponti e sezioni composte, i riferimenti da citare sono D.M. 17 gennaio 2018, Capitoli 3, 4 e 5, UNI EN 1991-1-5:2004 per azioni termiche, UNI EN 1992-1-1:2015 e UNI EN 1994-2:2006.

Controllo dimensionale. Un riferimento tecnico deve permettere al lettore di controllare gli ordini di grandezza. Ogni risultato numerico dovrebbe essere accompagnato da un controllo dimensionale, da una interpretazione fisica e da una indicazione del parametro dominante. Se una formula restituisce un valore in $\mathrm{kN}$, $\mathrm{kNm}$, $\mathrm{kPa}$ o $\mathrm{mm}$, l'unita deve essere esplicita e coerente con le grandezze inserite.

La forma generale di una verifica puo essere letta come:

$$
\eta=\frac{E_d}{R_d} \le 1
$$

dove $E_d$ e l'effetto dell'azione di progetto e $R_d$ la resistenza di progetto. Questa scrittura, comune a molti problemi strutturali e geotecnici, aiuta a separare domanda, capacita e margine.

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
