---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Teil 2: Controller | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 2 werden Controller behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-2-controllers"></a>Teil 2: Controller
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.  
>   
> Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.  
>   
> Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 2 werden Controller behandelt.


Mit herkömmlichen webframeworks werden eingehende URLs in der Regel Dateien auf dem Datenträger zugeordnet. Zum Beispiel: eine Anforderung für eine URL wie "/ Products.aspx" oder "/ Products.php" möglicherweise durch eine Datei "Products.aspx" oder "Products.php" verarbeitet werden.

MVC-Frameworks webbasierte ordnen URLs Servercode etwas anders. Statt eingehende URLs Dateien, ordnen sie stattdessen URLs an Methoden für Klassen. Diese Klassen werden als "Controller" bezeichnet und sie sind verantwortlich für die Verarbeitung von eingehenden HTTP-Anforderungen, Behandeln von Benutzereingaben, abrufen und Speichern von Daten und bestimmen die zu sendende Antwort zurück an den Client (HTML-Seite anzeigen, eine Datei herunterladen, zu einer anderen umleiten URL, usw.).

## <a name="adding-a-homecontroller"></a>Hinzufügen einer HomeController

Wir beginnen unsere MVC Music Store-Anwendung durch Hinzufügen einer Controllerklasse, die URLs auf der Startseite unserer Website behandelt. Wir führen Sie die Standard-Benennungskonventionen von ASP.NET MVC und ihn HomeController aufrufen.

Mit der rechten Maustaste im Projektmappen-Explorer des Ordners "Controller" und wählen Sie "Hinzufügen" und dann auf "Controller..." Befehl:

![](mvc-music-store-part-2/_static/image1.jpg)

Hierdurch wird das Dialogfeld "Controller hinzufügen" angezeigt. Nennen Sie den Controller "HomeController.cs", und drücken Sie die Schaltfläche "hinzufügen".

![](mvc-music-store-part-2/_static/image1.png)

Dies erstellt eine neue Datei HomeController.cs, durch den folgenden Code:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Um so weit wie möglich zu starten, ersetzen Sie wir die Index-Methode, mit der eine einfache Methode, die nur eine Zeichenfolge zurückgibt. Wir stellen zwei Änderungen:

- Ändern Sie die Methode zum Zurückgeben einer Zeichenfolge statt ein Aktionsergebnis dar
- Ändern Sie die return-Anweisung um "Hello aus" zurückzukehren.

Die Methode sollte jetzt wie folgt aussehen:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Ausführen der Anwendung

Jetzt führen Sie zunächst den Standort. Wir können unsere Webserver starten und probieren Sie die Website mit einer der folgenden::

- Wählen Sie das Debuggen starten Menüelement für Debug ⇨
- Klicken Sie auf der grünen Pfeilschaltfläche auf der Symbolleiste ![](mvc-music-store-part-2/_static/image2.jpg)
- Verwenden Sie die Tastenkombination F5.

Mit einer der oben genannten Schritte werden unsere Projekt kompilieren und dann führen den ASP.NET Development Server, die erstellt in Visual Web Developer gestartet wird. Eine Benachrichtigung erscheint in der unteren Ecke des Bildschirms, um anzugeben, dass ASP.NET Development Server gestartet wurde, und zeigt der Portnummer an, dass er unter ausgeführt wird.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer wird dann automatisch geöffnet ein Browserfenster mit unserem Webserver, dessen URL verweist. Dadurch können wir schnell die Webanwendung zu testen:

![](mvc-music-store-part-2/_static/image3.png)

Okay, das war ziemlich schnell – wir eine neue Website erstellt, hinzugefügt, eine drei-Zeile-Funktion, und wir haben Text in einem Browser. Nicht rocket Science allerdings handelt es sich um einen Einstieg.

*Hinweis: Visual Web Developer enthält den ASP.NET Development Server, die Ihre Website für eine Anzahl von zufälligen freien "Port" ausgeführt wird. In der oben dargestellten Screenshot die Website ausgeführt wird, am `http://localhost:26641/`, sodass Port 26641 verwendet wird. Die Portnummer wird unterschiedlich sein. Wenn wir in diesem Lernprogramm URLs like /Store/Browse erörtern, geht, die nach der Portnummer. Wenn 26641 die Portnummer an, Navigieren zum/Store/durchsuchen bedeutet Navigieren zum `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Hinzufügen einer StoreController

Eine einfache HomeController, die die Homepage von unserer Website implementiert wurde hinzugefügt. Nun fügen wir einen anderen Controller, mit dem wir die Suche nach Funktionalität unserer-Music Store implementieren. Unser Store-Controller unterstützt drei Szenarien:

- Eine Auflistung-Seite mit der Musik Genres in unserer-Music store
- Seite "Durchsuchen", die alle von der Musikalben in einer bestimmten "Genre" aufgelistet sind
- Seite "Details", das Informationen zu einem bestimmten Musikalbum anzeigt

Wir beginnen, indem Sie eine neue StoreController-Klasse hinzufügen... Wenn Sie nicht bereits geschehen, beenden Sie die Ausführung der Anwendung durch Schließen des Browsers oder das Beenden des Debuggens Menüelement für Debug ⇨ auswählen.

Fügen Sie jetzt eine neue StoreController hinzu. Wie wir mit HomeController haben, müssen wir dies ausführen, indem Sie mit der rechten Maustaste auf den Ordner "Controller" im Projektmappen-Explorer und Add -&gt;Controller Menüelement

![](mvc-music-store-part-2/_static/image4.png)

Unsere neue StoreController verfügt bereits über eine "Index"-Methode. Wir verwenden diese Methode "Index" unsere Angebotsseite implementieren, die alle Genres in unserer-Music Store aufgelistet. Fügen wir auch zwei weitere Methoden, um die beiden anderen Szenarien implementieren wir unsere StoreController behandeln soll: Durchsuchen und Details.

Diese Methoden ("Index", "Durchsuchen" und "Details") in unserer Controller "Controlleraktionen" aufgerufen werden, und wie Sie bereits mit der HomeController.Index ()-Aktionsmethode gesehen haben, ist ihre Arbeit auf URL-Anforderungen zu reagieren und welche Inhalte (im Allgemeinen) ermitteln sollte gesendet werden, zurück an den Browser oder Benutzer, der die URL aufgerufen hat.

Beginnen wir unsere StoreController Implementierung durch Ändern der theIndex()-Methode, um die Zeichenfolge "Hello aus Store.Index()" zurück, und wir werden ähnliche Methoden für Browse() und Details() hinzufügen:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Führen Sie das Projekt erneut aus, und Durchsuchen Sie die folgenden URLs:

- /Store
- /Store/Browse
- /Store/Details

Zugriff auf diese URLs wird Aufrufen der Aktionsmethoden in unserer Controller und Antworten Zeichenfolge zurückzugeben:

![](mvc-music-store-part-2/_static/image5.png)

Das ist hervorragend, aber dies nur Konstanten Zeichenfolgen sind. Stellen sie dynamisch, damit sie Informationen aus der URL und sie auf der Seite ausgegeben zeigt.

Wechseln Sie zunächst die Aktionsmethode durchsuchen, um eine Querystring-Wert aus der URL abzurufen. Dazu können wir unsere Aktionsmethode einen Parameter "Genre" hinzugefügt. Wenn wir dies tun übergibt ASP.NET-MVC automatisch alle Abfragezeichenfolge und Formularbereitstellungsparameter, mit dem Namen "Genre" in unseren Aktionsmethode, wenn er aufgerufen wird.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Hinweis: Wir haben HttpUtility.HtmlEncode durchführen Utility-Methode verwenden, um die Benutzereingabe zu bereinigen. Dadurch wird verhindert, dass Benutzer Räumen Javascript in unserer Sicht mit einem Link wie /Store/Browse? "Genre" =&lt;Skript&gt;window.location= "http://hackersite.com"&lt;/script&gt;.*

Jetzt sehen wir navigieren Sie zu/Store/durchsuchen? "Genre" Disco =

![](mvc-music-store-part-2/_static/image6.png)

Nehmen wir als Nächstes die Aktion "Details" zum Lesen und Anzeigen der Eingabeparameter mit dem Namen ID ändern Im Gegensatz zu unserer vorherigen Methode wird nicht wir den ID-Wert als eine Querystring-Parameter einbetten. Wir müssen es stattdessen direkt in der URL selbst einbetten. Zum Beispiel: /Store/Details/5.

ASP.NET MVC können uns problemlos erreichen, ohne etwas konfigurieren zu müssen. Routing ASP.NETs Standardkonvention ist, das eine URL-Segment nach dem Methodennamen Aktion als einen Parameter namens "ID" zu behandeln. Wenn die Aktionsmethode einen Parameter namens ID hat wird ASP.NET MVC automatisch das URL-Segment für Sie als Parameter übergeben.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Führen Sie die Anwendung, und navigieren Sie zu /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Wir kurz zusammengefasst, bisher Fertigstellung:

- Wir haben ein neues ASP.NET MVC-Projekt in Visual Web Developer erstellt.
- Wir haben die grundlegenden Ordnerstruktur einer ASP.NET MVC-Anwendung erläutert.
- Wir haben gelernt, wie Sie unsere Website mithilfe von ASP.NET Development Server ausführen
- Wir haben zwei Controllerklassen erstellt: ein HomeController und eine StoreController
- Wir haben Aktionsmethoden unserer Domänencontroller hinzugefügt, das auf URL-Anforderungen reagieren und Text an den Browser zurück.


> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-1.md)
> [Weiter](mvc-music-store-part-3.md)
