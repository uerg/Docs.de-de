---
title: Filter
author: ardalis
description: Erfahren Sie, wie *Filter* funktionieren, und wie Sie sie in ASP.NET Core MVC verwenden.
manager: wpickett
ms.author: tdykstra
ms.date: 12/12/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/filters
ms.openlocfilehash: 8549083ad42f3b81f850c0572b36dd99c4f50350
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="filters"></a>Filter

Von [Tom Dykstra](https://github.com/tdykstra/) und [Steve Smith](https://ardalis.com/)

In ASP.Net Core MVC ermöglichen *Filter* Ihnen, Code vor oder nach bestimmten Stufen der Anforderungsverarbeitungspipeline auszuführen.

 Eingebaute Filter behandeln Aufgaben wie die Autorisierung (Verhindern des Zugriffs auf Ressourcen, die für einen Benutzer nicht genehmigt sind), wodurch sichergestellt wird, dass alle Anforderungen HTTPS verwenden und Antworten zwischenspeichern (Kurzschließen der Anforderungspipeline für die Rückgabe einer zwischengespeicherten Antwort). 

Sie können benutzerdefinierte Filter erstellen, um übergreifende Belange für Ihre Anwendung zu behandeln. Filter sind die Lösung, wenn Sie vermeiden möchten, Code über mehrere Aktionen hinweg zu duplizieren. Sie können zum Beispiel Code für die Fehlerbehandlung in einem Ausnahmefilter konsolidieren.

[Beispiel anzeigen oder von GitHub herunterladen](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).

## <a name="how-do-filters-work"></a>Wie funktionieren Filter?

Filter werden in der *MVC-Aktionsaufrufpipeline* ausgeführt, die manchmal auch als *Filterpipeline* bezeichnet wird.  Die Filterpipeline wird ausgeführt, nachdem MVC die auszuführende Aktion ausgewählt hat.

![Die Anforderung wird von weiterer Middleware, Routingmiddleware, Aktionsauswahl und der MVC-Aktionsaufrufpipeline verarbeitet. Die Verarbeitung von Anforderungen verläuft weiterhin durch Aktionsauswahl, Routingmiddleware und verschiedenen weiteren Middlewares, bevor eine Antwort an den Client gesendet wird.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a>Filtertypen

Jeder Filtertyp wird zu einem anderen Zeitpunkt in der Filterpipeline ausgeführt.

* [Autorisierungfilter](#authorization-filters) werden zuerst ausgeführt und werden verwendet, um festzustellen, ob der aktuelle Benutzer für die aktuelle Anforderung berechtigt ist. Sie können die Pipeline kurzschließen, wenn eine Anforderung nicht autorisiert ist. 

* [Ressourcenfilter](#resource-filters) sind die ersten Filter, die eine Anforderung nach der Autorisierung behandeln.  Diese Filter können vor der restlichen Filterpipeline, und nachdem der Rest der Filterpipeline abgeschlossen wurde, Code ausführen. Sie helfen dabei, das Zwischenspeichern zu implementieren, oder beim Kurzschließen der Filterpipeline zur Verbesserung der Leistung. Da sie vor der Modellbindung ausgeführt werden, sind sie für alles nützlich, was Einfluss auf die Modellbindung ausüben muss.

* [Aktionsfilter](#action-filters) können, vor und nach dem Aufruf einer individuelle Aktionsmethode, Code ausführen. Sie können zur Bearbeitung der Argumente, die an eine Aktion übermittelt werden, und des Ergebnisses, das von der Aktion zurückgegeben wird, verwendet werden.

* [Ausnahmefilter](#exception-filters) werden dazu verwendet, globale Richtlinien auf unbehandelte Ausnahmen anzuwenden, die auftreten, bevor etwas in den Antworttext geschrieben wurde.

* [Ergebnisfilter](#result-filters) können, vor und nach dem Ergebnis einer ausgeführten individuellen Aktion, Code ausführen. Sie werden nur ausgeführt, wenn die Aktionsmethode erfolgreich ausgeführt wurde, und wenn sie für eine Logik nützlich sind, die die Ausführung eines Formatierers oder einer Ansicht umgeben muss.

Das folgende Diagramm veranschaulicht, wie diese Filtertypen mit der Filterpipeline interagieren.

![Die Anforderung wird von Autorisierungsfilter, Ressourcenfilter, Modellbindung, Aktionsfilter, Aktionsausführung sowie Aktionsergebniskonvertierung, Ausnahmefilter, Ergebnisfilter und Ergebnisausführung verarbeitet. Beim Verlassen der Pipeline wird die Anforderung nur von Ergebnisfiltern und Ressourcenfiltern verarbeitet, bevor sie an den Client gesendet wird.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a>Implementierung

Filter unterstützen sowohl synchrone als auch asynchrone Implementierungen über verschiedene Schnittstellendefinitionen. Wählen Sie je nach Art der auszuführenden Aufgabe entweder synchron oder asynchron. 

Synchrone Filter, die Code sowohl vor als auch nach ihrer Pipeline ausführen können, definieren die Methoden On*Stage*Executing und On*Stage*Executed. Zum Beispiel wird `OnActionExecuting` aufgerufen, bevor die Aktionsmethode aufgerufen wird, und `OnActionExecuted` wird aufgerufen, nachdem die Aktionsmethode zurückgegeben wird.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?highlight=6,8,13)]

Asynchrone Filter definieren eine einzelne On*Stage*ExecutionAsync-Methode. Diese Methode verwendet einen *FilterType*ExecutionDelegate-Delegaten, der die Pipelinestufe des Filters ausführt. Zum Beispiel ruft `ActionExecutionDelegate` die Aktionsmethode auf, und Sie können Code ausführen, bevor oder nachdem Sie sie aufrufen.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleAsyncActionFilter.cs?highlight=6,8-10,13)]

Sie können Schnittstellen für mehrere Filterstufen in eine einzelne Klasse implementieren. Zum Beispiel implementiert die abstrakte Klasse [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) sowohl `IActionFilter` und `IResultFilter` als auch deren asynchrone Gegenstücke.

> [!NOTE]
> Implementieren Sie **entweder** die synchrone oder asynchrone Version einer Filterschnittstelle, aber nicht beide. Das Framework prüft zuerst, ob der Filter die asynchrone Schnittstelle implementiert, und wenn dies der Fall ist, ruft es sie auf. Wenn dies nicht der Fall ist, ruft es die Methode(n) der synchronen Schnittstelle auf. Wenn Sie beide Schnittstellen für eine Klasse implementieren würden, würde nur die asynchrone Methode aufgerufen werden. Wenn Sie abstrakte Klassen wie [ActionFilterAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionfilterattribute) verwenden, würden Sie nur die synchronen oder asynchronen Methoden für jeden Filtertyp überschreiben.

### <a name="ifilterfactory"></a>IFilterFactory

`IFilterFactory` implementiert `IFilter`. Deshalb kann eine `IFilterFactory`-Instanz an einer beliebigen Stelle in der Filterpipeline als `IFilter`-Instanz verwendet werden. Wenn das Framework den Aufruf des Filters vorbereitet, versucht es ihn in `IFilterFactory` umzuwandeln. Wenn diese Umwandlung gelingt, wird die Methode `CreateInstance` aufgerufen, um die Instanz `IFilter` zu erstellen, die aufgerufen wird. Dies bietet ein sehr flexibles Design, da die exakte Filterpipeline nicht explizit festgelegt sein muss, wenn die Anwendung gestartet wird.

Als weiteres Verfahren zum Erstellen von Filtern, können Sie `IFilterFactory` in Ihre eigenen Implementierungen von Attributen integrieren:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

### <a name="built-in-filter-attributes"></a>Integrierte Filterattribute

Das Framework enthält integrierte attributbasierte Filter, die Sie als Unterklasse erstellen und anpassen können. Zum Beispiel fügt der folgende Ergebnisfilter der Antwort einen Header hinzu.

<a name="add-header-attribute"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/AddHeaderAttribute.cs?highlight=5,16)]

Wie im obigen Beispiel dargestellt wird, können Filter mithilfe von Attributen Argumente akzeptieren. Sie können dieses Attribut einem Controller oder einer Aktionsmethode hinzufügen, und den Namen und Wert des HTTP-Headers angeben:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

Das Ergebnis der Aktion `Index` wird unten angezeigt – die Antwortheader werden unten rechts angezeigt.

![Entwicklertools von Microsoft Edge zeigen Antwortheader an, einschließlich Autor Steve Smith @ardalis](filters/_static/add-header.png)

Einige der Filterschnittstellen besitzen entsprechende Attribute, die als Basisklassen für benutzerdefinierte Implementierungen verwendet werden können.

Filterattribute:

* `ActionFilterAttribute`
* `ExceptionFilterAttribute`
* `ResultFilterAttribute`
* `FormatFilterAttribute`
* `ServiceFilterAttribute`
* `TypeFilterAttribute`

`TypeFilterAttribute` und `ServiceFilterAttribute` werden [weiter unten in diesem Artikel](#dependency-injection) erklärt.

## <a name="filter-scopes-and-order-of-execution"></a>Filterbereiche und Reihenfolge der Ausführung

Der Pipeline kann an einem von drei *Bereichen* ein Filter hinzugefügt werden. Sie können einen Filter einer bestimmten Aktionsmethode oder Controllerklasse hinzufügen, indem Sie ein Attribut verwenden. Sie können einen Filter auch global registrieren (für alle Controller und Aktionen), indem Sie ihn der Auflistung `MvcOptions.Filters` in der Methode `ConfigureServices` der Klasse `Startup` hinzufügen:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-8)]

### <a name="default-order-of-execution"></a>Standardreihenfolge der Ausführung

Wenn es mehrere Filter für eine bestimmte Stufe der Pipeline gibt, bestimmt der Bereich die Standardreihenfolge der Filterausführung.  Globale Filter umfassen Klassenfilter, welche hingegen Methodenfilter umfassen. Diese Verschachtelung ähnelt russischen Puppen, weil jede Erweiterung des Bereichs den vorherigen Bereich umschließt wie eine [Matroschka](https://wikipedia.org/wiki/Matryoshka_doll). Normalerweise erhalten Sie das erwünschte überschreibende Verhalten, ohne dass Sie die Reihenfolge explizit bestimmen müssen.

Der *nach dem Ereignis* auszuführende Filtercode wird in umgekehrter Reihenfolge zum Code *vor dem Ereignis* ausgeführt. Die Sequenz sieht folgendermaßen aus:

* Der *before*-Code der Filter, die global angewendet werden
  * Der *before*-Code der Filter, die auf Controller angewendet werden
    * Der *before*-Code der Filter, die auf Aktionsmethoden angewendet werden
    * Der *after*-Code der Filter, die auf Aktionsmethoden angewendet werden
  * Der *after*-Code der Filter, die auf Controller angewendet werden
* Der *after*-Code der Filter, die global angewendet werden
  
Hier ist ein Beispiel, das die Reihenfolge, in der Filtermethoden für synchrone Aktionsfilter aufgerufen werden, veranschaulicht.

| Sequenz | Filterbereich | Filtermethode |
|:--------:|:------------:|:-------------:|
| 1 | Global | `OnActionExecuting` |
| 2 | Controller | `OnActionExecuting` |
| 3 | Methode | `OnActionExecuting` |
| 4 | Methode | `OnActionExecuted` |
| 5 | Controller | `OnActionExecuted` |
| 6 | Global | `OnActionExecuted` |

Diese Sequenz zeigt, dass der Methodenfilter im Controllerfilter geschachtelt ist, und dass der Controllerfilter im globalen Filter geschachtelt ist. Anders ausgedrückt: Wenn Sie sich in der Methode On*Stage*ExecutionAsync des asynchronen Filters befinden, werden alle Filter mit einem engeren Bereich ausgeführt, während Ihr Code sich auf dem Stapel befindet.

> [!NOTE]
> Jeder Controller, der von der Basisklasse `Controller` erbt, enthält die Methoden `OnActionExecuting` und `OnActionExecuted`. Diese Methode umschließt die Filter, die für eine bestimmte Aktion ausgeführt werden: `OnActionExecuting` wird vor allen anderen Filtern aufgerufen, und `OnActionExecuted` wird nach allen Filtern aufgerufen.

### <a name="overriding-the-default-order"></a>Überschreiben der Standardreihenfolge

Sie können die Standardsequenz der Ausführung überschreiben, indem Sie `IOrderedFilter` implementieren. Diese Schnittstelle macht die Eigenschaft `Order` verfügbar, die bei der Bestimmung der Ausführungsreihenfolge Vorrang vor dem Bereich hat. Bei einem Filter mit einem niedrigeren `Order`-Wert wird der *before*-Code vor dem Code eines Filters mit einem höheren `Order`-Wert ausgeführt. Bei einem Filter mit einem niedrigeren `Order`-Wert wird der *after*-Code nach dem Code eines Filters mit einem höheren `Order`-Wert ausgeführt. Sie können einen Konstruktorparameter verwenden, um die Eigenschaft `Order` festzulegen:

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

Wenn Sie dieselben drei Aktionsfilter wie im obigen Beispiel verwenden, die Eigenschaft `Order` des Controllers und des globalen Filters jedoch auf jeweils 1 und 2 festlegen, wäre die Ausführungsreihenfolge umgekehrt.

| Sequenz | Filterbereich | `Order`-Eigenschaft | Filtermethode |
|:--------:|:------------:|:-----------------:|:-------------:|
| 1 | Methode | 0 | `OnActionExecuting` |
| 2 | Controller | 1  | `OnActionExecuting` |
| 3 | Global | 2  | `OnActionExecuting` |
| 4 | Global | 2  | `OnActionExecuted` |
| 5 | Controller | 1  | `OnActionExecuted` |
| 6 | Methode | 0  | `OnActionExecuted` |

Die Eigenschaft `Order` hat eine höhere Priorität als der Bereich bei der Bestimmung der Reihenfolge, in der Filter ausgeführt werden. Filter werden erst nach der Reihenfolge sortiert, und dann werden Gleichstände über den Bereich aufgelöst. Alle integrierten Filter implementieren `IOrderedFilter` und legen den Standardwert von `Order` auf 0 (null) fest, damit der Bereich die Reihenfolge bestimmt, sofern Sie `Order` auf einen Wert festlegen, der nicht 0 (null) ist.

## <a name="cancellation-and-short-circuiting"></a>Abbrechen und Kurzschließen

Sie können die Filterpipeline jederzeit kurzschließen, indem Sie die Eigenschaft `Result` auf den Parameter `context` in der Filtermethode festlegen. Zum Beispiel verhindert der folgende Ressourcenfilter, dass der Rest der Pipeline ausgeführt wird.

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?highlight=12,13,14,15)]

Im folgenden Code verwenden sowohl der Filter `ShortCircuitingResourceFilter` und der Filter `AddHeader` die Aktionsmethode `SomeResource` als Ziel. Da `ShortCircuitingResourceFilter` allerdings zuerst ausgeführt wird (weil es ein Ressourcenfilter und `AddHeader` ein Aktionsfilter ist) und die restliche Pipeline kurzschließt, wird der Filter `AddHeader` nicht für die Aktion `SomeResource` ausgeführt. Dieses Verhalten wäre unverändert, wenn beide Filter auf Aktionsmethodenebene angewandt werden würden, wenn `ShortCircuitingResourceFilter` zuerst ausgeführt wird (z.B. wegen seines Filtertyps oder expliziter Verwendung der Eigenschaft `Order`).

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a>Dependency Injection

Filter können nach Typ oder Instanz hinzugefügt werden. Wenn Sie eine Instanz hinzufügen, wird diese Instanz für jede Anforderung verwendet. Wenn Sie einen Typ hinzufügen, wird der Filter typ-aktiviert, d.h. für jede Anforderung wird eine Instanz erstellt, und alle Konstruktorabhängigkeiten werden durch [Dependency Injection](../../fundamentals/dependency-injection.md) (DI) aufgefüllt. Das Hinzufügen eines Filters über einen Typ entspricht `filters.Add(new TypeFilterAttribute(typeof(MyFilter)))`.

Filter, die als Attribute implementiert und Controllerklassen oder Aktionsmethoden direkt hinzugefügt werden, können keine Konstruktorabhängigkeiten von [Dependency Injection](../../fundamentals/dependency-injection.md) (DI) erhalten. Dies liegt daran, dass die Konstruktorparameter für Attribute dort bereitgestellt werden müssen, wo sie angewendet werden. Dies ist eine Einschränkung der Funktionsweise von Attributen.

Wenn Ihre Filter Abhängigkeiten enthalten, die Sie von DI beziehen, gibt es mehrere unterstützte Ansätze. Sie können Ihren Filter auf eine Klasse oder eine Aktionsmethode anwenden, indem Sie eine der folgenden Methoden verwenden:

* `ServiceFilterAttribute`
* `TypeFilterAttribute`
* `IFilterFactory` in Ihrem Attribut implementiert

> [!NOTE]
> Sie sollten die Protokollierung von DI verwenden. Es wird jedoch empfohlen, dass Sie vermeiden, Filter ausschließlich für die Protokollierung zu erstellen und zu verwenden, da [integrierte Frameworkprotokollierungsfeatures](xref:fundamentals/logging/index) möglicherweise bereits das enthalten, was Sie brauchen. Wenn Sie Ihren Filtern die Protokollierung hinzufügen, sollten diese sich auf Probleme mit Geschäftsdomänen oder auf das spezifische Verhalten Ihres Filters beziehen anstelle von MVC-Aktionen oder anderen Frameworkereignissen.

### <a name="servicefilterattribute"></a>ServiceFilterAttribute

Ein `ServiceFilter` ruft eine Instanz des Filter von DI ab. Fügen Sie dem Container den Filter in `ConfigureServices` hinzu, und verweisen Sie auf ihn in einem `ServiceFilter`-Attribut.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=11)]

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

Die Verwendung von `ServiceFilter` ohne Registrierung der Filtertypergebnisse löst eine Ausnahme aus:

```
System.InvalidOperationException: No service for type
'FiltersSample.Filters.AddHeaderFilterWithDI' has been registered.
```

Durch `ServiceFilterAttribute` wird `IFilterFactory` implementiert, wodurch eine einzelne Methode für die Erstellung einer `IFilter`-Instanz verfügbar gemacht wird. Im Fall von `ServiceFilterAttribute` wird die Methode `CreateInstance` der Schnittstelle `IFilterFactory` implementiert, um den angegebenen Typ des Dienstecontainers (DI) zu laden.

### <a name="typefilterattribute"></a>TypeFilterAttribute

`TypeFilterAttribute` ähnelt `ServiceFilterAttribute` (und implementiert ebenfalls `IFilterFactory`), aber sein Typ wird nicht direkt vom DI-Container aufgelöst. Stattdessen wird der Typ mithilfe von `Microsoft.Extensions.DependencyInjection.ObjectFactory` instanziiert.

Aufgrund dieses Unterschieds müssen Typen, auf die mithilfe von `TypeFilterAttribute` verwiesen wird, nicht zuerst im Container registriert werden (dennoch werden ihre Abhängigkeiten vom Container erfüllt). Darüber hinaus kann `TypeFilterAttribute` Konstruktorargumente optional für den betreffenden Typ akzeptieren. Das folgende Beispiel demonstriert, wie Sie Argumente durch Verwendung von `TypeFilterAttribute` an einen Typ übergeben:

[!code-csharp[Main](../../mvc/controllers/filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

Wenn Sie einen Filter haben, der keine Argumente benötigt, aber Konstruktorabhängigkeiten besitzt, die durch DI erfüllt werden müssen, können Sie Ihr eigenes benanntes Attribut auf Klassen und Methoden anwenden (anstelle von `[TypeFilter(typeof(FilterType))]`). Im folgenden Filter wird gezeigt, wie dies implementiert werden kann:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

Dieser Filter kann mithilfe der Syntax `[SampleActionFilter]` auf Klassen oder Methoden angewendet werden, um nicht `[TypeFilter]` oder `[ServiceFilter]` verwenden zu müssen.

## <a name="authorization-filters"></a>Autorisierungsfilter

*Autorisierungsfilter* steuern den Zugriff auf Aktionsmethoden und sind die ersten Filter, die in der Filterpipeline ausgeführt werden. Sie haben nur eine before-Methode, im Gegensatz zu den meisten Filtern, die before- und after-Methoden unterstützen. Sie sollten nur einen eigenen Autorisierungsfilter schreiben, wenn Sie Ihr eigenes Autorisierungsframework schreiben. Statt einen benutzerdefinierten Filter zu schreiben, wird empfohlen, dass Sie Ihre Autorisierungsrichtlinien konfigurieren oder eine benutzerdefinierte Autorisierungsrichtlinie schreiben. Die Implementierung eines integrierten Filters ist nur für das Aufrufen des Autorisierungssystems zuständig.

Beachten Sie, dass Sie Ausnahmen nicht innerhalb von Autorisierungsfiltern auslösen sollten, da die Ausnahme dann nicht behandeln würde (Ausnahmefilter behandeln sie nicht). Verwenden Sie stattdessen eine Abfrage, oder finden Sie einen anderen Weg.

Weitere Informationen finden Sie unter [Autorisierung](../../security/authorization/index.md).

## <a name="resource-filters"></a>Ressourcenfilter

*Ressourcenfilter* implementieren entweder die `IResourceFilter`- oder `IAsyncResourceFilter`-Schnittstelle, und ihre Ausführung umschließt einen Großteil der Filterpipeline. (Nur [Autorisierungsfilter](#authorization-filters) werden vor ihnen ausgeführt.) Ressourcenfilter sind besonders nützlich, wenn Sie die meiste Arbeit einer Anforderung kurzschließen müssen. Ein zwischenspeichernder Filter kann z.B. den Rest der Pipeline umgehen, wenn die Antwort sich bereits im Cache befindet.

Der weiter oben dargestellte [kurzschließende Ressourcenfilter](#short-circuiting-resource-filter) ist ein Beispiel für einen Ressourcenfilter. Ein weiteres Beispiel ist [DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/1.1.1/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs), was verhindert, dass die Modellbindung auf Formulardaten zugreift. Es ist nützlich, wenn Sie wissen, dass Sie große Dateiuploads erhalten werden, und verhindern wollen, dass das Formular in den Arbeitsspeicher gelesen wird.

## <a name="action-filters"></a>Aktionsfilter

*Aktionsfilter* implementieren entweder die `IActionFilter`- oder `IAsyncActionFilter`-Schnittstelle, und ihre Ausführung umgibt die Ausführung von Aktionsmethoden.

Hier ist ein Beispiel für einen Aktionsfilter:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/SampleActionFilter.cs?name=snippet_ActionFilter)]

[ActionExecutingContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutingcontext) stellt die folgenden Eigenschaften zur Verfügung:

* `ActionArguments` erlaubt Ihnen, die Bearbeitung der Eingaben in die Aktion.
* `Controller` erlaubt Ihnen die Bearbeitung der Controllerinstanz. 
* `Result` schließt die Ausführung der Aktionsmethode und nachfolgende Aktionsfilter kurz. Eine Ausnahme auszulösen verhindert auch die Ausführung der Aktionsmethode und nachfolgender Filter, wird jedoch wie ein Fehler behandelt, und nicht wie ein erfolgreiches Ergebnis.

[ActionExecutedContext](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.filters.actionexecutedcontext) stellt `Controller` und `Result` sowie folgende Eigenschaften zur Verfügung:

* `Canceled` ist TRUE, wenn die Aktionsausführung durch einen anderen Filter kurzgeschlossen wurde.
* `Exception` ist ungleich NULL, wenn die Aktion oder ein nachfolgender Aktionsfilter eine Ausnahme ausgelöst hat. Diese Eigenschaft auf NULL zu setzen, „behandelt“ eine Ausnahme, und `Result` wird ausgeführt, als ob es standardgemäß von der Aktionsmethode zurückgegeben wurde.

Bei einem `IAsyncActionFilter` führt ein Aufruf von `ActionExecutionDelegate` alle nachfolgenden Aktionsfilter und die Aktionsmethode aus und gibt `ActionExecutedContext` zurück. Weisen Sie einer Ergebnisinstanz `ActionExecutingContext.Result` zu, und rufen Sie nicht `ActionExecutionDelegate` auf, um die Pipeline kurzzuschließen.

Das Framework stellt ein abstraktes `ActionFilterAttribute` bereit, dass Sie als Unterklasse verwenden können. 

Sie können einen Aktionsfilter verwenden, um den Modellstatus automatisch zu überprüfen und Fehler zurückzugeben, wenn der Status ungültig ist:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/ValidateModelAttribute.cs)]

Die Methode `OnActionExecuted` wird nach der Aktionsmethode ausgeführt, und sie kann die Ergebnisse der Aktion durch die Eigenschaft `ActionExecutedContext.Result` sehen und bearbeiten. `ActionExecutedContext.Canceled` wird auf TRUE festgelegt, wenn die Ausführung der Aktion durch einen anderen Filter kurzgeschlossen wurde. `ActionExecutedContext.Exception` wird auf einen Wert festgelegt, der ungleich NULL ist, wenn eine Aktion oder ein nachfolgender Aktionsfilter eine Ausnahme ausgelöst hat. `ActionExecutedContext.Exception` auf NULL zu setzen, „behandelt“ eine Ausnahme, und `ActionExectedContext.Result` wird dann ausgeführt, als ob es standardgemäß von der Aktionsmethode zurückgegeben wurde.

## <a name="exception-filters"></a>Ausnahmefilter

*Ausnahmefilter* implementieren entweder die `IExceptionFilter`- oder `IAsyncExceptionFilter`-Schnittstelle. Sie können verwendet werden, um gängige Fehlerbehandlungsrichtlinien für eine App zu implementieren. 

Der folgende Beispielausnahmefilter verwendet eine benutzerdefinierte Fehleransicht für Entwickler, um Details über Ausnahmen anzuzeigen, die auftreten, wenn die Anwendung sich in der Entwicklung befindet:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=1,14)]

Ausnahmefilter besitzen keine zwei Ereignisse (für „before“ und „after“) – sie implementieren nur `OnException` (oder `OnExceptionAsync`). 

Ausnahmefilter behandeln unbehandelte Ausnahmen, die bei der Controllererstellung, [Modellbindung](../models/model-binding.md), Aktionsfiltern oder bei Aktionsmethoden auftreten. Sie erfassen keine Ausnahmen, die in Ressourcenfiltern, Ergebnisfiltern oder bei der Ausführung von MVC-Ergebnissen auftreten.

Legen Sie die Eigenschaft `ExceptionContext.ExceptionHandled` auf TRUE fest, oder schreiben Sie eine Antwort, um eine Ausnahme zu behandeln. Dadurch wird die Weitergabe der Ausnahme beendet. Beachten Sie, dass ein Ausnahmefilter eine Ausnahme nicht in einen „Erfolg“ umwandeln kann. Nur ein Aktionsfilter kann das.

> [!NOTE]
> In ASP.NET 1.1 wird die Antwort nicht gesendet, wenn Sie `ExceptionHandled` auf TRUE festlegen **und** eine Antwort schreiben. In diesem Szenario sendet ASP.NET Core 1.0 die Antwort und ASP.NET Core 1.1.2 kehrt zum Verhalten von 1.0 zurück. Weitere Informationen finden Sie unter [Problem #5594](https://github.com/aspnet/Mvc/issues/5594) im GitHub-Repository. 

Ausnahmefilter eignen sich zum Auffangen von Ausnahmen, die in MVC-Aktionen auftreten. Sie sind jedoch nicht so flexibel wie Middleware für die Fehlerbehandlung. Es wird empfohlen, dass Sie Middleware für allgemeine Fälle vorziehen und Filter nur dann verwenden, wenn Sie die Fehlerbehandlung *auf einem anderen Weg* angehen müssen, je nachdem, welche MVC-Aktion gewählt wurde. Zum Beispiel besitzt Ihre App möglicherweise Aktionsmethoden für beide API-Endpunkte und für Ansichten bzw. HTML. Die API-Endpunkte können Fehlerinformationen als JSON zurückgeben, während ansichtsbasierte Aktionen eine Fehlerseite als HTML zurückgeben können.

Das Framework stellt ein abstraktes `ExceptionFilterAttribute` bereit, dass Sie als Unterklasse verwenden können. 

## <a name="result-filters"></a>Ergebnisfilter

*Ergebnisfilter* implementieren entweder die `IResultFilter`- oder `IAsyncResultFilter`-Schnittstelle, und ihre Ausführung umgibt die Ausführung von Aktionsergebnissen. 

Hier ist ein Beispiel für einen Ergebnisfilter, der einen HTTP-Header hinzufügt.

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

Die Art des Ergebnisses, das ausgeführt wird, hängt von der jeweiligen Aktion ab. Eine MVC-Aktion, die eine Ansicht zurückgibt, würde alle Razor-Verarbeitungen als Teil der Ausführung von `ViewResult` beinhalten. Eine API-Methode führt möglicherweise eine Art Serialisierung als Teil der Ausführung des Ergebnisses durch. Weitere Informationen zu [Aktionsergebnissen](actions.md)

Ergebnisfilter werden nur für erfolgreiche Ergebnisse ausgeführt, wenn also die Aktion oder Aktionsfilter ein Aktionsergebnis erzeugen. Ergebnisfilter werden nicht ausgeführt, wenn Ausnahmefilter eine Ausnahme behandeln.

Die Methode `OnResultExecuting` kann die Ausführung des Aktionsergebnisses und nachfolgende Ergebnisfilter durch Festlegen von `ResultExecutingContext.Cancel` auf TRUE kurzschließen. Im Allgemeinen sollten Sie beim Kurzschließen an das Antwortobjekt schreiben, um das Erstellen einer leeren Antwort zu vermeiden. Eine Ausnahme auszulösen, verhindert auch die Ausführung des Aktionsergebnisses und nachfolgender Filter, wird jedoch wie ein Fehler behandelt, und nicht wie ein erfolgreiches Ergebnis.

Wenn die Methode `OnResultExecuted` ausgeführt wird, wurde die Antwort wahrscheinlich an den Client gesendet und kann nicht weiterhin geändert werden (außer es wurde eine Ausnahme ausgelöst). `ResultExecutedContext.Canceled` wird auf TRUE festgelegt, wenn die Ausführung des Aktionsergebnisses durch einen anderen Filter kurzgeschlossen wurde.

`ResultExecutedContext.Exception` wird auf einen Wert festgelegt, der ungleich NULL ist, wenn das Aktionsergebnis oder ein nachfolgender Ergebnisfilter eine Ausnahme ausgelöst hat. Das Festlegen von `Exception` auf NULL „behandelt“ eine Ausnahme und verhindert, dass die Ausnahme zu einem späteren Zeitpunkt in der Pipeline ein weiteres Mal von MVC ausgelöst wird. Wenn Sie eine Ausnahme in einem Ergebnisfilter verarbeiten, sind Sie möglicherweise nicht in der Lage, Daten in die Antwort zu schreiben. Wenn das Aktionsergebnis während der Ausführung ausgelöst wird und die Header bereits an den Client übergeben wurden, gibt es keinen zuverlässigen Mechanismus zum Senden von Fehlercode.

Bei einem `IAsyncResultFilter` führt ein Aufruf von `await next()` auf `ResultExecutionDelegate` alle nachfolgenden Ergebnisfilter und das Aktionsergebnis aus. Legen Sie `ResultExecutingContext.Cancel` auf TRUE fest, und rufen Sie nicht `ResultExectionDelegate` auf, um die Pipeline kurzzuschließen.

Das Framework stellt ein abstraktes `ResultFilterAttribute` bereit, dass Sie als Unterklasse verwenden können. Die weiter oben dargestellte Klasse [AddHeaderAttribute](#add-header-attribute) ist ein Beispiel für ein Ergebnisfilterattribut.

## <a name="using-middleware-in-the-filter-pipeline"></a>Verwenden von Middleware in der Filterpipeline

Ressourcenfilter funktionieren wie [Middleware](../../fundamentals/middleware.md), sie umschließen alle Ausführungen, die später in der Pipeline enthalten sind. Filter unterscheiden sich jedoch insofern von Middleware, dass sie Teil von MVC sind, was heißt, dass sie Zugriff auf MVC-Kontext und Konstrukte haben.

In ASP.NET Core 1.1 können Sie Middleware in der Filterpipeline verwenden. Das sollten Sie tun, wenn Sie eine Middlewarekomponente verwenden, die Zugriff auf MVC-Routendaten benötigt bzw. nur für bestimmte Controller oder Aktionen ausgeführt werden soll.

Erstellen Sie einen Typ mit einer `Configure`-Methode, die die Middleware festlegt, die Sie in die Filterpipeline einfügen möchten, um Middleware als Filter zu verwenden. Hier ist ein Beispiel, dass die Lokalisierungsmiddleware verwendet, um die aktuelle Kultur einer Anforderung einzurichten:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

Anschließend können Sie ein `MiddlewareFilterAttribute` verwenden, um die Middleware für einen ausgewählten Controller, eine Aktion oder global auszuführen:

[!code-csharp[Main](./filters/sample/src/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

Middlewarefilter werden zum selben Zeitpunkt in der Filterpipeline wie Ressourcenfilter, vor der Modellbindung und nach dem Rest der Pipeline ausgeführt.

## <a name="next-actions"></a>Nächste Schritte

[Laden Sie das Beispiel herunter, testen und bearbeiten Sie es](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/filters/sample), um Filter näher kennenzulernen.
