---
title: Protokollierung in ASP.NET Core
author: ardalis
description: Erfahren Sie mehr über das Protokollierungsframework in ASP.NET Core. Lernen Sie die integrierten Anbieter für die Protokollierung kennen, und erfahren Sie mehr über beliebte Anbieter von Drittanbietern.
ms.author: tdykstra
ms.date: 07/24/2018
uid: fundamentals/logging/index
ms.openlocfilehash: 35bb7fa51db541f825a79151fb7fbe85d48e1998
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655358"
---
# <a name="logging-in-aspnet-core"></a>Protokollierung in ASP.NET Core

Von [Steve Smith](https://ardalis.com/) und [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core unterstützt eine Protokollierungs-API, die mit mehreren verschiedenen Protokollanbietern funktioniert. Mit den integrierten Anbietern können Sie Protokolle an verschiedene Ziele senden, und Sie können ein Protokollierungsframework eines Drittanbieters einbinden. Dieser Artikel zeigt, wie Sie die integrierte Protokollierungs-API und die Anbieter in Ihrem Code verwenden.

::: moniker range=">= aspnetcore-2.0"

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample2) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

::: moniker-end

Informationen zur stdout-Protokollierung beim Hosten mit IIS finden Sie unter <xref:host-and-deploy/iis/troubleshoot#aspnet-core-module-stdout-log>. Informationen zur stdout-Protokollierung mit Azure App Service finden Sie unter <xref:host-and-deploy/azure-apps/troubleshoot#aspnet-core-module-stdout-log>.

## <a name="how-to-create-logs"></a>Erstellen von Protokollen

Um Protokolle zu erstellen, implementieren Sie ein [ILogger&lt;TCategoryName&gt;](/dotnet/api/microsoft.extensions.logging.ilogger-1)-Objekt aus dem Container für die [Dependency Injection](xref:fundamentals/dependency-injection):

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Dann rufen Sie Protokollierungsmethoden für dieses Protokollierungsobjekt auf:

[!code-csharp[](index/sample/Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

In diesem Beispiel werden Protokolle mit der Klasse `TodoController` als *Kategorie* erstellt. Kategorien werden [weiter unten in diesem Artikel](#log-category) erklärt.

ASP.NET Core stellt keine asynchronen Protokollierungsmethoden zur Verfügung, weil eine Protokollierung so schnell erfolgen sollte, dass der Aufwand einer asynchronen Implementierung nicht gerechtfertigt ist. Wenn dies für Sie nicht zutrifft, können Sie eine Änderung der Protokollierungsmethode in Erwägung ziehen. Wenn Sie einen langsamen Datenspeicher verwenden, schreiben Sie die Protokollmeldungen zuerst in einen schnellen Speicher, und verschieben Sie sie dann später in einen langsamen Speicher. Führen Sie die Protokollierung beispielsweise in einer Nachrichtenwarteschlange durch, die von einem anderen Prozess gelesen und in einem langsamen Speicher persistiert wird.

## <a name="how-to-add-providers"></a>Hinzufügen von Anbietern

::: moniker range=">= aspnetcore-2.0"

Ein Protokollierungsanbieter ruft die mit einem `ILogger`-Objekt erstellte Meldung ab und zeigt sie an oder speichert sie. Beispielsweise zeigt der Konsolenanbieter Meldungen auf der Konsole an, und der Azure App Service-Anbieter kann Meldungen in Azure Blob Storage speichern.

Um einen Anbieter zu verwenden, rufen Sie die `Add<ProviderName>`-Erweiterungsmethode des Anbieters in *Program.cs* auf:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=16,17)]

Die Standardprojektvorlage aktiviert die Anbieter der Konsole und der Debugprotokollierung durch einen Aufruf der Erweiterungsmethode [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) in *Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_TemplateCode&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Ein Protokollierungsanbieter ruft die mit einem `ILogger`-Objekt erstellte Meldung ab und zeigt sie an oder speichert sie. Beispielsweise zeigt der Konsolenanbieter Meldungen auf der Konsole an, und der Azure App Service-Anbieter kann Meldungen in Azure Blob Storage speichern.

Um einen Anbieter zu verwenden, installieren Sie das zugehörige NuGet-Paket, und rufen Sie, wie im folgenden Beispiel gezeigt, die Erweiterungsmethode des Anbieters für eine Instanz von [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) auf.

[!code-csharp[](index/sample//Startup.cs?name=snippet_AddConsoleAndDebug&highlight=3,5-7)]

Die ASP.NET Core-[Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) stellt die `ILoggerFactory`-Instanz bereit. Die Erweiterungsmethoden `AddConsole` und `AddDebug` sind in den Paketen [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console/) und [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug/) definiert. Jede Erweiterungsmethode ruft die Methode `ILoggerFactory.AddProvider` auf und übergibt eine Instanz des Anbieters.

> [!NOTE]
> Die [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/logging/index/sample) fügt Protokollanbieter in der `Startup.Configure`-Methode hinzu. Wenn Sie für zuvor ausgeführten Code eine Protokollausgabe erhalten möchten, fügen Sie Protokollierungsanbieter im `Startup`-Klassenkonstruktor hinzu.

::: moniker-end

Informationen zu den [integrierten Protokollierungsanbietern](#built-in-logging-providers) sowie Links zu [Protokollierungsanbietern von Drittanbietern](#third-party-logging-providers) finden Sie weiter unten in diesem Artikel.

## <a name="configuration"></a>Konfiguration

Die Konfiguration von Protokollierungsanbietern erfolgt durch mindestens einen Konfigurationsanbieter:

* Dateiformate (INI, JSON und XML)
* Befehlszeilenargumenten
* Umgebungsvariablen.
* Speicherinterne .NET-Objekte
* Der unverschlüsselte Speicher von [Secret Manager](xref:security/app-secrets).
* Ein unverschlüsselter Benutzerspeicher, z.B. [Azure Key Vault](xref:security/key-vault-configuration).
* Benutzerdefinierte Anbieter (installiert oder erstellt).

Die Konfiguration der Protokollierung wird meist vom `Logging`-Abschnitt von Anwendungseinstellungsdateien angegeben. Im folgenden Beispiel werden die Inhalte einer herkömmlichen *appsettings.Development.json*-Datei veranschaulicht:

::: moniker range=">= aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    },
    "Console":
    {
      "IncludeScopes": "true"
    }
  }
}
```

`LogLevel`-Schlüssel stellen Protokollnamen dar. Der `Default`-Schlüssel gilt für Protokolle, die nicht explizit aufgeführt werden. Der Wert entspricht dem auf das jeweilige Protokoll angewendeten [Protokolliergrad](#log-level). Protokollschlüssel, die `IncludeScopes` festlegen (im Beispiel `Console`), geben an, ob [Protokollbereiche](#log-scopes) für das angegebene Protokoll aktiviert sind.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Debug",
      "System": "Information",
      "Microsoft": "Information"
    }
  }
}
```

`LogLevel`-Schlüssel stellen Protokollnamen dar. Der `Default`-Schlüssel gilt für Protokolle, die nicht explizit aufgeführt werden. Der Wert entspricht dem auf das jeweilige Protokoll angewendeten [Protokolliergrad](#log-level).

::: moniker-end

Informationen zur Implementierung von Konfigurationsanbieter finden Sie hier: <xref:fundamentals/configuration/index>.

## <a name="sample-logging-output"></a>Beispiel einer Protokollierungsausgabe

Wenn Sie den im vorherigen Abschnitt gezeigten Beispielcode von der Befehlszeile ausführen, werden die Protokolle in der Konsole angezeigt. Hier ein Beispiel für eine Konsolenausgabe:

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

Diese Protokolle wurden durch einen Wechsel zu `http://localhost:5000/api/todo/0` erstellt, um die Ausführung beider `ILogger`-Aufrufe im vorherigen Abschnitt auszulösen.

Nachfolgend sehen Sie, wie die gleichen Protokolle im Debugfenster angezeigt werden, wenn Sie die Beispielanwendung in Visual Studio ausführen:

```console
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request starting HTTP/1.1 GET http://localhost:53104/api/todo/0  
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executing action method TodoApi.Controllers.TodoController.GetById (TodoApi) with arguments (0) - ModelState is Valid
TodoApi.Controllers.TodoController:Information: Getting item 0
TodoApi.Controllers.TodoController:Warning: GetById(0) NOT FOUND
Microsoft.AspNetCore.Mvc.StatusCodeResult:Information: Executing HttpStatusCodeResult, setting HTTP status code 404
Microsoft.AspNetCore.Mvc.Internal.ControllerActionInvoker:Information: Executed action TodoApi.Controllers.TodoController.GetById (TodoApi) in 152.5657ms
Microsoft.AspNetCore.Hosting.Internal.WebHost:Information: Request finished in 316.3195ms 404
```

Die über die `ILogger`-Aufrufe aus dem vorherigen Abschnitt erstellten Protokolle beginnen mit „TodoApi.Controllers.TodoController“. Protokolle, die mit Microsoft-Kategorien beginnen, stammen aus ASP.NET Core. ASP.NET Core selbst und Ihr Anwendungscode verwenden dieselbe Protokollierungs-API und dieselben Protokollierungsanbieter.

In den übrigen Abschnitten dieses Artikels werden einige Details und Optionen für die Protokollierung erläutert.

## <a name="nuget-packages"></a>NuGet-Pakete

Die Schnittstellen `ILogger` und `ILoggerFactory` befinden sich in [Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/), ihre Standardimplementierungen sind in [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) enthalten.

## <a name="log-category"></a>Protokollkategorie

Jedes von Ihnen erstellte Protokoll umfasst eine *Kategorie*. Sie geben die Kategorie an, wenn Sie ein `ILogger`-Objekt erstellen. Die Kategorie kann eine beliebige Zeichenfolge sein, aber gemäß Konvention wird der vollqualifizierte Name der Klasse verwendet, mit der die Protokolle geschrieben werden. Beispiel: TodoApi.Controllers.TodoController

Sie können die Kategorie als Zeichenfolge angeben oder eine Erweiterungsmethode verwenden, die die Kategorie aus dem Typ ableitet. Um die Kategorie als Zeichenfolge anzugeben, rufen Sie `CreateLogger` für eine `ILoggerFactory`-Instanz auf, wie unten gezeigt.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CreateLogger&highlight=7,10)]

Meistens ist es einfacher, `ILogger<T>` zu verwenden, wie im folgenden Beispiel gezeigt wird.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LoggerDI&highlight=7)]

Dies entspricht dem Aufruf von `CreateLogger` mit dem vollqualifizierten Typnamen `T`.

## <a name="log-level"></a>Protokolliergrad

Jedes Mal, wenn Sie ein Protokoll schreiben, geben Sie den zugehörigen [LogLevel](/dotnet/api/microsoft.extensions.logging.logLevel) an. Der Protokolliergrad gibt den Schweregrad oder die Wichtigkeit an. Beispielsweise könnten Sie ein `Information`-Protokoll schreiben, wenn eine Methode normal beendet wird, ein `Warning`-Protokoll, wenn eine Methode einen 404-Rückgabecode zurückgibt, und ein `Error`-Protokoll, wenn Sie eine unerwartete Ausnahme abfangen.

Im folgenden Codebeispiel geben die Namen der Methoden (z.B. `LogWarning`) den Protokolliergrad an. Der erste Parameter ist die [Protokollereignis-ID](#log-event-id). Der zweite Parameter ist eine [Meldungsvorlage](#log-message-template) mit Platzhaltern für Argumentwerte, die von den verbleibenden Methodenparametern bereitgestellt werden. Die Methodenparameter werden weiter unten in diesem Artikel ausführlicher erläutert.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Protokollmethoden, die den Protokolliergrad im Methodennamen enthalten, sind [Erweiterungsmethoden für ILogger](/dotnet/api/microsoft.extensions.logging.loggerextensions). Diese Methoden rufen im Hintergrund eine `Log`-Methode auf, die einen `LogLevel`-Parameter verwendet. Sie können anstelle eines Aufrufs dieser Erweiterungsmethoden die `Log`-Methode direkt aufrufen, aber die Syntax ist relativ kompliziert. Weitere Informationen finden Sie unter [ILogger-Schnittstelle](/dotnet/api/microsoft.extensions.logging.ilogger) und im [Quellcode für Protokollierungserweiterungen](https://github.com/aspnet/Logging/blob/master/src/Microsoft.Extensions.Logging.Abstractions/LoggerExtensions.cs).

ASP.NET Core definiert die folgenden [Protokolliergrade](/dotnet/api/microsoft.extensions.logging.loglevel), angeordnet vom geringsten bis zum höchsten Schweregrad.

* Trace = 0

  Für Informationen, die nur für Entwickler beim Debuggen von Problemen nützlich sind. Diese Meldungen können sensible Anwendungsdaten enthalten und sollten daher nicht in einer Produktionsumgebung aktiviert werden. *Standardmäßig deaktiviert.* Ein Beispiel: `Credentials: {"User":"someuser", "Password":"P@ssword"}`

* Debug = 1

  Für Informationen, die während der Entwicklung und beim Debuggen kurzfristig von Nutzen sind. Beispiel: `Entering method Configure with flag set to true.` Sie würden Protokolle mit dem Protokolliergrad `Debug` aufgrund des hohen Protokollaufkommens nur zur Problembehandlung in der Produktion aktivieren.

* Information = 2

  Zur Nachverfolgung des allgemeinen Ablaufs der Anwendung. Diese Protokolle haben typischerweise einen langfristigen Nutzen. Ein Beispiel: `Request received for path /api/todo`

* Warning = 3

  Für ungewöhnliche oder unerwartete Ereignisse im Anwendungsverlauf. Dazu können Fehler oder andere Bedingungen gehören, die zwar nicht zum Beenden der Anwendung führen, aber eventuell untersucht werden müssen. Behandelte Ausnahmen sind eine typische Verwendung für den Protokolliergrad `Warning`. Ein Beispiel: `FileNotFoundException for file quotes.txt.`

* Error = 4

  Für Fehler und Ausnahmen, die nicht behandelt werden können. Diese Meldungen weisen auf einen Fehler in der aktuellen Aktivität oder im aktuellen Vorgang (z.B. die aktuelle HTTP-Anforderung) und nicht auf einen anwendungsweiten Fehler hin. Beispielprotokollmeldung: `Cannot insert record due to duplicate key violation.`

* Critical = 5

  Für Fehler, die sofortige Aufmerksamkeit erfordern. Beispiel: Szenarios mit Datenverlust, Speichermangel.

Mithilfe des Protokolliergrads können Sie die Menge an Protokollausgabedaten steuern, die in ein bestimmtes Speichermedium geschrieben oder an ein Anzeigefenster ausgegeben werden. Beispielsweise können Sie in der Produktion alle Protokolle der Ebene `Information` und niedriger in einen Volumendatenspeicher schreiben und alle Protokolle der Ebene `Warning` und höher in einen Wertdatenspeicher ausgeben. Während der Entwicklung senden Sie normalerweise Protokolle mit dem Schweregrad `Warning` und höher an die Konsole. Wenn Sie eine Problembehandlung durchführen müssen, können Sie die Ebene `Debug` hinzufügen. Im Abschnitt [Protokollfilterung](#log-filtering) weiter unten in diesem Artikel wird erläutert, wie Sie steuern, welche Protokolliergrade ein Anbieter verarbeitet.

Das ASP.NET Core-Framework schreibt Protokolle der Ebene `Debug` für Frameworkereignisse. Das zuvor in diesem Artikel gezeigte Protokollbeispiel enthielt Protokolle unterhalb der Ebene `Information`, deshalb wurden keine Protokolle der Ebene `Debug` gezeigt. Hier sehen Sie ein Beispiel für Konsolenprotokolle, die bei Ausführung der Beispielanwendung angezeigt werden, wenn diese zur Anzeige von Protokollen der Ebene `Debug` und höher für den Konsolenanbieter konfiguriert ist.

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

## <a name="log-event-id"></a>Protokollereignis-ID

Jedes Mal, wenn Sie ein Protokoll schreiben, geben Sie eine *Ereignis-ID* an. Die Beispiel-App verwendet hierzu eine lokal definierte `LoggingEvents`-Klasse:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

[!code-csharp[](index/sample//Core/LoggingEvents.cs?name=snippet_LoggingEvents)]

Eine Ereignis-ID ist ein ganzzahliger Wert, mit dessen Hilfe Sie eine Reihe von protokollierten Ereignissen einander zuordnen können. Beispielsweise könnte ein Protokoll zum Hinzufügen eines Artikels zu einem Warenkorb die Ereignis-ID 1000 und ein Protokoll zum Abschließen eines Kaufvorgangs die Ereignis-ID 1001 aufweisen.

Bei der Protokollausgabe kann die Ereignis-ID je nach Anbieter in einem Feld gespeichert oder in die Textmeldung aufgenommen werden. Der Debuganbieter zeigt keine Ereignis-IDs an, aber beim Konsolenanbieter werden die IDs in Klammern hinter der Kategorie angezeigt:

```console
info: TodoApi.Controllers.TodoController[1002]
      Getting item invalidid
warn: TodoApi.Controllers.TodoController[4000]
      GetById(invalidid) NOT FOUND
```

## <a name="log-message-template"></a>Protokollmeldungsvorlage

Jedes Mal, wenn Sie eine Protokollmeldung schreiben, stellen Sie eine Meldungsvorlage bereit. Die Meldungsvorlage kann eine Zeichenfolge sein oder benannte Platzhalter enthalten, in die Argumentwerte eingefügt werden. Die Vorlage ist keine Formatzeichenfolge, und Platzhalter sollten benannt und nicht nummeriert werden.

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_CallLogMethods&highlight=3,7)]

Die Reihenfolge der Platzhalter – nicht ihre Namen – bestimmt, welche Parameter verwendet werden, um ihre Werte zur Verfügung zu stellen. Wenn Ihr Code folgendermaßen lautet:

```csharp
string p1 = "parm1";
string p2 = "parm2";
_logger.LogInformation("Parameter values: {p2}, {p1}", p1, p2);
```

Die angezeigte Protokollmeldung sieht so aus:

```
Parameter values: parm1, parm2
```

Das Protokollierungsframework formatiert Meldungen in dieser Weise, um den Protollierungsanbietern das Implementieren einer [semantischen Protokollierung (auch bezeichnet als strukturierte Protokollierung)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) zu ermöglichen. Da nicht nur die formatierte Meldungsvorlage, sondern die Argumente selbst an das Protokollierungssystem übergeben werden, können Protokollierungsanbieter die Parameterwerte zusätzlich zur Nachrichtenvorlage als Felder speichern. Angenommen, Sie verwenden Azure Table Storage als Ziel für die Protokollausgabe, und Ihr Aufruf der Protokollierungsmethode sieht wie folgt aus:

```csharp
_logger.LogInformation("Getting item {ID} at {RequestTime}", id, DateTime.Now);
```

Jede Azure Table Storage-Entität kann `ID`- und `RequestTime`-Eigenschaften aufweisen, wodurch das Abfragen von Protokolldaten vereinfacht wird. Sie können alle Protokolle innerhalb eines bestimmten `RequestTime`-Bereichs ermitteln, ohne die Uhrzeit aus den Textmeldungen auslesen zu müssen.

## <a name="logging-exceptions"></a>Protokollieren von Ausnahmen

Die Protokollierungsmethoden umfassen Überladungen, die das Übergeben von Ausnahmen ermöglichen, wie im folgenden Beispiel gezeigt:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_LogException&highlight=3)]

Die verschiedenen Anbieter verarbeiten die Ausnahmeinformationen auf unterschiedliche Weise. Hier sehen Sie ein Beispiel einer Debuganbieterausgabe aus dem obigen Codebeispiel.

```
TodoApi.Controllers.TodoController:Warning: GetById(036dd898-fb01-47e8-9a65-f92eb73cf924) NOT FOUND

System.Exception: Item not found exception.
 at TodoApi.Controllers.TodoController.GetById(String id) in C:\logging\sample\src\TodoApi\Controllers\TodoController.cs:line 226
```

## <a name="log-filtering"></a>Protokollfilterung

::: moniker range=">= aspnetcore-2.0"

Sie können einen Mindestprotokolliergrad für einen bestimmten Anbieter und eine bestimmte Kategorie oder für alle Anbieter oder alle Kategorien festlegen. Alle Protokolle unter dem Mindestgrad werden nicht an diesen Anbieter weitergeleitet, sodass sie nicht angezeigt oder gespeichert werden.

Wenn Sie alle Protokolle unterdrücken möchten, können Sie `LogLevel.None` als Mindestprotokolliergrad angeben. Der ganzzahlige Wert von `LogLevel.None` lautet 6 und liegt über `LogLevel.Critical` (5).

### <a name="create-filter-rules-in-configuration"></a>Erstellen von Filterregeln in der Konfiguration

Die Projektvorlagen erstellen Code, der `CreateDefaultBuilder` aufruft, um die Protokollierung für die Konsolen- und Debuganbieter einzurichten. Die `CreateDefaultBuilder`-Methode richtet die Protokollierung auch ein, um nach der Konfiguration in einem `Logging`-Abschnitt zu suchen. Hierbei wird Code wie der folgende verwendet:

[!code-csharp[](index/sample2/Program.cs?name=snippet_ExpandDefault&highlight=15)]

Die Konfigurationsdaten geben die Mindestprotokolliergrade nach Anbieter und Kategorie an, wie im folgenden Beispiel gezeigt:

[!code-json[](index/sample2/appsettings.json)]

Diese JSON-Konfiguration erstellt sechs Filterregeln: eine für den Debuganbieter, vier für den Konsolenanbieter und eine für alle Anbieter. Sie werden später sehen, wie beim Erstellen eines `ILogger`-Objekts immer nur eine dieser Regeln für jeden Anbieter ausgewählt wird.

### <a name="filter-rules-in-code"></a>Filterregeln im Code

Sie können Filterregeln im Code registrieren, wie das folgende Beispiel zeigt:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterInCode&highlight=4-5)]

Das zweite `AddFilter` gibt den Debuganbieter durch Verwendung des zugehörigen Typnamens an. Das erste `AddFilter` gilt für alle Anbieter, weil kein Anbietertyp angegeben wird.

### <a name="how-filtering-rules-are-applied"></a>Anwendung von Filterregeln

Die Konfigurationsdaten und der in den vorangegangenen Beispielen gezeigte `AddFilter`-Code erzeugen die in der folgenden Tabelle gezeigten Regeln. Die ersten sechs Regeln stammen aus dem Konfigurationsbeispiel, die letzten zwei Filter stammen aus dem Codebeispiel.

| Anzahl | Anbieter      | Kategorien beginnend mit...          | Mindestprotokolliergrad |
| :----: | ------------- | --------------------------------------- | ----------------- |
| 1      | Debug         | Alle Kategorien                          | Information       |
| 2      | Konsole       | Microsoft.AspNetCore.Mvc.Razor.Internal | Warnung           |
| 3      | Konsole       | Microsoft.AspNetCore.Mvc.Razor.Razor    | Debug             |
| 4      | Konsole       | Microsoft.AspNetCore.Mvc.Razor          | Fehler             |
| 5      | Konsole       | Alle Kategorien                          | Information       |
| 6      | Alle Anbieter | Alle Kategorien                          | Debug             |
| 7      | Alle Anbieter | System                                  | Debug             |
| 8      | Debug         | Microsoft                               | Ablaufverfolgung             |

Wenn Sie ein `ILogger`-Objekt zum Schreiben von Protokollen erstellen, wählt das `ILoggerFactory`-Objekt eine einzige Regel pro Anbieter aus, die auf diese Protokollierung angewendet wird. Alle über dieses `ILogger`-Objekt geschriebenen Meldungen werden auf Grundlage der ausgewählten Regeln gefiltert. Aus den verfügbaren Regeln wird die für jeden Anbieter und jedes Kategoriepaar spezifischste Regel ausgewählt.

Beim Erstellen von `ILogger` für eine vorgegebene Kategorie wird für jeden Anbieter der folgende Algorithmus verwendet:

* Es werden alle Regeln ausgewählt, die dem Anbieter oder dem zugehörigen Alias entsprechen. Falls keine solchen Regeln vorhanden sind, werden alle Regeln mit einem leeren Anbieter ausgewählt.
* Aus dem Ergebnis des vorherigen Schritts werden die Regeln mit dem längsten übereinstimmenden Kategoriepräfix ausgewählt. Falls keine solchen Regeln vorhanden sind, werden alle Regeln ohne Angabe einer Kategorie ausgewählt.
* Wenn mehrere Regeln ausgewählt sind, wird die **letzte** Regel verwendet.
* Wenn keine Regel ausgewählt ist, wird `MinimumLevel` verwendet.

Angenommen, Sie verfügen über die vorherige Liste mit Regeln und erstellen ein `ILogger`-Objekt für die Kategorie „Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine“:

* Für den Debuganbieter gelten die Regeln 1, 6 und 8. Regel 8 ist die spezifischste Regel, deshalb wird diese Regel ausgewählt.
* Für den Konsolenanbieter gelten die Regeln 3, 4, 5 und 6. Regel 3 ist die spezifischste Regel.

Wenn Sie mit `ILogger` Protokolle für die Kategorie „Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine“ erstellen, werden Protokolle der Ebene `Trace` und höher an den Debuganbieter ausgegeben, und Protokolle der Ebene `Debug` und höher gehen an den Konsolenanbieter.

### <a name="provider-aliases"></a>Anbieteraliase

Sie können den Typnamen für die Angabe eines Anbieters in der Konfiguration verwenden, aber jeder Anbieter definiert einen kürzeren *Alias*, der einfacher zu verwenden ist. Verwenden Sie für die integrierten Anbieter die folgenden Aliase:

* Konsole
* Debug
* EventLog
* AzureAppServices
* TraceSource
* EventSource

### <a name="default-minimum-level"></a>Standardmindestprotokolliergrad

Es gibt eine Einstellung für den Mindestprotokolliergrad, die nur dann wirksam wird, wenn für einen bestimmten Anbieter und eine bestimmte Kategorie keine Regeln aus Konfiguration oder Code gelten. Im folgenden Beispiel wird das Festlegen des Mindestprotokolliergrads veranschaulicht:

[!code-csharp[](index/sample2/Program.cs?name=snippet_MinLevel&highlight=3)]

Wenn Sie den Mindestprotokolliergrad nicht explizit festlegen, lautet der Standardwert `Information`, d.h. Protokolle der Ebene `Trace` und `Debug` werden ignoriert.

### <a name="filter-functions"></a>Filterfunktionen

Sie können Code in einer Filterfunktion schreiben, um Filterregeln anzuwenden. Eine Filterfunktion wird für alle Anbieter und Kategorien aufgerufen, denen keine Regeln durch Konfiguration oder Code zugewiesen sind. Über den Code in der Funktion kann auf Anbietertyp, Kategorie und Protokollebene zugegriffen werden. So kann entschieden werden, ob eine Meldung protokolliert werden soll oder nicht. Zum Beispiel:

[!code-csharp[](index/sample2/Program.cs?name=snippet_FilterFunction&highlight=5-13)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Bei einigen Protokollierungsanbietern können Sie angeben, wann Protokolle in ein Speichermedium geschrieben oder ignoriert werden sollen – je nach Protokolliergrad und Kategorie.

Die Erweiterungsmethoden `AddConsole` und `AddDebug` bieten Überladungen, mit denen Filterkriterien übergeben werden können. Der folgende Beispielcode veranlasst den Konsolenanbieter, Protokolle unterhalb der Ebene `Warning` zu ignorieren, während der Debuganbieter vom Framework erstellte Protokolle ignoriert.

[!code-csharp[](index/sample/Startup.cs?name=snippet_AddConsoleAndDebugWithFilter&highlight=6-7)]

Die Methode `AddEventLog` umfasst eine Überladung mit einer `EventLogSettings`-Instanz, die eine Filterfunktion in ihrer `Filter`-Eigenschaft enthalten kann. Der TraceSource-Anbieter stellt keine dieser Überladungen zur Verfügung, da der zugehörige Protokolliergrad und weitere Parameter auf der Verwendung von `SourceSwitch` und `TraceListener` basieren.

Sie können Filterregeln für alle bei einer `ILoggerFactory`-Instanz registrierten Anbieter festlegen, indem Sie die Erweiterungsmethode `WithFilter` verwenden. Das nachstehende Beispiel begrenzt Frameworkprotokolle (Kategorie beginnt mit „Microsoft“ oder „System“) auf Warnungen, während für App-Protokolle die Debugebene verwendet wird.

[!code-csharp[](index/sample/Startup.cs?name=snippet_FactoryFilter&highlight=6-11)]

Wenn Sie die Filterung verwenden, um zu verhindern, dass alle Protokolle für eine bestimmte Kategorie geschrieben werden, können Sie `LogLevel.None` als Mindestprotokolliergrad für diese Kategorie angeben. Der ganzzahlige Wert von `LogLevel.None` lautet 6 und liegt über `LogLevel.Critical` (5).

Die Erweiterungsmethode `WithFilter` wird vom NuGet-Paket [Microsoft.Extensions.Logging.Filter](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Filter) bereitgestellt. Die Methode gibt eine neue `ILoggerFactory`-Instanz zurück, die Protokollmeldungen für alle bei ihr registrierten Protokollierungsanbieter filtert. Andere `ILoggerFactory`-Instanzen, die ursprüngliche `ILoggerFactory`-Instanz eingeschlossen, sind nicht betroffen.

::: moniker-end

## <a name="log-scopes"></a>Protokollbereiche

Sie können eine Gruppe von logischen Operationen in einem *Bereich* gruppieren, um an jedes der für diese Gruppe erstellten Protokolle dieselben Daten anzuhängen. Beispielsweise kann es sinnvoll sein, dass jedes im Rahmen der Verarbeitung einer Transaktion erstellte Protokoll die Transaktions-ID enthält.

Ein Bereich ist ein `IDisposable`-Typ, der von der Methode [ILogger.BeginScope&lt;TState&gt;](/dotnet/api/microsoft.extensions.logging.ilogger.beginscope) zurückgegeben und so lange beibehalten wird, bis er verworfen wird. Sie verwenden einen Bereich, indem Sie Ihre Protokollierungsaufrufe mit einem `using`-Block umschließen, wie hier gezeigt:

[!code-csharp[](index/sample//Controllers/TodoController.cs?name=snippet_Scopes&highlight=4-5,13)]

Der folgende Code aktiviert Bereiche für den Konsolenanbieter:

::: moniker range="> aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Um die bereichsbasierte Protokollierung zu aktivieren, muss die Konsolenprotokollierungsoption `IncludeScopes` konfiguriert werden.
>
> Weitere Informationen zur Konfiguration finden Sie im Abschnitt [Konfiguration](#configuration).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

*Program.cs*:

[!code-csharp[](index/sample2/Program.cs?name=snippet_Scopes&highlight=4)]

> [!NOTE]
> Um die bereichsbasierte Protokollierung zu aktivieren, muss die Konsolenprotokollierungsoption `IncludeScopes` konfiguriert werden.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

*Startup.cs*:

[!code-csharp[](index/sample/Startup.cs?name=snippet_Scopes&highlight=6)]

::: moniker-end

Jede Protokollmeldung enthält die bereichsbezogenen Informationen:

```
info: TodoApi.Controllers.TodoController[1002]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      Getting item 0
warn: TodoApi.Controllers.TodoController[4000]
      => RequestId:0HKV9C49II9CK RequestPath:/api/todo/0 => TodoApi.Controllers.TodoController.GetById (TodoApi) => Message attached to logs created in the using block
      GetById(0) NOT FOUND
```

## <a name="built-in-logging-providers"></a>Integrierte Protokollierungsanbieter

ASP.NET Core wird mit den folgenden Anbietern bereitgestellt:

* [Konsole](#console-provider)
* [Debuggen](#debug-provider)
* [EventSource](#eventsource-provider)
* [EventLog](#windows-eventlog-provider)
* [TraceSource](#tracesource-provider)
* [Azure App Service](#azure-app-service-provider)

### <a name="console-provider"></a>Der Konsolenanbieter

Das Anbieterpaket [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) sendet eine Protokollausgabe an die Konsole.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddConsole();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddConsole();
```

Mithilfe von [AddConsole-Überladungen](/dotnet/api/microsoft.extensions.logging.consoleloggerextensions) können Sie einen Mindestprotokolliergrade, eine Filterfunktion und einen booleschen Wert übergeben, der angibt, welche Bereiche unterstützt werden. Darüber hinaus kann ein `IConfiguration`-Objekt übergeben werden, mit dem Bereichsunterstützung und Protokolliergrade angegeben werden können.

Wenn Sie erwägen, den Konsolenanbieter in der Produktion einzusetzen, sollten Sie sich darüber im Klaren sein, dass er einen erheblichen Einfluss auf die Leistung hat.

Wenn Sie ein neues Projekt in Visual Studio erstellen, sieht die `AddConsole`-Methode folgendermaßen aus:

```csharp
loggerFactory.AddConsole(Configuration.GetSection("Logging"));
```

Dieser Code verweist auf den `Logging`-Abschnitt der Datei *appSettings.json*:

[!code-json[](index/sample//appsettings.json)]

Die gezeigten Einstellungen schränken die Frameworkprotokolle auf Warnungen ein, während die App eine Protokollierung auf Debugebene durchführt, wie im Abschnitt [Protokollfilterung](#log-filtering) erläutert. Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).

::: moniker-end

### <a name="debug-provider"></a>Der Debuganbieter

Beim Anbieterpaket [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) erfolgt die Protokollausgabe unter Verwendung der Klasse [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) (`Debug.WriteLine`-Methodenaufrufe).

Unter Linux werden Protokolle dieses Anbieters in */var/log/message* geschrieben.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddDebug();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddDebug();
```

Mithilfe von [AddDebug-Überladungen](/dotnet/api/microsoft.extensions.logging.debugloggerfactoryextensions) können Sie einen Mindestprotokolliergrad oder eine Filterfunktion übergeben.

::: moniker-end

### <a name="eventsource-provider"></a>Der EventSource-Anbieter

Für Apps, die für ASP.NET Core 1.1.0 oder höher konzipiert sind, kann mit dem Anbieterpaket [Microsoft.Extensions.Logging.EventSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventSource) eine Ereignisablaufverfolgung implementiert werden. Verwenden Sie unter Windows [ETW](https://msdn.microsoft.com/library/windows/desktop/bb968803). Der Anbieter ist plattformunabhängig, aber für Linux oder macOS sind Ereignissammlung und Anzeigetools noch nicht verfügbar.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventSourceLogger();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventSourceLogger();
```

::: moniker-end

Eine gute Möglichkeit zum Erfassen und Anzeigen von Protokollen ist die Verwendung des Hilfsprogramms [PerfView](https://github.com/Microsoft/perfview). Es gibt andere Tools zur Anzeige von ETW-Protokollen, aber PerfView bietet die besten Ergebnisse bei der Arbeit mit ETW-Ereignissen, die von ASP.NET ausgegeben werden.

Um PerfView für das Erfassen von Ereignissen zu konfigurieren, die von diesem Anbieter protokolliert wurden, fügen Sie die Zeichenfolge `*Microsoft-Extensions-Logging` zur Liste **Zusätzliche Anbieter** hinzu. (Vergessen Sie nicht das Sternchen am Anfang der Zeichenfolge.)

![Zusätzliche PerfView-Anbieter](index/_static/perfview-additional-providers.png)

### <a name="windows-eventlog-provider"></a>Der Windows-EventLog-Anbieter

Das Anbieterpaket [Microsoft.Extensions.Logging.EventLog](https://www.nuget.org/packages/Microsoft.Extensions.Logging.EventLog) sendet eine Protokollausgabe in das Windows-Ereignisprotokoll.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddEventLog();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddEventLog();
```

Mithilfe von [AddEventLog-Überladungen](/dotnet/api/microsoft.extensions.logging.eventloggerfactoryextensions) können Sie `EventLogSettings` oder einen Mindestprotokolliergrad übergeben.

::: moniker-end

### <a name="tracesource-provider"></a>Der TraceSource-Anbieter

Das Anbieterpaket [Microsoft.Extensions.Logging.TraceSource](https://www.nuget.org/packages/Microsoft.Extensions.Logging.TraceSource) verwendet die [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource)-Bibliotheken und -Anbieter.

::: moniker range=">= aspnetcore-2.0"

```csharp
logging.AddTraceSource(sourceSwitchName);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
loggerFactory.AddTraceSource(sourceSwitchName);
```

::: moniker-end

Mithilfe von [AddTraceSource-Überladungen](/dotnet/api/microsoft.extensions.logging.tracesourcefactoryextensions) können Sie eine Quelloption und einen Listener für die Ablaufverfolgung übergeben.

Um diesen Anbieter zu verwenden, muss eine Anwendung (anstelle von .NET Core) unter .NET Framework ausgeführt werden. Mit dem Anbieter können Sie Meldungen an verschiedenste [Listener](/dotnet/framework/debug-trace-profile/trace-listeners) weiterleiten, z.B. an den [TextWriterTraceListener](/dotnet/api/system.diagnostics.textwritertracelistenerr), der in der Beispielanwendung verwendet wird.

Im folgenden Beispiel wird ein `TraceSource`-Anbieter konfiguriert, der Protokollmeldungen der Ebene `Warning` und höher an das Konsolenfenster ausgibt.

[!code-csharp[](index/sample/Startup.cs?name=snippet_TraceSource&highlight=9-12)]

### <a name="azure-app-service-provider"></a>Der Azure App Service-Anbieter

Das Anbieterpaket [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) schreibt Protokolle in Textdateien in das Dateisystem einer Azure App Service-App und in [Blob Storage](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#what-is-blob-storage) in einem Azure Storage-Konto. Das Anbieterpaket ist für Apps verfügbar, die für .NET Core 1.1 oder höher konzipiert sind.

::: moniker range=">= aspnetcore-2.0"

Wenn Sie Ihre App auf .NET Core ausrichten, beachten Sie Folgendes:

* Das Anbieterpaket ist zwar im Lieferumfang des ASP.NET Core-Metapakets [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) enthalten, aber nicht im Metapaket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).
* Rufen Sie [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) nicht explizit auf. Der Anbieter wird Ihrer App automatisch zur Verfügung gestellt, wenn diese in Azure App Service bereitgestellt wird.

Wenn Sie Anwendungen für .NET Framework entwickeln oder auf das `Microsoft.AspNetCore.App`-Metapaket verweisen, fügen Sie dem Projekt das Anbieterpaket hinzu. Rufen Sie `AddAzureWebAppDiagnostics` auf einer [ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory)-Instanz auf:

```csharp
logging.AddAzureWebAppDiagnostics();
```

::: moniker-end

::: moniker range="= aspnetcore-1.1"

```csharp
loggerFactory.AddAzureWebAppDiagnostics();
```

::: moniker-end

Mithilfe einer [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics)-Überladung können Sie [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings) übergeben. Damit können Sie Standardeinstellungen wie die Vorlage für die Protokollierungsausgabe, den BLOB-Namen und die Dateigrößenbeschränkung überschreiben. (Eine *Ausgabevorlage* ist eine Meldungsvorlage, die zusätzlich zu dem Protokoll, das Sie beim Aufruf einer `ILogger`-Methode angeben, auf alle Protokolle angewendet wird.)

Wenn Sie eine Bereitstellung für eine App Service-App durchführen, berücksichtigt die App die Einstellungen im Abschnitt [Diagnoseprotokolle](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/#enablediag) der Seite **App Service** im Azure-Portal. Bei einem Update dieser Einstellungen werden die Änderungen sofort wirksam, ohne dass ein Neustart oder eine erneute Bereitstellung der App notwendig ist.

![Einstellungen für die Azure-Protokollierung](index/_static/azure-logging-settings.png)

Der Standardspeicherort für Protokolldateien ist der Ordner *D:\\home\\LogFiles\\Application*, und der standardmäßige Dateiname lautet *diagnostics-yyyymmdd.txt*. Die Dateigröße ist standardmäßig auf 10 MB beschränkt, und die maximal zulässige Anzahl beibehaltener Dateien lautet 2. Der Standardblobname lautet *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*. Weitere Informationen zum Standardverhalten finden Sie unter [AzureAppServicesDiagnosticsSettings](/dotnet/api/microsoft.extensions.logging.azureappservices.azureappservicesdiagnosticssettings).

Der Anbieter funktioniert nur, wenn das Projekt in der Azure-Umgebung ausgeführt wird. Bei einer lokalen Ausführung zeigt er keine Auswirkungen. Der Anbieter schreibt keine Protokolle in lokale Dateien oder den lokalen Entwicklungsspeicher für BLOBs.

## <a name="third-party-logging-providers"></a>Protokollierungsanbieter von Drittanbietern

Protokollierungsframeworks von Drittanbietern aufgeführt, die mit ASP.NET Core funktionieren:

* [elmah.io](https://elmah.io/) ([GitHub-Repository](https://github.com/elmahio/Elmah.Io.Extensions.Logging))
* [Gelf](http://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub-Repository](https://github.com/mattwcole/gelf-extensions-logging))
* [JSNLog](http://jsnlog.com/) ([GitHub-Repository](https://github.com/mperdeck/jsnlog))
* [Loggr](http://loggr.net/) ([GitHub-Repository](https://github.com/imobile3/Loggr.Extensions.Logging))
* [NLog](http://nlog-project.org/) ([GitHub-Repository](https://github.com/NLog/NLog.Extensions.Logging))
* [Serilog](https://serilog.net/) ([GitHub-Repository](https://github.com/serilog/serilog-extensions-logging))

Einige Drittanbieterframeworks können eine [semantische Protokollierung (auch als strukturierte Protokollierung bezeichnet)](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging) ausführen.

Die Verwendung eines Frameworks von Drittanbietern ist ähnlich wie die Verwendung eines integrierten Anbieters:

1. Fügen Sie Ihrem Paket ein NuGet-Paket hinzu.
1. Rufen Sie eine Erweiterungsmethode für `ILoggerFactory` auf.

Weitere Informationen finden Sie in der Dokumentation zum jeweiligen Framework.

## <a name="azure-log-streaming"></a>Azure-Protokollstreaming

Das Azure-Protokollstreaming ermöglicht Ihnen eine Echtzeitanzeige der Protokollaktivität für:

* Anwendungsserver
* Webserver
* Ablaufverfolgung für Anforderungsfehler

So konfigurieren Sie das Azure-Protokollstreaming

* Navigieren Sie von der Portalseite Ihrer Anwendung zur Seite **Diagnoseprotokolle**.
* Aktivieren Sie **Anwendungsprotokollierung (Dateisystem)**.

![Seite „Diagnoseprotokolle“ im Azure-Portal](index/_static/azure-diagnostic-logs.png)

Navigieren Sie zur Seite **Protokollstreaming**, um Anwendungsmeldungen anzuzeigen. Diese werden von der Anwendung über die `ILogger`-Schnittstelle protokolliert.

![Anwendungsprotokollstreaming im Azure-Portal](index/_static/azure-log-streaming.png)

## <a name="azure-application-insights-trace-logging"></a>Ablaufverfolgungsprotokollierung für Azure Application Insights

Das [Application Insights](https://azure.microsoft.com/services/application-insights/) SDK kann Ablaufverfolgungstelemetrie von Protokollen sammeln, die über die ASP.NET Core-Protokollierungsinfrastruktur generiert wurden. Weitere Informationen finden Sie unter [Application Insights for ASP.NET Core (Application Insights für ASP.NET Core)](https://docs.microsoft.com/azure/application-insights/app-insights-asp-net-core) und [Microsoft/ApplicationInsights-aspnetcore Wiki: Logging (Microsoft/ApplicationInsights-aspnetcore-Wiki: Protokollierung)](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Logging).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:fundamentals/logging/loggermessage>
