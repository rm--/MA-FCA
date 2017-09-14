# Termination-Competition
\label{chapter:termcomp}

Die Termination-Competition ist ein Wettbewerb zur automatischen Analyse der Termination von Problemen der *Termination Problems Data Base*. Solver untersuchen diese Probleme sogenannte Benchmarks und versuchen deren Termination zu beweisen. Seit dem Jahre 2014 findet diese Competition auf der Plattform StarExec statt. Die Ergebnisse werden auf Star-Exec-Presenter zusammengefasst und präsentiert. [@TCP15] Die Competition-Resultate bilden die Daten-Grundlage der Formalen Begriffsanalyse. Alle genannten Bestandteile der Termination-Competition werden in diesem Kapitel erläutert.

------

## Termination Problems Data Base
\label{subchapter:tpdb}

Während der Termination-Comeptition werden alle Benchmarks der Termination Problems Data Base (abgekürzt \gls{TPDB}) auf StarExec bearbeitet. Es existiert ein Mercurial\footnote{\url{https://www.mercurial-scm.org/}}-Respository\footnote{\url{http://cl2-informatik.uibk.ac.at/mercurial.cgi/TPDB/summary}} zur Verwaltung der Benchmark-Dateien. Eine Benchmark-Datei beschreibt ein Problem. Je nach Problem-Kategorie ist das Dateiformat verschieden. Der Benchmark "TRS_Relative/Relative_05/rt3-1.xml"\footnote{\url{http://cl2-informatik.uibk.ac.at/mercurial.cgi/TPDB/file/d085ae59ef47/TRS_Relative/Relative_05/rt3-1.xml}} gehört zur Kategorie der Termersetzungssysteme bzw. \gls{TRS}. Eine \gls{XML}-Datei beschreibt in dieser Kategorie über die Signatur $\sum=\{(a/0),(f/3),(g/1)\}$ das folgende Termersetzungssystem $R$:

\begin{itemize}
\item $f(g(x),y,z) \rightarrow f(x,y,g(z))$
\item $f(x,y,g(z)) \rightarrow f(x,g(y),z)$
\item $f(x,a(),z) \rightarrow f(x,g(a()),z)$
\item $f(x,y,z) \rightarrow f(x,y,g(z))$
\end{itemize}

Das aktuelle Release ist 10.3 (Revision \href{http://cl2-informatik.uibk.ac.at/mercurial.cgi/TPDB/rev/4d76dd84fd49}{4d76dd84fd49}, Stand 11.7.2016). In TPDB 10.3 befindet sich eine Sammlung von 16006\footnote{find TPDB-4d76dd84fd49 | wc -l} Dateien. Der Aufbau der TPDB ist ähnlich der Competition-Kategorien [@TC15Report]. Die Tabelle \ref{tab:tpdb} gibt eine Übersicht über Verteilung der Kategorien in der TPDB.

\newpage

\begin{table}[h]
\centering
\begin{tabular}{lll}
\hline
Kategorie                  & Anzahl der Dateien & Dateinnamenerweiterung \\ \hline
Termersetzungssysteme      & 10811              & xml                    \\
Haskell-Programme          & 1671               & hs                     \\
Integer-Transitionssysteme & 1222               & SMT2                   \\
C-Programme                & 803                & c                      \\
Prolog-Programme           & 633                & pl                     \\
Java-Programme             & 443                & jar                    \\
Integer Termersetzungssysteme & 117                & itrs                   \\ \hline
\end{tabular}
\caption{Übersicht über den Inhalt der TPDB}
\label{tab:tpdb}
\end{table}


*Anmerkung:* Die Anzahlen wurden mit dem Befehl `find TPDB-4d76dd84fd49 -name "*.<extension>" | wc -l` ermittelt, wobei \text{<extension>} mit der zugehörigen Dateinnamenerweiterung ersetzt wurde.


## Solver
\label{subchapter:solver}

Die an einer Competition teilnehmenden Solver müssen eine Menge von Benchmarks lösen. Ein Algorithmus [@TermCompSelectAlg] wählt eine Untermenge an Benchmarks aus den existierenden Benchmarks der aktuellen Kategorie. Die ausgewählte Menge aus Solvern und Benchmarks wird als Job bezeichnet. Bei den Termination-Competitions laufende Jobs sind Competitions.


## Plattformlatform StarExec
\label{subchapter:se}

StarExec\footnote{\url{https://www.starexec.org/}} ist ein Community-übergreifender Service der University of Iowa und der University of Miami.
Die Ziele des Services sind unter anderem die experimentelle Auswertung von Logik-Solvern und das Ausführen von Solver-Competitions. Dabei dient StarExec als Plattform und stellt Speicher, Recheninfrastruktur und Bibliotheken zur Verfügung. [@SE]

## Star-Exec-Presenter
\label{subchapter:sep}

Star-Exec-Presenter ist im Rahmen der Masterarbeit von Stefan von der Krone entstanden und wurde von Prof. Waldmann weiterentwickelt. Die Applikation ist in Haskell (Kapitel \ref{chapter:haskell})/Yesod (Kapitel \ref{subchapter:yesod}) implementiert. Die Features von Star-Exec-Presenter sind vielfältig, das wichtigste Feature ist die Visualisierung der Ergebnisse der Termination-Competition auf StarExec.

Die Daten der Competiton werden auf der \nameref{subchapter:se} berechnet und falls auf Star-Exec-Presenter benötigt, angefordert und in einer lokalen Datenbank abgelegt. Aus der Datenbank werden die verschiedenen Daten geladen und auf entsprechenden Seiten dargestellt. Solver werden als Spalten und die Benchmarks der TPDB (Kapitel \ref{subchapter:tpdb}) als Zeilen in der Tabelle aus Abbildung \ref{img:SEP_SMJR_5375} dargestellt. Die Zellen/JobPairIDs als Kreuzungspunkte beschreiben die \nameref{subchapter:result-data}.

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/SEP_old_table.pdf}
  \caption{Star-Exec-Presenter: Resultat-Tabelle bzgl. der Job-ID 5375}
  \label{img:SEP_SMJR_5375}
\end{figure}

\url{http://termcomp.imn.htwk-leipzig.de/results/standard/noquery/5375}

\newpage

## Resultat-Daten
\label{subchapter:result-data}
Jeder Solver untersucht die Benchmarks auf ihre Termination. Die Kombination aus Solver und Benchmark wird als JobPair oder kurz Pair bezeichnet. Das Resultat eines JobPairs wird in Tabelle \ref{tab:result-data} dargestellt.


\begin{table}[h]
\centering
\begin{tabular}{lll}
\hline
Information     & Beispiel       & Erläuterung                       \\ \hline
JobId           & 5375           & Id des Jobs                       \\
Score           & Nothing        & Punktzahl                         \\
PairId          & 26929910       & Id des JobPairs                   \\
Benchmark       & rt3-1.xml    & Pfad der Benchmarkdatei           \\
BenchmarkId     & 1123229        & Id des Benchmarks                 \\
Solver          & AProVE\_JRE2 & Name des Solvers                  \\
SolverId        & 1681           & Id des Solvers                    \\
Configuration   & trs          & Konfiguration des Solvers         \\
ConfigurationId & 2656           & Id der Konfiguration              \\
Status          & complete       & Abarbeitungsstatus                \\
CpuTime         & 3.46947        & Zeit durch Nutzung mehrerer Kerne \\
WallclockTime   & 1.24966        & real vergangene Zeit              \\
Result          & YES            & Antwort bzgl. Termination         \\ \hline
\end{tabular}
\caption{Resultat-Daten des JobPairs 26929910}
\label{tab:result-data}
\end{table}


*Anmerkungen zur Tabelle \ref{tab:result-data}:*

* Auf Grund der Übersichtlichkeit wurde der Name des Benchmarks verkürzt. Der vollständige Name lautet: "TRS_Relative/Relative_05/rt3-1.xml"
* Ein Postprozessor prüft das Resultat des Solvers. Zur Vereinfachung wird sich auf die Antworten des Solvers beschränkt. Antworten des Postprozessors sind: CERTIFIED, ERROR, OTHER.
