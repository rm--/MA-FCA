# Implementierung
\label{chapter:impl}

Im Kapitel \ref{subchapter:spec} wurden Ziele aus der bestehenden Implementierung und den Feature-Wünschen der Nutzer erarbeitet.
Die Implementierung dieser Ziele wird in der Programmiersprache Haskell (Kapitel \ref{chapter:haskell}) umgesetzt. Ein Entwurf stellt die Teilaufgaben sowie deren Zusammenhänge vor und erläutert allgemeine Entscheidungen. Bedienelemente und zugehörige Entscheidungen zur Nutzeroberfläche werden anschließend diskutiert. Die Implementierung der Teilaufgaben folgt danach in einzelnen Unterkapiteln. Optimierungen für die bestehende Implementierung bilden den Abschluss.

------

## Entwurf
\label{subchapter:design}

Die Formale Begriffsanalyse soll genutzt werden, dem Nutzer eine weitere Filter-Möglichkeit bereitzustellen. Dazu muss die Formale Begriffsanalyse auf die Resultat-Daten der Jobs angewendet werden. Der Nutzer soll den neuen Filter auf einer neuen Seite der Yesod-Applikation (Kapitel \ref{subchapter:yesod}) Star-Exec-Presenter verwenden können. Dazu muss eine neue Seite erstellt und diese in die bestehende Applikation integriert werden. Eine Nutzeroberfläche zur Visualisierung, Interaktion und Navigation dient dem Transport der Informationen der Formalen Begriffsanalyse.

Die Implementierung erfolgt durch die Lösung der folgenden Teilaufgaben:

1. \nameref{subchapter:impl-applyfca}
2. \nameref{subchapter:ingrsep}
3. \nameref{subchapter:impl-visfca}
4. \nameref{subchapter:impl-internav}

Es werden verschiedene Aspekte der Teilaufgaben angesprochen und begründete Entscheidungen getroffen.

\newpage

### Wahl der Ausgangsmenge zur Berechnung des Begriffsverbandes

Zur Berechnung der Formalen Begriffe werden alle Teilmengen der Merkmale oder Gegenstände des Formalen Kontextes bestimmt.
Wie in Kapitel \ref{subchapter:fca-derivation} angegeben, bestimmt die geringere Mächtigkeit der Merkmale oder Gegenstände die Ausgangsmenge für den Algorithmus zur Berechnung des Verbandes. Im Falle der Termination-Resultat-Daten ist die Mächtigkeit der Merkmale oft geringer als die der Gegenstände.

\begin{bsp}[Mächtigkeit der Gegenstände und Merkmale der JobID 10299]\label{bsp:cardset}
\url{http://termcomp.imn.htwk-leipzig.de/concepts/0/Ids%20%5B%5D/10299}

 Die Mächtigkeit der Gegenstände entspricht der Anzahl der JobPairIDs. JobID 10299 besitzt 820 JobPairIDs. Diese resultieren aus vier Solvern und 205 Benchmarks.

Die Mächtigkeit der Merkmale ergibt sich aus den verschiedenen Merkmalen alle JobPairIDs. Im Falle der JobID 10299 gibt es vier Solver mit jeweils einer Konfiguration, 3 Resultat-Werte (YES, MAYBE, NO) und jeweils den booleschen Werten für CPU-Zeit, Anzahl-Regeln und links-linear. In Summe sind das $8+3+6 = 17$ Merkmale.

Mächtigkeit der Gegenstände $|G| = 820$ und Mächtigkeit der Merkmale $|M| = 17$.
\end{bsp}

**Entscheidung:** Auf Basis des Beispiels \ref{bsp:cardset} wurde entschieden, dass die Merkmale der JobPairIDs als Ausgangsmenge für die Berechnung des Begriffsverbandes gewählt werden.

### Reduktion der Merkmalmenge
\label{subchapter:des-attr-set}

Durch das Berechnen der Potenzmenge der Merkmale $M$ werden alle Teilmengen der Menge $M$ bestimmt. Jedoch schließen alle Teilmengen von $M$ auch die Menge $\{$Solvername1, Solvername2$\}$ ein. Eine JobPairId kann jedoch stets nur einen Solver als Merkmal besitzen. Das heißt, die Projektion (Kapitel \ref{subchapter:fca-projections}) $\{$Solvername1, Solvername2$\}'$ ist die leere Menge, da die Projektionen $\{$Solvername1$\}'$ und $\{$Solvername2$\}'$ disjunkt\footnote{Zwei Mengen heißen disjunkt, wenn sie keine gemeinsamen Elemente besitzen.} sind. Das gilt für alle Merkmale einer Merkmalsgruppe. Eine Merkmalsgruppe bezeichnet eine Menge von Merkmalen, die zu dem gleichen Merkmalstyp wie Solvername gehören.

\newpage

\begin{bsp}[Potenzmenge der Merkmale und Kombination von Merkmalsgruppen]\label{bsp:no-powerset}

Im Beispiel \ref{bsp:cardset} wurde die Mächtigkeit der Merkmale für die JobPairId mit |M| = 17 bestimmt. Zur Berechnung aller Teilmengen von $M$ wird mit \textit{subsequences} die Potenzmenge der Menge |M| bestimmt. Das entspricht der Mächtigkeit von $2^{|M|} = 2^{17} = 131072$ Teilmengen.

Die 17 Merkmale der JobID 10299 zerlegen sich in die sechs Merkmalsgruppen: $|\text{Solver}| = 4, |\text{Konfigurationen}| = 4, |\text{Solver-Antworten}| = 3, |\text{CPU-Zeit}| = 2, |\text{Anzahl Regeln}| = 2, |\text{links-linear}| = 2$ \\
Bei der Kombination der Merkmalsgruppen sei es auch erlaubt das kein Merkmal einer oder mehrerer Merkmalsgruppe gewählt wird. Aus diesem Grund wird imaginär zu jeder Merkmalsgruppe die Möglichkeit des Nichtwählens durch eine weitere Option hinzugefügt. Aus den Mächtigkeiten der Merkmalsgruppen ergibt sich die Obergrenze der Teilmenge von
$\binom{5}{1} * \binom{5}{1} * \binom{4}{1} * \binom{3}{1} * \binom{3}{1} * \binom{3}{1} = 5*5*4*3*3 = 2700$.

Der Unterschied zwischen den beiden Werten für die Anzahl der Teilmengen zur Berechnung des Begriffsverbandes ist deutlich. Unter Verwendung von Kombination von Merkmalsgruppen werden deutlich weniger überflüssige Projektionen und Berechnungen ausgeführt.
\end{bsp}

**Entscheidung:** Im Beispiel \ref{bsp:no-powerset} wird gezeigt, dass es für die StarExec-Daten sinnvoll ist, die Kombination der Merkmalsgruppen anstatt die Potenzmenge aller Merkmale zu berechnen.

### Codierung der Merkmale

Jedes JobPair einer Resultat-Tabelle besitzt Merkmale. Jedes JobPair kann nach der Vorstellung einer Kreuztabelle ein Merkmal besitzen oder nicht. Bei dem Merkmal Solvername oder Solver-Konfiguration sind die möglichen Werte begrenzt und überschreiten nicht die Anzahl von zehn. Bei dem Merkmal *CPU-Zeit* können die JobPair jedoch alle Werte zwischen 0 Sekunden und 300 Sekunden annehmen. Ähnlich verhält es sich bei dem Merkmal *Anzahl der Regeln* eines Termersetzungssystems. Es existieren TRS mit den unterschiedlichsten Regel-Anzahlen.

**Entscheidung:** Bei Merkmalen *CPU-Zeit* und *Anzahl der Regeln* bestimmt der boolesche Wert des Ausdrucks den Merkmalswert. In der Tabelle \ref{tab:attr-cond} werden die Merkmale und ihre Ausdrücke aufgeführt.

\begin{table}[h]
\centering
\begin{tabular}{lll}
\hline
Merkmal & Ausdruck \\ \hline
CPU-Zeit & >10 Sekunden (ist langsam) \\
Anzahl der Regeln & <10 Regeln (ist gering) \\ \hline
\end{tabular}
\caption{Merkmale und ihre booleschen Ausdrücke}
\label{tab:attr-cond}
\end{table}

## Nutzeroberfläche
\label{subchapter:impl:ui}

Die Formale Begriffsanalyse soll dem Nutzer, ohne Kenntnis über diese, zur Verfügung gestellt werden. Im Rahmen der Arbeit wurden einige Entscheidungen getroffen, um die Visualisierung der Daten, der Interaktion mit der neuen Seite so intuitiv wie möglich zu gestalten. Screenshots der Bedienelemente der Nutzeroberfläche zeigen die Umsetzung der Entscheidungen. Die Grafik \ref{img:components} zeigt die Bedienelemente einschließlich URL und ihre Beeinflussung innerhalb der neuen Seite.

\begin{figure}[H]
  \centering
  \includegraphics[width=.8\textwidth]{content/figures/components.pdf}
  \caption{Bedienelemente und ihre gegenseitige Beeinflussung}
  \label{img:components}
\end{figure}

\newpage

### Formular

Für einige JobIDs, speziell für die Competition ist die Anzahl der JobPairs sehr groß.

**Entscheidung:** Ein HTML-Formular (Abbildung \ref{img:SEP_Concepts_10299_5376_Form}) soll dem Nutzer die Möglichkeit geben, seine Daten bereits so frühzeitig wie möglich einzuschränken. Das HTML-Formular zeigt alle Merkmale der aktuellen JobPairs, die zur gewählten JobIDs gehören. Die Merkmale sind ihren Merkmalsgruppen zugeordnet. Nach dem Absenden des Formulars verbleiben nur JobPairs mit gewählten Merkmalen.

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/SEP_10299_5376_form.pdf}
  \caption{HTML-Formular zur Wahl der Merkmale aus den Merkmalgruppen}
  \small{\url{http://termcomp.imn.htwk-leipzig.de/concepts/0/Ids%20%5B%5D/10299/5376?_hasdata&SolverNames=1&SolverNames=2}}
  \label{img:SEP_Concepts_10299_5376_Form}
\end{figure}

### Hasse-Diagramm \label{dec:hasse}

Ein Hasse-Diagramm visualisiert den Begriffsverband eines Formalen Kontextes. Im Hasse-Diagramm befinden sich nach Definition im Kapitel \ref{def-hasse-diagramm} das Infimum oben und das Supremum unten. Die Lese- und Scrollrichtung auf einer Webseite ist von oben nach unten.

**Entscheidung:** Das Hasse-Diagramm ist im Gegensatz zur Definition in Kapitel \ref{def-hasse-diagramm} vertikal gespiegelt (Abbildung \ref{img:SEP_Concepts_10299_Hasse_Diagramm}).

Für den Nutzer muss die Richtung der Kanten eindeutig erkennbar sein.

**Entscheidung:** Um die Richtung im Hasse-Diagramm für den Nutzer deutlich zu machen, wurden gerichtete Kanten verwendet (Abbildung \ref{img:SEP_Concepts_10299_Hasse_Diagramm}).

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/SEP_10299_hasse_diagram_part.pdf}
  \caption{Hasse-Diagramm-Ausschnitt einschließlich des Zoom-Sliders}
  \small{\url{http://termcomp.imn.htwk-leipzig.de/concepts/0/Ids%20%5B%5D/10299?_hasdata&SolverNames=1}}
  \label{img:SEP_Concepts_10299_Hasse_Diagramm}
\end{figure}

Das Hasse-Diagramm zeigt alle Begriffe des Begriffsverbandes. In jedem Knoten werden die Merkmale des Formalen Begriffs aufgeführt. Das führt zu Platzproblemen.

**Entscheidungen:**

* Merkmale werden innerhalb eines Knotens zeilenweise aufgeführt. Die Knoten-Farbe entspricht dem Merkmal Solver-Antwort und wurde aus der Zuordnung von Farbe und Solver-Antwort (zum Beispiel \colorbox{yes}{YES} und \colorbox{maybe}{MAYBE}) übernommen.
* Das Infimum (siehe Kapitel \ref{def-infimum-supremum}) des Begriffsverbandes ist der Begriff in dem alle Merkmale vereinigt sind. Dieser Knoten wurde entfernt, da es kein JobPair das alle Merkmale besitzt.
* Zur Reduktion der Begriffe des Begriffsverbandes werden dem Nutzer zwei Links zur Verfügung gestellt:
    * Klick auf einen Knoten ermöglicht die Anzeige einer Teilmenge des Begriffsverband (Kapitel \ref{subchapter:impl-incl} und Abbildung \ref{img:SEP_Concepts_10299_Hasse_Diagramm_YES_AProVE}).
    * Klick auf ein X in der rechten oberen Ecke des Knotens im Hasse-Diagramm um das Komplement des Begriffsverbandes (Kapitel \ref{subchapter:impl-compl}) mit dem aktuellen Begriff zu bilden (Abbildung \ref{img:SEP_Concepts_10299_Hasse_Diagramm_W/O_YES}).
* Ein Zoom-Slider unterhalb des Diagramms ermöglicht das Zoomen im Hasse-Diagramm (Abbildung \ref{img:SEP_Concepts_10299_Hasse_Diagramm}).

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/SEP_10299_hasse_diagram_s1_yes_incl.pdf}
  \caption{Hasse-Diagramm mit Begriffsverband-Teilmenge (Merkmale AProVE und YES)}
  \small{\url{http://termcomp.imn.htwk-leipzig.de/concepts/12/Ids%20%5B%5D/10299?_hasdata&SolverNames=1}}
  \label{img:SEP_Concepts_10299_Hasse_Diagramm_YES_AProVE}
\end{figure}

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/SEP_10299_hasse_diagram_w_o_yes.pdf}
  \caption{Hasse-Diagramm mit Begriffsverband-Komplement ohne YES}
  \small{\url{http://termcomp.imn.htwk-leipzig.de/concepts/0/Ids%20%5B12,12%5D/10299?_hasdata&SolverNames=1}}
  \label{img:SEP_Concepts_10299_Hasse_Diagramm_W/O_YES}
\end{figure}

### gefilterte Resultat-Tabelle

Die Resultat-Tabelle zeigt die Ergebnisse der Jobs an und ist das zu filternde Objekt.

**Entscheidung:** Die bekannte Resultat-Tabelle bleibt bestehen und zeigt nur die Gegenstände des obersten Formalen Begriff des Hasse-Diagramms an.


\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/SEP_10299_result_table_w_o_yes.pdf}
  \caption{Ausschnitt Resultat-Tabelle}
  \small{\url{http://termcomp.imn.htwk-leipzig.de/concepts/0/Ids%20%5B12,12%5D/10299?_hasdata&SolverNames=1}}
  \label{img:SEP_Concepts_10299_Result_Table_W/O_YES}
\end{figure}

### Navigation

Ein Feature-Wunsch ist die Einführung eines Menüs.

**Entscheidung:** Es wird ein Menü zur besseren Erreichbarkeit einzelner Routen eingeführt.

\newpage

## Anwendung der Formalen Begriffsanalyse
\label{subchapter:impl-applyfca}

Aus dem Theorie-Kapitel sind die theoretischen Komponenten Formaler Kontext, Formaler Begriff und Begriffsverband bekannt. In diesem Kapitel soll die Formale Begriffsanalyse auf die Resultat-Daten der Jobs angewendet werden.

###Formaler Kontext
\label{subchapter:impl-context}

Es wird die Implementierung der Gegenstände, Merkmale und der sich daraus zusammengesetzte Formale Kontext (Kapitel \ref{subchapter:fca-context}) vorgestellt.

Die zellenbeschreibenden `JobPairIDs` sind die Gegenstände. Die Merkmale für StarExec sind durch den algebraischen Datentyp `Attribute` definiert. Das Ausrufezeichen vor einem Typ bedeutet dass dieser Typ nicht lazy ist.

~~~{.haskell caption="Presenter/Model/RouteTypes.hs-L113" label=code:JobPairID}
data JobPairID =
  StarExecPairID Int
  | LriPairID Int
  | UibkPairID Int
  deriving (Show, Read, Eq, Ord)
~~~

~~~{.haskell caption="FCA/StarExec.hs-L17" label=code:Attribute}
data Attribute =
  ASolverBasename !Text
   | AJobResultInfoSolver !Text
   | AYearSpecificSolverName !Text
   | AJobResultInfoConfiguration !Text
   | ASlowCpuTime !Bool
   | ASolverResult !SolverResult
   | ABenchmarkNumberRules !(Maybe Bool)
   | ABenchmarkLeftLinear !(Maybe Bool)
  deriving (Eq, Ord, Show)
~~~

Aus den beiden Typen `JobPairID` als Gegenstand und `Attribute` als Merkmal wird aus dem Typparameter der polymorphen Definition in Kapitel \ref{def-formaler-kontext} der Typ des Formalen Kontextes `Context` zur Compile-Zeit konkret.

\newpage

~~~{.haskell caption="FCA/StarExec.hs-L164 zur Compile-Zeit" label=code:context_runtime}
data Context JobPairID Attribute = Context
  { fore :: Map JobPairID (Set Attribute)
  , back :: Map Attribute (Set JobPairID)
  } deriving (Show)
~~~

Zur Berechnung des Formalen Kontextes dienen alle JobPairIDs aus den gewählten Tabellen als Gegenstände. Die Tabellen werden durch die Routenparameter (siehe \nameref{subchapter:ingrsep}) für `JobIDs` bestimmt. Es werden für jede `JobPairID` alle Merkmale des Types `Attribute` kreiert.

Die Werte aller Konstruktoren von `Attribute` wurden aus dem `JobPair`-Datensatz der Datenbank (siehe Übersichtstabelle \ref{tab:result-data}) abgeleitet. Eine besondere Rolle spielen die Merkmale `ABenchmarkNumberRules` und `ABenchmarkLeftLinear`. Diese beziehen sich auf die `BenchmarkId` des `JobPairs`. Um die Information über die Anzahl der Regeln und die Links-Linearität eines Benchmarks zur Verfügung zu haben, wurde das Haskell-Modul `FCA.Benchmarks` geschrieben. Das Modul durchläuft die aktuelle Version der TPDB (Kapitel \ref{subchapter:tpdb}) um jeden Benchmark zu untersuchen. Mit der neuen Version *1.3.3* des Haskell-Paketes `tpdb`\footnote{\url{https://hackage.haskell.org/package/tpdb}} konnten die geforderten Merkmale für Termersetzungssysteme
berechnet werden. Eine Erweiterung des Persistent-Models `BenchmarkInfo` durch die Felder `numberRules` und `leftLinear` in `config/models` erlaubt den Zugriff auf die Werte in der Datenbank.

~~~{.haskell caption="FCA/StarExec.hs-L61" label=code:attributePairs}
-- get attribute pairs of given job results
attributePairs :: [[JobResult]] -> Handler [(JobPairID, [Attribute])]
~~~

Ein HTML-Formular schließt Gegenstände aus. Das Formular muss mit den zur Auswahl stehenden Merkmalen befüllt werden. Dazu ist es nicht notwendig den Formalen Kontext zu bestimmen. Stattdessen werden im ersten Schritt Paare vom Typ `attributePairs` berechnet. Im zweiten Schritt werden alle zur Verfügung stehenden Merkmale nach ihrem Konstruktor geordnet in eine Mehrfachauswahl\footnote{\url{https://getbootstrap.com/css/\#selects}} eingefügt. Die Abbildung \ref{img:SEP_Concepts_10299_5376_Form} zeigt sechs der Felder mit ihren Optionen. Der \glslink{Algebraischer Datentyp}{Algebraische Datentyp} `Attribute` hat acht Konstruktoren, jedoch werden `AJobResultInfoSolver` und `ASolverBasename` derzeit im Frontend nicht als Option verwendet.

~~~{.haskell caption="FCA/StarExec.hs-L80" label=code:filterPairsByAttributes}
-- filter all attribute pairs by given attribute groups
filterPairsByAttributes :: [(JobPairID, [Attribute])] -> [[Attribute]]
                        -> Maybe [(JobPairID, [Attribute])]
filterPairsByAttributes pairs attrCombinations = do
  let anyMember = or . (\s -> map
                              (\v -> Set.isSubsetOf (Set.fromList v) s)
                              attrCombinations)
  let filteredJobResults = filter
                           (\(_,attrs) -> anyMember $ Set.fromList attrs) pairs
  case filteredJobResults of
    [] -> Nothing
    _ -> Just filteredJobResults
~~~

Nach dem Absenden des Formulars werden die `attributePairs` mit `filterPairsByAttributes` gefiltert. Aus diesen gefilterten Paaren wird der Formale Kontext mit der Funktion `contextFromList` bestimmt.

~~~{.haskell caption="FCA/Basic.hs-L86" label=code:contextFromList}
-- create a context of given input data
contextFromList :: (Ord at, Ord ob) => [(ob, [at])] -> Context ob at
contextFromList l = Context
  { fore=Map.fromListWith
         Set.union $ map (\(ob, attrs) -> (ob, Set.fromList attrs)) l
  , back=Map.fromListWith
         Set.union $ do (ob, attrs) <- l; at <- attrs;
                     return (at,Set.singleton ob)
  }
~~~

### Begriffsverband
\label{subchapter:impl-concepts}

Der Begriffsverband beinhaltet alle Formalen Begriffe (siehe \ref{subchapter:fca-concept}) eines Formalen Kontextes. Im Implementierungskapitel \ref{subchapter:impl-context} wird gezeigt, wie ein Formaler Kontext aus den Routenparametern berechnet wird. Auf Basis des Formalen Kontextes werden die einzelnen Formalen Begriffe berechnet. Der Begriffsverband wird als Hasse-Diagramm (siehe Kapitel \ref{subchapter:impl-hasse-diagramm}) dargestellt.

Ähnlich zum Formalen Kontext wird der polymorphe \gls{Record-Typ} `Concept` aus Kapitel \ref{subchapter:fca-concept} für die Implementierung des Formalen Begriffs konkret.

~~~{.haskell caption="FCA/Basic.hs-L31 zur Compile-Zeit" label=code:concept_runtime}
data Concept JobPairID Attribute = Concept
  { obs :: Set JobPairID
  , ats :: Set Attribute
  } deriving (Show, Eq)
~~~

Zur Vermeidung von überflüssigen Teilmengen mit Elementen einer Merkmalsgruppe wurden die Berechnung aller Teilmengen der Menge $M$ durch die Berechnung von Merkmalskombinationen `attributeCombination` substituiert (siehe Entscheidung in \ref{subchapter:des-attr-set}). In Abbildung \ref{img:SEP_Concepts_10299_5376_Form} wird eine Merkmalsgruppe durch eine Mehrfachauswahl präsentiert.

~~~{.haskell caption="FCA/StarExec.hs-L157" label=code:attributeCombination}
-- create all attribute combinations from existing attributes without duplicates
attributeCombination :: (Ord at) => Context ob at -> [Set at]
attributeCombination context = do
  let attrs = Map.elems $ fore context
  -- using ordNub to reduce duplicate items and keep order
  ordNub . fmap Set.fromList . concatMap (subsequences . Set.toList) $ ordNub attrs
~~~

Zur Berechnung valider - tatsächlich vorkommender Merkmalskombinationen - werden einzig die Potenzmengen von Merkmalen einer JobPairID vorgenommen. Das schließt Teilmengen mit mehreren Merkmalen einer Merkmalsgruppe aus. Das haskell-Paket `ordNub`\footnote{\url{https://github.com/nh2/haskell-ordnub\#dont-use-nub}} dient zum Entfernen von Duplikaten in einer Liste und das Beibehalten der Reihenfolge.

In der allgemeinen Vorschrift für die Berechnung des Begriffsverbandes aus dem Theorie-Kapitel (Definition \ref{def-formaler-begriff}) wird demnach `subsequences` durch die Funktion `attributeCombination` ersetzt. Daraus entsteht die neue Implementierung von `concepts` in Quelltext \ref{code:concepts}. Aus der Reihenfolge der Merkmale entsteht die Reihenfolge der Begriffe des Begriffsverbandes `[Concept ob at]`.

~~~{.haskell caption="FCA/StarExec.hs-L164" label=code:concepts}
-- determine all concepts of given context with StarExec attributes
concepts :: (Ord at, Ord ob, Show ob, Show at)
         => Context ob at -> [Concept ob at]
concepts c = do
  attrs <- attributeCombination c
  let objs = getObjects c attrs
  unless (Set.null objs) . guard $ (attrs == (getAttributes c) objs)
  return (Concept objs attrs)
~~~

Zur Visualisierung des Begriffsverbandes dient ein Hasse-Diagramm. Die Gegenstände eines Begriffs werden durch die Resultat-Tabelle dargestellt.

\newpage

## Integration in Star-Exec-Presenter
\label{subchapter:ingrsep}

Um die Formale Begriffsanalyse nutzen zu können, bedarf es einer neuen Route und zugehörigen Handler in Star-Exec-Presenter. Es wird die Implementierung der Elemente des Yesod Web-Frameworks vorgestellt.

### Concepts-Route
\label{subchapter:impl-route}

Die Concepts-Route hat den folgenden Aufbau:

`/concepts/#ConceptId/#ComplementIds/*JobIds ConceptsR GET`

Der erste Teil ``/concepts/#ConceptId/#ComplementIds/*JobIds` ist der Pfad der Route einschließlich der drei Routenparameter `#ConceptId/#ComplementIds/*JobIds`. Nach einem Leerzeichen folgt der Name der Route *ConceptsR*. Der letzte Teil `GET` gibt an, dass nur \gls{GET}-Requests verarbeitet werden. Aus `ConceptsR` und `GET` ergibt sich der Name der Handler-Funktion `getConceptsR`, der bei Aufruf der URL die Verarbeitung übernimmt.

~~~{.haskell caption="Presenter/Model/RouteTypes.hs" label=code:routeparas}
-- Begriffs-ID
type ConceptId = Int

-- Komplement-IDs
data ComplementIds =
  Ids [ConceptId]
  deriving (Show, Read, Eq, Ord)

-- JobIDs
newtype JobIds = JobIds
  { getIds :: [JobID]
  }
  deriving (Show, Eq, Read)
~~~

An der Implementierung der Routenparameter lässt sich die Parameter-Anzahl erkennen. Es wird nur eine Begriffs-ID, mehrere Komplement-IDs und mehrere JobIDs verwendet. Die Gründe für die Anzahlen ergeben sich aus Erläuterungen in diesem Kapitel. Die JobIDs sind ein Beispiel für einen `Dynamic multi`-Routenparameter. Begriff-ID und Komplement-IDs sind `Dynamic single`-Routenparameter. Für die Implementierung der Komplement-IDs musste ein `Dynamic single` mit einer Liste verwendet werden. Die Gründe dafür sind die begrenzte Anzahl an `Dynamic multi`-Routenparametern in einer Route und die Notwendigkeit von mehreren Komplement-IDs. Es gibt ein HTML-Formular mit dem zusätzlich GET-Parameter in der URL (siehe Beispiel) möglich sind.

\begin{bsp}[Concepts-Route]
\url{http://termcomp.imn.htwk-leipzig.de/concepts/0/Ids%20%5B%5D/10299/5376?_hasdata&SolverNames=1&SolverNames=2}

$\text{ConceptId}: 0$

\text{ComplementIds}: \texttt{Ids\%20\%5B\%5D (}$\hat{=}$ \texttt{URL-escaped Ids [])}

$\text{JobIDs}: 10299 \text{ und } 5376$

$\text{GET-Parameter: SolverNames=}1\text{ und SolverNames}=2$
\end{bsp}

Die Routenparameter skizzieren einen Formalen Kontext. Durch JobIDs werden die Tabellen ausgewählt, aus denen alle Zellen die Gegenstände bilden. Die GET-Parameter des Formulars beschränken diese Gegenstände nach einer Vorschrift (siehe Kapitel \ref{subchapter:impl-context}) auf gewählte Merkmale.


### Concepts-Handler

Der Concepts-Handler ist eine Funktion. Diese Funktion wird mit den Parametern der Route aufgerufen (siehe Quellcode \ref{code:cHandler}). Innerhalb des Concepts-Handler wird die Formale Begriffsanalyse gestartet und visualisiert. Auch alle weiteren Bestandteile wie die Form, das Hasse-Diagramm und die Resultat-Tabelle befinden sich in der Funktion.

~~~{.haskell caption="Handler/Concepts.hs-L36" label=code:cHandler}
-- route with multiselect to choose attributes of JobID
getConceptsR :: ConceptId -> ComplementIds -> JobIds -> Handler Html
~~~

## Visualisierung der Formalen Begriffsanalyse
\label{subchapter:impl-visfca}

Die Darstellung des Begriffsverbandes inklusive der Merkmale ist durch ein Hasse-Diagramm (siehe Definition \ref{def-hasse-diagramm}) realisiert. Die Gegenstände werden in der Resultat-Tabelle angezeigt.

### Hasse-Diagramm
\label{subchapter:impl-hasse-diagramm}

Gemäß den Entscheidungen im Kapitel \ref{dec:hasse} wurde das Hasse-Diagramm implementiert. Den Ablauf für die Erzeugung des Hasse-Diagramm aus Formalen Begriffen `[Concept ob at]` bis zum \gls{SVG} zeigt die folgende Zeile:

\begin{center}
\texttt{[Concept ob at] -> Graph -> DOT -> SVG}
\end{center}

\newpage

Das Hasse-Diagramm wird aus den Begriffen `[Concept ob FSE.Attribute]` und den beiden URLs in der Funktion `renderConceptSVG` berechnet. In der Funktion `dottedGraph` wird für jeden Formalen Begriff ein Knoten erstellt. Falls *Begriff I* Oberbegriff von *Begriff II* ist, wird eine gerichtete Kante von *Begriff I* zu *Begriff II* erzeugt. In den nächsten Schritten wird aus den Knoten und Kanten ein Graph und aus diesem \glslink{DOT}{DOT-Code}\footnote{http://www.graphviz.org/Documentation/dotguide.pdf} erzeugt. Aus dieser Graphviz\footnote{https://hackage.haskell.org/package/graphviz}-internen Beschreibungssprache wird danach ein \gls{SVG} generiert. Dieses SVG kann nativ im Hamlet-Template verwendet werden.

~~~{.haskell caption="FCA/DotGraph.hs-L24" label=code:dot}
renderConceptSVG :: (Eq ob, Show ob, MonadIO m) => [Concept ob FSE.Attribute]
                 -> [T.Text] -> [T.Text]-> m B.Markup
renderConceptSVG concepts' nodeURLs complURLs = do
  svg <- liftIO . readProcess "dot" [ "-Tsvg" ]
         $ dottedGraph concepts' nodeURLs complURLs
  return . B.preEscapedLazyText . TL.pack . L.unlines
         . L.dropWhile ( not . L.isPrefixOf "<svg" ) $ L.lines svg

dottedGraph :: (Eq ob, Show ob) => [Concept ob FSE.Attribute]
            -> [T.Text] -> [T.Text] -> String
dottedGraph conceptLattice nodeURLs complURLs = do
  let graph_params = getGraphParams conceptLattice nodeURLs complURLs
  let graphWithTransEdges = G.graphToDot graph_params
                            $ createGraph conceptLattice
  (TL.unpack . renderDot) . toDot $ transitiveReduction graphWithTransEdges
~~~

Der oberste Knoten des Hasse-Diagramms entspricht dem Begriff mit dem Index der `ConceptId` (aus der `Concepts-Route`) in der Liste des Begriffsverbandes `[Concept ob at]`. Bei einem vollständigem Begriffsverband entspricht das dem Formalen Begriff mit allen Gegenständen des Formalen Kontext mit deren gemeinsamen Merkmalen.

In einem Formalem Kontext mit zu vielen Merkmalen und einem daraus resultierenden Begriffsverband mit mehr als $50$ Begriffen\footnote{Die 50 Begriffe wurden empirisch ermittelt. \href{http://termcomp.imn.htwk-leipzig.de/concepts/0/Ids\%20\%5B\%5D/9515/10299?_hasdata=&SolverNames=1&SolverNames=2&SolverNames=3&Results=1}{Ein Beispiel mit mehr als 50  Begriffen} verdeutlicht die Problematik.} wird die Darstellung sehr problematisch. Dieses Problem tritt häufig bei dem Vergleich von mehreren JobIDs auf. Es ist nicht mehr möglich den Begriffsverband in einer Mindestgröße darzustellen. Um diesem Problem zu begegnen, wurde entschieden, dass nach Definition prädestinierte \gls{SVG} in das \gls{jQuery}-Plugin jQuery Panzoom\footnote{\url{https://github.com/timmywil/jquery.panzoom/}} einzubetten.

Die Abbildung \ref{img:SEP_Concepts_10299_Hasse_Diagramm_W/O_YES} zeigt deutlich die Hasse-Diagramm-Struktur einschließlich des Supremum. Einen Ausschnitt aus einem Hasse-Diagramm mit mehr Begriffen einschließlich Zoom zeigt die Grafik \ref{img:SEP_Concepts_10299_Hasse_Diagramm}.

\newpage

### Resultat-Tabelle
\label{subchapter:impl-table}

Eine Tabelle dient zur Darstellung der JobPairs deren *JobPairIds* im aktuellen Begriff (Routenparameter `ConceptId`) vorhanden sind. Durch die Verwendung von Komplementen (siehe \ref{subchapter:impl-compl}) kann der Formale Kontext außerdem um Gegenstände beschnitten sein. Um genau die Gegenstände anzuzeigen, werden in `currObjects` die aktuellen Gegenstände des Begriffsverbandes bestimmt. In dem Bezeichner `filteredJobResults` werden alle JobPairIds herausgefiltert, die sich nicht in `currObjects` befinden. Die bestehende Implementierung für die Darstellung der Resultate (bisherige Verwendung in \nameref{subchapter:smjr} und \nameref{subchapter:flexible-table}) wurde nur minimal angepasst. Die Anzeige des *Scorings* - die Anzahl der Solver-Antwort `YES` pro Solver - wurde entfernt, da diese Information bereits im Hasse-Diagramm verfügbar ist. Die Abbildung \ref{img:SEP_Concepts_10299_Result_Table_W/O_YES} zeigt einen Ausschnitt aus dem Begriffsumfang des obersten Begriffs des Hasse-Diagramms in \ref{img:SEP_Concepts_10299_Hasse_Diagramm_W/O_YES}.

~~~{.haskell caption="Handler/Concepts.hs-L71" label=code:concepts-currObjects}
let currObjects = maybe
                    Set.empty
                    (\ c -> safeGetConceptObjectsFromLattice c cid)
                    concepts'

let filteredJobResults = fmap
                        ((wrapResults .
                          filter
                          (\jr -> Set.member (getPairID jr) currObjects)
                          . getStarExecResults)
                          . snd . queryResult)
                        qJobs
~~~

\newpage

## Interaktion und Navigation
\label{subchapter:impl-internav}

Jeder Knoten des Hasse-Diagramms besitzt zwei Arten von Links (\nameref{subchapter:impl-compl} und \nameref{subchapter:impl-incl}) zur Interaktion.

Innerhalb der `Concepts-Seite` gibt es verschiedene Möglichkeiten der Navigation. Es gibt zwei Reset-URLs, eine um die Auswahl im Formular und eine weitere um die Komplemente auf der `Concepts-Seite` zurückzusetzen. Beim Klicken auf einen Knoten im Hasse-Diagramm wird der Begriffsumfang angezeigt. Die Resultat-Tabelle ist nahezu die gleiche wie die in `ShowManyJobResults` und besitzt aus diesem Grund auch ähnliche Navigationsmöglichkeiten. Die Unterschiede sind in Kapitel \ref{subchapter:impl-table} beschrieben.

Ein Menü soll unter anderem die `Concepts-Seite` auf der ganzen Seite besser erreichbar machen.

### Begriffsverband-Teilmenge
\label{subchapter:impl-incl}

Der aktuelle Begriffsverband beinhaltet alle Formalen Begriffe, die sich aus den gemeinsamen Merkmalen aller Gegenstände des Formalen Kontextes ergeben. Die Begriffsmenge soll sich auf eine Teilmenge einschränken lassen. Zur Auswahl der Teilmenge dient eine Merkmalmenge eines Begriffes.

~~~{.haskell caption="FCA/Basic.hs-L76" label=code:reduceConceptsToProperSubsets}
-- reduce concepts to concepts with proper subsets of given concept id
reduceConceptsToProperSubsets :: (Ord at) => Maybe [Concept ob at] -> ConceptId
                              -> Maybe [Concept ob at]
reduceConceptsToProperSubsets conceptLattice cid =
    case conceptLattice of
      Nothing -> Nothing
      Just concepts' -> do
        let concept = safeGetIndex concepts' cid
        case concept of
          Nothing -> Nothing
          Just c  -> return $ c:filter (Set.isProperSubsetOf (ats c) . ats) concepts'
~~~

In der Implementierung `reduceConceptsToProperSubsets` werden alle Begriffe herausgefiltert, deren Merkmale nicht echte Teilmenge der Merkmale des ausgewählten Begriffes sind. Der Begriff selbst soll Teil dieser Teilmenge des Begriffsverbandes bleiben. Abbildung \ref{img:SEP_Concepts_10299_Hasse_Diagramm_YES_AProVE} begrenzt den Begriffsverband auf Begriffe mit den Merkmalen $\{\text{YES, AProVE}\}$. An den `Concepts-Handler` wird per Routenparameter (Kapitel \ref{subchapter:impl-route}) `ConceptId` die ID eines Begriffes übergeben. Der erste Begriff in `[Concept ob at]` beschreibt den allgemeinsten Begriff, mit allen Gegenständen und deren gemeinsamen Merkmalen.

### Begriffsverband-Komplement
\label{subchapter:impl-compl}

Um Begriffe aus dem Begriffsverband zu lösen, wurde die Möglichkeit einer Komplementbildung implementiert. Durch die Wahl eines  Begriffes werden alle zu diesem gehörenden Gegenstände aus dem Formalen Kontext entfernt. Um mehrere Begriffe aus dem Kontext entfernen zu können, wurde die Möglichkeit eines *chainings*, also dem verketteten Entfernen von Begriffen implementiert.

~~~{.haskell caption="FCA/StarExec.hs-L211" label=code:reducePairsByComplements}
-- recursive function to calculate concepts list and reduce attribute pairs
-- by objects of concept at first index of complement list.
-- Each complement list element refers to the index of the current concepts list
reducePairsByComplements :: [(JobPairID, [Attribute])] -> ComplementIds
                         -> [(JobPairID, [Attribute])]
reducePairsByComplements pairs (Ids complIds) = case complIds of
  []   -> pairs
  compl:compls -> case pairs of
    []         -> []
    _ -> do
      let concept = safeGetIndex (concepts $ contextFromList pairs) compl
      case concept of
        Nothing -> reducePairsByComplements pairs $ Ids compls
        Just c  -> reducePairsByComplements
                   (filterPairsByObjects pairs $ obs c) $ Ids compls
~~~

Die rekursive Funktion `reducePairsByComplements` erhält eine Liste von Gegenstand-Merkmal-Paaren `pairs` als Vorstufe eines Formalen Kontextes und Komplement-Ids. Aus `pairs` wird der Formale Kontext und alle Begriffe bestimmt. Für die erste `ConceptId` der Komplement-Ids wird der Begriff via Index und dessen Gegenstände bestimmt. Diese Gegenstände werden aus der Liste von Paaren entfernt und es folgt ein weiterer Aufruf der Funktion mit der neuen Liste `pairs` und den verbleibenden Komplement-Ids. In der `Concepts-Route` werden die Komplement-Ids als Routenparameter übergeben. Die Implementierung der `ComplementIds` in Kapitel \ref{subchapter:impl-route} als `Ids [ConceptId]` zeigt den Zusammenhang zwischen Komplementbildung und den Ids der Formalen Begriffen.

Die Abbildung \ref{img:SEP_Concepts_10299_Hasse_Diagramm_W/O_YES} zeigt die verbleibenden Begriffe nachdem alle Gegenstände mit den Merkmalen $\{\text{YES}\}$ entfernt wurden.

\newpage

### Menü
\label{subchapter:menu}

Mit Graphviz wurde manuell ein Graph (zu sehen in Abbildung \ref{img:nav}) erstellt, der die Verlinkung ausgewählter Routen innerhalb von Star-Exec-Presenter aufzeigt. Ein Knoten entspricht einer Route auf Star-Exec-Presenter und eine gerichtete Kante von *Route I* nach *Route II* entspricht einem Link auf *Route I* nach *Route II*. Es ist deutlich die zentrale Position der grau gefärbten Routen für \nameref{subchapter:smjr} und \nameref{subchapter:flexible-table} zu sehen. Der Nutzer hat eingeschränkte Möglichkeiten um diese Routen von `HomeR` zu besuchen.

\begin{figure}[H]
  \centering
  \includegraphics[width=\textwidth]{content/figures/navigation.pdf}
  \caption{Star-Exec-Presenter: Navigation nach Optimierung}
  \label{img:nav}
\end{figure}

\newpage

Die mit blau hervorgehobenen Knoten und Kanten sind durch die Implementierung der `Concepts`-Route und das Hinzufügen eines einfachen Navigationsmenüs (in Quellcode \ref{code:menu}) entstanden. In der Implementierung des Menü wird zwischen zwei verschiedenen Menü-Elementen im Datentyp `MenuElement` unterschieden. Zum Einen ein einfacher Menü-Eintrag `MenuEntry` und zum Anderen ein Dropdown-Menü `MenuDropdown`. Ein `MenuEntry` besitzt nur einen Namen und eine Route, wohingegen `MenuDropdown` einen Text und eine Liste von `MenuEntry` besitzt. Die Dropdown-Menü-Einträge erlauben eine Gruppierung nach Themen wie "Analyse" oder "\gls{StarExec}-Interaktion". Das Menü hat zur Folge, dass Einträge in diesem von allen Knoten verfügbar sind (im Beispiel `ConceptsR` deutlich in Abbildung \ref{img:nav} zu sehen). Die Menü-Einträge für `Competitions` existieren nur aus Gründen der Illustration und wurden nicht implementiert.

~~~{.haskell caption="Foundation.hs" label=code:menu}
-- navigation bar types
data MenuElement = MenuEntry (Text, Route App) | MenuDropdown (Text, [MenuElement])

-- navigation bar elements
let menuElements = [MenuEntry ("Home", HomeR)
                   , MenuDropdown ("Competitions"
                     , [MenuEntry ("2015", CompetitionR)
                     , MenuEntry ("2014", CompetitionR)
                   ])
                   , MenuDropdown ("Analysis"
                     , [MenuEntry ("TRS Standard 2015", ConceptsR 0 (Ids []) (JobIds [StarExecJobID 10257]))
                     , MenuEntry ("TRS Standard 2014", ConceptsR 0 (Ids []) (JobIds [StarExecJobID 5373]))
                     , MenuEntry ("Example many jobs", ConceptsR 0 (Ids []) (JobIds [StarExecJobID 9515, StarExecJobID 10299]))
                   ])
                   , MenuDropdown ("StarExec Interaction"
                     , [MenuEntry ("Import", ImportR)
                     , MenuEntry ("Install Solver", InstallSolversR Y2015)
                     , MenuEntry ("Job Control", ControlR Y2015)
                   ])]
~~~

\newpage

## Optimierungen
\label{subchapter:impl-opt}

Die aktuelle Optimierung kann in verschiedenen Aspekten verbessert werden. Zwei konkrete Optimierungen werden erläutert. In der aktuellen Implementierung wird bereits die Bibliothek `begriff`\footnote{\url{https://gitlab.imn.htwk-leipzig.de/waldmann/begriff}} verwendet, die Optimierungen vorgenommen hat. Diese Optimierungen wurden nicht berücksichtigt.

### Datentyp des Begriffsverband

Die Nutzung des Datentyps `[Concept ob at]` ist nicht ideal. Die Liste beinhaltet die einzelnen Formalen Begriffe des Begriffsverbandes, jedoch keine Information über die Beziehung zwischen diesen. Bei der Bildung der Begriffsverband-Teilmengen (Quelltext \ref{subchapter:impl-incl}) und bei Berechnung der Kanten für das Hasse-Diagramm (Quelltext \ref{subchapter:impl-hasse-diagramm}) sind die Beziehungen zwischen den Begriffen hilfreich. Bei der Erstellung des Hasse-Diagramms wird aus der Liste von Begriffen ein Graph aufgebaut. Wenn die Funktion zur Berechnung der Begriffe `concepts` bereits einen Graphen als Repräsentation des Begriffsverbandes als Resultat hätte, könnten verschiedene Berechnungen eingespart werden. Mit einer zusätzlichen Funktion könnten zu einem Knoten alle in Oberbegriff-Relation stehenden Knoten abgefragt werden. Das würde die angesprochenen Probleme optimieren.

### Codierung der Merkmale

In der Implementierung der Merkmale `Attribute` (Quelltext \ref{code:Attribute}) wird für einige Felder der Datentyp `Text` verwendet. Bei dem Algorithmus zur Bestimmung des Begriffsverbandes `concepts` (Quelltext \ref{code:concepts}) und der Berechnung der Merkmalkombinationen `attributeCombination` (Quelltext \ref{code:attributeCombination}) werden exzessiv Vergleiche der Merkmale und somit Vergleiche von `Text` verwendet. Besser wäre jedes Merkmal durch einen Hash zu ersetzen, um in konstanter Zeit den Vergleich von zwei Merkmalen beantworten zu können.
