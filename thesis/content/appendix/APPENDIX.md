# Anhang
## Anforderungen an die Arbeit: Interview-Fragen\label{Anhang_Interview_Fragen}

**allgemein**

* Für was nutzen Sie Star-Exec-Presenter?
    * Betrachtung der Termination-Competition-Ergebnisse
    * Analyse der Resultate einzelner Kategorien
    * Sonstiges
* Was ist die von Ihnen am meisten genutzte Route? (Angabe von Bsp.-URL hilfreich)
* Skizzieren Sie einen typischen Besuch der Seite bis zur gesuchten Informationen.

* Welche guten/schlechten Eigenschaften hat das query interface \url{http://nfa.imn.htwk-leipzig.de/termcomp-devel/results/standard/Query%20%5BFilter_Rows%20%28And%20%5BEquals%20%22nothing%22,Equals%20%22solver-maybe%22,Equals%20%22solver-maybe%22,Equals%20%22solver-maybe%22,Equals%20%22solver-no%22,Equals%20%22solver-maybe%22,Equals%20%22solver-maybe%22%5D%29%5D/10296} ?
* Was sind die Probleme mit der derzeitigen Darstellung?

* Welche Darstellungsformen, wuerden bei der
    * Visualisierung
    * Analyse
    * Navigation zwischen den Seiten, den Analyse-Daten
helfen?
* Welche Informationen werden benötigt und sind derzeit nicht in star-exec-presenter eingebaut?

* Welche Features/Funktionen wären hilfreich, um konkurrierende Solver besser zu analysieren?
* Was sind interessante Merkmale eines Job-Pairs?
* Gibt es Merkmale eines JobPairs, die wichtiger sind als andere? Kann man eindeutige Prioritäten vergeben?
    * Solvername
    * Solver-Konfiguration
    * Solver-Resultat (YES, NO, MAYBE)
    * CPU-Zeit
    * Wallclock-Zeit
    * Informationen ueber den gelösten Benchmark


**konkrete Ansätze**

* Sollte es eine Navigationsleiste/Menu geben?
* Sollten die Tabellenspalten sortierbar sein?
* Sollte es einen Filter fuer die Job-Tabelle geben?
* Sollte es eine Suche fuer Job, JobPair, Solver, Benchmark geben?
* Wuerde Pagination auf den Seiten helfen? Wieviele Zeilen sollte pro Seite angezeigt werden?
    * 10
    * 20
    * 50

* Sollte es eine zusammenfassende Tabelle zu einem Job geben? Was sollte diese beinhalten?
* Sollte es mehr Informationen zu einem Benchmark geben?
* Sollte es einen Vergleich von
    * Benchmarks
    * Solvern (+ Config)
    * Termination-Competition
geben?
* Wenn ja, unter welchen Kriterien sollten diese verglichen werden?

* Sollten die Resultate zu einem Job exportierbar sein?
    * Wenn ja welches Format wird bevorzugt? CSV, XML, JSON, andere?

\newpage

## Anforderungen an die Arbeit: Interview-Antworten \label{Anhang_Interview_Antworten}

**Nutzung SEP**

* Betrachtung Competition-Ergebnisse
* Analyse der einzelnen Competition-Kategorien
* Beobachtung der Tool-Entwicklung
* Identifikation von YES/NO-Konflikten (gut gelöst)
* Regression: Vergleich mit anderen Versionen des Tools
* Vergleich mit anderen Tools


**Benutzte Routen**

* Competition-Übersicht
* Flexible Table für drill down in Kombinationen
* Competition-Ergebnis, Job-Resultate, Flexible Table nicht


**Gesuchte Information auf SEP**

* Benchmarks, bei denen sich das eigene Tool von Anderen unterscheidet
* Ort von Flexible Table
* Übersichtstabellen (Job-Result?)
* Suche nach unterschiedlichem Result (YES, MAYBE) von versch. Solver-Versionen
* Wieviele der Benchmarks werden vom eigenen Solver geschafft. YES/(Anz. Benchmarks)


**Eigenschaften Flexible-Table Query-Interface**

* nicht bekannt, dass man eigene Queries absenden kann
    * (fehlende Dokumenation für die Features der Seite)
* vorhandene Funktionalität ist gut
* "solver-yes" sollte eher nur "yes" heißen
* In langen Tabellen wuerden Zwischenueberschriften / 'mit-laufende tabellen header' sehr helfen (s.u. 2.Link)
* Nutzungsmöglichkeiten unbekannt
* Fetchen der Informationen verwirrt, Nutzer weiß nicht, das man nur Warten muss


**gewünschte Features**

* Inhalt des Benchmarks
* C-Programm? XML? (s.u. 1.Link)
* direkte Darstellung von Prover-Inputs (fuer text-only formate) wäre schoen
* komplette Ausgabe des Tools (Solvers?)
* weitere Eigenschaften (Größe, Anzahl der Zeilen) eines Benchmarks
* Sortierung nach Benchmark-Eigenschaften
* Summe der Laufzeit pro Solver für die Benchmarks, die von allen Solvern gelöst wurden
* ein "Show examples with |tool1.runtime - tool2.runtime| > k"-Feature
* Übersichtstabellen mit Durchschnittszeiten
* Plots: Scatterplots, Cactusplots, ... (z.B. Scatterplot in den man reinzoomen kann, und wo die Punkte ein mouseover haben, das Details anzeigt)
* Übersicht proofs
* Übersicht selbst abgefeuerte Queries/interessante Resultatmengen
* Übersichtlichkeit und intuitive Bedienbarkeit von (s.u. 3.Link) war gut


**Navigationsleiste**

* keine Präferenz
* 2x ja


**Sortierung für Tabellenspalten**

* 3x ja


**Filter für Tabellen**

* 3x ja


**Suche für Job, JobPair, Solver, Benchmark**

* keine Notwendigkeit
* nein


**Pagination**

* 2x nein
* keine bzw. default alle Ergebnisse anzeigen


**Zusammenfassende Tabelle pro Job**

* Anzahl (von was?)
* Count YES/NO/MAYBE/ERROR
* 2x avg. time for YES/NO/MAYBE/ERROR (s.u. 3.Link)
* à la (s.u. 3.Link)


**Zusätzliche Informationen zu einem Benchmark**

* Dateigröße und/oder Anzahl Zeilen


**Vergleich von Benchmarks/Solver/Competition**

* ja
* Vergleichskriterien: Anzahl gelöste Benchmarks
* nein, ja, ja


**Export für Star-Exec-Presenter**

* StarExec CSV-Export ausreichend
* CSV-Export gewünscht


**Links**

1. \url{http://sv-comp.sosy-lab.org/2016/results/results-verified/Termination.table.html}
2. \url{http://stackoverflow.com/questions/17584702/how-to-add-a-scrollbar-to-an-html5-table}
3. \url{http://termcomp.uibk.ac.at/termcomp/competition/categoryResults.seam?cat=10235&comp=15991&cid=368094}
4. \url{http://cl-informatik.uibk.ac.at/software/tct/experiments/tct2/RaML/index.php#details}

\newpage

## Auswertung: Einzelmesswerte Begriffsverband \label{appendix:tab:measurement-detail}

\begin{table}[h]
\centering
\begin{tabular}{ccccccc}
\hline
JobID & Messwert 1 & Messwert 2 & Messwert 3 & Messwert 4 & Messwert 5 & $\varnothing$-Zeit in Sekunden \\ \hline
9515  & 0,09 & 0,07 & 0,08 & 0,06 & 0,09 & 0,08            \\
10299  & 1,01 & 1,06 & 1,04 & 1,03 & 1,06 & 1,04            \\
10257  & 16,66 & 17,31 & 17,33 & 17,25 & 16,70 & 17,05       \\\hline
\end{tabular}
\caption{Einzel-Messwerte für die Jobs 9515, 10299 und 10257 (alle Messwerte inklusive $\varnothing$-Zeit in Sekunden)}
\label{tab:measurement-detail}
\end{table}


## Auswertung: Code-Metriken Star-Exec-Presenter \label{appendix:tab:code}

```
Total number of files: 169
Total number of lines: 12,367
Total number of commits: 712
+--------------------------+-------+---------+-------+--------------------+
| name                     | loc   | commits | files | distribution       |
+--------------------------+-------+---------+-------+--------------------+
| Stefan von der Krone     | 5,872 | 212     | 108   | 47.5 / 29.8 / 63.9 |
| Johannes Waldmann        | 4,078 | 260     | 93    | 33.0 / 36.5 / 55.0 |
| René Muhl                | 1,467 | 182     | 67    | 11.9 / 25.6 / 39.6 |
| stefanvonderkrone@gmx.de | 777   | 17      | 27    | 6.3 / 2.4 / 16.0   |
| rm--                     | 113   | 33      | 8     | 0.9 / 4.6 / 4.7    |
| vagrant                  | 60    | 7       | 3     | 0.5 / 1.0 / 1.8    |
| stefanvonderkrone        | 0     | 1       | 0     | 0.0 / 0.1 / 0.0    |
+--------------------------+-------+---------+-------+--------------------+
```
