---
title: Analyse und Visualisierung formaler Konzepte am Beispiel von ...
author: René Muhl
documentclass: scrbook
classoption:
   - 11pt
   - a4paper
   - twoside
   - bibliography=totoc
fontsize: 11pt
spacing: onehalfspacing
geometry:
    - left=3cm
    - right=2.5cm
    - top=3.5cm
    - bottom=3cm
    - headsep=1.5cm
header-includes:
    - \usepackage{tocbibind}
    - \usepackage{tocloft}
    - \renewcommand{\contentsname}{Inhaltsverzeichnis}
    - \renewcommand{\listfigurename}{Abbildungsverzeichnis}
    - \renewcommand{\listtablename}{Tabellenverzeichnis}
biblio-files: content/bib/references.bib
biblio-style: alphabetic
biblio-title: Literaturverzeichnis
toc: true
lof: false
lot: false
include-before:
    - content/include/titlepage.tex
    - content/include/abstract.tex
    - content/include/acknowledgements.tex

chapterheadstartvskip: -1cm
chapterheadendvskip: 1cm
scrlayer-scrpage:
  - \clearpairofpagestyles
  - \cfoot[\pagemark]{\pagemark}
  - \lehead{\headmark}
  - \rohead{\headmark}
  - \pagestyle{scrheadings}
---
