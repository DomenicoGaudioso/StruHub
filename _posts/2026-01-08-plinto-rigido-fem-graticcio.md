---
layout: post
title: "Confronto tra modello di plinto rigido e modello FEM a graticcio"
description: "Metodo di calcolo, esempio numerico svolto e workflow reale in PlintoPali per controllare ripartizione, cedimenti e verifiche del gruppo di pali."
date: 2026-01-08
order: 8
permalink: /posts/plinto-rigido-fem-graticcio.html
meta: "Quaderno tecnico - 2900 parole circa"
---

![Schema di principio del confronto tra plinto rigido e plinto FEM a graticcio]({{ site.baseurl }}/assets/images/plinto-rigido-fem-graticcio-schema.svg)

**Perche questo confronto conta davvero**

Nel progetto di un plinto su pali il primo modello utile e quasi sempre quello rigido: e veloce, controllabile a mano e dice subito come si ripartiscono $N$, $M_x$ e $M_y$ sui pali. Il punto critico arriva quando il plinto non e piu tanto rigido da potersi considerare un vincolo cinematica perfetto. In quel momento non basta sapere il valore medio della reazione: bisogna capire se il palo governante cambia, se i cedimenti differenziali crescono e se l'armatura del plinto va letta con un modello deformabile.

Per un professionista, il confronto rigido/FEM non e un esercizio accademico. E' il passaggio che separa un predimensionamento corretto da una verifica difendibile in relazione.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme tecniche per le costruzioni*, con richiamo ai Capitoli 4, 6 e 7.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle Norme tecniche per le costruzioni*.
- UNI EN 1992-1-1:2015, Eurocodice 2 - Progettazione delle strutture di calcestruzzo.
- UNI EN 1997-1:2013, Eurocodice 7 - Progettazione geotecnica - Parte 1: Regole generali.
- Poulos, H. G.; Davis, E. H., *Pile Foundation Analysis and Design*, John Wiley & Sons, 1980.

Nota StruHub.

Su questo tema il collegamento piu naturale e `PlintoPali`, applicativo CivilBox che mette nello stesso flusso tre letture diverse dello stesso caso: portata del palo singolo, ripartizione cinematica delle reazioni e confronto con graticcio FEM del plinto. Usato bene, non sostituisce il controllo a mano: lo rende tracciabile e ripetibile.

**1. Teoria e metodo**

Nel modello rigido si assume che le teste dei pali restino complanari e che il plinto distribuisca le azioni con una legge affine nelle coordinate del gruppo. La reazione verticale del palo $i$ vale:

$$
R_i = \frac{N}{n} + \frac{M_x y_i}{\sum_j y_j^2} + \frac{M_y x_i}{\sum_j x_j^2}
$$

dove:

- $R_i$ e la reazione verticale sul palo $i$ in $\mathrm{kN}$;
- $N$ e il carico normale totale in $\mathrm{kN}$;
- $M_x$, $M_y$ sono i momenti rispetto agli assi baricentrici del gruppo in $\mathrm{kNm}$;
- $x_i$, $y_i$ sono le coordinate del palo rispetto al baricentro in $\mathrm{m}$;
- $n$ e il numero totale di pali.

Questo modello e molto efficace quando:

- il plinto e massiccio rispetto agli interassi;
- il layout dei pali e regolare;
- le rigidezze dei pali sono confrontabili;
- non si e gia vicini al limite geotecnico.

Il controllo di massima sugli eccentricita resta comunque utile:

$$
e_x = \frac{M_y}{N}, \qquad e_y = \frac{M_x}{N}
$$

Se le eccentricita sono piccole, il campo di compressione rimane distribuito su tutti i pali; se crescono o il gruppo diventa irregolare, il problema non e piu solo "quanto vale $R_{max}$", ma "su quale palo cade davvero".

Nel modello FEM a graticcio il plinto viene discretizzato con elementi trave e ogni palo viene rappresentato da una molla verticale:

$$
k_v = \frac{\Delta R}{\Delta s}
$$

dove $k_v$ e la rigidezza assiale equivalente in $\mathrm{kN/m}$, $\Delta R$ la variazione di reazione e $\Delta s$ il corrispondente spostamento verticale. In pratica, il modello deformabile aggiunge tre informazioni che il plinto rigido non ha:

- i cedimenti relativi tra le teste dei pali;
- la possibilita che il palo governante cambi anche con $R_{max}$ quasi invariato;
- una lettura piu credibile delle fasce di plinto da armare.

La portata ammissibile di progetto del palo singolo, nel caso trattato qui, e calcolata dal software come:

$$
Q_{amm} = \eta_g \frac{Q_{ult}}{\gamma_s}
$$

dove:

- $Q_{ult}$ e la capacita ultima del palo singolo in $\mathrm{kN}$;
- $\eta_g$ e il coefficiente di efficienza del gruppo;
- $\gamma_s$ e il fattore globale di sicurezza.

Il controllo finale sul palo $i$ e leggibile con:

$$
FS_i = \frac{Q_{amm}}{R_i}
$$

Se $FS_i$ si porta vicino a $1.50$ in statico, il modello deformabile smette di essere un lusso e diventa un controllo necessario.

**2. Esempio numerico realistico**

Il caso seguente e lo stesso benchmark pubblico `test/caso_base.json` del repository `DomenicoGaudioso/PlintoPali`, eseguito localmente nell'app Streamlit. E' un caso semplice ma professionale: gruppo 2x2, plinto quadrato, azione eccentrica e stratigrafia mista sabbia-argilla.

Dati geometrici e meccanici:

- plinto $B = L = 5.0\,\mathrm{m}$;
- spessore plinto $t = 1.0\,\mathrm{m}$;
- modulo elastico del calcestruzzo $E_c = 30000\,\mathrm{MPa}$;
- numero pali $n_x = n_y = 2$;
- interassi $s_x = s_y = 3.0\,\mathrm{m}$;
- diametro palo $D = 0.60\,\mathrm{m}$;
- lunghezza palo $L = 12.0\,\mathrm{m}$;
- fattori geotecnici: $N_q = 35$, $N_c = 9$, $\beta = 0.35$, $\alpha = 0.70$, $\gamma_s = 2.50$;
- azioni di progetto: $N = 1200\,\mathrm{kN}$, $M_x = 120\,\mathrm{kNm}$, $M_y = 80\,\mathrm{kNm}$;
- coefficienti pseudo-statici: $k_h = 0.06$, $k_v = 0.00$;
- falda a $100\,\mathrm{m}$, quindi fuori dal volume efficace del problema.

Stratigrafia di input:

- strato 1, spessore $2.0\,\mathrm{m}$, $\gamma_{dry} = 18\,\mathrm{kN/m^3}$, $\phi = 30^\circ$, $c_u = 0$, $E = 25000\,\mathrm{kPa}$;
- strato 2, spessore $3.0\,\mathrm{m}$, $\gamma_{dry} = 19\,\mathrm{kN/m^3}$, $\phi = 34^\circ$, $c_u = 0$, $E = 40000\,\mathrm{kPa}$;
- strato 3, spessore $5.0\,\mathrm{m}$, $\gamma_{dry} = 20\,\mathrm{kN/m^3}$, $\phi = 0^\circ$, $c_u = 120\,\mathrm{kPa}$, $E = 60000\,\mathrm{kPa}$.

La schermata seguente mostra il caso impostato nell'app e la sintesi numerica principale:

![Input del benchmark 2x2 in PlintoPali con geometria, azioni eccentriche e sintesi delle verifiche]({{ site.baseurl }}/assets/images/plinto-rigido-fem-graticcio-plintopali-input-sintesi.png)

**3. Capacita del palo singolo**

Per il palo trivellato:

$$
A_b = \frac{\pi D^2}{4} = \frac{\pi \cdot 0.60^2}{4} = 0.2827\,\mathrm{m^2}
$$

$$
u = \pi D = \pi \cdot 0.60 = 1.885\,\mathrm{m}
$$

La punta lavora nello strato coesivo di base, quindi il contributo di punta viene letto con:

$$
Q_b = N_c c_u A_b = 9 \cdot 120 \cdot 0.2827 = 305.4\,\mathrm{kN}
$$

Il fusto viene invece calcolato per strati. Nei terreni granulari l'app usa:

$$
q_s = \beta \sigma'_v
$$

mentre nello strato coesivo adotta:

$$
q_s = \alpha c_u
$$

Per il primo strato sabbioso, al punto medio $z = 1.0\,\mathrm{m}$:

$$
\sigma'_{v,1} = 18 \cdot 1.0 = 18.0\,\mathrm{kPa}
$$

$$
Q_{s,1} = 0.35 \cdot 18.0 \cdot 1.885 \cdot 2.0 = 23.8\,\mathrm{kN}
$$

Per il secondo strato, al punto medio $z = 3.5\,\mathrm{m}$:

$$
\sigma'_{v,2} = 18 \cdot 2.0 + 19 \cdot 1.5 = 64.5\,\mathrm{kPa}
$$

$$
Q_{s,2} = 0.35 \cdot 64.5 \cdot 1.885 \cdot 3.0 = 127.7\,\mathrm{kN}
$$

Per lo strato coesivo finale:

$$
Q_{s,3} = 0.70 \cdot 120 \cdot 1.885 \cdot 5.0 = 791.6\,\mathrm{kN}
$$

Quindi:

$$
Q_s = Q_{s,1} + Q_{s,2} + Q_{s,3} = 943.1\,\mathrm{kN}
$$

$$
Q_{ult} = Q_b + Q_s = 305.4 + 943.1 = 1248.5\,\mathrm{kN}
$$

Con interasse $s = 3.0\,\mathrm{m}$ e diametro $D = 0.60\,\mathrm{m}$ si ha $s/D = 5.0$, quindi il benchmark e letto dal software come gruppo senza penalizzazione di efficienza:

$$
\eta_g = 1.00
$$

La portata ammissibile effettiva del palo vale quindi:

$$
Q_{amm} = \eta_g \frac{Q_{ult}}{\gamma_s} = 1.00 \cdot \frac{1248.5}{2.50} = 499.4\,\mathrm{kN}
$$

La verifica dimensionale torna: le tensioni geotecniche sono inserite in $\mathrm{kPa}$, la superficie e il perimetro in $\mathrm{m^2}$ e $\mathrm{m}$, quindi i contributi risultano correttamente in $\mathrm{kN}$.

**4. Ripartizione rigida delle reazioni**

Le coordinate dei pali rispetto al baricentro sono:

$$
(x_i, y_i) = (\pm 1.5, \pm 1.5)\,\mathrm{m}
$$

per cui:

$$
\sum x_i^2 = \sum y_i^2 = 4 \cdot 1.5^2 = 9.0\,\mathrm{m^2}
$$

Il contributo uniforme e:

$$
\frac{N}{n} = \frac{1200}{4} = 300.0\,\mathrm{kN}
$$

Sul palo P4 posto in $(+1.5,+1.5)$:

$$
R_4 = 300 + \frac{120 \cdot 1.5}{9} + \frac{80 \cdot 1.5}{9}
= 300 + 20.0 + 13.3 = 333.3\,\mathrm{kN}
$$

Sul palo P1 posto in $(-1.5,-1.5)$:

$$
R_1 = 300 - 20.0 - 13.3 = 266.7\,\mathrm{kN}
$$

Le quattro reazioni statiche del modello rigido sono:

| Palo | Coordinate $(x,y)$ [m] | $R_{rig}$ [kN] | $FS = Q_{amm}/R_{rig}$ [-] |
| --- | --- | ---: | ---: |
| P1 | $(-1.5,-1.5)$ | 266.7 | 1.87 |
| P2 | $(+1.5,-1.5)$ | 293.3 | 1.70 |
| P3 | $(-1.5,+1.5)$ | 306.7 | 1.63 |
| P4 | $(+1.5,+1.5)$ | 333.3 | 1.50 |

Il controllo governante in statico e quindi:

$$
FS_{min,stat} = \frac{499.4}{333.3} = 1.50
$$

In pseudo-statica, con $k_h = 0.06$, l'app porta la reazione massima a $335.3\,\mathrm{kN}$ e il fattore minimo a:

$$
FS_{min,sis} = \frac{499.4}{335.3} = 1.49
$$

Il caso resta verificato, ma si colloca esattamente nella fascia in cui un professionista prudente non si ferma al solo schema rigido.

**5. Cosa mostra davvero il FEM a graticcio**

La schermata seguente e importante perche visualizza insieme il quadro delle reazioni e il modello geometrico usato dal software:

![Confronto pali e modello 3D del benchmark 2x2 in PlintoPali, con planimetria delle reazioni rigide e plinto su quattro pali]({{ site.baseurl }}/assets/images/plinto-rigido-fem-graticcio-plintopali-geometria-3d.png)

Nel benchmark eseguito in `PlintoPali` il modello FEM usa una rigidezza assiale equivalente per palo pari a:

$$
k_v = 44070\,\mathrm{kN/m}
$$

Il risultato piu interessante non e un aumento del massimo assoluto, che resta sostanzialmente pari a $333.3\,\mathrm{kN}$ in statico, ma il fatto che il palo governante cambia. Nel confronto tabellare dell'app:

- il plinto rigido governa sul palo P4 con $R = 333.3\,\mathrm{kN}$;
- il FEM governa sul palo P2 con $R = 333.3\,\mathrm{kN}$;
- i cedimenti FEM alle teste pali variano da $6.05$ a $7.56\,\mathrm{mm}$;
- il cedimento differenziale massimo tra i pali e circa $1.51\,\mathrm{mm}$.

Questa e una lettura progettuale molto concreta: in un caso regolare il valore massimo puo anche restare quasi invariato, ma la fascia di plinto che assorbe l'effetto locale cambia. Se il dettaglio delle armature superiori e inferiori e stato pensato solo sul palo "intuitivamente" piu gravoso, il FEM puo spostare il punto da presidiare.

**6. Risultati statici, sismici e confronto tra casi**

L'output reale dell'app, ottenuto sullo stesso caso numerico, rende immediato il confronto tra statico e pseudo-statico:

![Output reale di PlintoPali con mesh del graticcio FEM, profilo stratigrafico, bolle di reazione statiche e sismiche e confronto a barre dei pali]({{ site.baseurl }}/assets/images/plinto-rigido-fem-graticcio-plintopali-risultati-statico-sismico.png)

Da questa lettura si ricavano quattro messaggi tecnici utili:

1. Il gruppo non entra in trazione ne in statico ne in pseudo-statica.
2. L'efficienza di gruppo non riduce la portata, perche il caso ha $s/D = 5.0$.
3. Il sisma pseudo-statico non cambia il meccanismo, ma sposta leggermente il palo piu caricato da $333$ a $335\,\mathrm{kN}$.
4. La differenza rigido/FEM non va letta solo sul massimo numerico, ma sulla combinazione tra reazione locale e cedimento.

Per questo motivo il confronto da riportare in relazione non dovrebbe fermarsi a una sola riga "OK/NON OK". La tabella minima utile e:

| Modello | $R_{max}$ [kN] | Palo governante | $FS_{min}$ [-] | Nota tecnica |
| --- | ---: | --- | ---: | --- |
| Rigido statico | 333.3 | P4 | 1.50 | predimensionamento vicino alla soglia |
| FEM statico | 333.3 | P2 | 1.50 | cambia il palo governante |
| Rigido pseudo-statico | 335.3 | P4 | 1.49 | incremento lieve ma non nullo |

Il caso e interessante proprio per questo: non "esplode", ma si colloca nella zona in cui il progettista non puo piu accontentarsi del solo Navier.

**7. Verifiche finali e warnings automatici**

La parte finale dell'app e quella che piu somiglia alla lettura da relazione:

![Tabella verifiche, esiti e note automatiche di PlintoPali sul benchmark 2x2 con soglia statica prossima a FS 1.50]({{ site.baseurl }}/assets/images/plinto-rigido-fem-graticcio-plintopali-verifiche-warning.png)

I controlli restituiti dal software sul caso numerico sono:

- $FS_{min,stat} = 1.50$ con esito `ATTENZIONE`;
- $FS_{min,sis} = 1.49$ con esito `VERIFICATO` rispetto al limite configurato per il caso sismico;
- nessuna trazione nei pali;
- nessun superamento di $Q_{amm}$ ne con modello rigido ne con FEM.

Le note automatiche sono coerenti con la lettura manuale:

- il plinto e segnalato come relativamente sottile rispetto all'interasse;
- il palo piu caricato in ipotesi rigida e il n. 4 con $333\,\mathrm{kN}$;
- la portata laterale di fusto governa sulla punta, quindi la stratigrafia non e un dato accessorio ma il cuore del problema.

Questo e il punto in cui `PlintoPali` e utile in modo discreto ma concreto: non sostituisce il giudizio del progettista, ma mette nello stesso pannello il dato che serve a decidere se basta la modellazione rigida o se conviene chiudere la pratica con il graticcio FEM e una nota esplicita in relazione.

**8. Come usare PlintoPali sullo stesso caso**

Per rifare il caso numerico nell'app:

1. Inserire in `Geometria plinto` i valori $B = 5.0\,\mathrm{m}$, $L = 5.0\,\mathrm{m}$, spessore $1.0\,\mathrm{m}$ ed $E_c = 30000\,\mathrm{MPa}$.
2. Impostare in `Pali` una maglia $2 \times 2$ con interassi $3.0\,\mathrm{m}$, diametro $0.60\,\mathrm{m}$ e lunghezza $12.0\,\mathrm{m}$.
3. Inserire in `Capacita geotecnica` i coefficienti $N_q = 35$, $N_c = 9$, $\beta = 0.35$, $\alpha = 0.70$ e fattore di sicurezza $2.50$.
4. Compilare in `Azioni` i valori $N = 1200\,\mathrm{kN}$, $M_x = 120\,\mathrm{kNm}$, $M_y = 80\,\mathrm{kNm}$, $k_h = 0.06$, $k_v = 0.00$, falda $100\,\mathrm{m}$.
5. Incollare in `Stratigrafia` le tre righe CSV del benchmark:

```text
2.0,18,20,30,0,25000
3.0,19,21,34,0,40000
5.0,20,20,0,120,60000
```

6. Leggere prima `Risultati principali` e `Sintesi` per controllare $Q_{amm}$, $FS_{min}$ e cedimento di gruppo.
7. Passare a `Confronto pali`, `Geometria in pianta`, `Mesh FEM Graticcio` e ai plot di reazione per capire se il palo governante cambia tra rigido e deformabile.
8. Chiudere con `Verifiche`, verificando che i messaggi automatici siano coerenti con la lettura tecnica che si vuole riportare in relazione.

L'ultima schermata utile, spesso trascurata, e la parte di export:

![Sidebar finale di PlintoPali con stratigrafia del benchmark e pulsanti di export JSON e relazione Word]({{ site.baseurl }}/assets/images/plinto-rigido-fem-graticcio-plintopali-export-report.png)

I due export da usare davvero sono:

- `Salva input JSON`, per congelare il caso di calcolo e poterlo riaprire senza errori di trascrizione;
- `Scarica relazione Word`, da usare solo dopo avere verificato che il palo governante, il rapporto domanda/capacita e le note automatiche siano coerenti con il modello scelto.

**9. Quando il plinto rigido non basta piu**

Su casi come questo il modello rigido e ancora molto utile, ma a una condizione: deve restare un primo livello di controllo, non l'unico. Se gia in un gruppo 2x2 regolare il FEM sposta il palo governante pur lasciando quasi invariato $R_{max}$, allora su layout irregolari o su casi con momento dominante la sola tabella Navier rischia di essere troppo ottimistica.

Nel repository `PlintoPali` c'e anche il benchmark `gruppo_pali_irregolare_trazione.json`, pensato proprio per questo salto di complessita: layout personalizzato, palo in trazione e portata ammissibile superata. E' il proseguimento naturale del caso qui sopra quando si vuole capire quando attivare il layout tabellare/irregolare e quando il graticcio FEM non e piu facoltativo.

Riferimenti normativi e bibliografici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme tecniche per le costruzioni*, Ministero delle Infrastrutture e dei Trasporti, G.U. n. 42 del 20 febbraio 2018, S.O. n. 8.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle Norme tecniche per le costruzioni di cui al decreto ministeriale 17 gennaio 2018*, G.U. n. 35 del 11 febbraio 2019, S.O. n. 5.
- UNI EN 1992-1-1:2015, Eurocodice 2 - Progettazione delle strutture di calcestruzzo - Parte 1-1: Regole generali e regole per gli edifici.
- UNI EN 1997-1:2013, Eurocodice 7 - Progettazione geotecnica - Parte 1: Regole generali.
- Poulos, H. G.; Davis, E. H., *Pile Foundation Analysis and Design*, John Wiley & Sons, 1980.
