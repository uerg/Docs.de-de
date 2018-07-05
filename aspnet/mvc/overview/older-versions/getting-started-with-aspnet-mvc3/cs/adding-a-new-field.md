---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Hinzufügen eines neuen Felds zum Modell "Movie" und Tabelle (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 91b02f9991b714f8da2aa736c9ba5e58a7228350
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829071"
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Hinzufügen eines neuen Felds zum Modell "Movie" und Tabelle (c#)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

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


In diesem Abschnitt werden Sie die ViewModel-Klassen ein paar Änderungen vornehmen und erfahren Sie, wie Sie das Datenbankschema entsprechend den Änderungen des Datenmodells aktualisieren können.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Hinzufügen einer Rating-Eigenschaft zum Movie-Modell

Starten Sie durch Hinzufügen eines neuen `Rating` Eigenschaft, um die vorhandene `Movie` Klasse. Öffnen der *Movie.cs* -Datei und fügen die `Rating` Eigenschaft dieser Art:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Die vollständige `Movie` -Klasse sieht wie im folgenden Code:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Kompilieren der Anwendung mithilfe der **Debuggen** &gt; **erstellen Film** Menübefehl.

Nun, dass Sie aktualisiert haben die `Model` -Klasse, Sie müssen auch aktualisieren die *\Views\Movies\Index.cshtml* und *\Views\Movies\Create.cshtml* Vorlagen anzeigen, um die neue unterstützen`Rating`Eigenschaft.

Öffnen der *\Views\Movies\Index.cshtml* -Datei und fügen eine `<th>Rating</th>` Spaltenüberschrift direkt nach der **Preis** Spalte. Fügen Sie dann eine `<td>` Spalte am Ende der Vorlage zum Rendern der `@item.Rating` Wert. Im folgenden finden Sie welche die aktualisierte *"Index.cshtml"* ansichtsvorlage sieht wie folgt aus:

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

Öffnen Sie als Nächstes die *\Views\Movies\Create.cshtml* Datei, und fügen Sie das folgende Markup in der Nähe des Formulars. Dies rendert ein Textfeld, sodass Sie eine Bewertung angeben können, wenn Sie ein neuer Film erstellt wird.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Verwalten von Modell- und Schemaunterschiede Datenbank

Sie haben jetzt den Anwendungscode so, dass die Unterstützung der neuen `Rating` Eigenschaft.

Die Anwendung jetzt ausführen, und navigieren Sie zu der */Movies* URL. Wenn Sie dies tun, allerdings werden Sie den folgenden Fehler angezeigt:

![](adding-a-new-field/_static/image1.png)

Sie sehen diesen Fehler, da die aktualisierte `Movie` Modellklasse, die in der Anwendung unterscheidet sich von dem Schema der der `Movie` Tabelle der vorhandenen Datenbank. (Die Datenbanktabelle enthält nicht die Spalte `Rating`.)

In der Standardeinstellung, wenn Sie Entity Framework Code First verwenden, um automatisch eine Datenbank erstellen, wie weiter oben in diesem Tutorial Code First eine Tabelle der Datenbank hinzugefügt, um nachzuverfolgen, ob das Schema der Datenbank mit den Modellklassen synchron ist, die sie aus generiert wurde. Wenn sie nicht synchron sind, löst Entity Framework einen Fehler aus. Dies erleichtert es, die zum Zeitpunkt der Entwicklung Identifizieren von Problemen, die andernfalls nur (durch ungewöhnliche Fehler) zur Laufzeit auftreten. Das Sync-Überprüfung-Feature ist, wodurch die Fehlermeldung, die angezeigt wird, die Sie gerade gesehen haben.

Es gibt zwei Ansätze zum Beheben des Fehlers:

1. Lassen Sie die Datenbank von Entity Framework automatisch löschen und basierend auf dem neuen Modellklassenschema neu erstellen. Dieser Ansatz ist sehr praktisch, bei der aktiven Entwicklung in einer Testdatenbank, da es Ihnen ermöglicht, dass das Modell und das Datenbankschema schnell gemeinsam weiterzuentwickeln. Der Nachteil ist jedoch, dass Sie in der Datenbank vorhandene Daten verloren gehen, damit Sie *nicht* dieser Ansatz in einer Produktionsdatenbank verwendet werden soll.
2. Ändern Sie das Schema der vorhandenen Datenbank explizit so, dass es mit den Modellklassen übereinstimmt. Der Vorteil dieses Ansatzes ist, dass Sie Ihre Daten behalten. Sie können diese Änderung entweder manuell oder durch Erstellen eines Änderungsskripts für die Datenbank vornehmen.

In diesem Tutorial verwenden wir den ersten Ansatz – müssen Sie die Entity Framework Code First die Datenbank automatisch neu erstellt, jedes Mal, wenn das Modell ändert.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Erstellen die Datenbank auf Änderungen des Datenmodells automatisch erneut

Aktualisieren wir die Anwendung, sodass Code First automatisch gelöscht und neu die Datenbank erstellt jedes Mal, wenn Sie das Modell für die Anwendung zu ändern.

> [!NOTE] 
> 
> **Warnung** sollten Sie diesen Ansatz, der automatisch löschen und die Datenbank neu zu erstellen, nur, wenn Sie eine Datenbank Entwicklungs- oder testumgebung verwenden, aktivieren und *nie* in einer Produktionsdatenbank, die tatsächlichen Daten enthält. Verwenden es auf einem Produktionsserver kann zu Datenverlust führen.


In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die *Modelle* Ordner **hinzufügen**, und wählen Sie dann **Klasse**.

![](adding-a-new-field/_static/image2.png)

Nennen Sie die Klasse "MovieInitializer". Update der `MovieInitializer` Klasse, um den folgenden Code enthalten:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Die `MovieInitializer` Klasse gibt an, dass die vom Modell verwendete Datenbank gelöscht und automatisch neu erstellt werden, wenn die Modellklassen jemals ändern. Der Code enthält eine `Seed` Methode, um anzugeben, einigen Standarddaten hinzuzufügende automatisch an die Datenbank eine Zeit erstellt wurde (oder neu erstellt). Dies bietet eine gute Möglichkeit, füllen die Datenbank mit Beispieldaten, ohne dass Sie manuell sie jedes Mal auffüllen, die Sie vornehmen, dass ein Modell zu ändern.

Nun, da Sie definiert haben die `MovieInitializer` -Klasse, Sie sollten, um zu verknüpfen, sodass jedes Mal, die die Anwendung ausgeführt wird, er überprüft, ob die Modellklassen aus dem Schema in der Datenbank unterschiedlich sind. Wenn dies der Fall, können Sie den Initialisierer zum Neuerstellen der Datenbank, um das Modell übereinstimmen, und füllen dann die Datenbank mit den Beispieldaten ausführen.

Öffnen der *"Global.asax"* -Datei im Stammverzeichnis der `MvcMovies` Projekt:

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

Die *"Global.asax"* Datei enthält die Klasse, die die gesamte Anwendung für das Projekt definiert und enthält eine `Application_Start` -Ereignishandler, der beim ersten der Anwendung starten ausgeführt wird.

Fügen Sie zwei using-Anweisungen am Anfang der Datei. Die erste verweist auf den Entity Framework-Namespace und die zweite verweist auf den Namespace, in dem unsere `MovieInitializer` Leben Klasse:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

Suchen Sie dann die `Application_Start` Methode, und fügen Sie einen Aufruf von `Database.SetInitializer` am Anfang der Methode, wie unten dargestellt:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

Die `Database.SetInitializer` Anweisung, die Sie gerade hinzugefügt haben gibt an, dass die Datenbank verwendet die `MovieDBContext` Instanz sollte automatisch gelöscht und neu erstellt werden, wenn das Schema und die Datenbank nicht übereinstimmen. Und wie Sie gesehen haben, füllen sie auch die Datenbank mit den Beispieldaten, die im angegebenen die `MovieInitializer` Klasse.

Schließen der *"Global.asax"* Datei.

Führen Sie die Anwendung erneut aus, und navigieren Sie zu der */Movies* URL. Wenn die Anwendung gestartet wird, erkennt er, dass es sich bei die Modellstruktur nicht mehr das Datenbankschema entspricht. Wird automatisch die Datenbank entsprechend der Struktur des neuen neu erstellt und füllt die Datenbank mit der Beispiel-Filme:

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Klicken Sie auf die **neu erstellen** Link, um einen neuen Film hinzuzufügen. Beachten Sie, dass Sie eine Bewertung hinzufügen können.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

Klicken Sie auf **Erstellen**. Der neue Film, einschließlich "Rating", wird jetzt in den Filmen auflisten:

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

In diesem Abschnitt wurde erläutert, wie Sie Modellobjekte ändern und die Datenbank mit den Änderungen synchron zu halten. Sie haben zudem eine Möglichkeit, eine neu erstellte Datenbank mit Beispieldaten aufzufüllen, damit Sie die Szenarien ausprobieren können. Als Nächstes sehen wir uns, wie Sie die Modellklassen umfangreichere Validierungslogik hinzugefügt und einige Geschäftsregeln erzwungen werden zu aktivieren.

> [!div class="step-by-step"]
> [Zurück](examining-the-edit-methods-and-edit-view.md)
> [Weiter](adding-validation-to-the-model.md)
