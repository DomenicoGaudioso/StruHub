---
layout: post
title: "Ripartizione trasversale dei carichi negli impalcati da ponte"
description: "Metodo tecnico completo con piastra ortotropa equivalente, caso numerico su due corsie e workflow operativo con GuyonMassonnetBares."
date: 2026-01-14
order: 14
permalink: /posts/ripartizione-trasversale-carichi-impalcati.html
meta: "Quaderno tecnico - ripartizione trasversale con caso numerico e workflow GuyonMassonnetBares"
---

**La ripartizione trasversale e il punto in cui il traffico smette di essere un carico globale e diventa domanda sulla singola trave**

Nel calcolo di un impalcato da ponte il modello longitudinale fornisce il momento globale, il taglio globale e l'inviluppo lungo la luce. Questo non basta ancora per verificare una trave principale. Serve capire quanta parte di quell'effetto entra davvero nella trave di bordo, in quella centrale e nella trave piu vicina alla corsia caricata.

Qui il collegamento piu naturale con gli applicativi di Domenico Gaudioso e il repository `GuyonMassonnetBares`, che implementa la lettura semianalitica Guyon-Massonnet-Bares con piastra ortotropa equivalente, sviluppo dei carichi in serie di Fourier e output per momento, taglio e deformata delle travi principali. Nel seguito non viene usato come vetrina: viene usato come controllo ripetibile dello stesso caso numerico riportato nel post.

Le immagini inserite nel testo sono output reali generati dal repository `GuyonMassonnetBares` sul caso qui descritto. Il caso e stato costruito in due varianti, cosi da far vedere una cosa che in pratica conta molto piu del formalismo: con la stessa sezione trasversale basta spostare la corsia per cambiare in modo netto la trave governante.

**1. Teoria e metodo**

La ripartizione trasversale non e una semplice divisione per il numero di travi. Se il modello longitudinale restituisce un effetto globale $E_{tot}(x)$, per esempio un momento in campata o un taglio in prossimita dell'appoggio, il primo passaggio e solo contabile:

$$
E_{medio}(x)=\frac{E_{tot}(x)}{n}
$$

dove:

- $E_{tot}(x)$ e l'effetto globale dell'impalcato, in $\mathrm{kN m}$ oppure $\mathrm{kN}$;
- $n$ e il numero delle travi principali;
- $E_{medio}(x)$ e il valore medio per trave, utile solo come base di confronto.

La quota sulla trave $j$ si scrive poi come:

$$
E_j(x)=\eta_j(x,e)\,E_{medio}(x)
$$

dove $\eta_j$ dipende da:

- posizione trasversale del carico $e$;
- rigidezza flessionale delle travi longitudinali;
- rigidezza trasversale efficace di soletta e traversi;
- rigidezza torsionale dell'impalcato;
- larghezza efficace e sbalzi laterali.

Nel metodo Guyon-Massonnet-Bares l'impalcato reale viene assimilato a una piastra ortotropa equivalente di luce $L$ e semi-larghezza $b$. Le rigidezze equivalenti principali sono:

$$
\rho_P=\frac{E I_l}{s}
$$

$$
\rho_E=\frac{E I_t}{s_t}
$$

dove:

- $E$ e il modulo elastico, in $\mathrm{Pa}$;
- $I_l$ e l'inerzia longitudinale equivalente della singola trave, in $\mathrm{m^4}$;
- $I_t$ e l'inerzia trasversale equivalente, in $\mathrm{m^4}$;
- $s$ e l'interasse tra travi longitudinali, in $\mathrm{m}$;
- $s_t$ e il passo caratteristico della rigidezza trasversale, in $\mathrm{m}$.

I due parametri che governano la diffusione trasversale nel repository sono:

$$
\theta=\frac{b}{L}\sqrt[4]{\frac{\rho_P}{\rho_E}}
$$

$$
\alpha=\frac{\gamma_P+\gamma_E}{2\sqrt{\rho_P\rho_E}}
$$

con $\gamma_P$ e $\gamma_E$ associate alle rigidezze torsionali longitudinale e trasversale. In lettura professionale:

- $\theta$ governa il rapporto tra rigidezza longitudinale e trasversale;
- $\alpha$ misura quanto la torsione aiuta a redistribuire il carico.

Il repository non applica direttamente una "corsia equivalente" sulla singola trave. Costruisce prima il carico longitudinale come serie di Fourier:

$$
p(x)=\sum_{m=1}^{N} A_m \sin\left(\frac{m\pi x}{L}\right)
$$

Per un asse concentrato $P_r$ applicato alla posizione $x_r$:

$$
A_m^{(r)}=\frac{2P_r}{L}\sin\left(\frac{m\pi x_r}{L}\right)
$$

Per una fascia uniformemente caricata di intensita $q$ e larghezza efficace $b_{eff}$:

$$
p_{line}=q\,b_{eff}
$$

Il solver calcola quindi per ogni trave un peso trasversale $W_j$ e restituisce:

$$
M_j(x)=W_j\,M_{medio}(x)
$$

$$
V_j(x)=W_j\,V_{medio}(x)
$$

Per riportare in tabella una quota percentuale leggibile conviene normalizzare:

$$
w_j=\frac{W_j}{\sum_i W_i}
$$

cosi che:

$$
\sum_j w_j = 1
$$

Questa normalizzazione e utile in relazione per confrontare casi diversi, mentre i grafici di momento e taglio restano quelli restituiti dal solver.

**2. Caso numerico reale con due posizioni di corsia**

Si considera lo stesso impalcato per due casi di carico, uno centrato e uno eccentrico, cosi da separare bene il controllo di simmetria dal controllo di bordo. I dati geometrici e meccanici sono quelli usati nel repository `GuyonMassonnetBares`:

| Parametro | Valore |
| --- | ---: |
| Luce $L$ | $22.30\,\mathrm{m}$ |
| Larghezza totale $L_y$ | $11.50\,\mathrm{m}$ |
| Semi-larghezza $b=L_y/2$ | $5.75\,\mathrm{m}$ |
| Numero travi $n$ | $11$ |
| Interasse travi $s$ | $1.00\,\mathrm{m}$ |
| Modulo elastico $E$ | $32.308\,\mathrm{GPa}$ |
| Inerzia longitudinale $I_l$ | $0.11375171\,\mathrm{m^4}$ |
| Inerzia torsionale longitudinale $J_l$ | $0.00510383\,\mathrm{m^4}$ |
| Inerzia trasversale $I_t$ | $0.10382507\,\mathrm{m^4}$ |
| Inerzia torsionale trasversale $J_{lt}$ | $0.01228304\,\mathrm{m^4}$ |
| Termini di Fourier $N$ | $67$ |

Nel repository il passo trasversale equivalente e assunto pari a:

$$
s_t=\frac{L}{2}=11.15\,\mathrm{m}
$$

Da qui si ricavano:

$$
\rho_P=\frac{32.308\cdot10^9 \cdot 0.11375171}{1.00}=3.675\cdot10^9\,\mathrm{N\,m}
$$

$$
\rho_E=\frac{32.308\cdot10^9 \cdot 0.10382507}{11.15}=3.008\cdot10^8\,\mathrm{N\,m}
$$

$$
\theta=0.482
$$

$$
\alpha=0.0397
$$

Il valore di $\theta$ segnala un impalcato molto piu rigido lungo la luce che in direzione trasversale. Il valore basso di $\alpha$ dice che la torsione aiuta la diffusione, ma non al punto da annullare l'effetto dell'eccentricita.

I due casi di carico studiati sono:

1. `LC-C corsia centrata`: fascia da traffico uniforme $q=9.0\,\mathrm{kN/m^2}$ su larghezza $3.0\,\mathrm{m}$ centrata in asse, piu due assi da $320\,\mathrm{kN}$ a $x=10.60\,\mathrm{m}$ e $x=11.80\,\mathrm{m}$, entrambi a $y=0$.
2. `LC-E corsia eccentrica`: stessa fascia da traffico e stesso tandem spostati a $y=+2.8\,\mathrm{m}$, con in piu un carico distribuito residuo $q=2.5\,\mathrm{kN/m^2}$ su fascia larga $1.5\,\mathrm{m}$ a $y=-3.2\,\mathrm{m}$, attivo tra $x=3.0\,\mathrm{m}$ e $x=19.5\,\mathrm{m}$.

![Schema reale dell'impalcato equivalente e dei casi LC-C centrato e LC-E eccentrico generato dalle funzioni del repository GuyonMassonnetBares]({{ site.baseurl }}/assets/images/ripartizione-trasversale-carichi-impalcati-gmb-casi-input.png)

Per il caso eccentrico il carico totale vale:

$$
P_{tot}=q_1 b_1 L + q_2 b_2 (x_2-x_1) + 2P
$$

$$
P_{tot}=9.0\cdot3.0\cdot22.30 + 2.5\cdot1.5\cdot16.5 + 2\cdot320 = 1303.98\,\mathrm{kN}
$$

La risultante trasversale cade a:

$$
e=\frac{\sum_i P_i y_i}{P_{tot}}=2.52\,\mathrm{m}
$$

quindi circa a meta tra l'asse dell'impalcato e il bordo caricato. E' gia un valore sufficiente per aspettarsi una crescita netta della domanda sulle travi del lato positivo.

**3. Risultati numerici e controlli**

Il primo controllo utile e quello di confronto tra quote normalizzate sulle travi. Per il caso centrato la distribuzione deve risultare simmetrica; per il caso eccentrico deve spostarsi verso le travi prossime alla corsia senza perdere del tutto la collaborazione del resto dell'impalcato.

![Confronto tra le quote normalizzate w_j delle 11 travi nei casi di corsia centrata e corsia eccentrica, ricavate dal solver GM]({{ site.baseurl }}/assets/images/ripartizione-trasversale-carichi-impalcati-gmb-quote-ripartizione.png)

Le quote normalizzate mostrano subito la differenza meccanica:

- nel caso `LC-C` la trave d'asse G6 prende l'11.35% della quota totale, mentre i due bordi G1 e G11 stanno al 6.43%;
- nel caso `LC-E` la trave G9, piu vicina alla corsia, sale al 12.20%;
- il bordo caricato G11 sale al 10.62%;
- il bordo opposto G1 scende al 4.00%.

Tradotto in termini pratici: spostando la corsia dalla mezzeria verso $y=+2.8\,\mathrm{m}$, la quota della trave G9 cresce rispetto al caso centrato di circa:

$$
\frac{12.20-9.01}{9.01}=35.4\%
$$

mentre la quota della trave G1 si riduce di:

$$
\frac{6.43-4.00}{6.43}=37.8\%
$$

Il secondo controllo e sulle grandezze di progetto, non solo sulle percentuali. La tabella seguente riassume travi e valori governanti:

![Tabella tecnica con quote di ripartizione, momenti massimi e deformata massima nei due casi LC-C e LC-E]({{ site.baseurl }}/assets/images/ripartizione-trasversale-carichi-impalcati-gmb-tabella-confronto.png)

La lettura ingegneristica e immediata:

- con corsia centrata il massimo momento e sulla trave G6, con $M_{max}=459.8\,\mathrm{kN\,m}$;
- con corsia eccentrica la trave governante diventa G9, con $M_{max}=466.0\,\mathrm{kN\,m}$;
- la stessa eccitazione fa salire la trave di bordo G11 da $260.6$ a $405.8\,\mathrm{kN\,m}$;
- il rapporto tra trave caricata G9 e bordo opposto G1 nel caso eccentrico vale:

$$
\frac{466.0}{152.9}=3.05
$$

Questa e la ragione per cui la media per trave e quasi sempre fuorviante: il massimo globale cambia poco, ma la trave che governa cambia molto. La deformata massima della piastra passa infatti solo da $63.7$ a $65.1\,\mathrm{mm}$, cioe circa il $2.2\%$ in piu, mentre la distribuzione locale delle sollecitazioni cambia in modo molto piu marcato.

Sul lato dei momenti lungo la luce, il caso eccentrico restituisce questo quadro:

![Diagrammi reali del momento flettente M(x) sulle travi G1, G6, G9 e G11 per il caso eccentrico LC-E]({{ site.baseurl }}/assets/images/ripartizione-trasversale-carichi-impalcati-gmb-momenti-travi.png)

Si vede bene che:

- G9 e la trave governante in quasi tutta la zona centrale;
- G11 resta molto sollecitata per effetto della vicinanza al carico e della torsione dell'impalcato;
- G6, pur non essendo direttamente caricata, resta su livelli elevati per effetto della diffusione trasversale;
- G1 riceve ancora una quota non trascurabile, segno che l'impalcato non si comporta come un insieme di travi isolate.

Lo stesso ragionamento va fatto sul taglio, perche un metodo serio non si ferma alla tabella dei momenti:

![Diagrammi reali del taglio V(x) sulle travi G1, G6, G9 e G11 per il caso eccentrico LC-E]({{ site.baseurl }}/assets/images/ripartizione-trasversale-carichi-impalcati-gmb-tagli-travi.png)

Nel caso LC-E i massimi di taglio seguono la stessa gerarchia dei momenti. Questo e importante per due motivi:

1. conferma che la posizione trasversale della corsia sposta davvero la domanda sulla coppia di travi del lato caricato;
2. evita l'errore frequente di usare un coefficiente valido per il momento e trasferirlo automaticamente anche al taglio senza controllo.

L'ultima lettura utile e la superficie deformata:

![Superficie deformata reale della piastra ortotropa equivalente nel caso eccentrico LC-E, con massimo spostamento in zona centrale e lato caricato]({{ site.baseurl }}/assets/images/ripartizione-trasversale-carichi-impalcati-gmb-deformata-3d.png)

La deformata non e qui una verifica di esercizio autonoma. Serve a controllare che la forma della risposta sia coerente con:

- carico centrato in campata;
- eccentricita positiva del tandem e della fascia caricata;
- maggiore coinvolgimento del lato destro dell'impalcato.

In altre parole, la deformata e un controllo di consistenza del modello, non solo un grafico da allegare.

**4. Come usare GuyonMassonnetBares sullo stesso caso**

Per rifare il caso nell'app `GuyonMassonnetBares` conviene seguire questa sequenza operativa.

1. Avviare l'app con `streamlit run streamlit_app.py`.
2. Nella sidebar impostare il modello: `Lx = 22.30 m`, `Ly = 11.50 m`, `n_travi = 11`, `s = 1.00 m`.
3. Inserire le proprieta di materiale e sezione: `E = 32.308e9 Pa`, `nu = 0.20`, `Il = 0.11375171 m^4`, `Jl = 0.00510383 m^4`, `It = 0.10382507 m^4`, `Jlt = 0.01228304 m^4`.
4. Lasciare una discretizzazione coerente con il benchmark: `nx = 1450`, `ny = 60`, `n_iter = 67`.
5. Controllare subito i parametri adimensionali: se non si leggono circa `theta = 0.482` e `alpha = 0.0397`, c'e un errore negli input di rigidezza o di geometria.
6. Nel tab `Casi di carico` creare `LC-C` con una fascia `udl_full` a `y = 0.0 m`, `width = 3.0 m`, `q = 9000 N/m^2`, e due carichi `point` da `320000 N` a `x = 10.60 m` e `x = 11.80 m`.
7. Duplicare il caso e costruire `LC-E`: spostare fascia e tandem a `y = +2.8 m`, poi aggiungere un secondo `udl_partial` a `y = -3.2 m`, `width = 1.5 m`, `q = 2500 N/m^2`, attivo tra `x1 = 3.0 m` e `x2 = 19.5 m`.
8. Nel tab `Risultati` leggere prima `M(x)` e `V(x)` delle travi G1, G6, G9 e G11, non il solo massimo globale.
9. Controllare che `LC-C` sia simmetrico tra travi opposte e che in `LC-E` la trave G9 superi G6, mentre G11 cresca in modo sensibile rispetto al caso centrato.
10. Esportare il PDF solo dopo avere verificato coerenza tra geometria del carico, gerarchia delle travi e forma della deformata.

Questa e la parte in cui il richiamo a `GuyonMassonnetBares` diventa davvero utile dentro StruHub: non come pagina promozionale, ma come prova pratica che il passaggio da carico globale a domanda locale puo essere rifatto, controllato e documentato.

**5. Cosa portare davvero in relazione**

Per una relazione di progetto o verifica conviene archiviare almeno questi elementi:

- geometria trasversale dell'impalcato e coordinate delle travi;
- valori di $\theta$ e $\alpha$ con nota sulle rigidezze equivalenti adottate;
- posizione trasversale della corsia o del tandem che governa;
- confronto tra almeno un caso centrato e uno eccentrico;
- diagrammi di momento e taglio della trave governante e di una trave di bordo;
- verifica che la forma deformata sia coerente con la posizione del carico.

Se manca uno di questi passaggi, il coefficiente di ripartizione resta un numero poco revisionabile. Se invece restano traccia di input, curva e trave governante, la verifica diventa difendibile anche a distanza di tempo.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, *Aggiornamento delle "Norme tecniche per le costruzioni"*, con modifiche introdotte dal D.M. 9 marzo 2023.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, istruzioni applicative delle NTC 2018.
- UNI EN 1991-2:2005, *Eurocodice 1 - Azioni sulle strutture - Parte 2: Carichi da traffico sui ponti*.
- Hambly, E. C., *Bridge Deck Behaviour*, 2nd edition.
- Guyon, Massonnet e Bares, riferimenti classici sulla ripartizione trasversale degli impalcati assimilati a piastra ortotropa equivalente.
- Implementazione applicativa nel repository `GuyonMassonnetBares`, usato qui come benchmark reale del caso numerico riportato nel post.
