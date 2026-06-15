---
layout: post
title: "Verifica tensionale di sezioni composte acciaio-calcestruzzo"
description: "Riferimento tecnico StruHub con esempio numerico completo, schema della sezione composta e riferimenti normativi tracciabili."
date: 2026-01-19
order: 19
permalink: /posts/verifica-tensionale-sezioni-composte.html
meta: "Quaderno tecnico - 1910 parole circa"
---

![Schema della sezione composta, della trasformazione elastica e della tabella tensionale di controllo]({{ site.baseurl }}/assets/images/verifica-tensionale-sezioni-composte-schema.svg)

**La tensione finale non nasce da una sola sezione**

Nelle travi composte acciaio-calcestruzzo l'errore piu comune non e nella formula di Navier, ma nell'oggetto a cui la formula viene applicata. Sezione metallica in varo, sezione composta in esercizio, soletta efficace in campata positiva, soletta fessurata o parzialmente esclusa in zona di momento negativo: ogni fase cambia baricentro, inerzia e fibre governanti.

Per un professionista il punto non e riempire una tabella di $\sigma$, ma poter ricostruire questa catena:

- quale porzione di sezione sta collaborando davvero;
- con quale modulo elastico del calcestruzzo la si sta omogeneizzando;
- quale combinazione genera la tensione governante;
- se il punto critico e in calcestruzzo, nell'acciaio o all'interfaccia.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme tecniche per le costruzioni*, con richiamo al Capitolo 4 per le costruzioni composte acciaio-calcestruzzo e al Capitolo 5 per i ponti.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, istruzioni applicative delle NTC 2018 per modellazione, combinazioni e lettura delle verifiche di esercizio.
- UNI EN 1994-2:2006, Eurocodice 4 - Progettazione delle strutture composte acciaio-calcestruzzo - Parte 2: Regole generali e regole per i ponti.
- UNI EN 1993-1-1:2014, Eurocodice 3 - Progettazione delle strutture di acciaio.
- UNI EN 1992-1-1:2015, Eurocodice 2 - Progettazione delle strutture di calcestruzzo.
- UNI EN 1991-2:2005, Eurocodice 1 - Azioni sulle strutture - Parte 2: Carichi da traffico sui ponti.

Nota StruHub.

Nel lavoro quotidiano conviene tenere distinti tre livelli: proprieta geometriche della sola carpenteria, trasformazione elastica della sezione composta e tabella tensionale delle fibre critiche. E' la stessa logica utile quando si parte da un archivio sezionale come `Profilario` per leggere area e inerzie della trave metallica e si chiude il controllo con una routine nello spirito di `SteelSectionCheck`, che ha senso solo se rende rifacibili asse neutro, moduli resistenti, segni delle tensioni e input della relazione.

**Quale sezione stai verificando davvero**

Prima di scrivere una sola formula conviene dichiarare almeno questi dati:

- geometria della carpenteria metallica con quote e spessori reali;
- larghezza efficace della soletta $b_{eff}$ adottata nella combinazione considerata;
- fase di calcolo: varo, getto, esercizio a breve termine, esercizio a lungo termine;
- modulo del calcestruzzo usato nella trasformazione, cioe $E_{cm}$ oppure $E_{c,eff}=E_{cm}/(1+\phi)$ se si stanno leggendo gli effetti viscosi;
- ipotesi di collaborazione: piena, parziale oppure sezione fessurata in zona tesa;
- convenzione dei segni per $N$, $M_y$ e coordinate delle fibre.

Se uno di questi punti manca, il valore finale di tensione puo essere numericamente ordinato ma tecnicamente debole.

Per le campate ordinarie da ponte, con momento positivo, la soletta lavora in compressione e la trasformazione elastica della sezione composta e un ottimo controllo di primo livello. Sopra la pila, invece, il quadro cambia: il calcestruzzo in zona tesa puo fessurarsi, la collaborazione non e piu la stessa e spesso governa la fibra superiore dell'acciaio o l'armatura della soletta. Usare la stessa sezione omogeneizzata in entrambe le zone e una scorciatoia che porta facilmente a tensioni fuorvianti.

**Il modello elastico che serve davvero**

Per la verifica tensionale in campo elastico si usa una sezione omogeneizzata. Se si trasformano le aree di calcestruzzo in aree equivalenti di acciaio, il coefficiente modulare vale:

$$
n = \frac{E_s}{E_c}
$$

Per gli effetti a lungo termine, una scrittura pratica e:

$$
E_{c,eff} = \frac{E_{cm}}{1+\phi}
\qquad
\Longrightarrow
\qquad
n_{LT} = \frac{E_s}{E_{c,eff}}
$$

L'area equivalente della soletta diventa:

$$
A_c^{*} = \frac{A_c}{n}
$$

e il baricentro della sezione omogeneizzata:

$$
y_G = \frac{\sum_i A_i^{*} y_i}{\sum_i A_i^{*}}
$$

Il momento di inerzia si ricostruisce con Steiner:

$$
I^{*} = \sum_i \left( I_i^{*} + A_i^{*}(y_i-y_G)^2 \right)
$$

Una volta ottenuti $A^{*}$, $y_G$ e $I^{*}$, la tensione equivalente in una fibra di quota $y$ si legge con:

$$
\sigma^{*}(y)=\frac{N_{Ed}}{A^{*}}+\frac{M_{Ed}}{I^{*}}(y-y_G)
$$

Per l'acciaio:

$$
\sigma_s(y)=\sigma^{*}(y)
$$

Per il calcestruzzo:

$$
\sigma_c(y)=\frac{\sigma^{*}(y)}{n}
$$

Questa catena e semplice da scrivere ma molto utile da auditare, perche obbliga a mostrare in chiaro dove nasce ogni numero. Quando la verifica e quasi al limite, conviene sempre allegare almeno le quattro fibre che davvero orientano il progetto:

- estradosso soletta;
- intradosso soletta in prossimita dell'interfaccia;
- lembo superiore della trave metallica;
- lembo inferiore della trave metallica.

Un controllo spesso dimenticato e il flusso di taglio all'interfaccia, indispensabile per capire se la tabella tensionale e coerente con la progettazione dei connettori:

$$
q = \frac{V_{Ed} Q^{*}}{I^{*}}
$$

dove $Q^{*}$ e il momento statico della parte di sezione al di sopra dell'interfaccia, calcolato nella stessa trasformazione elastica usata per le tensioni.

**Caso numerico: trave saldata da ponte con soletta collaborante**

Si consideri una sezione di campata positiva coerente con un workflow reale: trave metallica saldata tipo doppio T, cioe la stessa famiglia geometrica che si puo preimpostare in `SteelSectionCheck`, abbinata a una soletta efficace da ponte la cui larghezza e tracciata in relazione.

Dati geometrici della sola carpenteria metallica:

- piattabanda inferiore $800 \times 70 \, \mathrm{mm}$;
- anima $25 \times 1760 \, \mathrm{mm}$;
- piattabanda superiore $600 \times 50 \, \mathrm{mm}$;
- altezza totale trave acciaio $h_s = 1880 \, \mathrm{mm}$.

Dati della soletta collaborante:

- larghezza efficace assunta $b_{eff}=3200 \, \mathrm{mm}$;
- spessore soletta $t_c=250 \, \mathrm{mm}$;
- calcestruzzo C40/50;
- acciaio S355.

Le proprieta geometriche di base sono:

$$
A_s = 130000 \, \mathrm{mm^2}
$$

$$
A_c = 800000 \, \mathrm{mm^2}
$$

Per il controllo elastico a breve termine assumiamo:

$$
E_s = 210000 \, \mathrm{MPa},
\qquad
E_c = 36000 \, \mathrm{MPa}
$$

quindi:

$$
n_0 = \frac{210000}{36000} = 5.83
$$

L'area equivalente della soletta vale:

$$
A_c^{*} = \frac{800000}{5.83} = 137143 \, \mathrm{mm^2}
$$

e l'area totale omogeneizzata:

$$
A^{*} = A_s + A_c^{*} = 267143 \, \mathrm{mm^2}
$$

Ricostruendo il baricentro rispetto al lembo inferiore dell'acciaio si ottiene:

$$
y_G = 1401 \, \mathrm{mm}
$$

mentre il momento di inerzia omogeneizzato risulta:

$$
I^{*} = 1.82 \cdot 10^{11} \, \mathrm{mm^4}
$$

Assumiamo ora, per una combinazione di esercizio governante in campata:

$$
N_{Ed} = -1200 \, \mathrm{kN}
$$

$$
M_{Ed} = 22500 \, \mathrm{kNm}
$$

$$
V_{Ed} = 1450 \, \mathrm{kN}
$$

con convenzione dei segni tale che la compressione all'estradosso della soletta risulti positiva e la trazione al lembo inferiore dell'acciaio negativa.

Applicando la formula elastica alle fibre principali si ottiene:

| Fibra di controllo | Quota letta | Tensione |
| --- | --- | ---: |
| Estradosso soletta | $y = 2130 \, \mathrm{mm}$ | $+14.7 \, \mathrm{MPa}$ |
| Intradosso soletta / interfaccia | $y = 1880 \, \mathrm{mm}$ | $+9.4 \, \mathrm{MPa}$ |
| Lembo superiore acciaio | $y = 1880 \, \mathrm{mm}$ | $+54.8 \, \mathrm{MPa}$ |
| Lembo inferiore acciaio | $y = 0$ | $-178.0 \, \mathrm{MPa}$ |

La lettura progettuale utile non e soltanto "la sezione passa". E' questa:

- il calcestruzzo superiore lavora in compressione moderata;
- la fibra superiore dell'acciaio e ancora lontana da livelli critici;
- il punto che merita attenzione e il lembo inferiore della trave metallica, che porta la trazione massima di campata;
- la differenza di circa $45 \, \mathrm{MPa}$ tra lembo superiore e inferiore dell'acciaio rende evidente che l'azione e dominata dal momento flettente, non dallo sforzo normale.

Se si vuole tradurre il risultato in un indicatore di screening immediato, il rapporto con le resistenze caratteristiche vale:

$$
\eta_{s,bot}=\frac{178}{355}=0.50
$$

$$
\eta_{c,top}=\frac{14.7}{40}=0.37
$$

Non e ancora una verifica normativa completa di per se, perche il progetto deve usare la combinazione e il limite esatti della propria procedura, ma e gia un controllo molto piu utile del solo "massimo momento in campata".

**Lo stesso caso cambia se introduci il lungo termine**

Nel post sulle fasi costruttive e sugli effetti lenti il punto chiave e che il calcestruzzo non resta rigido come nel breve termine. Se, per esempio, si assume un coefficiente viscoso $\phi = 2.2$, allora:

$$
E_{c,eff}=\frac{36000}{1+2.2}=11250 \, \mathrm{MPa}
$$

e il coefficiente modulare cresce a:

$$
n_{LT}=\frac{210000}{11250}=18.67
$$

Ricalcolando la sezione omogeneizzata sul lungo termine, la tensione di estradosso della soletta scende a circa:

$$
\sigma_{c,top,LT} \approx +9.6 \, \mathrm{MPa}
$$

ma le tensioni nell'acciaio crescono:

$$
\sigma_{s,top,LT} \approx +134.8 \, \mathrm{MPa}
$$

$$
\sigma_{s,bot,LT} \approx -195.1 \, \mathrm{MPa}
$$

Questa e una lezione pratica importante: se la soletta perde rigidezza efficace per creep, una quota maggiore di flessione ritorna sulla carpenteria metallica. In ufficio il controllo da fare non e soltanto "quanto vale $\phi$?", ma "quali fibre cambiano di piu quando aggiorno $E_c$?".

**Il flusso di taglio che collega tensioni e connettori**

Sul medesimo caso, il momento statico equivalente della soletta rispetto all'interfaccia vale:

$$
Q^{*} = 8.28 \cdot 10^{7} \, \mathrm{mm^3}
$$

Da cui:

$$
q = \frac{1450 \cdot 10^3 \cdot 8.28 \cdot 10^7}{1.82 \cdot 10^{11}}
= 660 \, \mathrm{N/mm}
$$

Il dato e molto utile per una prima lettura dei connettori. Se il progetto prevede due file di pioli con resistenza di progetto pari a $95 \, \mathrm{kN}$ ciascuno, la capacita disponibile per sezione trasversale e:

$$
P_{Rd,tot}=2 \cdot 95 = 190 \, \mathrm{kN}
$$

e il passo teorico massimo compatibile con il picco di flusso vale:

$$
s_{max}=\frac{190000}{660}=288 \, \mathrm{mm}
$$

Naturalmente la disposizione finale dei connettori dipende anche dalle prescrizioni geometriche e costruttive, ma questo controllo dice subito se la tabella tensionale che stai leggendo e coerente con un ordine di grandezza realistico del dettaglio.

**Dove si sbaglia piu spesso**

Nella pratica di progetto gli errori ricorrenti sono quasi sempre questi:

- usare la sezione composta finale anche per le azioni di varo o di getto;
- non distinguere campata positiva e zona di appoggio con soletta fessurata;
- cambiare $b_{eff}$ senza aggiornare baricentro e inerzia;
- confondere tensione equivalente $\sigma^{*}$ e tensione reale del calcestruzzo $\sigma_c$;
- riportare solo il massimo valore assoluto senza combinazione governante, quota della fibra e segno.

Il quarto punto e il piu insidioso: se nella relazione compare la tensione del calcestruzzo senza esplicitare se e gia stata divisa per $n$, il rischio di un errore silenzioso e concreto.

**Che cosa dovrebbe comparire in una relazione seria**

Per trasformare il calcolo in un elaborato davvero controllabile conviene riportare almeno:

- schema della sezione con quote della carpenteria e $b_{eff}$ adottato;
- tabella materiali con $E_s$, $E_c$, classe dell'acciaio, classe del calcestruzzo e, se serve, $\phi$;
- proprieta della sezione omogeneizzata: $A^{*}$, $y_G$, $I^{*}$, moduli resistenti;
- combinazione che governa ciascuna fibra;
- tabella tensionale con segno, unita di misura e quota della fibra;
- controllo del flusso di taglio all'interfaccia se i connettori sono parte del medesimo modello;
- nota esplicita sulle zone escluse dalla stessa modellazione, per esempio la campata negativa con soletta fessurata.

Questa e la differenza tra un foglio che fornisce un numero e una verifica che puo essere riletta sei mesi dopo, magari da un altro progettista dello stesso studio.

**Lettura finale da progettista**

La verifica tensionale di una sezione composta non e una formalita di chiusura. E' il punto in cui si vede se geometria, fase costruttiva, larghezza efficace, modulo del calcestruzzo e schema globale stanno davvero raccontando la stessa struttura. Se anche solo uno di questi pezzi e incoerente, la tensione finale smette di essere una misura del comportamento e diventa un numero fragile.

Per questo gli strumenti digitali servono solo quando lasciano una traccia tecnica completa. Un archivio di profili come `Profilario`, una utility di controllo come `SteelSectionCheck` o un modello piu specifico di impalcato composto hanno valore non perche automatizzano la formula di Navier, ma perche costringono a conservare input, ipotesi, segni, combinazioni e fibre critiche. E' questa tracciabilita, piu del singolo valore di $\sigma$, che rende il risultato professionale.
