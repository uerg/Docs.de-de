---
title: Sitzungs- und Anwendungszustand in ASP.NET Core
author: rick-anderson
description: "Ansätze zum Beibehalten des Anwendungs- und Benutzerzustands (Sitzungszustands) zwischen den Anforderungen"
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 11/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/app-state
ms.openlocfilehash: f4ed38f7395e3f4fe939584c1f3f5b0dba93724c
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/01/2018
---
# <a name="introduction-to-session-and-application-state-in-aspnet-core"></a>Einführung zum Sitzungs- und Anwendungszustand in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/) und [Diana LaRose](https://github.com/DianaLaRose)

Bei HTTP handelt es sich um ein zustandsloses Protokoll. Webserver behandeln HTTP-Anforderungen als unabhängige Anforderungen und speichern keine Benutzerwerte aus vorherigen Anforderungen. In diesem Artikel erhalten Sie Informationen zu den verschiedenen Möglichkeiten, den Anwendungs- und Sitzungszustand zwischen Anforderungen zu speichern. 

## <a name="session-state"></a>Sitzungszustand

Sitzungszustand ist ein Feature in ASP.NET Core, das Sie verwenden können, um Benutzerdaten zu speichern, während der Benutzer Ihre Web-App durchsucht. Der Sitzungszustand besteht aus einem Wörterbuch oder einer Hashtabelle auf dem Server und speichert Daten für mehrere Anforderungen, die von einem Browser gesendet werden. Die Sitzungsdaten werden in einem Cache gesichert.

ASP.NET Core verwaltet den Sitzungszustand, indem ein Cookie an den Client übergeben wird, das die Sitzungs-ID enthält, die mit jeder Anforderung an den Server gesendet wird. Der Server verwendet die Sitzungs-ID, um die Sitzungsdaten abzurufen. Da das Sitzungscookie browserspezifisch ist, können Sie Sitzungen nicht für mehrere Browser freigeben. Sitzungscookies werden nur gelöscht, wenn die Browsersitzung abläuft. Wenn für eine abgelaufene Sitzung ein Cookie empfangen wird, wird eine neue Sitzung erstellt, die dasselbe Cookie verwendet. 

Der Server speichert Sitzungen für einen beschränkten Zeitraum nach der letzten Anforderung. Legen Sie entweder ein Zeitlimit für die Sitzungen fest, oder verwenden Sie den Standardwert von 20 Minuten. Der Sitzungszustand eignet sich ideal zum Speichern von Benutzerdaten, die für eine bestimmte Sitzung zwar wichtig sind, jedoch nicht dauerhaft gespeichert werden müssen. Daten werden entweder aus dem Sicherungsspeicher gelöscht, wenn Sie `Session.Clear` aufrufen, oder wenn die Sitzung im Datenspeicher abläuft. Der Server weiß nicht, wann der Browser geschlossen wird, oder wann das Sitzungscookie gelöscht wird.

> [!WARNING]
> Speichern Sie keine vertraulichen Daten in der Sitzung. Es kann sein, dass der Client den Browser nicht schließt oder das nicht Sitzungscookie bereinigt. Einige Browser geben Sitzungscookies sogar an mehrere Fenster weiter. Außerdem kann es sein, dass Sitzungen nicht nur auf einen Benutzer beschränkt sind, sodass der nächste Benutzer dieselbe Sitzung fortführt.

Der speicherinterne Sitzungsanbieter speichert Sitzungsdaten auf dem lokalen Server. Wenn Sie Ihre Web-App auf einer Serverfarm ausführen möchten, müssen Sie beständige Sitzungen verwenden, um jede Sitzung an einen bestimmten Server zu binden. Die Windows Azure Web Sites-Plattform führt standardmäßig beständige Sitzungen durch (Routing von Anwendungsanforderungen, ARR). Diese Sitzungen können allerdings Auswirkungen auf die Skalierbarkeit haben und Updates für Web-Apps erschweren. Verwenden Sie deshalb besser Redis oder über SQL Server verteilte Caches, für die keine beständigen Sitzungen notwendig sind. Weitere Informationen finden Sie unter [Working with a Distributed Cache (Arbeiten mit einem verteilten Cache)](xref:performance/caching/distributed). Details zum Einrichten von Dienstanbietern finden Sie im Abschnitt [Konfigurationssitzung](#configuring-session).

<a name="temp"></a>
## <a name="tempdata"></a>TempData

ASP.NET Core MVC macht die Eigenschaft [TempData](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) auf einem [Controller](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.controller?view=aspnetcore-2.0) verfügbar. Diese Eigenschaft speichert Daten, bis sie gelesen wurden. Die Methoden `Keep` und `Peek` können verwendet werden, um die Daten zu überprüfen, ohne sie zu löschen. `TempData` eignet sich besonders für die Weiterleitung, wenn Daten für mehr als eine Anforderung benötigt werden. `TempData` wird von TempData-Anbietern implementiert, indem z.B. Cookies oder der Sitzungszustand verwendet werden.

<a name="tempdata-providers"></a>
### <a name="tempdata-providers"></a>TempData-Anbieter

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

In ASP.NET Core 2.0 und höher wird der cookiebasierte TempData-Anbieter standardmäßig verwendet, um TempData in Cookies zu speichern.

Die Cookiedaten werden mithilfe des [Base64UrlTextEncoder](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.webutilities.base64urltextencoder?view=aspnetcore-2.0) verschlüsselt. Da das Cookie verschlüsselt und in Blöcke unterteilt wird, gilt die Größenbeschränkung für einzelne Cookies nicht, die in ASP.NET Core 1.x gefunden wird. Die Cookiedaten werden nicht komprimiert, da es zu Sicherheitsproblemen kommen kann, wenn Daten verschlüsselt werden, z.B. durch [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit))- und [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit))-Angriffe. Weitere Informationen zum cookiebasierten TempData-Anbieter finden Sie unter [CookieTempDataProvider](https://github.com/aspnet/Mvc/blob/dev/src/Microsoft.AspNetCore.Mvc.ViewFeatures/ViewFeatures/CookieTempDataProvider.cs).

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

In ASP.NET Core 1.0 und 1.1 wird der TempData-Anbieter für den Sitzungszustand als Standard verwendet.

--------------

<a name="choose-temp"></a>
### <a name="choosing-a-tempdata-provider"></a>Auswählen eines TempData-Anbieters

Bevor Sie einen TempData-Anbieter auswählen, müssen Sie folgende Überlegungen anstellen:

1. Verwendet die Anwendung den Sitzungszustand bereits für andere Zwecke? Falls dies der Fall ist, hat die Verwendung des TempData-Anbieters abgesehen von der Größe der Daten keine zusätzlichen Auswirkungen auf die Anwendung.
2. Verwendet die Anwendung TempData nur selten für verhältnismäßig kleine Datenmengen (bis zu 500 Bytes)? Falls dies der Fall ist, entsteht durch das Cookie des TempData-Anbieters nur ein kleiner zusätzlicher Aufwand für jede Anforderung, die TempData enthält. Falls dies nicht der Fall ist, kann der Sitzungszustand des TempData-Anbieters nützlich sein, um Roundtrips für große Datenmengen für jede Anforderung durchzuführen, bis TempData verarbeitet wird.
3. Wird die Anwendung in einer Webfarm (also auf mehreren Servern) ausgeführt? Falls dies der Fall ist, ist keine zusätzliche Konfiguration erforderlich, um das Cookie des TempData-Anbieters zu verwenden.

> [!NOTE]
> Die meisten Webclients (z.B. Webbrowser) erzwingen Einschränkungen für die maximale Größe der Cookies, für die Gesamtanzahl der Cookies oder für beides. Aus diesem Grund sollten Sie, wenn Sie das Cookie des TempData-Anbieters verwenden, überprüfen, ob die App diese Einschränkungen überschreitet. Überprüfen Sie die Gesamtgröße der Daten, die bei der Verschlüsselung und der Unterteilung in Blöcke einen Mehraufwand verursachen.

<a name="config-temp"></a>
### <a name="configure-the-tempdata-provider"></a>Konfigurieren des TempData-Anbieters

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Der cookiebasierte TempData-Anbieter ist standardmäßig aktiviert. Der folgende `Startup`-Klassencode konfiguriert den sitzungsbasierten TempData-Anbieter:

[!code-csharp[](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,6,11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Der folgende `Startup`-Klassencode konfiguriert den sitzungsbasierten TempData-Anbieter:

[!code-csharp[](app-state/sample/src/WebAppSession/StartupTempDataSession.cs?name=snippet_TempDataSession&highlight=4,9)]

---

Die Reihenfolge, in der Middlewarekomponenten angeordnet werden, ist wichtig. Im vorherigen Beispiel kommt es zu einer Ausnahme für den `InvalidOperationException`-Typ, wenn `UseSession` nach `UseMvcWithDefaultRoute` aufgerufen wird. Weitere Informationen finden Sie unter [Middleware Ordering (Festlegen einer Reihenfolge für Middleware)](xref:fundamentals/middleware/index#ordering).

> [!IMPORTANT]
> Wenn Sie für .NET Framework entwickeln und den sitzungsbasierten Anbieter verwenden, fügen Sie das NuGet-Paket [Microsoft.AspNetCore.Session](https://www.nuget.org/packages/Microsoft.AspNetCore.Session) zu Ihrem Projekt hinzu.

## <a name="query-strings"></a>Abfragezeichenfolgen

Sie können eine begrenzte Menge an Daten von einer Anforderung an eine andere übergeben, indem Sie diese zu der Abfragezeichenfolge einer neuen Anforderung hinzufügen. Dies ist nützlich, um den Zustand dauerhaft zu erfassen und Links mit einem eingebetteten Zustand zuzulassen, die über E-Mail oder soziale Netzwerke geteilt werden sollen. Aus diesem Grund sollten Sie jedoch für sensible Daten keine Abfragezeichenfolgen verwenden. Wenn Daten in Abfragezeichenfolgen eingefügt werden, können sie nicht nur leicht freigegeben werden, sondern es entstehen auch Gelegenheiten für Versuche einer [websiteübergreifenden Anforderungsfälschung](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)), wodurch Benutzer dazu gebracht werden können, auf bösartige Websites zuzugreifen, während sie authentifiziert sind. Angreifer können dann Benutzerdaten von Ihrer App stehlen oder schädliche Aktionen im Namen eines Benutzers durchführen. Jeder gespeicherte Anwendungs- oder Sitzungszustand muss vor solchen Anforderungsfälschungen geschützt sein. Weitere Informationen zu websiteübergreifenden Anforderungsfälschungen finden Sie unter [Preventing Cross-Site Request Forgery (XSRF/CSRF) Attacks in ASP.NET Core (Verhindern von websiteübergreifenden Anforderungsfälschungen (XSRF/CSRF) in ASP.NET Core)](../security/anti-request-forgery.md).

## <a name="post-data-and-hidden-fields"></a>Bereitgestellte Daten und ausgeblendete Felder

Daten können in ausgeblendeten Formularfeldern gespeichert und an die nächste Anforderung zurückgesendet werden. Dies ist häufig in mehrseitigen Formularen der Fall. Allerdings muss der Server die Daten immer erneut überprüfen, da der Client die Daten manipulieren könnte. 

## <a name="cookies"></a>Cookies

Cookies stellen eine Möglichkeit dar, um benutzerdefinierte Daten in Webanwendungen zu speichern. Da mit jeder Anforderung Cookies gesendet werden, sollte deren Größe auf ein Minimum begrenzt sein. Idealerweise sollte nur ein Bezeichner in einem Cookie gespeichert werden, während die tatsächlichen Daten auf dem Server gespeichert werden sollten. Die meisten Browser beschränken die Anzahl von Cookies auf 4096 Byte. Außerdem ist nur eine begrenzte Anzahl von Cookies für jede Domäne verfügbar.  

Da Cookies manipuliert werden können, müssen sie auf dem Server überprüft werden. Obwohl die Dauerhaftigkeit des Cookies auf einem Client abhängig vom Eingreifen des Benutzers und des Ablaufdatums ist, stellen Cookies die am längsten andauernde Form von Datenpersistenz auf dem Client dar.

Cookies werden häufig aus Personalisierungsgründen verwendet, wenn Inhalt für einen bekannten Benutzer angepasst wird. Da Benutzer in den meisten Fällen nur identifiziert und nicht authentifiziert werden, können Sie Cookies in der Regel sichern, indem Sie den Benutzernamen, den Kontonamen oder eine eindeutige Benutzer-ID (wie den eindeutigen Bezeichner) im Cookie speichern. Sie können dann das Cookie verwenden, um auf die Infrastruktur der Benutzerpersonalisierung einer Website zuzugreifen.

## <a name="httpcontextitems"></a>HttpContext.Items

Die `Items`-Auflistung eignet sich gut zum Speichern von Daten, die nur benötigt werden, wenn eine bestimmte Anforderung verarbeitet wird. Die Inhalte der Auflistung werden nach jeder Aufforderung verworfen. Die `Items`-Auflistung sollte idealerweise für Komponenten oder Middleware als Mittel verwendet werden, um miteinander zu kommunizieren, wenn sie zu unterschiedlichen Zeitpunkten während einer Anforderung agieren und es keinen direkten Weg gibt, um Parameter zu übergeben. Weitere Informationen finden Sie im Abschnitt [Arbeiten mit „HttpContext.Items“](#working-with-httpcontextitems).

## <a name="cache"></a>cache

Das Caching stellt eine effiziente Möglichkeit zum Speichern und Abrufen von Daten dar. Sie können die Lebensdauer von Cachelementen steuern und dafür sowohl die Zeit als auch andere Bedingungen berücksichtigen. Weitere Informationen über [Caching](../performance/caching/index.md).

<a name="session"></a>
## <a name="working-with-session-state"></a>Arbeiten mit dem Sitzungszustand

### <a name="configuring-session"></a>Konfigurationssitzung

Das `Microsoft.AspNetCore.Session`-Paket stellt Middleware zum Verwalten eines Sitzungszustands zur Verfügung. `Startup` muss folgende Elemente enthalten, um die Sitzungsmiddleware zu aktivieren:

- Einen der [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)-Arbeitsspeichercaches Die `IDistributedCache`-Implementierung wird als Sicherungsspeicher für Sitzungen verwendet.
- Den [AddSession](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.dependencyinjection.sessionservicecollectionextensions#Microsoft_Extensions_DependencyInjection_SessionServiceCollectionExtensions_AddSession_Microsoft_Extensions_DependencyInjection_IServiceCollection_)-Aufruf, für den das NuGet-Paket „Microsoft.AspNetCore.Session“ erforderlich ist
- Den [UseSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.sessionmiddlewareextensions#methods_)-Aufruf

Der folgende Code zeigt, wie Sie den speicherinternen Sitzungsanbieter einrichten.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/Startup.cs?highlight=11-19,24)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/Startup.cs?highlight=11-19,24)]

---

Sie können auf Sitzungen über `HttpContext` verweisen, sobald dieser installiert und konfiguriert ist.

Wenn Sie versuchen, vor `UseSession` auf `Session` zuzugreifen, wird die Ausnahme `InvalidOperationException: Session has not been configured for this application or request` ausgelöst.

Wenn Sie versuchen, eine neue `Session` zu erstellen (d.h. es wurde noch kein Sitzungscookie erstellt), nachdem Sie begonnen haben, an den `Response`-Stream zu schreiben, wird die Ausnahme `InvalidOperationException: The session cannot be established after the response has started` ausgelöst. Die Ausnahme wird nicht im Browser angezeigt. Stattdessen finden Sie sie im Protokoll des Webservers.

### <a name="loading-session-asynchronously"></a>Asynchrones Laden der Sitzung 

Der Standardsitzungsanbieter in ASP.NET Core lädt Sitzungsaufzeichnungen aus dem zugrunde liegenden [IDistributedCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.distributed.idistributedcache)-Speicher nur asynchron, wenn die [ISession.LoadAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession#Microsoft_AspNetCore_Http_ISession_LoadAsync)-Methode explizit vor den Methoden `TryGetValue`, `Set` oder `Remove` aufgerufen wird. Wenn `LoadAsync` nicht zuerst aufgerufen wird, werden die zugrunde liegenden Sitzungsaufzeichnungen asynchron geladen, was Auswirkungen auf die Skalierbarkeit der App haben kann.

Damit Anwendungen dieses Muster erzwingen, umschließen Sie die Implementierungen [DistributedSessionStore](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsessionstore) und [DistributedSession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.session.distributedsession) mit Versionen, die eine Ausnahme auslösen, wenn die `LoadAsync`-Methode nicht vor `TryGetValue`, `Set` oder `Remove` aufgerufen wird. Registrieren Sie die umschlossenen Versionen in den Dienstcontainern.

### <a name="implementation-details"></a>Implementierungsdetails

Die Sitzung verwendet ein Cookie, um Anforderungen eines Browsers nachzuverfolgen und zu identifizieren. Standardmäßig wird das Cookie „.AspNet.Session“ genannt und verwendet den Pfad von „/“. Da das Standardcookie keine Domäne festlegt, wird es dem clientseitigen Skript auf der Seite nicht zur Verfügung gestellt, da der Standardwert von `CookieHttpOnly` `true` ist.

Verwenden Sie `SessionOptions`, um Standardwerte für Sitzungen zu überschreiben:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](app-state/sample/src/WebAppSessionDotNetCore2.0App/StartupCopy.cs?name=snippet1&highlight=8-12)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](app-state/sample/src/WebAppSession/StartupCopy.cs?name=snippet1&highlight=8-12)]

---

Der Server verwendet die `IdleTimeout`-Eigenschaft, um zu bestimmen, wie lange sich die Sitzung im Leerlauf befinden kann, bevor ihre Inhalte verworfen werden. Diese Eigenschaft ist unabhängig vom Ablauf der Cookies. Jede Anforderung, die über die Sitzungsmiddleware (aus der gelesen oder in die geschrieben wird) übergeben wird, setzt das Zeitlimit zurück.

Da die `Session` *keine sperrenden* Vorgänge durchführt, wird die ältere Anforderung überschrieben, wenn zwei Anforderungen versuchen, die Inhalte einer Sitzung zu bearbeiten. `Session` wird als *kohärente Sitzung* implementiert, d.h., dass alle Inhalte zusammen gespeichert werden. Es kann sein, dass zwei Anforderungen, die verschiedene Teile einer Sitzung (verschiedene Schlüssel) verändern, einander beeinflussen.

### <a name="setting-and-getting-session-values"></a>Festlegen und Abrufen von Sitzungswerten

Über die `Session`-Eigenschaft von `HttpContext` wird auf die Sitzung zugegriffen. Bei der Eigenschaft handelt es sich um eine [ISession](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.isession)-Implementierung.

Im folgenden Beispiel wird dargestellt, wie Sie eine ganze Zahl und eine Zeichenfolge festlegen und abrufen:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?range=8-27,49)]

Wenn Sie die folgenden Erweiterungsmethoden hinzufügen, können Sie serialisierbare Objekte für eine Sitzung festlegen und abrufen:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Extensions/SessionExtensions.cs)]

Im folgenden Beispiel wird dargestellt, wie Sie serialisierbare Objekte festlegen und abrufen:

[!code-csharp[Main](app-state/sample/src/WebAppSession/Controllers/HomeController.cs?name=snippet2)]


## <a name="working-with-httpcontextitems"></a>Arbeiten mit „HttpContext.Items“

Die `HttpContext`-Abstraktion stellt die Unterstützung für eine Wörterbuchauflistung des Typs `IDictionary<object, object>` namens `Items` bereit. Die Auflistung ist ab dem Start des *HttpRequest* verfügbar und wird am Ende einer Anforderung verworfen. Sie können darauf zugreifen, indem Sie einen Wert einem Eintrag mit einem Schlüssel zuweisen oder den Wert für einen bestimmten Schlüssel anfordern.

Im folgenden Beispiel wird per [Middleware](xref:fundamentals/middleware/index) `isVerified` zur Sammlung `Items` hinzugefügt.

```csharp
app.Use(async (context, next) =>
{
    // perform some verification
    context.Items["isVerified"] = true;
    await next.Invoke();
});
```

Zu einem späteren Punkt in der Pipeline kann eine andere Middleware auf die Auflistung zugreifen:

```csharp
app.Run(async (context) =>
{
    await context.Response.WriteAsync("Verified request? " + 
        context.Items["isVerified"]);
});
```

Für Middleware, die nur von einer App verwendet wird, werden `string`-Schlüssel akzeptiert. Trotzdem sollte Middleware, die für mehrere Anwendungen freigegeben ist, einen eindeutigen Objektschlüssel verwenden, um Schlüsselkonflikte zu vermeiden. Wenn Sie Middleware entwickeln, die für mehrere Anwendungen funktionieren soll, verwenden Sie einen eindeutigen Objektschlüssel, der wie im Folgenden dargestellt in Ihrer Middlewareklasse definiert ist:

```csharp
public class SampleMiddleware
{
    public static readonly object SampleKey = new Object();

    public async Task Invoke(HttpContext httpContext)
    {
        httpContext.Items[SampleKey] = "some value";
        // additional code omitted
    }
}
```

Anderer Code kann auf den Wert zugreifen, der in `HttpContext.Items` gespeichert ist, indem er den Schlüssel verwendet, der von der Middlewareklasse zur Verfügung gestellt wird:

```csharp
public class HomeController : Controller
{
    public IActionResult Index()
    {
        string value = HttpContext.Items[SampleMiddleware.SampleKey];
    }
}
```

Dieser Ansatz hat außerdem den Vorteil, dass die Wiederholung von „magischen“ Zeichenfolgen an mehreren Stellen im Code vermieden wird.

<a name="appstate-errors"></a>

## <a name="application-state-data"></a>Daten des Anwendungszustands

Verwenden Sie [Dependency Injection](xref:fundamentals/dependency-injection), um Daten für Benutzer bereitzustellen:

1. Definieren Sie einen Dienst, der die Daten enthält (z.B. eine Klasse namens `MyAppData`).

```csharp
public class MyAppData
{
    // Declare properties/methods/etc.
} 
```
2. Fügen Sie die Dienstklasse zu `ConfigureServices` hinzu (z.B. `services.AddSingleton<MyAppData>();`).
3. Verwenden Sie die Datendienstklasse in allen Controllern:

```csharp
public class MyController : Controller
{
    public MyController(MyAppData myService)
    {
        // Do something with the service (read some data from it, 
        // store it in a private field/property, etc.)
    }
} 
```

## <a name="common-errors-when-working-with-session"></a>Häufig auftretende Fehler beim Arbeiten mit Sitzungen

* „Unable to resolve service for type 'Microsoft.Extensions.Caching.Distributed.IDistributedCache' while attempting to activate 'Microsoft.AspNetCore.Session.DistributedSessionStore'. (Dienst kann nicht für den Typ „Microsoft.Extensions.Caching.Distributed.IDistributedCache“ aufgelöst werden, während versucht wird, auf „Microsoft.AspNetCore.Session.DistributedSessionStore“ zuzugreifen.)

  Dieser Fehler entsteht in der Regel, wenn nicht alle `IDistributedCache`-Implementierungen konfiguriert werden. Weitere Informationen finden Sie unter [Working with a Distributed Cache (Arbeiten mit einem verteilten Cache)](xref:performance/caching/distributed) und [In memory caching (Speicherinternes Caching)](xref:performance/caching/memory).

* Falls die Sitzungsmiddleware eine Sitzung nicht speichert (z.B. wenn die Datenbank nicht verfügbar ist), protokolliert sie die Ausnahme und lässt diese verloren gehen. Die Anforderung wird dann wie gewöhnlich fortgesetzt, wodurch das Verhalten nicht vorhergesehen werden kann.

Ein typisches Beispiel:

Jemand speichert einen Warenkorb in einer Sitzung. Der Benutzer fügt ein Element hinzu, aber der Commit schlägt fehl. Die App wird nicht über den Fehler informiert und gibt die Meldung „Das Element wurde hinzugefügt“ zurück. Dies stimmt jedoch nicht.

Es wird empfohlen, nach solchen Fehlern zu suchen, indem Sie `await feature.Session.CommitAsync();` aus App-Code aufrufen, wenn Sie mit dem Schreiben der Sitzung fertig sind. Dann können Sie den Fehler nach Belieben verarbeiten. Wenn Sie `LoadAsync` aufrufen, entsteht derselbe Effekt.

### <a name="additional-resources"></a>Zusätzliche Ressourcen

* [ASP.NET Core 1.x: in diesem Artikel verwendeter Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSession)
* [ASP.NET Core 2.x: in diesem Artikel verwendeter Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/app-state/sample/src/WebAppSessionDotNetCore2.0App)
