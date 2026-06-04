---
layout: post
title: "Analisi dinamica del passaggio del treno su un ponte"
description: "Articolo tecnico StruHub con immagini e passaggi di calcolo."
date: 2026-01-22
order: 22
permalink: /posts/analisi-dinamica-del-passaggio-del.html
meta: "Archivio StruHub · 313 parole circa"
---

**Impostazione tecnica**

Quando un treno attraversa un ponte, il sistema non è statico, il carico si muove e induce vibrazioni. Questo fenomeno è descritto dall' analisi dinamica, fondamentale per rispettare le normative (Eurocodici, UNI EN, ecc.) e garantire sicurezza.

![Immagine tecnica StruHub]({{ site.baseurl }}/assets/images/analisi-dinamica-del-passaggio-del-01.jpg)

**Formule di riferimento**

$$
\n\nm \frac{\partial^2 w(x,t)}{\partial t^2} + c \frac{\partial w(x,t)}{\partial t} + EI \frac{\partial^4 w(x,t)}{\partial x^4} = P \cdot \delta(x - vt)\n\n
$$

$$
f_1 = \frac{\pi^2}{2 L^2} \sqrt{\frac{EI}{m}}
$$

$$
v_c = f_1 \cdot s
$$

$$
\Phi = 1 + \frac{\alpha v}{\beta + v}
$$

$$
\Phi = \frac{1}{\sqrt{\left(1 - \left(\frac{v}{v_c}\right)^2\right)^2 + \left(2 \zeta \frac{v}{v_c}\right)^2}}
$$

$$
f_{\text{eccitazione}} = \frac{v}{s}
$$

Modello teorico: trave semplicemente appoggiata

Consideriamo una trave di luce L, massa per unità di lunghezza mmm, rigidezza EI, attraversata da un carico P che si muove con velocità costante v.

L'equazione del moto è:

Dove:

- w(x,t) = spostamento verticale

- c = coefficiente di smorzamento

- δ(xˆ’vt) = funzione di Dirac che rappresenta il carico in movimento

Frequenza propria e velocità critica

La prima frequenza naturale della trave:

La velocità critica (risonanza con il passo dei carrelli):

dove s è il passo tra gli assi del treno.

Coefficiente di amplificazione dinamica (Φ)

Le normative forniscono formule semplificate, ad esempio:

oppure, in forma teorica:

dove:

- ζ = coefficiente di smorzamento

- vc = velocità critica

Questa è la classica curva di risonanza: Φ cresce fino a un picco vicino a vc, poi decresce.

![Immagine tecnica StruHub]({{ site.baseurl }}/assets/images/analisi-dinamica-del-passaggio-del-02.jpg)

Cosa prevedono le normative ferroviarie

Le normative non si limitano a imporre un limite sul coefficiente di amplificazione dinamica (tipicamente Φ \leq 1,67), ma stabiliscono anche vincoli sulla frequenza naturale del ponte per ridurre il rischio di risonanza.

Limiti sulla frequenza propria

Secondo EN 1991-2 (Eurocodice):

- La prima frequenza verticale del ponte deve essere maggiore di 3 Hz per ponti ferroviari ad alta velocità.

- Per ponti di luce maggiore, il limite può essere più basso, ma mai inferiore a circa 1,5 Hz.

Questi valori derivano dal fatto che:

- La frequenza di eccitazione del treno è legata al passo dei carrelli e alla velocità:

dove v è la velocità del treno e s il passo tra gli assi.

- Se feccitazione ≈ f1 (frequenza propria del ponte), si ha risonanza, con amplificazione dinamica elevata.
