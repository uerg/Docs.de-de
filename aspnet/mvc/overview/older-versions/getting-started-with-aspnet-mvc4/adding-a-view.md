---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Hinzufügen einer Ansicht | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Tutorials ist hier verfügbar, dass das ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist eine sicherere, viel einfacher zu folgen und demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 6244bfd96c547c5ccbcaed7ba17214df49d2886c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830336"
---
<a name="adding-a-view"></a>Hinzufügen einer Ansicht
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.


In diesem Abschnitt also ändern Sie die `HelloWorldController` anzeigen, die Vorlagendateien an sauber zu kapseln den Prozess der Generierung von HTML-Antworten an einen Client zu verwendende Klasse an.

Erstellen Sie eine Ansicht Vorlage mithilfe der [Razor-ansichtsengine](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) mit ASP.NET MVC 3 eingeführt wurde. Razor-basierte Ansichtsvorlagen haben eine *.cshtml* Dateierweiterung aus, und geben Sie eine elegante Methode zum Erstellen von HTML-Ausgabe mithilfe von c#. Razor minimiert die Anzahl von Zeichen und Tastaturanschläge erforderlich, wenn eine ansichtsvorlage zu schreiben und ermöglicht eine schnelle, fließende Workflows programmieren.

Derzeit gibt die `Index`-Methode eine Zeichenfolge mit der Meldung zurück, die in der Controllerklasse hartcodiert ist. Ändern der `Index` -Methode zur Rückgabe einer `View` Objekt, wie im folgenden Code gezeigt:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Die `Index` oben genannte Methode verwendet eine ansichtsvorlage zum Generieren einer HTML-Antwortinhalts an den Browser. Controllermethoden (auch bekannt als [Aktionsmethoden](http://rachelappel.com/asp.net-mvc-actionresults-explained)), wie z. B. die `Index` oben in der Regel Methodenrückgabewert ein [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (oder eine abgeleitete Klasse [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), keine primitiven Typen wie Zeichenfolge.

Fügen Sie in das Projekt eine ansichtsvorlage, mit denen Sie mit der `Index` Methode. Klicken Sie dazu auf, auf die `Index` -Methode, und klicken Sie auf **Ansicht hinzufügen**.

![](adding-a-view/_static/image1.png)

Die **Ansicht hinzufügen** Dialogfeld wird angezeigt. Behalten Sie die Möglichkeit, sie sind, und klicken Sie auf, die Standardwerte der **hinzufügen** Schaltfläche:

![](adding-a-view/_static/image2.png)

Die *MvcMovie\Views\HelloWorld* Ordner und die *MvcMovie\Views\HelloWorld\Index.cshtml* -Datei erstellt werden. Sie finden diese in **Projektmappen-Explorer**:

![](adding-a-view/_static/image3.png)

Im folgenden dargestellt die *"Index.cshtml"* -Datei, die erstellt wurde:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Fügen Sie folgenden HTML-Code unter der `<h2>` Tag.

[!code-html[Main](adding-a-view/samples/sample2.html)]

Die vollständige *MvcMovie\Views\HelloWorld\Index.cshtml* Datei ist unten dargestellt.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Wenn Sie Visual Studio 2012 im Projektmappen-Explorer verwenden, mit der rechten Maustaste die *"Index.cshtml"* und wählen Sie **in Seitenprüfung anzeigen**.

![PI](adding-a-view/_static/image5.png)

Die [Page Inspector Tutorial](../../views/using-page-inspector-in-aspnet-mvc.md) finden Sie weitere Informationen zu diesem neuen Tool.

Alternativ führen Sie die Anwendung, und navigieren Sie zu der `HelloWorld` Controller (`http://localhost:xxxx/HelloWorld`). Die `Index` -Methode in Ihrer Controller nicht viel Arbeit ausführen; es einfach ausgeführt wurde, die Anweisung `return View()`, die angab, dass die Methode eine ansichtsvorlagendatei verwenden soll, um eine Antwort an den Browser auszugeben. Da Sie den Namen der ansichtsvorlagendatei mit nicht explizit angegeben haben, ASP.NET MVC standardmäßig die *"Index.cshtml"* -Datei in die *\Views\HelloWorld* Ordner. Die folgende Abbildung zeigt die Zeichenfolge &quot;Hello from our View Template!&quot; in der Ansicht hartcodiert.

![](adding-a-view/_static/image6.png)

Das sieht recht gut. Beachten Sie jedoch, dass die Titelleiste des Browsers wird &quot;Index-meine ASP.NET A&quot; und der große Link oben auf der Seite &quot;Ihr Logo hier einfügen.&quot; Unterhalb der &quot;Ihr Logo hier.&quot; Link etwa werden von Registrierung und melden Sie sich in der Links, und klicken Sie unten, Links zur Startseite, und wenden Sie sich an Seiten. Ändern wir einige dieser.

## <a name="changing-views-and-layout-pages"></a>Ändern von Ansichten und Layoutseiten

Zunächst, den Sie ändern möchten die &quot;Ihr Logo hier.&quot; Titel am oberen Rand der Seite. Dieser Text ist üblich, jede Seite. Es ist tatsächlich in nur an einer Stelle im Projekt implementiert, obwohl er auf jeder Seite in der Anwendung angezeigt wird. Wechseln Sie zu der */Views/Shared* Ordner **Projektmappen-Explorer** , und öffnen Sie die  *\_Layout.cshtml* Datei. Diese Datei heißt eine *Layoutseite* und ist der gemeinsam verwendeten &quot;Shell&quot; , die alle anderen Seiten zu verwenden.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Layoutvorlagen können Sie die HTML-Containerlayout Ihrer Website zentral anzugeben und wenden Sie es über mehrere Seiten auf Ihrer Website. Suchen Sie die Zeile `@RenderBody()`. `RenderBody` ist ein Platzhalter, bei dem alle ansichtsspezifischen Seiten, die Sie erstellen, von der Layoutseite &quot;umschlossen&quot; angezeigt werden. Wenn Sie auswählen, auf den Link Info, z. B. der *Views\Home\About.cshtml* Ansicht gerendert wird, in der `RenderBody` Methode.

Ändern Sie die Titel der Site Überschrift in der Layoutvorlage aus &quot;Ihr Logo hier einfügen&quot; zu &quot;MVC Movie&quot;.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Ersetzen Sie den Inhalt der Title-Elements, durch das folgende Markup:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Ausführen der Anwendung und beachten Sie, der jetzt angeblich &quot;MVC Movie &quot;. Klicken Sie auf die **zu** Link, und Sie sehen wie diese Seite zeigt &quot;MVC Movie&quot;ebenfalls. Wir konnten für die Änderung einmal in der Layoutvorlage aus, und haben alle Seiten auf der Website entsprechend den neuen Titel.

![](adding-a-view/_static/image8.png)

Jetzt ändern wir den Titel der Ansicht "Index".

Open *MvcMovie\Views\HelloWorld\Index.cshtml*. Es gibt zwei Bereiche zu ändern: zuerst der Text, der angezeigt wird in der Titelleiste des Browsers, und klicken Sie dann in die sekundäre Kopfzeile (die `<h2>` Element). Sie nehmen diese geringfügig anders vor, damit Sie erkennen können, welcher Teil der App mithilfe einer Codebearbeitung geändert wird.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

An die HTML-Titel angezeigt werden, den Code oben legt ein `Title` Eigenschaft der `ViewBag` Objekt (dieser befindet sich in der *"Index.cshtml"* Vorlage anzeigen). Wenn Sie wieder den Quellcode der Layoutvorlage betrachten, werden Sie feststellen, dass die Vorlage wird, diesen Wert in verwendet der `<title>` -Element als Teil der `<head>` Teil der HTML-Code, die wir zuvor geändert. Mit diesem `ViewBag` Ansatz können Sie problemlos andere Parameter übergeben zwischen Ihrer Vorlage anzeigen und die Layoutdatei.

Führen Sie die Anwendung, und navigieren Sie zu `http://localhost:xx/HelloWorld`. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG+F5, um das Laden der Antwort vom Server zu erzwingen.) Der Titel des Browsers wird erstellt, mit der `ViewBag.Title` legen wir den *"Index.cshtml"* Anzeigen der Vorlage und die zusätzlichen &quot;-Movie App&quot; in der Layoutdatei hinzugefügt.

Beachten Sie außerdem wie der Inhalt in die *"Index.cshtml"* ansichtsvorlage zusammengeführt wurde, mit der  *\_Layout.cshtml* Vorlage anzeigen und eine einzelne HTML-Antwort an den Browser gesendet wurde. Layoutvorlagen erleichtern ungemein das Vornehmen von Änderungen an allen Seiten in Ihrer Anwendung.

![](adding-a-view/_static/image9.png)

Unsere wenig &quot;Daten&quot; (in diesem Fall die &quot;Hello from our View Template!&quot; Nachricht) ist hartcodiert, jedoch. Die MVC-Anwendung verfügt über eine &quot;V&quot; (Ansicht), verfügen Sie über eine &quot;C&quot; (Controller), aber kein &quot;M&quot; (model) noch. Kurz darauf begleiten wir erläutert, wie eine Datenbank erstellen und Modellieren von Daten daraus abrufen.

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Bevor wir mit einer Datenbank und über Modelle sprechen, jedoch zunächst sprechen wir über Informationen vom Controller an eine Ansicht übergeben. Controller-Klassen werden als Reaktion auf eine eingehende URL-Anforderung aufgerufen. Eine Controllerklasse ist, Schreiben Sie der Code, der den eingehende Browser verarbeitet, angefordert wird, ruft Daten aus einer Datenbank ab, und letztlich entscheidet, welche Art der Antwort zurück an den Browser senden. Anzeigen von Vorlagen können dann verwendet werden von einem Controller zum Generieren und Formatieren einer HTML-Antwortinhalts an den Browser.

Controller sind verantwortlich für das Bereitstellen der beliebige Daten oder Objekte erforderlich sind, in der Reihenfolge für eine ansichtsvorlage eine Antwort an den Browser auszugeben. Eine bewährte Methode: **eine ansichtsvorlage sollte nie Geschäftslogik ausführen bzw. interagieren mit einer Datenbank direkt**. Stattdessen sollte eine ansichtsvorlage nur mit den Daten arbeiten, die sie durch den Controller bereitgestellt wird. Verwalten diese &quot;Trennung der Belange&quot; bleibt der Code übersichtlich, testfähig und besser verwaltbar.

Derzeit den `Welcome` Aktionsmethode in der `HelloWorldController` -Klasse eine `name` und `numTimes` Parameter und gibt anschließend die Werte direkt an den Browser. Anstatt den Controller diese Antwort als Zeichenfolge Rendern zu lassen, ändern wir den Controller, um stattdessen eine ansichtsvorlage zu verwenden. Die Ansichtsvorlage generiert eine dynamische Antwort. Das bedeutet, dass Sie die entsprechenden Datenelemente vom Controller an die Ansicht übergeben müssen, um die Antwort zu generieren. Hierzu können Sie den Controller die dynamischen Daten (Parameter) zu platzieren, die die ansichtsvorlage, in benötigt einem `ViewBag` -Objekt, das die ansichtsvorlage zugreifen kann.

Wechseln Sie zurück zur der *HelloWorldController.cs* und zu ändern der `Welcome` -Methode zum Hinzufügen einer `Message` und `NumTimes` Wert der `ViewBag` Objekt. `ViewBag` ist ein dynamisches Objekt, was bedeutet, dass die gewünschten, Sie platzieren können. die `ViewBag` Objekt verfügt über keine definierten Eigenschaften aus, bis Sie etwas darin einfügen. Die [ASP.NET MVC-modellbindungssystem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter (`name` und `numTimes`) aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode. Die vollständige Datei *HelloWorldController.cs* sieht wie folgt aus:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Jetzt die `ViewBag` Objekt enthält Daten, die automatisch an die Ansicht übergeben werden.

Als Nächstes fügen Sie eine ansichtsvorlage! In der **erstellen** , wählen Sie im Menü **erstellen MvcMovie** um sicherzustellen, dass das Projekt kompiliert wird.

Klicken Sie dann mit der rechten Maustaste innerhalb der `Welcome` -Methode, und klicken Sie auf **Ansicht hinzufügen**.

![](adding-a-view/_static/image10.png)

Dabei handelt es sich die **Ansicht hinzufügen** das Dialogfeld sieht wie folgt aus:

![](adding-a-view/_static/image11.png)

Klicken Sie auf **hinzufügen**, und fügen Sie den folgenden Code unter der `<h2>` Element in der neuen *Welcome.cshtml* Datei. Erstellen Sie eine Schleife, die besagt, &quot;Hello&quot; so oft wie der Benutzer sagt sollte. Die vollständige *Welcome.cshtml* Datei ist unten dargestellt.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Führen Sie die Anwendung, und navigieren Sie zur folgenden URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Jetzt Daten der URL entnommen und übergeben dem Controller mithilfe der [model Binders](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). Der Controller Packt die Daten in einem `ViewBag` Objekt und übergibt das Objekt an die Ansicht. Die Ansicht zeigt dann die Daten im HTML-Format für den Benutzer.

![](adding-a-view/_static/image12.png)

Im obigen Beispiel haben wir verwendet eine `ViewBag` Objekt, das Daten vom Controller an eine Ansicht zu übergeben. In diesem Tutorial letzteren Fall ein Ansichtsmodell verwenden wir Daten von einem Controller an eine Ansicht zu übergeben. Die ViewModel-Ansatz zum Übergeben von Daten wird über den Ansicht-Behälter-Ansatz im Allgemeinen vorzuziehen. Finden Sie im Blogeintrag [dynamische V stark typisierte Ansichten](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) für Weitere Informationen.

Nun, das eine Art wurde von einem &quot;M&quot; für Modell, jedoch nicht der Art Datenbank. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.

> [!div class="step-by-step"]
> [Zurück](adding-a-controller.md)
> [Weiter](adding-a-model.md)
