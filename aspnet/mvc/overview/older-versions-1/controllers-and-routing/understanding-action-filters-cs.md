---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: Grundlegendes zu Aktionsfiltern (c#) | Microsoft-Dokumentation
author: microsoft
description: Das Ziel dieses Lernprogramms wird Aktionsfilter beschrieben. Ein Aktionsfilter ist ein Attribut, das Sie auf eine Controlleraktion – oder einen gesamten Controller anwenden können...
ms.author: aspnetcontent
ms.date: 10/16/2008
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: aaec90a14e53f0173ea09fbdbaa591ec0eb680b1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37810162"
---
<a name="understanding-action-filters-c"></a>Grundlegendes zu Aktionsfiltern (c#)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> Das Ziel dieses Lernprogramms wird Aktionsfilter beschrieben. Ein Aktionsfilter ist ein Attribut, das Sie anwenden können, um eine Controlleraktion – oder einen gesamten Controller –, der die Methode ändert, in der die Aktion ausgeführt wird.


## <a name="understanding-action-filters"></a>Grundlegendes zu Aktionsfiltern

Das Ziel dieses Lernprogramms wird Aktionsfilter beschrieben. Ein Aktionsfilter ist ein Attribut, das Sie anwenden können, um eine Controlleraktion – oder einen gesamten Controller –, der die Methode ändert, in der die Aktion ausgeführt wird. ASP.NET MVC-Framework umfasst mehrere Aktionsfilter verwendet werden:

- OutputCache-wird dieser Aktionsfilter die Ausgabe eine Controlleraktion für einen angegebenen Zeitraum zwischengespeichert.
- HandleError – behandelt diese Aktionsfilter Fehler wird ausgelöst, wenn eine Controlleraktion ausgeführt wird.
- Autorisieren – diese Aktionsfilter können Sie den Zugriff auf einen bestimmten Benutzer oder Rolle einzuschränken.

Sie können auch eigene benutzerdefinierte Aktionsfilter erstellen. Beispielsweise empfiehlt es sich um eines benutzerdefinierten Aktionsfilters zu erstellen, um ein benutzerdefiniertes Authentifizierungssystem implementieren. Oder Sie möchten einen Aktionsfilter erstellen, der die sichtdaten zurückgegeben werden, indem eine Controlleraktion ändert.

In diesem Tutorial erfahren Sie, wie einen Aktionsfilter von Grund auf neu erstellt werden müssten. Erstellen wir einen Protokoll-Aktionsfilter, der verschiedene Phasen der Verarbeitung einer Aktion in der Visual Studio-Ausgabefenster protokolliert.

### <a name="using-an-action-filter"></a>Mithilfe eines Aktionsfilters

Ein Aktionsfilter ist ein Attribut. Sie können die meisten der Aktionsfilter entweder eine einzelne Controlleraktion oder einen gesamten Controller anwenden.

Die Verantwortlichen in Codebeispiel 1 stellt beispielsweise eine Aktion, die mit dem Namen `Index()` , die die aktuelle Uhrzeit zurückgibt. Diese Aktion wird ergänzt, mit der `OutputCache` Action-Filter. Dieser Filter führt dazu, dass den Rückgabewert von der Aktion, die für 10 Sekunden zwischengespeichert werden.

**Codebeispiel 1: `Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

Wenn Sie wiederholt Aufrufen der `Index()` Aktionen durch Eingabe der URL/Data/Index in die Adressleiste Ihres Browsers, und drücken die Aktualisierung Schaltfläche mehrmals gleichzeitig für 10 Sekunden angezeigt wird. Die Ausgabe der `Index()` Aktion für 10 Sekunden (siehe Abbildung 1) zwischengespeichert wird.


[![Cachezeit](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**Abbildung 01**: Zeit zwischengespeichert ([klicken Sie, um das Bild in voller Größe anzeigen](understanding-action-filters-cs/_static/image3.png))


Im Codebeispiel 1, eine einzelne Aktion-Filter – die `OutputCache` Action-Filter – gilt für die `Index()` Methode. Wenn Sie benötigen, können Sie mehrere Aktionsfilter zur gleichen Aktion anwenden. Angenommen, Sie möchten beide gelten die `OutputCache` und `HandleError` Aktionsfilter zur gleichen Aktion.

Im Codebeispiel 1 die `OutputCache` Action-Filter wird angewendet, um die `Index()` Aktion. Sie auch konnte Anwenden dieses Attributs auf die `DataController` selbst. In diesem Fall würde das Ergebnis eine Aktion, die verfügbar gemacht werden, durch den Controller für 10 Sekunden zwischengespeichert werden.

### <a name="the-different-types-of-filters"></a>Die verschiedenen Arten von Filtern

ASP.NET MVC-Framework unterstützt vier verschiedene Arten von Filtern:

1. Autorisierungsfilter werden also – implementiert die `IAuthorizationFilter` Attribut.
2. Aktionsfilter – implementiert die `IActionFilter` Attribut.
3. Ergebnisfilter – implementiert die `IResultFilter` Attribut.
4. Ausnahmefilter – implementiert die `IExceptionFilter` Attribut.

Filter werden in der oben aufgeführten Reihenfolge ausgeführt. Beispielsweise Autorisierungsfilter werden immer vor dem Aktionsfilter ausgeführt und Ausnahmefilter werden immer ausgeführt, nachdem alle anderen Arten von Filtern.

Autorisierungsfilter werden verwendet, um Authentifizierung und Autorisierung für Controlleraktionen zu implementieren. Der Authorize-Filter ist z. B. ein Beispiel für einen Autorisierungsfilter.

Aktionsfilter enthalten Logik, die vor und nach der Ausführung einer Controlleraktion ausgeführt wird. Sie können einen Aktionsfilter, z. B. verwenden, so ändern Sie die Daten anzeigen, die eine Controlleraktion zurückgibt.

Ergebnisfilter enthalten Logik, die ausgeführt wird, vor und nach einem Ergebnis ausgeführt wird. Beispielsweise kann ein Ergebnis ändern möchten direkt vor die Ansicht im Browser gerendert wird.

Ausnahmefilter sind, diese Art der Filter ausgeführt wird. Sie können einen Ausnahmefilter Behandeln von Fehlern, die ausgelöst wird, indem Sie entweder Ihre Controlleraktionen oder die Ergebnisse von Controlleraktionen verwenden. Sie können auch die Ausnahmefilter verwenden, um Fehler zu protokollieren.

Jede andere Art von Filter wird in einer bestimmten Reihenfolge ausgeführt. Wenn zur Steuerung der Reihenfolge, in der Filter desselben Typs ausgeführt werden, sollen, können Sie einen Filter in der Order-Eigenschaft festlegen.

Die Basisklasse für alle Aktionsfilter der `System.Web.Mvc.FilterAttribute` Klasse. Wenn Sie eine bestimmte Art von Filter implementieren möchten, müssen Sie eine Klasse erstellen, die die Filter-Basisklasse erbt und implementiert eine oder mehrere der der `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`, oder `IExceptionFilter` Schnittstellen.

### <a name="the-base-actionfilterattribute-class"></a>Die Basisklasse von ActionFilterAttribute ab

Um Ihnen die Implementierung eines benutzerdefinierten Aktionsfilters vereinfachen, enthält das ASP.NET MVC-Framework eine Base `ActionFilterAttribute` Klasse. Diese Klasse implementiert sowohl die `IActionFilter` und `IResultFilter` Schnittstellen und erbt von der `Filter` Klasse.

Die Fachbegriffe ist nicht vollständig konsistent. Technisch gesehen ist eine Klasse, die von der ActionFilterAttribute-Klasse erbt, sowohl für einen Aktionsfilter als auch für einen Ergebnisfilter. Allerdings ist insofern lose, der Word-Action-Filter verwendet, zum Verweisen auf jede Art von Filter in ASP.NET MVC-Framework.

Die Basis `ActionFilterAttribute` -Klasse verfügt über die folgenden Methoden, die Sie überschreiben können:

- OnActionExecuting – ist diese Methode aufgerufen, bevor eine Controlleraktion ausgeführt wird.
- OnActionExecuted – ist diese Methode aufgerufen, nach der Ausführung einer Controlleraktion.
- OnResultExecuting – ist diese Methode aufgerufen, bevor ein Aktionsergebnis Controller ausgeführt wird.
- OnResultExecuted – ist diese Methode aufgerufen, nachdem ein Controller Aktionsergebnis ausgeführt wurde.

Im nächsten Abschnitt erfahren Sie, wie Sie einer dieser anderen Methoden implementieren können.

### <a name="creating-a-log-action-filter"></a>Erstellen einen Protokoll-Aktionsfilter

Um zu veranschaulichen, wie Sie einen benutzerdefinierte Aktionsfilter erstellen können, erstellen wir ein benutzerdefinierten Aktionsfilters, das die Phasen der Verarbeitung einer Controlleraktion auf die Visual Studio-Ausgabefenster protokolliert. Unsere `LogActionFilter` Programmausdruck 2 enthalten ist.

**Codebeispiel 2: `ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

Programmausdruck 2 die `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, und `OnResultExecuted()` alle Methoden aufrufen, die `Log()` Methode. Der Name der Methode und die aktuelle Routendaten übergeben wird, um die `Log()` Methode. Die `Log()` -Methode schreibt eine Meldung an das Ausgabefenster von Visual Studio-Fenster (siehe Abbildung 2).


[![Das Schreiben in das Ausgabefenster von Visual Studio](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**Abbildung 02**: Schreiben in das Ausgabefenster von Visual Studio-Fenster ([klicken Sie, um das Bild in voller Größe anzeigen](understanding-action-filters-cs/_static/image6.png))


Die Home-Controller in Codebeispiel 3 wird veranschaulicht, wie Sie die Log-Action-Filter auf eine gesamte Controllerklasse anwenden können. Jedes Mal, wenn die von den Home-Controller verfügbar gemachten Aktionen aufgerufen werden – entweder die `Index()` Methode oder der `About()` Methode: die Phasen der Verarbeitung, die die Aktion im Ausgabefenster von Visual Studio angemeldet sind.

**Codebeispiel 3: `Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wurden Sie in ASP.NET MVC-Aktionsfilter eingeführt. Sie haben über die vier verschiedene Arten von Filtern: Autorisierungsfilter, Aktionsfilter, Ergebnisfilter und Ausnahmefilter. Sie haben auch über die die Basis `ActionFilterAttribute` Klasse.

Schließlich haben Sie einen einfachen Aktionsfilter zu implementieren. Es erstellt einen Protokoll-Aktionsfilter, der die Phasen der Verarbeitung einer Controlleraktion auf die Visual Studio-Ausgabefenster protokolliert.

> [!div class="step-by-step"]
> [Zurück](asp-net-mvc-routing-overview-cs.md)
> [Weiter](improving-performance-with-output-caching-cs.md)
