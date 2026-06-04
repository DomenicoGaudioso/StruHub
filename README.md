# StruHub

Quaderno tecnico pubblico di ingegneria strutturale digitale.

Il sito raccoglie articoli su ponti, geotecnica, calcestruzzo armato, acciaio, sezioni composte, modellazione numerica e monitoraggio.

## Gestione contenuti

Il sito è gestito con GitHub Pages e Jekyll.

- `index.md`: pagina principale e indice automatico degli articoli.
- `about.md`: pagina Metodo.
- `_posts/`: articoli in Markdown.
- `_layouts/`: template Jekyll.
- `_includes/mathjax.html`: caricamento MathJax per le formule LaTeX.
- `assets/`: logo, immagini e stile.

Ogni articolo in `_posts` usa un permalink del tipo `/posts/nome-articolo.html`, così i vecchi link restano validi anche se la sorgente è Markdown.

Le formule sono scritte in LaTeX e renderizzate come equazioni tramite MathJax.
