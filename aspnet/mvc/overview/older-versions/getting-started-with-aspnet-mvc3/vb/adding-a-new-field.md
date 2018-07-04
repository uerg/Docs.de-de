---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Hinzufügen eines neuen Felds zum Modell "Movie" und zur Datenbanktabelle (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 06d9f08ea3e1a85327083639adc6aa0f2cfbaa48
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382940"
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>Hinzufügen eines neuen Felds zum Modell "Movie" und zur Datenbanktabelle (VB)
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
> Ein Visual Web Developer-Projekt mit Quellcode VB.NET steht zur Ergänzung dieses Themas. [Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie C# -Code bevorzugen, wechseln Sie zu der [c#-Version](../cs/adding-a-new-field.md) in diesem Tutorial.


In diesem Abschnitt werden Sie die ViewModel-Klassen ein paar Änderungen vornehmen und erfahren Sie, wie Sie das Datenbankschema entsprechend den Änderungen des Datenmodells aktualisieren können.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Hinzufügen einer Rating-Eigenschaft zum Movie-Modell

Starten Sie durch Hinzufügen eines neuen `Rating` Eigenschaft, um die vorhandene `Movie` Klasse. Öffnen der *Movie.cs* -Datei und fügen die `Rating` Eigenschaft dieser Art:

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

Die vollständige `Movie` -Klasse sieht wie im folgenden Code:

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

Kompilieren der Anwendung mithilfe der **Debuggen** &gt; **erstellen Film** Menübefehl.

Nun, dass Sie aktualisiert haben die `Model` -Klasse, Sie müssen auch aktualisieren die *\Views\Movies\Index.vbhtml* und *\Views\Movies\Create.vbhtml* Vorlagen anzeigen, um die neue unterstützen`Rating`Eigenschaft.

Öffnen der<em>\Views\Movies\Index.vbhtml</em> -Datei und fügen eine `<th>Rating</th>` Spaltenüberschrift direkt nach der <strong>Preis</strong> Spalte. Fügen Sie dann eine `<td>` Spalte am Ende der Vorlage zum Rendern der `@item.Rating` Wert. Im folgenden finden Sie welche die aktualisierte <em>Index.vbhtml</em> ansichtsvorlage sieht wie folgt aus:

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

Öffnen Sie als Nächstes die *\Views\Movies\Create.vbhtml* Datei, und fügen Sie das folgende Markup in der Nähe des Formulars. Dies rendert ein Textfeld, sodass Sie eine Bewertung angeben können, wenn Sie ein neuer Film erstellt wird.

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

Nennen Sie die Klasse &quot;MovieInitializer&quot;. Update der `MovieInitializer` Klasse, um den folgenden Code enthalten:

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

Die `MovieInitializer` Klasse gibt an, dass die vom Modell verwendete Datenbank gelöscht und automatisch neu erstellt werden, wenn die Modellklassen jemals ändern. Der Code enthält eine `Seed` Methode, um anzugeben, einigen Standarddaten hinzuzufügende automatisch an die Datenbank eine Zeit erstellt wurde (oder neu erstellt). Dies bietet eine gute Möglichkeit, füllen die Datenbank mit Beispieldaten, ohne dass Sie manuell sie jedes Mal auffüllen, die Sie vornehmen, dass ein Modell zu ändern.

Nun, da Sie definiert haben die `MovieInitializer` -Klasse, Sie sollten, um zu verknüpfen, sodass jedes Mal, die die Anwendung ausgeführt wird, er überprüft, ob die Modellklassen aus dem Schema in der Datenbank unterschiedlich sind. Wenn dies der Fall, können Sie den Initialisierer zum Neuerstellen der Datenbank, um das Modell übereinstimmen, und füllen dann die Datenbank mit den Beispieldaten ausführen.

Öffnen der *"Global.asax"* -Datei im Stammverzeichnis der `MvcMovies` Projekt:

Die *"Global.asax"* Datei enthält die Klasse, die die gesamte Anwendung für das Projekt definiert und enthält eine `Application_Start` -Ereignishandler, der beim ersten der Anwendung starten ausgeführt wird.

Suchen der `Application_Start` Methode, und fügen Sie einen Aufruf von `Database.SetInitializer` am Anfang der Methode, wie unten dargestellt:

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

Die `Database.SetInitializer` Anweisung, die Sie gerade hinzugefügt haben gibt an, dass die Datenbank verwendet die `MovieDBContext` Instanz sollte automatisch gelöscht und neu erstellt werden, wenn das Schema und die Datenbank nicht übereinstimmen. Und wie Sie gesehen haben, füllen sie auch die Datenbank mit den Beispieldaten, die im angegebenen die `MovieInitializer` Klasse.

Schließen der *"Global.asax"* Datei.

Führen Sie die Anwendung erneut aus, und navigieren Sie zu der */Movies* URL. Wenn die Anwendung gestartet wird, erkennt er, dass es sich bei die Modellstruktur nicht mehr das Datenbankschema entspricht. Wird automatisch die Datenbank entsprechend der Struktur des neuen neu erstellt und füllt die Datenbank mit der Beispiel-Filme:

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

Klicken Sie auf die **neu erstellen** Link, um einen neuen Film hinzuzufügen. Beachten Sie, dass Sie eine Bewertung hinzufügen können.

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

Klicken Sie auf **Erstellen**. Der neue Film, einschließlich "Rating", wird jetzt in den Filmen auflisten:

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

In diesem Abschnitt wurde erläutert, wie Sie Modellobjekte ändern und die Datenbank mit den Änderungen synchron zu halten. Sie haben zudem eine Möglichkeit, eine neu erstellte Datenbank mit Beispieldaten aufzufüllen, damit Sie die Szenarien ausprobieren können. Als Nächstes sehen wir uns, wie Sie die Modellklassen umfangreichere Validierungslogik hinzugefügt und einige Geschäftsregeln erzwungen werden zu aktivieren.

> [!div class="step-by-step"]
> [Zurück](examining-the-edit-methods-and-edit-view.md)
> [Weiter](adding-validation-to-the-model.md)
