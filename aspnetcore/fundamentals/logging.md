---
title: Protokollierung in ASP.NET Core
author: ardalis
description: "Wird das protokollierungsframework in ASP.NET Core eingeführt. Enthält einen Abschnitt für jede integrierte Protokollierungsanbieter und Links zu einigen beliebten Drittanbieter."
keywords: ASP.NET Core, Protokollierung, Protokollanbieter, Microsoft.Extensions.Logging ILogger iloggerfactory-Standardobjekt zur LogLevel WithFilter TraceSource EventLog, EventSource, Bereiche
ms.author: tdykstra
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ac27ac68-d76a-4f8e-b8ab-ea045803e5f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/logging
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30e00e2a442225bbe04be0d343f7048efe484477
ms.sourcegitcommit: 4e84d8bf5f404bb77f3d41665cf7e7374fc39142
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/05/2017
---
# <a name="introduction-to-logging-in-aspnet-core"></a>Einführung in ASP.NET Core anmelden

Durch [Steve Smith](http://ardalis.com) und [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core unterstützt eine Protokollierung-API, die mit einer Vielzahl von Protokollanbieter funktioniert. Integrierte Anbieter können Sie die Protokolle auf einem oder mehreren Zielen senden, und Sie eine Drittanbieter-protokollierungsframework einbinden können. In diesem Artikel zeigt, wie die integrierte Protokollierung-API und Anbieter in Ihrem Code verwenden.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample2)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/sample)

---

## <a name="how-to-create-logs"></a>Vorgehensweise: Erstellen von Protokollen

Um Protokolle zu erstellen, erhalten eine `ILogger` -Objekt aus der [Abhängigkeitsinjektion](dependency-injection.md) Container:

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Rufen Sie anschließend Protokollierungsmethoden für dieses Protokollierungsobjekt:

[!code-csharp[](logging/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

In diesem Beispiel wird die Protokolle mit der `TodoController` der Klasse die *Kategorie*.  Kategorien werden erläutert [weiter unten in diesem Artikel](#log-category).

ASP.NET Core bietet asynchrone Methoden Protokollierung keine da Protokollierung so schnell sein sollte, dass es sich lohnt die Kosten für das Verwenden von Async. Wenn sind in einer Situation, in denen, nicht "true" ist, erwägen Sie, wie, die Sie sich anmelden.  Datenspeicher langsam ist, Schreiben Sie zuerst die protokollmeldungen in einem schnellen gespeichert, und klicken Sie dann später in einem langsamen Speicher verschieben. Melden Sie sich z. B. in eine Warteschlange, die gelesen und von einem anderen Prozess langsam Speicher beibehalten.

## <a name="how-to-add-providers"></a>Gewusst wie: Hinzufügen von Anbietern

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Ein Protokollierungsanbieter nimmt die Nachrichten, die Sie erstellen ein `ILogger` -Objekt und zeigt an, oder gespeichert werden. Z. B. die Konsole Anbieter angezeigt, in der Konsole, und der Azure App Service-Anbieter können sie im Azure-Blob-Speicher speichern.

Um einen Anbieter zu verwenden, rufen Sie des Anbieters `Add<ProviderName>` Erweiterungsmethode in *"Program.cs"*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Eingerichtet, dass der Standardprojektvorlage Protokollierung die Möglichkeit, die Sie im vorangehenden Code angezeigt, aber die `ConfigureLogging` Aufruf erfolgt durch die `CreateDefaultBuilder` Methode. Hier ist der Code in *"Program.cs"* werden von Projektvorlagen erstellt:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Ein Protokollierungsanbieter nimmt die Nachrichten, die Sie erstellen ein `ILogger` -Objekt und zeigt an, oder gespeichert werden. Z. B. die Konsole Anbieter angezeigt, in der Konsole, und der Azure App Service-Anbieter können sie im Azure-Blob-Speicher speichern.

Um einen Anbieter zu verwenden, die NuGet-Paket installieren, und rufen Sie die Anbieter-Erweiterungsmethode in einer Instanz von `ILoggerFactory`, wie im folgenden Beispiel gezeigt.

[!code-csharp[](logging/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

ASP.NET Core [Abhängigkeitsinjektion](dependency-injection.md) (DI) bietet die `ILoggerFactory` Instanz. Die `AddConsole` und `AddDebug` Erweiterungsmethoden werden definiert, der [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) und [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) Pakete. Jede Erweiterungsmethode Ruft die `ILoggerFactory.AddProvider` -Methode auf und übergibt eine Instanz des Anbieters. 

> [!NOTE]
> Die beispielanwendung für diesen Artikel fügt Protokollanbieter in der `Configure` Methode der `Startup` Klasse. Wenn Sie möchten Code Ausgabeprotokoll entnommen werden, die früher ausgeführt wird, fügen Sie Protokollanbieter in der `Startup` stattdessen-Klassenkonstruktor. 

---

Finden Sie Informationen zu den einzelnen [integrierte Protokollierungsanbieter](#built-in-logging-providers) sowie links zu [eines Drittanbieters Protokollanbieter](#third-party-logging-providers) weiter unten in den Artikel.

## <a name="sample-logging-output"></a>Beispiele für Protokollausgaben

Durch den Beispielcode, der im vorherigen Abschnitt gezeigt wird Protokolle in der Konsole angezeigt werden, wenn Sie über die Befehlszeile ausführen. Hier ist ein Beispiel der Ausgabe in der Konsole:

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/api/todo/0
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 42.9286ms
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 148.889ms 404
```
 
Diese Protokolle erstellt wurden, navigieren Sie zu `http://localhost:5000/api/todo/0`, die Ausführung beider löst `ILogger` Aufrufe, die im vorherigen Abschnitt gezeigt.

Hier ist ein Beispiel für die gleichen Protokolle, wie sie im Debug-Fenster angezeigt werden, wenn Sie die beispielanwendung in Visual Studio ausführen:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404 
```

Die Protokolle, die erstellt wurden die `ILogger` Aufrufe, die im vorherigen Abschnitt gezeigt, die mit "TodoApi.Controllers.TodoController" beginnen. Die Protokolle, die mit "Microsoft" Kategorien beginnen, werden von ASP.NET Core. ASP.NET Core selbst und Anwendungscode verwenden die gleiche API für die Protokollierung und die gleichen Protokollanbieter.

Der Rest dieses Artikels erläutert einige Details und Optionen für die Protokollierung.

## <a name="nuget-packages"></a>NuGet-Pakete

Die `ILogger` und `ILoggerFactory` Schnittstellen sind [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), und standardimplementierungen für sie sind im [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/).

## <a name="log-category"></a>Log-Kategorie

Ein *Kategorie* ist im Lieferumfang jedes Protokoll, die Sie erstellen.  Beim Erstellen, geben Sie die Kategorie ein `ILogger` Objekt. Die Kategorie kann eine beliebige Zeichenfolge sein, aber eine Konvention ist die Verwendung der vollqualifizierte Name der Klasse, von dem die Protokolle geschrieben werden.  Zum Beispiel: "TodoApi.Controllers.TodoController".

Sie können die Kategorie angeben, als Zeichenfolge oder eine Erweiterungsmethode, die die Kategorie aus dem Typ abgeleitet ist. Rufen Sie zum Angeben der Kategorie als Zeichenfolge `CreateLogger` auf eine `ILoggerFactory` Instanz, wie unten dargestellt.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

In den meisten Fällen, es wird einfacher sein, verwenden Sie `ILogger<T>`, wie im folgenden Beispiel.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Dies entspricht dem Aufruf `CreateLogger` mit dem vollqualifizierten Typnamen des `T`.

## <a name="log-level"></a>Protokollebene

Jedes Mal Sie ein Protokoll zu schreiben, Sie geben die [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel). Die Protokollebene gibt den Grad der Schweregrad "oder" Wichtigkeit an.  Sie können z. B. Schreiben einer `Information` zu protokollieren, wenn eine Methode in der Regel beendet eine `Warning` melden Sie sich nach der Rückkehr von eine Methode 404-Rückgabecode zurück, und eine `Error` zu protokollieren, wenn Sie eine unerwartete Ausnahme abfangen.

Im folgenden Codebeispiel wird die Namen der Methoden (z. B. `LogWarning`) Geben Sie die Protokollebene.  Der erste Parameter ist der [Protokollieren der Ereignis-ID](#log-event-id) (weiter unten in diesem Artikel erläutert).  Die verbleibenden Parameter erstellen eine Meldungszeichenfolge Protokoll.

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Log-Methoden, die die Ebene im Methodennamen enthalten sind [Erweiterungsmethoden für ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions). Hinter den Kulissen diese Methoden Aufrufen einer `Log` Methode, eine `LogLevel` Parameter. Sie erreichen die `Log` -Methode direkt statt eines dieser Erweiterungsmethoden, aber die Syntax ist relativ kompliziert. Weitere Informationen finden Sie unter der [ILogger-Schnittstelle](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) und [protokollierungserweiterungen Quellcode](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core definiert die folgenden [Protokollebenen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel), hier vom niedrigsten zu höchsten Schweregrad sortiert.

* Trace = 0

  Informationen, die nur für ein Entwickler ein Problem Debuggen nützlich ist. Diese Nachrichten können vertrauliche Daten enthalten und sollte daher nicht in einer produktiven Umgebung aktiviert werden. *Standardmäßig deaktiviert.* Ein Beispiel: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Debug = 1

  Informationen hat, der kurzfristige Nützlichkeit während der Entwicklung und Debuggen. Beispiel: `Entering method Configure with flag set to true.` Sie in der Regel würde nicht aktivieren `Debug` Ebene in der Produktion protokolliert, es sei denn, Sie wegen hoher Transaktionsvolumen von Protokollen behandeln möchten.

* Informationen = 2

  Für das Nachverfolgen des allgemeinen Ablaufs der Anwendung an. Diese Protokolle werden in der Regel einige langfristigen Wert aufweisen. Ein Beispiel: `Request received for path /api/todo`

* Warnung = 3

  Ungewöhnliche oder unerwartete Ereignisse in den Anwendungsablauf. Diesen zählen möglicherweise Fehler oder aus anderen Gründen nicht beenden die Anwendung zu verursachen, aber die untersucht werden müssen. Behandelte Ausnahmen werden von einer gemeinsamen Stelle zum Verwenden der `Warning` Protokollebene. Ein Beispiel: `FileNotFoundException for file quotes.txt.`

* Fehler = 4

  Für Fehler und Ausnahmen, die nicht behandelt werden können. Diese Meldungen zeigen an, einen Fehler in der aktuellen Aktivität oder einen Vorgang (z. B. die aktuelle HTTP-Anforderung), keine anwendungsweite-Fehler. Beispiel-Nachricht:`Cannot insert record due to duplicate key violation.`

* Kritische = 5

  Für Fehler, die sofortige Aufmerksamkeit erfordern. Beispiele: Datenverluste, nicht genügend Speicherplatz vorhanden.

Die Protokollebene können Sie steuern, wie viel Protokollausgabe für einen bestimmten Speichermedium geschrieben werden oder das Fenster auch anzeigen. Z. B. in einer produktionsumgebung empfiehlt alle Protokolle von `Information` Ebene, und fahren Sie mit einem Datenspeicher Volume und alle Protokolle des senken `Warning` Ebene und höher, fahren Sie mit einem Wert-Datenspeicher. Während der Entwicklung können normalerweise Protokolle senden `Warning` oder einem höheren Schweregrad an die Konsole. Wenn Sie beheben müssen, können Sie hinzufügen `Debug` Ebene. Die [Protokoll filtern](#log-filtering) Abschnitt weiter unten in diesem Artikel wird erläutert, wie die Protokollierungsstufen steuern ein Anbieters behandelt.

Schreibt das Framework ASP.NET Core `Debug` Ebene Protokolle für die Framework-Ereignisse. Die Protokoll-Beispielen weiter oben in diesem Artikel ausgeschlossen Protokolle unten `Information` Ebene, sodass keine `Debug` Ebene Protokolle angezeigt wurden. Hier ist ein Beispiel der konsolenprotokolle, wenn das Ausführen der beispielanwendung, die so konfiguriert, dass `Debug` und höher Protokolle, um die Konsole-Anbieter.

```console
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:62555/api/todo/0
dbug: Microsoft.AspNetCore.Routing.Tree.TreeRouter[1]
      Request successfully matched the route with name 'GetTodo' and template 'api/Todo/{id}'.
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Update (TodoApi)' with id '089d59b6-92ec-472d-b552-cc613dfd625d' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ActionSelector[2]
      Action 'TodoApi.Controllers.TodoController.Delete (TodoApi)' with id 'f3476abe-4bd9-4ad3-9261-3ead09607366' did not match the constraint 'Microsoft.AspNetCore.Mvc.Internal.HttpMethodActionConstraint'
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action TodoApi.Controllers.TodoController.GetById (TodoApi)
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[1]
      Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
info: TodoApi.Controllers.TodoController[1002]
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      GetById(0) NOT FOUND
dbug: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action method TodoApi.Controllers.TodoController.GetById (TodoApi), returned result Microsoft.AspNetCore.Mvc.NotFoundResult.
info: Microsoft.AspNetCore.Mvc.StatusCodeResult[1]
      Executing HttpStatusCodeResult, setting HTTP status code 404
info: Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker[2]
      Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 0.8788ms
dbug: Microsoft.AspNetCore.Server.Kestrel[9]
      Connection id "0HL6L7NEFF2QD" completed keep alive response.
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[2]
      Request finished in 2.7286ms 404
```

## <a name="log-event-id"></a>Melden Sie sich Ereignis-ID

Sie schreiben ein Protokolls, Sie jedes Mal angeben können eine *Ereignis-ID*. Die Beispiel-app wird mit einer lokal definierten `LoggingEvents` Klasse:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](logging/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Ereignis-ID ist ein Ganzzahlwert, den Sie verwenden können, um einen Satz von protokollierten Ereignisse miteinander zu verknüpfen. Beispielsweise könnte ein Protokoll für das Hinzufügen eines Elements zu einem Einkaufswagen Ereignis-ID 1000 und ein Protokoll für einen Kauf abschließen konnte Ereignis-ID 1001 sein.

In die Protokollausgabe kann die Ereignis-ID in einem Feld gespeichert oder in der Textnachricht, je nach Anbieter enthalten.  Der Debug-Anbieter keine Ereignis-IDs anzeigen, aber der Anbieter für die Konsole zeigt sie in Klammern nach der Kategorie:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-format-string"></a>Log-Format Meldungszeichenfolge

Jedes Mal, wenn Sie ein Ereignisprotokoll zu schreiben, geben Sie eine Textnachricht. Die Meldungszeichenfolge kann benannte Platzhalter enthalten, in welches Argument Werte, wie im folgenden Beispiel gezeigt eingefügt werden:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Die Reihenfolge der Platzhalter, nicht ihren Namen, bestimmt, welche Parameter für diese verwendet werden. Wenn Ihr Code beispielsweise folgendermaßen lautet:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Die resultierende protokollmeldung würde wie folgt aussehen:

```
Parameter values: parm1, parm2
```

Das protokollierungsframework message Formatierung auf diese Weise an, für die Protokollierung Anbieter implementieren können [semantische Protokollierung, auch bekannt als strukturierte Protokollierung](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging). Da die Argumente an das ereignisprotokollierungssystem, nicht nur die Zeichenfolge formatierte Nachricht übergeben werden können Protokollanbieter Parameterwerte als Felder zusätzlich zu der die Meldungszeichenfolge speichern. Wenn Sie gerichtet sind die Protokollausgabe, um Azure Table Storage ein, und der Methodenaufruf Protokollierung sieht wie folgt aus:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Jedes Azure-Tabellenentität möglicherweise `ID` und `RequestTime` Eigenschaften, die Abfragen von Protokolldaten zu vereinfachen, würden. Sie alle Protokolle in einer bestimmten gefunden `RequestTime` Bereich, ohne das Timeout der Textnachricht zu analysieren.

## <a name="logging-exceptions"></a>Protokollieren von Ausnahmen

Die Protokollierung-Methoden verfügen über Überladungen, mit denen Sie eine Ausnahme aus, wie im folgenden Beispiel übergeben:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Andere Anbieter behandeln die Ausnahmeinformationen auf unterschiedliche Weise. Hier ist ein Beispiel der Ausgabe der Debug-Anbieter aus der oben gezeigte Code ein.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Protokoll filtern

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Sie können eine minimale Protokollebene für einen bestimmten Anbieter und die Kategorie oder für alle Anbieter oder alle Kategorien angeben.  Alle Protokolle unter dem Mindestwert liegt werden nicht an den Anbieter übergeben, damit sie angezeigt oder gespeichert abrufen nicht. 

Wenn Sie alle Protokolle unterdrücken möchten, können Sie angeben `LogLevel.None` als die minimale Protokollebene. Der ganzzahlige Wert der `LogLevel.None` ist 6. Dies ist höher als `LogLevel.Critical` (5).

**Erstellen von Filterregeln in der Konfiguration**

Die Projektvorlagen erstellen Code, der Aufrufe `CreateDefaultBuilder` zum Einrichten der Protokollierung für die Konsole und Debuggen. Die `CreateDefaultBuilder` Methode auch so eingerichtet, die Protokollierung für die Konfiguration im suchen eine `Logging` mit Code wie im folgenden Abschnitt:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Die Konfigurationsdaten gibt die minimale Protokollebenen, Anbieter und die Kategorie, wie im folgenden Beispiel an:

[!code-json[](logging/sample2/appsettings.json)]

Diese JSON erstellt sechs Filterregeln für den Debug-Anbieter, vier für den Anbieter für die Konsole, und eine, die für alle Anbieter gilt. Sehen Sie, wie später nur eine dieser Regeln für jeden Anbieter ausgewählt wird bei einer `ILogger` Objekt erstellt wird.

**Filterregeln in code**

Sie können im Code Filterregeln registrieren, wie im folgenden Beispiel gezeigt:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Die zweite `AddFilter` gibt den Debug-Anbieter mithilfe der Typname. Die erste `AddFilter` gilt für alle Anbieter, da es einen Anbietertyp angibt.

**Wie das Filtern von Regeln angewendet werden**

Die Konfigurationsdaten werden und die `AddFilter` in den vorherigen Beispielen gezeigten Code erstellen Sie die Regeln, die in der folgenden Tabelle gezeigt. Die ersten sechs stammen aus dem Konfigurationsbeispiel und die letzten beiden stammen aus dem Codebeispiel.

Anzahl|Anbieter|Kategorien, die mit beginnen|Minimale Protokollebene|
------|--------|--------------------------|-----------------|
1|Debuggen|Alle Kategorien|Information|
2|Konsole|Microsoft.AspNetCore.Mvc.Razor.Internal|Warnung|
3|Konsole|Microsoft.AspNetCore.Mvc.Razor.Razor|Debuggen|
4|Konsole|Microsoft.AspNetCore.Mvc.Razor|Fehler|
5|Konsole|Alle Kategorien|Information|
6|Alle Anbieter|Alle Kategorien|Warnung
7|Alle Anbieter|System|Debuggen
8|Debuggen|Microsoft|Ablaufverfolgung

Beim Erstellen einer `ILogger` Objekt zum Schreiben von Protokollen, die `ILoggerFactory` Objekt wählt eine einzelne Regel pro Anbieter angewendet, die diese Protokollierung. Alle Nachrichten, die von diesem geschrieben `ILogger` Objekt basierend auf den ausgewählten Regeln gefiltert werden. Die spezifischsten Regel möglich, dass jeder Anbieter und jede Kategorie Paar wird aus den verfügbaren Regeln ausgewählt.

Der folgende Algorithmus wird für jeden Anbieter verwendet, wenn ein `ILogger` wird nach einer bestimmten Kategorie erstellt:

* Wählen Sie alle Regeln, die den Anbieter oder dessen Alias entsprechen.  Wenn keiner gefunden werden, wählen Sie alle Regeln mit einem leeren Anbieter ein.
* Die aus dem Ergebnis des vorherigen Schritts wählen Sie Regeln mit der längsten Kategorie Präfix. Wenn keiner gefunden werden, wählen Sie alle Regeln, die eine Kategorie angeben.
* Wenn mehrere Regeln ausgewählt sind, nehmen der **letzten** eine.
* Wenn keine Regeln ausgewählt sind, verwenden Sie `MinimumLevel`.
 
Angenommen, Sie haben der vorangehenden Liste der Regeln und erstellen Sie ein `ILogger` Objekt für die Kategorie "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine":

* Für den Debug-Anbieter gelten die Regeln 1, 6 und 8. Regel 8 ist am spezifischsten, damit das Element ausgewählt ist.
* Wenden Sie Regeln, 3, 4, 5 und 6, für den Anbieter Konsole. Regel 3 ist am spezifischsten.

Beim Erstellen von Protokollen mit einer `ILogger` Protokolle auf der Kategorie "Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine" `Trace` Ebene und höher ist mit der Debug-Anbieter und Protokolle von `Debug` Ebene und wird über die Konsole-Anbieter gesendet.

**Anbieter-Aliasen**

Können Sie den Namen ein Anbieters in der Konfiguration angeben, aber jeder Anbieter definiert eine kürzere *Alias* ist einfacher zu verwenden. Verwenden Sie die folgenden Aliase für die integrierte Anbieter:

- Konsole
- Debuggen
- Im Ereignisprotokoll
- AzureAppServices
- TraceSource
- EventSource

**Minimale Standardebene**

Es gibt eine minimale festlegen, die in Kraft tritt nur dann, wenn keine Regeln von Konfigurations- oder Code für einen bestimmten Anbieter und Kategorie gelten. Im folgende Beispiel wird gezeigt, wie die Mindestebene festgelegt wird:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Wenn Sie die minimale Stufe nicht explizit festlegen, der Standardwert ist `Information`, was bedeutet, dass `Trace` und `Debug` Protokolle werden ignoriert.

**Filterfunktionen**

Sie können Code schreiben, in einer Filterfunktion Filterregeln anwenden. Filter-Funktion wird aufgerufen, für alle Anbieter und Kategorien, die keine Regeln, die sie per Konfiguration oder Code zugewiesen haben. Code in der Funktion hat Zugriff auf die Anbietertyp, Kategorie und Protokollebene, um zu entscheiden, und zwar unabhängig davon, ob eine Nachricht protokolliert werden sollen. Zum Beispiel:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Einige Anbieter Protokollierung können Sie angeben, wenn Protokolle auf einem Speichermedium geschrieben oder ignoriert werden sollte, basierend auf Protokollebene und Kategorie zur Verfügung stehen.

Die `AddConsole` und `AddDebug` Erweiterungsmethoden bieten Überladungen, mit denen Sie die Filterkriterien zu übergeben. Der folgende Code bewirkt, dass den Konsole-Anbieter, um Protokolle unten ignorieren `Warning` auf Ebene, während der Debug-Anbieter Protokolle ignoriert, die das Framework erstellt.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

Die `AddEventLog` Methode verfügt über eine Überladung mit einer `EventLogSettings` -Instanz, die möglicherweise eine Filterfunktion im enthält seine `Filter` Eigenschaft. TraceSource-Anbieter bietet keine keines dieser Überladungen seit seiner Protokolliergrad und andere Parameter abhängig von der `SourceSwitch` und `TraceListener` verwendet.

Sie können festlegen, Filtern von Regeln für alle Anbieter, die registriert sind ein `ILoggerFactory` Instanz mithilfe der `WithFilter` Erweiterungsmethode. Das folgende Beispiel schränkt Framework Protokolle (Kategorie beginnt mit "Microsoft" oder "System")-Warnungen während des app-Protokolls auf Debugebene zugelassen.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Wenn Sie möchten die Filterung verwenden, um zu verhindern, dass alle Protokolle, die für eine bestimmte Kategorie geschrieben wird, können Sie angeben `LogLevel.None` als die minimale Protokollebene für diese Kategorie. Der ganzzahlige Wert der `LogLevel.None` ist 6. Dies ist höher als `LogLevel.Critical` (5).

Die `WithFilter` Erweiterungsmethode wird bereitgestellt, indem die [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) NuGet-Paket. Die Methode gibt ein neues `ILoggerFactory` -Instanz, die die protokollmeldungen an alle Logger-Anbieter übergeben gefiltert wird, die bei ihm registrierten. Er hat keinen Einfluss auf andere `ILoggerFactory` Instanzen, einschließlich der ursprünglichen `ILoggerFactory` Instanz.

---

## <a name="log-scopes"></a>Log-Bereiche

Sie können einen Satz von logischen Operationen innerhalb Gruppieren eine *Bereich* um dieselben Daten an jedes Protokoll anfügen, die im Rahmen dieser Menge erstellt wird.  Beispielsweise sollten jedes Protokoll, das als Teil der Verarbeitung einer Transaktions zum Einschließen der Transaktions-ID erstellt, Sie

Ein Bereich ist ein `IDisposable` von zurückgegebene Typ der `ILogger.BeginScope<TState>` -Methode und dauert, bis es entfernt wird. Sie verwenden einen Bereich, indem Wrapping Ruft die Protokollierung in eine `using` block, wie hier gezeigt:

[!code-csharp[](logging/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Der folgende Code ermöglicht Bereiche für den Anbieter Konsole:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

In *"Program.cs"*:

[!code-csharp[](logging/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

In *Startup.cs*:

[!code-csharp[](logging/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

---

Jede Nachricht enthält die Bereichsinformationen:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Integrierte Protokollanbieter

ASP.NET Core liefert die folgenden Anbieter:

* [Konsole](#console)
* [Debuggen](#debug)
* [EventSource](#eventsource)
* [EventLog](#eventlog)
* [TraceSource](#tracesource)
* [Azure App Service](#appservice)

<a id="console"></a>
### <a name="the-console-provider"></a>Die Konsole-Anbieter

Die [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) anbieterpakets sendet die Ausgabe an die Konsole. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddConsole()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddConsole()
```

[AddConsole Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.consoleloggerextensions) können Sie übergeben ein einem minimalen Protokollebene, eine Filterfunktion und einen booleschen Wert ab, der angibt, ob die Bereiche unterstützt werden.  Eine andere Möglichkeit ist die Übergabe in einem `IConfiguration` -Objekt, das Unterstützung von Bereichen und Protokolliergrade angeben kann. 

Wenn Sie die Konsole-Anbieter für die Verwendung in der Produktion in Betracht ziehen, Bedenken Sie, dass es einen entscheidenden Einfluss auf die Leistung auswirkt.

Beim Erstellen eines neuen Projekts in Visual Studio die `AddConsole` -Methode sieht folgendermaßen aus:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Dieser Code bezieht sich auf die `Logging` Teil der *appSettings.json* Datei:

[!code-json[](logging/sample//appsettings.json)]

Die Einstellungen, die Grenzwert Framework Protokolle, um Warnungen beim ermöglicht es der app angezeigt, die auf Debugebene zu melden, wie in beschrieben die [Protokoll filtern](#log-filtering) Abschnitt. Weitere Informationen finden Sie unter [Konfiguration](configuration.md).

---

<a id="debug"></a>
### <a name="the-debug-provider"></a>Der Debug-Anbieter

Die [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) anbieterpakets schreibt Ausgabeprotokoll mithilfe der [System.Diagnostics.Debug](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.debug#System_Diagnostics_Debug) Klasse (`Debug.WriteLine` Methodenaufrufe).

Unter Linux, diesen Anbieter schreibt die Protokolle, um */var/log/message*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddDebug()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddDebug()
```

[AddDebug Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.debugloggerfactoryextensions) können Sie eine minimale Protokollebene oder eine Filterfunktion übergeben.

---

<a id="eventsource"></a>
### <a name="the-eventsource-provider"></a>EventSource-Anbieter

Für apps, die ASP.NET Core 1.1.0 ausgerichtet oder höher, die [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) anbieterpakets ereignisablaufverfolgung implementieren kann. Unter Windows verwendet [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Der Anbieter ist plattformübergreifende allerdings gibt es kein Ereignis Auflistung und der Anzeige Tools sind noch für Linux oder MacOS. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventSourceLogger()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventSourceLogger()
```

---

Eine gute Möglichkeit zum Sammeln und Anzeigen von Protokollen ist die Verwendung der [PerfView-Dienstprogramm](https://www.microsoft.com/download/details.aspx?id=28567). Es gibt andere Tools zum Anzeigen von ETW-Protokolle allerdings PerfView bietet die beste Lösung für das Arbeiten mit ETW-Ereignisse, die von ASP.NET ausgegeben. 

Fügen Sie zum Konfigurieren von PerfView zum Erfassen von Ereignissen, die von diesem Anbieter protokolliert die Zeichenfolge `*Microsoft-Extensions-Logging` auf die **zusätzliche Anbieter** Liste. (Verpassen Sie nicht das Sternchen am Anfang der Zeichenfolge.)

![Zusätzliche Perfview-Anbieter](logging/_static/perfview-additional-providers.png)

Erfassen von Ereignissen unter Nano Server sind einige zusätzliche Konfigurationsschritte erforderlich:

* Verbinden Sie PowerShell-Remoting mit Nano Server:

  ```powershell
  Enter-PSSession [name]
  ```

* Erstellen Sie eine ETW-Sitzung:

  ```powershell
  New-EtwTraceSession -Name "MyAppTrace" -LocalFilePath C:\trace.etl
  ```

* Hinzufügen von ETW-Anbietern für [CLR](https://msdn.microsoft.com/library/ff357718), ASP.NET Core und andere je nach Bedarf. Der ASP.NET Core-Anbieter-GUID ist `3ac73b97-af73-50e9-0822-5da4367920d0`. 

  ```powershell
  Add-EtwTraceProvider -Guid "{e13c0d23-ccbc-4e12-931b-d9cc2eee27e4}" -SessionName MyAppTrace
  Add-EtwTraceProvider -Guid "{3ac73b97-af73-50e9-0822-5da4367920d0}" -SessionName MyAppTrace
  ```

* Führen Sie den Standort aus, und möchten Sie die Aktionen aus, die Ablaufverfolgungsinformationen für den.

* Beenden Sie die Ablaufverfolgung-Sitzung, wenn Sie fertig sind:

  ```powershell
  Stop-EtwTraceSession -Name "MyAppTrace"
  ```

Das resultierende *C:\trace.etl* Datei mit PerfView analysiert werden kann, wie auf anderen Editionen von Windows.

<a id="eventlog"></a>
### <a name="the-windows-eventlog-provider"></a>Der Anbieter Windows-Ereignisprotokoll

Die [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) anbieterpakets sendet Ausgabeprotokoll in das Windows-Ereignisprotokoll.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddEventLog()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddEventLog()
```

[AddEventLog Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.eventloggerfactoryextensions) Let übergebenen `EventLogSettings` oder eine minimale Protokollebene.

---

<a id="tracesource"></a>
### <a name="the-tracesource-provider"></a>TraceSource-Anbieter

Die [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) anbieterpakets verwendet die [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) Bibliotheken und Anbietern.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
logging.AddTraceSource(sourceSwitchName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

---

[AddTraceSource Überladungen](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.tracesourcefactoryextensions) können Sie ein Quellschalter und einen Ablaufverfolgungslistener übergeben.

Zur Verwendung dieses Anbieters muss eine Anwendung für die .NET Framework (anstelle von .NET Core) ausgeführt. Mit dem Anbieter können Weiterleiten von Nachrichten mit einer Vielzahl von [Listener](https://msdn.microsoft.com/library/4y5y10s7), wie z. B. die [TextWriterTraceListener](https://msdn.microsoft.com/library/system.diagnostics.textwritertracelistener) in der beispielanwendung verwendet.

Das folgende Beispiel konfiguriert eine `TraceSource` Anbieter, der protokolliert `Warning` und höher Nachrichten an das Konsolenfenster.

[!code-csharp[](logging/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

<a id="appservice"></a>
### <a name="the-azure-app-service-provider"></a>Der Azure App Service-Anbieter

Die [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) anbieterpakets schreibt Protokolle in Textdateien in eine Azure App Service-app-Dateisystem und [blob-Speicher](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in einem Azure-Speicherkonto. Der Anbieter ist nur für apps, die ASP.NET Core 1.1.0 ausgerichtet verfügbar oder höher. 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

> [!NOTE]
> ASP.NET Core 2.0 ist in der Vorschau.  Apps, mit der neuesten Preview-Version erstellt wurden möglicherweise nicht ausgeführt werden, wenn in Azure App Service bereitgestellt. Wenn ASP.NET Core 2.0 veröffentlicht wird, führt Azure App Service 2.0-apps und das Azure App Service-Anbieter funktioniert als hier angegeben.

Sie müssen die Anbieter-Paket oder den Aufruf installieren die `AddAzureWebAppDiagnostics` Erweiterungsmethode.  Der Anbieter ist automatisch für Ihre app verfügbar, wenn Sie die app in Azure App Service bereitstellen.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

Ein `AddAzureWebAppDiagnostics` überladen ermöglicht die Übergabe in [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs), mit denen Sie können Standardeinstellungen, z. B. die Protokollierung Ausgabe Vorlage, die Blob-Name und die maximale Dateigröße überschreiben. (*Ausgabe Vorlage* ist eine Nachricht-Formatzeichenfolge, die auf alle Protokolle, zusätzlich zu den angewendet wird, die Sie, beim Aufrufen Bereitstellen einer `ILogger` Methode.)  

---

Beim Bereitstellen einer App Service-App wird Ihrer Anwendung berücksichtigt die Einstellungen in der [Diagnoseprotokolle](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) Teil der **Anwendungsdienst** Azure-Portal auf der Seite. Wenn Sie diese Einstellungen ändern, werden die Änderungen sofort wirksam ohne, dass Sie die app starten oder erneut Code darauf bereitstellen. 

![Azure protokollierungseinstellungen](logging/_static/azure-logging-settings.png)

Der Standardspeicherort für Protokolldateien ist der *D:\\home\\LogFiles\\Anwendung* Ordner, und der Standarddateiname ist *Diagnose yyyymmdd.txt*. Die standardmäßige maximale Dateigröße beträgt 10 MB, und die Standardeinstellung für die maximale Anzahl von Dateien beibehalten, ist 2. Der Name des Blob ist *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Weitere Informationen zum Standardverhalten finden Sie unter [AzureAppServicesDiagnosticsSettings](https://github.com/aspnet/Logging/blob/c7d0b1b88668ff4ef8a86ea7d2ebb5ca7f88d3e0/src/Microsoft.Extensions.Logging.AzureAppServices/AzureAppServicesDiagnosticsSettings.cs).

Der Anbieter funktioniert nur, wenn das Projekt in der Azure-Umgebung ausgeführt wird.  Sie hat keine Auswirkungen, wenn Sie lokal ausführen &mdash; schreibt nicht auf lokale Dateien oder des lokalen Entwicklungs-Speicher für Blobs.

## <a name="third-party-logging-providers"></a>Drittanbieter-Protokollanbieter

Hier sind einige Drittanbieter-Protokollierungsframeworks, die mit ASP.NET Core arbeiten:

* [ELMAH.IO](https://github.com/elmahio/Elmah.Io.Extensions.Logging) -Anbieter für den Dienst Elmah.Io

* [JSNLog](http://jsnlog.com) -JavaScript-Ausnahmen und anderen für clientseitige Ereignisse im Protokoll serverseitige protokolliert.

* [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging) -Anbieter für den Dienst Loggr

* [NLog](https://github.com/NLog/NLog.Extensions.Logging) -Anbieter für die NLog-Bibliothek

* [Serilog](https://github.com/serilog/serilog-framework-logging) -Anbieter für die Serilog-Bibliothek

Einige Drittanbieter-Frameworks möglich [semantische Protokollierung, auch bekannt als strukturierte Protokollierung](http://programmers.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

Verwenden ein Drittanbieter-Framework ähnelt der mit einem der integrierten Anbieter: das Projekt ein NuGet-Paket hinzu, und rufen Sie eine Erweiterungsmethode für `ILoggerFactory`. Weitere Informationen finden Sie unter jeder Framework-Dokumentation.

Sie können auch Ihre eigenen benutzerdefinierten Anbieter erstellen, um andere Protokollierungsframeworks oder Ihren eigenen protokollierungsanforderungen unterstützen.
