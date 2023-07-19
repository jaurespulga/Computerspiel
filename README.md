# Computerspiel
In diesem Projekt werden wir ein Spiel in Java entwickeln, bei dem mehrere Spieler versuchen, die Knoten eines Graphen mit der eigenen Farbe einzufärben 
GitLab Team Repositories

In diesem Projekt werden wir ein Spiel entwickeln, bei dem mehrere Spieler versuchen, die Knoten eines
Graphen mit der eigenen Farbe einzufärben. Die genaue Beschreibung des Spiels finden Sie in Aufgabe 2.
Zuerst sollen Sie jedoch in Aufgabe 1 eine Datenstruktur für gerichtete Graphen implementieren. Wie schon
im letzten Projekt müssen Sie auch hier eigene Testfälle für die Datenstruktur erstellen.
In Aufgabe 2 implementieren und testen Sie die Spiellogik.
Aufgabe 3 besteht darin, Ihr Spiel auszuprobieren. Das Spiel bietet eine graphische Benutzeroberfläche.
Den Code dazu haben wir Ihnen vorgegeben. Das Programmieren von graphischen Benutzeroberflächen
gehört nicht zu den Lernzielen des Programmierpraktikums, diese Kompetenz können Sie zum Beispiel im
Software-Entwicklungsprojekt2
erwerben.
In der freiwilligen Aufgabe 4 können Sie einen Computerspieler implementieren.
1Wenn Sie sich bei diesem Meilenstein für die Testfälle entscheiden, dann sollten diese einen Umfang haben, mit dem die vorgegebenen Coverage Kriterien voraussichtlich erreicht werden. Sie brauchen für jede Methode aus der Graph Schnittstelle mehrere
Testfälle: Für das erfolgreiche Ausführen der Methode sowie Tests für die möglichen Ausnahmen.
2https://modhb.uni-kl.de/mhb/modules/INF-02-20-M-2/
Aufgabe 1 Gerichtete Graphen
In der Projektvorlage finden Sie die Schnittstelle de.rptu.programmierpraktikum2023.p2.a1.Graph. Die einzelnen Methoden sind mit Javadoc versehen. Lesen Sie sich den restlichen Aufgabentext sowie die Javadoc
Kommentare durch, um die Schnittstelle zu verstehen.
Die Schnittstelle Graph modelliert einen gerichteten Graphen. Der Graph besteht aus einer Menge von Knoten (engl. node). Zwischen den Knoten gibt es Kanten (engl. edge), die jeweils eine Richtung haben. Für
zwei Knoten a, b unterscheiden wir also zwischen den Kanten a → b und b → a. Der Graph kann keine,
eine oder beide dieser Kanten enthalten. Dieselbe Kante (also in derselben Richtung) kann jedoch nicht
mehrfach enthalten sein. Auch die Kante a → a ist möglich.
In den Knoten speichern wir Daten (engl. data). Jede Kante ist mit einer Gewichtung (engl. weight) versehen. Die Typparameter D und W der Schnittstelle sind die Typen für die Daten und Gewichtungen.
Der Benutzer der Schnittstelle muss die Knoten durch eindeutige Identifier vom Typ int referenzieren. Mit
der Methode int addNode(D data) wird ein neuer Knoten im Graph hinzugefügt. Der Identifier des neuen
Knotens wird von der Graph Implementierung vorgegeben und in dieser Methode zurückgegeben. Die Graph
Implementierung kann zur Festlegung der Identifier die erstellten Knoten fortlaufend nummerieren oder
eine andere Vergabemethode benutzen, solange garantiert ist, dass die vergebenen Identifier eindeutig sind.
Sollte der Benutzer der Schnittstelle eine Methode wie z. B. void setData(int nodeId, D data) mit einem
ungültigen Knoten-Identifier aufrufen, so wird die Ausnahme InvalidNodeException geworfen.
Kanten werden durch die Identifier der beiden angrenzenden Knoten identifiziert. Der Aufruf
graph.addEdge(42, 4711, weight)
fügt eine neue Kante 42 → 4711 mit der Kantengewichtung weight hinzu. Falls die übergebenen KnotenIdentifier ungültig sind, wird auch hier eine InvalidNodeException geworfen. Falls die Kante (in der angegebenen Richtung) bereits im Graph existiert, so wird eine DuplicateEdgeException geworfen.
Mit der Methode W getWeight(int fromId, int toId) kann die Kantengewichtung abgefragt werden. Die
Methode void setWeight(int fromId, int toId, W weight) aktualisiert die Gewichtung. Falls die angegebene Kante nicht existiert, werfen die beiden Methoden die Ausnahme InvalidEdgeException, unabhängig
davon, ob die angegebenen Knoten-Identifier gültig sind. Es wird keine InvalidNodeException geworfen.
Die Methode Set<Integer> getIncomingNeighbors(int nodeId) gibt die Menge aller Knoten-Identifier x
zurück, für die die Kante x → nodeId im Graph enthalten ist. Umgekehrt gibt die Methode
Set<Integer> getOutgoingNeighbors(int nodeId) die Menge aller Knoten-Identifier y zurück, für die die
Kante nodeId → y im Graph enthalten ist.
Legen Sie Ihre Lösungen der folgenden Aufgaben im Paket de.rptu.programmierpraktikum2023.p2.a1 ab.
a) Implementieren Sie eine Klasse GraphImpl implements Graph. Überlegen Sie sich, wie Sie den Graph in
Java repräsentieren möchten. Es gibt verschiedene sinnvolle Möglichkeiten. Achten Sie darauf, dass die
Implementierung effizient ist. Sie können für Ihre Implementierung gerne Werkzeuge und Datenstrukturen aus der Java Standardbibliothek benutzen (Listen, Arrays, Maps, Sets, . . . ). Sie dürfen jedoch keine
Bibliothek zur Repräsentation von Graphen verwenden, da dies das Lernziel torpedieren würde!
b) Erstellen Sie JUnit Tests für Ihre Graph Implementierung. Wenn Sie in der Testmethode Methoden aufrufen, die Ausnahmen werfen können, dann müssen Sie auch die Testmethode mit throws GraphException
versehen. Zum Testen einer erwarteten Ausnahme sei auf diese Online-Quelle3 verwiesen.
Stellen Sie dabei sicher, dass Sie (auch ohne die Tests von Aufgabe 2b) eine Instruction Coverage von
100% und eine Branch Coverage von mindestens 95% erreichen.
3https://howtodoinjava.com/junit5/expected-exception-example/
Aufgabe 2 Spiellogik
In unserem Spiel spielen zwei bis vier Spieler gegeneinander. Das Spielfeld ist ein Graph mit farbigen
Knoten. Jeder Spieler hat eine eigene Farbe. Zu Beginn des Spiels sind alle Knoten weiß – diese Farbe
gehört keinem Spieler. Die Spieler sind abwechselnd an der Reihe und können jeweils einen Knoten des
Graphen mit der eigenen Farbe einfärben. Ziel des Spiels ist es, möglichst viele Knoten mit der eigenen
Farbe zu versehen.
Das taktische Element des Spiels besteht darin, dass Knoten auch unabhängig von der Einfärbung durch
einen Spieler ihre Farbe wechseln können: Wenn für einen Knoten x mehr als die Hälfte der eingehenden
Kanten von Knoten einer einheitlichen Farbe c stammen, dann wechselt die Farbe von Knoten x auf c.
Das Spielfeld ist ein Graph<Color, Integer>. Die Kanten sind also mit Integer Werten gewichtet, mehr dazu
später. Der für die Knotenmarkierung verwendete Typ Color ist folgendes Enum:
public enum Color {
WHITE , RED , GREEN , BLUE , YELLOW
}
Zu Beginn des Spiels haben alle Knoten die neutrale Farbe WHITE. Die übrigen Farben gehören den Spielern.
Die Spieler sind abwechselnd an der Reihe und müssen einen der folgenden Spielzüge ausführen:
• Die Farbe eines beliebigen Knotens auf die eigene Spieler-Farbe ändern.
• Die Gewichtung einer beliebigen Kante um eins erhöhen.
• Die Gewichtung einer beliebigen Kante um eins verringern, wobei die Kantengewichtung dadurch
nicht negativ werden darf. Alle Kanten haben zu jeder Zeit eine Gewichtung ≥ 0.
Nach jedem Spielzug wird überprüft, ob weitere Knoten ihre Farbe ändern müssen. Für einen Knoten x sei
• wtotal(x) die Summe der Kantengewichtungen aller eingehenden Kanten ? → x und
• für eine Spieler-Farbe c , WHITE sei wc(x) die Summe der Kantengewichtungen aller eingehenden
Kanten y → x von Knoten y, deren aktuelle Farbe c ist.
Falls wc(x) >
wtotal(x)
2
ist, so erhält Knoten x automatisch die Farbe c. Beachten Sie, dass die automatische
Umfärbung eines Knotens weitere Umfärbungen auslösen kann. Ein Knoten kann niemals zur neutralen
Farbe weiß umgefärbt werden. Ein paar konkrete Beispiele finden Sie im Anhang.
Kein gültiger Spielzug ist es, die Farbe eines Knotens zu ändern, wenn dieser gemäß der eben genannten
Regel seine aktuelle Farbe aufgezwungen bekommt. Es ist jedoch erlaubt, einen Knoten, der bereits die
eigene Farbe hat, erneut einzufärben. Damit verschenkt man zwar seinen Spielzug, aber es ist regelkonform.
Beachten Sie bitte die Beispiele im Anhang, insbesondere die Beispiele 4 – 6.
Die Struktur des Graphen wird zu Spielbeginn fest vorgegeben. Während des Spiels können keine Knoten oder Kanten hinzugefügt oder entfernt werden. Im Spielverlauf verändert werden können lediglich die
Farben der Knoten sowie die Gewichtungen der Kanten. Zu Spielbeginn sind alle Knoten weiß und die
Kantengewichtungen sind ≥ 0.
In der Projektvorlage finden Sie die Schnittstelle de.rptu.programmierpraktikum2023.p2.a2.GameMove, die für
jeden der möglichen Spielzüge eine Methode beinhaltet:
• void setColor(int nodeId, Color color) ändert die Farbe des Knotens nodeId auf color. Falls der
Spielzug nicht legal sein sollte, weil der Knoten direkt wieder seine bisherige Farbe annehmen würde,
dann wird eine ForcedColorException geworfen.
• void increaseWeight(int fromId, int toId) erhöht die Gewichtung der Kante fromId → toId um eins.
• void decreaseWeight(int fromId, int toId) verringert die Gewichtung der Kante fromId → toId um
eins. Sofern die Gewichtung dadurch negativ werden würde, wird eine NegativeWeightException geworfen und die Gewichtung bleibt unverändert.
Legen Sie Ihre Lösungen der folgenden Aufgaben im Paket de.rptu.programmierpraktikum2023.p2.a2 ab.
a) Implementieren Sie eine Klasse GameMoveImpl implements GameMove. Der Konstruktor der Klasse erhält
als Parameter einen Graphen vom Typ Graph<Color, Integer>. Dieser Graph stellt den Initialzustand dar,
d. h. die für das Spiel zu verwendende Struktur wurde angelegt und alle Knoten sind weiß. Sie sollen im
Konstruktor keine Änderungen an diesem Graphen vornehmen.
Denken Sie daran, in den drei Methoden die oben erklärte automatische Umfärbung zu implementieren.
b) Erstellen Sie JUnit Tests für Ihre GameMove Implementierung. Sie können sich an den Beispielen im
Anhang orientieren. Es bietet sich an, pro Beispiel einen Testfall (d. h. eine mit @Test markierte Methode)
zu erstellen und darin die Spielzüge auszuführen. Nach jedem Spielzug sollte mit assertEquals und der
getData Methode aus der Graph Schnittstelle die Farbe aller Knoten überprüft werden.
Sie müssen dem Konstruktor der Klasse GameMoveImpl ja einen Graph<Color, Integer> übergeben. Verwenden Sie hierzu die Klasse GraphImpl aus Aufgabe 1. Erstellen Sie einige sinnvolle Graphen, die
die Anforderungen für den Anfangszustand des Spiels erfüllen, und führen Sie dann mit den GameMove
Methoden darauf unterschiedliche Spielzüge aus. Am Ende müssen Sie überprüfen, dass die von den
Spielregeln vorgegebenen Änderungen am Graph vorgenommen wurden.
Erstellen Sie mindestens einen Test so wie in dieser Teilaufgabe gefordert (also mit GraphImpl).
c) Mit den Testfällen der vorherigen Teilaufgabe testen Sie nicht nur Ihre GameMove Implementierung, sondern auch die Graph Implementierung. Dadurch handelt es sich dabei streng genommen nicht mehr um
Komponententests sondern um Integrations- oder Systemtests. Das Problem dabei ist, dass es deutlich
schwieriger ist, einen fehlgeschlagenen Testfall zu analysieren: Wir wissen nicht, ob das Problem in der
Implementierung für Graph oder in der für GameMove liegt, oder ob der Test selbst fehlerhaft ist.
Daher sollen Sie nun weitere Tests erstellen, die unabhängig von der Implementierung aus Aufgabe 1
sind. Erstellen Sie dazu innerhalb des src/test Ordners eine oder mehrere weitere Implementierungen
der Klasse Graph und verwenden Sie diese in Ihren Testfällen. Der gewaltige Unterschied ist nun, dass
Sie die Struktur des Graphen (d. h. die Knoten und Kanten) in der Klasse fest vorgeben können. Für
einen Graphen mit drei Knoten enthält diese spezielle Graph Implementierung also drei Objektattribute
vom Typ Color. Die Kantengewichtungen können als einzelne Objektattribute vom Typ int gespeichert
werden. Sie benötigen also kein Array, keine Liste, keine Map oder ähnliche Datenstrukturen. Da die
Methoden addNode und addEdge durch GameMoveImpl nicht aufgerufen, können Sie diese in der Implementierung auslassen: throw new UnsupportedOperationException();
Erstellen Sie mindestens einen Test so wie in dieser Teilaufgabe gefordert (also ohne GraphImpl).
d) Stellen Sie sicher, dass Sie mit den Testfällen der beiden vorherigen Teilaufgaben für den in Teilaufgabe a) geschriebenen Code eine Instruction Coverage von mindestens 95% und eine Branch Coverage
von mindestens 85% erreichen. Außerdem müssen alle im Anhang gezeigten Beispiele getestet werden.
Wie Sie diese Anforderung auf die beiden Vorgehensweisen (mit oder ohne Verwendung von GraphImpl)
aufteilen, bleibt Ihnen überlassen. Wir verlangen lediglich, dass Sie für beide Vorgehensweisen je mindestens einen Testfall erstellen.
Aufgabe 3 Spiel starten
Wie eingangs schon erwähnt haben wir Ihnen den kompletten Code für die graphische Benutzeroberfläche vorgegeben. Des Weiteren haben wir Methoden bereitgestellt, die basierend auf der DelaunayTriangulierung4 Knoten und Kanten so generieren, dass sich der Graph schön darstellen lässt. Außerdem
liefern wir eine recht simple Implementierung für einen Computerspieler mit, sodass Sie gegen den Computer spielen können. Diese Implementierung dürfen Sie gerne in der freiwilligen Aufgabe 4 optimieren.
Für diese Aufgabe müssen Sie die Klasse de.rptu.programmierpraktikum2023.p2.main.Factory bearbeiten.
Die beiden enthaltenen statischen Methoden sollen Objekte vom Typ Graph bzw. GameMove zurückgeben. In
der Vorlage werfen die beiden Methoden eine Ausnahme. Ersetzen Sie diese Zeile einfach durch den Aufruf
des Konstruktors Ihrer GraphImpl bzw. GameMoveImpl Klasse aus den vorherigen beiden Aufgaben. Dieser
kleine Umweg ist notwendig, weil wir in unserer Vorlage nicht den Konstruktor einer noch nicht existierenden Klasse aufrufen können. Stattdessen nutzen wir die statischen Methoden aus der Factory Klasse. So ist
sichergestellt, dass die unbearbeitete Vorlage keine Compilerfehler enthält.
Nachdem Sie die Factory Klasse vervollständigt haben, können Sie das Spiel über den Gradle Task run
starten. Es öffnet sich erst ein Willkommensbildschirm auf dem Sie einige Einstellungen wie die Anzahl
und Art der Spieler sowie die Größe des Graphen vornehmen können. Nach einem Klick auf “Start Game”
startet das Spiel. Das kann je nach Hardware und Betriebssystem einige Sekunden dauern. Falls nicht direkt
ein Graph angezeigt wird haben Sie also bitte etwas Geduld.
Oben steht, welcher Spieler gerade am Zug ist. Klicken Sie auf einen Knoten, um dessen Farbe zu ändern.
Klicken Sie auf eine Kante, um deren Gewichtung zu verändern – hierzu öffnet sich dann ein Dialogfenster. Wenn Sie eine ForcedColorException oder eine NegativeWeightException auslösen, dann erscheint eine
entsprechende Fehlermeldung und Sie müssen einen anderen Spielzug ausführen.
Falls Sie von Ihrem Spiel so begeistert sind, dass Sie es auch unabhängig von IntelliJ oder Gradle starten
wollen, dann führen Sie den Gradle Task jar (in der Rubrik build) aus. Dadurch wird im Ordner build/libs
eine Datei GraphColoringGame.jar erstellt. Diese Datei können Sie auf jedem System mit installierter Java
Runtime Environment (JRE) ausführen – den Projektordner, Gradle oder IntelliJ brauchen Sie dazu dann
nicht mehr.
Vorbereitung auf Projektabnahme
Bereiten Sie sich auf die Projektabnahme vor. Erklären Sie dabei unter anderem:
• Ihre Datenstruktur für Graphen (wie werden Knoten/Kanten gespeichert?)
• Ihren Algorithmus für automatische Umfärbungen (welche Knoten werden dabei abgelaufen/überprüft? Gibt es Unterschiede bzgl. Umfärbungen bei den drei verschiedenen möglichen Spielzügen?)
4https://de.wikipedia.org/wiki/Delaunay-Triangulierung
Aufgabe 4 Computerspieler (freiwillig)
Im Paket de.rptu.programmierpraktikum2023.p2.a4 finden Sie eine Schnittstelle ComputerPlayer sowie eine
Implementierungsklasse RandomComputerPlayer. In der Methode Factory.constructComputerPlayer wird festgelegt, welche Implementierung verwendet werden soll. Wenn Sie also eine neue Implementierung erstellen,
dann sollten Sie deren Konstruktor in der Factory Methode eintragen.
Die Schnittstelle sieht vor, dass zu Beginn des Spiels einmalig die initialize Methode aufgerufen wird.
Als Parameter übergeben werden die Referenzen vom Typ Graph und GameMove, sowie die Farbe des eigenen
Spielers und die Anzahl der am Spiel teilnehmenden Spieler. Die Farben der teilnehmenden Spieler sind
Color.values()[1] (das ist Color.RED) bis Color.values()[numPlayers]. Die Spieler sind immer in dieser
Reihenfolge am Zug. In der initialize Methode müssen Sie sich das was Sie von den übergebenen Daten
benötigen als Objektattribute abspeichern. Außerdem können Sie vorbereitende Berechnungen durchführen,
etwa Informationen zur Struktur des Graphen in geeigneter Form abspeichern.
Die makeMove Methode wird aufgerufen, wenn Ihr Spieler am Zug ist. Die Implementierung muss dann eine
Methode des in initialize übergebenen GameMove Objektes aufrufen.
Es liegt in der Verantwortung der ComputerPlayer Implementierung, dass dieser sich an die Regeln hält. Er
sollte zum Beispiel keine Änderungen direkt am Graph Objekt durchführen oder mehrere Spielzüge ausführen. Die Spiellogik selbst nimmt keine zusätzliche Überprüfung vor und vertraut dem Computerspieler.
Wir haben eine Verzögerung von einer Sekunde vor jedem Computerspieler-Spielzug eingebaut. Dies ist
Absicht und dient dazu, dass man in der graphischen Oberfläche auch schnell hintereinander ausgeführte
Spielzüge nachvollziehen kann, zum Beispiel wenn man mehrere Computerspieler gegeneinander antreten
lässt. Sollten Sie diese Wartezeit ändern wollen, so werden Sie in der Methode nextPlayer in der Klasse Game
aus dem Paket de.rptu.programmierpraktikum2023.p2.main fündig. Den Code der Klasse Game müssen Sie
nicht ganz nachvollziehen können. Hauptproblem ist, dass wir es hier mit nebenläufiger Programmierung
zu tun haben, weil die graphische Benutzeroberfläche ja nicht einfrieren soll, während der Computerspieler
rechnet oder die Pause vor seinem Zug abwartet. Mehr dazu werden Sie in der Veranstaltung Verteilte und
nebenläufige Programmierung5
lernen.
