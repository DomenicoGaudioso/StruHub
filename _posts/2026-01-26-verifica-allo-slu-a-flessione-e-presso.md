---
layout: post
title: "Verifica allo SLU a flessione e presso-flessione nelle sezioni in c.a."
description: "Riferimento tecnico StruHub con esempio numerico completo, schema della sezione e riferimenti normativi tracciabili."
date: 2026-01-26
order: 26
permalink: /posts/verifica-allo-slu-a-flessione-e-presso.html
meta: "Quaderno tecnico - 1510 parole circa"
---

![Schema di deformazioni, blocco di compressione e forze interne in una sezione rettangolare in c.a.]({{ site.baseurl }}/assets/images/verifica-allo-slu-a-flessione-e-presso-schema.svg)

**La sezione non si verifica con una formula isolata**

Nella pratica di progetto la verifica allo SLU di una sezione in cemento armato e' un percorso: si fissano materiali e copriferro, si sceglie il legame di calcolo, si impone la compatibilita' delle deformazioni, si trovano asse neutro e forze interne e solo alla fine si confronta la domanda con la capacita'. Saltare uno di questi passaggi produce risultati poco revisionabili, soprattutto quando la stessa sezione passa da flessione semplice a presso-flessione.

Le grandezze minime da dichiarare sono:

- geometria della sezione $b \times h$;
- posizione delle armature e copriferro reale;
- classe del calcestruzzo e acciaio di armatura;
- combinazione di progetto $(N_{Ed}, M_{Ed})$;
- convenzione dei segni e unita' di misura.

Per una verifica manuale o con una piccola routine numerica, il punto chiave resta questo:

$$
N_{Rd}=\sum F_{c,Rd}+\sum F_{s,Rd},
\qquad
M_{Rd}=\sum F_{Rd}\,z
$$

In altre parole, la resistenza nasce dall'equilibrio tra blocco compresso di calcestruzzo e armature, non da un coefficiente isolato.

**Ipotesi di base da tenere visibili**

Per le sezioni ordinarie in c.a. le ipotesi classiche, coerenti con NTC 2018 e con l'impostazione dell'Eurocodice 2, sono:

- conservazione delle sezioni piane;
- perfetta aderenza acciaio-calcestruzzo;
- resistenza a trazione del calcestruzzo trascurata allo SLU;
- crisi del calcestruzzo a compressione al raggiungimento della deformazione ultima;
- legame dell'acciaio assunto elastico-perfettamente plastico di progetto.

Le resistenze di progetto da cui partire sono:

$$
f_{cd}=\frac{\alpha_{cc} f_{ck}}{\gamma_c},
\qquad
f_{yd}=\frac{f_{yk}}{\gamma_s}
$$

Assumendo $\alpha_{cc}=0.85$, $\gamma_c=1.50$ e $\gamma_s=1.15$, per una sezione in calcestruzzo C30/37 con armature B450C si ottiene:

$$
f_{cd}=\frac{0.85 \cdot 30}{1.50}=17.0\,\mathrm{MPa},
\qquad
f_{yd}=\frac{450}{1.15}=391\,\mathrm{MPa}
$$

Questi sono i numeri che entrano davvero nell'equilibrio di sezione.

**Caso 1: flessione semplice su una trave rettangolare**

Si consideri una sezione rettangolare di base $b=300\,\mathrm{mm}$ e altezza $h=500\,\mathrm{mm}$, con copriferro nominale $c_{nom}=40\,\mathrm{mm}$, staffe $\phi 8$ e 4 barre $\phi 20$ all'intradosso. Per una verifica rapida in flessione positiva, la presenza dell'armatura superiore viene trascurata in modo conservativo.

L'area tesa vale:

$$
A_s=4 \cdot \frac{\pi 20^2}{4}=1256\,\mathrm{mm^2}
$$

La distanza utile delle barre tese dal lembo compresso e':

$$
d=500-40-8-10=442\,\mathrm{mm}
$$

Se il momento sollecitante di progetto in campata e' $M_{Ed}=175\,\mathrm{kNm}$, un controllo manuale trasparente puo' essere impostato con il blocco rettangolare equivalente:

$$
x=\frac{A_s f_{yd}}{0.8\,b\,f_{cd}}=
\frac{1256 \cdot 391}{0.8 \cdot 300 \cdot 17.0}
=120.4\,\mathrm{mm}
$$

Il braccio interno diventa:

$$
z=d-0.4x=442-0.4 \cdot 120.4=393.8\,\mathrm{mm}
$$

e quindi:

$$
M_{Rd}=A_s f_{yd} z=
1256 \cdot 391 \cdot 393.8
\cdot 10^{-6}
=193.4\,\mathrm{kNm}
$$

Il rapporto domanda/capacita' e':

$$
\eta_M=\frac{M_{Ed}}{M_{Rd}}=
\frac{175}{193.4}=0.91
$$

La sezione e' verificata, ma il margine non e' grande. In pratica, questa e' la situazione tipica in cui il progettista non dovrebbe limitarsi al solo valore finale: conviene controllare anche copriferro effettivo, diametro realmente disponibile in officina, possibilita' di aumentare la base compressa o di redistribuire il momento nella modellazione globale.

**Dove si nascondono gli errori piu' frequenti**

Su verifiche come questa i due errori ricorrenti non sono "di formula", ma di tracciabilita':

- usare $d$ geometrico invece della distanza reale al baricentro delle barre;
- riportare $M_{Ed}$ in $\mathrm{kNm}$ e le forze interne in $\mathrm{N}$ o $\mathrm{mm}$ senza coerenza di unita'.

Un tool serio, anche molto semplice, ha senso solo se rende espliciti questi passaggi. E' questo il tipo di logica utile in un calcolatore di sezione nello spirito StruHub o di una piccola utility CivilBox: mostrare asse neutro, braccio interno e rapporto $E_d/R_d$, non soltanto un esito verde o rosso.

**Caso 2: presso-flessione retta su una sezione con armatura simmetrica**

Consideriamo ora la stessa sezione $300 \times 500\,\mathrm{mm}$, ma con 4 barre $\phi 20$ sia all'estradosso sia all'intradosso. La distanza del baricentro delle armature superiori dal bordo compresso vale:

$$
d'=40+8+10=58\,\mathrm{mm}
$$

Supponiamo di dover verificare una combinazione di progetto realistica per un pilastro o per un elemento di estremita' di telaio:

$$
N_{Ed}=700\,\mathrm{kN},
\qquad
M_{Ed}=250\,\mathrm{kNm}
$$

Assumendo, per il punto analizzato del dominio resistente, che entrambe le armature abbiano raggiunto la tensione di progetto, l'equilibrio assiale porta a:

$$
N_{Rd}=0.8\,b\,x\,f_{cd}
$$

perche' il contributo di compressione dell'armatura superiore e quello di trazione dell'armatura inferiore si compensano in valore assoluto. Da qui:

$$
x=\frac{N_{Ed}}{0.8\,b\,f_{cd}}=
\frac{700000}{0.8 \cdot 300 \cdot 17.0}
=171.6\,\mathrm{mm}
$$

Il controllo di congruenza sulle deformazioni e' rapido ma essenziale:

$$
\varepsilon_s=
\frac{d-x}{x}\,\varepsilon_{cu}=
\frac{442-171.6}{171.6} \cdot 0.35\%
=0.55\%
$$

quindi l'armatura tesa e' effettivamente oltre lo snervamento. Per l'armatura compressa:

$$
\varepsilon'_s=
\frac{x-d'}{x}\,\varepsilon_{cu}=
\frac{171.6-58}{171.6} \cdot 0.35\%
=0.23\%
$$

anche questo valore e' compatibile con l'ipotesi di snervamento in compressione.

Le forze interne risultano:

$$
C_c=0.8\,b\,x\,f_{cd}=700\,\mathrm{kN}
$$

$$
C_s=A'_s f_{yd}=491\,\mathrm{kN},
\qquad
T_s=A_s f_{yd}=491\,\mathrm{kN}
$$

Il momento resistente, calcolato rispetto al baricentro della sezione, vale:

$$
M_{Rd}=C_c(250-0.4x)+C_s(250-d')+T_s(d-250)
$$

$$
M_{Rd}=700 \cdot 181.4 + 491 \cdot 192 + 491 \cdot 192
=315.7 \cdot 10^3\,\mathrm{kN\,mm}
$$

che in $\mathrm{kNm}$ diventa:

$$
M_{Rd}=
\left(700 \cdot 181.4 + 491 \cdot 192 + 491 \cdot 192\right)\cdot 10^{-3}
=315.7\,\mathrm{kNm}
$$

ovvero:

$$
\eta_{NM}=\frac{M_{Ed}}{M_{Rd}}=
\frac{250}{315.7}=0.79
$$

La lettura tecnica e' utile: in questo punto del dominio la compressione assiale sposta l'asse neutro verso il basso e fa crescere la zona compressa; la verifica non e' governata dall'acciaio, ma dal modo in cui la combinazione $N$-$M$ utilizza il blocco di calcestruzzo.

**Dal controllo manuale al dominio di interazione**

Quando le combinazioni aumentano o la sezione smette di essere rettangolare semplice, il passaggio corretto non e' "complicare il foglio Excel", ma costruire il dominio di interazione $N$-$M$ mantenendo visibili le ipotesi di calcolo.

![Diagramma di interazione momento-sforzo normale per la sezione analizzata]({{ site.baseurl }}/assets/images/verifica-allo-slu-a-flessione-e-presso-04.png)

Questa e' la vera utilita' operativa del dominio: non tanto produrre un'immagine elegante, quanto capire rapidamente se una combinazione si trova vicino al ramo a forte compressione, al ramo quasi puramente flessionale o alla zona in cui piccole variazioni del copriferro e dell'armatura cambiano sensibilmente il margine disponibile.

**Checklist pratica prima di chiudere la verifica**

Per una relazione di calcolo o per una revisione interna conviene lasciare sempre espliciti questi punti:

1. materiali e resistenze di progetto effettivamente usate;
2. geometria della sezione con quote ai baricentri delle armature;
3. convenzione dei segni di $N_{Ed}$ e $M_{Ed}$;
4. posizione dell'asse neutro e controllo delle deformazioni;
5. rapporto finale $\eta=E_d/R_d$ con l'unita' di misura coerente.

Se uno di questi passaggi manca, il numero finale resta poco auditabile anche se formalmente corretto.

**Riferimenti normativi e tecnici**

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme tecniche per le costruzioni*, Capitolo 4, par. 4.1.2.1.2, Ministero delle Infrastrutture e dei Trasporti, G.U. n. 42 del 20 febbraio 2018, Suppl. Ord. n. 8.
- Circolare 21 gennaio 2019, n. 7 C.S.LL.PP., *Istruzioni per l'applicazione dell'Aggiornamento delle "Norme tecniche per le costruzioni" di cui al decreto ministeriale 17 gennaio 2018*, par. C4.1.2.1.2, G.U. n. 35 del 11 febbraio 2019, Suppl. Ord. n. 5.
- UNI EN 1992-1-1, Eurocodice 2 - Progettazione delle strutture di calcestruzzo - Parte 1-1: Regole generali e regole per gli edifici. Per chi lavora con riferimenti aggiornati, la scheda UNI segnala la sostituzione della edizione 2015 con la UNI EN 1992-1-1:2024 dal 18 gennaio 2024.
