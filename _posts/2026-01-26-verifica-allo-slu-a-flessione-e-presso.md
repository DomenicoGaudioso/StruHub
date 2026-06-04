---
layout: post
title: "Verifica allo SLU a flessione e presso-flessione nelle sezione in c.a."
description: "Articolo tecnico StruHub con immagini e passaggi di calcolo."
date: 2026-01-26
order: 26
permalink: /posts/verifica-allo-slu-a-flessione-e-presso.html
meta: "Archivio StruHub · 660 parole circa"
---

**Impostazione tecnica**

Nel progetto strutturale in calcestruzzo armato, la verifica allo Stato Limite Ultimo (SLU) rappresenta il momento decisivo: è qui che accertiamo che la sezione sia in grado di resistere alle azioni di progetto senza collassare.

In questo articolo analizziamo:

- la verifica a flessione semplice

- la verifica a presso-flessione retta

- il significato fisico delle formule normative

- esempi pratici di calcolo implementati in Python, con un approccio coerente con librerie tipo ConcreteSection

![Immagine tecnica StruHub]({{ site.baseurl }}/assets/images/verifica-allo-slu-a-flessione-e-presso-01.jpg)

**Formule di riferimento**

$$
( \frac{ M_{xRd}}M_{xEd} )^\alpha+(\frac{ M_{yRd}}M_{yEd} )^\alpha \le 1
$$

Ipotesi di base allo SLU

Le verifiche a flessione e presso-flessione si basano su alcune ipotesi fondamentali:

- Ipotesi di Bernoulli: Le sezioni piane restano piane dopo la deformazione.

- Leggi costitutive di progetto: Ad esempio:

- Calcestruzzo: diagramma parabola-rettangolo

- Acciaio: elastico-perfettamente plastico

- Stato tensionale ultimo

Il calcestruzzo lavora solo a compressione, l'acciaio anche a trazione.

Queste ipotesi consentono di passare dal problema reale a un equilibrio di forze e momenti.

Il Modello resistente

In flessione semplice:

- il calcestruzzo compresso fornisce una risultante C

- l'armatura tesa fornisce una risultante T

- l'equilibrio è garantito da: C=T

Il momento resistente ultimo è: MRd=T‹…z

dove z è il braccio interno.

![Progetto di una sezione in c.a. sottoposta a flessione...]({{ site.baseurl }}/assets/images/verifica-allo-slu-a-flessione-e-presso-02.jpg)

Secondo §4.1.2.1.3 NTC, in assenza (o quasi) di sforzo normale:

MEd\leqMRd

dove:

- MEd è il momento sollecitante di progetto

- MRd è il momento resistente ultimo della sezione

Il valore di MRd deve essere determinato con modelli coerenti con le leggi costitutive di progetto.

La Verifica a presso-flessione retta

Quando è presente anche uno sforzo normale N, la situazione diventa più interessante.

A seconda dell'entità di N, cambiano:

- posizione dell'asse neutro

- stato tensionale dell'armatura

- contributo del calcestruzzo

È per questo che la norma introduce il dominio di interazione N-M.

![A simple method for N-M interaction diagrams of circular reinforced concrete cross sections | Engissol Ltd.- Structural Engineering Software]({{ site.baseurl }}/assets/images/verifica-allo-slu-a-flessione-e-presso-03.jpg)

Verifica allo SLU a presso-flessione deviata (NTC 2018)

La presso-flessione deviata la sezione è soggetta contemporaneamente a:

- sforzo normale N;

- momento flettente Mx;

- momento flettente My;

È il caso tipico di:

- pilastri in c.a.

- setti

- travi soggette ad azioni eccentriche

Il riferimento normativo è sempre il §4.1.2.1.3 - Verifiche delle sezioni.

Le NTC stabiliscono che: La verifica allo SLU di una sezione soggetta a sforzo normale e momenti flettenti in due direzioni deve essere condotta tenendo conto dell'interazione tra N, Mx e My, verificando che le azioni di progetto non superino le resistenze della sezione.

(NEd, MxEd, MyEd)ˆˆDominio resistente della sezione

La norma non impone una formula chiusa, ma richiede che:

- siano rispettate le leggi costitutive di progetto

- sia garantito l' equilibrio globale della sezione

- sia verificata la compatibilità delle deformazioni

Per sezioni ordinarie, le NTC ammettono anche una verifica approssimata, del tipo:

con:

- α generalmente compreso tra 1 e 2

- Mx,Rd ed My,Rd valutati in presenza dello stesso sforzo normale NEd

La costruzione dei domini tramite python, la libreria ConcreteSection

concreteproperties è un pacchetto Python che può essere utilizzato per calcolare le proprietà della sezione di sezioni in cemento armato, eseguire analisi momento -curvatura e generare diagrammi di interazione. https://robbievanleeuwen.github.io/concrete-properties /

Import delle librerie necessarie import numpy as np

from concreteproperties.material import Concrete, SteelBar

from concreteproperties.stress_strain_profile import (

ConcreteLinear,

RectangularStressBlock,

SteelElasticPlastic,

)

from sectionproperties.pre.library.concrete_sections import concrete_rectangular_section

from concreteproperties.concrete_section import ConcreteSection

from concreteproperties.results import MomentInteractionResults Definizione dei Materiali concrete = Concrete(

name="32 MPa Concrete",

density=2.4e-6,

stress_strain_profile=ConcreteLinear(elastic_modulus=30.1e3),

ultimate_stress_strain_profile=RectangularStressBlock(

compressive_strength=32,

alpha=0.802,

gamma=0.89,

ultimate_strain=0.003,

),

flexural_tensile_strength=3.4,

colour="lightgrey",

)

steel = SteelBar(

name="500 MPa Steel",

density=7.85e-6,

stress_strain_profile=SteelElasticPlastic(

yield_strength=500,

elastic_modulus=200e3,

fracture_strain=0.05,

),

colour="grey",

) Creazione della sezione geom = concrete_rectangular_section(

b=400,

d=600,

dia_top=20,

n_top=3,

dia_bot=24,

n_bot=3,

n_circle=4,

cover=30,

area_top=310,

area_bot=450,

conc_mat=concrete,

steel_mat=steel,

)

conc_sec = ConcreteSection(geom)

conc_sec.plot_section() Momento d'interazione geom = concrete_rectangular_section(

b=400,

d=600,

dia_top=20,

n_top=3,

dia_bot=24,

n_bot=3,

n_circle=4,

cover=30,

area_top=310,

area_bot=450,

conc_mat=concrete,

steel_mat=steel,

)

conc_sec = ConcreteSection(geom)

mi_res_pos = conc_sec.moment_interaction_diagram(progress_bar=False)

mi_res_neg = conc_sec.moment_interaction_diagram(theta=np.pi, progress_bar=False)

MomentInteractionResults.plot_multiple_diagrams(

moment_interaction_results=[mi_res_pos, mi_res_neg],

labels=["Positive", "Negative"],

fmt="-",

)

![Immagine tecnica StruHub]({{ site.baseurl }}/assets/images/verifica-allo-slu-a-flessione-e-presso-04.png)

Oppure, se vogliamo il diagramma 3D: n_list = np.linspace(0, 7400e3, 11)

biaxial_results = []

for n in n_list:

biaxial_results.append(conc_sec.biaxial_bending_diagram(n=n, progress_bar=False))

![Immagine tecnica StruHub]({{ site.baseurl }}/assets/images/verifica-allo-slu-a-flessione-e-presso-05.png)
