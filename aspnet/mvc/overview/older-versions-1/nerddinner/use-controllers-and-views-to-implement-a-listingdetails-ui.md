---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Verwenden von Controllern und Ansichten zum Implementieren einer Auflistungs-/Detailbenutzeroberfläche | Microsoft-Dokumentation
author: microsoft
description: Schritt 4 veranschaulicht das Hinzufügen ein Controllers für die Anwendung, die nutzt unseres Modells für ein Data listungsdetails/Navigationserlebnis Benutzer bereit...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 7a0a057efb52a869a72722b24d7283cb883db858
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838584"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Verwenden Sie zum Implementieren einer Auflistungs-/Detailbenutzeroberfläche Controller und Ansichten
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 4, der ein kostenloses ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 4 veranschaulicht das Hinzufügen ein Controllers an die Anwendung, die nutzt unseres Modells für ein Data listungsdetails/Navigationserlebnis für Dinner auf unserer Website NerdDinner Benutzer bereit.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner, Schritt 4: Controller und Ansichten

Mit herkömmlichen Web-Frameworks (klassisches ASP, PHP, ASP.NET Web Forms usw.) werden eingehende URLs in der Regel Dateien auf dem Datenträger zugeordnet. Zum Beispiel: eine Anforderung für eine URL wie "/ Products.aspx" oder "/" Products.PHP "" kann von einer "Products.aspx" oder ""Products.PHP ""-Datei verarbeitet werden.

Web-basierten MVC-Frameworks Servercode URLs in eine etwas andere Weise zugeordnet. Anstatt das Zuordnen von eingehenden URLs zu Dateien, sie stattdessen URLs auf Methoden in Klassen zugeordnet. Diese Klassen werden als "Controller" bezeichnet und sie sind zuständig für die Verarbeitung der eingehender HTTP-Anforderungen, Behandeln von Benutzereingaben, abrufen und Speichern von Daten und bestimmen die zu sendende Antwort zurück an den Client (HTML-Seite anzeigen, Herunterladen einer Datei, auf einen anderen umleiten URL usw.).

Nun, dass wir Sie ein einfaches Modell für die NerdDinner-Anwendung erstellt haben, werden im nächsten Schritt zum Hinzufügen eines Controllers für die Anwendung, die nutzt es, Benutzern eine Daten listungsdetails/navigationserfahrung zum Dinner auf unserer Website bereitzustellen.

### <a name="adding-a-dinnerscontroller-controller"></a>Hinzufügen eines Controllers "dinnerscontroller"

Wir beginnen, indem Sie mit der rechten Maustaste auf den Ordner "Controllers" in unsere Webprojekt, und wählen Sie dann die **Add-&gt;Controller** Menübefehl (Sie können auch Ausführen dieses Befehls durch Eingabe von STRG + M, STRG + C):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Hierdurch wird das Dialogfeld "Controller hinzufügen" angezeigt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Wir nennen Sie den neuen Controller "DinnersController" und klicken Sie auf die Schaltfläche "Hinzufügen". Visual Studio wird eine Datei DinnersController.cs unsere \Controllers-Verzeichnis hinzugefügt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Es wird auch die neue "dinnerscontroller"-Klasse im Code-Editor geöffnet.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>Hinzufügen von Index() und Details() Aktionsmethoden anwenden, um die DinnersController-Klasse

Wir möchten Besucher mithilfe unserer Anwendung, eine Liste von bevorstehenden Dinner durchsuchen, und ermöglichen sie auf alle Dinner in der Liste, um bestimmte Details dazu finden Sie unter aktivieren. Dazu müssen wir die folgenden URLs aus unserer Anwendung zu veröffentlichen:

| **URL** | **Zweck** |
| --- | --- |
| */Dinners/* | Eine HTML-Liste mit anstehenden Dinner anzeigen |
| *"/ Dinners" / Details / [Id]* | Anzeigen von Details zu einem bestimmten Dinner durch ein "Id"-Parameter, die in der URL, die die DinnerID von der Dinner in der Datenbank übereinstimmen, werden eingebettete angegeben. Zum Beispiel: /Dinners/Details/2 würde eine HTML-Seite mit den Details der Dinner DinnerID, dessen Wert 2 angezeigt. |

Durch das Hinzufügen von zwei öffentlichen "Aktionsmethoden" unserer Klasse "dinnerscontroller" wie unten veröffentlichen wir anfängliche Implementierungen der folgenden URLs:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Wir klicken Sie dann die NerdDinner-Anwendung ausführen und verwenden unsere Browser, um sie aufzurufen. Eingeben der *"/ Dinners /"* URL bewirkt, dass unsere *Index()* Methode ausführen, und es wird die folgende Antwort zurücksenden:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Geben der *"/ Dinners/Details/2"* URL bewirkt, dass unsere *Details()* Methode zum Ausführen und senden Sie die folgende Antwort zurück:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Vielleicht – wie ASP.NET MVC wusste, unsere Klasse "dinnerscontroller" zu erstellen und diese Methoden aufgerufen? Zu verstehen, dass wir einen kurzen Blick auf die Funktionsweise des Routing nutzen.

### <a name="understanding-aspnet-mvc-routing"></a>Grundlegendes zu ASP.NET MVC-Routings

ASP.NET MVC umfasst eine leistungsfähige Engine zum Weiterleiten der URL, die ein hohes Maß an Flexibilität bei der Steuerung der Zuordnung von URLs zu Controllerklassen bereitstellt. Es können vollständig benutzerdefiniert anpassen, wie ASP.NET MVC wählt die Controllerklasse zu erstellen, welche Methode Sie aufrufen, die darauf als auch so konfigurieren Sie verschiedene Möglichkeiten, Variablen automatisch aus der URL-Abfragezeichenfolge analysiert und an die Methode als Parameter übergeben Argumente. Es bietet die Flexibilität zum vollständig Optimieren eines Standorts für die Suchmaschinenoptimierung (Search Engine Optimization) sowie alle URL-Struktur, die von einer Anwendung sollten veröffentlichen.

Neue ASP.NET MVC-Projekte enthalten standardmäßig eine vorkonfigurierte Sammlung von URL-Routingregeln, die bereits registriert. Dadurch kann wir ganz einfach mit einer Anwendung beginnen, ohne explizit etwas konfigurieren zu müssen. Die Standard-routing-Regel-Registrierungen können innerhalb der Klasse "Application" unserer Projekte – gefunden werden, die wir durch Doppelklicken auf die Datei "Global.asax" im Stammverzeichnis des unser Projekt öffnen können:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Die standardmäßige ASP.NET-MVC-Routingregeln werden innerhalb der Methode "RegisterRoutes" dieser Klasse registriert:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

Die "Routen. MapRoute() "Methodenaufruf, der oben genannten registriert die Standardroutingregel, der eingehende URLs Controllerklassen mit URL-Format zugeordnet:" / {Controller} / {Action} / {Id} "– wobei"Controller"der Name der Controllerklasse instanziiert wird, ist"Action"ist der Name einer öffentliche Methode hinzu, und "Id" aufgerufen werden soll, ist ein optionaler Parameter, eingebettet in der URL, die als Argument an die Methode übergeben werden kann. Der dritte Parameter übergeben wird, auf den Aufruf der Methode "MapRoute()" ist ein Satz von Standardwerten für die Controller/Aktion/Id-Werte verwendet werden soll, die sie nicht in der URL vorhanden sind (Controller = "Home" Action = "Index", Id = "").

Im folgenden finden Sie eine Tabelle, die veranschaulicht, wie eine Vielzahl von URLs zugeordnet sind, über das standardmäßige "<em>/ {Controller} / {Action} / {Id}"</em>Weiterleitungsregel:

| **URL** | **Controller-Klasse** | **Action-Methode** | **Übergebene Parameter** |
| --- | --- | --- | --- |
| */ Dinners/Details/2* | "Dinnerscontroller" | Details(ID) | id=2 |
| */Dinners/Edit/5* | "Dinnerscontroller" | Edit(ID) | ID = 5 |
| */Dinners/Create* | "Dinnerscontroller" | Create() | Nicht zutreffend |
| */ Dinners* | "Dinnerscontroller" | Index()-Klausel | Nicht zutreffend |
| */Home* | HomeController | Index()-Klausel | Nicht zutreffend |
| */* | HomeController | Index()-Klausel | Nicht zutreffend |

Die letzten drei Zeilen angezeigt, die Standardwerte (Controller = Home, Aktion = der Index, Id = "") verwendet wird. Da die Methode "Index" als den Standardnamen für die Aktion registriert ist, wenn eine nicht angegeben ist, die "/ Dinners" und "/ Home" URLs Grund der Aktionsmethode Index() für ihre Controllerklassen aufgerufen werden soll. Da der Controller "Home" als den Standardcontroller registriert ist, wenn eine nicht angegeben ist, wird die URL "/" HomeController erstellt werden soll, und die Aktionsmethode Index() darauf aufgerufen werden.

Wenn Ihnen nicht, dass diese Standard-URL-Routingregeln gefällt, ist die gute Nachricht, dass sie einfach ändern – bearbeiten Sie sie einfach in der RegisterRoutes-Methode, die oben genannten sind. Für die NerdDinner-Anwendung, allerdings wird hier nicht die Standard-URL-Routingregeln nicht ändern – stattdessen verwenden wir sie als-ist.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Verwenden die "dinnerrepository" von unserer "dinnerscontroller"

Ersetzen Sie die aktuelle Implementierung von der "dinnerscontroller" lassen Sie uns jetzt Index() und Details() Aktionsmethoden mit Implementierungen, die unser Modell verwenden.

Wir verwenden die "dinnerrepository"-Klasse, die wir zuvor erstellt, um das Verhalten zu implementieren. Wir beginnen, die durch das Hinzufügen einer "using"-Anweisung, die den Namespace "NerdDinner.Models" verweist, und klicken Sie dann eine Instanz von unserem "dinnerrepository" als Feld DinnerController Klasse deklarieren.

Weiter unten in diesem Kapitel wir führen das Konzept der "Dependency Injection" und zeigen eine andere Möglichkeit, einen Verweis auf eine "dinnerrepository" zu erhalten, die eine bessere-Einheit können unsere Controller testen – aber für die rechts-jetzt erstellen wir nur eine Instanz von unserem "dinnerrepository" Inline wie unten.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Jetzt können wir unsere Modellobjekte abgerufenen Daten mit HTML-Antwort generiert.

### <a name="using-views-with-our-controller"></a>Verwenden von Sichten mit Controller

Es ist zwar möglich, zum Schreiben von Code in unserer Aktionsmethoden assemblieren von HTML und verwenden Sie dann die *Response.Write()* Hilfsmethode zurück an den Client senden, dass Ansatz schnell Recht schwerfällig wird. Ein viel besserer Ansatz ist für uns, nur-Anwendung und Datenlogik in unser Aktionsmethoden "dinnerscontroller" auszuführen, und übergeben Sie dann die Daten benötigt, um eine HTML-Antwort in eine separate "View"-Vorlage zu rendern, die für die Ausgabe der HTML-Darstellung zuständig ist davon. Wie wir gleich sehen werden, ist eine Vorlage "anzeigen" eine Textdatei, die in der Regel eine Kombination aus HTML-Markup und Code zum Rendern der eingebetteten enthält.

Trennen unsere Controllerlogik aus unserer Ansicht rendern, bringt mehrere große Vorteile. Insbesondere ist es hilfreich, eine klare "Trennung von Belangen" zwischen der Anwendung und Benutzeroberfläche Formatierung/Rendering-Code zu erzwingen. Dies erleichtert es auf die Anwendungslogik für Komponententests isoliert von UI-Rendering-Logik. Es erleichtert später die UI-Rendering-Vorlagen ändern, ohne Änderungen am Anwendungscode vornehmen zu müssen. Und es kann einfacher für Entwickler und Designer gemeinsam an Projekten zusammenarbeiten.

Wir können unsere Klasse "dinnerscontroller", um anzugeben, dass wir eine ansichtsvorlage zu verwenden, um eine HTML-UI-Antwort zurücksenden, durch Ändern der Signaturen von unseren zwei Aktionsmethoden müssen Sie den Rückgabetyp "void", um stattdessen einen Rückgabetyp von "ActionResult" erhalten möchten, aktualisieren. Wir können dann aufrufen, die *View()* Hilfsmethode für die Basisklasse für Controller wieder zurückgegeben. ein "ViewResult"-Objekt wie die folgende:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

Die Signatur der *View()* Hilfsmethode wir oben verwenden sieht wie folgt aus:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

Der erste Parameter für die *View()* Hilfsmethode ist der Name des der ansichtsvorlagendatei zum Rendern der HTML-Antwort verwendet werden soll. Der zweite Parameter ist ein Modellobjekt, das Daten, die die ansichtsvorlage benötigt enthält, um die HTML-Antwort zu rendern.

Innerhalb unserer Index() Aktionsmethode sind wir rufen die *View()* Hilfsmethode und gibt an, dass eine HTML-Liste der Dinner mithilfe einer Vorlage der "Index" Ansicht gerendert werden soll. Der ansichtsvorlage werden eine Sequenz von Dinner-Objekten, um die Liste von generieren übergeben:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

In unserem Details() Action-Methode versucht, ein Dinner-Objekt, das mithilfe der in der URL angegebenen Id abzurufen. Wenn eine gültige Dinner gefunden wird, rufen wir die *View()* Hilfsprogramm-Methode, der angibt, eine ansichtsvorlage "Details" zum Rendern des abgerufenen Dinner-Objekts verwendet werden soll. Wenn ein ungültiger Dinner angefordert wird, Rendern wir eine nützliche Fehlermeldung, der angibt, dass die Dinner nicht vorhanden, verwenden eine ansichtsvorlage "NotFound" (und eine überladene Version der dem *View()* Hilfsmethode, die nur den Vorlagennamen verwendet ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Nun implementieren wir die Ansichtsvorlagen "NotFound", "Details" und "Index".

### <a name="implementing-the-notfound-view-template"></a>Implementieren die Vorlage "NotFound" anzeigen

Wir beginnen, indem Sie die Implementierung der "NotFound" Ansicht-Vorlage angezeigt wird, eine benutzerfreundliche Fehlermeldung, dass die angeforderte Abendessen nicht gefunden werden kann.

Wir erstellen eine neue ansichtsvorlage für die über die Positionierung der Textcursor in eine Aktionsmethode des Controllers, und klicken Sie dann mit der rechten Maustaste und wählen Sie den Befehl im Menü "Ansicht hinzufügen" (es können auch Ausführen dieses Befehls durch Eingabe von STRG + M, STRG + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Hierdurch wird ein Dialogfeld "Ansicht hinzufügen" wie unten angezeigt. In der Standardeinstellung im Dialogfeld den Namen des zu erstellenden Ansicht vorab auffüllen entsprechend den Namen der Aktionsmethode wurde des Cursors beim das Dialogfeld "(in diesem Fall"Details") gestartet wurde. Da wir zunächst die Vorlage "NotFound" implementieren möchten, wir außer Kraft setzen dieser Name anzeigen und festlegen, dass stattdessen "NotFound" sein:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Wenn wir auf die Schaltfläche "Hinzufügen" klicken, wird Visual Studio "NotFound.aspx" Ansicht für uns innerhalb des Verzeichnisses "\Views\Dinners" erstellen Sie eine Vorlage (die ebenfalls erstellt wird, falls das Verzeichnis nicht bereits vorhanden ist):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Es wird auch unsere neue "NotFound.aspx" ansichtsvorlage im Code-Editor geöffnet:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Anzeigen von Vorlagen in der Standardeinstellung haben zwei "Inhaltsbereiche", in dem wir die Inhalte und Code hinzufügen können. Die erste ermöglicht uns, die "Title" der HTML-Seite zurückgesendet anpassen. Die zweite ermöglicht uns, Anpassen die "main"der HTML-Seite gesendete Inhalt zurück.

Zum Implementieren der unsere "NotFound" ansichtsvorlage fügen wir einige grundlegende Inhalte:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Wir können dann innerhalb des Browsers ausprobieren. Lassen Sie uns dazu Anforderung die *"/ Dinners/Details/9999"* URL. Dies wird zu einem Dinner beziehen, die nicht derzeit in der Datenbank vorhanden, und führt dazu, dass unsere DinnersController.Details() Aktionsmethode zum Rendern von unserem "NotFound"-Vorlage anzeigen:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Eine Sache, die Sie im Screenshot oben sehen ist, dass unsere einfache ansichtsvorlage eine Reihe von HTML geerbt hat, der den Hauptinhalt auf dem Bildschirm umgibt. Dies ist, da unsere ansichtsvorlage eine Vorlage "Masterseite" verwendet, die ein konsistentes Layout für alle Sichten auf der Website anwenden können. Wird besprochen, wie in einem späteren Teil dieses Tutorials Weitere von Masterseiten.

### <a name="implementing-the-details-view-template"></a>Implementieren die Vorlage "Details" anzeigen

Nun implementieren wir die ansichtsvorlage "Details" – die HTML für ein einzelnes Dinner-Modell generieren wird.

Wir werden Positionierung der Textcursor innerhalb der Aktionsmethode Details dazu und klicken Sie dann mit der rechten Maustaste und wählen Sie den Befehl im Menü "Ansicht hinzufügen" (oder drücken Sie STRG + M, STRG + V):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Hierdurch wird das Dialogfeld "Ansicht hinzufügen" angezeigt. Wir halten der standardansichtsname ("Details"). Wir außerdem wählen Sie im Dialogfeld das Kontrollkästchen "Stark typisierte Ansicht erstellen" und wählen (mithilfe der Dropdownliste "Combobox") der Name des Modelltyps, die vom Controller an die Ansicht übergeben werden. Für diese Ansicht ein Dinner-Objekt übergeben werden (der vollständig qualifizierte Name für diesen Typ ist: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Im Gegensatz zu der vorherigen Vorlage, in dem wir eine "leere"Ansicht erstellen möchten, dieses Mal auf, die wir automatisch ausgewählt wird "Erstellen des Gerüsts für" der Ansicht, die mithilfe einer Vorlage "Details". Wir können dies darauf hinweisen, durch Ändern der Dropdownliste die "Inhalt anzeigen" im Dialogfeld "oben".

"Gerüstbau für" generieren eine anfängliche Implementierung unserer Details anzeigen-Vorlage basierend auf der Dinner-Objekt, das wir an sie übergeben werden. Dadurch wird eine einfache Möglichkeit, uns auf unsere Vorlage ansichtsimplementierung schnellen Einstieg.

Wenn wir auf die Schaltfläche "Hinzufügen" klicken, wird Visual Studio eine neue "Details.aspx" ansichtsvorlagendatei für uns in unserer "\Views\Dinners"-Verzeichnis erstellt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Es wird auch unsere neue Ansicht "Details.aspx" Diese Vorlage im Code-Editor geöffnet. Es wird eine anfängliche Gerüst-Implementierung von einer Detailansicht auf Grundlage eines Modells Dinner enthalten. Die Gerüstbau-Engine .NET Reflektion betrachten Sie die öffentlichen Eigenschaften verfügbar gemacht werden, in der Klasse übergeben, und fügen geeigneten Inhalte basierend auf den einzelnen gefundenen:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Wir können anfordern, die *"/ Dinners/Details/1"* URL, um festzustellen, wie diese "details" Scaffold-Implementierung im Browser aussieht. Verwenden diese URL wird eine von der Dinner angezeigt, die wir manuell mit unserer Datenbank hinzugefügt, wenn wir zuerst erstellt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Dies bringt uns schnelle Betriebsbereitschaft gewährleisten können, und mit einer anfänglichen Implementierung unserer Ansicht "Details.aspx" bietet. Wir können dann und zum Anpassen der Benutzeroberfläche zu unserer Zufriedenheit optimieren.

Wenn wir die Vorlage Details.aspx genauer betrachten, werden wir feststellen, dass enthält statische HTML-Code sowie Code zum Rendern der eingebettet werden sollen. &lt;%%&gt; codenuggets Code ausführen, wenn die ansichtsvorlage gerendert wird, und &lt;% = %&gt; codenuggets den darin enthaltenen Code ausführen, und klicken Sie dann das Ergebnis in den Ausgabestream, der die Vorlage zu rendern.

Wir können Code in unserer Ansicht schreiben, die das Modellobjekt "Dinner" zugreift, das von den Controller, die mit einer stark typisierten "Model"-Eigenschaft übergeben wurde. Visual Studio bietet uns mit vollständiger Code: Intellisense, beim Zugriff auf diese "Modell" Eigenschaft im Editor:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Lassen Sie uns ein paar Optimierungen vorzunehmen, so, dass die Quelle für unsere Vorlage für endgültige Details anzeigen sieht:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Wenn wir Zugriff auf die *"/ Dinners/Details/1"* URL erneut wird nun rendern wie die folgende:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Implementieren die Vorlage "Index" anzeigen

Nun implementieren wir die ansichtsvorlage "Index": die eine Liste der bevorstehenden Dinner generiert wird. To-do dies wir unsere Textcursor innerhalb der Aktionsmethode für den Index der Position an, und dann rechts klicken Sie auf und wählen Sie den Befehl im Menü "Ansicht hinzufügen" (oder drücken Sie STRG + M, STRG + V).

Das Dialogfeld "Ansicht hinzufügen" in "Wir halten die ansichtsvorlage namens"Index"und aktivieren Sie das Kontrollkästchen"Eine stark typisierte Ansicht erstellen". Wir wählen automatisch eine ansichtsvorlage "List" zu generieren, und wählen "NerdDinner.Models.Dinner" als den Typ des Modells an die Ansicht übergeben diesmal (die da wir angegeben haben, erstellen wir eine "Liste" Scaffold dazu, das Dialogfeld "Ansicht hinzufügen dass führt", wird davon ausgegangen werden. Übergeben einer Sequenz von Dinner-Objekten aus dem Controller an die Ansicht):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Wenn wir auf die Schaltfläche "Hinzufügen" klicken, wird Visual Studio eine neue "Index.aspx" ansichtsvorlagendatei für uns in unserer "\Views\Dinners" Verzeichnis erstellt. Es wird "eine anfängliche Implementierung darin, die ein HTML-Tabelle aus der Dinner bereitstellt, die wir für die Ansicht übergeben Erstellen des Gerüsts für".

Ausführen der Anwendung und den Zugriff der *"/ Dinners /"* URL wird die Liste der Dinner ausgegeben wie folgt:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

Die oben aufgeführten Tabelle-Lösung erhalten Sie ein Raster-ähnliches Layout Dinner werden – dies ist nicht sehr wir für unsere kundenorientierte Dinner auflisten möchten. Wir können die ansichtsvorlage Index.aspx aktualisieren und ändern Sie sie zum Auflisten von weniger Spalten mit Daten und Verwenden von einem &lt;Ul&gt; Element sie statt einer Tabelle mit den folgenden Code gerendert werden soll:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Wir verwenden das Schlüsselwort "Var" in der oben genannten Foreach-Anweisung, wie wir jede Dinner in unserem Modell Schleife zu durchlaufen. Die nicht mit c# 3.0 vertraut denken möglicherweise, dass mit "Var" bedeutet, dass die Dinner-Objekt spät gebunden ist. Es wird stattdessen bedeutet, dass der Compiler Typrückschluss für die stark typisierte "Modell"-Eigenschaft verwendet (vom Typ "IEnumerable&lt;Dinner&gt;") und Kompilieren die lokalen "Dinner"-Variable als Dinner-Typ – d.h. wir erhalten vollständige IntelliSense und zur Kompilierzeit für die sie innerhalb der Codeblöcke:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Bei Treffer Aktualisierung auf die *"/ dinners"* unserem Browser unsere aktualisierte Ansicht sieht wie die folgende URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Dies ist besser – suchen Sie aber noch nicht vollständig vorhanden. Im letzten Schritt werden Endbenutzer, klicken Sie auf einzelne Dinner in der Liste aus, und erfahren Sie mehr über diese zu aktivieren. Wir werden dies implementieren, mit dem Rendern HTML-Link-Elemente, die an die Aktionsmethode "Details" auf unserer "dinnerscontroller" zu verknüpfen.

Wir können diese Links in unserer Ansicht "Index" auf zwei Arten generieren. Die erste besteht darin, manuell erstellen, HTML &lt;eine&gt; Elemente wie die folgende, wobei betten wir ein &lt;%%&gt; blockiert, die innerhalb der &lt;eine&gt; HTML-Element:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Wir können alternativansatz ist die integrierte "Html.ActionLink()"-Hilfsmethode in ASP.NET MVC nutzen, die unterstützt Programmgesteuertes Erstellen von HTML &lt;eine&gt; -Element, das links zu einer anderen Aktionsmethode auf eine Controller:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Der erste Parameter an die Hilfsmethode Html.ActionLink() ist der Link-anzuzeigende Text (in diesem Fall den Titel von der Dinner), wird des zweiten Parameters den Namen der Controlleraktion den Link, um (die Details-Methode in diesem Fall) erstellt werden sollen, und der dritte Parameter ist ein Satz von Parametern zum Senden an die Aktion (implementiert als ein anonymer Typ mit Eigenschaftennamen und Werten). In diesem Fall geben wir an den Parameter "Id" von der Dinner wir eine Verknüpfung herstellen möchten, und da die Standard-URL-routing in ASP.NET MVC-Regel "{Controller} / {Action} / {Id}" die Hilfsmethode Html.ActionLink() generiert die folgende Ausgabe:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Unsere Ansicht Index.aspx wir gehen Sie vor Html.ActionLink() Helper-Methode und jedes Dinner haben, in der Liste Link zur entsprechenden Details-URL:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Und wir nun bei Erreichen der *"/ dinners"* URL, die die Dinner-Liste sieht wie folgt aus:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Wenn Sie keines der Dinner in der Liste klicken gelangen wir um Details dazu finden Sie unter:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Konventionen basierender Benennung und der \Views-Verzeichnisstruktur

ASP.NET MVC-Anwendungen in der Standardeinstellung verwenden ein auf Konventionen basierendes Verzeichnis Benennungsstruktur beim Auflösen von Vorlagen anzeigen. Dadurch können Entwickler zu vermeiden, dass ein Location-Path vollqualifiziert werden Ansichten von innerhalb einer Controllerklasse verweisen auf. Standardmäßig sieht sich ASP.NET MVC für die Vorlagendatei "Ansicht" in der * \Views\[Namedescontrollers]\* Verzeichnis unterhalb der Anwendung.

Angenommen, wir gearbeitet haben die DinnersController-Klasse –, die explizit drei Ansichtsvorlagen verweist: "Index", "Details" und "NotFound". ASP.NET MVC in der Standardeinstellung sucht diese Ansichten innerhalb der *\Views\Dinners* Verzeichnis unterhalb unserer Stammverzeichnis der Anwendung:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Beachten Sie, über wie es derzeit drei Controllerklassen innerhalb des Projekts ("dinnerscontroller" "," HomeController "und" AccountController – die letzten zwei standardmäßig hinzugefügt wurden, wenn wir das Projekt erstellt haben), und es gibt drei Unterverzeichnisse (einer für jede Controller) innerhalb des Verzeichnisses \Views.

Ansichten, die auf die verwiesen wird durch den Controller, Home und Konten aufgelöst werden automatisch ihre Vorlagen anzeigen, aus der jeweiligen *\Views\Home* und *\Views\Account* Verzeichnisse. Die *\Views\Shared* Unterverzeichnis bietet eine Möglichkeit zum Anzeigen von Vorlagen zu speichern, die auf mehrere Controller innerhalb der Anwendung erneut verwendet werden. Wenn ASP.NET MVC versucht, eine ansichtsvorlage zu aufzulösen, wird zunächst überprüft innerhalb der *\Views\[Controller]* bestimmtes Verzeichnis, und wenn sie nicht die ansichtsvorlage finden es sieht in der *\Views\ Freigegebene* Verzeichnis.

Wenn es darum geht, die einzelne Ansichtsvorlagen benennen, ist die empfohlene Vorgehensweise die ansichtsvorlage den gleichen Namen wie die Aktionsmethode zu verwenden, die sie zum Rendern verursacht hat. Z. B. über unsere "Index" Action-Methode ist die Ansicht "Index" zum Rendern verwenden, das Ansichtsergebnis, und die Aktionsmethode "Details" verwendet die Ansicht "Details", um die Ergebnisse zu rendern. Dies erleichtert Ihnen schnell anzeigen, welche Vorlage jede Aktion zugeordnet ist.

Entwickler müssen nicht explizit Namen der Sicht angeben, wenn die ansichtsvorlage den gleichen Namen wie die Aktionsmethode aufgerufen wird, auf dem Controller hat. Wir können stattdessen übergeben Sie einfach das Modellobjekt an die Hilfsmethode "View()" (ohne Angabe der Sichtname) und ASP.NET MVC wird automatisch abzuleiten, die verwendet werden soll die *\Views\[Namedescontrollers]\[ActionName]* Vorlage anzeigen, auf dem Datenträger zu rendern.

Dadurch können wir unser controllercode ein wenig bereinigen und den Namen zweimal in unserem Code duplizieren zu vermeiden:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Der obige Code ist, dass alles, die was erforderlich ist, um eine schöne Dinner/listungsdetails implementieren für den Standort auftreten.

#### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt eine schöne Dinner browsererlebnisses erstellt.

Jetzt aktivieren wir Support-formularbearbeitung, die Daten der CRUD (Create, Read, Update, Delete).

> [!div class="step-by-step"]
> [Zurück](build-a-model-with-business-rule-validations.md)
> [Weiter](provide-crud-create-read-update-delete-data-form-entry-support.md)
