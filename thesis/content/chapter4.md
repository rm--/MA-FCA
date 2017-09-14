# Anforderungen an die Arbeit
\label{chapter:requirements}

Es werden die Resultate der geführten Interviews ausgewertet und bestehende Möglichkeiten für die Nutzer auf Star-Exec-Presenter untersucht. Zusammenfassend wird eine Spezifikation an die Arbeit formuliert.

------

\vspace{-.7cm}

## Interview

Es ist die Aufgabe mit Formaler Begriffsanalyse Star-Exec-Presenter zu verbessern. Um herauszufinden an welchen Stellen diese Methode eine Verbesserung erzielen kann, wird mit den Nutzern ein Interview geführt.

### Fragen

Das Interview deckt drei Aspekte ab um die Nutzer und deren Wünsche einordnen zu können.
Im Anhang \ref{Anhang_Interview_Fragen} sind alle Fragen an die drei freiwilligen Interview-Partner aufgelistet. Hier wird nur eine Auswahl vorgestellt:

**1. Verhalten auf der Webseite**

  a. Für was nutzen Sie Star-Exec-Presenter?
  b. Was ist die von Ihnen am meisten genutzte Route? (Angabe von Bsp.-URL hilfreich)

**2. benötigte Verbesserungen**

  a. Welche Darstellungsformen, würden bei der Visualisierung, Analyse, Navigation zwischen den Seiten, den Analyse-Daten helfen?
  b. Welche Features/Funktionen wären hilfreich, um konkurrierende Solver besser zu analysieren?
  c. Welche Informationen werden benötigt und sind derzeit nicht in Star-Exec-Presenter eingebaut?

**3. Meinung zu eigenen Verbesserungsvorschlägen**

  a. Sollte es eine Navigationsleiste/Menü geben?
  b. Sollte es einen Filter für die Job-Tabelle geben?
  c. Würde Pagination auf den Seiten helfen? Wieviele Zeilen sollte pro Seite angezeigt werden? (10, 20, 50)
  d. Sollte es mehr Informationen zu einem Benchmark geben?
  e. Sollte es einen Vergleich von Benchmarks/Solvern (+ Config)/Termination-Competition geben?


### Antworten

Die Antworten der einzelnen Nutzer werden zusammengefasst dargestellt. In Kapitel \ref{subchapter:spec} wird geprüft, welche der Features mit Formaler Begriffsanalyse oder innerhalb des Rahmens der Masterarbeit umgesetzt werden können.
Im Anhang \ref{Anhang_Interview_Antworten} befindet sich eine ausführlichere Zusammenfassung der Antworten der Interview-Teilnehmer.

**1. Verhalten auf der Webseite**

  a. Betrachtung Competition-Ergebnisse, Analyse der einzelnen Competition-Kategorien, Beobachtung der Tool-Entwicklung, Anzahl gelöste Benchmarks
  b. Competition-Übersicht, Job-Resultate, *Flexible Table*


**2. benötigte Verbesserungen**

  a. Übersichtstabellen mit Durchschnittszeiten, Plots
  b. verbesserte Übersichtlichkeit
  c. mehr Informationen über die Benchmarks (Inhalt, Größe, Anzahl Zeilen)


**3. Meinung zu eigenen Verbesserungsvorschlägen**

  a. zwei Zustimmungen, eine Enthaltung
  b. drei Zustimmungen
  c. drei Ablehnungen
  d. zwei Zustimmungen, eine Enthaltung
  e. einmal kein Vergleich von Benchmarks, eine Enthaltung und eine generelle Zustimmung

## Bestandsaufnahme

Es werden die bestehenden Routen mit ihren Möglichkeiten und Problemen vorgestellt. Die Gründe dafür sind das Verstehen der Nutzer-Wünsche und -Probleme. Außerdem soll es keine Implementierung zu bereits gelösten Problemen geben. Die gesamte Bestandsaufnahme bezieht sich auf die Star-Exec-Presenter-Version mit dem Commit cc84625\footnote{\url{https://github.com/jwaldmann/star-exec-presenter/commit/cc846252ac57899c163bf6e0270e334129047013}} vom 7. August 2015.

\newpage

### Competition-Übersicht
\label{subchapter:comp-overview}

**Beschreibung**

Die Competition-Übersicht zeigt die Competition-Resultate bezüglich eines Jahres.

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/SEP_term_comp_2015.pdf}
  \caption{Star-Exec-Presenter: Competition-Übersicht 2015 (Begrenzung auf Metakategorie \textit{Termination of Term Rewriting (and Transition Systems)})}
  \small{\url{http://nfa.imn.htwk-leipzig.de/termcomp-2015/competitions/4}}
  \label{img:SEP_CO_9515}
\end{figure}

\vspace{-.3cm}

**Möglichkeiten**

* Ermittlung des Scorings der Solver in einer Kategorie oder Metakategorie.
* Zu jeder Competition können teilnehmende Solver, der Post-Prozessor und Statistik-Werte wie Anzahl JobPairs, CPU-Zeit und WallclockTime angesehen werden.

**Probleme**

* Es werden sehr viele Daten dargestellt.

\newpage

### Show Many Job Results
\label{subchapter:smjr}

**Beschreibung**

Die Route *Show Many Job Results* zeigt ein einfaches Listing der Resultate eines oder mehrerer Jobs. Die Resultate eines Jobs werden in Form einer Tabelle visualisiert. Dabei sind alle teilnehmenden Solver (Kapitel \ref{subchapter:solver}) in Spalten und die zu lösenden Benchmarks der TPDB (Kapitel \ref{subchapter:tpdb}) in Zeilen dargestellt. Eine Zelle eines JobPairs (Paar aus Solver und Benchmark) zeigt die verschiedenen Resultatdaten (Kapitel \ref{subchapter:result-data}) CPU-Zeit, (Wanduhr-)Zeit und als Zellen-Farbe\footnote{Hellgrün=YES,Dunkelgrün=NO,Gelb=MAYBE} die Antwort. Zum Vergleich von mehreren Jobs müssen die JobIds in der URL mit einem Slash voneinander getrennt angegeben werden.

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/SEP_9515.pdf}
  \caption{Star-Exec-Presenter: Show Many Job Results 9515}
  \small{\url{http://nfa.imn.htwk-leipzig.de/termcomp-devel/results/standard/noquery/9515}}
  \label{img:SEP_SMJR_9515}
\end{figure}


**Möglichkeiten**

* Anzeigen der Resultat-Daten.
* Vergleich von mehreren Job-IDs.

**Probleme**

* Mit steigender Anzahl von Benchmarks und Solvern steigt die Anzahl der Zeilen und Spalten. Damit wächst auch die Unübersichtlichkeit.
* Es ist kein Filtern der Daten möglich.

### Flexible Table
\label{subchapter:flexible-table}

**Beschreibung**

Die *Flexible Table* bietet Filterungen der Resultat-Tabelle durch eine \gls{Query} an. Als Beispiel werden alle Benchmarks herausgefiltert, die von allen Solvern gelöst wurden. In Abbildung \ref{img:SEP_FT_9515_input} wird dazu im Zeilenfilter die Zeile ausgewählt in der beide Solver mit `YES` geantwortet haben. Die ursprüngliche Tabelle (Abbildung \ref{img:SEP_SMJR_9515}) wird auf die zwei `JobPairs` mit `YES` reduziert. Abbildung \ref{img:SEP_FT_9515_output} zeigt die gefilterte Resultat-Tabelle.

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/SEP_9515_flexible_table_choose.pdf}
  \caption{Star-Exec-Presenter: Flexible Table Auswahl von Spalten- und Zeilenfiltern 9515}
  \small{\url{http://nfa.imn.htwk-leipzig.de/termcomp-devel/flexible-table/Query%20%5B%5D/9515}}
  \label{img:SEP_FT_9515_input}
\end{figure}

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/SEP_9515_flexible_table_result.pdf}
  \caption{Star-Exec-Presenter: Flexible Table Resultat 9515}
  \small{\url{http://nfa.imn.htwk-leipzig.de/termcomp-devel/flexible-table/Query%20%5B%5D/9515}}
  \label{img:SEP_FT_9515_output}
\end{figure}

**Möglichkeiten**

* Dem Nutzer wird das Filtern der gesamten Resultat-Tabelle bezüglich der Solver-Antworten ermöglicht.
Es gibt Filter für Spalten und Zeilen. Konkret lässt sich die Tabelle auf eine Solver-Antwort-Kombination YES-MAYBE in einer Zeile oder Zellen mit YES in einer Spalte beschränken. Die Negation der Auswahl ist auch möglich.

**Probleme**

* Die Vielzahl an möglichen Solver-Antwort-Kombinationen macht die Seite unübersichtlich.
* Nach mehrfacher Auswahl dieser Selektoren für Zeilen oder Spalten verlängert sich die Seite und der Nutzer erhält weitere Möglichkeiten.
* Es besteht Redundanz mit *Show Many Job Results*.

\newpage

## Spezifikation
\label{subchapter:spec}
Aus den Interview-Antworten und bestehenden Lösungen wird die Spezifikation erarbeitet.

Die Haupt-Anwendung für Nutzer von Star-Exec-Presenter ist das Betrachten und Analysieren der Competition-Resultate.
Demnach sollte die Implementierung diese Anwendung unterstützen. Die Analyse der Resultat-Daten beinhaltet Vergleiche von Solvern/Konfigurationen bezüglich einer Auswahl von Benchmarks aus der TPDB \ref{subchapter:tpdb}. Es können auch ganze Competitions untereinander vergleichen werden.

Für Star-Exec-Presenter werden die folgenden Features gewünscht:

1. Einschränkung der Datenmenge durch Filter
2. mehr Übersichtlichkeit, besserer Zugang zu Informationen
3. eine verbesserte Navigation (z.B.: durch ein Menü)
4. mehr Informationen über die Benchmarks (Inhalt, Größe, Anzahl Zeilen)
5. Übersichtstabellen, Plots

Mit der Route *Show Many Job Results* (Kapitel \ref{subchapter:smjr}) ist es möglich verschiedene Jobs zu vergleichen. Bei diesen Vergleichen empfängt der Nutzer eine Menge von Daten. In der größten Kategorie *TRS Standard* aus dem Jahre 2015 treten sieben Solver in 1498 Benchmarks gegeneinander an und bilden im Produkt 10486 Resultate\footnote{\url{http://nfa.imn.htwk-leipzig.de/termcomp-2015/results/standard/noquery/10257}}. Wenn der Nutzer diese Competition noch mit weiteren Competitions vergleichen möchte, ist es nachvollziehbar, weshalb ein Filter zur Einschränkung der Datenmenge ein Feature-Wunsch ist. Die Route *Flexible Table* (Kapitel \ref{subchapter:flexible-table}) erfüllt die Anforderungen an einen Filter, jedoch nur in Bezug auf die Solver-Antwort. Die *Competition-Übersicht* (Kapitel \ref{subchapter:comp-overview}) erfüllt den Zweck des rudimentären Listings der Resultate verschiedener Kategorien. Dieses Listing berührt den Kontext der Analyse nicht und steht außerhalb des Rahmens der Arbeit.

Mit dem Einsatz von Formaler Begriffsanalyse ist es denkbar die Features 1 und 2 zu erfüllen. Durch die Bestimmung des \glslink{Begriffsverband}{Begriffsverbandes} entstehen Teilmengen von Zellen der Resultat-Tabelle, deren Inhalt angezeigt werden kann. Wenn weniger Zellen als Umfang eines Begriffes angezeigt werden, erfüllt dies den zweiten Feature-Wunsch.

Im Rahmen der Visualisierung und Verbesserung der Übersichtlichkeit soll eine Navigationsleiste hinzugefügt werden. Weitere Querverbindungen zwischen den Routen sollen die Navigation im Allgemeinen und gleichzeitig auch die Übersicht fördern.

Die Feature-Wünsche 4. und 5. überschneiden sich nicht mit dem Themenkomplex der Arbeit und werden nicht betrachtet.

Zusammenfassend soll die Implementierung die Features 1-3 erfüllen. Mithilfe Formaler Begriffsanalyse soll ein weiterer Filter für die Resultat-Tabelle erstellt werden. Eine Navigationsleiste und Verbindungen der Routen untereinander sollen die Navigation und auch den Zugang zu Informationen erleichtern.

Die Spezifikation ist erfüllt wenn die Zielsetzungen umgesetzt wurden. Weiter soll geprüft werden, ob die Implementierung für die Nutzer von Star-Exec-Presenter hilfreich ist. Zur Evaluation wurden die Interview-Partner erneut angeschrieben, die Features beschrieben und präsentiert. Die Auswertung befindet sich im Kapitel \ref{chapter:evaluation}.
