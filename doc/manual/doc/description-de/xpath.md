title: Handbuch Publisher
---
XPath Ausdrücke
===============

Der Publisher akzeptiert in den den entsprechend markierten Attributen
(zumeist `select` und `test`) XPath Ausdrücke. In allen anderen
Attributen kann durch die geschweiften Klammern (`{` und `}`) ein XPath
Ausdruck erzwungen werden. In diesem Beispiel werden im Attribut
`width` und im Element `Value` die Werte dynamisch erzeugt, d.h. für die
Angabe der Breite wird auf den Inhalt der Variablen `breite`
zurückgegriffen, der Inhalt des Absatzes ist der Inhalt (Textwert) des
gerade aktuellen Datenknotens.

    <Textblock width="{$breite}" fontface="text" textformat="Text mit Einrückung">
      <Paragraph>
        <Value select="."/>
      </Paragraph>
    </Textblock>

Folgende XPath-Ausdrücke erkennt das System:
--------------------------------------------

-   Zahl: gibt den Wert direkt zurück. Beispiel: `"5"`
-   Text: gibt den Wert direkt zurück. Beispiel: `'Text'`
-   Rechenoperationen (`*`, `div`, `idiv`, `+`, `-`, `mod`). Beispiel:
    `( 6 + 4.5 ) * 2`
-   Zugriff auf Variablen. Beispiel: `$spalte + 2`
-   Zugriff auf den aktuellen Knoten (Punkt-Operator). Beispiel: `. + 2`
-   Zugriff auf Unterelemente. Beispiel: `produktdaten`, `node()`, `*`, `foo/bar`
-   Zugriff auf Attribute im aktuellen Knoten. Beispiel `@a`
-   Zugriff auf Attribute in Kindelementen, zum Beispiel `foo/@bar`.
-   Boolesche Ausdrücke: `<`, `>`, `<=`, `>=`, `=`, `!=`. Vorsicht, das
    Zeichen `<` muss in XML als `&lt;` geschrieben werden, das Zeichen
    `>` kann als `&gt;` geschrieben werden. Beispiel: `$zahl > 6`. Kann
    in Bedingungen benutzt werden.
-   Einfache `if/then/else`-Abfragen: `if (...) then ... else ...`

Folgende XPath-Funktionen stehen bereit:
----------------------------------------

Es gibt zwei Klassen von XPath Funktionen: standardkonforme und speedata
Publisher spezifische Funktionen. Die spezifischen Funktionen sind im
Namensraum `urn:speedata:2009/publisher/functions/de` (im Folgenden mit
`sd:` gekennzeichnet). Die standard-Funktionen sollten sich wie XPath
2.0 verhalten.


Funktion | Beschreibung
---------|-------------
sd:current-framenumber(\<name\>)|  Gibt die Nummer des aktuellen Rahmens im Platzierungsbereich zurück.
sd:current-page()|  Gibt die Seitennummer zurück.
sd:current-row(\<name\>)|  Gibt die aktuelle Zeile zurück. Wenn `name` angegeben, gibt die Zeile des gegebenen Positionsrahmens zurück.
sd:current-column(\<name\>)|  Gibt die aktuelle Spalte zurück. Wenn `name` angegeben, gibt die Spalte des gegebenen Positionsrahmens zurück.
sd:alternating(\<typ\>, \<text\>,\<text\>,.. )|  Bei jedem Aufruf wird das nächste Argument zurück gegeben. Wert des Typs ist beliebig, muss aber eindeutig sein. Beispiel: `sd:alternating("tbl", "Weiß","Grau")` könnte für die Hintergrundfarbe von Tabellen benutzt werden.
sd:reset-alternating(\<typ\>)|  Setzt den Zustand für `sd:alternating()` für den angegebenen Typ zurück.
sd:keep-alternating(\<typ\>)| Nutzt den aktuellen Wert von `sd:alternating(<typ>)`, ohne diesen zu verändern.
sd:count-saved-pages(\<Name\>)|  Gibt die Anzahl der gespeicherten Seiten, die mit \<SavePages\> zwischen speichert wurden.
sd:number-of-datasets(\<Sequenz\>)|  Gibt die Anzahl der Datensätze der Sequenz zurück.
sd:number-of-pages(\<Dateiname oder URI-Schema\>)|  Ermittelt die Anzahl der Seiten der angegebenen (PDF-)Datei.
sd:number-of-columns()|  Gibt die Anzahl der Spalten im aktuellen Raster.
sd:number-of-rows()|  Gibt die Anzahl der Zeilen im aktuellen Raster.
sd:attr(\<Name\>, ...)|  ist dasselbe wie `@Name`, nur mit der Möglichkeit den Namen auch dynamisch (z.B. mit `concat()`) zu erzeugen. Siehe Beispiel bei `sd:variable()`.
sd:allocated(x,y,\<Bereichsname\>,\<Rahmennummer\>) | Gibt wahr zurück, wenn die Zelle belegt ist (seit 2.3.71).
sd:imagewidth(\<Dateiname oder URI-Schema\>)|  Breite des Bildes in Rasterzellen. Vorsicht: sollte das Bild nicht gefunden werden, wird die Breite des Platzhalters für nicht gefundene Bilder zurückgegeben. Daher muss vorher überprüft werden, ob das Bild existiert.
sd:imageheight(\<Dateiname oder URI-Schema\>)|  Höhe des Bildes in Rasterzellen. Vorsicht: sollte das Bild nicht gefunden werden, wird die Höhe des Platzhalters für nicht gefundene Bilder zurückgegeben. Daher muss vorher überprüft werden, ob das Bild existiert.
sd:file-exists(\<Dateiname oder URI-Schema\>)|  Wahr, wenn der Dateiname im Suchpfad existiert, ansonsten false.
sd:format-number(Zahl oder String, Tausenderzeichen, Kommazeichen)|  Formatiert die übergebene Zahl und fügt Tausender-Trennzeichen hinzu und ändert den Kommatrenner. Beispiel: `sd:format-number(12345.67, '.',',')` ergibt die Zeichenkette `1.2345,67`.
sd:format-string(Objekt,Objekt,...,Formatierungsangaben)|  Gibt eine Zeichenkette zurück, die die gegebenen Objekte mit den im zweiten Argument gegebenen Formatierungsanweisungen darstellt. Die Formatierungsanweisungen entsprechen der aus der Programmiersprache C bekannten `printf()`-Funktion.
sd:even(\<zahl\>)|  Wahr, wenn die angegebene Zahl gerade ist. Beispiel: `sd:even(sd:current-page())`
sd:decode-html(\<Node\>)|  Wandelt Texte wie `&lt;i&gt;Kursiv&lt;/i&gt;` in entsprechendes HTML-Markup.
sd:odd(\<zahl\>)|  Wahr, wenn die angegebene Zahl ungerade ist.
sd:group-width(\<string\>)|  Gibt die Breite in Rasterzellen für die Gruppe im ersten Argument an. Beispiel: `sd:group-width('Beispielgruppe')`
sd:group-height(\<string\>)|  Gibt die Höhe in Rasterzellen für die Gruppe im ersten Argument an. Beispiel: `sd:group-height('Beispielgruppe')`
sd:pagenumber(\<Marke\>)|  Liefert die Seitenzahl der Seite auf der die angegebene Marke ausgegeben wurde. Siehe den Befehl [Mark](../commands-de/mark.html)
sd:aspectratio(\<Bildname>) | Gibt das Ergebnis der Division Bildbreite / Bildhöhe zurück. (D.h. < 1 für Hochkantbilder, > 1 für Querformat.)
sd:merge-pagenumbers(\<Seitenzahlen\>,\<Trenner für Bereiche\>,\<Trenner für Leerraum\>) | Fasst Seitenzahlenbereiche zusammen. Beispielsweise aus `"1, 3, 4, 5"` wird `1, 3–5`. Voreinstellung für den Trenner für Bereiche ist ein Halbgeviertstrich (–), Voreinstellung für den Trenner für Leerraum ist ', ' (Komma, Leerzeichen). Diese Funktion sortiert die Zahlen und löscht doppelte Einträge. Bei leerem Trenner für Bereiche werden Zahlen nicht zusammengeführt, sondern einzeln mit dem Trenner für Leerraum verbunden.
sd:sha1(\<Wert\>,\<Wert\>, …)|  Erzeugt die SHA-1 Summe der Hintereinanderkettung der Werte als Hex-Zeichenkette. Beispiel: `sd:sha1('Hallo ', 'Welt')` ergibt die Zeichenkette `28cbbc72d6a52617a7abbfff6756d04bbad0106a`.
sd:variable(\<Name\>, ...)|  ist dasselbe wie $Name, nur mit der Möglichkeit den Namen auch dynamisch zu erzeugen. Falls `$i` den Wert 3 enthält, liest `sd:variable('foo',$i)` den Inhalt der Variablen `$foo3`. Damit lassen sich Arrays abbilden.
sd:variable-exists(\<Name\>)|  Prüft, ob eine Variable vorhanden ist.
sd:dummytext() | Gibt den Blindtext "Lorem ipsum..." mit über 50 Wörtern zurück.
sd:loremipsum() | Alias für `sd:dummytext()`
sd:randomitem(\<Wert\>,\<Wert\>, …) | Gibt einen der Werte zurück.

Funktion | Beschreibung
---------|-------------
abs()      |
ceiling()  |
concat( \<Wert\>,\<Wert\>, … )|  Erzeugt einen neuen Text aus der Verkettung der einzelnen Werte.
contains(\<heuhaufen\>,\<nadel\>)  | Wahr, wenn `heuhaufen` `nadel` enthält.
count()|  Zählt alle Kindelemente mit dem angegebenen Namen. Beispiel: `count(eintrag)` zählt, wie viele Kindelemente mit den Namen `eintrag` existieren.
ceiling()|  Gibt den aufgerundeten Wert einer Zahl zurück.
empty(\<Attribut\>)|  Prüft, ob ein Attribut (nicht) vorhanden ist.
false()|  Gibt „Falsch“ zurück.
floor()|  Gibt den abgerundeten Wert einer Zahl zurück.
last()|  Gibt die Anzahl der Datensätze der gleichnamigen Geschwister-Elemente zurück. **Achtung: noch nicht XPath-konform.**
normalize-space(\<text\>) | Gibt den Text ohne führende und nachstehende Leerzeichen zurück. Alle Zeilenvorschübe werden durch Leerzeichen ersetzt. Mehrfach hintereinander auftretende Leerzeichen/Zeilenvorschübe werden durch ein einzelnes Leerzeichen ersetzt.
max()  |
min()  |
node()  |
not()|  Negiert den Wahrheitswert des Arguments. Beispiel: `not(true())` ergibt `false()`.
position()|  Ermittelt die Position des aktuellen Datensatzes.
replace(\<Eingabe\>,\<Regexp\>, \<Ersetzung\>) | Ersetzt die Eingabe mit dem regulären Ausdruck durch den Ersetzungstext. Beispiel: `replace("banana", "a", "o")` ergibt `bonono`.
string(\<Sequenz\>)|  Gibt den Textwert der Sequenz zurück, d.h. den Inhalt der Elemente.
string-join(\<Sequenz\>, Separator)|  Gibt den Textwert der Sequenz zurück, wobei alle Elemente durch den Separator getrennt werden.
string-length(\<string\>) | Gibt die Länge der Zeichenkette zurück. Multi-byte UTF-8 Sequenzen werden als eine Position gezählt.
substring(\<input>,\<start>,\<length>) | Gibt einen Teil der Zeichenkette aus `input` zurück, die bei `start` anfängt und (optional) die Länge `length` hat.
true()|  Gibt „Wahr“ zurück.
tokenize(\<Eingabe\>,\<Regexp\>) |  Die Rückgabe ist eine Sequenz von Zeichenketten. Die Eingabe wird von links nach rechts gelesen. Sobald eine Stelle gefunden wird, auf die der Reguläre Ausdruck passt, wird die bisherige Eingabe zurück gegeben. Beispiel (aus M. Kays XPath / XSLT-Buch): `tokenize("Go home, Jack!", "\W+")` ergibt die Sequenz `"Go", "home", "Jack", ""`.
upper-case() |

