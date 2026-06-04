---
layout: post
title: "Applicazione del metodo Guyon-Massonnet-Bareš per piastre ortotrope equivalenti"
description: "Riferimento tecnico StruHub con formule, ipotesi di modello e riferimenti normativi."
date: 2026-01-04
order: 4
permalink: /posts/guyon-massonnet-bares-piastre-ortotrope.html
meta: "Quaderno tecnico · 1352 parole circa"
---

**Piastra ortotropa equivalente**

Il metodo sostituisce il sistema reale di travi e soletta con una piastra equivalente capace di rappresentare rigidezze longitudinali, trasversali e torsionali.

$$
ho=fleft(\frac{D_y}{D_x}\right), qquad alpha=fleft(\frac{D_{xy}}{sqrt{D_xD_y}}\right)
$$

Coefficienti di effetto.

$$
M_{x,j}=k_{M,j}M_x, qquad V_{x,j}=k_{V,j}V_x
$$

Esempio.

Se (k_M=0.32) e (M_x=900,mathrm{kNm}):

$$
M_{x,j}=288,mathrm{kNm}
$$

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, NTC 2018, Capitolo 5.
- UNI EN 1991-2, Eurocodice 1: Azioni da traffico sui ponti.

Nota StruHub.

Il metodo è utile come controllo semianalitico prima o a fianco di un modello FEM.

**Lettura ingegneristica dei parametri**

I parametri del metodo Guyon-Massonnet-Bareš non vanno letti come coefficienti astratti. Essi rappresentano il rapporto tra rigidezza longitudinale, rigidezza trasversale e torsione. Variando questi parametri si passa da un impalcato che lavora quasi per travi indipendenti a uno che si comporta più come piastra.

**Controllo contro un modello graticcio**

Una buona pratica è confrontare l'ordine di grandezza dei coefficienti ottenuti con un semplice modello a graticcio. Se una trave laterale riceve un coefficiente molto alto, bisogna chiedersi se il carico è effettivamente vicino al bordo o se la rigidezza trasversale è stata sottostimata.

Il metodo semianalitico è utile proprio perché rende visibili le dipendenze parametriche, che in un FEM possono restare nascoste dietro la discretizzazione.

Nel tema di il metodo Guyon-Massonnet-Bares, il punto non è ottenere un coefficiente isolato, ma costruire una catena coerente tra modello globale e verifica locale. Il carico applicato al piastra ortotropa equivalente attraversa una gerarchia resistente: soletta, traversi, travi principali, appoggi e infine sottostrutture.

Il primo controllo è l'equilibrio. Se gli effetti ripartiti non ricostruiscono l'azione globale, il modello non è accettabile. Il secondo controllo è la compatibilità: gli elementi connessi non possono deformarsi come sistemi indipendenti se la soletta o i collegamenti impongono continuità.

$$
\sum_j E_j \simeq E_{tot}
$$

**Rigidezze equivalenti e sensibilità**

La ripartizione dipende dai rapporti di rigidezza, non dai soli interassi geometrici. Un elemento molto rigido attira più domanda, mentre un elemento flessibile tende a scaricare verso gli elementi vicini. Questo vale per travi, piastre equivalenti e modelli a graticcio.

$$
D_x \propto EI_x, \qquad D_y \propto EI_y, \qquad D_{xy} \propto GJ
$$

Una variazione del 20% nella rigidezza trasversale può modificare in modo significativo il coefficiente di ripartizione. Per questo l'articolo tecnico non deve limitarsi a presentare il risultato: deve spiegare quali parametri lo muovono.

Esempio di confronto. Si consideri un effetto globale (E_{tot}=1000\,\mathrm{kNm}). Un primo modello fornisce coefficienti (0.20,0.35,0.35,0.10); un secondo modello, con maggiore rigidezza trasversale, fornisce (0.24,0.29,0.29,0.18). I due risultati sono entrambi equilibrati, ma raccontano impalcati diversi.

$$
E_{j}=\eta_j E_{tot}
$$

Nel primo caso la domanda è concentrata sulle travi interne; nel secondo la distribuzione è più uniforme. La scelta progettuale deve leggere questa differenza, non solo copiare il massimo valore.

**Uso nella relazione tecnica**

Una relazione ben costruita dovrebbe riportare schema dell'impalcato, rigidezze equivalenti, posizione dei carichi, coefficienti ottenuti e controllo di equilibrio. Se il metodo è semianalitico, è utile affiancare un confronto con un modello graticcio o FEM semplificato.

Il pregio del metodo è rendere esplicite le dipendenze parametriche. Un FEM dà un numero; il metodo semianalitico aiuta a capire perché quel numero cambia quando varia la rigidezza trasversale. In questo modo il post diventa un riferimento tecnico: non propone soltanto un numero, ma insegna a giudicarlo.

**Dal modello globale alla verifica locale**

Nei temi di impalcato, sezioni composte e azioni di progetto, il passaggio più importante è collegare una grandezza globale a un controllo locale. Un momento globale lungo l'impalcato non è ancora una verifica di fibra; un carico da traffico non è ancora una tensione; un coefficiente di ripartizione non è ancora una domanda resistente. Il calcolo diventa affidabile quando ogni trasformazione è esplicita.

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

Piastra ortotropa equivalente.

Il metodo Guyon-Massonnet-Bares nasce per trattare impalcati a travi e soletta come piastre ortotrope equivalenti. L'idea e sostituire il sistema discreto di travi longitudinali e collegamenti trasversali con un continuo dotato di rigidezze diverse nelle due direzioni principali. In questa rappresentazione, la rigidezza flessionale longitudinale $D_x$, quella trasversale $D_y$ e la rigidezza torsionale $D_{xy}$ governano la diffusione del carico.

Per una piastra isotropa, la rigidezza flessionale e:

$$
D=\frac{E h^3}{12(1-\nu^2)}
$$

In una piastra ortotropa, invece, non basta un unico $D$. La struttura ha una direzione piu rigida, in genere quella delle travi principali, e una direzione piu debole, governata da soletta e traversi. Il rapporto tra rigidezze modifica la forma della superficie di influenza e quindi la quota di carico attribuita a ciascuna trave.

Il parametro tecnico piu importante non e il nome del metodo, ma la coerenza tra impalcato reale e piastra equivalente. Travi molto ravvicinate, soletta collaborante e traversi efficaci giustificano una diffusione piu continua. Travi molto distanziate, collegamenti trasversali deboli o dettagli costruttivi discontinui richiedono cautela.

Dal coefficiente alla sollecitazione. Il coefficiente di ripartizione ottenuto dal metodo deve essere trasformato in azioni interne. Se $m_x(x,y)$ e il momento longitudinale per unita di larghezza della piastra, la sollecitazione nella trave puo essere ricavata integrando sulla fascia tributaria equivalente:

$$
M_i(x)=\int_{b_i} m_x(x,y)\\,dy
$$

Questa scrittura ricorda che il coefficiente non e un numero isolato: e il risultato di una distribuzione spaziale. Nei casi pratici, tabelle e coefficienti rendono il metodo operativo, ma la verifica ingegneristica resta la stessa: confrontare la diffusione teorica con una modellazione a graticcio o FEM quando geometria, sbalzi, curvature o eccentricita rendono il comportamento meno regolare.

Un esempio semplice: se una trave di bordo riceve $\alpha=0.34$ di un asse da $Q=300\,\mathrm{kN}$, la quota verticale e $Q_i=102\,\mathrm{kN}$. Se lo stesso carico genera anche torsione globale, il massimo momento nella trave di bordo puo non coincidere con la sola quota verticale. Per i carichi mobili e la progettazione degli impalcati, i riferimenti da citare sono D.M. 17 gennaio 2018, Capitolo 5, e UNI EN 1991-2:2005.

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
