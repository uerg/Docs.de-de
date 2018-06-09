---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Hinzufügen einer Ansicht | Microsoft Docs
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher zu verfolgen und demo...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 61a93c1430e9e39543c69b84901a50ceb710a5ae
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "30874513"
---
<a name="adding-a-view"></a>Hinzufügen einer Ansicht
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Lernprogramms steht [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.


In diesem Abschnitt Sie so ändern Sie nun die `HelloWorldController` Klasse, die anzeigen, die Vorlagendateien sauberes kapseln den Prozess der Generierung der HTML-Antworten auf einem Client verwendet.

Erstellen Sie eine Ansicht Vorlage mithilfe der [Razor-Ansichtsmodul](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) mit ASP.NET MVC 3 eingeführt wurde. Razor-basierte Ansichtsvorlagen haben eine *cshtml* Dateierweiterung aus, und geben Sie eine elegante Methode zum Erstellen von HTML-Ausgabe mithilfe von c#. Razor minimiert die Anzahl von Zeichen und Tastatureingaben erforderlich, wenn eine Ansichtenvorlage schreiben und ermöglicht eine schnelle, Fluid Codierung Workflow.

Derzeit gibt die `Index`-Methode eine Zeichenfolge mit der Meldung zurück, die in der Controllerklasse hartcodiert ist. Ändern der `Index` -Methode zur Rückgabe einer `View` -Objekts, wie im folgenden Code gezeigt:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Die `Index` oben genannten Methode verwendet eine Vorlage für die Sicht zum Generieren einer HTML-Antwortinhalts an den Browser. Controllermethoden (auch bekannt als [Aktionsmethoden](http://rachelappel.com/asp.net-mvc-actionresults-explained)), wie z. B. die `Index` oben in der Regel Methodenrückgabewert ein [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (oder eine abgeleitete Klasse [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), nicht primitive Typen wie Zeichenfolge.

Fügen Sie im Projekt eine Vorlage anzeigen, die mit der `Index` Methode. Zu diesem Zweck Maustaste innerhalb der `Index` -Methode, und klicken Sie auf **Ansicht hinzufügen**.

![](adding-a-view/_static/image1.png)

Die **Ansicht hinzufügen** Dialogfeld wird angezeigt. Lassen Sie die Möglichkeit, sie sind, und klicken Sie auf, die Standardeinstellungen der **hinzufügen** Schaltfläche:

![](adding-a-view/_static/image2.png)

Die *MvcMovie\Views\HelloWorld* Ordner und die *MvcMovie\Views\HelloWorld\Index.cshtml* -Datei erstellt werden. Sehen Sie diese in **Projektmappen-Explorer**:

![](adding-a-view/_static/image3.png)

Im folgenden gezeigt die *Index.cshtml* -Datei, die erstellt wurde:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Fügen Sie folgenden HTML-Code unter der `<h2>` Tag.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Die vollständige *MvcMovie\Views\HelloWorld\Index.cshtml* Datei wird unten gezeigt.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Bei Verwendung von Visual Studio 2012 im Projektmappen-Explorer mit der rechten Maustaste klicken Sie auf die *Index.cshtml* Datei, und wählen Sie **in Seitenprüfung anzeigen**.

![PI](adding-a-view/_static/image5.png)

Die [Page Inspector Lernprogramm](../../views/using-page-inspector-in-aspnet-mvc.md) bietet weitere Informationen zur dieses neue Tool.

Alternativ können Sie die Anwendung auszuführen, und navigieren Sie zu der `HelloWorld` Controller (`http://localhost:xxxx/HelloWorld`). Die `Index` Methode in Ihren in der Regel nicht viel Arbeit ausführen; es einfach ausgeführt wurde, die Anweisung `return View()`, der angegeben wird, dass die Methode eine Vorlagendatei Ansicht verwenden soll, um eine Antwort an den Browser zu rendern. Da Sie den Namen der Vorlagendatei Ansicht verwenden explizit angegeben haben, ASP.NET MVC verwendet ab sofort standardmäßig mit der *Index.cshtml* -Datei in die *\Views\HelloWorld* Ordner. Die folgende Abbildung zeigt die Zeichenfolge &quot;Hello aus unserem Vorlage anzeigen!&quot; in der Ansicht fest programmiert.

![](adding-a-view/_static/image6.png)

Sieht ziemlich guten. Beachten Sie jedoch, dass der Browser-Titelleiste zeigt &quot;Index-meine ASP.NET A&quot; und sagt der big Link oben auf der Seite &quot;Ihr Logo hier einfügen.&quot; Unterhalb der &quot;Ihr Logo hier.&quot; Link Registrierung und Protokolldateien in Links, und klicken Sie unten, Links zur Startseite, etwa sind, und wenden Sie sich an Seiten. Einige dieser ändern.

## <a name="changing-views-and-layout-pages"></a>Ändern von Sichten und Layoutseiten

Zunächst, den Sie ändern möchten die &quot;Ihr Logo hier.&quot; Titel am oberen Rand der Seite. Dieser Text ist üblich, jeder Seite. Er ist tatsächlich an nur einem Ort im Projekt implementiert, obwohl er auf jeder Seite in der Anwendung angezeigt wird. Wechseln Sie zu der */Ansichten/freigegeben* Ordner **Projektmappen-Explorer** , und öffnen Sie die  *\_Layout.cshtml* Datei. Diese Datei heißt ein *Layoutseite* und es wird die freigegebene &quot;Shell&quot; , die alle anderen Seiten zu verwenden.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Layoutvorlagen können Sie zum Angeben von HTML-Container-Layout Ihres Standorts an einem Ort, und wenden Sie es über mehrere Seiten an Ihrem Standort. Suchen Sie die Zeile `@RenderBody()`. `RenderBody` ist ein Platzhalter, bei dem alle ansichtsspezifischen Seiten, die Sie erstellen, von der Layoutseite &quot;umschlossen&quot; angezeigt werden. Angenommen, Sie wählen Sie den Link Info der *Views\Home\About.cshtml* Ansicht gerendert wird, innerhalb der `RenderBody` Methode.

Ändern Sie den Titel der Website-Überschrift in der Layoutvorlage aus &quot;Ihr Logo hier einfügen&quot; auf &quot;MVC Film&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Ersetzen Sie den Inhalt der Title-Element, durch das folgende Markup:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Ausführen der Anwendung und den Hinweis, der angeblich jetzt &quot;MVC Film &quot;. Klicken Sie auf die **zu** Link, und Sie finden Sie unter veranschaulicht diese Seite &quot;MVC Film&quot;ebenfalls. Wir konnten die Änderung in der Layoutvorlage einmal durchgeführt, und haben alle Seiten auf der Website widerspiegeln den neuen Titel.

![](adding-a-view/_static/image8.png)

Jetzt sehen wir Ändern des Titels die Indexansicht.

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Es gibt zwei Bereiche zu ändern: zuerst den Text, der angezeigt wird in der Titelleiste des Browsers, und klicken Sie dann in den sekundären Header (der `<h2>` Element). Sie nehmen diese geringfügig anders vor, damit Sie erkennen können, welcher Teil der App mithilfe einer Codebearbeitung geändert wird.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

An, dass der Titel der HTML-anzuzeigenden Code über legt eine `Title` Eigenschaft von der `ViewBag` Objekt (Dies ist der *Index.cshtml* Vorlage anzeigen). Wenn Sie wieder den Quellcode der Layoutvorlage betrachten, werden Sie bemerken, dass die Vorlage, diesen Wert in verwendet der `<title>` -Element als Teil der `<head>` Abschnitt der HTML-Datei, die wir zuvor geändert. Mit diesem `ViewBag` Ansatz, Sie können problemlos andere Parameter übergeben zwischen Ihrer Vorlage anzeigen und Layout-Datei.

Führen Sie die Anwendung, und navigieren Sie zu `http://localhost:xx/HelloWorld`. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG+F5, um das Laden der Antwort vom Server zu erzwingen.) Titel des Browsers wird erstellt, mit der `ViewBag.Title` legen wir der *Index.cshtml* anzeigen, Vorlage und die zusätzlichen &quot;-Film-App&quot; in der Layoutdatei hinzugefügt.

Beachten Sie außerdem wie der Inhalt in die *Index.cshtml* Vorlage anzeigen mit zusammengeführt wurde die  *\_Layout.cshtml* Vorlage anzeigen und eine einzelne HTML-Antwort an den Browser gesendet wurde. Layoutvorlagen erleichtern ungemein das Vornehmen von Änderungen an allen Seiten in Ihrer Anwendung.

![](adding-a-view/_static/image9.png)

Unsere wenig &quot;Daten&quot; (in diesem Fall die &quot;Hello aus unserem Vorlage anzeigen!&quot; Nachricht) ist hartcodiert, obwohl. Die MVC-Anwendung verfügt über eine &quot;V&quot; (Ansicht), haben Sie eine &quot;C&quot; (Domänencontroller), aber kein &quot;M&quot; (Modell) noch. In Kürze, wir wird exemplarisch erklärt, wie eine Datenbank erstellen und das Modelldaten daraus abrufen.

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Bevor wir mit einer Datenbank aufrufen und erörtern, Modelle, jedoch, zunächst sprechen wir über Informationen auf dem Controller an eine Ansicht übergeben. Controllerklassen werden als Antwort auf eine eingehende URL-Anforderung aufgerufen. Eine Controllerklasse ist, Schreiben Sie der Code, der den eingehende Browser verarbeitet, Anforderungen, ruft Daten aus einer Datenbank ab und letztlich entscheidet, welche Art der Antwort zurück an den Browser gesendet wird. Ansichtsvorlagen können dann verwendet werden von einem Domänencontroller zum Generieren und Formatieren einer HTML-Antwortinhalts an den Browser.

Controller sind verantwortlich für das Bereitstellen von den Daten oder Objekte in Auftrag für eine Sicht Vorlage zum Rendern einer Antwortinhalts an den Browser erforderlich sind. Eine bewährte Methode: **eine Ansichtenvorlage sollte nie Geschäftslogik ausführen oder direkt mit einer Datenbank interagieren**. Stattdessen sollte eine Ansichtenvorlage nur mit den Daten arbeiten, die vom Controller bereitgestellt wird. Verwalten diese &quot;Trennung von Anliegen&quot; bleibt der Code bereinigen, einfach zu testender und sind besser verwaltbar.

Derzeit die `Welcome` Aktionsmethode in der `HelloWorldController` -Klasse akzeptiert eine `name` und ein `numTimes` übergeben wird und Ausgaben die Werte direkt an den Browser. Anstatt den Controller, die dieser Antwort als Zeichenfolge Rendern nutzen zu können, Ändern des Controllers, um stattdessen verwenden Sie eine Vorlage anzeigen. Die Ansichtsvorlage generiert eine dynamische Antwort. Das bedeutet, dass Sie die entsprechenden Datenelemente vom Controller an die Ansicht übergeben müssen, um die Antwort zu generieren. Hierzu können Sie durch die Verwendung des Controllers, die die dynamic Data (Parameter) zu konzentrieren, die in der Vorlage anzeigen muss ein `ViewBag` -Objekt, das die Ansicht klicken Sie dann auf Sie zugreifen können.

Zurück zu der *HelloWorldController.cs* Datei und ändern Sie die `Welcome` -Methode zum Hinzufügen einer `Message` und `NumTimes` -Wert an die `ViewBag` Objekt. `ViewBag` ist ein dynamisches Objekt, d. h. Sie beliebig ihm aufnehmen können. die `ViewBag` Objekt verfügt über keine definierten Eigenschaften aus, bis Sie etwas darin einfügen. Die [Bindungssystem für ASP.NET MVC-Modell](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter (`name` und `numTimes`) aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode. Die vollständige Datei *HelloWorldController.cs* sieht wie folgt aus:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Jetzt die `ViewBag` -Objekt enthält Daten, die automatisch an die Ansicht übergeben werden.

Als Nächstes fügen Sie eine Vorlage Willkommen anzeigen! In der **erstellen** klicken Sie im Menü **MvcMovie erstellen** um sicherzustellen, dass das Projekt kompiliert wird.

Klicken Sie dann mit der rechten Maustaste innerhalb der `Welcome` -Methode, und klicken Sie auf **Ansicht hinzufügen**.

![](adding-a-view/_static/image10.png)

Hier wird die **Ansicht hinzufügen** das Dialogfeld sieht wie folgt aus:

![](adding-a-view/_static/image11.png)

Klicken Sie auf **hinzufügen**, und fügen Sie folgenden Code unter der `<h2>` Element in der neuen *Welcome.cshtml* Datei. Erstellen Sie eine Schleife, die besagt, dass &quot;Hello&quot; so häufig wie der Benutzer sagt sinnvoll. Die vollständige *Welcome.cshtml* Datei wird unten gezeigt.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zu folgender URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Jetzt Daten aus der URL und werden übergeben, auf dem Controller mithilfe der [modellieren Binder](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Der Controller Packt die Daten in einem `ViewBag` Objekt auf und übergibt das Objekt in der Ansicht. Die Ansicht zeigt die Daten dann als HTML an den Benutzer.

![](adding-a-view/_static/image12.png)

Im obigen Beispiel wird ein `ViewBag` Objekt, das Daten auf dem Controller an eine Ansicht zu übergeben. In diesem Lernprogramm letzteren Fall einem Ansichtsmodell verwenden wir Daten von einem Controller an eine Ansicht zu übergeben. Die Ansicht Modell Vorgehensweise bei der Weitergabe von Daten wird über die Ansicht Behälter Ansatz viel bevorzugt. Finden Sie im Blogeintrag [dynamische V stark typisierte Ansichten](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) für Weitere Informationen.

Nun, das eine Art wurde von einer &quot;M&quot; Modell, aber nicht die Art der Datenbank. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.

> [!div class="step-by-step"]
> [Zurück](adding-a-controller.md)
> [Weiter](adding-a-model.md)
