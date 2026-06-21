---
layout: post
title: "Calcolo della capacita portante assiale di pali singoli"
description: "Riferimento tecnico StruHub con metodo di calcolo, esempio numerico completo e workflow verificabile nell'app Palo."
date: 2026-01-05
order: 5
permalink: /posts/capacita-portante-assiale-pali.html
meta: "Quaderno tecnico - palo singolo, portanza assiale e controllo con Palo"
---

**La portanza assiale di un palo non si giudica con una formula isolata**

Nel progetto reale di un palo singolo la domanda non e soltanto "quanto vale $Q_{ult}$". Le domande corrette sono altre: quale parte della resistenza viene dal fusto e quale dalla punta, come entra la stratigrafia nel calcolo, quale parametro domina il risultato e se il margine resta leggibile quando si passa dalla stima manuale al controllo software.

Per questo conviene tenere separati quattro oggetti:

1. il profilo geotecnico di calcolo;
2. la costruzione delle tensioni efficaci;
3. la scomposizione della resistenza in attrito laterale e resistenza di punta;
4. il controllo finale domanda/capacita.

Nel seguito il caso viene riprodotto con il repository `Palo`, eseguito localmente come app Streamlit. Non e usato come pagina promozionale, ma come verifica pratica dello stesso caso numerico riportato nel testo.

![Dashboard iniziale dell'app Palo con sintesi assiale, combinazione governante e input importati del caso numerico]({{ site.baseurl }}/assets/images/capacita-portante-assiale-pali-palo-input-sintesi.png)

**1. Teoria e metodo**

La capacita assiale ultima di un palo trivellato o infisso nasce dalla somma di due meccanismi:

$$
Q_{ult} = Q_s + Q_b
$$

dove:

- $Q_s$ e la resistenza laterale mobilitata lungo il fusto, in $\mathrm{kN}$;
- $Q_b$ e la resistenza di punta alla base, in $\mathrm{kN}$.

Il primo passaggio non e il calcolo delle resistenze, ma la costruzione del modello geotecnico. Senza una stratigrafia chiara il numero finale puo sembrare ordinato ma resta poco difendibile. Per un palo assiale servono almeno:

- quota e spessore di ogni strato;
- peso di volume naturale o asciutto $\gamma$ in $\mathrm{kN/m^3}$;
- peso di volume saturo $\gamma_{sat}$ in $\mathrm{kN/m^3}$, se la falda e presente;
- angolo di attrito $\phi'$ per i terreni granulari;
- resistenza non drenata $c_u$ per i terreni coesivi;
- quota di falda, per costruire $\sigma'_v(z)$.

La tensione verticale efficace e il filo conduttore del problema:

$$
\sigma'_v(z) = \sigma_v(z) - u(z)
$$

dove $\sigma_v$ e la tensione totale e $u$ la pressione neutra. Se la falda non intercetta il palo, allora $u(z)=0$ nel tratto considerato e la tensione efficace coincide con l'integrale dei pesi di volume asciutti.

Una volta definita $\sigma'_v(z)$, la resistenza laterale si valuta per tratti:

$$
Q_s = \sum_i q_{s,i} \, P \, L_i
$$

dove:

- $q_{s,i}$ e la tensione tangenziale media nello strato $i$, in $\mathrm{kPa}$;
- $P = \pi D$ e il perimetro del palo, in $\mathrm{m}$;
- $L_i$ e il tratto di palo immerso nello strato $i$, in $\mathrm{m}$.

Nel caso di terreni granulari il modello operativo usato in `Palo` e:

$$
q_{s,i} = \beta \, \sigma'_{v,m,i}
$$

con $\beta$ coefficiente adimensionale e $\sigma'_{v,m,i}$ tensione efficace media nello strato.

Nel caso di terreni coesivi il modello operativo e:

$$
q_{s,i} = \alpha \, c_{u,i}
$$

con $\alpha$ coefficiente di aderenza e $c_u$ in $\mathrm{kPa}$.

La resistenza di punta dipende invece dal terreno alla quota di base. Se la punta e in materiale granulare:

$$
Q_b = N_q \, \sigma'_v(L) \, A_b
$$

Se la punta e in materiale coesivo:

$$
Q_b = N_c \, c_{u,b} \, A_b
$$

con area di base:

$$
A_b = \frac{\pi D^2}{4}
$$

Infine, il controllo di sintesi puo essere scritto come:

$$
FS = \frac{Q_{ult}}{N_d}
$$

dove $N_d$ e il carico assiale di progetto. Nell'app `Palo`, per il caso pseudo-statico mostrato qui, il carico verticale viene ridotto come:

$$
N_{d,sis} = N \, (1-k_v)
$$

e il fattore di sicurezza sismico viene letto rispetto a quel valore. E' una logica di controllo interna al tool, utile per il benchmark del caso, ma non sostituisce il quadro completo dei coefficienti parziali normativi.

**2. Esempio numerico realistico**

Si consideri un palo trivellato in c.a. con:

- diametro $D = 0.80\,\mathrm{m}$;
- lunghezza $L = 20.0\,\mathrm{m}$;
- carico assiale statico $N = 1500\,\mathrm{kN}$;
- azione laterale di servizio $H = 120\,\mathrm{kN}$;
- coefficienti pseudo-statici $k_h = 0.15$ e $k_v = 0.05$;
- parametri di calcolo $N_q = 25$, $N_c = 9$, $\beta = 0.30$, $\alpha = 0.70$.

La stratigrafia del caso, importata nell'app e usata per tutte le schermate del post, e la seguente:

| Strato | Profondita | Terreno | Parametri principali |
| --- | --- | --- | --- |
| 1 | $0-2\,\mathrm{m}$ | sabbia | $\gamma = 18\,\mathrm{kN/m^3}$, $\phi' = 30^\circ$ |
| 2 | $2-5\,\mathrm{m}$ | sabbia addensata | $\gamma = 19\,\mathrm{kN/m^3}$, $\phi' = 34^\circ$ |
| 3 | $5-20\,\mathrm{m}$ | argilla consistente | $\gamma = 20\,\mathrm{kN/m^3}$, $c_u = 120\,\mathrm{kPa}$ |

La falda e posta a $99\,\mathrm{m}$ dal piano campagna, quindi non intercetta il palo e non riduce le tensioni efficaci del caso.

Le grandezze geometriche di base sono:

$$
A_b = \frac{\pi (0.80)^2}{4} = 0.503\ \mathrm{m^2}
$$

$$
P = \pi \cdot 0.80 = 2.513\ \mathrm{m}
$$

Le tensioni efficaci medie negli strati risultano:

- strato 1: $\sigma'_{v,m,1} = 18\,\mathrm{kPa}$;
- strato 2: $\sigma'_{v,m,2} = 64.5\,\mathrm{kPa}$;
- strato 3: $\sigma'_{v,m,3} = 243\,\mathrm{kPa}$.

Nel tratto argilloso il modello dell'app usa $q_s = \alpha c_u$, quindi la tensione efficace media del terzo strato non entra direttamente nel termine di attrito laterale, ma resta comunque utile per il controllo geotecnico generale del profilo.

**Calcolo dell'attrito laterale**

Per il primo strato sabbioso:

$$
q_{s,1} = \beta \sigma'_{v,m,1} = 0.30 \cdot 18 = 5.4\ \mathrm{kPa}
$$

$$
Q_{s,1} = 5.4 \cdot 2.513 \cdot 2 = 27.1\ \mathrm{kN}
$$

Per il secondo strato sabbioso:

$$
q_{s,2} = 0.30 \cdot 64.5 = 19.35\ \mathrm{kPa}
$$

$$
Q_{s,2} = 19.35 \cdot 2.513 \cdot 3 = 145.9\ \mathrm{kN}
$$

Per il tratto argilloso:

$$
q_{s,3} = \alpha c_u = 0.70 \cdot 120 = 84.0\ \mathrm{kPa}
$$

$$
Q_{s,3} = 84.0 \cdot 2.513 \cdot 15 = 3166.7\ \mathrm{kN}
$$

Quindi:

$$
Q_s = Q_{s,1} + Q_{s,2} + Q_{s,3} = 27.1 + 145.9 + 3166.7 = 3339.7\ \mathrm{kN}
$$

**Calcolo della resistenza di punta**

La punta del palo ricade nello strato coesivo, quindi:

$$
Q_b = N_c \, c_{u,b} \, A_b = 9 \cdot 120 \cdot 0.503 = 542.9\ \mathrm{kN}
$$

La resistenza ultima totale e:

$$
Q_{ult} = Q_s + Q_b = 3339.7 + 542.9 = 3882.6\ \mathrm{kN}
$$

**Controllo statico e pseudo-statico**

Per il caso statico:

$$
FS_{stat} = \frac{3882.6}{1500} = 2.59
$$

Per il caso pseudo-statico verticale:

$$
N_{d,sis} = 1500 \cdot (1-0.05) = 1425\ \mathrm{kN}
$$

$$
FS_{sis} = \frac{3882.6}{1425} = 2.72
$$

Il caso e quindi verificato nel controllo assiale interno dell'app sia in statico sia in pseudo-statico. Il punto tecnico interessante e un altro: la maggior parte della capacita non arriva dalla punta ma dal fusto, con rapporto:

$$
\frac{Q_s}{Q_{ult}} = \frac{3339.7}{3882.6} = 0.86
$$

cioe circa l'86% della resistenza totale.

![Schema geometrico del palo e tabelle dell'app Palo con Qult, domanda/capacita e contributi Qb-Qs del caso coerente fino a 20 m]({{ site.baseurl }}/assets/images/capacita-portante-assiale-pali-palo-contributi-assiali.png)

Questa schermata e utile per una ragione precisa: consente di controllare nello stesso punto geometria, risultato globale e scomposizione meccanica del risultato. Se un professionista vede solo $Q_{ult}$ senza $Q_b$ e $Q_s$, perde la lettura del meccanismo dominante.

**3. Che cosa controllare oltre al numero finale**

Un palo assialmente verificato non e automaticamente un palo "chiuso". Restano almeno quattro verifiche di buon senso:

1. coerenza tra lunghezza del palo e spessore cumulato degli strati;
2. sensibilita della portanza alla quota di falda;
3. peso relativo di punta e fusto;
4. presenza di richieste laterali o sismiche che non cambiano $Q_{ult}$ ma cambiano spostamenti e momenti.

Nel caso qui riportato il controllo laterale non governa la portanza assiale, ma e comunque istruttivo perche mostra l'effetto dell'azione pseudo-statica orizzontale:

- in statico l'app restituisce $|y_{testa}| = 4.50\,\mathrm{mm}$ e $M_{max,FD} = 160.1\,\mathrm{kNm}$;
- in pseudo-statico il carico orizzontale di testa sale a $H_{sis} = H + k_h N = 120 + 0.15 \cdot 1500 = 345\,\mathrm{kN}$;
- lo spostamento di testa cresce a $12.94\,\mathrm{mm}$;
- il momento massimo cresce a $460.3\,\mathrm{kNm}$.

![Grafici dell'app Palo con spostamenti e momenti lungo il palo in statico e pseudo-statico]({{ site.baseurl }}/assets/images/capacita-portante-assiale-pali-palo-risposta-laterale.png)

Per un articolo sulla portanza assiale, questa figura non serve a cambiare il tema del post. Serve a ricordare una cosa molto pratica: un palo puo essere tranquillo in compressione e allo stesso tempo richiedere attenzione per la risposta laterale o per il dettaglio strutturale del fusto.

L'app espone anche il confronto tra differenze finite e FEM Hermitiano per la risposta laterale. Nel caso in esame:

- spostamento di testa statico: $4.50\,\mathrm{mm}$ con FD contro $3.44\,\mathrm{mm}$ con FEM;
- momento massimo statico: $160.1\,\mathrm{kNm}$ con FD contro $136.3\,\mathrm{kNm}$ con FEM;
- momento massimo pseudo-statico: $460.3\,\mathrm{kNm}$ con FD contro $391.9\,\mathrm{kNm}$ con FEM.

![Confronto nell'app Palo tra solver a differenze finite e solver FEM Hermitiano per spostamento, momento e taglio]({{ site.baseurl }}/assets/images/capacita-portante-assiale-pali-palo-confronto-fem.png)

Questo confronto non sostituisce una validazione esterna, ma e molto utile come controllo interno di robustezza numerica. Se due solver costruiti con ipotesi vicine divergono troppo, il progettista ha un segnale da leggere prima di portare il numero in relazione.

**4. Procedura operativa con l'app Palo**

Per rifare il caso numerico nell'app `Palo` conviene seguire questa sequenza:

1. impostare geometria e rigidezza: `D = 0.80 m`, `L = 20.0 m`, `E = 30000000 kPa`, `I = 0.02 m^4`, `40` elementi;
2. inserire i parametri assiali: `Nq = 25`, `Nc = 9`, `beta = 0.30`, `alpha = 0.70`, `N = 1500 kN`, `H = 120 kN`;
3. impostare l'azione pseudo-statica: `kh = 0.15`, `kv = 0.05`;
4. lasciare la falda a `99 m` per mantenere il caso asciutto;
5. caricare la stratigrafia come CSV:

```text
2.0,18,20,30,0,25000
3.0,19,21,34,0,40000
15.0,20,20,0,120,60000
```

6. leggere nella tabella di sintesi `Qult`, `FS`, `Qb/Qs`, combinazione governante e rapporto domanda/capacita;
7. passare ai grafici laterali per verificare che l'incremento sismico di momento e spostamento non stia aprendo un problema diverso da quello assiale;
8. esportare la sintesi CSV e la relazione Word/PDF quando il caso e tecnicamente coerente.

La lettura corretta dei risultati e questa:

- `Qult = 3882.6 kN` e il dato meccanico globale;
- `Qb = 542.9 kN` e `Qs = 3339.8 kN` spiegano da dove arriva il risultato;
- `FS_static = 2.59` e `FS_sismico = 2.72` dicono che il caso supera i riferimenti interni del software;
- `y_{testa}` e `M_{max}` servono a non perdere il lato deformativo e strutturale del problema.

![Sezione finale dell'app Palo con export Word/PDF, controlli di esito e warning del caso pseudo-statico]({{ site.baseurl }}/assets/images/capacita-portante-assiale-pali-palo-report-controlli.png)

Il valore pratico di `Palo`, in ottica StruHub/CivilBox, non e "dare il numero al posto del progettista". E' rendere verificabile il percorso: input, stratigrafia, tensioni efficaci, contributi $Q_b/Q_s$, controlli, grafici e report finale.

**5. Cosa deve comparire in una relazione seria**

Quando questo tipo di calcolo entra in una relazione geotecnica o di fondazione, conviene rendere espliciti almeno questi punti:

1. origine dei parametri geotecnici e distinzione tra valori caratteristici e di progetto;
2. quota della falda e criterio con cui sono state costruite le tensioni efficaci;
3. formula usata per attrito laterale nei terreni granulari e coesivi;
4. criterio scelto per la resistenza di punta;
5. scomposizione numerica di $Q_s$ e $Q_b$;
6. eventuale verifica complementare di spostamento o di risposta laterale.

Se mancano questi passaggi, anche un valore finale formalmente corretto resta debole in revisione.

**Riferimenti tecnici**

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme tecniche per le costruzioni*, con particolare riferimento al Capitolo 6 e al paragrafo 6.4.3 sulle fondazioni su pali.
- Circolare C.S.LL.PP. 21 gennaio 2019 n. 7, *Istruzioni per l'applicazione dell'Aggiornamento delle Norme tecniche per le costruzioni*, con riferimento ai chiarimenti applicativi sulle verifiche geotecniche e strutturali delle fondazioni profonde.
- UNI EN 1997-1:2013, *Eurocodice 7 - Progettazione geotecnica - Parte 1: Regole generali*.
- Metodi operativi di tipo $\alpha$-$\beta$ per il calcolo preliminare della portanza assiale e schema beam on Winkler per la risposta laterale, come implementati nel repository `Palo`.
