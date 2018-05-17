---
title: Fehlerbehandlung in ASP.NET Core
author: ardalis
description: Erfahren Sie mehr über die Fehlerbehandlung in ASP.NET Core-Anwendungen.
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5443cbeb1ef95c579e5fc12b625babbfa27c7ec2
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="handle-errors-in-aspnet-core"></a>Fehlerbehandlung in ASP.NET Core

Von [Steve Smith](https://ardalis.com/) und [Tom Dykstra](https://github.com/tdykstra/)

Dieser Artikel beschreibt grundsätzliche Vorgehensweisen zur Behandlung von Fehlern in ASP.NET Core-Anwendungen.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Die Seite mit Ausnahmen für Entwickler

Wenn Sie eine App so konfigurieren möchten, dass eine Seite mit detaillierten Informationen zu Ausnahmen angezeigt wird, müssen Sie das NuGet-Paket `Microsoft.AspNetCore.Diagnostics` installieren und eine Zeile zur [Configure-Methode in der Startklasse](startup.md) hinzufügen:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Fügen Sie `UseDeveloperExceptionPage` vor Middleware ein, in der Sie Ausnahmen abfangen möchten, wie z.B. `app.UseMvc`.

>[!WARNING]
> Aktivieren Sie die Seite mit Ausnahmen für Entwickler **nur, wenn die App in der Entwicklungsumgebung ausgeführt wird**. Wenn die App in der Produktionsumgebung ausgeführt wird, sollten Sie keine detaillierten Ausnahmeinformationen öffentlich teilen. [Weitere Informationen zum Konfigurieren von Umgebungen](environments.md).

Wenn Sie die Seite mit Ausnahmen für Entwickler anzeigen möchten, führen Sie die Beispielanwendung mit der auf `Development` festgelegten Umgebung aus, und fügen Sie `?throw=true` zur Basis-URL der App hinzu. Die Seite enthält mehrere Registerkarten mit Informationen zu der Ausnahme und der Anforderung. Die erste Registerkarte enthält eine Stapelüberwachung. 

![Stapelüberwachung](error-handling/_static/developer-exception-page.png)

Auf der nächsten Registerkarte werden, sofern vorhanden, Abfragezeichenfolgeparameter angezeigt.

![Abfragezeichenfolgeparameter](error-handling/_static/developer-exception-page-query.png)

Diese Anforderung enthielt keine Cookies. Andernfalls würden Sie auf der Registerkarte **Cookies** angezeigt. Sie können die Header sehen, die auf der letzten Registerkarte übergeben wurden.

![Header](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Konfigurieren einer benutzerdefinierten Seite zur Ausnahmebehandlung

Es ist eine gute Idee, eine Seite zur Ausnahmebehandlung zu konfigurieren, die verwendet werden kann, wenn die App nicht in der `Development`-Umgebung ausgeführt wird.

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

Die Aktionsmethode zur Fehlerbehandlung wird in einer MVC-App nicht explizit durch HTTP-Methodenattribute wie `HttpGet` ergänzt. Durch die Verwendung expliziter Verben könnte bei einigen Anforderungen verhindert werden, dass diese Methode zum Einsatz kommt.

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>Konfigurieren von Statuscodeseiten

Ihre App stellt für HTTP-Statuscodes wie *404 – Nicht gefunden* standardmäßig keine ausführliche Statuscodeseite zur Verfügung. Um Statuscodeseiten bereitzustellen, muss die Middleware „Status Code Pages“ durch Hinzufügen einer Zeile zu `Startup.Configure` konfiguriert werden:

```csharp
app.UseStatusCodePages();
```

Standardmäßig fügt die Middleware „Satus Code Pages“ für gängige Statuscodes wie 404 einfache Handler im Textformat hinzu:

![404-Seite](error-handling/_static/default-404-status-code.png)

Die Middleware unterstützt zahlreiche Erweiterungsmethoden. Eine Methode verwendet einen Lambdaausdruck:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

Eine andere Methode verwendet einen Inhaltstyp und eine Formatzeichenfolge:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Es gibt auch Erweiterungsmethoden für Umleitungen und erneutes Ausführen. Die Umleitungsmethode sendet den Statuscode 302 an den Client:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

Die andere Methode gibt den ursprünglichen Statuscode an den Client zurück, führt jedoch auch den Handler für die Umleitungs-URL aus:

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Statuscodeseiten können für bestimmte Anforderungen in der Handlermethode einer Razor-Seite oder in einem MVC-Controller deaktiviert werden. Versuchen Sie, [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) aus der Sammlung [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) der Anforderung abzurufen und das Feature zu deaktivieren (falls es verfügbar ist), um Statuscodeseiten zu deaktivieren:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Ausnahmebehandlungscode

Code auf Seiten zur Ausnahmebehandlung kann Ausnahmen auslösen. Es ist häufig empfehlenswert, wenn Produktionsfehlerseiten nur statische Inhalte aufweisen.

Bedenken Sie zudem, dass Sie den Statuscode einer Antwort nicht ändern können, sobald Sie die Header für diese Antwort gesendet haben, und dass weder Ausnahmeseiten noch Handler ausgeführt werden können. Die Antwort muss abgeschlossen oder die Verbindung unterbrochen werden.

## <a name="server-exception-handling"></a>Sichere Ausnahmebehandlung

Neben der Ausnahmebehandlungslogik in Ihrer App werden auf dem [Server](servers/index.md), der Ihre App hostet, einige Ausnahmebehandlungen durchgeführt. Wenn der Server eine Ausnahme erfasst, bevor die Header gesendet werden, sendet der Server die Antwort *500 – Interner Serverfehler* ohne Text. Wenn der Server eine Ausnahme auffängt, nachdem die Header gesendet wurden, schließt der Server die Verbindung. Anforderungen, die nicht von Ihrer App verarbeitet werden, werden vom Server verarbeitet. Jede auftretende Ausnahme wird durch die Ausnahmebehandlung des Servers behandelt. Konfigurierte benutzerdefinierte Fehlerseiten, Middleware oder Filter zur Fehlerbehandlung haben keine Auswirkungen auf dieses Verhalten.

## <a name="startup-exception-handling"></a>Fehlerbehandlung während des Starts

Nur auf Hostebene können Ausnahmen behandelt werden, die während des Starts einer App auftreten. Sie können mit `captureStartupErrors` und dem Schlüssel `detailedErrors` [das Verhalten des Hosts bei Fehlern während des Starts konfigurieren](hosting.md#detailed-errors).

Das Hosting kann nur dann eine Fehlerseite für einen erfassten Fehler beim Start anzeigen, wenn der Fehler nach der Bindung der Hostadresse bzw. des Ports auftritt. Wenn eine Bindung aus irgendeinem Grund fehlschlägt, wird auf der Hostebene eine kritische Ausnahme protokolliert, der .NET-Prozess stürzt ab, und es wird keine Fehlerseite angezeigt, wenn die App auf dem [Kestrel](xref:fundamentals/servers/kestrel)-Server ausgeführt wird.

Wenn sie auf [IIS](/iis) oder [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ausgeführt wird, wird ein *Prozessfehler 502.5* vom [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module) zurückgegeben, wenn der Prozess nicht gestartet werden kann. Befolgen Sie die Hinweise zur Fehlerbehebung im Thema [Troubleshoot ASP.NET Core on IIS (Problembehandlung bei ASP.NET Core in IIS)](xref:host-and-deploy/iis/troubleshoot).

## <a name="aspnet-mvc-error-handling"></a>ASP.NET MVC-Fehlerbehandlung

[MVC](xref:mvc/overview)-Apps verfügen über einige zusätzliche Optionen für die Fehlerbehandlung, z.B. das Konfigurieren von Ausnahmefiltern und das Durchführen einer Modellvalidierung.

### <a name="exception-filters"></a>Ausnahmefilter

Ausnahmefilter können in einer MVC-App global oder auf der Grundlage eines Controllers oder einer Aktion konfiguriert werden. Diese Filter verarbeiten jede nicht behandelte Ausnahme, die während der Ausführung eine Controlleraktion oder eines anderen Filters auftritt, und werden andernfalls nicht aufgerufen. Weitere Informationen zu Ausnahmefiltern finden Sie unter [Filter](../mvc/controllers/filters.md).

>[!TIP]
> Ausnahmefilter eignen sich zum Auffangen von Ausnahmen, die in MVC-Aktionen auftreten. Sie sind jedoch nicht so flexibel wie Middleware für die Fehlerbehandlung. Es wird empfohlen, dass Sie Middleware für allgemeine Fälle vorziehen und Filter nur dann verwenden, wenn Sie die Fehlerbehandlung *auf einem anderen Weg* angehen müssen, je nachdem, welche MVC-Aktion gewählt wurde.

### <a name="handling-model-state-errors"></a>Behandeln von Modellstatusfehlern

Die [Modellvalidierung](../mvc/models/validation.md) tritt vor jeder ausgelösten Controlleraktion auf. Die Aktionsmethode ist dafür verantwortlich, `ModelState.IsValid` zu untersuchen und angemessen zu reagieren.

Einige Apps wählen eine Standardkonvention aus, um Modellvalidierungsfehler zu behandeln. Dann kann ein [Filter](../mvc/controllers/filters.md) hilfreich sein, um eine solche Richtlinie zu implementieren. Sie sollten testen, wie sich Ihre Aktionen mit ungültigen Modellstatus verhalten. Weitere Informationen finden Sie unter [Testen von Controllerlogik](../mvc/controllers/testing.md).



