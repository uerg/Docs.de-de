---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Implementieren von Zuordnungsszenarien mithilfe von AJAX | Microsoft-Dokumentation
author: microsoft
description: Schritt 11 zeigt, wie das Integrieren von AJAX-Mapping-Unterstützung in unserer NerdDinner-Anwendung, und Aktivieren von Benutzern erstellen, bearbeiten oder Anzeigen von Dinner, um den l finden Sie unter...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: f8b9704e966c0211a690156555f4a272a823023a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825978"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Implementieren von Zuordnungsszenarien mithilfe von AJAX
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 11 ein kostenloses ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 11 veranschaulicht, wie die NerdDinner-Anwendung, aktivieren Benutzer erstellen, bearbeiten oder Anzeigen von Dinner, um den Speicherort der Dinner grafisch finden Sie unter Zuordnung AJAX-Unterstützung integriert.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner Schritt 11: Integrieren einer AJAX-Karte

Wir erstellen jetzt, unsere Anwendung visuell etwas aufregende, durch die Integration von AJAX unterstützt. Dadurch können Benutzer, die erstellen, bearbeiten oder Anzeigen von Dinner, um den Speicherort der Dinner grafisch finden Sie unter.

### <a name="creating-a-map-partial-view"></a>Erstellen eine partielle Kartenansicht

Wir sind Zuordnungsfunktionalität an mehreren Stellen in unserer Anwendung verwenden. Um unser Code DRY zu halten müssen wir die allgemeine Map-Funktionalität in eine einzelne partielle Vorlage kapseln, die wir über mehrere Controlleraktionen und Ansichten erneut verwenden können. Wir nennen Sie diese partielle Ansicht "map.ascx" und erstellen Sie ihn in das Verzeichnis \Views\Dinners.

Wir können die map.ascx partielle erstellen, indem Sie mit der rechten Maustaste auf das Verzeichnis \Views\Dinners und Auswählen der hinzufügen -&gt;Menübefehl anzeigen. Wir nennen Sie die Ansicht "Map.ascx", checken Sie es als Teilansicht und anzugeben, dass wir es sich um eine stark typisierte "Dinner" Model-Klasse übergeben werden soll:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Beim Klicken auf die Schaltfläche "Hinzufügen" wird unsere Vorlage für eine teilweise erstellt werden. Wir aktualisieren Sie dann die Map.ascx-Datei, um den folgenden Inhalt aufweisen:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

Die erste &lt;Skript&gt; verweisen Sie auf der Microsoft Virtual Earth 6.2-Mapping-Bibliothek verweist. Die zweite &lt;Skript&gt; Verweis zeigt auf eine map.js-Datei, die wir in Kürze erstellen, die unsere allgemeine Javascript Zuordnungslogik gekapselt wird. Die &lt;Div Id = "TheMap"&gt; Element ist der HTML--Container, die Virtual Earth verwendet werden, um die Zuordnung zu hosten.

Wir haben dann ein eingebettetes &lt;Skript&gt; Block, der zwei JavaScript-Funktionen, die speziell für diese Sicht enthält. Die erste Funktion verwendet jQuery, um eine Funktion anschließen, die ausgeführt wird, wenn die Seite des clientseitigen Skripts ausgeführt werden kann. Er ruft eine LoadMap()-Hilfsfunktion, die wir in unserer Map.js Skriptdatei Laden Sie das Kartensteuerelement virtual Earth definieren. Die zweite Funktion ist es sich um einen Rückrufhandler für das Ereignis, der die Zuordnung eine Pin hinzufügt, die einen Speicherort angibt.

Beachten Sie, wie wir ein serverseitiges verwenden &lt;% = %&gt; Block innerhalb des Skriptblocks mit der clientseitigen einbetten, die Breiten- und Längengrad der Dinner in den JavaScript-Code zugeordnet werden soll. Dies ist eine nützliche Methode zum dynamischen Werte aus, die vom Client-seitige Skript (ohne einen separaten AJAX-Aufruf an den Server zum Abrufen der Werte – Dadurch wird es schneller) verwendet werden kann. Die &lt;% = %&gt; Blöcke werden immer dann ausgeführt, wenn die Ansicht auf dem Server – rendering ist und daher die Ausgabe des HTML-Code einfach enden mit eingebetteten JavaScript-Werte (z. B.: Var Latitude 47.64312; =).

### <a name="creating-a-mapjs-utility-library"></a>Erstellen eine Map.js Hilfsprogrammbibliothek

Nun erstellen wir die Map.js-Datei, die wir verwenden können, die JavaScript-Funktionen für die Karte kapseln (und implementieren Sie die oben genannten Methoden LoadMap und LoadPin). Wir können zu diesem Zweck mit der rechten Maustaste auf das Verzeichnis "\Scripts" in das Projekt, und wählen Sie dann die "Add -&gt;neues Element" Menübefehls, so wählen Sie das JScript-Element, und nennen Sie es mit "Map.js".

Im folgenden finden Sie im JavaScript-Code, wir fügen, der Map.js-Datei, die mit Virtual Earth Karte anzuzeigen, und fügen Standorte Pins hinzu, für unsere Dinner interagiert:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrieren der Zuordnung erstellen und Bearbeiten von Formularen

Wir werden nun die Unterstützung von Karten in unsere vorhandenen erstellen und Bearbeiten von Szenarios integrieren. Die gute Nachricht ist, dass dies ziemlich einfache Aufgabe ist und nicht zum Ändern des Controllercodes erfordern. Da unsere Bearbeitungs- und änderungsansichten eine allgemeine "DinnerForm" partielle Ansicht, um die Dinner-Formular-Benutzeroberfläche implementieren gemeinsam nutzen, können die Zuordnung an einem Ort hinzufügen und verwenden beide unsere erstellen und Bearbeiten von Szenarien sie verwenden.

Müssen wir nur die to-do ist, öffnen Sie die Teilansicht \Views\Dinners\DinnerForm.ascx und aktualisieren, um unsere neue Zuordnung, die teilweise enthalten. Im folgenden finden Sie wie die aktualisierte DinnerForm aussehen, nachdem die Zuordnung hinzugefügt wurde (Hinweis: der HTML-Formularelemente werden aus den nachstehenden Codeausschnitt aus Gründen der Übersichtlichkeit weggelassen):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Die über partielle DinnerForm nimmt ein Objekt vom Typ "DinnerFormViewModel" als der Typ des Modells, (da es sowohl ein Dinner-Objekt, als auch eine SelectList zum Auffüllen der Dropdownliste von Ländern erforderlich). Die Karte, die teilweise lediglich ein Objekt vom Typ "Dinner" als der Typ des Modells, und also wenn wir die Zuordnung teilweise Rendern übergeben werden nur die Dinner Untereigenschaft des DinnerFormViewModel darauf:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

Die JavaScript-Funktion haben wir die partielle verwendet jQuery, fügen Sie ein Ereignis "Weichzeichnen" dem Textfeld "Adresse" HTML hinzugefügt. Sie haben wahrscheinlich ""-Fokusereignisse, die ausgelöst werden, wenn ein Benutzer klickt oder Registerkarten gehört, in ein Textfeld. Das Gegenteil ist ein "Weichzeichnen"-Ereignis, das ausgelöst wird, wenn ein Benutzer ein Textfeld beendet wird. Der oben genannten Ereignishandler löscht die Werte für Breiten- und Längengrad Textfeld aus, wenn dies passiert, und dann den neuen Adressspeicherort auf die Karte stellt. Ein Rückruf-Ereignishandler, den wir in der Datei map.js definiert aktualisiert dann die Längen- und Breitengrad Textfelder ein, auf das Formular mit Werten, die von virtual Earth basierend auf der Adresse, die wir erstmal zurückgegeben.

Und führen Sie nun Wenn wir unsere Anwendung erneut aus und klicken Sie auf der Registerkarte "Dinner" Host "" "sehen wir den Standardwert zugeordnet zusammen mit unserem standard Dinner-Form-Elemente angezeigt:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Wenn wir eine Adresse eingeben und TAB-entfernt Taste, die Zuordnung wird dynamisch aktualisiert, um den Ort anzuzeigen, und der Ereignishandler füllt die Breiten-und Längengrad Textfelder ein, durch die Speicherort-Werte:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Wenn wir dann erneut zur Bearbeitung öffnen, speichern das neue Dinner, werden wir feststellen, dass die Standorte der Karte angezeigt wird, wenn die Seite geladen wird:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Jedes Mal, wenn das Adressfeld geändert wird, werden die Zuordnung und die Koordinaten für Breitengrad und Längengrad aktualisiert.

Nun, dass die Dinner-Position die Karte angezeigt wird, können wir auch die Formularfelder breiten- und Längengrad ändern, nicht sichtbaren Textfelder soll stattdessen ausgeblendete Elemente (da die Zuordnung automatisch sie jedes Mal, wenn keine Adresse eingegeben wurde aktualisiert wird). To-do dies wir wechseln das Html.TextBox() HTML-Hilfsobjekt, mit der Hilfsmethode Html.Hidden() verwenden:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Und jetzt unsere Formulare sind ein wenig Benutzerfreundlicher und zu vermeiden, die unformatierten breiten-/Längengrad (während Sie weiterhin mit jeder Dinner in der Datenbank speichern) anzeigen:

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integrieren von der Zuordnung in der Detailansicht

Der nun die Zuordnung erstellen und Bearbeiten von Szenarios, integriert sind, lassen Sie uns, die auch sie in unserem Szenario Details integriert werden kann. Alles, was wir benötigen die Aufgabe besteht darin, aufzurufen &lt;% Html.RenderPartial("map"); %&gt; in der Detailansicht.

Im folgenden sehen, wie Quellcode und die vollständige Detailansicht (mit Map-Integration) aussieht:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Und nun, wenn ein Benutzer zu einem "/ dinners" / Details / [Id] URL navigiert. sie erhalten Informationen zu den Essen, den Speicherort der Dinner auf der Karte (mit einem Push-Pin bei Normalzustand zeigt den Titel des der Dinner und die Adresse des Zertifikats), und einen AJAX-Link zur Bestätigung fo R es:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementieren in unserer Datenbank und das Repository Standortsuche

Zum Deaktivieren der AJAX-Implementierung fertig sind, fügen Sie eine Zuordnung zur Startseite der Anwendung, die Benutzern ermöglicht, für die Dinner in der Nähe sie grafisch durchsuchen.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Wir beginnen, indem das Implementieren der Unterstützung in unserer Datenbank und die Daten Repositoryschicht standortbasierte RADIUS-Suche nach Dinner effizient ausführt. Wir verwenden die neue [Geospatial-Funktionen von SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) implementiert, oder wir können auch einen SQL-Funktion-Ansatz, die Gary Dryden im Artikel erörtert verwenden: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) und Rob Conery Blog zur Verwendung mit LINQ to SQL hier: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Um dieses Verfahren zu implementieren, wird wir "Server-Explorer" in Visual Studio öffnen, wählen Sie die NerdDinner-Datenbank, und klicken Sie dann mit der rechten Maustaste auf die "Funktionen" untergeordnete Knoten und wählen Sie zum Erstellen einer neuen "skalarwertige Funktion":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Klicken Sie dann fügen in der folgenden DistanceBetween Funktion:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Wir klicken Sie dann erstellen eine neue Funktion mit Tabellenrückgabe in SQL Server, dass wir "NearestDinners" nenne:

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Diese Funktion "Table" "NearestDinners" verwendet die DistanceBetween-Hilfsfunktion, um alle Dinner in 100 Meilen Latitude zurückzugeben und Längengrad wir bereitstellen:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Um diese Funktion aufrufen, werden wir zunächst die LINQ to SQL-Designer aktivieren, indem Sie doppelklicken auf die Datei NerdDinner.dbml in unser \Models-Verzeichnis:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Klicken Sie dann ziehen die NearestDinners und DistanceBetween-Funktionen in LINQ to SQL-Designer, wir dadurch werden als Methoden auf unserem LINQ auf SQL NerdDinnerDataContext-Klasse hinzugefügt werden:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Wir können eine Abfragemethode "FindByLocation" Sie verfügbar machen, unserer "dinnerrepository"-Klasse, die die NearestDinner-Funktion verwendet, um bevorstehende zurückzugeben Dinner, die im 100 Meilen von der angegebenen Position befinden:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementieren eine JSON-basierten AJAX Aktion Suchmethode

Implementieren wir jetzt eine Controlleraktionsmethode, die nutzt die Vorteile der neuen FindByLocation() Repository-Methode, die eine Liste der Dinner-Daten zurückgegeben, die zum Auffüllen einer Zuordnung verwendet werden kann. Wir müssen diese Aktionsmethode, die die Dinner-Daten in einem Format JSON (JavaScript Object Notation) zurückgegeben, damit sie problemlos verarbeitet werden, kann mithilfe von JavaScript auf dem Client.

Um dies zu implementieren, werden wir eine neue "SearchController"-Klasse erstellen, indem Sie mit der rechten Maustaste auf das Verzeichnis \Controllers und Auswählen der hinzufügen -&gt;Kontextmenübefehl von "Controller". Klicken Sie dann implementieren wir eine "SearchByLocation" Action-Methode in der neuen Klasse SearchController wie unten:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Die SearchControllers SearchByLocation Action-Methode ruft intern die FindByLocation-Methode auf DinnerRespository, um eine Liste der in der Nähe Dinner zu erhalten. Anstatt die Dinner-Objekte direkt an den Client zurückgeben, allerdings es stattdessen JsonDinner gibt Objekte zurück. Die JsonDinner-Klasse stellt eine Teilmenge der Dinner-Eigenschaften (z. B.: Sicherheitsgründen weitergeben nicht die Namen der Personen, die zu einem Dinner um Antwort gebeten haben). Darüber hinaus eine RSVPCount-Eigenschaft, nicht vorhanden ist, auf der Dinner – und das durch zählen der Anzahl von RSVP-Objekten, die ein bestimmtes Essen zugeordneten dynamisch berechnet wird.

Wir klicken Sie dann verwenden die Parse()"-Hilfsmethode für die Basisklasse für Controller, um die Reihenfolge der Dinner mit einem JSON-basierten Wireformat zurück. JSON ist ein standard-Text-Format für einfache Datenstrukturen darstellt. Es folgt ein Beispiel für eine JSON-formatierte Liste zwei JsonDinner Objekte wie bei, die von unserer Aktionsmethode zurückgegeben:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Aufrufen der JSON-basierten AJAX-Methode, die unter Verwendung von jQuery

Wir sind jetzt bereit zum Aktualisieren von der Startseite der NerdDinner-Anwendung, die SearchControllers SearchByLocation Action-Methode verwenden. To-do, wir die ansichtsvorlage /Views/Home/Index.aspx öffnen und aktualisieren sie verfügen über ein Textfeld, Schaltfläche "Suchen", die Karte, und ein &lt;Div&gt; Element mit dem Namen DinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Wir können dann zwei JavaScript-Funktionen auf der Seite hinzufügen:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

Die erste JavaScript-Funktion lädt die Zuordnung beim ersten der Seite laden. Die zweite JavaScript-Funktion verbindet sich ein JavaScript-click-Ereignishandler auf die Schaltfläche "Suchen". Wenn die Schaltfläche gedrückt wird, ruft es die FindDinnersGivenLocation()-JavaScript-Funktion, die wir in unserer Map.js-Datei hinzufügen:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Diese Funktion FindDinnersGivenLocation() Ruft Map. Find() auf das Virtual Earth-Steuerelement, das ihn auf den eingegebenen Pfad zu zentrieren. Wenn die virtual Earth-Kartendienst, gibt die Karte zurück. Find()-Methode ruft die CallbackUpdateMapDinners Callback-Methode, die wir sie als das letzte Argument übergeben.

Die callbackUpdateMapDinners()-Methode ist, in denen die eigentliche Arbeit erfolgt ist. Es verwendet die jQuery-$.post()-Hilfsmethode, um ein AJAX-Aufruf an unsere SearchControllers SearchByLocation() Aktionsmethode – und übergeben sie den Breiten- und Längengrad, von dem neu fokussierte Karte durchzuführen. Es definiert eine Inlinefunktion, die aufgerufen wird, wenn die Hilfsmethode $.post() abgeschlossen ist, und die JSON-formatierte Dinner-Ergebnisse zurückgegeben werden, aus der SearchByLocation() Action-Methode mit einer Variablen namens "Dinner" übergeben wird. Klicken Sie dann eine foreach-Anweisung für jede zurückgegebene Dinner ist, und das Dinner des breiten- und Längengrad und andere Eigenschaften verwendet, um eine neue Pin auf der Karte hinzuzufügen. Außerdem werden die HTML-Liste der Dinner rechts neben der Zuordnung einen Dinner-Eintrag hinzugefügt. Sie einfach oben klicken Sie dann ein Mauszeiger-Bewegungsereignis für Pins und die HTML-Liste, damit Informationen über das Dinner angezeigt werden, wenn ein Benutzer über diese bewegt:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Und jetzt Wenn wir die Anwendung auszuführen und finden Sie auf der Startseite, die wir mit einer Karte angezeigt. Wenn wir den Namen einer Stadt eingeben wird die Zuordnung in der Nähe der bevorstehenden Dinner angezeigt:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Mit dem Mauszeiger auf einem Dinner werden Details zu ihr anzuzeigen.

Klicken auf den Titel der Dinner wird entweder im Blasendiagramm oder auf der rechten Seite in der Liste der HTML-uns auf der Dinner – navigieren Sie in der wir dann optional für Antworten kann:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt die Anwendungsfunktionen unserer NerdDinner-Anwendung implementiert. Lassen Sie uns nun ansehen, wie wir können automatisierte Komponententests des Tests.

> [!div class="step-by-step"]
> [Zurück](use-ajax-to-deliver-dynamic-updates.md)
> [Weiter](enable-automated-unit-testing.md)
