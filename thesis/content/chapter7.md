# Auswertung
\label{chapter:evaluation}

Die Implementierung wird in Bezug auf die Nutzer-Wünsche in Kapitel \ref{subchapter:spec} ausgewertet. Eine erneute Befragung der Nutzer und deren Antworten wird vorgestellt. Nach quantifizierbaren Merkmalen der Lösung erfolgt eine Auswertung mit Fokus auf die Implementierung.

------

## Implementierung

Die in dem Kapitel \nameref{subchapter:spec} erarbeiteten Ziele waren:

1. Einschränkung der Datenmenge durch Filter
2. mehr Übersichtlichkeit, besserer Zugang zu Informationen
3. eine verbesserte Navigation (z.B.: durch ein Menü)


Durch das Einführen der `Concepts-Route` inklusive `Concepts-Handler` werden dem Nutzer eine weitere Möglichkeit zur Analyse der Resultat-Daten zur Verfügung gestellt. `Flexible Table`, als bestehende Möglichkeit zur Analyse, erlaubt es die Daten nach Zeile und Spalte zu Filtern.

Bei Aufruf der `Concepts-Route` mit bestimmten Routenparametern wird eine oder mehrere Resultat-Tabellen ausgewählt.
Die Zellen dieser Resultat-Tabellen, sogenannte `JobPairs` besitzen verschiedene Merkmale. Zur ID eines `JobPairs` werden durch die Konstruktion des Typs `Attribute` als Merkmale abgelegt. In Form von Paaren `[(JobPairID, [Attribute])]` werden diese Informationen verknüpft.

Ein Formular erlaubt es dem Nutzer interessante Merkmale auszuwählen. Alle Paare die keines der ausgewählten Merkmale enthalten, werden entfernt. Aus den verbleibenden Paaren wird durch Formale Begriffsanalyse ein Formaler Kontext erstellt. Der Kontext enthält die `JobPairs` der gewählte(n) Tabelle(n) als Gegenstände und deren `Attribute` als Merkmale.

Aus dem Formalen Kontext wird der Begriffsverband bestimmt. Alle `JobPairs` der Ausgangstabelle werden anhand von gleichen Merkmalen in Formalen Begriffen zusammengefasst. Der Begriffsverband beinhaltet all diese Formalen Begriffe. Ein Hasse-Diagramm visualisiert den Verband und macht die Teilmengen-Beziehung zwischen den Formalen Begriffen deutlich.

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/SEP_9515_hasse_diagram_table_2016_6_24.pdf}
  \caption{Hasse-Diagramm und Resultat-Tabelle zur JobID 9515}
  \small{\url{http://termcomp.imn.htwk-leipzig.de/concepts/0/Ids%20%5B%5D/9515?_hasdata=&SolverNames=1&SolverNames=2}}
  \label{img:concepts_SEP_9515_hasse_diagram_table}
\end{figure}

In Abbildung \ref{img:concepts_SEP_9515_hasse_diagram_table} ist zu erkennen, dass sich die acht JobPairs im Ausgangsbegriff in drei \colorbox{yes}{YES}-Knoten und fünf \colorbox{maybe}{MAYBE}-Knoten aufteilen.
In der nächsten Ebene wird ersichtlich, dass *AProVE 2015* zwei Benchmarks und *TTT2* nur einen Benchmark gelöst hat. Die Verteilung der Solver für die MAYBE-Knoten lässt sich analog ablesen.

Vor dieser Masterarbeit gab es in der *Show Many Job Results*-Route nur Aussagen über die erfolgreich gelösten Benchmarks. Wobei ohne die Formale Begriffsanalyse und das Hasse-Diagramm die Information nur kontextlose Zahl dargestellt war. Die kompakte Darstellung und Teilmengen-Beziehung zwischen den Begriffsknoten im Hasse-Diagramm transportiert die Informationen einfacher zum Anwender.

Durch einen Klick auf die Knoten zeigt die Resultat-Tabelle nur die *JobPair*, die die Merkmale des Knotens besitzen. Demnach ist der erste Punkt der Anforderungen - das Einschränken der Datenmenge - erfüllt. Mit dem Hasse-Diagramm wird ein Überblick über die verfügbaren Merkmale und deren Beziehungen visualisiert. Der Nutzer kann vom ersten Knoten mit allen gemeinsamen Merkmalen im aktuellen Formalen Kontext den gerichteten Kanten bis zur gesuchten Merkmalmenge folgen. Bei einer kleinen Menge an Begriffsknoten ist der zweite Punkt der Anforderungen erfüllt. Mit dem Zoom-Plugin `jQuery Panzoom` können auch Informationen von größeren Begriffsverbänden zum Nutzer transportiert werden. Die zwei Links innerhalb eines Knotens ermöglichen es dem Nutzer die Menge der Begriffe im Begriffsverband zu reduzieren und die gesuchte Information zu finden. Die Einführung eines Menüs erfüllt den dritten Punkt der Anforderung. Demnach sind alle Anforderungen erfüllt.


## Interview \label{eval:interview}

Um die Seite gegenüber den Nutzer zu evaluieren, wurden diese erneut befragt. Das Interview bestand aus den folgenden Fragen:

1. Ist die derzeitige Implementierung hilfreich um die Tabelle zu filtern?
2. Können Sie sich vorstellen, das darüber für Sie interessante Datenmengen gefunden werden können?
3. Was stört Sie an der aktuellen Implementierung?
4. Welche Verbesserungen fallen Ihnen sofort ein?


Antworten:
\begin{enumerate}
\item 2x Ja, 1x Nein
\item 1x Ja, 1x Vielleicht, 1x Nein
\item \begin{enumerate}
      \item Vergleich von Ergebnissen verschiedener Jahre
      \item Benchmark-Merkmale für nicht TRS-Competitions uninteressant
      \item Bedeutung von ''CPU times'' unklar
      \end{enumerate}
\item \begin{enumerate}
      \item selbstdefinierte Auswahlkriterien (warum ausgerechnet $<=10s$ und nicht $<=42s$?)
      \item Erweiterung auf andere Competitions z.B. \gls{SMT}.
      \item Vergleich zu selbst gestarteten Jobs
      \end{enumerate}
\end{enumerate}


Die Antworten deuten auf einen insgesamt positiven Eindruck. Die neue Route hilft beim Filtern und vor allem Benchmark-Merkmale für andere Competitions sind von Interesse.

\newpage

## Quantifizierbare Merkmale

Eine Zeitmessung, die Übersicht über die geschriebenen Haskell-Module und Code-Metriken fassen die Implementierung in messbaren Größen und Zahlenwerten zusammen.

**Zeit für Berechnung Begriffsverband**

Als quantifizierbares Merkmal der Implementierung soll eine Zeitmessung dienen. Es wird die Zeitdauer für die Berechnung des Begriffsverbandes einschließlich der Konstruktion des Hasse-Diagramms gemessen. Zur Messung wird die neue `Concepts`-Route für eine JobID aufgerufen und gewartet bis auf dem Terminal des lokal laufenden Servers der erfolgreiche Response vermeldet wird. Zur Messung wurde Revision \href{https://github.com/rm--/star-exec-presenter/tree/92b031d3e354fe8bdf62676ee4b654dd09135cca}{92b031d} von Star-Exec-Presenter verwendet, einschließlich Optimierung durch die Bibliothek begriff\footnote{\url{https://gitlab.imn.htwk-leipzig.de/waldmann/begriff}}.

Die folgende Hard- und Software wurde für die Zeitmessung genutzt.

\begin{multicols}{2}
\textit{Hardware:}

\begin{itemize}
\item Prozessor: Intel Core i5
\item Prozessor-Geschwindigkeit: 2,6 GHz
\item Anzahl Kerne: 2
\item RAM: 16 GB
\end{itemize}

\columnbreak
\textit{Software:}

\begin{itemize}
\item Betriebssystem: OSX 10.10.5
\item Graphviz 2.36.0
\item \gls{GHC} 7.10.3
\item Yesod 1.4.3
\item Google Chrome 51.0.2704.103 (64-bit)
\end{itemize}
\end{multicols}

Die Tabelle \ref{tab:measurement-sum} zeigt die Ergebnisse der Messung. Die Berechnung des Begriffsverbandes zur JobID 10299 mit einer Anzahl von 95 Formalen Begriffen hat beispielsweise 1,04 Sekunden gedauert. Für jede JobID wurden fünf Messwerte aufgenommen und zur Berechnung der $\varnothing$-Zeit wurde das arithmetische Mittel dieser gebildet. Die Werte der Einzelmessungen befinden sich im Anhang \ref{appendix:tab:measurement-detail}.

\hspace{0.1cm}

\begin{table}[h]
\centering
\begin{tabular}{cccccc}
\hline
JobID & |Gegenstände G| & |Merkmale M| & |Relationen R| & |B(G,M,R)| & $\varnothing$-Zeit in Sekunden \\ \hline
9515  & 8               & 13           & 56           &  9         & 0.08             \\
10299 & 820             & 24           & 5740         &  95        & 1,04            \\
10257 & 10486           & 37           & 73402        &  658       & 17,05            \\ \hline
\end{tabular}
\caption{Messwerte für die Jobs 9515, 10299 und 10257}
\label{tab:measurement-sum}
\end{table}

\newpage

**Haskell-Module**

Die Umsetzung der Formalen Begriffsanalyse während der Masterarbeit brachte die folgenden sechs Haskell-Module hervor:

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/hs_modules.pdf}
  \caption{Abhängigkeiten der implementierten Haskell-Module}
  \label{img:hs_modules}
\end{figure}

Der Graph in Abbildung \ref{img:hs_modules} zeigt die Abhängigkeiten der Haskell-Module untereinander. Wenn *Modul1* von *Modul2* verwendet wurde, existiert eine Kante von *Modul2* zu *Modul1*. Zur Erstellung des Graphen wurde das graphmod\footnote{\url{https://github.com/yav/graphmod}} verwendet\footnote{find . -name '*.hs' | xargs graphmod -q --no-cluster | dot -Tpdf > concepts.pdf}.

**Code-Metriken**

Ein weiteres quantifizierbares Merkmal in der Softwareentwicklung sind Code-Metriken. Die Tabelle \ref{tab:code} zeigt die Verteilung von \gls{LOC}, Commits und Editierungen von Dateien auf die Entwickler von Star-Exec-Presenter. Die Original-Tabelle im Anhang \ref{appendix:tab:code} wurde mit `git-fame`\footnote{\url{https://github.com/oleander/git-fame-rb}} erstellt\footnote{git fame --exclude static/} und aufgrund mehrerer Nutzer mit unterschiedlicher Namen aggregiert. Das Verzeichnis static/ wurde ausgeschlossen, da das Einfügen von statischen Dateien der Tools `Bootstrap`, \gls{jQuery}, `jQuery Panzoom` und `normalize.css` nur die LOC-Statistik verfälscht. Die Tabelle zeigt das 1580 LOC (12,8 % Anteil an den gesamten LOC), 215 Commits und 75 Datei-Editierungen durch diese Arbeit entstanden sind.

\begin{table}[h]
\centering
\begin{tabular}{lllll}
\hline
Name & LOC  & Commits & Dateien & Verteilung     \\ \hline
Stefan von der Krone   & 6709 & 237     & 138     & 54,3/33,3/81,7 \\
Johannes Waldmann   & 4078 & 260     & 93      & 33,0/36,5/39,6 \\
René Muhl   & 1580 & 215     & 75      & 12,8/30,2/44,4 \\ \hline
\end{tabular}
\caption{LOC, Commits und Datei-Editierungen der Entwickler von Star-Exec-Presenter}
\label{tab:code}
\end{table}


## Resultat

Die erarbeiteten Anforderungen aus dem Kapitel \ref{subchapter:spec} wurden durch die Implementierung (Kapitel \ref{chapter:impl}) erfüllt. Die Nutzer nehmen die neue Seite positiv auf und entgegneten mit weiteren Wünschen. Die Zeitmessung zeigt, dass die aktuelle Implementierung die Begriffsverbände für große *Jobs* wie die Competition *TRS Standard* (JobID 10257) noch nicht schnell genug berechnet. Im Kapitel \ref{subchapter:impl-opt} wurden Optimierungen angegeben, die die Zeit bereits verbessern sollte. Die Implementierung besteht aus sechs Haskell-Modulen und wurde mit 1580 LOC und 215 Commits zu Star-Exec-Presenter hinzugefügt.
