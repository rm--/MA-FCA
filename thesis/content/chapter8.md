# Zusammenfassung
\label{chapter:summary}

Nach einer Einleitung in das Thema durch eine Motivation werden im Kapitel \nameref{chapter:termcomp} beteiligte Komponenten und die Daten untersucht. Die Einführung in die Formale Begriffsanalyse erhält der Leser in Kapitel \ref{chapter:fca} nachdem grundlegende Mathematik der Ordnungs- und Verbandstheorie vorgestellt wurde. Im \ref{chapter:requirements}. Kapitel wird ein Interview mit  Nutzern von Star-Exec-Presenter in Form einer Gegenüberstellung von Fragen und Antworten ausgeführt. Aus den Antworten und einer Betrachtung der aktuellen Möglichkeiten für den Nutzer auf Star-Exec-Presenter werden im Kapitel \ref{subchapter:spec} Ziele erarbeitet. Bevor die Implementierung erläutert wird, gibt es eine Übersicht über die Programmiersprache Haskell und das Web-Framework Yesod. In Kapitel \ref{chapter:impl} wird ein Entwurf, Entscheidungen bezüglich der Implementierung im Allgemeinen und der Nutzeroberfläche und die Implementierung an Code-Beispielen erläutert. Optimierungen in Kapitel \ref{subchapter:impl-opt} leiten in die Auswertung in Kapitel \ref{chapter:evaluation} über. Eine Untersuchung der Implementierung auf die gestellten Anforderungen an die Arbeit mit anschließender Erhebung von quantifizierbaren Merkmalen schließen die Arbeit ab.

## Einschätzung

Durch die Erweiterung von Star-Exec-Presenter durch Formaler Begriffsanalyse wurde die Applikation verbessert und das Ziel erreicht. Der Nutzer erhält auf der Seite `Concepts` eine neuartige Möglichkeit zur Analyse und Visualisierung der Resultatdaten der Termination-Competition.

Die aktuelle Implementierung erlaubt das Filtern mit bekannten und neuen Merkmalen der `JobPairs`. Durch die Einführung von Benchmark-Merkmalen wie *Anzahl der Regeln* und *Links-Linearität* wird eine weitere Dimension des Filtern eröffnet.

Die Visualisierung des Begriffsverbandes durch ein Hasse-Diagramm ermöglicht den Einblick in die Struktur der Daten und veranschaulicht die Beziehungen zwischen Begriffen und deren Merkmalen. Das erleichert die Navigation und Analyse der Daten.

Die Vielzahl an Daten erschwert die Visualisierung und ein Überblick über das gesamte Hasse-Diagramm ist nicht immer gewährleistet. Nachteil der aktuellen Implementierung ist die lange Zeitdauer für die Berechnung von großen Begriffsverbänden. Durch weitere Optimierungen bezüglich der Algorithmik und dem Verwenden anderer Technologien sind Verbesserungen möglich.

Die Formale Begriffsanalyse eignet sich zur Analyse der Resultatdaten der Termination-Competition.

\newpage

## Ausblick

Nachdem die `Concepts`-Seite intensiver durch die Nutzer von Star-Exec-Presenter verwendet wurde, ergeben sich wieder neue Anforderungen an diese. In den Nutzerantworten zur Evaluation der Implementierung in Kapitel \ref{eval:interview} sind bereits erste Forderungen zu finden.

Mit dem Haskell-Paket tpdb ist es bereits jetzt möglich weitere Benchmark-Merkmale\footnote{\url{https://hackage.haskell.org/package/tpdb-1.3.3/docs/TPDB-Data-Attributes.html}} zu berechnen. Die Einführung weiterer Merkmale in die aktuelle Implementierung erlaubt eine noch gezieltere Suche nach interessanten Teilmengen in den Resultat-Daten. Jedoch bedeuten weitere Merkmale eine weitere Verlangsamung der Berechnung und macht eine Optimierung des Algorithmus unerlässlich.

Das Haskell-Modul der Formalen Begriffsnalyse ist allgemein formuliert und erlaubt eine Nutzung in Star-Exec-Presenter. Mit anderen Merkmalen könnten Kategorie-spezifische Analysen und Visualisierungen entstehen. Denkbar ist eine Visualisierung der Kategorie über mehrere Competition-Jahre und die Anzahl an gelösten Benchmarks pro Solver.

Nach der Termination-Competition 2016 im September 2016\footnote{\url{http://termination-portal.org/wiki/Termination_and_Complexity_Competition_2016}} wird es neue Resultatdaten geben, die analysiert werden müssen.
