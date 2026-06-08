---
layout: post
title: "La spinta idraulica sulle Pile da Ponte"
description: "Riferimento tecnico StruHub con schema idraulico, caso numerico e riferimenti tracciabili."
date: 2026-01-24
order: 24
permalink: /posts/la-spinta-idraulica-sulle-pile-da.html
meta: "Quaderno tecnico - 1450 parole circa"
---

**Quando la corrente entra davvero nel modello**

La spinta idraulica su una pila non e un accessorio del calcolo del ponte. In un attraversamento fluviale reale incide sulla sottostruttura, modifica il quadro sollecitativo della fondazione e va letta insieme alla vulnerabilita per scalzamento. Limitarsi a una sola formula di drag e spesso troppo poco: bisogna chiarire quale area e investita dal flusso, quale coefficiente di forma e stato assunto, se esiste rigurgito monte-valle e a quale quota agisce la risultante.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, NTC 2018, con particolare attenzione ai Capitoli 3, 5 e 6.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7.
- FHWA, *Hydraulic Design of Safe Bridges*, HDS 7, Publication No. HIF-24-001, 2024.
- FHWA, *Evaluating Scour at Bridges*, HEC-18, Fifth Edition, April 2012.

Nota StruHub.

Nel lavoro quotidiano conviene mantenere separati tre livelli: azione idraulica locale sulla pila, effetti strutturali sulla sottostruttura, perdita di confinamento dovuta allo scalzamento. E' anche la logica utile in piccole utility dedicate come `ForzaIdrodinamica`, `Scalzamento` e `ScoglieraPila`: il numero finale serve solo se resta collegato a geometria, tirante, velocita, coefficiente di drag, skew del flusso e quota effettiva della fondazione.

![Schema delle componenti della spinta idraulica su una pila da ponte]({{ site.baseurl }}/assets/images/la-spinta-idraulica-sulle-pile-da-schema.svg)

**Drag: formula corretta, area corretta**

La relazione di base resta quella del drag:

$$
F_D = \frac{1}{2}\rho C_D A_p V^2
$$

dove:

- $\rho$ e la densita dell'acqua;
- $C_D$ e il coefficiente di drag legato alla forma;
- $A_p$ e l'area proiettata normale al flusso;
- $V$ e la velocita media che investe la pila.

Per una pila verticale investita fino al tirante $h$:

$$
A_p = b_\perp h
$$

dove $b_\perp$ e la larghezza proiettata ortogonale alla corrente. Questo dettaglio conta piu di quanto sembri, perche nei casi reali la corrente non sempre arriva perfettamente normale all'asse della pila. Se la pianta e assimilabile a un rettangolo di dimensioni $b \times l$ e l'angolo tra asse maggiore e flusso e $\alpha$, una stima geometrica utile in predimensionamento e:

$$
b_\perp \approx b \cos \alpha + l \sin \alpha
$$

Questa non sostituisce lo studio idraulico, ma evita un errore frequente: usare sempre la sola larghezza frontale nominale anche quando la pila e in skew oppure il vettore di velocita non e allineato all'asse del ponte.

**La forma della sezione conta davvero**

Il coefficiente $C_D$ non dovrebbe essere scelto "a memoria". Nelle tabelle richiamate da FHWA HDS-7 e dal manuale HEC-RAS compaiono valori molto diversi a seconda della forma della pila. Alcuni valori tipici sono:

- pila circolare: $C_D \approx 1.20$;
- pila a prua quadra: $C_D \approx 2.00$;
- pila con punta triangolare a $30^\circ$: $C_D \approx 1.00$;
- pila ellittica con rapporto lunghezza/larghezza pari a 4:1: $C_D \approx 0.32$.

![Confronto tra forme di pila e loro risposta idraulica qualitativa]({{ site.baseurl }}/assets/images/la-spinta-idraulica-sulle-pile-da-01.jpg)

Questo significa che, a parita di area proiettata e velocita, la forza orizzontale puo cambiare di parecchio solo per effetto della geometria. E' un punto progettualmente molto utile: se il ponte e sensibile a spinte orizzontali o a momenti al piede, il profilo della pila non e solo una scelta architettonica, ma una leva meccanica.

Un confronto rapido aiuta a fissare gli ordini di grandezza. Per $A_p=9.24\,\mathrm{m^2}$ e $V=3.0\,\mathrm{m/s}$ si ottiene:

- con $C_D=2.00$, $F_D=83.2\,\mathrm{kN}$;
- con $C_D=1.20$, $F_D=49.9\,\mathrm{kN}$;
- con $C_D=0.32$, $F_D=13.3\,\mathrm{kN}$.

La differenza tra prua quadra e profilo ellittico non e un dettaglio numerico: e quasi un rapporto di uno a sei.

**Quando il drag non basta**

La corrente intorno alla pila puo generare un rigurgito tra monte e valle. Se dal modello idraulico o dal rilievo di piena emerge una differenza di livelli, la sola azione dinamica puo sottostimare la spinta totale. In prima approssimazione, con base allo stesso fondo alveo e livelli $h_m$ a monte e $h_v$ a valle, la componente idrostatica differenziale puo essere stimata come:

$$
F_H = \frac{1}{2}\gamma_w b \left(h_m^2 - h_v^2\right)
$$

dove $\gamma_w$ e il peso specifico dell'acqua e $b$ la larghezza frontale efficace della pila.

La quota della risultante idrostatica, misurata dal fondo, vale:

$$
z_H = \frac{h_m^3 - h_v^3}{3\left(h_m^2 - h_v^2\right)}
$$

Per la componente di drag, se si assume una distribuzione uniforme sul tirante investito:

$$
z_D \approx \frac{h}{2}
$$

La risultante totale e quindi:

$$
F_{tot} = F_D + F_H
$$

e il momento rispetto al piano di fondazione diventa:

$$
M_{base} = F_D z_D + F_H z_H
$$

Questa scrittura e semplice, ma mette ordine in un passaggio spesso opaco nei report: non basta sapere "quanto vale la spinta", bisogna sapere anche dove agisce.

**Caso numerico di predimensionamento**

Si consideri una pila di ponte fluviale in calcestruzzo armato con sezione assimilabile a un cilindro allungato. Assumiamo:

- larghezza proiettata $b_\perp = 2.20\,\mathrm{m}$;
- tirante investito $h = 4.20\,\mathrm{m}$;
- velocita media della corrente $V = 3.0\,\mathrm{m/s}$;
- coefficiente di drag $C_D = 1.20$;
- livello a monte $h_m = 4.55\,\mathrm{m}$;
- livello a valle $h_v = 4.20\,\mathrm{m}$.

L'area proiettata e:

$$
A_p = 2.20 \cdot 4.20 = 9.24\,\mathrm{m^2}
$$

La componente dinamica vale:

$$
F_D = \frac{1}{2}\cdot 1000 \cdot 1.20 \cdot 9.24 \cdot 3.0^2 = 49.9\,\mathrm{kN}
$$

La componente idrostatica differenziale vale:

$$
F_H = \frac{1}{2}\cdot 9.81 \cdot 2.20 \cdot \left(4.55^2 - 4.20^2\right)=33.1\,\mathrm{kN}
$$

La forza totale e quindi:

$$
F_{tot}=49.9+33.1=83.0\,\mathrm{kN}
$$

Le quote di applicazione sono:

$$
z_D \approx \frac{4.20}{2}=2.10\,\mathrm{m}
$$

$$
z_H = \frac{4.55^3 - 4.20^3}{3\left(4.55^2 - 4.20^2\right)} \approx 2.19\,\mathrm{m}
$$

La risultante agisce quindi circa a:

$$
z_{tot}=\frac{F_D z_D + F_H z_H}{F_{tot}} \approx 2.14\,\mathrm{m}
$$

e il momento rispetto al piano di imposta vale:

$$
M_{base}=F_{tot} z_{tot}\approx 177\,\mathrm{kNm}
$$

Lettura tecnica. Se nello stesso caso si fosse considerato soltanto il drag, il momento di base sarebbe sceso a circa $105\,\mathrm{kNm}$. In questo esempio il contributo dovuto al rigurgito non e dominante, ma aumenta il momento di oltre il 65%. E' il tipo di differenza che puo cambiare la lettura di una verifica su palo, plinto o fusto della pila.

**Dalla spinta locale alla verifica del ponte**

La forza idraulica non dovrebbe restare isolata nel capitolo idraulico. Va trasferita al modello strutturale con almeno quattro informazioni esplicite:

- valore della forza orizzontale per ciascuno scenario idraulico;
- quota di applicazione della risultante;
- eventuale eccentricita o torsione se il flusso arriva in skew;
- quota di fondazione efficace nella configurazione scalzata.

Qui entra il collegamento con la verifica geotecnica. Se lo studio di alveo fornisce una profondita di scalzamento $y_s$, il problema non e piu solo una forza orizzontale sul fusto: cambia anche il vincolo della sottostruttura. In pratica:

$$
h_{inf,eff} = h_{inf,0} - y_s
$$

dove $h_{inf,0}$ e l'affondamento iniziale e $h_{inf,eff}$ quello residuo nella condizione di piena. Anche una pila formalmente verificata per la sola forza orizzontale puo diventare critica se la stessa azione agisce con minore confinamento laterale o con una lunghezza libera maggiore.

Per questo, in un workflow realistico di studio ponte, ha senso usare strumenti distinti ma coerenti:

- una routine tipo `ForzaIdrodinamica` per il calcolo rapido di $F_D$, area proiettata e sensibilita a $C_D$;
- una routine tipo `Scalzamento` per portare nel report la profondita di erosione locale e la quota di fondazione residua;
- una routine tipo `ScoglieraPila` quando si deve motivare il dimensionamento della protezione al piede contro l'erosione.

Il valore aggiunto non e il "software" in se, ma la tracciabilita della catena logica: input idraulici, trasformazione in azioni, ipotesi di forma, confronto tra scenari e conseguenze sulla fondazione.

**Errori ricorrenti da evitare**

Ci sono alcuni errori molto frequenti nei calcoli preliminari sulle pile in alveo:

- usare un solo coefficiente $C_D$ per tutte le geometrie;
- assumere che la larghezza investita coincida sempre con la larghezza nominale in pianta;
- fermarsi alla sola spinta dinamica anche in presenza di rigurgito evidente;
- verificare il fusto della pila senza riportare nel modello la quota di scalzamento;
- indicare nel report la forza finale senza dichiarare tirante, velocita, forma e scenario idraulico di riferimento.

Un buon elaborato tecnico dovrebbe invece permettere a un altro progettista di rifare il conto a mano in pochi minuti, almeno come controllo di ordine di grandezza.

**Dal numero alla decisione di progetto**

La spinta idraulica diventa utile quando guida una decisione: cambiare la forma della pila, aumentare la snellezza del rostro, proteggere il piede con scogliera, ridefinire la profondita di fondazione, distinguere le verifiche della piena ordinaria da quelle dell'evento estremo. In tutti questi casi il dato piu importante non e il singolo valore di $F_{tot}$, ma capire da quali parametri dipende e quanto cambia al variare di forma, livello e velocita.

Per una pila da ponte, il vero controllo professionale non e "quanto spinge l'acqua" in astratto, ma se la sottostruttura resta verificata nello scenario idraulico che governa davvero: forza, quota di applicazione, scalzamento e protezione dell'alveo devono parlare tra loro.

Riferimenti normativi e bibliografici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle "Norme tecniche per le costruzioni"*, Ministero delle Infrastrutture e dei Trasporti, G.U. n. 42 del 20 febbraio 2018, S.O. n. 8.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle "Norme tecniche per le costruzioni" di cui al decreto ministeriale 17 gennaio 2018*, G.U. n. 35 del 11 febbraio 2019, S.O. n. 5.
- Federal Highway Administration, *Hydraulic Design of Safe Bridges*, Hydraulic Design Series No. 7, Publication No. HIF-24-001, 2024.
- Federal Highway Administration, *Evaluating Scour at Bridges*, Hydraulic Engineering Circular No. 18, Fifth Edition, Publication No. HIF-12-003, April 2012.
