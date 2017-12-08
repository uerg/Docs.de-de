---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Zugriff auf das Modell Daten aus einem Controller | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b60913cef4b62745cf167e6074834bf7d0c228d1
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2017
---
<a name="accessing-your-models-data-from-a-controller"></a>Zugriff auf das Modell Daten aus einem Controller
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

In diesem Abschnitt erstellen Sie ein neues `MoviesController` Klasse, und Schreiben von Code, ruft die Filmdaten ab und zeigt ihn im Browser mit einer Vorlage anzeigen.

**Erstellen Sie die Anwendung** bevor Sie mit dem nächsten Schritt fortfahren. Wenn Sie die Anwendung nicht erstellen, erhalten Sie Fehler beim Hinzufügen eines Controllers.

Klicken Sie im Projektmappen-Explorer mit der Maustaste die *Controller* Ordner, und klicken Sie dann auf **hinzufügen**, klicken Sie dann **Controller**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

In der **Gerüst hinzufügen** (Dialogfeld), klicken Sie auf **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework**, und klicken Sie dann auf **hinzufügen**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Wählen Sie **Film (MvcMovie.Models)** für die Modell-Klasse.
- Wählen Sie **MovieDBContext (MvcMovie.Models)** für die Daten Context-Klasse.
- Geben Sie für den Controllernamen **MoviesController**.

 Die folgende Abbildung zeigt das Dialogfeld "Abgeschlossene".  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Klicken Sie auf **Hinzufügen**. (Wenn Sie eine Fehlermeldung erhalten, haben nicht Sie wahrscheinlich die Anwendung vor dem Starten den Controller hinzufügen erstellen.) Visual Studio erstellt die folgenden Dateien und Ordner:

- *Eine MoviesController.cs* in der Datei die *Controller* Ordner.
- Ein *Views\Movies* Ordner.
- *Create.cshtml Delete.cshtml, Details.cshtml, Edit.cshtml*, und *Index.cshtml* in der neuen *Views\Movies* Ordner.

Visual Studio erstellt automatisch die [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (erstellen, lesen, aktualisieren und löschen) Aktionsmethoden und Ansichten für Sie (als Gerüstbau wird die automatische Erstellung von CRUD-Aktionsmethoden und Ansichten bezeichnet). Sie verfügen jetzt über eine voll funktionsfähige Webanwendung, mit dem Sie erstellen, auflisten, bearbeiten und löschen Film-Einträge.

Führen Sie die Anwendung, und klicken Sie auf die **MVC Film** Link (oder navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in der Adressleiste des Browsers). Da die Anwendung auf das Standardrouting der vertrauenden Seite ist (definiert der *App\_Start\RouteConfig.cs* Datei), die Browseranforderung `http://localhost:xxxxx/Movies` an Standardeinstellung weitergeleitet `Index` Aktionsmethode, die von der `Movies` Controller. Das heißt, die Browseranforderung `http://localhost:xxxxx/Movies` entspricht effektiv der Browseranforderung `http://localhost:xxxxx/Movies/Index`. Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Erstellen einen Film

Klicken Sie auf den Link **Neu erstellen**. Geben Sie einige Details über einen Film, und klicken Sie dann auf die **erstellen** Schaltfläche.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Sie können im Feld "Preis" Dezimaltrennzeichen oder Kommas eingeben möglicherweise nicht. Darin jQuery-Validierung bei nicht englischen Gebietsschemas zu unterstützen, verwenden ein Komma (&quot;,&quot;) für ein Dezimaltrennzeichen und einem nicht US-englischen Datums-und Uhrzeitformate, enthalten Sie *globalize.js* und Ihre spezifischen  *Cultures/globalize.Cultures.js* Datei (aus [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) und JavaScript verwenden `Globalize.parseFloat`. Ich zeige wie dies in den nächsten Lernprogrammen ausgeführt werden. Geben Sie einstweilen ganze Zahlen wie 10 ein.


Klicken auf die **erstellen** Schaltfläche bewirkt, dass das Formular an den Server zurückgesendet werden, in dem die Informationen in der Datenbank gespeichert. Sie sind dann umgeleitet, um die */Movies* URL, wo Sie in die Auflistung den neu erstellten Film finden können.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Erstellen Sie ein paar weitere Filmeinträge. Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.

## <a name="examining-the-generated-code"></a>Überprüfen des generierten Codes

Öffnen der *Controllers\MoviesController.cs* Datei, und überprüfen Sie die generierte `Index` Methode. Ein Teil der Film-Controller mit dem `Index` Methode wird unten gezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Eine Anforderung an die `Movies` Controller gibt alle Einträge in der `Movies` -Tabelle und übergibt dann die Ergebnisse in der `Index` anzeigen. Die folgende Zeile aus der `MoviesController` Klasse instanziiert einen Film-Datenbankkontext aus, wie zuvor beschrieben. Den Datenbankkontext Film können Sie Abfragen, bearbeiten und Löschen von Filmen.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Stark typisierte Modelle und die @model Schlüsselwort

Weiter oben in diesem Lernprogramm Sie gesehen haben wie ein Controller Daten oder Objekte in eine Ansicht Vorlage übergeben kann die `ViewBag` Objekt. Die `ViewBag` ist ein dynamisches Objekt, das eine spät gebundene auf bequeme Weise Informationen an eine Ansicht übergeben werden.

MVC bietet auch die Möglichkeit, übergeben *stark* typisierte Objekte einer Vorlage anzeigen. Diese stark typisierte Ansatz ermöglicht eine bessere Kompilierung des Codes überprüfen und umfangreichere [IntelliSense](https://msdn.microsoft.com/en-us/library/hcw1s69b(v=vs.120).aspx) in Visual Studio-Editor. Der Gerüstbau in Visual Studio verwendet diesen Ansatz (d. h. übergeben einer *stark* typisierten Modell) mit der `MoviesController` Klasse, und zeigen Vorlagen bei der Erstellung der Methoden und Ansichten.

In der *Controllers\MoviesController.cs* untersuchen Sie die generierte Datei `Details` Methode. Die `Details` Methode wird unten gezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Die `id` Parameter ist im Allgemeinen übergebene als Weiterleitung von Daten, z. B. `http://localhost:1234/movies/details/1` wird den Controller mit dem Film-Controller, die Aktion, die festgelegt `details` und `id` auf 1. Sie könnten die Id mit einer Abfragezeichenfolge auch wie folgt übergeben:

`http://localhost:1234/movies/details?id=1`

Wenn eine `Movie` gefunden wird, wird eine Instanz von der `Movie` Modell wird zum Übergeben der `Details` anzeigen:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Untersuchen des Inhalts der *Views\Movies\Details.cshtml* Datei:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Durch Einschließen einer `@model` -Anweisung am Anfang der Vorlagendatei anzeigen, können Sie den Typ des Objekts, das die Sicht erwartet angeben. Beim Erstellen des „Movies“-Controllers hat Visual Studio automatisch die folgende `@model`-Anweisung am Anfang der Datei *Details.cshtml* hinzugefügt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Diese `@model`-Direktive ermöglicht Ihnen den Zugriff auf den Film, den der Controller an die Ansicht übergeben hat, indem ein stark typisiertes `Model`-Objekt verwendet wir. Beispielsweise ist in der *Details.cshtml* Vorlage, die Code übergibt die jeweiligen Film-Feld können Sie die `DisplayNameFor` und [DisplayFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML-Hilfsmethoden mit stark typisierten `Model` Objekt. Die `Create` und `Edit` Methoden und Ansichtsvorlagen auch Film Model-Objekts übergeben.

Überprüfen Sie die *Index.cshtml* Vorlage anzeigen und die `Index` Methode in der *MoviesController.cs* Datei. Beachten Sie, wie der Code erstellt ein [ `List` ](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) Objekt beim Aufrufen der `View` Hilfsmethode in der `Index` Aktionsmethode. Der Code übergibt dann diese `Movies` aus Liste der `Index` Aktionsmethode zur Ansicht:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Beim Erstellen des Controllers Film enthalten Visual Studio automatisch die folgenden `@model` Anweisung am Anfang der *Index.cshtml* Datei:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Dies `@model` Richtlinie können Sie die Liste von Filmen zuzugreifen, die mithilfe der Controller auf die Ansicht durch Übergeben einer `Model` -Objekt, das stark typisiert ist. Beispielsweise ist in der *Index.cshtml* Vorlage, der Code durchläuft die Filme durch praktische eine `foreach` Anweisung über die stark typisierte `Model` Objekt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Da die `Model` Objekt stark typisiert ist (als ein `IEnumerable<Movie>` Objekt), die jeweils `item` als Objekt in der Schleife typisiert ist `Movie`. U. a. bedeutet dies, dass Sie erhalten die kompilierzeitüberprüfung des Codes und vollständige IntelliSense-Unterstützung im Code-Editor:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Arbeiten mit SQL Server LocalDB

Entity Framework Code First wurde festgestellt, dass die Datenbank-Verbindungszeichenfolge, die bereitgestellt wurden, zeigt eine `Movies` Datenbank, die noch nicht vorhanden ist, sodass Code First die Datenbank automatisch erstellt. Sie können überprüfen, dass es durch die Überprüfung erstellt wird die *App\_Daten* Ordner. Wenn Sie sehen die *Movies.mdf* Datei, klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** -Symbolleiste klicken Sie auf die **aktualisieren** Schaltfläche, und erweitern Sie dann die *App\_Daten* Ordner.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Doppelklicken Sie auf *Movies.mdf* öffnen **SERVER-EXPLORER**, erweitern Sie dann die **Tabellen** Ordner Filme anzuzeigen. Beachten Sie das Schlüsselsymbol neben ID. Standardmäßig stellen EF eine Eigenschaft namens-ID der primären Schlüssel. Weitere Informationen zu EF und MVC, finden Sie unter Tom Dykstras ausgezeichnete Lernprogramm auf [MVC und EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendaten anzeigen** die Daten sehen Sie erstellt haben.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendefinition öffnen** , finden in der Tabelle, die Struktur dieser Entity Framework Code First für Sie erstellt.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Beachten Sie, dass wie das Schema der `Movies` Tabelle zugeordnet, die `Movie` Klasse, die Sie zuvor erstellt haben. Entity Framework Code First automatisch erstellt, dieses Schema für Sie auf der Grundlage Ihrer `Movie` Klasse.

Wenn Sie fertig sind, schließen Sie die Verbindung, indem Sie mit der rechten Maustaste auf *MovieDBContext* auswählen und **Verbindung schließen**. (Wenn Sie die Verbindung nicht schließen, erhalten Sie möglicherweise einen Fehler das nächste Mal des Projekts ausführen).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Sie verfügen jetzt über eine Datenbank und Seiten zum Anzeigen, Bearbeiten, Aktualisieren und Löschen von Daten. In den nächsten Lernprogrammen wir untersuchen Sie den Rest des Codes scaffolded und Hinzufügen einer `SearchIndex` Methode und eine `SearchIndex` anzeigen, die für Filme in dieser Datenbank durchsuchen kann. Weitere Informationen zur Verwendung von Entity Framework mit MVC finden Sie unter [Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

>[!div class="step-by-step"]
[Zurück](creating-a-connection-string.md)
[Weiter](examining-the-edit-methods-and-edit-view.md)
