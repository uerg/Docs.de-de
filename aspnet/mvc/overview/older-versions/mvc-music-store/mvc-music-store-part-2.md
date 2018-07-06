---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Teil 2: Controller | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 2 werden die Controller behandelt.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8824d5d2f5670aee2df6dc6e74767e4a851dd4ae
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841094"
---
<a name="part-2-controllers"></a>Teil 2: Controller
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.  
>   
> Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.  
>   
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 2 werden die Controller behandelt.


Mit herkömmlichen Web-Frameworks werden eingehende URLs in der Regel Dateien auf dem Datenträger zugeordnet. Zum Beispiel: eine Anforderung für eine URL wie "/ Products.aspx" oder "/" Products.PHP "" kann von einer "Products.aspx" oder ""Products.PHP ""-Datei verarbeitet werden.

Web-basierten MVC-Frameworks Servercode URLs in eine etwas andere Weise zugeordnet. Anstatt das Zuordnen von eingehenden URLs zu Dateien, sie stattdessen URLs auf Methoden in Klassen zugeordnet. Diese Klassen werden als "Controller" bezeichnet und sie sind zuständig für die Verarbeitung der eingehender HTTP-Anforderungen, Behandeln von Benutzereingaben, abrufen und Speichern von Daten und bestimmen die zu sendende Antwort zurück an den Client (HTML-Seite anzeigen, Herunterladen einer Datei, auf einen anderen umleiten URL usw.).

## <a name="adding-a-homecontroller"></a>Hinzufügen einen HomeController

Zunächst müssen wir unsere MVC Music Store-Anwendung Hinzufügen einer Controllerklasse, die URLs, die auf der Startseite unserer Website behandelt. Wir führen Sie die Standard-Benennungskonventionen von ASP.NET MVC und HomeController aufrufen.

Mit der rechten Maustaste in des Ordners "Controllers" im Projektmappen-Explorer, und wählen Sie "Hinzufügen", und klicken Sie dann den Befehl "Controller...":

![](mvc-music-store-part-2/_static/image1.jpg)

Hierdurch wird das Dialogfeld "Controller hinzufügen" angezeigt. Nennen Sie den Controller "HomeController" aus, und drücken Sie die Schaltfläche "hinzufügen".

![](mvc-music-store-part-2/_static/image1.png)

Dies erstellt eine neue Datei "HomeController.cs", mit dem folgenden Code:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Um so weit wie möglich zu starten, ersetzen Sie die Index-Methode lassen Sie uns eine einfache Möglichkeit, die nur eine Zeichenfolge zurückgibt. Wir erstellen zwei Änderungen:

- Ändern Sie die Methode, um eine Zeichenfolge anstatt in ein Aktionsergebnis zurückgegeben.
- Ändern Sie die return-Anweisung um "Hello aus Home" zurückzugeben.

Die Methode sollte jetzt wie folgt aussehen:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Ausführen der Anwendung

Jetzt führen wir die Website. Können wir unsere Web-Server starten und Testen Sie die Website mithilfe eines der folgenden:

- Wählen Sie das Debuggen ⇨ Debuggen starten im Menü-Element
- Klicken Sie auf der grünen Pfeilschaltfläche auf der Symbolleiste ![](mvc-music-store-part-2/_static/image2.jpg)
- Verwenden Sie die Tastenkombination F5.

Mit einer der oben genannten Schritte wird unser Projekt kompilieren, und dann dazu führen, dass der ASP.NET Development Server, die integriert in Visual Web Developer gestartet wird. Eine Benachrichtigung wird angezeigt, in der unteren Ecke des Bildschirms, um anzugeben, dass der ASP.NET Development Server gestartet wurde, und zeigt der Portnummer an, dass er unter ausgeführt wird.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer wird dann automatisch ein Browserfenster geöffnet, dessen URL auf unsere Web-Server verweist. Dies ermöglicht uns, wie Sie unserer Webanwendung testen:

![](mvc-music-store-part-2/_static/image3.png)

Okay, das war ziemlich schnell – wir eine neue Website erstellt haben, eine Zeile mit drei-Funktion hinzugefügt, und haben wir Text in einem Browser. Nicht rocket Science, aber es ist der Anfang.

*Hinweis: Visual Web Developer bietet ASP.NET Development Server, die Ihre Website auf einer Reihe zufälliger kostenlose "Port" ausgeführt wird. Im obigen Screenshot, der Standort ausgeführt wird, am `http://localhost:26641/`, sodass sie Port 26641 verwendet wird. Die Portnummer wird unterschiedlich sein. Wenn wir in diesem Tutorial zu der URL wie /Store/Browse sprechen, geht, die nach der Portnummer. Wenn 26641 die Portnummer an, navigieren zu/Store/durchsuchen bedeutet Navigieren zu `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Hinzufügen einer StoreController

Wir haben einen einfachen HomeController, die auf der Startseite unserer Website implementiert hinzugefügt. Nun fügen Sie einen anderen Controller, mit denen auf die Durchsuchen-Funktionalität unseres neuen Speichers Musik zu implementieren. Unser Controller Store unterstützt drei Szenarien:

- Eine Angebotsseite von Musik Genres in unserer Music store
- Seite "Durchsuchen", der alle von der Alben in einem bestimmten Genre auflistet
- Eine Seite, die Informationen zu einem bestimmten Musikalbum anzeigt

Wir beginnen, indem Sie eine neue StoreController-Klasse hinzufügen... Wenn Sie noch nicht geschehen, beenden Sie die Anwendung entweder über den Browser schließen, oder wählen das Debuggen ⇨ Debuggen beenden-Menüelement ausführen.

Fügen Sie nun eine neue StoreController hinzu. Wie wir mit HomeController, wir erreichen dies, indem mit der rechten Maustaste auf den Ordner "Controllers" im Projektmappen-Explorer und Auswählen der hinzufügen -&gt;Controller-Menüelement

![](mvc-music-store-part-2/_static/image4.png)

Unsere neue StoreController verfügt bereits über eine Methode "Index". Wir verwenden diese Methode "Index" auf um unserer Seite mit den Preisen zu implementieren, die alle Genres in unserer Music Store auflistet. Wir fügen auch zwei weitere Methoden, um die beiden anderen Szenarien implementieren wir unsere StoreController behandeln soll: Durchsuchen und Details.

Diese Methoden ("Index", "Durchsuchen" und "Details") in den Controller "Controlleraktionen" aufgerufen werden, und wie Sie mit der HomeController.Index ()-Aktionsmethode bereits gesehen haben, ist ihre Arbeit auf URL-Anforderungen zu reagieren, und (im Allgemeinen) zu ermitteln welcher Inhalt sollten gesendet werden, zurück an den Browser oder die Benutzer, die die URL aufgerufen.

Wird zunächst unsere Implementierung StoreController ändern theIndex()-Methode, um die Zeichenfolge "Hello aus Store.Index()" zurückzugeben, und fügen es ähnliche Methoden für Browse() und Details():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Führen Sie das Projekt erneut aus, und Durchsuchen Sie die folgenden URLs:

- /Store
- / Store/durchsuchen
- / Store/Details

Zugriff auf diese URLs werden Aufrufen die Aktionsmethoden in unserem Controller und String-Antworten zurückgeben:

![](mvc-music-store-part-2/_static/image5.png)

Das ist großartig, aber dies sind nur Konstanten Zeichenfolgen. Lassen Sie uns diese dynamisch, damit sie nutzen Informationen aus der URL, und sie auf der Seite ausgegeben zeigt.

Zuerst ändern wir die Aktionsmethode durchsuchen, um eine Querystring-Wert aus der URL abzurufen. Dazu können wir unsere Aktionsmethode einen Parameter für "Genre" hinzugefügt. Wenn wir dies tun übergibt ASP.NET MVC automatisch alle Querystring oder Formular Post-Parameter, die mit dem Namen "Genre" in unserem Aktionsmethode, wenn sie aufgerufen wird.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Hinweis: Die HttpUtility.HtmlEncode durchführen Utility-Methode verwendet, um die Benutzereingabe zu bereinigen. Dadurch wird verhindert, dass Benutzer Einfügen von Javascript in unserer Ansicht mit einem Link wie /Store/Browse? Genre =&lt;Skript&gt;Window.Location = "http://hackersite.com"&lt;/script&gt;.*

Nun navigieren wir zu/Store/durchsuchen? Genre = Disco

![](mvc-music-store-part-2/_static/image6.png)

Als Nächstes ändern wir die Aktion "Details" zum Lesen und Anzeigen der Eingabeparameter mit dem Namen ID. Im Gegensatz zu unseren vorherigen Methode wird nicht wir den ID-Wert als Querystring-Parameter einbetten. Stattdessen werden wir es direkt in der URL selbst einbetten. Zum Beispiel: /Store/Details/5.

ASP.NET MVC ermöglicht es uns, problemlos erreichen, ohne etwas konfigurieren zu müssen. Routing ASP.NETs Standardkonvention ist, das eine URL-Segment nach dem Namen der Aktionsmethode als einen Parameter namens "ID" zu behandeln. Wenn Ihrer Aktionsmethode einen Parameter namens-ID hat wird ASP.NET MVC automatisch das URL-Segment für Sie als Parameter übergeben.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Führen Sie die Anwendung, und navigieren Sie zu /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Betrachten wir nun, was wir bisher gemacht haben:

- Wir haben ein neues ASP.NET MVC-Projekt in Visual Web Developer erstellt.
- Wir haben die grundlegenden Ordnerstruktur einer ASP.NET MVC-Anwendung erläutert.
- Wir haben wie unsere Website, die mithilfe von ASP.NET Development Server ausgeführt werden gelernt.
- Wir haben zwei Controller-Klassen erstellt: einen HomeController, und eine StoreController
- Wir haben Aktionsmethoden unsere-Controller hinzugefügt, das auf URL-Anforderungen reagieren und Text an den Browser zurück.


> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-1.md)
> [Weiter](mvc-music-store-part-3.md)
