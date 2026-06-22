---
layout: post
title: "Valutazione della risposta in frequenza di impalcati soggetti a transito ferroviario"
description: "Riferimento tecnico StruHub con sweep di velocita, caso numerico reale OPSTrainLoad e lettura professionale dei picchi dinamici."
date: 2026-01-18
order: 18
permalink: /posts/risposta-frequenza-transito-ferroviario.html
meta: "Quaderno tecnico - risposta in frequenza ferroviaria e workflow OPSTrainLoad"
---

**La risposta in frequenza serve a scegliere dove guardare davvero**

Nel progetto di un ponte ferroviario non basta sapere che il treno e un carico mobile. Bisogna capire quali periodicita del convoglio entrano in risonanza con il ponte, in quale fascia di velocita, e se il parametro governante e lo spostamento, l'accelerazione o una sollecitazione locale. La risposta in frequenza non sostituisce la time-history, ma evita di analizzare "tutte le velocita allo stesso modo" e permette di concentrare il calcolo dove il ponte e piu sensibile.

Per questo il flusso corretto e:

1. stimare la frequenza fondamentale del ponte;
2. associare a ciascun interasse ricorrente del treno una velocita critica teorica;
3. eseguire uno sweep di velocita con passo coerente;
4. aprire le storie temporali nelle zone dove la curva mostra un picco vero.

Nel seguito il caso e collegato al repository `OPSTrainLoad`, che e l'applicativo locale piu pertinente per questo tema e consente di rifare davvero il caso numerico con output riproducibili.

**1. Teoria e metodo**

Per una trave semplicemente appoggiata di tipo Euler-Bernoulli, la prima frequenza propria si puo stimare con:

$$
f_1 = \frac{\pi}{2L^2}\sqrt{\frac{EI}{m}}
$$

dove:

- $L$ e la luce del ponte, in $\mathrm{m}$;
- $EI$ e la rigidezza flessionale equivalente, in $\mathrm{N\,m^2}$;
- $m$ e la massa lineare, in $\mathrm{kg/m}$.

La generica equazione del moto resta:

$$
M\ddot{u}(t) + C\dot{u}(t) + Ku(t) = F(t)
$$

ma per il treno il punto decisivo e il contenuto periodico di $F(t)$. Se un certo interasse caratteristico vale $d_j$, la frequenza di eccitazione associata alla velocita $v$ e:

$$
f_{p,j} = \frac{v}{d_j}
$$

con $v$ in $\mathrm{m/s}$ e $d_j$ in $\mathrm{m}$. La velocita critica teorica del primo modo diventa quindi:

$$
v_{cr,j} = f_1 d_j
$$

oppure, direttamente in $\mathrm{km/h}$:

$$
v_{cr,j,\mathrm{km/h}} = 3.6 f_1 d_j
$$

La risposta in frequenza non va letta come una formula isolata ma come una mappa preliminare. Se il ponte ha $f_1$ elevata e il treno presenta interassi ripetuti di $2.6\,\mathrm{m}$, $4.8\,\mathrm{m}$ o $12.4\,\mathrm{m}$, le tre fasce critiche possono stare molto lontane tra loro e richiedere densita diverse di campionamento.

Una grandezza operativa utile nello sweep e la risposta massima in funzione della velocita:

$$
R_{\max}(v) = \max_t |R(t;v)|
$$

dove $R$ puo essere spostamento verticale in mezzeria, accelerazione, momento o taglio. In `OPSTrainLoad` le grandezze monitorate in modo piu immediato sono spostamento e accelerazione in mezzeria.

**2. Caso numerico reale sul benchmark OPSTrainLoad**

Per evitare un esempio artificiale, il post usa il caso default del repository `DomenicoGaudioso/OPSTrainLoad`, eseguito localmente. I dati di input sono:

- luce del ponte $L = 33.25\,\mathrm{m}$;
- discretizzazione $l_e = 0.50\,\mathrm{m}$;
- area della sezione $A = 0.8573\,\mathrm{m^2}$;
- inerzia $I = 0.9529\,\mathrm{m^4}$;
- modulo elastico $E = 167000\,\mathrm{MPa}$;
- massa lineare $m = 6602\,\mathrm{kg/m}$;
- smorzamento viscoso equivalente $\xi = 1.0\%$;
- 7 assi da $170\,\mathrm{kN}$ con progressiva assi $0.0$, $4.8$, $7.4$, $19.8$, $22.4$, $27.2$, $29.8\,\mathrm{m}$.

Le periodicita che contano per la lettura in frequenza non sono le progressive assolute ma gli interassi ricorrenti:

- $d_1 = 2.6\,\mathrm{m}$;
- $d_2 = 4.8\,\mathrm{m}$;
- $d_3 = 12.4\,\mathrm{m}$.

Il report reale del caso mostra input, configurazione treno e riepilogo analitico:

![Riepilogo del report OPSTrainLoad con parametri del modello, tabella degli assi e frequenza fondamentale del caso benchmark]({{ site.baseurl }}/assets/images/risposta-frequenza-transito-ferroviario-opstrainload-input-report.png)

Con conversione coerente in unita SI:

$$
EI = 167000 \cdot 10^6 \cdot 0.9529 = 1.591 \cdot 10^{11}\,\mathrm{N\,m^2}
$$

e quindi:

$$
f_1 = \frac{\pi}{2 \cdot 33.25^2}\sqrt{\frac{1.591 \cdot 10^{11}}{6602}} = 6.98\,\mathrm{Hz}
$$

Le velocita critiche teoriche del primo modo associate ai tre interassi ricorrenti diventano:

$$
v_{cr,2.6} = 3.6 \cdot 6.98 \cdot 2.6 = 65.3\,\mathrm{km/h}
$$

$$
v_{cr,4.8} = 3.6 \cdot 6.98 \cdot 4.8 = 120.5\,\mathrm{km/h}
$$

$$
v_{cr,12.4} = 3.6 \cdot 6.98 \cdot 12.4 = 311.4\,\mathrm{km/h}
$$

Queste tre velocita spiegano gia la forma attesa della curva: un primo rigonfiamento attorno a $60$-$70\,\mathrm{km/h}$, un secondo attorno a $120\,\mathrm{km/h}$ e una fascia alta attorno a $300\,\mathrm{km/h}$. Nel report automatico del repo compare anche una velocita caratteristica $v_{cr} = 2Lf_1 \approx 1670\,\mathrm{km/h}$; e utile come dato interno del modello, ma per lo sweep progettuale contano le velocita $v_{cr,j} = f_1 d_j$ legate agli interassi del convoglio.

Prima del lancio del solver, il repository permette di controllare anche lo schema FEM e la posizione degli assi:

![Schema FEM prodotto dal repo OPSTrainLoad con trave semplicemente appoggiata, vincoli e posizione dei sette assi da 170 kN]({{ site.baseurl }}/assets/images/risposta-frequenza-transito-ferroviario-opstrainload-modello-fem.png)

Questo passaggio non e decorativo. Se luce, vincoli o disposizione degli assi non sono coerenti, il picco dinamico successivo puo essere numericamente stabile ma ingegneristicamente inutile.

**3. Sweep di velocita, formule di lettura e risultati**

Nel run reale e stato impostato l'intervallo `20-350 km/h` con passo `10 km/h`. Nell'implementazione corrente dell'applicazione la sequenza effettivamente analizzata e `20, 30, ..., 340 km/h`, cioe il limite superiore non viene incluso. Questa e una nota pratica utile quando si confrontano tabella di input e risultati esportati.

Per lo spostamento, il report del repository usa come indice di confronto interno:

$$
I_w(v) = \frac{w_{\max}(v)}{w_{\max}(20)}
$$

dove $w_{\max}(20)$ e il massimo spostamento del primo run a $20\,\mathrm{km/h}$. Non e un DAF statico in senso stretto, ma un indice molto utile per leggere rapidamente l'amplificazione relativa lungo lo sweep.

I risultati piu significativi del caso sono:

| Velocita | $a_{\max}$ [m/s$^2$] | $w_{\max}$ [mm] | $I_w(v)$ |
| --- | ---: | ---: | ---: |
| $20\,\mathrm{km/h}$ | 0.0392 | 3.1102 | 1.000 |
| $60\,\mathrm{km/h}$ | 0.2385 | 3.1697 | 1.019 |
| $120\,\mathrm{km/h}$ | 0.2994 | 3.2069 | 1.031 |
| $270\,\mathrm{km/h}$ | 0.9860 | 3.3503 | 1.077 |
| $280\,\mathrm{km/h}$ | 1.0076 | 3.3380 | 1.073 |
| $310\,\mathrm{km/h}$ | 0.7745 | 3.2733 | 1.052 |

La curva completa prodotta dal motore del repo e la seguente:

![Curva reale accelerazione-velocita e indice di amplificazione dello spostamento generati dal solver OPSTrainLoad sul benchmark default]({{ site.baseurl }}/assets/images/risposta-frequenza-transito-ferroviario-opstrainload-curva-velocita-daf.png)

La lettura tecnica corretta e questa:

- la velocita teorica $65.3\,\mathrm{km/h}$ genera un primo aumento della risposta, ma non governa;
- la velocita teorica $120.5\,\mathrm{km/h}$ produce un secondo rigonfiamento, anch'esso non governante;
- la fascia alta prevista attorno a $311\,\mathrm{km/h}$ e quella davvero sensibile, ma il massimo di spostamento si sposta a $270\,\mathrm{km/h}$ e quello di accelerazione a $280\,\mathrm{km/h}$;
- il fatto che il picco reale non coincida esattamente con la stima $f_1 d_j$ e normale: entrano in gioco piu periodicita del treno, la discretizzazione temporale e la vibrazione libera dopo l'uscita dell'ultimo asse.

La tabella estratta dal report conferma bene il plateau di alta sensibilita tra $250$ e $310\,\mathrm{km/h}$:

![Estratto della tabella risultati OPSTrainLoad nella fascia 220-340 km/h, con massimi di spostamento a 270 km/h e di accelerazione a 280 km/h]({{ site.baseurl }}/assets/images/risposta-frequenza-transito-ferroviario-opstrainload-tabella-picchi.png)

Per un professionista questo e il punto chiave: la risposta in frequenza non serve a dire "la velocita critica e 311 km/h" e fermarsi li. Serve a decidere che la fascia da densificare e circa $250$-$310\,\mathrm{km/h}$, con controlli piu fini sui due picchi distinti di spostamento e accelerazione.

**4. Storie temporali e controlli sul caso governante**

Nel caso di massimo spostamento, il solver restituisce:

- $w_{\max} = 3.350\,\mathrm{mm}$ a $270\,\mathrm{km/h}$;
- $a_{\max} = 0.986\,\mathrm{m/s^2}$ alla stessa velocita.

La storia temporale corrispondente e:

![Storia temporale reale del run OPSTrainLoad a 270 km/h con accelerazione, velocita e spostamento in mezzeria del ponte]({{ site.baseurl }}/assets/images/risposta-frequenza-transito-ferroviario-opstrainload-time-history-picco-spostamento.png)

Nel caso di massimo di accelerazione, invece:

- $a_{\max} = 1.008\,\mathrm{m/s^2}$ a $280\,\mathrm{km/h}$;
- $w_{\max} = 3.338\,\mathrm{mm}$ alla stessa velocita.

La storia temporale del run a $280\,\mathrm{km/h}$ mostra in modo pulito il decadimento della vibrazione libera dopo il passaggio del treno:

![Storia temporale reale del run OPSTrainLoad a 280 km/h con picco di accelerazione e coda in vibrazione libera post-transito]({{ site.baseurl }}/assets/images/risposta-frequenza-transito-ferroviario-opstrainload-time-history-picco-acc.png)

Queste due figure chiariscono due controlli che in pratica evitano molti errori:

1. il massimo puo verificarsi poco dopo l'uscita dell'ultimo asse, non solo mentre il treno e interamente in campata;
2. lo spostamento governante e l'accelerazione governante non sono obbligati a cadere alla stessa velocita.

Se il progetto richiede un controllo di comfort, di accelerazione verticale o di interazione con la via, il run a $280\,\mathrm{km/h}$ e piu importante del run a $270\,\mathrm{km/h}$. Se il controllo riguarda soprattutto la deformazione o una grandezza correlata allo spostamento, il run da approfondire e invece quello a $270\,\mathrm{km/h}$.

**5. Procedura operativa finale con OPSTrainLoad**

Per rifare esattamente il caso numerico del post nel software:

1. avviare l'app con `streamlit run runApp.py` dal repository `OPSTrainLoad`;
2. inserire `L = 33.25 m`, `l_segment = 0.50 m`, `A = 0.8573 m^2`, `I = 0.9529 m^4`, `E = 167000 MPa`, `m = 6602 kg/m`, `xi = 0.01`;
3. lasciare il convoglio default a 7 assi da `170 kN` con interassi `0, 4.8, 2.6, 12.4, 2.6, 4.8, 2.6 m`;
4. impostare lo sweep `v_min = 20 km/h`, `v_max = 350 km/h`, `step = 10 km/h`;
5. verificare nello schema FEM che i vincoli siano cerniera a sinistra e carrello a destra e che gli assi coprano davvero la luce;
6. eseguire `Run` e leggere subito tre uscite: `f1_fem`, curva accelerazione-velocita e tabella dei massimi;
7. aprire le velocita attorno a `270-280 km/h` e controllare le storie temporali complete, non solo il valore massimo;
8. esportare il report Word dall'interfaccia e, se serve una relazione tecnica piu completa, usare le funzioni del repo per generare anche il PDF del run batch.

Nel caso di questo post la sequenza visiva e stata ottenuta con output reali dello stesso repository: solver eseguito localmente, grafici generati dalle funzioni del repo e pagine del report renderizzate in PNG. Questo rende il collegamento con StruHub e CivilBox utile come verifica pratica del metodo, non come richiamo promozionale.

**6. Cosa archiviare davvero in relazione**

Quando la risposta in frequenza entra in una relazione di ponte ferroviario, conviene archiviare almeno:

- formula e valore di $f_1$ con unita coerenti;
- interassi ricorrenti del convoglio e corrispondenti velocita $v_{cr,j}$;
- sweep realmente eseguito, con indicazione esplicita del passo;
- velocita governante per spostamento e velocita governante per accelerazione;
- storie temporali dei casi governanti;
- criterio con cui si passa dal picco dinamico alla verifica progettuale successiva.

E' qui che un applicativo come `OPSTrainLoad` diventa utile nel flusso StruHub/CivilBox: non per sostituire il giudizio del progettista, ma per lasciare una traccia verificabile tra teoria, caso numerico, curva di sweep e report finale.

**Riferimenti tecnici e normativi**

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme Tecniche per le Costruzioni*, con riferimento al Capitolo 5 per ponti e azioni da traffico.
- Circolare C.S.LL.PP. 21 gennaio 2019 n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle Norme Tecniche per le Costruzioni*.
- UNI EN 1991-2:2005, *Eurocodice 1 - Azioni sulle strutture - Parte 2: Carichi da traffico sui ponti*, con riferimento alla dinamica del traffico ferroviario e ai criteri di valutazione della risposta.
- UNI EN 1990:2006, *Eurocodice - Criteri generali di progettazione strutturale*, per l'impostazione coerente delle verifiche e dei livelli prestazionali.
