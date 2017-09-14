# Haskell
\label{chapter:haskell}

Die Programmiersprache Haskell wird als Modellierungs- und Implementierungssprache verwendet. Der Vorteil der Verwendung von Haskell als Modellierungssprache ist die einfache Übertragung der mathematischen Spezifikation in Programmcode. Die Implementierungssprache ist durch die bestehende Web-Applikation Star-Exec-Presenter vorgegeben. Es werden verwendete Features von Haskell und des Yesod Web-Frameworks erläutert.

---

## Die Sprache

\blockquote{"Haskell is a general purpose, purely functional programming language incorporating many recent innovations in programming language design. Haskell provides higher-order functions, non-strict semantics, static polymorphic typing, user-defined algebraic datatypes, pattern-matching, list comprehensions, a module system, a monadic I/O system, and a rich set of primitive datatypes, including lists, arrays, arbitrary and fixed precision integers, and floating-point numbers. Haskell is both the culmination and solidification of many years of research on non-strict functional languages." \cite[S.3]{Haskell2010}}

In diesem Zitat aus dem *Haskell 2010 Language Report*\cite{Haskell2010} werden die wichtigsten Sprach-Feature genannt. Ergänzend sei anzumerken, das die Innovationen aus Haskell eine Vielzahl von Programmiersprachen wie *C#*, *Isabelle*, *Java*, *Python*, *Scala* und neue Sprachen wie *Rust* beeinflusst haben \cite[S.45ff]{HaskellHistory} \cite{RustRef}. Demnach spielt Haskell eine wichtige Rolle unter den Programmiersprachen. Erklärungen und allgemeine Beispiele führen einzelne Features der Sprache aus. Quellcode-Verweise für die Verwendung(en) der Sprach-Elemente in der Arbeit werden neben dem Feature in Klammern angegeben.

*Anmerkung:* Einige der Sprach-Feature verfügen über kein passendes deutsches Pendant und wurden aus dem Englischen übernommen.

\newpage

**Funktionale Programmierung**

In der funktionalen Programmierung werden Programme durch Ausdrücke\footnote{engl. expressions} in Form von Funktionen beschrieben. Im Gegensatz zu Programmen, die in imperativen Programmiersprachen wie *Java* oder *C* geschrieben sind. In diesen wird ein globaler Zustand durch eine Folge von Anweisungen\footnote{engl. statements} verändert. In der funktionalen Programmierung ist das nicht möglich. Ein Wert hat für den gesamten Programmablauf den gleichen Wert. \cite[S.XVff]{lyah} Weitere entscheidende Eigenschaften sind *Lazy Evaluation*, *pure functions* und *Funktionen höherer Ordnung* (es sei auf die zugehörigen Abschnitte verwiesen).

**Lazy Evaluation**

Haskell ist *lazy* und wertet keinen Ausdruck aus, bevor er nicht tatsächlich benötigt wird \cite[S.XVff]{lyah}. Das Beispiel \ref{code:lazyCompre} zeigt die Verwendung einer unendlichen Liste von der nur die ersten 7 Elemente berechnet werden. Mit dem Rest der Liste passiert nichts, da dieser nicht gebraucht wird.

**List Comprehension**

*List Comprehension* ist eine kompakte Form Listen durch Generator-Ausdrücke zu erzeugen, die speziellen Bedingungen genügen. Im Beispiel \ref{code:lazyCompre} werden durch `[x*x | x <- [1..]]` die Quadratzahlen aller Zahlen ab 1 berechnet. Der Ausdruck `even x` schränkt die generierten Werte für `x` auf gerade Zahlen ein.

~~~{.haskell caption="List Comprehension und Laziness" label=code:lazyCompre}
take 7 [x*x | x <- [1..], even x]
~~~

**Pure Functions** [\ref{code:getAttributes},\ref{code:getObjects}, \ref{code:concepts_basic}]

In der rein funktionalen Programmiersprache Haskell sind Funktionen *pure\footnote{engl. für rein/pur}*. Das bedeutet eine Funktion ist frei von Nebenwirkungen. Bei mehrmaligem Aufruf einer *pure function* mit den gleichen Argumenten erhält man stets das gleiche Resultat. \cite[S.153ff]{lyah} Das Beispiel \ref{code:pure} zeigt zwei Aufrufe von *pure functions*. Haskell bietet auch die Möglichkeiten Funktionen mit Nebeneffekten zu handhaben, dazu wurden Monaden eingeführt.

~~~{.haskell caption="Pure Function" label=code:pure}
length [0..99]

sin (pi/2)
~~~

\newpage

**Funktionen höherer Ordnung** [\ref{code:getAttributes},\ref{code:getObjects}, \ref{code:concepts_basic}, \ref{code:concepts-currObjects}]

Funktionen höherer Ordnung sind Funktionen, die Funktionen als Argument oder Rückgabewert akzeptieren \cite[S.63]{lyah}. Ein Beispiel (\ref{code:highorderLambda}) für eine Funktion höherer Ordnung ist `map`. Im Beispiel erhält `map` eine anonyme Funktion durch einen Lambda-Ausdruck und wendet diese Funktion auf jedes Element der Liste `[0..5]` an.

**Lambda-Ausdruck** [\ref{code:getObjects}, \ref{code:getAttributes}, \ref{code:concepts-currObjects}, \ref{code:contextFromList}]

Ein Lambda-Ausdruck bezeichnet eine anonyme Funktion. Es wird ein Lambda-Ausdruck statt einer Funktion verwendet, wenn dieser nur einmal benötigt wird. Das Beispiel \ref{code:highorderLambda} zeigt eine einfache Funktion die jedes Argument `x` inkrementiert.

~~~{.haskell caption="Funktion höherer Ordnung und Lambda" label=code:highorderLambda}
map (\x -> x +1) [0..5]
~~~

**Algebraischer Datentyp** [\ref{code:JobPairID},\ref{code:Attribute}, \ref{code:menu}]

Ein algebraischer Datentyp erlaubt die Kombination von Typen durch die algebraischen Operationen Summe und Produkt. Die Summe der Konstruktoren ergibt den algebraischen Typ. Ein Konstruktor ist eine Alternative zum Instanziieren des algebraischen Typs. Jeder Konstruktor stellt mit seinen Subtypen ein Produkt dar. Demnach ist ein algebraischer Datentyp eine Summe von Produkten. Das Beispiel \ref{code:algebraDT} zeigt den algebraischen Datentyp `Tree` mit der Summe aus den beiden Konstruktoren `Leaf` und `Branch`. `Leaf` ist ein einstelliges und `Branch` ein zweistelliges Produkt. \cite[S.15]{Haskell2010}

~~~{.haskell caption="Algebraischer Datentyp" label=code:algebraDT}
data Tree a = Leaf a | Branch (Tree a) (Tree a)
~~~

**Statisch polymorphe Typisierung** [\ref{code:context},\ref{code:concept}, \ref{code:context_runtime}]

Haskell ist eine statisch typisierte Programmiersprache - das heißt zur Compile-Zeit stehen alle Typen fest und ändern sich zur Laufzeit nicht, wie das bei dynamisch typisierten Sprachen möglich ist. Das hat den Vorteil das Fehler frühzeitig durch den Compiler erkannt und Typ-Fehler zur Laufzeit vorgebeugt werden. Das Beispiel \ref{code:algebraDT} zeigt den Typparameter `a` für den beliebige Typen wie beispielsweise `String` oder `Maybe Int` eingesetzt werden können.

**Record-Typ** [\ref{code:context},\ref{code:concept}, \ref{code:context_runtime}, \ref{code:concept_runtime}]

Haskell stellt mit dem Record-Typ einen Datentyp mit benannten Feldern zur Verfügung \cite[S.16]{Haskell2010}. Alle Felder müssen bei Instanziierung angegeben werden. Aus den Namen der Felder leiten sich Funktionen ab, die das Abfragen der Werte des Records ermöglichen. Im Beispiel \ref{code:recordTypeclass} wird eine Person mit einem Namen und einem Alter deklariert.

\newpage

~~~{.haskell caption="Record-Typ und Typklassen" label=code:recordTypeclass}
data Person = Person
              {name :: Text
              , age :: Int
              } deriving {Eq,Show}
~~~


**Typklassen** [\ref{code:context},\ref{code:concept}, \ref{code:Attribute}, \ref{code:JobPairID}]

Typklassen definieren durch eine Menge von Funktionen ein Verhalten \cite[S.63]{lyah}. Je nach Typ für den die Typklasse implementiert wird, kann die Implementierung und das Verhalten unterschiedlich sein \cite[S.136]{rwhs}. Es ist für Funktionen möglich die Implementierung einer Typklasse vorauszusetzen, um sicher zu gehen, dass das Verhalten für den Typ sichergestellt ist. Das Beispiel \ref{code:typeclass} zeigt die Typklasse `Eq`, die instanziiert sein muss, um den Gleich- und Ungleichheitsoperator *==*, */=* verwenden zu können. Mit der Implementierung der Typklassenfunktionen für `Eq` wird definiert, wann ein Typ `a` gleich und ungleich ist. Der Haskell-Compiler kann die Instanzen für `Eq`, `Show` und andere Typklassen selbst ableiten \cite[S.136]{rwhs}. Bei einer Datentyp-Deklaration wird diese automatische Ableitung mit dem Schlüsselwort `deriving` (siehe Beispiel \ref{code:recordTypeclass} für die Ableitung der Typklassen `Eq` und `Show`) eingeleitet.

~~~{.haskell caption="Typklassen" label=code:typeclass}
class Eq a where
    (==) :: a -> a -> Bool
    (/=) :: a -> a -> Bool
~~~

**Pattern Matching** [\ref{code:filterPairsByAttributes} \ref{code:reduceConceptsToProperSubsets} \ref{code:reducePairsByComplements}]

Mit *pattern\footnote{engl. für Muster} matching* wird auf eine übersichtliche Art und Weise Datentypen von oben nach unten auf eine Reihe von Muster geprüft und bei Übereinstimmung entsprechende Aktionen ausgelöst. Für jeden Konstruktor des Datentyps sollte ein Fall der Behandlung existieren. Ein Muster erlaubt die Zerlegung des Typs in seine Bestandteile und den Zugriff auf dessen Werte. Das Beispiel \ref{code:patternm} zeigt eine Reimplementierung der partiellen Funktion `head`, die das erste Element einer Liste zurückgibt. Im ersten Fall der Funktion `maybeHead` wird auf die leere Liste `[]` geprüft und `Nothing` zurückgegeben. Der zweite Fall spaltet die Liste in das erste Element und den Rest der Liste auf. Das erste Element wird mit `Just x` zurückgeben.

~~~{.haskell caption="Pattern Matching" label=code:patternm}
maybeHead :: [a] -> Maybe a
maybeHead [] = Nothing
maybeHead (x:_) = Just x
~~~

\newpage

**Monaden** [\ref{code:filterPairsByAttributes}, \ref{code:contextFromList}, \ref{code:concepts}]

Eine Monade beschreibt einen Berechnungskontext und wird in Haskell genutzt, um *pure code* von *inpure code* zu trennen. Bei der Verwendung einer Monade wird ein Wert in den Berechnungskontext dieser Monade verpackt und muss zunächst wieder entpackt werden, um diesen in *pure functions* verwenden zu können. Eine Monade implementiert die Typklasse `Monad` und ihre Funktionen. Beispielanwendungen für Monaden sind der Zugriff auf eine Datenbank und Dateien. Beispiel-Monaden sind die `Handler`-Monade in Yesod \ref{subchapter:yesod}, der `Maybe`-Datentyp sowie die Listen-Datenstruktur `[a]`. Die Verwendung des Schlüsselwortes `do` ist eine Kurzschreibweise um einfacher auf die Werte innerhalb Monaden zugreifen und diese mit Werten anderer Monaden verknüpfen zu können. Die Funktion `maybeHead` im Quelltext \ref{code:patternm} gibt einen Wert oder nichts im Kontext der `Maybe` Monade zurück.

**Modulsystem**

Ein Modul beinhaltet Werte, Funktionen, Datentypen, Typklassen usw., ein Haskell-Programm wiederum eine Sammlung von Modulen \cite[S.61]{Haskell2010}. Mit dem Schlüsselwort `export` werden Bestandteile aus einem Modul anderen Modulen zur Verfügung gestellt. Mit `import` wird in einem Modul Bestandteile anderer Module verfügbar gemacht.

## Yesod Web Framework
\label{subchapter:yesod}

\blockquote{"We want a language that gives us guarantees that our code is doing what it should. Instead of writing up a unit test to cover every bit of functionality in our application, wouldn’t it be wonderful if the compiler could automatically ensure that our code is correct? And as an added bonus, wouldn’t it be nice if our code ran quickly too? Yesod is a web framework bringing the strengths of the Haskell programming language to the web development world." \cite[S.5]{yesod}}


Da Star-Exec-Presenter in Yesod geschrieben ist, sollen die Eigenschaften von Yesod kurz zusammengefasst werden. Yesod\footnote{\url{http://www.yesodweb.com/}} ist ein Haskell Web-Framework und wurde von Michael Snoyman\footnote{\url{http://www.snoyman.com/}} initiiert. Yesod besteht aus mehreren Sprachen sogenannten DSLs\footnote{(Domain Specific Language) engl. für anwendungsspezifische Sprache}. Diese Sprachen besitzen je nach ihrem Aufgabengebiet eine unterschiedliche Syntax. Es werden in der Arbeit verwendete DSLs weitere wichtige Features vorgestellt und ausgeführt.

\newpage

**Routing und Handler**

Die Routing-DSL beschreibt die auf dem Server zur Verfügung gestellten Routen. Das Beispiel \ref{code:route} zeigt eine einfache Route aus dem Star-Exec-Presenter. Der Pfad der Route ist `/flexible-table/#Query/*JobIds` und beinhaltet die zwei Routenparameter `#Query` und `*JobIds`. Es wird zwischen verschiedenen Arten von Routenparametern unterschieden. Parameter mit einem `*` beginnend werden als `Dynamic single` bezeichnet und besitzen beim Aufruf der \gls{URL} nur ein Parameter. `Dynamic multi`-Routenparameter wird ein `#` vorangestellt. \cite[S.60]{yesod} Diese Art von Routenparameter darf nur einmal am Ende einer URL vorkommen und besitzt mehrere Parameter. Der Grund für das Vorkommen am Ende einer URL ist die Zuordnung der URL-Parameter zu den richtigen Routenparametern. \cite[S.60]{yesod} Der Name der Route ist `FlexibleTableR` und beantwortet `GET`-Requests\footnote{engl. für Anfragen}. \cite[S.16]{yesod}

~~~{.haskell caption="config/routes-L48" label=code:route}
/flexible-table/#Query/*JobIds FlexibleTableR GET
~~~

Den Routen muss eine Funktion zugeordnet werden, die auf den Aufruf reagiert. Diese Funktion wird im Yesod Web-Framework `Handler` genannt. Der Name für die Funktion und das Modul in dem die Funktion beschrieben wird, leiten sich aus dem Routenname und der HTTP-Methode ab. \cite[S.17]{yesod} Für das Beispiel \ref{code:route} ergeben sich der Modulname `FlexibleTable.hs` und `getFlexibleTableR` als Funktionsnamen (siehe Beispiel \ref{code:handler}).

~~~{.haskell caption="Handler/FlexibleTable.hs-L6" label=code:handler}
getFlexibleTableR :: Query -> JobIds -> Handler Html
getFlexibleTableR = getShowManyJobResultsR Standard
~~~

**Persistent als Datenbank-Backend**

Eine weitere anwendungsspezifische Sprache ist Persistent\footnote{\url{https://www.stackage.org/package/persistent}} - die Datenbank-Schnittstelle mit verschiedenen Backends \cite[S.93ff]{yesod}. Das Beispiel \ref{code:persist-model} zeigt ein Model zur Beschreibung einer `Competition` mit dessen Feldern. Eine Datenbank-Anfrage zum Abfragen von öffentlichen Competitions zeigt die Funktion `getPersistPublicCompetitions'` im Beispiel \ref{code:persist-db}.

~~~{caption="config/models-L180" label=code:persist-model}
CompetitionInfo
    competition Competition
    date UTCTime default=now()
    public Bool default=True
    deriving Show
~~~

~~~{.haskell caption="Handler/Presenter/PersistHelper.hs-L93" label=code:persist-db}
getPersistPublicCompetitions' :: YesodDB App [Entity CompetitionInfo]
getPersistPublicCompetitions' = selectList [ CompetitionInfoPublic ==. True ] [ Desc CompetitionInfoDate ]
~~~

**Hamlet für HTML-Templates**

Das Template-System von Yesod verwendet die Template-Sprachen der Shakespeare-Familie\footnote{\url{https://hackage.haskell.org/package/shakespeare}} \cite[S.23]{yesod}. Das Shakespeare-Paket beinhaltet Template-Sprachen für HTML, CSS\footnote{(Cascading Style Sheets) Deklarative Sprache, die die Darstellung einer Webseite kontrolliert (\url{https://developer.mozilla.org/en-US/docs/Glossary/css})} und Javascript. Alle DSLs sind typsicher und werden statisch compiliert. Im Folgenden wird nur auf Hamlet als DSL für HTML eingegangen, da die Verwendung diese Template-Sprache im Rahmen der Arbeit den größten Anteil besaß. Das Beispiel \ref{code:html-template} zeigt einen Ausschnitt des Hamlet-Templates zur Darstellung der Resultat-Tabelle (siehe Abbildung \ref{img:SEP_SMJR_9515}). Schließende HTML-Tags sind nicht notwendig, da durch Einrückungen der Scope geklärt wird. CSS-Klassen werden standardmäßig verwendet. Es stehen Template-typische Kontrollelemente wie eine for-Schleife zur Verfügung um im Beispiel über alle Benchmarks zu iterieren. Yesod-Routen können mit einem @-Zeichen beginnend verwendet werden.

~~~{.html caption="templates/listbenchmarks.hamlet" label=code:html-template}
<h2>All solvers listed in the database

<div class="container-fluid">
  <table class="table table-condensed">
    <thead>
      <tr>
        <th>Name
    <tbody>
      $forall benchmark <- benchmarks
        <tr>
          <td><a href=@{ShowBenchmarkInfoR $ toBenchmarkID benchmark}>#{toBenchmarkName benchmark}</a>
~~~


