---
layout: post
title: "Verifiche di ribaltamento, scorrimento e portanza nei muri di sostegno"
description: "Metodo operativo per leggere le verifiche GEO/EQU dei muri di sostegno, con formule, esempio numerico e controllo software."
date: 2026-01-20
order: 20
permalink: /posts/verifiche-ribaltamento-scorrimento-portanza-muri.html
meta: "Quaderno tecnico - verifiche GEO/EQU dei muri di sostegno"
---

**Perche le tre verifiche non sono intercambiabili**

Un muro di sostegno puo risultare soddisfacente a ribaltamento e allo stesso tempo essere critico a scorrimento, portanza o stabilita globale. Il controllo non va letto come un unico numero di sicurezza, ma come una catena di equilibri: spinta del terreno, pesi stabilizzanti, reazione del piano di posa e resistenza del terreno di fondazione.

La verifica a ribaltamento intercetta la tendenza alla rotazione attorno al ciglio di valle:

$$
FS_{rib}=\frac{M_{stab}}{M_{rib}}
$$

dove $M_{stab}$ raccoglie i momenti stabilizzanti, mentre $M_{rib}$ raccoglie i momenti ribaltanti. In una verifica agli stati limite il rapporto sintetico deve essere letto insieme alle combinazioni EQU/GEO e ai coefficienti parziali prescritti.

La verifica a scorrimento confronta la resistenza tangenziale disponibile alla base con la risultante orizzontale:

$$
R_d=N_d \tan \delta_d+c_{a,d} B+P_{p,d}
$$

$$
FS_{scorr}=\frac{R_d}{H_d}
$$

dove $N_d$ e la risultante verticale, $\delta_d$ e l'attrito alla base, $c_{a,d}$ e l'eventuale adesione, $B$ e la larghezza della fondazione, $P_{p,d}$ e il contributo passivo eventualmente considerabile e $H_d$ e l'azione orizzontale agente.

La verifica di portanza non deve limitarsi alla pressione media. Il momento alla base sposta la risultante verticale e produce eccentricita:

$$
e=\frac{M_d}{N_d}
$$

Se la risultante resta nel nucleo centrale della fondazione, cioe:

$$
|e|\leq \frac{B}{6}
$$

la pressione di contatto puo essere rappresentata con distribuzione lineare:

$$
q_{max,min}=\frac{N_d}{B}\left(1\pm\frac{6e}{B}\right)
$$

Il controllo operativo diventa:

$$
q_{max}\leq q_{Rd}
$$

oppure, in forma di indice:

$$
\eta_q=\frac{q_{max}}{q_{Rd}}\leq 1
$$

Se $q_{min}<0$, una parte della base risulta in decompressione teorica. In quel caso non e piu corretto usare la distribuzione lineare su tutta la larghezza: si passa a una base reagente efficace, con pressione triangolare o trapezia solo sulla zona compressa.

**Dalla spinta alla risultante**

Nel caso piu semplice, con terreno omogeneo drenato e superficie orizzontale, il coefficiente di spinta attiva di Rankine e:

$$
K_a=\frac{1-\sin\phi'}{1+\sin\phi'}
$$

La pressione orizzontale efficace a profondita $z$ e:

$$
\sigma'_h(z)=K_a\sigma'_v(z)
$$

Con sovraccarico uniforme $q$ a tergo:

$$
\sigma'_h(z)=K_a(\gamma' z+q)
$$

In presenza di falda, la pressione neutra non va confusa con la resistenza del terreno. La pressione totale agente sul paramento e:

$$
\sigma_h(z)=K_a\sigma'_v(z)+u(z)
$$

con:

$$
u(z)=\gamma_w(z-z_w)
$$

per $z>z_w$. Questa separazione e importante: il terreno resiste tramite tensioni efficaci, mentre l'acqua aggiunge spinta senza aumentare l'attrito disponibile alla base.

**Esempio numerico**

Si consideri un muro a mensola alto $H=5.0\,\mathrm{m}$, con base totale $B=3.50\,\mathrm{m}$, punta $1.00\,\mathrm{m}$, tallone $2.50\,\mathrm{m}$ e spessore di base $0.50\,\mathrm{m}$. Il calcestruzzo e assunto con peso di volume $\gamma_c=24\,\mathrm{kN/m^3}$.

Il terreno a tergo e multistrato:

$$
\begin{array}{c|c|c|c|c}
\text{Strato} & h\,[\mathrm{m}] & \gamma_{dry}\,[\mathrm{kN/m^3}] & \gamma_{sat}\,[\mathrm{kN/m^3}] & \phi'\,[^\circ] \\
\hline
1 & 2.0 & 18 & 20 & 30 \\
2 & 3.0 & 19 & 21 & 34 \\
3 & 5.0 & 20 & 20 & 0
\end{array}
$$

Si assume un sovraccarico a tergo $q=10\,\mathrm{kPa}$, attrito alla base $\mu=0.55$, attrito terra-muro $\delta=20^\circ$, pressione ammissibile di riferimento $q_{amm}=250\,\mathrm{kPa}$, terreno a fronte alto $0.50\,\mathrm{m}$ e falda profonda. Per il controllo sismico pseudo-statico si usano $k_h=0.15$ e $k_v=0.05$.

Il calcolo manuale di primo livello puo essere controllato partendo dal primo strato drenato. Con $\phi'=30^\circ$:

$$
K_a=\frac{1-\sin30^\circ}{1+\sin30^\circ}=0.333
$$

Una stima rapida della spinta attiva, trascurando per un momento la stratigrafia dettagliata e l'attrito terra-muro, e:

$$
P_a \simeq \frac{1}{2}K_a\gamma H^2+K_a qH
$$

$$
P_a \simeq \frac{1}{2}\cdot0.333\cdot18\cdot5^2+0.333\cdot10\cdot5=91.6\,\mathrm{kN/m}
$$

Il motore di calcolo, integrando stratigrafia, sovraccarico, attrito terra-muro e condizioni statiche, restituisce una spinta statica pari a circa:

$$
P_{a,statico}=90.1\,\mathrm{kN/m}
$$

L'ordine di grandezza e coerente con il controllo manuale. Questo e il primo filtro: prima ancora di leggere i fattori di sicurezza, la spinta deve essere fisicamente credibile.

Nel caso statico GEO la sintesi numerica e:

$$
FS_{scorr}=6.16
$$

$$
FS_{portanza}=4.54
$$

$$
q_{max}=110.8\,\mathrm{kPa}
$$

Il rapporto rispetto alla pressione ammissibile di riferimento e:

$$
\frac{q_{max}}{q_{amm}}=\frac{110.8}{250}=0.44
$$

La combinazione EQU per il ribaltamento fornisce:

$$
FS_{rib,EQU}=3.85
$$

Il controllo sismico e piu severo sulla spinta orizzontale. Con $k_h=0.15$ e $k_v=0.05$ il modello restituisce:

$$
P_{a,sisma}=112.3\,\mathrm{kN/m}
$$

e, per la combinazione con $k_v$ sfavorevole:

$$
FS_{rib}=2.42,\qquad FS_{scorr}=3.36,\qquad FS_{portanza}=4.31
$$

Il muro risulta quindi ampiamente verificato nelle verifiche esterne principali. Il punto interessante e un altro: attivando anche la verifica di stabilita globale del pendio, il caso restituisce $FS=1.14$ in statico e circa $0.92$ in sismico. Questo non annulla le verifiche locali del muro, ma segnala che il sistema opera-terreno va letto oltre il solo piede della fondazione.

**Lettura con l'applicazione**

La schermata seguente e stata acquisita dal repository `Muro`, eseguendo l'app Streamlit con lo stesso caso numerico. La dashboard riporta le verifiche principali, i riferimenti NTC/EC7 e la sintesi per relazione.

![Schermata dell'app Muro con sintesi delle verifiche GEO/EQU]({{ site.baseurl }}/assets/images/verifiche-ribaltamento-scorrimento-portanza-muri-muro-app.png)

L'aspetto utile non e il colore della dashboard, ma la sequenza dei controlli. In un unico flusso si vede se il muro e governato da ribaltamento, scorrimento, portanza, pressione di contatto o stabilita globale. Questo riduce il rischio di fermarsi al primo fattore di sicurezza favorevole.

**Come riprodurre il caso nel software**

Nel repository `Muro`, il caso si riproduce in questo modo:

1. Avviare l'app con `streamlit run app.py`.
2. Nella sidebar impostare la geometria: $H=5.0\,\mathrm{m}$, $B=3.50\,\mathrm{m}$, punta $1.00\,\mathrm{m}$, tallone $2.50\,\mathrm{m}$, base $0.50\,\mathrm{m}$, fusto $0.30/0.50\,\mathrm{m}$.
3. Impostare i parametri di carico: $q=10\,\mathrm{kPa}$, $\gamma_c=24\,\mathrm{kN/m^3}$, $\mu=0.55$, $\delta=20^\circ$, $q_{amm}=250\,\mathrm{kPa}$.
4. Lasciare la falda profonda e inserire la stratigrafia nel formato `spessore,gamma_dry,gamma_sat,phi,cu,k`:

```text
2.0,18,20,30,0,25000
3.0,19,21,34,0,40000
5.0,20,20,0,120,60000
```

5. Attivare il contributo passivo e impostare il terreno a fronte pari a $0.50\,\mathrm{m}$, verificando poi se quel contributo e accettabile nel contesto progettuale.
6. Impostare il controllo pseudo-statico con $k_h=0.15$ e $k_v=0.05$.
7. Leggere prima il cruscotto: `FS Ribaltamento`, `FS Scorrimento`, `FS Portanza`, `q_max/q_amm` e `FS Pendio`.
8. Passare alle tabelle di sintesi per distinguere combinazioni statiche, EQU e sismiche.
9. Esportare la relazione Word o PDF solo dopo aver controllato warning, ipotesi di falda, passivo e stabilita globale.

La parte finale e volutamente operativa: il software non sostituisce il giudizio del progettista, ma costringe a tenere insieme input, formule, combinazioni e output. Nel caso dei muri di sostegno, questa tracciabilita e spesso piu importante del singolo numero finale.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, "Aggiornamento delle Norme tecniche per le costruzioni", Capitolo 6, con particolare attenzione alle opere di sostegno e alle verifiche geotecniche.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, istruzioni applicative per le verifiche geotecniche e per il complesso opera-terreno.
- UNI EN 1997-1:2013, Eurocodice 7 - Progettazione geotecnica - Parte 1: Regole generali.
- UNI EN 1998-5:2005, Eurocodice 8 - Fondazioni, strutture di contenimento e aspetti geotecnici, per il richiamo alle condizioni sismiche.
