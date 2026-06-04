---
layout: post
title: "Analisi di muri di sostegno a mensola in campo statico e sismico"
description: "Riferimento tecnico StruHub con formule, ipotesi di modello e riferimenti normativi."
date: 2026-01-01
order: 1
permalink: /posts/muri-sostegno-statico-sismico.html
meta: "Quaderno tecnico · 1286 parole circa"
---

**Spinte, pesi e stabilità**

Un muro a mensola equilibra spinte del terreno, sovraccarichi, falda e azioni sismiche pseudo-statiche.

Per Rankine:

$$
K_a=\frac{1-sinphi}{1+sinphi}
$$

$$
sigma'_h(z)=K_asigma'_v(z)
$$

Risultante.

Per diagramma triangolare:

$$
P_a=\frac{1}{2}K_agamma H^2
$$

Esempio.

Con (phi=30^circ), (gamma=18,mathrm{kN/m^3}), (H=5,mathrm{m}):

$$
K_a=0.333, qquad P_aapprox75,mathrm{kN/m}
$$

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, NTC 2018, Capitolo 6.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7.
- UNI EN 1997-1, Eurocodice 7: Progettazione geotecnica.

Nota StruHub.

StruHub separa spinte, verifiche globali e sollecitazioni strutturali, perché sono tre letture diverse dello stesso muro.

**Sovraccarichi e falda**

Alla spinta da peso proprio del terreno si sommano sovraccarichi superficiali e pressione idraulica. Un sovraccarico uniforme $q$ produce una pressione orizzontale:

$$
\Delta\sigma_h=K_a q
$$

La falda introduce invece pressione neutra:

$$
u(z)=\gamma_w z_w
$$

che deve essere aggiunta come azione idraulica e sottratta nel calcolo delle tensioni efficaci.

**Sisma pseudo-statico**

L'approccio pseudo-statico introduce coefficienti $k_h$ e $k_v$. La sua utilità sta nella semplicità, ma la scelta dei coefficienti e dello schema di spinta deve essere coerente con il livello di analisi richiesto.

Per il muro di sostegno in campo statico e sismico, la lettura corretta parte dall'equilibrio tra spinta del terreno, peso proprio, eventuali sovraccarichi, acqua e risposta del piano di posa. Il muro non è solo un elemento in calcestruzzo: è il bordo rigido di un volume di terreno che tende a muoversi.

**Spinte e pressione neutra**

In condizioni semplici, la spinta attiva può essere rappresentata con il coefficiente di Rankine:

$$
K_a=\frac{1-\sin\phi}{1+\sin\phi}
$$

La pressione efficace orizzontale è:

$$
\sigma'_h(z)=K_a\sigma'_v(z)
$$

Se è presente acqua, la pressione neutra deve essere aggiunta come azione idraulica. Separare tensione efficace e pressione neutra evita di confondere resistenza del terreno e spinta dell'acqua.

**Equilibrio alla base**

La risultante verticale (N) e il momento alla base definiscono l'eccentricità:

$$
e=\frac{M}{N}
$$

Se la risultante resta nel nucleo centrale, la distribuzione delle pressioni può essere letta con:

$$
q_{max,min}=\frac{N}{B}\left(1\pm\frac{6e}{B}\right)
$$

**Esempio di controllo**

Con (B=3.0\,\mathrm{m}), (N=450\,\mathrm{kN/m}) e (M=90\,\mathrm{kNm/m}):

$$
e=\frac{90}{450}=0.20\,\mathrm{m}
$$

Poiché (B/6=0.50\,\mathrm{m}), la risultante resta nel nucleo centrale. Le pressioni sono:

$$
q_{max}=\frac{450}{3}\left(1+\frac{6\cdot0.20}{3}\right)=210\,\mathrm{kPa}
$$

$$
q_{min}=90\,\mathrm{kPa}
$$

Lettura tecnica. Un muro è una struttura e un problema geotecnico nello stesso tempo. Le spinte definiscono la domanda; geometria, peso e terreno definiscono l’equilibrio. Un articolo di riferimento deve mostrare come si passa dalla spinta alla risultante, dalla risultante alle pressioni e dalle pressioni alla verifica. Solo così il lettore può ricostruire il ragionamento.

Dal terreno alla risultante. Nel muro di sostegno il percorso tecnico parte dalla spinta del terreno e arriva alle pressioni di contatto alla base. Ogni passaggio deve essere esplicito: coefficiente di spinta, diagramma delle pressioni, risultante, braccio, momento ribaltante, peso stabilizzante, eccentricità e verifica della fondazione.

Separare terreno e acqua. La pressione neutra non è una variante della spinta efficace: è un'azione idraulica distinta. In presenza di falda, la pressione totale orizzontale può essere letta come somma di contributo efficace e contributo idraulico:

$$
\sigma_h(z)=K_a\sigma'_v(z)+u(z)
$$

Questa separazione è importante perché il terreno offre resistenza tramite tensioni efficaci, mentre l'acqua aggiunge spinta senza contribuire alla resistenza al taglio.

Eccentricità e distribuzione delle pressioni. Il controllo della base non si esaurisce nella risultante verticale. Il momento sposta la risultante e genera eccentricità:

$$
e=\frac{M}{N}
$$

Se (|e|\le B/6), la distribuzione resta compressa su tutta la base. Se la risultante esce dal nucleo centrale, il modello lineare su tutta la larghezza non è più valido e bisogna usare una distribuzione parziale.

Esempio esteso. Con (N=500\,\mathrm{kN/m}), (M=110\,\mathrm{kNm/m}), (B=3.2\,\mathrm{m}), si ha:

$$
e=0.22\,\mathrm{m}, \qquad B/6=0.53\,\mathrm{m}
$$

La risultante resta nel nucleo centrale. Le pressioni sono:

$$
q_{max}=\frac{500}{3.2}\left(1+\frac{6\cdot0.22}{3.2}\right)=220\,\mathrm{kPa}
$$

$$
q_{min}=92\,\mathrm{kPa}
$$

Il valore (q_{max}) va confrontato con la resistenza di progetto del terreno, non con una pressione media generica.

Scrittura da riferimento tecnico. Un testo tecnico sul muro deve far vedere l'intera catena dell'equilibrio. Solo così il lettore capisce se il margine deriva dal peso del muro, dalla larghezza della base, dalla riduzione della spinta o dalla capacità del terreno.

Spinta attiva, acqua e sovraccarichi. Un muro a mensola e un sistema di equilibrio tra terreno e struttura. La prima azione da ricostruire e la spinta attiva del terreno. Nel caso elementare di paramento verticale, terreno orizzontale e attrito muro-terreno trascurato, il coefficiente di Rankine e:

$$
K_a=\frac{1-\sin\phi}{1+\sin\phi}
$$

La pressione efficace orizzontale varia con la profondita:

$$
\sigma'_h(z)=K_a\gamma' z
$$

e la risultante del diagramma triangolare vale:

$$
P_a=\frac{1}{2}K_a\gamma' H^2
$$

Questa e solo la parte efficace. Se e presente falda, si aggiunge la pressione neutra $u(z)=\gamma_w z$. Se e presente un sovraccarico uniforme $q$, la pressione orizzontale aggiuntiva e:

$$
\Delta \sigma_h=K_a q
$$

Separare questi contributi e essenziale: terreno, acqua e sovraccarico hanno origini fisiche diverse e possono avere combinazioni diverse.

Equilibrio globale del muro. Una volta calcolate le spinte, si costruiscono le risultanti verticali e orizzontali. Il momento ribaltante deriva dalle spinte, mentre il momento stabilizzante deriva dai pesi del muro e del terreno sulla mensola di monte. Il fattore di sicurezza al ribaltamento in forma tradizionale e:

$$
FS_{rib}=\frac{M_{stab}}{M_{rib}}
$$

Nella progettazione agli stati limite, questa lettura deve essere tradotta secondo coefficienti parziali e combinazioni di progetto, ma resta utile per capire la fisica del problema. Il controllo successivo riguarda l'eccentricita della risultante alla base:

$$
e=\frac{M}{N}
$$

Se $|e|\le B/6$, le pressioni di contatto restano compresse su tutta la base:

$$
q_{max,min}=\frac{N}{B}\left(1\pm\frac{6e}{B}\right)
$$

Se la risultante esce dal nucleo centrale, una parte della fondazione si decomprime e la distribuzione va ricalcolata su una larghezza efficace.

Effetto sismico pseudo-statico. In campo sismico si puo usare una schematizzazione pseudo-statica con coefficienti $k_h$ e $k_v$. La spinta aumenta e cambia la posizione della risultante. L'uso di Mononobe-Okabe o di metodi equivalenti richiede coerenza tra geometria, parametri geotecnici e livello di duttilita accettato. L'obiettivo non e solo ottenere una spinta piu grande, ma verificare se il muro puo tollerare spostamenti permanenti, rotazioni e variazioni di pressione sul piano di posa.

Un esempio numerico: con $\phi=30^\circ$, $H=5\,\mathrm{m}$, $\gamma'=18\,\mathrm{kN/m^3}$, si ottiene $K_a=0.333$ e $P_a=75\,\mathrm{kN/m}$. Se si aggiunge un sovraccarico $q=10\,\mathrm{kPa}$, la spinta rettangolare aggiuntiva vale $K_a q H=16.7\,\mathrm{kN/m}$. Il momento ribaltante non cresce solo in valore, ma anche in funzione del braccio: il contributo triangolare agisce a $H/3$, quello rettangolare a $H/2$.

Per muri di sostegno, i riferimenti da citare sono D.M. 17 gennaio 2018, Capitolo 6, Circolare C.S.LL.PP. 21 gennaio 2019 n. 7 e UNI EN 1997-1:2013. Per il sisma occorre richiamare anche il Capitolo 7 delle NTC 2018 e, quando pertinente, UNI EN 1998-5:2005.

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
