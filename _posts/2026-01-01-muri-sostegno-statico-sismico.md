---
layout: post
title: "Analisi di muri di sostegno a mensola in campo statico e sismico"
description: "Metodo di calcolo, benchmark pseudo-statico completo e workflow reale con Muro per leggere spinte, verifiche GEO/EQU e stabilita del pendio."
date: 2026-01-01
order: 1
permalink: /posts/muri-sostegno-statico-sismico.html
meta: "Quaderno tecnico - muro a mensola, pseudo-statica e controllo con Muro"
---

Per questo tema il collegamento tecnico corretto e il repository `Muro`, applicativo CivilBox dedicato ai muri di sostegno a mensola con verifiche GEO/EQU, spinta pseudo-statica, pressione di contatto, portanza e stabilita globale del pendio. Nel seguito non e citato come vetrina: viene usato come controllo ripetibile dello stesso caso numerico riportato nel post.

![Schermata reale dell'app Muro con benchmark pseudo-statico, stratigrafia a due strati e campi principali per geometria, sovraccarico e coefficienti sismici]({{ site.baseurl }}/assets/images/muri-sostegno-statico-sismico-muro-input-overview.png)

Il caso di riferimento e il benchmark `test/benchmark/asdip_cantilever_surcharge_seismic.json` del repo `Muro`. E' interessante per un motivo pratico: il muro resta verificato nelle letture locali piu immediate, ma la combinazione sismica riduce il margine di portanza e la verifica globale del pendio resta critica. E' esattamente il tipo di situazione in cui un progettista non puo fermarsi al solo `FS` di scorrimento o ribaltamento.

## 1. Teoria e metodo di calcolo

Un muro a mensola va letto come un sistema opera-terreno, non come un semplice setto in c.a. Il percorso corretto e:

1. definire le pressioni laterali efficaci e idrauliche;
2. ricavare risultanti e bracci delle spinte;
3. costruire le azioni stabilizzanti del muro e del terreno sul tallone;
4. verificare ribaltamento, scorrimento e pressione di contatto;
5. controllare la portanza del piano di posa;
6. se il rilevato o il pendio di monte lo richiedono, chiudere il problema con una verifica di stabilita globale.

Nel caso statico, con attrito muro-terreno $\delta$ e terreno non saturo, il motore di `Muro` usa una formulazione di tipo Rankine/Coulomb semplificata per ricavare il coefficiente attivo:

$$
K_a = \frac{\cos^2 \phi}{\cos \delta \left(1 + \sqrt{\frac{\sin(\phi + \delta)\sin \phi}{\cos \delta}}\right)^2}
$$

dove:

- $\phi$ e l'angolo di attrito del terreno $[deg]$;
- $\delta$ e l'attrito terra-muro $[deg]$.

Nel caso pseudo-statico il coefficiente viene aggiornato con la forma di Mononobe-Okabe:

$$
\theta = \arctan \left(\frac{k_h}{1-k_v}\right)
$$

$$
K_{ae} =
\frac{\cos^2(\phi-\theta)}
{\cos\theta \cos(\delta+\theta)
\left[
1+\sqrt{
\frac{\sin(\phi+\delta)\sin(\phi-\theta)}
{\cos(\delta+\theta)}
}
\right]^2}
(1-k_v)
$$

con:

- $k_h$ coefficiente pseudo-statico orizzontale $[-]$;
- $k_v$ coefficiente pseudo-statico verticale $[-]$.

La pressione orizzontale totale a tergo vale quindi:

$$
\sigma_h(z) = K_{ae}(z)\left[\sigma_v'(z)+q\right]\cos\delta + u(z)
$$

dove:

- $\sigma_v'(z)$ e la tensione verticale efficace alla quota $z$ $[kPa]$;
- $q$ e il sovraccarico uniforme in sommita $[kPa]$;
- $u(z)$ e la pressione neutra $[kPa]$.

La risultante della spinta attiva si ottiene per integrazione:

$$
P_a = \int_0^H \sigma_h(z)\,dz
$$

e il suo braccio rispetto alla base si legge con:

$$
z_a = \frac{\int_0^H \sigma_h(z)\,z\,dz}{P_a}
$$

Per il fronte valle, se si considera il contributo passivo sul terreno davanti alla punta:

$$
P_p = \int_0^{h_f} \sigma_p(z)\,dz
$$

con $h_f$ altezza del terreno a fronte $[m]$.

Le verifiche principali sono poi:

$$
FS_{rib} = \frac{M_{stab}}{M_{rib}}
$$

$$
FS_{scorr} = \frac{\mu V + c_b B + P_p}{H_h}
$$

$$
q_{max,min} = \frac{V}{B}\left(1 \pm \frac{6e}{B}\right)
$$

$$
FS_{port} = \frac{q_{lim}}{q_{max}}
$$

con:

- $V$ risultante verticale al piano di posa $[kN/m]$;
- $H_h$ risultante orizzontale destabilizzante $[kN/m]$;
- $\mu$ coefficiente di attrito alla base $[-]$;
- $c_b$ coesione efficace di base $[kPa]$;
- $B$ larghezza di fondazione $[m]$;
- $e$ eccentricita della risultante $[m]$.

Per il carico limite il repository `Muro` usa una formulazione trinomia di Brinch-Hansen:

$$
q_{lim} = c' N_c i_c + q N_q i_q + \frac{1}{2}\gamma B' N_\gamma i_\gamma
$$

che e la parte decisiva quando il muro resta apparentemente stabile ma la domanda al piano di posa si avvicina troppo alla capacita del terreno.

## 2. Caso numerico reale del repository Muro

Il benchmark usato nel post rappresenta un muro a mensola drenato, con sovraccarico superficiale e controllo pseudo-statico:

- altezza del terreno trattenuto: $H = 5.50 \ \mathrm{m}$;
- base totale: $B = 3.80 \ \mathrm{m}$;
- punta: $B_p = 1.00 \ \mathrm{m}$;
- tallone: $B_t = 2.80 \ \mathrm{m}$;
- spessore base: $t_b = 0.55 \ \mathrm{m}$;
- spessore fusto: da $0.30$ a $0.60 \ \mathrm{m}$;
- calcestruzzo: $\gamma_{cls} = 24 \ \mathrm{kN/m^3}$;
- sovraccarico di monte: $q = 20 \ \mathrm{kPa}$;
- attrito alla base: $\mu = 0.52$;
- attrito terra-muro: $\delta = 18^\circ$;
- riferimento di pressione ammissibile: $q_{amm} = 280 \ \mathrm{kPa}$;
- terreno a fronte: $h_f = 0.50 \ \mathrm{m}$;
- falda assente nel benchmark: `falda_retro = falda_fronte = 99 m`;
- pseudo-statica: $k_h = 0.12$, $k_v = 0.04$;
- pendio di monte per il controllo globale: $H_p = 5.50 \ \mathrm{m}$, $\beta = 26^\circ$, berma $= 6.00 \ \mathrm{m}$.

La stratigrafia del benchmark e a due strati:

- strato 1, da $0$ a $3.0 \ \mathrm{m}$: $\gamma = 18 \ \mathrm{kN/m^3}$, $\phi = 30^\circ$;
- strato 2, da $3.0$ a $9.0 \ \mathrm{m}$: $\gamma = 19 \ \mathrm{kN/m^3}$, $\phi = 34^\circ$.

![Output reale del repo Muro con geometria del muro, terreno a tergo, terreno a fronte e schema fisico del benchmark pseudo-statico]({{ site.baseurl }}/assets/images/muri-sostegno-statico-sismico-muro-geometria.png)

Con questi dati i pesi stabilizzanti principali per metro di muro sono:

$$
W_{base} = B t_b \gamma_{cls} = 3.80 \cdot 0.55 \cdot 24 = 50.2 \ \mathrm{kN/m}
$$

$$
W_{fusto} =
\frac{t_{top}+t_{bot}}{2} H \gamma_{cls}
= \frac{0.30+0.60}{2}\cdot 5.50 \cdot 24
= 59.4 \ \mathrm{kN/m}
$$

La tensione verticale media efficace del terreno sul tallone, nel benchmark, vale circa:

$$
\gamma_{eq} = \frac{18\cdot 3.0 + 19\cdot 2.5}{5.5} = 18.45 \ \mathrm{kN/m^3}
$$

da cui:

$$
W_{tallone} = B_t H \gamma_{eq}
= 2.80 \cdot 5.50 \cdot 18.45
= 284.2 \ \mathrm{kN/m}
$$

Nel caso statico GEO il sovraccarico viene amplificato con il coefficiente della combinazione geotecnica del benchmark, per cui il contributo stabilizzante sul tallone vale:

$$
W_q = q \, B_t \, \gamma_Q
= 20 \cdot 2.80 \cdot 1.30
= 72.8 \ \mathrm{kN/m}
$$

Questi numeri sono utili perche fanno vedere subito che, in un muro del genere, il peso del terreno sul tallone conta spesso piu del solo peso del calcestruzzo.

## 3. Passaggi di calcolo sul benchmark

### 3.1 Coefficienti di spinta

Usando le stesse formule implementate nel repository, si ottengono valori di riferimento molto vicini a questi:

- strato 1 statico: $K_{a,1} \approx 0.299$;
- strato 2 statico: $K_{a,2} \approx 0.256$;
- strato 1 sismico governante `kv-`: $K_{ae,1} \approx 0.393$;
- strato 2 sismico governante `kv-`: $K_{ae,2} \approx 0.341$.

Il significato fisico e immediato: con il sisma pseudo-statico la spinta laterale cresce, soprattutto nello strato piu superficiale, dove il sovraccarico uniforme entra con maggiore peso relativo.

### 3.2 Risultanti a tergo e a fronte

L'integrazione numerica del diagramma delle pressioni, eseguita dal solver `Muro`, porta a:

- combinazione `Statica GEO`: $P_a = 133.1 \ \mathrm{kN/m}$;
- combinazione `Statica EQU`: $P_a = 148.9 \ \mathrm{kN/m}$;
- combinazione `Sismica kv+`: $P_a = 150.9 \ \mathrm{kN/m}$;
- combinazione `Sismica kv-`: $P_a = 160.3 \ \mathrm{kN/m}$.

Il contributo passivo sul fronte resta molto piccolo rispetto alla spinta attiva:

$$
P_p \approx 5.5 \ \mathrm{kN/m} \quad \text{(statico)}
$$

$$
P_p \approx 5.2 \ \mathrm{kN/m} \quad \text{(sismico)}
$$

Questa e gia una prima lettura professionale: il benchmark non e dominato dal passivo davanti alla punta, quindi un eventuale degrado del terreno di fronte non cambia l'ordine di grandezza del problema.

![Output reale del repo Muro con diagrammi delle pressioni attive e passive, confronto tra caso statico e pseudo-statico sul benchmark]({{ site.baseurl }}/assets/images/muri-sostegno-statico-sismico-muro-pressioni.png)

### 3.3 Verifiche GEO/EQU e combinazione governante

I risultati sintetici piu utili del benchmark sono:

- ribaltamento statico `EQU`: $FS_{rib} = 3.282$;
- scorrimento statico `GEO`: $FS_{scorr} = 2.033$;
- portanza statica `GEO`: $FS_{port} = 1.935$;
- portanza sismica `kv+`: $FS_{port} = 1.094$;
- portanza sismica `kv-`: $FS_{port} = 1.071$;
- pressione di contatto governante: $q_{max} = 136.8 \ \mathrm{kPa}$ in `Sismica kv-`.

Il punto importante non e che il muro "passa" il ribaltamento. Quello era prevedibile gia dai pesi stabilizzanti. Il punto importante e che la pseudo-statica riduce il margine di portanza fino a poco piu del 7% sulla combinazione governante `kv-`. Questo e il genere di esito che merita una nota esplicita in relazione, non una riga automatica senza commento.

![Tabella reale generata dal benchmark Muro con spinte, fattori di sicurezza e combinazione governante per la portanza]({{ site.baseurl }}/assets/images/muri-sostegno-statico-sismico-muro-verifiche.png)

### 3.4 Sollecitazioni allo spiccato

Per il dimensionamento strutturale del fusto, il solver restituisce anche le sollecitazioni allo spiccato. Sulla combinazione `Sismica kv-`:

$$
N = 113.8 \ \mathrm{kN/m}
$$

$$
V = 167.4 \ \mathrm{kN/m}
$$

$$
M = 366.1 \ \mathrm{kNm/m}
$$

Questi valori sono la cerniera tra geotecnica e strutture: servono per armare il muro, ma hanno senso solo se la catena delle spinte e delle verifiche geotecniche e stata prima ricostruita bene.

### 3.5 Stabilita globale del pendio

Il benchmark e ancora piu istruttivo sulla stabilita globale. Il controllo del pendio di monte, eseguito dal repo con metodo Bishop semplificato in statico e Fellenius semplificato in sismica, restituisce:

$$
FS_{pend,stat} = 1.005
$$

$$
FS_{pend,sis} = 0.962
$$

Questo significa che il problema vero non e piu il solo muro, ma il complesso opera-terreno. In pratica:

- il muro locale resta leggibile e quasi "tranquillo" sulle verifiche immediate;
- la portanza in pseudo-statica si avvicina al limite;
- la stabilita globale del pendio non ha margine sufficiente.

E' una situazione molto realistica nelle opere di sostegno a valle di riporti o rilevati: il progettista che guarda solo il `FS` di ribaltamento rischia di perdere il meccanismo governante.

![Output reale del repo Muro con superficie critica statica e sismica del pendio associato al benchmark pseudo-statico]({{ site.baseurl }}/assets/images/muri-sostegno-statico-sismico-muro-pendio.png)

## 4. Come usare Muro sullo stesso caso

Per rifare il benchmark in `Muro` conviene seguire questa sequenza operativa:

1. avviare l'app con `streamlit run app.py` nella cartella del repository `Muro`;
2. importare `test/benchmark/asdip_cantilever_surcharge_seismic.json` oppure copiare a mano geometria, sovraccarico, $\mu$, $\delta$, `kh`, `kv` e stratigrafia;
3. verificare in sidebar che la base totale resti `B = B_p + B_t = 3.80 m`, che la falda sia esclusa e che il contributo passivo a fronte sia attivo;
4. leggere nella sezione iniziale la `Sintesi per relazione` e le `Combinazioni governanti`, senza fermarsi al primo `FS` favorevole;
5. passare al `Modello di calcolo` per controllare geometria, tallone, terreno a fronte e ordine di grandezza delle leve stabilizzanti;
6. controllare `Pressioni sul terreno` e verificare che la crescita sismica della spinta sia coerente con i coefficienti inseriti;
7. usare `Verifiche geotecniche` per leggere insieme `FS_rib`, `FS_scorr`, `q_max` e `FS_portanza`, individuando la combinazione governante;
8. leggere `Sollecitazioni strutturali allo spiccato` per trasferire al progetto del fusto i valori di $N$, $V$ e $M$;
9. attivare la sezione `Verifica di stabilita del pendio` quando il muro fa parte di un rilevato o di un fronte geotecnico piu ampio;
10. esportare Word, PDF o JSON solo dopo avere annotato quale verifica governa davvero il progetto.

Questo e il punto in cui `Muro` entra in modo utile dentro StruHub/CivilBox: non per sostituire il giudizio del progettista, ma per lasciare una traccia verificabile tra ipotesi del terreno, combinazioni, diagrammi, superfici critiche e relazione finale.

## 5. Cosa conta davvero nel benchmark

Dal caso numerico emergono tre messaggi professionali:

- un muro apparentemente molto stabile a ribaltamento puo avere un margine di portanza sismica molto piu ridotto di quanto sembri a una prima lettura;
- il contributo del terreno sul tallone e spesso dominante, quindi errori su stratigrafia, sovraccarico o geometria valgono piu di piccole differenze sul peso del calcestruzzo;
- quando la verifica globale del pendio va in crisi, non basta "irrobustire il fusto": bisogna ripensare il sistema opera-terreno.

Per questo un archivio tecnico come StruHub ha senso solo se collega teoria, formule, benchmark e output verificabili. E per questo un'app come `Muro` e utile in modo discreto ma concreto: rende leggibile dove nasce il margine e dove invece il progetto comincia a diventare sensibile.

## Riferimenti tecnici e normativi

- D.M. 17 gennaio 2018, NTC 2018, Capitolo 6 e Capitolo 7.
- Circolare C.S.LL.PP. 21 gennaio 2019 n. 7, paragrafo C6.5.3.
- UNI EN 1997-1, Sezione 9, opere di sostegno e verifiche geotecniche.
- UNI EN 1998-5, aspetti geotecnici e opere di sostegno in zona sismica.
- Mononobe, N. and Okabe, S., metodo pseudo-statico per la spinta sismica dei terreni.
- Brinch Hansen, J., formulazioni classiche per il carico limite delle fondazioni superficiali.
