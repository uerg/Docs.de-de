---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Zugreifen auf Modelldaten anhand eines Controllers | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 8d359de17bc35d0e14047ccd0589f080e0f949fc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396111"
---
<a name="accessing-your-models-data-from-a-controller"></a>Zugreifen auf Modelldaten anhand eines Controllers
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

In diesem Abschnitt erstellen Sie ein neues `MoviesController` Klasse, und Schreiben von Code, das die Movie-Daten abgerufen und im Browser eine ansichtsvorlage anzeigt.

**Erstellen Sie die Anwendung** bevor Sie mit dem nächsten Schritt fortfahren. Wenn Sie die Anwendung nicht erstellen, erhalten Sie einen Fehler mit dem Hinzufügen eines Controllers.

Klicken Sie im Projektmappen-Explorer mit der Maustaste der *Controller* Ordner, und klicken Sie dann auf **hinzufügen**, klicken Sie dann **Controller**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

In der **Gerüst hinzufügen** Dialogfeld klicken Sie auf **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework**, und klicken Sie dann auf **hinzufügen**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Wählen Sie **Movie (MvcMovie.Models)** für die Model-Klasse.
- Wählen Sie **MovieDBContext (MvcMovie.Models)** für das die Datenkontextklasse.
- Geben Sie für den Controllernamen **MoviesController**.

  Die folgende Abbildung zeigt das Dialogfeld "Abgeschlossene".  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Klicken Sie auf **Hinzufügen**. (Wenn Sie eine Fehlermeldung erhalten, nicht haben Sie wahrscheinlich die Anwendung vor dem Starten den Controller hinzufügen erstellen.) Visual Studio erstellt die folgenden Dateien und Ordner:

- *Eine "moviescontroller.cs"* Datei die *Controller* Ordner.
- Ein *ansichten\filme* Ordner.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, und *"Index.cshtml"* in der neuen *ansichten\filme* Ordner.

Visual Studio automatisch erstellt, die [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (erstellen, lesen, aktualisieren und löschen) Aktionsmethoden und Ansichten für Sie (die automatische Erstellung von CRUD-Aktionsmethoden und Ansichten wird als Gerüstbau bezeichnet). Sie verfügen nun über eine voll funktionsfähige Webanwendung, mit dem Sie das Erstellen, auflisten, bearbeiten und löschen filmeinträge.

Führen Sie die Anwendung, und klicken Sie auf die **MVC Movie** Link (oder navigieren Sie zu der `Movies` Controller durch Anfügen */Movies* an die URL in die Adressleiste des Browsers). Da die Anwendung auf das Standardrouting der vertrauenden Seite ist (in definiert die *App\_start\routeconfig* Datei), der Browseranforderung `http://localhost:xxxxx/Movies` weitergeleitet wird, auf den Standardwert `Index` Action-Methode der der `Movies` Controller. Das heißt, der die Browseranforderung `http://localhost:xxxxx/Movies` ist identisch mit die Browseranforderung `http://localhost:xxxxx/Movies/Index`. Das Ergebnis ist eine leere Liste von Filmen, da Sie noch keine hinzugefügt haben.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Erstellen einen Film

Klicken Sie auf den Link **Neu erstellen**. Geben Sie einige Details zu einem Film, und klicken Sie dann auf die **erstellen** Schaltfläche.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Sie können im Feld für den Preis Dezimaltrennzeichen oder Kommas eingeben möglicherweise nicht. zur Unterstützung von jQuery-Validierung für nicht englische Gebietsschemas, in denen ein Komma (&quot;,&quot;) für ein Dezimaltrennzeichen und einem nicht-US-englischen Datums-und Uhrzeitformate, müssen Sie enthalten *globalize.js* und Ihren speziellen  *Cultures/globalize.Cultures.js* Datei (aus [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) und JavaScript verwenden `Globalize.parseFloat`. Ich zeige, wie Sie dazu im nächsten Tutorial. Geben Sie einstweilen ganze Zahlen wie 10 ein.


Klicken auf die **erstellen** -Schaltfläche bewirkt, dass das Formular an den Server gesendet werden, in dem die Filminformationen in der Datenbank gespeichert. Sie werden dann auf umgeleitet, die */Movies* URL hier Sie den neu erstellten Film in der Auflistung sehen.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Erstellen Sie ein paar weitere Filmeinträge. Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen), die alle funktionsbereit sind.

## <a name="examining-the-generated-code"></a>Überprüfen des generierten Codes

Öffnen der *Controllers\MoviesController.cs* Datei, und untersuchen Sie die generierte `Index` Methode. Ein Teil der Movie-Controller mit dem `Index` Methode wird unten angezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Eine Anforderung an die `Movies` Controller gibt alle Einträge in der `Movies` -Tabelle und übergibt dann die Ergebnisse an die `Index` anzeigen. Die folgende Zeile aus der `MoviesController` Klasse eine filmdatenbankkontext instanziiert, wie zuvor beschrieben. Die filmdatenbankkontext können Sie Abfragen, bearbeiten und Löschen von Filmen.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Stark typisierte Modelle und die @model Schlüsselwort

Weiter oben in diesem Tutorial haben Sie gesehen haben wie ein Controller Daten oder Objekte in eine Ansicht Vorlage übergeben kann, die `ViewBag` Objekt. Die `ViewBag` ist ein dynamisches Objekt, das spät gebundene bequem übergeben Informationen an eine Ansicht bereitstellt.

MVC bietet außerdem die Möglichkeit, übergeben Sie *stark* typisierte Objekte eine ansichtsvorlage. Dieser stark typisierte Ansatz ermöglicht eine bessere während der Kompilierung des Codes überprüfen und umfassendere [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in Visual Studio-Editor. Der Gerüstbau in Visual Studio verwendet diesen Ansatz (übergeben, also eine *stark* typisierten Modell) mit der `MoviesController` Klasse, und zeigen-Vorlagen, wenn sie die Methoden und Ansichten erstellt.

In der *Controllers\MoviesController.cs* untersuchen Sie die generierte Datei `Details` Methode. Die `Details` Methode wird unten angezeigt.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Die `id` Parameter wird in der Regel als übergeben, Weiterleiten von Daten, z. B. `http://localhost:1234/movies/details/1` des Controllers wird festgelegt werden, mit der Movie-Controller, die Aktion, die `details` und `id` auf 1. Sie könnten auch wie folgt in die Id mit einer Abfragezeichenfolge übergeben:

`http://localhost:1234/movies/details?id=1`

Wenn eine `Movie` gefunden wird, wird eine Instanz von der `Movie` Modell wird übergeben, um die `Details` anzeigen:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Untersuchen des Inhalts der *Views\Movies\Details.cshtml* Datei:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Durch Einschließen einer `@model` -Anweisung am Anfang der Ansichtsdatei für die Vorlage können Sie den Typ des Objekts, das die Ansicht erwartet angeben. Beim Erstellen des „Movies“-Controllers hat Visual Studio automatisch die folgende `@model`-Anweisung am Anfang der Datei *Details.cshtml* hinzugefügt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Diese `@model`-Direktive ermöglicht Ihnen den Zugriff auf den Film, den der Controller an die Ansicht übergeben hat, indem ein stark typisiertes `Model`-Objekt verwendet wir. Z. B. in der *Details.cshtml* Vorlage übergibt der Code jedes filmfeld an die `DisplayNameFor` und ["DisplayFor"](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML-Hilfsprogrammen, mit dem stark typisierten `Model` Objekt. Die `Create` und `Edit` Methoden und Anzeigen von Vorlagen auch ein Movie-Modell-Objekt übergeben.

Überprüfen Sie die *"Index.cshtml"* Vorlage anzeigen und die `Index` -Methode in der die *"moviescontroller.cs"* Datei. Beachten Sie, wie der Code erstellt eine [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) Objekt beim Aufrufen der `View` -Hilfsmethode in den `Index` Aktionsmethode. Der Code übergibt diese dann `Movies` Liste aus der `Index` Aktionsmethode zur Ansicht:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Bei der Erstellung des Movie-Controllers enthalten Visual Studio automatisch die folgenden `@model` Anweisung am Anfang der *"Index.cshtml"* Datei:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Dies `@model` -Direktive ermöglicht Ihnen Zugriff auf die Liste von Filmen, die mithilfe der Controller an die Ansicht übergeben eine `Model` -Objekt, das stark typisiert ist. Z. B. in der *"Index.cshtml"* Vorlage, der Code durchläuft die Filme durch praktische Übungen einen `foreach` Anweisung für das stark typisierte `Model` Objekt:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Da die `Model` -Objekt stark typisiert ist (als ein `IEnumerable<Movie>` Objekt), die jeweils `item` Objekt in der Schleife wird als eingegeben `Movie`. Neben anderen Vorteilen bedeutet dies, dass Sie während der Kompilierung des Codes überprüfen und vollständige IntelliSense-Unterstützung im Code-Editor:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Arbeiten mit SQL Server LocalDB

Entity Framework Code First hat festgestellt, dass es sich bei die Datenbank-Verbindungszeichenfolge, die bereitgestellt wurden, zeigt eine `Movies` -Datenbank, die noch nicht vorhanden ist, sodass Code First die Datenbank automatisch erstellt. Sie können überprüfen, ob dieser erstellt wurde anhand der *App\_Daten* Ordner. Wenn Sie nicht angezeigt wird der *Movies.mdf* , klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** -Symbolleiste klicken Sie auf die **aktualisieren** Schaltfläche, und erweitern Sie dann die *App\_Daten* Ordner.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Doppelklicken Sie auf *Movies.mdf* öffnen **SERVER-EXPLORER**, erweitern Sie dann die **Tabellen** Ordner finden in der Tabelle "Movies". Beachten Sie das Schlüsselsymbol neben ID Standardmäßig wird EF eine Eigenschaft namens-ID der primäre Schlüssel stellen. Weitere Informationen zu EF und MVC, finden Sie unter Tom Dykstras ausgezeichnetes Tutorial auf [MVC und EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendaten anzeigen** zu ermitteln, die Daten erstellt wurde.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Mit der rechten Maustaste die `Movies` Tabelle, und wählen Sie **Tabellendefinition öffnen** um die Tabelle, Entity Framework Code First für Sie erstellten Struktur anzuzeigen.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Beachten Sie, dass wie das Schema der `Movies` Tabelle zugeordnet, die `Movie` Klasse, die Sie zuvor erstellt haben. Entity Framework Code First automatisch erstellt, diesem Schema basierend auf Ihrer `Movie` Klasse.

Wenn Sie fertig sind, schließen Sie die Verbindung mit der rechten Maustaste *MovieDBContext* und **Verbindung schließen**. (Wenn Sie die Verbindung nicht schließen, Sie erhalten möglicherweise einen Fehler beim nächsten des Projekts ausführen).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Sie verfügen jetzt über eine Datenbank und Seiten zum Anzeigen, Bearbeiten, Aktualisieren und Löschen von Daten. Im nächsten Tutorial, wir die restlichen aus der eingerüstete Code überprüfen und Hinzufügen einer `SearchIndex` Methode und eine `SearchIndex` an, die für Filme in dieser Datenbank durchsuchen kann. Weitere Informationen zur Verwendung von Entity Framework mit MVC finden Sie unter [erstellen ein Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Zurück](creating-a-connection-string.md)
> [Weiter](examining-the-edit-methods-and-edit-view.md)
