# Motivation
\label{chapter:motivation}

---

Bei der Termination-Competition treten verschiedene Computerprogramme (sogenannte \gls{Solver}) auf der Plattform StarExec gegeneinander an. In diesem Wettstreit muss eine Menge von Aufgaben (\glslink{Benchmark}{Benchmarks}) bezüglich ihrer Termination überprüft werden.

\gls{Termination} beschreibt das (An-)Halten eines Programmes auf eine Eingabe nach einer endlichen Anzahl von Arbeitsschritten. Das algorithmische Entscheiden der Termination von allen Programmen für alle Eingaben ist nicht möglich und findet seine Beschreibung durch das Halteproblem.

Aufgrund der Entwicklung neuer Techniken zum Beweisen der Termination in den neunziger Jahren wurden mehrere Programme für das vollautomatische Lösen von speziellen Aufgabeklassen entwickelt. Daraus entwickelte sich ein jährliches Event. Ab 2004 wurde diese \gls{Competition} vier Jahre am *Laboratoire de Recherche en Informatique (LRI)* in Paris und anschließend 2008 bis 2013
an der Universität Innsbruck durchgeführt. Der Wettstreit findet seit 2014 an der HTWK Leipzig statt, Organisator ist Prof. Waldmann. [@TCP15] Seitdem Stattfinden an der HTWK Leipzig dient Star-Exec-Presenter\footnote{\url{https://github.com/jwaldmann/star-exec-presenter}} als Präsentationschicht von StarExec. Diese Web-Applikation wurde durch Stefan von der Krone in Zusammenarbeit mit Prof. Waldmann entwickelt.

Die Termination eines Benchmarks kann von jedem Solver mit YES, NO oder MAYBE beantwortet werden. Die Antwort YES gibt an, dass der Solver einen Beweis für eine und NO - einen Beweis für keine Termination gefunden hat. Die Antwort MAYBE drückt aus, dass kein Beweis gefunden wurde. Zusätzlich zu dieser Antwort werden noch weitere Daten wie die Zeit, wie lange für dieses Ergebnis benötigt wurde, erfasst [@CompProcedure] [@TC].

Aufgrund der Vielzahl an Daten können nur wenige Informationen aus der Resultatsdarstellung der internationalen Termination-Competition 2015 gewonnen werden. In der größten Kategorie *TRS Standard* treten sieben Solver in 1498 Benchmarks gegeneinander an und bilden im Produkt 10486 Resultate\footnote{\url{http://nfa.imn.htwk-leipzig.de/termcomp-2015/results/standard/noquery/10257}}.

Ziel dieser Arbeit ist die Verbesserung der Visualisierung und Analyse dieser Resultat-Daten.
Zur Erreichung des Ziels soll Formale Begriffsanalyse genutzt werden, um die Menge der Resultat-Daten in Teilmengen zu zerlegen.
Die Formale Begriffsanalyse agiert innerhalb eines formalen Kontextes. Dieser Kontext besteht aus einer Menge von Gegenständen, einer Menge von Merkmalen und der Beziehung zwischen beiden. In einem \glslink{Formaler Kontext}{Formalen Kontext} werden \glslink{Formaler Begriff}{Formale Begriffe} bestimmt. Ein Formaler Begriff besteht aus einer Menge von Gegenständen und einer Menge von Merkmalen. Zwei Gegenstände gehören genau dann zum gleichen Begriff, wenn diese exakt die gleichen Merkmale teilen.

\begin{bsp}[Formale Begriffsanalyse]
Formaler Kontext $(\{1,2,3\}, \{blau,rot\},\{(1,rot),(2,blau),(3,rot)\})$ \\
Formale Begriffe $\{(\emptyset,\{blau,rot\}),(\{1,3\},\{rot\}),(\{2\},\{blau\}),(\{1,2,3\},\emptyset)\}$
\end{bsp}

Diese Mengen sollen die Analyse zwischen verschiedenen Solvern sowie den Vergleich der Ergebnisse vergangener Competitions vereinfachen. Aufgrund der Vielzahl von Daten ist es notwendig die Ergebnisse neu zu strukturieren. Durch eine geeignete Navigation innerhalb dieser, soll es möglich sein die vorliegenden Informationen besser zu untersuchen.

Die Stärken der Formalen Begriffsanalyse sind unter anderem das "Entfalten der Daten", indem die Struktur der Daten sichtbar und zugänglich gemacht wird \cite[S.6]{ApplLatticeT} \cite[S.49]{FCAAnwendung}. So können Zusammenhänge, Muster, Regelmäßigkeiten und Ausnahmen gefunden werden. \cite[S.6]{ApplLatticeT}

Als Vorbereitung auf diese Arbeit fand bereits ein Masterprojekt [@Masterprojekt] statt. In dieser Vorarbeit wurde die bestehenden Applikation, die Programmiersprache \gls{Haskell} und das Web-Framework \glslink{Yesod Web Framework}{Yesod} untersucht. Im Rahmen des Masterprojektes wurden die Grundlagen der Formalen Begriffsanalyse recherchiert und rudimentär implementiert.

Nach der Vorstellung der Termination-Competition einschließlich *StarExec*, *Termination Problems Data Base* und Star-Exec-Presenter folgt eine Einführung in die Formale Begriffsanalyse. Nachdem der thematische und theoretische Rahmen vorgestellt wurde, werden die Anforderungen an die Arbeit formuliert. In Kapitel \ref{chapter:haskell} wird auf die funktionale Programmiersprache Haskell eingegangen um in die Hauptkapitel Implementierung und Auswertung überzuleiten. Abschließende Worte sind in der Zusammenfassung zu finden.

*Anmerkung:* In der Arbeit werden die offiziellen StarExec-Bezeichnungen wie \gls{Competition}, \gls{Job}, \gls{JobPair} verwendet. Um Verwirrung zu vermeiden werden keine neuen Bezeichnungen eingeführt.
