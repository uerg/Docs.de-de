---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: Verwenden von AJAX zum Zuordnen von Szenarien implementieren | Microsoft Docs
author: microsoft
description: Schritt 11 veranschaulicht das Integrieren von Unterstützung für AJAX-Zuordnung in unserer NerdDinner Anwendung Benutzer erstellen, bearbeiten oder Anzeigen von Abendessen Beiträgen l aktivieren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 4b3f1e46886c4c1f054e43768b0a44695d71bf09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>Verwenden von AJAX implementieren Szenarien zuordnen
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 11 mit einem kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 11 veranschaulicht, wie Unterstützung für AJAX-Zuordnung in unsere NerdDinner-Anwendung, aktivieren Benutzer erstellen, bearbeiten oder Anzeigen von Abendessen um Diagramme einsehen, die den Speicherort von der Dinner integrieren.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner Schritt 11: Integrieren einer AJAX-Karte

Wir nun treffen unsere Anwendung ein wenig aufregenden visuell durch die Integration von Unterstützung für AJAX-Zuordnung. Dadurch können Benutzer, die erstellen, bearbeiten oder Anzeigen von Abendessen, um den Speicherort von der Dinner grafisch finden Sie unter.

### <a name="creating-a-map-partial-view"></a>Erstellen eine Zuordnung Teilansicht

Kegel zum Zuordnungsvorgang an mehreren Stellen innerhalb der Anwendung zu verwenden. Zum Beibehalten des getesteten Codes trocken müssen wir die allgemeine Funktionalität der Zuordnung in eine einzelne partielle Vorlage kapseln, die wir über mehrere Controlleraktionen und Sichten erneut verwenden können. Wir nennen diese partielle Ansicht "map.ascx" und erstellen Sie ihn in das Verzeichnis \Views\Dinners.

Wir können die map.ascx partielle erstellen, indem Sie mit der rechten Maustaste auf das Verzeichnis \Views\Dinners und Add -&gt;Menübefehl anzeigen. Wir nennen Sie die Ansicht "Map.ascx", checken Sie es als eine Teilansicht und anzugeben, dass wir werden eine Modellklasse stark typisierte "Dinner" übergeben:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Beim Klicken auf die Schaltfläche "Hinzufügen" wird unsere partielle Vorlage erstellt werden. Wir werden dann die Map.ascx-Datei, um den folgenden Inhalt haben aktualisieren:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

Die erste &lt;Skript&gt; verweist auf die Bibliothek der Microsoft Virtual Earth 6.2 Zuordnung verweisen. Die zweite &lt;Skript&gt; verweisen verweist auf eine map.js-Datei, die wir in Kürze erstellen, die unsere allgemeine Javascript Zuordnungslogik gekapselt wird. Die &lt;Div Id = "TheMap"&gt; -Element ist der HTML-Container, die Virtual Earth nutzt, um die Zuordnung zu hosten.

Wir haben dann ein eingebetteter &lt;Skript&gt; Datenblock, der zwei JavaScript-Funktionen, die speziell für diese Sicht enthält. Die erste Funktion verwendet jQuery, um über das Netzwerk eine Funktion von Volltextkatalogen, die ausgeführt wird, wenn die Seite für die Ausführung von clientseitigem Skript bereit ist. Er ruft eine LoadMap()-Hilfsfunktion, die wir in unserer Map.js-Skriptdatei, laden Sie das virtual Earth-Kartensteuerelement definieren. Die zweite Funktion ist ein Rückruf-Ereignishandler eine Pin zur Zuordnung hinzu, die einen Ort identifiziert.

Beachten Sie, wie ein serverseitiger verwenden &lt;% = %&gt; Block innerhalb des Blocks clientseitigem Skript eingebettet, die Breiten- und Längengrad der Dinner in der JavaScript-Code zugeordnet werden soll. Dies ist ein nützliches Verfahren, um dynamische Werte ausgegeben werden, die von clientseitigem Skript (ohne einen separaten AJAX-Aufruf an den Server zum Abrufen der Werte –, wodurch schneller) verwendet werden kann. Die &lt;% = %&gt; Blöcke werden ausgeführt, wenn die Ansicht, auf dem Server gerendert wird – und daher die Ausgabe der HTML-Datei nur enden mit eingebetteten JavaScript-Werte (z. B.: Breitengrad Var = 47.64312;).

### <a name="creating-a-mapjs-utility-library"></a>Erstellen einer Bibliothek für Map.js-Hilfsprogramm

Jetzt erstellen wir die Map.js-Datei, die wir zum Kapseln der JavaScript-Funktionen für unsere Map (und implementieren Sie die oben genannten Methoden LoadMap und LoadPin) verwenden können. Sie können zu diesem Zweck mit der rechten Maustaste auf das Verzeichnis \Scripts in unserem Projekt und wählen Sie dann die "Add -&gt;neues Element" Menübefehl, wählen Sie die JScript-Element, und nennen Sie sie "Map.js".

Im folgenden finden Sie der JavaScript-Code wir fügen der Map.js-Datei, die in Virtual Earth eine Karte anzuzeigen und Speicherorte Pins hinzuzufügen, für unsere Abendessen interagieren:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Integrieren der Zuordnung erstellen und Bearbeiten von Formularen

Jetzt müssen wir unsere vorhandenen erstellen und Bearbeiten von Szenarien die Unterstützung von Karten integrieren. Die gute Nachricht ist, dass dies sehr einfache Aufgabe ist, und erfordert nicht, wir unsere controllercode nicht ändern. Da unsere erstellen und Bearbeiten von Ansichten eine allgemeine "DinnerForm" Teilansicht um Dinner-Formular-Benutzeroberfläche implementieren können, kann die Zuordnung an einem Ort hinzufügen und haben beide unsere erstellen und Bearbeiten von Szenarien verwendet werden.

Müssen wir nur die Aufgabe besteht darin die Teilansicht \Views\Dinners\DinnerForm.ascx öffnen und aktualisieren, um unsere neue Zuordnung, die teilweise enthalten. Im folgenden finden Sie die aktualisierte DinnerForm aussehen, nachdem die Zuordnung hinzugefügt wurde (Hinweis: die HTML-Formularelemente werden aus der Codeausschnitt unten aus Gründen der Übersichtlichkeit weggelassen):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

Die partielle oben DinnerForm akzeptiert ein Objekt vom Typ "DinnerFormViewModel" als seine Modelltyp (da sowohl ein Dinner-Objekt als auch eine SelectList zum Auffüllen der Dropdownlist Länder oder Regionen benötigt). Unsere partielle Zuordnung benötigt nur ein Objekt vom Typ "Dinner" als Modelltyp, und daher Wenn wir die Zuordnung teilweise Rendern wir werden nur die untergeordneten Dinner-Eigenschaft, der DinnerFormViewModel an sie übergibt:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

Die JavaScript-Funktion haben wir die partielle verwendet jQuery an ein Ereignis "Blur" auf das Textfeld "Address" HTML hinzugefügt. Sie haben wahrscheinlich "den"-Fokusereignisse, die ausgelöst werden, wenn ein Benutzer klickt oder Registerkarten gehört, in ein Textfeld. Das Gegenteil ist ein "Blur"-Ereignis, das ausgelöst wird, wenn ein Benutzer ein Textfeld beendet wird. Der oben genannten Ereignishandler löscht die Breiten- und Längengrad Textbox-Werte auf, wenn dies passiert, und klicken Sie dann den neuen Adressspeicherort auf der Karte gezeichnet. Ein Rückruf-Ereignishandler, den wir in der Datei map.js definiert aktualisieren Sie dann die Längen- und Breitengrad Textfelder ein, in unserem Formular mithilfe der Rückgabewerte von virtual Earth basierend auf der Adresse, die wir es zugewiesen haben.

Und führen Sie nun Wenn wir unsere Anwendung dann erneut aus und klicken Sie auf der Registerkarte "Host Dinner" "" eingehendem einen Standardwert zugeordnet zusammen mit unserer Dinner Formular Standardelemente angezeigt:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Wenn wir geben Sie eine Adresse, und klicken Sie dann die Registerkarte entfernt, die Zuordnung wird dynamisch aktualisiert, um den Ort anzuzeigen und unsere Ereignishandler füllt die Längen-und Breitengraden Textfelder ein, mit der die Werte:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Wenn wir neue Dinner speichern und es dann erneut zur Bearbeitung öffnen, fest wir, dass die Standorte der Karte angezeigt wird, wenn die Seite geladen:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Jedes Mal, wenn das Adressfeld geändert wird, werden die Karte und die Koordinaten des Längen-und Breitengraden aktualisiert.

Nun, dass die Zuordnung den Dinner Speicherort angezeigt wird, können wir auch Formularfelder breiten- und Längengrad ändern, nicht sichtbar Textfelder auf ausgeblendete Elemente empfiehlt sich u. (da die Zuordnung automatisch sie jedes Mal eine Adresse eingegebene aktualisiert wird). To-do dies wir wechseln das Html.TextBox() HTML-Hilfsobjekt, verwenden die Hilfsmethode Html.Hidden() verwenden:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Und jetzt unsere Formulare etwas Benutzerfreundlicher sind, und vermeiden Sie die unformatierten Längen-und Breitengraden (während Sie weiterhin mit jeder Dinner in der Datenbank speichern) anzeigen:

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Integration von der Zuordnung in der Detailansicht

Nun mit dem die Zuordnung wir in unserer Szenarien erstellen und Bearbeiten von integriert, die ihn auch in unserem Szenario Details zu integrieren. Wir benötigen lediglich Aufgabe besteht im Aufrufen &lt;"" Html.RenderPartial("map");&gt; innerhalb der Ansicht.

Im folgenden finden Sie Quellcode und die vollständige Detailansicht (mit Map-Integration) aussieht:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Nun, wenn ein Benutzer zu einer /Dinners/Informationen / [Id] URL navigiert. sie sehen Details zu den Essen, den Speicherort von der Dinner auf der Karte (mit einem Push-Pin abgeschlossen, die bei über bewegt wird zeigt den Titel der Dinner und die Adresse des Zertifikats), und über ein AJAX-Verknüpfung zum Antwort fo R es:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Implementieren in unserer Datenbank und des Repositorys Speicherort suchen

Um unsere AJAX-Implementierung zu beenden, fügen Sie eine Zuordnung zur Startseite der Anwendung, die Benutzern ermöglicht, die grafisch Abendessen in der Nähe sie suchen.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Wir beginnen, indem das Implementieren der Unterstützung unserer Datenbank und die Daten Repository Ebene eine standortbasierte Radius nach Abendessen effizient ausführen. Wir verwenden die neue [Geospatial-Funktionen von SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) implementiert, oder es können auch einen SQL-Funktion-Ansatz, der Gary Dryden im Artikel hier erläutert verwenden: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) und Rob Conery einem Blog beschrieben hat, zur Verwendung mit LINQ to SQL hier: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Um dieses Verfahren zu implementieren, wir die "Server-Explorer öffnen" in Visual Studio, wählen Sie die Datenbank NerdDinner, und klicken Sie dann mit der rechten Maustaste auf den Funktionsknoten "" untergeordnete, darunter und wählen Sie zum Erstellen einer neuen "skalarwertige Funktion":

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Wir müssen dann in der folgenden DistanceBetween-Funktion einfügen:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Wir erstellen Sie dann, dass wir "NearestDinners" aufrufen, wird eine neue Funktion mit Tabellenrückgabe in SQL Server:

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Diese "NearestDinners" Tabelle-Funktion verwendet die Hilfsfunktion DistanceBetween zurückzugebenden alle Abendessen innerhalb 100 Meilen von der Breitengrad und Längengrad wir angeben:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Um diese Funktion aufrufen, müssen wir zunächst die LINQ to SQL-Designer Öffnen durch Doppelklicken auf die Datei NerdDinner.dbml in unserem \Models-Verzeichnis:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Wir müssen dann NearestDinners und DistanceBetween-Funktionen in LINQ to SQL-Designer ziehen, werden als Methoden für unsere LINQ zu SQL NerdDinnerDataContext-Klasse hinzugefügt werden dadurch wird:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Wir können eine Abfragemethode "FindByLocation" Sie verfügbar machen, unsere DinnerRepository-Klasse, die die NearestDinner-Funktion verwendet, um anstehende zurückzugeben Abendessen angezeigt, die innerhalb des angegebenen Speicherorts 100 Meilen sind:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>Implementieren eine Aktionsmethode der JSON-basierte AJAX-Suche

Implementieren wir nun eine Controlleraktionsmethode, die nutzt die Vorteile der neuen FindByLocation() Repository Methode, um wieder eine Liste mit Dinner Daten zurückzugeben, die zum Auffüllen einer Zuordnung verwendet werden kann. Wir müssen diese Aktionsmethode zurück Dinner Rückgabe der Daten in einem Format der JSON (JavaScript Object Notation), damit sie problemlos verarbeitet werden, kann mit JavaScript auf dem Client.

Um dies zu implementieren, wir erstellen eine neue "SearchController"-Klasse indem Sie mit der rechten Maustaste auf das Verzeichnis \Controllers und Add -&gt;Controller Menübefehl aus. Klicken Sie dann implementieren wir eine Aktionsmethode "SearchByLocation" innerhalb der neuen Klasse SearchController wie unten:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

Die SearchController SearchByLocation Aktionsmethode ruft die FindByLocation-Methode intern für DinnerRespository, um eine Liste von benachbarten Abendessen abzurufen. Anstatt die Dinner Objekte direkt an den Client zurückgegeben, aber es stattdessen JsonDinner gibt Objekte zurück. Die Klasse JsonDinner macht eine Teilmenge der Eigenschaften von Dinner (z. B.: Sicherheitsgründen es nicht verpflichtet, die Namen von Personen, die auf die Antwort für eine Dinner gebeten haben). Darüber hinaus eine RSVPCount-Eigenschaft, die nicht vorhanden ist, auf der Dinner – und die dynamisch durch zählen der Anzahl der Antwort-Objekte, die einen bestimmten Dinner zugeordnet berechnet wird.

Klicken Sie dann verwenden die Json()-Hilfsmethode für die Basisklasse für Controller wir die Abfolge der Abendessen mit einem JSON-basierte Wireformat zurückgegeben. JSON ist ein standard-Text-Format für die Darstellung von einfachen Datenstrukturen. Es folgt ein Beispiel, wie eine JSON-formatierte Liste zwei JsonDinner Objekte aussieht, wie wenn aus unserem Aktionsmethode zurückgegeben:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>Aufrufen der unter Verwendung von jQuery AJAX-basierten JSON-Methode

Wir können nun auf der Startseite der Anwendung NerdDinner, verwenden Sie die SearchController SearchByLocation Aktionsmethode zu aktualisieren. To-do, wir öffnen Sie die Vorlage der /Views/Home/Index.aspx anzeigen und aktualisieren, um ein Textfeld, Schaltfläche "Suchen", unsere Map haben und ein &lt;Div&gt; Element mit dem Namen DinnerList:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

Wir können die Seite zwei JavaScript-Funktionen hinzugefügt werden:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

Die erste JavaScript-Funktion lädt die Zuordnung beim ersten die Seite laden. Die zweite JavaScript-Funktion Drähten von einer JavaScript-click-Ereignishandler auf die Schaltfläche "Suchen". Beim Klicken auf die Schaltfläche aufgerufen wird ruft es die FindDinnersGivenLocation()-JavaScript-Funktion, die wir in unserer Map.js-Datei hinzufügen:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Diese FindDinnersGivenLocation()-Funktion ruft Zuordnung. Wird auf dem Virtual Earth-Steuerelement um auf den eingegebenen Pfad zu zentrieren. Wenn die Zuordnung virtual Earth-Dienst, gibt die Karte zurück. Wird-Methode ruft die CallbackUpdateMapDinners-Rückrufmethode, die wir ihn als das letzte Argument übergeben.

Die callbackUpdateMapDinners()-Methode ist, in denen die eigentliche Arbeit ausgeführt wird. Es führt mithilfe des jQuery $.post() Hilfsmethode ein AJAX-Aufruf an unsere SearchController SearchByLocation() Aktionsmethode – und übergeben sie den Breiten- und Längengrad der Zuordnung neu zentriert aus. Definiert eine Inlinefunktion, die aufgerufen wird, wenn die Hilfsmethode $.post() abgeschlossen ist, und die JSON-formatierte Dinner Ergebnisse zurückgegeben, aus der SearchByLocation() Aktionsmethode mit einer Variablen namens "Abendessen" übergeben wird. Klicken Sie dann Foreach über jedes zurückgegebene Dinner verfügt, und der Dinner Breiten- und Längengrad und andere Eigenschaften verwendet, um eine neue Pin auf der Karte hinzuzufügen. Außerdem werden die HTML-Liste von Abendessen rechts neben der Karte einen Eintrag Dinner hinzugefügt. Es Drähten-nach-oben klicken Sie dann ein Mauszeiger-Bewegungsereignis für Pins und die HTML-Liste, damit Informationen zu den Dinner angezeigt werden, wenn ein Benutzer darauf zeigt:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Und jetzt Wenn wir die Anwendung auszuführen und finden Sie auf der Startseite, die wir mit einer Karte angezeigt. Wenn wir den Namen einer Stadt einzugeben wird die Zuordnung in der Nähe der bevorstehenden Abendessen anzuzeigen:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Mit dem Mauszeiger auf eine Dinner werden Details angezeigt.

Klicken auf den Titel Dinner wird entweder im Blasendiagramm oder auf der rechten Seite in der HTML-Liste uns auf der Dinner – navigieren, das wir dann optional für RSVP können:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt die Anwendungsfunktionalität unsere NerdDinner-Anwendung implementiert. Lassen Sie uns nun ansehen, wie wir aktivieren können automatisiert Einheit davon testen.

> [!div class="step-by-step"]
> [Zurück](use-ajax-to-deliver-dynamic-updates.md)
> [Weiter](enable-automated-unit-testing.md)
