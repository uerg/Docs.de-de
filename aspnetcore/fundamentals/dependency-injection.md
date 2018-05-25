---
title: Dependency Injection in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie ASP.NET Core Dependency Injection implementiert und wie Sie dieses Muster verwenden können.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 700ceb081b2067f932ce8ed08c45c62058775e33
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="dependency-injection-in-aspnet-core"></a>Dependency Injection in ASP.NET Core

<a name="fundamentals-dependency-injection"></a>

Von [Steve Smith](https://ardalis.com/) und [Scott Addie](https://scottaddie.com)

ASP.NET Core wurde von Grund auf für die Unterstützung und Nutzung von Dependency Injection entwickelt. ASP.NET Core-Anwendungen können integrierte Frameworkdienste nutzen, indem diese in Methoden in der Startklasse eingefügt werden. Zudem können Anwendungsdienste für die Injection konfiguriert werden. Der von ASP.NET Core bereitgestellte Standarddienstcontainer bietet eine minimale Featuregruppe und ist nicht als Ersatz für andere Container vorgesehen.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>Was ist Dependency Injection?

Bei Dependency Injection handelt es sich um eine Technik, bei der eine lose Kopplung zwischen Objekten und ihren Projektmitarbeitern bzw. Abhängigkeiten erzielt wird. Anstelle einer direkten Instanziierung von Projektmitarbeitern oder der Verwendung statischer Referenzen werden die Objekte, die eine Klasse für die Durchführung ihrer Aktionen benötigt, in gewisser Weise für die Klasse bereitgestellt. In den meisten Fällen deklarieren Klassen ihre Abhängigkeiten über ihren Konstruktor, wodurch sie dem [Prinzip der expliziten Abhängigkeiten](http://deviq.com/explicit-dependencies-principle/) folgen können. Dieser Ansatz wird als „Constructor Injection“ bezeichnet.

Wenn Klassen vor dem Hintergrund von Dependency Injection entworfen werden, sind sie loser gekoppelt, da sie über keine direkten, hartcodierten Abhängigkeiten für ihre Projektmitarbeiter verfügen. Dies ergibt sich aus dem [Prinzip der Abhängigkeitsinversion](http://deviq.com/dependency-inversion-principle/), das Folgendes besagt: *„Module der oberen Ebene sollten nicht von Modulen der niedrigen Ebene abhängen; beide sollten von Abstraktionen abhängen.“* Statt auf bestimmte Implementierungen zu verweisen, fordern Klassen Abstraktionen an (in der Regel `interfaces`), die bei der Erstellung der Klassen für sie bereitgestellt werden. Das Extrahieren von Abhängigkeiten in Schnittstellen und das Bereitstellen von Implementierungen dieser Schnittstellen als Parameter stellen ein weiteres Beispiel für das [Strategieentwurfsmuster](http://deviq.com/strategy-design-pattern/) dar.

Wenn ein System für die Verwendung von Dependency Injection entworfen wird und viele Klassen ihre Abhängigkeiten über ihren Konstruktur (oder Eigenschaften) anfordern, ist es hilfreich, wenn Sie über eine Klasse verfügen, die sich der Erstellung dieser Klassen mit den zugehörigen Abhängigkeiten widmet. Diese Klassen werden als *Container* oder genauer gesagt als [Inversion of Control(-IoC)](http://deviq.com/inversion-of-control/)-Container oder Dependency Injection-Container bezeichnet. Ein Container ist im Wesentlichen eine Factory, die für die Bereitstellung der von ihm angeforderten Instanzen von Typen verantwortlich ist. Wenn ein bestimmter Typ deklariert hat, dass er über Abhängigkeiten verfügt und der Container so konfiguriert wurde, dass die Abhängigkeitstypen bereitgestellt werden können, erstellt er diese Abhängigkeiten im Rahmen der Erstellung der angeforderten Instanz. Auf diese Weise können komplexe Abhängigkeitsdiagramme für Klassen bereitgestellt werden, ohne dass eine hartcodierte Objektkonstruktion erforderlich ist. Neben dem Erstellen von Objekten mit den zugehörigen Abhängigkeiten verwalten Container normalerweise auch die Objektlebensdauer innerhalb der Anwendung.

ASP.NET Core enthält einen einfachen integrierten Container (dargestellt durch die Schnittstelle `IServiceProvider`), der die Constructor Injection standardmäßig unterstützt. Zudem stellt ASP.NET bestimmte Dienste über Dependency Injection zur Verfügbar. Der ASP. NET-Container bezeichnet von ihm verwaltete Typen als *Dienste*. Im weiteren Verlauf dieses Artikels bezieht sich der Ausdruck *Dienste* auf Typen, die vom ASP.NET Core-IoC-Container verwaltet werden. Sie konfigurieren die integrierten Containerdienste in der `ConfigureServices`-Methode in der Klasse `Startup` Ihrer Anwendung.

> [!NOTE]
> Martin Fowler hat einen ausführlichen Artikel zur [Inversion of Control Containers and the Dependency Injection Pattern (Umkehrung von Steuerelementcontainern und dem Dependency Injection-Muster)](https://www.martinfowler.com/articles/injection.html) geschrieben. Microsoft Patterns and Practices verfügt auch über eine hervorragende Beschreibung für [Dependency Injection](https://msdn.microsoft.com/library/hh323705.aspx).

> [!NOTE]
> Dieser Artikel behandelt das Thema Dependency Injection. Dieses Muster kann für alle ASP.NET-Anwendungen verwendet werden. Dependency Injection in MVC-Controllern wird unter [Dependency Injection and Controllers (Dependency Injection und Controller)](../mvc/controllers/dependency-injection.md) behandelt.

### <a name="constructor-injection-behavior"></a>Verhalten von Constructor Injection

Für Constructor Injection ist erforderlich, dass der betreffende Konstruktor *öffentlich* ist. Andernfalls löst Ihre App eine `InvalidOperationException` aus:

> Es konnte kein geeigneter Konstruktor für den Typ „YourType“ gefunden werden. Stellen Sie sicher, dass der Typ konkret ist und für alle Parameter eines öffentlichen Konstruktors Dienste registriert sind.

Bei Constructor Injection darf nur ein anwendbarer Konstruktor vorhanden sein. Konstruktorüberladungen werden unterstützt. Es darf jedoch nur eine Überladung vorhanden sein, deren Argumente alle durch Dependency Injection erfüllt werden können. Wenn mehrere Überladungen vorhanden sind, löst Ihre App eine `InvalidOperationException` aus:

> Im Typ „YourType“ wurden mehrere Konstruktoren gefunden, die alle angegebenen Argumenttypen akzeptieren. Es sollte nur ein anwendbarer Konstruktor vorhanden sein.

Konstruktoren können Argumente akzeptieren, die nicht durch Dependency Injection bereitgestellt werden. Diese müssen jedoch die Standardwerte unterstützen. Zum Beispiel:

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a>Verwenden von im Framework bereitgestellten Diensten

Die Methode `ConfigureServices` in der `Startup`-Klasse ist verantwortlich für das Definieren der von der Anwendung verwendeten Dienste. Dies beinhaltet Plattformfeatures wie Entity Framework Core und ASP.NET Core MVC. Zunächst enthält die in `ConfigureServices` bereitgestellte `IServiceCollection` folgende definierten Dienste (abhängig von der [Vorgehensweise bei der Konfiguration des Hosts](xref:fundamentals/hosting)):

| Diensttyp | Lebensdauer |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Transient (vorübergehend) |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Transient (vorübergehend) |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Transient (vorübergehend) |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Transient (vorübergehend) |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |

Im Folgenden finden Sie ein Beispiel zur Vorgehensweise beim Hinzufügen zusätzlicher Dienste zum Container mit einer Reihe von Erweiterungsmethoden wie `AddDbContext`, `AddIdentity` und `AddMvc`.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Die von ASP.NET bereitgestellten Features und die bereitgestellte Middleware, wie z.B. MVC, folgen einer Konvention, bei der eine einzelne Erweiterungsmethode zum Hinzufügen von *ServiceName* für die Registrierung aller für dieses Feature erforderlichen Dienste verwendet wird.

> [!TIP]
> In `Startup`-Methoden können Sie über die zugehörigen Parameterlisten bestimmte im Framework bereitgestellten Dienste anfordern. Weitere Einzelheiten hierzu finden Sie unter [Anwendungsstart](startup.md).

## <a name="registering-services"></a>Registrieren von Diensten

Sie können Ihre eigenen Anwendungsdienste wie folgt registrieren. Der erste generische Typ stellt den vom Container angeforderten Typ dar (in der Regel eine Schnittstelle). Der zweite generische Typ stellt den konkreten Typ dar, der vom Container instanziiert und zur Erfüllung solcher Anforderungen verwendet wird.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Jede Erweiterungsmethode vom Typ `services.Add<ServiceName>` fügt Dienste hinzu (und konfiguriert diese möglicherweise). So fügt `services.AddMvc()` beispielsweise die für MVC erforderlichen Dienste hinzu. Es wird empfohlen, dieser Konvention zu folgen und dabei Erweiterungsmethoden in den Namespace `Microsoft.Extensions.DependencyInjection` einzufügen, damit Gruppen von Dienstregistrierungen eingeschlossen werden.

Mit der Methode `AddTransient` werden konkreten Diensten abstrakte Typen zugeordnet, die separat für jedes Objekt instanziiert werden, für das dies erforderlich ist. Dies wird als *Lebensdauer* des Dienstes bezeichnet. Weitere Lebensdaueroptionen werden nachfolgend beschrieben. Es ist wichtig, dass für jeden der von Ihnen registrierten Dienste eine geeignete Lebensdauer ausgewählt wird. Sollte für jede Klasse, die dies anfordert, eine neue Dienstinstanz bereitgestellt werden? Sollte eine Instanz über eine bestimmte Webanforderung hindurch verwendet werden? Oder sollte für die Lebensdauer der Anwendung eine einzelne Instanz verwendet werden?

In dem Beispiel für diesen Artikel gibt es einen einfachen Controller, der Zeichennamen anzeigt, die als `CharactersController` bezeichnet werden. Die zugehörige Methode `Index` zeigt die aktuelle Liste der Zeichen an, die in der Anwendung gespeichert worden sind, und initialisiert die Auflistung mit einer Reihe von Zeichen, sofern keine Werte vorhanden sind. Beachten Sie, dass obwohl diese Anwendung Entity Framework Core und die Klasse `ApplicationDbContext` für ihre Persistenz verwendet, nichts davon im Controller offensichtlich ist. Stattdessen wurde der spezifische Datenzugriffsmechanismus hinter einer Schnittstelle, `ICharacterRepository`, abstrahiert, die dem [Repositorymuster](http://deviq.com/repository-pattern/) folgt. Eine Instanz von `ICharacterRepository` wird über den Konstruktor angefordert und einem privaten Feld zugeordnet, über das anschließend ggf. auf Zeichen zugegriffen wird.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

Das `ICharacterRepository` definiert die beiden Methoden, in denen der Controller mit `Character`-Instanzen arbeiten muss.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Diese Schnittstelle wiederum wird von einem konkreten Typ, `CharacterRepository`, implementiert, der zur Laufzeit verwendet wird.

> [!NOTE]
> Die Verwendungsweise der Dependency Injection mit der Klasse `CharacterRepository` stellt ein allgemeines Modell dar, dem Sie bei allen Ihren Anwendungsdiensten folgen können, nicht nur in „Repositorys“ oder Datenzugriffsklassen.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Beachten Sie, dass das `CharacterRepository` einen `ApplicationDbContext` beim zugehörigen Konstruktor anfordert. Es ist nicht ungewöhnlich, dass Dependency Injection auf solch verkettete Weise verwendet wird, in der jede angeforderte Abhängigkeit wiederum eine eigene Abhängigkeit anfordert. Der Container ist für die Auflösung all dieser Abhängigkeiten im Diagramm und die Rückgabe der vollständig aufgelösten Dienste verantwortlich.

> [!NOTE]
> Die Erstellung des angeforderten Objekts, sowie aller dafür erforderlichen Objekte und wiederum aller dafür erforderlichen Objekte wird manchmal als *Objektdiagramm* bezeichnet. Gleichermaßen werden die gesammelten aufzulösenden Abhängigkeiten normalerweise als *Abhängigkeitsstruktur* oder *Abhängigkeitsdiagramm* bezeichnet.

In diesem Fall müssen das `ICharacterRepository` und `ApplicationDbContext` wiederum beim Dienstcontainer in `ConfigureServices` unter `Startup` registriert werden. `ApplicationDbContext` wird mit dem Aufruf der Erweiterungsmethode `AddDbContext<T>` konfiguriert. Im folgenden Code wird die Registrierung des Typs `CharacterRepository` veranschaulicht.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Entity Framework-Kontexte sollten mit der Lebensdauer `Scoped` zum Dienstcontainer hinzugefügt werden. Wenn Sie die Methoden des Hilfsprogramms wie oben dargestellt verwenden, geschieht dies automatisch. Repositorys, die Entity Framework nutzen, sollten die gleiche Lebensdauer verwenden.

> [!WARNING]
> Größte Vorsicht ist geboten, wenn ein `Scoped`-Dienst über ein Singleton aufgelöst wird. In einem solchen Fall ist die Wahrscheinlichkeit groß, dass der Dienst bei der Verarbeitung nachfolgender Anforderungen einen falschen Status aufweist.

Dienste, die über Abhängigkeiten verfügen, sollten diese im Container registrieren. Wenn für einen Dienstkonstruktor ein Grundtyp erforderlich ist, wie z.B. eine `string`, kann dieser über [Konfiguration](xref:fundamentals/configuration/index) und das [Optionsmuster](xref:fundamentals/configuration/options) eingefügt werden.

## <a name="service-lifetimes-and-registration-options"></a>Dienstlebensdauer und Registrierungsoptionen

ASP.NET-Dienste können mit folgender Lebensdauer konfiguriert werden:

**Transient** (vorübergehend)

Dienste mit vorübergehender Lebensdauer werden bei jeder Anforderung neu erstellt. Diese Lebensdauer ist am besten für einfache, zustandslose Dienste geeignet.

**Scoped** (bereichsbezogen)

Dienste mit bereichsbezogener Lebensdauer werden einmal pro Anforderung erstellt.

> [!WARNING]
> Wenn Sie einen bereichsbezogenen Dienst in einer Middleware verwenden, müssen Sie den Dienst in eine `Invoke`- oder `InvokeAsync`-Methode einfügen. Fügen Sie ihn nicht über Constructor Injection ein, da hierdurch der Dienst ein Verhalten wie ein Singleton zeigt.

**Singleton**

Dienste mit Singleton-Lebensdauer werden bei ihrer Anforderung zum ersten Mal erstellt (oder wenn `ConfigureServices` ausgeführt wird, wenn Sie dort eine Instanz angeben); in jeder nachfolgenden Anforderung wird anschließend dieselbe Instanz verwendet. Wenn für Ihre Anwendung Singleton-Verhalten erforderlich ist, sollte zugelassen werden, dass der Dienstcontainer die Lebensdauer des Dienstes verwaltet, statt das Singleton-Entwurfsmuster zu implementieren und die Lebensdauer Ihres Objekts selbst in der Klasse zu verwalten.

Dienste können auf verschiedene Weisen beim Container registriert werden. Es wurde bereits gezeigt, wie eine Dienstimplementierung mit einem angegebenen Typ durch die Angabe des zu verwendenden konkreten Typs registriert wird. Darüber hinaus kann eine Factory angegeben werden, die dann bei Bedarf zum Erstellen der Instanz verwendet wird. Im dritten Ansatz wird die Instanz des zu verwendenden Typs direkt angegeben. In diesem Fall versucht der Container niemals, eine Instanz zu erstellen (und er verfügt auch nicht über die Instanz).

Stellen Sie sich zur Veranschaulichung des Unterschieds zwischen diesen Lebensdauer- und Registrierungsoptionen eine einfache Schnittstelle vor, die mindestens eine Task als *Vorgang* mit einem eindeutigen Bezeichner, `OperationId`, darstellt. Der Container stellt abhängig davon, wie die Lebensdauer für diesen Dienst konfiguriert wird, entweder die gleichen oder andere Instanzen des Diensts für die anfordernde Klasse bereit. Es wird ein Typ pro Lebensdaueroption erstellt, um zu verdeutlichen, welche Lebensdauer angefordert wird:

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Diese Schnittstellen werden über eine einzelne Klasse, `Operation`, implementiert, die eine `Guid` in ihrem Konstruktor akzeptiert oder eine neue `Guid` verwendet, wenn keine bereitgestellt wurde.

Im nächsten Schritt werden in `ConfigureServices` die einzelnen Typen entsprechend ihrer benannten Lebensdauer zum Container hinzugefügt:

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Beachten Sie, dass der `IOperationSingletonInstance`-Dienst eine bestimmte Instanz mit einer bekannten ID von `Guid.Empty` verwendet, damit klar ist, wenn dieser Typ verwendet wird (die zugehörige GUID besteht ausschließlich aus 0 (Nullen)). Zudem wurde ein `OperationService` registriert, der von den jeweils anderen `Operation`-Typen abhängt, damit innerhalb einer Anforderung klar ist, ob dieser Dienst bei den einzelnen Vorgangstypen die gleiche Instanz abruft wie der Controller oder eine neue. Dieser Dienst macht nur seine Abhängigkeiten als Eigenschaften verfügbar, damit diese in der Ansicht angezeigt werden können.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Zur Veranschaulichung der Objektlebensdauer innerhalb und zwischen separaten einzelnen Anforderungen an die Anwendung enthält das Beispiel einen `OperationsController`, der die einzelnen Arten von `IOperation`-Typen und einen `OperationService` anfordert. Die `Index`-Aktion zeigt dann alle `OperationId`-Werte des Controllers und des Diensts an.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Nun werden zwei separate Anforderungen an diese Controlleraktion gesendet:

![Die Ansicht „Vorgänge“ in der Webanwendung mit dem Dependency Injection-Beispiel, die in Microsoft Edge ausgeführt wird und in der bei der ersten Anforderung Vorgangs-ID-Werte (GUID) für die Controller „Transient“ (vorübergehend), „Scoped“ (bereichsbezogen), „Singleton“ und „Instance“ (Instanz) sowie „Operation Service Operations“ (Vorgänge des Vorgangsdiensts) angezeigt werden.](dependency-injection/_static/lifetimes_request1.png)

![Die Vorgangsansicht, in der die Vorgangs-ID-Werte für eine zweite Anforderung angezeigt werden.](dependency-injection/_static/lifetimes_request2.png)

Beobachten Sie, welche der `OperationId`-Werte innerhalb einer Anforderung und zwischen Anforderungen variieren.

* *Vorübergehende* Objekte sind immer unterschiedlich. Für jeden Controller und jeden Dienst wird eine neue Instanz bereitgestellt.

* *Bereichsbezogene* Objekte sind innerhalb einer Anforderung identisch, anforderungsübergreifend sind sie jedoch unterschiedlich.

* *Singleton*-Objekte sind bei jedem Objekt und in jeder Anforderung identisch (unabhängig davon, ob eine Instanz in `ConfigureServices` bereitgestellt wird)

## <a name="resolve-a-scoped-service-within-the-application-scope"></a>Auflösen eines bereichsbezogenen Diensts innerhalb des Anwendungsbereichs

Erstellen Sie [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) mit [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope), um einen bereichsbezogenen Dienst innerhalb des Anwendungsbereichs aufzulösen. Dieser Ansatz eignet sich gut dafür, beim Start auf einen bereichsbezogenen Dienst zuzugreifen und Initialisierungsaufgaben auszuführen. Das folgende Beispiel veranschaulicht, wie man einen Kontext für `MyScopedService` in `Program.Main` erhält:

```csharp
public static void Main(string[] args)
{
    var host = BuildWebHost(args);

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a>Bereichsvalidierung

Wenn die App in der Entwicklungsumgebung unter ASP.NET Core 2.0 oder höher ausgeführt wird, führt der Standarddienstanbieter Überprüfungen aus, um sicherzustellen, dass:

* Bereichsbezogene Dienste nicht direkt oder indirekt vom Stammdienstanbieter aufgelöst werden
* Bereichsbezogene Dienste nicht direkt oder indirekt in Singletons eingefügt werden

Der Stammdienstanbieter wird erstellt, wenn [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) aufgerufen wird. Die Lebensdauer des Stammdienstanbieters entspricht der Lebensdauer der App/des Servers, wenn der Anbieter mit der App erstellt wird und verworfen wird, wenn die App beendet wird.

Bereichsbezogene Dienste werden von dem Container verworfen, der sie erstellt hat. Wenn ein bereichsbezogener Dienst im Stammcontainer erstellt wird, wird die Lebensdauer effektiv auf Singleton heraufgestuft, da er nur vom Stammcontainer verworfen wird, wenn die App/der Server heruntergefahren wird. Die Überprüfung bereichsbezogener Dienste erfasst diese Situationen, wenn `BuildServiceProvider` aufgerufen wird.

Weitere Informationen finden Sie unter [Bereichsvalidierung im Thema „Hosting“](xref:fundamentals/hosting#scope-validation).

## <a name="request-services"></a>Anfordern von Diensten

Die Dienste, die innerhalb einer ASP.NET-Anforderung von `HttpContext` verfügbar sind, werden über die `RequestServices`-Sammlung verfügbar gemacht.

![Kontextbezogener IntelliSense-Dialog von HttpContext-Anforderungsdiensten, in dem angegeben wird, dass Anforderungsdienste den IServiceProvider abrufen oder festlegen, der Zugriff auf den Dienstcontainer der Anforderung bietet.](dependency-injection/_static/request-services.png)

Anforderungsdienste stellen die Dienste dar, die Sie als Teil Ihrer Anwendung konfigurieren und anfordern. Wenn Ihre Objekte Abhängigkeiten angeben, werden diese von den in `RequestServices`, nicht in `ApplicationServices`, gefundenen Typen erfüllt.

Im Allgemeinen sollten Sie diese Eigenschaften nicht direkt verwenden. Fordern Sie stattdessen lieber die Typen der erforderlichen Klassen über den Konstruktor Ihrer Klasse an, und lassen Sie diese Abhängigkeiten über das Framework einfügen. Dadurch werden Klassen angehalten, die leichter getestet werden können (siehe [Testen und Debuggen](../testing/index.md)) und loser gekoppelt sind.

> [!NOTE]
> Fordern Sie für den Zugriff auf die `RequestServices`-Sammlung Abhängigkeiten lieber als Konstruktorparameter an.

## <a name="designing-services-for-dependency-injection"></a>Entwerfen von Diensten für Dependency Injection

Sie sollten Ihre Dienste so entwerfen, dass Dependency Injection zum Abrufen ihrer Projektmitarbeiter verwendet wird. Dies bedeutet, dass die Verwendung zustandsbehafteter statischer Methodenaufrufe (die zu einem Code-Smell führen, auch bekannt als [Static Cling (statischer Zusammenhang)](http://deviq.com/static-cling/)) und die direkte Instanziierung abhängiger Klassen innerhalb Ihrer Dienste vermieden werden sollten. Es kann hilfreich sein, bei der Entscheidung, ob ein Typ instanziiert oder über Dependency Injection angefordert werden soll, an den Merksatz [New is Glue](https://ardalis.com/new-is-glue) („New“ hält besser) zu denken. Indem Sie den [SOLID-Grundsätzen für objektorientiertes Design](http://deviq.com/solid/) folgen, tendieren Ihre Klassen auf natürliche Weise dazu, klein und gut gestaltet zu sein und ohne großen Aufwand getestet werden zu können.

Was geschieht, wenn Sie den Eindruck haben, dass in Ihre Klassen tendenziell zu viele Abhängigkeiten eingefügt werden? Dies ist im Allgemeinen ein Zeichen dafür, dass Ihre Klasse versucht, zu viele Vorgänge durchzuführen, und gegen das SRP, das [Single-Responsibility-Prinzip](http://deviq.com/single-responsibility-principle/), verstößt. Versuchen Sie, die Klasse umzugestalten, indem Sie einige ihrer Aufgaben in eine neue Klasse verschieben. Denken Sie daran, dass der Schwerpunkt Ihrer `Controller`-Klassen auf Problemen der Benutzeroberfläche liegen sollte. Daher sollten Geschäftsregeln und Implementierungsdetails für den Datenzugriff in für diese [separaten Probleme](http://deviq.com/separation-of-concerns/) angemessenen Klassen enthalten sein.

Insbesondere im Hinblick auf den Datenzugriff können Sie den `DbContext` in Ihre Controller einfügen (vorausgesetzt, Sie haben in `ConfigureServices` Entity Framwork zum Dienstcontainer hinzugefügt). Einige Entwickler bevorzugen die Verwendung einer Repository-Schnittstelle für die Datenbank, statt den `DbContext` direkt einzufügen. Durch die Verwendung einer Schnittstelle zum Einschließen der Datenzugriffslogik an einer Stelle kann die Anzahl der Stellen minimiert werden, die bei Änderungen Ihrer Datenbank geändert werden müssen.

### <a name="disposing-of-services"></a>Freigeben von Diensten

Der Container ruft `Dispose` für die erstellten `IDisposable`-Typen auf. Wenn Sie jedoch selbst eine Instanz zum Container hinzufügen, wird diese nicht freigegeben.

Beispiel:

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> In Version 1.0 hat der Container für *alle* `IDisposable`-Objekte, einschließlich der nicht von ihm erstellten Objekte, „Dispose“ aufgerufen.

## <a name="replacing-the-default-services-container"></a>Ersetzen der Standarddienstcontainer

Der integrierte Dienstcontainer dient dazu, die grundlegenden Anforderungen des Frameworks und der meisten darin integrierten Consumeranwendungen zu erfüllen. Entwickler können den integrierten Container jedoch durch ihren bevorzugten Container ersetzen. Bei der Methode `ConfigureServices` wird in der Regel `void` zurückgegeben. Wenn ihre Signatur jedoch so geändert wird, dass `IServiceProvider` zurückgegeben wird, kann ein anderer Container konfiguriert und zurückgegeben werden. Für .NET sind viele IOC-Container verfügbar. In diesem Beispiel wird das Paket [Autofac](https://autofac.org/) verwendet.

Installieren Sie zunächst das entsprechende Containerpaket bzw. die entsprechenden Containerpakete:

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

Konfigurieren Sie als Nächstes den Container in `ConfigureServices`, und geben Sie einen `IServiceProvider` zurück:

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add other framework services

    // Add Autofac
    var containerBuilder = new ContainerBuilder();
    containerBuilder.RegisterModule<DefaultModule>();
    containerBuilder.Populate(services);
    var container = containerBuilder.Build();
    return new AutofacServiceProvider(container);
}
```

> [!NOTE]
> Wenn Sie den Dependency Injection-Container eines Drittanbieters verwenden, müssen Sie `ConfigureServices` so ändern, dass `IServiceProvider` anstelle von `void` zurückgegeben wird.

Konfigurieren Sie Autofac abschließend wie gewohnt in `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

Zur Laufzeit wird Autofac zum Auflösen von Typen und Einfügen von Abhängigkeiten verwendet. [Weitere Informationen zur Verwendung von Autofac und ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Threadsicherheit

Singleton-Dienste müssen threadsicher sein. Wenn ein Singleton-Dienst eine Abhängigkeit von einem vorübergehenden Dienst aufweist, muss der vorübergehende Dienst abhängig von der Verwendungsweise durch den Singleton-Dienst ebenfalls threadsicher sein.

## <a name="recommendations"></a>Empfehlungen

Beachten Sie folgende Empfehlungen bei der Arbeit mit Dependency Injection:

* Dependency Injection ist für Objekte mit komplexen Abhängigkeiten bestimmt. Controller, Dienste, Adapter und Repositorys sind alles Beispiele für Objekte, die zu Dependency Injection hinzugefügt werden könnten.

* Vermeiden Sie das Speichern von Daten und die direkte Konfiguration in Dependency Injection. Der Einkaufswagen eines Benutzers sollte z.B. normalerweise nicht zum Dienstcontainer hinzugefügt werden. Bei der Konfiguration sollte das [Optionsmuster](xref:fundamentals/configuration/options) verwendet werden. Gleichermaßen sollten Sie „Datencontainer“-Objekte vermeiden, die nur vorhanden sind, um den Zugriff auf einige andere Objekte zuzulassen. Wenn möglich, sollte dass tatsächlich benötige Element besser über Dependency Injection angefordert werden.

* Vermeiden Sie statischen Zugriff auf Dienste.

* Vermeiden Sie die Dienstidentifizierung in Ihrem Anwendungscode.

* Vermeiden Sie statischen Zugriff auf `HttpContext`.

Wie bei allen Empfehlungen treffen Sie möglicherweise auf Situationen, in denen eine der Empfehlungen ignoriert werden muss. Es gibt nur wenige Ausnahmen, die sich meistens auf ganz besondere Fälle innerhalb des Frameworks selbst beziehen.

Dependency Injection stellt eine *Alternative* zu statischen bzw. globalen Objektzugriffsmustern dar. Sie werden keinen Nutzen aus der Dependency Injection ziehen können, wenn Sie diese mit dem Zugriff auf statische Objekte kombinieren.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Abhängigkeitsinjektion in Ansichten](xref:mvc/views/dependency-injection)
* [Abhängigkeitsinjektion in Controller](xref:mvc/controllers/dependency-injection)
* [Abhängigkeitsinjektion in Anforderungshandlern](xref:security/authorization/dependencyinjection)
* [Application Startup (Starten von Anwendungen)](xref:fundamentals/startup)
* [Testen und Debuggen](xref:testing/index)
* [Factorybezogene Middlewareaktivierung](xref:fundamentals/middleware/extensibility)
* [Schreiben von sauberem Code in ASP.NET Core über Dependency Injection (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Entwurf einer mit Containern verwalteten Anwendung, Einleitung: Welche Zugehörigkeit hat der Container?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [Prinzip der expliziten Abhängigkeiten](http://deviq.com/explicit-dependencies-principle/)
* [Die Umkehrung von Steuerelementcontainern und das Dependency Injection-Muster](https://www.martinfowler.com/articles/injection.html) (Fowler)
