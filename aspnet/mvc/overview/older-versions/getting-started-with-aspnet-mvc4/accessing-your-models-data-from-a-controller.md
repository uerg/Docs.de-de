---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Zugreifen auf Modelldaten anhand eines Controllers | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Tutorials ist hier verfügbar, dass das ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist eine sicherere, viel einfacher zu folgen und demo...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: fb052b85d033f2c60f1fab6f5d5a1773aad22d35
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368611"
---
<a name="accessing-your-models-data-from-a-controller"></a>Zugreifen auf Modelldaten anhand eines Controllers
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.


In diesem Abschnitt erstellen Sie ein neues `MoviesController` Klasse, und Schreiben von Code, das die Movie-Daten abgerufen und im Browser eine ansichtsvorlage anzeigt.

**Erstellen Sie die Anwendung** bevor Sie mit dem nächsten Schritt fortfahren.

Mit der rechten Maustaste die *Controller* Ordner, und erstellen Sie ein neues `MoviesController` Controller. Die unten aufgeführten Optionen werden nicht angezeigt, bis Sie Ihre Anwendung zu erstellen. Wählen Sie die folgenden Optionen:

- Controllername: **MoviesController**. (Dies ist die Standardeinstellung. )
- Vorlage: **MVC-Controller mit Lese-/schreibaktionen und Ansichten unter Verwendung von Entity Framework**.
- Modellklasse: **Movie (MvcMovie.Models)**.
- Datenkontextklasse: **MovieDBContext (MvcMovie.Models)**.
- Ansichten: **Razor (CSHTML)**. (Die Standardeinstellung.)

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Klicken Sie auf **Hinzufügen**. Visual Studio Express erstellt die folgenden Dateien und Ordner:

- *Eine "moviescontroller.cs"* Datei des Projekts *Controller* Ordner.
- Ein *Filme* Ordner des Projekts *Ansichten* Ordner.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, und *"Index.cshtml"* in der neuen *ansichten\filme* Ordner.

ASP.NET MVC 4 automatisch erstellt, die CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) Aktionsmethoden und Ansichten für Sie (die automatische Erstellung von CRUD-Aktionsmethoden und Ansichten wird als Gerüstbau bezeichnet). Sie verfügen nun über eine voll funktionsfähige Webanwendung, mit dem Sie das Erstellen, auflisten, bearbeiten und löschen filmeinträge.

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in die Adressleiste Ihres Browsers. Da die Anwendung auf das Standardrouting der vertrauenden Seite ist (definiert der *"Global.asax"* Datei), der die Browseranforderung `http://localhost:xxxxx/Movies` weitergeleitet wird, auf den Standardwert `Index` Action-Methode der der `Movies` Controller. Das heißt, der die Browseranforderung `http://localhost:xxxxx/Movies` ist identisch mit die Browseranforderung `http://localhost:xxxxx/Movies/Index`. Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Erstellen einen Film

Klicken Sie auf den Link **Neu erstellen**. Geben Sie einige Details zu einem Film, und klicken Sie dann auf die **erstellen** Schaltfläche.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Klicken auf die **erstellen** -Schaltfläche bewirkt, dass das Formular an den Server gesendet werden, in dem die Filminformationen in der Datenbank gespeichert. Sie werden dann auf umgeleitet, die */Movies* URL hier Sie den neu erstellten Film in der Auflistung sehen.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Erstellen Sie ein paar weitere Filmeinträge. Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.

## <a name="examining-the-generated-code"></a>Überprüfen des generierten Codes

Öffnen der *Controllers\MoviesController.cs* Datei, und untersuchen Sie die generierte `Index` Methode. Ein Teil der Movie-Controller mit dem `Index` Methode wird unten angezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Die folgende Zeile aus der `MoviesController` Klasse eine filmdatenbankkontext instanziiert, wie zuvor beschrieben. Die filmdatenbankkontext können Sie Abfragen, bearbeiten und Löschen von Filmen.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Eine Anforderung an die `Movies` Controller gibt alle Einträge in der `Movies` Tabelle mit der filmdatenbank und übergibt dann die Ergebnisse an die `Index` anzeigen.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Stark typisierte Modelle und die @model Schlüsselwort

Weiter oben in diesem Tutorial haben Sie gesehen haben wie ein Controller Daten oder Objekte in eine Ansicht Vorlage übergeben kann, die `ViewBag` Objekt. Die `ViewBag` ist ein dynamisches Objekt, das spät gebundene bequem übergeben Informationen an eine Ansicht bereitstellt.

ASP.NET MVC bietet außerdem die Möglichkeit, stark übergeben Daten oder Objekte, um eine ansichtsvorlage eingegeben. Stark typisierte dieser Ansatz ermöglicht eine bessere Überprüfungen zur Kompilierzeit von Ihrem Code und umfangreichere IntelliSense in Visual Studio-Editor. Der Gerüstbau in Visual Studio verwendet diesen Ansatz mit der `MoviesController` Klasse, und zeigen-Vorlagen, wenn sie die Methoden und Ansichten erstellt.

In der *Controllers\MoviesController.cs* untersuchen Sie die generierte Datei `Details` Methode. Ein Teil der Movie-Controller mit dem `Details` Methode wird unten angezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Wenn eine `Movie` gefunden wird, wird eine Instanz von der `Movie` Modell wird übergeben, um die Detailansicht. Untersuchen des Inhalts der *Views\Movies\Details.cshtml* Datei.

Durch Einschließen einer `@model` -Anweisung am Anfang der Ansichtsdatei für die Vorlage können Sie den Typ des Objekts, das die Ansicht erwartet angeben. Beim Erstellen des „Movies“-Controllers hat Visual Studio automatisch die folgende `@model`-Anweisung am Anfang der Datei *Details.cshtml* hinzugefügt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Diese `@model`-Direktive ermöglicht Ihnen den Zugriff auf den Film, den der Controller an die Ansicht übergeben hat, indem ein stark typisiertes `Model`-Objekt verwendet wir. Z. B. in der *Details.cshtml* Vorlage übergibt der Code jedes filmfeld an die `DisplayNameFor` und ["DisplayFor"](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML-Hilfsprogrammen, mit dem stark typisierten `Model` Objekt. Die Erstellungs- und Bearbeitungsmethoden und Vorlagen anzeigen, auch ein Movie-Modell-Objekt übergeben.

Überprüfen Sie die *"Index.cshtml"* Vorlage anzeigen und die `Index` -Methode in der die *"moviescontroller.cs"* Datei. Beachten Sie, wie der Code erstellt eine [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) Objekt beim Aufrufen der `View` -Hilfsmethode in den `Index` Aktionsmethode. Der Code übergibt diese dann `Movies` Liste vom Controller an die Ansicht:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Bei der Erstellung des Movie-Controllers Visual Studio Express automatisch enthalten die folgenden `@model` Anweisung am Anfang der *"Index.cshtml"* Datei:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Dies `@model` -Direktive ermöglicht Ihnen Zugriff auf die Liste von Filmen, die mithilfe der Controller an die Ansicht übergeben eine `Model` -Objekt, das stark typisiert ist. Z. B. in der *"Index.cshtml"* Vorlage, der Code durchläuft die Filme durch praktische Übungen einen `foreach` Anweisung für das stark typisierte `Model` Objekt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Da die `Model` -Objekt stark typisiert ist (als ein `IEnumerable<Movie>` Objekt), die jeweils `item` Objekt in der Schleife wird als eingegeben `Movie`. Neben anderen Vorteilen bedeutet dies, dass Sie während der Kompilierung des Codes überprüfen und vollständige IntelliSense-Unterstützung im Code-Editor:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Arbeiten mit SQL Server LocalDB

Entity Framework Code First hat festgestellt, dass es sich bei die Datenbank-Verbindungszeichenfolge, die bereitgestellt wurden, zeigt eine `Movies` -Datenbank, die noch nicht vorhanden ist, sodass Code First die Datenbank automatisch erstellt. Sie können überprüfen, ob dieser erstellt wurde anhand der *App\_Daten* Ordner. Wenn Sie nicht angezeigt wird der *Movies.mdf* , klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** -Symbolleiste klicken Sie auf die **aktualisieren** Schaltfläche, und erweitern Sie dann die *App\_Daten* Ordner.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Doppelklicken Sie auf *Movies.mdf* öffnen **Datenbank-EXPLORER**, erweitern Sie dann die **Tabellen** Ordner finden in der Tabelle "Movies".

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Wenn die Datenbank-Explorer nicht angezeigt wird, aus der **TOOLS** , wählen Sie im Menü **mit Datenbank verbinden**, Abbrechen der **Datenquelle auswählen** Dialogfeld. Dadurch wird Öffnen der Datenbank-Explorer erzwungen.


> [!NOTE]
> Wenn Sie VWD oder Visual Studio 2010 verwenden und eine ähnlich der folgenden folgenden Fehlermeldung:
> 
> - Die Datenbank "C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF-Datei ' kann nicht geöffnet werden, da es sich um Version 706 handelt. Dieser Server unterstützt Version 655 und früher. Ein Herabstufungspfad wird nicht unterstützt.
> - &quot;InvalidOperation-Ausnahme wurde nicht vom Benutzercode behandelt&quot; die bereitgestellte SqlConnection gibt kein Anfangskatalogs anmelden.
> 
> Sie müssen zum Installieren der [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) und [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Überprüfen Sie die `MovieDBContext` Verbindungszeichenfolge, die auf der vorherigen Seite angegeben.


Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendaten anzeigen** zu ermitteln, die Daten erstellt wurde.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendefinition öffnen** um die Tabelle, Entity Framework Code First für Sie erstellten Struktur anzuzeigen.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Beachten Sie, dass wie das Schema der `Movies` Tabelle zugeordnet, die `Movie` Klasse, die Sie zuvor erstellt haben. Entity Framework Code First automatisch erstellt, diesem Schema basierend auf Ihrer `Movie` Klasse.

Wenn Sie fertig sind, schließen Sie die Verbindung mit der rechten Maustaste *MovieDBContext* und **Verbindung schließen**. (Wenn Sie die Verbindung nicht schließen, Sie erhalten möglicherweise einen Fehler beim nächsten des Projekts ausführen).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Sie haben jetzt die Datenbank und einer einfachen Angebotsseite, Inhalte daraus anzuzeigen. Im nächsten Tutorial, wir die restlichen aus der eingerüstete Code überprüfen und Hinzufügen einer `SearchIndex` Methode und eine `SearchIndex` an, die für Filme in dieser Datenbank durchsuchen kann.

> [!div class="step-by-step"]
> [Zurück](adding-a-model.md)
> [Weiter](examining-the-edit-methods-and-edit-view.md)
