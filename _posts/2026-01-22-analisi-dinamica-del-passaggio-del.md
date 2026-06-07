---
layout: post
title: "Analisi dinamica del passaggio del treno su un ponte"
description: "Riferimento tecnico StruHub con schema di calcolo, caso numerico e riferimenti normativi ferroviari tracciabili."
date: 2026-01-22
order: 22
permalink: /posts/analisi-dinamica-del-passaggio-del.html
meta: "Quaderno tecnico - 1450 parole circa"
---

**Dal treno reale al modello di calcolo**

Quando un treno attraversa un ponte non applica un carico statico traslato lungo la luce, ma una sequenza di assi con interassi, masse sospese, velocita e contenuto in frequenza propri. Per chi progetta davvero, il punto non e dire genericamente che "c'e dinamica": il punto e capire quali velocita meritano una verifica piu fine, quale grandezza governa tra spostamento, accelerazione e sollecitazione, e quanto il risultato dipenda da massa, rigidezza e smorzamento.

![Ponte ferroviario soggetto a transito dinamico]({{ site.baseurl }}/assets/images/analisi-dinamica-del-passaggio-del-01.jpg)

La formulazione di base, per un impalcato discretizzato o continuo, resta:

$$
M\ddot{u}(t)+C\dot{u}(t)+Ku(t)=F(t)
$$

dove $F(t)$ varia per effetto della posizione degli assi:

$$
x_i(t)=x_{i,0}+vt
$$

Questa scrittura e nota, ma da sola non basta. In un controllo professionale bisogna archiviare almeno:

- schema resistente, luce e vincoli;
- massa lineare comprensiva di impalcato, ballast, binario e accessori permanenti;
- rigidezza effettiva della sezione nella fase resistente corretta;
- smorzamento adottato e motivazione della scelta;
- interassi caratteristici del convoglio usati per lo sweep di velocita;
- grandezza letta in uscita: spostamento, accelerazione, momento, taglio o reazione.

**Schema operativo**

L'ordine corretto non parte dalla time-history, ma dal controllo modale preliminare. Prima si stima la frequenza fondamentale, poi si individuano le velocita critiche associate agli interassi ricorrenti del convoglio, infine si raffina l'analisi attorno ai picchi.

![Schema del workflow per l'analisi dinamica del transito ferroviario]({{ site.baseurl }}/assets/images/analisi-dinamica-del-passaggio-del-schema.svg)

Per una trave semplicemente appoggiata di tipo Euler-Bernoulli, la prima frequenza propria puo essere stimata con:

$$
f_1=\frac{\pi}{2L^2}\sqrt{\frac{EI}{m}}
$$

dove:

- $L$ e la luce;
- $EI$ e la rigidezza flessionale equivalente nella direzione studiata;
- $m$ e la massa per unita di lunghezza.

Se un interasse caratteristico del convoglio vale $d$, la frequenza di eccitazione associata alla velocita $v$ e:

$$
f_p=\frac{v}{d}
$$

e la corrispondente velocita critica del modo fondamentale e:

$$
v_{cr,1}=f_1 d
$$

La relazione non sostituisce il modello dinamico, ma serve a non usarlo in modo cieco. Se una velocita commerciale cade vicino a $v_{cr,1}$, oppure vicino a una velocita critica del secondo o terzo modo, quella zona va campionata con passo piu fitto.

**Riduzione modale e risposta massima**

Per una lettura rapida del problema si puo scrivere:

$$
w(x,t)=\sum_{n=1}^{N}\phi_n(x)q_n(t)
$$

con forme modali, per la trave semplicemente appoggiata:

$$
\phi_n(x)=\sin\left(\frac{n\pi x}{L}\right)
$$

e frequenze circolari:

$$
\omega_n=\left(\frac{n\pi}{L}\right)^2\sqrt{\frac{EI}{m}}
$$

In termini di interpretazione pratica, il professionista non deve salvare solo "il massimo assoluto", ma anche la velocita che lo genera, l'istante di occorrenza e la sezione critica. In caso contrario, il risultato non e riproducibile.

Un indicatore utile per confrontare risposta statica e dinamica e:

$$
\eta_{dyn}=\frac{R_{dyn,max}}{R_{stat,max}}
$$

dove $R$ puo essere freccia, momento, taglio o accelerazione. La grandezza da confrontare va scelta in funzione della verifica reale, non della variabile piu facile da estrarre.

**Caso numerico realistico**

Si consideri un impalcato ferroviario semplicemente appoggiato con:

- $L=24.0\,\mathrm{m}$;
- $EI=8.2\cdot10^{10}\,\mathrm{N\,m^2}$;
- $m=26\,000\,\mathrm{kg/m}$;
- smorzamento viscoso equivalente $\xi=1.5\%$.

La stima preliminare della frequenza fondamentale e:

$$
f_1=\frac{\pi}{2\cdot24^2}\sqrt{\frac{8.2\cdot10^{10}}{26\,000}}=4.84\,\mathrm{Hz}
$$

quindi:

$$
T_1=\frac{1}{f_1}=0.206\,\mathrm{s}
$$

Questo valore e gia un primo filtro tecnico: se il modello FEM restituisse, ad esempio, $T_1=0.34\,\mathrm{s}$, la differenza non sarebbe un dettaglio, ma un segnale da indagare su masse, inerzie, vincoli o unita.

Assumiamo ora due distanze caratteristiche del convoglio:

- $d_1=3.0\,\mathrm{m}$, interasse tra assi dello stesso carrello;
- $d_2=18.0\,\mathrm{m}$, distanza ricorrente tra gruppi di carrelli.

Le velocita critiche teoriche del primo modo diventano:

$$
v_{cr,1}^{(d_1)}=4.84\cdot3.0=14.53\,\mathrm{m/s}=52.3\,\mathrm{km/h}
$$

$$
v_{cr,1}^{(d_2)}=4.84\cdot18.0=87.17\,\mathrm{m/s}=313.8\,\mathrm{km/h}
$$

Questo passaggio e molto utile in pratica. Significa che lo sweep di velocita non va distribuito in modo uniforme "da 0 a 350 km/h" senza criterio, ma addensato almeno attorno a due finestre:

- $45\div60\,\mathrm{km/h}$;
- $280\div320\,\mathrm{km/h}$.

Nel primo intervallo si intercetta una possibile eccitazione legata al passo corto degli assi. Nel secondo si controlla la compatibilita con le velocita tipiche dell'alta velocita. Per luci medie, proprio questa seconda fascia puo diventare progettualmente piu interessante.

**Come leggere i risultati dello sweep**

Supponiamo di eseguire uno sweep dinamico con lo stesso convoglio e di ottenere, nella sezione di mezzeria, i risultati seguenti per la combinazione piu gravosa di assi:

| Velocita | Freccia max dinamica | Accelerazione max | Momento max |
| --- | ---: | ---: | ---: |
| $120\,\mathrm{km/h}$ | $6.8\,\mathrm{mm}$ | $0.38\,\mathrm{m/s^2}$ | $1280\,\mathrm{kNm}$ |
| $220\,\mathrm{km/h}$ | $9.7\,\mathrm{mm}$ | $0.71\,\mathrm{m/s^2}$ | $1510\,\mathrm{kNm}$ |
| $300\,\mathrm{km/h}$ | $14.2\,\mathrm{mm}$ | $1.08\,\mathrm{m/s^2}$ | $1820\,\mathrm{kNm}$ |

La corrispondente analisi quasi-statica, con identica configurazione di assi nella posizione piu sfavorevole, restituisce:

- $w_{stat,max}=11.6\,\mathrm{mm}$;
- $M_{stat,max}=1540\,\mathrm{kNm}$.

I rapporti dinamici principali diventano:

$$
\eta_w=\frac{14.2}{11.6}=1.22
$$

$$
\eta_M=\frac{1820}{1540}=1.18
$$

La lettura utile non e dire soltanto che "l'amplificazione vale circa 1.2". La lettura utile e questa:

- il picco compare nella fascia attesa dalla stima di $v_{cr,1}^{(d_2)}$;
- la freccia e il momento non crescono nello stesso rapporto, quindi il parametro governante dipende dalla verifica;
- l'accelerazione verticale puo diventare la grandezza da tenere piu sotto controllo in esercizio, anche quando il margine resistente resta accettabile.

![Esempio di risposta dinamica del transito ferroviario]({{ site.baseurl }}/assets/images/analisi-dinamica-del-passaggio-del-02.jpg)

**Tre controlli che evitano errori frequenti**

1. **Controllo del periodo fondamentale.** Un valore FEM che non torna con la stima semplificata va spiegato prima di discutere qualsiasi picco dinamico.
2. **Controllo del passo temporale.** Una regola pratica robusta e imporre $v\Delta t \le l_e/10$, dove $l_e$ e la lunghezza caratteristica dell'elemento o del passo spaziale di integrazione del carico mobile.
3. **Controllo dei massimi concomitanti.** Se il massimo momento si verifica a un certo istante, le altre grandezze da usare nella verifica locale dovrebbero essere quelle dello stesso istante, non i massimi indipendenti presi da storie diverse.

Quest'ultimo punto e spesso il piu trascurato. Per una verifica a fatica, una verifica locale del dettaglio o una lettura combinata con reazioni agli appoggi, estrarre massimi non concomitanti puo produrre una domanda numericamente ordinata ma fisicamente impossibile.

**Cosa chiedere a un modello davvero utile**

Nel lavoro quotidiano, il post-processing ha valore solo se lascia una traccia tecnica. Una piccola routine nello spirito StruHub o una utility CivilBox dedicata all'archiviazione delle scansioni di velocita ha senso quando, per ogni run, salva:

- velocita analizzata e interassi considerati;
- frequenze proprie e masse partecipanti del modello;
- sezione critica e istante del picco;
- freccia, accelerazione, taglio e momento concomitanti;
- rapporto domanda/capacita della verifica che interessa davvero.

Se invece il software restituisce un solo semaforo finale, il progettista perde la possibilita di ricostruire perche il picco cade proprio a quella velocita e non a un'altra.

**Riferimenti normativi da citare in modo esplicito**

Per rendere il calcolo tracciabile, in un report o in un post tecnico conviene richiamare almeno questi riferimenti, in forma completa:

- D.M. 17 gennaio 2018, *Aggiornamento delle Norme Tecniche per le Costruzioni*, Capitolo 5, per il quadro nazionale sulle azioni e sulle verifiche dei ponti.
- Circolare C.S.LL.PP. 21 gennaio 2019 n. 7, per i chiarimenti applicativi alle NTC 2018.
- UNI EN 1991-2:2005, Eurocodice 1 - Azioni sulle strutture - Parte 2: Carichi da traffico sui ponti, con particolare attenzione alle prescrizioni e ai criteri di lettura per i ponti ferroviari e all'Allegato D per la dinamica del traffico ferroviario.
- UNI EN 1990:2006, Eurocodice - Criteri generali di progettazione strutturale, per l'impostazione delle verifiche e dei livelli prestazionali.

Quando si citano limiti di accelerazione, soglie di comfort o criteri particolari del gestore ferroviario, la fonte deve essere riportata per esteso nel report. E' uno dei punti in cui molti elaborati restano deboli: il numero compare, ma non e chiaro da quale documento provenga.

**Sintesi professionale**

L'analisi dinamica del transito ferroviario diventa utile quando collega quattro oggetti precisi: frequenza propria del ponte, periodicita del convoglio, fascia di velocita critica e grandezza strutturale governante. La formula iniziale serve solo ad aprire il problema. Il valore progettuale nasce invece dal controllo incrociato tra stima manuale, sweep numerico e riferimento normativo scritto in modo verificabile.
