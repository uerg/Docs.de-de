---
title: Migrieren von ClaimsPrincipal.Current
author: mjrousos
description: Informationen Sie zum Migrieren von ClaimsPrincipal.Current zum Abrufen des aktuellen authentifizierten Benutzers Identität und Ansprüche in ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
uid: migration/claimsprincipal-current
ms.openlocfilehash: 477f9fe8f0249bdfc528e1b401f072851f2f0d23
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277185"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>Migrieren von ClaimsPrincipal.Current

In ASP.NET-Projekten war es üblich, dass verwendet [ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current) zum Abrufen des aktuellen authentifizierte Identität und Ansprüche des Benutzers. In ASP.NET Core ist diese Eigenschaft nicht mehr festgelegt werden. Code, der diese abhängig war, muss aktualisiert werden, um die aktuelle Identität des authentifizierten Benutzers auf eine andere Weise zu erhalten.

## <a name="context-specific-data-instead-of-static-data"></a>Kontext-spezifische Daten anstelle von statischen Daten

Bei Verwendung von ASP.NET Core, die Werte der beiden `ClaimsPrincipal.Current` und `Thread.CurrentPrincipal` sind nicht eingestellt. Diese Eigenschaften beide repräsentieren statischen Zustand, wodurch ASP.NET Core im Allgemeinen vermieden werden. Stattdessen wird ASP.NET Kernarchitektur zum Abrufen von Abhängigkeiten (z. B. die Identität des aktuellen Benutzers) aus Service kontextspezifisch Sammlungen (mit seiner [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) (DI)-Modell). Was mehr ist `Thread.CurrentPrincipal` Thread statisch ist, damit die Änderungen in einigen asynchronen Szenarien nicht beibehalten werden kann (und `ClaimsPrincipal.Current` ruft nur `Thread.CurrentPrincipal` standardmäßig).

Um zu verstehen, sortiert die Probleme Thread können statische Member führen, den folgende Codeausschnitt in asynchronen Szenarien zu berücksichtigen:

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

Das vorhergehende Beispiel Code setzt `Thread.CurrentPrincipal` und dessen Wert überprüft wird, vor und nach dem Warten auf einen asynchronen Aufruf. `Thread.CurrentPrincipal` bezieht sich auf die *Thread* auf dem sie festgelegt ist, und die Methode ist wahrscheinlich die Ausführung in einem anderen Thread nach dem await-Anweisung fortgesetzt werden. Folglich `Thread.CurrentPrincipal` liegt vor, wenn er wird zuerst überprüft, aber null ist, nach dem Aufruf von `await Task.Yield()`.

Abrufen der Identität des aktuellen Benutzers aus der app DI Dienst Auflistung ist mehr getestet werden können, zu, da Test Identitäten problemlos eingefügt werden können.

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>Den aktuellen Benutzer in einer ASP.NET Core-app abrufen

Es gibt mehrere Optionen zum Abrufen des aktuellen authentifizierten Benutzers `ClaimsPrincipal` in ASP.NET Core anstelle von `ClaimsPrincipal.Current`:

* **ControllerBase.User**. MVC-Controller erreichen den aktuellen authentifizierte Benutzer mit ihren [Benutzer](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user) Eigenschaft.
* **HttpContext.User**. Komponenten mit Zugriff auf die aktuelle `HttpContext` (z. B. Middleware) erhalten des aktuellen Benutzers `ClaimsPrincipal` aus [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user).
* **Vom Aufrufer übergeben**. Bibliotheken ohne Zugriff auf die aktuelle `HttpContext` werden oft von Controllern oder Middleware-Komponenten bezeichnet und kann die Identität des aktuellen Benutzers als Argument übergeben.
* **IHttpContextAccessor**. Die Migration zu ASP.NET Core ASP.NET-Projekt möglicherweise zu groß, um die Identität des aktuellen Benutzers problemlos an alle erforderlichen Stellen zu übergeben. In solchen Fällen [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) können verwendet werden, dieses Problem zu umgehen. `IHttpContextAccessor` kann auf die aktuelle zugreifen `HttpContext` (falls vorhanden). Eine kurzfristige Lösung zum Abrufen der Identität des aktuellen Benutzers in Code, der noch nicht aktualisiert wurden für die Zusammenarbeit mit ASP.NET des DI-driven Kernarchitektur würde folgendermaßen lauten:

  * Stellen Sie `IHttpContextAccessor` verfügbar im DI Container durch Aufrufen von [AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793) in `Startup.ConfigureServices`.
  * Rufen Sie eine Instanz des `IHttpContextAccessor` während des Starts und in einer statischen Variablen zu speichern. Die Instanz wird in Code verfügbar gemacht, die zuvor den aktuellen Benutzer über eine statische Eigenschaft abgerufen wurde.
  * Abrufen des aktuellen Benutzers `ClaimsPrincipal` mit `HttpContextAccessor.HttpContext?.User`. Wenn dieser Code, außerhalb des Kontexts einer HTTP-Anforderung verwendet wird, die `HttpContext` ist null.

Der endgültige option mit `IHttpContextAccessor`, im Gegensatz zur ASP.NET Kernprinzipien (ausgehandelt eingefügte Abhängigkeiten für eine statische Abhängigkeiten) ist. Schließlich die Abhängigkeit von der statischen entfernen möchten `IHttpContextAccessor` Helper. Es kann nützlich Brücke, aber sein, wenn große vorhandene ASP.NET-Anwendungen migrieren, die zuvor verwendet haben `ClaimsPrincipal.Current`.
