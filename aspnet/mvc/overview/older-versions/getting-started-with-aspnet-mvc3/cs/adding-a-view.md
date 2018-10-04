---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Hinzufügen einer Ansicht (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 9fc8c6cad44016511c462b4206c28ea3449ff97e
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576611"
---
<a name="adding-a-view-c"></a>Hinzufügen einer Ansicht (c#)
====================
durch [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.
> 
> 
> In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist verfügbar, zur Ergänzung dieses Themas. [Laden Sie die C#-Version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) in diesem Tutorial.


In diesem Abschnitt also ändern Sie die `HelloWorldController` anzeigen, die Vorlagendateien an sauber zu kapseln den Prozess der Generierung von HTML-Antworten an einen Client zu verwendende Klasse an.

Sie erstellen eine ansichtsvorlagendatei mithilfe der neuen [Razor-ansichtsengine](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) mit ASP.NET MVC 3 eingeführt wurde. Razor-basierte Ansichtsvorlagen haben eine *.cshtml* Dateierweiterung aus, und geben Sie eine elegante Methode zum Erstellen von HTML-Ausgabe mithilfe von c#. Razor minimiert die Anzahl von Zeichen und Tastaturanschläge erforderlich, wenn eine ansichtsvorlage zu schreiben und ermöglicht eine schnelle, fließende Workflows programmieren.

Starten Sie mithilfe einer Vorlage anzeigen, mit der `Index` -Methode in der die `HelloWorldController` Klasse. Derzeit gibt die `Index`-Methode eine Zeichenfolge mit der Meldung zurück, die in der Controllerklasse hartcodiert ist. Ändern der `Index` -Methode zur Rückgabe einer `View` -Objekts, wie im folgenden gezeigt:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Dieser Code verwendet eine ansichtsvorlage zum Generieren einer HTML-Antwortinhalts an den Browser. Fügen Sie in das Projekt eine ansichtsvorlage, mit denen Sie mit der `Index` Methode. Klicken Sie dazu auf, auf die `Index` -Methode, und klicken Sie auf **Ansicht hinzufügen**.

![](adding-a-view/_static/image1.png)

Die **Ansicht hinzufügen** Dialogfeld wird angezeigt. Behalten Sie die Möglichkeit, sie sind, und klicken Sie auf, die Standardwerte der **hinzufügen** Schaltfläche:

![](adding-a-view/_static/image2.png)

Die *MvcMovie\Views\HelloWorld* Ordner und die *MvcMovie\Views\HelloWorld\Index.cshtml* -Datei erstellt werden. Sie finden diese in **Projektmappen-Explorer**:

![](adding-a-view/_static/image3.png)

Im folgenden dargestellt die *"Index.cshtml"* -Datei, die erstellt wurde:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Hinzufügen von HTML-Code unter der `<h2>` Tag. Die geänderte *MvcMovie\Views\HelloWorld\Index.cshtml* Datei ist unten dargestellt.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zu der `HelloWorld` Controller (`http://localhost:xxxx/HelloWorld`). Die `Index` -Methode in Ihrer Controller nicht viel Arbeit ausführen; es einfach ausgeführt wurde, die Anweisung `return View()`, die angab, dass die Methode eine ansichtsvorlagendatei verwenden soll, um eine Antwort an den Browser auszugeben. Da Sie den Namen der ansichtsvorlagendatei mit nicht explizit angegeben haben, ASP.NET MVC standardmäßig die *"Index.cshtml"* -Datei in die *\Views\HelloWorld* Ordner. Die folgende Abbildung zeigt die Zeichenfolge in der Ansicht hartcodiert ist.

![](adding-a-view/_static/image6.png)

Das sieht recht gut. Beachten Sie jedoch, dass im Browser auf die Titelleiste, "Index", sagt aus, und der große Titel auf der Seite "Meine MVC-Anwendung.", sagt Diese ändern.

## <a name="changing-views-and-layout-pages"></a>Ändern von Ansichten und Layoutseiten

Erstens, Sie möchten den Titel "Meine MVC-Anwendung" am oberen Rand der Seite zu ändern. Dieser Text ist üblich, jede Seite. Es ist tatsächlich in nur an einer Stelle im Projekt implementiert, obwohl er auf jeder Seite in der Anwendung angezeigt wird. Wechseln Sie zu der */Views/Shared* Ordner **Projektmappen-Explorer** , und öffnen Sie die  *\_Layout.cshtml* Datei. Diese Datei heißt eine *Layoutseite* und die freigegebenen "Shell", die alle anderen Seiten zu verwenden ist.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Layoutvorlagen können Sie die HTML-Containerlayout Ihrer Website zentral anzugeben und wenden Sie es über mehrere Seiten auf Ihrer Website. Beachten Sie die `@RenderBody()` Zeile am unteren Rand der Datei. `RenderBody` ist ein Platzhalter, in dem alle ansichtsspezifischen Seiten, die Sie erstellen, "umschlossenen" Seite "Layout" angezeigt. Ändern Sie die Title-Überschrift in der Layoutvorlage aus "Meine MVC-Anwendung" in "MVC Movie App".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Führen Sie die Anwendung, und beachten Sie, dass er jetzt sagt "MVC Movie App". Klicken Sie auf die **zu** Link, und Sie sehen, wie die Seite mit "MVC Movie App", zeigt zu. Wir konnten für die Änderung einmal in der Layoutvorlage aus, und haben alle Seiten auf der Website entsprechend den neuen Titel.

![](adding-a-view/_static/image9.png)

Die vollständige  *\_Layout.cshtml* Datei ist unten dargestellt:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Jetzt ändern wir den Titel der Seite "Index" (Ansicht).

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Es gibt zwei Bereiche zu ändern: zuerst der Text, der angezeigt wird in der Titelleiste des Browsers, und klicken Sie dann in die sekundäre Kopfzeile (die `<h2>` Element). Sie nehmen diese geringfügig anders vor, damit Sie erkennen können, welcher Teil der App mithilfe einer Codebearbeitung geändert wird.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

An die HTML-Titel angezeigt werden, den Code oben legt ein `Title` Eigenschaft der `ViewBag` Objekt (dieser befindet sich in der *"Index.cshtml"* Vorlage anzeigen). Wenn Sie wieder den Quellcode der Layoutvorlage betrachten, werden Sie feststellen, dass die Vorlage wird, diesen Wert in verwendet der `<title>` -Element als Teil der `<head>` Teil der HTML-Code. Mit diesem Ansatz können Sie problemlos andere Parameter zwischen Ihrer Vorlage anzeigen und die Layoutdatei übergeben.

Führen Sie die Anwendung, und navigieren Sie zu `http://localhost:xx/HelloWorld`. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG+F5, um das Laden der Antwort vom Server zu erzwingen.)

Beachten Sie außerdem wie der Inhalt in die *"Index.cshtml"* ansichtsvorlage zusammengeführt wurde, mit der  *\_Layout.cshtml* Vorlage anzeigen und eine einzelne HTML-Antwort an den Browser gesendet wurde. Layoutvorlagen erleichtern ungemein das Vornehmen von Änderungen an allen Seiten in Ihrer Anwendung.

![](adding-a-view/_static/image10.png)

Unser kleiner Beitrag zu den „Daten“ (in diesem Fall die Meldung "Hello from our View Template!") ist jedoch hartcodiert. Die MVC-Anwendung weist ein „V“ (View [Ansicht]) auf, und Sie verfügen über ein „C“ (Controller), aber noch nicht über ein „M“ (Modell). Kurz darauf begleiten wir erläutert, wie eine Datenbank erstellen und Modellieren von Daten daraus abrufen.

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Bevor wir mit einer Datenbank und über Modelle sprechen, jedoch zunächst sprechen wir über Informationen vom Controller an eine Ansicht übergeben. Controller-Klassen werden als Reaktion auf eine eingehende URL-Anforderung aufgerufen. Eine Controllerklasse ist, in dem Sie den Code schreiben, der die eingehenden Parameter verarbeitet, ruft Daten aus einer Datenbank ab und letztlich entscheidet, welche Art der Antwort zurück an den Browser senden. Anzeigen von Vorlagen können dann verwendet werden von einem Controller zum Generieren und Formatieren einer HTML-Antwortinhalts an den Browser.

Controller sind verantwortlich für das Bereitstellen der beliebige Daten oder Objekte erforderlich sind, in der Reihenfolge für eine ansichtsvorlage eine Antwort an den Browser auszugeben. Eine ansichtsvorlage sollte nie Geschäftslogik ausführen bzw. direkt mit einer Datenbank interagieren. Stattdessen sollte es nur mit den Daten arbeiten, die sie durch den Controller bereitgestellt wird. Diese "Trennung von Belangen" verwalten kann, bleibt Ihr Code übersichtlich und besser verwaltbar.

Derzeit den `Welcome` Aktionsmethode in der `HelloWorldController` -Klasse eine `name` und `numTimes` Parameter und gibt anschließend die Werte direkt an den Browser. Anstatt den Controller diese Antwort als Zeichenfolge Rendern zu lassen, ändern wir den Controller, um stattdessen eine ansichtsvorlage zu verwenden. Die Ansichtsvorlage generiert eine dynamische Antwort. Das bedeutet, dass Sie die entsprechenden Datenelemente vom Controller an die Ansicht übergeben müssen, um die Antwort zu generieren. Hierzu können Sie den Controller die dynamischen Daten einfügen, die die ansichtsvorlage, in benötigt einem `ViewBag` -Objekt, das die ansichtsvorlage zugreifen kann.

Wechseln Sie zurück zur der *HelloWorldController.cs* und zu ändern der `Welcome` -Methode zum Hinzufügen einer `Message` und `NumTimes` Wert der `ViewBag` Objekt. `ViewBag` ist ein dynamisches Objekt, was bedeutet, dass die gewünschten, Sie platzieren können. die `ViewBag` Objekt verfügt über keine definierten Eigenschaften aus, bis Sie etwas darin einfügen. Die vollständige Datei *HelloWorldController.cs* sieht wie folgt aus:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Jetzt die `ViewBag` Objekt enthält Daten, die automatisch an die Ansicht übergeben werden.

Als Nächstes fügen Sie eine ansichtsvorlage! In der **Debuggen** , wählen Sie im Menü **erstellen MvcMovie** um sicherzustellen, dass das Projekt kompiliert wird.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Klicken Sie dann mit der rechten Maustaste innerhalb der `Welcome` -Methode, und klicken Sie auf **Ansicht hinzufügen**. Dabei handelt es sich die **Ansicht hinzufügen** das Dialogfeld sieht wie folgt aus:

![](adding-a-view/_static/image13.png)

Klicken Sie auf **hinzufügen**, und fügen Sie den folgenden Code unter der `<h2>` Element in der neuen *Welcome.cshtml* Datei. Erstellen Sie eine Schleife mit dem Text "Hello" so oft wie der Benutzer meldet, dass er es sollte. Die vollständige *Welcome.cshtml* Datei ist unten dargestellt.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zur folgenden URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Daten werden jetzt der URL entnommen und automatisch an den Controller übergeben. Der Controller Packt die Daten in einem `ViewBag` Objekt und übergibt das Objekt an die Ansicht. Die Ansicht zeigt dann die Daten im HTML-Format für den Benutzer.

![](adding-a-view/_static/image14.png)

Das war also eine Art eines „M“ für Modell, jedoch nicht der Art Datenbank. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.

> [!div class="step-by-step"]
> [Zurück](adding-a-controller.md)
> [Weiter](adding-a-model.md)
