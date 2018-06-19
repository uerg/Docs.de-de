---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
title: Grundlegendes zu Aktionsfiltern (VB) | Microsoft Docs
author: microsoft
description: Das Ziel dieses Lernprogramms ist zur Erläuterung der Aktionsfilter verwendet werden. Ein Aktionsfilter wird ein Attribut, das Sie auf eine Controlleraktion – oder eine gesamte Controller anwenden können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: e83812f2-c53e-4a43-a7c1-d64c59ecf694
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-vb
msc.type: authoredcontent
ms.openlocfilehash: 2796b67cba6a2ddaee7a006a170dfb7e5ff89888
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869815"
---
<a name="understanding-action-filters-vb"></a>Grundlegendes zu Aktionsfiltern (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_VB.pdf)

> Das Ziel dieses Lernprogramms ist zur Erläuterung der Aktionsfilter verwendet werden. Ein Aktionsfilter ist ein Attribut, das Sie anwenden können, um eine Controlleraktion – oder eine gesamte Controller--, der die Methode ändert, in der die Aktion ausgeführt wird.


## <a name="understanding-action-filters"></a>Understanding Aktionsfiltern

Das Ziel dieses Lernprogramms ist zur Erläuterung der Aktionsfilter verwendet werden. Ein Aktionsfilter ist ein Attribut, das Sie anwenden können, um eine Controlleraktion – oder eine gesamte Controller--, der die Methode ändert, in der die Aktion ausgeführt wird. ASP.NET MVC-Framework umfasst mehrere Aktionsfilter verwendet werden:

- OutputCache – diese Aktionsfilter speichert die Ausgabe eine Controlleraktion für einen angegebenen Zeitraum.
- Verwendung von HandleError – behandelt diese Aktionsfilter Fehler ausgelöst, wenn eine Controlleraktion ausgeführt wird.
- Autorisierung – diese Aktionsfilter ermöglicht es Ihnen, den Zugriff auf einen bestimmten Benutzer oder eine Rolle einschränken.

Sie können auch eigene benutzerdefinierte Aktionsfilter erstellen. Sie möchten z. B. ein benutzerdefinierten Aktionsfilters zu erstellen, um ein benutzerdefiniertes Authentifizierungssystem implementieren. Oder Sie möchten einen Aktionsfilter erstellen, der die Ansichtsdaten eine Controlleraktion zurückgegebenes ändert.

In diesem Lernprogramm erfahren Sie, wie Sie einen Aktionsfilter von Grund auf erstellen werden. Wir erstellen einen Protokoll-Action-Filter, der verschiedene Phasen der Verarbeitung einer Aktion im Ausgabefenster von Visual Studio protokolliert.

### <a name="using-an-action-filter"></a>Verwenden einen Aktionsfilter

Ein Aktionsfilter ist ein Attribut. Sie können die meisten Aktionsfilter auf eine einzelne Controlleraktion oder eine gesamte Controller anwenden.

Der Controller Daten im Codebeispiel 1 macht z. B. eine Aktion, die mit dem Namen `Index()` , die die aktuelle Uhrzeit zurückgibt. Diese Aktion ist mit ergänzt die `OutputCache` Aktionsfilter. Dieser Filter führt dazu, dass der Rückgabewert von der Aktion für 10 Sekunden zwischengespeichert werden soll.

**Auflisten von 1 – `Controllers\DataController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample1.vb)]

Wenn Sie wiederholt Aufrufen der `Index()` Aktionen durch Eingeben der URL/Data/Index in der Adressleiste des Browsers ein, und drücken die Aktualisierung Schaltfläche mehrere Male gleichzeitig für 10 Sekunden angezeigt wird. Die Ausgabe der `Index()` Aktion wird für 10 Sekunden (siehe Abbildung 1) zwischengespeichert.


[![Cachezeit](understanding-action-filters-vb/_static/image2.png)](understanding-action-filters-vb/_static/image1.png)

**Abbildung 01**: Zeit zwischengespeichert ([klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-action-filters-vb/_static/image3.png))


Im Codebeispiel 1 eines einzelnen Aktionsfilters – die `OutputCache` Aktionsfilter – wird angewendet, um die `Index()` Methode. Wenn Sie möchten, können Sie mehrere Aktionsfilter auf dieselbe Aktion anwenden. Möglicherweise möchten z. B. beide gelten die `OutputCache` und `HandleError` Aktionsfilter auf die gleiche Aktion.

Im Codebeispiel 1 die `OutputCache` Action-Filter wird angewendet, um die `Index()` Aktion. Dieses Attribut kann auch anwenden der `DataController` selbst dar. In diesem Fall würde das Ergebnis zurückgegeben, die für jede Aktion, die vom Controller verfügbar gemacht für 10 Sekunden zwischengespeichert werden.

### <a name="the-different-types-of-filters"></a>Verschiedenen Arten von Filtern

Das ASP.NET MVC-Framework unterstützt vier verschiedene Arten von Filtern:

1. Autorisierungsfilter – implementiert die `IAuthorizationFilter` Attribut.
2. Aktionsfilter – implementiert die `IActionFilter` Attribut.
3. Führen Sie die Filter – implementiert die `IResultFilter` Attribut.
4. Ausnahmefilter – implementiert die `IExceptionFilter` Attribut.

Filter werden in der oben aufgeführten Reihenfolge ausgeführt. Beispielsweise Autorisierungsfilter werden immer vor dem Aktionsfilter ausgeführt und Ausnahmefilter werden immer ausgeführt, nachdem jede andere Art von Filter.

Autorisierungsfilter werden verwendet, um Authentifizierung und Autorisierung für Controlleraktionen zu implementieren. Die Authorize-Filter ist z. B. ein Beispiel für einen Autorisierungsfilter.

Aktionsfilter enthalten die Logik, die vor und nach der Ausführung einer Controlleraktion ausgeführt wird. Sie können einen Aktionsfilter z. B. verwenden, um Daten für die Ansicht zu ändern, die eine Controlleraktion zurückgibt.

Ergebnisfilter enthalten die Logik, die vor und nach der Ausführung einer Ansichtsergebnis ausgeführt wird. Möglicherweise möchten z. B. Ändern einer Ansichtsergebnis mit der rechten Maustaste, bevor die Ansicht im Browser gerendert wird.

Ausnahmefilter sind der letzte Filter ausgeführt. Einen Ausnahmefilter können zum Behandeln von Fehlern, die durch Ihre Controlleraktionen oder die Aktionsergebnisse Controller ausgelöst. Ausnahmefilter können auch ein Fehler protokolliert wird.

Jeder andere Typ des Filters wird in einer bestimmten Reihenfolge ausgeführt. Wenn zur Steuerung der Reihenfolge, in der Filter desselben Typs ausgeführt werden, soll, können Sie einen Filter Order-Eigenschaft festlegen.

Ist die Basisklasse für alle Aktionsfilter der `System.Web.Mvc.FilterAttribute` Klasse. Wenn Sie eine bestimmte Art von Filter implementieren möchten, müssen Sie eine Klasse erstellen, die von der Basisklasse Filter erbt und implementiert eine oder mehrere der IAuthorizationFilter, IActionFilter, IResultFilter oder ExceptionFilter-Schnittstellen.

### <a name="the-base-actionfilterattribute-class"></a>Die Basisklasse von ActionFilterAttribute ab

Um Ihnen das Implementieren eines benutzerdefinierten Aktionsfilters erleichtern, enthält das ASP.NET MVC-Framework eine Base `ActionFilterAttribute` Klasse. Diese Klasse implementiert die `IActionFilter` und `IResultFilter` Schnittstellen und erbt von der `Filter` Klasse.

Hier die Terminologie ist nicht vollständig konsistent. Technisch gesehen ist eine Klasse, die von der Klasse ActionFilterAttribute erbt einen Aktionsfilter und einen Ergebnisfilter. Allerdings ist insofern lose der Aktionsfilter Word verwendet, mit jeder Art von Filter in ASP.NET MVC-Framework verweisen.

Die Basisklasse von ActionFilterAttribute hat die folgenden Methoden, die Sie überschreiben können:

- OnActionExecuting – ist diese Methode aufgerufen, bevor eine Controlleraktion ausgeführt wird.
- OnActionExecuted – ist diese Methode aufgerufen, nachdem eine Controlleraktion ausgeführt wird.
- OnResultExecuting – ist diese Methode aufgerufen, bevor ein Aktionsergebnis Controller ausgeführt wird.
- OnResultExecuted – ist diese Methode aufgerufen, nachdem ein Aktionsergebnis Controller ausgeführt wird.

Im nächsten Abschnitt sehen wir, wie Sie jede dieser anderen Methoden implementieren können.

### <a name="creating-a-log-action-filter"></a>Erstellen einen Aktionsfilter Protokoll

Um zu veranschaulichen, wie Sie eine benutzerdefinierte Aktionsfilter erstellen können, erstellen wir ein benutzerdefinierten Aktionsfilters, das die Phasen der Verarbeitung einer Controlleraktion der Visual Studio-Ausgabefenster protokolliert. Unsere `LogActionFilter` auflisten 2 enthalten ist.

**Auflisten von 2 – `ActionFilters\LogActionFilter.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample2.vb)]

Auflisten von 2 die `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`, und `OnResultExecuted()` Aufrufen von Methoden, die alle die `Log()` Methode. Der Name der Methode und die aktuelle Routendaten übergeben wird, um die `Log()` Methode. Die `Log()` -Methode schreibt eine Meldung im Ausgabefenster von Visual Studio (siehe Abbildung 2).


[![Das Schreiben in das Ausgabefenster von Visual Studio](understanding-action-filters-vb/_static/image5.png)](understanding-action-filters-vb/_static/image4.png)

**Abbildung 02**: Schreiben in das Ausgabefenster von Visual Studio ([klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-action-filters-vb/_static/image6.png))


Der Home-Controller im Codebeispiel 3 wird veranschaulicht, wie Sie die Protokoll-Action-Filter auf eine gesamte Controllerklasse anwenden können. Sobald die Aktionen, die verfügbar gemacht werden, indem Sie den Home-Controller aufgerufen werden – entweder die `Index()` Methode oder die `About()` Methode – die Phasen der Verarbeitung die Aktion in einer Visual Studio-Ausgabefenster protokolliert werden.

**Auflisten von 3: `Controllers\HomeController.vb`**

[!code-vb[Main](understanding-action-filters-vb/samples/sample3.vb)]

### <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wurden Sie zu ASP.NET MVC-Aktionsfilter eingeführt. Sie haben gelernt, über die vier verschiedenen Arten von Filtern: Autorisierungsfilter, Aktionsfilter Ergebnisfilter und Ausnahmefilter. Sie haben auch erfahren zur Basis `ActionFilterAttribute` Klasse.

Schließlich haben Sie gelernt, wie Sie einen einfachen Aktionsfilter zu implementieren. Es erstellt einen Protokoll Action-Filter, der die Phasen der Verarbeitung einer Controlleraktion der Visual Studio-Ausgabefenster protokolliert.

> [!div class="step-by-step"]
> [Zurück](asp-net-mvc-routing-overview-vb.md)
> [Weiter](improving-performance-with-output-caching-vb.md)
