---
layout: post
title: "Applicazione del metodo Guyon-Massonnet-Bares per piastre ortotrope equivalenti"
description: "Metodo tecnico per la ripartizione trasversale dei carichi negli impalcati, con formule, esempio numerico e controllo software."
date: 2026-01-04
order: 4
permalink: /posts/guyon-massonnet-bares-piastre-ortotrope.html
meta: "Quaderno tecnico - ripartizione trasversale con piastra ortotropa"
---

**Dall'impalcato reale alla piastra equivalente**

Il metodo Guyon-Massonnet-Bares nasce per leggere la ripartizione trasversale dei carichi negli impalcati a travi e soletta. L'idea e sostituire il sistema discreto di travi longitudinali, traversi e soletta con una piastra ortotropa equivalente, cioe una piastra che non ha la stessa rigidezza in tutte le direzioni.

Il modello non serve a "fare sparire" le travi. Serve a capire come un carico applicato in una certa posizione trasversale si distribuisce tra le travi principali. In un impalcato molto rigido trasversalmente, la domanda tende a diffondersi. In un impalcato piu debole trasversalmente, il carico resta concentrato vicino alla trave caricata.

La piastra equivalente e descritta da rigidezze longitudinali, trasversali e torsionali. In forma sintetica:

$$
\rho_P=\frac{E I_l}{s}
$$

$$
\rho_E=\frac{E I_t}{s_t}
$$

dove $I_l$ e l'inerzia longitudinale equivalente, $I_t$ e l'inerzia trasversale equivalente, $s$ e l'interasse tra le travi principali e $s_t$ e il passo caratteristico dei traversi o della ripartizione trasversale.

Il parametro di intreccio del metodo puo essere scritto come:

$$
\theta=\frac{b}{L}\sqrt[4]{\frac{\rho_P}{\rho_E}}
$$

dove $2b$ e la larghezza efficace dell'impalcato e $L$ e la luce. Il parametro di torsione e:

$$
\alpha=\frac{\gamma_P+\gamma_E}{2\sqrt{\rho_P\rho_E}}
$$

con $\gamma_P$ e $\gamma_E$ legati alla rigidezza torsionale longitudinale e trasversale. Valori bassi di $\alpha$ indicano una struttura poco efficace a torsione; valori piu alti indicano maggiore capacita di redistribuzione.

**Coefficiente di ripartizione**

Per un carico applicato con eccentricita trasversale $e$, il coefficiente di ripartizione sulla trave in posizione $y$ e indicato come:

$$
K_\alpha(y,e,\theta,\alpha)
$$

Nella formulazione operativa usata nell'app `GuyonMassonnetBares`, il coefficiente viene interpolato tra due casi limite:

$$
K_\alpha \simeq K_0+(K_1-K_0)\alpha^n
$$

dove $K_0$ e $K_1$ rappresentano i comportamenti limite e l'esponente $n$ dipende dal campo di $\theta$. Il passaggio e importante perche rende esplicita la dipendenza del risultato dai rapporti di rigidezza, non solo dalla posizione geometrica del carico.

Il carico longitudinale $p(x)$ viene sviluppato in serie di Fourier:

$$
p(x)=\sum_{m=1}^{N} A_m\sin\left(\frac{m\pi x}{L}\right)
$$

Per una forza concentrata $P$ applicata in $x_c$:

$$
A_m=\frac{2P}{L}\sin\left(\frac{m\pi x_c}{L}\right)
$$

Per ogni armonica si calcola l'effetto lungo la trave e lo si pesa trasversalmente con $K_\alpha$. Il momento sulla trave $j$ puo essere letto come:

$$
M_j(x)=W_j M_{medio}(x)
$$

dove $W_j$ e il peso trasversale associato alla posizione della trave. Il controllo di equilibrio resta essenziale: la ripartizione deve essere compatibile con il carico globale e con la rigidezza dell'impalcato.

**Esempio numerico**

Si considera un impalcato con:

- luce $L=22.30\,\mathrm{m}$;
- larghezza $L_y=11.50\,\mathrm{m}$, quindi $b=5.75\,\mathrm{m}$;
- $11$ travi longitudinali con interasse $s=1.00\,\mathrm{m}$;
- modulo elastico $E=32.308\,\mathrm{GPa}$;
- inerzia longitudinale $I_l=11375171\,\mathrm{cm^4}$;
- inerzia torsionale longitudinale $J_l=510383\,\mathrm{cm^4}$;
- inerzia trasversale $I_t=10382507\,\mathrm{cm^4}$;
- inerzia torsionale trasversale $J_{lt}=1228304\,\mathrm{cm^4}$.

Con questi dati l'app calcola:

$$
\rho_P=3.675\cdot10^9\,\mathrm{N\,m}
$$

$$
\rho_E=3.008\cdot10^8\,\mathrm{N\,m}
$$

$$
\theta=0.482
$$

$$
\alpha=0.0397
$$

Il valore di $\theta$ descrive una piastra equivalente con forte differenza tra direzione longitudinale e trasversale; il valore basso di $\alpha$ segnala una torsione non dominante. In termini pratici, ci si aspetta una ripartizione non uniforme e sensibile alla posizione trasversale del carico.

Il caso di carico usato nell'esempio e composto da:

- carico distribuito $q_1=9.5\,\mathrm{kN/m^2}$ su fascia larga $3.0\,\mathrm{m}$ a $y=+2.5\,\mathrm{m}$;
- carico distribuito $q_2=2.5\,\mathrm{kN/m^2}$ su fascia larga $3.0\,\mathrm{m}$ a $y=-0.5\,\mathrm{m}$;
- due assi concentrati da $300\,\mathrm{kN}$ a $x=0.47L$ e $x=0.53L$, entrambi a $y=+2.5\,\mathrm{m}$.

I risultati sulle travi principali mostrano la concentrazione della domanda verso il lato caricato:

| Trave | Coordinata $y$ | Peso $W_j$ | $M_{max}$ | $V_{max}$ |
| --- | ---: | ---: | ---: | ---: |
| bordo sinistro | $-5.0\,\mathrm{m}$ | $0.351$ | $171.9\,\mathrm{kNm}$ | $22.2\,\mathrm{kN}$ |
| centrale | $0.0\,\mathrm{m}$ | $0.864$ | $422.5\,\mathrm{kNm}$ | $54.7\,\mathrm{kN}$ |
| vicino al carico | $+2.0\,\mathrm{m}$ | $0.975$ | $476.8\,\mathrm{kNm}$ | $61.7\,\mathrm{kN}$ |
| bordo destro | $+5.0\,\mathrm{m}$ | $0.806$ | $394.5\,\mathrm{kNm}$ | $51.0\,\mathrm{kN}$ |

Lo spostamento massimo della superficie equivalente, nel caso dimostrativo, e:

$$
w_{max}=67.8\,\mathrm{mm}
$$

Questo valore non va usato da solo come verifica di esercizio: serve a leggere la forma deformata e la distribuzione trasversale. La verifica locale della singola trave richiede poi le sezioni resistenti, le combinazioni e i limiti di progetto.

**Uso dell'app GuyonMassonnetBares**

La schermata seguente proviene dal repository `GuyonMassonnetBares`, eseguito localmente come app Streamlit. Si vedono i parametri $\theta$, $\alpha$, $\rho$ e $\gamma$ e la vista in pianta dell'impalcato a graticcio.

![Schermata dell'app GuyonMassonnetBares con parametri e vista in pianta]({{ site.baseurl }}/assets/images/guyon-massonnet-bares-piastre-ortotrope-app.png)

Per riprodurre il caso:

1. Avviare l'app con `streamlit run streamlit_app.py`.
2. Nella sidebar impostare luce, larghezza, numero di travi e interasse.
3. Inserire le proprieta di materiale e sezioni: $E$, $\nu$, $I_l$, $J_l$, $I_t$ e $J_{lt}$.
4. Controllare subito $\theta$ e $\alpha$: sono il primo indicatore di comportamento trasversale.
5. Nel tab `Casi di carico`, creare o usare il caso demo con carichi distribuiti e assi concentrati.
6. Nel tab `Risultati`, leggere deformata, superficie dei momenti e curve per trave.
7. Usare la tabella dei pesi $W_j$ per capire quale trave riceve la quota governante.
8. Esportare il report solo dopo aver controllato equilibrio, posizione del carico e rigidezze equivalenti.

Il valore tecnico dell'app non e sostituire la teoria, ma renderla ispezionabile. Il progettista vede come cambiano $\theta$, $\alpha$ e la distribuzione quando modifica interasse, rigidezza trasversale, torsione o posizione del carico. Questo e il modo corretto di usare Guyon-Massonnet-Bares: non come tabella da copiare, ma come ponte tra modello semianalitico e verifica locale.

Riferimenti tecnici utilizzati:

- D.M. 17 gennaio 2018, "Aggiornamento delle Norme tecniche per le costruzioni", Capitolo 5, per il quadro nazionale delle azioni sui ponti.
- Circolare C.S.LL.PP. 21 gennaio 2019, n. 7, per l'applicazione delle NTC 2018.
- UNI EN 1991-2:2005, Eurocodice 1 - Azioni sulle strutture - Parte 2: Carichi da traffico sui ponti.
- Guyon, Y., Massonnet, C. e Bares, R., riferimenti classici sulla ripartizione trasversale degli impalcati a piastra ortotropa equivalente.
