---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: "Hinzufügen einer Ansicht (c#) | Microsoft Docs"
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 46d5494e668dfe156aeb6647ded83e6ce5366714
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view-c"></a>Hinzufügen einer Ansicht (c#)
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Lernprogramms steht [hier](../../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.
> 
> 
> In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist zu diesem Thema steht zur Verfügung. [Die C#-Version herunterladen](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) dieses Lernprogramm.


In diesem Abschnitt Sie so ändern Sie nun die `HelloWorldController` Klasse, die anzeigen, die Vorlagendateien sauberes kapseln den Prozess der Generierung der HTML-Antworten auf einem Client verwendet.

Erstellen Sie eine Datei mit der neuen Vorlage [Razor-Ansichtsmodul](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) mit ASP.NET MVC 3 eingeführt wurde. Razor-basierte Ansichtsvorlagen haben eine *cshtml* Dateierweiterung aus, und geben Sie eine elegante Methode zum Erstellen von HTML-Ausgabe mithilfe von c#. Razor minimiert die Anzahl von Zeichen und Tastatureingaben erforderlich, wenn eine Ansichtenvorlage schreiben und ermöglicht eine schnelle, Fluid Codierung Workflow.

Starten Sie mithilfe einer Vorlage anzeigen, mit der `Index` Methode in der `HelloWorldController` Klasse. Derzeit gibt die `Index`-Methode eine Zeichenfolge mit der Meldung zurück, die in der Controllerklasse hartcodiert ist. Ändern der `Index` -Methode zur Rückgabe einer `View` -Objekts, wie im folgenden gezeigt:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Dieser Code verwendet eine Vorlage für die Sicht zum Generieren einer HTML-Antwortinhalts an den Browser. Fügen Sie im Projekt eine Vorlage anzeigen, die mit der `Index` Methode. Zu diesem Zweck Maustaste innerhalb der `Index` -Methode, und klicken Sie auf **Ansicht hinzufügen**.

![](adding-a-view/_static/image1.png)

Die **Ansicht hinzufügen** Dialogfeld wird angezeigt. Lassen Sie die Möglichkeit, sie sind, und klicken Sie auf, die Standardeinstellungen der **hinzufügen** Schaltfläche:

![](adding-a-view/_static/image2.png)

Die *MvcMovie\Views\HelloWorld* Ordner und die *MvcMovie\Views\HelloWorld\Index.cshtml* -Datei erstellt werden. Sehen Sie diese in **Projektmappen-Explorer**:

![](adding-a-view/_static/image3.png)

Im folgenden gezeigt die *Index.cshtml* -Datei, die erstellt wurde:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Hinzufügen von HTML-Code unter der `<h2>` Tag. Das geänderte *MvcMovie\Views\HelloWorld\Index.cshtml* Datei wird unten gezeigt.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zu der `HelloWorld` Controller (`http://localhost:xxxx/HelloWorld`). Die `Index` Methode in Ihren in der Regel nicht viel Arbeit ausführen; es einfach ausgeführt wurde, die Anweisung `return View()`, der angegeben wird, dass die Methode eine Vorlagendatei Ansicht verwenden soll, um eine Antwort an den Browser zu rendern. Da Sie den Namen der Vorlagendatei Ansicht verwenden explizit angegeben haben, ASP.NET MVC verwendet ab sofort standardmäßig mit der *Index.cshtml* -Datei in die *\Views\HelloWorld* Ordner. Die folgende Abbildung zeigt die Zeichenfolge in der Ansicht fest programmiert.

![](adding-a-view/_static/image6.png)

Sieht ziemlich guten. Beachten Sie jedoch, dass die Titelleiste des Browsers sagt "Index" und besagt, dass große Titel auf der Seite "Meine MVC-Anwendung." Wir ändern die.

## <a name="changing-views-and-layout-pages"></a>Ändern von Sichten und Layoutseiten

Zuerst möchten Sie den Titel "Meine MVC-Anwendung" am oberen Rand der Seite zu ändern. Dieser Text ist üblich, jeder Seite. Er ist tatsächlich an nur einem Ort im Projekt implementiert, obwohl er auf jeder Seite in der Anwendung angezeigt wird. Wechseln Sie zu der */Ansichten/freigegeben* Ordner **Projektmappen-Explorer** , und öffnen Sie die  *\_Layout.cshtml* Datei. Diese Datei heißt ein *Layoutseite* und die freigegebenen "Shell", die alle anderen Seiten zu verwenden ist.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Layoutvorlagen können Sie zum Angeben von HTML-Container-Layout Ihres Standorts an einem Ort, und wenden Sie es über mehrere Seiten an Ihrem Standort. Beachten Sie die `@RenderBody()` Zeile am unteren Rand der Datei. `RenderBody`ist ein Platzhalter, in dem alle Ansicht-spezifische Seiten, die Sie erstellen, "umschlossene" in der Seite "Layout" angezeigt. Ändern Sie die Titelüberschrift in die Layoutvorlage aus "Meine MVC-Anwendung" in "MVC-Film-App".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Führen Sie die Anwendung, und beachten Sie, dass es jetzt "MVC-Film-App" lautet. Klicken Sie auf die **zu** finden Sie unter Link, und Sie die Seite "MVC Film-App" zu veranschaulicht. Wir konnten die Änderung in der Layoutvorlage einmal durchgeführt, und haben alle Seiten auf der Website widerspiegeln den neuen Titel.

![](adding-a-view/_static/image9.png)

Die vollständige  *\_Layout.cshtml* Datei wird unten gezeigt:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Jetzt sehen wir ändern Sie den Titel der Seite "Index" (Ansicht).

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Es gibt zwei Bereiche zu ändern: zuerst den Text, der angezeigt wird in der Titelleiste des Browsers, und klicken Sie dann in den sekundären Header (der `<h2>` Element). Sie nehmen diese geringfügig anders vor, damit Sie erkennen können, welcher Teil der App mithilfe einer Codebearbeitung geändert wird.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

An, dass der Titel der HTML-anzuzeigenden Code über legt eine `Title` Eigenschaft von der `ViewBag` Objekt (Dies ist der *Index.cshtml* Vorlage anzeigen). Wenn Sie wieder den Quellcode der Layoutvorlage betrachten, werden Sie bemerken, dass die Vorlage, diesen Wert in verwendet der `<title>` -Element als Teil der `<head>` Teil der HTML-Code. Bei diesem Ansatz können Sie problemlos andere Parameter zwischen Ihrer Vorlage anzeigen und die Layoutdatei übergeben.

Führen Sie die Anwendung, und navigieren Sie zu `http://localhost:xx/HelloWorld`. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG+F5, um das Laden der Antwort vom Server zu erzwingen.)

Beachten Sie außerdem wie der Inhalt in die *Index.cshtml* Vorlage anzeigen mit zusammengeführt wurde die  *\_Layout.cshtml* Vorlage anzeigen und eine einzelne HTML-Antwort an den Browser gesendet wurde. Layoutvorlagen erleichtern ungemein das Vornehmen von Änderungen an allen Seiten in Ihrer Anwendung.

![](adding-a-view/_static/image10.png)

Unser kleiner Beitrag zu den „Daten“ (in diesem Fall die Meldung "Hello from our View Template!") ist jedoch hartcodiert. Die MVC-Anwendung weist ein „V“ (View [Ansicht]) auf, und Sie verfügen über ein „C“ (Controller), aber noch nicht über ein „M“ (Modell). In Kürze, wir wird exemplarisch erklärt, wie eine Datenbank erstellen und das Modelldaten daraus abrufen.

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Bevor wir mit einer Datenbank aufrufen und erörtern, Modelle, jedoch, zunächst sprechen wir über Informationen auf dem Controller an eine Ansicht übergeben. Controllerklassen werden als Antwort auf eine eingehende URL-Anforderung aufgerufen. Eine Controllerklasse ist, in dem Sie den Code schreiben, der Daten aus einer Datenbank abgerufen, verarbeitet die eingehenden Parameter und letztlich entscheidet, welche Art der Antwort zurück an den Browser gesendet. Ansichtsvorlagen können dann verwendet werden von einem Domänencontroller zum Generieren und Formatieren einer HTML-Antwortinhalts an den Browser.

Controller sind verantwortlich für das Bereitstellen von den Daten oder Objekte in Auftrag für eine Sicht Vorlage zum Rendern einer Antwortinhalts an den Browser erforderlich sind. Eine Ansichtenvorlage sollte nie Geschäftslogik ausführen oder direkt mit einer Datenbank interagieren. Stattdessen sollte es nur mit den Daten arbeiten, die vom Controller bereitgestellt wird. Verwalten diese "Trennung von Anliegen" hilft Ihnen, Ihre Ihren Code bereinigen und sind besser verwaltbar.

Derzeit die `Welcome` Aktionsmethode in der `HelloWorldController` -Klasse akzeptiert eine `name` und ein `numTimes` übergeben wird und Ausgaben die Werte direkt an den Browser. Anstatt den Controller, die dieser Antwort als Zeichenfolge Rendern nutzen zu können, Ändern des Controllers, um stattdessen verwenden Sie eine Vorlage anzeigen. Die Ansichtsvorlage generiert eine dynamische Antwort. Das bedeutet, dass Sie die entsprechenden Datenelemente vom Controller an die Ansicht übergeben müssen, um die Antwort zu generieren. Hierzu können Sie, dass des Controllers die dynamische Daten zu konzentrieren, die in der Vorlage anzeigen muss ein `ViewBag` -Objekt, das die Ansicht klicken Sie dann auf Sie zugreifen können.

Zurück zu der *HelloWorldController.cs* Datei und ändern Sie die `Welcome` -Methode zum Hinzufügen einer `Message` und `NumTimes` -Wert an die `ViewBag` Objekt. `ViewBag`ist ein dynamisches Objekt, d. h. Sie beliebig ihm aufnehmen können. die `ViewBag` Objekt verfügt über keine definierten Eigenschaften aus, bis Sie etwas darin einfügen. Die vollständige Datei *HelloWorldController.cs* sieht wie folgt aus:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Jetzt die `ViewBag` -Objekt enthält Daten, die automatisch an die Ansicht übergeben werden.

Als Nächstes fügen Sie eine Vorlage Willkommen anzeigen! In der **Debuggen** klicken Sie im Menü **MvcMovie erstellen** um sicherzustellen, dass das Projekt kompiliert wird.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Klicken Sie dann mit der rechten Maustaste innerhalb der `Welcome` -Methode, und klicken Sie auf **Ansicht hinzufügen**. Hier wird die **Ansicht hinzufügen** das Dialogfeld sieht wie folgt aus:

![](adding-a-view/_static/image13.png)

Klicken Sie auf **hinzufügen**, und fügen Sie folgenden Code unter der `<h2>` Element in der neuen *Welcome.cshtml* Datei. Erstellen Sie eine Schleife, die besagt, "Hello" so häufig dass wie der Benutzer besagt, dass es sollten. Die vollständige *Welcome.cshtml* Datei wird unten gezeigt.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zu folgender URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Jetzt werden die Daten der URL entnommen und automatisch an den Controller übergeben. Der Controller Packt die Daten in einem `ViewBag` Objekt auf und übergibt das Objekt in der Ansicht. Die Ansicht zeigt die Daten dann als HTML an den Benutzer.

![](adding-a-view/_static/image14.png)

Das war also eine Art eines „M“ für Modell, jedoch nicht der Art Datenbank. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.

>[!div class="step-by-step"]
[Zurück](adding-a-controller.md)
[Weiter](adding-a-model.md)
