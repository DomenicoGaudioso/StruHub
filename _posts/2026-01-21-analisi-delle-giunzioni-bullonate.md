---
layout: post
title: "Analisi delle giunzioni bullonate"
description: "Riferimento tecnico StruHub con esempio numerico completo, schema del gruppo bullonato e riferimenti EC3/NTC tracciabili."
date: 2026-01-21
order: 21
permalink: /posts/analisi-delle-giunzioni-bullonate.html
meta: "Quaderno tecnico · 1680 parole circa"
---

![Giunzione bullonata in carpenteria metallica]({{ site.baseurl }}/assets/images/analisi-delle-giunzioni-bullonate-01.jpg)

**Il bullone non si verifica mai da solo**

Nelle unioni in acciaio il risultato finale dipende da una catena di controlli: distribuzione delle azioni nel gruppo, resistenza a taglio del bullone, rifollamento della piastra, eventuale trazione, scorrimento nei giunti precaricati e, quando la geometria si accorcia, block tearing della lamiera. Saltare un passaggio porta quasi sempre a una verifica formalmente "OK" ma tecnicamente fragile.

Per questo conviene leggere il problema in questo ordine:

1. definire la geometria del gruppo bullonato e le coordinate dei bulloni;
2. trasformare taglio, trazione e momento in azioni sul singolo bullone;
3. confrontare il bullone più sollecitato con le resistenze di progetto;
4. chiudere con le verifiche della piastra e del dettaglio esecutivo.

Una sintesi utile, per gruppi soggetti a taglio nel piano della piastra e momento torcente rispetto al baricentro, è:

$$
\mathbf{F}_{Ed,i}=
\begin{bmatrix}
\dfrac{V_{x,Ed}}{n} \\
\dfrac{V_{y,Ed}}{n}
\end{bmatrix}
+
\dfrac{M_{z,Ed}}{\sum_j r_j^2}
\begin{bmatrix}
-y_i \\
x_i
\end{bmatrix}
$$

con:

$$
r_i=\sqrt{x_i^2+y_i^2},
\qquad
F_{v,Ed,i}=\left\lVert \mathbf{F}_{Ed,i}\right\rVert
$$

Questa forma rende subito visibile un punto pratico: il bullone critico non è sempre quello più lontano nella direzione del taglio diretto, ma quello in cui taglio uniforme e contributo torsionale si sommano nel verso più sfavorevole.

![Schema di un gruppo bullonato con taglio eccentrico e quote geometriche]({{ site.baseurl }}/assets/images/analisi-delle-giunzioni-bullonate-schema.svg)

**Geometria minima e ipotesi da dichiarare**

Prima delle formule di resistenza bisogna chiarire tre dati che spesso nei fogli veloci restano impliciti:

- diametro nominale del bullone $d$ e diametro del foro $d_0$;
- distanze dai bordi $e_1$, $e_2$ e interassi $p_1$, $p_2$;
- posizione del piano di taglio rispetto alla filettatura.

Per unioni ordinarie a rifollamento, i minimi geometrici comunemente richiamati dall'Eurocodice 3 sono:

$$
e_1,e_2 \geq 1.2\,d_0
$$

$$
p_1 \geq 2.2\,d_0,
\qquad
p_2 \geq 2.4\,d_0
$$

Sono minimi geometrici, non obiettivi progettuali. In un nodo reale conviene spesso stare più larghi per tre ragioni:

- aumenta la resistenza a rifollamento della lamiera;
- si riduce il rischio di block tearing;
- il montaggio in officina e in cantiere diventa meno sensibile alle tolleranze.

La resistenza del singolo bullone a taglio, per un piano di taglio, può essere scritta come:

$$
F_{v,Rd}=\frac{\alpha_v\,f_{ub}\,A}{\gamma_{M2}}
$$

dove $A$ è l'area lorda o resistente a seconda che il piano tagli il gambo pieno o la parte filettata. Se la filettatura cade nel piano di taglio, è prudente lavorare con l'area resistente $A_s$.

La verifica di rifollamento della piastra resta distinta:

$$
F_{b,Rd}=\frac{k_1\,\alpha_b\,f_u\,d\,t}{\gamma_{M2}}
$$

con $\alpha_b$ e $k_1$ dipendenti da bordo, interasse, classe del bullone e materiale della piastra. In pratica, il messaggio è semplice: la resistenza del nodo non coincide automaticamente con la resistenza del gambo del bullone.

**Esempio numerico: piastra di estremita con 4 bulloni M20**

Si consideri il collegamento di una trave secondaria a una piastra d'anima tramite un gruppo di 4 bulloni M20 classe 8.8 in un solo piano di taglio. L'obiettivo non è progettare l'intero nodo, ma mostrare il percorso di verifica del gruppo bullonato.

Dati di input adottati:

- bulloni M20 classe 8.8, fori normali con $d_0=22\,\mathrm{mm}$;
- area resistente del bullone $A_s=245\,\mathrm{mm^2}$;
- piastra in acciaio S275 con $f_u=430\,\mathrm{MPa}$ e spessore $t=12\,\mathrm{mm}$;
- coordinate dei bulloni rispetto al baricentro: $(\pm40,\pm60)\,\mathrm{mm}$;
- interassi: $p_1=120\,\mathrm{mm}$ nella direzione del carico e $p_2=80\,\mathrm{mm}$ trasversalmente;
- distanze minime dai bordi assunte nel dettaglio: $e_1=50\,\mathrm{mm}$, $e_2=40\,\mathrm{mm}$;
- taglio di progetto sulla connessione: $V_{Ed}=160\,\mathrm{kN}$;
- eccentricità della linea d'azione rispetto al baricentro del gruppo: $e=70\,\mathrm{mm}$.

Il momento torcente sul gruppo è:

$$
M_{z,Ed}=V_{Ed}\,e=160 \cdot 70 = 11200\,\mathrm{kN\,mm}=11.2\,\mathrm{kNm}
$$

La somma dei quadrati delle distanze radiali vale:

$$
\sum r_i^2=4\,(40^2+60^2)=20800\,\mathrm{mm^2}
$$

Il contributo di taglio diretto è uniforme:

$$
\frac{V_{Ed}}{n}=\frac{160}{4}=40\,\mathrm{kN}
$$

Il contributo da momento, espresso in componenti, porta ai valori della tabella seguente.

| Bullone | $(x_i,y_i)$ [mm] | $F_{x,i}^{(M)}$ [kN] | $F_{y,i}^{(M)}$ [kN] | $F_{v,Ed,i}$ [kN] |
| --- | --- | ---: | ---: | ---: |
| 1 | $(40,60)$ | $-32.3$ | $+21.5$ | $37.2$ |
| 2 | $(-40,60)$ | $-32.3$ | $-21.5$ | $69.5$ |
| 3 | $(-40,-60)$ | $+32.3$ | $-21.5$ | $69.5$ |
| 4 | $(40,-60)$ | $+32.3$ | $+21.5$ | $37.2$ |

I bulloni critici sono quindi quelli sulla colonna sinistra, dove il contributo torsionale si somma al taglio diretto. Questo è un caso tipico di officina: la geometria sembra simmetrica, ma l'eccentricità del carico rompe la simmetria delle domande.

**Verifica a taglio del bullone**

Assumendo il piano di taglio passante nella filettatura e $\alpha_v=0.6$, con $\gamma_{M2}=1.25$:

$$
F_{v,Rd}=
\frac{0.6 \cdot 800 \cdot 245}{1.25}
=94.1\,\mathrm{kN}
$$

Per il bullone più sollecitato:

$$
\eta_v=\frac{F_{v,Ed,max}}{F_{v,Rd}}=
\frac{69.5}{94.1}=0.74
$$

Il bullone risulta verificato a taglio, ma il margine non è enorme. Se l'eccentricità salisse a $100\,\mathrm{mm}$, la domanda locale crescerebbe sensibilmente senza che cambi il taglio globale della trave. Questo è il tipo di sensibilità che il calcolo deve rendere leggibile.

**Verifica a rifollamento della piastra**

Per la piastra S275 da $12\,\mathrm{mm}$:

$$
\alpha_b=
\min\left(
\frac{e_1}{3d_0},
\frac{p_1}{3d_0}-0.25,
\frac{f_{ub}}{f_u},
1.0
\right)
=
\min(0.758,1.568,1.86,1.0)=0.758
$$

$$
k_1=
\min\left(
2.8\frac{e_2}{d_0}-1.7,
1.4\frac{p_2}{d_0}-1.7,
2.5
\right)
=2.5
$$

Quindi:

$$
F_{b,Rd}=
\frac{2.5 \cdot 0.758 \cdot 430 \cdot 20 \cdot 12}{1.25}
=156.4\,\mathrm{kN}
$$

Anche il rifollamento non governa:

$$
\eta_b=\frac{69.5}{156.4}=0.44
$$

In questo caso il gambo del bullone è più vicino al limite della piastra. In altri dettagli, soprattutto con piastre sottili o bordi ridotti, può accadere l'opposto. È per questo che taglio del bullone e rifollamento vanno sempre letti separatamente.

**Se il giunto è precaricato**

Se lo stesso gruppo venisse concepito come unione ad attrito con bulloni precaricati, il controllo meccanico cambierebbe. Per un M20 8.8:

$$
F_{p,C}=0.7\,f_{ub}\,A_s
=0.7 \cdot 800 \cdot 245
=137.2\,\mathrm{kN}
$$

Assumendo superficie di classe con coefficiente di attrito $\mu=0.40$, foro normale e $\gamma_{M3}=1.25$:

$$
F_{s,Rd}=
\frac{\mu\,F_{p,C}}{\gamma_{M3}}
=\frac{0.40 \cdot 137.2}{1.25}
=43.9\,\mathrm{kN}
$$

Per 4 bulloni e una sola superficie d'attrito:

$$
F_{s,Rd,gruppo}=4 \cdot 43.9 = 175.6\,\mathrm{kN}
$$

Il nodo sarebbe ancora verificato rispetto a $V_{Ed}=160\,\mathrm{kN}$, ma la logica progettuale non sarebbe più "taglio sul gambo", bensì "assenza di scorrimento allo stato di progetto". Questa distinzione va dichiarata fin dall'inizio del calcolo e documentata anche nei particolari costruttivi.

**Quando il gruppo bullonato non basta**

Ci sono almeno tre casi in cui il controllo del solo bullone più sollecitato non chiude il problema:

- **Trazione e prying action.** Nelle end-plate tese, il bullone può vedere trazione elevata anche se il taglio resta moderato. In quel caso si entra nel dominio di interazione taglio-trazione e nella deformabilità della piastra di estremità.
- **Block tearing.** Con bordi corti, passi ridotti o lamiere sottili, la rottura del blocco di materiale attorno ai fori può governare il nodo prima del collasso del bullone.
- **Esecuzione e tolleranze.** Classe dei fori, preparazione delle superfici, serraggio e accessibilita in montaggio possono cambiare la verifica realmente pertinente, soprattutto nei giunti precaricati.

Una check-list minimale da studio comprende quindi:

1. geometria del gruppo con coordinate e quote effettive;
2. classe del bullone e materiale della piastra;
3. distinzione tra unione a rifollamento e unione ad attrito;
4. verifica locale del bullone critico;
5. verifica della piastra e del percorso di rottura nella lamiera.

**Lettura da progettista**

Il numero finale da solo conta poco. In un dettaglio bullonato bisogna capire da dove arriva la domanda e quale elemento governa la resistenza. In questo esempio la variabile dominante non è la classe del materiale della piastra, ma l'eccentricità del carico rispetto al baricentro del gruppo. Se il nodo viene riprogettato spostando la linea di azione della reazione o allargando il braccio del gruppo bullonato, il beneficio meccanico è immediato e misurabile.

E qui uno strumento digitale diventa utile solo se resta trasparente. Una piccola routine nello spirito di StruHub, o una app di supporto al progetto acciaio come `SteelSectionCheck` o `Profilario`, ha senso quando mantiene visibili coordinate, formule, coefficienti e rapporto domanda/capacità, non quando restituisce un solo semaforo finale.

**Riferimenti normativi e tecnici**

- D.M. 17 gennaio 2018, "Aggiornamento delle Norme tecniche per le costruzioni", §4.2.4 Costruzioni di acciaio, Ministero delle Infrastrutture e dei Trasporti, Supplemento ordinario alla Gazzetta Ufficiale n. 42 del 20 febbraio 2018.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, §C4.2.4, "Istruzioni per l'applicazione dell'Aggiornamento delle Norme tecniche per le costruzioni di cui al decreto ministeriale 17 gennaio 2018".
- UNI EN 1993-1-8:2005, Eurocodice 3 - Progettazione delle strutture di acciaio - Parte 1-8: Progettazione dei collegamenti.
- UNI EN 1090-2:2018, Esecuzione di strutture di acciaio e di alluminio - Parte 2: Requisiti tecnici per strutture di acciaio.
- UNI EN 1993-1-1:2014, Eurocodice 3 - Progettazione delle strutture di acciaio - Parte 1-1: Regole generali e regole per gli edifici.
