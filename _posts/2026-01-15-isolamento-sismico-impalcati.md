---
layout: post
title: "Valutazione degli effetti dell'isolamento sismico negli impalcati da ponte"
description: "Riferimento tecnico StruHub con schema meccanico, caso numerico su impalcato isolato e riferimenti normativi tracciabili."
date: 2026-01-15
order: 15
permalink: /posts/isolamento-sismico-impalcati.html
meta: "Quaderno tecnico - 1860 parole circa"
---

![Schema del modello equivalente di un impalcato isolato, della legge forza-spostamento del dispositivo e dei controlli di progetto]({{ site.baseurl }}/assets/images/isolamento-sismico-impalcati-schema.svg)

**L'isolamento non riduce il sisma: lo ridistribuisce**

Quando si parla di isolamento sismico dei ponti, il rischio piu comune e leggere il dispositivo come un "coefficiente che abbassa le forze". Non e cosi. L'isolamento cambia il problema meccanico: allunga il periodo proprio della sottostruttura-impalcato, introduce dissipazione aggiuntiva e concentra una quota rilevante della deformazione negli appoggi speciali. Se questa catena non viene resa esplicita, il modello puo sembrare convincente ma resta poco auditabile.

Per un professionista, le domande corrette sono quattro:

- di quanto si sposta il periodo equivalente;
- quale spostamento di progetto viene chiesto ai dispositivi;
- quale taglio orizzontale resta sulle pile e sulle fondazioni;
- quali dettagli secondari diventano governanti: giunti, ritegni, tubazioni, appoggi di fine corsa, connessioni e ispezionabilita.

L'isolamento diventa davvero utile quando riduce in modo misurabile la domanda sulle sottostrutture senza aprire criticita non controllate sui movimenti relativi.

**Dati minimi da dichiarare prima di ogni modello**

Prima di discutere formule o software, conviene mettere in chiaro almeno questi input:

- massa partecipante dell'impalcato e quota di massa delle pile realmente inclusa nel primo modo;
- tipologia dei dispositivi: elastomerici ad alto smorzamento, LRB, FPS o altri;
- rigidezza post-elastica o secante dichiarata al livello di spostamento considerato;
- eventuale forza caratteristica di snervamento o attrito equivalente;
- combinazione sismica e spettro di riferimento;
- spostamenti non sismici da sommare o confrontare: temperatura, ritiro, creep, frenatura, montaggio.

Se uno di questi dati manca, il confronto tra alternative di appoggio resta poco difendibile.

**Il modello equivalente che serve davvero**

Per un primo controllo, l'impalcato isolato puo essere letto come sistema a un grado di liberta:

$$
M \ddot{u}(t) + C_{eq} \dot{u}(t) + K_{eq} u(t) = - M \ddot{u}_g(t)
$$

dove $M$ e la massa partecipante e $K_{eq}$ la rigidezza orizzontale equivalente del sistema di isolamento sommata, se necessario, alla quota di rigidezza residua della sottostruttura che resta significativa nel modo governante.

Il periodo efficace vale:

$$
T_{eff} = 2 \pi \sqrt{\frac{M}{K_{eff}}}
$$

e la forza trasmessa al sistema, in lettura semplificata, e:

$$
V_{b,eff} = K_{eff} \, d_d
$$

dove $d_d$ e lo spostamento di progetto del sistema isolato. La formula e semplice, ma la sua interpretazione non lo e: ridurre $K_{eff}$ allunga il periodo e in genere abbassa il taglio trasmesso, pero aumenta la domanda di spostamento sui dispositivi e sui giunti.

Per dispositivi bilineari tipo LRB, una scrittura molto utile e:

$$
F(d) = Q_d + K_d d
$$

con:

- $Q_d$ forza caratteristica di snervamento del sistema;
- $K_d$ rigidezza post-elastica;
- $d$ spostamento orizzontale del dispositivo.

Alla quota di progetto $d_d$ la rigidezza secante diventa:

$$
K_{eff} = \frac{Q_d + K_d d_d}{d_d}
$$

La dissipazione ciclica puo essere riportata tramite smorzamento equivalente:

$$
\xi_{eff} = \frac{E_D}{4 \pi E_S}
$$

dove $E_D$ e l'area del ciclo isteretico ed $E_S$ l'energia elastica massima associata allo spostamento di progetto. Questa e la grandezza che permette di passare dalla meccanica del dispositivo alla correzione dello spettro.

**Caso numerico: impalcato da 36 m su 4 isolatori LRB**

Si consideri un impalcato in c.a.p. semplicemente appoggiato, di luce tipica da viadotto ordinario, con massa partecipante allo stato sismico:

$$
M = 2.40 \cdot 10^6 \, \mathrm{kg}
$$

Il benchmark elastico della sottostruttura non isolata, ottenuto con un modello lineare delle pile e degli appoggi ordinari, restituisce una rigidezza orizzontale globale pari a:

$$
K_{fix} = 52 \, \mathrm{MN/m}
$$

e quindi:

$$
T_{fix} = 2 \pi \sqrt{\frac{2.40 \cdot 10^6}{52 \cdot 10^6}} = 1.35 \, \mathrm{s}
$$

Questa stima non sostituisce il modello completo, ma e il controllo preliminare che conviene sempre fare. In pratica e proprio il tipo di verifica rapida che una libreria come `beamfeapy`, dichiaratamente orientata ad analisi statiche e modali di telai 3D, puo aiutare a rendere trasparente sul sistema non isolato prima di inserire dispositivi non lineari nel modello definitivo.

Supponiamo ora di adottare 4 isolatori LRB identici, ciascuno con:

- forza caratteristica $Q_{d,1} = 180 \, \mathrm{kN}$;
- rigidezza post-elastica $k_{d,1} = 2.6 \, \mathrm{MN/m}$;
- spostamento di progetto atteso dell'ordine di $d_d = 260 \, \mathrm{mm}$;
- spostamento di snervamento assunto $d_y = 8 \, \mathrm{mm}$.

Per il sistema complessivo:

$$
Q_d = 4 \cdot 180 = 720 \, \mathrm{kN}
$$

$$
K_d = 4 \cdot 2.6 = 10.4 \, \mathrm{MN/m}
$$

La rigidezza secante al livello di progetto diventa:

$$
K_{eff} = \frac{720 + 10.4 \cdot 0.26 \cdot 10^3}{0.26}
= 13.17 \, \mathrm{MN/m}
$$

e quindi:

$$
T_{eff} = 2 \pi \sqrt{\frac{2.40 \cdot 10^6}{13.17 \cdot 10^6}}
= 2.68 \, \mathrm{s}
$$

Il salto di periodo e il primo effetto strutturalmente rilevante: il sistema passa da circa $1.35 \, \mathrm{s}$ a circa $2.68 \, \mathrm{s}$, spostandosi verso un tratto dello spettro normalmente meno severo in accelerazione ma piu impegnativo in spostamento.

**Smorzamento equivalente e spettro corretto**

Per il ciclo isteretico bilineare, l'energia dissipata per ciclo puo essere stimata come:

$$
E_D = 4 Q_d (d_d - d_y)
$$

Sostituendo i numeri:

$$
E_D = 4 \cdot 720 \cdot (0.260 - 0.008)
= 725.8 \, \mathrm{kN\,m}
$$

L'energia elastica massima vale:

$$
E_S = \frac{1}{2} K_{eff} d_d^2
= \frac{1}{2} \cdot 13.17 \cdot 0.260^2
= 445.1 \, \mathrm{kN\,m}
$$

Quindi:

$$
\xi_{eff} = \frac{725.8}{4 \pi \cdot 445.1} = 0.130
\approx 13.0\%
$$

Assumendo, per il periodo efficace trovato, uno spettro elastico al 5% pari a:

$$
S_{e,5\%}(T_{eff}) = 1.85 \, \mathrm{m/s^2}
$$

il fattore correttivo di smorzamento puo essere letto in forma sintetica come:

$$
\eta_{\xi} = \sqrt{\frac{10}{5 + \xi_{eff,\%}}}
= \sqrt{\frac{10}{5 + 13.0}}
= 0.746
$$

e quindi:

$$
S_e(T_{eff}, \xi_{eff}) = \eta_{\xi} S_{e,5\%}
= 0.746 \cdot 1.85
= 1.38 \, \mathrm{m/s^2}
$$

Lo spostamento spettrale di progetto diventa:

$$
d_{sp} = \frac{S_e}{\omega_{eff}^2}
\qquad
\omega_{eff} = \frac{2 \pi}{T_{eff}}
$$

ovvero:

$$
d_{sp} = \frac{1.38}{(2 \pi / 2.68)^2}
= 0.251 \, \mathrm{m}
= 251 \, \mathrm{mm}
$$

La domanda trovata e coerente con il target iniziale di circa $260 \, \mathrm{mm}$: questo e il tipo di chiusura numerica che rende leggibile il progetto, perche collega dispositivo scelto, smorzamento equivalente e spostamento risultante.

**Confronto con il sistema non isolato**

Se per il benchmark non isolato si assume, allo stesso livello di progetto, una accelerazione spettrale:

$$
S_e(T_{fix}, 5\%) = 3.10 \, \mathrm{m/s^2}
$$

lo spostamento equivalente diventa:

$$
d_{fix} = \frac{3.10}{(2 \pi / 1.35)^2}
= 0.143 \, \mathrm{m}
= 143 \, \mathrm{mm}
$$

e il taglio globale trasmesso risulta:

$$
V_{b,fix} = K_{fix} d_{fix}
= 52 \cdot 0.143
= 7.44 \, \mathrm{MN}
$$

Per il sistema isolato:

$$
V_{b,iso} = K_{eff} d_{sp}
= 13.17 \cdot 0.251
= 3.31 \, \mathrm{MN}
$$

La lettura tecnica e chiara:

- riduzione del taglio globale di circa il $55\%$;
- aumento della domanda di spostamento di circa il $76\%$;
- trasferimento del problema dalle pile ai dispositivi e ai dettagli di escursione.

Questa e la ragione per cui l'isolamento non si valuta mai sul solo taglio alla base.

**Il controllo che spesso governa: lo spazio disponibile**

Una volta trovato $d_{sp}$, il progetto deve verificare se il ponte puo davvero muoversi cosi. Una scrittura pratica per il gioco minimo del giunto e:

$$
g_{min} \geq 1.2 \, d_{sp} + d_{temp} + d_{costr}
$$

Assumendo:

$$
d_{temp} = 25 \, \mathrm{mm},
\qquad
d_{costr} = 15 \, \mathrm{mm}
$$

si ottiene:

$$
g_{min} \geq 1.2 \cdot 251 + 25 + 15 = 341 \, \mathrm{mm}
$$

Questo numero e spesso piu decisivo della riduzione di taglio: se il dettaglio di giunto, il ritegno laterale o la sede d'appoggio non accomodano circa $34 \, \mathrm{cm}$ di escursione, il beneficio dinamico non e immediatamente utilizzabile.

**Che cosa deve controllare davvero il progettista**

Su un impalcato isolato, le verifiche che cambiano davvero il giudizio sono almeno queste:

1. compatibilita tra spostamento di progetto del dispositivo e spostamento disponibile del dettaglio;
2. stabilita verticale e compressione massima sui dispositivi nella combinazione sismica con effetti torsionali;
3. domanda residua sulle pile e sulle fondazioni, non solo come taglio ma anche come momento e taglio concomitanti;
4. funzionamento dei ritegni e degli arresti per eventi eccezionali o spostamenti oltre soglia;
5. combinazione tra sisma e movimenti lenti di servizio: temperatura, ritiro, viscosita e frenatura.

Il punto pratico e che l'isolatore non vive mai da solo. Vive insieme a travi, pulvino, ritegni, tubazioni, barriere, appoggi di estremita e strategia ispettiva.

**Quando il modello semplificato non basta piu**

La schematizzazione SDOF o a rigidezza equivalente e molto utile, ma smette di essere sufficiente in almeno quattro casi:

- impalcati curvi o con forte eccentricita tra baricentro delle masse e centro di rigidezza;
- pile con rigidezze molto diverse tra loro, che generano partecipazioni torsionali o modi accoppiati;
- dispositivi con leggi costitutive fortemente dipendenti da velocita, temperatura o pressione di contatto;
- presenza di gap, ritegni o vincoli che introducono non linearita a tratti.

In questi casi il modello equivalente deve restare come benchmark, non come modello unico. Se il FEM completo restituisce un periodo molto diverso, una ripartizione torsionale inattesa o una concentrazione anomala degli spostamenti su pochi dispositivi, il problema non si risolve "fidandosi del software": si torna al benchmark e si capisce quale ipotesi e cambiata.

**Workflow utile tra controllo manuale e modello completo**

Un flusso di lavoro solido, coerente con il modo in cui andrebbe documentato un progetto serio, e questo:

1. stima della massa partecipante e della rigidezza della sottostruttura non isolata;
2. calcolo preliminare di $T_{fix}$ e confronto con il primo modo del modello elastico;
3. scelta del dispositivo con parametri dichiarati a uno spostamento obiettivo;
4. costruzione di $K_{eff}$, $\xi_{eff}$ e verifica dello spostamento spettrale;
5. inserimento dei dispositivi nel modello globale con le stesse ipotesi di taratura;
6. confronto tra risultati globali e benchmark: periodo, taglio, spostamento e ripartizione sui dispositivi.

Questo passaggio e molto piu utile di un'unica analisi complessa lanciata senza controllo preliminare. Nello spirito StruHub, il valore non e nel grafico finale ma nella possibilita di rifare a mano i punti chiave: periodo, rigidezza, spostamento, margine.

**Errori ricorrenti da evitare**

Negli studi preliminari o nei report sintetici ricorrono spesso gli stessi errori:

- usare la rigidezza iniziale del dispositivo al posto della rigidezza secante al livello di spostamento di progetto;
- dichiarare lo smorzamento equivalente senza mostrare come e stato ricavato;
- confrontare il taglio isolato con il fisso e dimenticare il controllo dei giunti;
- sommare massimi non concomitanti di $N$, $V$ e $M$ nella lettura delle pile;
- citare un dispositivo "da catalogo" senza riportare pressione verticale, temperatura di prova e range di spostamento.

Sono errori piccoli in apparenza, ma ciascuno puo cambiare di molto la lettura finale.

**Dal numero alla decisione di progetto**

L'isolamento sismico degli impalcati non si giudica chiedendo se "funziona" in astratto. Si giudica chiedendo se, per quel ponte e per quel dettaglio costruttivo, il bilancio tra riduzione delle forze e aumento degli spostamenti resta favorevole e documentabile.

Nel caso mostrato, il beneficio sulle sottostrutture e netto: il taglio globale scende da $7.44 \, \mathrm{MN}$ a $3.31 \, \mathrm{MN}$. Ma il prezzo progettuale e altrettanto chiaro: circa $25 \, \mathrm{cm}$ di spostamento sismico e un giunto utile dell'ordine di $34 \, \mathrm{cm}$ una volta considerate anche le altre escursioni. E proprio questa lettura integrata a rendere il post utile a chi deve davvero firmare il progetto o rivedere una relazione di calcolo.

**Riferimenti normativi e tecnici**

- D.M. 17 gennaio 2018, *Aggiornamento delle "Norme tecniche per le costruzioni"*, Ministero delle Infrastrutture e dei Trasporti, G.U. n. 42 del 20 febbraio 2018, S.O. n. 8. Per questo tema i richiami principali stanno nei Capitoli 3, 5 e 7.
- Circolare 21 gennaio 2019, n. 7 C.S.LL.PP., *Istruzioni per l'applicazione dell'Aggiornamento delle "Norme tecniche per le costruzioni" di cui al decreto ministeriale 17 gennaio 2018*, G.U. n. 35 del 11 febbraio 2019, S.O. n. 5. Utili, in particolare, i commenti applicativi ai capitoli ponti e sisma.
- UNI EN 15129:2018, *Dispositivi antisismici*.
- UNI EN 1998-2:2025, *Eurocodice 8 - Progettazione sismica delle strutture - Parte 2: Ponti*. Nella pratica corrente compaiono ancora modelli legacy tarati sulla precedente UNI EN 1998-2:2011: conviene dichiararlo quando si confrontano risultati o dettagli di progetto sviluppati in anni diversi.
- Priestley, M. J. N.; Seible, F.; Calvi, G. M., *Seismic Design and Retrofit of Bridges*, John Wiley & Sons.
