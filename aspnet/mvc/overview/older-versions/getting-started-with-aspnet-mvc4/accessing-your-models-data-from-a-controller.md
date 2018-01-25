---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Zugriff auf das Modell Daten aus einem Controller | Microsoft Docs
author: Rick-Anderson
description: "Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher zu verfolgen und demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f323fe37da739d957a609dc7ca4e71a3c3ab475e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Zugriff auf das Modell Daten aus einem Controller
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Lernprogramms steht [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.


In diesem Abschnitt erstellen Sie ein neues `MoviesController` Klasse, und Schreiben von Code, ruft die Filmdaten ab und zeigt ihn im Browser mit einer Vorlage anzeigen.

**Erstellen Sie die Anwendung** bevor Sie mit dem nächsten Schritt fortfahren.

Mit der rechten Maustaste die *Controller* Ordner und erstellen Sie ein neues `MoviesController` Controller. Die folgenden Optionen werden nicht angezeigt, bis Sie eine Anwendung erstellen. Wählen Sie die folgenden Optionen:

- Controllername: **MoviesController**. (Dies ist die Standardeinstellung. )
- Vorlage: **MVC-Controller mit Lese-/schreibaktionen und Ansichten, Verwendung von Entity Framework**.
- Modellschemas: **Film (MvcMovie.Models)**.
- Datenkontextklasse: **MovieDBContext (MvcMovie.Models)**.
- Ansichten: **Razor (CSHTML)**. (Die Standardeinstellung.)

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Klicken Sie auf **Hinzufügen**. Visual Studio Express erstellt die folgenden Dateien und Ordner:

- *Eine MoviesController.cs* -Datei in des Projekts *Controller* Ordner.
- Ein *Filme* Ordner des Projekts *Ansichten* Ordner.
- *Create.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, und *Index.cshtml* in der neuen *Views\Movies* Ordner.

ASP.NET MVC 4 automatisch erstellt, die CRUD-Vorgänge (erstellen, lesen, aktualisieren und löschen) Aktionsmethoden und Ansichten für Sie (als Gerüstbau wird die automatische Erstellung von CRUD-Aktionsmethoden und Ansichten bezeichnet). Sie verfügen jetzt über eine voll funktionsfähige Webanwendung, mit dem Sie erstellen, auflisten, bearbeiten und löschen Film-Einträge.

Führen Sie die Anwendung, und navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in der Adressleiste des Browsers. Da die Anwendung auf das Standardrouting der vertrauenden Seite ist (definiert der *"Global.asax"* Datei), die Browseranforderung `http://localhost:xxxxx/Movies` an Standardeinstellung weitergeleitet `Index` Aktionsmethode von der `Movies` Controller. Das heißt, die Browseranforderung `http://localhost:xxxxx/Movies` entspricht effektiv der Browseranforderung `http://localhost:xxxxx/Movies/Index`. Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Erstellen einen Film

Klicken Sie auf den Link **Neu erstellen**. Geben Sie einige Details über einen Film, und klicken Sie dann auf die **erstellen** Schaltfläche.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Klicken auf die **erstellen** Schaltfläche bewirkt, dass das Formular an den Server zurückgesendet werden, in dem die Informationen in der Datenbank gespeichert. Sie sind dann umgeleitet, um die */Movies* URL, wo Sie in die Auflistung den neu erstellten Film finden können.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Erstellen Sie ein paar weitere Filmeinträge. Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.

## <a name="examining-the-generated-code"></a>Überprüfen des generierten Codes

Öffnen der *Controllers\MoviesController.cs* Datei, und überprüfen Sie die generierte `Index` Methode. Ein Teil der Film-Controller mit dem `Index` Methode wird unten gezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Die folgende Zeile aus der `MoviesController` Klasse instanziiert einen Film-Datenbankkontext aus, wie zuvor beschrieben. Den Datenbankkontext Film können Sie Abfragen, bearbeiten und Löschen von Filmen.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Eine Anforderung an die `Movies` Controller gibt alle Einträge in der `Movies` Tabelle der Datenbank Film und übergibt dann die Ergebnisse in der `Index` anzeigen.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Stark typisierte Modelle und die @model Schlüsselwort

Weiter oben in diesem Lernprogramm Sie gesehen haben wie ein Controller Daten oder Objekte in eine Ansicht Vorlage übergeben kann die `ViewBag` Objekt. Die `ViewBag` ist ein dynamisches Objekt, das eine spät gebundene auf bequeme Weise Informationen an eine Ansicht übergeben werden.

ASP.NET MVC bietet außerdem die Möglichkeit, stark übergeben Daten oder Objekte zu einer Sicht Vorlage eingegeben. Stark typisierte dieser Ansatz ermöglicht eine bessere Kompilierung von Code und umfangreichere IntelliSense in Visual Studio-Editor. Der Gerüstbau in Visual Studio verwendet diesen Ansatz, mit der `MoviesController` Klasse, und zeigen Vorlagen bei der Erstellung der Methoden und Ansichten.

In der *Controllers\MoviesController.cs* untersuchen Sie die generierte Datei `Details` Methode. Ein Teil der Film-Controller mit dem `Details` Methode wird unten gezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Wenn eine `Movie` gefunden wird, wird eine Instanz von der `Movie` Modell an die Ansicht übergeben wird. Untersuchen des Inhalts der *Views\Movies\Details.cshtml* Datei.

Durch Einschließen einer `@model` -Anweisung am Anfang der Vorlagendatei anzeigen, können Sie den Typ des Objekts, das die Sicht erwartet angeben. Beim Erstellen des „Movies“-Controllers hat Visual Studio automatisch die folgende `@model`-Anweisung am Anfang der Datei *Details.cshtml* hinzugefügt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Diese `@model`-Direktive ermöglicht Ihnen den Zugriff auf den Film, den der Controller an die Ansicht übergeben hat, indem ein stark typisiertes `Model`-Objekt verwendet wir. Beispielsweise ist in der *Details.cshtml* Vorlage, die Code übergibt die jeweiligen Film-Feld können Sie die `DisplayNameFor` und [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML-Hilfsmethoden mit stark typisierten `Model` Objekt. Erstellen und Bearbeiten von Methoden und Vorlagen anzeigen, auch ein Modellobjekt Film übergeben.

Überprüfen Sie die *Index.cshtml* Vorlage anzeigen und die `Index` Methode in der *MoviesController.cs* Datei. Beachten Sie, wie der Code erstellt ein [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) Objekt beim Aufrufen der `View` Hilfsmethode in der `Index` Aktionsmethode. Der Code übergibt dann diese `Movies` Liste auf dem Controller aus, um die Ansicht:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Beim Erstellen des Controllers Film Visual Studio Express automatisch zählen folgende `@model` Anweisung am Anfang der *Index.cshtml* Datei:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Dies `@model` Richtlinie können Sie die Liste von Filmen zuzugreifen, die mithilfe der Controller auf die Ansicht durch Übergeben einer `Model` -Objekt, das stark typisiert ist. Beispielsweise ist in der *Index.cshtml* Vorlage, der Code durchläuft die Filme durch praktische eine `foreach` Anweisung über die stark typisierte `Model` Objekt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Da die `Model` Objekt stark typisiert ist (als ein `IEnumerable<Movie>` Objekt), die jeweils `item` als Objekt in der Schleife typisiert ist `Movie`. U. a. bedeutet dies, dass Sie erhalten die kompilierzeitüberprüfung des Codes und vollständige IntelliSense-Unterstützung im Code-Editor:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Arbeiten mit SQL Server LocalDB

Entity Framework Code First wurde festgestellt, dass die Datenbank-Verbindungszeichenfolge, die bereitgestellt wurden, zeigt eine `Movies` Datenbank, die noch nicht vorhanden ist, sodass Code First die Datenbank automatisch erstellt. Sie können überprüfen, dass es durch die Überprüfung erstellt wird die *App\_Daten* Ordner. Wenn Sie sehen die *Movies.mdf* Datei, klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** -Symbolleiste klicken Sie auf die **aktualisieren** Schaltfläche, und erweitern Sie dann die *App\_Daten* Ordner.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Doppelklicken Sie auf *Movies.mdf* öffnen **Datenbank-EXPLORER**, erweitern Sie dann die **Tabellen** Ordner Filme anzuzeigen.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Wenn die Datenbank-Explorer nicht angezeigt wird, aus der **TOOLS** klicken Sie im Menü **mit Datenbank verbinden**, klicken Sie dann "Abbrechen" die **Datenquelle auswählen** Dialogfeld. Hierdurch wird Öffnen der Datenbank-Explorer erzwungen.


> [!NOTE]
> Wenn Sie die VWD oder Visual Studio 2010 verwenden und eine Fehlermeldung ähnlich der folgenden folgenden erhalten:
> 
> - Die Datenbank "C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF-Datei ' kann nicht geöffnet werden, da es sich um Version 706 handelt. Dieser Server unterstützt Version 655 und früher. Ein Herabstufungspfad wird nicht unterstützt.
> - &quot;InvalidOperation-Ausnahme nicht behandelt wurde durch Benutzercode&quot; die bereitgestellte SqlConnection gibt keinen Anfangskatalog.
> 
> Sie müssen zum Installieren der [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) und [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Überprüfen Sie die `MovieDBContext` Verbindungszeichenfolge, die auf der vorherigen Seite angegeben.


Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendaten anzeigen** die Daten sehen Sie erstellt haben.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendefinition öffnen** , finden in der Tabelle, die Struktur dieser Entity Framework Code First für Sie erstellt.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Beachten Sie, dass wie das Schema der `Movies` Tabelle zugeordnet, die `Movie` Klasse, die Sie zuvor erstellt haben. Entity Framework Code First automatisch erstellt, dieses Schema für Sie auf der Grundlage Ihrer `Movie` Klasse.

Wenn Sie fertig sind, schließen Sie die Verbindung, indem Sie mit der rechten Maustaste auf *MovieDBContext* auswählen und **Verbindung schließen**. (Wenn Sie die Verbindung nicht schließen, erhalten Sie möglicherweise einen Fehler das nächste Mal des Projekts ausführen).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Sie haben jetzt die Datenbank und eine einfache Liste Seite daraus angezeigt werden sollen. In den nächsten Lernprogrammen wir untersuchen Sie den Rest des Codes scaffolded und Hinzufügen einer `SearchIndex` Methode und eine `SearchIndex` anzeigen, die für Filme in dieser Datenbank durchsuchen kann.

>[!div class="step-by-step"]
[Zurück](adding-a-model.md)
[Weiter](examining-the-edit-methods-and-edit-view.md)
