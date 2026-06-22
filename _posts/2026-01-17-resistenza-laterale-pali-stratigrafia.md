---
layout: post
title: "Valutazione della resistenza laterale di pali in terreni stratificati"
description: "Metodo di calcolo, esempio numerico completo e workflow verificabile nell'app Palo per leggere spostamenti, momenti e controlli laterali."
date: 2026-01-17
order: 17
permalink: /posts/resistenza-laterale-pali-stratigrafia.html
meta: "Quaderno tecnico - palo singolo, risposta laterale e controllo con Palo"
---

**La risposta laterale di un palo non si chiude con un solo valore di carico orizzontale**

Quando un palo lavora con azioni orizzontali, il dato importante non e soltanto "quanti kN posso applicare in testa". Il problema vero e capire come la stratigrafia modifica la deformata, dove si forma il momento massimo, se la falda riduce la rigidezza efficace del terreno e quale combinazione diventa governante tra statico e pseudo-statico.

Per questo tema il collegamento piu naturale e il repository `Palo`, applicativo CivilBox per palo singolo con calcolo assiale, risposta laterale, confronto differenze finite/FEM ed export Word-PDF. Nel seguito non viene citato come vetrina commerciale: viene usato come controllo pratico dello stesso caso numerico riportato nel testo.

Le immagini inserite nel post sono output reali generati dal repo `Palo` sul benchmark `lateral_seismic_governing`, eseguito localmente.

![Output reale del repository Palo con input del benchmark, sintesi assiale-laterale e tabella domanda-capacita del caso sismico governante]({{ site.baseurl }}/assets/images/resistenza-laterale-pali-stratigrafia-palo-input-sintesi.png)

**1. Teoria e metodo**

Il modello base piu utile per un palo laterale in terreno stratificato e quello di trave su suolo elastico. Il palo viene trattato come una trave di rigidezza flessionale $EI$, mentre il terreno restituisce una reazione distribuita proporzionale allo spostamento locale:

$$
p(z) = k_h(z)\,y(z)
$$

dove:

- $p(z)$ e la reazione laterale del terreno per unita di lunghezza, in $\mathrm{kN/m}$;
- $k_h(z)$ e il coefficiente di reazione orizzontale, in $\mathrm{kN/m^3}$;
- $y(z)$ e lo spostamento laterale del palo, in $\mathrm{m}$;
- $z$ e la profondita misurata dal piano campagna, in $\mathrm{m}$.

Nel motore del repo `Palo` la discretizzazione beam on Winkler viene letta come:

$$
EI \, \frac{d^4 y}{dz^4} + k_h(z)\,D\,y(z) = 0
$$

dove $D$ e il diametro del palo, in $\mathrm{m}$. Il prodotto $k_h(z)D$ trasforma il coefficiente volumetrico di reazione del terreno in una rigidezza distribuita agente sul palo.

La stratigrafia entra quindi in modo diretto:

$$
k_h(z) =
\begin{cases}
k_{h,1} & 0 < z \le z_1 \\
k_{h,2} & z_1 < z \le z_2 \\
\dots \\
k_{h,n} & z_{n-1} < z \le z_n
\end{cases}
$$

Il massimo momento non e obbligato a stare in testa. In molti casi si forma pochi diametri sotto il piano campagna, proprio dove il terreno passa da zona piu deformabile a zona piu contrastata.

Le grandezze da controllare sono sempre almeno quattro:

$$
y(z), \qquad \theta(z)=\frac{dy}{dz}, \qquad M(z)=EI\frac{d^2y}{dz^2}, \qquad V(z)=\frac{dM}{dz}
$$

Nel caso di falda, il modello geotecnico non puo prescindere dalle tensioni efficaci:

$$
\sigma'_v(z) = \sigma_v(z) - u(z)
$$

$$
u(z) = \gamma_w \, (z-z_f) \qquad \text{per } z > z_f
$$

dove:

- $\sigma'_v(z)$ e la tensione verticale efficace, in $\mathrm{kPa}$;
- $\sigma_v(z)$ e la tensione totale, in $\mathrm{kPa}$;
- $u(z)$ e la pressione neutra, in $\mathrm{kPa}$;
- $\gamma_w = 9.81\,\mathrm{kN/m^3}$;
- $z_f$ e la profondita di falda, in $\mathrm{m}$.

Nel repo `Palo` la stessa stratigrafia viene riusata anche per i controlli assiali. Questo e utile perche costringe il progettista a non separare artificialmente il problema laterale da quello geotecnico complessivo.

Per una lettura rapida dell'ordine di grandezza della zona attiva, resta utile anche la lunghezza caratteristica di trasferimento:

$$
\ell_0 = \left(\frac{4EI}{k_h D}\right)^{1/4}
$$

Se $\ell_0$ cade nei primi metri di palo, e ragionevole aspettarsi che la curvatura massima si concentri nella parte alta della fondazione, dove si gioca la gran parte del dettaglio strutturale.

Nel caso pseudo-statico implementato nel software:

$$
H_{sis} = H + k_h^{(sis)} N
$$

$$
N_{sis} = N(1-k_v)
$$

dove:

- $H$ e il carico orizzontale statico di testa, in $\mathrm{kN}$;
- $k_h^{(sis)}$ e il coefficiente pseudo-statico orizzontale adimensionale;
- $N$ e il carico assiale applicato, in $\mathrm{kN}$;
- $k_v$ e il coefficiente pseudo-statico verticale.

Questa scrittura non sostituisce il quadro completo delle verifiche normative, ma e molto utile per capire se il sisma apre un problema laterale che non si legge nel solo caso statico.

**2. Esempio numerico realistico**

Il caso seguente coincide con il benchmark pubblico `lateral_seismic_governing.json` del repository `DomenicoGaudioso/Palo`.

Dati del palo:

- diametro $D = 0.70\,\mathrm{m}$;
- lunghezza $L = 18.0\,\mathrm{m}$;
- modulo elastico $E = 30000000\,\mathrm{kPa}$;
- momento di inerzia $I = 0.0118\,\mathrm{m^4}$;
- carico assiale $N = 1200\,\mathrm{kN}$;
- carico laterale statico $H = 180\,\mathrm{kN}$;
- coefficienti pseudo-statici $k_h^{(sis)} = 0.28$ e $k_v = 0.02$;
- parametri assiali di appoggio geotecnico: $N_q = 28$, $N_c = 9$, $\beta = 0.32$, $\alpha = 0.65$;
- discretizzazione numerica: $54$ elementi.

Stratigrafia di input:

| Strato | Profondita | Terreno | Parametri |
| --- | --- | --- | --- |
| 1 | $0-3\,\mathrm{m}$ | sabbia | $\gamma_{dry}=18$, $\gamma_{sat}=20\,\mathrm{kN/m^3}$, $\phi'=30^\circ$, $k_h=18000\,\mathrm{kN/m^3}$ |
| 2 | $3-8\,\mathrm{m}$ | sabbia addensata | $\gamma_{dry}=18.5$, $\gamma_{sat}=20.5\,\mathrm{kN/m^3}$, $\phi'=32^\circ$, $k_h=26000\,\mathrm{kN/m^3}$ |
| 3 | $8-18\,\mathrm{m}$ | argilla consistente | $\gamma_{dry}=19$, $\gamma_{sat}=21\,\mathrm{kN/m^3}$, $c_u=95\,\mathrm{kPa}$, $k_h=32000\,\mathrm{kN/m^3}$ |

La falda intercetta il palo a:

$$
z_f = 3.0\,\mathrm{m}
$$

La rigidezza flessionale del palo vale:

$$
EI = 30000000 \cdot 0.0118 = 354000\ \mathrm{kN\,m^2}
$$

Le lunghezze caratteristiche ottenute sui tre strati sono:

$$
\ell_{0,1} = \left(\frac{4 \cdot 354000}{18000 \cdot 0.70}\right)^{1/4} = 3.26\ \mathrm{m}
$$

$$
\ell_{0,2} = 2.97\ \mathrm{m}, \qquad \ell_{0,3} = 2.82\ \mathrm{m}
$$

Gia questo controllo dice molto: la zona in cui ci si aspetta la curvatura dominante cade esattamente nella fascia alta del palo, in prossimita della transizione tra primo e secondo strato.

Le tensioni efficaci che il software costruisce sullo stesso input sono coerenti con questa lettura:

- a $z = 3\,\mathrm{m}$: $\sigma'_v = 54.0\,\mathrm{kPa}$;
- a $z = 8\,\mathrm{m}$: $\sigma'_v = 107.45\,\mathrm{kPa}$;
- a $z = 18\,\mathrm{m}$: $\sigma'_v = 219.35\,\mathrm{kPa}$.

![Output reale del repository Palo con geometria del palo, stratigrafia, falda e tabella delle tensioni efficaci per strato]({{ site.baseurl }}/assets/images/resistenza-laterale-pali-stratigrafia-palo-stratigrafia-tensioni.png)

L'immagine precedente non e decorativa: e il controllo che mette insieme terreno, falda e geometria del modello. Senza questa coerenza, il grafico laterale finale e molto meno difendibile.

**3. Passaggi di calcolo e risultati**

Il primo passaggio da verificare e la costruzione della combinazione pseudo-statica:

$$
H_{sis} = 180 + 0.28 \cdot 1200 = 516\ \mathrm{kN}
$$

$$
N_{sis} = 1200 \cdot (1-0.02) = 1176\ \mathrm{kN}
$$

Il secondo passaggio utile e il controllo di consistenza geotecnica del profilo, che il repo `Palo` restituisce nello stesso caso numerico anche sul lato assiale. Con perimetro:

$$
P = \pi D = \pi \cdot 0.70 = 2.199\ \mathrm{m}
$$

e area di base:

$$
A_b = \frac{\pi D^2}{4} = 0.3848\ \mathrm{m^2}
$$

i contributi laterali di fusto risultano:

$$
Q_{s,1} = 57.0\ \mathrm{kN}, \qquad
Q_{s,2} = 284.0\ \mathrm{kN}, \qquad
Q_{s,3} = 1358.0\ \mathrm{kN}
$$

quindi:

$$
Q_s = 1699.0\ \mathrm{kN}
$$

La punta, in materiale coesivo, vale:

$$
Q_b = N_c c_u A_b = 9 \cdot 95 \cdot 0.3848 = 329.0\ \mathrm{kN}
$$

e la capacita ultima globale del palo e:

$$
Q_{ult} = Q_s + Q_b = 2028.0\ \mathrm{kN}
$$

Questo dato non governa il post, ma spiega perche l'app segnala un caso dominato dal comportamento laterale e non da una crisi assiale pura.

Sul lato laterale, i risultati del solver a differenze finite sono:

Per il caso statico:

$$
|y_{testa,stat}| = 10.57\ \mathrm{mm}
$$

$$
M_{max,stat} = 215.67\ \mathrm{kNm}
$$

Per il caso pseudo-statico:

$$
|y_{testa,sis}| = 30.30\ \mathrm{mm}
$$

$$
M_{max,sis} = 618.26\ \mathrm{kNm}
$$

In entrambi i casi il momento massimo si localizza a circa:

$$
z_{M,max} \approx 3.0\ \mathrm{m}
$$

cioe proprio nel tratto in cui la lunghezza caratteristica e dell'ordine di pochi metri e la falda inizia a ridurre le tensioni efficaci.

Anche il controllo sintetico domanda-capacita conferma che il problema vero e laterale:

- caso statico: $y/\!y_{lim} = 10.57/10.0 = 1.06$;
- caso sismico: $y/\!y_{lim} = 30.30/20.0 = 1.51$.

Quindi la combinazione governante letta dal software e:

$$
\text{governante} = \text{Sismico - laterale}
$$

![Output reale del repository Palo con deformata laterale e diagramma del momento flettente per i casi statico e sismico]({{ site.baseurl }}/assets/images/resistenza-laterale-pali-stratigrafia-palo-risposta-laterale.png)

La figura merita una lettura precisa:

1. lo spostamento di testa cresce di quasi tre volte passando da statico a pseudo-statico;
2. il momento massimo non cresce in modo proporzionale, ma cambia sensibilmente intensita e campo di influenza;
3. la deformata si richiude in profondita, segno che il contrasto laterale degli strati inferiori e efficace;
4. il massimo momento non si forma in punta, ma nella parte alta del palo, dove il dettaglio strutturale va davvero presidiato.

**4. Confronto tra differenze finite e FEM Hermitiano**

Uno degli aspetti piu utili del repository `Palo` e il confronto interno tra due letture numeriche dello stesso caso: differenze finite e FEM Euler-Bernoulli con funzioni di forma Hermitiane.

Sul benchmark:

- statico: $|y_{testa}| = 10.57\,\mathrm{mm}$ con FD e $8.65\,\mathrm{mm}$ con FEM;
- statico: $M_{max} = 215.67\,\mathrm{kNm}$ con FD e $192.07\,\mathrm{kNm}$ con FEM;
- sismico: $|y_{testa}| = 30.30\,\mathrm{mm}$ con FD e $24.80\,\mathrm{mm}$ con FEM;
- sismico: $M_{max} = 618.26\,\mathrm{kNm}$ con FD e $550.61\,\mathrm{kNm}$ con FEM.

![Output reale del repository Palo con confronto tra differenze finite e FEM Hermitiano su spostamento di testa e momento massimo]({{ site.baseurl }}/assets/images/resistenza-laterale-pali-stratigrafia-palo-confronto-fd-fem.png)

Questo confronto non va usato come gara tra solver. Va usato come verifica di robustezza:

- se i due risultati sono vicini, il modello e probabilmente ben condizionato;
- se divergono troppo, il primo indiziato non e il software ma la schematizzazione di $k_h(z)$, dei vincoli o della discretizzazione;
- se il caso sismico governa in entrambi i solver, la gerarchia progettuale e chiara anche se i numeri cambiano un poco.

Nel benchmark qui usato la differenza e leggibile ma non patologica. Il messaggio pratico e netto: il caso sismico resta governante con entrambi i modelli, quindi il progetto non puo fermarsi alla sola risposta statica.

**5. Come usare l'app Palo sullo stesso caso**

Per rifare il caso numerico nell'app `Palo` conviene seguire questa sequenza operativa.

1. Impostare geometria e rigidezza del palo:
   $D = 0.70\,\mathrm{m}$, $L = 18.0\,\mathrm{m}$, $E = 30000000\,\mathrm{kPa}$, $I = 0.0118\,\mathrm{m^4}$, $54$ elementi.
2. Inserire i parametri geotecnici e le azioni:
   $N_q = 28$, $N_c = 9$, $\beta = 0.32$, $\alpha = 0.65$, $N = 1200\,\mathrm{kN}$, $H = 180\,\mathrm{kN}$.
3. Impostare l'azione pseudo-statica:
   $k_h = 0.28$, $k_v = 0.02$.
4. Definire la falda a $3.0\,\mathrm{m}$.
5. Incollare la stratigrafia come CSV:

```text
3,18,20,30,0,18000
5,18.5,20.5,32,0,26000
10,19,21,0,95,32000
```

6. Leggere subito la sintesi iniziale:
   `Qult`, `FS`, `H testa`, `|Spostamento testa|`, `Mmax` e combinazione governante.
7. Passare alla tabella domanda-capacita:
   se il rapporto $y/\!y_{lim}$ supera $1.0$, il problema e gia chiaramente laterale anche con margine assiale accettabile.
8. Aprire la sezione stratigrafia e tensioni efficaci:
   serve a verificare che la quota di falda e i pesi immersi stiano davvero costruendo il profilo atteso.
9. Leggere i grafici di risposta laterale:
   controllare quota del momento massimo, spostamento di testa e differenza tra statico e sismico.
10. Aprire il confronto FD/FEM:
    se lo scarto e troppo ampio, vale la pena rivedere discretizzazione, valori di $k_h$ o attivare il modello con curve $p$-$y$.
11. Esportare Word e PDF solo dopo avere letto warning ed esiti:
    il report ha senso quando la sequenza input-modello-risultati-controlli e gia coerente.

![Output reale del repository Palo con esiti automatici, warning tecnici e conferma dell'export Word-PDF del caso benchmark]({{ site.baseurl }}/assets/images/resistenza-laterale-pali-stratigrafia-palo-report-controlli.png)

La schermata finale e quella che avvicina davvero il calcolo alla relazione. Nel benchmark l'app segnala:

- attenzione sullo spostamento statico, appena oltre la soglia interna di $10\,\mathrm{mm}$;
- attenzione sullo spostamento sismico, ben oltre la soglia interna di $20\,\mathrm{mm}$;
- falda nella meta superiore del palo, quindi tensioni efficaci ridotte sotto falda;
- incremento marcato del momento massimo nel caso sismico.

Questa e esattamente la ragione per cui `Palo` e un richiamo utile dentro StruHub: non per "fare il numero" al posto del progettista, ma per rendere tracciabile il percorso tra input, grafici, tabelle, warning ed export finale dello stesso caso numerico.

**6. Che cosa deve restare in relazione**

Se questo calcolo entra in una relazione geotecnica o strutturale seria, conviene rendere espliciti almeno questi punti:

1. stratigrafia adottata con quote, pesi di volume, parametri di resistenza e coefficienti $k_h$;
2. quota di falda e costruzione delle tensioni efficaci;
3. criterio con cui e stata costruita la combinazione pseudo-statica;
4. valore e quota del momento massimo;
5. spostamento di testa statico e sismico;
6. confronto sintetico tra solver o, almeno, controllo di robustezza della modellazione;
7. warning residui da presidiare nel dettaglio strutturale del palo.

Se manca questa catena, il grafico finale puo essere elegante ma resta poco difendibile in revisione.

**Riferimenti tecnici**

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme tecniche per le costruzioni*, con richiamo ai Capitoli 6 e 7.
- Circolare C.S.LL.PP. 21 gennaio 2019 n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle Norme tecniche per le costruzioni*.
- UNI EN 1997-1:2013, *Eurocodice 7 - Progettazione geotecnica - Parte 1: Regole generali*.
- Reese, L. C. e Matlock, H., riferimenti classici per la schematizzazione beam on Winkler e per l'estensione a curve $p$-$y$ nei terreni granulari e coesivi.
- Implementazione applicativa nel repository `Palo`, usato qui come benchmark reale del caso numerico riportato nel post.
