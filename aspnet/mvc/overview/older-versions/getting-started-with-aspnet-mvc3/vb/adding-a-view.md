---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: "Hinzufügen einer Ansicht (VB) | Microsoft Docs"
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 7e8564c743510780b93d56bc1215f4c5b1faeb43
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-view-vb"></a>Hinzufügen einer Ansicht (VB)
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit Quellcode VB.NET ist zu diesem Thema steht zur Verfügung. [Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie C#-bevorzugen, wechseln Sie zu der [C#-Version](../cs/adding-a-view.md) dieses Lernprogramm.


In diesem Abschnitt wir werden so ändern Sie die `HelloWorldController` für die Verwendung einer Sicht Vorlagendatei sauberes Klasse zu kapseln, den Prozess der Generierung der HTML-Antworten auf einem Client.

Beginnen wir mit einer Vorlage anzeigen, mit der `Index` Methode in der `HelloWorldController` Klasse. Derzeit die `Index` Methode gibt eine Zeichenfolge mit einer Meldung, die innerhalb der Controllerklasse hartcodiert ist. Ändern der `Index` -Methode zur Rückgabe einer `View` -Objekts, wie im folgenden gezeigt:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Nehmen wir jetzt eine Vorlage anzeigen unsere Projekt hinzugefügt, die wir aufrufen können, mit der `Index` Methode. Zu diesem Zweck Maustaste innerhalb der `Index` -Methode, und klicken Sie auf **Ansicht hinzufügen**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

Die **Ansicht hinzufügen** Dialogfeld wird angezeigt. Lassen Sie die Standardeinträge, und klicken Sie auf die **hinzufügen** Schaltfläche.

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

Die *MvcMovie\Views\HelloWorld* Ordner und die *MvcMovie\Views\HelloWorld\Index.vbhtml* -Datei erstellt werden. Sehen Sie diese in **Projektmappen-Explorer**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Hinzufügen von HTML-Code unter der `<h2>` Tag. Das geänderte *MvcMovie\Views\HelloWorld\Index.vbhtml* Datei wird unten gezeigt.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Führen Sie die Anwendung, und navigieren Sie zu der &quot;Hello World&quot; Controller (`http://localhost:xxxx/HelloWorld`). Die `Index` Methode in Ihren in der Regel nicht viel Arbeit ausführen; es einfach ausgeführt wurde, die Anweisung `return View()`, selbiges angegeben, dass wir eine Vorlagendatei Ansicht verwenden, um eine Antwort an den Client zu rendern möchten. Da wir den Namen der Vorlagendatei anzeigen, verwenden Sie nicht explizit angegeben wurde, standardmäßig ASP.NET MVC mithilfe der *Index.vbhtml* Sichtdatei innerhalb der *\Views\HelloWorld* Ordner. Die folgende Abbildung zeigt die Zeichenfolge in der Ansicht fest programmiert.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Sieht ziemlich guten. Beachten Sie jedoch, dass die Titelleiste des Browsers sagt &quot;Index&quot; und große Titel auf der Seite "sagt &quot;Meine MVC-Anwendung.&quot; Wir ändern die.

## <a name="changing-views-and-layout-pages"></a>Ändern von Ansichten und Layoutseiten

Zunächst sehen wir Ändern des Texts &quot;Meine MVC-Anwendung.&quot; Dieser Text wird freigegeben und auf jeder Seite angezeigt. Anscheinend tatsächlich nur zentral in unserer-Projekt, obwohl er auf jeder Seite in der vorliegenden Anwendung ist. Wechseln Sie zu der */Ansichten/freigegeben* Ordner **Projektmappen-Explorer** , und öffnen Sie die  *\_Layout.vbhtml* Datei. Diese Datei eine Layoutseite aufgerufen wird und es wird die freigegebene &quot;Shell&quot; , die alle anderen Seiten zu verwenden.

Beachten Sie die `@RenderBody()` Codezeile am unteren Rand der Datei. `RenderBody`ist ein Platzhalter, in dem alle Seiten, die Sie erstellen, anzeigen, &quot;umschlossen&quot; in die Seite "Layout". Ändern der `<h1>` Überschrift aus  **&quot;**  Meine MVC-Anwendung&quot; auf &quot;MVC Film-App&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Führen Sie die Anwendung, und notieren Sie sich jetzt angeblich &quot;MVC Film-App&quot;. Klicken Sie auf die **zu** Link, und dass Seite zeigt &quot;MVC Film-App&quot;ebenfalls.

Die vollständige  *\_Layout.vbhtml* Datei wird unten gezeigt:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Jetzt sehen wir ändern Sie den Titel der Seite "Index" (Ansicht).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Open *MvcMovie\Views\HelloWorld\Index.vbhtml*. Es gibt zwei Bereiche zu ändern: zuerst den Text, der angezeigt wird in der Titelleiste des Browsers, und klicken Sie dann in den sekundären Header (der `<h2>` Element). Wir treffen sie geringfügig, damit Sie sehen, welche Bit des Codes, welcher Teil der app ändert.

Führen Sie die Anwendung, und navigieren Sie zu`http://localhost:xx/HelloWorld`. Sie sehen, dass sich der Browsertitel, die primäre Überschrift und die sekundären Überschriften geändert haben. Es ist ganz einfach große Änderungen in Ihrer Anwendung mit kleinen Änderungen in eine Ansicht. (Wenn die Änderungen nicht im Browser angezeigt werden, zeigen Sie ggf. zwischengespeicherten Inhalt an. Drücken Sie in Ihrem Browser STRG+F5, um das Laden der Antwort vom Server zu erzwingen.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Unsere wenig &quot;Daten&quot; (in diesem Fall die &quot;Hello World!&quot; Nachricht) ist hartcodiert, obwohl. Unsere MVC-Anwendung hat V (Views), und wir haben C (Domänencontroller), aber noch keine M (Modell). In Kürze, wir wird exemplarisch erklärt, wie eine Datenbank erstellen und das Modelldaten daraus abrufen.

## <a name="passing-data-from-the-controller-to-the-view"></a>Übergeben von Daten vom Controller an die Ansicht

Bevor wir mit einer Datenbank aufrufen und erörtern, Modelle, jedoch, zunächst sprechen wir über Informationen auf dem Controller an eine Ansicht übergeben. Wir möchten übergeben, was eine Ansichtenvorlage ist erforderlich, um eine HTML-Antwort an einen Client zu rendern. Diese Objekte werden in der Regel erstellt und von einer Controllerklasse an eine Ansichtenvorlage übergeben und sie dürfen nur die Daten, die die Vorlage anzeigen erforderlich sind – und nicht mehr.

Zuvor mit der `HelloWorldController` -Klasse, die `Welcome` Aktionsmethode gedauert hat eine `name` und ein `numTimes` Parameter und klicken Sie dann Ausgabe, die die Parameterwerte für den Browser. Stattdessen werden als durch den Controller weiterhin direkt rendern, diese Antwort ist, wir stattdessen wir uns konzentrieren diese Daten in einer Sammlung für die Ansicht. Controller und Ansichten können eine `ViewBag` Objekt zum Speichern dieser Daten. Automatisch an eine Ansichtenvorlage übergeben, und werden zum Rendern der HTML-Antwort unter Verwendung des Inhalts der Behälter als Daten verwendet. Auf diese Weise der Controller hinsichtlich aufpassen und die Ansichtenvorlage mit einem anderen Bedenken hat – aktivieren uns bereinigen verwalten &quot;Trennung von Anliegen&quot; innerhalb der Anwendung.

Alternativ können Sie konnte wir eine benutzerdefinierte Klasse definieren, dann erstellen Sie eine Instanz dieses Objekts auf unseren eigenen, mit Daten gefüllt und übergibt ihn dann an die Ansicht. Ist häufig ein ViewModel aufgerufen, da es sich um eine benutzerdefinierte Modell für die Ansicht handelt. Für kleine Mengen von Daten funktioniert jedoch ViewBag hervorragend.

Zurück zu den *HelloWorldController.vb* Änderung der Datei die `Welcome` Methode innerhalb der Controller auf die Nachricht und NumTimes ViewBag abgelegt. Das ViewBag-Objekt ist ein dynamisches Objekt ist. Das bedeutet, dass Sie ihm beliebig platzieren können. Das ViewBag-Objekt verfügt über keine definierten Eigenschaften aus, bis Sie etwas darin einfügen.

Die vollständige `HelloWorldController.vb` mit der neuen Klasse in der gleichen Datei.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Unsere ViewBag enthält nun Daten, die auf die Ansicht automatisch über übergeben werden. Erneut konnte Alternativ wir im eigenen Objekt wie folgt erfolgreich abgeschlossen, wenn es gefallen:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Nun wir müssen ein `WelcomeView` Vorlage! Führen Sie die Anwendung aus, damit der neue Code kompiliert wird. Schließen Sie den Browser, mit der rechten Maustaste innerhalb der `Welcome` -Methode, und klicken Sie dann auf **Ansicht hinzufügen**.

Hier wird Ihre **Ansicht hinzufügen** das Dialogfeld sieht wie folgt aus.

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Fügen Sie den folgenden Code unter der `<h2>` Element in der neuen *Willkommen.* Vbhtml-Datei. Wir stellen eine Schleife und sagen &quot;Hello&quot; so häufig wie der Benutzer sagt es soll!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Führen Sie die Anwendung, und navigieren Sie zu`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Jetzt werden die Daten der URL entnommen und automatisch an den Controller übergeben. Der Controller und die Daten in Paketen ein `Model` Objekt auf und übergibt das Objekt in der Ansicht. Die Sicht als dem Benutzer die Daten als HTML angezeigt.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Nun, das eine Art wurde von einer &quot;M&quot; Modell, aber nicht die Art der Datenbank. Lassen Sie uns das Gelernte umsetzen und eine Filmdatenbank erstellen.

>[!div class="step-by-step"]
[Zurück](adding-a-controller.md)
[Weiter](adding-a-model.md)
