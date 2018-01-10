---
title: "Eine Sicht hinzufügen um eine MVC-Anwendung"
author: Rick-Anderson
description: "Eine Sicht hinzufügen um eine MVC-Anwendung"
ms.author: riande
manager: wpickett
ms.date: 09/1721/2017
ms.topic: article
ms.technology: dotnet-mvc
ms.prod: .net-framework
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: d273eb5e99da6c6b7678e03b1a8973041113744c
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/09/2018
---
<a name="adding-a-view"></a>Hinzufügen einer Ansicht
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

In diesem Abschnitt Sie so ändern Sie nun die `HelloWorldController` Klasse, die anzeigen, die Vorlagendateien sauberes kapseln den Prozess der Generierung der HTML-Antworten auf einem Client verwendet. 

Erstellen Sie eine Ansicht Vorlage mithilfe der [Razor-Ansichtsmodul](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Razor-basierte Ansichtsvorlagen haben eine *cshtml* Dateierweiterung aus, und geben Sie eine elegante Methode zum Erstellen von HTML-Ausgabe mithilfe von c#. Razor minimiert die Anzahl von Zeichen und Tastatureingaben erforderlich, wenn eine Ansichtenvorlage schreiben und ermöglicht eine schnelle, Fluid Codierung Workflow.

Derzeit gibt die `Index`-Methode eine Zeichenfolge mit der Meldung zurück, die in der Controllerklasse hartcodiert ist. Ändern der `Index` -Methode zur Rückgabe einer `View` -Objekts, wie im folgenden Code gezeigt:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

Die `Index` oben genannten Methode verwendet eine Vorlage für die Sicht zum Generieren einer HTML-Antwortinhalts an den Browser. Controllermethoden (auch bekannt als [Aktionsmethoden](http://rachelappel.com/asp.net-mvc-actionresults-explained)), wie z. B. die `Index` oben in der Regel Methodenrückgabewert ein [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx) (oder eine abgeleitete Klasse [ActionResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.actionresult.aspx)), nicht primitive Typen wie Zeichenfolge.

Klicken Sie mit der rechten Maustaste auf die *Views\HelloWorld* Ordner, und klicken Sie auf **hinzufügen**, klicken Sie dann auf **MVC 5-Ansichtsseite mit Layout (Razor)**.
  
![](adding-a-view/_static/image1.png)   
  
In der **Namen für Artikel angeben** Dialogfeld Geben Sie *Index*, und klicken Sie dann auf **OK**.  
  
![](adding-a-view/_static/image2.png)  
  
In der **Seite Layout auswählen** Dialogfeld, übernehmen Sie den Standardnamen  **\_Layout.cshtml** , und klicken Sie auf **OK**.  
  
![](adding-a-view/_static/image3.png)  
  
Im Dialogfeld "oben" die *Views\Shared* Ordner im linken Bereich ausgewählt ist. Wenn Sie eine benutzerdefiniertes Layout-Datei in einen anderen Ordner enthält, können Sie es auswählen. Wir müssen die Layoutdatei später im Lernprogramm erörtern

Die *MvcMovie\Views\HelloWorld\Index.cshtml* Datei wird erstellt.

![](adding-a-view/_static/image4.png)

Fügen Sie das folgende hervorgehobene Markup hinzu.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Klicken Sie mit der rechten Maustaste auf die *Index.cshtml* Datei, und wählen Sie **in Browser anzeigen**.

![PI](adding-a-view/_static/image5.png)

Sie können auch mit der rechten Maustaste die *Index.cshtml* Datei, und wählen Sie **in Seitenprüfung anzeigen.** Finden Sie unter der [Page Inspector Lernprogramm](../../views/using-page-inspector-in-aspnet-mvc.md) für Weitere Informationen.

Alternativ können Sie die Anwendung auszuführen, und navigieren Sie zu der `HelloWorld` Controller (`http://localhost:xxxx/HelloWorld`). Die `Index` Methode in Ihren in der Regel nicht viel Arbeit ausführen; es einfach ausgeführt wurde, die Anweisung `return View()`, der angegeben wird, dass die Methode eine Vorlagendatei Ansicht verwenden soll, um eine Antwort an den Browser zu rendern. Da Sie den Namen der Vorlagendatei Ansicht verwenden explizit angegeben haben, ASP.NET MVC verwendet ab sofort standardmäßig mit der *Index.cshtml* -Datei in die *\Views\HelloWorld* Ordner. Die folgende Abbildung zeigt die Zeichenfolge &quot;Hello aus unserem Vorlage anzeigen!&quot; in der Ansicht fest programmiert.

![](adding-a-view/_static/image6.png)

Sieht ziemlich guten. Beachten Sie jedoch, dass der Browser-Titelleiste zeigt &quot;Index - meine ASP.NET Anwe "und der big Link oben auf der Seite"Anwendungsname". Je nachdem wie kleine Sie Ihr Browserfenster vornehmen, müssen Sie möglicherweise die drei Balken in der oberen rechten Ecke, um anzuzeigen klicken Sie auf die zu der **Home**, **zu**, **wenden Sie sich an**, **Registrieren** und **melden Sie sich** Links.

## <a name="changing-views-and-layout-pages"></a>Ändern von Sichten und Layoutseiten

Zunächst, den Sie ändern möchten die &quot;Anwendungsname&quot; Link am oberen Rand der Seite. Dieser Text ist üblich, jeder Seite. Er ist tatsächlich an nur einem Ort im Projekt implementiert, obwohl er auf jeder Seite in der Anwendung angezeigt wird. Wechseln Sie zu der */Ansichten/freigegeben* Ordner **Projektmappen-Explorer** , und öffnen Sie die  *\_Layout.cshtml* Datei. Diese Datei heißt ein *Layoutseite* und befindet sich in den freigegebenen Ordner, die alle anderen Seiten zu verwenden.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Layoutvorlagen können Sie zum Angeben von HTML-Container-Layout Ihres Standorts an einem Ort, und wenden Sie es über mehrere Seiten an Ihrem Standort. Suchen Sie die Zeile `@RenderBody()`. `RenderBody` ist ein Platzhalter, bei dem alle ansichtsspezifischen Seiten, die Sie erstellen, von der Layoutseite &quot;umschlossen&quot; angezeigt werden. Angenommen, Sie wählen die **zu** Link, der *Views\Home\About.cshtml* Ansicht gerendert wird, innerhalb der `RenderBody` Methode.

Ändern Sie den Inhalt des Elements „title“. Ändern der [ActionLink](https://msdn.microsoft.com/en-us/library/dd504972(v=vs.108).aspx) in die Layoutvorlage aus &quot;Anwendungsname&quot; auf &quot;MVC Film&quot; und dem Controller aus `Home` auf `Movies`. Die vollständige Layoutdatei wird unten gezeigt:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Ausführen der Anwendung und den Hinweis, der angeblich jetzt &quot;MVC Film &quot;. Klicken Sie auf die **zu** Link, und Sie finden Sie unter veranschaulicht diese Seite &quot;MVC Film&quot;ebenfalls. Wir konnten die Änderung in der Layoutvorlage einmal durchgeführt, und haben alle Seiten auf der Website widerspiegeln den neuen Titel.

![](adding-a-view/_static/image8.png)

Wenn wir zuerst erstellt die *Views\HelloWorld\Index.cshtml* Datei, sie enthalten den folgenden Code:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Razor-Code wird die Seite "Layout" explizit festlegen. Untersuchen Sie die *Ansichten\\Ansichten "_viewstart.cshtml"* Datei, sie genaue dieselbe Razor Markup enthält. Die  *[Ansichten\\Ansichten "_viewstart.cshtml"](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)*  Datei definiert, das allgemeine Layout, die alle Ansichten verwendet werden, daher können Sie kommentieren, out oder entfernen diesen Code aus der *Views\HelloWorld\ Index.cshtml* Datei.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Sie können mithilfe der `Layout`-Eigenschaft eine andere Layoutansicht festlegen oder diese auf `null` festlegen, damit keine Layoutdatei verwendet wird.

Jetzt sehen wir Ändern des Titels die Indexansicht.

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Es gibt zwei Bereiche zu ändern: zuerst den Text, der angezeigt wird in der Titelleiste des Browsers, und klicken Sie dann in den sekundären Header (der `<h2>` Element). Sie nehmen diese geringfügig anders vor, damit Sie erkennen können, welcher Teil der App mithilfe einer Codebearbeitung geändert wird.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

An, dass der Titel der HTML-anzuzeigenden Code über legt eine `Title` Eigenschaft von der `ViewBag` Objekt (Dies ist der *Index.cshtml* Vorlage anzeigen). Beachten Sie, dass die Layoutvorlage ( *Views\Shared\\_Layout.cshtml* ) verwendet diesen Wert in der `<title>` -Element als Teil der `<head>` Abschnitt der HTML-Datei, die wir zuvor geändert.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Mit diesem `ViewBag` Ansatz, Sie können problemlos andere Parameter übergeben zwischen Ihrer Vorlage anzeigen und Layout-Datei.

Führen Sie die Anwendung aus. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG+F5, um das Laden der Antwort vom Server zu erzwingen.) Titel des Browsers wird erstellt, mit der `ViewBag.Title` legen wir der *Index.cshtml* anzeigen, Vorlage und die zusätzlichen &quot;-Film-App&quot; in der Layoutdatei hinzugefügt.

Beachten Sie außerdem wie der Inhalt in die *Index.cshtml* Vorlage anzeigen mit zusammengeführt wurde die  *\_Layout.cshtml* Vorlage anzeigen und eine einzelne HTML-Antwort an den Browser gesendet wurde. Layoutvorlagen erleichtern ungemein das Vornehmen von Änderungen an allen Seiten in Ihrer Anwendung.

![](adding-a-view/_static/image9.png)

Unsere wenig &quot;Daten&quot; (in diesem Fall die &quot;Hello aus unserem Vorlage anzeigen!&quot; Nachricht) ist hartcodiert, obwohl. Die MVC-Anwendung verfügt über eine &quot;V&quot; (Ansicht), haben Sie eine &quot;C&quot; (Domänencontroller), aber kein &quot;M&quot; (Modell) noch. In Kürze, müssen wir Exemplarische Vorgehensweise zum Erstellen einer Datenbank und das Modelldaten daraus abrufen.

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Bevor wir mit einer Datenbank aufrufen und erörtern, Modelle, jedoch, zunächst sprechen wir über Informationen auf dem Controller an eine Ansicht übergeben. Controllerklassen werden als Antwort auf eine eingehende URL-Anforderung aufgerufen. Eine Controllerklasse ist, Schreiben Sie der Code, der den eingehende Browser verarbeitet, Anforderungen, ruft Daten aus einer Datenbank ab und letztlich entscheidet, welche Art der Antwort zurück an den Browser gesendet wird. Ansichtsvorlagen können dann verwendet werden von einem Domänencontroller zum Generieren und Formatieren einer HTML-Antwortinhalts an den Browser.

Controller sind verantwortlich für das Bereitstellen von den Daten oder Objekte in Auftrag für eine Sicht Vorlage zum Rendern einer Antwortinhalts an den Browser erforderlich sind. Eine bewährte Methode: **eine Ansichtenvorlage sollte nie Geschäftslogik ausführen oder direkt mit einer Datenbank interagieren**. Stattdessen sollte eine Ansichtenvorlage nur mit den Daten arbeiten, die vom Controller bereitgestellt wird. Verwalten diese &quot;Trennung von Anliegen&quot; bleibt der Code bereinigen, einfach zu testender und sind besser verwaltbar.

Derzeit die `Welcome` Aktionsmethode in der `HelloWorldController` -Klasse akzeptiert eine `name` und ein `numTimes` übergeben wird und Ausgaben die Werte direkt an den Browser. Anstatt den Controller, die dieser Antwort als Zeichenfolge Rendern nutzen zu können, Ändern des Controllers, um stattdessen verwenden Sie eine Vorlage anzeigen. Die Ansichtsvorlage generiert eine dynamische Antwort. Das bedeutet, dass Sie die entsprechenden Datenelemente vom Controller an die Ansicht übergeben müssen, um die Antwort zu generieren. Hierzu können Sie durch die Verwendung des Controllers, die die dynamic Data (Parameter) zu konzentrieren, die in der Vorlage anzeigen muss ein `ViewBag` -Objekt, das die Ansicht klicken Sie dann auf Sie zugreifen können.

Zurück zu der *HelloWorldController.cs* Datei und ändern Sie die `Welcome` -Methode zum Hinzufügen einer `Message` und `NumTimes` -Wert an die `ViewBag` Objekt. `ViewBag`ist ein dynamisches Objekt, d. h. Sie beliebig ihm aufnehmen können. die `ViewBag` Objekt verfügt über keine definierten Eigenschaften aus, bis Sie etwas darin einfügen. Die [Bindungssystem für ASP.NET MVC-Modell](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter (`name` und `numTimes`) aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode. Die vollständige Datei *HelloWorldController.cs* sieht wie folgt aus:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Jetzt die `ViewBag` -Objekt enthält Daten, die automatisch an die Ansicht übergeben werden. Als Nächstes fügen Sie eine Vorlage Willkommen anzeigen! In der **erstellen** klicken Sie im Menü **Projektmappe** (oder STRG + UMSCHALT + B) um sicherzustellen, dass das Projekt kompiliert wird. Klicken Sie mit der rechten Maustaste auf die *Views\HelloWorld* Ordner, und klicken Sie auf **hinzufügen**, klicken Sie dann auf **MVC 5-Ansichtsseite mit Layout (Razor)**.
  
![](adding-a-view/_static/image10.png)   
  
In der **Namen für Artikel angeben** Dialogfeld Geben Sie *Willkommen*, und klicken Sie dann auf **OK**.   
  
In der **Seite Layout auswählen** Dialogfeld, übernehmen Sie den Standardnamen  **\_Layout.cshtml** , und klicken Sie auf **OK**.  
  
![](adding-a-view/_static/image11.png)   

Die *MvcMovie\Views\HelloWorld\Welcome.cshtml* Datei wird erstellt.

Ersetzen Sie das Markup in der *Welcome.cshtml* Datei. Erstellen Sie eine Schleife, die besagt, dass &quot;Hello&quot; so häufig wie der Benutzer sagt sinnvoll. Die vollständige *Welcome.cshtml* Datei wird unten gezeigt.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zu folgender URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Jetzt Daten aus der URL und werden übergeben, auf dem Controller mithilfe der [modellieren Binder](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Der Controller Packt die Daten in einem `ViewBag` Objekt auf und übergibt das Objekt in der Ansicht. Die Ansicht zeigt die Daten dann als HTML an den Benutzer.

![](adding-a-view/_static/image12.png)

Im obigen Beispiel wird ein `ViewBag` Objekt, das Daten auf dem Controller an eine Ansicht zu übergeben. Später in diesem Tutorial verwenden wir eine Ansichtsmodell, um Daten von einem Controller an eine Ansicht zu übergeben. Die Ansicht Modell Vorgehensweise bei der Weitergabe von Daten wird über die Ansicht Behälter Ansatz viel bevorzugt. Finden Sie im Blogeintrag [dynamische V stark typisierte Ansichten](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) für Weitere Informationen. 

Nun, das eine Art wurde von einer &quot;M&quot; Modell, aber nicht die Art der Datenbank. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.

>[!div class="step-by-step"]
[Zurück](adding-a-controller.md)
[Weiter](adding-a-model.md)
