---
title: Anzeigen von Komponenten
author: rick-anderson
description: "Anzeigen von Komponenten dienen an einer beliebigen Stelle, dass Sie wiederverwendbare Renderinglogik verfügen."
keywords: ASP.NET Core, Anzeigen von Komponenten, Teilansicht
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: ab4705b7-59d7-4f31-bc97-ea7f292fe926
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-components
ms.openlocfilehash: 3bc6d3f85d8ea7fb9b72b18cfd9c5ec2d07293b0
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2017
---
# <a name="view-components"></a>Anzeigen von Komponenten

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample))

## <a name="introducing-view-components"></a>Einführung zum Anzeigen von Komponenten

Neue zu ASP.NET Core MVC, ähneln sich Komponenten anzeigen, Teilansichten, aber sie sind deutlich leistungsfähiger. Anzeigen von Komponenten nicht wurden die modellbindung verwenden und nur richten sich nach den Daten, die Sie angeben, wenn er aufgerufen. Eine Komponente anzeigen:

* Rendert ein Block, statt eine gesamte Antwort
* Enthält die gleichen Trennung von Anliegen und die Prüfbarkeit Vorteile, die zwischen einem Controller und Ansicht gefunden
* Parameter und Geschäftslogik können haben
* Wird normalerweise aus einer Layoutseite aufgerufen.

Anzeigen von Komponenten dienen an einer beliebigen Stelle, dass Sie wiederverwendbare Renderinglogik verfügen, z. B. zu komplex für eine Teilansicht ist:

* Dynamische Navigationsmenüs
* Tagcloud (, in dem sie die Datenbank (Abfragen)
* Login-Bereich
* Einkaufswagen
* Kürzlich veröffentlichten Artikeln
* Randleiste Inhalte auf einem typischen blog
* Ein Anmeldefenster, die auf jeder Seite gerendert und entweder die Links, melden Sie sich ab, oder melden Sie sich, abhängig von das Protokoll in der Status des Benutzers anzeigen

Eine Ansichtskomponente besteht aus zwei Teilen: der-Klasse (normalerweise abgeleitet aus [ViewComponent](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponent)) und das Ergebnis gibt (in der Regel eine Ansicht) zurück. Wie Domänencontroller, eine Ansichtskomponente auf einer POCO werden kann, aber die meisten Entwickler sollten die verfügbaren Methoden und Eigenschaften nutzen durch Ableiten von `ViewComponent`.

## <a name="creating-a-view-component"></a>Erstellen einer Komponente anzeigen

Dieser Abschnitt enthält die allgemeinen Anforderungen zum Erstellen einer Komponente anzeigen. Später in diesem Artikel wir untersuchen Sie jeden Schritt im Detail und erstellen Sie eine Komponente anzeigen.

### <a name="the-view-component-class"></a>Die Komponente Ansichtsklasse

Eine Komponentenklasse anzeigen kann durch eine der folgenden erstellt werden:

* Ableiten von *ViewComponent*
* Indem eine Klasse mit der `[ViewComponent]` Attribut oder das Ableiten einer Klasse mit der `[ViewComponent]` Attribut
* Erstellen einer Klasse, wenn der Name mit dem Suffix endet *ViewComponent*

Controller Schemas müssen wie ansichtskomponenten öffentliche, nicht geschachtelten und nicht abstrakten Klassen sein. Der Komponentenname Ansicht wird der Klassenname mit dem "ViewComponent" Suffix entfernt. Sie können auch explizit angegeben werden mithilfe der `ViewComponentAttribute.Name` Eigenschaft.

Eine Komponentenklasse für die Sicht:

* Unterstützt uneingeschränkt Konstruktor [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md)

* Verwendet keinen Teil des Controller-Lebenszyklus, d. h., Sie können keine [Filter](../controllers/filters.md) in einer Ansichtskomponente

### <a name="view-component-methods"></a>Die Komponentenmethoden anzuzeigen

Eine Ansichtskomponente definiert ihre Logik in einer `InvokeAsync` Methode, die zurückgibt ein `IViewComponentResult`. Parameter stammen direkt aus dem Aufruf der Komponente anzeigen, nicht von der modellbindung. Eine Ansichtskomponente nie direkt eine Anforderung behandelt. In der Regel eine Ansichtskomponente Initialisiert ein Modell und übergibt sie an einer Ansicht durch Aufrufen der `View` Methode. Zeigen Sie zusammengefasst Komponentenmethoden an:

* Definieren einer `InvokeAsync` Methode, die zurückgibt ein`IViewComponentResult`
* In der Regel ein Modell initialisiert und übergibt sie an einer Ansicht durch Aufrufen der `ViewComponent` `View` Methode
* Parameter aus der aufrufenden Methode nicht HTTP stammen, es erfolgt keine modellbindung
* Werden direkt als ein HTTP-Endpunkt nicht erreichbar, sie werden aufgerufen, aus dem Code (in der Regel in einer Ansicht). Eine Ansichtskomponente behandelt nie eine Anforderung.
* Alle Details aus der aktuellen HTTP-Anforderung, anstatt die Signatur sind überladen werden.

### <a name="view-search-path"></a>Suchpfad für die Ansicht

Die Laufzeit sucht, für die Ansicht in den folgenden Pfaden:

   * Ansichten /\<Controller_name > /Components/\<View_component_name > /\<View_name >
   * Ansichten/freigegeben/Components/\<View_component_name > /\<View_name >

Der Standardname für die Sicht für eine Ansichtskomponente ist *Standard*, was bedeutet, dass die Datei wird in der Regel namens *Default.cshtml*. Beim Erstellen der Komponente Ansichtsergebnis oder beim Aufrufen, geben Sie einen Namen für die andere Ansicht kann die `View` Methode.

Es wird empfohlen, Sie benennen die Ansichtsdatei *Default.cshtml* und Verwenden der *Ansichten/freigegeben/Components/\<View_component_name > /\<View_name >* Pfad. Die `PriorityList` verwendet in diesem Beispiel verwendete Ansichtskomponente *Views/Shared/Components/PriorityList/Default.cshtml* für die Komponentensicht anzeigen.

## <a name="invoking-a-view-component"></a>Aufrufen einer Komponente anzeigen

Rufen Sie zum Verwenden der Ansichtskomponente auf Folgendes in einer Ansicht:

```html
@Component.InvokeAsync("Name of view component", <anonymous type containing parameters>)
```

Werden die Parameter an übergeben der `InvokeAsync` Methode. Die `PriorityList` Ansichtskomponente entwickelt, in dem Artikel aus aufgerufen wird die *Views/Todo/Index.cshtml* Datei anzeigen. Im folgenden wird die `InvokeAsync` Methode mit zwei Parametern aufgerufen wird:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

## <a name="invoking-a-view-component-as-a-tag-helper"></a>Aufrufen einer Ansichtskomponente als ein Tag-Hilfsprogramm

Für ASP.NET Core 1.1 und höher können Sie eine Ansichtskomponente als Aufrufen einer [Tag Helper](xref:mvc/views/tag-helpers/intro):

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

In Pascal-Schreibweise angegeben Klassen- und Parameter für den Tag-Hilfsprogrammen übersetzt ihre [senken Kebab Groß-/Kleinschreibung](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Der Tag-Hilfskomponente aufzurufenden eine Ansichtskomponente auf die `<vc></vc>` Element. Die Ansichtskomponente wird wie folgt angegeben:

```html
<vc:[view-component-name]
  parameter1="parameter1 value"
  parameter2="parameter2 value">
</vc:[view-component-name]>
```

Hinweis: Um eine Ansichtskomponente als ein Tag-Hilfsprogramm verwenden, müssen Sie die Assembly registrieren, enthält die Ansicht mit der `@addTagHelper` Richtlinie. Beispielsweise ist die View-Komponente in einer Assembly mit dem Namen "MyWebApp", die folgende Anweisung zum Hinzufügen der `_ViewImports.cshtml` Datei:

```csharp
@addTagHelper *, MyWebApp
```

Sie können eine Ansichtskomponente auf als ein Tag Hilfsprogramm auf einer beliebigen Datei registrieren, die die Komponente für die Sicht verweist. Finden Sie unter [verwalten Tag Helper Bereich](xref:mvc/views/tag-helpers/intro#managing-tag-helper-scope) für Weitere Informationen zum Registrieren von Tag-Hilfsprogramme.

Die `InvokeAsync` in diesem Lernprogramm verwendete Methode:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Im Hilfsprogramm-Tag Markup:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexTagHelper.cshtml?range=37-38)]

Im obigen Beispiel die `PriorityList` Ansichtskomponente wird `priority-list`. Die Parameter der Komponente anzeigen, werden als Attribute in Kleinbuchstaben Kebab übergeben.

### <a name="invoking-a-view-component-directly-from-a-controller"></a>Aufrufen einer Ansichtskomponente direkt von einem controller

Anzeigen von Komponenten in der Regel aus einer Sicht aufgerufen werden, aber Sie direkt von einem Controllermethode aufrufen. Während ansichtskomponenten keine Endpunkte wie Controller definieren, können Sie problemlos eine Controlleraktion, die den Inhalt des zurückgibt implementieren eine `ViewComponentResult`.

In diesem Beispiel wird die Ansichtskomponente direkt auf dem Controller aufgerufen:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

## <a name="walkthrough-creating-a-simple-view-component"></a>Exemplarische Vorgehensweise: Erstellen einer einfachen Ansicht

[Herunterladen](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/view-components/sample), erstellen und Testen Sie den Startcode. Es ist ein einfaches Projekt mit einem `Todo` Controller, der zeigt eine Liste der *Todo* Elemente.

![Liste der ToDos](view-components/_static/2dos.png)

### <a name="add-a-viewcomponent-class"></a>Fügen Sie eine ViewComponent-Klasse

Erstellen einer *ViewComponents* Ordner, und fügen Sie die folgenden `PriorityListViewComponent` Klasse:

[!code-csharp[Main](view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponent1.cs?name=snippet1)]

Hinweise zum Code:

* Komponente Ansichtsklassen in enthalten sein können **alle** Ordner des Projekts.
* Da der Klassenname PriorityList**ViewComponent** endet mit dem Suffix **ViewComponent**, die Common Language Runtime wird die Zeichenfolge "PriorityList" verwenden, wenn Sie die Klasse, Komponente aus einer Sicht verweisen. Ich werde, die später ausführlicher erläutert.
* Die `[ViewComponent]` Attribut den Namen verwendet, um eine Ansicht Komponentenverweis ändern. Angenommen, wir konnte haben mit dem Namen der Klasse `XYZ` angewendet, und die `ViewComponent` Attribut:

  ```csharp
  [ViewComponent(Name = "PriorityList")]
     public class XYZ : ViewComponent
     ```

* Die `[ViewComponent]` Attribut teilt die Ansichtsauswahl für die Komponente den Namen `PriorityList` bei der Suche nach der Sichten verknüpft sind, mit der Komponente und die Zeichenfolge "PriorityList" verwenden, wenn Sie die Klasse, Komponente aus einer Sicht verweisen. Ich werde, die später ausführlicher erläutert.
* Die Komponente verwendet [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md) um den Datenkontext verfügbar zu machen.
* `InvokeAsync`macht dauert eine Methode, die aus einer Sicht aufgerufen werden, kann eine beliebige Anzahl von Argumenten.
* Die `InvokeAsync` Methode gibt den Satz der `ToDo` Elemente, die erfüllen die `isDone` und `maxPriority` Parameter.

### <a name="create-the-view-component-razor-view"></a>Erstellen Sie die Komponente Razor-Ansicht

* Erstellen der *Ansichten/freigegeben/Komponenten* Ordner. In diesem Ordner **müssen** heißen *Komponenten*.

* Erstellen der *Ansichten/freigegeben/Components/PriorityList* Ordner. Diese Ordnername muss übereinstimmen, den Namen der Komponente Ansichtsklasse oder den Namen der Klasse ohne das Suffix (wenn es folgt der Konvention und verwendet die *ViewComponent* Suffix im Klassennamen). Bei Verwendung der `ViewComponent` -Attribut, der Klassennamen müsste die Bezeichnung Attribut entsprechen.

* Erstellen einer *Views/Shared/Components/PriorityList/Default.cshtml* Razor-Ansicht:[!code-html[Main](view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/Default1.cshtml)]
    
   Der Razor-Ansicht kann eine Liste von `TodoItem` und zeigt diese an. Wenn die Ansichtskomponente `InvokeAsync` Methode nicht übergeben Sie den Namen der Sicht (wie in diesem Beispiel), *Standard* wird für den Ansichtsnamen gemäß der Konvention verwendet. Später in diesem Lernprogramm zeige ich Ihnen wie der Name der Ansicht zu übergeben. Um das standardmäßige Format für einen bestimmten Controller zu überschreiben, fügen Sie eine Ansicht in den Ansichtordner Controller-spezifische (z. B. *Views/Todo/Components/PriorityList/Default.cshtml)*.
    
    Wenn die Ansichtskomponente Controller spezifisch ist, können Sie es in den Ordner Controller-spezifische hinzufügen (*Views/Todo/Components/PriorityList/Default.cshtml*).

* Hinzufügen einer `div` , die einen Aufruf an das Ende der Priorität List-Komponente enthält die *Views/Todo/index.cshtml* Datei:

    [!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFirst.cshtml?range=34-38)]

Das Markup `@await Component.InvokeAsync` zeigt die Syntax zum Aufrufen von Komponenten anzeigen. Das erste Argument ist der Name der Komponente, die wir aufrufen oder aufgerufen werden soll. Nachfolgende Parameter werden an die Komponente übergeben. `InvokeAsync`kann eine beliebige Anzahl von Argumenten dauern.

Testen der app an. Die folgende Abbildung zeigt die TODO-Liste und Elemente mit der Priorität:

![TODO-Liste und die Priorität der Elemente](view-components/_static/pi.png)

Sie können auch die Ansichtskomponente direkt auf dem Controller aufrufen:

[!code-csharp[Main](view-components/sample/ViewCompFinal/Controllers/ToDoController.cs?name=snippet_IndexVC)]

![Elemente der Priorität von IndexVC-Aktion](view-components/_static/indexvc.png)

### <a name="specifying-a-view-name"></a>Einen Ansichtsnamen angeben

Eine komplexe Ansichtskomponente müssen möglicherweise eine nicht standardmäßige Ansicht unter bestimmten Umständen angeben. Der folgende Code zeigt, wie die Ansicht "PVC" aus der `InvokeAsync` Methode. Update der `InvokeAsync` Methode in der `PriorityListViewComponent` Klasse.

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityListViewComponentFinal.cs?highlight=4,5,6,7,8,9&range=28-39)]

Kopieren der *Views/Shared/Components/PriorityList/Default.cshtml* Datei in eine Ansicht mit dem Namen *Views/Shared/Components/PriorityList/PVC.cshtml*. Fügen Sie eine Überschrift, um anzugeben, dass die Sicht PVC verwendet wird.

[!code-html[Main](../../mvc/views/view-components/sample/ViewCompFinal/Views/Shared/Components/PriorityList/PVC.cshtml?highlight=3)]

Update *Views/TodoList/Index.cshtml*:

<!-- Views/TodoList/Index.cshtml is never imported, so change to test tutorial -->

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexFinal.cshtml?range=35)]

Führen Sie die app, und überprüfen Sie PVC anzeigen.

![Priorität Ansichtskomponente](view-components/_static/pvc.png)

Wenn die Sicht PVC nicht gerendert wird, stellen Sie sicher, dass Sie die Ansichtskomponente mit einer Priorität von 4 oder höher aufrufen.

### <a name="examine-the-view-path"></a>Überprüfen Sie den Pfad anzeigen

* Ändern Sie den Parameter Priorität auf drei oder weniger ein, damit die Ansicht Priorität nicht zurückgegeben wird.
* Benennen Sie vorübergehend die *Views/Todo/Components/PriorityList/Default.cshtml* auf *1Default.cshtml*.
* Testen der app, die Sie erhalten die folgende Fehlermeldung:

   ```
   An unhandled exception occurred while processing the request.
   InvalidOperationException: The view 'Components/PriorityList/Default' was not found. The following locations were searched:
   /Views/ToDo/Components/PriorityList/Default.cshtml
   /Views/Shared/Components/PriorityList/Default.cshtml
   EnsureSuccessful
   ```

* Kopie *Views/Todo/Components/PriorityList/1Default.cshtml* auf *Views/Shared/Components/PriorityList/Default.cshtml*.
* Einige Markup zum Hinzufügen der *Shared* Komponente Todo-Ansicht an, dass die Ansicht ist von der *Shared* Ordner.
* Testen der **Shared** Komponentensicht.

![TODO-Ausgabe mit der freigegebenen Komponente anzeigen](view-components/_static/shared.png)

### <a name="avoiding-magic-strings"></a>Vermeiden von Magic-Zeichenfolgen

Wenn Sie die Sicherheit kompilieren möchten, können Sie den Namen der Sicht hartcodierte Komponente mit dem Klassennamen ersetzen. Erstellen Sie die Komponente anzeigen, ohne das Suffix "ViewComponent":

[!code-csharp[Main](../../mvc/views/view-components/sample/ViewCompFinal/ViewComponents/PriorityList.cs?highlight=10&range=5-35)]

Hinzufügen einer `using` Anweisung, um Ihre Razor zeigen Sie an, und verwenden die `nameof` Operator:

[!code-html[Main](view-components/sample/ViewCompFinal/Views/Todo/IndexNameof.cshtml?range=1-6,33-)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Abhängigkeitsinjektion in Ansichten](dependency-injection.md)
