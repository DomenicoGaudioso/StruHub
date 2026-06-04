# StruHub

StruHub è un quaderno tecnico pubblico dedicato all'ingegneria strutturale digitale.

Il progetto raccoglie articoli, appunti di calcolo e riferimenti metodologici su ponti, geotecnica, calcestruzzo armato, acciaio, sezioni composte, modellazione FEM, analisi dinamica, monitoraggio e automazione dei processi di verifica.

Sito pubblico: [domenicogaudioso.github.io/StruHub](https://domenicogaudioso.github.io/StruHub/)

## Obiettivo

L'obiettivo di StruHub è costruire una biblioteca tecnica consultabile nel tempo: ogni articolo deve spiegare il problema, esplicitare le ipotesi, mostrare le formule, collegare il modello fisico al modello numerico e chiudere con una lettura progettuale.

Il tono editoriale è tecnico-divulgativo: rigoroso nelle formule e nei riferimenti, ma scorrevole nella lettura.

## Struttura del sito

Il sito è pubblicato con GitHub Pages e generato con Jekyll.

- `index.md`: home page e indice automatico degli articoli.
- `about.md`: pagina sul metodo editoriale.
- `_posts/`: articoli in Markdown.
- `_layouts/`: template Jekyll per home, pagine e post.
- `_includes/mathjax.html`: configurazione MathJax per il rendering LaTeX.
- `assets/`: logo, immagini, manifest e foglio stile.
- `_config.yml`: configurazione GitHub Pages/Jekyll.

Gli articoli sorgente sono Markdown, ma mantengono permalink del tipo:

```text
/posts/nome-articolo.html
```

In questo modo i link pubblici restano stabili anche se i contenuti sono gestiti come pagine Markdown.

## Scrivere un articolo

Un nuovo articolo va creato in `_posts/` con nome:

```text
YYYY-MM-DD-slug-articolo.md
```

Esempio:

```text
_posts/2026-02-01-analisi-modale-impalcato.md
```

Ogni post deve iniziare con un front matter simile:

```yaml
---
layout: post
title: "Titolo tecnico dell'articolo"
description: "Breve descrizione del contenuto tecnico."
date: 2026-02-01
order: 27
permalink: /posts/analisi-modale-impalcato.html
meta: "Quaderno tecnico · 1500 parole circa"
---
```

Campi principali:

- `title`: titolo mostrato nella pagina e nell'indice.
- `description`: descrizione SEO e sintesi del contenuto.
- `date`: data Jekyll del post.
- `order`: posizione nell'indice della home.
- `permalink`: URL pubblico stabile.
- `meta`: testo breve mostrato nella card dell'articolo.

## Formule LaTeX

Le formule sono scritte direttamente in LaTeX e renderizzate con MathJax.

Formula inline:

```markdown
La snellezza può essere indicata come $\lambda = L/i$.
```

Formula display:

```markdown
$$
M\ddot{u}(t)+C\dot{u}(t)+Ku(t)=F(t)
$$
```

Per compatibilità con Jekyll/Kramdown, nel Markdown è preferibile usare `$...$` e `$$...$$`. MathJax è configurato anche per riconoscere `\(...\)` e `\[...\]`, ma i delimitatori dollar-style sono più robusti in questa struttura.

## Immagini

Le immagini degli articoli sono salvate in:

```text
assets/images/
```

E possono essere richiamate nei post con:

```markdown
![Descrizione tecnica dell'immagine]({{ site.baseurl }}/assets/images/nome-immagine.png)
```

La descrizione `alt` deve essere significativa, soprattutto quando l'immagine contiene schemi, diagrammi o risultati di calcolo.

## Metodo editoriale

Ogni articolo dovrebbe seguire questa logica:

1. Definire il problema tecnico.
2. Dichiarare ipotesi, grandezze e unità di misura.
3. Presentare il modello fisico.
4. Tradurre il modello in formule o schema numerico.
5. Evidenziare controlli dimensionali e ordini di grandezza.
6. Riportare riferimenti normativi o bibliografici quando vengono citati.
7. Chiudere con una lettura progettuale, non con una semplice esposizione di risultati.

I sottotitoli devono essere pochi e leggeri. La lettura deve restare continua: l'articolo non deve sembrare una scaletta di appunti, ma un riferimento tecnico ragionato.

## Riferimenti normativi

Quando un articolo cita norme, linee guida o Eurocodici, il riferimento deve essere scritto in modo esplicito.

Esempio:

```text
D.M. 17 gennaio 2018, Aggiornamento delle Norme tecniche per le costruzioni, Ministero delle Infrastrutture e dei Trasporti, Supplemento ordinario alla Gazzetta Ufficiale n. 42 del 20 febbraio 2018.
```

Oppure:

```text
UNI EN 1991-2:2005, Eurocodice 1 - Azioni sulle strutture - Parte 2: Carichi da traffico sui ponti.
```

## Pubblicazione

Il sito viene pubblicato automaticamente da GitHub Pages a ogni push sul branch `main`.

Repository:

```text
https://github.com/DomenicoGaudioso/StruHub
```

URL pubblico:

```text
https://domenicogaudioso.github.io/StruHub/
```

## Note tecniche

- Il sito usa Jekyll con `kramdown` e input GitHub Flavored Markdown.
- Le formule sono renderizzate lato browser con MathJax.
- I vecchi URL degli articoli HTML sono preservati tramite permalink.
- La home genera automaticamente l'elenco degli articoli ordinandoli tramite il campo `order`.

## Licenza e uso

I contenuti sono pensati come materiale tecnico-divulgativo. Prima di riutilizzare testi, immagini o schemi in altri contesti, verificare sempre paternità, fonti e riferimenti normativi applicabili.
