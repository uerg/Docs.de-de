---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Hinzufügen einer Ansicht (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 140dc8a7302c52112c2d8873325b2567e3ba66a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829980"
---
<a name="adding-a-view-vb"></a>Hinzufügen einer Ansicht (VB)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit Quellcode VB.NET steht zur Ergänzung dieses Themas. [Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie C# -Code bevorzugen, wechseln Sie zu der [c#-Version](../cs/adding-a-view.md) in diesem Tutorial.


In diesem Abschnitt wir werden so ändern Sie die `HelloWorldController` eine ansichtsvorlagendatei, um ordnungsgemäß zu verwendende Klasse an den Prozess zum Generieren von HTML-Antworten an einen Client zu kapseln.

Beginnen wir mit einer Vorlage anzeigen, mit der `Index` -Methode in der die `HelloWorldController` Klasse. Derzeit den `Index` Methode gibt eine Zeichenfolge mit einer Meldung, die innerhalb der Controllerklasse hartcodiert ist. Ändern der `Index` -Methode zur Rückgabe einer `View` -Objekts, wie im folgenden gezeigt:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Nun fügen Sie eine ansichtsvorlage auf das Projekt, das wir mit aufrufen kann die `Index` Methode. Klicken Sie dazu auf, auf die `Index` -Methode, und klicken Sie auf **Ansicht hinzufügen**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

Die **Ansicht hinzufügen** Dialogfeld wird angezeigt. Lassen Sie die Standardeinträge, und klicken Sie auf die **hinzufügen** Schaltfläche.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

Die *MvcMovie\Views\HelloWorld* Ordner und die *MvcMovie\Views\HelloWorld\Index.vbhtml* -Datei erstellt werden. Sie finden diese in **Projektmappen-Explorer**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Hinzufügen von HTML-Code unter der `<h2>` Tag. Die geänderte *MvcMovie\Views\HelloWorld\Index.vbhtml* Datei ist unten dargestellt.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Führen Sie die Anwendung, und navigieren Sie zu der &quot;Hallo Welt&quot; Controller (`http://localhost:xxxx/HelloWorld`). Die `Index` -Methode in Ihrer Controller nicht viel Arbeit ausführen; es einfach ausgeführt wurde, die Anweisung `return View()`, die angegeben, dass wir eine ansichtsvorlagendatei zum Rendern einer Antwort an den Client verwenden möchten. Da wir den Namen der ansichtsvorlagendatei mit nicht explizit angegeben hat, ASP.NET MVC standardmäßig die *Index.vbhtml* Ansichtsdatei innerhalb der *\Views\HelloWorld* Ordner. Die folgende Abbildung zeigt die Zeichenfolge in der Ansicht hartcodiert ist.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Das sieht recht gut. Beachten Sie jedoch, dass die Titelleiste des Browsers, sagt &quot;Index&quot; und große Titel auf der Seite &quot;Meine MVC-Anwendung.&quot; Diese ändern.

## <a name="changing-views-and-layout-pages"></a>Ändern von Ansichten und Layoutseiten

Zuerst ändern wir den Text &quot;Meine MVC-Anwendung.&quot; Dieser Text wird freigegeben und auf jeder Seite angezeigt wird. Er wird tatsächlich in nur an einer Stelle in Ihrem Projekt, auch wenn es auf jeder Seite in der vorliegenden Anwendung ist. Wechseln Sie zu der */Views/Shared* Ordner **Projektmappen-Explorer** , und öffnen Sie die  *\_Layout.vbhtml* Datei. Diese Datei ist eine Layoutseite bezeichnet und ist der gemeinsam verwendeten &quot;Shell&quot; , die alle anderen Seiten zu verwenden.

Beachten Sie die `@RenderBody()` Codezeile im unteren Bereich der Datei. `RenderBody` ist ein Platzhalter, in dem alle Seiten, die Sie erstellen, anzeigen, &quot;umschlossen&quot; der Layoutseite. Ändern der `<h1>` Überschrift von **&quot;** Meine MVC-Anwendung&quot; zu &quot;MVC-Filmapp&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Führen Sie die Anwendung, und notieren Sie sich, es sagt jetzt &quot;MVC-Filmapp&quot;. Klicken Sie auf die **zu** Link, und dass Seite zeigt &quot;MVC-Filmapp&quot;ebenfalls.

Die vollständige  *\_Layout.vbhtml* Datei ist unten dargestellt:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Jetzt ändern wir den Titel der Seite "Index" (Ansicht).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Open *MvcMovie\Views\HelloWorld\Index.vbhtml*. Es gibt zwei Bereiche zu ändern: zuerst der Text, der angezeigt wird in der Titelleiste des Browsers, und klicken Sie dann in die sekundäre Kopfzeile (die `<h2>` Element). Wir nehmen diese geringfügig, damit Sie sehen, welche Codeabschnitt welcher Teil der app ändert.

Führen Sie die Anwendung, und navigieren Sie zu`http://localhost:xx/HelloWorld`. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. Es ist einfach, umfassende Änderungen in Ihrer Anwendung mit kleinen Änderungen an eine Ansicht zu machen. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG+F5, um das Laden der Antwort vom Server zu erzwingen.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Unsere wenig &quot;Daten&quot; (in diesem Fall die &quot;Hello World!&quot; Nachricht) ist hartcodiert, jedoch. Unsere MVC-Anwendung hat V (Ansichten) aus, und wir haben C (Controller), aber noch keine M (Modell). Kurz darauf begleiten wir erläutert, wie eine Datenbank erstellen und Modellieren von Daten daraus abrufen.

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Bevor wir mit einer Datenbank und über Modelle sprechen, jedoch zunächst sprechen wir über Informationen vom Controller an eine Ansicht übergeben. Wir möchten übergeben, was eine ansichtsvorlage benötigt, um eine HTML-Antwort an einen Client zu rendern. Diese Objekte in der Regel erstellt und als eine Controller-Klasse an eine ansichtsvorlage übergeben, und sie sollten nur die Daten, die die ansichtsvorlage benötigt enthalten, nicht mehr.

Zuvor mit der `HelloWorldController` -Klasse, die `Welcome` Aktionsmethode hat eine `name` und `numTimes` -Parameter, und klicken Sie dann die Parameterwerte an den Browser Ausgabe. Anstatt werden als den Controller diese Antwort direkt rendern weiterhin vorhanden sind, lassen Sie uns stattdessen diese Daten in einem Behälter für die Ansicht wir. Controller und Ansichten können eine `ViewBag` Objekt zum Speichern dieser Daten. Automatisch an eine ansichtsvorlage übergeben, und werden zum Rendern der HTML-Antwort unter Verwendung des Inhalts der Behälter als Daten verwendet. Auf diese Weise der Controller ist eine Sache, und die ansichtsvorlage mit einem anderen kümmern – ermöglicht uns, saubere verwalten &quot;Trennung der Belange&quot; innerhalb der Anwendung.

Alternativ konnte wir eine benutzerdefinierte Klasse definieren, klicken Sie dann erstellen Sie eine Instanz des Objekts selbst, mit Daten zu füllen und an die Ansicht übergeben. Wird häufig ein "ViewModel", bezeichnet, da es sich um ein benutzerdefiniertes Modell für die Ansicht handelt. Für kleinere Datenmengen funktioniert jedoch die ViewBag-Klasse hervorragend.

Wechseln Sie zurück zur der *HelloWorldController.vb* wurde eine dateiänderung der `Welcome` Methode innerhalb der Controller die ViewBag-Klasse die Nachricht und NumTimes abgelegt. Die ViewBag-Klasse ist ein dynamisches Objekt. Das bedeutet, dass Sie einfügen können, was auch immer sie möchten. Die ViewBag-Klasse verfügt über keine definierten Eigenschaften aus, bis Sie etwas darin einfügen.

Die vollständige `HelloWorldController.vb` mit der neuen Klasse in der gleichen Datei.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Unsere "ViewBag" enthält nun Daten, die an die Ansicht automatisch über übergeben werden. In diesem Fall können auch wir in unseren eigenen Objekt wie folgt erfolgreich abgeschlossen, wenn wir waren beeindruckt von:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Nun wir müssen eine `WelcomeView` Vorlage! Führen Sie die Anwendung aus, damit der neue Code kompiliert wird. Schließen Sie den Browser, mit der rechten Maustaste innerhalb der `Welcome` -Methode, und klicken Sie dann auf **Ansicht hinzufügen**.

Dabei handelt es sich Ihre **Ansicht hinzufügen** das Dialogfeld sieht wie folgt aus.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Fügen Sie den folgenden Code unter der `<h2>` Element in der neuen <em>Willkommen.</em> Vbhtml-Datei. Wir stellen eine Schleife und sagen &quot;Hello&quot; so oft wie der Benutzer sagt, wir sollten!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Führen Sie die Anwendung, und navigieren Sie zu `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Daten werden jetzt der URL entnommen und automatisch an den Controller übergeben. Der Controller Packt die Daten in einem `Model` Objekt und übergibt das Objekt an die Ansicht. Die Ansicht als die Daten im HTML-Format dem Benutzer angezeigt.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Nun, das eine Art wurde von einem &quot;M&quot; für Modell, jedoch nicht der Art Datenbank. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.

> [!div class="step-by-step"]
> [Zurück](adding-a-controller.md)
> [Weiter](adding-a-model.md)
