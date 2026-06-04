---
layout: post
title: "La ripartizione trasversale dei carichi negli impalcati da Ponte"
description: "Articolo tecnico StruHub con immagini e passaggi di calcolo."
date: 2026-01-23
order: 23
permalink: /posts/la-ripartizione-trasversale-dei-carichi.html
meta: "Archivio StruHub · 816 parole circa"
---

![Immagine tecnica StruHub]({{ site.baseurl }}/assets/images/la-ripartizione-trasversale-dei-carichi-01.jpg)

**Formule di riferimento**

\[\n\rho=\frac{E\,I_\ell}{s}\]

\[\gamma=\frac{E\,I_t}{s_t}\]

\[\n\n\rho_G=\frac{G\,J_\ell}{s},\qquad\n\n\gamma_G=\frac{G\,J_t}{s_t}\n\]

\[\alpha=\frac{\gamma_G+\rho_G}{2\sqrt{\rho\,\gamma}},\qquad\n\n\theta=\frac{b}{L}\,\sqrt[4]{\frac{\rho}{\gamma}}\n\n\]

\[\n\n\n\lambda b=\frac{\pi\,\theta}{\sqrt{2}},\qquad \sigma=\pi\,\theta\n\]

**Formule di riferimento**

\[p(x)=\sum_{m\ge1} a_m\sin\!\Big(\frac{m\pi x}{L}\Big)\]

\[a_m=\frac{2}{L}\int_0^L p(x)\sin\!\Big(\frac{m\pi x}{L}\Big)\,dx\]

\[a_m=\frac{2P}{L}\,\sin\!\Big(\frac{m\pi x_c}{L}\Big)\]

\[a_m=\frac{2p}{m\pi}\left[\cos\!\Big(\frac{m\pi x_1}{L}\Big)-\cos\!\Big(\frac{m\pi x_2}{L}\Big)\right\]

\[a_m=\frac{4q}{m\pi}\]

\[\mathrm{den}=\sinh(2\lambda b)^2-\sin(2\lambda b)^2,\qquad
\mathrm{pf}=\frac{2\,\lambda b}{\mathrm{den}}\]

\[\Lambda_{y+b}=\lambda\,(y^*+b),\qquad
\Lambda_{b\pm e}=\lambda\,(b\pm e^*)\]

\[\begin{aligned}
a'&=2\cosh(\Lambda_{y+b})\cos(\Lambda_{y+b}),\
b'&=\cosh(\Lambda_{y+b})\sin(\Lambda_{y+b})+\sinh(\Lambda_{y+b})\cos(\Lambda_{y+b}),\
A&=\sinh(2\lambda b)\cos(\Lambda_{b+e})\cosh(\Lambda_{b-e})-\sin(2\lambda b)\cosh(\Lambda_{b+e})\cos(\Lambda_{b-e}),\
B_1&=\sinh(2\lambda b)\,[\sin(\Lambda_{b+e})\cosh(\Lambda_{b-e})-\cos(\Lambda_{b+e})\sinh(\Lambda_{b-e})],\
B_2&=\sin(2\lambda b)\,[\sinh(\Lambda_{b+e})\cos(\Lambda_{b-e})-\cosh(\Lambda_{b+e})\sin(\Lambda_{b-e})]
\end{aligned}\]

\[K_0(y,e;\theta)=\mathrm{pf}\,[\,a'\,A+b'(B_1+B_2)\,\]

\[\beta=\dfrac{\pi y}{b}\]

\[\psi=\dfrac{\pi e}{b}\]

\[\chi=\pi-|\beta-\psi|\]

\[\Theta_u=\theta\,u\]

\[\mathrm{pf}=\dfrac{\sigma}{2\sinh(\sigma)^2}\]

\[\begin{aligned}
R(u)&=\cosh(\Theta_u)\,[\sigma\cosh(\sigma)-\sinh(\sigma)]-\Theta_u\sinh(\sigma)\sinh(\Theta_u),\
Q(u)&=\sinh(\Theta_u)\,[2\sinh(\sigma)+\sigma\cosh(\sigma)]-\Theta_u\sinh(\sigma)\cosh(\Theta_u),\
C&=\cosh(\Theta_\chi)\,[\sigma\cosh(\sigma)+\sinh(\sigma)],\qquad
D=\Theta_\chi\sinh(\sigma)\sinh(\Theta_\chi),\
E&=\dfrac{R(\beta)R(\psi)}{3\sinh(\sigma)\cosh(\sigma)-\sigma},\qquad
F=\dfrac{Q(\beta)Q(\psi)}{3\sinh(\sigma)\cosh(\sigma)+\sigma}
\end{aligned}
\]

\]

\[K_1(y,e;\theta)=\mathrm{pf}\,[\,C-D+E+F\,]\]

\[\nK_\alpha(y,e;\theta,\alpha)=K_0+\big(K_1-K_0\big)\,\alpha^{\,\eta(\theta)}\n\]

\[\eta=1-\exp\!\big((0.065-\theta)/0.663\big)\]

\[w(y,x)=\sum_{m\ge1}\left[\frac{1}{\rho\,\alpha_m^4}\sum_e a_m(e)\,K_\alpha(y,e)\right]\sin(\alpha_m x)\]

\[M_{\text{medio}}(x)=\frac{M_{\text{tot}}(x)}{n_{\text{travi}}},\qquad\n\nV_{\text{medio}}(x)=\frac{V_{\text{tot}}(x)}{n_{\text{travi}}}\]

\[P(e)=\int_0^L p_e(x)\,dx\]

\[W(y_j)=\frac{\sum_e P(e)\,K_\alpha(y_j,e)}{\sum_e P(e)},\qquad \sum_j W(y_j)=1\]

\[M^{(j)}(x)=W(y_j)\,M_{\text{medio}}(x),\qquad\n\nV^{(j)}(x)=W(y_j)\,V_{\text{medio}}(x)\]

**Impostazione tecnica**

La ripartizione trasversale dei carichi in impalcati a travi con soletta collaborante può essere ricondotta alla risposta di una piastra ortotropa equivalente: l'impalcato discreto (travi longitudinali, eventuali traversi, soletta) viene rappresentato come mezzo continuo con rigidezze per unità di lunghezza nelle due direzioni principali. La formulazione di Guyon-Massonnet-BareÅ¡ (GMB) permette di combinare uno sviluppo modale lungo la luce con funzioni di ripartizione trasversale per stimare, in modo coerente e rapido, gli effetti (momento/taglio) su ciascuna trave.

Ipotesi alla base della formulazione

- Equivalenza continua travi-soletta: l'impalcato discreto è rappresentato come piastra ortotropa con rigidezze distribuite per unità di lunghezza nelle due direzioni principali (longitudinale e trasversale).

- Linearità elastica: comportamento lineare dei materiali; sovrapposizione degli effetti (principio di sovrapposizione) nelle combinazioni di carico.

- Sezioni piane: in direzione longitudinale (compatibilità ala-anima) e aderenza perfetta soletta-travi (niente scorrimenti relativi).

- Rigidezze quasi costanti lungo la campata: le grandezze equivalenti ρ,, ρ G, G sono assunte uniformi lungo x (variazioni lente tollerate).

- Condizioni al contorno semplificate: in direzione longitudinale: schema di vincolo equivalente "appoggi semplici" per lo sviluppo modale sinusoidale.

- Carichi rappresentabili su serie: le azioni mobili (concentrate, distribuite parziali/totali) sono ricondotte a serie di Fourier lungo la luce (convergenti entro un numero finito di termini).

- Valutazione trasversale tramite K: la distribuzione trasversale si basa sulle funzioni limite k 0 (piastra non torsorigida) e k 1 (piastra isotropa), con interpolazione non lineare k α in funzione di α e ϑ.

- Larghezza attiva convenzionale: l'ampiezza efficace 2b include l'eventuale contributo (ridotto) degli sbalzi laterali.

- Torsione "globale": l'effetto torsionale è sintetizzato in α mediante ρ G, G equivalenti (Saint-Venant), senza dettagli locali di distorsione o warping complesso.

![Immagine tecnica StruHub]({{ site.baseurl }}/assets/images/la-ripartizione-trasversale-dei-carichi-02.jpg)

Modello equivalente e parametri caratteristici

Si definiscono le rigidezze unitarie della piastra ortotropa:

- Rigidezza flessionale longitudinale (per interasse s delle travi):

- Rigidezza flessionale trasversale (per passo s t dei traversi o rigidezza trasversale equivalente):

- Rigidezze torsionali per unità di lunghezza nelle due direzioni (con G=E/[2(1+v)]):

La risposta trasversale è governata da due parametri adimensionali:

dove 2b è la larghezza attiva convenzionale e L la luce. Torna utile introdurre:

Interpretazione: α misura l'efficacia torsionale globale della piastra equivalente (da α=0, non torsiorigida, a α=1, isotropa, mentre θ lega la snellezza trasversale alla geometria (b/L) e al rapporto di rigidezze ρ/.

Sviluppo modale dei carichi lungo la luce

Assumendo appoggi semplici in direzione longitudinale, il carico p(x) è sviluppato in serie di Fourier sinusoidale:

Per le classi di azioni tipiche:

- Carico Concentrato P in x c:

(se x c =L/2, sopravvivono i soli modi dispari).

- Uniforme parziale p su [x_1,x_2]:

- Uniforme su tutta la luce q (modi dispari):

Ripartizione trasversale: funzioni k 0, k 1 e interpolazione k α

L'effetto in una trave posta alla coordinata "y" di un'azione applicata alla coordinata trasversale "e" si ottiene moltiplicando l'ampiezza modale a m(e) per un coefficiente di ripartizione k(y,e) che dipende da ϑ,α e da (y,e).

Caso non torsiorigido (α=0): k 0

Si definiscono (con lo scambio di segni per garantire e>y in termini di formula):

- Denominatore e prefattore:

- Argomenti:

- Blocchi elementari:

- Coefficiente:

Caso isotropo (α=1): k 1

Con

Interpolazione torsionale: k α

Si adotta una interpolazione non lineare fra i due limiti:

con esponente η ϑ calibrato (ad es. η=0.05 per ϑ \leq0.10; η=0.50 per 1.00 \leq ϑ \leq2.00; altrimenti:

Ricostruzione della risposta della piastra e proiezione su travi

Per il modo m si pone α m =m π/L. La deformata modale della piastra è:

La pesante penalizzazione α m ^-4 garantisce la convergenza per m elevati.

Per i diagrammi 1D lungo la luce (schema SS), i contributi modali sono:

Con più corsie, le azioni si sommano e si definisce la media per trave:

La pesatura trasversale verso la trave in yj usa le risultanti lineari di ciascuna corsia con P(e):

e quindi

Limiti e condizioni di cautela

- Impalcati curvi o con planimetria complessa: la formulazione GMB nasce per impalcati rettilinei; su tracciati in curva (o con variazioni brusche di direzione) la ripartizione può risultare non rappresentativa senza correzioni dedicate o un modello FEM di piastra/trave.

- Cassoni molto rigidi a torsione (alti α): per ponti a cassone chiuso/mono-cellula/multi-cellula, la rigidezza torsionale può alterare sensibilmente la ripartizione; la sola interpolazione k α può richiedere calibrazione o l'impiego di estensioni specifiche.

- Variazioni marcate lungo la luce: cambi di spessore della soletta, irrigidimenti localizzati, diaframmi irregolari, travi con inerzia variabile in modo significativo, l'assunzione di rigidezze costanti perde accuratezza; raccomandata una segmentazione o una validazione FEM.

- Sbalzi laterali estesi: se la larghezza a sbalzo è grande e la sua efficacia non è ridotta in b, il metodo può sovrastimare la partecipazione degli sbalzi; opportuno introdurre coefficienti di efficacia dedicati.

Tool
