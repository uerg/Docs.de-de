---
title: Fehlerbehandlung in ASP.NET Core
author: ardalis
description: "Erläutert das Behandeln von Fehlern in ASP.NET Core-Anwendungen"
keywords: ASP.NET Core, Fehlerbehandlung, Behandlung von Ausnahmen
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 93f0724dbe98316e2b5a0af0ac1760c3aac2f1d0
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>Einführung in die Error Handling in ASP.NET Core

Durch [Steve Smith](https://ardalis.com/) und [Tom Dykstra](https://github.com/tdykstra/)

Dieser Artikel behandelt allgemeine Appoaches zum Behandeln von Fehlern in ASP.NET Core-apps.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)

## <a name="the-developer-exception-page"></a>Die Developer-Ausnahme-Seite

Installieren, um eine Anwendung, um eine Seite anzeigen, die zeigt detaillierte Informationen zu Ausnahmen Konfigurieren der `Microsoft.AspNetCore.Diagnostics` NuGet Verpacken, und fügen Sie eine Zeile, um die [Methoden konfigurieren, in die Startklasse](startup.md):

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Put `UseDeveloperExceptionPage` vor Middleware, die Sie Ausnahmen, z. B. abfangen möchten `app.UseMvc`.

>[!WARNING]
> Aktivieren Sie die Developer-Seite "Ausnahme" **nur, wenn die app in der Entwicklungsumgebung ausgeführt wird**. Sie möchten detaillierte Ausnahmeinformationen öffentlich freigeben, wenn die app in der produktionsumgebung ausgeführt wird. [Weitere Informationen zum Konfigurieren von Umgebungen](environments.md).

Um die Developer-Ausnahme-Seite angezeigt wird, führen Sie die beispielanwendung mit der Umgebung festgelegt `Development`, und fügen `?throw=true` an die Basis-URL der app. Die Seite enthält mehrere Registerkarten mit Informationen über die Ausnahme und die Anforderung. Die erste Registerkarte enthält eine stapelüberwachung. 

![Stapelüberwachung](error-handling/_static/developer-exception-page.png)

Der Registerkarte "Weiter" zeigt die Abfrage Zeichenfolgenparametern, sofern vorhanden.

![Abfragezeichenfolgen-Parameter](error-handling/_static/developer-exception-page-query.png)

Diese Anforderung keine Cookies haben, aber wenn dem so wäre, würden sie angezeigt, auf die **Cookies** Registerkarte. Sie können die Header, die in der Registerkarte "letzten" übergeben wurden, finden Sie unter.

![Header](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Konfigurieren einen benutzerdefinierte Ausnahmebehandlung-Seite

Es ist eine gute Idee, eine Ausnahme-Handler-Seite verwenden, wenn die app nicht, in ausgeführt wird "konfigurieren" die `Development` Umgebung.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

In einer MVC-app nicht explizit ergänzen die Fehler Handler-Aktionsmethode mit HTTP-Attribute, z. B. `HttpGet`. Verwendung von expliziten Verben könnte verhindern, dass manche Anforderungen erreichen die Methode.

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>Konfigurieren von Status-Codepages

Standardmäßig wird Ihre app keine rich-Status-Codepage für HTTP-Statuscodes, wie z. B. 500 (Interner Serverfehler) oder 404 (Nichtgefunden) bereit. Sie können konfigurieren, die `StatusCodePagesMiddleware` durch Hinzufügen einer Zeile auf die `Configure` Methode:

```csharp
app.UseStatusCodePages();
```

Standardmäßig fügt diese Middleware einfache, nur-Text-Handler für die allgemeinen Statuscodes wie 404:

![404-Seite](error-handling/_static/default-404-status-code.png)

Die Middleware unterstützt mehrere verschiedene Erweiterungsmethoden. Eine Überladung einen Lambda-Ausdruck, anderer eine Art und Format-Inhaltszeichenfolge.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Es gibt auch Erweiterungsmethoden umleiten. Eine einen Statuscode "302" an den Client sendet und eine den ursprünglichen Statuscode an den Client zurückgegeben, aber auch für die umleitungs-URL führt den Handler.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Wenn in diesem Fall müssen Sie eine Status-Codepages für bestimmte Anforderungen zu deaktivieren, können Sie dies tun:

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Ausnahmebehandlungscode

Code in Seiten für die Ausnahmebehandlung kann Ausnahmen auslösen. Es ist häufig eine gute Idee für Produktion Fehlerseiten rein statischer Inhalte bestehen.

Bedenken Sie außerdem, nachdem die Header für eine Antwort gesendet wurden, Statuscode der Antwort kann nicht geändert werden, noch können Ausnahme-Seiten oder der Handler ausgeführt. Die Antwort muss abgeschlossen werden, oder die Verbindung abgebrochen.

## <a name="server-exception-handling"></a>Server-Ausnahmebehandlung

Zusätzlich zu der Logik in Ihrer app für die Ausnahmebehandlung der [Server](servers/index.md) hosten Ihre app führt einige Ausnahmebehandlung. Wenn der Server eine Ausnahme abfängt, bevor die Header gesendet werden, sendet der Server eine 500 Internal Server Error Antwort ohne Text an. Wenn der Server eine Ausnahme abfängt, nachdem die Header gesendet wurden, schließt der Server die Verbindung an. Vom Server werden Anforderungen, die von Ihrer Anwendung behandelt werden nicht behandelt. Jede Ausnahme, die auftritt, erfolgt durch den Server-Ausnahme behandeln. Eine benutzerdefinierte Fehlerseiten konfiguriert oder Ausnahmebehandlung Middleware oder Filter wirken sich nicht auf dieses Verhalten.

## <a name="startup-exception-handling"></a>Start-Ausnahmebehandlung

Nur der Hostebene kann Ausnahmen behandeln, die während des Starts der app erfolgen. Sie können [konfigurieren, wie der Host als Antwort auf Fehler während des Starts verhält](hosting.md#detailed-errors) mit `captureStartupErrors` und die `detailedErrors` Schlüssel.

Hosting kann nur anzeigen eine Fehlerseite für den erfassten beim Start ein Fehler tritt der Fehler auf, nachdem der Host-Adresse/Port Bindung. Wenn eine Bindung aus irgendeinem Grund fehlschlägt, die Hostebene meldet eine kritische Ausnahme, die Dotnet Prozessabstürzen, und keine Fehler angezeigt wird.

## <a name="aspnet-mvc-error-handling"></a>ASP.NET MVC-Fehlerbehandlung

[MVC](../mvc/index.md) apps haben einige zusätzlichen Optionen zur Behandlung von Fehlern, z. B. Ausnahmefilter konfigurieren und Ausführen von modellvalidierung.

### <a name="exception-filters"></a>Ausnahmefilter

Ausnahmefilter können global oder für eine pro Controller oder pro-Aktion in einer MVC-Anwendung konfiguriert werden. Diese Filter behandelt jede nicht behandelte Ausnahme, die während der Ausführung eine Controlleraktion oder einen anderen Filter tritt auf, und andernfalls nicht aufgerufen. Erfahren Sie mehr über Ausnahmefilter in [Filter](../mvc/controllers/filters.md).

>[!TIP]
> Ausnahmefilter eignen sich für Auffangen von Ausnahmen, die in MVC Aktionen auftreten, aber sie sind nicht so flexibel wie Middleware für die Fehlerbehandlung. Middleware für die allgemeine Groß-/Kleinschreibung vorziehen, und verwenden Sie Filter, in denen Sie nur müssen Vorgehensweise Fehlerbehandlung *anders* basierend auf die MVC-Aktion ausgewählt wurde.

### <a name="handling-model-state-errors"></a>Modellstatus Behandeln von Fehlern

[Modellüberprüfung](../mvc/models/validation.md) vor jeder Controlleraktion aufgerufen wurde, und es wird die Aktionsmethode dafür verantwortlich, um zu überprüfen `ModelState.IsValid` und entsprechend reagieren.

Einige apps werden in diesem Fall eine Standardkonvention für den Umgang mit modellvalidierungsfehler, folgen, wählen Sie eine [Filter](../mvc/controllers/filters.md) eine geeignete Stelle eine solche Richtlinie implementiert werden kann. Sie sollten das Verhalten Ihrer Aktionen mit ungültigen modellzustände testen. Erfahren Sie mehr [testen Controllerlogik](../mvc/controllers/testing.md).



