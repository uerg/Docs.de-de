---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Verwenden Sie Controller und Ansichten, um einer Auflistung/Detail-Benutzeroberfläche implementieren | Microsoft Docs
author: microsoft
description: Schritt 4 veranschaulicht das Hinzufügen ein Controllers an die Anwendung, die nutzt unserem Modell Benutzern einen Umgang mit Daten auflisten/Detail-Navigation bereitstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: ac3568941eeef24bd9857c5787471aadea15fc7f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Verwenden von Controller und Ansichten zum Implementieren einer Auflistung/Detail-Benutzeroberflächenautomatisierungs
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 4 mit einer kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 4 veranschaulicht das Hinzufügen ein Controllers an die Anwendung, die von unserem Modell Benutzern eine Liste/Detail-Daten Oberfläche Abendessen auf unserer Website NerdDinner Navigation bereitstellen nutzt.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner Schritt 4: Controller und Ansichten

Mit herkömmlichen webframeworks (klassisches ASP, PHP, ASP.NET Web Forms usw.) werden eingehende URLs in der Regel Dateien auf dem Datenträger zugeordnet. Zum Beispiel: eine Anforderung für eine URL wie "/ Products.aspx" oder "/ Products.php" möglicherweise durch eine Datei "Products.aspx" oder "Products.php" verarbeitet werden.

MVC-Frameworks webbasierte ordnen URLs Servercode etwas anders. Statt eingehende URLs Dateien, ordnen sie stattdessen URLs an Methoden für Klassen. Diese Klassen werden als "Controller" bezeichnet und sie sind verantwortlich für die Verarbeitung von eingehenden HTTP-Anforderungen, Behandeln von Benutzereingaben, abrufen und Speichern von Daten und bestimmen die zu sendende Antwort zurück an den Client (HTML-Seite anzeigen, eine Datei herunterladen, zu einer anderen umleiten URL, usw.).

Nun, dass wir für unsere NerdDinner Anwendung um ein grundlegendes Modell erstellt haben, werden im nächsten Schritt der Anwendung einen Controller hinzufügen, die es Benutzern einer Daten auflisten/Detail-Navigation für Abendessen auf unserer Website bieten nutzt.

### <a name="adding-a-dinnerscontroller-controller"></a>Hinzufügen eines Controllers DinnersController

Wir beginnen, indem Sie mit der rechten Maustaste auf den Ordner "Controller" in unseren Webprojekt und wählen Sie dann die **Add -&gt;Controller** Menübefehl (Sie können auch Ausführen dieses Befehls durch Eingabe von STRG-M, STRG + C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Hierdurch wird das Dialogfeld "Controller hinzufügen" angezeigt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Wir nennen Sie den neuen Controller "DinnersController" und klicken Sie auf die Schaltfläche "Hinzufügen". Visual Studio wird dann eine Datei DinnersController.cs unsere \Controllers-Verzeichnis hinzufügen:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Es wird auch von der neuen DinnersController Klasse innerhalb der Code-Editor geöffnet.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Hinzufügen von Index() und Details() Aktionsmethoden zur DinnersController-Klasse

Wir möchten Besuchern, die die Anwendung eine Liste der bevorstehenden Abendessen durchsuchen und ermöglicht es ihnen, klicken auf alle Dinner in der Liste, um bestimmte Details dazu finden Sie unter aktivieren. Dazu müssen wir die folgenden URLs aus der Anwendung zu veröffentlichen:

| **URL** | **Purpose** |
| --- | --- |
| */Dinners/* | Eine HTML-Liste von bevorstehenden Abendessen anzeigen |
| */Dinners/Details/[id]* | Anzeigen von Details zu einer bestimmten Dinner, angegeben durch ein "Id"-Parameter, die in der die DinnerID von der Dinner in der Datenbank übereinstimmen, wird der URL eingebettet. Zum Beispiel: /Dinners/Details/2 würde eine HTML-Seite mit den Details der Dinner DinnerID, dessen Wert 2 angezeigt. |

Durch Hinzufügen von zwei öffentlichen "Aktionsmethoden"-Klasse DinnersController wie unten veröffentlichen wir anfängliche Implementierungen der folgenden URLs:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Wir müssen dann führen Sie die Anwendung NerdDinner und verwenden Sie unsere Browser aufrufen. Geben Sie der *"/ Abendessen /"* URL führt dazu, dass unsere *Index()* aufzurufende Methode ausführen, und es wird wieder die folgende Antwort senden:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Geben Sie der *"/ Abendessen/Informationen/2"* URL führt dazu, dass unsere *Details()* Methode zum Ausführen und senden Sie die folgende Antwort zurück:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Sie Fragen sich möglicherweise - wie ASP.NET MVC wussten, unsere DinnersController-Klasse erstellen und diese Methoden aufrufen? Zu wissen, dass wir einen kurzen Blick auf die Funktionsweise des Routings dauern.

### <a name="understanding-aspnet-mvc-routing"></a>Grundlegendes zu ASP.NET MVC-Routings

ASP.NET MVC umfasst eine leistungsstarke URL-Routingmodul, das ein hohes Maß an Flexibilität bei der Steuerung der Zuordnung von URLs zu Controllerklassen bereitstellt. Kontakt mit vollständig anpassen, wie ASP.NET MVC wählt die Controllerklasse zu erstellen, welche Methode dafür aufrufen sowie das Konfigurieren von Möglichkeiten, um Variablen automatisch in die Abfragezeichenfolge analysiert und an die Methode als Parameter übergeben Argumente. Es bietet die Flexibilität, völlig optimieren eine Website für SEO (Search Engine Optimization) sowie alle URL-Struktur, die wir von einer Anwendung möchten veröffentlichen.

Standardmäßig sind neue ASP.NET MVC-Projekte mit einem vorkonfigurierten Satz von URL-Routingregeln, die bereits registriert. Dadurch kann wir einfach auf eine Anwendung beginnen, ohne etwas explizit zu konfigurieren. Registrierungen Regel routing der Standardeinstellung können innerhalb der Klasse "Application" unsere Projekte – gefunden werden, die wir durch Doppelklicken auf die Datei "Global.asax" im Stammverzeichnis des unsere Projekt öffnen können:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Die ASP.NET-MVC-Standardregeln für routing sind innerhalb der Methode "RegisterRoutes" dieser Klasse registriert:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Die "Routen. MapRoute() "Methodenaufruf oben registriert Standardroutingregel, der eingehende URLs Controllerklassen mit URL-Format zugeordnet:" / {Controller} / {Aktion} / {Id} "– wobei"Controller"der Name der Controllerklasse instanziiert wird, ist"Action"ist der Name einer öffentliche Methode hinzu, und "Id" aufgerufen werden soll, ist ein optionaler Parameter, die eingebettete innerhalb der URL, die als Argument an die Methode übergeben werden kann. Der dritte Parameter übergeben, für den Methodenaufruf "MapRoute()" ist ein Satz von Standardwerten für die Controller/Aktion/Id-Werte verwenden, wenn sie nicht in der URL vorhanden sind (Controller = "Home", Aktion = "Index", Id = "").

Im folgenden finden Sie eine Tabelle, die veranschaulicht, wie eine Vielzahl von URLs zugeordnet sind, unter Verwendung des standardmäßigen "<em>/ {Controller} / {Aktion} / {Id}"</em>Weiterleitungsregel:

| **URL** | **Controller-Klasse** | **Action-Methode** | **Übergebenen Parameter** |
| --- | --- | --- | --- |
| */Dinners/Details/2* | DinnersController | Details(ID) | id=2 |
| */Dinners/Edit/5* | DinnersController | Edit(ID) | id=5 |
| */Dinners/Create* | DinnersController | Eine Signatures-Auflistung | Nicht zutreffend |
| */Dinners* | DinnersController | Index() | Nicht zutreffend |
| */Home* | HomeController | Index() | Nicht zutreffend |
| */* | HomeController | Index() | Nicht zutreffend |

Die letzten drei Zeilen angezeigt werden die Standardwerte (Controller = Home, Aktion = Index, Id = "") verwendet wird. Da die Methode "Index" als Name für die Standard-Aktion registriert ist, wenn eine nicht angegeben ist, die "/ Abendessen" und "/ Home" URLs durch einen der Aktionsmethode Index() für ihre Controllerklassen aufgerufen werden soll. Da des Controllers "Home" als Standard-Domänencontroller registriert ist, wenn eine nicht angegeben ist, führt dazu, dass die URL "/" HomeController erstellt werden soll, und die Aktionsmethode Index() darauf, aufgerufen werden soll.

Wenn Sie diese routing-URL-Standardregeln nicht mögen, ist die gute Nachricht sind einfach zu ändern: Bearbeiten Sie sie nur innerhalb der oben genannten RegisterRoutes-Methode. Für unsere Anwendung NerdDinner jedoch hier nicht die Senderegeln für die Standard-URL nicht ändern – stattdessen nur verwenden wir diese als-ist.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Verwenden die DinnerRepository aus unserem DinnersController

Ersetzen Sie die aktuelle Implementierung von der DinnersController wir jetzt Index() und Details() Aktionsmethoden mit-Implementierungen, unserem Modell verwenden.

Wir verwenden die DinnerRepository-Klasse, die wir zuvor erstellt, um das Verhalten zu implementieren. Wir beginnen, indem eine "using-Anweisung, die den Namespace"NerdDinner.Models"verweist auf Hinzufügen, und deklarieren Sie eine Instanz von unseren DinnerRepository als Feld für unsere DinnerController-Klasse.

Weiter unten in diesem Kapitel wir führen das Konzept der "Abhängigkeitsinjektion" und eine weitere Möglichkeit für unsere Controller aus, um einen Verweis auf eine DinnerRepository erhalten, die ermöglicht eine bessere Einheit testen – aber für Recht anzeigen, jetzt wir nur eine Instanz von unseren DinnerRepository erstellen Inline wie unten.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Jetzt können wir unsere Modellobjekte abgerufenen Daten mit HTML-Antwort generiert.

### <a name="using-views-with-our-controller"></a>Verwenden von Sichten mit unserem Controller

Es ist zwar möglich, zum Schreiben von Code in unserer Aktionsmethoden HTML zusammenzustellen und dann mithilfe der *Response.Write()* Hilfsmethode hinzu, dass Ansatz schnell relativ unhandlich wird zurück an den Client senden. Ein viel besserer Ansatz ist für uns bei der Anwendung und Datenlogik in unserer DinnersController Aktionsmethoden nur ausgeführt, und übergeben Sie die Daten, die zum Rendern von HTML-Antwort in eine separate "View"-Vorlage, die zum Ausgeben der HTML-Darstellung zuständig ist erforderlich davon. Wie es in wenigen Augenblicken sehen werden, ist eine "view"-Vorlage eine Textdatei, die in der Regel eine Kombination aus HTML-Markup und Code zum Rendern der eingebetteten enthält.

Trennen unsere Controllerlogik unserer Ansicht rendern, bringt mehrere große Vorteile. Insbesondere ist es hilfreich, eine klare "Abgrenzung der Aspekte" zwischen dem Anwendungscode und die Formatierung/Rendering Benutzeroberflächencode zu erzwingen. Dies erleichtert, Komponententests Anwendungslogik isoliert von UI-Renderinglogik. Dies erleichtert die UI-Rendering-Vorlagen später ändern, ohne dass Änderungen am Anwendungscode vornehmen. Und es kann erleichtern Entwicklern und Designern zusammen auf Projekte zusammenarbeiten.

Wir können unsere DinnersController-Klasse, um anzugeben, dass wir Ansichtenvorlage verwenden, um wieder eine HTML-UI-Antwort senden, indem die Methodensignaturen unsere zwei Aktionsmethoden müssen einen Rückgabetyp "void", um stattdessen einen Rückgabetyp von "ActionResult" ändern möchten, aktualisieren. Wir rufen können dann die *View()* Hilfsmethode für die Basisklasse für Controller wieder zurückgeben ein Objekts "ViewResult" wie unten:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

Die Signatur der *View()* Hilfsmethode wir oben verwenden sieht wie unten:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Der erste Parameter für die *View()* Hilfsmethode ist der Name der Ansicht-Vorlagendatei, die zum Rendern die HTML-Antwort verwendet werden soll. Der zweite Parameter ist ein Modellobjekt, das Daten, die der Vorlage anzeigen benötigt enthält, um die HTML-Antwort zu rendern.

In unserem Index() Aktionsmethode aufrufen wir die *View()* Hilfsmethode und angeben, dass eine HTML-Sprachfeatures Abendessen mithilfe einer Vorlage der "Index" Ansicht gerendert werden soll. Wir sind der Vorlage anzeigen eine Sequenz von Dinner-Objekten, die der Liste erzeugen übergeben:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

In unserem Details() Aktionsmethode Versuch Abrufen eines Dinner-Objekts, das mit der Id innerhalb der URL bereitgestellt werden. Wenn eine gültige Dinner gefunden wird, rufen wir die *View()* Hilfsmethode, der angibt, Ansichtenvorlage "Details" verwenden, um die abgerufenen Objekts Dinner gerendert werden soll. Wenn ein ungültiger Dinner angefordert wird, Rendern wir eine hilfreiche Fehlermeldung, der angibt, dass die Dinner mithilfe einer "NotFound" Ansichtenvorlage nicht vorhanden (und eine überladene Version der der *View()* Hilfsmethode, die nur den Vorlagennamen akzeptiert ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Nun implementieren wir die Ansichtsvorlagen "NotFound", "Details" und "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementieren die Vorlage der "NotFound" anzeigen

Wir beginnen, durch die Implementierung der Vorlage anzeigen "NotFound" – dem angezeigt wird, eine benutzerfreundliche Fehlermeldung an, die, dass der angeforderte Dinner nicht gefunden werden kann.

Wir werden unsere Textcursor innerhalb einer Controlleraktionsmethode Positionierung erstellen eine neue Ansichtenvorlage für die mit der rechten Maustaste und klicken Sie Befehls im Menü "Ansicht hinzufügen" (es können auch Ausführen dieses Befehls durch Eingabe von STRG-M, STRG + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Hierdurch wird ein Dialogfeld "Ansicht hinzufügen" wie unten angezeigt. In der Standardeinstellung im Dialogfeld den Namen des zu erstellenden Ansicht vorab aufgefüllt wurde entsprechend den Namen der Aktionsmethode des Cursors, wenn das Dialogfeld "(in diesem Fall"Details") gestartet wurde. Da wir zunächst die Vorlage "NotFound" implementieren möchten, wir überschreiben diese Sichtname und legen Sie sie stattdessen "NotFound" sein:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Wenn wir die Schaltfläche "Hinzufügen" klicken, wird Visual Studio eine neue Vorlage für "NotFound.aspx" Ansicht für uns innerhalb des Verzeichnisses "\Views\Dinners" erstellt (die auch erstellt wird, wenn das Verzeichnis nicht bereits vorhanden ist):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Es wird auch unsere neue "NotFound.aspx" Ansicht-Vorlage in den Code-Editor geöffnet:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Ansichtsvorlagen standardmäßig haben zwei "Inhaltsbereiche", in dem wir Inhalte und Code hinzufügen können. Die erste erlaubt es uns zum Anpassen der "Title" der HTML-Seite gesendet wird. Die zweite erlaubt es uns zum Anpassen der "Hauptinhalt" der HTML-Seite gesendet wird.

Zum Implementieren der unsere "NotFound" Ansichtenvorlage fügen wir einige grundlegende Inhalt:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Wir können Sie sich dann innerhalb des Browsers versuchen. Wir hierzu Anforderung der *"/ Abendessen/Informationen/9999"* URL. Dies wird auf eine Dinner verweisen, die nicht aktuell in der Datenbank vorhanden, und führt dazu, dass unsere DinnersController.Details() Aktionsmethode zum Rendern der Vorlage unsere "NotFound" anzeigen:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Aufpassen, Sie in der Abbildung oben sehen, ist, dass unsere Übersicht-Vorlage eine Reihe von HTML geerbt hat, der den Hauptinhalt auf dem Bildschirm umgibt. Dies ist da unsere Vorlage anzeigen eine Vorlage "Masterseite" verwendet, die wir ein konsistentes Layout für alle Sichten auf der Website gelten können. Wie in einer späteren Teil dieses Lernprogramms weitere Masterseiten erläutert.

### <a name="implementing-the-details-view-template"></a>Implementieren die Ansicht "Details"-Vorlage

Nun implementieren wir Vorlage anzeigen "Details" – die HTML für eine einzelnes Dinner Modell generieren wird.

Wir müssen Positionierung unserer Textcursor innerhalb der Aktionsmethode Details hierzu und klicken Sie dann mit der rechten Maustaste und wählen Sie den Befehl im Menü "Ansicht hinzufügen" (oder drücken Sie STRG-M, STRG + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt. Wir behalten den Standardnamen für Ansicht ("Details"). Wir auch das Kontrollkästchen "Stark typisierte Ansicht erstellen" im Dialogfeld fort und wählen (mithilfe der Dropdownliste Combobox) der Name des Modelltyps, die wir auf dem Controller an die Ansicht übergeben. Für diese Ansicht, die wir übergeben eines Dinner-Objekts (der vollqualifizierte Name für diesen Typ ist: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Im Gegensatz zu der vorherigen Vorlage, in dem wir eine "leer"Sicht erstellen möchten, dieses Mal, die wir automatisch ausgewählt wird "Gerüst" der Ansicht, die mithilfe einer Vorlage "Details". Es kann dies darauf hinweisen, durch Ändern der Dropdownliste die "Inhalt anzeigen" im Dialogfeld "oben".

"Gerüstbau" generieren eine anfängliche Implementierung unserer Details anzeigen Vorlage basierend auf der Dinner-Objekt, das wir an ihn übergeben werden. Dies bietet eine einfache Möglichkeit für uns, unsere Ansicht Vorlage Implementierung Schnelleinstieg.

Wenn wir die Schaltfläche "Hinzufügen" klicken, wird Visual Studio eine neue "Details.aspx" Ansicht-Vorlagendatei für uns in unserem "\Views\Dinners" Verzeichnis erstellt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Es wird auch unsere neue "Details.aspx" Ansicht-Vorlage in den Code-Editor geöffnet. Es wird eine anfängliche Gerüst Implementierung einer Detailansicht auf Grundlage eines Modells Dinner enthalten. Das Modul Gerüstbau Reflektion .NET, betrachten Sie die öffentlichen Eigenschaften verfügbar gemacht werden, in der Klasse übergeben und entsprechende Inhalt basierend auf den einzelnen gefundenen wird hinzufügen:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Wir können anfordern, die *"/ Abendessen/Informationen/1"* URL, um festzustellen, wie diese Gerüst-Implementierung von "details" im Browser aussieht. Verwenden diese URL wird eine von der Abendessen angezeigt, die wir manuell unserer Datenbank hinzugefügt, wenn wir zuerst erstellt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Dies ruft uns schnell einzurichten und freizugeben und stellt uns mit einer anfänglichen Implementierung unserer Ansicht "Details.aspx" haben. Wir können wechseln und sie zum Anpassen der Benutzeroberfläche zu unserem Zufriedenheit optimieren.

Wenn die Vorlage "Details.aspx" haben genauer betrachten, werden wir feststellen, dass es enthält statische HTML-Code als auch Renderingcodes eingebettet. &lt;%%&gt; Code Brocken Code ausführen, wenn die Ansichtenvorlage gerendert wird, und &lt;% = %&gt; Code Brocken führen Sie den Code, der in ihnen enthaltenen und rendert Sie dann das Ergebnis in den Ausgabestream, der Vorlage.

Wir können Code in unserer Ansicht schreiben, die auf das Modellobjekt "Dinner" zugreift, das von unseren Controller mithilfe einer stark typisierten "Model"-Eigenschaft übergeben wurde. Visual Studio bietet uns vollständiger Code-IntelliSense-Unterstützung beim Zugriff auf diese "Modell" Eigenschaft innerhalb des Editors:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Wir stellen sein, damit die Quelle für unsere letzte Details anzeigen Vorlage unten aussieht:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Wenn wir Zugriff auf die *"/ Abendessen/Informationen/1"* URL erneut es wird nun Render wie unten:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementieren die Vorlage der "Index" anzeigen

Nun implementieren wir Vorlage anzeigen "Index" – die eine Auflistung von bevorstehenden Abendessen generiert wird. Die Aufgabe dies wir unsere Textcursor innerhalb der Aktionsmethode für den Index zu positionieren und klicken Sie dann mit der rechten Maustaste klicken Sie auf, und wählen Sie den Befehl im Menü "Ansicht hinzufügen" (oder drücken Sie STRG-M, STRG + V).

Innerhalb des Dialogfelds "Ansicht hinzufügen" Wir behalten Sie die Ansichtenvorlage namens "Index" und aktivieren Sie das Kontrollkästchen "Eine stark typisierte Ansicht erstellen". Wir werden automatisch generieren einer Ansicht "Listenvorlage", und wählen "NerdDinner.Models.Dinner" als Typ des Modells an die Ansicht übergeben, wählen Sie diesmal (die da wir angegeben haben, erstellen wir eine "Liste" Gerüst dazu, das Dialogfeld Ansicht hinzufügen dass führt, wird davon ausgegangen werden Übergeben einer Sequenz von Objekten Dinner aus unserem Controller zur Ansicht):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Wenn wir die Schaltfläche "Hinzufügen" klicken, erstellt Visual Studio eine neue "Index.aspx" Ansicht Vorlagendatei für uns in unserem "\Views\Dinners" Verzeichnis. Es wird "eine anfängliche Implementierung darin, die eine HTML-Tabellenliste der Abendessen bereitgestellt, die wir auf die Ansicht übergeben Gerüst".

Ausführen der Anwendung und den Zugriff der *"/ Abendessen /"* wird gerendert, unserer Liste Abendessen URL wie folgt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

Die oben aufgeführten Tabelle-Projektmappe erhalten Sie ein Raster-ähnliches Layout mit unserer Daten Dinner – entspricht nicht sehr unseren vorstellungen für unsere Consumer mit Internetzugriff Dinner-Auflistung. Wir können aktualisieren Index.aspx anzeigen und ändern, um weniger Spalten mit Daten aus, und Verwenden einer &lt;Ul&gt; Element sie statt einer Tabelle mit den folgenden Code gerendert werden soll:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Wir verwenden das Schlüsselwort "Var" in der oben genannten Foreach-Anweisung, da wir über jede Dinner in unserem Modell Schleife. Die nicht mithilfe von c# 3.0 vertraut können mithilfe von "Var" bedeutet, dass die Dinner Objekt spät gebundene vorstellen. Stattdessen bedeutet das, dass der Compiler Typrückschluss für die stark typisierte "Model"-Eigenschaft verwendet wird (vom Typ "IEnumerable&lt;Dinner&gt;") und die lokale "Dinner"-Variable als Dinner-Typ – d. h., erhalten wir vollständige kompilieren IntelliSense und der Kompilierung dafür in Codeblöcken:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Bei Treffer Aktualisierung auf die */Dinners* URL in unserem Browser unsere aktualisierte Ansicht sieht wie unten:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Dies ist eine bessere – suchen, jedoch befindet sich noch nicht vollständig vorhanden. Der letzte Schritt ist zum Aktivieren von Endbenutzern, klicken Sie auf einzelne Abendessen in der Liste aus, und finden Sie weitere Informationen zu diesen. Implementieren wir dies durch das Rendern von HTML-Hyperlink-Elemente, die an die Aktionsmethode Details auf unserer DinnersController verknüpft sind.

Wir können diese Hyperlinks in unserer Indexansicht auf zwei Arten generieren. Der erste Schritt besteht im Erstellen Sie manuell auf HTML &lt;eine&gt; Elemente wie unten, wo wir einbetten &lt;%%&gt; blockiert innerhalb der &lt;eine&gt; HTML-Element:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Ein alternativer Ansatz wir verwenden wird die integrierte "Html.ActionLink()" Hilfsmethode in ASP.NET MVC nutzen, das programmgesteuerte Erstellen einer HTML unterstützt &lt;ein&gt; Element, das mit einer anderen Aktionsmethode auf verknüpft eine Domänencontroller:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Der erste Parameter für die Hilfsmethode Html.ActionLink() ist der anzuzeigenden Linktext (in diesem Fall die Titel von der Dinner), der zweite Parameter ist der Controller Aktionsname wir den Link, um (Details-Methode in diesem Fall) generieren möchten, und der dritte Parameter ist ein Satz von Parametern zum Senden an die Aktion (implementiert als anonymer Typ mit Eigenschaftennamen und Werten). In diesem Fall geben wir den Parameter "Id" von der Dinner wir eine Verknüpfung herstellen möchten, und da die Standard-URL-routing in ASP.NET MVC Regel ist "{Controller} / {Aktion} / {Id}" die Hilfsmethode Html.ActionLink() generiert die folgende Ausgabe:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Für unsere Index.aspx Ansicht müssen wir gehen Html.ActionLink() Helper-Methode vor, und jede Dinner in der Liste Link an die entsprechenden Details-URL:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Und wenn wir jetzt erreicht die */Dinners* unserer Liste Dinner wie im folgenden sieht URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Wenn Sie keines der Abendessen in der Liste klicken müssen wir navigieren, um Details dazu finden Sie unter:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Konventionsbasierte benennen und die \Views-Verzeichnisstruktur

ASP.NET MVC-Anwendungen standardmäßig verwenden eine konventionsbasierte Directory Struktur Benennung, beim Auflösen von Vorlagen anzeigen. Dadurch können Entwickler zu vermeiden, dass ein Speicherortpfad vollqualifiziert Wenn Ansichten von innerhalb einer Controllerklasse verweisen. ASP.NET MVC sieht in der Standardeinstellung für die Sicht Vorlagedatei innerhalb der * \Views\[ControllerName]\* Verzeichnis unterhalb der Anwendung.

Angenommen, wir gearbeitet haben in der DinnersController-Klasse – explizit drei Ansichtsvorlagen verweist auf: "Index", "Details" und "NotFound". ASP.NET MVC wird standardmäßig suchen Sie nach diesen Ansichten innerhalb der *\Views\Dinners* Verzeichnis unterhalb unsere Stammverzeichnis der Anwendung:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Beachten Sie, über wie es derzeit drei Controllerklassen innerhalb des Projekts werden (DinnersController, HomeController und AccountController – die letzten zwei standardmäßig hinzugefügt wurden, wenn wir das Projekt erstellt haben), und es gibt drei Unterverzeichnisse (einer für jede Controller) innerhalb des Verzeichnisses \Views.

Sichten verwiesen wird, von der Startseite "und" Konten-Controllern aufgelöst werden automatisch ihre Ansichtsvorlagen aus der entsprechenden *\Views\Home* und *\Views\Account* Verzeichnisse. Die *\Views\Shared* Unterverzeichnisses bietet eine Möglichkeit zum Speichern von Vorlagen anzeigen, die von mehreren Controllern in der Anwendung erneut verwendet werden. ASP.NET MVC Versuch, eine Vorlage anzeigen aufzulösen, er prüft zunächst innerhalb der *\Views\[Controller]* bestimmtes Verzeichnis und die anzeigen-Vorlage gefunden Es sieht sie innerhalb der *\Views\ Freigegebene* Verzeichnis.

Wenn es sich um einzelne Ansichtsvorlagen naming geht, ist die empfohlene Vorgehensweise der Vorlage anzeigen, die den gleichen Namen wie die Aktionsmethode freigeben, die zum Rendern geführt hat. Z. B. über unsere "Index" Aktionsmethode wird mithilfe der "Index" Ansicht das Ansichtsergebnis gerendert, und die Aktionsmethode "Details" ist die Ansicht "Details" verwenden, um die Ergebnisse zu rendern. Dies erleichtert es schnell zu erkennen, welche Vorlage für jede Aktion zugeordnet ist.

Entwickler müssen nicht den Namen der Sicht explizit angeben, wenn die Vorlage anzeigen, den gleichen Namen wie die Aktionsmethode aufgerufen wird, auf dem Controller hat. Wir können stattdessen übergeben Sie einfach das Modellobjekt an die Hilfsmethode "View()" (ohne Angabe des Ansichtsnamens) und ASP.NET MVC wird automatisch ableiten, dass wir verwenden möchten die *\Views\[ControllerName]\[ActionName]* Vorlage anzeigen, auf dem Datenträger, diesen zu rendert.

Dadurch können wir unsere controllercode etwas bereinigen und zu vermeiden, dupliziert den Namen zweimal in der getesteten Codes:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Der obige Code ist, dass alle, die zum Implementieren von nice Dinner auflisten/Details benötigt für den Standort auftreten.

#### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt eine schöne Dinner durchsuchen die Umgebung für die Zusammenarbeit.

Jetzt aktivieren wir CRUD (Create, Read, Update, Delete) Datenformulars Unterstützung bearbeiten.

> [!div class="step-by-step"]
> [Zurück](build-a-model-with-business-rule-validations.md)
> [Weiter](provide-crud-create-read-update-delete-data-form-entry-support.md)
