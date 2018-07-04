---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: Zugreifen auf Modelldaten anhand eines Controllers (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 9984e8dbe4485093dd0061895bc4308574f3aaea
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380868"
---
<a name="accessing-your-models-data-from-a-controller-c"></a>Zugreifen auf Modelldaten anhand eines Controllers (c#)
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

In diesem Abschnitt erstellen Sie ein neues `MoviesController` Klasse, und Schreiben von Code, das die Movie-Daten abgerufen und im Browser eine ansichtsvorlage anzeigt. Achten Sie darauf, dass zum Erstellen der Anwendung, bevor Sie fortfahren.

Mit der rechten Maustaste die *Controller* Ordner, und erstellen Sie ein neues `MoviesController` Controller. Wählen Sie die folgenden Optionen:

- Controllername: **MoviesController**. (Dies ist die Standardeinstellung. )
- Vorlage: **Controller mit Lese-/schreibaktionen und Ansichten per Entity Framework**.
- Modellklasse: **Movie (MvcMovie.Models)**.
- Datenkontextklasse: **MovieDBContext (MvcMovie.Models)**.
- Ansichten: **Razor (CSHTML)**. (Die Standardeinstellung.)

[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

Klicken Sie auf **Hinzufügen**. Die folgenden Dateien und Ordner, erstellt Visual Web Developer:

- *Eine "moviescontroller.cs"* Datei des Projekts *Controller* Ordner.
- Ein *Filme* Ordner des Projekts *Ansichten* Ordner.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, und *"Index.cshtml"* in der neuen *ansichten\filme* Ordner.

[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)

Der ASP.NET MVC 3-Gerüstbau automatisch erstellt, die CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) Aktionsmethoden und Ansichten für Sie. Sie verfügen nun über eine voll funktionsfähige Webanwendung, mit dem Sie das Erstellen, auflisten, bearbeiten und löschen filmeinträge.

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in die Adressleiste Ihres Browsers. Da die Anwendung auf das Standardrouting der vertrauenden Seite ist (definiert der *"Global.asax"* Datei), der die Browseranforderung `http://localhost:xxxxx/Movies` weitergeleitet wird, auf den Standardwert `Index` Action-Methode der der `Movies` Controller. Das heißt, der die Browseranforderung `http://localhost:xxxxx/Movies` ist identisch mit die Browseranforderung `http://localhost:xxxxx/Movies/Index`. Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Erstellen einen Film

Klicken Sie auf den Link **Neu erstellen**. Geben Sie einige Details zu einem Film, und klicken Sie dann auf die **erstellen** Schaltfläche.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Klicken auf die **erstellen** -Schaltfläche bewirkt, dass das Formular an den Server gesendet werden, in dem die Filminformationen in der Datenbank gespeichert. Sie werden dann auf umgeleitet, die */Movies* URL hier Sie den neu erstellten Film in der Auflistung sehen.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Erstellen Sie ein paar weitere Filmeinträge. Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.

## <a name="examining-the-generated-code"></a>Überprüfen des generierten Codes

Öffnen der *Controllers\MoviesController.cs* Datei, und untersuchen Sie die generierte `Index` Methode. Ein Teil der Movie-Controller mit dem `Index` Methode wird unten angezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Die folgende Zeile aus der `MoviesController` Klasse eine filmdatenbankkontext instanziiert, wie zuvor beschrieben. Die filmdatenbankkontext können Sie Abfragen, bearbeiten und Löschen von Filmen.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Eine Anforderung an die `Movies` Controller gibt alle Einträge in der `Movies` Tabelle mit der filmdatenbank und übergibt dann die Ergebnisse an die `Index` anzeigen.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Stark typisierte Modelle und die @model Schlüsselwort

Weiter oben in diesem Tutorial haben Sie gesehen haben wie ein Controller Daten oder Objekte in eine Ansicht Vorlage übergeben kann, die `ViewBag` Objekt. Die `ViewBag` ist ein dynamisches Objekt, das spät gebundene bequem übergeben Informationen an eine Ansicht bereitstellt.

ASP.NET MVC bietet außerdem die Möglichkeit, stark übergeben Daten oder Objekte, um eine ansichtsvorlage eingegeben. Stark typisierte dieser Ansatz ermöglicht eine bessere Überprüfungen zur Kompilierzeit von Ihrem Code und umfangreichere IntelliSense im Visual Web Developer-Editor. Wir verwenden diesen Ansatz mit der `MoviesController` Klasse und *"Index.cshtml"* Vorlage anzeigen.

Beachten Sie, wie der Code erstellt eine [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) Objekt beim Aufrufen der `View` -Hilfsmethode in den `Index` Aktionsmethode. Der Code übergibt diese dann `Movies` Liste vom Controller an die Ansicht:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Durch Einschließen einer `@model` -Anweisung am Anfang der Ansichtsdatei für die Vorlage können Sie den Typ des Objekts, das die Ansicht erwartet angeben. Bei der Erstellung des Movie-Controllers enthalten Visual Web Developer automatisch die folgenden `@model` Anweisung am Anfang der *"Index.cshtml"* Datei:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Dies `@model` -Direktive ermöglicht Ihnen Zugriff auf die Liste von Filmen, die mithilfe der Controller an die Ansicht übergeben eine `Model` -Objekt, das stark typisiert ist. Z. B. in der *"Index.cshtml"* Vorlage, der Code durchläuft die Filme durch praktische Übungen einen `foreach` Anweisung für das stark typisierte `Model` Objekt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Da die `Model` -Objekt stark typisiert ist (als ein `IEnumerable<Movie>` Objekt), die jeweils `item` Objekt in der Schleife wird als eingegeben `Movie`. Neben anderen Vorteilen bedeutet dies, dass Sie während der Kompilierung des Codes überprüfen und vollständige IntelliSense-Unterstützung im Code-Editor:

[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Arbeiten mit SQL Server Compact

Entity Framework Code First hat festgestellt, dass es sich bei die Datenbank-Verbindungszeichenfolge, die bereitgestellt wurden, zeigt eine `Movies` -Datenbank, die noch nicht vorhanden ist, sodass Code First die Datenbank automatisch erstellt. Sie können überprüfen, ob dieser erstellt wurde anhand der *App\_Daten* Ordner. Wenn Sie nicht angezeigt wird der *Movies.sdf* , klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** -Symbolleiste klicken Sie auf die **aktualisieren** Schaltfläche, und erweitern Sie dann die *App\_Daten* Ordner.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Doppelklicken Sie auf *Movies.sdf* öffnen **Server-Explorer**. Erweitern Sie dann die **Tabellen** Ordner, um die Tabellen anzuzeigen, die in der Datenbank erstellt wurden.

> [!NOTE]
> Wenn Sie eine Fehlermeldung erhalten, wenn Sie einen Doppelklick auf *Movies.sdf*, stellen Sie sicher, dass Sie installiert haben [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen). (Links zu der Software, finden Sie in der Liste der erforderlichen Komponenten in Teil 1 dieser tutorialreihe.) Wenn Sie die Version jetzt installieren, müssen Sie schließen und öffnen Sie Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

Es gibt zwei Tabellen: eine für die `Movie` Entitätenmenge und dann die `EdmMetadata` Tabelle. Die `EdmMetadata` Tabelle wird vom Entity Framework verwendet, um zu bestimmen, wenn das Modell und die Datenbank nicht mehr synchron sind.

Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendaten anzeigen** zu ermitteln, die Daten erstellt wurde.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)

Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellenschema bearbeiten**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Beachten Sie, dass wie das Schema der `Movies` Tabelle zugeordnet, die `Movie` Klasse, die Sie zuvor erstellt haben. Entity Framework Code First automatisch erstellt, diesem Schema basierend auf Ihrer `Movie` Klasse.

Wenn Sie fertig sind, schließen Sie die Verbindung. (Wenn Sie die Verbindung nicht schließen, Sie erhalten möglicherweise einen Fehler beim nächsten des Projekts ausführen).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

Sie haben jetzt die Datenbank und einer einfachen Angebotsseite, Inhalte daraus anzuzeigen. Im nächsten Tutorial, wir die restlichen aus der eingerüstete Code überprüfen und Hinzufügen einer `SearchIndex` Methode und eine `SearchIndex` an, die für Filme in dieser Datenbank durchsuchen kann.

> [!div class="step-by-step"]
> [Zurück](adding-a-model.md)
> [Weiter](examining-the-edit-methods-and-edit-view.md)
