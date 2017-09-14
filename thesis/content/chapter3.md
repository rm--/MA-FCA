# Formale Begriffsanalyse
\label{chapter:fca}

Die Formale Begriffsanalyse ist eine Methode des Data-Minings und dient der Klassifikation von JobPairs in den Resultat-Daten auf \gls{Star-Exec-Presenter}. Es werden mathematische Grundlagen aus der Ordnungs- und Verbandstheorie und darauf aufbauend benötigte Aspekte der Formalen Begriffsanalyse erläutert.

---

## Allgemein

Die \gls{Formale Begriffsanalyse} (engl. formal concept analysis/FCA) seltener Formale Konzeptanalyse genannt, stellt eine Anwendung der Verbandstheorie dar. Die Theorie der FCA wurde Anfang der 1980er Jahre von Rudolf Wille im Fachbereich Mathematik der Technischen Hochschule Darmstadt definiert \cite[S.1]{ApplLatticeT} \cite[S.58]{GanterWille96}.

Um sich von dem bereits vergebenen Begriff "Begriff" abzuheben, wird mit der zusätzlichen Eigenschaft "formal" die Absicht einer mathematischen Beschreibung verdeutlicht \cite[S.17]{GanterWille96}.
In der vorliegenden Auseinandersetzung wird der Ausdruck Begriffsanalyse dem der Konzeptanalyse vorgezogen, da in der deutschen Sprache ein Begriff für eine Sache eine Menge von Merkmalen besser beschreibt als der Begriff "Konzept".

*Anmerkung:* Zur Theorie werden Modellierungen in Haskell\footnote{\url{https://www.haskell.org/}} angegeben. Eine ausführliche Erläuterung der Sprache und deren Sprach-Features werden in Kapitel \ref{chapter:haskell} vorgenommen.

Um die Formale Begriffsanalyse vorzustellen sind Grundlagen aus der Mengenlehre sowie der Ordnungstheorie notwendig.
Mathematik ist die Grundlage der Informatik und die Ordnungstheorie als Teilgebiet der Mathematik die Basis für die Formale Begriffsanalyse.

Es wird die Kenntnis grundlegender mengentheoretischer Relationen und Operatoren vorausgesetzt. In der Masterarbeit werden die folgenden verwendet/bezeichnet:

* Relationen: Teilmenge $\subseteq$, Element $\in$

* Operationen: Kartesisches Produkt $\times$, Vereinigungsmenge $\cup$, Differenzmenge $\setminus$, Potenzmenge $\mathcal{P}(M)$, Mächtigkeit $|M|$

\newpage

## Ordnungs- und Verbandstheorie
\label{subchapter:math-theory}

\begin{newdef}{}\label{def-halbordnung}
$(M, R)$ mit $R \subseteq M^2$ ist eine \textbf{Halbordnung} oder \textbf{partiell geordnete Menge}, wenn folgende Eigenschaften erfüllt sind:
\begin{itemize}
\item Reflexivität: $\forall a \in M: (a,a) \in R$
\item Transitivität: $\forall a,b,c \in M: (a,b) \in R \land (b,c) \in R \rightarrow (a,c) \in R$
\item Antisymmetrie: $\forall a,b \in M: (a,b) \in R \land (b,a) \in R \rightarrow a = b$
\end{itemize}
\end{newdef}

\begin{bsp}[Halbordnung]
$(\mathbb{N},\le), (\mathbb{N},\mid), (\mathcal{P}(M), \subseteq)$ \cite[S.103]{Math4Inf}
\end{bsp}

\begin{newdef}{}\label{def-obere-untere-schranke}
\cite[S.237]{Math4Inf}
Ist in einer Halbordnung $(M, \preceq)$ die Menge $N \subseteq M$ nicht leer,
\begin{itemize}
\item so heißt $x \in M$ eine \textbf{untere Schranke} von $N$, falls $\forall n \in N: x \preceq n$.
\item so heißt $y \in M$ eine \textbf{obere Schranke} von $N$, falls $\forall n \in N: n \preceq y$.
\end{itemize}
\end{newdef}

\begin{bsp}[untere/obere Schranken]
Halbordnung $(\mathbb{N},\le)$ mit $N = \{1,2,3\} \subseteq \mathbb{N}$ \\ untere Schranke von $N$: $0$ obere Schranken von N: 3,4,7

Halbordnung $(\mathbb{N},\mid)$ mit $N = \{6,12,18\} \subseteq \mathbb{N}$ \\ untere Schranke von $N$: $1$ obere Schranken von N: 72,144

Halbordnung $(\mathcal{P}(\{a,b,c\}), \subseteq)$ mit $N = \{\emptyset,\{a\},\{c\}\} \subseteq \mathcal{P}(\{a,b,c\})$ \\ untere Schranke von $N$: $\emptyset$ obere Schranke von $N$: $\{a,b,c\}$
\end{bsp}


\begin{newdef}{}\label{def-infimum-supremum}
\cite[S.237]{Math4Inf}
In einer Halbordnung $(M, \preceq)$ mit $N \subseteq M$ sei $x \in M$ eine untere Schranke von $N$ und $y \in M$ eine obere Schranke von $N$.
\begin{itemize}
\item $x$ ist ein \textbf{Infimum} oder eine \textbf{größte untere Schranke} von $N$, falls für alle weiteren unteren Schranken $r $ von $N$ $r \preceq x$ gilt.
\item $y$ ist ein \textbf{Supremum} oder eine \textbf{kleinste obere Schranke} von $N$, falls für alle weiteren oberen Schranken $r $ von $N$ $y \preceq r$ gilt.
\end{itemize}

Aus der Antisymmetrie von $\preceq$ ist das Infimum und Supremum (sofern sie existieren) eindeutig bestimmt.

Notation:
\begin{itemize}
\item x ist genau dann Infimum von $N$, wenn $x = \inf(N)$
\item y ist genau dann Supremum von $N$, wenn $y = \sup(N)$
\end{itemize}
\end{newdef}

\begin{bsp}[Infimum \& Supremum]\label{bsp-sup-inf}
Halbordnung $(\mathbb{N},\le)$ mit $N = \{1,2,3\} \subseteq \mathbb{N}$ \\ $\inf(N) = 1$, $\sup(N) = 3$

Halbordnung $(\mathbb{N},\mid)$ mit $N = \{6,12,18\} \subseteq \mathbb{N}$ \\ $\inf(N) = 6$, $\sup(N) = 36$

Halbordnung $(\mathcal{P}(\{a,b,c\}), \subseteq)$ mit $N = \{\emptyset,\{a\},\{c\}\} \subseteq \mathcal{P}(\{a,b,c\})$ \\ $\inf(N) = \emptyset$, $\sup(N) = \{a,c\}$
\end{bsp}

\begin{newdef}{}\label{def-verband}

\cite[S.65]{Ganter13}
\cite[S.237]{Math4Inf}
Eine Halbordnung $(M,\le)$ ist genau dann ein \textbf{Verband}, wenn $\forall a,b \in M$:
\begin{itemize}
\item $\inf(\{a,b\})$
\item $\sup(\{a,b\})$
\end{itemize}
existieren.
\end{newdef}

\begin{bsp}[Verband]
Die Halbordnungen $(\mathbb{N},\le), (\mathbb{N},\mid), (\mathcal{P}(M), \subseteq)$ sind Verbände, da sie ein Infimum und Supremum (in Beispiel \ref{bsp-sup-inf} gezeigt) besitzen.
\end{bsp}

\begin{newdef}{}\label{def-hasse-diagramm}
Jede endliche Halbordnung $(M, \preceq)$ lässt sich als \textbf{Hasse\footnote{Helmut Hasse (1898 - 1976): Professor für Mathematik in Berlin und Hamburg}-Diagramm} (gelegentlich auch Liniendiagramm) darstellen. Die Elemente von $M$ werden als Kreise dargestellt. Wenn $x,y \in M$ mit $x \prec y$, dann ist der Kreis für $y$ im Diagramm oberhalb des Kreises von $x$. Beide Kreise werden durch eine Linie miteinander verbunden. Um eine Zuordnung zwischen Element und Kreis zu erhalten, wird das Element in den Kreis geschrieben. Die Ordnungsrelation lässt sich wie folgt aus dem Diagramm ablesen: $x \prec y$, wenn es der Kreis für x durch eine aufsteigende Linie den Kreis für y erreichbar ist. \cite[S.2]{GanterWille96} Linien die sich aus der Reflexivität und Transitivität der Relation $\preceq$ über die Menge $M$ ergeben werden ausgespart \cite[S.48]{DMathGrundlagen}. Im folgenden werden Kreise ''Knoten'' und Linien ''Kanten'' genannt.
\end{newdef}

\newpage

\begin{bsp}[Hasse-Diagramm]
\begin{tikzpicture}[
roundnode/.style={circle, draw=black, very thick, minimum size=7mm},
]
%Nodes
\node[roundnode] (3) {3};
\node[roundnode] (2) [below=of 3] {2};
\node[roundnode] (1) [below=of 2] {1};
\node (0) [below=of 1] {Halbordnung $(\{1,2,3\},\le)$};
%Lines
\draw[-] (3.south) -- (2.north);
\draw[-] (2.south) -- (1.north);
\end{tikzpicture}
\hspace{1cm}
\begin{tikzpicture}[
roundnode/.style={circle, draw=black, very thick, minimum size=7mm},
]
%Nodes
\node[roundnode] (18) at (1,0) {18};
\node[roundnode] (9) at (0,-1) {9};
\node[roundnode] (6) at (2,-1) {6};
\node[roundnode] (3) at (0,-2) {3};
\node[roundnode] (2) at (2,-2) {2};
\node[roundnode] (1) at (1,-3) {1};
\node (0) [below=of 1] {Halbordnung $(\{n \in \mathbb{N}: n | 18\},\mid)$};
%Lines
\draw[-] (18) -- (9);
\draw[-] (18) -- (6);
\draw[-] (9) -- (3);
\draw[-] (6) -- (2);
\draw[-] (6) -- (3);
\draw[-] (3) -- (1);
\draw[-] (2) -- (1);
\end{tikzpicture}

\begin{tikzpicture}[
roundnode/.style={circle, draw=black, very thick, minimum size=7mm},
]
%Nodes
\node[roundnode] (abc) at (3,0) {$\{a,b,c\}$};
\node[roundnode] (ab) at (0,-2) {$\{a,b\}$};
\node[roundnode] (ac) at (3,-2) {$\{a,c\}$};
\node[roundnode] (bc) at (6,-2) {$\{b,c\}$};
\node[roundnode] (a) at (0,-4) {$\{a\}$};
\node[roundnode] (b) at (3,-4) {$\{b\}$};
\node[roundnode] (c) at (6,-4) {$\{c\}$};
\node[roundnode] (emptyset) at (3,-6) {$\emptyset$};
\node (0) at (3,-8) {Halbordnung $(\mathcal{P}(\{a,b,c\}), \subseteq)$};
%Lines
\draw[-] (abc) -- (ab);
\draw[-] (abc) -- (ac);
\draw[-] (abc) -- (bc);
\draw[-] (ab) -- (a);
\draw[-] (ab) -- (b);
\draw[-] (ac) -- (a);
\draw[-] (ac) -- (c);
\draw[-] (bc) -- (b);
\draw[-] (bc) -- (c);
\draw[-] (a) -- (emptyset);
\draw[-] (b) -- (emptyset);
\draw[-] (c) -- (emptyset);
\end{tikzpicture}
\end{bsp}

## Formaler Kontext
\label{subchapter:fca-context}
Zur Veranschaulichung dient die Resultat-Tabelle \ref{tab:context} der JobID 9515. Die Gegenstände sind die Zellen der Tabelle, die sogenannten JobPairIDs. Als Merkmale dienen die Bestandteile: *Solver*, *Result* und *CpuTime* des JobPair-Datensatzes (Kapitel \ref{subchapter:result-data}). Es werden Modellierungen in Haskell angegeben. Eine Beschreibung der Sprache Haskell erfolgt in Kapitel \ref{chapter:haskell}.

\newpage

\begin{newdef}{}\label{def-formaler-kontext}
Das Tripel $(G,M,R)$ wird \textbf{Formaler Kontext} genannt, wenn:

\begin{itemize}
\item $G$ die Menge der Gegenstände
\item $M$ die Menge der Merkmale
\item $R$ die Relation $R \subseteq G \times M$
\end{itemize}

Ein Kontext wird oft mit einer Kreuztabelle beschrieben. Dabei befinden sich die Gegenstände in einer Zeile und die Merkmale in einer Spalte.
Ein boolescher Wert in Form eines \ding{56} markiert, ob ein Gegenstand ein Merkmal besitzt. Ist kein \ding{56} vorhanden, besitzt der Gegenstand den Wert nicht.
\end{newdef}

~~~{.haskell caption="FCA/Basic.hs-L26" label=code:context}
data Context ob at = Context
  { fore :: Map ob (Set at)
  , back :: Map at (Set ob)
  } deriving (Show)
~~~

\begin{table}[ht]
\centering
\begin{tabular}{c|c|c|c|c|c}
G | M     & TTT2 & AProVE 2015 & YES & MAYBE & CPU time \textgreater 10s \\ \hline
118913107 & \ding{56}   &             &     & \ding{56}    & \ding{56}                        \\ \hline
118913108 &      & \ding{56}          &     & \ding{56}    & \ding{56}                        \\ \hline
118913109 & \ding{56}   &             & \ding{56}  &       & \ding{56}                        \\ \hline
118913110 &      & \ding{56}          & \ding{56}  &       & \ding{56}                        \\ \hline
118913111 & \ding{56}   &             &     & \ding{56}    & \ding{56}                        \\ \hline
118913112 &      & \ding{56}          & \ding{56}  &       & \ding{56}                        \\ \hline
118913113 & \ding{56}   &             &     & \ding{56}    & \ding{56}                        \\ \hline
118913114 &      & \ding{56}          &     & \ding{56}    & \ding{56}
\end{tabular}
\caption{Kreuztabelle zur JobID 9515}
\label{tab:context}
\end{table}


\begin{bsp}[Formaler Kontext]
Die Gegenstände $G=\{118913107,118913108,\ldots,118913113, 118913114\}$ mit den Merkmalen $M=\{\text{TTT2, AProVE 2015, YES, MAYBE, CPU time \textgreater 10s}\}$ in der Relation $R=\{(118913107,\text{TTT2}),...,(118913114,\text{ CPU time \textgreater 10s})\}$ ergeben den Formalen Kontext $(G,M,R)$ in Tabelle \ref{tab:context}.
\end{bsp}

\newpage

## Projektionen
\label{subchapter:fca-projections}

Die Menge der gemeinsamen Merkmale der Gegenstände aus $A \subseteq G$ sei:

* $A' := \{ m \in M ~ | ~ \forall g \in A : (g,m) \in R \}$

~~~{.haskell caption="FCA/Basic.hs-L66" label=code:getAttributes}
-- get all attributes of given context and specific objects
getAttributes :: (Ord ob, Ord at) => Context ob at -> Set ob -> Set at
getAttributes c obs = foldr Set.intersection (attributes c)
  $ map (\o -> fore c Map.! o) $ Set.toList obs
~~~


\begin{bsp}[Projektion A']
$A = \{118913109\} \subseteq G$ \\
$A' = \{118913109\}' = \{\text{TTT2, YES, CPU time \textgreater 10s}\}$ \\
Die Merkmale der JobID $118913109$ sind $\text{TTT2, YES}$ und $\text{CPU time \textgreater 10s}$.
\end{bsp}


Entsprechend die Menge der Gegenstände, die alle Merkmale aus $B \subseteq M$ besitzen:

* $B' := \{g \in G ~ | ~ \forall m \in B : (g,m) \in R \}$

~~~{.haskell caption="FCA/Basic.hs-L71" label=code:getObjects}
-- get all objects of given context and specific attributes
getObjects :: (Ord ob, Ord at) => Context ob at -> Set at -> Set ob
getObjects c ats = foldr Set.intersection (objects c)
  $ map (\a -> back c Map.! a) $ Set.toList ats
~~~

\begin{bsp}[Projektion B']
$B = \{\text{TTT2, YES, CPU time \textgreater 10s}\} \subseteq M$ \\
$B' = \{\text{TTT2, YES, CPU time \textgreater 10s}\}' = \{118913109\}$ \\
Der Gegenstand (in diesem Fall nur ein Einzelner), der alle \\ Merkmale $\text{TTT2, YES, CPU time \textgreater 10s}$ beseitzt, ist die JobID $118913109$.
\end{bsp}

\newpage

## Formaler Begriff
\label{subchapter:fca-concept}

\begin{newdef}{}\label{def-formaler-begriff}
Ein Paar $(A,B)$ ist ein \textbf{formaler Begriff} des Formalen Kontextes $(G,M,R)$ mit \cite[S.18]{GanterWille96}:

\begin{itemize}
\item $A \subseteq G$ (Begriffsumfang/extent)
\item $B \subseteq M$ (Begriffsinhalt/intent)
\item $A' = B$ (siehe \nameref{subchapter:fca-projections})
\item $B' = A$ (siehe \nameref{subchapter:fca-projections})
\end{itemize}
\end{newdef}

~~~{.haskell caption="FCA/Basic.hs-L31" label=code:concept}
data Concept ob at = Concept
  { obs :: Set ob
  , ats :: Set at
  } deriving (Show, Eq)
~~~

\begin{table}[ht]
\centering
\begin{tabular}{c|c|c|c|c|c}
G | M     & TTT2 & AProVE 2015 & YES & MAYBE & CPU time \textgreater 10s \\ \hline
118913107 & \ding{56}   &             &     & \ding{56}    & \ding{56}                        \\ \hline
\cellcolor{maybe}118913108 &      & \cellcolor{maybe}\ding{56}          &     & \cellcolor{maybe}\ding{56}    & \cellcolor{maybe}\ding{56}                        \\ \hline
\cellcolor{yes}118913109 & \cellcolor{yes}\ding{56}   &             & \cellcolor{yes}\ding{56}  &       & \cellcolor{yes}\ding{56}                        \\ \hline
118913110 &      & \ding{56}          & \ding{56}  &       & \ding{56}                        \\ \hline
118913111 & \ding{56}   &             &     & \ding{56}    & \ding{56}                        \\ \hline
118913112 &      & \ding{56}          & \ding{56}  &       & \ding{56}                        \\ \hline
118913113 & \ding{56}   &             &     & \ding{56}    & \ding{56}                        \\ \hline
\cellcolor{maybe}118913114 &      & \cellcolor{maybe}\ding{56}          &     & \cellcolor{maybe}\ding{56}    & \cellcolor{maybe}\ding{56}
\end{tabular}
\caption{Beispiele für Formale Begriffe der JobID 9515}
\label{tab:concepts}
\end{table}

\begin{bsp}[Formaler Begriff]
Der Formale Kontext des Jobs 9515 beinhaltet 10 Formale Begriffe (Beispiel \ref{bsp-begriffsverband} zeigt alle). Die Markierungen in Tabelle \ref{tab:concepts} zeigen beispielhaft zwei der zehn Formalen Begriffe: \colorbox{yes}{$(\{118913109\},\{\text{TTT2,YES,CPU time \textgreater 10s}\})$}, \colorbox{maybe}{$(\{118913108, 118913114\},$}
\colorbox{maybe}{$\{\text{AProVE 2015,MAYBE,CPU time \textgreater 10s}\})$}.
\end{bsp}

\newpage

### Algorithmus zur Begriffsbestimmung
\label{subchapter:fca-derivation}

Innerhalb eines Formalen Kontext $(G,M,R)$ lässt sich ein Formaler Begriff durch die Ausführung einer der beiden Schrittfolgen (jeweils eine Spalte) [@Schwarz11] erreichen:

\begin{multicols}{2}

1. $A \subseteq G$ wählen \\
2. $A'$ und $A''$ bestimmen \\
3. wenn $A''$ = $A$, neuer Begriff $(A,A')$

\columnbreak

1. $B \subseteq M$ wählen \\
2. $B'$ und $B''$ bestimmen \\
3. wenn $B''$ = $B$, neuer Begriff $(B',B)$
\end{multicols}

Die folgende Modellierung in Haskell beschreibt einen Algorithmus zur Bestimmung aller Formalen Begriffe innerhalb eines Formalen Kontextes. Die Umsetzung erfüllt die Spezifikation und wird in Kapitel \ref{subchapter:impl-concepts} durch eine effizientere Variante ersetzt.

~~~{.haskell caption="FCA/Basic.hs-L53" label=code:concepts_basic}
-- determine all concepts of given context
concepts :: (Ord ob, Ord at) => Context ob at -> [(Set ob, Set at)]
concepts c = do
  ats <- map (\ats -> Set.fromList ats) $ subsequences $ Set.toList $ attributes c
  guard $ ats == (getAttributes c $ getObjects c ats)
  return (getObjects c ats, ats)
~~~

\begin{bsp}[erfolgreiches Bestimmen eines Begriffes]
$B = \{\text{TTT2, YES, CPU time \textgreater 10s}\} \subseteq M$ \\
$B' = \{\text{TTT2, YES, CPU time \textgreater 10s}\}' = \{118913109\}$ \\
$B'' = \{118913109\}'= \{\text{TTT2, YES, CPU time \textgreater 10s}\}$ \\ \\
Da die Gleichung $B = \{\text{TTT2, YES, CPU time \textgreater 10s}\} = \{\text{TTT2, YES, CPU time \textgreater 10s}\} = B''$ erfüllt ist, entsteht der neue Formale Begriff \\ $(B',B) = (\{118913109\},\{\text{TTT2, YES, CPU time \textgreater 10s}\})$.
\end{bsp}

\newpage

\begin{bsp}[nicht erfolgreiches Bestimmen eines Begriffes]
$A = \{118913110\} \subseteq G$ \\
$A' = \{118913110\}' = \{\text{AProVE 2015, YES, CPU time \textgreater 10s}\}$ \\
$A'' = \{\text{AProVE 2015, YES, CPU time \textgreater 10s}\}'$  = \{118913110,118913112\} \\ \\
Da die Gleichung $A'' = \{118913110,118913112\} = \{118913110\} = A $ nicht erfüllt ist, folgt kein Formaler Begriff.
\end{bsp}


*Anmerkung:* Bei der Wahl der Schrittfolgen zur Begriffsbestimmung sollte darauf geachtet werden, das die Menge mit der kleinsten Mächtigkeit (Gegenstände $G$ oder Merkmale $M$) verwendet wird. Zur Berechnung aller Teilmengen der gewählten Ausgangsmenge dient die Potenzmenge dieser Menge. Die Potenzmenge einer Ausgangsmenge $C \subseteq M \text{ oder } C \subseteq G$ besitzt eine Mächtigkeit von $2^{|C|}$.

### Hierachie zwischen Begriffen
\label{subchapter:hierarchy}
Seien in einem Formalen Kontext $(G,M,R)$ zwei Formale Begriffe $(A_1 \subseteq G,B_1 \subseteq M), (A_2 \subseteq G,B_2 \subseteq M)$ heißt der Begriff $(A_1,B_1)$ \textbf{Unterbegriff} von $(A_2,B_2)$ falls $A_1 \subseteq A_2$ oder $B_2 \subseteq B_1$. $(A_2,B_2)$ heißt dann \textbf{Oberbegriff} von $(A_1,B_1)$ und man schreibt $(A_1,B_1) \le (A_2,B_2)$ \cite[S.20]{GanterWille96}.

\begin{bsp}[Begriffshierachie]
Gegeben seien die Formalen Begriffe:
\begin{itemize}
\item $f1 = (A_1,B_1) = (\{118913109\},\{\text{TTT2,YES,CPU time \textgreater 10s}\})$
\item $f2 = (A_2,B_2) = (\{118913109, 118913110, 118913112\},\{\text{YES,CPU time \textgreater 10s}\})$
\end{itemize}

$f1$ ist Unterbegriff von $f2$, da $A_1 = \{118913109\} \subseteq \{118913109, 118913110, 118913112\} = A_2$ bzw $B_2 = \{\text{YES,CPU time \textgreater 10s}\} \subseteq \{\text{TTT2,YES,CPU time \textgreater 10s}\} = B_1$ gilt.
\end{bsp}

\newpage

### Begriffsverband \label{subchapter:concept-lattice}

\begin{satz}{}\label{def-begriffsverband}
Alle Formalen Begriffe des Formalen Kontextes (G,M,R) bilden mit der Relation $\le$ (Kapitel \ref{subchapter:hierarchy}) einen Verband \cite[S.20]{GanterWille96}.
\end{satz}

\begin{bsp}[Begriffsverband]\label{bsp-begriffsverband}
Aus Gründen der Übersichtlichkeit wurden in den Knoten des \glslink{Hasse-Diagramm}{Hasse-Diagramms} die Gegenstände auf die letzten zwei Nummern verkürzt (aus 118913109 wird 09).

\begin{tikzpicture}[
roundnode/.style={circle, draw=black, very thick, minimum size=7mm},
]
%Nodes
\node (t) at (5.125,0) {$(G,\{\text{CPU time \textgreater 10s}\})$};
\node (ts1) at (0,-3) {\shortstack{$(\{07,09,11,13\}$, \\ $\{$CPU time \textgreater 10s, \\ TTT2$\})$}};
\node (tm) at (3.5,-3) {\shortstack{$(\{07,08,11,13,14\}$, \\ $\{$CPU time \textgreater 10s, \\ MAYBE$\})$}};
\node (ty) at (7,-3) {\shortstack{$(\{09,10,12\}$, \\ $\{$CPU time \textgreater 10s, \\ YES$\})$}};
\node (ts2) at (10.5,-3) {\shortstack{$(\{08,10,12,14\}$, \\ $\{$CPU time \textgreater 10s, \\ AProVE 2015$\})$}};
\node (ts1m) at (0,-6) {\shortstack{$(\{07,11,13\}$, \\ $\{$CPU time \textgreater 10s, \\ TTT2, \\ MAYBE$\})$}};
\node (ts1y) at (3.5,-6) {\shortstack{$(\{09\}$, \\ $\{$CPU time \textgreater 10s, \\ TTT2, \\ YES$\})$}};
\node (ts2m) at (7,-6) {\shortstack{$(\{08,14\}$, \\ $\{$CPU time \textgreater 10s, \\ MAYBE, \\ AProVE 2015$\})$}};
\node (ts2y) at (10.5,-6) {\shortstack{$(\{10,12\}$, \\ $\{$CPU time \textgreater 10s, \\ YES, \\ AProVE 2015$\})$}};
\node (ts1s2ym) at (5.125,-9) {($\emptyset$, $M$)};
\node (0) at (5.125,-10) {Begriffsverband des Kontextes zum Job 9515};
%Lines
\draw[-] (t) -- (ts1);
\draw[-] (t) -- (ts2);
\draw[-] (t) -- (ty);
\draw[-] (t) -- (tm);
\draw[-] (ts1) -- (ts1y);
\draw[-] (ts2) -- (ts2y);
\draw[-] (ts1) -- (ts1m);
\draw[-] (ts2) -- (ts2m);
\draw[-] (ty) -- (ts1y);
\draw[-] (ty) -- (ts2y);
\draw[-] (tm) -- (ts1m);
\draw[-] (tm) -- (ts2m);
\draw[-] (ts1m) -- (ts1s2ym);
\draw[-] (ts2m) -- (ts1s2ym);
\draw[-] (ts1y) -- (ts1s2ym);
\draw[-] (ts2y) -- (ts1s2ym);
\end{tikzpicture}
\end{bsp}

